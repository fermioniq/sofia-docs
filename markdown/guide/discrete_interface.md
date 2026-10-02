# Discrete Interface

It is always possible to manually write equations in discrete form by
indexing a `Field` directly, using the `Field[]` interface with
Python `int` or symbolic integers. Each dimension of a `Field` is
indexed from $0$ to $N−1$, and any ghost cells introduced
for boundary conditions are included in this indexing range.

#### NOTE
When indexing a `Field` directly, you are accessing the points of
the underlying storage. Care must be taken to account for the staggered
nature of the grid, and any required interpolations must be performed
manually.

```python
>>> from sofia import Grid, Field

>>> G = Grid(ndim=2)
>>> u = Field("u", domain=G)
>>> i, j = G.indices

>>> u[i, j]
>>> u[i + 1, j]
>>> u[i + 1, j - 1]
```

$$
\displaystyle {u}_{\mathtt{\text{i}},\mathtt{\text{j}}}
$$

$$
\displaystyle {u}_{\mathtt{\text{i}} + 1,\mathtt{\text{j}}}
$$

$$
\displaystyle {u}_{\mathtt{\text{i}} + 1,\mathtt{\text{j}} - 1}
$$

Protections against invalid indexing are built in, preventing most
common mistakes.

```python
# Invalid non integer shift
>>> try:
...     u[i + 0.3, j]
... except IndexError as e:
...     print("u[i + 0.3, j]: ", e)
u[i + 0.3, j]:  Invalid Field indexing (non-(half-)integer shift). Allowed form is (i + a) for symbol i and (half-)integer a; got i + 5404319552844595/18014398509481984.

# Invalid multiple of i
>>> try:
...    u[2 * i - 3, j]
... except IndexError as e:
...    print("u[2 * i - 3, j]: ", e)
u[2 * i - 3, j]:  Invalid Field indexing (unrecognised format). Allowed form is (i + a) for symbol i and (half-)integer a; got 2*i - 3.
```

## Finite Difference Derivatives

Finite difference stencils can be written out manually by using indexed
fields or by using the shorthands defined in the `sofia` package. The
most general interface is through `diff_finite`.

```python
# Example (2nd derivative wrt i):
diff_finite(expr, wrt=i, order=2)
```
