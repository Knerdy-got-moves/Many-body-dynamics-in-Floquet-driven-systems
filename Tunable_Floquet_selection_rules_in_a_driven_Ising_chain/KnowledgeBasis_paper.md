# Knowledge Basis: Tunable Floquet Selection Rules in a Driven Ising Chain

**Based on**: [`Draft_2.tex`](./Draft_2.tex)

**Authors**: Rishi Paresh Joshi, Sanchayan Banerjee, Sneha Narasimha Moorthy, Tapan Mishra (NISER / HBNI)

---

## 1. Model

We study a periodically driven spin-$1/2$ Ising chain of length $L$ with a symmetric square-pulse sign-flip protocol. The full Hamiltonian is

$$
H(t) = H_0(t) + H_1,
$$

where the diagonal (driven) part and the static transverse perturbation are

$$
H_0(t) = \Lambda(t)\left(-J_0\sum_{i=1}^{L_b}\sigma_i^z\sigma_{i+1}^z - h_0\sum_{i=1}^{L}\sigma_i^z\right), \qquad H_1 = -g\sum_{i=1}^{L}\sigma_i^x,
$$

with $|g| \ll \{|J_0|, |h_0|\}$ a weak transverse field that induces single-spin flips. Here $L_b = L - 1$ for open boundary conditions (OBC) and $L_b = L$ for periodic boundary conditions (PBC), with $\sigma_{L+1}^z \equiv \sigma_1^z$ in the periodic case. Units are $\hbar = 1$.

### 1.1 Drive Protocol

The Ising coupling and longitudinal field follow the same symmetric square-wave sign-flip:

$$
\Lambda(t) = \begin{cases} +1, & 0 \le t < T/2, \\ -1, & T/2 \le t < T. \end{cases}
$$

### 1.2 Diagonal Operator $\mathcal{O}$

The driven part is most conveniently expressed through the time-independent diagonal operator

$$
\mathcal{O} := J_0\sum_{i=1}^{L_b}\sigma_i^z\sigma_{i+1}^z + h_0\sum_{i=1}^{L}\sigma_i^z,
$$

which measures the diagonal energy cost. A spin flip induced by $H_1$ connects two basis states with different eigenvalues of $\mathcal{O}$; that eigenvalue change controls the corresponding matrix element of the effective Floquet Hamiltonian.

### 1.3 Unperturbed Propagator

Because $\mathcal{O}$ is time-independent, $H_0(t)$ commutes with itself at all times. The unperturbed propagator is

$$
U_0(t,0) = \exp\!\left[i\,f(t)\,\mathcal{O}\right], \qquad f(t) = \begin{cases} t, & 0 \le t \le T/2, \\ T - t, & T/2 < t \le T, \end{cases}
$$

where $f(t)$ is a triangular micromotion function. Crucially, $f(T) = 0$, so $U_0(T,0) = \mathbb{I}$. The unperturbed evolution returns to the identity after one period, and the full Floquet operator $U_F = e^{-iH_F T}$ is generated entirely by the interaction-picture perturbation.

---

## 2. First-Order Floquet Hamiltonian and the Sinc Selection Rule

The interaction-picture perturbation is $V_I(t) = U_0^\dagger(t,0)\,H_1\,U_0(t,0)$. The first-order Floquet Hamiltonian is

$$
H_F^{(1)} = \frac{1}{T}\int_0^T V_I(t)\,dt.
$$

Because the symmetric sign-flip protocol causes the second half-cycle to retrace the first in the interaction picture, the integral doubles into a single half-cycle contribution. Evaluating in the eigenbasis of $\mathcal{O}$ ($\mathcal{O}|m\rangle = P_m|m\rangle$):

$$
\left(H_F^{(1)}\right)_{nm} = (H_1)_{nm}sinc\left(\frac{P_{nm}T}{4}\right) e^{-iP_{nm}T/4}, \qquad P_{nm} := P_n - P_m.
$$

> **Key result**: The sinc factor is a finite-time destructive-interference filter. Channels with $P_{nm} = 0$ survive with full first-order weight. Channels with $P_{nm}T/4 = \pi k$ ($k \neq 0$ integer) are deleted. The drive removes selected channels altogether, not merely renormalizing them.

### 2.1 Local Energy Difference $\Delta P_i(s)$

Since $H_1$ connects only states differing by one spin flip, the relevant quantity for flipping spin $i$ in configuration $|s\rangle$ is

$$
\Delta P_i(s) := P(s^{(i)}) - P(s),
$$

where $|s^{(i)}\rangle = \sigma_i^x|s\rangle$. Explicitly:

- **Bulk site** ($2 \le i \le L-1$): $\Delta P_i(s) = -2s_i\left[J_0(s_{i-1} + s_{i+1}) + h_0\right]$
- **Edge site** ($i = 1$, OBC): $\Delta P_1(s) = -2s_1(J_0 s_2 + h_0)$

This coordination dependence is the microscopic origin of the bulk-edge distinction.

---

## 3. Selection Rule Tables

At $h_0 = 2J_0$, the allowed single-flip $|\Delta P|$ values differ between OBC and PBC.

### 3.1 OBC Channels

| $|\Delta P|$ | Origin | Neighbor condition |
|---|---|---|
| $0$ | Bulk | $s_{i-1} = s_{i+1} = -1$ (both neighbors down) |
| $4J_0$ | Bulk | $s_{i-1} + s_{i+1} = 0$ (mixed neighbors) |
| $8J_0$ | Bulk | $s_{i-1} = s_{i+1} = +1$ (both neighbors up) |
| $2J_0$ | Edge only | neighbor down ($s_2 = -1$) |
| $6J_0$ | Edge only | neighbor up ($s_2 = +1$) |

**OBC sinc-filter table** ($h_0 = 2J_0$):

| $T$ | $\|\Delta P\| = 0$ | $2J_0$ (edge) | $4J_0$ | $6J_0$ (edge) | $8J_0$ | Summary |
|---|---|---|---|---|---|---|
| $\pi/(2J_0)$ | on | on | on | on | off | only $8J_0$ killed |
| $\pi/J_0$ | on | on | off | on | off | kills $4J_0$, $8J_0$ |
| $2\pi/J_0$ | on | off | off | off | off | **all nonzero $\Delta P$ killed** |

### 3.2 PBC Channels

Every site has two neighbors, so the edge-only channels ($2J_0$, $6J_0$) are absent:

$$
|\Delta P|_{\text{PBC}} \in \{0,\; 4J_0,\; 8J_0\}.
$$

**PBC sinc-filter table** ($h_0 = 2J_0$):

| $T$ | $\|\Delta P\| = 0$ | $4J_0$ | $8J_0$ |
|---|---|---|---|
| $\pi/(2J_0)$ | on | on | off |
| $\pi/J_0$ | on | off | off |
| $2\pi/J_0$ | on | off | off |

> **Key distinction**: PBC reaches the fully constrained (PXP) regime at the shorter period $T^*_{\text{PBC}} = \pi/J_0$, while OBC requires $T^*_{\text{OBC}} = 2\pi/J_0$ to also kill the edge channels.

---

## 4. Hilbert Space Fragmentation

### 4.1 Constrained Bulk Generator

At the primary resonance $h_0 = 2J_0$ and $T = 2\pi/J_0$ (OBC), only $\Delta P_i = 0$ channels survive. This requires both neighbors to be down ($s_{i-1} = s_{i+1} = -1$). Defining $\pi_i = (\mathbb{I} - \sigma_i^z)/2$ (projector onto $|\downarrow\rangle$), the surviving first-order Floquet Hamiltonian is a PXP-type constrained generator:

$$
H_F^{(1)} = -g\sum_{i=2}^{L-1}\pi_{i-1}\,\sigma_i^x\,\pi_{i+1} \qquad \text{(OBC)},
$$

with the periodic extension $i \in \{1, \ldots, L\}$ for PBC. This constraint is not imposed by hand; it is generated dynamically by the sinc interference filter.

### 4.2 Conserved Bond Projectors

Let $n_i = (\mathbb{I} + \sigma_i^z)/2$ project onto $|\uparrow\rangle$. The operator $b_i = n_i n_{i+1}$ checks for adjacent up-up pairs. Since the PXP move can never create or destroy such pairs:

$$
[H_F^{(1)},\; b_i] = 0 \qquad \forall\; i.
$$

These conserved bond projectors partition the Hilbert space into exponentially many dynamically disconnected Krylov sectors (strong fragmentation).

### 4.3 Largest Fragment Dimension

The largest fragment is the sector with no adjacent up spins ($b_i = 0$ for all bonds):

- **OBC** (edges fixed to $|\downarrow\rangle$): Dimension $= F_L$ (Fibonacci number)
- **PBC** (no-adjacent-up ring): Dimension $= \mathcal{L}_L = F_{L-1} + F_{L+1}$ (Lucas number)

Growth is $\sim \varphi^L$ with $\varphi = (1+\sqrt{5})/2$, so $D_{\max}/2^L \sim (\varphi/2)^L \to 0$ exponentially. Even the largest fragment occupies a vanishing fraction of the full Hilbert space.

### 4.4 Mazur Bound on Late-Time Memory

From three traceless conserved operators built from frozen local patterns around a bulk site $j$, the Mazur inequality gives a parameter-independent lower bound on the infinite-temperature autocorrelation:

$$
M_{\sigma_j^z} = \frac{3}{5}.
$$

> **Key result**: At resonance, the bulk autocorrelation develops a long-lived plateau above the Mazur bound $C_{\text{Mazur}} = 0.6$. Increasing $J_0/g$ pushes the system deeper into the prethermal fragmented regime.

---

## 5. Prethermal Edge Memory (OBC)

### 5.1 Coordination-Dependent Suppression

An edge spin has only one neighbor, so every edge flip produces $|\Delta P| \in \{2J_0, 6J_0\}$, which are both nonzero. At $T = 2\pi/J_0$, the sinc filter kills all edge channels at first order. The bulk remains active within constrained sectors (PXP dynamics), while the boundary is frozen more strongly.

### 5.2 Second-Order Cancellation

The symmetric sign-flip protocol also removes the second-order Floquet contribution at the boundary:

$$
H_{F,\text{edge}}^{(2)} = 0.
$$

This cancellation arises from the temporal symmetry of the drive: the two half-cycles contribute with opposite sign in the second-order perturbation series.

### 5.3 Third-Order Leakage

The first nonvanishing edge-flip channel appears at third order. For the left edge at $h_0 = 2J_0$:

$$
H_{F,\text{edge}}^{(3)} = -\frac{g^3}{2J_0^2}\left(\Pi_2^{\downarrow} + \frac{1}{9}\Pi_2^{\uparrow}\right)\sigma_1^x,
$$

where $\Pi_2^{\downarrow} = (\mathbb{I} - \sigma_2^z)/2$ and $\Pi_2^{\uparrow} = (\mathbb{I} + \sigma_2^z)/2$ project onto the down/up state of the adjacent site. The edge-memory timescale therefore scales as

$$
\tau_{\text{edge}} \sim \frac{J_0^2}{g^3}.
$$

> **Key result**: The prethermal edge memory is **not** topological, **not** a strong zero mode, and **not** symmetry-protected. It is purely structural: the Floquet filter generates a coordination-selective perturbative hierarchy where the edge is suppressed through second order and leaks only at third order. The lifetime $\tau_{\text{edge}}$ is $L$-independent.

---

## 6. PXP Scars in the Largest Fragment (PBC)

Under periodic boundary conditions, there are no boundary-specific channel suppressions. At the commensurate drive, the largest fragment is exactly the constrained PXP Hilbert space at first order:

$$
H_{\text{eff}}\big|_{\mathcal{H}^{\text{PBC}}_{\max}} = H_{\text{PXP}}.
$$

### 6.1 Scar Diagnostics

The standard PXP scar signatures are inherited within the prethermal window:

- **Low-entanglement outliers**: Floquet eigenstates with $S_A$ well below the fragment Page value
- **$\mathbb{Z}_2$ CDW overlap**: States $|\phi_\alpha\rangle$ with $|c_\alpha|^2 = |\langle \mathbb{Z}_2|\phi_\alpha\rangle|^2 \gg 1/D_{\text{frag}}$, where $|\mathbb{Z}_2\rangle = |\uparrow\downarrow\uparrow\downarrow\cdots\rangle$
- **Return fidelity revivals**: $F(n) = |\langle \mathbb{Z}_2|U_F^n|\mathbb{Z}_2\rangle|^2$ shows pronounced revivals above the ergodic floor $1/D_{\text{frag}}$

> **Key result**: The PXP scar sector is not imposed by hand; it is generated dynamically by the same sinc selection rule that produces fragmentation. The scar phenomenology is explicitly prethermal and confined to the largest fragment.

---

## 7. Floquet Freezing

Floquet freezing is the complementary outcome of the same selection rule. The distinction from fragmentation is sharp:

| Regime | Condition | $\Delta P = 0$ channels? | $H_F^{(1)}$ | Dynamics |
|---|---|---|---|---|
| **Fragmentation** | $h_0 = 2J_0$ | Yes (bulk, both neighbors down) | Constrained PXP | Active within fragments |
| **Freezing** | $h_0 = 2nJ_0$, $n > 1$ | No | $H_F^{(1)} = 0$ | Frozen at first order |

At the higher commensurate ratios $h_0 = 2nJ_0$ ($n > 1$, integer) with $T = 2\pi/J_0$, every first-order single-spin-flip channel is pushed onto a sinc zero. The first-order Floquet Hamiltonian vanishes:

$$
H_F^{(1)} = 0 \qquad \text{(Floquet freezing)}.
$$

**Incomplete freezing (OBC)**: At $T = \pi/J_0$ with $h_0 = 2nJ_0$ ($n \ge 2$), bulk channels are suppressed but some edge channels remain active. The bulk is frozen while the edges exhibit oscillatory dynamics.

---

## 8. Generalization to Higher Dimensions

The mechanism extends beyond one dimension. On a finite induced subgraph $G = (V, E)$ of a $z$-regular parent graph with site degrees $d_i$, the driven Ising model is

$$
H(t) = -\lambda(t)\left[J\sum_{(ij) \in E}Z_i Z_j + h\sum_{i \in V}Z_i\right] - g\sum_{i \in V}X_i,
$$

with the same symmetric sign-flip protocol $\lambda(t) = \pm 1$. The single-flip gap at site $i$ is

$$
\Delta_i(s) = -2s_i\left(J\sum_{j \in \partial i}s_j + h\right).
$$

Choosing $h = zJ$ and the commensurate period $T_\star = 2\pi/J$:

- **Bulk sites** ($d_i = z$): admit a zero-gap channel only when all neighbors are down
- **Boundary sites** ($d_i < z$): cannot satisfy $\Delta_i = 0$

The first-order Floquet Hamiltonian is the graph-PXP generator:

$$
H_F^{(1)}(T_\star) = -g\sum_{i:\,d_i = z}\left(\prod_{j \in \partial i}\pi_j\right)X_i.
$$

> The same Floquet filter that isolates bulk flips in the 1D open chain isolates the full-coordination core of any finite piece of a regular Ising graph.

---

## 9. Summary of Key Results

| Phenomenon | Conditions | Driving Period | Key Result |
|---|---|---|---|
| **Hilbert space fragmentation** | $h_0 = 2J_0$ | $T = 2m\pi/J_0$ (OBC), $T = m\pi/J_0$ (PBC) | PXP-type constrained generator; exponentially many Krylov sectors; Fibonacci/Lucas fragment dimensions |
| **Prethermal edge memory** | $h_0 = 2J_0$, OBC | $T = 2\pi/J_0$ | Edge frozen through $\mathcal{O}(g^2)$; leaks at $\mathcal{O}(g^3)$; $\tau_{\text{edge}} \sim J_0^2/g^3$; not topological |
| **PXP scar phenomenology** | $h_0 = 2J_0$, PBC | $T = \pi/J_0$ | Largest fragment $=$ PXP sector; inherited $\mathbb{Z}_2$ revivals and low-$S_A$ outliers |
| **Floquet freezing** | $h_0 = 2nJ_0$, $n > 1$ | $T = 2\pi/J_0$ | $H_F^{(1)} = 0$; complete suppression of first-order dynamics |
| **Graph generalization** | $h = zJ$, $z$-regular graph | $T_\star = 2\pi/J$ | Coordination-selective graph-PXP generator on full-degree sites |

---

## 10. References

1. S. Ghosh, I. Paul, and K. Sengupta, "Prethermal Fragmentation in a Periodically Driven Fermionic Chain," *Phys. Rev. Lett.* **130**, 120401 (2023). [doi:10.1103/PhysRevLett.130.120401](https://doi.org/10.1103/PhysRevLett.130.120401)
2. P. Sala *et al.*, "Ergodicity breaking arising from Hilbert space fragmentation in dipole-conserving Hamiltonians," *Phys. Rev. X* **10**, 011047 (2020). [doi:10.1103/PhysRevX.10.011047](https://doi.org/10.1103/PhysRevX.10.011047)
3. V. Khemani, M. Hermele, and R. Nandkishore, "Localization from Hilbert space shattering," *Phys. Rev. B* **101**, 174204 (2020). [doi:10.1103/PhysRevB.101.174204](https://doi.org/10.1103/PhysRevB.101.174204)
4. A. Sen, D. Sen, and K. Sengupta, "Analytic approaches to periodically driven closed quantum systems: methods and applications," *J. Phys.: Condens. Matter* **33**, 443003 (2021). [doi:10.1088/1361-648X/ac1b61](https://doi.org/10.1088/1361-648X/ac1b61)
5. S. Ghosh, B. Paul, and K. Sengupta, "Driven Ising model: selection rules, interference, and Floquet scars," *arXiv:* 2506.xxxxx (2025).
6. C. J. Turner *et al.*, "Weak ergodicity breaking from quantum many-body scars," *Nature Physics* **14**, 745 (2018). [doi:10.1038/s41567-018-0137-5](https://doi.org/10.1038/s41567-018-0137-5)
7. H. Bernien *et al.*, "Probing many-body dynamics on a 51-atom quantum simulator," *Nature* **551**, 579 (2017). [doi:10.1038/nature24622](https://doi.org/10.1038/nature24622)
8. D. V. Else, B. Bauer, and C. Nayak, "Floquet Time Crystals," *Phys. Rev. Lett.* **117**, 090402 (2016). [doi:10.1103/PhysRevLett.117.090402](https://doi.org/10.1103/PhysRevLett.117.090402)
9. D. A. Abanin, W. De Roeck, and F. Huveneers, "Exponentially Slow Heating in Periodically Driven Many-Body Systems," *Phys. Rev. Lett.* **115**, 256803 (2015). [doi:10.1103/PhysRevLett.115.256803](https://doi.org/10.1103/PhysRevLett.115.256803)
10. A. Das, "Exotic freezing of response in a quantum many-body system," *Phys. Rev. B* **82**, 172402 (2010). [doi:10.1103/PhysRevB.82.172402](https://doi.org/10.1103/PhysRevB.82.172402)
