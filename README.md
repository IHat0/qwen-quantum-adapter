# Qwen2.5-Coder-7B Quantum DSL Adapter

This repository contains the fine-tuned LoRA (Low-Rank Adaptation) adapter weights for `Qwen2.5-Coder-7B-Instruct` [2, 3]. The model is trained to act as a secure compiler, translating natural language research queries into a custom, highly structured Quantum Domain-Specific Language (DSL) [1]. This DSL is designed to be parsed and executed safely on quantum simulators and physical hardware via Qiskit [1].

---

## What It Is & What It Does

In conversational quantum computing systems, asking an LLM to generate raw Python/Qiskit code on the fly is highly unstable due to API deprecations, syntax hallucinations, and execution safety risks [1]. 

This adapter solves that problem by acting as an intermediary **compiler**. It restricts the LLM's output to a strict, structured DSL rather than unconstrained code [1]. This output is easily parsed, validated, and executed by a classical software boundary (the "Bouncer" shield), ensuring stable execution [1].

### Supported Translations
1. **Quantum Superposition / Randomness:**
   - *User Input:* "Can you do a quantum coin flip using 3 qubits?"
   - *DSL Output:* `[ACTION: RANDOM] [QUBITS: 3]`
2. **Quantum Chemistry (H₂ VQE):**
   - *User Input:* "Hey, compute the ground state energy of hydrogen with a distance of 1.4 Angstroms."
   - *DSL Output:* `[ACTION: VQE] [DISTANCE: 1.4]`

---

## How It Works

The architecture relies on a **Neuro-Symbolic** loop, separating the creative translation of natural language from the strict mathematical execution of the quantum circuit [1, 4].

```
+-------------------------------------------------------+
|                 User Natural Query                    |
|  "Simulate Hydrogen molecule at 1.4 Angstroms"        |
+---------------------------+---------------------------+
                            |
                            v
+---------------------------+---------------------------+
|          Fine-Tuned Qwen-7B LoRA Adapter              |
|     Translates natural language to strict DSL         |
+---------------------------+---------------------------+
                            |
                            v [Compiled DSL Output]
               "[ACTION: VQE] [DISTANCE: 1.4]"
                            |
                            v
+---------------------------+---------------------------+
|          Classical Python Parser (The Bouncer)        |
|      Checks variable bounds & prevents execution      |
|           crashes or arbitrary code run               |
+---------------------------+---------------------------+
                            |
                            v [Clean, Validated Call]
             Local Qiskit AerSimulator Run              
                            |
                            v [Raw Measurements]
               {"raw_counts": {"01": 2000}}             
                            |
                            v
+---------------------------+---------------------------+
|               AI Physics Interpreter                  |
|     Explains raw counts back to user in English       |
+-------------------------------------------------------+
```

### The Fine-Tuning Process
The model was fine-tuned on a synthetic dataset of **1,200 conversational-to-DSL pairs** generated using randomized, physically realistic parameter variations ($N \in [1, 5]$ qubits, $D \in [0.5, 2.5]$ Å) [2]. 

Using QLoRA (Quantized Low-Rank Adaptation) on Kaggle Dual T4 GPUs, the model's training loss dropped from **`1.89`** to **`0.11`**, indicating stable convergence and high syntactic replication of the target DSL [2, 5, 6].

---

## Comparison with Similar Approaches

When building AI-Quantum interfaces, developers typically use one of two architectures. The table below compares the standard approach with this repository's custom DSL compilation pipeline [1].

| Feature | Standard LLM Code Gen (Raw Qiskit) | Custom DSL Adapter + Bouncer (This Project) |
| :--- | :--- | :--- |
| **Method** | LLM writes raw Python script containing Qiskit commands [1]. | LLM compiles parameters into a strict, validated schema [1]. |
| **Syntax Stability** | **Poor.** Frequently generates deprecated Qiskit functions or incorrect library imports [1]. | **High.** 100% stable since classical, pre-tested Python handles the Qiskit calls [1]. |
| **Security / Safety** | **Low.** Vulnerable to Prompt Injection where a malicious prompt forces the LLM to run arbitrary local OS commands [1]. | **High.** Sanitized by a regular-expression parser. Malformed syntax or non-DSL inputs are blocked [1]. |
| **VRAM Footprint** | Requires massive frontier models (e.g. GPT-4, Claude) to reliably output correct syntax [1]. | Runs efficiently on local consumer hardware using quantized 7B parameter adapters [3, 5]. |

