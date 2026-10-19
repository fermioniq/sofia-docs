# sofia.domain.particle_domain

### *class* sofia.domain.particle_domain.ParticleDomain(ndim: Literal[1, 2, 3] = 3, name: str = 'P', \_n_particles: Expr | int | None = None, \_extent: Tuple | None = None, \_smoothing_length: Expr | float | None = None)

Bases: [`Domain`](sofia.domain.domain.md#sofia.domain.domain.Domain)

Symbolic representation of a particle domain.

A domain where fields are indexed by particle.
Particles live in ndim-dimensional Cartesian space.

* **Parameters:**
  * **ndim** – Spatial dimensionality.
  * **name** – Name used to identify the particle domain.

#### *property* smoothing_length *: Expr*

Cutoff radius of every NeighbourSum on this domain.

#### concrete(n_particles: int | None = None, extent: tuple[Numeric | tuple[Numeric, Numeric] | list[Numeric], ...] | list[Numeric | tuple[Numeric, Numeric] | list[Numeric]] | Numeric | None = None, smoothing_length: float | None = None, subs: dict | None = None) → dict[Symbol, int | float]

Return substitutions corresponding to the concrete numbers of the domain.
