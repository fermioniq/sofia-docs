# sofia.domain.coordinate

### *class* sofia.domain.coordinate.CoordSys(name, transformation=None, parent=None, location=None, rotation_matrix=None, vector_names=None, variable_names=None)

Bases: `CoordSys3D`

#### create_new(name, transformation, variable_names=None, vector_names=None, transformation_inverse=None)

Return a CoordSys3D which is connected to self by transformation.

* **Parameters:**
  * **name** – The name of the new CoordSys3D instance.
  * **transformation** – Transformation defined by transformation equations or chosen
    from predefined ones.
  * **vector_names** – Iterables of 3 strings each, with custom names for base
    vectors and base scalars of the new system respectively.
    Used for simple str printing.
  * **variable_names** – Iterables of 3 strings each, with custom names for base
    vectors and base scalars of the new system respectively.
    Used for simple str printing.
  * **transformation_inverse** – Inverse transformation defined by transformation equations.

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
