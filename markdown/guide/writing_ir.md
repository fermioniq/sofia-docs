<a id="writing-ir"></a>

# Writing IR

## Programs and Intermediate Representation (IR)

As introduced in the [Running a Simulation](build_solver.md#build-solver) section, the symbolic engine enables users to express the complete solver specification
as an intermediate representation (IR). This provides finer control over the solver structure and behavior.
The IR is designed to function as a complete, standalone program specification that can be stored, inspected, and analyzed independently of any
particular implementation or hardware target.

This page documents the individual IR statements. They fall into two groups:
general-purpose constructs — assignments, blocks, loops, inputs and outputs — and
tokens that encode specific numerical methods, described under
[PDE Tokens](#pde-tokens).

### Assignments

The central operation is `codegen.ir.EvalAssignment`, which specifies the
numerical evaluation of an expression and subsequent storage of the
result in a field.

For example, a simple update to the field `u` can be written as:

```python
ir.EvalAssignment(u, u + dt * diff(u, x))
```

The `<<` operator is shorthand for the same thing, and is the form used
throughout these docs:

```python
u << u + dt * diff(u, x)
```

Expressions may be written in continuous form, as above, in which case derivatives
are discretised at code generation time. They can also be written in fully
discretised form, using explicit indices and shifts:

```python
ir.EvalAssignment(u[i, j], u[i, j] + dt * (u[i + 1, j] - u[i - 1, j]) / (2*dx))
```

### Blocks

Statements are grouped with [`ir.CodeBlock`](../api/sofia.ir.md#sofia.ir.CodeBlock), which
executes them in order:

```python
ir.CodeBlock(
    u << u + dt * diff(u, x),
    v << v + dt * diff(u, y),
)
```

Blocks nest, so a block can be used anywhere a single statement is expected — as
the body of a loop, for instance.

## Loops

[`ir.For`](../api/sofia.ir.md#sofia.ir.For) executes its body a fixed number of times,
carrying field values from one iteration to the next:

```python
ir.For(
    length=100,
    body=ir.CodeBlock(
        u << u + dt * diff(u, x),
        v << v + dt * diff(u, y),
    ),
)
```

- `length`: number of iterations.
- `body`: statement or block executed on each iteration.

### Inputs

Optional inputs can be declared using `codegen.ir.Inputs`, which is useful for passing arrays as initial fields:

```python
ir.Inputs(u)
```

or, for several fields:

```python
ir.Inputs((u, v))
```

If the array should be loaded from a file, use `codegen.ir.Load`

```python
ir.Load("path/to/input/array", v)
```

### Outputs

[`ir.Outputs`](../api/sofia.ir.md#sofia.ir.Outputs) declares which fields the simulation
returns, and is placed outside the loop:

```python
ir.Outputs((u, v))
```

To write results to disk, use `codegen.ir.Save`.

```python
ir.Save("path/to/output/folder", (u, v))
```

<a id="pde-tokens"></a>

## PDE Tokens

Alongside the general constructs above, the IR provides tokens that encode
specific numerical methods. Each expands into ordinary assignments and blocks when
the program is processed, so they can be used wherever a statement is expected.

### Time Integration

[`ir.RKn`](../api/sofia.ir.md#sofia.ir.RKn) generates the stages of an explicit Runge-Kutta
step. It takes a mapping from each evolved field to its right-hand side, the order
or name of the scheme, and the time step:

```python
ir.RKn({u: F_u, v: F_v}, order=2, dt=1e-5)
```

Statements that should be applied after each stage, such as boundary conditions,
are passed to `post`. The available schemes are listed in [Running a Simulation](build_solver.md#build-solver).

### Pressure Projection

[`ir.ProjectDivFree`](../api/sofia.ir.md#sofia.ir.ProjectDivFree) removes the divergence of a
vector field by solving a Poisson equation for the pressure and subtracting its
gradient:

```python
ir.ProjectDivFree((u, v, w), p)
```

- `velocities`: the vector field components, overwritten in place.
- `pressure`: field used for the pressure solve.

All fields must be `GridField` instances on the same domain.

### Wall modelling

[`sofia.ir.pde.WallModelReconstruct`](../api/sofia.ir.pde.md#sofia.ir.pde.WallModelReconstruct) implements wall-modelled velocity reconstruction
around a 3D geometry:

```python
wm = ir.WallModelReconstruct(
    "reichardt",
    yp, # symbol for yplus
    (u, v, w), # velocities
    geo, # geometry
    NU, # kinematic viscosity (constant)
    2.5,  # band height
    6,  # match height
    15,  # wall fn solver iterations (Newton)
    1e2,  # max dist (for geometry queries)
    0,  # side (0 means double-sided)
)

code_ir = ir.CodeBlock(
    ir.Inputs((geo, u, v, w)),
    # Reconstruct (u,v,w) by simply inserting the token here:
    wm,
    ir.Outputs((u, v, w)),
)
```

See also [Geometry](domains_fields.md#guide-geometry-reference-label)

### Large Eddy Simulation turbulence modelling

[`sofia.ir.pde.EddyViscositySmagorinsky`](../api/sofia.ir.pde.md#sofia.ir.pde.EddyViscositySmagorinsky) and `sofia.pde.les.viscous_stress_divergence()` can be used
together to quickly set up LES simulations with a constant Smagorinsky subgrid scale model:

```python
# Constant kinematic viscosity
nu_sym = sp.symbols("nu_sym")
# Variable eddy viscosity
nu_sgs = Field("nu_field", G, Staggered.CENTER)

# Eddy viscosity computation IR
eddy_ir = ir.EddyViscositySmagorinsky((u, v, w), nu_sgs, subs[C_sgs], strain_squaring)

# Diffusion term of Navier-Stokes (viscous stress divergence with variable viscosity)
Fu_a, Fv_a, Fw_a = viscous_stress_divergence(u, v, w, nu_sgs + nu_sym)

# Use in update steps:
ir.RKn({u: Fu_a, v: Fv_a, w: Fw_a}, 1, 0.01),
```

It is also possible to use the individual components. For example, any subgrid scale model can be used to compute
the effective viscosity nu_eff = nu_sym + nu_sgs and passed to `sofia.pde.les.viscous_stress_divergence()`
in order to implement alternative LES schemes.
