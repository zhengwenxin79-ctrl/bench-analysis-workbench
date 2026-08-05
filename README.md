# Bench Analysis Workbench

Bench Analysis Workbench is an internal research demo for turning benchmark
names into structured benchmark paper analysis reports.

The current system focuses on:

- discovering official pages, papers, GitHub repositories, datasets, and leaderboards;
- fetching webpages, arXiv PDFs, GitHub READMEs, and cached fallback pages;
- extracting paper-analysis fields such as motivation, benchmark design, rubric,
  scoring, model results, conclusions, and failure modes;
- saving discovered and user-provided sources into a reusable seed library;
- running evidence-grounded LLM analysis when an API key is available;
- rendering Chinese web UI pages, batch comparison reports, single-benchmark
  reports, evidence review pages, and exportable HTML artifacts.

## Quick Start

Run the web workbench:

```bash
python3 bench_server.py
```

Open:

```text
http://127.0.0.1:8765/
```

In the web UI:

- use `新建批量分析` for normal batch jobs;
- use `手动补充来源` when search cannot find a benchmark;
- open `种子库` to review saved benchmark sources and rerun from cached/manual sources.

Run a CLI batch:

```bash
python3 -m bench_analysis batch GDPval "SpreadsheetBench v2" FinSearchComp --with-web
```

Generate the Chinese research brief prototype:

```bash
python3 -m bench_analysis brief-prototype --lang zh-CN
```

## Output

Runtime outputs are written to:

```text
bench_analysis_outputs/
```

This directory is ignored by Git because it contains SQLite job state, cached raw
documents, PDFs, generated HTML reports, and export zip files.

## Project Structure

```text
bench_analysis/
  source_discovery.py       # discover official pages, papers, GitHub, datasets, leaderboard
  fetch.py                  # fetch webpages, arXiv PDFs, README files, fallback mirrors
  extract.py                # extract benchmark metadata
  paper_analysis_extract.py # extract paper-note-style analysis fields
  evidence_pack.py          # compress fetched sources into an LLM-ready evidence pack
  llm_client.py             # DeepSeek/OpenAI-compatible JSON completion client
  llm_analysis.py           # evidence-grounded LLM analysis schema and prompt
  results.py                # extract model result candidates
  reconcile.py              # merge sources, conflicts, confidence, localized brief
  render.py                 # batch and single-benchmark HTML reports
  brief_render.py           # Chinese research brief pages
  web_app.py                # internal web UI
  job_store.py              # jobs plus reusable bench seed library

docs/
  FRAMEWORK.md
  PROJECT_PROGRESS.md
  DEPLOY.md
```

## Deployment

Standalone service:

```bash
HOST=0.0.0.0 BENCH_PORT=8765 python3 bench_server.py
```

Platform Procfile:

```text
web: python bench_server.py
```

See `docs/DEPLOY.md` for server notes.

## Status

This repository is the standalone version split from `content-pipeline`. The MVP
is usable for internal demos, but evidence extraction, PDF table parsing,
leaderboard parsing, and result verification still need continued hardening.
