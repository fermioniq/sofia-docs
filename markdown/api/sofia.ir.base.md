# sofia.ir.base

### *class* sofia.ir.base.Assignment(lhs, rhs)

Bases: `Assignment`

### *class* sofia.ir.base.FunctionDefinition(\*args, \*\*kwargs)

Bases: `FunctionDefinition`

### *class* sofia.ir.base.FunctionCall(\*args, \*\*kwargs)

Bases: `FunctionCall`

### *class* sofia.ir.base.For(\*args, \*\*kwargs)

Bases: [`CustomToken`](sofia.ir.token.md#sofia.ir.token.CustomToken)

A for-loop: repeat a block of code (`body`) a fixed number of times.

The input fields of `body` are carried and output along with its output fields.
A field that is only ever written inside the body is considered local and not
carried nor returned.

* **Parameters:**
  * **length** (*sympy.core.numbers.Integer* *|* *sympy.core.symbol.Symbol*) – Number of iterations.
  * **body** ([*sofia.ir.base.CodeBlock*](#sofia.ir.base.CodeBlock)) – Loop body.
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
[`Checkpoint`](#sofia.ir.base.Checkpoint)
: Checkpoint an arbitrary block, rather than a whole loop body.

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

### *class* sofia.ir.base.CodeBlock(\*args)

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

### *class* sofia.ir.base.Pass(\*args, \*\*kwargs)

Bases: `Token`

### *class* sofia.ir.base.Print(\*args, \*\*kwargs)

Bases: `Print`

### *class* sofia.ir.base.Store(\*args, \*\*kwargs)

Bases: `Token`

### *class* sofia.ir.base.Save(\*args, \*\*kwargs)

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

### *class* sofia.ir.base.Load(\*args, \*\*kwargs)

Bases: [`CodegenToken`](sofia.ir.token.md#sofia.ir.token.CodegenToken)

Load fields from a `.npz` file.

* **Parameters:**
  * **filename** (*str*) – Path to the file to read.
  * **fields** (*sympy.core.containers.Tuple*) – Fields to populate. Names are matched against the archive keys.
  * **clip_larger** (*bool*) – If `True` (default), arrays larger than the target field are clipped
    to fit.

### *class* sofia.ir.base.Inputs(\*args, \*\*kwargs)

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

### *class* sofia.ir.base.Outputs(\*args, \*\*kwargs)

Bases: `Token`

Specify outputs of the program.

Any fields or other symbols can be added to the output by using this
statement in the IR. Note that the position where the `Output`
appears in the IR is significant - any changes to the fields afterwards
are not take into account.

* **Parameters:**
  **symbols** – Symbol or tuple of symbols that should be added to the output
  of the program.

### *class* sofia.ir.base.IndicesDeclaration(\*args, \*\*kwargs)

Bases: `Token`

### *class* sofia.ir.base.PrintTime(\*args, \*\*kwargs)

Bases: [`CodegenToken`](sofia.ir.token.md#sofia.ir.token.CodegenToken)

Print elapsed wall-clock time at this point in the program.

### *class* sofia.ir.base.EvalAssignment(lhs, rhs)

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

### *class* sofia.ir.base.CustomFunctionCall(\*args, \*\*kwargs)

Bases: `Token`

### *class* sofia.ir.base.SolveCirculant(\*args, \*\*kwargs)

Bases: `Token`

Solves a separable linear problem Ax=b with periodic boundary conditions.

Each axis should have periodic boundary conditions, since the system is
solved using Fourier transforms.

* **Parameters:**
  * **x** – Field to solve for (must be the same as in the `exprs`).
  * **expr** – Expression representing the linear operator acting on `x`.
  * **b** – Right-hand side Field.

### *class* sofia.ir.base.SolveBlockTridiagonal(\*args, \*\*kwargs)

Bases: [`CodegenToken`](sofia.ir.token.md#sofia.ir.token.CodegenToken)

Solve a block-tridiagonal system of equations.

* **Parameters:**
  * **x** (*sympy.core.containers.Tuple*) – Output field symbols.
  * **exprs** (*sympy.core.containers.Tuple*) – A tuple of symbolic matrices representing the blocks of the system.
    The matrices should be of the form (D, A, C) where D is the diagonal,
    A is the lower diagonal, and C is the upper diagonal.
  * **b** (*sympy.core.containers.Tuple*) – Discretised right hand side expressions.

### *class* sofia.ir.base.Checkpoint(\*args, \*\*kwargs)

Bases: [`CodegenToken`](sofia.ir.token.md#sofia.ir.token.CodegenToken)

Insert a checkpoint (for autodiff) around the code generated from `body`.

The intermediate values computed inside the `body` are not stored for the
backward pass in autodiff, and are instead recomputed.
See e.g. [https://docs.jax.dev/en/latest/_autosummary/jax.checkpoint.html](https://docs.jax.dev/en/latest/_autosummary/jax.checkpoint.html) for
more on checkpointing.

Outside autodiff this token has no effect.

* **Parameters:**
  **body** ([*sofia.ir.base.CodeBlock*](#sofia.ir.base.CodeBlock)) – Block of IR to checkpoint.

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

## )

To checkpoint an entire loop body, it’s better to use
`ir.For(..., checkpoint=True)` which will apply checkpointing in the same way
as the example above.

#### SEE ALSO
[`For`](#sofia.ir.base.For)
: For loop token, whose `checkpoint` argument checkpoints the body of the loop.

### *class* sofia.ir.base.While(\*args, \*\*kwargs)

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
  * **body** ([*sofia.ir.base.CodeBlock*](#sofia.ir.base.CodeBlock)) – Loop body.
  * **max_iter** (*sympy.core.numbers.Integer* *|* *sympy.core.symbol.Symbol* *|* *None*) – Maximum allowed number of iterations. By default it is `None`, for which
    the loop runs as long as the condition holds.
  * **n_iter** ([*sofia.scalar.scalar.Scalar*](sofia.md#sofia.Scalar) *|* *None*) – Where to store the number of iterations run in the loop. By default it is
    `None`, for which the number of iterations is not returned.
* **Raises:**
  * **ValueError** – If `condition` is not a boolean expression, if `max_iter` is a
        non-positive integer or a symbol assumed to be non-positive, or if `n_iter`
        appears in `condition` or `body`.
  * **TypeError** – If `max_iter` is neither an integer nor a symbol, or `n_iter` is not a
        `Scalar`.
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
n_iter = Scalar(“n_iter”)

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
