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

#### warp_kernel(output: Basic | Sequence[Basic], inputs: Iterable[Basic], body: Callable[[...], str], assign: bool = True) → str

Add a `@warp.kernel` that computes `output` and return its call.

`output` is one field, or a sequence of fields that one kernel computes
together, e.g. so that they share work. `body(out)`, or
`body(out0, out1, ...)` for a sequence, returns the kernel body, given the
name of each array the kernel writes, in the order of `output`. Each array
is separate from that field’s input array, so the body reads the old values
of every output. The body must write every element of every output array,
since an array may start uninitialised. The kernel launches over the shape
of the outputs, which must all be the same.

The returned statement assigns the results to `output`. With
`assign=False`, it is only the call, e.g. to assign to temporaries named
with `print_in_program`. For a sequence, the call returns a tuple, even
one with a single element.

### Examples

One kernel sets `(a, b)` to `(a + b, a - b)`:

```
``
```

```
`
```

python
a, b = ctx.print_in_kernel(self.a), ctx.print_in_kernel(self.b)
ctx.warp_kernel(

> output=(self.a, self.b),
> inputs=(self.a, self.b),
> body=lambda out_a, out_b: (

> > “i = warp.tid()n”
> > f”{out_a}[i] = {a}[i] + {b}[i]n”
> > f”{out_b}[i] = {a}[i] - {b}[i]”

> ),

## )

#### function(body: str, inputs: Iterable[Basic], outputs: Iterable[Basic]) → str

Wrap the statements `body` in a backend function and return its call.

The function takes `inputs` and returns `outputs`, which the returned
statement assigns.
