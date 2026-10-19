# sofia.field.neighbour_sum

### *class* sofia.field.neighbour_sum.NeighbourSum(\*args)

Bases: `Function`

Sum a per-pair expression over the neighbours of each particle.

* **Parameters:**
  * **expr** – Per-pair expression. field is the value at the particle itself,
    field.neighbour the value at the neighbour. Can be scalar or vector.
  * **pos** – Particle position, determines which particles count as neighbours. Only
    pairs closer than the domain’s smoothing_length contribute to the sum.
  * **exclude_self** – If True, skip the particle itself.

#### *property* cutoff *: Basic*

Condition for a neighbour to contribute to the sum.

A neighbour contributes if it is closer than smoothing_length to the
particle.
