# sofia package

## Subpackages

* [sofia.domain package](sofia.domain.md)
  * [Submodules](sofia.domain.md#submodules)
    * [sofia.domain.coordinate](sofia.domain.coordinate.md)
      * [`CoordSys`](sofia.domain.coordinate.md#sofia.domain.coordinate.CoordSys)
    * [sofia.domain.domain](sofia.domain.domain.md)
      * [`Domain`](sofia.domain.domain.md#sofia.domain.domain.Domain)
      * [`DomainEdge`](sofia.domain.domain.md#sofia.domain.domain.DomainEdge)
      * [`DomainStart`](sofia.domain.domain.md#sofia.domain.domain.DomainStart)
      * [`DomainEnd`](sofia.domain.domain.md#sofia.domain.domain.DomainEnd)
    * [sofia.domain.grid](sofia.domain.grid.md)
      * [`DD`](sofia.domain.grid.md#sofia.domain.grid.DD)
      * [`Grid`](sofia.domain.grid.md#sofia.domain.grid.Grid)
  * [Module contents](sofia.domain.md#module-sofia.domain)
* [sofia.field package](sofia.field.md)
  * [Submodules](sofia.field.md#submodules)
    * [sofia.field.field](sofia.field.field.md)
      * [`Field`](sofia.field.field.md#sofia.field.field.Field)
      * [`fields()`](sofia.field.field.md#sofia.field.field.fields)
    * [sofia.field.grid_field](sofia.field.grid_field.md)
      * [`IndexedGridField`](sofia.field.grid_field.md#sofia.field.grid_field.IndexedGridField)
      * [`GridField`](sofia.field.grid_field.md#sofia.field.grid_field.GridField)
      * [`GridFieldLocus`](sofia.field.grid_field.md#sofia.field.grid_field.GridFieldLocus)
    * [sofia.field.locus](sofia.field.locus.md)
      * [`Locus`](sofia.field.locus.md#sofia.field.locus.Locus)
      * [`ScalarLocus`](sofia.field.locus.md#sofia.field.locus.ScalarLocus)
    * [sofia.field.reductions](sofia.field.reductions.md)
      * [`Reduce`](sofia.field.reductions.md#sofia.field.reductions.Reduce)
      * [`Min()`](sofia.field.reductions.md#sofia.field.reductions.Min)
      * [`Max()`](sofia.field.reductions.md#sofia.field.reductions.Max)
      * [`Mean()`](sofia.field.reductions.md#sofia.field.reductions.Mean)
      * [`Sum()`](sofia.field.reductions.md#sofia.field.reductions.Sum)
      * [`Prod()`](sofia.field.reductions.md#sofia.field.reductions.Prod)
      * [`All()`](sofia.field.reductions.md#sofia.field.reductions.All)
      * [`Any()`](sofia.field.reductions.md#sofia.field.reductions.Any)
      * [`Std()`](sofia.field.reductions.md#sofia.field.reductions.Std)
      * [`Var()`](sofia.field.reductions.md#sofia.field.reductions.Var)
      * [`Median()`](sofia.field.reductions.md#sofia.field.reductions.Median)
      * [`Norm()`](sofia.field.reductions.md#sofia.field.reductions.Norm)
    * [sofia.field.staggered](sofia.field.staggered.md)
      * [`Staggered`](sofia.field.staggered.md#sofia.field.staggered.Staggered)
    * [sofia.field.sums](sofia.field.sums.md)
      * [`FieldAxisSum`](sofia.field.sums.md#sofia.field.sums.FieldAxisSum)
      * [`FieldAxisCumSum`](sofia.field.sums.md#sofia.field.sums.FieldAxisCumSum)
      * [`sum_axis()`](sofia.field.sums.md#sofia.field.sums.sum_axis)
      * [`integrate_axis()`](sofia.field.sums.md#sofia.field.sums.integrate_axis)
    * [sofia.field.transfer](sofia.field.transfer.md)
      * [`transfer()`](sofia.field.transfer.md#sofia.field.transfer.transfer)
      * [`Transfer`](sofia.field.transfer.md#sofia.field.transfer.Transfer)
      * [`TransferGridGrid`](sofia.field.transfer.md#sofia.field.transfer.TransferGridGrid)
      * [`TransferScalarGrid`](sofia.field.transfer.md#sofia.field.transfer.TransferScalarGrid)
      * [`TransferGridScalar`](sofia.field.transfer.md#sofia.field.transfer.TransferGridScalar)
      * [`insert_transfers()`](sofia.field.transfer.md#sofia.field.transfer.insert_transfers)
  * [Module contents](sofia.field.md#module-sofia.field)
* [sofia.geometry package](sofia.geometry.md)
  * [Submodules](sofia.geometry.md#submodules)
    * [sofia.geometry.geometry](sofia.geometry.geometry.md)
      * [`Geometry`](sofia.geometry.geometry.md#sofia.geometry.geometry.Geometry)
      * [`SDF`](sofia.geometry.geometry.md#sofia.geometry.geometry.SDF)
  * [Module contents](sofia.geometry.md#module-sofia.geometry)
    * [`Geometry`](sofia.geometry.md#sofia.geometry.Geometry)
      * [`Geometry.is_commutative`](sofia.geometry.md#sofia.geometry.Geometry.is_commutative)
      * [`Geometry.sdf()`](sofia.geometry.md#sofia.geometry.Geometry.sdf)
      * [`Geometry.default_assumptions`](sofia.geometry.md#sofia.geometry.Geometry.default_assumptions)
* [sofia.ir package](sofia.ir.md)
  * [Submodules](sofia.ir.md#submodules)
    * [sofia.ir.base](sofia.ir.base.md)
      * [`Assignment`](sofia.ir.base.md#sofia.ir.base.Assignment)
      * [`FunctionDefinition`](sofia.ir.base.md#sofia.ir.base.FunctionDefinition)
      * [`FunctionCall`](sofia.ir.base.md#sofia.ir.base.FunctionCall)
      * [`For`](sofia.ir.base.md#sofia.ir.base.For)
      * [`CodeBlock`](sofia.ir.base.md#sofia.ir.base.CodeBlock)
      * [`Pass`](sofia.ir.base.md#sofia.ir.base.Pass)
      * [`Print`](sofia.ir.base.md#sofia.ir.base.Print)
      * [`Store`](sofia.ir.base.md#sofia.ir.base.Store)
      * [`Save`](sofia.ir.base.md#sofia.ir.base.Save)
      * [`Load`](sofia.ir.base.md#sofia.ir.base.Load)
      * [`Inputs`](sofia.ir.base.md#sofia.ir.base.Inputs)
      * [`Outputs`](sofia.ir.base.md#sofia.ir.base.Outputs)
      * [`IndicesDeclaration`](sofia.ir.base.md#sofia.ir.base.IndicesDeclaration)
      * [`PrintTime`](sofia.ir.base.md#sofia.ir.base.PrintTime)
      * [`EvalAssignment`](sofia.ir.base.md#sofia.ir.base.EvalAssignment)
      * [`CustomFunctionCall`](sofia.ir.base.md#sofia.ir.base.CustomFunctionCall)
      * [`SolveCirculant`](sofia.ir.base.md#sofia.ir.base.SolveCirculant)
      * [`SolveBlockTridiagonal`](sofia.ir.base.md#sofia.ir.base.SolveBlockTridiagonal)
      * [`Checkpoint`](sofia.ir.base.md#sofia.ir.base.Checkpoint)
      * [`While`](sofia.ir.base.md#sofia.ir.base.While)
    * [sofia.ir.pde](sofia.ir.pde.md)
      * [`ProjectDivFree`](sofia.ir.pde.md#sofia.ir.pde.ProjectDivFree)
      * [`WallModelReconstruct`](sofia.ir.pde.md#sofia.ir.pde.WallModelReconstruct)
      * [`EddyViscositySmagorinsky`](sofia.ir.pde.md#sofia.ir.pde.EddyViscositySmagorinsky)
      * [`RKn`](sofia.ir.pde.md#sofia.ir.pde.RKn)
    * [sofia.ir.token](sofia.ir.token.md)
      * [`CodegenToken`](sofia.ir.token.md#sofia.ir.token.CodegenToken)
      * [`CustomToken`](sofia.ir.token.md#sofia.ir.token.CustomToken)
  * [Module contents](sofia.ir.md#module-sofia.ir)
    * [`Assignment`](sofia.ir.md#sofia.ir.Assignment)
      * [`Assignment.default_assumptions`](sofia.ir.md#sofia.ir.Assignment.default_assumptions)
    * [`Checkpoint`](sofia.ir.md#sofia.ir.Checkpoint)
      * [`Checkpoint.body`](sofia.ir.md#sofia.ir.Checkpoint.body)
      * [`Checkpoint.default_assumptions`](sofia.ir.md#sofia.ir.Checkpoint.default_assumptions)
      * [`Checkpoint.defaults`](sofia.ir.md#sofia.ir.Checkpoint.defaults)
    * [`CodeBlock`](sofia.ir.md#sofia.ir.CodeBlock)
      * [`CodeBlock.default_assumptions`](sofia.ir.md#sofia.ir.CodeBlock.default_assumptions)
    * [`CodegenToken`](sofia.ir.md#sofia.ir.CodegenToken)
      * [`CodegenToken.default_assumptions`](sofia.ir.md#sofia.ir.CodegenToken.default_assumptions)
      * [`CodegenToken.defaults`](sofia.ir.md#sofia.ir.CodegenToken.defaults)
    * [`CustomFunctionCall`](sofia.ir.md#sofia.ir.CustomFunctionCall)
      * [`CustomFunctionCall.name`](sofia.ir.md#sofia.ir.CustomFunctionCall.name)
      * [`CustomFunctionCall.lhs`](sofia.ir.md#sofia.ir.CustomFunctionCall.lhs)
      * [`CustomFunctionCall.input_args`](sofia.ir.md#sofia.ir.CustomFunctionCall.input_args)
      * [`CustomFunctionCall.default_assumptions`](sofia.ir.md#sofia.ir.CustomFunctionCall.default_assumptions)
    * [`CustomToken`](sofia.ir.md#sofia.ir.CustomToken)
      * [`CustomToken.transform()`](sofia.ir.md#sofia.ir.CustomToken.transform)
      * [`CustomToken.default_assumptions`](sofia.ir.md#sofia.ir.CustomToken.default_assumptions)
      * [`CustomToken.defaults`](sofia.ir.md#sofia.ir.CustomToken.defaults)
      * [`CustomToken.doit()`](sofia.ir.md#sofia.ir.CustomToken.doit)
    * [`EvalAssignment`](sofia.ir.md#sofia.ir.EvalAssignment)
      * [`EvalAssignment.default_assumptions`](sofia.ir.md#sofia.ir.EvalAssignment.default_assumptions)
    * [`EddyViscositySmagorinsky`](sofia.ir.md#sofia.ir.EddyViscositySmagorinsky)
      * [`EddyViscositySmagorinsky.defaults`](sofia.ir.md#sofia.ir.EddyViscositySmagorinsky.defaults)
      * [`EddyViscositySmagorinsky.doit()`](sofia.ir.md#sofia.ir.EddyViscositySmagorinsky.doit)
      * [`EddyViscositySmagorinsky.velocities`](sofia.ir.md#sofia.ir.EddyViscositySmagorinsky.velocities)
      * [`EddyViscositySmagorinsky.nu_sgs`](sofia.ir.md#sofia.ir.EddyViscositySmagorinsky.nu_sgs)
      * [`EddyViscositySmagorinsky.Cs`](sofia.ir.md#sofia.ir.EddyViscositySmagorinsky.Cs)
      * [`EddyViscositySmagorinsky.strain_squaring`](sofia.ir.md#sofia.ir.EddyViscositySmagorinsky.strain_squaring)
      * [`EddyViscositySmagorinsky.default_assumptions`](sofia.ir.md#sofia.ir.EddyViscositySmagorinsky.default_assumptions)
    * [`For`](sofia.ir.md#sofia.ir.For)
      * [`For.length`](sofia.ir.md#sofia.ir.For.length)
      * [`For.body`](sofia.ir.md#sofia.ir.For.body)
      * [`For.output_fields`](sofia.ir.md#sofia.ir.For.output_fields)
      * [`For.checkpoint`](sofia.ir.md#sofia.ir.For.checkpoint)
      * [`For.transform()`](sofia.ir.md#sofia.ir.For.transform)
      * [`For.default_assumptions`](sofia.ir.md#sofia.ir.For.default_assumptions)
      * [`For.defaults`](sofia.ir.md#sofia.ir.For.defaults)
    * [`FunctionCall`](sofia.ir.md#sofia.ir.FunctionCall)
      * [`FunctionCall.name`](sofia.ir.md#sofia.ir.FunctionCall.name)
      * [`FunctionCall.function_args`](sofia.ir.md#sofia.ir.FunctionCall.function_args)
      * [`FunctionCall.default_assumptions`](sofia.ir.md#sofia.ir.FunctionCall.default_assumptions)
    * [`FunctionDefinition`](sofia.ir.md#sofia.ir.FunctionDefinition)
      * [`FunctionDefinition.body`](sofia.ir.md#sofia.ir.FunctionDefinition.body)
      * [`FunctionDefinition.default_assumptions`](sofia.ir.md#sofia.ir.FunctionDefinition.default_assumptions)
    * [`IndicesDeclaration`](sofia.ir.md#sofia.ir.IndicesDeclaration)
      * [`IndicesDeclaration.symbols`](sofia.ir.md#sofia.ir.IndicesDeclaration.symbols)
      * [`IndicesDeclaration.default_assumptions`](sofia.ir.md#sofia.ir.IndicesDeclaration.default_assumptions)
    * [`Inputs`](sofia.ir.md#sofia.ir.Inputs)
      * [`Inputs.symbols`](sofia.ir.md#sofia.ir.Inputs.symbols)
      * [`Inputs.default_assumptions`](sofia.ir.md#sofia.ir.Inputs.default_assumptions)
    * [`Load`](sofia.ir.md#sofia.ir.Load)
      * [`Load.filename`](sofia.ir.md#sofia.ir.Load.filename)
      * [`Load.fields`](sofia.ir.md#sofia.ir.Load.fields)
      * [`Load.clip_larger`](sofia.ir.md#sofia.ir.Load.clip_larger)
      * [`Load.default_assumptions`](sofia.ir.md#sofia.ir.Load.default_assumptions)
      * [`Load.defaults`](sofia.ir.md#sofia.ir.Load.defaults)
    * [`Outputs`](sofia.ir.md#sofia.ir.Outputs)
      * [`Outputs.symbols`](sofia.ir.md#sofia.ir.Outputs.symbols)
      * [`Outputs.default_assumptions`](sofia.ir.md#sofia.ir.Outputs.default_assumptions)
    * [`Pass`](sofia.ir.md#sofia.ir.Pass)
      * [`Pass.default_assumptions`](sofia.ir.md#sofia.ir.Pass.default_assumptions)
    * [`Print`](sofia.ir.md#sofia.ir.Print)
      * [`Print.print_args`](sofia.ir.md#sofia.ir.Print.print_args)
      * [`Print.format_string`](sofia.ir.md#sofia.ir.Print.format_string)
      * [`Print.file`](sofia.ir.md#sofia.ir.Print.file)
      * [`Print.default_assumptions`](sofia.ir.md#sofia.ir.Print.default_assumptions)
    * [`PrintTime`](sofia.ir.md#sofia.ir.PrintTime)
      * [`PrintTime.show`](sofia.ir.md#sofia.ir.PrintTime.show)
      * [`PrintTime.default_assumptions`](sofia.ir.md#sofia.ir.PrintTime.default_assumptions)
      * [`PrintTime.defaults`](sofia.ir.md#sofia.ir.PrintTime.defaults)
    * [`ProjectDivFree`](sofia.ir.md#sofia.ir.ProjectDivFree)
      * [`ProjectDivFree.defaults`](sofia.ir.md#sofia.ir.ProjectDivFree.defaults)
      * [`ProjectDivFree.doit()`](sofia.ir.md#sofia.ir.ProjectDivFree.doit)
      * [`ProjectDivFree.velocities`](sofia.ir.md#sofia.ir.ProjectDivFree.velocities)
      * [`ProjectDivFree.pressure`](sofia.ir.md#sofia.ir.ProjectDivFree.pressure)
      * [`ProjectDivFree.bcs`](sofia.ir.md#sofia.ir.ProjectDivFree.bcs)
      * [`ProjectDivFree.default_assumptions`](sofia.ir.md#sofia.ir.ProjectDivFree.default_assumptions)
    * [`RKn`](sofia.ir.md#sofia.ir.RKn)
      * [`RKn.F`](sofia.ir.md#sofia.ir.RKn.F)
      * [`RKn.order`](sofia.ir.md#sofia.ir.RKn.order)
      * [`RKn.dt`](sofia.ir.md#sofia.ir.RKn.dt)
      * [`RKn.post`](sofia.ir.md#sofia.ir.RKn.post)
      * [`RKn.transform()`](sofia.ir.md#sofia.ir.RKn.transform)
      * [`RKn.default_assumptions`](sofia.ir.md#sofia.ir.RKn.default_assumptions)
      * [`RKn.defaults`](sofia.ir.md#sofia.ir.RKn.defaults)
    * [`Save`](sofia.ir.md#sofia.ir.Save)
      * [`Save.out_folder`](sofia.ir.md#sofia.ir.Save.out_folder)
      * [`Save.fields`](sofia.ir.md#sofia.ir.Save.fields)
      * [`Save.sequential`](sofia.ir.md#sofia.ir.Save.sequential)
      * [`Save.overwrite`](sofia.ir.md#sofia.ir.Save.overwrite)
      * [`Save.downcast_dtype`](sofia.ir.md#sofia.ir.Save.downcast_dtype)
      * [`Save.use_threading`](sofia.ir.md#sofia.ir.Save.use_threading)
      * [`Save.default_assumptions`](sofia.ir.md#sofia.ir.Save.default_assumptions)
      * [`Save.defaults`](sofia.ir.md#sofia.ir.Save.defaults)
    * [`SolveBlockTridiagonal`](sofia.ir.md#sofia.ir.SolveBlockTridiagonal)
      * [`SolveBlockTridiagonal.x`](sofia.ir.md#sofia.ir.SolveBlockTridiagonal.x)
      * [`SolveBlockTridiagonal.exprs`](sofia.ir.md#sofia.ir.SolveBlockTridiagonal.exprs)
      * [`SolveBlockTridiagonal.b`](sofia.ir.md#sofia.ir.SolveBlockTridiagonal.b)
      * [`SolveBlockTridiagonal.default_assumptions`](sofia.ir.md#sofia.ir.SolveBlockTridiagonal.default_assumptions)
      * [`SolveBlockTridiagonal.defaults`](sofia.ir.md#sofia.ir.SolveBlockTridiagonal.defaults)
    * [`SolveCirculant`](sofia.ir.md#sofia.ir.SolveCirculant)
      * [`SolveCirculant.x`](sofia.ir.md#sofia.ir.SolveCirculant.x)
      * [`SolveCirculant.expr`](sofia.ir.md#sofia.ir.SolveCirculant.expr)
      * [`SolveCirculant.b`](sofia.ir.md#sofia.ir.SolveCirculant.b)
      * [`SolveCirculant.default_assumptions`](sofia.ir.md#sofia.ir.SolveCirculant.default_assumptions)
    * [`WallModelReconstruct`](sofia.ir.md#sofia.ir.WallModelReconstruct)
      * [`WallModelReconstruct.defaults`](sofia.ir.md#sofia.ir.WallModelReconstruct.defaults)
      * [`WallModelReconstruct.doit()`](sofia.ir.md#sofia.ir.WallModelReconstruct.doit)
      * [`WallModelReconstruct.wall_fn`](sofia.ir.md#sofia.ir.WallModelReconstruct.wall_fn)
      * [`WallModelReconstruct.y_plus`](sofia.ir.md#sofia.ir.WallModelReconstruct.y_plus)
      * [`WallModelReconstruct.velocities`](sofia.ir.md#sofia.ir.WallModelReconstruct.velocities)
      * [`WallModelReconstruct.geometry`](sofia.ir.md#sofia.ir.WallModelReconstruct.geometry)
      * [`WallModelReconstruct.nu`](sofia.ir.md#sofia.ir.WallModelReconstruct.nu)
      * [`WallModelReconstruct.band`](sofia.ir.md#sofia.ir.WallModelReconstruct.band)
      * [`WallModelReconstruct.match_height`](sofia.ir.md#sofia.ir.WallModelReconstruct.match_height)
      * [`WallModelReconstruct.maxiter`](sofia.ir.md#sofia.ir.WallModelReconstruct.maxiter)
      * [`WallModelReconstruct.max_dist`](sofia.ir.md#sofia.ir.WallModelReconstruct.max_dist)
      * [`WallModelReconstruct.side`](sofia.ir.md#sofia.ir.WallModelReconstruct.side)
      * [`WallModelReconstruct.default_assumptions`](sofia.ir.md#sofia.ir.WallModelReconstruct.default_assumptions)
    * [`While`](sofia.ir.md#sofia.ir.While)
      * [`While.condition`](sofia.ir.md#sofia.ir.While.condition)
      * [`While.body`](sofia.ir.md#sofia.ir.While.body)
      * [`While.max_iter`](sofia.ir.md#sofia.ir.While.max_iter)
      * [`While.n_iter`](sofia.ir.md#sofia.ir.While.n_iter)
      * [`While.transform()`](sofia.ir.md#sofia.ir.While.transform)
      * [`While.default_assumptions`](sofia.ir.md#sofia.ir.While.default_assumptions)
      * [`While.defaults`](sofia.ir.md#sofia.ir.While.defaults)
* [sofia.pde package](sofia.pde.md)
  * [Subpackages](sofia.pde.md#subpackages)
    * [sofia.pde.fdm package](sofia.pde.fdm.md)
      * [Submodules](sofia.pde.fdm.md#submodules)
      * [Module contents](sofia.pde.fdm.md#module-sofia.pde.fdm)
    * [sofia.pde.rkn package](sofia.pde.rkn.md)
      * [Submodules](sofia.pde.rkn.md#submodules)
      * [Module contents](sofia.pde.rkn.md#module-sofia.pde.rkn)
  * [Submodules](sofia.pde.md#submodules)
    * [sofia.pde.drawing](sofia.pde.drawing.md)
      * [`Style`](sofia.pde.drawing.md#sofia.pde.drawing.Style)
      * [`set_theme()`](sofia.pde.drawing.md#sofia.pde.drawing.set_theme)
      * [`get_style()`](sofia.pde.drawing.md#sofia.pde.drawing.get_style)
      * [`update_style()`](sofia.pde.drawing.md#sofia.pde.drawing.update_style)
      * [`draw_grid()`](sofia.pde.drawing.md#sofia.pde.drawing.draw_grid)
    * [sofia.pde.jacobian](sofia.pde.jacobian.md)
      * [`Jacobian`](sofia.pde.jacobian.md#sofia.pde.jacobian.Jacobian)
    * [sofia.pde.les](sofia.pde.les.md)
    * [sofia.pde.preprocess](sofia.pde.preprocess.md)
    * [sofia.pde.simulate](sofia.pde.simulate.md)
    * [sofia.pde.wall_functions](sofia.pde.wall_functions.md)
  * [Module contents](sofia.pde.md#module-sofia.pde)
    * [`Del`](sofia.pde.md#sofia.pde.Del)
      * [`Del.gradient()`](sofia.pde.md#sofia.pde.Del.gradient)
      * [`Del.dot()`](sofia.pde.md#sofia.pde.Del.dot)
      * [`Del.cross()`](sofia.pde.md#sofia.pde.Del.cross)
      * [`Del.default_assumptions`](sofia.pde.md#sofia.pde.Del.default_assumptions)
    * [`Derivative`](sofia.pde.md#sofia.pde.Derivative)
      * [`Derivative.side`](sofia.pde.md#sofia.pde.Derivative.side)
      * [`Derivative.discretisation_order`](sofia.pde.md#sofia.pde.Derivative.discretisation_order)
      * [`Derivative.allow_higher_order`](sofia.pde.md#sofia.pde.Derivative.allow_higher_order)
      * [`Derivative.integer_stencils`](sofia.pde.md#sofia.pde.Derivative.integer_stencils)
      * [`Derivative.allow_expand`](sofia.pde.md#sofia.pde.Derivative.allow_expand)
      * [`Derivative.expand_terms()`](sofia.pde.md#sofia.pde.Derivative.expand_terms)
      * [`Derivative.from_derivative()`](sofia.pde.md#sofia.pde.Derivative.from_derivative)
      * [`Derivative.func()`](sofia.pde.md#sofia.pde.Derivative.func)
      * [`Derivative.as_finite_difference()`](sofia.pde.md#sofia.pde.Derivative.as_finite_difference)
      * [`Derivative.default_assumptions`](sofia.pde.md#sofia.pde.Derivative.default_assumptions)
    * [`as_finite_diff()`](sofia.pde.md#sofia.pde.as_finite_diff)
    * [`build_simulation()`](sofia.pde.md#sofia.pde.build_simulation)
    * [`curl()`](sofia.pde.md#sofia.pde.curl)
    * [`diff()`](sofia.pde.md#sofia.pde.diff)
    * [`diff_finite()`](sofia.pde.md#sofia.pde.diff_finite)
    * [`diff_upwind()`](sofia.pde.md#sofia.pde.diff_upwind)
    * [`discretise()`](sofia.pde.md#sofia.pde.discretise)
    * [`divergence()`](sofia.pde.md#sofia.pde.divergence)
    * [`fix()`](sofia.pde.md#sofia.pde.fix)
    * [`gradient()`](sofia.pde.md#sofia.pde.gradient)
    * [`laplacian()`](sofia.pde.md#sofia.pde.laplacian)
    * [`rkn()`](sofia.pde.md#sofia.pde.rkn)
    * [`simulate()`](sofia.pde.md#sofia.pde.simulate)
    * [`eval_expr()`](sofia.pde.md#sofia.pde.eval_expr)
    * [`apply_boundary_conditions()`](sofia.pde.md#sofia.pde.apply_boundary_conditions)

## Module contents

### sofia.requires(\*core_deps: str)

Build a decorator raising ImportError if given core dependencies are missing.

### *class* sofia.Domain(transformation: [CoordSys](sofia.domain.coordinate.md#sofia.domain.coordinate.CoordSys) = global)

Bases: `Basic`

The position space where a field lives.

#### dim(coordinate: Symbol) → int

Return the dimension index of a coordinate.

#### vec(\*components: [Field](#sofia.Field))

#### default_assumptions *= {}*

### *class* sofia.Field(name, domain, \*args, \*\*kwargs)

Bases: `Expr`

Symbolic field living on `Domain`.

#### is_commutative *= True*

#### copy(new_name: str, append_name: bool = False)

#### as_dummy(name=None)

Return the expression with any objects having structurally
bound symbols replaced with unique, canonical symbols within
the object in which they appear and having only the default
assumption for commutativity being True. When applied to a
symbol a new symbol having only the same commutativity will be
returned.

### Examples

```pycon
>>> from sympy import Integral, Symbol
>>> from sympy.abc import x
>>> r = Symbol('r', real=True)
>>> Integral(r, (r, x)).as_dummy()
Integral(_0, (_0, x))
>>> _.variables[0].is_real is None
True
>>> r.as_dummy()
_r
```

### Notes

Any object that has structurally bound variables should have
a property, `bound_symbols` that returns those symbols
appearing in the object.

#### default_assumptions *= {'commutative': True}*

### *class* sofia.GridField(name: str, domain: [Grid](sofia.domain.grid.md#sofia.domain.grid.Grid), staggered: [Staggered](sofia.field.staggered.md#sofia.field.staggered.Staggered) = Center, discretisation_order: int = 0, axes: Tuple | tuple[Symbol | int, ...] = None, \_shifts: Tuple | tuple[Symbol | float | int, ...] | None = None)

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

#### is_commutative *= True*

#### copy(new_name: str, append_name: bool = False)

#### with_order(order)

#### to_staggered_origin()

#### reset_shifts()

#### s(\*shifts, \*\*kwargs)

#### indexify(bare=False)

#### fractional_offset(wrt) → Expr

Combine shift and staggered offset of a field.

#### interpolate()

#### default_assumptions *= {'commutative': True}*

### *class* sofia.Grid(ndim: Literal[1, 2, 3], name: str = 'G', discretisation_order: int = 2, boundaries: Numeric | tuple[Numeric | tuple[Numeric, Numeric], ...] | None = None, \_C: [CoordSys](sofia.domain.coordinate.md#sofia.domain.coordinate.CoordSys) | None = None, \_shape: tuple[Symbol | int, ...] | None = None, , transformations: \_\_annotationlib_name_1_\_ | Callable | None = None, transformations_inverse: \_\_annotationlib_name_2_\_ | Callable | None = None)

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

#### coord_to_ind(coordinate: Symbol) → Symbol

#### dds_eval(L, boundary_left, boundary_right)

#### maps_coord_to_ind()

#### maps_ind_to_coord()

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

#### vec(\*components)

#### default_assumptions *= {}*

### *class* sofia.Scalar(name: str | Symbol, , dtype: DTypeLike | None = None, \*\*assumptions: bool | None)

Bases: `AtomicExpr`

Symbolic scalar.

Identified by `name` and `assumptions`, compared after sympy has
drawn their consequences. If `dtype` is boolean, a `BooleanScalar` is
returned instead.

* **Parameters:**
  * **name** – Identifier the generated code declares this scalar under.
  * **dtype** – Data type. By default it is the backend’s float dtype.
  * **assumptions** – Sympy assumptions, restricted to `SUPPORTED_ASSUMPTIONS`.

#### is_commutative *= True*

#### is_symbol *= True*

#### is_Symbol *= True*

#### kind *= NumberKind*

#### default_assumptions *= {'commutative': True}*

### *class* sofia.Staggered(offsets: dict[int, Expr | float] | None = None, name=None)

Bases: `Basic`

Fractional per-axis offsets of a field relative to cell centers.

#### CENTER *: ClassVar[[Staggered](sofia.field.staggered.md#sofia.field.staggered.Staggered)]* *= Center*

#### XFACE *: ClassVar[[Staggered](sofia.field.staggered.md#sofia.field.staggered.Staggered)]* *= XFACE*

#### YFACE *: ClassVar[[Staggered](sofia.field.staggered.md#sofia.field.staggered.Staggered)]* *= YFACE*

#### ZFACE *: ClassVar[[Staggered](sofia.field.staggered.md#sofia.field.staggered.Staggered)]* *= ZFACE*

#### staggered_offsets(ndim: int) → tuple[Expr, ...]

#### XY_EDGE *= XY_EDGE*

#### XZ_EDGE *= XZ_EDGE*

#### YZ_EDGE *= YZ_EDGE*

#### default_assumptions *= {}*

### sofia.fields(names: str, domain: [Domain](#sofia.Domain), \*\*kwargs: object) → tuple[[Field](sofia.field.field.md#sofia.field.field.Field), ...]

Create one or more Fields on a shared domain.

Equivalent to `sympy.symbols()`, but for Fields. Always returns
a tuple for better type support.

* **Parameters:**
  * **names** – Field names, comma- or whitespace-separated; see `sympy.symbols()`.
  * **domain** – Domain the fields live on.
  * **\*\*kwargs** – Forwarded to the Field subclass registered for `domain`.
* **Returns:**
  A tuple of the created Field(s).
* **Return type:**
  tuple[[Field](#sofia.Field), …]

### Examples

```pycon
>>> (u,) = fields("u", domain=Grid(ndim=1))
>>> v, w = fields("v w", domain=Grid(ndim=2))
```

### sofia.print_environment_info() → None

Print the environment info for reporting bugs, mimicking the jax equivalent.

### sofia.scalars(names: str, \*\*kwargs: object) → tuple[[Scalar](#sofia.Scalar), ...]

Create one or more Scalars.

Equivalent to `sympy.symbols()`, but for Scalars. Always returns
a tuple for better type support.

* **Parameters:**
  * **names** – Scalar names, comma- or whitespace-separated; see `sympy.symbols()`.
  * **\*\*kwargs** – Keyword arguments passed to Scalar.
* **Returns:**
  A tuple of the created Scalar(s).
* **Return type:**
  tuple[[Scalar](#sofia.Scalar), …]

### Examples

```pycon
>>> (a,) = scalars("a")
>>> b, c = scalars("b c", positive=True)
```
