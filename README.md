<p align="left">
  <img src="assets/logo_ori_minimal.svg" width="120"/>
</p>


# ORI · Conversational BI Insight Engine

![Version](https://img.shields.io/badge/CBI--Engine-v0.7-blue?style=flat-square)
![LLM Agnostic](https://img.shields.io/badge/LLM-Agnostic-forestgreen?style=flat-square)
![Dataset Agnostic](https://img.shields.io/badge/Dataset-Agnostic%20(YAML--Driven)-teal?style=flat-square)
![No Numerical Hallucinations](https://img.shields.io/badge/Design-No%20Hallucination-critical?style=flat-square)
![Deterministic Mode](https://img.shields.io/badge/Mode-Deterministic%20Tabular-orange?style=flat-square)
![PDF Reporting](https://img.shields.io/badge/Reporting-PDF%20Generation-lightgrey?style=flat-square)
![Architecture](https://img.shields.io/badge/Architecture-Modular%20·%20Pipeline--Driven-9cf?style=flat-square)


Technical Architecture Overview (v0.7)

1. Project Overview

2. System Architecture

3. Key Design Decisions and Trade-offs

4. Guarantees and Boundaries

5. v0.7 Architecture-Stabilized Scope

6. Roadmap

7. Availability and Access

8. License

9. Disclosure
   
---


## 1. Project Overview

ORI (Conversational BI Insight Engine) is a deterministic-first analytical system designed to transform structured datasets into traceable, auditable insights through controlled language-model narration.

It does not optimize for always answering.
It optimizes for answering safely, or not answering at all.

The core design principle of ORI is simple:

**Python calculates. The LLM narrates.**

All numerical computation, including aggregations, rankings, divergence measures, trends and YoY deltas, is performed deterministically in Python. The language model is never responsible for calculations, estimations, or implicit data reasoning. Its role is limited to explanation, summarization, and structured commentary over already-computed results.

ORI is conversational by design, but refuses to simulate understanding when analytical guarantees cannot be met.

**Why This Matters**

In many BI and RAG-style systems, numerical hallucinations, silent joins, and implicit assumptions are common failure modes. ORI addresses these risks by enforcing architectural separation between computation and language generation, making analytical behavior explicit and auditable.

---


## 2. System Architecture

ORI follows a layered pipeline architecture, combining deterministic data processing with controlled generative narration.


**High-Level Components**

- **Runner / Entry Point**

	- Orchestrates configuration loading, data ingestion, profiling, and report generation.
	- Acts as the single source of truth for execution context.

- **Deterministic Data Pipeline**

	- Data intake and schema validation
	- Profiling and statistical computation
      (aggregations, rankings, trends, YoY growth, correlations, divergence metrics)
	- Data quality assessment


- **Decision & Logging Layer (v0.7)**

Starting from v0.7, ORI introduces an explicit decision logging layer.

Each execution is associated with a unique run_id.
Analytical decisions such as KPI selection, routing outcomes, and Q&A resolution paths are recorded as structured artifacts.

This enables post-hoc inspection of what ORI decided, under which analytical assumptions, and with which limitations.

- **Insight Engine**

	- Routes user questions
	- Attempts deterministic answers first
	- Invokes the LLM only when narrative synthesis is required

The architecture is dataset-agnostic. Dataset-specific logic (column names, KPIs, constraints) is defined externally via configuration files, not hardcoded into the engine.

---

## 3. Key Design Decisions and Trade-offs
3.1 **Deterministic First, Generative Second**

All analytical results are computed before any language model interaction. The LLM never performs calculations and cannot introduce new numerical facts.


3.2 **No Retrieval-Augmented Generation (RAG) by Default**

ORI does not perform document chunking, embedding-based retrieval, or semantic search as a default mechanism. This avoids opaque context assembly and uncontrolled prompt injection.


3.3 **Explicit Failure Signaling (Partial, v0.7)**

In v0.7, ORI detects deterministic computation failures but does not yet enforce full analytical contract validation.
Explicit failure signaling is therefore partial by design and will be strengthened in future iterations.


3.4 **Human-in-the-Loop KPI Selection**

KPI focus is explicitly defined and reviewed. ORI does not automatically promote or invent KPIs based on model inference.


3.5 **Runner as Single Source of Truth**

All analytical context (statistics, quality metrics, KPI focus) is built once and passed downstream. No component recomputes or mutates analytical state implicitly.


3.6 **Constraints as a Feature, Not a Limitation**

ORI intentionally limits scope and flexibility to preserve correctness, interpretability, and auditability.


3.7 **Why Divergence Is Explicit and Limited**

ORI supports divergence-based analysis, a core concept in financial risk and performance monitoring, through explicit, deterministic, and user-defined metrics.

Divergence is never inferred implicitly by the language model. It is computed in Python, surfaced transparently, and narrated only after validation.


3.8 **Multi-Source Controlled Context**

ORI supports analysis over multiple structured datasets as a design principle, not as an implicit behavior.

- Each dataset is ingested and profiled independently.
- Schema, quality metrics, and KPI definitions are preserved per source.
- ORI does not perform implicit joins or automatic schema mapping.

Cross-dataset analyses are executed only when KPIs from multiple sources are explicitly included in the active analytical context.

When a user query references KPIs from multiple sources, ORI:

- recognizes all referenced KPIs,
- identifies which are computable in the current context,
- explicitly reports KPIs that are present but out of scope.

This prevents silent assumptions and hallucinated relationships between datasets.


3.9 **Data Privacy by Architectural Separation**

ORI is designed to minimize data exposure by construction, rather than relying on model-level privacy guarantees.

Raw datasets are ingested and processed locally by deterministic Python components. Sensitive data is never passed directly to the language model.

The LLM only receives:

- aggregated statistics,
- derived metrics,
- explicit analytical summaries,
- and, when strictly necessary, a minimal non-authoritative data sample for narrative grounding.

ORI does not claim absolute data privacy. Datasets are loaded into the system and must be handled according to the user’s operational and security context. ORI’s contribution is architectural: enforcing strict separation between data computation and language generation to reduce the privacy surface by design.


3.10 **Explicit Decision Logging and Auditability (v0.7)**

Starting from v0.7, ORI treats analytical choices as first-class artifacts.

KPI selection, analytical scope, and Q&A routing outcomes are persisted as structured logs, enabling traceability and audit-style inspection.

This design choice reinforces ORI’s positioning as a deterministic BI engine rather than a conversational assistant optimized for fluency.

---


## 4. Guarantees and Boundaries

ORI guarantees:

- No numerical hallucinations
- No implicit joins
- No silent data corrections
- No undocumented assumptions

ORI does **not** guarantee:

- Exhaustive exploratory analysis
- Automatic insight discovery
- Semantic reasoning beyond provided context

These boundaries are intentional and documented.

---


## 5. v0.7 Architecture-Stabilized Scope

Version v0.7 represents an architecture-stabilized milestone for ORI.

This release consolidates the system as a deterministic, auditable Business Intelligence engine with a governed conversational layer.

- **Scope and guarantees in v0.7**:

	- Run-scoped logging introduced
Each execution is associated with a unique run_id, enabling traceability of analytical decisions across profiling, KPI selection, reporting, and Q&A.

	- Persistent KPI selection snapshots
KPI ranking and final focus selection are stored as explicit decision artifacts, preserving the analytical context used for each run.

	- Deterministic Q&A behavior consolidated
Analytical questions are resolved deterministically whenever sufficient context is available.
Language models are used exclusively for constrained narrative explanation over pre-computed results.

	- Multi-dataset controlled context
ORI supports multiple structured datasets as a design principle.
Datasets are ingested and profiled independently, with no implicit joins or automatic cross-dataset inference.

	- No implicit analytical assumptions
ORI does not infer missing relationships, perform silent data corrections, or approximate numerical results.

	- Known architectural gap: analytical contract validation
Version v0.7 makes explicit a remaining architectural requirement: v0.7 surfaces the need for an explicit analytical contract validation step, which will be addressed in future iterations.
At present, ORI determines how to answer a question and whether sufficient data is available for deterministic computation, but it does not yet formally validate whether a user question is fully specified with respect to its implied analytical contract.
This capability is intentionally deferred to preserve correctness and avoid implicit assumptions, and it will be introduced in a subsequent release.**

---


## 6. Roadmap

v0.7 — Architecture-stabilized release (current)
- Deterministic analytics enforced
- Decision logging and traceability
- Human-in-the-loop KPI selection

v0.8 — MCP Adapter Layer
- External agent interoperability
- Controlled invocation of ORI as an analytical backend
- Explicit boundary between ORI’s deterministic core and external agent reasoning

Non-goal:  
v0.8 does not turn ORI into a general-purpose agent or delegate analytical authority to external models.

v0.9 — GOFAI-lite Modules
- Rule-based analytical reasoning
- Explicit analytical contracts
- Non-probabilistic decision layers

v1.0 — Engine-grade release
- Stable API surface
- Governance-oriented documentation

---


## 7. Availability and Access

ORI is currently provided as a local, architecture-focused prototype intended for experimentation, learning, and portfolio demonstration.

---


## 8. License

This repository is released under a Source-Available License for evaluation and demonstration purposes.

The code may be read, executed, and reviewed for non-commercial evaluation.
Commercial use, redistribution, or derivative works require an explicit licensing agreement with the author.

For commercial inquiries or pilot programs, please contact the author.

---

## 9. Disclosure

This project was developed by Virginia Levy Abulafia with collaborative support from ChatGPT for architectural review, debugging, prompt refinement, and design optimization.

All technical decisions, system architecture, and implementation choices are the direct responsibility of the author.
