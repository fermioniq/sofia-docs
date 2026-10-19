# sofia.domain.domain

### *class* sofia.domain.domain.Domain(transformation: [CoordSys](sofia.domain.coordinate.md#sofia.domain.coordinate.CoordSys) = global)

Bases: `Basic`

The position space where a field lives.

#### dim(coordinate: Symbol) → int

Return the dimension index of a coordinate.

#### *property* identity

Return a sum over the outer products of the unit vectors with themselves.

### *class* sofia.domain.domain.DomainEdge(\*args)

Bases: `Basic`

Symbolic representation of a domain boundary edge.

This class acts as a base type for symbolic enum-like objects that
represent the start or end of a domain.

#### START

Singleton instance representing the start of the domain.

* **Type:**
  [sofia.domain.domain.DomainStart](#sofia.domain.domain.DomainStart)

#### END

Singleton instance representing the end of the domain.

* **Type:**
  [sofia.domain.domain.DomainEnd](#sofia.domain.domain.DomainEnd)

### *class* sofia.domain.domain.DomainStart(\*args)

Bases: [`DomainEdge`](#sofia.domain.domain.DomainEdge)

### *class* sofia.domain.domain.DomainEnd(\*args)

Bases: [`DomainEdge`](#sofia.domain.domain.DomainEdge)

### sofia.domain.domain.symbols_or_concrete(given, symbols, name: str)

Return given if it is a concrete value, else the domain’s symbols.

Some domain properties, such as the shape or extent, start out as
symbols the domain creates from its name. They are kept in the
domain’s args so that substituting the dict from domain.concrete() also
replaces them inside the domain. When sympy rebuilds the domain,
\_\_new_\_ gets back either those symbols unchanged or the values
substituted for them. Anything else is rejected, because users set
these through concrete(), not the constructor.
