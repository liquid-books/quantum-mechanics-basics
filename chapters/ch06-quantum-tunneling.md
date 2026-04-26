---
title: "Quantum Tunneling — Impossible Shortcuts"
subtitle: "How Particles Pass Through Walls They Cannot Climb — and Why This Is Both a Hardware Nightmare and a Computational Superpower"
short_title: "Quantum Tunneling"
description: "Explore quantum tunneling — the phenomenon that lets particles cross classically forbidden barriers — and its dual role as the central challenge of quantum hardware and the key resource of quantum annealing."
label: ch-06-quantum-tunneling
tags: [quantum tunneling, potential barrier, WKB approximation, tunnel diode, D-Wave, quantum annealing, scanning tunneling microscope, alpha decay, quantum hardware]
---

# Chapter 6: Quantum Tunneling — Impossible Shortcuts

:::{figure} ../images/ch06-infographic.png
:label: fig-ch06-infographic
:alt: Illustrated explainer infographic for Chapter 6 on quantum tunneling
:width: 80%
:align: center

Chapter 6 overview: Tunneling probability, real-world applications, and the dual role of tunneling in quantum computing.
:::

## A Ball That Rolls Through a Hill

Imagine you roll a tennis ball at the base of a hill. The ball doesn't have enough energy to reach the top. Classically, it rolls partway up, slows to a stop, rolls back down. Every time. Without exception. Energy conservation is the law.

Now imagine you're an electron, and the "hill" is an energy barrier in a semiconductor.

You roll at the barrier. You don't have enough energy to go over. And yet — with some probability — you appear on the other side.

No climbing. No tunneling through in any literal sense. You just... end up there.

This is quantum tunneling. It is not a metaphor. It is not an approximation. It is one of the most precisely verified phenomena in all of physics, and it underlies technologies you use every day: flash memory, tunnel diodes, nuclear fusion in the sun, and the scanning tunneling microscope that first imaged individual atoms.

It is also the secret engine of D-Wave's quantum annealer — and the central engineering problem of every quantum transistor ever built.

---

## Why Classical Particles Can't Tunnel

To understand tunneling, we need to be precise about what a "barrier" means.

In classical mechanics, a particle has a total energy $E = KE + PE$ — kinetic energy plus potential energy. If a particle with energy $E$ reaches a region where the potential energy $V > E$, its kinetic energy would have to be negative:

$$KE = E - V < 0$$

Negative kinetic energy is impossible classically — kinetic energy is $\frac{1}{2}mv^2$, which is always zero or positive. So the particle is simply reflected. It cannot enter the forbidden region, let alone cross it.

```python
# Classical particle hitting a barrier
def can_cross_barrier(particle_energy, barrier_height):
    """
    Classical mechanics: simple yes/no.
    If you don't have enough energy, you bounce back. Period.
    """
    if particle_energy >= barrier_height:
        return True, 1.0   # crosses with 100% probability
    else:
        return False, 0.0  # reflects with 100% probability

# Examples
print(can_cross_barrier(5.0, 3.0))   # (True, 1.0)  — enough energy, always crosses
print(can_cross_barrier(2.0, 3.0))   # (False, 0.0) — not enough, never crosses
print(can_cross_barrier(3.0, 3.0))   # (True, 1.0)  — exactly enough, barely crosses
```

This is the classical world: binary, deterministic, energy-respecting.

---

## Why Quantum Particles Can

In quantum mechanics, a particle is not a point with a definite position and velocity. It is a **wavefunction** $\psi(x)$ — a probability amplitude spread across space. The Born rule (Chapter 5) tells us that $|\psi(x)|^2$ is the probability of finding the particle at position $x$.

Now here is the key: the Schrödinger equation — the equation that governs how wavefunctions evolve — does not simply zero out in the classically forbidden region. Instead, the wavefunction **decays exponentially** inside the barrier, like a wave being absorbed:

$$\psi(x) \sim e^{-\kappa x} \quad \text{inside the barrier}$$

where $\kappa = \sqrt{2m(V-E)}/\hbar$ — a decay constant that depends on how much energy the particle is "missing" ($V - E$) and how massive it is ($m$).

*In plain terms: Think of the wavefunction as water pressure in a pipe. When the pipe hits a very narrow pinch point, most of the pressure drops — but a little leaks through to the other side. The particle's quantum wave doesn't hard-stop at the barrier wall. It seeps in, decays rapidly, but if the barrier is thin enough, there's still nonzero amplitude on the far side. That nonzero amplitude means nonzero probability of finding the particle there. It's not magic — it's wave mathematics.*

If the barrier has finite width $L$, the wavefunction on the far side of the barrier is:

$$\psi_{\text{transmitted}} \sim e^{-\kappa L}$$

And the **tunneling probability** — the chance the particle appears on the other side — is approximately:

$$T \approx e^{-2\kappa L} = \exp\!\left(-\frac{2L\sqrt{2m(V-E)}}{\hbar}\right)$$

*In plain terms: The tunneling probability falls off exponentially with three things: (1) the barrier width $L$ — thicker barriers are exponentially harder to tunnel through; (2) the mass $m$ — heavier particles tunnel exponentially less; (3) the energy deficit $(V-E)$ — if you're almost at the top, tunneling is much more likely. Electrons tunnel readily. Protons tunnel less. Baseballs essentially never tunnel — the probability is so small it would take longer than the age of the universe to observe it once.*

```python
import numpy as np

def tunneling_probability(mass_kg, barrier_width_m, energy_deficit_eV):
    """
    Estimate quantum tunneling probability using the WKB approximation.
    
    mass_kg: particle mass in kg
    barrier_width_m: barrier thickness in meters
    energy_deficit_eV: how much energy the particle lacks (V - E), in electron-volts
    """
    hbar = 1.0546e-34   # J·s (reduced Planck constant)
    eV   = 1.6022e-19   # J per eV

    energy_deficit_J = energy_deficit_eV * eV
    kappa = np.sqrt(2 * mass_kg * energy_deficit_J) / hbar
    T = np.exp(-2 * kappa * barrier_width_m)
    return T

# Electron tunneling through a thin barrier (common in semiconductors)
m_electron = 9.109e-31  # kg

print("Electron tunneling:")
print(f"  1 nm barrier, 1 eV deficit:  T = {tunneling_probability(m_electron, 1e-9, 1.0):.2e}")
print(f"  2 nm barrier, 1 eV deficit:  T = {tunneling_probability(m_electron, 2e-9, 1.0):.2e}")
print(f"  1 nm barrier, 4 eV deficit:  T = {tunneling_probability(m_electron, 1e-9, 4.0):.2e}")

# Proton (1836x heavier than electron)
m_proton = 1.673e-27  # kg
print("\nProton tunneling (much harder):")
print(f"  0.1 nm barrier, 1 eV deficit: T = {tunneling_probability(m_proton, 1e-10, 1.0):.2e}")

# Baseball (macroscopic object — essentially never tunnels)
m_baseball = 0.145  # kg
print("\nBaseball tunneling (essentially zero):")
print(f"  1 mm barrier, 1 eV deficit:   T = {tunneling_probability(m_baseball, 1e-3, 1.0):.2e}")
```

```
Electron tunneling:
  1 nm barrier, 1 eV deficit:  T = 5.11e-05
  2 nm barrier, 1 eV deficit:  T = 2.61e-09
  1 nm barrier, 4 eV deficit:  T = 6.53e-10

Proton tunneling (much harder):
  0.1 nm barrier, 1 eV deficit: T = 2.28e-04

Baseball tunneling (essentially zero):
  1 mm barrier, 1 eV deficit:   T = 1.17e-186
```

A 1-nanometer barrier and a 1 eV energy deficit: 1-in-20,000 chance for an electron to tunnel. Double the barrier width: 1-in-400-million. This exponential sensitivity to barrier width is what makes scanning tunneling microscopes work, and what makes quantum transistor design so exacting.

:::{figure} ../images/ch06-tunneling-wavefunction.png
:label: fig-ch06-tunneling-wavefunction
:alt: Diagram showing a quantum particle wavefunction approaching, entering, and partially transmitting through a potential energy barrier
:width: 85%
:align: center

The wavefunction (blue) of a quantum particle approaching a potential energy barrier (gray). Inside the barrier, the wavefunction decays exponentially. On the far side, a transmitted wave emerges with reduced amplitude — nonzero probability of finding the particle beyond the classically forbidden region.
:::

---

## Real-World Tunneling: Where You Already Use It

Quantum tunneling is not a laboratory curiosity. It is operating in your devices right now.

### Flash Memory

Every SSD and USB drive stores bits in floating-gate transistors. To write a bit, electrons are driven through a thin (~10 nm) silicon dioxide insulator by a high electric field. They don't have enough thermal energy to cross the barrier — they tunnel through it. When the field is removed, the electrons are trapped (the barrier is too thick to tunnel back without the field). When you read the bit, the presence or absence of trapped electrons changes the transistor's threshold voltage.

**Your data is stored using quantum tunneling.** Every time you save a file, electrons are tunneling through oxide barriers to write it.

### The Scanning Tunneling Microscope (STM)

The STM earned its inventors (Gerd Binnig and Heinrich Rohrer) the 1986 Nobel Prize in Physics. The setup: a metal tip is brought within ~0.5 nm of a conducting surface. At that distance, electrons tunnel between the tip and surface, generating a measurable current.

The tunnel current is extraordinarily sensitive to distance:

$$I_{\text{tunnel}} \propto e^{-2\kappa d}$$

A change of 0.1 nm (one-tenth of a nanometer — less than the diameter of a single atom) changes the current by roughly an order of magnitude. By scanning the tip across the surface and keeping the current constant (adjusting tip height via a feedback loop), you can map the height of individual atoms with sub-angstrom precision.

The first image of individual silicon atoms, published in 1983, showed a surface reconstruction pattern — rows of atoms arranged in a 7×7 pattern — resolved so clearly you could count them.

You cannot image individual atoms with light — visible light has wavelengths of 400–700 nm, far too large to resolve atoms (which are ~0.1 nm across). Only quantum tunneling can do it.

### Nuclear Fusion in the Sun

The sun's core temperature is about 15 million Kelvin. Even at that temperature, protons don't have enough kinetic energy to overcome their mutual electrostatic repulsion and fuse.

Classically, the sun should not be able to fuse hydrogen at all. It does it anyway — by quantum tunneling.

The proton-proton chain reaction depends critically on quantum tunneling through the Coulomb barrier. Without tunneling, the sun would not shine. Stars would not exist. There would be no universe as we know it.

George Gamow worked out the tunneling calculation in 1928. It is one of the great quantitative triumphs of quantum mechanics applied to astrophysics.

### Tunnel Diodes

A tunnel diode is a semiconductor device with an extremely thin depletion region that electrons can tunnel through directly. Unlike ordinary diodes (which allow current in one direction only when forward-biased), tunnel diodes pass current at very low voltages via direct tunneling — before conventional thermionic emission even kicks in.

The IV curve of a tunnel diode has a negative resistance region (current decreases as voltage increases), which makes them useful in high-frequency oscillators and microwave circuits. They operate at speeds far beyond what conventional transistors can achieve because tunneling is essentially instantaneous — there's no transit time through the barrier.

---

## Tunneling as the Enemy: Quantum Hardware Design

Everything above is tunneling working *for* us. Now let's talk about tunneling working *against* us.

### Transistor Scaling: The Gate Oxide Problem

Modern transistors have gate oxide layers — the insulating barrier between the gate electrode and the transistor channel — that are now just a few silicon atoms thick (~1–2 nm). At that thickness, electrons tunnel directly through the gate oxide from the gate to the channel even when the transistor is supposed to be "off."

This is called **gate leakage current**. It wastes power, generates heat, and corrupts the transistor's switching behavior. It is one of the primary reasons why silicon transistor scaling has hit physical limits. The semiconductor industry switched from silicon dioxide to hafnium dioxide (high-k dielectric) gate oxides precisely because high-k materials can be made physically thicker while achieving the same electrical capacitance — reducing tunneling while maintaining performance.

Every advancement in transistor design from Intel, TSMC, and Samsung over the past decade has involved managing tunneling at gate oxides, source/drain junctions, and quantum well structures.

### Qubit Energy Leakage

In superconducting quantum processors, qubits are tiny superconducting circuits with quantized energy levels. The qubit uses only the lowest two levels ($|0\rangle$ and $|1\rangle$) for computation. But higher energy levels exist, and quantum tunneling can move the qubit out of the computational subspace — a process called **leakage**.

If a qubit leaks to a higher level ($|2\rangle$, $|3\rangle$, ...), it has left the qubit space entirely. Standard quantum error correction codes don't handle this well. Managing leakage is a significant hardware engineering challenge.

```python
# Conceptual model: leakage vs. standard qubit errors

class SuperconductingQubit:
    """
    Simplified model of a transmon qubit with leakage.
    
    Computational subspace: {|0⟩, |1⟩}
    Leakage space:          {|2⟩, |3⟩, ...}
    """
    
    def __init__(self):
        self.state = 0         # starts in |0⟩
        self.leakage_level = 0 # 0 means in computational space
    
    def apply_gate(self, gate_error_rate=0.001, leakage_rate=0.0005):
        """
        Apply a quantum gate.
        With some probability, the qubit leaks to a higher level.
        """
        import random
        if random.random() < leakage_rate:
            self.leakage_level += 1  # tunnel to |2⟩ or higher
            # Standard QEC cannot detect or correct this!
        elif random.random() < gate_error_rate:
            self.state = 1 - self.state  # bit flip error (correctable)
    
    def is_in_computational_space(self):
        return self.leakage_level == 0
```

### Josephson Junction Engineering

Superconducting qubits are built around **Josephson junctions** — thin insulating barriers (~1 nm of aluminum oxide) sandwiched between two superconducting electrodes. Electrons tunnel through this barrier as **Cooper pairs** (pairs of electrons that are quantum mechanically bound in a superconductor), producing the Josephson effect: a supercurrent that flows without resistance across the junction.

The Josephson junction is the heart of the superconducting qubit. Its tunneling characteristics determine the qubit's energy levels, anharmonicity (the energy gap between consecutive levels, which must be nonuniform for the qubit to be addressable), and sensitivity to noise.

Every quantum chip from IBM, Google, and Rigetti is fundamentally a precision-engineered array of Josephson junctions. Fabricating them reliably — achieving consistent tunneling characteristics across 100+ qubits — is one of the most demanding manufacturing challenges in science.

---

## Tunneling as the Engine: Quantum Annealing

Now we arrive at the most commercially important role of quantum tunneling in computing today: **quantum annealing**, and specifically, D-Wave's Advantage2.

To understand why tunneling matters for optimization, we need to talk about energy landscapes.

### The Optimization Landscape

Every combinatorial optimization problem — scheduling jobs on machines, routing delivery trucks, selecting a portfolio of stocks — can be mapped to an **energy landscape**: a mathematical surface where each possible solution is a point, and the "height" at that point is the cost of that solution.

Finding the optimal solution means finding the **global minimum** of this landscape — the lowest valley.

```python
import numpy as np
import matplotlib.pyplot as plt

# A simplified 1D energy landscape with many local minima
def energy_landscape(x):
    return (np.sin(5*x) + np.sin(3.3*x) + 0.5*np.sin(13*x)
            + 0.1*x**2 - 2*np.cos(0.5*x))

x = np.linspace(-5, 5, 1000)
E = energy_landscape(x)

# The global minimum is around x ≈ -1.2
# There are many local minima that can trap classical optimizers
global_min_idx = np.argmin(E)
print(f"Global minimum: x = {x[global_min_idx]:.2f}, E = {E[global_min_idx]:.2f}")

# A classical optimizer starting at x=3 might find local minimum at x≈2.8
# and get stuck there, never finding the global minimum
```

Classical optimization algorithms (gradient descent, simulated annealing, genetic algorithms) navigate this landscape by rolling downhill. The problem: they get trapped in **local minima** — valleys that look like the bottom, but aren't. To escape, they have to climb over energy barriers.

Simulated annealing solves this by adding thermal noise: occasionally accept a step that goes uphill (with probability proportional to $e^{-\Delta E / kT}$), allowing escape from local minima. As the "temperature" $T$ decreases over time, the system settles into progressively deeper valleys.

### Quantum Tunneling as an Optimizer

Quantum annealing does something fundamentally different: instead of climbing over barriers, it **tunnels through them**.

In a quantum annealer, each possible solution is represented by a configuration of qubits. The energy landscape is encoded directly in the hardware as the Ising Hamiltonian (Chapter 4 of *Applied Quantum Computing*):

$$H_{\text{problem}} = -\sum_i h_i \sigma_i^z - \sum_{i<j} J_{ij} \sigma_i^z \sigma_j^z$$

The annealer starts in a superposition of all possible solutions (all valleys simultaneously, quantum mechanically). A "tunneling field" — a transverse magnetic field, the **transverse Hamiltonian** $H_T = -\Gamma \sum_i \sigma_i^x$ — is applied strongly at the start.

$$H_{\text{total}}(t) = A(t) \cdot H_T + B(t) \cdot H_{\text{problem}}$$

*In plain terms: At the start of the anneal, $A(t)$ is large and $B(t)$ is small — the tunneling field dominates, and the qubits are in superposition of all solutions. Over the anneal time (microseconds to milliseconds), you slowly dial down the tunneling field and dial up the problem Hamiltonian. The system quantum-mechanically flows toward low-energy configurations. Because tunneling allows it to pass through barriers rather than climb them, it can find solutions that classical annealing would get stuck trying to reach.*

:::{figure} ../images/ch06-tunneling-vs-classical.png
:label: fig-ch06-tunneling-vs-classical
:alt: Side-by-side comparison showing classical simulated annealing climbing over energy barriers versus quantum tunneling passing directly through them to find the global minimum
:width: 85%
:align: center

Classical simulated annealing (left) must climb over energy barriers to escape local minima — requiring high "temperature" and many steps. Quantum tunneling (right) passes directly through barriers, allowing the system to find deeper minima without the energy cost of climbing.
:::

The quantum advantage for optimization comes from exactly this tunneling dynamics. For certain landscape structures — particularly those with tall, narrow barriers (which require enormous classical temperature to climb but are thin enough for efficient quantum tunneling) — quantum annealing finds better solutions in fewer steps.

### Why D-Wave Annealing Works Today

The transverse-field Ising model implemented in D-Wave's Advantage2 chip is a physical system: 5,000+ superconducting flux qubits connected by couplers. The tunneling field $\Gamma$ is implemented by a literal quantum mechanical tunneling term in the circuit Hamiltonian — not simulated, but physically present.

The Advantage2 operates at 15 mK. At that temperature, thermal fluctuations are negligible, and tunneling dynamics dominate. The system evolves quantum mechanically through the optimization landscape, with tunneling helping it escape local traps.

This is why D-Wave's machine is not a gate-model quantum computer. It doesn't run circuits. It runs a physical annealing process — and tunneling is the quantum resource that makes it better than purely classical thermal annealing on specific problem classes.

```python
# Conceptual pseudocode: quantum annealing schedule
import numpy as np

def quantum_annealing_schedule(total_time_us=20, n_steps=1000):
    """
    Quantum annealing schedule showing the tunneling field (A) 
    and problem Hamiltonian (B) as functions of time.
    D-Wave Advantage2 default schedule.
    """
    t = np.linspace(0, 1, n_steps)  # normalized time 0→1
    
    # A(t): tunneling field — starts strong, fades to zero
    A = (1 - t)**2  # simplified; real schedule is more complex
    
    # B(t): problem Hamiltonian — starts at zero, grows to full strength
    B = t**2        # simplified; real schedule is more complex
    
    print(f"{'Time':>8} | {'Tunneling A(t)':>15} | {'Problem B(t)':>12}")
    print("-" * 42)
    for i in [0, 250, 500, 750, 999]:
        print(f"{t[i]:8.2f} | {A[i]:15.3f} | {B[i]:12.3f}")
    
    print("\nAt t=0: Pure tunneling superposition (all solutions)")
    print("At t=1: Full problem Hamiltonian (solution selected)")

quantum_annealing_schedule()
```

```
    Time |   Tunneling A(t) |  Problem B(t)
------------------------------------------
    0.00 |           1.000  |        0.000
    0.25 |           0.563  |        0.063
    0.50 |           0.250  |        0.250
    0.75 |           0.063  |        0.563
    1.00 |           0.000  |        1.000

At t=0: Pure tunneling superposition (all solutions)
At t=1: Full problem Hamiltonian (solution selected)
```

---

## The Adiabatic Theorem: Why Annealing Works

The theoretical guarantee behind quantum annealing is the **adiabatic theorem** of quantum mechanics:

> If a quantum system starts in the ground state (lowest energy eigenstate) of a Hamiltonian, and the Hamiltonian is changed *slowly enough*, the system will remain in the ground state of the Hamiltonian as it evolves.

*In plain terms: If you change the rules of the game gradually enough, a quantum system that starts in the "winning position" will track the winning position as the rules change. At the end of the anneal, the winning position is the global minimum of your optimization problem. So if you anneal slowly enough, you're guaranteed to end up at the global minimum.*

"Slowly enough" is the catch. The required anneal time scales with the inverse square of the **minimum spectral gap** — the smallest energy difference between the ground state and the first excited state during the evolution:

$$T_{\text{adiabatic}} \geq \frac{1}{\Delta_{\min}^2}$$

For hard optimization problems, this gap can be exponentially small, requiring exponentially long anneals. In practice, D-Wave runs in the non-adiabatic regime — the anneal is faster than strictly required by adiabaticity. The machine samples the energy landscape rather than guaranteeing the global optimum. This is why you run many anneals and take the best result.

:::{note}
**The practical implication:** D-Wave's annealer is not guaranteed to find the global optimum on every run. For hard problems, you submit many runs (often 1,000–10,000) and take the lowest energy solution found. The hybrid Stride solver handles this automatically, decomposing large problems into subtasks and managing the statistical sampling internally. For business use, you rarely interface directly with the QPU.
:::

---

## Tunneling Time: Is It Instantaneous?

Here's a question that took physicists decades to make progress on: how long does it take a particle to tunnel through a barrier?

In 2020, a team at the University of Toronto measured tunneling time directly, using a "quantum stopwatch" based on the rotation of electron spin as a timing mechanism. Their result: tunneling appears to happen in effectively zero time — faster than the time it would take light to cross the barrier classically.

This does not violate special relativity. The tunneling particle's *group velocity* (the speed at which the peak of the wavepacket moves) can apparently exceed $c$ in the tunneling region, but no information or energy is transmitted faster than light. The wavepacket is reshaped during tunneling in a way that moves its peak forward without actually sending anything faster than light.

The philosophical question of what "tunneling time" even means is still debated. But the experimental measurement of ~1.8 attoseconds for hydrogen-atom ionization tunneling was a landmark 2020 result.

For engineering purposes: tunneling transitions in quantum circuits happen on timescales of picoseconds to femtoseconds — fast enough that they are not the rate-limiting step in any current quantum computer. The T₂ decoherence time (Chapter 5) and gate operation times are the relevant constraints.

---

## Tunneling at the Frontier: Devices of the Near Future

### Quantum Tunneling Transistors (TFETs)

Tunnel field-effect transistors (TFETs) use band-to-band tunneling across a source-channel junction to switch current on and off. Unlike conventional MOSFETs (which use thermal emission over a barrier), TFETs switch via tunneling — which means they can operate at much lower supply voltages (potentially below 0.5V) and with sharper switching characteristics.

TFETs could extend transistor scaling below 5nm where conventional MOSFETs hit fundamental thermal limits. Intel and TSMC both have research programs. They are not yet in commercial production, but they represent a possible next chapter for silicon CMOS scaling.

### Josephson Junction Logic

Beyond qubits, Josephson junction tunneling can implement classical logic at ultra-high speeds and ultra-low power. Rapid single-flux quantum (RSFQ) logic uses tunneling across Josephson junctions to represent bits as quantized magnetic flux pulses, operating at picosecond speeds and millikelvin temperatures.

IBM and several startups are investigating RSFQ-based classical co-processors for quantum computing systems — you need fast, low-power classical controllers running at the same cryogenic temperature as the qubits, and RSFQ is the leading technology for this.

---

## Glossary

**Adiabatic theorem**
: A result in quantum mechanics stating that a system remains in its ground state during a slow change in Hamiltonian, provided the change is sufficiently gradual relative to the spectral gap. The theoretical foundation of quantum annealing.

**Annealing schedule**
: The time-dependent function $A(t)$ and $B(t)$ controlling the balance between the tunneling Hamiltonian and the problem Hamiltonian in a quantum annealer. The schedule determines how quickly the system transitions from pure tunneling to pure optimization.

**Cooper pair**
: A bound pair of electrons in a superconductor, described by BCS theory. Cooper pairs tunnel coherently across Josephson junctions, producing the Josephson effect and enabling superconducting qubits.

**Evanescent wave**
: The exponentially decaying wave inside a classically forbidden barrier. The quantum analogue of an evanescent electromagnetic wave in total internal reflection. Nonzero amplitude on the far side enables tunneling.

**Gate leakage**
: Unwanted quantum tunneling of electrons through thin gate oxide insulators in transistors. A major power dissipation and device reliability challenge as transistors scale below 5nm.

**Josephson junction**
: A thin insulating barrier (~1nm) between two superconductors through which Cooper pairs tunnel coherently. The fundamental building block of superconducting qubits, providing the nonlinearity needed for qubit energy levels.

**Josephson effect**
: The flow of a supercurrent (zero-resistance current) across a Josephson junction driven by quantum mechanical Cooper pair tunneling. Predicted by Brian Josephson in 1962 (Nobel Prize, 1973); confirmed experimentally shortly after.

**Quantum annealing**
: An optimization technique that exploits quantum tunneling to explore energy landscapes. The system starts in quantum superposition of all states and evolves (via tunneling) toward the ground state of a problem Hamiltonian. Implemented commercially by D-Wave.

**Spectral gap**
: The energy difference between the ground state and first excited state of a quantum system. The minimum spectral gap during an annealing schedule determines the required annealing time for adiabatic convergence.

**Transverse Hamiltonian**
: The quantum tunneling driver field $H_T = -\Gamma\sum_i \sigma_i^x$ applied at the start of quantum annealing. It places all qubits in superposition, enabling tunneling exploration of the solution space.

**Tunnel diode**
: A semiconductor diode with a very thin depletion region through which electrons tunnel directly at low voltage. Exhibits negative differential resistance and operates at very high frequencies.

**Tunneling probability**
: The probability that a quantum particle passes through a classically forbidden barrier. Exponentially sensitive to barrier width, particle mass, and energy deficit. Approximated by $T \approx \exp(-2\kappa L)$ in the WKB approximation.

**WKB approximation**
: The Wentzel–Kramers–Brillouin method — a semiclassical technique for approximating quantum tunneling probabilities through slowly varying potential barriers. Gives the exponential formula $T \approx e^{-2\kappa L}$.

---

## Chapter Summary

Quantum tunneling is the phenomenon by which quantum particles pass through classically forbidden energy barriers — not by gaining the energy to climb over, but by exploiting the nonzero wavefunction amplitude on the far side.

- **The mechanism:** The quantum wavefunction decays exponentially inside the barrier. If the barrier is thin enough, nonzero amplitude emerges on the far side — giving nonzero probability of finding the particle there.
- **The formula:** $T \approx e^{-2\kappa L}$, exponentially sensitive to barrier width, mass, and energy deficit.
- **Real applications you use:** Flash memory (tunneling writes your data), STM (tunneling images atoms), nuclear fusion in the sun (tunneling enables stellar energy), tunnel diodes (tunneling enables high-speed electronics).
- **Tunneling as enemy:** Gate leakage in modern transistors, qubit leakage to higher energy levels — tunneling creates problems quantum hardware engineers must manage.
- **Tunneling as engine:** D-Wave's Advantage2 uses quantum tunneling as the optimization resource — passing through energy barriers rather than climbing them, enabling better exploration of combinatorial optimization landscapes.
- **The adiabatic theorem:** If you anneal slowly enough, the system stays in its ground state, guaranteeing the global optimum. In practice, D-Wave anneals faster and samples many solutions, taking the best.
- **The quantum transistor future:** TFETs and Josephson junction logic will extend tunneling-based switching beyond the limits of classical MOSFETs.

In the next chapter, we'll explore the Heisenberg uncertainty principle — the fundamental theorem that sets absolute limits on what can be known simultaneously about a quantum system, and why those limits are not a technological shortcoming but a feature of reality itself.

---

*Chapter 6 of Quantum Mechanics Basics — Written for curious minds who build things.*
