# sofia.pde.fdm.stencils

### *class* sofia.pde.fdm.stencils.StencilSpec(derivative_order: int, discretisation_order: int, side: Literal['center', 'forward', 'backward'], integer_stencils: bool, offset_eval: sympy.core.expr.Expr)

Bases: `object`

### sofia.pde.fdm.stencils.stencil_points(coord, dd, spec: [StencilSpec](#sofia.pde.fdm.stencils.StencilSpec)) → list

Return the points at which the function should be evaluated.

These points can be used for a finite difference approximation of a derivative.
