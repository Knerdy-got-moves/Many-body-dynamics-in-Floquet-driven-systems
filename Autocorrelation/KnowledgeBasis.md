# Knowledge Basis: Many-Body Dynamics in Floquet-Driven Systems

**Based on**: [`Optimised_Autocorrelation_in_large_system_sizes.ipynb`](./Optimised_Autocorrelation_in_large_system_sizes.ipynb)

---

## 1. Model

We study a one-dimensional spin-1/2 Ising chain of length $L$ with **open boundary conditions (OBC)**, driven by a symmetric square-wave protocol. The full Hamiltonian is:

$$
H(t) = H_0(t) + H_1
$$

$$
H_0(t) = -J(t)\sum_{i=1}^{L-1}\sigma_i^z\sigma_{i+1}^z - h(t)\sum_{i=1}^{L}\sigma_i^z, \qquad H_1 = -g\sum_{i=1}^{L}\sigma_i^x
$$

where $|g| \ll \{|J_0|, |h_0|\}$ is a weak static transverse perturbation, and the couplings follow a symmetric square-wave drive of period $T$:

$$
J(t) = \begin{cases} +J_0, & 0 \le t \le T/2 \\ -J_0, & T/2 < t \le T \end{cases}, \qquad h(t) = \begin{cases} +h_0, & 0 \le t \le T/2 \\ -h_0, & T/2 < t \le T \end{cases}
$$

### Floquet Hamiltonian

The Floquet unitary (one-period time evolution operator) defines the Floquet Hamiltonian $H_F$:

$$
U(T,0) = e^{-iH_F T/\hbar}
$$

The first-order Floquet Hamiltonian, computed via Floquet perturbation theory in the eigenbasis of $\mathcal{O} = J_0\sum_{i=1}^{L-1}\sigma_i^z\sigma_{i+1}^z + h_0\sum_{i=1}^{L}\sigma_i^z$, has matrix elements:

$$
(H_F^{(1)})_{nm} = -\langle n|g\sum_{i=1}^{L}\sigma_i^x|m\rangle \left[\delta_{P_{nm},0} + (1 - \delta_{P_{nm},0})sinc\left(\frac{P_{nm}T}{4\hbar}\right)e^{-iP_{nm}T/(4\hbar)}\right]
$$

where $\mathcal{O}|m\rangle = P_m|m\rangle$ and $P_{nm} = P_n - P_m$.

---

## 2. Observable: Infinite-Temperature Autocorrelation

The primary diagnostic is the infinite-temperature autocorrelation function:

$$
C_j(nT) = \langle \psi_0 | \sigma_j^z(nT)\,\sigma_j^z(0) | \psi_0 \rangle
$$

computed via exact diagonalization using an ensemble of random initial states $|\psi_0\rangle$ (Haar-random in the Fock basis). A non-zero saturation value of $C_j(nT)$ at long times ($\sim 500$ Floquet steps) signals a violation of the eigenstate thermalization hypothesis (ETH).

---

## 3. Key Observations (Open Boundary Conditions)

All observations below are made in the OBC setting.

### 3.1 Hilbert Space Fragmentation

**Observation**: Hilbert space fragmentation (HSF) occurs at driving time periods

$$
T = \frac{2m\pi}{J_0}, \qquad m \in \mathbb{N}
$$

when $h_0 = 2J_0$.

**Mechanism**: At these special driving periods, the sinc filter in the first-order Floquet Hamiltonian kills all off-diagonal matrix elements connecting states with different classical energies ($P_{nm} \neq 0$). Only transitions satisfying $P_{nm} = 0$ survive, fragmenting the Hilbert space into exponentially many disconnected Krylov sectors. Specifically, writing $P = 2kJ_0$ with integer $k$, and $T = 2\pi p / J_0$ with $p \in \mathbb{Z}^+$, the argument of the sinc becomes $k p \pi$, which is a zero for all $k \neq 0$. This yields:

$$
(H_F^{(1)})_{nm} = -\langle n|g\sum_{i=1}^{L}\sigma_i^x|m\rangle\,\delta_{P_{nm},0}
$$

The effective Hamiltonian at $T = 2\pi/J_0$ with $h_0 = 2J_0$ reduces to:

$$
H_F^{(1)} = -g\sum_{i=2}^{L-1}\pi_{i-1}\,\sigma_i^x\,\pi_{i+1}, \qquad \pi_i = \frac{\mathbb{I} - \sigma_i^z}{2}
$$

This constrained Hamiltonian only allows bulk spin flips when both neighbors are in the $|\downarrow\rangle$ state, leading to fragmentation.

### 3.2 Dynamical Freezing

**Observation**: Dynamical freezing is observed when

$$
\frac{h_0}{J_0} = 2n, \qquad n \in \mathbb{Z},\; n \ge 2
$$

at driving time periods $T = m\pi/J_0$ ($m \in \mathbb{N}$).

- **Bulk freezing with oscillating edges**: For odd $m$, at $h_0/J_0 = 2n$ with $n\ge 2$, the bulk of the chain is dynamically frozen (the autocorrelation remains near unity), while the edge sites exhibit oscillatory behavior.
- **Complete freezing at even $m$**: At this specific driving period, the entire system—both bulk and edges—is frozen. The autocorrelation saturates at a finite value across all sites, indicating a complete absence of thermalization.

**Mechanism**: For larger values of $h_0/J_0$, the energy mismatches $P_{nm}$ grow, and more spin-flip channels are suppressed by the sinc filter. At sufficiently large $h_0/J_0 = 2n$ ($n \ge 2$), essentially all channels that could cause bulk spin flips are deactivated at $T = m\pi/J_0$. The edge sites, having only one neighbor, have different energy mismatches and may retain residual dynamics (oscillations) except at the fully resonant period $T = 2m\pi/J_0$ where all channels shut off.

### 3.3 Edge Modes

**Observation**: Edge modes appear at driving time periods

$$
T = \frac{2\pi}{J_0}
$$

for $h_0 = 2nJ_0$ ($n$ a positive integer).

At these parameters, edge sites (sites $1$ and $L$) retain their initial magnetization for hundreds of Floquet cycles, while bulk sites thermalize within their respective fragments. This constitutes a **prethermal edge memory**.

**Mechanism**: The effective first-order Floquet Hamiltonian at $T = 2\pi/J_0$,

$$
H_F^{(1)} = -g\sum_{i=2}^{L-1}\pi_{i-1}\,\sigma_i^x\,\pi_{i+1}
$$

sums only from $i = 2$ to $L - 1$. There are **no terms corresponding to edge flips** (sites $1$ and $L$), because edge spins have only one neighbor, giving them energy mismatches $P = 2J_0$ or $P = 6J_0$ (rather than $P = 0, 4J_0, 8J_0$ for bulk spins), and these are killed by the sinc zeros at $T = 2\pi/J_0$. The edge spins are thus dynamically decoupled from the rest of the chain at first order in $g$.

**Not symmetry-protected**: The edge modes are **not** symmetry-protected topological (SPT) modes. Although the effective Hamiltonian possesses:
- **Inversion symmetry**: $I\,H_F\,I^{-1} = H_F$ where $I\,\sigma_i^{x,y,z}\,I^{-1} = \sigma_{L+1-i}^{x,y,z}$
- **Chiral symmetry**: $\{\Gamma, H_F^{(1)}\} = 0$ where $\Gamma = \prod_{j=1}^L \sigma_j^z$

there is no nontrivial on-site symmetry group $G = \bigotimes_i g_i$ with $[G, H_F^{(1)}] = 0$ that could protect the edge modes in the conventional SPT sense. The edge memory can be destroyed by a symmetry-preserving perturbation such as $V = \varepsilon(\sigma_1^x \pi_2 + \pi_{L-1}\sigma_L^x)$ with $|\varepsilon| \ll |g|$.

The edge modes arise purely from the **structure of the effective Floquet Hamiltonian**, not from topological protection.

**System size dependence**: The lifetime of edge memory shows no systematic dependence on chain length $L$.

---

## 4. Selection Rules (Sinc Filter)

The driving period controls which spin-flip channels are active through the sinc function. For $h_0 = 2J_0$ in OBC, single spin flips produce energy mismatches $P \in \{0, 2J_0, 4J_0, 6J_0, 8J_0\}$. The selection rules are:

| $T$ | $P = 0$ | $P = 2J_0$ (edge) | $P = 4J_0$ | $P = 6J_0$ (edge) | $P = 8J_0$ | Summary |
|-----|---------|-------------------|-------------|-------------------|-------------|---------|
| $\pi/(2J_0)$ | on | on | on | on | off | only $\|P\| = 8J_0$ killed |
| $\pi/J_0$ | on | on | off | on | off | kills $\|P\| = 4J_0, 8J_0$ |
| $2\pi/J_0$ | on | off | off | off | off | all nonzero $P$ killed |

**Key**: "on/off" indicates whether $sinc(PT/4)$ is nonzero/zero. For OBC, $P = 2J_0$ and $P = 6J_0$ arise only from edge flips (one neighbor), while $P = 0, 4J_0, 8J_0$ are from bulk flips (two neighbors).

---

## 5. Methodology

### Analytical
- **Floquet perturbation theory**: First-order effective Hamiltonian via cumulant expansion in the weak transverse field $g$.
- **Sinc-filter analysis**: Identification of resonant driving periods where specific spin-flip channels vanish.

### Numerical
- **Exact diagonalization** using SciPy/NumPy sparse matrix methods.
- **System sizes**: Up to $L = 12$ sites (Hilbert space dimension $2^{12} = 4096$).
- **Autocorrelation**: Ensemble-averaged over random initial states at infinite temperature.
- **Fragmentation analysis**: Construction of connectivity graphs from the effective Floquet Hamiltonian; identification of connected components (Krylov sectors).
- **Magnetization contour plots**: Spatial-temporal profiles of $\langle\psi(nT)|\sigma_i^z|\psi(nT)\rangle$ revealing edge-bulk contrast.

### Typical Parameters
- $J_0 = 10$, $h_0 = 2J_0 = 20$, $g = 1$
- Driving periods: $T = \pi/J_0$ and $T = 2\pi/J_0$
- Floquet steps: up to 500

---

## 6. Summary of Phenomena

| Phenomenon | Conditions | Driving Period | Observation |
|------------|-----------|----------------|-------------|
| **Hilbert space fragmentation** | $h_0 = 2J_0$, OBC | $T = 2m\pi/J_0$, $m \in \mathbb{N}$ | Exponentially many disconnected Krylov sectors; sub-thermal autocorrelation plateaus |
| **Dynamical freezing (bulk)** | $h_0/J_0 = 2n$, $n > 2$, OBC | $T = m\pi/J_0$ | Bulk frozen, edges oscillate |
| **Dynamical freezing (complete)** | $h_0/J_0 = 2n$, $n > 2$, OBC | $T = 2\pi/J_0$ | Entire system frozen |
| **Edge modes** | $h_0 = 2nJ_0$, $n \in \mathbb{Z}^+$, OBC | $T = 2\pi/J_0$ | Edge magnetization persists $\sim 300$ Floquet steps; not SPT-protected |

---

## 7. References

1. S. Ghosh, I. Paul, and K. Sengupta, "Prethermal Fragmentation in a Periodically Driven Fermionic Chain," *Phys. Rev. Lett.* **130**, 120401 (2023). [doi:10.1103/PhysRevLett.130.120401](https://doi.org/10.1103/PhysRevLett.130.120401)
2. P. Sala *et al.*, "Ergodicity breaking arising from Hilbert space fragmentation in dipole-conserving Hamiltonians," *Phys. Rev. X* **10**, 011047 (2020). [doi:10.1103/PhysRevX.10.011047](https://doi.org/10.1103/PhysRevX.10.011047)
3. V. Khemani, M. Hermele, and R. Nandkishore, "Localization from Hilbert space shattering," *Phys. Rev. B* **101**, 174204 (2020). [doi:10.1103/PhysRevB.101.174204](https://doi.org/10.1103/PhysRevB.101.174204)
