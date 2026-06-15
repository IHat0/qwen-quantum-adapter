# Qwen2.5-Coder-7B Quantum DSL Adapter

This repository contains the fine-tuned LoRA (Low-Rank Adaptation) adapter weights for `Qwen2.5-Coder-7B-Instruct` [1, 2]. The model is trained to act as a secure compiler, translating natural language research queries into a custom, highly structured Quantum Domain-Specific Language (DSL) [3]. This DSL is designed to be parsed and executed safely on quantum simulators and physical hardware via Qiskit [3].

---

## What It Is & What It Does

In conversational quantum computing systems, asking an LLM to generate raw Python/Qiskit code on the fly is highly unstable due to API deprecations, syntax hallucinations, and execution safety risks [3]. 

This adapter solves that problem by acting as an intermediary **compiler**. It restricts the LLM's output to a strict, structured DSL rather than unconstrained code [3]. This output is easily parsed, validated, and executed by a classical software boundary (the "Bouncer" shield), ensuring stable execution [3].

### Supported Translations
1. **Quantum Superposition / Randomness:**
   - *User Input:* "Can you do a quantum coin flip using 3 qubits?"
   - *DSL Output:* `[ACTION: RANDOM] [QUBITS: 3]`
2. **Quantum Chemistry (H₂ VQE):**
   - *User Input:* "Hey, compute the ground state energy of hydrogen with a distance of 1.4 Angstroms."
   - *DSL Output:* `[ACTION: VQE] [DISTANCE: 1.4]`

---

## How It Works

The architecture relies on a **Neuro-Symbolic** loop, separating the creative translation of natural language from the strict mathematical execution of the quantum circuit [3, 4].
