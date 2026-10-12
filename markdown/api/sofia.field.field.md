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

It is a sympy vector so it can be used in any,vector expression.

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
