---
title: "Appendix C: Further Reading & Free Resources"
subtitle: "The Best Books, Courses, Free Quantum Computers, and Communities — Curated for Programmers"
short_title: "Further Reading & Resources"
description: "A curated guide to quantum mechanics and quantum computing resources — free online quantum computers, textbooks, video courses, simulators, and communities — organized by level and topic."
label: appendix-c-resources
---

# Appendix C: Further Reading & Free Resources

*You've finished the book. Now what? This appendix is your map to everything that comes next — free online quantum computers you can use today, the best textbooks at every level, video courses, simulators, and communities.*

---

## Free Quantum Computers You Can Use Right Now

These platforms give you real or simulated quantum hardware access — for free.

:::{admonition} Start Here
:class: tip

You don't need to install anything to try quantum computing. IBM Quantum and D-Wave Leap both have browser-based interfaces. Create a free account and run your first quantum job in 15 minutes.
:::

---

### 🔵 IBM Quantum — Gate-Model Quantum Computers

**URL:** [quantum.ibm.com](https://quantum.ibm.com)

IBM's free tier gives you access to real superconducting quantum processors (Eagle, Heron) through their browser-based composer and Python SDK (Qiskit). No credit card required.

**What you can do free:**
- Write and run quantum circuits on real 127+ qubit processors
- Use IBM's circuit composer (drag-and-drop gates)
- Access dozens of real backends plus high-fidelity simulators
- Run Grover's search, Bell state experiments, VQE on real hardware

**Best for:** Learning gate-model quantum computing; writing and running Qiskit circuits; implementing algorithms from Chapter 8.

---

### 🟢 D-Wave Leap — Quantum Annealing in the Cloud

**URL:** [cloud.dwavesys.com/leap](https://cloud.dwavesys.com/leap)

D-Wave's free developer plan provides real QPU access (D-Wave Advantage systems) plus full access to the Leap hybrid solver (Stride/LeapHybridSampler). This is the same hardware class as FAU's on-campus Advantage2.

**What you can do free:**
- Submit QUBO/Ising problems to D-Wave Advantage hardware
- Use the LeapHybridSampler (Stride) for large optimization problems
- Access Ocean SDK tutorials, Jupyter notebooks, and problem examples
- Run combinatorial optimization: scheduling, routing, portfolio selection

**Best for:** Quantum annealing; QUBO formulation; optimization problems; the skills from Chapter 6 of this book and *Applied Quantum Computing* Chapters 6–9.

**Why it matters locally:** D-Wave is relocating its US headquarters to Boca Raton — minutes from FAU. The Advantage2 on FAU's campus is the same system you're accessing through Leap. This is your on-ramp to the local quantum ecosystem.

---

### 🟡 Google Quantum AI — Cirq and Quantum Computing Service

**URL:** [quantumai.google](https://quantumai.google) | [Cirq](https://quantumai.google/cirq)

Google's Cirq SDK is free and open-source. The Quantum Computing Service (QCS) provides access to Google's Sycamore processor, though free access is limited to research collaborations. Cirq simulators are fully free.

**What you can do free:**
- Simulate quantum circuits up to ~30 qubits with Cirq
- Access Google's quantum programming tutorials
- Apply for research access to Sycamore hardware

**Best for:** Gate-model simulation; researchers; those interested in Google's noise models and error characterization.

---

### 🟠 Amazon Braket — Multi-Cloud Quantum Access

**URL:** [aws.amazon.com/braket](https://aws.amazon.com/braket)

AWS Braket provides a unified interface to multiple quantum hardware providers (IonQ, Rigetti, QuEra, and others) plus on-demand simulators. The first month includes free simulator time; hardware access is pay-per-shot.

**What you can do free:**
- 12 months free tier includes limited simulator time
- Local simulator runs are free (in your own Python environment)
- Access to managed Jupyter notebook environments

**Best for:** Multi-vendor exploration; trying trapped-ion (IonQ) and neutral atom (QuEra) hardware; AWS-native quantum workflows.

---

### 🔴 IonQ — Trapped-Ion Quantum Computing

**URL:** [cloud.ionq.com](https://cloud.ionq.com)

IonQ's trapped-ion processors achieve the highest gate fidelities of any commercially available quantum computer. Access is available through Amazon Braket and Microsoft Azure Quantum, as well as direct API access.

**What you can do free:**
- Free simulator (IonQ Harmony emulator) via Braket or direct API
- Limited free hardware credits through academic programs
- Access through Microsoft Azure Quantum free trial

**Best for:** High-fidelity gate operations; comparing superconducting vs. trapped-ion hardware; the hardware comparison discussion in Chapter 4 of *Applied Quantum Computing*.

---

### 🟣 Microsoft Azure Quantum — Hybrid Quantum Platform

**URL:** [azure.microsoft.com/en-us/products/quantum](https://azure.microsoft.com/en-us/products/quantum)

Azure Quantum provides access to IonQ, Quantinuum, and Rigetti hardware plus Microsoft's own quantum simulators. Free credits available for new accounts.

**What you can do free:**
- $500 free Azure credits for new accounts
- Free quantum simulator access (resource estimator)
- Q# language tutorials and development environment

**Best for:** Microsoft's Q# language; quantum resource estimation; integrated Azure cloud workflows.

---

### ⚪ Quantinuum H-Series — Highest-Fidelity Trapped-Ion

**URL:** [www.quantinuum.com](https://www.quantinuum.com)

Quantinuum (formerly Honeywell Quantum Solutions) operates the H1 and H2 trapped-ion processors with all-to-all qubit connectivity and best-in-class two-qubit gate fidelities (>99.8%). Access via Microsoft Azure Quantum.

**What you can do free:**
- Access through Azure Quantum free credits
- TKET SDK (Quantinuum's circuit compiler) — free and open source
- Quantum chemistry and error correction research programs

---

### 🔵 QuEra — Neutral Atom Quantum Computing

**URL:** [www.quera.com](https://www.quera.com) | Access via [Amazon Braket](https://aws.amazon.com/braket)

QuEra's Aquila processor uses neutral rubidium atoms in reconfigurable optical tweezer arrays — a fundamentally different architecture than superconducting or trapped-ion. Supports analog quantum simulation.

**What you can do free:**
- Bloqade simulator (open source, runs locally) — free
- Aquila hardware access via Amazon Braket (pay-per-shot, but small jobs are cheap)
- Analog quantum simulation of strongly correlated systems

---

## Free Online Courses and Tutorials

### Quantum Computing

**IBM Quantum Learning**
: [learning.quantum.ibm.com](https://learning.quantum.ibm.com) — IBM's comprehensive free course platform. Covers Qiskit from basics to advanced algorithms. Interactive circuits, real hardware exercises, certificates. The best structured quantum computing course available for free.

**Qiskit Textbook (Open Source)**
: [qiskit.org/learn](https://qiskit.org/learn) — The full open-source interactive textbook. Covers linear algebra review, quantum circuits, algorithms, error correction, and quantum machine learning.

**D-Wave Ocean SDK Tutorials**
: [docs.ocean.dwavesys.com](https://docs.ocean.dwavesys.com) — Official D-Wave tutorials for QUBO formulation, hybrid solving, and real hardware submission. Includes worked examples in logistics, scheduling, and portfolio optimization.

**MIT OpenCourseWare: Quantum Computation**
: [ocw.mit.edu](https://ocw.mit.edu) → Search "quantum computation" — MIT's 8.370/18.435 course materials, lecture notes, and problem sets. Free, rigorous, graduate-level.

**Microsoft's Quantum Katas**
: [github.com/microsoft/QuantumKatas](https://github.com/microsoft/QuantumKatas) — Self-paced programming exercises in Q# covering all quantum computing fundamentals. Free, open source, browser-runnable.

**Quantum Country (Andy Matuschak & Michael Nielsen)**
: [quantum.country](https://quantum.country) — A beautifully written introduction to quantum computing using spaced repetition. Free. Covers quantum circuits through Grover's algorithm. Ideal for this book's audience.

### Quantum Mechanics (Physics)

**MIT OCW: 8.04 Quantum Physics I**
: [ocw.mit.edu](https://ocw.mit.edu) → 8.04 — Full undergraduate quantum mechanics lecture videos, notes, and problem sets. Free.

**The Feynman Lectures on Physics (Volume III)**
: [feynmanlectures.caltech.edu](https://www.feynmanlectures.caltech.edu/III_toc.html) — Richard Feynman's quantum mechanics lectures, available free online in full. The most beautifully written physics exposition ever produced.

**3Blue1Brown: Quantum Mechanics Series**
: [youtube.com/@3blue1brown](https://www.youtube.com/@3blue1brown) — Visual, mathematically rigorous explainers. The "Essence of Linear Algebra" series is the ideal companion to Appendix B of this book.

---

## Essential Textbooks

### For Programmers and Beginners

**"Quantum Computing: An Applied Approach" — Jack Hidary**
: Pragmatic, code-first introduction covering Qiskit, Cirq, and D-Wave Ocean. Exactly the right level for readers of this book who want to go deeper into implementation.

**"Programming Quantum Computers" — Johnston, Harrigan, Gimeno-Segovia (O'Reilly)**
: Hands-on quantum programming with worked examples in multiple languages. Good for those who learn by doing.

**"Quantum Computation and Quantum Information" — Nielsen & Chuang**
: *The* graduate textbook. Comprehensive and rigorous — covers everything from complexity theory to error correction to quantum cryptography. Dense but worth owning. Available free as a PDF from various academic sources.

### For Physics Depth

**"Introduction to Quantum Mechanics" — David Griffiths**
: The standard undergraduate physics textbook. Clear writing, excellent problems. Read this if you want the full wavefunction formalism beyond what this book covers.

**"Principles of Quantum Mechanics" — R. Shankar**
: Graduate-level, very well written. Builds quantum mechanics from linear algebra up — natural complement to Appendix B of this book.

**"The Feynman Lectures on Physics, Vol. III" — Feynman, Leighton, Sands**
: (Free online, see above.) Feynman's approach to quantum mechanics is unique — physical intuition first, formalism second. Beautifully written.

---

## Communities and Stay Current

**IBM Quantum Network**
: [quantum.ibm.com/network](https://quantum.ibm.com/network) — Join IBM's quantum computing community. Access to hardware, research collaborations, and industry partnerships.

**D-Wave Developer Community**
: [support.dwavesys.com/hc/en-us/community](https://support.dwavesys.com/hc/en-us/community) — Forums, example code, and community Q&A for D-Wave Ocean SDK and Leap platform.

**Quantum Computing Stack Exchange**
: [quantumcomputing.stackexchange.com](https://quantumcomputing.stackexchange.com) — The best Q&A community for quantum computing questions at all levels. Highly recommended.

**arXiv Quantum Physics (quant-ph)**
: [arxiv.org/list/quant-ph/recent](https://arxiv.org/list/quant-ph/recent) — Preprints of new quantum physics and quantum computing research, posted daily. Free. Where the field publishes first.

**Quantum Computing Report**
: [quantumcomputingreport.com](https://quantumcomputingreport.com) — Industry news, hardware comparisons, and market analysis. The most reliable source for tracking vendor claims and milestone announcements.

---

## Simulators You Can Run Locally

These run on your laptop — no account, no cloud, no hardware access required:

```bash
# Install the main quantum computing simulators
pip install qiskit qiskit-aer         # IBM's SDK + statevector simulator
pip install cirq                       # Google's circuit framework
pip install pennylane                  # Differentiable quantum computing (for QML)
pip install dwave-ocean-sdk            # D-Wave Ocean (includes local classical solver)
pip install qutip                      # QuTiP: quantum optics and open systems
pip install pyquil                     # Rigetti's SDK + QVM simulator
```

**Qiskit Aer** — Simulates up to ~30 qubits with exact statevector; noise models for realistic NISQ simulation. Best general-purpose simulator.

**PennyLane** — Differentiable quantum circuits; best for quantum machine learning research. Interfaces with all major hardware backends.

**D-Wave's SimulatedAnnealingSampler** — Runs quantum annealing problems classically. Try QUBO formulations before submitting to real hardware:

```python
from dwave.samplers import SimulatedAnnealingSampler

# Solve a tiny 3-variable QUBO problem locally (no account needed)
Q = {(0,0): -1, (1,1): -1, (2,2): -1, (0,1): 2, (1,2): 2}
sampler = SimulatedAnnealingSampler()
response = sampler.sample_qubo(Q, num_reads=100)
print(response.first.sample)   # Best solution found
```

---

## Local Florida Quantum Ecosystem

For FAU students and South Florida readers — the quantum computing ecosystem is literally in your backyard:

**FAU D-Wave Advantage2**
: Florida Atlantic University's on-campus quantum computer — the first in Florida. Contact the College of Engineering and Computer Science for research access opportunities.

**D-Wave US Headquarters — Boca Raton**
: D-Wave is relocating its US HQ to the Boca Raton Innovation Campus. Watch for local events, hackathons, and career opportunities.

**Boca Raton Innovation Campus (BRIC)**
: [bocainnovationcampus.com](https://bocainnovationcampus.com) — A tech hub hosting quantum, AI, and deep-tech companies. Networking events and industry meetups.

**IEEE Quantum Week**
: Annual international conference on quantum computing. Watch for call-for-papers and student registration discounts.

**SC (Supercomputing) Conference**
: [sc-conference.org](https://sc-conference.org) — SC26 is in Chicago, November 2026. Quantum + HPC convergence track. The premier annual event for quantum-HPC hybrid computing.

---

*This resource list is current as of 2026. The quantum computing landscape evolves rapidly — check platform websites directly for the latest access tiers, free credit offers, and hardware availability.*
