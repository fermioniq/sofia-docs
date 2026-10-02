# sofia.pde.fdm.derivative

### *class* sofia.pde.fdm.derivative.partial_unordered(func, , \*args, \*\*keywords)

Bases: `partial`

### *class* sofia.pde.fdm.derivative.Derivative(expr, \*variables, side: Literal['center', 'forward', 'backward'] = 'center', discretisation_order: int | None = None, allow_higher_order: bool = True, integer_stencils: bool | None = None, evaluate: bool = False, \_order_explicit: bool | None = None, allow_expand: bool | None = None, \_skip_sp=False, \*\*kwargs)

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

#### expand_terms(evaluate=False)

Differentiate and attach metadata.

Apply SymPy’s differentiation rules (product/chain/.. etc.) and
attach metadata to the resulting terms
(cannot be passed through SymPys evaluation machinery).

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

### sofia.pde.fdm.derivative.diff(expr: Expr, \*symbols: Symbol, side: Literal['center', 'forward', 'backward'] = 'center', discretisation_order: int | None = None, allow_higher_order: bool = True, integer_stencils: bool | None = None, upwind: bool = False, evaluate: bool = True, physical: bool = False, allow_expand: bool | None = None, \*\*kwargs) → Expr

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

### sofia.pde.fdm.derivative.diff_upwind(expr: Expr, \*symbols: Symbol, variable: Expr | Symbol | None = None, eps: float = 1e-06, discretisation_order: int | None = None, upwind_discretisation_order: int | None = None, side: Literal['center', 'forward', 'backward'] = 'center', integer_stencils: bool | None = None, allow_expand: bool | None = None, \*\*kwargs)

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

### sofia.pde.fdm.derivative.diff_finite(expr, wrt, order: int = 1, \*\*kwargs) → Expr

Discretised derivative with finite differences.

Equivalent to `discretise(diff(expr, coord, order))`: it differentiates
with respect to the coordinate of the requested dimension and immediately
applies the finite-difference stencil.
