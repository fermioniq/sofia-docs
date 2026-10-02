# sofia.pde.fdm package

## Submodules

* [sofia.pde.fdm.derivative](sofia.pde.fdm.derivative.md)
  * [`partial_unordered`](sofia.pde.fdm.derivative.md#sofia.pde.fdm.derivative.partial_unordered)
  * [`Derivative`](sofia.pde.fdm.derivative.md#sofia.pde.fdm.derivative.Derivative)
    * [`Derivative.expand_terms()`](sofia.pde.fdm.derivative.md#sofia.pde.fdm.derivative.Derivative.expand_terms)
    * [`Derivative.func()`](sofia.pde.fdm.derivative.md#sofia.pde.fdm.derivative.Derivative.func)
    * [`Derivative.as_finite_difference()`](sofia.pde.fdm.derivative.md#sofia.pde.fdm.derivative.Derivative.as_finite_difference)
  * [`diff()`](sofia.pde.fdm.derivative.md#sofia.pde.fdm.derivative.diff)
  * [`diff_upwind()`](sofia.pde.fdm.derivative.md#sofia.pde.fdm.derivative.diff_upwind)
  * [`diff_finite()`](sofia.pde.fdm.derivative.md#sofia.pde.fdm.derivative.diff_finite)
* [sofia.pde.fdm.discretisation](sofia.pde.fdm.discretisation.md)
  * [`as_finite_diff()`](sofia.pde.fdm.discretisation.md#sofia.pde.fdm.discretisation.as_finite_diff)
* [sofia.pde.fdm.stencils](sofia.pde.fdm.stencils.md)
  * [`StencilSpec`](sofia.pde.fdm.stencils.md#sofia.pde.fdm.stencils.StencilSpec)
  * [`stencil_points()`](sofia.pde.fdm.stencils.md#sofia.pde.fdm.stencils.stencil_points)

## Module contents

### *class* sofia.pde.fdm.Derivative(expr, \*variables, side: Literal['center', 'forward', 'backward'] = 'center', discretisation_order: int | None = None, allow_higher_order: bool = True, integer_stencils: bool | None = None, evaluate: bool = False, \_order_explicit: bool | None = None, allow_expand: bool | None = None, \_skip_sp=False, \*\*kwargs)

Bases: `Derivative`

A symbolic derivative class that extends `sympy.Derivative`.

This class extends `sympy.Derivative` with additional
metadata for finite-difference discretisation. Supports stencil direction,
order, and higher-order derivative handling.

* **Parameters:**
  * **expr** – Expression to differentiate.
  * **\*variables** – Variables and optional orders for differentiation.
  * **side** (*Literal* *[* *'center'* *,*  *'forward'* *,*  *'backward'* *]*) – Stencil direction for finite-difference approximation.
  * **discretisation_order** (*int* *|* *None*) – Accuracy order for finite-difference stencil.
  * **allow_higher_order** (*bool*) – Enable higher-order stencil use.
  * **\*\*kwargs** – Additional keyword arguments passed to `sympy.Derivative`.

#### side *: Literal['center', 'forward', 'backward']*

#### discretisation_order *: int | None*

#### allow_higher_order *: bool*

#### integer_stencils *: bool | None*

#### allow_expand *: bool | None*

#### expand_terms(evaluate=False)

Differentiate and attach metadata.

Apply SymPy’s differentiation rules (product/chain/.. etc.) and
attach metadata to the resulting terms
(cannot be passed through SymPys evaluation machinery).

#### *classmethod* from_derivative(deriv)

#### func(\*args, \*\*kwargs)

The top-level function in an expression.

The following should hold for all objects:

```default
>> x == x.func(*x.args)
```

### Examples

```pycon
>>> from sympy.abc import x
>>> a = 2*x
>>> a.func
<class 'sympy.core.mul.Mul'>
>>> a.args
(2, x)
>>> a.func(*a.args)
2*x
>>> a == a.func(*a.args)
True
```

#### as_finite_difference(points=None, lhs=None, wrt=None)

Return a finite-difference approximation of this derivative.

# See `pde.fdm.as_finite_diff` for full documentation
#

#### default_assumptions *= {}*

### *class* sofia.pde.fdm.StencilSpec(derivative_order: int, discretisation_order: int, side: Literal['center', 'forward', 'backward'], integer_stencils: bool, offset_eval: sympy.core.expr.Expr)

Bases: `object`

#### derivative_order *: int*

#### discretisation_order *: int*

#### side *: Literal['center', 'forward', 'backward']*

#### integer_stencils *: bool*

#### offset_eval *: Expr*

### sofia.pde.fdm.as_finite_diff(expr, points=None, wrt=None, lhs=None, integer_stencils=None)

Compute a finite-difference approximation of a derivative.

This function returns a symbolic expression approximating a derivative
of a function using a finite difference formula. The derivative is
expressed as a weighted sum of the function evaluated at discrete
points of the independent variable(s).

* **Parameters:**
  * **expr** – The derivative to approximate.
  * **points** – 
    - If a sequence: the discrete values of the independent variable
      used to generate the finite difference weights (length >= order + 1).
    - If a scalar: treated as the step size to generate an equidistant
      sequence of length order + 1 centered around x0.
    - Default: 1 (unit step size).
  * **lhs** – Symbol of field representing the left-hand side, in which the result
    of the derivative is to be stored.
  * **wrt** – Variable with respect to which the derivative is taken.
    Required for partial derivatives if derivative is not ordinary.
    Default: `None`.
  * **integer_stencils** – Allows the use of optimised (integer stencils) or fall back to general stencils
    made from half-integer steps.
* **Returns:**
  Symbolic expression representing the finite-difference approximation
  of the derivative, as a weighted sum of the function evaluated at
  discrete points.
* **Return type:**
  sympy.Expr

#### SEE ALSO
`sympy.calculus.finite_diff.apply_finite_diff`, `sympy.calculus.finite_diff.finite_diff_weights`

### sofia.pde.fdm.diff(expr: Expr, \*symbols: Symbol, side: Literal['center', 'forward', 'backward'] = 'center', discretisation_order: int | None = None, allow_higher_order: bool = True, integer_stencils: bool | None = None, upwind: bool = False, evaluate: bool = True, physical: bool = False, allow_expand: bool | None = None, \*\*kwargs) → Expr

Differentiate expression with respect to one or more variables.

This is an extension of the `sympy.diff` interface that allows for more control
over discretisation.

* **Parameters:**
  * **expr** – Expression to be differentiated.
  * **side** – Specify stencil direction after discretisation. `center` (default) corresponds
    to central difference, `"forward"` to forward differencing using points ahead,
    and `"backward"` to backward differencing using points behind the target
    location.
  * **discretisation_order** – Specify stencil discretisation accuracy order.
  * **allow_higher_order** – If `True`, stencils of higher differentiation order are used when possible.
    Otherwise  higher-order derivatives are discretised as derivatives of
    derivatives.
  * **upwind** – If `True`, applies upwind differencing based on the sign of the expression.
  * **\*symbols** – See `sympy.diff` for the interface.
  * **evaluate** – Allows SymPy automatic evaluation.
  * **integer_stencils** – Allows the use of optimised (integer stencils) or fall back to general stencils
    made from half-integer steps.
  * **physical** – Take a derivative wrt the physical coordinates. This will insert lame
    coefficients, similar to the vector derivative operators.
  * **allow_expand** – If `True`, product rule expansion is applied (before discretisation). The
    default `None` takes its value from the `derivatives_allow_expand` context
    var, which defaults to `True` and can be set from higher-level operators like
    gradient.
  * **\*\*kwargs** – 

    …
* **Returns:**
  Derivative of `expr`.
* **Return type:**
  sp.Expr

### sofia.pde.fdm.diff_finite(expr, wrt, order: int = 1, \*\*kwargs) → Expr

Discretised derivative with finite differences.

Equivalent to `discretise(diff(expr, coord, order))`: it differentiates
with respect to the coordinate of the requested dimension and immediately
applies the finite-difference stencil.

### sofia.pde.fdm.diff_upwind(expr: Expr, \*symbols: Symbol, variable: Expr | Symbol | None = None, eps: float = 1e-06, discretisation_order: int | None = None, upwind_discretisation_order: int | None = None, side: Literal['center', 'forward', 'backward'] = 'center', integer_stencils: bool | None = None, allow_expand: bool | None = None, \*\*kwargs)

Return an upwind finite-difference approximation of a derivative.

The stencil selection is defined as:

- forward difference if `expr < -eps`
- backward difference if `expr > eps`
- centered difference otherwise

Forward and backward stencils by default use the same order as the
centered stencil: `discretisation_order`.
This can be overridden by specifying `upwind_discretisation_order`.

* **Parameters:**
  * **expr** – Expression to differentiate. Its sign determines the upwind direction.
  * **\*symbols** – One or more symbols with respect to which to differentiate.
  * **variable** – Expression whose sign determines the upwind direction. Defaults to `expr`
    if not specified, i.e. the sign of the expression being differentiated.
  * **eps** – Small tolerance used to determine the sign of `expr`.
    Defaults to 1e-6.
  * **discretisation_order** – Order of accuracy of the finite-difference stencil.
    Forward and backward stencils use one order lower.
    Defaults to 2.
  * **upwind_discretisation_order** – Order of accuracy of the forward and backward stencils.
    If not specified, defaults to `discretisation_order`.
  * **side** (`{'center', 'forward', 'backward'}`, default 

    ```
    ``
    ```

    ’center’\`) – Default stencil direction if `expr` is within `eps` of zero.
  * **integer_stencils** – Allows the use of optimised (integer stencils) or fall back to general stencils
    made from half-integer steps.
  * **allow_expand** – If `True`, product rule expansion is applied (before discretisation). The default
    `None` takes its value from the `derivatives_allow_expand` context var, which
    defaults to `True` and can be set from higher-level operators like gradient.
  * **\*\*kwargs** – Additional keyword arguments passed to `diff`.
* **Returns:**
  Symbolic finite-difference approximation using upwind stencil selection.
* **Return type:**
  sp.Expr

### Examples

```pycon
>>> from sofia import Grid, Field
>>> from sofia.pde import diff_upwind
```

```pycon
>>> G = Grid(ndim=1)
>>> u = Field("u", domain=G)
```

```pycon
>>> diff_upwind(u, G.x)
Piecewise(
    (Derivative(u, x, side='forward', discretisation_order=2), u < -1e-6),
    (Derivative(u, x, side='backward', discretisation_order=2), u > 1e-6),
    (Derivative(u, x, side='center', discretisation_order=2), True)
```

```pycon
>>> diff_upwind(u, G.x, discretisation_order=4, upwind_discretisation_order=2)
Piecewise(
    (Derivative(u, x, side='forward', discretisation_order=2), u < -1e-6),
    (Derivative(u, x, side='backward', discretisation_order=2), u > 1e-6),
    (Derivative(u, x, side='center', discretisation_order=4), True)
)
```

### sofia.pde.fdm.discretise(expr, lhs=None)

### sofia.pde.fdm.stencil_points(coord, dd, spec: [StencilSpec](sofia.pde.fdm.stencils.md#sofia.pde.fdm.stencils.StencilSpec)) → list

Return the points at which the function should be evaluated.

These points can be used for a finite difference approximation of a derivative.
