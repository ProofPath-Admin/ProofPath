# Proofpath

**Deterministic claims about software behavior from compiler-verified structure.**

Proofpath is not a traditional static analyzer. Proofpath is a deterministic claim engine.
Static analyzers attempt to detect potential defects. Proofpath establishes verifiable structural truth.

It answers precise, bounded questions about program structure and behavior and produces verifiable evidence for each claim.
A claim is a precise statement about program structure or behavior that can be derived and reproduced under defined conditions.

In other words it proves what exists, doesn't guess what it cannot prove and unknown unknown states are represented explicitly.


---
## Example questions Proofpath may answer:

- Does function A call function B in this build?
- What allocation path leads to this memory write?
- Where is this macro expanded in the compiled program?
- What code paths can reach this state?

---

## Example answers Proofpath may give:
---

Query:
Does function A call function B?

Result:
Status: PROVEN
Evidence: Call expression found in A at src/foo.cpp:128
Scope: Build clang-18.1.0 -O2

---

Query:
Does function A call function C?

Result:
Status: FALSE
Evidence: No call path found in extracted call graph
Scope: Build clang-18.1.0 -O2

---

Query:
Can user input reach this memory write?

Result:
Status: UNKNOWN
Reason: Required structural facts unavailable (external dependency)
Scope: Build clang-18.1.0 -O2

---

## Why Proofpath Exists

Modern software systems are difficult to reason about reliably at scale. Most existing tools rely on heuristics, probabilistic interpretation, or incomplete approximations of program behavior. These approaches can produce results that are difficult to reproduce, verify, or audit.

Proofpath explores a different model:

- Operate on compiler-verified structural facts  
- Constrain reasoning to a bounded environment  
- Produce explicit evidence for every claim  
- Fail when certainty cannot be established  

The goal is deterministic understanding of software structure within defined scope.

---

## Core Properties

Proofpath is designed around four principles.

### 1. Compiler-Verified Structural Facts
Analysis operates on normalized program structure extracted from compiler outputs.

### 2. Bounded Deterministic Reasoning
All results are constrained to a specific build, environment, and query scope.

### 3. Auditable Evidence Chains
Each claim includes the facts and reasoning steps used to derive it.

### 4. Accumulating Derived Knowledge
Verified results are stored and reused safely, enabling compounding structural understanding.

---

## System Guarantees

Within its defined scope, Proofpath provides:

- Deterministic results under identical build conditions  
- Reproducible claims tied to a specific execution context  
- Explicit reasoning steps for every result  
- Bounded validity of all outputs  
- No hidden heuristics or probabilistic inference  
- Fail-loud behavior when claims cannot be established  

---

## Non-Goals

Proofpath does **not** attempt to:

- Prove full program correctness  
- Replace formal verification systems  
- Generate code automatically  
- Perform heuristic bug detection  
- Provide probabilistic predictions  
- Reason outside defined build bounds  

The system focuses exclusively on deterministic structural analysis.

---

## Intended Use

Proofpath is designed for environments that require:

- Reproducibility  
- Auditability  
- Explicit reasoning  
- High-assurance software analysis  
- Air-gapped operation  
- Deterministic tooling  

The architecture naturally aligns with safety-critical and mission-critical systems, but the system answers general bounded questions about software structure.

---

## Project Status

**Early development.**

Current work focuses on:

- Deterministic analysis core  
- Structural fact extraction  
- Reasoning model design  
- System architecture definition  

This repository primarily documents system design and principles.

---

## Documentation

See `/docs`:

- Origin — system motivation and development history  
- Principles — design philosophy and constraints  
- System Model — conceptual architecture  
- Roadmap — development stages  
- Glossary — terminology and definitions  

---

## Design Philosophy

Proofpath prioritizes:

- Correctness over convenience  
- Explicit reasoning over implicit behavior  
- Deterministic execution over probabilistic output  
- Auditable results over opaque inference  
- Long-term architectural stability  

---

## License

TBD.

---

## Status Disclaimer

Proofpath is experimental and under active design.  
System guarantees describe intended behavior and may evolve as implementation progresses.
