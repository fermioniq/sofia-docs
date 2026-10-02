# sofia.pde.rkn.rkn

### sofia.pde.rkn.rkn.rkn(F: dict[[Field](sofia.md#sofia.Field), sp.Expr], order: RK_ORDERS_OR_NAMES, dt: sp.Symbol, post: CodegenAST | None = None) → [RKn](sofia.ir.pde.md#sofia.ir.pde.RKn)

Return a Runge-Kutta update scheme of the given update expressions.

* **Parameters:**
  * **F** – Mapping of fields to their time derivative expressions.
    Each entry `{u: expr}` corresponds the equation `du/dt = expr`.
  * **dt** – Symbol representing the timestep in any of the given expressions.
  * **order** – 

    Runge-Kutta order or scheme name. Supported values include:
    - Integer orders: 1, 2, 3, 4
    - Named schemes: “euler”, “midpoint”, “heun2”, “ralston2”, “kutta3”,
      “heun3”, “ralston3”, “SSPRK3”, “3/8”, “m2s2”
  * **post** – Optional extra IR that should be appended to each RK stage. Any appearance of
    `dt` in this IR will be adjusted to the total timestep of the stage.
* **Returns:**
  IR token representing the Runge-Kutta integration scheme, which can be expanded
  by calling .doit().
* **Return type:**
  rkn_token
