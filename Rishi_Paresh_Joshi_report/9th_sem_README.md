

# Hilbert Space Fragmentation in Floquet-Driven Ising Chains

This repository hosts the 9th semester thesis report by Rishi Paresh Joshi (5th Year Integrated M.Sc., NISER Bhubaneswar) on Hilbert space fragmentation (HSF) in a periodically driven spin-1/2 Ising chain with transverse and longitudinal fields.

## Report Overview

The core report (10 pages) investigates non-ergodic dynamics in a Floquet-driven Ising model, where symmetric square-wave driving suppresses Floquet heating and induces HSF. Key results include sub-Page entanglement entropy saturation and persistent autocorrelation plateaus, signaling ETH violation without disorder. Analytical Floquet perturbation theory derives constrained single-spin-flip rules, while QuSpin-based exact diagonalization provides numerical evidence for L=8 chains under open/periodic boundaries.

## Contents

- **Rishi_Paresh_Joshi_report-1.pdf**: Full thesis (main report + appendices).
    - **Main Report (Pages 1-13)**: Introduction to Floquet ETH/HSF, model (H(t) = J_t ∑ z_i z_{i+1} + h_t ∑ z_i + g ∑ x_i), perturbation theory, numerics (entanglement S_A(t), C_j(t)), results, conclusions.
    - **Appendices (A-E)**: ETH proofs, basis conventions, Floquet derivations, numerics details, extended data (e.g., detunings for L=3, T/J_0 patterns).
- **Figures**: Embedded plots of S_A(t)/S_Page (Fig. 1.1), autocorrelations C_j(t) vs. steps/T (Figs. 1.2-1.4).


## Key Findings

- First-order Floquet Hamiltonian H_F^(1) filters spin flips via sinc(ΔP T/4), with detunings ΔP ∈ {0, ±2J_0, ±4J_0, ±6J_0, ±8J_0} at h_0=2J_0.
- For T = n J_0 (n=0.5,1,2,3), bulk/edge flips selectively suppressed, yielding subthermal S_A plateaus and nonzero lim C_j^∞.
- Open vs. periodic BCs show edge-bulk contrast in C_j(t).


## Usage

Download `Rishi_Paresh_Joshi_report-1.pdf` (1.9 MB) for full details—GitHub rendering may fail for large PDFs. Codes use QuSpin/SciPy for Floquet operator U_F = exp(-i ∫ H(t) dt), eigen-decomp, SVD for S_A, trace for C(t).

## References

See bibliography in report (21 entries, e.g., Deutsch 1991 ETH, d'Alessio 2014 Floquet heating, Sala 2020 HSF).

## Author \& Supervisor

- **Rishi Paresh Joshi** (rishiparesh.joshi@niser.ac.in), NISER Bhubaneswar.
- **Supervisor**: Dr. Tapan Mishra, Associate Professor, NISER.
- **Acknowledgements**: Biswajit Paul for discussions.

Contributions or extensions (e.g., larger L via tensor networks, higher-order H_F) welcome via issues/PRs.
