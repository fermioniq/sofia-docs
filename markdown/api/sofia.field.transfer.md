# sofia.field.transfer

### sofia.field.transfer.transfer(expr: \_\_annotationlib_name_1_\_ | CodegenAST, output_field=None)

Transfer IR code or single expression to output domain(s).

If a single expression (sp.Expr) is given, an optional output_field
can be passed, from which the output domain is extracted.

If a general block of IR code is passed, the output domains will be
extracted from the individual assignments within the code, and passing
an output_field argument is not allowed.

* **Parameters:**
  * **expr** – Single expression or IR code.
  * **output_field** – (Only allowed for single expressions) output field from which the output
    domain is extracted.
* **Returns:**
  Expression or IR code with expressions transferred to appropriate output
  domain(s).
* **Return type:**
  transferred_expr

### *class* sofia.field.transfer.Transfer(expr: Expr, to: [Locus](sofia.field.locus.md#sofia.field.locus.Locus) | None = None, \*\*kwargs)

Bases: `Expr`

Symbolic, lazy operator that transfers expr to another locus.

Contains metadata (as kwargs) describing *how* the transfer should be
performed once resolved, e.g. interpolation order. Transfer instances may
be nested arbitrarily, merging their kwargs, with the innermost transfer’s
kwargs taking precedence.

The target locus, to, may be omitted, which means the target locus will
be taken from an enclosing Transfer operator (just like the kwargs).
Nested transfer operators with same (or missing) target loci will be
merged. Nested transfer operators with a different target locus will be
kept, possibly resulting in back-and-forth transferring between loci.

Constructing a Transfer evaluates it, it recursively inserts a Transfer
around every subexpression. Calling doit() (with to set) makes the
transfer explicit: it dispatches Transfer class to a locus specific
subclass. Each pair of (source locus type, target locus type) has its own
subclass, e.g. a grid-to-grid subclass.

* **Parameters:**
  * **expr** – Expression to transfer.
  * **to** – Target locus. May be omitted, leaving the transfer “bare”.
  * **\*\*kwargs** – Metadata describing how the transfer should be carried out, such as
    interpolation order. Forwarded to any nested Transfer or Transfer
    inserted by doit.

### Examples

```pycon
>>> from sofia.field.locus import SCALAR
>>> from sofia import Grid, Field
>>> u, v = Field("u", Grid(ndim=2)), Field("v", Grid(ndim=2))
```

```pycon
>>> Transfer(1, to=SCALAR).doit()
1
>>> Transfer(u, to=SCALAR).doit()
TransferGridScalar(u, ScalarLocus())
>>> Transfer(u, to=v.locus).doit()
u
>>> u.locus == v.locus
True
>>> w = Field("w", Grid(ndim=3))
>>> Transfer(u, to=w.locus).doit()
TransferGridGrid(u, GridFieldLocus(Grid(ndim=3), Center, (0, 0, 0)))
>>> u.locus == w.locus
False
>>> Transfer(v + 2 * w, to=v.locus).doit()
v + 2*TransferGridGrid(w, GridFieldLocus(Grid(ndim=2), Center, (0, 0)))
>>> Transfer(u + Transfer(v, to=v.locus), to=u.locus).doit()
u + v
```

#### doit(deep: bool = True, \*\*hints: object) → Expr

Resolve and dispatch all transfer operators in the expression.

* **Parameters:**
  * **deep** – If False, return self unchanged. If True (the default),
    insert a concrete transfer operator around every subexpression of
    self.expr that is not already in the target locus, propagating
    any extra keyword arguments self was constructed with.
    See `_insert_transfers_expr` for a full explanation how transfer operators
    are inserted.
  * **\*\*hints** – Unused, accepted for compatibility with sp.Basic.doit.

### *class* sofia.field.transfer.TransferGridGrid(expr: Expr, to: [Locus](sofia.field.locus.md#sofia.field.locus.Locus) | None = None, \*\*kwargs)

Bases: [`Transfer`](#sofia.field.transfer.Transfer)

### *class* sofia.field.transfer.TransferScalarGrid(expr: Expr, to: [Locus](sofia.field.locus.md#sofia.field.locus.Locus) | None = None, \*\*kwargs)

Bases: [`Transfer`](#sofia.field.transfer.Transfer)

### *class* sofia.field.transfer.TransferGridScalar(expr: Expr, to: [Locus](sofia.field.locus.md#sofia.field.locus.Locus) | None = None, \*\*kwargs)

Bases: [`Transfer`](#sofia.field.transfer.Transfer)

### sofia.field.transfer.insert_transfers(expr: Basic) → Basic

Wrap the RHS of every `EvalAssignment` throughout a block of IR.

Traverses expr, handling each EvalAssignment found within it
independently: the RHS of each assignment gets wrapped in a bare
Transfer targeting the locus of its own LHS, unless it already is one.
Call .doit(deep=True) on the result (or on the individual Transfer
nodes) to insert the low-level transfer operators this requires.

* **Parameters:**
  **expr** – IR code (e.g. a CodeBlock) to insert transfer operators into.
* **Returns:**
  expr with the RHS of every EvalAssignment it contains wrapped in
  a bare Transfer.
* **Return type:**
  [expr](sofia.ir.md#sofia.ir.SolveCirculant.expr)
