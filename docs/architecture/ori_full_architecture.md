# ORI · Conversational BI Insight Engine

Mermaid architecture map for the current v0.8 preparation state.

```mermaid

flowchart TD

%% =========================
%% ENTRYPOINT
%% =========================
Q[User Question] --> CLI[cli.run_report_openai]

%% =========================
%% DATA PIPELINE
%% =========================
CLI --> CFG[ConfigLoader / dataset_config.yml]
CFG --> DI[DataIntake]
DI --> DF[DataFrame]

DF --> PROF[DataProfiler]
PROF --> STAT[stat_summary]
PROF --> QUAL[quality_report]

%% =========================
%% KPI SCOPE
%% =========================
STAT --> KSEL[KPI Selector]
KSEL --> KFOCUS[kpi_focus]
KSEL --> KDATA[kpi_data]
KSEL --> KALL[kpi_data_all]

%% =========================
%% CONTEXT LAYER
%% =========================
CLI --> ENGINE[InsightEngine.answer_question]

STAT --> PREP[_prepare_qa_context]
QUAL --> PREP
KFOCUS --> PREP
KDATA --> PREP
KALL --> PREP

PREP --> CTX[Runtime Context]
PREP --> BICTX[BIContext]

%% =========================
%% POLICY / CONTRACT LAYER
%% =========================
ENGINE --> CONTRACT[Contract Validator]
BICTX --> CONTRACT

CONTRACT -->|ASK| ASK[Clarification Response]
CONTRACT -->|REFUSE| REFUSE[Refusal Response]
CONTRACT -->|OK| ROUTER[Deterministic Router]

%% =========================
%% DETERMINISTIC TOOL LAYER
%% =========================
ROUTER --> INTENTS[Intent Detection]
INTENTS --> TOOLSEL[Tool Selection]

TOOLSEL --> TABLE[table_tool]
TOOLSEL --> TREND[trend_summary_tool]
TOOLSEL --> YOY[yoy_tool]
TOOLSEL --> DIV[divergence_tool]
TOOLSEL --> MTR[multi_trend_tool]
TOOLSEL --> QUALITY[quality_tool]

TABLE --> DETOUT[Deterministic Output]
TREND --> DETOUT
YOY --> DETOUT
DIV --> DETOUT
MTR --> DETOUT
QUALITY --> DETOUT

%% =========================
%% CALLBACK / UTILITY LAYER
%% =========================
ROUTER --> TSU[time_series_utils]
ROUTER --> FMT[formatting_utils]
ROUTER --> SYSMSG[system_messages]

TSU --> TABLE
TSU --> YOY
TSU --> TREND
TSU --> DIV
TSU --> MTR

FMT --> TABLE
FMT --> YOY
FMT --> TREND
FMT --> DIV
FMT --> MTR

SYSMSG --> DIV
SYSMSG --> MTR
SYSMSG --> QUALITY

%% =========================
%% CONTROLLED LLM LAYER
%% =========================
DETOUT --> COMMENT_CHECK[_can_add_llm_commentary]

COMMENT_CHECK -->|yes| LLM_COMMENT[LLM Commentary Only]
COMMENT_CHECK -->|no| FINAL_DET[Final Deterministic Answer]

LLM_COMMENT --> FINAL_DET

%% =========================
%% FALLBACK LAYER
%% =========================
ROUTER -->|no deterministic answer| FALLBACK_CONTRACT[Fallback Contract Validator]

FALLBACK_CONTRACT -->|REFUSE| FALLBACK_BLOCK[Blocked Fallback]
FALLBACK_CONTRACT -->|OK| LLM_FALLBACK[LLM Fallback]

LLM_FALLBACK --> GUARD[No New Metrics Check]
GUARD -->|fail| METRIC_BLOCK[Blocked Output]
GUARD -->|pass| FINAL_LLM[Final LLM Answer]

%% =========================
%% OBSERVABILITY
%% =========================
ENGINE --> DLOG[Decision Logger JSONL]
ENGINE --> TLOG[Technical Logger ORI_DEBUG]

%% =========================
%% OUTPUT
%% =========================
ASK --> FINAL[Final Answer]
REFUSE --> FINAL
FINAL_DET --> FINAL
FINAL_LLM --> FINAL
FALLBACK_BLOCK --> FINAL
METRIC_BLOCK --> FINAL

```
