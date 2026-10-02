<a id="auto-disc-label"></a>

# Automatic Discretisation

## Unindexed and Indexed Fields

It is important to understand the distinction between the two ways of
referencing field values, as they are treated differently by the
discretisation engine.

**Unindexed fields** — `u`, `u.s(i=1/2)` — represent field values **without** reference to the subgrid
on which they are stored. Interpolation and discretisation are handled automatically.

**Indexed fields** — `u[i, j]`, `u[i+1, j]` — refer directly to
specific elements of the underlying storage array. They bypass all
automatic processing. This makes them useful when precise manual control is required, for instance when
implementing boundary conditions on specific array elements.

## Automatic Discretisation

Before entering the code generation core, every symbolic expression
passes through the `fix` function, which converts
symbolic expressions into their final discrete form. This function is
also a useful debugging tool for inspecting how the symbolic engine
processes expressions.

The `fix` function performs three steps:

1. **Field indexing.**
   Unindexed fields are converted to indexed array expressions whose
   indices correspond to the elements of the underlying storage.
   Staggered fields include fractional index offsets to reflect their
   position on the grid.
2. **Interpolation.**
   If an expression references a location where a field is not natively
   stored (e.g. a half-integer position), the value is replaced by an
   interpolation of the neighbouring stored values.
3. **Derivative discretisation.**
   Symbolic derivatives (produced by `diff()`) are replaced with
   finite-difference stencils at the specified discretisation order.

#### NOTE
The precedence for the discretisation order is (highest to lowest):
the `discretisation_order` argument on a specific `diff` call,
the `discretisation_order` of the `Field`, and finally the
`discretisation_order` of the `Grid`.

## Examples

```python
>>> from sofia import Grid, Field, Staggered
>>> from sofia.pde import fix, diff, diff_upwind
>>> G = Grid(ndim=2, discretisation_order=2)
>>> i, j = G.indices
>>> u = Field("u", domain=G)
>>> v = Field("v", domain=G, staggered=Staggered.XFACE)
```

**Field indexing** — on a centered field, `fix` simply adds array indices:

```python
>>> fix(u << u).rhs
u[i, j]
```

**Interpolation** — on a staggered field, the centered evaluation point does
not coincide with a stored value of `v`, so interpolation is inserted:

```python
>>> fix(u << v).rhs
v[i - 1, j]/2 + v[i, j]/2
```

Already-indexed fields are left unchanged, since they refer directly to
array elements:

```python
>>> fix(u << v[i, j]).rhs
v[i, j]
```

**Derivative discretisation** — symbolic derivatives are replaced by
finite-difference stencils. A second-order central difference:

```python
>>> fix(u << diff(u, G.x)).rhs
u[i + 1, j]/(2*G.dx) - u[i - 1, j]/(2*G.dx)
```

A second derivative:

```python
>>> fix(u << diff(u, G.x, 2)).rhs
u[i + 1, j]/G.dx**2 + u[i - 1, j]/G.dx**2 - 2*u[i, j]/G.dx**2
```

**Overriding the grid discretisation order** — a fourth-order stencil on a
second-order grid:

```python
>>> fix(u << diff(u, G.x, discretisation_order=4)).rhs
2*u[i + 1, j]/(3*G.dx) - u[i + 2, j]/(12*G.dx) - 2*u[i - 1, j]/(3*G.dx) + u[i - 2, j]/(12*G.dx)
```

**Stencil direction** — forward and backward differences:

```python
>>> fix(u << diff(u, G.x, side="forward", discretisation_order=1)).rhs
u[i + 1, j]/G.dx - u[i, j]/G.dx

>>> fix(u << diff(u, G.x, side="backward", discretisation_order=1)).rhs
-u[i - 1, j]/G.dx + u[i, j]/G.dx

>>> fix(u << diff(u, G.x, side="forward", discretisation_order=2)).rhs
2*u[i + 1, j]/G.dx - u[i + 2, j]/(2*G.dx) - 3*u[i, j]/(2*G.dx)
```

**Staggered derivatives** — derivatives of staggered fields automatically
account for the field’s position on the grid:

```python
>>> fix(u << diff(v, G.x)).rhs
-v[i - 1, j]/G.dx + v[i, j]/G.dx
```

**Field offsets** — half-integer shifts introduced by `.s()` are
resolved by interpolation:

```python
>>> fix(u << u.s(1/2, 0)).rhs
u[i + 1, j]/2 + u[i, j]/2

>>> fix(u << v.s(1/2, 0)).rhs  # v is staggered, so the shift references a location where v is stored
v[i, j]

>>> fix(u << u.s(1/2, 1/2)).rhs
u[i + 1, j + 1]/4 + u[i + 1, j]/4 + u[i, j + 1]/4 + u[i, j]/4
```

**Combined expressions** — all three steps are applied together. For
example, a product of differently-staggered fields with a derivative:

```python
>>> fix(u << diff(u * v, G.x)).rhs
(u[i + 1, j]/(2*G.dx) - u[i - 1, j]/(2*G.dx))*(v[i - 1, j]/2 + v[i, j]/2) + (-v[i - 1, j]/G.dx + v[i, j]/G.dx)*u[i, j]
```
