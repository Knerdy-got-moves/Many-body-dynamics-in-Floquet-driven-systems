# Many-Body dynamics in Floquet-driven systems

**MSc thesis repository (9th semester progress)**

**Student**: Rishi Paresh Joshi (5th Year Integrated M.Sc., NISER Bhubaneswar)

**Supervisor**: Dr. Tapan Mishra, Associate professor, NISER

---

## Overview

This repository contains the work on my MSc thesis (part 1) on investigating **Hilbert space fragmentation (HSF)** and **eigenstate thermalization hypothesis (ETH) violation** in periodically driven (Floquet) quantum Ising chains. The research combines analytical Floquet perturbation theory with numerical probes like entanglement entropy and autocorrelation evolution to demonstrate prethermal non-ergodic dynamics in non-integrable many-body systems without disorder.

## Recent accomplishments

The computational work in this repository extends significantly beyond the formal thesis document, particularly in the following areas:

### 1. **Hilbert Space Fragmentation Analysis** ([`Autocorrelation,_HSF_and_edge_modes_in_open_Ising_chain.ipynb`](./Autocorrelation,_HSF_and_edge_modes_in_open_Ising_chain.ipynb))
- **Fragment decomposition**: Performed detailed fragmentation analysis for specific system sizes (L=8), identifying the structure and connectivity of disconnected Krylov subspaces
- **Fragment-specific statistics**: Analyzed the properties of individual fragments, including their dimensions and structure
- **Connectivity graphs**: Constructed and visualized the fragmentation of the Hilbert space under the effective Floquet Hamiltonian

### 2. **Fragment page value analysis** ([`Periodic_Frag_and_EE_kicked_Ising_chain.ipynb`](./Periodic_Frag_and_EE_kicked_Ising_chain.ipynb))
- **Key finding**: Demonstrated that entanglement entropy saturates at the **fragment-specific Page value** rather than the full system Page value
- **Monte Carlo sampling**: Used random sampling within fragments to compute fragment Page values
- **ETH within fragments**: Showed that states thermalize only within their respective fragments, not the entire Hilbert space
- **Quantitative verification**: Numerically confirmed that $S_A^{\text{sat}} \approx S_{\text{Page}}^{\text{fragment}} < S_{\text{Page}}^{\text{system}}$

### 3. **Edge mode characterization** ([`Autocorrelation,_HSF_and_edge_modes_in_open_Ising_chain.ipynb`](./Autocorrelation,_HSF_and_edge_modes_in_open_Ising_chain.ipynb))
- **Prethermal edge memory**: Demonstrated that edge modes persist for hundreds of Floquet cycles at resonant frequencies ($T = 2\pi/J_0$)
- **Magnetization profiles**: Generated contour plots showing spatial and temporal evolution of local magnetization, revealing edge-bulk contrast
- **Symmetry analysis**: Investigated inversion and chiral symmetries and their connection to edge mode protection
- **System size dependence**: Studied edge memory conservation across different chain lengths, finding no systematic size-dependent decay
- **Non-SPT nature**: Established that edge modes arise from the structure of the effective Floquet Hamiltonian rather than conventional symmetry-protected topological (SPT) mechanisms

These results provide concrete numerical evidence for prethermal Hilbert space fragmentation and demonstrate phenomena not fully covered in the written thesis.

---

## Repository structure

### 📊 Computational notebooks (main results)

#### 1. **Entanglement entropy & fragmentation (periodic boundaries)**
**File**: [`Periodic_Frag_and_EE_kicked_Ising_chain.ipynb`](./Periodic_Frag_and_EE_kicked_Ising_chain.ipynb)
**Open in Colab**: [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Knerdy-got-moves/Many-body-dynamics-in-Floquet-driven-systems/blob/main/Periodic_Frag_and_EE_kicked_Ising_chain.ipynb)

**Key features**:
- Bipartite entanglement entropy dynamics in kicked Ising chains (PBC)
- Violation of ETH: saturation below thermal Page value at resonant frequencies
- **Fragment-specific Page value calculations via Monte Carlo sampling**
- Construction and analysis of Hilbert space connectivity graphs
- Identification of largest fragments and their properties
- Demonstrates that entanglement thermalization is fragment-limited

**Main results**:
- At $T = n\pi/J_0$, entanglement entropy plateaus at $S_A^{\text{sat}} < S_{\text{Page}}^{\text{system}}$
- Fragmentation quantitatively explains reduced thermalization
- States thermalize only to fragment-specific ensembles

---

#### 2. **Autocorrelation, fragmentation & edge modes (open boundaries)**
**File**: [`Autocorrelation,_HSF_and_edge_modes_in_open_Ising_chain.ipynb`](./Autocorrelation,_HSF_and_edge_modes_in_open_Ising_chain.ipynb)
**Open in Colab**: [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Knerdy-got-moves/Many-body-dynamics-in-Floquet-driven-systems/blob/main/Autocorrelation,_HSF_and_edge_modes_in_open_Ising_chain.ipynb)

**Key features**:
- Infinite-temperature autocorrelation functions $C_j(nT) = \langle \sigma_j^z(nT) \sigma_j^z(0) \rangle_\infty$
- **Detailed fragmentation analysis**: decomposition into disconnected sectors
- **Prethermal edge memory**: long-lived magnetization at boundary sites
- Magnetization contour plots revealing edge-bulk dynamics
- Symmetry analysis (inversion, chiral) and edge mode mechanisms
- System size dependence of edge memory lifetime

**Main results**:
- Non-zero autocorrelation plateaus signal ETH violation
- Edge sites retain memory for $\sim 300$ Floquet steps at $T = 2\pi/J_0$
- Edge modes emerge from effective Floquet Hamiltonian structure: $H_F^{(1)} \sim -g\sum_{i=2}^{L-1}\pi_{i-1}\sigma_i^x\pi_{i+1}$ (lacks edge flip terms)
- Edge memory shows no systematic system-size dependence
- Fragmentation explains both bulk thermalization failure and edge persistence

---

### 📄 Thesis report (10-page main content)

**Folder**: [`Rishi_Paresh_Joshi_report/`](./Rishi_Paresh_Joshi_report/)
**Main document**: [`Rishi_Paresh_Joshi_report.pdf`](./Rishi_Paresh_Joshi_report/Rishi_Paresh_Joshi_report.pdf)

**Contents**:
- **Pages 1-13**: Main report (introduction, model, Floquet perturbation theory, numerical methods, results, conclusions)
- **Appendices A-E**: ETH derivations, basis conventions, Floquet formalism details, numerical implementation, extended data

**Key sections**:
1. **Introduction**: Floquet systems, ETH, Hilbert space fragmentation
2. **Model**: Kicked Ising chain Hamiltonian $H(t) = -J(t)\sum_i \sigma_i^z\sigma_{i+1}^z - h(t)\sum_i \sigma_i^z - g\sum_i \sigma_i^x$
3. **Analytical results**: First-order Floquet Hamiltonian, spin-flip suppression via sinc filters
4. **Numerical results**: Entanglement entropy saturation, autocorrelation plateaus (L=8)
5. **Conclusions**: Prethermal fragmentation without disorder or integrability

**Quick navigation**: See [`Rishi_Paresh_Joshi_report/9th_sem_README.md`](./Rishi_Paresh_Joshi_report/9th_sem_README.md) for detailed overview.

---

### 🎤 9th Semester presentation

**Folder**: [`9th_sem_presentation/`](./9th_sem_presentation/)
**Slides**: [`9th_sem_presentation.pdf`](./9th_sem_presentation/9th_sem_presentation.pdf)

**Coverage**:
- Out-of-equilibrium dynamics and Floquet formalism
- Eigenstate thermalization hypothesis and Floquet heating
- ETH-violating mechanisms (integrability, MBL, scars, fragmentation)
- Model description and effective Hamiltonian derivation
- Numerical diagnostics: entanglement entropy and autocorrelations
- Key results and prethermal fragmentation signatures

**Quick navigation**: See [`9th_sem_presentation/9th_sem_presentation_README.md`](./9th_sem_presentation/9th_sem_presentation_README.md) for slide breakdown.

---

## Physical system

The system under investigation is a **periodically driven spin-1/2 Ising chain** of length $L$ with symmetric square-wave driving:

$$H(t) = H_0(t) + H_1$$

where:

$$H_0(t) = -J(t)\sum_{i}\sigma_i^z\sigma_{i+1}^z - h(t)\sum_{i}\sigma_i^z$$

$$H_1 = -g\sum_{i}\sigma_i^x$$

The couplings follow:

$$J(t) = \begin{cases} +J_0, & 0 \le t < T/2 \\ -J_0, & T/2 \le t < T \end{cases}, \quad h(t) = \begin{cases} +h_0, & 0 \le t < T/2 \\ -h_0, & T/2 \le t < T \end{cases}$$

with $|g| \ll \{|J_0|, |h_0|\}$ (weak transverse perturbation) and $h_0 = 2J_0$ (typical parameter choice).

### Boundary conditions
- **Periodic (PBC)**: $\sigma_{L+1}^z \equiv \sigma_1^z$ — used for entanglement entropy studies
- **Open (OBC)**: No wraparound — used for edge mode and autocorrelation studies

---

## Key physical phenomena

### 1. Prethermal Hilbert space fragmentation
At resonant driving periods $T = n\pi/J_0$ (n ∈ ℤ), the effective first-order Floquet Hamiltonian becomes:

$$H_F^{(1)} \approx -g\sum_{i=2}^{L-1}\pi_{i-1}\sigma_i^x\pi_{i+1}$$

where $\pi_i = (\mathbb{I} - \sigma_i^z)/2$. This Hamiltonian:
- Suppresses spin flips unless neighbors satisfy specific classical configurations
- Fragments the Hilbert space into exponentially many disconnected sectors
- Leads to sub-thermal dynamics on prethermal timescales

### 2. ETH violation without disorder
Unlike many-body localization, the system exhibits:
- Non-ergodic dynamics in a **non-integrable, translation-invariant** system
- No quenched disorder or conservation laws required
- Mechanism: constrained dynamics from Floquet engineering

### 3. Fragment-limited thermalization
- States thermalize only within their respective fragments
- Observables converge to fragment microcanonical averages
- Entanglement entropy: $S_A^{\text{sat}} = S_{\text{Page}}^{\text{fragment}} < S_{\text{Page}}^{\text{system}}$

### 4. Prethermal edge memory (OBC)
- Edge sites retain initial magnetization for $\sim 10^2$ Floquet cycles
- Mechanism: effective Hamiltonian lacks edge flip terms ($H_F^{(1)}$ sums from $i=2$ to $L-1$)
- Not protected by conventional symmetries (non-SPT)

---

## Methodology

### Analytical
- **Floquet perturbation theory**: Derives effective Hamiltonian $H_F = H_F^{(1)} + \mathcal{O}(g^3)$ via cumulant expansion
- **Detuning analysis**: Spin-flip amplitudes scale as $\text{sinc}(\Delta P T/4)$ where $\Delta P$ is the energy mismatch in the classical diagonal basis

### Numerical
- **Package**: [QuSpin](http://weinbe58.github.io/QuSpin/) for exact diagonalization
- **System sizes**: Primarily $L=8$ (Hilbert dimension $2^8 = 256$)
- **Diagnostics**:
  - Bipartite entanglement entropy via Schmidt decomposition
  - Infinite-temperature autocorrelations from Haar-random initial states
  - Fragmentation via connectivity graph analysis

---

## Key results summary

| **Observable** | **Diagnostic** | **ETH prediction** | **Our result** | **Interpretation** |
|----------------|----------------|--------------------|-----------------|--------------------|
| Entanglement $S_A(t)$ | Time evolution | $S_A^{\text{sat}} = S_{\text{Page}}^{\text{system}}$ | $S_A^{\text{sat}} = S_{\text{Page}}^{\text{fragment}} < S_{\text{Page}}^{\text{system}}$ | Fragment-limited thermalization |
| Autocorrelation $C_j(t)$ | Infinite-$T$ ensemble | $\lim_{t\to\infty} C_j(t) = 0$ | $\lim_{t\to\infty} C_j(t) \neq 0$ | Memory of initial conditions |
| Edge magnetization | Open boundaries | Decays rapidly | Persists $\sim 300$ steps | Prethermal edge modes |
| Hilbert space | Connectivity | Fully connected | Exponentially many fragments | HSF |

---

## Dependencies

All computational notebooks require:

```bash
pip install quspin numpy scipy matplotlib numba
```

QuSpin handles:
- Hamiltonian construction in spin bases
- Sparse matrix exponentiation for time evolution
- Eigenvalue decomposition for Floquet operators

---

## Running the notebooks

### Option 1: Google Colab (recommended)
Click the "Open in Colab" badges in the notebook sections above. QuSpin will be installed automatically.

### Option 2: Local Jupyter

```bash
git clone https://github.com/Knerdy-got-moves/Many-body-dynamics-in-Floquet-driven-systems.git
cd Many-body-dynamics-in-Floquet-driven-systems
pip install quspin numpy scipy matplotlib
jupyter notebook
```

---

## References

This work builds upon foundational studies in Floquet physics and Hilbert space fragmentation:

### Floquet systems & ETH
1. L. D'Alessio and M. Rigol, "Long-time behavior of isolated periodically driven interacting lattice systems," *Phys. Rev. X* **4**, 041048 (2014). [doi:10.1103/PhysRevX.4.041048](https://doi.org/10.1103/PhysRevX.4.041048)
2. A. Lazarides, A. Das, and R. Moessner, "Fate of many-body localization under periodic driving," *Phys. Rev. Lett.* **115**, 030402 (2015). [doi:10.1103/PhysRevLett.115.030402](https://doi.org/10.1103/PhysRevLett.115.030402)

### Hilbert space fragmentation
3. P. Sala et al., "Ergodicity breaking arising from Hilbert space fragmentation in dipole-conserving Hamiltonians," *Phys. Rev. X* **10**, 011047 (2020). [doi:10.1103/PhysRevX.10.011047](https://doi.org/10.1103/PhysRevX.10.011047)
4. V. Khemani, M. Hermele, and R. Nandkishore, "Localization from Hilbert space shattering," *Phys. Rev. B* **101**, 174204 (2020). [doi:10.1103/PhysRevB.101.174204](https://doi.org/10.1103/PhysRevB.101.174204)

### Prethermal fragmentation in driven systems
5. **S. Ghosh, I. Paul, and K. Sengupta**, "Prethermal fragmentation in a periodically driven fermionic chain," *Phys. Rev. Lett.* **130**, 120401 (2023). [doi:10.1103/PhysRevLett.130.120401](https://doi.org/10.1103/PhysRevLett.130.120401)
   *(Primary inspiration for this thesis)*

### ETH foundations
6. J. M. Deutsch, "Quantum statistical mechanics in a closed system," *Phys. Rev. A* **43**, 2046 (1991). [doi:10.1103/PhysRevA.43.2046](https://doi.org/10.1103/PhysRevA.43.2046)
7. M. Srednicki, "Chaos and quantum thermalization," *Phys. Rev. E* **50**, 888 (1994). [doi:10.1103/PhysRevE.50.888](https://doi.org/10.1103/PhysRevE.50.888)

### Page entropy
8. D. N. Page, "Average entropy of a subsystem," *Phys. Rev. Lett.* **71**, 1291 (1993). [doi:10.1103/PhysRevLett.71.1291](https://doi.org/10.1103/PhysRevLett.71.1291)

### Computational tools
9. P. Weinberg and M. Bukov, "QuSpin: a Python package for dynamics and exact diagonalization of quantum many body systems," *SciPost Phys.* **2**, 003 (2017). [doi:10.21468/SciPostPhys.2.1.003](https://doi.org/10.21468/SciPostPhys.2.1.003)

---

## Contact & acknowledgments

**Student**: Rishi Paresh Joshi
**Email**: rishiparesh.joshi@niser.ac.in
**Institution**: National Institute of Science Education and Research (NISER), Bhubaneswar

**Supervisor**: Dr. Tapan Mishra, Associate Professor, NISER

**Acknowledgments**: Special thanks to Biswajit Paul for valuable discussions.

---

## License

This project is available for academic and educational purposes. Please cite appropriately if you use any results or code from this repository.

---

**Keywords**: Floquet systems, Hilbert space fragmentation, eigenstate thermalization hypothesis, entanglement entropy, autocorrelation, Ising model, prethermal dynamics, non-ergodic behavior, quantum many-body physics, QuSpin
