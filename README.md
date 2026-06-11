# SHARC-SimLink-Rust

> High-performance Rust simulation kernel and interoperability layer for SHARC, providing accelerated spectrum sharing, NTN, propagation, interference, and Monte Carlo simulations while maintaining scientific compatibility with the original framework.


---

## Motivation

SHARC is a powerful simulation framework used for spectrum sharing and compatibility studies. However, large-scale scenarios involving:

* NTN constellations
* Satellite visibility analysis
* Interference aggregation
* Monte Carlo campaigns
* Large IMT deployments

can require substantial computational resources.

SHARC-SimLink-Rust aims to provide a modern high-performance simulation backend capable of leveraging:

* Native machine code
* Multi-core CPUs
* Efficient memory management
* Data-parallel execution
* Scientific reproducibility

while maintaining compatibility with existing SHARC workflows.

---

## Project Goals

### Performance

Accelerate computationally intensive simulation workloads.

### Compatibility

Preserve compatibility with existing SHARC configurations, topologies, and workflows.

### Scientific Integrity

Ensure numerical consistency with the original SHARC implementation through automated validation.

### Extensibility

Provide a modular architecture for future simulation capabilities and research projects.

---

## Architecture

```text
                SHARC
                  │
                  ▼
           SHARC-SimLink
                  │
      ┌───────────┴───────────┐
      │                       │
      ▼                       ▼
Python Backend         Rust Backend
(Reference)            (Accelerated)
      │                       │
      └───────────┬───────────┘
                  ▼
              Results
```

The Python backend serves as the scientific reference implementation, while the Rust backend provides accelerated execution.

---

## Planned Components

### Geometry Engine

* Geodetic ↔ ECEF conversion
* ENU transformations
* Satellite visibility
* Elevation angle calculations
* Off-axis angle calculations
* Footprint generation

### Propagation Engine

* Free-space path loss
* ITU-R propagation models
* Atmospheric attenuation
* Clutter and terrain losses

### Antenna Engine

* Radiation patterns
* Beamforming support
* NTN antenna models
* Gain calculations

### Interference Engine

* Aggregate interference
* INR calculations
* C/I calculations
* C/N calculations

### Monte Carlo Engine

* Snapshot execution
* Parallel campaign processing
* Statistical aggregation

---

## Repository Structure

```text
SHARC-SimLink-Rust/
│
├── docs/
│
├── python/
│   ├── simlink/
│   ├── backends/
│   ├── validators/
│   └── benchmarks/
│
├── rust/
│   ├── geometry/
│   ├── propagation/
│   ├── antenna/
│   ├── topology/
│   ├── interference/
│   ├── statistics/
│   ├── engine/
│   └── bindings/
│
├── tests/
├── examples/
├── benchmarks/
└── scripts/
```

---

## Technology Stack

### Rust

* Rust
* Rayon
* PyO3
* Maturin
* Nalgebra
* ndarray

### Python

* NumPy
* SciPy
* PyTest
* SHARC

### Validation

* Automated cross-validation
* Scientific regression testing
* Numerical consistency verification

---

## Development Roadmap

### Phase 0

* SHARC architecture analysis
* Profiling and bottleneck identification

### Phase 1

* SimLink abstraction layer

### Phase 2

* Rust geometry engine

### Phase 3

* Python-Rust bindings

### Phase 4

* Scientific validation framework

### Phase 5

* Propagation engine

### Phase 6

* Interference engine

### Phase 7

* Monte Carlo engine

### Phase 8

* Antenna engine

### Phase 9

* Complete Rust backend

For detailed planning, see:

```text
docs/implementation_plan.md
```

---

## Scientific Validation

A core principle of this project is that performance improvements must never compromise scientific correctness.

Every Rust implementation will be validated against the original SHARC implementation using:

* Unit tests
* Regression tests
* Cross-backend comparisons
* Numerical tolerance verification

Target validation thresholds:

| Metric    | Maximum Error |
| --------- | ------------- |
| Distance  | < 1 cm        |
| Angle     | < 0.001°      |
| Power     | < 0.01 dB     |
| Path Loss | < 0.01 dB     |

---

## Current Status

Early Development

The project is currently in the architecture and planning phase.

Initial efforts focus on:

* SHARC profiling
* Backend abstraction
* Geometry engine implementation
* PyO3 integration

---

## Contributing

Contributions are welcome.

Areas of interest include:

* Rust development
* Scientific computing
* Telecommunications
* Satellite systems
* NTN research
* Spectrum sharing studies
* Validation and benchmarking

Please open an issue before starting major changes.

---

## License

GNU Affero General Public License v3.0 (AGPL-3.0)

This project is intended to remain compatible with the open-source ecosystem surrounding SHARC and related scientific research tools.

---

## Acknowledgments

This project is inspired by the SHARC framework and the broader spectrum-sharing research community.

Special thanks to all contributors and researchers involved in the development of SHARC and ITU-R based spectrum coexistence methodologies.
