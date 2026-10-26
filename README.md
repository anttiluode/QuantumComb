# The Quantum Comb

**Live page:** https://anttiluode.github.io/QuantumComb/

The quantum sibling of [The Comb](https://github.com/anttiluode/TheComb). In The Comb, a stack of ReLU layers is a lathe: each comb (a row of weighted teeth) presses on a sheet of paper and cuts grooves on the next sheet, and a negative pressure lifts the stylus. This page asks what happens when the paper follows quantum rules.

It is a single `index.html` with no build step and no server. Everything runs locally in plain JavaScript.

## What changes when the comb goes quantum

- **The paper.** Each cell is a qubit. A sheet no longer holds one groove per cell. It holds one complex amplitude for every whole pattern of the n cells: 2ⁿ numbers. Eight qubits make 256 amplitudes; 96 qubits would make 2⁹⁶.
- **The teeth.** Each comb is a row of two-qubit gates on neighbouring pairs, `G = exp(−i·c·Z⊗Z) · (Rx(a) ⊗ Rx(b))`. "Twist" sets the Rx angles (mixing) and "grip" sets c (entangling). Every other comb is shifted one cell round the ring (a brickwork circuit), so the comb turns as it goes.
- **Three new rules:**
  - **It never crushes.** The combs are unitary. Σ|a|² stays exactly 1, there is no ReLU, and any cut can be undone by running the combs backwards.
  - **No sheet can be copied** (no-cloning). A real device has one sheet, cut in place. Only a classical simulator can keep every sheet.
  - **You see one groove per spin.** A reading returns one pattern, drawn with probability |a|². n qubits give n bits per reading, however many amplitudes were inside (Holevo's bound).

## The three schedules

The page computes the same amplitude ⟨y|U|x⟩ three ways:

$$\langle y|U_L\cdots U_1|x\rangle=\sum_{z_1,\dots,z_{L-1}}\ \langle y|U_L|z_{L-1}\rangle\cdots\langle z_2|U_2|z_1\rangle\langle z_1|U_1|x\rangle$$

| Mode | Paper held at once | Work | Classical counterpart in The Comb |
|---|---|---|---|
| **Schrödinger** | (L + 1) · 2ⁿ amplitudes (or 2 · 2ⁿ with two alternating sheets) | about L · 2ⁿ per layer | Spin: keep every sheet |
| **Feynman** | L − 1 bookmark patterns of n bits + L partial products + 1 sum | 2^{n(L−1)} histories | Regrow: depth first |
| **Measure** (the physical device) | n qubits | L layers, then n bits per shot | none: the bill moves to the readout |

- **Schrödinger** keeps every sheet, so you can watch the rings fill. *Run backwards* applies U† comb by comb, and the input pattern comes back with probability 1.000000000000.
- **Feynman** enumerates histories depth first, like an odometer. The bookmarks are the intermediate patterns z₁ … z_{L−1}. The running sum is drawn in the complex plane: it wanders, spirals and folds back as histories cancel, and ends on the amplitude the kept sheets give. This schedule is the standard proof that BQP ⊆ PSPACE (Bernstein & Vazirani, 1993), the Regrow button of The Comb with complex teeth.
- **Measure** fires shots at the final sheet. The histogram converges to |a|², with a ±√(p(1−p)/shots) estimate for the chosen outcome. The phase strip under the bars shows what shots never see.

## How to use it

- Click a cell on the **innermost ring** to flip an input bit.
- Click the **outer halo** (all 2ⁿ amplitudes of the newest sheet: length |a|, colour = phase) to choose the outcome y.
- On each sheet, a cell's groove is the chance that qubit reads 1. A dashed blue halo round a socket means the qubit is entangled with the rest of the ring, so its own cell can't hold its description. The groove fades with the qubit's Bloch-vector length.
- Feynman history counts double with every qubit in every layer. 8 qubits × 3 combs is 65,536 histories; 12 × 4 is 2³⁶, which the page tells you not to wait for.

## What was checked

Over eight settings (n = 4–10 qubits, 1–5 combs):

- every gate is unitary to 2 × 10⁻¹⁶;
- Σ|a|² stays 1 to within 9 × 10⁻¹⁶ on every sheet;
- running the combs backwards returns the input with probability 1 to within 10⁻¹⁵;
- the Feynman path sum equals the Schrödinger amplitude to within 7 × 10⁻¹⁵, for three different outcomes each. The difference is rounding, because the two schedules add in different orders.

## What this is not

- **Not a quantum computer.** It is a classical simulation, so it pays the classical bills shown in the ledger.
- **Not a speedup claim.** The physical comb is cheap in space and time only because the readout is expensive: one random n-bit pattern per run. Useful quantum algorithms are the art of arranging interference so the answer is likely.
- **Not a noise model.** Real devices decohere. Quantum error correction can buy exactness back if the error rate per gate is below a threshold, at a large overhead.

## Related ideas

- **Reversibility instead of erasure.** A classical comb that crushes values must keep them or regrow them. A quantum comb never crushes, so side results must be *uncomputed* by running backwards. Bennett's reversible pebbling (1989) gives the time–space trade for that, and it is the reversible cousin of the regrowing tree in Ryan Williams' *Simulating Time With Square-Root Space* (2025).
- **Measurement and ReLU.** ReLU is a deterministic projection onto a cone (max(0, x) is the nearest point with no negative part). Measurement is a random projection onto a subspace. Each is the only irreversible step in its machine.
- **Simulation trade-offs.** Hybrid Schrödinger–Feynman simulators (e.g. Aaronson & Chen, 2017) trade between the two schedules, the same paper-for-presses dial as in Williams.

## Origin

Built by Claude (Opus 5.5) for Antti Luode, on the same day as The Comb, from his question: what if the comb is a field, and the field has quantum rules?

MIT licence (see `LICENSE`).
