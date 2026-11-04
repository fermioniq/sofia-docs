# sofia.ir.extension

Extension points for code that runs inside generated programs.

Plugins add runtime behaviour by subclassing `KernelExtension` (a pointwise
function inside a kernel expression) or `ProgramExtension` (a statement in the
generated `main`). Nothing is registered: each code printer has one generic
print method for each base class, which delegates to the instance. Importing a
plugin defines its classes; using it means putting instances in the IR.

Both classes name the modules their generated code needs in `runtime_imports`.
These are import lines added to the generated module, so plugin code never
imports its own Warp code.

### *class* sofia.ir.extension.KernelExtension(\*args)

Bases: `Function`

Base class for a pointwise function inside a kernel expression.

A subclass is used like any sympy function, e.g. `chi << InsideBody(geo, x,
y, z)`, and appears in kernel expressions. Its code is printed by the method
named after the kernel target, which receives the arguments already printed
and returns the printed call:

- `warp(self, *args: str) -> str` for Warp kernels
- `backend(self, *args: str) -> str` for backend (numpy / jax) kernels

A subclass implements one or both; codegen picks a kernel target the subclass
implements. Arguments are sympified on construction, as for any sympy
function, so codegen can traverse and cache expressions containing them.

#### runtime_imports

Import lines the printed code needs, added to the generated module.

* **Type:**
  ClassVar[tuple[str, …]]

### Examples

```
``
```

```
`
```

python
from sofia import ir

class InsideBody(ir.KernelExtension):
: runtime_imports = (
  : “from sofia_contrib_kernels.ibm.kernels import inside_body”,
  <br/>
  )
  <br/>
  def warp(self, geo, x, y, z):
  : return f”inside_body({geo}, {x}, {y}, {z})”

```
``
```

```
`
```

### *class* sofia.ir.extension.ProgramExtension(\*args, \*\*kwargs)

Bases: [`CodegenToken`](sofia.ir.token.md#sofia.ir.token.CodegenToken)

Base class for a statement in the generated program.

A subclass is an IR token, with fields declared as for `CodegenToken`, and
is placed in an `ir.CodeBlock` like any other statement. Codegen calls
`emit` with a `ProgramContext`, which adds kernels and functions to the
generated module, and puts the returned statement in `main`.

All fields must hold sympy objects, so that the token can be traversed,
substituted and cached by hash. Use a converter for other values, e.g.
`maxiter: sp.Integer = field(converter=sp.sympify, default=8)`.

#### runtime_imports

Import lines the emitted code needs, added to the generated module.

* **Type:**
  ClassVar[tuple[str, …]]

### Examples

```
``
```

```
`
```

python
from sofia import Field, Geometry, ir

class GhostCellFill(ir.ProgramExtension):
: runtime_imports = (
  : “from sofia_contrib_kernels.ibm.kernels import ghost_value”,
  <br/>
  )
  <br/>
  field: Field
  geometry: Geometry
  <br/>
  @property
  def inputs(self):
  <br/>
  > return {self.field, self.geometry}
  <br/>
  @property
  def outputs(self):
  <br/>
  > return {self.field}
  <br/>
  def emit(self, ctx):
  : geo, f = ctx.print_in_kernel(self.geometry), ctx.print_in_kernel(self.field)
    return ctx.warp_kernel(
    <br/>
    > output=self.field,
    > inputs=(self.geometry, self.field),
    > body=lambda out: (
    <br/>
    > > “i, j, k = warp.tid()n”
    > > f”{out}[i, j, k] = ghost_value({geo}, {f}, i, j, k)”
    <br/>
    > ),
    <br/>
    )

```
``
```

```
`
```

#### *abstract property* inputs *: set*

Fields, scalars and geometries the statement reads.

#### *abstract property* outputs *: set*

Fields and scalars the statement writes.

#### *abstractmethod* emit(ctx: [ProgramContext](#sofia.ir.extension.ProgramContext)) → str

Return the statement for `main`, adding what it calls through `ctx`.

### *class* sofia.ir.extension.ProgramContext(\*args, \*\*kwargs)

Bases: `Protocol`

What `ProgramExtension.emit` can add to the generated module.

Each method that adds code returns the code that uses it, to be put in the
statement `emit` returns.

#### print_in_kernel(expr: Basic) → str

Print `expr` as it appears in a Warp kernel, e.g. a field’s array.

Use it for code in the body given to `warp_kernel`.

#### print_in_program(expr: Basic) → str

Print `expr` as it appears in the program, outside any kernel.

E.g. a field’s variable, or the unique name of an `sp.Dummy`. Use it for
the statement `emit` returns and the body given to `function`.

#### add_warp_func(expr: Basic, args: Iterable[Symbol] | None = None) → str

Compile `expr` into a `@warp.func` and return the function’s name.

`expr` is either a scalar expression, compiled to a function of
`float64` arguments, or an `sp.Tuple` of 2 or 3 components, each in one
symbol, compiled to a function of one `vec2d` or `vec3d`.

`args` sets a scalar function’s arguments, in order (default: the
symbols in `expr`, in no fixed order), so e.g. a constant still takes
them and returns `float64`.

#### warp_kernel(output: Basic, inputs: Iterable[Basic], body: Callable[[str], str], assign: bool = True) → str

Add a `@warp.kernel` that computes `output` and return its call.

`body(out)` returns the kernel body, given the name of the array the
kernel writes. That array is separate from `output`’s input array, so
the body reads the old values of `output`. The returned statement
assigns the result to `output`; with `assign=False`, it is only the
call, e.g. to assign to a temporary named with `print_in_program`.

#### function(body: str, inputs: Iterable[Basic], outputs: Iterable[Basic]) → str

Wrap the statements `body` in a backend function and return its call.

The function takes `inputs` and returns `outputs`, which the returned
statement assigns.
