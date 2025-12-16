# AI for Block-Encoding Quantum Circuits

This repository provides a **high-level overview** of a research project focused on
using machine learning to generate **efficient block-encoding quantum circuits**.

> **Note:** The full implementation and dataset are part of an academic research
> codebase and cannot be shared publicly. This repository documents the problem,
> system design, and my technical contributions.

---

## Motivation

### Why Block Encoding?
Many foundational quantum algorithms—such as HHL, QSVT, Hamiltonian simulation,
and amplitude amplification—require an efficient **block encoding** of a matrix
or operator \(A\).

Constructing such circuits manually is difficult and often requires:
- Careful ancilla management
- Numerical stability guarantees
- Extensive circuit optimization

For larger operators, heuristic builders (e.g., RACBEM) become computationally
expensive and produce circuits that are far from optimal.

---

## Research Goal

The goal of this project is to build a **data-driven pipeline** that allows a
machine-learning model to learn how to generate **correct and low-cost**
block-encoding circuits.

**Input:** Matrix \(A\)  
**Output:** Quantum circuit (QASM / gate list) that block-encodes \(A\)

---

## System Overview

The research pipeline consists of four core components:

### 1. Dataset Generation
- Automated generation of operator–circuit pairs \((A, U_A)\)
- Baseline circuits constructed using RACBEM
- Multiple transpiled variants produced to capture optimization tradeoffs

### 2. Circuit Cost Metrics
Each circuit is evaluated using multiple volume metrics:
- Pre-transpilation volume
- Post-transpilation depth-based volume
- Weighted gate-count volume

These metrics serve as **training targets** for machine-learning models.

---

### 3. Block-Encoding Verifier
A custom verifier validates whether a candidate circuit **correctly block-encodes**
a target operator \(A\).

Key properties:
- Extracts the encoded block from a simulated circuit
- Accounts for ancilla placement and global phase
- Uses Frobenius-norm error under numerical tolerance

This verifier is critical for:
- Filtering invalid model outputs
- Enforcing correctness during training
- Preventing reward hacking

---

### 4. ML Training Loop (Planned / In Progress)
- Model receives matrix \(A\)
- Outputs a candidate circuit
- Verifier checks correctness
- Valid circuits are scored using volume metrics
- Model is trained to minimize circuit cost while preserving correctness

---

## My Contributions

- Designed the end-to-end dataset generation pipeline
- Implemented standardized circuit-volume metrics
- Developed a robust block-encoding verifier tolerant to ancilla layout and phase
- Built tooling to scale dataset generation on HPC systems
- Defined ML training targets and validation strategy

---

## Key Challenges

- Verifying block encodings under numerical noise
- Handling arbitrary ancilla layouts
- Scaling dataset generation for large operators
- Preventing invalid circuits from contaminating training signals

---

## Research Status

- Large-scale dataset generation completed
- Verifier validated across multiple operator sizes
- ML model architecture and training loop under active development

---

## Summary

This project explores how **machine learning can assist quantum circuit
synthesis**, specifically for the challenging task of block encoding.

The work combines:
- Quantum algorithm theory
- Circuit optimization
- Large-scale dataset generation
- Verification-driven ML training

and serves as a foundation for learning-driven quantum compiler techniques.
