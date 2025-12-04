# 🌌 Quantum Holographic Codec

**A simulation of Quantum Data Compression and Encryption using Tensor Networks (MERA).**

This project demonstrates a quantum communication protocol where Alice maps high-dimensional classical data (8 data points) into a lower-dimensional quantum state (3 Qubits) using a "Holographic" encoding scheme.

## 📐 The Concept

In theoretical physics, **Holography** suggests that information contained in a volume of space can be represented by a theory on the boundary of that space. 

In this simulation:
1.  **Compression**: We use **Amplitude Embedding** to store $2^N$ classical data points into $N$ qubits. (Here: 8 numbers $\to$ 3 Qubits).
2.  **Encryption**: We apply a **MERA (Multi-scale Entanglement Renormalization Ansatz)** circuit. This circuit geometry and its parameters act as the "Key."
3.  **Transmission**: The compressed quantum state is sent to Bob.
4.  **Unzipping**: Bob applies the **mathematical inverse (adjoint)** of the MERA circuit. If he has the correct key, the data unfolds perfectly. If not, he sees garbage noise.

## 📂 Repository Structure

*   **`HolographicEncoder.ipynb`**: The Compressor. Alice takes an 8-point signal (like a time-series or image row), embeds it into quantum amplitudes, applies the Holographic MERA circuit, and saves the state.
*   **`HolographicDecoder.ipynb`**: The Decompressor. Bob receives the 3 qubits, applies the inverse circuit, and recovers the original 8 data points. It also simulates a hacker ("Eve") trying to decode with the wrong key.

## 🛠️ Prerequisites

Runs on local Python or Google Colab.

```bash
pip install pennylane numpy matplotlib
```

## 🚀 How to Run

### Step 1: Alice (Encode)
1.  Open `HolographicEncoder.ipynb`.
2.  Run the cells to define the secret data `[0.0, 0.1, 0.5, ...]` and the MERA circuit parameters.
3.  **Output**: A serialized file `holographic_transmission.pkl` containing the 3-qubit state vector.

### Step 2: Bob (Decode)
1.  Open `HolographicDecoder.ipynb`.
2.  Ensure `holographic_transmission.pkl` is accessible.
3.  Run the cells.
4.  **Output**: 
    *   A graph showing the perfectly recovered signal (Green).
    *   A graph showing the noise Eve sees by using the wrong key (Red).

## 🧠 Technical Details

### The Circuit (MERA)
We use a simplified MERA-like architecture consisting of:
*   **Entanglers (IsingXX)**: To spread information across qubits.
*   **Rotations (RY)**: To encode specific parameter dependencies.
*   **Mixers (CNOT)**: To create complex interference patterns.

### The Inverse
In PennyLane, we use `qml.adjoint(circuit)` to automatically generate the inverse of the unitary evolution. This mathematically guarantees that:
$$ U^{\dagger}_{Bob} \cdot U_{Alice} \cdot |\psi_{data}\rangle = |\psi_{data}\rangle $$
*Provided $U_{Bob}$ uses the exact same parameters as $U_{Alice}$.*

---
*Created by [Peter Babulik](https://github.com/peterbabulik)*
```
