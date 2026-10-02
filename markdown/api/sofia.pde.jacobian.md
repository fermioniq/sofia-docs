# sofia.pde.jacobian

### *class* sofia.pde.jacobian.Jacobian(exprs: tuple[Expr, ...], variables: tuple[[GridField](sofia.field.grid_field.md#sofia.field.grid_field.GridField), ...])

Bases: `object`

The symbolic Jacobian of a system of equations.

#### *property* entries *: dict[tuple[int, [GridField](sofia.field.grid_field.md#sofia.field.grid_field.GridField), tuple[int, ...]], Expr]*

Compute the Jacobian entries of the system of equations.

#### is_nearest_neighbor(axis: int) → bool

Whether entries only couples neighbors along axis.

#### to_block_tridiagonal(axis: int) → tuple[MutableDenseMatrix, MutableDenseMatrix, MutableDenseMatrix, tuple[Expr, ...]]

Return blocks of the Jacobian along axis as (D, A, C, b).

D is the diagonal, A is the lower diagonal, and C is the upper diagonal.

#### to_block_tridiagonal_solvable(axis: int, outputs: tuple[[GridField](sofia.field.grid_field.md#sofia.field.grid_field.GridField), ...] | None = None, negate: bool = True) → tuple[tuple[[GridField](sofia.field.grid_field.md#sofia.field.grid_field.GridField), ...], tuple[MutableDenseMatrix, MutableDenseMatrix, MutableDenseMatrix], tuple[Expr, ...]]

Return (x, exprs, b) as expected by ir.SolveBlockTridiagonal.

Example usage:
ir.SolveBlockTridiagonal(\*Jacobian(exprs, variables).to_block_tridiagonal_solvable(axis)).

* **Parameters:**
  * **axis** – The grid axis to build block-tridiagonal system along.
  * **outputs** – Fields to hold the Newton step for each field in variables, in
    the same order.
  * **negate** – Controls the sign of b. By default b = -fixed_exprs, matching
    the Newton step J @ dx = -R.
