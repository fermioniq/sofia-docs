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
