# sofia.pde.fdm.discretisation

### sofia.pde.fdm.discretisation.as_finite_diff(expr, points=None, wrt=None, lhs=None, integer_stencils=None)

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
