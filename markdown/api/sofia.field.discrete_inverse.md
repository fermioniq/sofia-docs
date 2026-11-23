# sofia.field.discrete_inverse

### *class* sofia.field.discrete_inverse.DiscreteInverse(field: Basic, v: Basic)

Bases: `Function`

Continuous index at which a GridField `field` reaches a value `v`.

`field` must vary along one axis, be unshifted and cell-centred. The array
passed for `field` at runtime must also be strictly increasing; this is not
checked and left to the user to enforce.

We find the solution by linear interpolation. Let the entries of `field` be
`f[0], ..., f[N-1]`, then the node stands for the value

> j + (v - f[j]) / (f[j+1] - f[j])

with `j` such that `f[j] <= v < f[j+1]`. If `v` is outside the range of `f`,
set `j=0` or `j=N-2`, when `v<f[0]` or `v>=f[N-1]`, respectively.
This choice extrapolates the field linearly beyond its range.

If `field` is a `PeriodicCoordField` with period `P`, `v` is first wrapped
into `[f[0], f[0] + P)`,

> v -> v - P \* floor((v - f[0]) / P),

and `j` ranges over `0 <= j <= N - 1`, with `f[N] = f[0] + P`. In this case
the array passed for `field` must also satisfy `f[N-1] - f[0] < P`, which is not
checked either.

`field` stands for the whole array, i.e. not the indexed field, so a substitution
of a grid index or coordinate acts on `v` only.

* **Parameters:**
  * **field** – Field along one axis of a `Grid`.
  * **v** – Value at which `field` is inverted.

#### *classmethod* eval(field: Basic, v: Basic) → None

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

#### *property* field *: [GridField](sofia.field.grid_field.md#sofia.field.grid_field.GridField)*

Field that is inverted.

#### *property* axis *: int*

Axis of the grid along which `field` varies.

#### *property* v *: Expr*

Value at which `field` is inverted.

#### *property* period *: Expr | None*

Period `P` of `field`, or `None` if its axis is not periodic.
