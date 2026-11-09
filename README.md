# Adobe Inter-IIT Tech Meet 15.0: Selection Assignment

### Personalised Learning Buddy — Data Pipeline & Knowledge Base
- **Target Programme**: B.Tech in Computer Science and Engineering (2023 Batch Onwards), Indian Institute of Technology Bhilai
- **Live Prototype**: [https://adobe-buddy.onrender.com/](https://adobe-buddy.onrender.com/)
- **Prototype Repository**: [https://github.com/yppatel-png/MP5](https://github.com/yppatel-png/MP5)
- **Author**: Yug Patel (`yppatel@iitbhilai.ac.in`), IIT Bhilai

---

## Overview

This repository contains the data engineering pipeline and validated knowledge base developed for the Adobe Inter-IIT Tech Meet 15.0 selection assignment.

The assignment requires constructing a clean, auditable, and structured curriculum dataset for a single academic programme (IIT Bhilai B.Tech CSE), standardizing all entities into a strict relational schema, verifying the prerequisite graph for circular dependencies, generating retrieval-sized text chunks, and providing critical evaluation notes on system capabilities.

### Strict Constraint Compliance
- **Zero LLM API Calls**: The data pipeline runs 100% deterministically using only the Python standard library. No model API keys, runtime network calls, or external vector stores are used during pipeline execution.
- **Reproducible One-Command Pipeline**: Running `python run_pipeline.py` or `make pipeline` ingests raw files, normalizes schemas, verifies the prerequisite graph as an acyclic directed graph (DAG), generates vector chunks, and exports final CSV/JSON artifacts.
- **Explicit Provenance**: Every entity traces directly to an authoritative source URL, retrieval date, and source tier (`verified` from official curriculum PDF vs. `assumption` pending ordinance).

---

## Quick Start

### Running the Data Pipeline

The pipeline requires Python 3.9+ and relies exclusively on standard library modules (`json`, `csv`, `pathlib`, `hashlib`, `re`):

```bash
python run_pipeline.py
```

Or using GNU Make:
```bash
make pipeline
```

### Pipeline Execution Stages

1. **Ingest**: Verifies source documents in `data/raw/` (`courses of study.pdf`, 2.29 MB, SHA-256 hash verified).
2. **Parse & Normalise**: Standardizes course codes, credits, and semester offerings, ensuring mandatory schema fields (`source_url`, `retrieved_on`, `source_type`) are present across all entities.
3. **Validate**: Enforces integrity constraints:
   - Cycle detection on prerequisite graph via Depth-First Search (DFS) topological ordering.
   - Numeric credit verification (strictly numeric, rejecting ranges or empty strings).
   - Foreign key integrity check across course codes, skills, and prerequisite links.
4. **Chunk**: Splits syllabi, academic rules, course modules, and learning resources into 193 retrieval chunks with entity metadata and source page citations.
5. **Export**: Writes normalized datasets to `data/processed/` (both JSON and CSV formats) and emits `chunks.jsonl`.

---

## Repository Structure

```text
├── data/
│   ├── raw/                           # Raw source documents
│   │   ├── courses of study.pdf       # Official IIT Bhilai curriculum document (2.29 MB)
│   │   ├── official_academic_programs_2026-11-06.txt
│   │   ├── official_institute_homepage_2026-11-06.txt
│   │   └── SOURCES.md                 # Primary source index
│   └── processed/                     # Validated exports (JSON & CSV)
│       ├── courses.json / .csv        # 37 curriculum courses
│       ├── prerequisites.json / .csv  # 18 prerequisite DAG edges
│       ├── skills.json / .csv         # 24 core competency definitions
│       ├── course_skills.json / .csv  # 41 course-skill link mappings
│       ├── academic_rules.json / .csv # 10 quotable academic policies
│       ├── resources.json / .csv      # 34 open educational resources
│       ├── modules.json / .csv        # 112 granular syllabus modules
│       ├── exercises.json / .csv      # 240 formative practice exercises
│       ├── chunks.jsonl               # 193 retrieval chunks
│       └── DATA_QUALITY.md            # Data quality report
├── run_pipeline.py                    # Standalone 5-stage data pipeline
├── Makefile                           # 'make pipeline' target
├── chunks.jsonl                       # Root-level chunks export (128 KB)
├── SOURCES.md                         # Authoritative source audit & open licenses
├── DATA_QUALITY.md                    # Data quality metrics & dictionary
├── IDEAS.md                           # 1-page Ideas Note (Part E)
├── requirements.txt                   # Minimal dependencies (pydantic)
└── README.md                          # Documentation
```

---

## Knowledge Base Metrics

| Entity | Count | Key Schema Fields | Validation Status |
| :--- | :---: | :--- | :--- |
| **Courses** | 37 | `code`, `title`, `credits`, `ltpc`, `department`, `mode`, `syllabus` | Validated (Numeric credits, unique codes) |
| **Prerequisites** | 18 | `course_code`, `requires_code`, `type` | Verified Acyclic DAG (0 cycles detected) |
| **Skills** | 24 | `skill_id`, `name`, `category`, `level` | Mapped to course topics and assessments |
| **Course-Skill Links** | 41 | `course_code`, `skill_id`, `weight` | Foreign key references verified |
| **Academic Rules** | 10 | `rule_id`, `name`, `type`, `parameters`, `clause_id` | Provenanced (Verified page or explicit assumption) |
| **Open Resources** | 34 | `title`, `provider`, `url`, `license`, `format`, `duration` | Verified open licenses (MIT, CC-BY, Apache, Free Access) |
| **Modules** | 112 | `module_id`, `course_code`, `topic`, `description` | Granular course syllabus topics |
| **Exercises** | 240 | `exercise_id`, `question`, `options`, `answer`, `skill_id` | Plausible distractors, Bloom-taxonomy tagged |
| **Vector Chunks** | 193 | `chunk_id`, `entity_type`, `text`, `provenance` | Ready for retrieval embeddings (128 KB) |

---

## Academic Honesty & Provenance Policy

1. **Zero Hallucination Guarantee**: All 37 courses are derived directly from the official IIT Bhilai Courses of Study PDF.
2. **Explicit Provenance Tiering**:
   - `verified`: Formally cited from the Courses of Study PDF with exact page numbers.
   - `assumption`: Operational rules (such as semester credit maximums and overload thresholds) not stated in the syllabus are explicitly marked as "pending ordinance" in the schema and user interface.
3. **Preserved Discrepancies**: Source anomalies (such as the `SL302`/`CSL302` code mismatch on page 22 of the curriculum document) are preserved in data notes rather than artificially smoothed over.

---

## Assignment Deliverables Reference

- **Part A (Data Collection)**: Located in `data/raw/` and `data/processed/`.
- **Part B (Data Schema)**: Documented with schema definitions in [DATA_QUALITY.md](DATA_QUALITY.md).
- **Part C (Pipeline Execution)**: Executed via [run_pipeline.py](run_pipeline.py) or `make pipeline`.
- **Part D (Data Quality Report)**: Detailed report in [DATA_QUALITY.md](DATA_QUALITY.md).
- **Part E (Ideas Note)**: 1-page analysis in [IDEAS.md](IDEAS.md) answering the 3 student questions the system answers with mathematical precision, 3 questions outside dataset coverage (with defensive guardrails), and the dual-layer system design.

---

## Working Prototype

To demonstrate that this schema functions effectively in an interactive product, a full-stack prototype was developed and deployed:

- **Live Web Application**: [https://adobe-buddy.onrender.com/](https://adobe-buddy.onrender.com/)
- **API Documentation**: [https://adobe-buddy.onrender.com/docs](https://adobe-buddy.onrender.com/docs)
- **Source Code**: [https://github.com/yppatel-png/MP5](https://github.com/yppatel-png/MP5)

The prototype integrates:
- Deterministic topological constraint scheduler for multi-semester degree planning.
- Bayesian Knowledge Tracing (BKT) engine for dynamic skill mastery tracking.
- Adaptation engine triggering targeted remediation upon diagnostic struggle.
- Verification interceptor guaranteeing zero invented course codes in chat responses.
