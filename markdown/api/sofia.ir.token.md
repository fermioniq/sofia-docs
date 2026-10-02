# sofia.ir.token

### *class* sofia.ir.token.CodegenToken(\*args, \*\*kwargs)

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
[`CustomToken`](#sofia.ir.token.CustomToken)
: For examples and the full explanation for defining fields.

### *class* sofia.ir.token.CustomToken(\*args, \*\*kwargs)

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

#### doit(\*\*hints) → Basic

Expand this token into its `transform` representation.

This makes `CustomToken` compatible with sympy’s expansion which calls
`doit` recursively on an IR tree.

* **Returns:**
  The token in terms of other tokens.
* **Return type:**
  sympy.Basic
