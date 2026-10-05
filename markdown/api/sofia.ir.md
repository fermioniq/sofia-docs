# sofia.ir package

## Submodules

* [sofia.ir.base](sofia.ir.base.md)
  * [`Assignment`](sofia.ir.base.md#sofia.ir.base.Assignment)
  * [`FunctionDefinition`](sofia.ir.base.md#sofia.ir.base.FunctionDefinition)
  * [`FunctionCall`](sofia.ir.base.md#sofia.ir.base.FunctionCall)
  * [`For`](sofia.ir.base.md#sofia.ir.base.For)
    * [`For.transform()`](sofia.ir.base.md#sofia.ir.base.For.transform)
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
    * [`While.transform()`](sofia.ir.base.md#sofia.ir.base.While.transform)
* [sofia.ir.pde](sofia.ir.pde.md)
  * [`ProjectDivFree`](sofia.ir.pde.md#sofia.ir.pde.ProjectDivFree)
    * [`ProjectDivFree.doit()`](sofia.ir.pde.md#sofia.ir.pde.ProjectDivFree.doit)
  * [`WallModelReconstruct`](sofia.ir.pde.md#sofia.ir.pde.WallModelReconstruct)
    * [`WallModelReconstruct.doit()`](sofia.ir.pde.md#sofia.ir.pde.WallModelReconstruct.doit)
  * [`EddyViscositySmagorinsky`](sofia.ir.pde.md#sofia.ir.pde.EddyViscositySmagorinsky)
    * [`EddyViscositySmagorinsky.doit()`](sofia.ir.pde.md#sofia.ir.pde.EddyViscositySmagorinsky.doit)
  * [`RKn`](sofia.ir.pde.md#sofia.ir.pde.RKn)
    * [`RKn.transform()`](sofia.ir.pde.md#sofia.ir.pde.RKn.transform)
* [sofia.ir.token](sofia.ir.token.md)
  * [`CodegenToken`](sofia.ir.token.md#sofia.ir.token.CodegenToken)
  * [`CustomToken`](sofia.ir.token.md#sofia.ir.token.CustomToken)
    * [`CustomToken.transform()`](sofia.ir.token.md#sofia.ir.token.CustomToken.transform)
    * [`CustomToken.doit()`](sofia.ir.token.md#sofia.ir.token.CustomToken.doit)

## Module contents

### *class* sofia.ir.Assignment(lhs, rhs)

Bases: `Assignment`

#### default_assumptions *= {}*

### *class* sofia.ir.Checkpoint(\*args, \*\*kwargs)

Bases: [`CodegenToken`](sofia.ir.token.md#sofia.ir.token.CodegenToken)

Insert a checkpoint (for autodiff) around the code generated from `body`.

The intermediate values computed inside the `body` are not stored for the
backward pass in autodiff, and are instead recomputed.
See e.g. [https://docs.jax.dev/en/latest/_autosummary/jax.checkpoint.html](https://docs.jax.dev/en/latest/_autosummary/jax.checkpoint.html) for
more on checkpointing.

Outside autodiff this token has no effect.

* **Parameters:**
  **body** ([*sofia.ir.base.CodeBlock*](sofia.ir.base.md#sofia.ir.base.CodeBlock)) – Block of IR to checkpoint.

### Examples

Checkpoint the body of a batched `For`.

```
``
```

```
`
```

python
from sofia import Field, Grid, ir

G = Grid(ndim=1, \_shape=(64,))
u = Field(“u”, domain=G)

code = ir.For(
: 100,
  ir.Checkpoint(
  <br/>
  > ir.For(
  > 100,
  > ir.EvalAssignment(u, u \* 2 + 1)
  > ),
  <br/>
  ),

### )

To checkpoint an entire loop body, it’s better to use
`ir.For(..., checkpoint=True)` which will apply checkpointing in the same way
as the example above.

#### SEE ALSO
[`For`](#sofia.ir.For)
: For loop token, whose `checkpoint` argument checkpoints the body of the loop.

#### body *: [CodeBlock](sofia.ir.base.md#sofia.ir.base.CodeBlock)*

#### default_assumptions *= {}*

#### defaults *= {}*

### *class* sofia.ir.CodeBlock(\*args)

Bases: `CodeBlock`

Basic building block for blocks of code.

Any chunks of IR can be wrapped inside a CodeBlock. This
can be necessary for example when a block of code is passed
as an argument to another IR statement such as BatchedScan.

### Examples

```pycon
>>> import numpy as np
>>> from sofia import Field, Grid, ir, indices
>>> from sofia.pde import simulate
```

```pycon
>>> G = Grid(ndim=1)
>>> u = Field("u", domain=G)
>>> v = Field("v", domain=G)
>>> i = indices.i
```

```pycon
>>> code_block = ir.CodeBlock(
... ir.EvalAssignment(u, u[i-1] + v[i-1]),  # u_i <- u_{i-1} + v_{i-1}
... ir.EvalAssignment(v, u[i+1] + v[i+1]),  # v_i <- u_{i+1} + v_{i+1}
... )
```

Repeat the above code block 5 times, assigning output to `us`:

```pycon
>>> scan = ir.CodeBlock(
...    ir.Inputs((u, v,)),
...    ir.For(
...        length=5,
...        body=code_block,
...    ),
...    ir.Outputs(u,)
... )
```

```pycon
>>> N = 10
>>> u = v = np.arange(N)
>>> #res = simulate(scan, {G: dict(shape=N)}, u, v) # TODO: fix
>>> #res
# (array([[270, 259, 298, 377, 466, 535, 544, 503, 422, 331]]), array([[635, 509, 443, 447, 531, 665, 789, 853, 847, 761]]))
```

#### default_assumptions *= {}*

### *class* sofia.ir.CodegenToken(\*args, \*\*kwargs)

Bases: `_Token`

Base class for IR tokens that can be handled by codegen.

These tokens cannot be transformed into lower-level tokens (either through
`transform` or `doit`). Therefore, these tokens must have an associated
\_print_{TokenName} method on the codegen printer.

Note that this class only has semantic meaning, it does not validate whether
the codegen printer actually supports any subclass.

### Examples

```
``
```

```
`
```

python
from sofia import ir
import sympy as sp

class MyToken(ir.CodegenToken):
: lhs: object
  rhs: object = “default”

my_token = MyToken(lhs=sp.x, rhs=”something”)

```
``
```

```
`
```

#### SEE ALSO
[`CustomToken`](#sofia.ir.CustomToken)
: For examples and the full explanation for defining fields.

#### default_assumptions *= {}*

#### defaults *= {}*

### *class* sofia.ir.CustomFunctionCall(\*args, \*\*kwargs)

Bases: `Token`

#### name

#### lhs

#### input_args

#### default_assumptions *= {}*

### *class* sofia.ir.CustomToken(\*args, \*\*kwargs)

Bases: `_Token`

Base class for defining custom internal representation (IR) tokens.

This class provides a convenient way to define your own custom tokens
with minimal boilerplate. The token is defined by a set of fields
(with optional defaults) and a `transform` method to represent it
in terms of other IR tokens.

Fields are declared as class variables with attrs annotations. Fields
without a default are defined as my_field: object, while providing a
default is as simple as my_field: object = “default_value”.

A field can also declare a converter via attrs.field(converter=…), in
which case the field’s own (stored) type is given by the annotation, while
the type accepted by \_\_init_\_ (as seen by type checkers) is inferred from
the converter’s argument type. Converters must be idempotent (i.e.
conv(conv(x)) == conv(x)), as sympy requires that Token(\*token.args) ==
token.

Subclasses must implement `transform` returning the representation
of the token in terms of other IR tokens. This method is called from `doit`,
which is used by sympy to expand an IR tree.

### Examples

```
``
```

```
`
```

python
import sympy as sp
from sofia import ir

class Assignment(ir.CustomToken):
: lhs: sp.Symbol = field(converter=sp.sympify)
  rhs: sp.Expr = field(converter=sp.sympify)
  <br/>
  def transform(self):
  : return ir.Assignment(self.lhs, self.rhs)

```
``
```

```
`
```

#### *abstractmethod* transform() → Basic

Represent this token in terms of other IR tokens.

All returned tokens must (eventually) expand to just tokens defined in
ir.base (e.g. ir.base.For), as the code generator is only defined
for those tokens. Note that tt is okay for this method to return other
custom tokens not from ir.base, as long as these tokens (possibly
through multiple .transform() calls) eventually expand to just
ir.base tokens.

Subclasses must override this method.

* **Returns:**
  The token in terms of other IR tokens.
* **Return type:**
  sympy.Basic

#### default_assumptions *= {}*

#### defaults *= {}*

#### doit(\*\*hints) → Basic

Expand this token into its `transform` representation.

This makes `CustomToken` compatible with sympy’s expansion which calls
`doit` recursively on an IR tree.

* **Returns:**
  The token in terms of other tokens.
* **Return type:**
  sympy.Basic

### *class* sofia.ir.EvalAssignment(lhs, rhs)

Bases: `CodegenAST`

Evaluate the `rhs` expression on the full grid.

The result will be stored in the `lhs` symbol, which should
be a `Field` or `sp.Indexed` object.

### Examples

Evaluate a constant expression and store in the 2D field `u`:

```pycon
>>> from sofia import Field, Grid
>>> G = Grid(ndim=2);
>>> u = Field('u', domain=G)
>>> v = Field('v', domain=G)
>>> i, j = G.indices
>>> EvalAssignment(u, 3)
EvalAssignment(u, 3)
```

If `v` is a `Field` the indices are optional:

```pycon
>>> EvalAssignment(v, 3)
EvalAssignment(v, 3)
```

Evaluate a mathematical function:

```pycon
>>> x = G.x
>>> EvalAssignment(u, sp.sin(3 * x) + x**2)
EvalAssignment(u, global.x**2 + sin(3*global.x))
```

Evaluate an expression containing other fields:

```pycon
>>> EvalAssignment(u, u[i + 1, j] - 3 * v[i, j])
EvalAssignment(u, u[i + 1, j] - 3*v[i, j])
```

Also on the `rhs` default indices are optional for `Field` objects:

```pycon
>>> EvalAssignment(u, u[i + 1, j] - 3 * v)
EvalAssignment(u, -3*v + u[i + 1, j])
```

* **Parameters:**
  * **lhs** – Symbol representing the left-hand side, in which the result
    is to be stored.
  * **rhs** – Right-hand side expression that will be evaluated.

#### default_assumptions *= {}*

### *class* sofia.ir.EddyViscositySmagorinsky(\*args, \*\*kwargs)

Bases: `Token`

Compute the Eddy viscosity for a given subgrid scale model.

Note: only the Smagorinsky model is supported right now.

* **Parameters:**
  * **velocities** – Tuple of (u,v,w) fields.
  * **nu_sgs** – The GridField to be used for storing the output.
  * **Cs** – Smagorinsky constant.
  * **strain_squaring** – 

    Mode of averaging:
    “edge”   (default) square each off-diagonal strain at its own edge, then
    > average to the centre. Preserves the adjointness that makes the
    > reported SGS dissipation equal the applied dissipation.
    > I.e. average-of-square.

    ”center” transfer the strains to the centre and square there. Reproduces
    : the square-of-average convention of SBP-derived formulations.

#### defaults *= {'Cs': 0.1, 'strain_squaring': 'edge'}*

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

#### velocities

#### nu_sgs

#### Cs

#### strain_squaring

#### default_assumptions *= {}*

### *class* sofia.ir.For(\*args, \*\*kwargs)

Bases: [`CustomToken`](sofia.ir.token.md#sofia.ir.token.CustomToken)

A for-loop: repeat a block of code (`body`) a fixed number of times.

The input fields of `body` are carried and output along with its output fields.
A field that is only ever written inside the body is considered local and not
carried nor returned.

* **Parameters:**
  * **length** (*sympy.core.numbers.Integer* *|* *sympy.core.symbol.Symbol*) – Number of iterations.
  * **body** ([*sofia.ir.base.CodeBlock*](sofia.ir.base.md#sofia.ir.base.CodeBlock)) – Loop body.
  * **output_fields** (*sympy.core.containers.Tuple*) – 

    Fields to report as outputs in addition to the loop carry.

    #### WARNING
    Not currently supported.
  * **checkpoint** (*bool*) – If `True` (default), checkpoint the loop body under automatic
    differentiation. See `Checkpoint` for details.

### Examples

```
``
```

```
`
```

python
from sofia import Field, Grid, ir

G = Grid(ndim=1, \_shape=(64,))
u = Field(“u”, domain=G)

code = ir.For(1000, [ir.EvalAssignment(u, u \* 2 + 1)])

```
``
```

```
`
```

The same loop with a checkpointed body

``python
code = ir.For(1000, [ir.EvalAssignment(u, u * 2 + 1)], checkpoint=True)
``

#### SEE ALSO
[`Checkpoint`](#sofia.ir.Checkpoint)
: Checkpoint an arbitrary block, rather than a whole loop body.

#### length *: Integer | Symbol*

#### body *: [CodeBlock](sofia.ir.base.md#sofia.ir.base.CodeBlock)*

#### output_fields *: Tuple*

#### checkpoint *: bool*

#### transform()

Represent this token in terms of other IR tokens.

All returned tokens must (eventually) expand to just tokens defined in
ir.base (e.g. ir.base.For), as the code generator is only defined
for those tokens. Note that tt is okay for this method to return other
custom tokens not from ir.base, as long as these tokens (possibly
through multiple .transform() calls) eventually expand to just
ir.base tokens.

Subclasses must override this method.

* **Returns:**
  The token in terms of other IR tokens.
* **Return type:**
  sympy.Basic

#### default_assumptions *= {}*

#### defaults *= {'checkpoint': False, 'output_fields': ()}*

### *class* sofia.ir.FunctionCall(\*args, \*\*kwargs)

Bases: `FunctionCall`

#### name

#### function_args

#### default_assumptions *= {}*

### *class* sofia.ir.FunctionDefinition(\*args, \*\*kwargs)

Bases: `FunctionDefinition`

#### body

#### default_assumptions *= {}*

### *class* sofia.ir.IndicesDeclaration(\*args, \*\*kwargs)

Bases: `Token`

#### symbols

#### default_assumptions *= {}*

### *class* sofia.ir.Inputs(\*args, \*\*kwargs)

Bases: `Token`

Specify inputs of the program.

Fields or other symbols specified by `Inputs` in the IR will be
linked to input arguments (in order) of the generated code
object.

#### NOTE
This is a special statement that can only appear once in
the IR. It is optional - if omitted, the generated code object
will not accept any arguments.

* **Parameters:**
  **symbols** – Symbol or tuple of symbols for which input arguments are to
  be expected.

#### symbols

#### default_assumptions *= {}*

### *class* sofia.ir.Load(\*args, \*\*kwargs)

Bases: [`CodegenToken`](sofia.ir.token.md#sofia.ir.token.CodegenToken)

Load fields from a `.npz` file.

* **Parameters:**
  * **filename** (*str*) – Path to the file to read.
  * **fields** (*sympy.core.containers.Tuple*) – Fields to populate. Names are matched against the archive keys.
  * **clip_larger** (*bool*) – If `True` (default), arrays larger than the target field are clipped
    to fit.

#### filename *: str*

#### fields *: Tuple*

#### clip_larger *: bool*

#### default_assumptions *= {}*

#### defaults *= {'clip_larger': True}*

### *class* sofia.ir.Outputs(\*args, \*\*kwargs)

Bases: `Token`

Specify outputs of the program.

Any fields or other symbols can be added to the output by using this
statement in the IR. Note that the position where the `Output`
appears in the IR is significant - any changes to the fields afterwards
are not take into account.

* **Parameters:**
  **symbols** – Symbol or tuple of symbols that should be added to the output
  of the program.

#### symbols

#### default_assumptions *= {}*

### *class* sofia.ir.Pass(\*args, \*\*kwargs)

Bases: `Token`

#### default_assumptions *= {}*

### *class* sofia.ir.Print(\*args, \*\*kwargs)

Bases: `Print`

#### print_args

#### format_string

#### file

#### default_assumptions *= {}*

### *class* sofia.ir.PrintTime(\*args, \*\*kwargs)

Bases: [`CodegenToken`](sofia.ir.token.md#sofia.ir.token.CodegenToken)

Print elapsed wall-clock time at this point in the program.

#### show *: bool*

#### default_assumptions *= {}*

#### defaults *= {'show': True}*

### *class* sofia.ir.ProjectDivFree(\*args, \*\*kwargs)

Bases: `Token`

Makes velocity fields divergence free by solving the Poisson equation.

Only uniform axes (without curvilinear coordinate transformations)
are supported for now.

This token expands to
: - ir.SolveSeparable: solves the Poisson equations, using
    : sofia.pde.laplacian(pressure, allow_expand=False) as the linear operator
  - Assignments that project out the divergence on the velocity fields

Due to the forwarding to `ir.SolveSeparable`, restrictions on the allowed boundary
conditions apply. Per axis, the following boundary conditions are supported:

> - periodic (solved via Fourier transform)
> - cell-centered neumann-neumann boundary conditions (via DCT-I)
> - edge neumann-neumann boundary conditions (via DCT-II)
> - mixed dirichlet/neumann boundary conditions at arbitrary
>   : locations, but not neumann-neumann since the linear system
>     becomes singular

One further restriction: at most one axis of the last type is allowed.

* **Parameters:**
  * **velocities** – Fields that store velocities, to be made divergence-free.
  * **pressure** – Field that represents the pressure. The contents of the pressure will
    be computed as part of this token and stored in this field.
  * **bcs** – Boundary conditions on `pressure` that should be enforced. This should be
    an `sp.Piecewise` expression with boundary conditions at the
    boundaries and `pressure` itself in the bulk. The best way to obtain this
    is via `sofia.pde.apply_boundary_conditions(bcs, pressure)`.

#### defaults *= {'bcs': NoneToken()}*

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

#### velocities

#### pressure

#### bcs

#### default_assumptions *= {}*

### *class* sofia.ir.RKn(\*args, \*\*kwargs)

Bases: [`CustomToken`](sofia.ir.token.md#sofia.ir.token.CustomToken)

#### F *: sp.Dict[[Field](sofia.md#sofia.Field), sp.Expr]*

#### order *: sp.Symbol*

#### dt *: sp.Basic*

#### post *: ast.CodegenAST*

#### transform() → Basic

Represent this token in terms of other IR tokens.

All returned tokens must (eventually) expand to just tokens defined in
ir.base (e.g. ir.base.For), as the code generator is only defined
for those tokens. Note that tt is okay for this method to return other
custom tokens not from ir.base, as long as these tokens (possibly
through multiple .transform() calls) eventually expand to just
ir.base tokens.

Subclasses must override this method.

* **Returns:**
  The token in terms of other IR tokens.
* **Return type:**
  sympy.Basic

#### default_assumptions *= {}*

#### defaults *= {'post': None}*

### *class* sofia.ir.Save(\*args, \*\*kwargs)

Bases: [`CodegenToken`](sofia.ir.token.md#sofia.ir.token.CodegenToken)

Save fields to disk during simulation.

* **Parameters:**
  * **out_folder** (*str*) – Path to location on disk where the output will be stored.
  * **fields** (*sympy.core.containers.Tuple*) – Fields that are to be saved.
  * **sequential** (*bool*) – If `True` (default), every time a `Save` statement is executed the
    filename will have an index appended to it, making sure outputs
    are stored in separate files.
    If `False`, any `Save` statement will overwrite the given file.
  * **overwrite** (*bool*) – If `False` (default), the `out_folder` should not exist prior to running
    the simulation (preventing accidental overwrites). If it does exist
    an exception is raised.
  * **downcast_dtype** (*Literal* *[* *'float16'* *,*  *'float32'* *]*  *|* *None*) – Before saving, each field can be converted to a set dtype in order to
    reduce file size. The options are (“float16”, “float32”). If omitted,
    all fields are saved in their original dtype.
  * **use_threading** (*bool*) – If True (default), saving will be performed in a separate thread without
    blocking the execution of the simulation. Note that while this enhances
    performance, it is possible that the simulation finishes before all
    save threads are finished.
    This issue (potentially leading to data loss) will be solved in future
    updates but it’s safest for now to add a `time.sleep` at the end of the
    simulation.

#### out_folder *: str*

#### fields *: Tuple*

#### sequential *: bool*

#### overwrite *: bool*

#### downcast_dtype *: Literal['float16', 'float32'] | None*

#### use_threading *: bool*

#### default_assumptions *= {}*

#### defaults *= {'downcast_dtype': None, 'overwrite': False, 'sequential': True, 'use_threading': True}*

### *class* sofia.ir.SolveBlockTridiagonal(\*args, \*\*kwargs)

Bases: [`CodegenToken`](sofia.ir.token.md#sofia.ir.token.CodegenToken)

Solve a block-tridiagonal system of equations.

* **Parameters:**
  * **x** (*sympy.core.containers.Tuple*) – Output field symbols.
  * **exprs** (*sympy.core.containers.Tuple*) – A tuple of symbolic matrices representing the blocks of the system.
    The matrices should be of the form (D, A, C) where D is the diagonal,
    A is the lower diagonal, and C is the upper diagonal.
  * **b** (*sympy.core.containers.Tuple*) – Discretised right hand side expressions.

#### x *: Tuple*

#### exprs *: Tuple*

#### b *: Tuple*

#### default_assumptions *= {}*

#### defaults *= {}*

### *class* sofia.ir.SolveCirculant(\*args, \*\*kwargs)

Bases: `Token`

Solves a separable linear problem Ax=b with periodic boundary conditions.

Each axis should have periodic boundary conditions, since the system is
solved using Fourier transforms.

* **Parameters:**
  * **x** – Field to solve for (must be the same as in the `exprs`).
  * **expr** – Expression representing the linear operator acting on `x`.
  * **b** – Right-hand side Field.

#### x

#### expr

#### b

#### default_assumptions *= {}*

### *class* sofia.ir.WallModelReconstruct(\*args, \*\*kwargs)

Bases: `Token`

Reconstruct near-wall velocity from a wall function.

* **Parameters:**
  * **wall_fn** – Symbolic wall function in forward form `u+ = f(y+)`, expressed in
    terms of `y_plus`. e.g. Reichardt’s law. The inverse form
    (`y+ = g(u+)`, e.g. Spalding) is not accepted.
  * **y_plus** – The symbol used for `y+` inside `wall_fn`.
    (will refactor into sp.Lambda later)
  * **velocities** – The velocity components `(u, v, w)`, overwritten in place.
  * **geometry** – Geometry object.
  * **nu** – Kinematic viscosity.
  * **band** – Reconstruct only where the wall distance is below this.
  * **match_height** – Wall distance of the matching point used to infer `u_tau`.
    Must exceed `band`.
  * **n_iter** – Newton iterations for `u_tau`.
  * **max_dist** – Search radius passed to `wp.mesh_query_point`.

#### defaults *= {'band': 2, 'match_height': 6, 'max_dist': 10.0, 'maxiter': 8, 'side': 1}*

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

#### wall_fn

#### y_plus

#### velocities

#### geometry

#### nu

#### band

#### match_height

#### maxiter

#### max_dist

#### side

#### default_assumptions *= {}*

### *class* sofia.ir.While(\*args, \*\*kwargs)

Bases: [`CustomToken`](sofia.ir.token.md#sofia.ir.token.CustomToken)

A while-loop.

Repeat a block of code (`body`) for as long as `condition` holds. The
condition is checked before each iteration, the first one included, so the body
never runs if it is false at the outset.

`condition` should be an expression of `BooleanKind` involving `Scalar`
variables and fields wrapped in a `sofia.field.reductions.Reduce`. Every scalar
and field the condition reads must already hold a value before the loop, and every
reduction is re-evaluated before each check.

With `max_iter` given, it stops after at most `max_iter` iterations. The loop
exits without raising any exception or warning.

The loop returns the inputs of `body` together with everything `condition`
reads and, when `n_iter` is given, also the number of iterations. A variable
written only inside the body, and not read by the condition, stays local to it.

* **Parameters:**
  * **condition** (*sympy.core.basic.Basic*) – The loop runs for as long as this holds.
  * **body** ([*sofia.ir.base.CodeBlock*](sofia.ir.base.md#sofia.ir.base.CodeBlock)) – Loop body.
  * **max_iter** (*sympy.core.numbers.Integer* *|* *sympy.core.symbol.Symbol* *|* *None*) – Maximum allowed number of iterations. By default it is `None`, for which
    the loop runs as long as the condition holds.
  * **n_iter** ([*sofia.scalar.scalar.Scalar*](sofia.md#sofia.Scalar) *|* *None*) – Integer `Scalar` where to store the number of iterations run in the loop.
    By default it is `None`, for which the number of iterations is not
    returned.
* **Raises:**
  * **ValueError** – If `condition` is not a boolean expression, if `max_iter` is a
        non-positive integer or a symbol assumed to be non-positive, or if `n_iter`
        appears in `condition` or `body`.
  * **TypeError** – If `max_iter` is neither an integer nor a symbol, or `n_iter` is not an
        integer `Scalar`.
  * **NotImplementedError** – If `condition` contains fields outside a reduction.

### Examples

```
``
```

```
`
```

python
from sofia import Field, Grid, Scalar, ir
from sofia.field.reductions import Norm

G = Grid(ndim=3)
u = Field(“u”, G)
threshold = Scalar(“threshold”)
n_iter = Scalar(“n_iter”, dtype=int)

code_ir = ir.CodeBlock(
: ir.Inputs((u, threshold)),
  ir.While(Norm(u) > threshold, ir.CodeBlock(u << u / 2), n_iter=n_iter),
  ir.Outputs((u, n_iter)),

)

```
``
```

```
`
```

#### condition *: Basic*

#### body *: [CodeBlock](sofia.ir.base.md#sofia.ir.base.CodeBlock)*

#### max_iter *: Integer | Symbol | None*

#### n_iter *: [Scalar](sofia.md#sofia.Scalar) | None*

#### transform()

Represent this token in terms of other IR tokens.

All returned tokens must (eventually) expand to just tokens defined in
ir.base (e.g. ir.base.For), as the code generator is only defined
for those tokens. Note that tt is okay for this method to return other
custom tokens not from ir.base, as long as these tokens (possibly
through multiple .transform() calls) eventually expand to just
ir.base tokens.

Subclasses must override this method.

* **Returns:**
  The token in terms of other IR tokens.
* **Return type:**
  sympy.Basic

#### default_assumptions *= {}*

#### defaults *= {'max_iter': None, 'n_iter': None}*
