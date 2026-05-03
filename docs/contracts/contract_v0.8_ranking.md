# 📊 ORI – Ranking Request Specification

**Version**: v0.8
**Type**: Contract specification (deterministic layer)

---

## 1. Purpose

This specification defines how ranking requests are represented and handled in a deterministic and auditable way.

It ensures that ranking operations:

* are explicitly defined
* do not rely on implicit assumptions
* follow contract validation rules

---

## 2. RankingRequest Schema

A ranking request is normalized into the following structure:

```json
{
  "mode": "numeric | semantic",
  "kpi": "string",
  "n": "integer (default 3, range 1–10)",
  "direction": "top | bottom",
  "scope": "all",
  "needs_better_when": "boolean",
  "better_when": "higher | lower | null",
  "period_term_used": "string | null",
  "raw_question": "string"
}
```

---

## 3. Field Definitions

### mode

* `numeric` → ranking based on numerical values
* `semantic` → ranking based on interpretation (“best”, “worst”)

---

### kpi

Target metric. Must be:

* explicitly identified
* present in `kpi_focus`
* computable

---

### n

Number of elements to return.
Default: 3
Recommended range: 1–10

---

### direction

* `top` → highest values
* `bottom` → lowest values

For `semantic` mode, direction depends on `better_when`.

---

### better_when (semantic only)

Defines what “better” means:

* `higher` → higher values are better
* `lower` → lower values are better

Required when semantic ambiguity cannot be resolved.

---

### period_term_used

Captures user wording such as:

* “years”
* “months”

Used for validation against dataset granularity.

---

### raw_question

Original user input, stored for traceability.

---

## 4. ASK Responses (Blocking)

The system must stop and request clarification in the following cases.

---

### ASK-1: Missing KPI

```
Ranking request detected, but no KPI was identified.

Specify a KPI and the number of elements (e.g., “top 3 highest values for X”).
```

---

### ASK-2: Semantic ambiguity

Trigger: “best/worst” without `better_when`.

```
For this KPI, it is not defined whether “better” means higher or lower values.

Do you want to consider higher or lower values as better?
```

---

### ASK-3: Granularity mismatch

Trigger: user refers to “years” but dataset is not annual.

```
The request refers to “years”, but the dataset is not annual (granularity: {granularity}).

Do you want to aggregate data by year before computing the ranking?
```

---

### ASK-4: Duplicate periods

Trigger: multiple values for the same period.

```
Multiple values were found for the same period in {kpi}.

Do you want to aggregate them (e.g., mean, sum, last value) or specify a rule?
```

---

### ASK-5: Insufficient data

Trigger: fewer valid points than requested `n`.

```
Not enough valid data points to compute a top/bottom {n} ranking for {kpi}.

Available points: {m}.
```

---

## 5. Design Constraints

* Ranking operates only on `kpi_data` (no joins)
* No implicit aggregation
* No derived KPI computation
* No silent correction

---

## 6. Design Note

Ranking is not a formatting operation.
It is a **contract-bound analytical decision**.
