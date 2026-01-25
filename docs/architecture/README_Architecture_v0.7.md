## **ORI Architecture (v0.7)**

This document describes the internal architecture of ORI as implemented in version 0.7.  
It is intended as a **technical reference** and reflects the system behavior as of the current release.

---

## **Architectural Layers**

### **1\. Deterministic Core**

The deterministic core is the **sole source of analytical truth**.

Responsibilities:

* data profiling and quality checks  
* KPI ranking and selection  
* deterministic computation of trends, growth, correlations, and divergence  
* explicit handling of missing data and anomalies

Characteristics:

* rule-based  
* reproducible  
* traceable  
* independent from language models

---

### **1.1 Deterministic Aggregations**

The deterministic core supports **constrained BI-style aggregations** over validated KPI series.

Supported operations:

* time-based aggregation (e.g. yearly)  
* `sum`, `mean`, `count`, `min`, `max`  
* year-over-year (YoY) percentage change  
* period-over-period variation  
* standardized divergence (Z-score) on a maximum of two KPIs

Aggregations are:

* executed exclusively in Python  
* applied only to KPIs explicitly included in the active analytical context  
* surfaced with explicit scope and limitation notes

---

### **2\. Governance and Constraints**

This layer enforces **non-negotiable architectural constraints**.

Constraints include:

* no implicit joins across datasets  
* no computation of KPIs outside the active analytical context  
* explicit limits on supported analytical operations  
* explicit failure or partial output for unsupported requests

If a user request implies a numeric aggregation, the aggregation **must** be computed deterministically.  
Language models are not allowed to introduce aggregated values or inferred metrics.

---

### **3\. Insight Engine**

The Insight Engine defines the **interpretation boundary** between analytics and language output.

Responsibilities:

* select admissible analytical signals  
* attach contextual warnings and quality notes  
* enforce architectural and contextual limits  
* prepare structured inputs for downstream output layers

Non-responsibilities:

* no numerical computation  
* no aggregation logic  
* no pattern discovery or inference beyond verified signals

---

### **4\. LLM Layer (Optional)**

When enabled, Large Language Models are used **exclusively for narrative synthesis**.

Properties:

* non-authoritative  
* replaceable via adapters  
* disabled by default in analytical validation workflows  
* operate only on structured inputs provided by the Insight Engine

LLMs never compute, aggregate, or interpret raw data.

---

### **5\. Output Layer**

Outputs include:

* user-facing textual responses  
* optional mini-report PDFs

Outputs are generated strictly from validated upstream inputs.   
No additional analytical logic is introduced at this stage.

---

## **Human-in-the-Loop**

Certain decisions, such as KPI focus selection, require explicit human confirmation.

This is a deliberate design choice favoring:

* accountability  
* analytical rigor  
* traceability

---

## **Architecture Artifacts**

* `ori_architecture_v0.7.mmd`  
   Source-of-truth architecture definition (Mermaid)

* `ori_architecture_v0.7.svg`  
   Rendered blueprint for documentation and presentation

---

## **Architectural Invariants**

The following invariants must hold across future versions:

* deterministic analytics remain the sole source of truth  
* aggregation logic remains deterministic and scoped  
* interpretation and language generation remain strictly separated  
* governance constraints are enforced by code, not prompts  
* no implicit assumptions or silent fallbacks are introduced

---

## **Version Scope**

This document reflects the architecture as of **ORI v0.7**.  
Any deviation in future versions must be explicitly documented.

