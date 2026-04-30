<p align="left">
  <img src="assets/logo_ori_minimal.svg" width="120"/>
</p>


![Version](https://img.shields.io/badge/CBI--Engine-v0.8-blue?style=flat-square)
![LLM Agnostic](https://img.shields.io/badge/LLM-Agnostic-forestgreen?style=flat-square)
![Dataset Agnostic](https://img.shields.io/badge/Dataset-Agnostic%20(YAML--Driven)-teal?style=flat-square)
![No Numerical Hallucinations](https://img.shields.io/badge/Design-No%20Hallucination-critical?style=flat-square)
![Deterministic Mode](https://img.shields.io/badge/Mode-Deterministic%20Tabular-orange?style=flat-square)
![PDF Reporting](https://img.shields.io/badge/Reporting-PDF%20Generation-lightgrey?style=flat-square)
![Architecture](https://img.shields.io/badge/Architecture-Modular%20·%20Pipeline--Driven-9cf?style=flat-square)


# ORI · Conversational BI Insight Engine

Deterministic analytics with controlled LLM narration.

---

## Overview

ORI is a Conversational BI system designed to ensure that:

* all numerical results are **computed deterministically (Python/pandas)**
* the LLM is used **only for explanation, never for calculation**
* every request is validated through an explicit **contract (ASK / REFUSE / OK)**

The goal is to make conversational analytics **reliable, traceable, and auditable**.

ORI treats governance as a runtime property, not as a documentation layer.

---

## Key Features

* Deterministic computation layer
* Contract-driven query validation
* Modular analytical tools (trend, YoY, divergence, quality)
* Human-in-the-loop KPI selection
* LLM fallback with guardrails (no new metrics)

---

## Architecture



The system is structured in layers:

* **Config (YAML)**
  Dataset definition, ingestion, KPI metadata

* **Data Intake**
  Multi-source ingestion + controlled merge

* **Profiling**
  Statistical summary + data quality

* **KPI Selection**
  Ranking + human validation

* **Insight Engine**

  * contract validation
  * deterministic dispatch
  * guarded LLM fallback

* **Tool Layer**

  * trend analysis
  * YoY growth
  * divergence (Z-score)
  * table extraction
  * quality analysis

---

## Governance by Design

ORI embeds governance directly into the system architecture.

Two formal policies define its behavior:

* **LLM Intervention Policy**
  Defines when and how the LLM is allowed to operate
  → `/docs/governance/LLM_Intervention_Policy.md`

* **Analytical Request Admission Policy**
  Defines the contract logic (ASK / REFUSE / OK)
  → `/docs/governance/Analytical_Request_Admission_Policy.md`

These policies ensure that:

* no numerical output is generated without deterministic computation
* ambiguous requests are explicitly handled
* system behavior is traceable and auditable

In ORI, governance is not documentation.
It is enforced at runtime.

---


## How It Works

1. User asks a question
2. Contract validation:

   * ASK → missing information
   * REFUSE → invalid request
   * OK → proceed
3. Deterministic computation (if possible)
4. Optional LLM narration (no calculations allowed)
5. Guardrails prevent hallucinated metrics

---

## Example

### 1. Deterministic answer

**User input**

```text
Mostra la tabella di Bologna_Passengers
```

**ORI output (deterministic)**

```text
1. Risposta sintetica
Tabella dei valori per Bologna_Passengers, estratta in modo deterministico da kpi_data.

2. Dettaglio numerico e tabella
Year | Value
2000 | 3524235
2001 | 3440639
...
2024 | 10775972

3. Limiti
Se mancano anni o valori in input, non vengono inventati.
```

---

### 2. Contract enforcement (no guessing)

**User input**

```text
Confronta la media dei voli nazionali e Bologna_Movements
```

**ORI output (contract)**

```text
[Calcolo deterministico non disponibile. Richiesta bloccata dal contratto]

Per rispondere in modo deterministico serve una scelta esplicita.

Scegli un secondo KPI tra:
Bologna_Passengers, Annual_CO2_Emissions, Bologna_Movements.
```

---

### 3. Deterministic comparison

**User input**

```text
Confronta Bologna_Passengers e Bologna_Movements: correlazione e divergenza
```

**ORI output (deterministic)**

```text
Correlazione complessiva: 0.825

Year | Bologna_Passengers | Bologna_Movements | Divergence_Z
2022 | 8496000 | 70871 | 3.67
2020 | 2506258 | 30139 | -1.93
...
```

---

### 4. Conceptual question handled safely

**User input**

```text
Mostrami gli anni peggiori per l’efficienza operativa
```

**ORI output**

```text
Nel contesto corrente non esiste una metrica unica chiamata 'efficienza'.

Possiamo operazionalizzarla usando KPI proxy:
- TOTAL_DELAY_MIN
- Annual_CO2_Emissions
- IFR_DEPARTURES

Indica quale proxy utilizzare.
```

---

### What this shows

* ORI computes metrics deterministically
* ORI blocks ambiguous or invalid requests
* ORI does not infer missing definitions
* ORI separates calculation from interpretation

---

## What ORI is NOT

### Not a RAG system

ORI **is not** a Retrieval-Augmented Generation system.

RAG architectures operate on **unstructured textual knowledge**.
ORI operates on **structured datasets**, computing deterministic metrics.

* No document retrieval
* No semantic embedding search
* No knowledge synthesis

All computations happen **outside the LLM**, in Python.

---

### Not a generative analytics engine

ORI does not:

* infer missing metrics
* approximate values
* generate plausible numbers

If data or meaning is missing → the system stops.

---

## Design Principles

* Deterministic first
* Explicit failure over silent correction
* No silent inference
* LLM as narrator, not calculator

> Without enforcement, governance is just intent.

---

## Project Structure

```text
config/
data/
src/
  core/
  pipeline/
  tools/
  viz/
  report/
cli/
outputs/
```

---

## Quick Start

```bash
cd ori
python -m cli.run_report_openai
```

---

## Current Status

v0.8 focuses on:

* deterministic tool extraction
* contract enforcement
* architecture stabilization

---

## Future Work

* derived KPI layer (v0.9)
* orchestration refactor
* extended governance modules

---

##  Disclosure

This project was developed by Virginia Levy Abulafia with collaborative support from ChatGPT for architectural review, debugging, prompt refinement, and design optimization.

All technical decisions, system architecture, and implementation choices are the direct responsibility of the author.

---


## License

This repository is released under a Source-Available License for evaluation and demonstration purposes.

The code may be read, executed, and reviewed for non-commercial evaluation.
Commercial use, redistribution, or derivative works require an explicit licensing agreement with the author.

For commercial inquiries or pilot programs, please contact the author.

---


## Author

Virginia Levy Abulafia
AI Operations Specialist

👉 https://virginialevy.com/

---