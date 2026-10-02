# sofia.field.sums

### *class* sofia.field.sums.FieldAxisSum(\*args)

Bases: `Function`

#### *classmethod* eval(\*args)

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

### *class* sofia.field.sums.FieldAxisCumSum(\*args)

Bases: `Function`

#### *classmethod* eval(\*args)

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

### sofia.field.sums.sum_axis(expr: Expr, \*coords, mask_exterior: bool = True, cumulative: bool = False, start: Literal['lower', 'upper'] | None = None) → [FieldAxisSum](#sofia.field.sums.FieldAxisSum) | [FieldAxisCumSum](#sofia.field.sums.FieldAxisCumSum)

Sum the field over the given coordinate axes.

### sofia.field.sums.integrate_axis(expr: Expr, \*coords, weights_scheme: WeightsScheme = None, cumulative: bool = False, start: Literal['lower', 'upper'] | None = None) → [FieldAxisSum](#sofia.field.sums.FieldAxisSum) | [FieldAxisCumSum](#sofia.field.sums.FieldAxisCumSum)

Integrate over the given axes.
