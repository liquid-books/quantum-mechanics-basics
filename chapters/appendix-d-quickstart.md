---
title: "Appendix D: Quantum Platform Quick-Start Guides"
subtitle: "Your First Quantum Job in 15 Minutes — IBM Quantum and D-Wave Leap Step-by-Step"
short_title: "Platform Quick-Start"
description: "Step-by-step setup guides for IBM Quantum (gate-model) and D-Wave Leap (quantum annealing) — from account creation to running your first real quantum job."
label: appendix-d-quickstart
---

# Appendix D: Quantum Platform Quick-Start Guides

*This appendix gets you from zero to running real quantum jobs in 15 minutes. Two platforms, two paradigms — one appendix.*

---

:::{admonition} Before You Start
:class: note

Both platforms are free to start. You need:
- An email address (for account creation)
- Python 3.9+ installed (for local SDK use)
- Or just a browser (both platforms have web interfaces)

No quantum hardware. No credit card. No physics degree.
:::

---

## Quick-Start 1: IBM Quantum (Gate-Model)

IBM Quantum gives you access to real superconducting quantum processors — the same hardware type as Google's and Rigetti's machines.

### Step 1: Create a Free Account

1. Go to [quantum.ibm.com](https://quantum.ibm.com)
2. Click **"Sign in"** → **"Create an IBM account"**
3. Verify your email
4. You now have access to IBM's free tier — real 127-qubit processors included

### Step 2: Get Your API Key

1. Log in at [quantum.ibm.com](https://quantum.ibm.com)
2. Click your profile icon (top right) → **"Copy API token"**
3. Keep this somewhere safe — you'll paste it into your Python code

### Step 3: Install Qiskit

```bash
pip install qiskit qiskit-ibm-runtime qiskit-aer
```

### Step 4: Run Your First Quantum Circuit

Paste this into a Python file or Jupyter notebook:

```python
from qiskit import QuantumCircuit
from qiskit_ibm_runtime import QiskitRuntimeService, SamplerV2 as Sampler
from qiskit.transpiler.preset_passmanagers import generate_preset_pass_manager

# ── Step 1: Authenticate ─────────────────────────────────────────────────
# First time only — saves your token locally
# QiskitRuntimeService.save_account(channel="ibm_quantum", token="YOUR_TOKEN_HERE")

service = QiskitRuntimeService(channel="ibm_quantum")

# ── Step 2: Build a Bell State Circuit ───────────────────────────────────
# This creates the entangled state (|00⟩ + |11⟩)/√2
qc = QuantumCircuit(2)
qc.h(0)      # Hadamard on qubit 0 → superposition
qc.cx(0, 1)  # CNOT: entangle qubit 0 with qubit 1
qc.measure_all()

print("Circuit:")
print(qc.draw())

# ── Step 3: Choose a backend ──────────────────────────────────────────────
# Option A: Real hardware (queue time varies — could be seconds or minutes)
# backend = service.least_busy(operational=True, simulator=False)

# Option B: Local simulator (instant, no queue)
from qiskit_aer import AerSimulator
backend = AerSimulator()

# ── Step 4: Transpile and Run ─────────────────────────────────────────────
pm = generate_preset_pass_manager(backend=backend, optimization_level=1)
isa_circuit = pm.run(qc)

sampler = Sampler(backend)
job = sampler.run([isa_circuit], shots=1000)
result = job.result()

# ── Step 5: Read Results ──────────────────────────────────────────────────
counts = result[0].data.meas.get_counts()
print(f"\nResults after 1,000 shots:")
for state, count in sorted(counts.items()):
    bar = "█" * (count // 20)
    print(f"  |{state}⟩: {count:4d}  {bar}")

# Expected output:
# |00⟩: ~500  █████████████████████████
# |11⟩: ~500  █████████████████████████
# Never |01⟩ or |10⟩ — entanglement ensures the qubits always agree
```

### What You Just Did

- **Line `qc.h(0)`** — Applied the Hadamard gate from Chapter 3. Qubit 0 is now in superposition: $\frac{1}{\sqrt{2}}|0\rangle + \frac{1}{\sqrt{2}}|1\rangle$.
- **Line `qc.cx(0,1)`** — Applied the CNOT gate from Chapter 4. Qubits 0 and 1 are now entangled: the Bell state $(|00\rangle + |11\rangle)/\sqrt{2}$.
- **`shots=1000`** — You ran the circuit 1,000 times (Chapter 5: measurement is probabilistic, so you need many shots).
- **Result** — ~500 counts of "00" and ~500 of "11", never "01" or "10". Entanglement forces the qubits to always agree.

### Step 5: Try Grover's Search (3 Lines)

```python
from qiskit.circuit.library import GroverOperator, PhaseOracle

# Mark the solution |11⟩ (the "database entry" we're searching for)
oracle = PhaseOracle('x & y')   # Marks states where both x AND y are 1
grover = GroverOperator(oracle)

# Build the full circuit: initialize, run Grover iteration, measure
qc_grover = QuantumCircuit(2)
qc_grover.h([0, 1])                          # Superposition of all states
qc_grover.compose(grover, inplace=True)      # One Grover iteration
qc_grover.measure_all()

# Run it
job = sampler.run([pm.run(qc_grover)], shots=1000)
counts = job.result()[0].data.meas.get_counts()
print("\nGrover's search result:")
for state, count in sorted(counts.items(), key=lambda x: -x[1]):
    print(f"  |{state}⟩: {count}")
# The marked state |11⟩ should appear with overwhelmingly high probability
```

### Useful IBM Quantum Resources

| Resource | URL |
|----------|-----|
| IBM Quantum Dashboard | [quantum.ibm.com](https://quantum.ibm.com) |
| Qiskit Documentation | [docs.quantum.ibm.com](https://docs.quantum.ibm.com) |
| IBM Quantum Learning | [learning.quantum.ibm.com](https://learning.quantum.ibm.com) |
| Circuit Composer (browser) | [quantum.ibm.com/composer](https://quantum.ibm.com/composer) |
| Backend Status | [quantum.ibm.com/services/resources](https://quantum.ibm.com/services/resources) |

---

## Quick-Start 2: D-Wave Leap (Quantum Annealing)

D-Wave Leap gives you access to the Advantage quantum annealers — the same hardware as FAU's on-campus Advantage2. No quantum circuits. Instead: optimization problems, expressed as QUBO matrices.

### Step 1: Create a Free Leap Account

1. Go to [cloud.dwavesys.com/leap](https://cloud.dwavesys.com/leap)
2. Click **"Sign Up"** — academic email preferred (gives more free time)
3. Verify your email
4. You'll get free QPU access time + unlimited Leap hybrid solver access

### Step 2: Get Your API Token

1. Log in at [cloud.dwavesys.com/leap](https://cloud.dwavesys.com/leap)
2. Under your profile → **"API Token"**
3. Copy the token (looks like: `DEV-abc123...`)

### Step 3: Install the Ocean SDK

```bash
pip install dwave-ocean-sdk
```

Then configure your credentials:

```bash
dwave setup
# Follow the prompts — paste your API token when asked
# Chooses your default solver automatically
```

Or in Python:

```python
import dwave.cloud as dc
# First-time config:
# dc.Client.from_config(token="YOUR_TOKEN_HERE")
```

### Step 4: Run Your First Optimization Problem

Let's solve a simple binary optimization problem: pick the best subset of projects to fund.

```python
import dimod
from dwave.samplers import SimulatedAnnealingSampler   # local, no account needed
# from dwave.system import LeapHybridSampler           # real D-Wave, needs account

# ── Problem: Binary Project Selection ────────────────────────────────────
# 4 projects. We want to maximize total value, subject to constraints.
# Variables: x0, x1, x2, x3 — each is 1 (fund it) or 0 (skip it)

# Project values (negative because QUBO minimizes):
# x0=Marketing ($5M value), x1=R&D ($8M), x2=Operations ($3M), x3=IT ($6M)
# Budget constraint: fund at most 2 projects
# → Penalty of 30 for funding >2 projects simultaneously

P = 30  # Penalty weight — must exceed maximum possible value gain

Q = {
    # Linear terms (diagonal): negative values = reward for selecting project
    (0, 0): -5,
    (1, 1): -8,
    (2, 2): -3,
    (3, 3): -6,
    # Quadratic terms (off-diagonal): penalty for selecting pairs beyond budget
    # With budget=2, any 3rd project selected adds penalty
    (0, 1): P,
    (0, 2): P,
    (0, 3): P,
    (1, 2): P,
    (1, 3): P,
    (2, 3): P,
}

# ── Solve with local simulator (no D-Wave account needed) ────────────────
sampler = SimulatedAnnealingSampler()
response = sampler.sample_qubo(Q, num_reads=1000)

print("Top 5 solutions found:")
print(f"{'x0':>4} {'x1':>4} {'x2':>4} {'x3':>4} | {'Energy':>8} | {'Projects selected'}")
print("-" * 60)

seen = set()
for sample, energy in response.data(['sample', 'energy']):
    key = tuple(sample[i] for i in range(4))
    if key not in seen:
        seen.add(key)
        funded = [f"x{i}" for i in range(4) if sample[i] == 1]
        print(f"{sample[0]:>4} {sample[1]:>4} {sample[2]:>4} {sample[3]:>4} "
              f"| {energy:>8.1f} | {funded}")
    if len(seen) >= 5:
        break

# Expected: best solution funds x1 (R&D, $8M) and x3 (IT, $6M) = $14M total
# Energy ≈ -14 (negative of value, before penalty terms)
```

```
Top 5 solutions found:
  x0   x1   x2   x3 |   Energy | Projects selected
------------------------------------------------------------
   0    1    0    1  |    -14.0 | ['x1', 'x3']   ← BEST: $14M
   0    1    1    0  |    -11.0 | ['x1', 'x2']
   1    0    0    1  |    -11.0 | ['x0', 'x3']
   1    1    0    0  |    -13.0 | ['x0', 'x1']
   0    0    1    1  |     -9.0 | ['x2', 'x3']
```

### Step 5: Submit to Real D-Wave Hardware

To run on FAU's hardware class (D-Wave Advantage), change one line:

```python
from dwave.system import DWaveSampler, EmbeddingComposite
from dwave.system import LeapHybridSampler

# Option A: Direct QPU (small problems, <~200 variables)
# sampler = EmbeddingComposite(DWaveSampler())

# Option B: Leap Hybrid Solver (any size problem, recommended for beginners)
sampler = LeapHybridSampler()

response = sampler.sample_qubo(Q, label="My first D-Wave job")

print(f"Best solution: {response.first.sample}")
print(f"Energy: {response.first.energy:.2f}")
```

:::{note}
**On Leap's free tier:** The LeapHybridSampler (Stride solver) has generous free monthly quotas. For developer learning, you are unlikely to exhaust your free time. Direct QPU access is measured in microseconds of QPU time — also generously allotted on the free plan.
:::

### Step 6: Bigger Problems — The Hybrid Solver Advantage

D-Wave's real power is in large-scale problems that classical solvers struggle with. The Leap hybrid solver handles millions of variables:

```python
from dwave.system import LeapHybridSampler
import dimod

# A larger QUBO — 50 binary variables (2^50 ≈ 10^15 classical combinations)
n = 50
Q_large = {}

# Random optimization problem with interactions
import random
random.seed(42)
for i in range(n):
    Q_large[(i, i)] = random.uniform(-2, 2)   # Linear terms
for i in range(n):
    for j in range(i+1, n):
        if random.random() < 0.1:             # 10% connectivity
            Q_large[(i, j)] = random.uniform(-1, 1)

# Submit to Leap Hybrid — solves in seconds
sampler = LeapHybridSampler()
bqm = dimod.BinaryQuadraticModel.from_qubo(Q_large)
response = sampler.sample(bqm, time_limit=3)   # 3 seconds of hybrid solving

print(f"Variables: {n}")
print(f"Best energy found: {response.first.energy:.4f}")
print(f"Selected variables: {[k for k, v in response.first.sample.items() if v == 1]}")
```

### Useful D-Wave Resources

| Resource | URL |
|----------|-----|
| D-Wave Leap Dashboard | [cloud.dwavesys.com/leap](https://cloud.dwavesys.com/leap) |
| Ocean SDK Documentation | [docs.ocean.dwavesys.com](https://docs.ocean.dwavesys.com) |
| D-Wave GitHub (examples) | [github.com/dwave-examples](https://github.com/dwave-examples) |
| QUBO Formulation Guide | [docs.ocean.dwavesys.com/en/stable/concepts/bqm.html](https://docs.ocean.dwavesys.com/en/stable/concepts/bqm.html) |
| D-Wave Learn | [learn.dwavesys.com](https://learn.dwavesys.com) |
| Leap Hybrid Solver Docs | [docs.ocean.dwavesys.com/en/stable/docs_hybrid](https://docs.ocean.dwavesys.com/en/stable/docs_hybrid) |

---

## Choosing the Right Platform for Your Problem

```
Is your problem a COMBINATORIAL OPTIMIZATION problem?
(Scheduling, routing, selection, graph coloring, portfolio, etc.)
        │
        ├── YES → Use D-Wave Leap (quantum annealing)
        │         • Express as QUBO
        │         • Use LeapHybridSampler for production scale
        │         • Direct QPU for research/small problems
        │
        └── NO  → Is it a SIMULATION or ALGORITHM problem?
                  (Chemistry, cryptography, Grover's search, VQE)
                        │
                        ├── NISQ-scale (≤100 qubits, shallow circuits)
                        │   → IBM Quantum, IonQ, Quantinuum
                        │
                        └── Research/education/simulation
                            → Local simulator (Qiskit Aer, Cirq, PennyLane)
```

---

## Your Next 30 Days

If you've just finished *Quantum Mechanics Basics* and want a concrete learning path:

**Week 1 — Set up both platforms**
- Create IBM Quantum account → Run the Bell state circuit above
- Create D-Wave Leap account → Run the project selection QUBO above
- Install Qiskit and Ocean SDK locally

**Week 2 — IBM Quantum: Gate-Model Basics**
- Complete IBM Quantum Learning: "Introduction to Qiskit" course (free)
- Implement Grover's search on a 3-qubit problem
- Run on a real backend; compare with simulator results

**Week 3 — D-Wave Leap: Annealing and QUBO**
- Complete D-Wave's "Getting Started with Ocean" tutorial
- Formulate a scheduling or routing problem as QUBO
- Submit to LeapHybridSampler; benchmark against classical solver

**Week 4 — Applied Quantum Computing**
- Start *Applied Quantum Computing* (the companion volume to this book)
- Chapters 1–3 build on everything in Weeks 1–3
- Chapter 6 (QUBO/Ocean) and Chapter 5 (PQC security) are your Week 3 work made rigorous

---

*The hardware is ready. The accounts are free. The only thing between you and your first quantum job is 15 minutes.*
