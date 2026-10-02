# Field Expressions

Fields can be used directly in mathematical expressions without having
to specify indices. Any unindexed [`Field`](../api/sofia.field.field.md#sofia.field.field.Field) can be used freely
without having to care about the underlying subgrid on which they are defined
(i.e. their staggeredness):

```python
>>> import sympy as sp
>>> from sofia import Grid, Field, Staggered

>>> G = Grid(ndim=2)
>>> u = Field("u", domain=G)
>>> v = Field("v", domain=G, staggered=Staggered.XFACE)

>>> f = u**2 + sp.sin(v) + sp.exp(u * v)
```

## Differentiation

Writing discrete equations from continuous differential equations is tedious and error-prone.
The symbolic engine is set up to perform these discretisations automatically for any differential equation at any discretisation accuracy order.

This functionality is provided through the [`sofia.pde.fdm.diff()`](../api/sofia.pde.fdm.md#sofia.pde.fdm.diff) function, which extends the standard
`sympy.diff` to support extra features.

The `diff` function computes derivatives of expressions with respect to one or more
variables. For example computing a the derivative of expression `f`
with respect to `x`:

```python
>>> from sofia.pde import diff

>>> f = u * v
>>> diff(f, G.x)
Derivative(u, global.x)*v + Derivative(v, global.x)*u
```

Higher-order derivatives can be computed by specifying the order, e.g for second order:

```python
>>> diff(f, G.x, 2)
2*Derivative(u, global.x)*Derivative(v, global.x) + Derivative(u, (global.x, 2))*v + Derivative(v, (global.x, 2))*u
```

Partial derivatives for multivariable expressions are supported as are mixed partial derivatives:

```python
>>> diff(f, G.x, G.y)
Derivative(u, global.x)*Derivative(v, global.y) + Derivative(u, global.y)*Derivative(v, global.x) + Derivative(u, global.x, global.y)*v + Derivative(v, global.x, global.y)*u
```

Here `diff(f, x, y)` computes the mixed partial derivative $\frac{\partial^2 f}{\partial x \, \partial y}$.

The diff function also supports an `allow_expand=False` option:

```python
>>> diff(f, G.x, allow_expand=False)
Derivative(u*v, global.x, allow_expand=False)
```

When used it returns the derivative in a symbolic
unevaluated form, meaning it will not automatically expand or apply
rules like the product rule.

### Discretisation Order and Stencil Direction

The derivatives produced by `diff` are automatically discretised into
finite-difference stencils. The accuracy order is determined by the
`discretisation_order` parameter, which controls how many grid points are
used in the stencil. Higher orders increase accuracy at the cost of wider
stencils. The order can be specified at three levels (highest precedence
first): on a specific `diff` call, on the `Field`, or on the `Grid`.

The `side` argument controls the direction of the finite-difference stencil:

- `"center"` (default) — symmetric central difference, highest accuracy
  for a given stencil width.
- `"forward"` — points ahead of the evaluation point.
- `"backward"` — points behind the evaluation point.

For a full description of how derivatives, interpolation, and field indexing
are resolved into discrete array expressions, see the [Automatic Discretisation](auto_discretisation.md#auto-disc-label) section.

### Upwinding

Upwind derivatives can be computed using the `upwind` option in `diff`.
This produces a symbolic `Piecewise` expression that automatically selects the derivative stencil
based on the sign of the expression:

- Forward difference if the expression is negative
- Backward difference if the expression is positive
- Centered difference otherwise

```python
>>> diff(u, G.x, upwind=True)
```

Output:

```python
Piecewise(
    (Derivative(u, global.x, side='forward', discretisation_order=1), u < -1e-6),
    (Derivative(u, global.x, side='backward', discretisation_order=1), u > 1e-6),
    (Derivative(u, global.x, discretisation_order=2), True)
)
```

By default, the forward/backward stencils use order `discretisation_order - 1`, while the centered
stencil uses order `discretisation_order`. The stability of finite difference solvers is a complicated topic and differently “sided”
discretisation stencils can show different behaviour. Using a lower-order accurate stencil for the forward/backward derivatives is a common occurence.
For more control, the [`sofia.pde.fdm.diff_upwind()`](../api/sofia.pde.fdm.md#sofia.pde.fdm.diff_upwind) can be called directly and the `upwind_discretisation_order` argument can be used to override
the default of `discretisation_order - 1`. The `variable` parameter allows the upwind direction to be determined by the sign of an
arbitrary expression instead of the field being differentiated.

```python
>>> from sofia.pde import diff_upwind
>>> diff_upwind(u, G.x, discretisation_order=4, upwind_discretisation_order=2)
```

Output:

```python
Piecewise(
    (Derivative(u, global.x, side='forward', discretisation_order=2), u < -1e-6),
    (Derivative(u, global.x, side='backward', discretisation_order=2), u > 1e-6),
    (Derivative(u, global.x, discretisation_order=4), True)
)
```

## Example

For the 2D convection-diffusion equation in component form:

$$
\displaystyle \frac{\partial u}{\partial t} = \frac{\partial^2 u}{\partial x^2} + \frac{\partial^2 u}{\partial y^2} - \frac{\partial}{\partial x}(u^2) - \frac{\partial}{\partial y}(uv), \\[1em]
\displaystyle \frac{\partial v}{\partial t} = \frac{\partial^2 v}{\partial x^2} + \frac{\partial^2 v}{\partial y^2} -  \frac{\partial}{\partial y}(v^2) - \frac{\partial}{\partial x}(uv).
$$

the right hand sides can be written as:

```python
>>> F_u = diff(u, G.x, 2) + diff(u, G.y, 2) - diff(u**2, G.x) - diff(u*v, G.y)
>>> F_v = diff(v, G.x, 2) + diff(v, G.y, 2) - diff(v**2, G.y) - diff(u*v, G.x)
```

which is displayed as:

$$
\displaystyle - 2 \frac{\partial u}{\partial \mathbf{{x}_{G}}} u + \frac{\partial^{2} u}{\partial \mathbf{{x}_{G}}^{2}} - \frac{\partial u}{\partial \mathbf{{y}_{G}}} v + \frac{\partial^{2} u}{\partial \mathbf{{y}_{G}}^{2}} - \frac{\partial v}{\partial \mathbf{{y}_{G}}} u
$$

$$
\displaystyle - \frac{\partial u}{\partial \mathbf{{x}_{G}}} v - \frac{\partial v}{\partial \mathbf{{x}_{G}}} u + \frac{\partial^{2} v}{\partial \mathbf{{x}_{G}}^{2}} - 2 \frac{\partial v}{\partial \mathbf{{y}_{G}}} v + \frac{\partial^{2} v}{\partial \mathbf{{y}_{G}}^{2}}
$$

#### NOTE
Mathematical expressions are processed by SymPy, which automatically performs certain simplifications (without changing the mathematical meaning), which can lead to unexpected rearrangements.
See also the [SymPy documentation](https://docs.sympy.org/latest/explanation/gotchas.html#symbolic-expressions)  for more details.

## Vector Equations

The above example is much simpler to write in vectorized form:

$$
\frac{\partial \mathbf{u}}{\partial t} = \Delta \mathbf{u} - \nabla (\mathbf{u} \otimes \mathbf{u}),
\quad \mathbf{u} = u \, \hat{i} + v \, \hat{j}
$$

by using the `sofia.domain.grid.Grid.ihat` and `sofia.domain.grid.Grid.jhat` unit vectors:

```python
>>> from sofia.pde import laplacian, divergence

# Define vector field
>>> uv = u * G.ihat + v * G.jhat

# (x | y) is shorthand for the outer product of x and y
>>> F_uv = laplacian(uv) - divergence(uv | uv)
```

After composing the vector expression, the individual components can be
extracted by `F_uv.coeff(...)`:

```python
>>> F_u = F_uv.coeff(G.ihat)
>>> F_v = F_uv.coeff(G.jhat)
```

Which gives the same result as in the example above:

$$
\displaystyle - 2 \frac{\partial u}{\partial \mathbf{{x}_{G}}} u + \frac{\partial^{2} u}{\partial \mathbf{{x}_{G}}^{2}} - \frac{\partial u}{\partial \mathbf{{y}_{G}}} v + \frac{\partial^{2} u}{\partial \mathbf{{y}_{G}}^{2}} - \frac{\partial v}{\partial \mathbf{{y}_{G}}} u
$$

$$
\displaystyle - \frac{\partial u}{\partial \mathbf{{x}_{G}}} v - \frac{\partial v}{\partial \mathbf{{x}_{G}}} u + \frac{\partial^{2} v}{\partial \mathbf{{x}_{G}}^{2}} - 2 \frac{\partial v}{\partial \mathbf{{y}_{G}}} v + \frac{\partial^{2} v}{\partial \mathbf{{y}_{G}}^{2}}
$$

These expressions are also automatically discretized taking into account the staggeredness of the fields, see the [Automatic Discretisation](auto_discretisation.md#auto-disc-label) section for more detail.

## Field shifts

Half-integer (or integer) offsets can be applied to any field using the
`Field.s` method. This shifts the
logical position at which the field is evaluated, without indexing into
the underlying array directly. This is useful when you need to reference
neighbouring values — for instance in boundary conditions or conditional expressions — without dropping down to the indexed interface.

```python
u
u.s(1, 0)
u.s(1, -1/2)
u.s(j=2)
```

$$
\displaystyle u
$$

$$
\displaystyle u_{\mathtt{\text{i}} \to \mathtt{\text{i}} + 1}
$$

$$
\displaystyle u_{\mathtt{\text{i}} \to \mathtt{\text{i}} + 1,\mathtt{\text{j}} \to \mathtt{\text{j}} - \frac{1}{2}}
$$

$$
\displaystyle u_{\mathtt{\text{j}} \to \mathtt{\text{j}} + 2}
$$

When the expression is discretised, any half-integer positions that do not
coincide with stored data points are resolved by interpolation automatically.
