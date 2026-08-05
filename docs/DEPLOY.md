# Bench Analysis Deployment

This document describes how to run the Bench Analysis Workbench as a standalone
Python web service.

## Start locally

```bash
python3 bench_server.py
```

Open:

```text
http://127.0.0.1:8765/
```

## Environment variables

- `HOST`: bind host. Default: `0.0.0.0`
- `PORT` or `BENCH_PORT`: service port. Default: `8765`
- `BENCH_OUTPUT_DIR`: job output directory. Default: `bench_analysis_outputs`
- `BENCH_PUBLIC_BASE_URL`: public route used to render share links, for example
  `https://medai.sugarclaw.top/bench`

## Render / Railway style deployment

Use this start command:

```bash
python bench_server.py
```

If the platform uses a Procfile, use:

```text
Procfile
```

or copy its command into the platform start-command field.

## VPS Deployment

The deployment script expects a server checkout at:

```text
/opt/bench-analysis-workbench
```

It pulls `origin/main` and restarts `bench_server.py`.

Example:

```bash
BENCH_PORT=8765 ./deploy.sh
```

If this service is placed behind Nginx, proxy the public route to:

```nginx
proxy_pass http://127.0.0.1:8765;
```

## Legacy Mounted Deployment

The original split point also mounted this workbench under the existing
`content-pipeline` public server at:

```text
https://medai.sugarclaw.top/bench
```

That route is useful for demos, but this standalone repository should be treated
as the source of truth for the Bench Analysis Workbench going forward.

## Notes

- Runtime outputs are intentionally not committed to Git:
  `bench_analysis_outputs/` contains SQLite state, raw PDFs, generated reports,
  and cached source files.
- Public users should use the hosted URL. Developers can inspect generated
  reports through the workbench UI.
