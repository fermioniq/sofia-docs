# sofia.field.field

### *class* sofia.field.field.Field(name, domain, \*args, \*\*kwargs)

Bases: `Expr`

Symbolic field living on `Domain`.

#### as_dummy(name=None)

Return the expression with any objects having structurally
bound symbols replaced with unique, canonical symbols within
the object in which they appear and having only the default
assumption for commutativity being True. When applied to a
symbol a new symbol having only the same commutativity will be
returned.

### Examples

```pycon
>>> from sympy import Integral, Symbol
>>> from sympy.abc import x
>>> r = Symbol('r', real=True)
>>> Integral(r, (r, x)).as_dummy()
Integral(_0, (_0, x))
>>> _.variables[0].is_real is None
True
>>> r.as_dummy()
_r
```

### Notes

Any object that has structurally bound variables should have
a property, `bound_symbols` that returns those symbols
appearing in the object.

### *class* sofia.field.field.VectorField(name: str | Symbol, domain: [Domain](sofia.md#sofia.Domain), staggered: str | tuple[[Staggered](sofia.field.staggered.md#sofia.field.staggered.Staggered), ...] | None = None, \*\*kwargs)

### *class* sofia.field.field.VectorField(\*terms: Vector, \*\*options)

Bases: `VectorAdd`

A vector field composed of one scalar Field per axis.

It is a sympy vector so it can be used in any vector expression.

* **Parameters:**
  * **name** – Each component field is named f”{name}_{axis}”
    (where axis is “x”, “y” or “z”).
  * **domain** – Domain the component fields live on.
  * **staggered** – Where the components sit within a cell on a Grid. This can be given
    either as a Staggered per component or as the string “faces”, which means
    each component lives on the face normal to its axis.
  * **\*\*kwargs** – Forwarded to every component Field.

### Examples

```pycon
>>> from sofia import Grid
>>> G = Grid(ndim=2, name="G")
>>> u = VectorField("u", G)
>>> u
u_x*global.ihat + u_y*global.jhat
>>> u.fields
(u_x, u_y)
>>> [f.staggered for f in VectorField("m", G, staggered="faces").fields]
[XFACE, YFACE]
>>> u << 0
CodeBlock(
EvalAssignment(u_x, 0),
EvalAssignment(u_y, 0)
)
```

#### *property* fields *: tuple[[Field](#sofia.field.field.Field), ...]*

The scalar field of each component.

#### assignments(other) → dict[[Field](#sofia.field.field.Field), Expr]

Return the value each component field gets when other is assigned.

### *class* sofia.field.field.TensorField(name: str | Symbol, domain: [Domain](sofia.md#sofia.Domain), symmetric: bool = False, staggered: str | tuple[tuple[[Staggered](sofia.field.staggered.md#sofia.field.staggered.Staggered) | None, ...], ...] | None = None, \*\*kwargs)

### *class* sofia.field.field.TensorField(\*terms: Dyadic, \*\*options)

Bases: `DyadicAdd`

An order 2 tensor field (dyadic) composed of one scalar Field per pair of axes.

It is a sympy dyadic, so it can be used in any dyadic expression.

* **Parameters:**
  * **name** – Each component field is named f”{name}_{a}{b}” (e.g. T_xy).
  * **domain** – Domain the component fields live on.
  * **symmetric** – Use one field for T_ab and T_ba, so the tensor is symmetric by construction.
  * **staggered** – Where the components sit within a cell on a Grid. This can be given
    either as one Staggered per component, as rows (staggered[a][b] for
    component T_ab), or as the string “mac”, which puts the diagonal
    components at cell centers and the off-diagonal ones on the edges.
  * **\*\*kwargs** – Forwarded to every component Field.

### Examples

```pycon
>>> from sofia import Grid
>>> G = Grid(ndim=2, name="G")
>>> T = TensorField("T", G)
>>> T.fields
(T_xx, T_xy, T_yx, T_yy)
>>> T.dot(G.unit_vectors[0])
T_xx*global.ihat + T_yx*global.jhat
>>> S = TensorField("S", G, symmetric=True)
>>> S.fields
(S_xx, S_xy, S_yy)
>>> S << 2
CodeBlock(
EvalAssignment(S_xx, 2),
EvalAssignment(S_xy, 2),
EvalAssignment(S_yy, 2)
)
```

#### *property* fields *: tuple[[Field](#sofia.field.field.Field), ...]*

The distinct component fields.

#### *property* symmetric *: bool*

Whether T_ab and T_ba are the same field for every pair of axes.

#### assignments(other) → dict[[Field](#sofia.field.field.Field), Expr]

Return the value each component field gets when other is assigned.

### sofia.field.field.component_fields(symbols) → tuple

Return symbols as a tuple, with vector and tensor fields split into fields.

### sofia.field.field.fields(names: str, domain: [Domain](sofia.md#sofia.Domain), \*\*kwargs: object) → tuple[[Field](#sofia.field.field.Field), ...]

Create one or more Fields on a shared domain.

Equivalent to `sympy.symbols()`, but for Fields. Always returns
a tuple for better type support.

* **Parameters:**
  * **names** – Field names, comma- or whitespace-separated; see `sympy.symbols()`.
  * **domain** – Domain the fields live on.
  * **\*\*kwargs** – Forwarded to the Field subclass registered for `domain`.
* **Returns:**
  A tuple of the created Field(s).
* **Return type:**
  tuple[[Field](#sofia.field.field.Field), …]

### Examples

```pycon
>>> (u,) = fields("u", domain=Grid(ndim=1))
>>> v, w = fields("v w", domain=Grid(ndim=2))
```
