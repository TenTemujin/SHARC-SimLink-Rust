# SHARC-SimLink-Rust Implementation Plan

## Overview

SHARC-SimLink-Rust is a high-performance Rust-based simulation backend and interoperability layer for the SHARC (Spectrum Harmonized Advanced Radio Calculator) framework.

The project aims to accelerate computationally intensive simulation workloads while preserving full compatibility with the original Python implementation. The architecture follows an incremental migration strategy, allowing both Python and Rust simulation backends to coexist and produce scientifically equivalent results.

---

# Objectives

## Primary Goals

* Preserve compatibility with existing SHARC workflows.
* Accelerate large-scale simulation campaigns.
* Improve scalability for NTN and satellite studies.
* Reduce execution time for Monte Carlo simulations.
* Enable efficient multi-core execution.
* Maintain scientific consistency with the original implementation.
* Create a modular architecture for future extensions.

## Non-Goals

* Rewriting the entire SHARC ecosystem at once.
* Replacing visualization or GUI components.
* Modifying existing SHARC campaign definitions.

---

# System Architecture

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
(Original SHARC)       (SHARC-RS Core)
      │                       │
      └───────────┬───────────┘
                  ▼
             Results Layer
```

---

# Repository Structure

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
│
├── examples/
│
├── benchmarks/
│
└── scripts/
```

---

# Phase 0 — SHARC Analysis and Profiling

## Objectives

Understand the internal architecture and identify performance bottlenecks.

## Tasks

### Execution Flow Analysis

Document:

* Campaign lifecycle
* Topology generation
* Propagation processing
* Interference calculations
* Statistics generation
* Output generation

### Performance Profiling

Tools:

```bash
python -m cProfile
py-spy top
snakeviz
```

Metrics:

* CPU usage
* Memory consumption
* Runtime distribution
* Function-level hotspots

### Deliverables

```text
docs/
├── architecture.md
├── execution_flow.md
├── bottlenecks.md
└── migration_candidates.md
```

---

# Phase 1 — SimLink Abstraction Layer

## Objectives

Create a backend-independent simulation interface.

## Components

### Backend Interface

```python
engine = SimulationEngine(
    backend="python"
)

results = engine.run(campaign)
```

Future usage:

```python
engine = SimulationEngine(
    backend="rust"
)
```

### Backend Implementations

```text
backends/
├── python_backend.py
└── rust_backend.py
```

### Deliverables

* Unified simulation API
* Backend selection mechanism
* Initial test framework

---

# Phase 2 — Geometry Engine

## Objectives

Develop the first Rust simulation module.

## Motivation

Geometry calculations are:

* Computationally intensive
* Easy to validate
* Independent from propagation models

## Features

### Coordinate Systems

* Geodetic to ECEF
* ECEF to Geodetic
* ENU transformations

### Distance Calculations

* Great-circle distance
* Slant range
* Line-of-sight distance

### NTN Functions

* Satellite visibility
* Elevation angle
* Off-axis angle
* Footprint calculations
* Earth central angle

### Deliverables

```text
rust/geometry/
```

---

# Phase 3 — Python-Rust Integration

## Objectives

Expose Rust functionality directly to Python.

## Technology Stack

### Rust

* PyO3
* Maturin

### Python

```python
import sharc_rs

angle = sharc_rs.off_axis(...)
```

## Deliverables

```text
rust/bindings/
```

Python package:

```text
sharc_rs
```

---

# Phase 4 — Scientific Validation Framework

## Objectives

Guarantee numerical equivalence between Python and Rust implementations.

## Validation Categories

### Geometry

* Coordinates
* Distances
* Angles
* Visibility

### Propagation

* Path loss
* Atmospheric losses
* Clutter losses

### Interference

* INR
* C/I
* C/N
* Aggregate interference

## Error Thresholds

| Metric    | Maximum Error |
| --------- | ------------- |
| Distance  | < 1 cm        |
| Angle     | < 0.001°      |
| Power     | < 0.01 dB     |
| Path Loss | < 0.01 dB     |

## Deliverables

```text
validators/
tests/validation/
```

---

# Phase 5 — Propagation Engine

## Objectives

Port propagation models to Rust.

## Candidate Models

* Free Space Path Loss
* ITU-R P.452
* ITU-R P.619
* ITU-R P.2108
* ITU-R P.2109

## Deliverables

```text
rust/propagation/
```

---

# Phase 6 — Interference Engine

## Objectives

Implement high-performance interference calculations.

## Features

### Link Budget Calculations

* Received power
* Carrier power
* Interference power

### Aggregation

* Multi-user aggregation
* Multi-satellite aggregation
* Multi-cell aggregation

### Deliverables

```text
rust/interference/
```

---

# Phase 7 — Monte Carlo Engine

## Objectives

Accelerate campaign execution using parallel processing.

## Technology

### Rayon

```rust
snapshots.par_iter()
```

### Features

* Snapshot scheduling
* Parallel execution
* Statistical aggregation

## Deliverables

```text
rust/engine/
```

---

# Phase 8 — Antenna Engine

## Objectives

Port antenna calculations to Rust.

## Features

* Gain calculations
* Radiation patterns
* Beam steering
* Off-axis attenuation
* NTN antenna models

## Deliverables

```text
rust/antenna/
```

---

# Phase 9 — Complete Rust Backend

## Objectives

Execute complete SHARC campaigns using the Rust simulation kernel.

## Example Configuration

```yaml
simulation:
  backend: rust
```

No modifications should be required to existing SHARC campaign files.

---

# Continuous Integration

## Platforms

* Linux
* Windows
* macOS

## Validation

### Python Backend

```bash
pytest
```

### Rust Backend

```bash
cargo test
```

### Cross-Validation

```bash
pytest tests/validation
```

---

# Benchmarking

## Metrics

* Runtime
* CPU utilization
* Memory consumption
* Scaling efficiency

## Scenarios

### Small Campaign

* Single satellite
* Single IMT network

### Medium Campaign

* Multiple satellites
* Multiple cells

### Large Campaign

* Large LEO constellations
* NTN coexistence studies

---

# Documentation

## User Documentation

* Installation
* Configuration
* Backend selection
* Benchmarking

## Developer Documentation

* Architecture
* API reference
* Contribution guide
* Validation procedures

---

# Long-Term Vision

SHARC-SimLink-Rust aims to become a next-generation computational backend for SHARC, providing:

* High-performance scientific computing
* Multi-core scalability
* NTN-ready simulations
* Large-scale spectrum sharing studies
* Scientific reproducibility
* Full interoperability with the original SHARC ecosystem

The project will follow an incremental migration strategy, ensuring that scientific correctness and compatibility remain the highest priorities throughout development.
