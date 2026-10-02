<a id="build-solver"></a>

# Running a Simulation

Once the update equations and boundary conditions have been defined, the next step is to to assemble them into a solver.
This is done symbolically, by writing an:ref:Intermediate Representation (IR)<writing_ir> program: a complete description
of the computation to be performed, expressed as symbolic statements rather than as
executable code.

This approach completely decouples the mathematical formulation and design of the solver
from its implementation, including optimisation, compilation, and hardware-specific code generation
such as GPU kernels.

The IR is compositional: loops, assignments and time-integration schemes are all
tokens that nest inside one another. This page walks through a complete
example; the individual tokens are documented in [Writing IR](writing_ir.md#writing-ir).

## Building a Solver

We solve the 1D diffusion equation

$$
\frac{\partial c}{\partial t} = \alpha \frac{\partial^2 c}{\partial x^2}
$$

on a periodic grid.The domain, the field and the right-hand side are set up asdescribed in the previous sections:

```python
>>> import sympy as sp
>>> from sofia import Grid, Field, ir
>>> from sofia.pde import diff

>>> G = Grid(ndim=1)
>>> c = Field("c", domain=G)
>>> alpha, dt = sp.symbols("alpha dt")  # diffusion coefficient and timestep
>>> F = alpha * diff(c, G.x, 2)
```

The solver itself is built up from IR statements. The simplest useful one is an
assignment, written with the `<<` operator: `c << expr` evaluates `expr` and
stores the result in `c`. Setting the initial condition is exactly that:

```python
>>> init_c = c << sp.sin(2 * sp.pi * G.x)
```

An explicit Euler step is also just an assignment, since the update is a closed
expression in the current state:

```python
>>> dt = 1e-5
>>> step = c << c + dt * F
```

To advance in time, the step is repeated. [`ir.For`](../api/sofia.ir.md#sofia.ir.For)
executes its body a fixed number of times, carrying the field values from one
iteration to the next:

```python
>>> loop = ir.For(length=10, body=step)
```

Finally, the statements are collected into a
[`ir.CodeBlock`](../api/sofia.ir.md#sofia.ir.CodeBlock), with
[`ir.Outputs`](../api/sofia.ir.md#sofia.ir.Outputs) declaring what the simulation should
return:

```python
>>> solver = ir.CodeBlock(
...     init_c,
...     loop,
...     ir.Outputs((c,)),
... )
```

## Higher-Order Time Integration

Euler stepping is rarely accurate or stable enough in practice. Runge-Kutta
schemes improve on it by evaluating the right-hand side several times per step,
which no longer fits in a single assignment.
[`ir.RKn`](../api/sofia.ir.md#sofia.ir.RKn) generates the stages, and takes the place of the
assignment in the loop body:

```python
>>> solver = ir.CodeBlock(
...     # Set initial condition
...     c << sp.sin(2 * sp.pi * G.x),
...
...     # Timestepping loop
...     ir.For(
...         length=10,          # number of steps
...         body=ir.RKn(        # 2nd-order Runge-Kutta scheme
...             {c: F},
...             order=2,
...             dt=1e-5,
...         ),
...     ),
...
...     # Define outputs
...     ir.Outputs((c,)),
... )
```

[`ir.RKn`](../api/sofia.ir.md#sofia.ir.RKn) takes a mapping from each evolved field to its
right-hand side, so systems of equations are written by adding entries to the dict.
The scheme is selected by `order`, either as an integer or by name:

|   Order | Options                                               |
|---------|-------------------------------------------------------|
|       1 | 1, “euler”                                            |
|       2 | 2, “midpoint”, “heun2”, “ralston2”, “m2s2”            |
|       3 | 3, “kutta3”, “heun3”, “ralston3”, “SSPRK3”,<br/>“3/8” |
|       4 | 4                                                     |

Implementing high-order RK schemes by hand is tedious, so in practice simple
second- or third-order schemes tend to be used. Working symbolically removes that
constraint: the stages are expanded for you, and the cost of the expansion is paid
once at code generation time rather than at every step.

Boundary conditions and any other assignments that should be applied after each
update are passed to the `post` argument of
[`ir.RKn`](../api/sofia.ir.md#sofia.ir.RKn).

The assembled program can be printed:

```python
>>> print(solver)
```

```text
CodeBlock(
EvalAssignment(c, sin(2*global.x*pi)),
For(10, body=CodeBlock(
    RKn({c: alpha*Derivative(c, (global.x, 2))}, order=2, dt=1.0e-5)
)),
Outputs((c,))
)
```

Nothing here is discretised or numeric yet — the derivatives are still symbolic
and the grid shape is still the symbol `G.Nx`.

## Generating Code

Up to this point everything has been symbolic. To run the simulation, the IR is
converted into executable code. This is also where the numerical values that the
program leaves open are supplied: the shape and extent of each domain, and the
values of any remaining free symbols.

These are given per domain, as a `domain_specs` mapping:

```python
>>> domain_specs = {
...     G: dict(
...         shape=120,             # number of points per dimension
...         extent=(0, 1),         # physical extent
...         subs={alpha: 0.01},    # parameter values
...     )
... }
```

Because the specification is keyed on the domain, a program involving several
domains — multiple grids, or a grid and a particle set — is discretised by adding
an entry for each.

`simulate` performs the conversion and runs the
result in one call:

```python
>>> from sofia.pde import simulate

>>> output = simulate(solver, domain_specs)
```

What comes back is determined by the [`ir.Outputs`](../api/sofia.ir.md#sofia.ir.Outputs)
statement in the program: one array per field listed there. Any arrays the program
declares as inputs are passed as positional arguments after `domain_specs`.

### Building ahead of time

Code generation and execution can also be separated, which is useful when the same
simulation is run repeatedly or when benchmarking, since the build cost is then
paid once. `build_simulation` returns
the generated function without calling it:

```python
>>> from sofia.pde import build_simulation

>>> sim = build_simulation(solver, domain_specs)
>>> output = sim()
```

Passing `output_file` writes the generated source to disk instead of loading it
from memory, so it can be inspected, kept, or edited by hand. An existing file is
only replaced if `overwrite=True`:

```python
>>> sim = build_simulation(
...     solver,
...     domain_specs,
...     output_file="diffusion_1d.py",
...     overwrite=True,
... )
```

### Choosing a backend

Both functions accept a `kernel_backend` argument, selecting the target for the
generated kernels. The default, `"auto"`, picks a backend based on what is
available in the environment.

### Evaluating a single expression

A whole program is not always what is wanted. When developing an update equation or
checking a discretisation, a single assignment can be evaluated on its own with
`eval_expr`, passing the input arrays as
keyword arguments named after their fields:

```python
>>> import numpy as np
>>> from sofia.pde import eval_expr

>>> c_in = np.sin(2 * np.pi * np.linspace(0, 1, 120))
>>> result = eval_expr(c << c + dt * F, domain_specs, c=c_in)
```

Passing `show=True` prints the generated kernel source, which is the quickest way
to see how an expression was discretised — what the stencil coefficients came out
as, and how the indices were laid out.
