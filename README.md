# Machine Learning–Enhanced Zero-Noise Extrapolation for Noisy Intermediate-Scale Quantum Systems

## Overview

This repository presents a **from-scratch, research-grade implementation** of **Machine Learning–assisted Zero-Noise Extrapolation (ML-ZNE)** for quantum circuits executed under realistic hardware noise. The work integrates **quantum noise modeling, circuit-level noise scaling, classical deep learning, and downstream application analysis**, forming a unified experimental framework suitable for doctoral-level research portfolios.

The project is entirely simulation-based but grounded in **realistic IBM Quantum hardware calibrations**, using backend-derived noise models rather than artificial or ad-hoc noise injections. The objective is to demonstrate how **data-driven models can outperform classical analytical extrapolation methods** for error mitigation in NISQ-era quantum algorithms.

---

## Key Contributions

- **Hardware-calibrated noise modeling** using IBM Quantum backend properties (T1, T2, readout error, gate error, CX error).
- **Density-matrix–level noisy simulation** to preserve physical validity under noise.
- **Multiple Zero-Noise Extrapolation (ZNE) strategies**:
  - No mitigation (baseline)
  - Static polynomial ZNE
  - ML-based adaptive ZNE
- **Hybrid CNN–LSTM architectures** for learning noise-response mappings:
  - PyTorch-based model using temporal calibration histories
  - TensorFlow-based model trained on circuit-folding noise trajectories
- **Rigorous sanity checks and physical constraints** embedded throughout the pipeline.
- **Generalization analysis** across circuit depth and problem instances.
- **Application-driven evaluation**, including:
  - Observable estimation (⟨Z⟩, ⟨Z₀Z₁⟩)
  - Variational/QAOA-style portfolio Hamiltonians
  - Risk-adjusted performance metrics (Sharpe ratio analogs)

---

## Methodology

### 1. Realistic Noise Modeling
- Backend: `FakeMontrealV2`
- Extracted parameters:
  - Qubit relaxation (T1)
  - Dephasing (T2)
  - Readout errors
  - Single-qubit gate errors (SX)
  - Two-qubit gate errors (CX)
- Noise instantiated via `NoiseModel.from_backend`

### 2. Circuit Construction & Observables
- Parametric multi-qubit circuits with controllable depth
- Entangling CX chains
- Observable evaluation using `SparsePauliOp`
- Exact reference values computed via statevector simulation

### 3. Noise Scaling (Circuit Folding)
- Linear and higher-order noise amplification using unitary folding:
  - λ ∈ {1, 3, 5, 7}
- Density matrix extraction for expectation evaluation
- Ensures monotonic noise growth and physical bounds

### 4. Static Zero-Noise Extrapolation
- Polynomial fitting (quadratic)
- Extrapolation to zero-noise limit
- Used as a classical benchmark

### 5. Machine Learning–Based ZNE

#### Architecture A (PyTorch)
- Input: Temporal calibration histories
- CNN for spatial (qubit-wise) feature extraction
- LSTM for temporal noise dynamics
- Output: Learned noise-scaling parameter (λ̂)

#### Architecture B (TensorFlow)
- Input: Folded-circuit noisy expectations
- Conv1D + LSTM hybrid
- End-to-end regression from noisy measurements to ideal observable

### 6. Evaluation Criteria
- Absolute error against ideal expectation
- Mean Absolute Error (MAE) across test distributions
- Robustness to circuit depth
- Downstream task performance (portfolio Hamiltonians)

---

## Results Summary

Across all evaluated regimes:

- **ML-ZNE consistently outperforms static ZNE**
- Error reductions persist under:
  - Increased circuit depth
  - Varying circuit structures
  - Application-specific Hamiltonians
- Learned models generalize beyond training distributions
- Classical extrapolation often overfits or diverges under strong noise

Representative findings include:
- Lower MAE for ML-ZNE compared to static ZNE
- Improved convergence toward ideal expectation values
- Enhanced stability under aggressive noise scaling

---

## Applications Demonstrated

- **Observable Estimation** for entangled quantum states
- **Variational Algorithms** (QAOA-style circuits)
- **Quantum Portfolio Optimization**
  - Hamiltonian encoding of expected returns and covariances
  - Noise-aware cost estimation
  - Risk-adjusted performance comparison

---

## Repository Structure

- Quantum noise extraction and simulation
- Circuit folding and ZNE utilities
- PyTorch CNN–LSTM training pipeline
- TensorFlow ML-ZNE training and evaluation
- Visualization and statistical analysis
- Exportable results for reproducibility

---

## Research Significance

This work demonstrates that **error mitigation in quantum computing can be reframed as a supervised learning problem**, where models infer latent noise structure directly from execution data. Unlike fixed analytical extrapolation, the ML-based approach adapts to:

- Hardware-specific noise profiles
- Circuit structure
- Depth-dependent error growth

The framework is extensible to:
- Real hardware execution
- Other mitigation strategies (PEC, dynamical decoupling)
- Larger variational workloads

---

## Reproducibility

- All simulations use fixed random seeds where applicable
- Hardware noise derived from backend calibration data
- Results exported to CSV for independent verification
- No proprietary datasets required

---

## Disclaimer

This repository is intended as a **research artifact** demonstrating methodological rigor, system-level integration, and experimental validation in quantum machine learning and error mitigation. The implementation emphasizes clarity, correctness, and extensibility rather than hardware-specific optimization.

---

## Keywords

Quantum Error Mitigation, Zero-Noise Extrapolation, Machine Learning, CNN-LSTM, NISQ, Density Matrix Simulation, IBM Quantum, QAOA, Quantum Finance, Hybrid Quantum-Classical Systems
