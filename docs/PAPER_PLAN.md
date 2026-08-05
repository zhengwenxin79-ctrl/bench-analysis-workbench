# ARR Paper Plan: Benchmark Understanding

## Working Title

Benchmark Understanding: Evidence-Grounded Analysis of Evaluation Benchmarks for Large Language Model Agents

## Target Venue

Primary target: ARR October 2026 cycle.

Official ARR dates checked on 2026-08-05:

- August 2026 ARR submission date: 2026-08-03.
- October 2026 ARR submission date: 2026-10-12.
- October 2026 cycle end: 2026-12-20.

Sources:

- https://aclrollingreview.org/dates
- https://aclrollingreview.org/authors

## Core Positioning

This project should not be framed as a web crawler or a report UI. The research
claim is that model-evaluation benchmarks have become important scientific
objects, but their meaning is difficult to recover from scattered papers,
project pages, GitHub repositories, datasets, and leaderboards.

We propose **Benchmark Understanding**:

> Given a benchmark name, automatically generate an evidence-grounded analysis
> report explaining what the benchmark evaluates, why it was introduced, how it
> constructs tasks, how it scores model outputs, which models were evaluated,
> what conclusions the authors draw, and why models fail.

The system is useful only if the generated analysis is traceable to evidence and
can be compared with human-written research notes.

## Research Questions

RQ1. Can a system automatically recover the core research logic of a benchmark
paper, including motivation, evaluated capability, task design, scoring
protocol, model results, conclusions, and failure modes?

RQ2. Does evidence grounding improve the factual reliability and reviewability
of benchmark analysis reports compared with plain LLM summarization?

RQ3. Can a unified schema support cross-benchmark comparison across different
benchmark families, such as real-work deliverables, financial search, financial
analysis, spreadsheet workflows, and agent tool use?

RQ4. Does the system reduce the time required for researchers to understand and
compare new benchmarks?

## Contributions

1. **Task definition**: We define Benchmark Understanding as an evidence-grounded
   document understanding task specialized for model-evaluation benchmarks.

2. **Schema**: We formalize a Benchmark Paper Analysis Schema covering:
   core question, motivation, evaluated capabilities, benchmark design,
   rubric/gold/scoring, model results, main conclusions, failure modes,
   reliability notes, and evidence snippets.

3. **System**: We build a with-web pipeline that discovers sources, fetches
   webpages/PDFs/READMEs/leaderboards, extracts analysis fields, parses model
   result tables, reconciles conflicts, and renders Chinese reports with English
   evidence appendices.

4. **Dataset and evaluation**: We construct a small human-annotated gold-note
   set and compare system outputs against it using field coverage, factual
   accuracy, evidence support rate, table extraction quality, and human
   usefulness ratings.

## Task Definition

Input:

- A benchmark name, e.g. `GDPval`.
- Optionally, user-provided source URLs such as paper, GitHub, project page,
  dataset, or leaderboard links.

Output:

- A structured JSON report.
- A human-readable HTML report.
- Field-level evidence snippets with source URL and source type.
- Cross-benchmark comparison metadata.

The output must answer:

- Why was this benchmark proposed?
- What model or agent capabilities does it evaluate?
- How are tasks/data/worlds/cases constructed?
- How are gold answers, rubrics, and scoring protocols defined?
- Which models were evaluated, and what are the main results?
- What conclusions does the benchmark paper draw?
- What model failure modes are revealed?
- What remains uncertain, unsupported, or hard to reproduce?

## Planned Benchmark Set

Development set:

- GDPval
- FAB
- SpreadsheetBench v2

Test set:

- APEX
- FinSearchComp
- OneMillion-Bench
- IBFE
- One additional agent or evaluation benchmark selected after source discovery.

The development set is used to refine schema, prompts, and extraction rules. The
test set should not be used to tune prompts or heuristics except for bug fixes
that are benchmark-agnostic.

## Gold Notes

Gold notes are human-written benchmark analysis records. Each gold note follows
`data/gold_notes/template.json`.

Each field must include:

- A concise human-written answer.
- One or more evidence snippets.
- Source URL.
- Source type.
- Annotator confidence.
- Optional notes about ambiguity or missing information.

Gold notes are not intended to be exhaustive paper summaries. They are compact
research notes that support benchmark selection, paper reading, and model
capability diagnosis.

## Evaluation

### Automatic / Semi-Automatic Metrics

Field Coverage:

- Percentage of required schema fields filled with non-empty, relevant content.

Evidence Support Rate:

- Percentage of claims with at least one evidence snippet that directly supports
  the claim.

Table Extraction Quality:

- Precision/recall or exact-match style checks for model, metric, score,
  source URL, and result status.

Conflict Tracking:

- Number of fields where multiple sources disagree and whether the system marks
  the conflict.

### Human Evaluation

Annotators compare system reports with gold notes and rate:

- Factual correctness.
- Usefulness for group-meeting presentation.
- Usefulness for choosing a benchmark.
- Clarity of failure-mode analysis.
- Evidence reviewability.

Recommended 1-5 scale:

- 1: unusable or misleading.
- 2: partially useful but requires major correction.
- 3: useful draft with noticeable gaps.
- 4: mostly reliable, minor edits needed.
- 5: presentation-ready.

### Efficiency Study

Measure:

- Time to produce a human note from scratch.
- Time to revise a system-generated report into an acceptable note.

The claim should be conservative: the system assists benchmark understanding; it
does not replace expert reading.

## Baselines and Ablations

Baseline 1: Vanilla LLM Summary

- Provide the benchmark paper text or source bundle to an LLM and ask for a
  summary without schema constraints or evidence grounding.

Baseline 2: Paper-Only Extraction

- Use only the paper/PDF source, without project pages, GitHub, datasets, or
  leaderboards.

Baseline 3: No Evidence Grounding

- Generate structured fields without requiring source snippets.

Ablation 1: No Web

- Use only existing seed/catalog information.

Ablation 2: No Table Parser

- Exclude PDF/HTML table extraction and compare model-result quality.

Full System:

- With-web source discovery, PDF/HTML/README fetching, paper analysis,
  leaderboard/table parser, reconciliation, LLM analysis, and evidence-grounded
  report rendering.

## Paper Outline

1. Introduction
2. Benchmark Understanding Task
3. Benchmark Analysis Schema
4. Evidence-Grounded Pipeline
5. Gold-Note Dataset
6. Experiments
7. Case Studies
8. Limitations
9. Conclusion

## Figures and Tables Needed

- Figure 1: Pipeline overview from benchmark name to evidence-grounded report.
- Figure 2: Benchmark analysis report UI screenshot.
- Table 1: Schema fields and definitions.
- Table 2: Benchmark set and source types.
- Table 3: Main evaluation results.
- Table 4: Ablation results.
- Case study box: GDPval or FinSearchComp.

## Risks

- Source discovery may retrieve wrong pages for ambiguous names.
- PDF table extraction can be noisy.
- Some leaderboards are dynamic, blocked, or unavailable.
- LLM-generated analysis may overstate unsupported claims.
- Human gold notes are small and may reflect annotator bias.
- Some benchmark papers may not describe failure modes explicitly.
- Public reproducibility is limited when full benchmark data is private.

## Phase 0 Exit Criteria

- `docs/PAPER_PLAN.md` exists and defines the ARR direction.
- `docs/ANNOTATION_GUIDELINE.md` exists and defines gold-note rules.
- `data/gold_notes/template.json` exists and can be copied for new benchmarks.
- `data/gold_notes/gdpval.gold.json` exists as the first draft gold note.
- `docs/PROJECT_PROGRESS.md` records Phase 0.

