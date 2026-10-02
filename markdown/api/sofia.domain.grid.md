# sofia.domain.grid

### *class* sofia.domain.grid.DD(name, lower, upper, L)

Bases: `Function`

#### *classmethod* eval(name, lower, upper, L)

Returns a canonical form of cls applied to arguments args.

## Explanation

The `eval()` method is called when the class `cls` is about to be
instantiated and it should return either some simplified instance
(possible of some other class), or if the class `cls` should be
unmodified, return None.

Examples of `eval()` for the function “sign”

```python
@classmethod
def eval(cls, arg):
    if arg is S.NaN:
        return S.NaN
    if arg.is_zero: return S.Zero
    if arg.is_positive: return S.One
    if arg.is_negative: return S.NegativeOne
    if isinstance(arg, Mul):
        coeff, terms = arg.as_coeff_Mul(rational=True)
        if coeff is not S.One:
            return cls(coeff) * cls(terms)
```

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

### *class* sofia.domain.grid.Grid(ndim: Literal[1, 2, 3], name: str = 'G', discretisation_order: int = 2, boundaries: Numeric | tuple[Numeric | tuple[Numeric, Numeric], ...] | None = None, \_C: [CoordSys](sofia.domain.coordinate.md#sofia.domain.coordinate.CoordSys) | None = None, \_shape: tuple[Symbol | int, ...] | None = None, , transformations: \_\_annotationlib_name_1_\_ | Callable | None = None, transformations_inverse: \_\_annotationlib_name_2_\_ | Callable | None = None)

Bases: [`Domain`](sofia.domain.domain.md#sofia.domain.domain.Domain)

Symbolic representation of a structured grid.

This class defines a discretized grid in arbitrary dimensional space using symbolic
quantities. A `Grid` instance serves multiple purposes when building a simulation
code, but most directly it can be used to conveniently get symbolic coordinates and
indices, as well as symbolic grid shape and size specifications.

* **Parameters:**
  * **ndim** – Number of dimensions of the grid.
  * **name** – Name of the grid. This name is used to generate symbolic names for the grid
    coordinates, indices, and other symbolic quantities.
  * **discretisation_order** – Order of the discretisation scheme. Must be an even positive integer.
  * **boundaries** – Boundary specifications for each dimension in index space. Each dimension can be
    specified as either a single value (for periodic boundaries) or a pair of values
    (for non-periodic boundaries). The values represent the insets from first and
    last grid nodes. For example, a boundary specification of `(0.5, -2.5)` means
    that the boundary is at i = 0.5 and i = N - 1 - 2.5, where N is the number of
    grid points in that dimension.
  * **transformation** – A callable function that defines a transformation from the base coordinate
    system to a new coordinate system.

#### *property* uniform_axes

Axes whose scale factor is constant – these stay circulant/spectral.

#### *property* boundaries_absolute

Physical domain edges in index space.

#### *property* exterior_nodes

Number of nodes per dim that lie outside the physcal domain (ghost cells).

#### dim(coordinate_or_index: Symbol) → int

Return the dimension index of a coordinate.

#### concrete(shape: tuple[int, ...] | int, extent: tuple[Numeric | tuple[Numeric, Numeric] | list[Numeric], ...] | list[Numeric | tuple[Numeric, Numeric] | list[Numeric]] | Numeric | None = None, subs: dict | None = None) → dict[Symbol, int | float]

Return the substitutions corresponding to the concrete numbers of the grid.

* **Parameters:**
  * **shape** – Grid shape per dimension.
  * **extent** – Physical domain extents per dimension, given as concrete floats.
  * **subs** – Any additional substitutions.

### Examples

```pycon
>>> from sofia import Grid
```

```pycon
>>> G = Grid(ndim=2)
>>> G.concrete(shape=(100, 200), extent=((0, 1), (0, 2)))
{G.Nx: 100, G.Ny: 200, G.Lx: 1, G.Ly: 2, G.x0: 0, G.y0: 0, global.x: i/100, global.y: j/100}
```
