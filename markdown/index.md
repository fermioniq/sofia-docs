<!-- sofia documentation master file, created by
sphinx-quickstart on Wed Dec 10 15:19:20 2025.
You can adapt this file completely to your liking, but it should at least
contain the root `toctree` directive. -->

# Sofia

– Version 0.4.1.dev7+2a6875f –

**Sofia** is a symbolic compute engine with high-level interface for building partial differential equations solvers.

Develop a full simulation in a few lines of code:

- **Available as Python package**: fast and easy installation (no compilation required) on any platform, and ready for integration in automated workflows and AI coding tools
- **Symbolic math input**: high-level interface for writing symbolic mathematical equations
- **Platform-agnostic code generation**: automatic translation of symbolic solver programs into code that runs on CPU, GPU, TPU and other hardware in the future
- **High-performance simulations**: no low-level coding and optimisation required

## Contents

* [Installation](installation.md)
  * [Requirements](installation.md#requirements)
  * [Installing uv](installation.md#installing-uv)
  * [Installation](installation.md#id2)
  * [Developing and Contributing](installation.md#developing-and-contributing)
* [User Manual](guide/index.md)
  * [Getting Started](guide/getting_started.md)
  * [SymPy Basics](guide/sympy_basics.md)
  * [Domains and Fields](guide/domains_fields.md)
  * [Field Expressions](guide/field_expressions.md)
  * [Discrete Interface](guide/discrete_interface.md)
  * [Automatic Discretisation](guide/auto_discretisation.md)
  * [Boundary Conditions](guide/boundary_conditions.md)
  * [Running a Simulation](guide/build_solver.md)
  * [Writing IR](guide/writing_ir.md)
* [API Reference](api/index.md)
  * [sofia.domain package](api/sofia.domain.md)
  * [sofia.field package](api/sofia.field.md)
  * [sofia.geometry package](api/sofia.geometry.md)
  * [sofia.pde package](api/sofia.pde.md)
  * [sofia.ir package](api/sofia.ir.md)
