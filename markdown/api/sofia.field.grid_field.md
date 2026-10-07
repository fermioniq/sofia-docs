# sofia.field.grid_field

### *class* sofia.field.grid_field.IndexedGridField(base, \*args, \*\*kw_args)

Bases: `Indexed`

### *class* sofia.field.grid_field.GridField(name: str, domain: [Grid](sofia.domain.grid.md#sofia.domain.grid.Grid), staggered: [Staggered](sofia.field.staggered.md#sofia.field.staggered.Staggered) = Center, discretisation_order: int = 0, axes: Tuple | tuple[Symbol | int, ...] = None, \_shifts: Tuple | tuple[Symbol | float | int, ...] | None = None)

Bases: [`Field`](sofia.field.field.md#sofia.field.field.Field)

Symbolic representation of a field living on a structured grid.

A `GridField` is a symbol that carries the information needed to discretise
expressions it appears in: the grid it is defined on, which of the grid axes it
varies along, where within a cell its values sit, and how far it is shifted from
its natural location.

* **Parameters:**
  * **name** – Name of the field, used for its symbolic and generated-code names.
  * **domain** – The `Grid` the field is defined on.
  * **staggered** – Location of the field values within a cell, as a
    [`Staggered`](sofia.field.staggered.md#sofia.field.staggered.Staggered) member. Defaults to
    `Staggered.CENTER`.
  * **discretisation_order** – Order of accuracy for derivatives and interpolation of this field. Must be
    even. The default of `0` means the order of `domain` is used instead.
  * **axes** – Grid axes the field varies along, given as indices or coordinates. Defaults
    to all of them. Passing a subset gives a field of lower dimensionality than
    the grid; passing `()` gives a scalar.

### Examples

```pycon
>>> import sympy as sp
>>> from sofia import Grid, Field, Staggered
```

```pycon
>>> G = Grid(ndim=2)
>>> u = Field("u", domain=G)
```

```pycon
>>> u.shape
(G.Nx, G.Ny)
>>> u.axes
(0, 1)
>>> Field("v", domain=G, axes=(G.x,)).shape
(G.Nx,)
>>> Field("dt", domain=G, axes=()).shape
()
```

Shifts:

```pycon
>>> u.s(1, 0).shifts
(1, 0)
>>> u.s(1, 0).s(2, 0).shifts
(3, 0)
>>> u.s(j=-1).shifts
(0, -1)
```

#### fractional_offset(wrt) → Expr

Combine shift and staggered offset of a field.

### *class* sofia.field.grid_field.GridFieldLocus(grid: [Grid](sofia.domain.grid.md#sofia.domain.grid.Grid), axes: Tuple, shifts: Tuple)

Bases: [`Locus`](sofia.field.locus.md#sofia.field.locus.Locus)

#### *property* shifts *: Tuple*

Tuple of (half-)integer shifts.
