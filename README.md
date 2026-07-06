# Alpha Tracker

Alpha Tracker is a scientific monitoring platform for detecting newly published complete amino-acid sequences that do not yet have public evidence of a corresponding structural model or structural publication in the sources queried by the system.

Important scientific wording:

> Alpha Tracker must never claim that nobody has run AlphaFold or any other structure predictor.
>
> The correct conclusion is: "No public evidence of a structural model or corresponding structural publication was found in the queried sources."

The system is designed to run continuously, deduplicate work, cache external evidence, rank candidate proteins, and generate alerts for promising structural biology opportunities.

## Capabilities

- Continuous monitoring of PubMed, PubMed Central OA, bioRxiv, medRxiv, arXiv, NCBI Protein, GenBank, RefSeq, UniProt, AlphaFold DB, PDB, and repository sources.
- Protein metadata extraction: name, organism, FASTA, length, DOI, authors, date, journal, source URL.
- Completeness checks for amino-acid sequences.
- Evidence checks across AlphaFold DB, PDB/RCSB, UniProt, NCBI, literature, Zenodo, Figshare, Dryad, OSF, and GitHub.
- Sequence analysis: molecular weight, amino-acid composition, low complexity, coarse disorder, signal peptide heuristic, transmembrane heuristic, coiled-coil heuristic, domain placeholders and InterPro-ready interfaces.
- Explainable scoring for novelty, recency, sequence confidence, structural evidence gap, disease relevance, human priority, biomedical importance, and pharmacological potential.
- Alerts with the exact evidence statement and queried-source audit trail.
- REST API and dashboard with ranking, filters, timeline, statistics, and export links.
- PostgreSQL storage, Redis/Celery workers, retries, logs, cache TTLs, Docker Compose, and tests.

## Architecture

```text
sources -> crawlers -> parsers -> sequence extraction -> completeness
        -> evidence checks -> sequence analysis -> scoring -> alerts
        -> PostgreSQL -> FastAPI -> React dashboard
```

Major backend modules:

- `alphatracker.crawlers`: source-specific monitoring clients.
- `alphatracker.parsers`: publication and sequence parsing.
- `alphatracker.pipeline`: orchestration, evidence checking, scoring, alerts.
- `alphatracker.analysis`: amino-acid feature extraction.
- `alphatracker.db`: SQLAlchemy models, sessions, persistence helpers.
- `alphatracker.api`: FastAPI application.
- `alphatracker.workers`: Celery app and scheduled jobs.

## Quick Start

Copy environment defaults:

```bash
cp .env.example .env
```

Start the stack:

```bash
docker compose up --build
```

Services:

- API: http://localhost:8000
- API docs: http://localhost:8000/docs
- Dashboard: http://localhost:5173
- PostgreSQL: localhost:5432
- Redis: localhost:6379

Run a single monitoring pass:

```bash
docker compose exec api alpha-tracker run-once
```

Run tests:

```bash
docker compose exec api pytest
```

## Configuration

Configuration is loaded from environment variables and `config/sources.yaml`.

Key settings:

- `ALPHATRACKER_DATABASE_URL`
- `ALPHATRACKER_REDIS_URL`
- `ALPHATRACKER_POLL_INTERVAL_MINUTES`
- `ALPHATRACKER_LOOKBACK_DAYS`
- `ALPHATRACKER_ALERT_SCORE_THRESHOLD`
- `ALPHATRACKER_NCBI_EMAIL`
- `ALPHATRACKER_NCBI_API_KEY`
- `ALPHATRACKER_GITHUB_TOKEN`

NCBI recommends identifying automated clients. Set `ALPHATRACKER_NCBI_EMAIL` before running production crawls.

## Score

The score is normalized to 0-100:

```text
score =
  20 * novelty +
  15 * recency +
  15 * sequence_confidence +
  15 * structural_gap +
  10 * human_priority +
  10 * disease_relevance +
   8 * biomedical_importance +
   5 * pharmacological_potential +
   2 * evidence_quality
```

Each component is stored in the database and returned by the API so alerts remain auditable.

## Evidence Statement

Every alert uses this conservative statement:

```text
No public evidence of a structural model or corresponding structural publication was found in the queried sources.
```

If any AlphaFold DB record, PDB structure, experimental structure, repository model, or structural-literature hit is found, the alert explains that evidence instead of claiming a gap.

## Development Notes

This repository intentionally separates "connectors" from "decisions." A failed external API call does not mean no evidence exists. It is recorded as an inconclusive query and lowers evidence quality.

The first implementation includes production-shaped interfaces and several live HTTP connectors. Some publisher-specific full-text extraction depends on legal/open access availability and is handled through public metadata, OA XML, or repository links when available.

