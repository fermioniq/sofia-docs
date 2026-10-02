# sofia.domain.domain

### *class* sofia.domain.domain.Domain(transformation: [CoordSys](sofia.domain.coordinate.md#sofia.domain.coordinate.CoordSys) = global)

Bases: `Basic`

The position space where a field lives.

#### dim(coordinate: Symbol) → int

Return the dimension index of a coordinate.

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
