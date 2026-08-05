# Gold Note Annotation Guideline

## Purpose

Gold notes are human-written benchmark analysis records used to evaluate the
Benchmark Understanding system. They are not general paper summaries. They are
compact, evidence-grounded notes that explain how a benchmark evaluates models
or agents.

The annotator should write as if preparing a group-meeting research brief:

- concise enough to compare across benchmarks;
- specific enough to avoid generic LLM-summary language;
- always traceable to source evidence.

## Unit of Annotation

One gold note corresponds to one benchmark or one clearly versioned benchmark,
for example:

- `GDPval`
- `FAB`
- `SpreadsheetBench v2`

If a name is ambiguous, annotate the resolved benchmark and record aliases or
ambiguity in `reliability_notes`.

## Required Sources

Each gold note should use at least one primary source whenever available:

- paper PDF or abstract page;
- official project page;
- official GitHub repository;
- official dataset page;
- official leaderboard.

Third-party pages can be included, but they should not override primary sources
unless the primary source is unavailable.

## Evidence Requirements

Every major claim must have evidence.

Good evidence:

- directly supports the claim;
- comes from a stable source URL;
- is short enough to inspect quickly;
- includes source type, e.g. `paper`, `official`, `github`, `dataset`,
  `leaderboard`;
- avoids unnecessary long quotation.

Weak evidence:

- only indirectly related;
- comes from a third-party summary;
- lacks a stable URL;
- supports only part of the claim.

Unsupported claim:

- cannot be traced to any source;
- is an annotator inference without being marked as inference;
- generalizes beyond the paper.

When inference is necessary, put it in `annotator_notes` or mark the evidence
field with `"support": "inferred"`.

## Field Definitions

### core_question

The main question the benchmark tries to answer.

Good:

> Can frontier models produce real-world professional deliverables that experts
> judge competitive with human work?

Bad:

> This benchmark evaluates AI.

### motivation

Why the benchmark was proposed and what existing evaluations miss.

This should mention the gap in previous benchmarks, not just restate task type.

### evaluated_capabilities

The model/agent abilities being tested.

Use capability tags plus short explanations. Examples:

- real-world professional deliverable generation;
- financial search and retrieval;
- multi-source evidence synthesis;
- spreadsheet workflow execution;
- tool use;
- long-horizon planning;
- expert rubric following.

### benchmark_design

How tasks are built.

Include:

- data source;
- task construction process;
- task types;
- task environment/tools;
- whether experts are involved;
- whether there are files, tables, PDFs, webpages, or interactive workflows.

### rubric_gold_scoring

How outputs are judged.

Include:

- gold answer or reference artifact definition;
- rubric dimensions;
- scoring protocol;
- metrics;
- judge type, e.g. automatic exact match, expert human judge, LLM judge,
  hybrid evaluator;
- whether human review is used.

### model_results

A structured list of reported model results.

Each row should include:

- model;
- metric;
- score;
- source URL;
- result status: `verified`, `candidate`, `reported_summary`, or `missing`;
- evidence.

Use `reported_summary` when the source states relative findings but not a clean
table. Use `missing` when no reliable model result is available.

### main_conclusions

What the authors want readers to conclude from the benchmark.

Do not mix this with the annotator's opinion. Put annotator judgments in
`reliability_notes` or `annotator_notes`.

### failure_modes

Why models fail according to the paper or evidence.

Examples:

- retrieval failure;
- stale or inconsistent sources;
- unit/currency mismatch;
- failure to use tools;
- long-artifact inconsistency;
- formatting constraint violations;
- professional detail omissions;
- domain-specific rubric misunderstanding.

If the paper does not explicitly analyze failures, write `"not_reported"` and
explain the gap in `reliability_notes`.

### reliability_notes

Notes about reproducibility, source uncertainty, leaderboard freshness, private
data, judging bias, or extraction ambiguity.

This field is especially important for benchmarks with private full sets,
blocked official pages, dynamic leaderboards, or LLM judges.

## Quality Checks

Before marking a gold note complete, check:

- All required fields are non-empty unless explicitly marked `not_reported`.
- Each major field has at least one evidence item.
- Evidence snippets are not overlong.
- The note distinguishes source claims from annotator inferences.
- Model result rows contain source URLs.
- Any uncertain or conflicting information is recorded.
- The note can be understood without opening the full paper.

## Annotation Status

Use one of:

- `draft`: first-pass annotation; needs review.
- `reviewed`: checked by a second annotator.
- `adjudicated`: disagreements resolved.

Phase 0 gold notes may remain `draft`. ARR experiments should use reviewed or
adjudicated notes whenever possible.

