# sofia.field.particle_field

### *class* sofia.field.particle_field.ParticleField(name: str, domain: [ParticleDomain](sofia.domain.particle_domain.md#sofia.domain.particle_domain.ParticleDomain))

Bases: [`Field`](sofia.field.field.md#sofia.field.field.Field)

Symbolic field living on a ParticleDomain.

### *class* sofia.field.particle_field.IndexedParticleField(base, \*args, \*\*kw_args)

Bases: `Indexed`

A ParticleField indexed by a single particle index, e.g. rho[p_i].

### *class* sofia.field.particle_field.Neighbour(field: Basic)

Bases: `Function`

Value of a particle field at a neighbouring particle.

Only meaningful inside a sum over neighbours.

#### *classmethod* eval(field: Basic) → None

Returns a canonical form of cls applied to arguments args.

## Explanation

The `eval()` method is called when the class `cls` is about to be
instantiated and it should return either some simplified instance
(possible of some other class), or if the class `cls` should be
unmodified, return None.

Examples of `eval()` for the function “sign”

```python
@classmethod
def eval(cls, arg):
    if arg is S.NaN:
        return S.NaN
    if arg.is_zero: return S.Zero
    if arg.is_positive: return S.One
    if arg.is_negative: return S.NegativeOne
    if isinstance(arg, Mul):
        coeff, terms = arg.as_coeff_Mul(rational=True)
        if coeff is not S.One:
            return cls(coeff) * cls(terms)
```

### *class* sofia.field.particle_field.ParticleLocus(domain: [ParticleDomain](sofia.domain.particle_domain.md#sofia.domain.particle_domain.ParticleDomain))

Bases: [`Locus`](sofia.field.locus.md#sofia.field.locus.Locus)
