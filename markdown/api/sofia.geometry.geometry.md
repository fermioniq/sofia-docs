# sofia.geometry.geometry

### *class* sofia.geometry.geometry.Geometry(name: str, support_winding_number: bool = False)

Bases: `Basic`

#### sdf(domain: [Domain](sofia.md#sofia.Domain), max_dist=1000.0, query_method: Literal['mesh_query_point', 'mesh_query_point_sign_normal', 'mesh_query_point_sign_parity', 'mesh_query_point_sign_winding_number', 'mesh_query_point_no_sign'] = 'mesh_query_point', epsilon: float = 0.001, n_sample: int = 1, perturbation_scale: float = 0.1, accuracy: float = 2.0, threshold: float = 0.5)

Return symbolic signed-distance field.

The generated warp kernel uses warp.mesh_query_point which is the
most general and robust method, but requires watertight meshes.

### Examples

The resulting symbolic SDF function can be used either on its own:
>> sdf = Field(‘sdf’, G)
>> geo = Geometry(‘geo’)
>> sdf << geo.sdf(G, max_dist=1e2)

Or inside a larger expression:
>> u = Field(‘u’, G)
>> u << u + 2 \* geo.sdf(G)

* **Parameters:**
  * **domain** – Domain that contains the `physical_coordinates` on which the
    SDF should be computed.
  * **max_dist** – Maximal distance for the SDF kernel. Beyond this distance the SDF is not
    computed anymore to improve performance.
  * **query_method** – 

    Warp query method to use. Each comes with its own tradeoffs between
    robustness, accuracy, versatility and efficiency:
    > - `mesh_query_point`:
    >   : - relatively robust
    >     - medium expensive
    >     - requires watertight mesh
    >     - sign check runs over entire mesh, closest point is limited
    >       : by `max_dist`
    > - `mesh_query_point_sign_normal`:
    >   : - fast
    >     - robust for well-conditioned mesh
    >     - requires watertight and non-self intersecting mesh
    >     - extra arguments: `epsilon`
    > - `mesh_query_point_sign_parity`:
    >   : - sign (inside/outside) is determined by casting n_sample rays from
    >       : point and counting how many mesh faces each ray crosses
    >         (deterministic)
    >     - sign check runs over entire mesh, closest point is limited by
    >       : `max_dist`
    >     - extra arguments: `n_sample`, `perturbation_scale`
    > - `mesh_query_point_sign_winding_number`:
    >   : - most robust but also most expensive
    >     - robust for poorly conditioned mesh
    >     - does not require watertight mesh
    >     - extra arguments: `accuracy`, `threshold`
    >     - *NOTE*: requires `support_winding_number=True` in the
    >       : `Geometry` construction, otherwise the kernel falls back
    >         to `mesh_query_point`
    > - `mesh_query_point_no_sign`:
    >   : - fastest, but does not compute the sign
  * **epsilon** – (only for `mesh_query_point_sign_normal`)
    Epsilon treating distance values as equal, when locating the minimum
    distance vertex/face/edge, as a fraction of the average edge length,
    also for treating closest point as being on edge/vertex
  * **n_sample** – (only for `mesh_query_point_sign_parity`)
    Number of rays used to classify the sign. Prefer a positive, odd value;
    larger values are more robust. A non-positive value casts no rays
    and classifies the point as inside.
  * **perturbation_scale** – (only for `mesh_query_point_sign_parity`)
    Scale of the perturbation. Each ray perturbs the base direction (1, 1, 1)
    by an offset drawn per axis from a uniform distribution over
    [-perturbation_scale, perturbation_scale).
  * **accuracy** – (only for `mesh_query_point_sign_winding_number`)
    Accuracy for computing the winding number with fast winding number method
    utilizing second-order dipole approximation
  * **threshold** – (only for `mesh_query_point_sign_winding_number`)
    The threshold of the winding number to be considered inside
* **Returns:**
  Symbolic `SDF` function that will be converted to an SDF kernel by
  codegen.
* **Return type:**
  sdf_function

### *class* sofia.geometry.geometry.SDF(\*args)

Bases: `Function`
