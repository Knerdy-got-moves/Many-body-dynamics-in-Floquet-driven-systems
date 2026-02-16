# Many-Body dynamics in Floquet-driven systems

**MSc thesis repository (9th semester progress)**

**Student**: Rishi Paresh Joshi (5th Year Integrated M.Sc., NISER Bhubaneswar)

**Supervisor**: Dr. Tapan Mishra, Associate professor, NISER

---

## Overview

This repository contains the work on my MSc thesis (part 1) on investigating **Hilbert space fragmentation (HSF)** and **eigenstate thermalization hypothesis (ETH) violation** in periodically driven (Floquet) quantum Ising chains. The research combines analytical Floquet perturbation theory with numerical probes like entanglement entropy and autocorrelation evolution to demonstrate prethermal non-ergodic dynamics in non-integrable many-body systems without disorder.

## Recent accomplishments

The computational work in this repository extends significantly beyond the formal thesis document. The latest developments include optimised large-system-size simulations (up to $L = 12$) and new analyses of entanglement entropy in both OBC and PBC geometries.

### 1. **Hilbert Space Fragmentation Analysis** ([`Autocorrelation/`](./Autocorrelation/))
- **Fragment decomposition**: Performed detailed fragmentation analysis for system sizes up to $L = 12$ ($2^{12} = 4096$ states), identifying the structure and connectivity of disconnected Krylov subspaces
- **Fibonacci dimension**: Verified that the largest fragment (no adjacent $\uparrow\uparrow$ bonds) has dimension $D = F_{L-1} + F_{L+1}$ where $F_n$ is the Fibonacci number (e.g., $D = 322$ at $L = 12$)
- **Up-up bond analysis**: Characterized fragments by the number and pattern of adjacent $\uparrow\uparrow$ bonds ($N_{\text{defect}}$), which is conserved within each fragment at leading order
- **Connectivity graphs**: Constructed and visualized the fragmentation of the Hilbert space under the effective Floquet Hamiltonian via BFS on the connectivity graph

### 2. **Dynamical Freezing** ([`Autocorrelation/Optimised_Autocorrelation_in_large_system_sizes.ipynb`](./Autocorrelation/Optimised_Autocorrelation_in_large_system_sizes.ipynb))
- **Bulk freezing with oscillating edges**: For $h_0/J_0 = 2n$ ($n > 2$) at driving periods $T = m\pi/J_0$, the bulk of the chain is dynamically frozen while edge sites exhibit oscillatory behavior
- **Complete freezing**: At $T = 2\pi/J_0$, the entire system—both bulk and edges—is frozen; autocorrelation saturates at a finite value across all sites
- **Sinc filter mechanism**: Systematically mapped which spin-flip channels are active/suppressed for each driving period via the sinc selection rules

### 3. **Fragment-Resolved Entanglement Entropy** ([`Entanglement_entropy/`](./Entanglement_entropy/))
- **Fragment Page value saturation**: Demonstrated that entanglement entropy saturates at the **fragment-specific Page value** $S_{\text{Page}}^{(\text{frag})} < S_{\text{Page}}^{(\text{full})}$, confirming fragmentation-confined thermalization
- **Monte Carlo sampling**: Estimated fragment Page values via Haar-random states within each fragment subspace ($N_{\text{MC}} = 1000$–$10000$ samples)
- **Multi-fragment comparison**: Showed that fragments of different dimensions $D$ each saturate to their own $S_{\text{Page}}^{(\mathcal{D})}$, systematically verifying that the thermalization ceiling scales with fragment dimension
- **OBC and PBC geometries**: Extended entanglement entropy analysis to both open and periodic boundary conditions with optimised Krylov-based time evolution

### 4. **Many-Body Scar-Type Eigenstates** ([`Entanglement_entropy/OBC_Optimised_entanglement_entropy_in_large_systems.ipynb`](./Entanglement_entropy/OBC_Optimised_entanglement_entropy_in_large_systems.ipynb))
- **$S_A$ vs. quasi-energy scatter**: Computed entanglement entropy for all Floquet eigenstates within the largest fragment, revealing a broad distribution rather than the narrow band predicted by ETH
- **Low-entropy outliers**: Identified eigenstates with anomalously low $S_A$ (well below $S_{\text{Page}}^{(\text{frag})}$), reminiscent of quantum many-body scars
- **Fragment-projected Floquet operator**: Constructed the $D \times D$ fragment operator via batched Krylov embedding, avoiding the full $N_s \times N_s$ dense unitary
- **Leakage diagnostics**: Quantified inter-fragment leakage ($\sim g^2/J_0^2$), confirming the prethermal validity of the fragment decomposition

### 5. **Edge Mode Characterization** ([`Autocorrelation/Autocorrelation,_HSF_and_edge_modes_in_open_Ising_chain.ipynb`](./Autocorrelation/Autocorrelation,_HSF_and_edge_modes_in_open_Ising_chain.ipynb))
- **Prethermal edge memory**: Demonstrated that edge modes persist for hundreds of Floquet cycles at resonant frequencies ($T = 2\pi/J_0$)
- **Frozen edge entropy**: Single-site edge entanglement entropy $S_{\{0\}}(nT) \approx 0$ throughout Floquet evolution, while bulk sites thermalize within the fragment
- **Magnetization profiles**: Generated contour plots showing spatial and temporal evolution of local magnetization, revealing edge-bulk contrast
- **Symmetry analysis**: Investigated inversion and chiral symmetries; established that edge modes are **not** symmetry-protected topological (SPT) but arise purely from the structure of the effective Floquet Hamiltonian
- **System size independence**: Edge memory lifetime shows no systematic dependence on chain length $L$

These results provide concrete numerical evidence for prethermal Hilbert space fragmentation, dynamical freezing, many-body scar-type states, and Floquet edge modes in non-integrable systems without disorder.

---

## Repository structure

### 📊 Computational notebooks (main results)

#### 1. **Autocorrelation, fragmentation & edge modes** ([`Autocorrelation/`](./Autocorrelation/))

```
Autocorrelation/
├── Autocorrelation,_HSF_and_edge_modes_in_open_Ising_chain.ipynb
├── Optimised_Autocorrelation_in_large_system_sizes.ipynb
├── optimised_autocorrelation_in_large_system_sizes.py
└── KnowledgeBasis.md
```

**Notebook**: [`Autocorrelation,_HSF_and_edge_modes_in_open_Ising_chain.ipynb`](./Autocorrelation/Autocorrelation,_HSF_and_edge_modes_in_open_Ising_chain.ipynb)
**Open in Colab**: [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Knerdy-got-moves/Many-body-dynamics-in-Floquet-driven-systems/blob/main/Autocorrelation/Autocorrelation%2C_HSF_and_edge_modes_in_open_Ising_chain.ipynb)

**Key features**:
- Infinite-temperature autocorrelation functions $C_j(nT) = \langle \sigma_j^z(nT) \sigma_j^z(0) \rangle_\infty$
- **Detailed fragmentation analysis**: decomposition into disconnected Krylov sectors via BFS on the connectivity graph
- **Prethermal edge memory**: long-lived magnetization at boundary sites
- Magnetization contour plots revealing edge-bulk dynamics
- Symmetry analysis (inversion, chiral) and edge mode mechanisms

**Notebook**: [`Optimised_Autocorrelation_in_large_system_sizes.ipynb`](./Autocorrelation/Optimised_Autocorrelation_in_large_system_sizes.ipynb)
**Open in Colab**: [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Knerdy-got-moves/Many-body-dynamics-in-Floquet-driven-systems/blob/main/Autocorrelation/Optimised_Autocorrelation_in_large_system_sizes.ipynb)

**Key features**:
- Optimised sparse Hamiltonian construction and Krylov-based time evolution for system sizes up to $L = 12$
- **Dynamical freezing**: bulk freezing at $h_0/J_0 = 2n$ ($n > 2$) with oscillating edges; complete freezing at $T = 2\pi/J_0$
- **Sinc filter selection rules**: systematic mapping of active/suppressed spin-flip channels for each driving period
- Up to 500 Floquet steps with ensemble-averaged autocorrelation

**Main results**:
- Non-zero autocorrelation plateaus signal ETH violation
- Edge sites retain memory for $\sim 300$ Floquet steps at $T = 2\pi/J_0$
- Edge modes emerge from effective Floquet Hamiltonian structure: $H_F^{(1)} \sim -g\sum_{i=2}^{L-1}\pi_{i-1}\sigma_i^x\pi_{i+1}$ (lacks edge flip terms)
- Dynamical freezing at large $h_0/J_0$ with complete system freeze at $T = 2\pi/J_0$
- Edge memory shows no systematic system-size dependence

**Knowledge basis**: [`KnowledgeBasis.md`](./Autocorrelation/KnowledgeBasis.md) — detailed analytical and numerical documentation

---

#### 2. **Entanglement entropy & fragmentation** ([`Entanglement_entropy/`](./Entanglement_entropy/))

```
Entanglement_entropy/
├── PBC_Optimised_entanglement_entropy_in_large_systems.ipynb
├── OBC_Optimised_entanglement_entropy_in_large_systems.ipynb
├── pbc_optimised_entanglement_entropy_in_large_systems.py
├── obc_optimised_entanglement_entropy_in_large_systems.py
└── KnowledgeBasis_EE.md
```

**Notebook**: [`PBC_Optimised_entanglement_entropy_in_large_systems.ipynb`](./Entanglement_entropy/PBC_Optimised_entanglement_entropy_in_large_systems.ipynb)
**Open in Colab**: [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Knerdy-got-moves/Many-body-dynamics-in-Floquet-driven-systems/blob/main/Entanglement_entropy/PBC_Optimised_entanglement_entropy_in_large_systems.ipynb)

**Key features**:
- Bipartite entanglement entropy dynamics in kicked Ising chains with **periodic boundary conditions (PBC)**
- Violation of ETH: saturation below thermal Page value at resonant frequencies
- Optimised Krylov-based stroboscopic evolution for large system sizes ($L = 10$–$12$)

**Notebook**: [`OBC_Optimised_entanglement_entropy_in_large_systems.ipynb`](./Entanglement_entropy/OBC_Optimised_entanglement_entropy_in_large_systems.ipynb)
**Open in Colab**: [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Knerdy-got-moves/Many-body-dynamics-in-Floquet-driven-systems/blob/main/Entanglement_entropy/OBC_Optimised_entanglement_entropy_in_large_systems.ipynb)

**Key features**:
- Entanglement entropy dynamics with **open boundary conditions (OBC)**
- **Fragment-specific Page value calculations via Monte Carlo sampling** ($N_{\text{MC}} = 1000$–$10000$)
- **Multi-fragment comparison**: different fragments saturate to their own $S_{\text{Page}}^{(\mathcal{D})}$
- **$S_A$ vs. quasi-energy scatter**: identification of many-body scar-type eigenstates within fragments
- **Fragment-projected Floquet operator** via batched Krylov embedding ($D \times D$ instead of $N_s \times N_s$)
- **Frozen edge mode confirmation**: edge-site entanglement entropy remains $\approx 0$ while bulk thermalizes
- Leakage diagnostics quantifying inter-fragment coupling ($\sim g^2/J_0^2$)

**Main results**:
- At $T = n\pi/J_0$, entanglement entropy plateaus at $S_A^{\text{sat}} = S_{\text{Page}}^{(\text{frag})} < S_{\text{Page}}^{(\text{full})}$
- Fragmentation quantitatively explains reduced thermalization; states thermalize only within their respective fragments
- Largest fragment dimension follows $D = F_{L-1} + F_{L+1}$ (Fibonacci identity)
- Floquet eigenstates show broad $S_A$ distribution with low-entropy outliers (scar-type states)
- Edge entanglement $S_{\{0\}} \approx 0$ confirms frozen boundary modes

**Knowledge basis**: [`KnowledgeBasis_EE.md`](./Entanglement_entropy/KnowledgeBasis_EE.md) — detailed analytical and numerical documentation

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
4. **Numerical results**: Entanglement entropy saturation, autocorrelation plateaus (L=8–12)
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

### 4. Dynamical freezing
At $h_0/J_0 = 2n$ ($n > 2$) and driving periods $T = m\pi/J_0$:
- **Bulk freezing**: The sinc filter suppresses essentially all bulk spin-flip channels; autocorrelation remains near unity
- **Edge oscillations**: Edge sites retain residual dynamics due to different energy mismatches ($P = 2J_0, 6J_0$)
- **Complete freezing at $T = 2\pi/J_0$**: All channels shut off; the entire system is dynamically frozen

### 5. Prethermal edge memory (OBC)
- Edge sites retain initial magnetization for $\sim 10^2$ Floquet cycles
- Edge entanglement entropy $S_{\{0\}} \approx 0$ throughout Floquet evolution (frozen site)
- Mechanism: effective Hamiltonian lacks edge flip terms ($H_F^{(1)}$ sums from $i=2$ to $L-1$)
- Not protected by conventional symmetries (non-SPT)

### 6. Many-body scar-type eigenstates
- Floquet eigenstates within individual fragments show a **broad distribution** of entanglement entropy
- Low-$S_A$ outlier states violate strong ETH even within the fragment
- Mechanism specific to the Floquet-fragmented structure

---

## Methodology

### Analytical
- **Floquet perturbation theory**: Derives effective Hamiltonian $H_F = H_F^{(1)} + \mathcal{O}(g^3)$ via cumulant expansion
- **Detuning analysis**: Spin-flip amplitudes scale as $\text{sinc}(\Delta P T/4)$ where $\Delta P$ is the energy mismatch in the classical diagonal basis

### Numerical
- **Exact diagonalization** using SciPy/NumPy sparse matrix methods and [QuSpin](https://quspin.github.io/QuSpin/)
- **System sizes**: Up to $L = 12$ (Hilbert space dimension $2^{12} = 4096$)
- **Krylov time evolution**: Matrix-free stroboscopic evolution via `scipy.sparse.linalg.expm_multiply` with adaptive subdivision
- **Diagnostics**:
  - Bipartite entanglement entropy via Schmidt decomposition (SVD)
  - Infinite-temperature autocorrelations from Haar-random initial states (up to 500 Floquet steps)
  - Fragmentation via connectivity graph analysis (BFS on $\mathcal{O}$-eigenvalue graph)
  - Fragment Page values via Monte Carlo sampling within fragment subspaces
  - $S_A$ vs. quasi-energy scatter for Floquet eigenstates (scar identification)
  - Inter-fragment leakage quantification

---

## Key results summary

| **Observable** | **Diagnostic** | **ETH prediction** | **Our result** | **Interpretation** |
|----------------|----------------|--------------------|-----------------|--------------------|
| Entanglement $S_A(t)$ | Time evolution | $S_A^{\text{sat}} = S_{\text{Page}}^{\text{system}}$ | $S_A^{\text{sat}} = S_{\text{Page}}^{\text{fragment}} < S_{\text{Page}}^{\text{system}}$ | Fragment-limited thermalization |
| Autocorrelation $C_j(t)$ | Infinite-T ensemble | $C_j(t) = 0$ rapidly | Non-zero plateaus; $\tau^* \sim e^{J_0/g}$ | Memory of initial conditions |
| Edge magnetization | Open boundaries | Decays rapidly | Persists $\sim 300$ steps; $S_{\{0\}} \approx 0$ | Prethermal edge modes |
| Hilbert space | Connectivity | Fully connected | Exponentially many fragments; $D_{\max} = F_{L-1} + F_{L+1}$ | HSF |
| Floquet eigenstates | $S_A(\varepsilon)$ scatter | Narrow band near $S_{\text{Page}}$ | Broad distribution with low-$S_A$ outliers | Many-body scar-type states |
| Bulk dynamics | $h_0/J_0 = 2n$, $n > 2$ | Thermalizes | Dynamically frozen; edges oscillate | Sinc-filter suppression |

---

## Dependencies

All computational notebooks require:

```bash
pip install quspin numpy scipy matplotlib numba
```

- **QuSpin**: Hamiltonian construction in spin bases, sparse matrix exponentiation, Floquet operator eigendecomposition
- **SciPy**: Sparse matrix construction (`scipy.sparse`), Krylov time evolution (`scipy.sparse.linalg.expm_multiply`), graph analysis
- **NumPy**: Linear algebra, random state generation, SVD-based entropy computation
- **Matplotlib**: Visualization (time series, contour plots, scatter plots)
- **Numba**: JIT compilation for performance-critical loops

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

**Keywords**: Floquet systems, Hilbert space fragmentation, eigenstate thermalization hypothesis, entanglement entropy, autocorrelation, Ising model, prethermal dynamics, non-ergodic behavior, quantum many-body physics, dynamical freezing, many-body scars, edge modes, QuSpin
