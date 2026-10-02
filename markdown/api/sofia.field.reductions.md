# sofia.field.reductions

### *class* sofia.field.reductions.Reduce(\*args)

Bases: `Function`

Reduce a field to a scalar using the given operation (e.g. “min”, “max”, “mean”).

Supported operations: min, max, mean, sum, prod, all, any, std, var, median, norm.

For each operation there is also a short-hand available, e.g.
Reduce(expr, “min”) is equivalent to Min(expr).

* **Parameters:**
  * **expr** – The expression containing fields to reduce.
  * **operation** – The operation to use for the reduction.

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

### sofia.field.reductions.Min(expr: Expr, \*\*kwargs) → [Reduce](#sofia.field.reductions.Reduce)

Compute the minimum of `expr`, returning a scalar.

### sofia.field.reductions.Max(expr: Expr, \*\*kwargs) → [Reduce](#sofia.field.reductions.Reduce)

Compute the maximum of `expr`, returning a scalar.

### sofia.field.reductions.Mean(expr: Expr, \*\*kwargs) → [Reduce](#sofia.field.reductions.Reduce)

Compute the arithmetic mean of `expr`, returning a scalar.

### sofia.field.reductions.Sum(expr: Expr, \*\*kwargs) → [Reduce](#sofia.field.reductions.Reduce)

Compute the sum of `expr`, returning a scalar.

### sofia.field.reductions.Prod(expr: Expr, \*\*kwargs) → [Reduce](#sofia.field.reductions.Reduce)

Compute the product of `expr`, returning a scalar.

### sofia.field.reductions.All(expr: Expr, \*\*kwargs) → [Reduce](#sofia.field.reductions.Reduce)

Compute the logical AND of `expr`, returning a scalar.

### sofia.field.reductions.Any(expr: Expr, \*\*kwargs) → [Reduce](#sofia.field.reductions.Reduce)

Compute the logical OR of `expr`, returning a scalar.

### sofia.field.reductions.Std(expr: Expr, \*\*kwargs) → [Reduce](#sofia.field.reductions.Reduce)

Compute the standard deviation of `expr`, returning a scalar.

### sofia.field.reductions.Var(expr: Expr, \*\*kwargs) → [Reduce](#sofia.field.reductions.Reduce)

Compute the variance of `expr`, returning a scalar.

### sofia.field.reductions.Median(expr: Expr, \*\*kwargs) → [Reduce](#sofia.field.reductions.Reduce)

Compute the median of `expr`, returning a scalar.

### sofia.field.reductions.Norm(expr: Expr, \*\*kwargs) → [Reduce](#sofia.field.reductions.Reduce)

Compute the entrywise 2-norm of `expr`, returning a scalar.
