<p align="left">
  <img src="assets/logo_ori_minimal.svg" width="120"/>
</p>


# ORI · Conversational BI Insight Engine

![Version](https://img.shields.io/badge/CBI--Engine-v0.6.1-blue?style=flat-square)
![LLM Agnostic](https://img.shields.io/badge/LLM-Agnostic-forestgreen?style=flat-square)
![Dataset Agnostic](https://img.shields.io/badge/Dataset-Agnostic%20(YAML--Driven)-teal?style=flat-square)
![No Numerical Hallucinations](https://img.shields.io/badge/Design-No%20Hallucination-critical?style=flat-square)
![Deterministic Mode](https://img.shields.io/badge/Mode-Deterministic%20Tabular-orange?style=flat-square)
![PDF Reporting](https://img.shields.io/badge/Reporting-PDF%20Generation-lightgrey?style=flat-square)
![Architecture](https://img.shields.io/badge/Architecture-Modular%20·%20Pipeline--Driven-9cf?style=flat-square)


**Technical Architecture Overview (v0.6.1-stable)**

## **Executive Summary**

ORI is a Conversational Business Intelligence engine designed to solve a specific and increasingly critical problem:

**how to safely combine deterministic analytics with generative AI without compromising data integrity, auditability, or decision reliability.**

In many BI and analytics environments, Large Language Models are introduced as shortcuts:  
they calculate, infer, estimate, or “explain” numbers they should never touch.  
This creates an invisible risk layer: answers that *sound* correct but cannot be traced, reproduced, or defended.

ORI takes a different approach.

It enforces a strict architectural separation between:

* **deterministic computation**, and

* **generative language capabilities**.

The system ensures that:

* all numerical values are computed outside the LLM,

* the LLM never introduces new numbers, estimates, or derived metrics,

* every analytical statement can be traced back to an explicit data context.

The result is a conversational BI system suitable for **decision-makers, regulated environments, and audit-sensitive contexts**, not just exploratory demos.

---

## **What ORI Is (and What It Is Not)**

**ORI is:**

* a deterministic BI pipeline with a conversational interface,

* an architecture-first system, not a prompt experiment,

* designed for clarity, traceability, and governance.

**ORI is not:**

* a forecasting engine,

* a statistical inference system,

* a Retrieval-Augmented Generation (RAG) chatbot,

* a “let the model figure it out” assistant.

If a required value is not present in the provided data context, ORI explicitly states so.  
Silence, guessing, or approximation are considered system failures.

---

## **Core Architectural Principle**

**Language models narrate. They do not calculate.**

All calculations, aggregations, KPI selection, and data validation are performed deterministically, using explicit rules and reproducible logic.

The LLM is introduced **only after**:

* the data has been profiled,

* KPIs have been selected (with optional human-in-the-loop validation),

* quality constraints are known.

Its role is limited to:

* structured commentary,

* explanation of already computed results,

* contextualisation for business decision-makers.

This constraint is not a limitation.  
It is the key design feature.

---

## **Why This Matters for Decision-Makers**

ORI was built with real operational concerns in mind:

* **Auditability**  
   Every number in the output can be traced to a deterministic computation step.

* **Governance**  
   The system makes explicit what it can and cannot say, instead of hiding uncertainty behind fluent language.

* **Risk Control**  
   No silent assumptions. No implicit forecasting. No hallucinated metrics.

* **Operational Credibility**  
   Outputs are designed to support decisions, not just conversations.

This makes ORI particularly relevant for:

* internal BI teams,

* compliance-aware organisations,

* regulated industries,

* environments where “explainability” is not optional.

---

## **System Status – v0.6-stable**

Version **v0.6-stable** represents a frozen architectural baseline.

At this stage:

* the core pipeline is stable and reproducible,

* deterministic and generative responsibilities are fully separated,

* KPI selection logic is explicit and reviewable,

* conversational Q\&A operates within strict, declared constraints.

Future evolutions (v0.7+) will focus on **controlled context expansion**, starting with multi-file ingestion, without altering the core architectural guarantees.

---

## **Access and Availability**

ORI is an active, privately maintained project.

The full architecture, codebase, and execution details are intentionally **not publicly distributed**.  
They are available **upon request**, in professional or evaluative contexts.

---

### **⬛ End of Section 1**

---


## **Architecture Overview**

ORI is designed as a **layered, responsibility-driven system**, where each component has a clearly bounded role.  
 This structure is intentional: it prevents capability overlap, implicit behaviour, and hidden dependencies.

At a high level, the system is organised around three core layers:

1. **The Runner (Orchestration Layer)**

2. **The Deterministic Analytics Pipeline**

3. **The Insight Engine (LLM Layer)**

Each layer can evolve independently without breaking the system’s guarantees.

---

## **1\. The Runner . Orchestration and Control**

The runner is the **single source of truth** for execution.

It is responsible for:

* loading dataset configuration,

* resolving file paths and execution context,

* instantiating the analytics pipeline,

* passing explicit metadata to the Insight Engine.

Crucially, the runner:

* defines the agent identity (name, version),

* defines the dataset identity,

* controls which artefacts are persisted and where.

The Insight Engine does **not** infer or override this information.  
 If the runner omits a value, the omission remains visible.

This design choice ensures:

* reproducibility across executions,

* transparent versioning,

* no silent fallback behaviour.

From a governance perspective, the runner acts as the **accountable execution boundary**.

---

## **2\. Deterministic Analytics Pipeline**

All numerical reasoning in ORI happens **outside** the language model.

The deterministic pipeline is responsible for:

* dataset loading and schema inspection,

* statistical profiling and data quality assessment,

* explicit KPI candidate discovery,

* KPI ranking based on declared criteria (completeness, variability, trend signal),

* optional human-in-the-loop validation of KPI focus.

Every computation step is:

* explicit,

* reproducible,

* auditable.

The pipeline produces:

* structured summaries,

* quality reports,

* deterministic KPI time series,

* artefacts that can be persisted independently from the conversational layer.

This separation ensures that:

* analytical correctness does not depend on prompt behaviour,

* changes in language models do not affect numerical outcomes,

* the system remains defensible in review or audit scenarios.

---

## **3\. Insight Engine . Language Without Calculation**

The Insight Engine introduces generative AI **only after** deterministic processing is complete.

Its role is strictly constrained.

The engine:

* receives a bounded analytical context,

* generates commentary and explanation,

* answers user questions **without performing calculations**,

* explicitly refuses to answer when required values are missing.

It does not:

* compute aggregates,

* infer trends beyond what is already stated,

* estimate or forecast,

* reconstruct missing data.

This is enforced through:

* architectural separation,

* prompt constraints,

* explicit failure rules.

In practice, the language model acts as a **narrative layer**, not an analytical one.

---

## **Why This Separation Matters**

By isolating responsibilities, ORI avoids common failure modes observed in conversational analytics systems:

* **Hallucinated metrics**

* **Undocumented assumptions**

* **Irreproducible answers**

* **Implicit data leakage between contexts**

Instead, ORI behaves as a system where:

* analytics can be trusted independently from language fluency,

* conversational interaction does not weaken analytical guarantees,

* responsibility for decisions remains explicit and reviewable.

This architecture makes ORI suitable for environments where **explanations must be as reliable as the numbers themselves**.

---

## **Architectural Stability**

The boundaries between runner, pipeline, and engine are considered **stable in v0.6**.

Future versions will extend capabilities (e.g. multi-file ingestion, richer contextual composition) **without collapsing these boundaries**.

This is a deliberate design constraint, not a temporary limitation.

---

### **⬛ End of Section 2**

---


## **Key Design Decisions and Trade-offs**

ORI is the result of a series of **intentional design decisions**, each addressing a specific risk commonly observed in conversational analytics systems.

These decisions were not made to maximise flexibility or novelty, but to **minimise ambiguity, hidden assumptions, and operational risk**.

Below are the most significant ones.

---

## **1\. Deterministic First, Generative Second**

**Decision**  
All analytical computation is performed before any interaction with a language model.

**Rationale**  
LLMs are probabilistic systems.  
Using them to compute, infer, or reconstruct numerical values introduces an irreducible uncertainty layer that cannot be audited or reproduced.

**Trade-off**  
This approach limits the apparent “intelligence” of the assistant.  
ORI will not guess, interpolate, or infer missing values.

**Outcome**

* Numerical integrity is preserved.

* Analytical outputs remain reproducible.

* Explanations are tied to explicit facts, not latent model behaviour.

---

## **2\. No Retrieval-Augmented Generation (RAG) by Default**

**Decision**  
ORI does not use RAG for numerical or analytical reasoning.

**Rationale**  
While RAG improves recall, it introduces:

* uncontrolled context mixing,

* implicit prioritisation of retrieved fragments,

* difficulty in proving which sources influenced a specific answer.

In BI and decision-support contexts, this weakens accountability.

**Trade-off**  
Context must be explicitly constructed and bounded.  
This requires more upfront design and discipline.

**Outcome**

* Every answer is traceable to a known context.

* The system never blends heterogeneous sources implicitly.

* Responsibility for data inclusion is explicit.

---

## **3\. Explicit Failure Over Silent Fallbacks**

**Decision**  
When required information is missing, ORI explicitly states that the context does not contain the necessary values.

**Rationale**  
Silence, approximation, or “best guess” behaviour erodes trust faster than refusal.

**Trade-off**  
The system may appear less helpful in the short term.

**Outcome**

* Users learn the real limits of the data.

* Decision-makers are not misled by confident but unfounded statements.

* Errors remain visible instead of being masked.

---

## **4\. Human-in-the-Loop KPI Selection**

**Decision**  
Key Performance Indicators are ranked deterministically, but final selection can involve explicit human validation.

**Rationale**  
KPI relevance is partly technical and partly contextual.  
Automating this decision entirely hides assumptions that should remain reviewable.

**Trade-off**  
An extra interaction step is introduced during setup.

**Outcome**

* KPI focus is transparent and defensible.

* Business context is preserved.

* The system supports decision-making rather than replacing it.

---

## **5\. Runner as Single Source of Truth**

**Decision**  
Execution metadata (agent identity, versioning, dataset identity, paths) is defined exclusively by the runner.

**Rationale**  
Allowing components to self-identify creates version drift and hidden inconsistencies.

**Trade-off**  
More responsibility is placed on orchestration.

**Outcome**

* Clear execution accountability.

* Reproducible runs.

* No implicit or duplicated configuration.

---

## **6\. Constraints as a Feature, Not a Limitation**

**Decision**  
ORI deliberately restricts what the language model is allowed to do.

**Rationale**  
In decision-support systems, freedom is not a virtue if it reduces reliability.

**Trade-off**  
ORI prioritises correctness and clarity over conversational breadth.

**Outcome**

* Outputs are suitable for professional review.

* The system aligns with governance and compliance expectations.

* Trust is built through predictability, not novelty.

---

## **Design Philosophy Summary**

ORI is not optimised to “sound smart”.  
It is optimised to **remain correct, explainable, and defensible under scrutiny**.

Every limitation in the system corresponds to a risk that has been consciously excluded.

---

### **⬛ End of Section 3**

---


## **Guarantees and Boundaries**

ORI is designed to provide **explicit guarantees** about what the system does and, equally important, what it deliberately does not do.

These guarantees are not aspirational.  
They are enforced through architecture, execution flow, and failure rules.

---

## **Analytical Guarantees**

ORI guarantees that:

### **1\. No Hallucinated Numbers**

The system never introduces numerical values that are not explicitly present in the deterministic analytical context.

All numbers:

* originate from pre-computed deterministic steps,

* are passed to the language model as immutable context,

* can be traced back to a specific computation.

If a value is missing, ORI does not infer or estimate it.

---

### **2\. Reproducible Outputs**

Given the same dataset and configuration, ORI produces the same analytical results.

Language variability may affect phrasing, but:

* numerical content,

* analytical conclusions,

* KPI focus

remain stable across runs.

This enables:

* internal review,

* comparison over time,

* defensible reporting.

---

### **3\. Explicit Data Quality Awareness**

Data quality signals (e.g. missingness, variability) are computed deterministically and made visible to the system.

Analytical commentary is produced **with knowledge of these constraints**, not in spite of them.

This prevents overconfidence when data completeness is partial or uneven.

---

## **Conversational Boundaries**

ORI enforces clear conversational limits.

The system will explicitly refuse to:

* forecast future values,

* perform statistical inference,

* reconstruct missing data,

* answer questions that require unavailable context.

Refusal is considered a correct outcome when necessary.

This behaviour is consistent across:

* reports,

* conversational Q\&A,

* derived mini-reports.

---

## **Responsibility Boundaries**

ORI does not:

* replace human judgement,

* make autonomous decisions,

* optimise or recommend actions beyond what the data explicitly supports.

Instead, it supports:

* structured understanding,

* informed discussion,

* accountable decision-making.

Responsibility remains with the user.

---

## **Audit and Review Readiness**

ORI is designed to be **audit-aware**, even in environments where formal audit is not required.

The system structure supports:

* inspection of analytical inputs,

* review of KPI selection logic,

* traceability between outputs and data sources.

This makes ORI suitable for contexts where:

* explanations must survive scrutiny,

* decisions may need post-hoc justification,

* transparency is a requirement, not a preference.

---

## **Boundary as a Design Asset**

The boundaries enforced by ORI are intentional.

They:

* reduce systemic risk,

* prevent misinterpretation,

* create predictable behaviour.

In ORI, **constraint is treated as an asset**, not a limitation.

---


### **⬛ End of Section 4**

## **v0.6-stable Scope and Future Evolution**

Version **v0.6-stable** represents a **consolidated architectural baseline** for ORI.

At this stage, the system is not a prototype in flux, but a controlled, reproducible foundation on which further capabilities can be built without compromising its core guarantees.

---

## **What v0.6.1-stable Delivers**

In its current state, ORI provides:

* a fully deterministic analytical pipeline,

* explicit KPI discovery and ranking logic,

* optional human-in-the-loop KPI validation,

* a constrained conversational interface for explanation and Q\&A,

* strict separation between computation and language,

* reproducible outputs and audit-ready behaviour.

The architecture, execution flow, and responsibility boundaries described in this document are considered **stable**.

Changes to phrasing, prompt tuning, or internal refactoring do not alter these guarantees.

---

## **What v0.6.1-stable Deliberately Does Not Include**

To preserve clarity and reliability, v0.6-stable explicitly excludes:

* multi-dataset or multi-file ingestion,

* cross-document reasoning,

* autonomous insight generation beyond declared KPIs,

* forecasting, simulation, or optimisation logic,

* adaptive or memory-based conversational behaviour.

These exclusions are intentional.  
 They prevent scope creep and protect the system’s analytical integrity.

---

## **Planned Evolution (v0.7 and Beyond)**

Future versions of ORI will focus on **controlled context expansion**, not architectural redesign.

Planned directions include:

### **Multi-File and Multi-Source Contexts**

* ingestion of multiple structured inputs,

* explicit context composition rules,

* preservation of traceability across sources.

### **Context-Aware Document Reading**

* structured document ingestion (reports, tables),

* deterministic extraction of relevant values,

* bounded exposure of extracted content to the Insight Engine.

### **Extended Audit Instrumentation**

* richer execution metadata,

* clearer lineage between inputs, transformations, and outputs,

* improved support for review and validation workflows.

Crucially, these evolutions will **not** relax the core constraints:

* deterministic first,

* explicit failure over inference,

* language as narration, not computation.

---

## **Design Commitment**

ORI is not evolving toward a general-purpose AI assistant.

It is evolving toward a **reliable analytical companion**, designed to support human decision-making in contexts where correctness, transparency, and accountability matter.

Every future extension will be evaluated against this commitment.

---

## **Availability and Access**

ORI is an actively developed private project.

The architecture, execution model, and selected outputs are shared for professional evaluation and discussion.  
Full implementation details are available **upon request**, in appropriate contexts.

Public reference build: ORI v0.6.1 (GitHub Release).
This page documents the frozen baseline currently exposed.

---

### **⬛ End of Section 5**

---


## **Author and Disclosure**

**Author**
ORI has been designed and developed by Virginia Levy Abulafia.

**Disclosure**
The development of this system was carried out by Virginia Levy Abulafia, with collaborative support from ChatGPT for selected activities including architectural review, debugging assistance, prompt drafting, and iterative design refinement.

All technical decisions, system architecture, execution logic, data handling choices, and code implementation are the direct responsibility and work of the author.

ChatGPT was used strictly as a support tool and did not autonomously design, implement, or make decisions about the system.



### **⬛ End of Technical README (v0.6-stable)**

