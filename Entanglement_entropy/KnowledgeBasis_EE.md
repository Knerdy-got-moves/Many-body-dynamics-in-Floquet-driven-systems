# Knowledge Basis: Entanglement Entropy in Floquet-Driven Fragmented Hilbert Spaces (OBC)

## 1. Physical Setup

### 1.1 Model Hamiltonian

We study a **kicked transverse-field Ising chain** of $L$ sites with **open boundary conditions** (OBC). The time-dependent Hamiltonian is

$$
H(t) = H_0(t) + H_1,
$$

where the unperturbed part follows a symmetric square-wave drive of period $T$:

$$
H_0(t) = -J(t)\sum_{i=0}^{L-2}\sigma_i^z\sigma_{i+1}^z - h(t)\sum_{i=0}^{L-1}\sigma_i^z,
$$

with couplings

$$
J(t) = \begin{cases} +J_0, & 0 \le t \le T/2, \\ -J_0, & T/2 < t \le T, \end{cases}
\qquad
h(t) = \begin{cases} +h_0, & 0 \le t \le T/2, \\ -h_0, & T/2 < t \le T, \end{cases}
$$

and a weak transverse perturbation $H_1 = -g\sum_{i=0}^{L-1}\sigma_i^x$ with $|g| \ll \{|J_0|, |h_0|\}$.

**Key distinction (OBC vs PBC):** The ZZ interaction sums over $L-1$ bonds $(i, i+1)$ for $i = 0, \ldots, L-2$. There is **no** wrap-around bond connecting site $L-1$ back to site $0$. This is the structural origin of the frozen edge modes discussed in §5.

### 1.2 Floquet Unitary and Stroboscopic Dynamics

The Floquet unitary for one period is

$$
U_F = e^{-iH_-T/(2\hbar)}\, e^{-iH_+T/(2\hbar)},
$$

where $H_\pm$ denote $H(t)$ evaluated at $(J, h) = (\pm J_0, \pm h_0)$ on the corresponding half-period. Stroboscopic evolution after $n$ periods gives $|\psi(nT)\rangle = U_F^n\,|\psi(0)\rangle$.

**Fragmentation driving condition:** ETH violation (and Hilbert space fragmentation at leading order in Floquet perturbation theory) occurs when the period satisfies

$$
T = \frac{n\pi}{J_0}, \qquad n \in \mathbb{Z}.
$$

At these special periods, the zeroth-order propagator $e^{-iH_0 T/2}$ acquires a discrete symmetry that freezes certain matrix elements of the perturbation $H_1$, producing the block-diagonal (fragmented) structure of the first-order Floquet Hamiltonian $H_F^{(1)}$.

### 1.3 Bipartite Entanglement Entropy

For a bipartition of the chain into subsystems $A$ (of size $L_A$) and $B$ (of size $L_B = L - L_A$), the von Neumann entanglement entropy at stroboscopic time $nT$ is

$$
S_A(nT) = -\mathrm{Tr}\!\big[\rho_A(nT)\ln\rho_A(nT)\big] = -\sum_k p_k(nT)\ln p_k(nT),
$$

where $\rho_A = \mathrm{Tr}_B|\psi\rangle\langle\psi|$ and $p_k = s_k^2$ are the squared Schmidt coefficients from the decomposition $|\psi\rangle = \sum_k s_k |k_A\rangle \otimes |k_B\rangle$.

**Numerically**, $S_A$ is computed via the SVD of the reshaped state vector $\psi \to M_{ab}$ with $M \in \mathbb{C}^{d_A \times d_B}$. When $A$ consists of contiguous leading sites $\{0, 1, \ldots, L_A - 1\}$, no tensor transpose is needed (the fast path). For non-contiguous $A$, the code transposes the $L$-index tensor before reshaping.


## 2. Prethermal Hilbert Space Fragmentation

### 2.1 First-Order Floquet Hamiltonian

At the fragmentation driving periods $T = n\pi/J_0$, Floquet perturbation theory (Ref: Sen, Sen & Sengupta, J. Phys.: Condens. Matter **33**, 443003, 2021) yields a first-order effective Hamiltonian with matrix elements

$$
\big(H_F^{(1)}\big)_{nm} = -\langle n| g\sum_{i=0}^{L-1}\sigma_i^x |m\rangle\;\delta_{P_{nm},0},
$$

where $P_{nm} = \mathcal{O}_n - \mathcal{O}_m$ is the difference in eigenvalues of the **fragmentation operator**

$$
\mathcal{O} = J_0\sum_{i=0}^{L-2}\sigma_i^z\sigma_{i+1}^z + 2J_0\sum_{i=0}^{L-1}\sigma_i^z.
$$

The Kronecker delta $\delta_{P_{nm}, 0}$ enforces that only states with identical $\mathcal{O}$-eigenvalue are coupled by the transverse field. This is the **resonance condition** that fragments the Hilbert space.

### 2.2 Fragment Identification via Connectivity Graph

The code identifies fragments by:

1. **Computing $\mathcal{O}_n$** for all $2^L$ computational basis states (diagonal operator; complexity $\mathcal{O}(L \cdot 2^L)$, memory $\mathcal{O}(2^L)$).

2. **Building a connectivity graph:** An edge $(n, m)$ exists if (a) states $n$ and $m$ differ by exactly one spin flip ($\sigma_i^x$ matrix element), and (b) $|\mathcal{O}_n - \mathcal{O}_m| < \epsilon$ with tolerance $\epsilon = 10^{-10}$. Complexity: $\mathcal{O}(L \cdot 2^L)$.

3. **BFS on the connectivity graph** to extract connected components (fragments). Each fragment is a set of computational basis states within which $H_F^{(1)}$ has nonzero off-diagonal matrix elements.

### 2.3 Largest Fragment and the Fibonacci Dimension

For OBC, the largest fragment consists of all spin configurations with **no adjacent up-up ($\uparrow\uparrow$) bonds** and corresponds to the $N_{\text{defect}} = 0$ sector. Its dimension is

$$
D_{\text{largest}} = F_{L-1} + F_{L+1},
$$

where $F_n$ is the $n$-th Fibonacci number ($F_0 = 0, F_1 = 1$). The code verifies this identity as a consistency check. For example, at $L = 12$: $D = F_{11} + F_{13} = 89 + 233 = 322$.

### 2.4 Up-Up Bond Analysis

Each state is characterised by a binary bond pattern $p_i \in \{0, 1\}$ where $p_i = 1$ iff bond $(i, i+1)$ is $\uparrow\uparrow$. The total number of up-up bonds $N_{\text{defect}} = \sum_i p_i$ is conserved within each fragment at leading order. Fragments with fixed bond patterns (singletons or small clusters) are dynamically frozen.


## 3. Entanglement Entropy Saturates to Fragment-Specific Page Value

### 3.1 The Page Value

For a random pure state drawn from the Haar measure on a Hilbert space of dimension $N = mn$ ($m \le n$) bipartitioned into subsystems of dimensions $m$ and $n$, the average (Page) entanglement entropy is

$$
S_{\text{Page}} \simeq \ln m - \frac{m}{2n}.
$$

For a half-chain cut at $L_A = \lfloor L/2 \rfloor$:

$$
S_{\text{Page}}^{(\text{full})} = L_A \ln 2 - 2^{2L_A - L - 1}.
$$

### 3.2 Fragment Page Value

When dynamics is restricted to a fragment $\mathcal{D}$ of dimension $D$, the relevant thermalisation target is the **fragment Page value**

$$
S_{\text{Page}}^{(\mathcal{D})} := \mathbb{E}_{\psi \in \mathcal{D}}[S_A(\psi)],
$$

where the expectation is over Haar-random states **within the fragment subspace** $\mathcal{D} \subset \mathcal{H}$.

Since the fragment is embedded non-trivially in the full $2^L$-dimensional Hilbert space (its basis states are specific computational basis vectors, not contiguous blocks of the tensor product), there is no simple closed-form expression. The code estimates $S_{\text{Page}}^{(\mathcal{D})}$ via **Monte Carlo sampling**: draw $N_{\text{MC}}$ Haar-random states supported on the $D$ fragment basis vectors, compute $S_A$ for each, and average.

A Haar-random fragment state is constructed as

$$
|\psi\rangle = \sum_{\alpha=1}^{D} c_\alpha\,|\phi_\alpha\rangle, \qquad c_\alpha = \frac{z_\alpha}{\|z\|}, \qquad z_\alpha \sim \mathcal{N}_{\mathbb{C}}(0,1),
$$

where $\{|\phi_\alpha\rangle\}$ are the computational basis states belonging to the fragment.

### 3.3 Key Result: $S_A \to S_{\text{Page}}^{(\text{frag})}$, Not $S_{\text{Page}}^{(\text{full})}$

The central numerical result is:

> Starting from random computational basis states within the largest fragment, the ensemble-averaged half-chain entanglement entropy $\langle S_A(n)\rangle$ saturates at late Floquet times to $S_{\text{Page}}^{(\text{frag})} < S_{\text{Page}}^{(\text{full})}$.

This confirms:

1. **ETH violation**: The system does not thermalise to the Page value of the full Hilbert space.
2. **Fragmentation-confined thermalisation**: The system thermalises *within* the fragment, reaching the fragment-restricted Page value.
3. **Prethermal nature**: This fragmentation is controlled by the leading-order Floquet Hamiltonian; at nonzero $g$, there is finite (but small) leakage between fragments, allowing eventual thermalisation on exponentially long timescales $\tau^* \sim e^{J_0/g}$.

The code also runs multi-fragment comparisons, showing that fragments of different dimensions $D$ each saturate to their own $S_{\text{Page}}^{(\mathcal{D})}$, providing a comprehensive check that the thermalisation ceiling scales with the fragment dimension, not the full Hilbert space dimension.


## 4. Entanglement Entropy vs. Quasi-Energies: Many-Body Scars / ETH Violation

### 4.1 Floquet Eigenstates in the Fragment Subspace

To probe ETH at the eigenstate level, the code constructs the fragment-projected Floquet operator:

1. **Embed** the $D$ fragment basis vectors into the full $N_s$-dimensional space as columns of an $N_s \times D$ matrix $I_{\text{frag}}$.
2. **Apply one batched Krylov Floquet step**: $U_F \cdot I_{\text{frag}} \to N_s \times D$ matrix (no dense $N_s \times N_s$ unitary is ever formed).
3. **Project back** by extracting the rows at fragment indices: $U_{\text{frag}}[a,b] = \langle\text{frag}[a]|U_F|\text{frag}[b]\rangle$, yielding a $D \times D$ matrix.

Cost: $\mathcal{O}((K_+ + K_-) \cdot \text{nnz} \cdot D)$ instead of $\mathcal{O}(N_s^3)$.

The $D \times D$ fragment operator is diagonalised via `np.linalg.eig` (it is unitary, not Hermitian). Eigenvalues lie on the unit circle: $U_{\text{frag}}|\phi_\alpha\rangle = e^{-i\varepsilon_\alpha T}|\phi_\alpha\rangle$, giving **quasi-energies** $\varepsilon_\alpha = -\arg(\lambda_\alpha)/T$.

### 4.2 $S_A(\varepsilon)$ Scatter Plot

For each Floquet eigenstate $|\phi_\alpha\rangle$, the code lifts it back to the full Hilbert space (embedding on the fragment basis) and computes $S_A$ via half-chain SVD.

**ETH prediction:** In a thermalising system, $S_A(\varepsilon)$ should form a smooth, featureless band near $S_{\text{Page}}$ across the quasi-energy spectrum.

**Observed behaviour:** The scatter plot of $S_A$ vs. $\varepsilon_\alpha$ reveals:

- A **broad distribution** of entanglement entropies, not a narrow band.
- Eigenstates with anomalously **low** $S_A$ (well below $S_{\text{Page}}^{(\text{frag})}$) — these are **many-body scar-type states** that violate the strong ETH within the fragment.
- The mean $\langle S_A\rangle$ over all eigenstates sits near (but can differ from) $S_{\text{Page}}^{(\text{frag})}$, while individual eigenstates span a wide range.

This establishes that even within a single Hilbert space fragment, the Floquet eigenstates are not uniformly thermalised. Certain eigenstates evade thermalisation in a manner reminiscent of quantum many-body scars (Ref: Turner et al., Nat. Phys. **14**, 745, 2018), though here the mechanism is specific to the Floquet-fragmented structure.

### 4.3 Leakage Diagnostic

The unitarity of $U_{\text{frag}}$ is checked via $\|U_{\text{frag}}^\dagger U_{\text{frag}} - I_D\|$, and the leakage (fraction of weight escaping the fragment under one Floquet step) is reported per basis vector. Small leakage ($\sim g^2 / J_0^2$) confirms the prethermal validity of the fragment decomposition.


## 5. Edge Modes: Frozen Boundary Entanglement

### 5.1 Frozen Edge Sites in OBC

In the OBC geometry, the edge sites $i = 0$ and $i = L-1$ have a special status within the largest fragment. Because the $\mathcal{O}$ operator includes the ZZ interaction only for bonds $(i, i+1)$ with $0 \le i \le L-2$, the edge sites participate in fewer constraints. The code explicitly checks whether a site is **frozen** in the fragment:

> A site $j$ is frozen if, across all $D$ basis states in the fragment, the bit at position $j$ takes a single fixed value (either always $|0\rangle$ or always $|1\rangle$).

For the largest (no-$\uparrow\uparrow$-bond) fragment, **both edge sites are frozen**. This is verified programmatically for all fragments with $D > 1$.

### 5.2 Consequence: $S_A^{(\text{edge})} \approx 0$

When we choose subsystem $A = \{\text{site } 0\}$ (a single edge site), the fragment Page value is exactly zero:

$$
S_{\text{Page}}^{(\text{frag, edge})} = 0,
$$

because a frozen site in a product-state superposition within the fragment never develops entanglement with the rest of the chain (at leading order in $g$).

### 5.3 Edge Entropy Dynamics Confirm Frozen Mode

The code evolves the single-site bipartite entanglement entropy $S_{\{0\}}(n)$ starting from random computational basis states drawn from the largest fragment. The key result:

> The edge-site entanglement entropy $S_{\{0\}}(nT)$ remains $\approx 0$ throughout the Floquet evolution, up to perturbative leakage $\sim \mathcal{O}(g^2/J_0^2)$.

In contrast, a **bulk site** (e.g., site $L/2$) is mobile within the fragment, develops nonzero entanglement entropy, and saturates to a finite fragment Page value $S_{\text{Page}}^{(\text{frag, bulk})} > 0$.

This edge-vs-bulk contrast is presented as a two-panel plot and constitutes direct evidence for **Floquet edge modes** in the fragmented prethermal regime. Any nonzero $S_{\{0\}}$ at finite $g$ is attributed to inter-fragment leakage from the perturbation $g\sum_i \sigma_i^x$, which weakly couples the fragment to the rest of the Hilbert space.


## 6. Computational Methods

### 6.1 Sparse Hamiltonian Construction

The Hamiltonians $H_\pm$ are built as `scipy.sparse.csr_matrix` with exactly $N_s \times (L+1)$ nonzero entries per matrix ($1$ diagonal + $L$ off-diagonal $\sigma^x$ flips per row). Memory scales as $\mathcal{O}(L \cdot 2^L)$, far below the dense $\mathcal{O}(4^L)$.

**Bit convention:** Site $j \to$ bit position $p(j) = L - 1 - j$. The spin eigenvalue is $\sigma_j^z = 1 - 2c_j$ where `c_j = (s >> p(j)) & 1`.

### 6.2 Krylov Time Evolution (Matrix-Free)

Instead of forming the dense $N_s \times N_s$ Floquet unitary, one Floquet period is applied via `scipy.sparse.linalg.expm_multiply`:

$$
|\psi'\rangle = e^{A_-}\, e^{A_+}\, |\psi\rangle, \qquad A_\pm = -i H_\pm T / (2\hbar).
$$

**Adaptive subdivision:** When $\|A_\pm\|_1 > 5$, the generator is split as $e^A = (e^{A/K})^K$ with $K = \lceil \|A\|_1 / 5 \rceil$, so each Krylov call sees a spectral norm $\le 5$ and completes in a single Lanczos pass.

**Batched evolution:** All $B$ initial states are stored as columns of an $N_s \times B$ matrix. The Krylov basis is reused across columns, giving near-linear speedup in $B$.

### 6.3 Complexity Summary

| Operation | Time | Memory | Applicable $L$ |
|-----------|------|--------|-----------------|
| Build $H_\pm$ (sparse) | $\mathcal{O}(L \cdot 2^L)$ | $\mathcal{O}(L \cdot 2^L)$ | $L \le 20+$ |
| One Floquet step (Krylov, $B$ states) | $\mathcal{O}(K \cdot L \cdot 2^L \cdot B)$ | $\mathcal{O}(2^L \cdot B)$ | $L \le 18$ |
| Bipartite entropy (SVD per state) | $\mathcal{O}(2^L \cdot \min(d_A, d_B))$ | $\mathcal{O}(2^L)$ | $L \le 20$ |
| Fragment identification (BFS) | $\mathcal{O}(L \cdot 2^L)$ | $\mathcal{O}(L \cdot 2^L)$ | $L \le 20$ |
| Fragment $U_F$ ($D \times D$ via embed+Krylov) | $\mathcal{O}(K \cdot L \cdot 2^L \cdot D)$ | $\mathcal{O}(2^L \cdot D)$ | $D \lesssim 10^3$ |
| Eigendecomposition of $U_{\text{frag}}$ | $\mathcal{O}(D^3)$ | $\mathcal{O}(D^2)$ | $D \lesssim 10^4$ |
| MC fragment Page value ($N_{\text{MC}}$ samples) | $\mathcal{O}(N_{\text{MC}} \cdot 2^L)$ | $\mathcal{O}(2^L)$ | $L \le 20$ |


## 7. Code Architecture

The code is structured as a sequential notebook (originally Colab), with the following logical parts:

| Part | Function | Key outputs |
|------|----------|-------------|
| **1** (Prompt 1) | `build_H_floquet_obc`, `FloquetIsingChain` | Sparse $H_\pm$, Krylov generators $A_\pm$ |
| **2** (Prompt 2) | `apply_floquet_step`, `build_floquet_operator` | Krylov evolution, dense $U_F$ (validation only) |
| **3** (Prompt 3) | `build_random_product_state`, `build_product_states_batched` | Initial product states with $S_A(0) = 0$ |
| **4** (Prompt 4) | `bipartite_entropy`, `compute_page_value` | SVD-based entropy, Page value |
| **5** (Prompt 5) | `compute_entanglement_dynamics` | Ensemble $\langle S_A(n)\rangle$ time series |
| **6** (Part 6A/B) | Simulation driver + plotting | Full Hilbert space dynamics, $S/S_{\text{Page}}$ plots |
| **8** (Part 8) | `build_O_operator` | Fragmentation operator $\mathcal{O}$ diagonal |
| **9** (Part 9) | `build_connectivity_graph`, `find_connected_components` | Fragment identification, $\uparrow\uparrow$ bond analysis |
| **10** (Part 10) | `compute_fragment_page_value_MC` | $S_{\text{Page}}^{(\text{frag})}$ via Monte Carlo |
| **11** (Part 11A/B) | Fragment-basis dynamics + plotting | $S_A(n)$ in largest fragment, saturation to $S_{\text{Page}}^{(\text{frag})}$ |
| **12** (Part 12) | Multi-fragment comparison | $S_A(n)$ for distinct-$D$ fragments |
| **13** | $S_A$ vs. quasi-energy scatter | ETH violation / scar-type states in fragment eigenstates |
| **14** | `compute_page_value_general`, `compute_edge_entropy_dynamics` | Arbitrary-$L_A$ Page value, edge-site dynamics |
| **15** | `check_site_frozen_in_fragment`, `run_edge_and_bulk`, `plot_edge_vs_bulk` | Edge mode confirmation, frozen vs. mobile sites |


## 8. Parameter Choices and Physical Regimes

The **prethermal condition** requires $h_0 = 2J_0$ and the fragmentation driving period $T = n\pi/J_0$. The transverse field $g = 1.0$ is chosen small relative to $J_0 = 10.0$ to ensure that fragmentation is well-defined at leading order, while still allowing numerically observable leakage effects.

**Production parameters** (from the final execution block):

| Parameter | Symbol | Value | Rationale |
|-----------|--------|-------|-----------|
| Chain length | $L$ | $10$–$12$ | Accessible to exact diag; $N_s = 1024$–$4096$ |
| Ising coupling | $J_0$ | $10.0$ | Sets energy scale |
| Longitudinal field | $h_0$ | $20.0$ | $h_0 = 2J_0$ (prethermal condition) |
| Transverse field | $g$ | $1.0$ | $g/J_0 = 0.1$; perturbative regime |
| Period | $T$ | $n\pi/J_0$ | Fragmentation resonance; $n = 1$ or $2$ |
| Floquet steps | $n_{\text{steps}}$ | $200$ | Sufficient for entropy saturation |
| Initial states | $B$ | $10$ | Ensemble average |
| MC Page samples | $N_{\text{MC}}$ | $1000$–$10000$ | SEM $\lesssim 10^{-3}$ |


## 9. Summary of Key Results

1. **ETH violation from fragmentation:** The half-chain entanglement entropy of states initialised in the largest fragment saturates to $S_{\text{Page}}^{(\text{frag})} < S_{\text{Page}}^{(\text{full})}$, confirming that prethermal Hilbert space fragmentation restricts ergodicity.

2. **Fragment-resolved thermalisation:** Different fragments with distinct dimensions $D$ each thermalise to their own $S_{\text{Page}}^{(\mathcal{D})}$, providing a systematic consistency check.

3. **Many-body scar-type eigenstates:** The $S_A$ vs. quasi-energy scatter for Floquet eigenstates within the largest fragment shows a wide distribution of entanglement entropies, with outlier low-entropy states reminiscent of quantum many-body scars.

4. **Frozen edge modes (OBC):** Both boundary sites ($i = 0$ and $i = L-1$) are frozen in all non-singleton fragments of the largest Hilbert space sector. The single-site edge entanglement entropy remains $\approx 0$ under Floquet evolution, while the bulk site thermalises within the fragment. This provides a direct signature of Floquet prethermal edge modes.


## 10. References

1. J. M. Deutsch, Phys. Rev. A **43**, 2046 (1991). — *Quantum statistical mechanics in a closed system.*
2. M. Srednicki, Phys. Rev. E **50**, 888 (1994). — *Chaos and quantum thermalization.*
3. L. D'Alessio et al., Adv. Phys. **65**, 239 (2016). — *From quantum chaos and ETH to statistical mechanics.*
4. D. N. Page, Phys. Rev. Lett. **71**, 1291 (1993). — *Average entropy of a subsystem.*
5. S. K. Foong and S. Kanno, Phys. Rev. Lett. **72**, 1148 (1994). — *Proof of Page's conjecture.*
6. L. Vidmar and M. Rigol, Phys. Rev. Lett. **119**, 220603 (2017). — *Entanglement entropy of eigenstates of quantum chaotic Hamiltonians.*
7. S. Ghosh, I. Paul, and K. Sengupta, Phys. Rev. Lett. **130**, 120401 (2023). — *Prethermal fragmentation in a periodically driven fermionic chain.*
8. P. Sala et al., Phys. Rev. X **10**, 011047 (2020). — *Ergodicity breaking from Hilbert space fragmentation.*
9. A. Sen, D. Sen, and K. Sengupta, J. Phys.: Condens. Matter **33**, 443003 (2021). — *Analytic approaches to periodically driven closed quantum systems.*
