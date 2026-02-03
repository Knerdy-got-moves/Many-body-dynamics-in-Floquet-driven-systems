# Knowledge Base: Autocorrelation in Spin-1/2 Ising Chain

## Source Notebook

`Optimised_Autocorrelation_in_large_system_sizes.ipynb` (86 cells, 40 code / 46 markdown)

---

## 1. Physical Model

### 1.1 System: Kicked Ising Chain (Open Boundary Conditions)

A one-dimensional spin-1/2 Ising chain of length L with **open boundary conditions** (OBC), periodically driven by a symmetric square-wave protocol.

**Full Hamiltonian:**

```
H(t) = H_0(t) + H_1
```

- **Driven part:** `H_0(t) = -J(t) * sum_{i=1}^{L-1} sigma_i^z sigma_{i+1}^z - h(t) * sum_{i=1}^{L} sigma_i^z`
- **Static perturbation:** `H_1 = -g * sum_{i=1}^{L} sigma_i^x`, with `|g| << {|J_0|, |h_0|}`

**Square-wave drive protocol (period T):**

```
J(t) = +J_0  for 0 <= t <= T/2,   -J_0  for T/2 < t <= T
h(t) = +h_0  for 0 <= t <= T/2,   -h_0  for T/2 < t <= T
```

This gives two half-period Hamiltonians:
- **H+** (first half): `-J_0 * ZZ - h_0 * Z - g * X`
- **H-** (second half): `+J_0 * ZZ + h_0 * Z - g * X`

### 1.2 Floquet Formalism

The **Floquet unitary** (one-period time-evolution operator) is:

```
U(T,0) = exp(-i * H_minus * T/2) @ exp(-i * H_plus * T/2)
```

The **Floquet Hamiltonian** H_F is defined implicitly: `U(T,0) = exp(-i * H_F * T)`.

### 1.3 First-Order Floquet Hamiltonian (Perturbation Theory)

The first-order Floquet Hamiltonian in the eigenbasis of the diagonal operator `O = J_0 * sum sigma_i^z sigma_{i+1}^z + h_0 * sum sigma_i^z` is:

```
(H_F^{(1)})_{nm} = -<n| g * sum sigma_i^x |m> * [delta_{P_nm,0} + (1 - delta_{P_nm,0}) * sinc(P_nm * T / 4) * exp(-i * P_nm * T / 4)]
```

where `P_nm = P_n - P_m` are eigenvalue differences of O.

**At the special period T = 2*pi/J_0**, the effective Hamiltonian reduces to:

```
H_F^{(1)} = -g * sum_{i=2}^{L-1} pi_{i-1} * sigma_i^x * pi_{i+1}
```

where `pi_i = (I - sigma_i^z) / 2` is the projector onto spin-down at site i.

**Key consequence:** No edge-flip terms appear in H_F^{(1)}, implying **prethermal edge memory** -- the edge spins are not flipped by the effective dynamics.

### 1.4 Key Physics Concepts

| Concept | Description |
|---------|-------------|
| **ETH Violation** | The Eigenstate Thermalization Hypothesis fails: autocorrelation saturates to a non-zero value instead of decaying to zero |
| **Hilbert Space Fragmentation (HSF)** | The Hilbert space breaks into exponentially many disconnected sectors under the Floquet dynamics |
| **Prethermal Edge Memory** | Edge spins retain magnetization for ~300 Floquet cycles at T = 2*pi/J_0 |
| **Non-SPT Edge Modes** | The edge memory is NOT symmetry-protected topological; it can be destroyed by perturbations respecting all symmetries |

### 1.5 Symmetries of H_F^{(1)}

- **Inversion symmetry:** `I * sigma_i^{x,y,z} * I^{-1} = sigma_{L+1-i}^{x,y,z}` so `I * H_F * I^{-1} = H_F`
- **Chiral symmetry:** `Gamma = prod_j sigma_j^z` anti-commutes with H_F: `{Gamma, H_F^{(1)}} = 0`
- **No on-site symmetry** protects the edge modes (proven by constraint analysis in the notebook)

### 1.6 ETH-Violating Driving Periods

The autocorrelation shows non-zero saturation (ETH violation) at special periods:

```
T = n * pi / J_0,  n in Z (integers)
```

---

## 2. Computational Architecture

### 2.1 Dependencies

```python
QuSpin          # Exact diagonalization, Hamiltonian construction
NumPy           # Linear algebra
SciPy           # Sparse matrices, Krylov methods (expm_multiply)
Matplotlib      # Plotting
```

### 2.2 Data Flow

```
FloquetIsingChain (build Hamiltonians)
       |
       v
make_floquet_propagator (Krylov-based U_F)
       |
       v
generate_random_states (Haar-random initial states)
       |
       v
compute_ensemble_average_batched (autocorrelation C(nT))
       |
       v
run_autocorr_coupling_comparison / run_autocorr_site_comparison (parameter scans)
       |
       v
plot_autocorr_*_comparison (publication figures)
```

### 2.3 Optimization Strategy: Sparse + Krylov

The notebook avoids dense matrices entirely for time evolution. Key design choices:

| Component | Dense Approach | This Notebook's Approach |
|-----------|---------------|-------------------------|
| Hamiltonians | `numpy.ndarray` (Ns x Ns) | `scipy.sparse.csr_matrix` |
| Time evolution | `scipy.linalg.expm` | `scipy.sparse.linalg.expm_multiply` (Krylov) |
| State storage | One state at a time | Batched `(Ns, B)` matrices |
| sigma_z action | Dense matrix multiply | Diagonal element-wise multiply |

**Memory comparison (from notebook):**

| L | Ns | Dense H memory | Sparse H memory | Compression |
|---|-----|---------------|-----------------|-------------|
| 8 | 256 | 0.5 MB | 0.03 MB | ~17x |
| 12 | 4096 | 128 MB | 0.5 MB | ~256x |
| 20 | 1,048,576 | 16 TB | 240 MB | ~68,000x |

---

## 3. Function Reference

### 3.1 Hamiltonian Construction

#### `class FloquetIsingChain`

**Purpose:** Constructs and stores the Floquet-driven Ising chain Hamiltonians in sparse format.

**Constructor:** `FloquetIsingChain(L, J_0, h_0, g, TP, hbar=1.0)`

| Parameter | Type | Description |
|-----------|------|-------------|
| `L` | int | Chain length (number of spins) |
| `J_0` | float | Ising coupling amplitude |
| `h_0` | float | Longitudinal field amplitude |
| `g` | float | Static transverse field (perturbation, `|g| << |J_0|`) |
| `TP` | float | Driving period T |
| `hbar` | float | Reduced Planck constant (default 1.0) |

**Key attributes:**
- `self.Ns = 2**L` -- Hilbert space dimension
- `self.basis` -- QuSpin `spin_basis_1d` object with Pauli matrices
- `self._H_plus_sparse`, `self._H_minus_sparse` -- CSR sparse Hamiltonians (built on demand)

**Methods:**

| Method | Returns | Description |
|--------|---------|-------------|
| `build_hamiltonians()` | `(H_plus_sparse, H_minus_sparse)` | Builds H+ and H- as CSR sparse matrices using QuSpin. H+ = -J_0*ZZ - h_0*Z - g*X, H- = +J_0*ZZ + h_0*Z - g*X |
| `_verify_hermiticity(n_samples=5)` | None | Checks H = H^dagger on random vectors |
| `_estimate_sparse_memory()` | float (MB) | Estimates CSR memory from nnz count |
| `get_H_plus_linop()` | `LinearOperator` | Matrix-free fallback using QuSpin's internal matvec |
| `get_H_minus_linop()` | `LinearOperator` | Matrix-free fallback for H- |
| `H_plus_sparse` (property) | `csr_matrix` | Access H+ sparse matrix (raises if not built) |
| `H_minus_sparse` (property) | `csr_matrix` | Access H- sparse matrix (raises if not built) |

**Typical instantiation:**
```python
ising = FloquetIsingChain(L=12, J_0=10.0, h_0=20.0, g=1.0, TP=2*np.pi/10.0)
H_plus, H_minus = ising.build_hamiltonians()
```

---

### 3.2 Time Evolution (Krylov Methods)

#### `evolve_state(H, psi, dt, *, tol=1e-10)`

**Purpose:** Single time step via Krylov subspace method: `psi_out = exp(-i * H * dt) @ psi`.

| Parameter | Type | Description |
|-----------|------|-------------|
| `H` | sparse matrix or LinearOperator | Hamiltonian, shape (Ns, Ns) |
| `psi` | ndarray | State vector (Ns,) or batch (Ns, B) |
| `dt` | float | Time step |
| `tol` | float | Krylov error tolerance |

**Returns:** `psi_out` -- evolved state, same shape as input.

**Implementation:** Wraps `scipy.sparse.linalg.expm_multiply(A=-1j*dt*H, B=psi)`.

---

#### `evolve_many(H, psi0, t_list, *, tol=1e-10, store_states=False)`

**Purpose:** Evolve through multiple time points sequentially.

| Parameter | Description |
|-----------|-------------|
| `t_list` | Sorted array of times |
| `store_states` | If True, returns all intermediate states; if False, only final state |

---

#### `apply_floquet_period(psi, H_plus, H_minus, T, hbar=1.0, *, tol=1e-10)`

**Purpose:** Apply one complete Floquet period: `U_F @ psi = exp(-i*H_minus*T/2) @ exp(-i*H_plus*T/2) @ psi`.

Accepts single states `(Ns,)` or batches `(Ns, B)`.

---

#### `apply_floquet_n_periods(psi, H_plus, H_minus, T, n_periods, ...)`

**Purpose:** Apply n complete Floquet periods: `U_F^n @ psi`. Has optional `callback(n, psi)` called after each period.

---

#### `make_floquet_propagator(ising_chain, *, tol=1e-10)`

**Purpose:** Returns a callable `apply_U(psi)` that applies one Floquet period. This is the main interface used throughout the notebook.

**Returns:** `apply_U` -- a callable with attached metadata `.Ns`, `.T`, `.tol`.

**Usage:**
```python
apply_U = make_floquet_propagator(ising, tol=1e-10)
psi_next = apply_U(psi)          # single state
Psi_next = apply_U(Psi_batch)    # batch of states (Ns, B)
```

---

### 3.3 Spin Operators

#### `get_sigma_z_diagonal(ising_chain, site_i)`

**Purpose:** Returns the diagonal of sigma_z at site `site_i` as an array of shape `(Ns,)`.

**Convention:** Uses QuSpin's bit convention: bit=0 -> spin UP (sigma_z = +1), bit=1 -> spin DOWN (sigma_z = -1).

**Implementation:** Direct bit extraction from basis states -- no matrix construction needed.

---

#### `make_sigma_z_matvec(ising_chain, site_i)`

**Purpose:** Creates a callable `apply_sigma_z(psi)` that applies sigma_z at site `site_i` to a state vector or batch.

| Input shape | Output shape | Operation |
|------------|-------------|-----------|
| `(Ns,)` | `(Ns,)` | `diag_z * psi` (element-wise) |
| `(Ns, B)` | `(Ns, B)` | `diag_z[:, None] * Psi` (broadcast) |

**Attached metadata:** `.site_i`, `.Ns`, `.diag` (the diagonal array)

**Key property:** `sigma_z^2 = I` (involution), verified by tests.

---

### 3.4 Random State Generation

#### `generate_random_states(Ns, num_states=20, seed=42, verbose=True, return_format='list')`

**Purpose:** Generate Haar-random normalized quantum states in C^Ns.

| `return_format` | Returns |
|----------------|---------|
| `'list'` | List of (Ns,) arrays (backward compatible) |
| `'matrix'` | Single (Ns, num_states) array, each column normalized |

**Algorithm:** For each state: sample `Re + i*Im` from N(0,1), normalize to unit norm. Deterministic via `np.random.seed(seed)`.

---

#### `generate_random_states_batched(Ns, num_states, seed, verbose)`

**Purpose:** Convenience wrapper returning `(Ns, num_states)` matrix directly.

---

#### `generate_random_states_generator(Ns, num_states, block_size, seed, verbose)`

**Purpose:** Memory-efficient generator yielding states in blocks of shape `(Ns, B)`. Final block may have fewer than `block_size` columns.

**Memory:** `O(Ns * block_size)` instead of `O(Ns * num_states)`.

---

#### `generate_random_states_indexed(Ns, start_idx, end_idx, seed)`

**Purpose:** Generate a specific range of states `[start_idx, end_idx)` while maintaining deterministic consistency with the full sequence. Useful for parallel/distributed generation.

---

### 3.5 Autocorrelation Computation

#### `compute_ensemble_average_batched(random_states, apply_U, apply_O, n_periods, ...)`

**Purpose:** Core computation of the ensemble-averaged infinite-temperature autocorrelation:

```
C_i(nT) = (1/num_states) * sum_k <psi_k| sigma_z_i(nT) sigma_z_i(0) |psi_k>
```

**Algorithm -- Two-Ket Method:**

For each initial state |psi_0>:
1. Initialize two kets: `|Psi_n> = |psi_0>` and `|Phi_n> = sigma_z |psi_0>`
2. At each step n, compute `C(nT) = <Psi_n| sigma_z |Phi_n>`
3. Evolve both: `|Psi_{n+1}> = U_F |Psi_n>`, `|Phi_{n+1}> = U_F |Phi_n>`

This avoids constructing or storing `sigma_z(nT) = U^n sigma_z U^{-n}` as a matrix.

| Parameter | Type | Description |
|-----------|------|-------------|
| `random_states` | list or (Ns, num_states) array | Initial states |
| `apply_U` | callable | Floquet propagator |
| `apply_O` | callable | Observable operator (typically sigma_z matvec) |
| `n_periods` | int | Number of Floquet periods |
| `batch_size` | int or None | States processed in parallel per block (None = auto, max 50) |
| `store_all` | bool | If True, store individual C(t) traces for each state |
| `return_real` | bool | If True, return Re(C) |

**Returns:**
- `store_all=True`: `(times, C_avg, C_all)` where `C_all` is `(num_states, n_periods+1)`
- `store_all=False`: `(times, C_avg)`

**Invariant:** `C(0) = 1.0` because `<psi| sigma_z^2 |psi> = <psi|I|psi> = 1` for normalized states.

---

#### `test_single_state_autocorrelation(apply_U, apply_O, Ns, n_periods=20, seed=42)`

**Purpose:** Diagnostic test for a single state to verify the two-ket formula produces C(0)=1.

---

### 3.6 Diagnostics

#### `diagnose_evolution(psi0, apply_U, n_test=50, verbose=True)`

**Purpose:** Compute the state overlap `|<psi_0|psi_n>|^2` over n Floquet periods to check for recurrences and verify the evolution is non-trivial.

**Returns:** `overlaps` array of shape `(n_test+1,)`.

**Expected behavior:** Overlaps decay rapidly to near zero for ergodic evolution, saturate above zero in fragmented sectors.

---

### 3.7 Floquet Spectrum Analysis

#### `analyze_floquet_spectrum_dense(ising_chain, verbose=True)`

**Purpose:** Full eigenspectrum of U_F via dense diagonalization. Only for L <= 12.

**Returns:** `(eigenvalues, eigenphases)` where eigenvalues should lie on the unit circle and `eigenphases = angle(eigenvalues)`.

---

#### `analyze_floquet_spectrum_sampled(ising_chain, n_samples=1000, n_moments=4, ...)`

**Purpose:** Sampling-based spectral analysis for large systems. Estimates:
1. Average survival probability `P(n) = <|<psi_0|U^n|psi_0>|^2>`
2. Spectral form factor estimate `K(n) ~ Ns * P(n)`
3. Norm preservation (unitarity check)

Works for any L since it only uses Krylov matvecs.

---

#### `analyze_floquet_spectrum(ising_chain, method='auto', **kwargs)`

**Purpose:** Unified interface that auto-selects dense (L <= 10) or sampled (L > 10) method.

---

### 3.8 Parameter Scan Functions

#### `run_autocorr_coupling_comparison(L, site_i, g, J_0_values, h_0_relation, NTP, n_steps, num_states, ...)`

**Purpose:** Scan autocorrelation across different coupling strengths J_0. The period T = NTP * pi/J_0 varies with J_0.

| Parameter | Description |
|-----------|-------------|
| `J_0_values` | List of J_0 values to scan (e.g., `[1, 10, 20, 40]`) |
| `h_0_relation` | `'2*J_0'` or `'J_0'` or explicit list of h_0 values |
| `NTP` | Period factor: `T = NTP * pi / J_0` |

**Returns:** `(results_dict, base_params)` where `results_dict[J_0]` contains `{'times', 'C_avg', 'C_all', 'J_0', 'h_0', 'T', 'ratio'}`.

**Default parameters used in notebook:**
```python
L = 12, J_0_list = [1, 10, 20, 40], h_0_list = [2, 20, 80, 40]
g = 1.0, NTP = 1, site_i = L//2, n_steps = 200, num_states = 20
```

---

#### `run_autocorr_site_comparison(L, J_0, h_0, g, period_factor, edge_sites, bulk_sites, n_periods, num_states, ...)`

**Purpose:** Scan autocorrelation across different sites (edge vs bulk) for a single set of Hamiltonian parameters.

| Parameter | Description |
|-----------|-------------|
| `period_factor` | Period factor n where `T = n * pi / J_0` |
| `edge_sites` | Site indices for edges (default: `[0, L-1]`) |
| `bulk_sites` | Site indices for bulk (default: `[L//2-1, L//2]`) |

**Returns:** `(results_dict, base_params)` where `results_dict[site_i]` contains `{'times', 'C_avg', 'site_type', 'site_label', 'position'}`.

**In the notebook this is run twice:**
1. `period_factor = 1` (T = pi/J_0)
2. `period_factor = 2` (T = 2*pi/J_0) -- this one shows edge memory

---

### 3.9 Hilbert Space Fragmentation Analysis

#### `build_O_operator(L, J0)`

**Purpose:** Construct the diagonal operator `O = J0 * sum sigma_z_i sigma_z_{i+1} + 2*J0 * sum sigma_z_i` used to identify Hilbert space fragments.

**Returns:** `O_diag` -- ndarray of shape `(2**L,)` with diagonal elements.

**Physical meaning:** O is the unperturbed part of the Hamiltonian in the sigma_z basis. States with the same O eigenvalue are resonant and can be connected by the perturbation g*sigma_x.

---

#### `build_connectivity_graph(L, O_diag, tol=1e-10)`

**Purpose:** Build the graph where an edge (n, m) exists if states n and m:
1. Differ by exactly one spin flip (single sigma_x action)
2. Have `|O_n - O_m| < tol` (resonance condition: `P_nm = 0`)

**Returns:** `(adjacency, num_edges)` where `adjacency[n]` is the list of neighbors of state n.

---

#### `find_connected_components(adjacency)`

**Purpose:** Find all connected components using BFS. Each component is a Hilbert space fragment.

**Returns:** `components` -- list of lists, each containing the state indices in that fragment.

---

#### `get_fragment_statistics(components)`

**Purpose:** Compute statistics: number of fragments, size distribution, max/min size, number of singletons.

---

#### `find_largest_fragment(components)`

**Purpose:** Return the largest fragment (the connected component with most states).

---

#### `get_spin_config(state_idx, L)`

**Purpose:** Convert state index to spin string using QuSpin convention (bit 0 = UP, bit 1 = DOWN).

---

#### `get_upup_bond_pattern(state_idx, L)`

**Purpose:** For OBC, return a length-(L-1) array where entry i = 1 if bond (i, i+1) is both spins UP.

---

#### `get_N_defect(state_idx, L)`

**Purpose:** Count the number of UP-UP bonds ("defects") in a configuration.

---

#### `display_fragment_states(...)` and `display_all_fragments(...)`

**Purpose:** Print detailed tables of fragment states with their spin configs, O values, N_defect, bond patterns, and neighbors.

---

#### `create_fragment_summary_table(...)` and `print_fragment_summary_table(...)`

**Purpose:** Create and display a compact summary of all fragments with bond pattern analysis.

---

#### `fib(n)`

**Purpose:** Compute the n-th Fibonacci number. Used to verify that the largest fragment size equals F_L (Fibonacci number at position L), which counts configurations on a linear chain with no adjacent up-up bonds.

---

### 3.10 Data Persistence

#### `save_autocorr_results(results_dict, base_params, filename, save_dir, format, verbose)`

**Purpose:** Save computation results for later plotting. Supports `.npz` (compressed numpy) and `.pkl` (pickle) formats.

**Auto-generated filename pattern:** `autocorr_L{L}_{method}_{timestamp}.{ext}`

---

#### `load_autocorr_results(filepath, verbose)`

**Purpose:** Load previously saved results. Returns `(results_dict, base_params)`.

---

#### `download_file_colab(filepath)`

**Purpose:** Trigger file download in Google Colab environment.

---

### 3.11 Plotting Functions

#### `plot_autocorr_coupling_comparison(results_dict, base_params)`

**Purpose:** Publication-quality 3-panel figure comparing autocorrelation across coupling strengths:
- Panel (a): C(nT) vs Floquet periods for all J_0 values
- Middle: Legend panel + early-time dynamics inset
- Panel (b): Statistics table

---

#### `plot_autocorr_site_comparison(results_dict, base_params)`

**Purpose:** Publication-quality 3-panel figure comparing autocorrelation across sites:
- Edge sites (solid lines, red/blue) vs bulk sites (dashed lines, green/purple)
- Shows edge memory preservation vs bulk thermalization

---

## 4. Validation Helpers

#### `assert_state_vector(v, Ns, name)`

**Purpose:** Validate shape `(Ns,)` and dtype `complex128`.

#### `assert_operator_shape(op, Ns, name)`

**Purpose:** Validate shape `(Ns, Ns)` for operators (works with sparse, dense, and LinearOperator).

---

## 5. Key Results from the Notebook

### 5.1 Autocorrelation vs Coupling Strength

At T = pi/J_0 with site_i = L//2 (bulk):
- **J_0 = 1 (weak coupling):** C(nT) decays rapidly to zero (thermalizes)
- **J_0 >= 10 (strong coupling):** C(nT) saturates to non-zero plateau (ETH violation)
- Higher J_0 leads to higher saturation values

### 5.2 Autocorrelation vs Site (Edge vs Bulk)

At T = 2*pi/J_0 with J_0 = 10, h_0 = 20:
- **Bulk sites (i = L/2):** C(nT) saturates to moderate non-zero value
- **Edge sites (i = 0, L-1):** C(nT) remains near 1.0 for ~300 Floquet cycles (prethermal edge memory)

At T = pi/J_0:
- Both edge and bulk show non-zero saturation but no enhanced edge memory

### 5.3 Hilbert Space Fragmentation (L = 10, OBC)

- The Hilbert space (1024 states) breaks into many disconnected fragments
- **Largest fragment** has dimension equal to the L-th Fibonacci number F_L
- Largest fragment corresponds to states with **no adjacent up-up bonds** (N_defect = 0)
- Singletons exist (isolated states with no resonant single-flip neighbors)
- Fragment sizes follow a distribution related to the number of frozen up-up bond patterns

### 5.4 Computational Performance

- L = 12 (Ns = 4096) with 200 Floquet steps and 20 ensemble states: **~2.5 hours**
- Batch processing (batch_size = 10) balances memory and speed
- Krylov tolerance of 1e-10 maintains unitarity to machine precision

---

## 6. Standard Parameter Sets Used

### Set A: Basic Test (L = 8)
```python
L = 8, J_0 = 10.0, h_0 = 20.0, g = 1.0
TP = 2*pi/J_0, site_i = 4, n_periods = 200, num_states = 1
```

### Set B: Coupling Scan (L = 12)
```python
L = 12, J_0_list = [1, 10, 20, 40], h_0_list = [2, 20, 80, 40]
g = 1.0, NTP = 1, site_i = L//2, n_steps = 200, num_states = 20, batch_size = 10
```

### Set C: Site Comparison (L = 12)
```python
L = 12, J_0 = 10.0, h_0 = 20.0, g = 1.0
period_factor = 1 (and 2), n_periods = 200, num_states = 20, batch_size = 10
edge_sites = [0, L-1], bulk_sites = [L//2-1, L//2]
```

### Set D: Fragmentation Analysis (L = 10)
```python
L_frag = 10, J0_frag = 10.0
Boundary conditions: OPEN
```

---

## 7. Notebook Section Map

| Section | Cells | Topic |
|---------|-------|-------|
| Model description | 0-2 | Physics, Hamiltonian, methodology overview |
| QuSpin install | 3 | `pip install quspin` |
| Hamiltonian construction | 4-6 | `FloquetIsingChain` class, sparse H+/H- |
| Time evolution | 7-8 | Krylov `evolve_state`, `apply_floquet_period`, `make_floquet_propagator` |
| Spin operators | 9-11 | `get_sigma_z_diagonal`, `make_sigma_z_matvec` |
| Random states | 12-13 | `generate_random_states` (list/matrix/generator/indexed modes) |
| Evolution diagnostics | 14-15 | `diagnose_evolution`, overlap `|<psi_0|psi_n>|^2` |
| Spectrum analysis | 16-17 | `analyze_floquet_spectrum` (dense/sampled) |
| Autocorrelation core | 18-19 | `compute_ensemble_average_batched` (two-ket method) |
| C(t) vs coupling | 20-31 | `run_autocorr_coupling_comparison`, save/load, plotting |
| C(t) vs site | 32-51 | `run_autocorr_site_comparison`, save/load, plotting |
| C(t) vs driving period | 52-63 | Period comparison (T = pi/J_0 vs 2*pi/J_0) |
| Fragmentation analysis | 64-72 | `build_O_operator`, connectivity graph, fragments |
| HSF functions | 73-85 | Full fragmentation pipeline with bond pattern analysis |

---

## 8. Glossary

| Symbol/Term | Definition |
|-------------|-----------|
| L | Number of sites in the spin chain |
| Ns = 2^L | Hilbert space dimension |
| J_0 | Ising coupling amplitude |
| h_0 | Longitudinal field amplitude |
| g | Transverse field (perturbation) strength |
| T, TP | Driving period |
| NTP | Period factor: T = NTP * pi / J_0 |
| H+ | Hamiltonian for first half-period (positive couplings) |
| H- | Hamiltonian for second half-period (negative couplings) |
| U_F | Floquet unitary (one-period propagator) |
| H_F | Floquet Hamiltonian (effective time-independent Hamiltonian) |
| C_i(nT) | Infinite-temperature autocorrelation of sigma_z at site i after n periods |
| O | Diagonal operator whose eigenvalues define resonance conditions |
| P_nm | Eigenvalue difference O_n - O_m (resonance parameter) |
| pi_i | Projector onto spin-down at site i: (I - sigma_z_i) / 2 |
| N_defect | Number of adjacent up-up bonds in a spin configuration |
| HSF | Hilbert Space Fragmentation |
| ETH | Eigenstate Thermalization Hypothesis |
| OBC | Open Boundary Conditions |
| PBC | Periodic Boundary Conditions |
| Haar measure | Uniform distribution on the unit sphere in C^Ns (for random states) |
| Two-ket method | Algorithm evolving both |psi> and sigma_z|psi> to compute C(nT) without constructing sigma_z(nT) |
| Krylov subspace | Iterative method for computing exp(-iHt)|psi> without matrix exponentiation |
| CSR | Compressed Sparse Row (sparse matrix format optimal for matvec) |
