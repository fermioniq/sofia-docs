# sofia.field.neighbour_sum

### *class* sofia.field.neighbour_sum.NeighbourSum(\*args)

Bases: `Function`

Sum a per-pair expression over the neighbours of each particle.

* **Parameters:**
  * **expr** – Per-pair expression. field is the value at the particle itself,
    field.neighbour the value at the neighbour. Can be scalar or vector.
  * **pos** – Particle position, determines which particles count as neighbours.
  * **h** – Cutoff radius, only pairs closer than h contribute to the sum.
  * **exclude_self** – If True, skip the particle itself. Only for sums over the same domain.
  * **neighbours** – Positions of the particles to sum over, on this or another particle
    domain. Defaults to pos. In a sum over another domain, fields of that
    domain are the values at the neighbour. .neighbour is only needed
    when both are on the same domain.

#### *property* cutoff *: Basic*

Condition for a neighbour to contribute to the sum.

A neighbour contributes if it is closer than h to the particle.
