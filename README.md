<p align="left">
  <img src="assets/logo_ori_minimal.svg" width="120"/>
</p>


# ORI · Conversational BI Insight Engine

![Version](https://img.shields.io/badge/CBI--Engine-v0.6-blue?style=flat-square)
![LLM Agnostic](https://img.shields.io/badge/LLM-Agnostic-forestgreen?style=flat-square)
![Dataset Agnostic](https://img.shields.io/badge/Dataset-Agnostic%20(YAML--Driven)-teal?style=flat-square)
![No Numerical Hallucinations](https://img.shields.io/badge/Design-No%20Hallucination-critical?style=flat-square)
![Deterministic Mode](https://img.shields.io/badge/Mode-Deterministic%20Tabular-orange?style=flat-square)
![PDF Reporting](https://img.shields.io/badge/Reporting-PDF%20Generation-lightgrey?style=flat-square)
![Architecture](https://img.shields.io/badge/Architecture-Modular%20·%20Pipeline--Driven-9cf?style=flat-square)


**Technical Architecture Overview (v0.6-stable)**

1. **Project Overview**

- What ORI is
- Design philosophy: Python calculates. LLM narrates.

2. **Core Capabilities**

- Deterministic profiling and analytics
- Deterministic-first Q&A routing
- Reporting (PDF, charts)

3. **Architecture**

- High-level pipeline
- Core modules and responsibilities

4. **Key Design Decisions**
4.1 Determinism by Construction
4.2 Why Divergence Is Explicit and Limited
4.3 Multi-Source Controlled Context
4.4 No Implicit Joins, No Silent Data Fixes
4.5 Data Privacy by Architectural Separation
4.6 Auditability and Traceability

5. **Configuration**

- dataset_config.yml
- KPI focus and quality constraints

6. **Usage**

- CLI quickstart
- Example analytical questions

7. **Roadmap**

- v0.7 Stabilization
- v0.8 MCP Adapter Layer
- v0.9 GOFAI-lite Modules
- v1.0 Engine-grade release

8. **Limitations**

- Explicit scope and constraints
- Data quality and interpretability caveats

9. **License and Disclosure**

- AI-assisted development disclosure
- Governance and alignment notes

---


1. **Project Overview**

ORI (Conversational BI Insight Engine) is a deterministic-first analytical system designed to transform structured datasets into traceable, auditable insights through controlled language-model narration.

The core design principle of ORI is simple:

**Python calculates. The LLM narrates.**

All numerical computation, aggregation, profiling, and KPI evaluation are performed deterministically in Python. The language model is never responsible for calculations, estimations, or implicit data reasoning. Its role is limited to explanation, summarization, and structured commentary over already-computed results.

ORI is intentionally not a generic chat-with-your-data system and does not aim to replicate spreadsheet manipulation or free-form exploratory querying. It is designed for environments where correctness, traceability, and explicit assumptions matter more than conversational flexibility.


**Why This Matters**

In many BI and RAG-style systems, numerical hallucinations, silent joins, and implicit assumptions are common failure modes. ORI addresses these risks by enforcing architectural separation between computation and language generation, making analytical behavior explicit and auditable.

---


2. **System Architecture**

ORI follows a layered pipeline architecture, combining deterministic data processing with controlled generative narration.


**High-Level Components**

- **Runner / Entry Point**

	- Orchestrates configuration loading, data ingestion, profiling, and report generation.
	- Acts as the single source of truth for execution context.

- **Deterministic Data Pipeline**

	- Data intake and schema validation
	- Profiling and statistical computation (means, distributions, trends, YoY growth)
	- Data quality assessment

- **Insight Engine**

	- Routes user questions
	- Attempts deterministic answers first
	- Invokes the LLM only when narrative synthesis is required

The architecture is dataset-agnostic. Dataset-specific logic (column names, KPIs, constraints) is defined externally via configuration files, not hardcoded into the engine.

---

3. **Key Design Decisions and Trade-offs**
3.1 **Deterministic First, Generative Second**

All analytical results are computed before any language model interaction. The LLM never performs calculations and cannot introduce new numerical facts.

3.2 **No Retrieval-Augmented Generation (RAG) by Default**

ORI does not perform document chunking, embedding-based retrieval, or semantic search as a default mechanism. This avoids opaque context assembly and uncontrolled prompt injection.

3.3 **Explicit Failure Over Silent Fallbacks**

When a question cannot be answered deterministically or falls outside the available analytical context, ORI reports this explicitly rather than approximating or guessing.

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

---

4. **Guarantees and Boundaries**

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

5. **v0.6-stable Scope and Future Evolution**

Version v0.6-stable operates on a single active dataset per execution and does not perform implicit multi-dataset ingestion or cross-source computation.

However, the architecture already supports **multi-source controlled contexts** as a design principle, enabling future extensions without altering core guarantees.

Planned evolution includes:

- MCP adapter layer for external agent interoperability
- Advanced deterministic analytics modules
- Optional rule-based and forecasting components

6. **Availability and Access**

ORI is currently provided as a local, architecture-focused prototype intended for experimentation, learning, and portfolio demonstration.

7. **License and Disclosure**

This project was developed by Virginia Levy Abulafia with collaborative support from ChatGPT for architectural review, debugging, prompt refinement, and design optimization.

All technical decisions, system architecture, and implementation choices are the direct responsibility of the author.
