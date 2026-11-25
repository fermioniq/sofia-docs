# sofia.field.neighbour_sum

### *class* sofia.field.neighbour_sum.NeighbourSum(\*args)

Bases: `Function`

Sum a per-pair expression over the neighbours of each particle.

The sum is evaluated at the particles of the field it is assigned to.

* **Parameters:**
  * **expr** – Per-pair expression. field is the value at the particle itself,
    field.neighbour the value at the neighbour. Can be scalar or vector.
  * **h** – Cutoff radius, only pairs closer than h contribute to the sum.
  * **neighbours** – Particle domain to sum over. Defaults to the domain the sum is evaluated
    at, i.e. a sum over the same particles. In a sum over another domain,
    fields of that domain are the values at the neighbour. .neighbour is
    only needed when both are on the same domain.
  * **exclude_self** – If True, skip the particle itself. Only for sums over the same domain.

#### cutoff(pos: Tuple) → Basic

Condition for a neighbour to contribute to the sum.

A neighbour contributes if it is closer than h to the particle at pos.
