# sofia.ir.pde

### *class* sofia.ir.pde.ProjectDivFree(\*args, \*\*kwargs)

Bases: `Token`

Makes velocity fields divergence free by solving the Poisson equation.

Only uniform axes (without curvilinear coordinate transformations)
are supported for now.

This token expands to
: - ir.SolveSeparable: solves the Poisson equations, using
    : sofia.pde.laplacian(pressure, allow_expand=False) as the linear operator
  - Assignments that project out the divergence on the velocity fields

Due to the forwarding to `ir.SolveSeparable`, restrictions on the allowed boundary
conditions apply. Per axis, the following boundary conditions are supported:

> - periodic (solved via Fourier transform)
> - cell-centered neumann-neumann boundary conditions (via DCT-I)
> - edge neumann-neumann boundary conditions (via DCT-II)
> - mixed dirichlet/neumann boundary conditions at arbitrary
>   : locations, but not neumann-neumann since the linear system
>     becomes singular

One further restriction: at most one axis of the last type is allowed.

* **Parameters:**
  * **velocities** – Fields that store velocities, to be made divergence-free.
  * **pressure** – Field that represents the pressure. The contents of the pressure will
    be computed as part of this token and stored in this field.
  * **bcs** – Boundary conditions on `pressure` that should be enforced. This should be
    an `sp.Piecewise` expression with boundary conditions at the
    boundaries and `pressure` itself in the bulk. The best way to obtain this
    is via `sofia.pde.apply_boundary_conditions(bcs, pressure)`.

#### doit(deep=False, \*\*hints)

Evaluate objects that are not evaluated by default like limits,
integrals, sums and products. All objects of this kind will be
evaluated recursively, unless some species were excluded via ‘hints’
or unless the ‘deep’ hint was set to ‘False’.

```pycon
>>> from sympy import Integral
>>> from sympy.abc import x
```

```pycon
>>> 2*Integral(x, x)
2*Integral(x, x)
```

```pycon
>>> (2*Integral(x, x)).doit()
x**2
```

```pycon
>>> (2*Integral(x, x)).doit(deep=False)
2*Integral(x, x)
```

### *class* sofia.ir.pde.WallModelReconstruct(\*args, \*\*kwargs)

Bases: `Token`

Reconstruct near-wall velocity from a wall function.

* **Parameters:**
  * **wall_fn** – Symbolic wall function in forward form `u+ = f(y+)`, expressed in
    terms of `y_plus`. e.g. Reichardt’s law. The inverse form
    (`y+ = g(u+)`, e.g. Spalding) is not accepted.
  * **y_plus** – The symbol used for `y+` inside `wall_fn`.
    (will refactor into sp.Lambda later)
  * **velocities** – The velocity components `(u, v, w)`, overwritten in place.
  * **geometry** – Geometry object.
  * **nu** – Kinematic viscosity.
  * **band** – Reconstruct only where the wall distance is below this.
  * **match_height** – Wall distance of the matching point used to infer `u_tau`.
    Must exceed `band`.
  * **n_iter** – Newton iterations for `u_tau`.
  * **max_dist** – Search radius passed to `wp.mesh_query_point`.

#### doit(deep=False, \*\*hints)

Evaluate objects that are not evaluated by default like limits,
integrals, sums and products. All objects of this kind will be
evaluated recursively, unless some species were excluded via ‘hints’
or unless the ‘deep’ hint was set to ‘False’.

```pycon
>>> from sympy import Integral
>>> from sympy.abc import x
```

```pycon
>>> 2*Integral(x, x)
2*Integral(x, x)
```

```pycon
>>> (2*Integral(x, x)).doit()
x**2
```

```pycon
>>> (2*Integral(x, x)).doit(deep=False)
2*Integral(x, x)
```

### *class* sofia.ir.pde.EddyViscositySmagorinsky(\*args, \*\*kwargs)

Bases: `Token`

Compute the Eddy viscosity for a given subgrid scale model.

Note: only the Smagorinsky model is supported right now.

* **Parameters:**
  * **velocities** – Tuple of (u,v,w) fields.
  * **nu_sgs** – The GridField to be used for storing the output.
  * **Cs** – Smagorinsky constant.
  * **strain_squaring** – 

    Mode of averaging:
    “edge”   (default) square each off-diagonal strain at its own edge, then
    > average to the centre. Preserves the adjointness that makes the
    > reported SGS dissipation equal the applied dissipation.
    > I.e. average-of-square.

    ”center” transfer the strains to the centre and square there. Reproduces
    : the square-of-average convention of SBP-derived formulations.

#### doit(deep=False, \*\*hints)

Evaluate objects that are not evaluated by default like limits,
integrals, sums and products. All objects of this kind will be
evaluated recursively, unless some species were excluded via ‘hints’
or unless the ‘deep’ hint was set to ‘False’.

```pycon
>>> from sympy import Integral
>>> from sympy.abc import x
```

```pycon
>>> 2*Integral(x, x)
2*Integral(x, x)
```

```pycon
>>> (2*Integral(x, x)).doit()
x**2
```

```pycon
>>> (2*Integral(x, x)).doit(deep=False)
2*Integral(x, x)
```

### *class* sofia.ir.pde.RKn(\*args, \*\*kwargs)

Bases: [`CustomToken`](sofia.ir.token.md#sofia.ir.token.CustomToken)

#### transform() → Basic

Represent this token in terms of other IR tokens.

All returned tokens must (eventually) expand to just tokens defined in
ir.base (e.g. ir.base.For), as the code generator is only defined
for those tokens. Note that tt is okay for this method to return other
custom tokens not from ir.base, as long as these tokens (possibly
through multiple .transform() calls) eventually expand to just
ir.base tokens.

Subclasses must override this method.

* **Returns:**
  The token in terms of other IR tokens.
* **Return type:**
  sympy.Basic
