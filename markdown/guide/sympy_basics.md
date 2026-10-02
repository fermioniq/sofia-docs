# SymPy Basics

sofia provides a symbolic interface for defining PDEs which relies on [SymPy](https://www.sympy.org/en/index.html).
SymPy is a library for working with symbolic mathematical expressions
and equations. It is widely used in scientific and engineering
communities and while it’s easy to get started using SymPy there are
many extra features that make it a powerful symbolic engine.

## Symbols and Expressions

SymPy uses `Symbol` and `symbols` to create symbolic variables. You
can define single symbols, several at once, or a sequence. For the
sequence case, you provide a start and end name, and SymPy generates all
intermediate names automatically.

```python
import sympy as sp

x = sp.Symbol("x")
a = sp.Symbol("a")
y, z = sp.symbols("y z")
many_symbols = sp.symbols("a:q")
```

Using these symbols you can build symbolic expressions:

```python
f = a * x + y
```

## Functions

SymPy has many built-in mathematical functions, usually following
similar naming conventions as `NumPy`:

```python
# Most names are drop-in replacements for numpy functions
sp.sin(x * sp.pi) + x**2 - sp.sinh(y) * sp.sqrt(x)

# Some are slightly different
sp.Abs(x**2 + y**2)
```

These expressions render as:

$$
\displaystyle - \sqrt{x} \sinh{\left(y \right)} + x^{2} + \sin{\left(\pi x \right)}
$$

$$
\displaystyle \left|{x^{2} + y^{2}}\right|
$$

## Piecewise Functions

A commonly used construction for boundary conditions is a conditional
function. The `sp.Piecewise` interface takes as input a sequence of
`(expression, condition)` pairs, where the `expression` for which
`condition` evaluates to `True` will be evaluated.

**Important**: the conditional clauses are evaluated eagerly *in order*,
such that the first clause with a truthy `condition` will be evaluated
without considering later clauses. As such, conditions can be defined
without explicitly excluding earlier conditions, and usually the last
clause has the simple condition `True` (meaning its `expression`
will be evaluated when all other conditions were `False`).

For example, the piecewise function

$$
g(x) =
\begin{cases}
    x & \quad \text{if } x > 0\\
    0 & \quad \text{otherwise}
\end{cases}
$$

is equivalent to the following code:

```python
g = sp.Piecewise(
    (x, x > 0),
    (0, True)
)
```

A more complex example, setting boundary conditions:

$$
u =
\begin{cases}
    0 & \quad \text{if } i = 0\\
    10 & \quad \text{if } i = N - 1\\
    u_{\text{interior}} & \quad \text{otherwise}
\end{cases}
$$

where `i` and `N` are symbols representing the index and total number of grid points,
and `u_interior` is a symbolic expression for the solution at interior nodes. This is expressed in code as:

```python
u = sp.Piecewise(
    (0, sp.Eq(i, 0)),
    (10, sp.Eq(i, N-1)),
    (u_interior, True)
)
```

#### IMPORTANT
The conditions specifying equality should be written as
`sp.Eq(...)` rather than with Python’s `==` operator since we are
expressing **mathematical** equality rather than equality of Python
objects. See [SymPy’s
documentation](https://docs.sympy.org/latest/explanation/gotchas.html#equals-signs)
for more information.
