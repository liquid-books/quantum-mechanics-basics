---
title: "Appendix B: The Math You Actually Need"
subtitle: "A Gentle Crash Course in the Linear Algebra Behind Quantum Mechanics — No Physics Degree Required"
short_title: "The Math You Need"
description: "A programmer-friendly introduction to complex numbers, vectors, matrices, and inner products — the minimum mathematical toolkit for reading and understanding quantum mechanics equations."
label: appendix-b-math
---

# Appendix B: The Math You Actually Need

*Quantum mechanics runs on linear algebra. This appendix covers the minimum mathematical toolkit — from complex numbers to matrix multiplication to inner products — with worked examples, color-coded notation, and programmer analogies throughout. No prerequisites beyond high school algebra.*

---

:::{admonition} Who This Appendix Is For
:class: tip

If you read an equation in a chapter and felt uncertain, this appendix has you covered. We'll build up the math from scratch — slowly, concretely, and always with the "why does this matter for quantum computing?" question answered. Every section has runnable Python code alongside the math.
:::

---

## B.1 Complex Numbers — The Upgrade to Real Numbers

The single most important mathematical idea in quantum mechanics is the **complex number**. If you've avoided them, now is the time to make peace.

### What Is a Complex Number?

A complex number has two parts: a **real part** and an **imaginary part**:

$$z = a + bi$$

:::{admonition} Color Key for This Appendix
:class: note
- 🔵 **Real part** — the ordinary number part you already know
- 🟡 **Imaginary part** — the new part, involving $i$
- 🟢 **Magnitude** — the "size" of the complex number
- 🔴 **Phase** — the "direction" of the complex number in the complex plane
:::

Where:
- $a$ is the **real part** (🔵 an ordinary number you already know)
- $b$ is the **imaginary part** (🟡 the new part)
- $i = \sqrt{-1}$ is the **imaginary unit** — the number that, when squared, gives $-1$

*In plain terms: Think of $i$ as a new dimension. Just like 2D coordinates have an $x$-direction and a $y$-direction, complex numbers have a "real" direction and an "imaginary" direction. A complex number is a point in this 2D plane.*

```python
# Python handles complex numbers natively
z1 = 3 + 4j       # Python uses 'j' for the imaginary unit
z2 = 1 - 2j

print(f"z1 = {z1}")                    # (3+4j)
print(f"Real part of z1: {z1.real}")   # 3.0
print(f"Imaginary part:  {z1.imag}")   # 4.0

# Complex arithmetic — same rules as real numbers, but i² = -1
print(f"z1 + z2 = {z1 + z2}")          # (4+2j)
print(f"z1 × z2 = {z1 * z2}")          # (11+2j)  [computed: (3+4i)(1-2i) = 3-6i+4i-8i² = 3-2i+8 = 11+2i]
```

### Why Quantum Mechanics Needs Complex Numbers

Here's the key reason: **quantum amplitudes must be complex so that they can interfere**.

Real numbers can only be positive or negative — they can cancel, but in a limited way. Complex amplitudes carry **phase** (🔴 direction in the complex plane), and two amplitudes with the same magnitude but opposite phase cancel perfectly. This is destructive interference — the engine of every quantum algorithm.

If amplitudes were just real numbers, quantum algorithms like Grover's and Shor's simply wouldn't work. The complex phase is not a mathematical curiosity; it is the physical resource that makes quantum computing qualitatively more powerful than classical.

### Magnitude: How Big Is a Complex Number?

The **magnitude** (or absolute value) of a complex number $z = a + bi$ is:

$$|z| = \sqrt{a^2 + b^2}$$

🟢 This is just the Pythagorean theorem — the distance from the origin to the point $(a, b)$ in the complex plane.

```python
import numpy as np

z = 3 + 4j
magnitude = abs(z)   # Python's abs() works on complex numbers
print(f"|z| = √(3² + 4²) = √(9+16) = √25 = {magnitude}")  # 5.0

# The Born rule uses magnitude squared:
z_amplitude = 1/np.sqrt(2) + 0j   # amplitude α = 1/√2
prob = abs(z_amplitude)**2          # |α|² = probability
print(f"Probability = |α|² = {prob:.3f}")   # 0.500 — 50% chance
```

**The Born rule in one line:** $P(\text{outcome}) = |z|^2$. The probability of a measurement outcome is the squared magnitude of its amplitude. This is why you always square the absolute value — and why complex phases affect interference but not individual measurement probabilities.

### Complex Conjugate: Flipping the Imaginary Part

The **complex conjugate** of $z = a + bi$ is $z^* = a - bi$ (just flip the sign of the imaginary part):

$$z = a + bi \quad \Rightarrow \quad z^* = a - bi$$

🟡 The imaginary part gets negated; 🔵 the real part stays the same.

```python
z = 3 + 4j
z_conj = z.conjugate()    # or np.conj(z)
print(f"z  = {z}")         # (3+4j)
print(f"z* = {z_conj}")    # (3-4j)

# Key identity: z × z* = |z|²  (always real and positive)
print(f"z × z* = {z * z_conj}")     # (25+0j) — always real
print(f"|z|² = {abs(z)**2}")         # 25.0
```

This identity ($z \cdot z^* = |z|^2$) is used constantly in quantum mechanics — including in the Born rule: $P = \alpha^* \alpha = |\alpha|^2$.

### Euler's Formula: The Elegant Connection

One of the most beautiful equations in mathematics connects complex numbers to trigonometry:

$$e^{i\phi} = \cos\phi + i\sin\phi$$

🔴 This is the **phase factor** — a complex number with magnitude exactly 1, sitting on the unit circle in the complex plane at angle $\phi$.

*In plain terms: If complex numbers are points in a 2D plane, then $e^{i\phi}$ is a point on the unit circle (radius 1) at angle $\phi$ from the positive real axis. Rotating by $\phi$ in the complex plane corresponds to multiplying by $e^{i\phi}$. Quantum gates that "shift the phase" of a qubit are doing exactly this — rotating the qubit's amplitude in the complex plane.*

```python
import numpy as np

phi = np.pi / 4    # 45 degrees

# Euler's formula: e^(i·phi) = cos(phi) + i·sin(phi)
euler = np.exp(1j * phi)
trig  = np.cos(phi) + 1j * np.sin(phi)

print(f"e^(iπ/4) = {euler:.4f}")   # (0.7071+0.7071j) — 1/√2 + i/√2
print(f"cos+isin = {trig:.4f}")    # same
print(f"Magnitude: {abs(euler):.4f}")   # 1.0000 — always on unit circle

# The famous identity e^(iπ) + 1 = 0:
print(f"e^(iπ) = {np.exp(1j * np.pi):.0f}")   # (-1+0j)
```

Every quantum gate's matrix entries are combinations of $e^{i\phi}$ values. When you see a quantum gate matrix with $i$ or $e^{i\pi/4}$ in it, you're looking at rotations in the complex plane.

---

## B.2 Vectors — Quantum States as Arrows

A quantum state is a **vector** — a list of complex numbers representing amplitudes for each possible measurement outcome.

### What Is a Vector?

A vector is an ordered list of numbers. In quantum computing, state vectors are **column vectors**:

$$|\psi\rangle = \begin{pmatrix} \alpha \\ \beta \end{pmatrix}$$

This is the amplitude for $|0\rangle$ on top, the amplitude for $|1\rangle$ on the bottom.

:::{admonition} Dirac Notation — The Quantum Physicist's Shorthand
:class: note

Quantum mechanics uses a special notation invented by Paul Dirac:

- **Ket** $|\psi\rangle$ — a column vector representing a quantum state
- **Bra** $\langle\psi|$ — a row vector, the conjugate transpose of the ket: $\langle\psi| = (|\psi\rangle)^\dagger$
- **Bra-ket** $\langle\phi|\psi\rangle$ — the inner product of two states (a single complex number)

The words "ket" and "bra" come from splitting "bracket" — $\langle\text{bra}|\text{ket}\rangle$. It's genuinely called that.
:::

```python
import numpy as np

# A qubit state: |ψ⟩ = (1/√2)|0⟩ + (1/√2)|1⟩
psi = np.array([1/np.sqrt(2),    # amplitude for |0⟩
                1/np.sqrt(2)])   # amplitude for |1⟩

print("State vector:")
print(f"  α (amplitude for |0⟩): {psi[0]:.4f}")
print(f"  β (amplitude for |1⟩): {psi[1]:.4f}")

# Verify normalization: |α|² + |β|² = 1
norm = np.sum(np.abs(psi)**2)
print(f"\nNormalization check: |α|² + |β|² = {norm:.4f}")   # 1.0000
```

### The Computational Basis States

The two simplest qubit states are the **basis states**:

$$|0\rangle = \begin{pmatrix} 1 \\ 0 \end{pmatrix} \qquad |1\rangle = \begin{pmatrix} 0 \\ 1 \end{pmatrix}$$

Every other qubit state is a linear combination (superposition) of these two:

$$|\psi\rangle = \alpha\begin{pmatrix} 1 \\ 0 \end{pmatrix} + \beta\begin{pmatrix} 0 \\ 1 \end{pmatrix} = \begin{pmatrix} \alpha \\ \beta \end{pmatrix}$$

🔵 **Real part intuition:** If $\alpha$ and $\beta$ were real numbers, this would be a vector in 2D space pointing in some direction. The qubit's state is literally a direction.

🟡 **Complex part:** But $\alpha$ and $\beta$ are complex — so the "direction" lives in a 4-dimensional space (real part of $\alpha$, imaginary part of $\alpha$, real part of $\beta$, imaginary part of $\beta$). The Bloch sphere is a way to visualize this while keeping it 3D.

### Multi-Qubit States: The Tensor Product

Two qubits together live in a **4-dimensional** space. The basis states are all combinations: $|00\rangle$, $|01\rangle$, $|10\rangle$, $|11\rangle$.

The mathematical operation for combining systems is the **tensor product** ($\otimes$):

$$|0\rangle \otimes |0\rangle = |00\rangle = \begin{pmatrix}1\\0\end{pmatrix} \otimes \begin{pmatrix}1\\0\end{pmatrix} = \begin{pmatrix}1\\0\\0\\0\end{pmatrix}$$

*In plain terms: To combine two qubits into a two-qubit state, multiply every element of the first vector by the entire second vector, stacking the results. For n qubits, the resulting vector has $2^n$ entries — one for each possible $n$-bit combination. This is why n qubits need $2^n$ amplitudes to describe fully.*

```python
# Tensor product: combining two qubit states
ket_0 = np.array([1, 0])   # |0⟩
ket_1 = np.array([0, 1])   # |1⟩

# |00⟩ = |0⟩ ⊗ |0⟩
ket_00 = np.kron(ket_0, ket_0)   # np.kron is tensor product
print(f"|00⟩ = {ket_00}")         # [1, 0, 0, 0]

# |01⟩ = |0⟩ ⊗ |1⟩
ket_01 = np.kron(ket_0, ket_1)
print(f"|01⟩ = {ket_01}")         # [0, 1, 0, 0]

# |10⟩ = |1⟩ ⊗ |0⟩
ket_10 = np.kron(ket_1, ket_0)
print(f"|10⟩ = {ket_10}")         # [0, 0, 1, 0]

# |11⟩ = |1⟩ ⊗ |1⟩
ket_11 = np.kron(ket_1, ket_1)
print(f"|11⟩ = {ket_11}")         # [0, 0, 0, 1]

# A Bell state: (|00⟩ + |11⟩)/√2
bell = (ket_00 + ket_11) / np.sqrt(2)
print(f"\nBell state: {bell}")      # [0.707, 0, 0, 0.707]
print(f"Interpretation: 50% chance of |00⟩, 50% chance of |11⟩")
```

For $n$ qubits, the state vector has $2^n$ entries. This is why 300 qubits would need more memory to store than there are atoms in the universe — but a quantum processor manipulates all $2^{300}$ amplitudes in parallel using physical quantum mechanics.

---

## B.3 Matrices — Quantum Gates as Transformations

**Matrices** are the mathematical objects that transform vectors. Every quantum gate is a matrix.

### What Is a Matrix?

A matrix is a rectangular array of numbers. A $2\times 2$ matrix looks like:

$$M = \begin{pmatrix} a & b \\ c & d \end{pmatrix}$$

It transforms a $2$-element column vector by **matrix-vector multiplication**:

$$M\begin{pmatrix} x \\ y \end{pmatrix} = \begin{pmatrix} ax + by \\ cx + dy \end{pmatrix}$$

🔵 **Each row** of the matrix dot-products with the input vector.

*In plain terms: A matrix is a function that takes a vector as input and produces a new vector as output. Quantum gates are matrices — they take a qubit state as input and produce a new qubit state as output.*

```python
import numpy as np

# The Hadamard gate: creates superposition
H = (1/np.sqrt(2)) * np.array([[1,  1],
                                 [1, -1]])

ket_0 = np.array([1, 0])   # |0⟩

# Apply H to |0⟩: matrix-vector multiplication
result = H @ ket_0    # @ is Python's matrix multiplication operator
print(f"H|0⟩ = {result}")   # [0.707, 0.707] — equal superposition

# Verify: probabilities sum to 1
probs = np.abs(result)**2
print(f"P(0) = {probs[0]:.3f}, P(1) = {probs[1]:.3f}, sum = {sum(probs):.3f}")
```

### The Key Quantum Gates as Matrices

Here are the fundamental quantum gates — every one a $2\times 2$ unitary matrix:

$$\underbrace{I = \begin{pmatrix}1&0\\0&1\end{pmatrix}}_{\text{🔵 Identity — does nothing}} \qquad \underbrace{X = \begin{pmatrix}0&1\\1&0\end{pmatrix}}_{\text{🟡 Pauli-X — bit flip (NOT gate)}}$$

$$\underbrace{Z = \begin{pmatrix}1&0\\0&-1\end{pmatrix}}_{\text{🔴 Pauli-Z — phase flip}} \qquad \underbrace{H = \frac{1}{\sqrt{2}}\begin{pmatrix}1&1\\1&-1\end{pmatrix}}_{\text{🟢 Hadamard — creates superposition}}$$

```python
# All fundamental gates
I = np.array([[1, 0], [0, 1]], dtype=complex)              # Identity
X = np.array([[0, 1], [1, 0]], dtype=complex)              # NOT / Pauli-X
Z = np.array([[1, 0], [0,-1]], dtype=complex)              # Phase flip
H = np.array([[1, 1], [1,-1]], dtype=complex) / np.sqrt(2) # Hadamard

ket_0 = np.array([1, 0], dtype=complex)
ket_1 = np.array([0, 1], dtype=complex)

print("Gate actions on |0⟩ and |1⟩:")
print(f"  I|0⟩ = {I @ ket_0}  (unchanged)")
print(f"  X|0⟩ = {X @ ket_0}  (flipped to |1⟩)")
print(f"  Z|0⟩ = {Z @ ket_0}  (unchanged — Z only affects |1⟩)")
print(f"  Z|1⟩ = {Z @ ket_1}  (phase flipped: -|1⟩)")
print(f"  H|0⟩ = {np.round(H @ ket_0, 4)}  (superposition)")
print(f"  H|1⟩ = {np.round(H @ ket_1, 4)}  (superposition with phase)")
```

### Why Quantum Gates Must Be Unitary

A matrix $U$ is **unitary** if $U^\dagger U = I$, where $U^\dagger$ is the **conjugate transpose** (transpose + complex conjugate of every entry).

🟢 **Why unitary?** Two reasons:
1. **Normalization:** Unitary matrices preserve the length (norm) of vectors. Since $|\alpha|^2 + |\beta|^2 = 1$ must always hold, gates must preserve this — and unitary matrices always do.
2. **Reversibility:** $U^\dagger U = I$ means $U^{-1} = U^\dagger$ — every unitary gate has an inverse (its conjugate transpose). Quantum operations are reversible; information is never lost.

```python
# Verify Hadamard is unitary: H†H = I
H_dagger = H.conj().T   # conjugate transpose

product = H_dagger @ H
print("H†H =")
print(np.round(product, 10))   # Should be identity matrix

is_unitary = np.allclose(product, np.eye(2))
print(f"H is unitary: {is_unitary}")   # True

# Verify X is unitary too
print(f"X is unitary: {np.allclose(X.conj().T @ X, np.eye(2))}")   # True

# A NON-unitary matrix (just for contrast)
not_unitary = np.array([[1, 1], [0, 1]])
print(f"[[1,1],[0,1]] is unitary: {np.allclose(not_unitary.conj().T @ not_unitary, np.eye(2))}")  # False
```

### Matrix Multiplication: Applying Gates in Sequence

When you apply gate $A$ then gate $B$, the combined transformation is $BA$ (note the order — matrix multiplication applies right-to-left, like function composition):

$$(B \circ A)|\psi\rangle = B(A|\psi\rangle)$$

```python
# Applying H then X: what does the combined gate do?
# Combined gate = X @ H  (X applied AFTER H, so X is on the LEFT)
XH = X @ H

print("Applying H then X to |0⟩:")
step1 = H @ ket_0                        # Apply H first
step2 = X @ step1                        # Apply X second
combined = XH @ ket_0                    # Combined gate

print(f"  After H:    {np.round(step1, 4)}")
print(f"  After X:    {np.round(step2, 4)}")
print(f"  Combined:   {np.round(combined, 4)}")
print(f"  Same result: {np.allclose(step2, combined)}")   # True

# Classic identity: HXH = Z  (the "phase kickback" technique)
HXH = H @ X @ H
print(f"\nHXH ≈ Z: {np.allclose(HXH, Z)}")   # True
```

---

## B.4 Inner Products — Measuring Overlap Between States

The **inner product** (or dot product, adapted for complex vectors) measures how much two quantum states "overlap." It's written $\langle\phi|\psi\rangle$ in Dirac notation.

### How to Compute an Inner Product

For two column vectors $|\phi\rangle$ and $|\psi\rangle$, the inner product $\langle\phi|\psi\rangle$ is computed by:
1. Take the **conjugate transpose** of the first vector (turning the ket $|\phi\rangle$ into the bra $\langle\phi|$)
2. Multiply (dot product) with the second vector

$$\langle\phi|\psi\rangle = \sum_i \phi_i^* \psi_i$$

🟡 The star means complex conjugate — flip the sign of any imaginary part.

```python
# Inner product <φ|ψ>
phi = np.array([1/np.sqrt(2), 1/np.sqrt(2)], dtype=complex)   # |+⟩
psi = np.array([1/np.sqrt(2),-1/np.sqrt(2)], dtype=complex)   # |−⟩

# Inner product: conjugate of phi, dot product with psi
inner = np.dot(phi.conj(), psi)
print(f"⟨+|−⟩ = {inner:.4f}")   # 0.0 — orthogonal! (|+⟩ and |−⟩ are perpendicular)

# |0⟩ and |1⟩ are also orthogonal:
print(f"⟨0|1⟩ = {np.dot(ket_0.conj(), ket_1):.4f}")   # 0.0

# A state's inner product with itself = 1 (normalization):
print(f"⟨ψ|ψ⟩ = {np.dot(psi.conj(), psi):.4f}")        # 1.0
```

### What Inner Products Tell You

| $\langle\phi|\psi\rangle$ | Meaning |
|---------------------------|---------|
| $= 1$ | States are identical |
| $= 0$ | States are **orthogonal** — no overlap (like 0 and 1 being completely distinct) |
| $= 0.707$ | States have 50% overlap |
| $\|\langle\phi\|\psi\rangle\|^2$ | **Probability** of measuring $|\phi\rangle$ when system is in $|\psi\rangle$ |

🔵 **Key insight:** The probability of getting outcome $|x\rangle$ when measuring state $|\psi\rangle$ is exactly $|\langle x|\psi\rangle|^2$ — the inner product squared. This is the Born rule expressed in Dirac notation.

```python
# Born rule via inner product
psi_super = np.array([1/np.sqrt(2), 1/np.sqrt(2)], dtype=complex)  # |+⟩

# P(measure |0⟩) = |⟨0|ψ⟩|²
prob_0 = abs(np.dot(ket_0.conj(), psi_super))**2
prob_1 = abs(np.dot(ket_1.conj(), psi_super))**2

print(f"P(measure |0⟩) = |⟨0|+⟩|² = {prob_0:.3f}")   # 0.500
print(f"P(measure |1⟩) = |⟨1|+⟩|² = {prob_1:.3f}")   # 0.500
```

---

## B.5 Expectation Values — Quantum Averages

The **expectation value** $\langle O \rangle$ of an observable $O$ in state $|\psi\rangle$ is the average measurement result over many repeated measurements:

$$\langle O \rangle = \langle\psi|O|\psi\rangle$$

*In plain terms: To compute the expected average of a quantum measurement: (1) apply the operator $O$ to the state $|\psi\rangle$, (2) take the inner product with $|\psi\rangle$ itself. The result is a real number — the average measurement value.*

This notation appears everywhere in quantum computing — especially in VQE, where the energy expectation value $\langle\psi(\theta)|H|\psi(\theta)\rangle$ is what the quantum circuit estimates, and what the classical optimizer minimizes.

```python
# Expectation value of Pauli-Z in state |ψ⟩
# ⟨Z⟩ = ⟨ψ|Z|ψ⟩ = P(measure 0) - P(measure 1)

Z = np.array([[1, 0], [0, -1]], dtype=complex)

def expectation_value(observable, state):
    """Compute ⟨ψ|O|ψ⟩ — the quantum average of observable O."""
    O_psi = observable @ state       # Apply O to state
    return np.real(np.dot(state.conj(), O_psi))   # ⟨ψ|O|ψ⟩

# For |0⟩: ⟨Z⟩ = 1 (definitely in +1 eigenstate)
print(f"⟨Z⟩ for |0⟩: {expectation_value(Z, ket_0):.3f}")   # 1.0

# For |1⟩: ⟨Z⟩ = -1 (definitely in -1 eigenstate)
print(f"⟨Z⟩ for |1⟩: {expectation_value(Z, ket_1):.3f}")   # -1.0

# For |+⟩: ⟨Z⟩ = 0 (50/50, average is zero)
ket_plus = np.array([1/np.sqrt(2), 1/np.sqrt(2)], dtype=complex)
print(f"⟨Z⟩ for |+⟩: {expectation_value(Z, ket_plus):.3f}")  # 0.0

print("\n⟨Z⟩ = P(measure 0) - P(measure 1)  — this is why VQE measures in Pauli bases")
```

---

## B.6 Putting It All Together: Reading a Quantum Equation

Now you can read the equations that appear throughout this book. Let's decode a few:

### The Qubit State

$$|\psi\rangle = \alpha|0\rangle + \beta|1\rangle \quad \text{where} \quad |\alpha|^2 + |\beta|^2 = 1$$

- 🔵 $|\psi\rangle$ — a column vector of two complex numbers
- 🟡 $\alpha, \beta$ — complex amplitudes (magnitude = probability weight; phase = interference behavior)
- 🟢 $|\alpha|^2 + |\beta|^2 = 1$ — normalization: total probability = 1
- 🔴 $|0\rangle, |1\rangle$ — the two basis column vectors $\begin{pmatrix}1\\0\end{pmatrix}$ and $\begin{pmatrix}0\\1\end{pmatrix}$

### The Hadamard Gate

$$H = \frac{1}{\sqrt{2}}\begin{pmatrix}1&1\\1&-1\end{pmatrix}, \qquad H|0\rangle = \frac{1}{\sqrt{2}}\begin{pmatrix}1\\1\end{pmatrix} = \frac{|0\rangle + |1\rangle}{\sqrt{2}}$$

- 🟢 $\frac{1}{\sqrt{2}}$ — normalization factor ensuring outputs have magnitude 1
- 🔴 The $-1$ in the bottom-right — creates a phase difference between the two superposition components; this phase difference is what makes the $|+\rangle$ and $|-\rangle$ states distinguishable

### The Uncertainty Principle

$$\sigma_x \cdot \sigma_p \geq \frac{\hbar}{2}$$

- 🔵 $\sigma_x$ — standard deviation of position measurements (how spread-out they are)
- 🔵 $\sigma_p$ — standard deviation of momentum measurements
- 🟡 $\hbar$ — reduced Planck constant ($\approx 10^{-34}$ J·s) — the scale of quantum effects
- 🟢 $\geq$ — the product can be larger but never smaller; this is a hard lower bound

### The Born Rule

$$P(\text{outcome } x) = |\langle x|\psi\rangle|^2$$

- 🔵 $\langle x|$ — the bra (row vector conjugate transpose) of basis state $|x\rangle$
- 🟡 $\langle x|\psi\rangle$ — inner product: a single complex number measuring "how much $|x\rangle$ is in $|\psi\rangle$"
- 🟢 $|\cdot|^2$ — squared magnitude: converts complex amplitude to real probability

```python
# The Born rule in full generality
def born_rule(basis_state, quantum_state):
    """P(outcome=basis_state) = |⟨basis_state|quantum_state⟩|²"""
    inner = np.dot(basis_state.conj(), quantum_state)
    return abs(inner)**2

# Test on the Bell state (|00⟩ + |11⟩)/√2
bell = np.array([1, 0, 0, 1], dtype=complex) / np.sqrt(2)

ket_00 = np.array([1, 0, 0, 0], dtype=complex)
ket_11 = np.array([0, 0, 0, 1], dtype=complex)
ket_01 = np.array([0, 1, 0, 0], dtype=complex)

print("Bell state measurement probabilities:")
print(f"  P(|00⟩) = {born_rule(ket_00, bell):.3f}")   # 0.500
print(f"  P(|11⟩) = {born_rule(ket_11, bell):.3f}")   # 0.500
print(f"  P(|01⟩) = {born_rule(ket_01, bell):.3f}")   # 0.000 — entanglement!
```

---

## B.7 Quick Reference

### Complex Number Operations

| Operation | Formula | Code |
|-----------|---------|------|
| Create | $z = a + bi$ | `z = a + b*1j` |
| Real part | $\text{Re}(z) = a$ | `z.real` |
| Imaginary part | $\text{Im}(z) = b$ | `z.imag` |
| Magnitude | $\|z\| = \sqrt{a^2+b^2}$ | `abs(z)` |
| Conjugate | $z^* = a - bi$ | `z.conjugate()` |
| Phase | $\phi = \arctan(b/a)$ | `np.angle(z)` |
| Polar form | $z = re^{i\phi}$ | `r * np.exp(1j*phi)` |

### Linear Algebra Operations

| Operation | Meaning | Code |
|-----------|---------|------|
| Vector norm | $\|\|\psi\rangle\| = \sqrt{\langle\psi\|\psi\rangle}$ | `np.linalg.norm(psi)` |
| Tensor product | $\|00\rangle = \|0\rangle \otimes \|0\rangle$ | `np.kron(ket_0, ket_0)` |
| Matrix-vector | $U\|\psi\rangle$ | `U @ psi` |
| Inner product | $\langle\phi\|\psi\rangle$ | `np.dot(phi.conj(), psi)` |
| Conjugate transpose | $U^\dagger$ | `U.conj().T` |
| Unitarity check | $U^\dagger U = I$? | `np.allclose(U.conj().T @ U, np.eye(n))` |
| Expectation value | $\langle\psi\|O\|\psi\rangle$ | `np.real(np.dot(psi.conj(), O @ psi))` |

### The Minimum You Must Know

If you take nothing else from this appendix:

:::{admonition} The Essential Five
:class: important

1. **Complex amplitudes** — quantum states use complex numbers because phase enables interference
2. **$|\alpha|^2$ = probability** — squared magnitude converts amplitude to measurement probability (Born rule)
3. **Vectors = quantum states** — a qubit is a 2D complex unit vector; $n$ qubits need $2^n$ entries
4. **Matrices = gates** — quantum gates are unitary $2^n \times 2^n$ matrices; `@` applies them
5. **Inner product = overlap** — $|\langle x|\psi\rangle|^2$ is the probability of measuring $|x\rangle$ in state $|\psi\rangle$

With these five tools, you can read and work with 90% of the equations in quantum computing literature.
:::

---

## Further Practice

The best way to get comfortable with this math is to run code. These libraries let you work with quantum states and gates directly:

```python
# Option 1: NumPy (what we used in this appendix)
import numpy as np
# Manual matrix-vector multiplication — full control, educational

# Option 2: Qiskit (IBM's quantum computing SDK)
from qiskit.quantum_info import Statevector, Operator
psi = Statevector.from_label('0')   # |0⟩
# Apply gates, compute expectation values, visualize on Bloch sphere

# Option 3: QuTiP (Quantum Toolbox in Python)
import qutip as qt
psi = qt.basis(2, 0)   # |0⟩ as a QuTiP ket object
# Full quantum mechanics: density matrices, Lindblad evolution, master equations
```

*Install with: `pip install numpy qiskit qutip`*

---

*With this mathematical foundation, every equation in this book is readable. The physics chapters told you what quantum mechanics does; this appendix tells you how the math works underneath. You are now equipped to move to Appendix C (further reading) and from there, into quantum computing practice.*
