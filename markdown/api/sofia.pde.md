# sofia.pde package

## Subpackages

* [sofia.pde.fdm package](sofia.pde.fdm.md)
  * [Submodules](sofia.pde.fdm.md#submodules)
    * [sofia.pde.fdm.derivative](sofia.pde.fdm.derivative.md)
      * [`partial_unordered`](sofia.pde.fdm.derivative.md#sofia.pde.fdm.derivative.partial_unordered)
      * [`Derivative`](sofia.pde.fdm.derivative.md#sofia.pde.fdm.derivative.Derivative)
      * [`diff()`](sofia.pde.fdm.derivative.md#sofia.pde.fdm.derivative.diff)
      * [`diff_upwind()`](sofia.pde.fdm.derivative.md#sofia.pde.fdm.derivative.diff_upwind)
      * [`diff_finite()`](sofia.pde.fdm.derivative.md#sofia.pde.fdm.derivative.diff_finite)
    * [sofia.pde.fdm.discretisation](sofia.pde.fdm.discretisation.md)
      * [`as_finite_diff()`](sofia.pde.fdm.discretisation.md#sofia.pde.fdm.discretisation.as_finite_diff)
    * [sofia.pde.fdm.stencils](sofia.pde.fdm.stencils.md)
      * [`StencilSpec`](sofia.pde.fdm.stencils.md#sofia.pde.fdm.stencils.StencilSpec)
      * [`stencil_points()`](sofia.pde.fdm.stencils.md#sofia.pde.fdm.stencils.stencil_points)
  * [Module contents](sofia.pde.fdm.md#module-sofia.pde.fdm)
    * [`Derivative`](sofia.pde.fdm.md#sofia.pde.fdm.Derivative)
      * [`Derivative.side`](sofia.pde.fdm.md#sofia.pde.fdm.Derivative.side)
      * [`Derivative.discretisation_order`](sofia.pde.fdm.md#sofia.pde.fdm.Derivative.discretisation_order)
      * [`Derivative.allow_higher_order`](sofia.pde.fdm.md#sofia.pde.fdm.Derivative.allow_higher_order)
      * [`Derivative.integer_stencils`](sofia.pde.fdm.md#sofia.pde.fdm.Derivative.integer_stencils)
      * [`Derivative.allow_expand`](sofia.pde.fdm.md#sofia.pde.fdm.Derivative.allow_expand)
      * [`Derivative.expand_terms()`](sofia.pde.fdm.md#sofia.pde.fdm.Derivative.expand_terms)
      * [`Derivative.from_derivative()`](sofia.pde.fdm.md#sofia.pde.fdm.Derivative.from_derivative)
      * [`Derivative.func()`](sofia.pde.fdm.md#sofia.pde.fdm.Derivative.func)
      * [`Derivative.as_finite_difference()`](sofia.pde.fdm.md#sofia.pde.fdm.Derivative.as_finite_difference)
      * [`Derivative.default_assumptions`](sofia.pde.fdm.md#sofia.pde.fdm.Derivative.default_assumptions)
    * [`StencilSpec`](sofia.pde.fdm.md#sofia.pde.fdm.StencilSpec)
      * [`StencilSpec.derivative_order`](sofia.pde.fdm.md#sofia.pde.fdm.StencilSpec.derivative_order)
      * [`StencilSpec.discretisation_order`](sofia.pde.fdm.md#sofia.pde.fdm.StencilSpec.discretisation_order)
      * [`StencilSpec.side`](sofia.pde.fdm.md#sofia.pde.fdm.StencilSpec.side)
      * [`StencilSpec.integer_stencils`](sofia.pde.fdm.md#sofia.pde.fdm.StencilSpec.integer_stencils)
      * [`StencilSpec.offset_eval`](sofia.pde.fdm.md#sofia.pde.fdm.StencilSpec.offset_eval)
    * [`as_finite_diff()`](sofia.pde.fdm.md#sofia.pde.fdm.as_finite_diff)
    * [`diff()`](sofia.pde.fdm.md#sofia.pde.fdm.diff)
    * [`diff_finite()`](sofia.pde.fdm.md#sofia.pde.fdm.diff_finite)
    * [`diff_upwind()`](sofia.pde.fdm.md#sofia.pde.fdm.diff_upwind)
    * [`discretise()`](sofia.pde.fdm.md#sofia.pde.fdm.discretise)
    * [`stencil_points()`](sofia.pde.fdm.md#sofia.pde.fdm.stencil_points)
* [sofia.pde.rkn package](sofia.pde.rkn.md)
  * [Submodules](sofia.pde.rkn.md#submodules)
    * [sofia.pde.rkn.rkn](sofia.pde.rkn.rkn.md)
      * [`rkn()`](sofia.pde.rkn.rkn.md#sofia.pde.rkn.rkn.rkn)
    * [sofia.pde.rkn.schemes](sofia.pde.rkn.schemes.md)
  * [Module contents](sofia.pde.rkn.md#module-sofia.pde.rkn)

## Submodules

* [sofia.pde.drawing](sofia.pde.drawing.md)
  * [`Style`](sofia.pde.drawing.md#sofia.pde.drawing.Style)
  * [`set_theme()`](sofia.pde.drawing.md#sofia.pde.drawing.set_theme)
  * [`get_style()`](sofia.pde.drawing.md#sofia.pde.drawing.get_style)
  * [`update_style()`](sofia.pde.drawing.md#sofia.pde.drawing.update_style)
  * [`draw_grid()`](sofia.pde.drawing.md#sofia.pde.drawing.draw_grid)
* [sofia.pde.jacobian](sofia.pde.jacobian.md)
  * [`Jacobian`](sofia.pde.jacobian.md#sofia.pde.jacobian.Jacobian)
    * [`Jacobian.entries`](sofia.pde.jacobian.md#sofia.pde.jacobian.Jacobian.entries)
    * [`Jacobian.is_nearest_neighbor()`](sofia.pde.jacobian.md#sofia.pde.jacobian.Jacobian.is_nearest_neighbor)
    * [`Jacobian.to_block_tridiagonal()`](sofia.pde.jacobian.md#sofia.pde.jacobian.Jacobian.to_block_tridiagonal)
    * [`Jacobian.to_block_tridiagonal_solvable()`](sofia.pde.jacobian.md#sofia.pde.jacobian.Jacobian.to_block_tridiagonal_solvable)
* [sofia.pde.les](sofia.pde.les.md)
* [sofia.pde.preprocess](sofia.pde.preprocess.md)
* [sofia.pde.simulate](sofia.pde.simulate.md)
* [sofia.pde.wall_functions](sofia.pde.wall_functions.md)

## Module contents

### *class* sofia.pde.Del

Bases: `Basic`

#### gradient(scalar_field, doit=False)

#### dot(vect, doit=False)

#### cross(vect, doit=False)

#### default_assumptions *= {}*

### *class* sofia.pde.Derivative(expr, \*variables, side: Literal['center', 'forward', 'backward'] = 'center', discretisation_order: int | None = None, allow_higher_order: bool = True, integer_stencils: bool | None = None, evaluate: bool = False, \_order_explicit: bool | None = None, allow_expand: bool | None = None, \_skip_sp=False, \*\*kwargs)

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

### sofia.pde.as_finite_diff(expr, points=None, wrt=None, lhs=None, integer_stencils=None)

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

### sofia.pde.build_simulation(code_ir: CodegenAST, domain_specs: dict[[Domain](sofia.md#sofia.Domain), Any], kernel_backend: KERNEL_NAMES | \_\_annotationlib_name_1_\_ = 'auto', output_file: Path | None = None, overwrite: bool = False, return_module: bool = False, codegen_options: dict | None = None)

### sofia.pde.curl(vect, doit=True, allow_expand=None)

### sofia.pde.diff(expr: Expr, \*symbols: Symbol, side: Literal['center', 'forward', 'backward'] = 'center', discretisation_order: int | None = None, allow_higher_order: bool = True, integer_stencils: bool | None = None, upwind: bool = False, evaluate: bool = True, physical: bool = False, allow_expand: bool | None = None, \*\*kwargs) → Expr

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

### sofia.pde.diff_finite(expr, wrt, order: int = 1, \*\*kwargs) → Expr

Discretised derivative with finite differences.

Equivalent to `discretise(diff(expr, coord, order))`: it differentiates
with respect to the coordinate of the requested dimension and immediately
applies the finite-difference stencil.

### sofia.pde.diff_upwind(expr: Expr, \*symbols: Symbol, variable: Expr | Symbol | None = None, eps: float = 1e-06, discretisation_order: int | None = None, upwind_discretisation_order: int | None = None, side: Literal['center', 'forward', 'backward'] = 'center', integer_stencils: bool | None = None, allow_expand: bool | None = None, \*\*kwargs)

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

### sofia.pde.discretise(expr, lhs=None)

### sofia.pde.divergence(vect, doit=True, allow_expand=None)

### sofia.pde.fix(code_ir: CodegenAST | \_\_annotationlib_name_1_\_, domain_specs: dict[[Domain](sofia.md#sofia.Domain), Any] | None = None)

### sofia.pde.gradient(field, doit=True, allow_expand=None)

### sofia.pde.laplacian(expr, allow_expand=None)

Return the laplacian of the given field.

It is computed in terms of
the base scalars of the given coordinate system.

* **Parameters:**
  * **expr** – expr denotes a scalar or vector field.
  * **allow_expand** – Allows the expansion of product/chain rules.

### sofia.pde.rkn(F: dict[[Field](sofia.md#sofia.Field), sp.Expr], order: RK_ORDERS_OR_NAMES, dt: sp.Symbol, post: CodegenAST | None = None) → [RKn](sofia.ir.pde.md#sofia.ir.pde.RKn)

Return a Runge-Kutta update scheme of the given update expressions.

* **Parameters:**
  * **F** – Mapping of fields to their time derivative expressions.
    Each entry `{u: expr}` corresponds the equation `du/dt = expr`.
  * **dt** – Symbol representing the timestep in any of the given expressions.
  * **order** – 

    Runge-Kutta order or scheme name. Supported values include:
    - Integer orders: 1, 2, 3, 4
    - Named schemes: “euler”, “midpoint”, “heun2”, “ralston2”, “kutta3”,
      “heun3”, “ralston3”, “SSPRK3”, “3/8”, “m2s2”
  * **post** – Optional extra IR that should be appended to each RK stage. Any appearance of
    `dt` in this IR will be adjusted to the total timestep of the stage.
* **Returns:**
  IR token representing the Runge-Kutta integration scheme, which can be expanded
  by calling .doit().
* **Return type:**
  rkn_token

### sofia.pde.simulate(code_ir: CodegenAST, domain_specs: dict[[Domain](sofia.md#sofia.Domain), Any], \*inputs, kernel_backend: KERNEL_NAMES | \_\_annotationlib_name_1_\_ = 'auto', output_file: Path | None = None, overwrite: bool = False, codegen_options: dict | None = None)

### sofia.pde.eval_expr(expr: [EvalAssignment](sofia.ir.base.md#sofia.ir.base.EvalAssignment), domain_specs, kernel_backend='auto', cse: bool = True, show=False, strict_inputs=False, \*\*inputs)

### sofia.pde.apply_boundary_conditions(conditions: tuple[tuple[Expr, Expr], ...] | tuple[Expr, Expr] | tuple[BoundarySpec, ...] | BoundarySpec, field: [Field](sofia.md#sofia.Field) | None = None, extrapolation_order: int = 1, base_expr: Expr | None = None, as_eval_assignment: bool = False) → \_\_annotationlib_name_1_\_ | CodegenAST

Augment Field with interpolations that satisfy given boundary conditions.

* **Parameters:**
  * **conditions** – Boundary condition specifiers from which the interpolations are determined.
    Either tuples of Sympy expressions, or dicts of type BoundarySpec.
  * **field** – Input Field to which boundary conditions should be applied.
  * **extrapolation_order** – Order of extrapolation polynomial (affects the number of Field values used).
  * **base_expr** – Optional expression that replaces the Field in the non-interpolated region.
    This can be used to make a general expression for the interior of the domain
    while switching to extrapolations of a (different) Field at the boundaries.
  * **as_eval_assignment** – If True, return an ir.EvalAssignment instead of a Sympy expression.
* **Returns:**
  Piecewise expression that contains additional clauses for boundary conditions.
* **Return type:**
  sp.Expr
