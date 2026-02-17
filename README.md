# Many-Body dynamics in Floquet-driven systems

**MSc thesis repository (9th semester progress)**

**Student**: Rishi Paresh Joshi (5th Year Integrated M.Sc., NISER Bhubaneswar)

**Supervisor**: Dr. Tapan Mishra, Associate professor, NISER

---

## Overview

This repository contains the work on my MSc thesis (part 1) on investigating **Hilbert space fragmentation (HSF)** and **eigenstate thermalization hypothesis (ETH) violation** in periodically driven (Floquet) quantum Ising chains. The research combines analytical Floquet perturbation theory with numerical probes like entanglement entropy and autocorrelation evolution to demonstrate prethermal non-ergodic dynamics in non-integrable many-body systems without disorder.

### Notation and conventions

Throughout this document, the following notation is used:

- $L$: number of spin-1/2 sites in the chain.
- $\sigma_i^{x,y,z}$: Pauli spin operators acting on site $i$ ($i = 1, 2, \ldots, L$).
- $J_0, h_0, g$: Ising coupling strength, longitudinal field amplitude, and transverse field strength, respectively.
- $T$: driving period of the symmetric square-wave protocol.
- $N_s = 2^L$: total Hilbert space dimension.
- $\hbar = 1$ throughout (natural units).
- $H_F$: Floquet Hamiltonian, defined via $U_F = e^{-iH_F T}$ where $U_F$ is the one-period Floquet unitary.
- $H_F^{(1)}$: first-order effective Floquet Hamiltonian (leading order in $g/J_0$).
- $\mathcal{O}$: fragmentation operator (diagonal in the computational basis), defined as $\mathcal{O} = J_0\sum_{i=1}^{L-1}\sigma_i^z\sigma_{i+1}^z + h_0\sum_{i=1}^{L}\sigma_i^z$.
- $D$: dimension of a given Hilbert space fragment.
- $S_A$: von Neumann entanglement entropy of subsystem $A$ for a bipartition of the chain into $A$ and its complement $B$, defined as $S_A = -\mathrm{Tr}[\rho_A \ln \rho_A]$ where $\rho_A = \mathrm{Tr}_B |\psi\rangle\langle\psi|$.
- $S_{\text{Page}}^{(\text{full})}$: Page value (average entanglement entropy of a Haar-random state in the full Hilbert space of dimension $N_s$).
- $S_{\text{Page}}^{(\text{frag})}$: fragment Page value (average entanglement entropy of a Haar-random state restricted to a fragment of dimension $D$).
- $C_j(nT)$: infinite-temperature autocorrelation function at site $j$ after $n$ Floquet periods, defined as $C_j(nT) = \langle\psi_0|\sigma_j^z(nT)\,\sigma_j^z(0)|\psi_0\rangle$ averaged over Haar-random initial states $|\psi_0\rangle$.
- $F_n$: the $n$-th Fibonacci number, with $F_0 = 0$, $F_1 = 1$, and $F_n = F_{n-1} + F_{n-2}$.
- $N_{\text{defect}}$: total number of adjacent up-up ($\uparrow\uparrow$) bonds in a spin configuration, $N_{\text{defect}} = \sum_{i} p_i$ where $p_i = 1$ if bond $(i, i+1)$ is $\uparrow\uparrow$ and $0$ otherwise.
- $\pi_i = (\mathbb{I} - \sigma_i^z)/2$: projector onto the spin-down state $|\downarrow\rangle$ at site $i$.
- $\varepsilon_\alpha$: quasi-energy of the $\alpha$-th Floquet eigenstate, defined via $U_F|\phi_\alpha\rangle = e^{-i\varepsilon_\alpha T}|\phi_\alpha\rangle$.
- $\text{sinc}(x) \equiv \sin(x)/x$ (unnormalised sinc function; $\text{sinc}(0) = 1$).
- $\tau^*$: prethermal timescale beyond which inter-fragment leakage becomes significant.

---

## Recent accomplishments

The computational work in this repository extends significantly beyond the formal thesis document. The latest developments include optimised large-system-size simulations (up to $L = 12$, corresponding to $N_s = 2^{12} = 4096$ states) and new analyses of entanglement entropy in both OBC and PBC geometries.

### 1. **Hilbert Space Fragmentation Analysis** ([`Autocorrelation/`](./Autocorrelation/))
- **Fragment decomposition**: Performed detailed fragmentation analysis for system sizes up to $L = 12$ ($N_s = 4096$), identifying the structure and connectivity of disconnected Krylov subspaces under the first-order Floquet Hamiltonian $H_F^{(1)}$.
- **Fibonacci dimension**: Verified that the largest fragment (containing all configurations with no adjacent $\uparrow\uparrow$ bonds, i.e., $N_{\text{defect}} = 0$) has dimension $D = F_{L-1} + F_{L+1}$ (e.g., $D = F_{11} + F_{13} = 89 + 233 = 322$ at $L = 12$).
- **Up-up bond analysis**: Characterized fragments by $N_{\text{defect}}$, which is conserved within each fragment at leading order in $g/J_0$.
- **Connectivity graphs**: Constructed and visualized the fragmentation of the Hilbert space under $H_F^{(1)}$ via breadth-first search (BFS) on the connectivity graph of $\mathcal{O}$-eigenvalue sectors.

### 2. **Dynamical Freezing** ([`Autocorrelation/Optimised_Autocorrelation_in_large_system_sizes.ipynb`](./Autocorrelation/Optimised_Autocorrelation_in_large_system_sizes.ipynb))
- **Bulk freezing with oscillating edges**: For $h_0/J_0 = 2n$ ($n \geq 2$, $n \in \mathbb{Z}$) at driving periods $T = (2m+1)\pi/J_0$ ($m \in \mathbb{Z}_{\geq 0}$), the bulk of the chain is dynamically frozen while edge sites exhibit oscillatory behavior.
- **Complete freezing**: At $T = 2m\pi/J_0$ ($m \in \mathbb{N}$), the entire system—both bulk and edges—is frozen; the autocorrelation $C_j(nT)$ saturates at a finite value across all sites $j$.
- **Sinc filter mechanism**: Systematically mapped which spin-flip channels are active/suppressed for each driving period via the sinc selection rules: a matrix element connecting states with energy mismatch $P_{nm}$ (the difference of $\mathcal{O}$-eigenvalues $P_n - P_m$) is proportional to $\text{sinc}(P_{nm}T/4)$.

### 3. **Fragment-Resolved Entanglement Entropy** ([`Entanglement_entropy/`](./Entanglement_entropy/))
- **Fragment Page value saturation**: Demonstrated that the entanglement entropy $S_A(nT)$ saturates at the **fragment-specific Page value** $S_{\text{Page}}^{(\text{frag})} < S_{\text{Page}}^{(\text{full})}$, confirming fragmentation-confined thermalization.
- **Monte Carlo sampling**: Estimated fragment Page values via Haar-random states within each fragment subspace ($N_{\text{MC}} = 1000$ to $10000$ samples).
- **Multi-fragment comparison**: Showed that fragments of different dimensions $D$ each saturate to their own $S_{\text{Page}}^{(\mathcal{D})}$ (the Page value for a fragment of dimension $D$), systematically verifying that the thermalization ceiling scales with fragment dimension.
- **OBC and PBC geometries**: Extended entanglement entropy analysis to both open and periodic boundary conditions with optimised Krylov-based time evolution.

### 4. **Many-Body Scar-Type Eigenstates** ([`Entanglement_entropy/OBC_Optimised_entanglement_entropy_in_large_systems.ipynb`](./Entanglement_entropy/OBC_Optimised_entanglement_entropy_in_large_systems.ipynb))
- **$S_A$ vs. quasi-energy scatter**: Computed entanglement entropy $S_A$ for all Floquet eigenstates within the largest fragment, revealing a broad distribution of $S_A(\varepsilon_\alpha)$ rather than the narrow band predicted by ETH.
- **Low-entropy outliers**: Identified eigenstates with anomalously low $S_A$ (well below $S_{\text{Page}}^{(\text{frag})}$), reminiscent of quantum many-body scars.
- **Fragment-projected Floquet operator**: Constructed a smaller size Floquet operator via batched Krylov embedding, avoiding the full $N_s \times N_s$ dense unitary. Cost (in time for construction): $\mathcal{O}((K_+ + K_-) \cdot \text{nnz}(H_\pm) \cdot N_s)$ where $K_\pm = \[ |A_\pm|1/5]$ are the Krylov subdivision counts for each half-period generator $A_\pm = -iH_\pm T/2$, and $\text{nnz}(H_\pm)$ denotes the number of nonzero entries in the sparse Hamiltonian matrices $H_\pm$.
- **Leakage diagnostics**: Quantified inter-fragment leakage ($\sim g^3/J_0^2$), confirming the prethermal validity of the fragment decomposition.

### 5. **Edge Mode Characterization** ([`Autocorrelation/`](./Autocorrelation/), [`Entanglement_entropy/`](./Entanglement_entropy/))
- **Prethermal edge memory**: Demonstrated that edge modes persist for hundreds of Floquet cycles at resonant frequencies ($T = 2\pi/J_0$), with the prethermal timescale $\tau^* \sim e^{J_0/g}$.
- **Frozen edge entropy**: Single-site edge entanglement entropy $S_{\{0\}}(nT) \approx 0$ throughout Floquet evolution (where $\{0\}$ denotes subsystem $A$ consisting of the first site only), while bulk sites thermalize within the fragment.
- **Magnetization profiles**: Generated contour plots showing spatial and temporal evolution of local magnetization $\langle\psi(nT)|\sigma_j^z|\psi(nT)\rangle$, revealing edge-bulk contrast.
- **Symmetry analysis**: Investigated inversion and chiral symmetries; established that edge modes are **not** symmetry-protected topological (SPT) but arise purely from the structure of the effective Floquet Hamiltonian.
- **System size independence**: Edge memory lifetime shows no systematic dependence on chain length $L$.

These results provide concrete numerical evidence for prethermal Hilbert space fragmentation, dynamical freezing, many-body scar-type states, and Floquet edge modes in non-integrable systems without disorder.

---

## Repository structure

### 📊 Computational notebooks (main results)

#### 1. **Autocorrelation, fragmentation & edge modes** ([`Autocorrelation/`](./Autocorrelation/))

```
Autocorrelation/
├── Optimised_Autocorrelation_in_large_system_sizes.ipynb
├── optimised_autocorrelation_in_large_system_sizes.py
└── KnowledgeBasis.md
```

**Notebook**: [`Optimised_Autocorrelation_in_large_system_sizes.ipynb`](./Autocorrelation/Optimised_Autocorrelation_in_large_system_sizes.ipynb)
**Open in Colab**: [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Knerdy-got-moves/Many-body-dynamics-in-Floquet-driven-systems/blob/main/Autocorrelation/Optimised_Autocorrelation_in_large_system_sizes.ipynb)

**Key features**:
- Optimised sparse Hamiltonian construction and Krylov-based time evolution for system sizes up to $L = 12$
- **Dynamical freezing**: bulk freezing at $h_0/J_0 = 2n$ ($n \geq 2$, $n \in \mathbb{Z}$) with oscillating edges; complete freezing at $T = 2\pi/J_0$
- **Sinc filter selection rules**: systematic mapping of active/suppressed spin-flip channels for each driving period
- Up to 500 Floquet steps with ensemble-averaged autocorrelation $C_j(nT)$

**Main results**:
- Non-zero autocorrelation plateaus signal ETH violation
- Edge sites retain memory for $\sim 300$ Floquet steps at $T = 2\pi/J_0$
- Edge modes emerge from the structure of the effective Floquet Hamiltonian: $H_F^{(1)} = -g\sum_{i=2}^{L-1}\pi_{i-1}\sigma_i^x\pi_{i+1}$, where $\pi_i = (\mathbb{I} - \sigma_i^z)/2$ is the spin-down projector at site $i$. This Hamiltonian lacks edge flip terms (the sum runs from $i=2$ to $L-1$, excluding the boundary sites).
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
- Bipartite entanglement entropy $S_A(nT)$ dynamics in kicked Ising chains with **periodic boundary conditions (PBC)**
- Violation of ETH: saturation below thermal Page value $S_{\text{Page}}^{(\text{full})}$ at resonant frequencies
- Optimised Krylov-based stroboscopic evolution for large system sizes ($L = 10$–$12$)

**Notebook**: [`OBC_Optimised_entanglement_entropy_in_large_systems.ipynb`](./Entanglement_entropy/OBC_Optimised_entanglement_entropy_in_large_systems.ipynb)
**Open in Colab**: [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Knerdy-got-moves/Many-body-dynamics-in-Floquet-driven-systems/blob/main/Entanglement_entropy/OBC_Optimised_entanglement_entropy_in_large_systems.ipynb)

**Key features**:
- Entanglement entropy dynamics with **open boundary conditions (OBC)**
- **Fragment-specific Page value calculations via Monte Carlo sampling** ($N_{\text{MC}} = 1000$–$10000$ samples)
- **Multi-fragment comparison**: different fragments saturate to their own $S_{\text{Page}}^{(\mathcal{D})}$
- **$S_A$ vs. quasi-energy $\varepsilon_\alpha$ scatter**: identification of many-body scar-type eigenstates within fragments
- **Fragment-projected Floquet operator** via batched Krylov embedding ($D \times D$ instead of $N_s \times N_s$)
- **Frozen edge mode confirmation**: edge-site entanglement entropy $S_{\{0\}}(nT)$ remains $\approx 0$ while bulk thermalizes
- Leakage diagnostics quantifying inter-fragment coupling ($\sim g^2/J_0^2$)

**Main results**:
- At resonant driving periods $T = n\pi/J_0$ ($n \in \mathbb{Z}$), entanglement entropy plateaus at $S_A^{\text{sat}} = S_{\text{Page}}^{(\text{frag})} < S_{\text{Page}}^{(\text{full})}$
- Fragmentation quantitatively explains reduced thermalization; states thermalize only within their respective fragments
- Largest fragment dimension follows $D = F_{L-1} + F_{L+1}$ (Lucas number identity for Fibonacci numbers $F_n$ with $F_0=0, F_1=1$)
- Floquet eigenstates show broad $S_A(\varepsilon_\alpha)$ distribution with low-entropy outliers (scar-type states)
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
2. **Model**: Kicked Ising chain Hamiltonian $H(t) = -J(t)\sum_i \sigma_i^z\sigma_{i+1}^z - h(t)\sum_i \sigma_i^z - g\sum_i \sigma_i^x$, where $J(t)$ and $h(t)$ are the square-wave-driven Ising coupling and longitudinal field, and $g$ is the static transverse field strength
3. **Analytical results**: First-order Floquet Hamiltonian $H_F^{(1)}$, spin-flip suppression via sinc filters
4. **Numerical results**: Entanglement entropy saturation, autocorrelation plateaus ($L=8$–$12$)
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
- Numerical diagnostics: entanglement entropy $S_A$ and autocorrelations $C_j$
- Key results and prethermal fragmentation signatures

**Quick navigation**: See [`9th_sem_presentation/9th_sem_presentation_README.md`](./9th_sem_presentation/9th_sem_presentation_README.md) for slide breakdown.

---

## Physical system

The system under investigation is a **periodically driven spin-1/2 Ising chain** of $L$ sites with symmetric square-wave driving of period $T$. Here $\sigma_i^{x,y,z}$ denote the Pauli spin operators on site $i$, and we work in natural units ($\hbar = 1$). The full Hamiltonian is:

$$H(t) = H_0(t) + H_1$$

where:

$$H_0(t) = -J(t)\sum_{i}\sigma_i^z\sigma_{i+1}^z - h(t)\sum_{i}\sigma_i^z$$

$$H_1 = -g\sum_{i}\sigma_i^x$$

The couplings follow a symmetric square-wave drive:

$$J(t) = \begin{cases} +J_0, & 0 \le t < T/2 \\ -J_0, & T/2 \le t < T \end{cases}, \quad h(t) = \begin{cases} +h_0, & 0 \le t < T/2 \\ -h_0, & T/2 \le t < T \end{cases}$$

with $|g| \ll \{|J_0|, |h_0|\}$ (weak transverse perturbation) and $h_0 = 2J_0$ (typical parameter choice). Here $J_0$ is the Ising coupling strength, $h_0$ the longitudinal field amplitude, and $g$ the static transverse field strength.

### Boundary conditions
- **Periodic (PBC)**: $\sigma_{L+1}^z \equiv \sigma_1^z$ — used for entanglement entropy studies
- **Open (OBC)**: No wraparound — used for edge mode and autocorrelation studies

---

## Key physical phenomena

### 1. Prethermal Hilbert space fragmentation
At resonant driving periods $T = n\pi/J_0$ ($n \in \mathbb{Z}$), the effective first-order Floquet Hamiltonian becomes:

$$H_F^{(1)} \approx -g\sum_{i=2}^{L-1}\pi_{i-1}\sigma_i^x\pi_{i+1}$$

where $\pi_i = (\mathbb{I} - \sigma_i^z)/2$ is the projector onto $|\downarrow\rangle$ at site $i$. This Hamiltonian:
- Suppresses spin flips unless neighbors satisfy specific classical configurations
- Fragments the Hilbert space into exponentially many disconnected sectors (Krylov subspaces)
- Leads to sub-thermal dynamics on prethermal timescales $\tau^* \sim e^{J_0/g}$

### 2. ETH violation without disorder
Unlike many-body localization, the system exhibits:
- Non-ergodic dynamics in a **non-integrable, translation-invariant** system
- No quenched disorder or conservation laws required
- Mechanism: constrained dynamics from Floquet engineering

### 3. Fragment-limited thermalization
- States thermalize only within their respective fragments of dimension $D$
- Observables converge to fragment microcanonical averages
- Entanglement entropy: $S_A^{\text{sat}} = S_{\text{Page}}^{\text{fragment}} < S_{\text{Page}}^{\text{system}}$

### 4. Dynamical freezing
At $h_0/J_0 = 2n$ ($n \geq 2$, $n \in \mathbb{Z}$) and driving periods $T = m\pi/J_0$ ($m \in \mathbb{N}$):
- **Bulk freezing**: The sinc filter $\text{sinc}(P_{nm}T/4)$ (where $P_{nm} = P_n - P_m$ is the $\mathcal{O}$-eigenvalue mismatch between states $|n\rangle$ and $|m\rangle$) suppresses essentially all bulk spin-flip channels; autocorrelation $C_j(nT)$ remains near unity
- **Edge oscillations**: Edge sites retain residual dynamics due to different energy mismatches ($P = 2J_0, 6J_0$)
- **Complete freezing at $T = 2\pi/J_0$**: All channels shut off; the entire system is dynamically frozen

### 5. Prethermal edge memory (OBC)
- Edge sites retain initial magnetization for $\sim 10^2$ Floquet cycles
- Edge entanglement entropy $S_{\{0\}} \approx 0$ throughout Floquet evolution (frozen site)
- Mechanism: $H_F^{(1)}$ sums from $i=2$ to $L-1$, so edge flip terms are absent
- Not protected by conventional symmetries (non-SPT)

### 6. Many-body scar-type eigenstates
- Floquet eigenstates $|\phi_\alpha\rangle$ within individual fragments show a **broad distribution** of entanglement entropy $S_A(\varepsilon_\alpha)$
- Low-$S_A$ outlier states violate strong ETH even within the fragment
- Mechanism specific to the Floquet-fragmented structure

---

## Methodology

### Analytical
- **Floquet perturbation theory**: Derives effective Hamiltonian $H_F = H_F^{(1)} + \mathcal{O}(g^3)$ via cumulant expansion
- **Detuning analysis**: Spin-flip amplitudes scale as $\text{sinc}(\Delta P \cdot T/4)$ where $\Delta P = P_n - P_m$ is the energy mismatch in the diagonal basis of $\mathcal{O}$

### Numerical
- **Exact diagonalization** using SciPy/NumPy sparse matrix methods
- **System sizes**: Up to $L = 12$ (Hilbert space dimension $N_s = 2^{12} = 4096$)
- **Krylov time evolution**: Matrix-free stroboscopic evolution via `scipy.sparse.linalg.expm_multiply` with adaptive subdivision (each half-period generator $A_\pm = -iH_\pm T/2$ is subdivided into $K_\pm = \lceil \|A_\pm\|_1 / 5 \rceil$ substeps, where $\|A_\pm\|_1$ denotes the matrix 1-norm)
- **Diagnostics**:
  - Bipartite entanglement entropy $S_A$ via Schmidt decomposition (SVD)
  - Infinite-temperature autocorrelations $C_j(nT)$ from Haar-random initial states (up to 500 Floquet steps)
  - Fragmentation via connectivity graph analysis (BFS on $\mathcal{O}$-eigenvalue graph)
  - Fragment Page values $S_{\text{Page}}^{(\text{frag})}$ via Monte Carlo sampling ($N_{\text{MC}}$ Haar-random states within fragment subspaces)
  - $S_A$ vs. quasi-energy $\varepsilon_\alpha$ scatter for Floquet eigenstates (scar identification)
  - Inter-fragment leakage quantification

---

## Key results summary

| **Observable** | **Diagnostic** | **ETH prediction** | **Our result** | **Interpretation** |
|----------------|----------------|--------------------|-----------------|--------------------|
| Entanglement $S_A(nT)$ | Time evolution | $S_A^{\text{sat}} = S_{\text{Page}}^{\text{system}}$ | $S_A^{\text{sat}} = S_{\text{Page}}^{\text{fragment}} < S_{\text{Page}}^{\text{system}}$ | Fragment-limited thermalization |
| Autocorrelation $C_j(nT)$ | Infinite-$T$ ensemble | $C_j(nT) \to 0$ rapidly | Non-zero plateaus; prethermal lifetime $\tau^* \sim e^{J_0/g}$ | Memory of initial conditions |
| Edge magnetization | Open boundaries | Decays rapidly | Persists $\sim 200$ steps; $S_{\{0\}} \approx 0$ | Prethermal edge modes |
| Hilbert space | Connectivity | Fully connected | Exponentially many fragments; $D_{\max} = F_{L-1} + F_{L+1}$ | HSF |
| Floquet eigenstates | $S_A(\varepsilon_\alpha)$ scatter | Narrow band near $S_{\text{Page}}$ | Broad distribution with low-$S_A$ outliers | Many-body scar-type states |
| Bulk dynamics | $h_0/J_0 = 2n$, $n \geq 2$ | Thermalizes | Dynamically frozen; edges oscillate | Sinc-filter suppression |

---

## Dependencies

All computational notebooks require:

```bash
pip install numpy scipy matplotlib numba
```

- **SciPy**: Sparse matrix construction (`scipy.sparse`), Krylov time evolution (`scipy.sparse.linalg.expm_multiply`), graph analysis
- **NumPy**: Linear algebra, random state generation, SVD-based entropy computation
- **Matplotlib**: Visualization (time series, contour plots, scatter plots)
- **Numba**: JIT compilation for performance-critical loops

---

## Running the notebooks

### Option 1: Google Colab (recommended)
Click the "Open in Colab" badges in the notebook sections above.

### Option 2: Local Jupyter

```bash
git clone https://github.com/Knerdy-got-moves/Many-body-dynamics-in-Floquet-driven-systems.git
cd Many-body-dynamics-in-Floquet-driven-systems
pip install numpy scipy matplotlib numba
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

**Keywords**: Floquet systems, Hilbert space fragmentation, eigenstate thermalization hypothesis, entanglement entropy, autocorrelation, Ising model, prethermal dynamics, non-ergodic behavior, quantum many-body physics, dynamical freezing, many-body scars, edge modes
