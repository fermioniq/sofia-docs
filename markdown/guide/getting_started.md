# Getting Started

## Introduction

In order to make development of PDE solvers more convenient, the
`sofia.pde` package contains its own symbolic layer, specific to PDEs.
The symbolic objects contain extra information that is used by the code
generation core and is designed to reduce user overhead. This interface
is in active development and new features will be added in the future
according to user feedback.

The goal of this interface is to provide convenient shorthands for
commonly used operations and to minimise cumbersome and error-prone
operations such as grid interpolations when using staggered grids. This
functionality will be added “on top” of the basic interface - expert
users will always be able to write explicit discrete expressions
whenever needed.

Writing a full program for a PDE solver using `sofia` can be divided in 4 simple steps:

1. **Define the \`Domain\`, \`Fields\` and symbolic parameters.**
2. **Symbolically write out update equations and boundary conditions.**
3. **Set up solver symbolically.**
4. **Generate the code and run the simulation.**

## Simple Example

Solving the 1D diffusion equation

$$
\frac{\partial c}{\partial t} = \frac{\partial^2 c}{\partial x^2}
$$

using a grid with 120 cells and evolved for 10 steps using a Euler timestepping scheme
is then done as follows:

```python
>>> import sympy as sp
>>> from sofia import Grid, Field, ir
>>> from sofia.pde import diff, simulate

# Step 1: Define Domain (here a structured Grid) and Fields and parameters
>>> G = Grid(ndim=1)
>>> c = Field("c", G)
>>> dt = 0.1

# Step 2: Write symbolic update equation
>>> c_update = c + dt * diff(c, G.x, 2)

# Step 3: Set up explicit timestepping loop solver
>>> solver = ir.CodeBlock(
...
...         # Set initial condition
...         c << sp.sin(2*sp.pi*G.x),
...
...         # Timestepping loop
...         ir.For(
...             length = 10,        # Number of steps
...             body = c << c_update
...         ),
...
...         # Define outputs
...         ir.Outputs((c,)),
...     )

# Step 4: Generate code and run simulation
>>> res = simulate(solver, domain_specs={G: dict(shape=10)})
```

You can express the code at a high level using the convenient shorthands in `sofia.pde`,
or alternatively manually write out the full discrete equations and solver setup for finer control.
Detailed explanations of each step for both high-level and manual approaches are provided in the following sections and in the examples.
