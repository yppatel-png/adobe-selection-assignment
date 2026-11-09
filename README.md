# Adobe Inter-IIT Tech Meet 15.0: Selection Assignment
## Personalized Learning Buddy — Data Pipeline & Knowledge Base
**Target Institute & Programme**: Indian Institute of Technology Bhilai — B.Tech in Computer Science and Engineering (2023 Batch Onwards)  
**Deliverable**: Reproducible 1-Command Data Pipeline, Verified Datasets, Chunked Knowledge Base, & Ideas Note  
**Author / Team**: Yug Patel (yppatel@iitbhilai.ac.in) — IIT Bhilai  

---

## 🌟 Executive Overview & Prototype Link

This repository represents the **strict, self-contained, 1-command data pipeline** requested in the **Adobe 1-Day Selection Assignment Brief**. It strictly complies with all selection constraints:
- ✅ **Zero LLM API calls**: Fully deterministic, reproducible data normalization and extraction.
- ✅ **Zero external vector database requirements**: Standalone, lightweight semantic chunking.
- ✅ **Acyclic DAG validation**: Depth-First Search cycle detection on the prerequisite graph.
- ✅ **Full Provenance & Auditability**: Every course, rule, and resource traces back to official URLs or `data/raw/courses of study.pdf`.

> ### 🚀 Live Full-Stack Working Prototype
> To see this exact data pipeline powering a live, production-grade application with **Bayesian Knowledge Tracing (BKT)**, **Constraint Optimization Multi-Semester Curriculum Scheduling**, **Adaptive Learning Engine**, and an interactive **Personalized Buddy UI**, visit our prototype repository:  
> **👉 Prototype Repository**: [https://github.com/yppatel-png/MP5](https://github.com/yppatel-png/MP5)

---

## ⚡ Quick Start: 1-Command Pipeline

Run the end-to-end 5-stage pipeline with a single command (requires only standard Python 3.9+; zero external pip dependencies needed):

```bash
python run_pipeline.py
```
*Or using GNU Make:*
```bash
make pipeline
```

### What Happens in the Pipeline:
1. **Stage 1 (Ingest)**: Verifies raw source files in `data/raw/` (`courses of study.pdf`, 2.29 MB, SHA-256 verified).
2. **Stage 2 (Parse & Normalise)**: Normalizes 37 courses, 18 prerequisite edges, 24 skills, 10 academic rules, 34 open resources, 112 modules, and 240 formative practice exercises.
3. **Stage 3 (Validate)**: Executes strict consistency checks:
   - Cycle detection on prerequisite graph via DFS.
   - Numeric credit validation.
   - Foreign key integrity check across course codes.
   - Source provenance checks (`source_url`, `retrieved_on`, `source_type`).
4. **Stage 4 (Chunk)**: Generates 193 retrieval-ready vector chunks (average length 150-300 words).
5. **Stage 5 (Export)**: Emits clean JSON and CSV datasets to `data/processed/` and `chunks.jsonl`.

---

## 📂 Repository File Structure

```text
├── data/
│   ├── raw/                           # Raw, untouched source documents
│   │   ├── courses of study.pdf       # Official IIT Bhilai B.Tech CSE curriculum (2.29 MB)
│   │   ├── official_academic_programs_2026-11-06.txt
│   │   ├── official_institute_homepage_2026-11-06.txt
│   │   └── SOURCES.md                 # Provenance audit trail
│   └── processed/                     # Clean, normalized, validated exports
│       ├── courses.json / .csv        # 37 unique CSE, Institute Core & Open Electives
│       ├── prerequisites.json / .csv  # 18 hard prerequisite DAG edges
│       ├── skills.json / .csv         # 24 canonical computer science competencies
│       ├── course_skills.json / .csv  # 41 granular course-to-skill links
│       ├── academic_rules.json / .csv # 10 quotable academic policies
│       ├── resources.json / .csv      # 34 verified open educational resources
│       ├── modules.json / .csv        # 112 granular syllabus modules
│       ├── exercises.json / .csv      # 240 formative practice exercises
│       ├── chunks.jsonl               # 193 retrieval chunks
│       └── DATA_QUALITY.md            # Data quality metrics and validation report
├── run_pipeline.py                    # Deterministic 1-command data pipeline
├── Makefile                           # 'make pipeline' automated build target
├── chunks.jsonl                       # Root-level vector chunks export
├── SOURCES.md                         # Authoritative source index & data ethics
├── DATA_QUALITY.md                    # Comprehensive data quality report
├── IDEAS.md                           # 1-page Ideas Note (3 answerable / 3 unanswerable)
├── requirements.txt                   # Minimal dependencies (optional)
└── README.md                          # This documentation file
```

---

## 📊 Summary of Knowledge Base Metrics

| Entity | Count | Key Attributes | Validation Status |
| :--- | :---: | :--- | :--- |
| **Courses** | **37** | Code, Title, Credits, L-T-P-C, Department, Mode, Syllabus | ✅ 100% Validated (Numeric credits, unique codes) |
| **Prerequisites** | **18** | Edge, Requires, Hard/Soft Type | ✅ Verified Acyclic DAG (Zero cycles detected) |
| **Skills** | **24** | Skill ID, Canonical Name, Category, Level | ✅ 100% Mapped to Courses and Exercises |
| **Course-Skill Links**| **41** | Course Code, Skill ID, Coverage Weight | ✅ 100% Foreign Key Coherence |
| **Academic Rules** | **10** | Rule ID, Clause, Category, Parameters | ✅ Cites official pages or explicit "assumption" |
| **Open Resources** | **34** | Title, Provider, URL, License, Duration, Format | ✅ Open Licenses (MIT, CC-BY, Apache, Free Access) |
| **Course Modules** | **112**| Module ID, Course Code, Topic, Description | ✅ Fine-grained syllabus topic segmentation |
| **Formative Exercises**| **240**| Exercise ID, Question, Options, Answer, Skill | ✅ Plausible distractors, Bloom taxonomy tagged |
| **Vector Chunks** | **193**| Chunk ID, Entity Type, Content, Provenance | ✅ 128 KB total JSONL ready for embedding |

---

## 🛡️ Data Quality & Academic Honesty Invariants

1. **Zero Hallucination Guarantee**: Every course code matches official IIT Bhilai records. When elective options from other departments were added, they were verified directly against the Courses of Study PDF.
2. **Provenance Tiering**:
   - `verified`: Directly stated in the official PDF with exact page number.
   - `assumption`: Working hypotheses (e.g. credit overload rules) explicitly tagged as "pending ordinance" so no false institutional authority is asserted.
3. **Preserved Source Discrepancies**: The discrepancy between `SL302` and `CSL302` in the raw PDF is preserved with documented data notes rather than being silently erased.

---

## 💡 Selection Deliverables Reference

- **Part D (Data Quality Report)**: See [DATA_QUALITY.md](DATA_QUALITY.md) for completeness analysis, schema dictionaries, and edge-case resolutions.
- **Part E (Ideas Note)**: See [IDEAS.md](IDEAS.md) for the 1-page report detailing:
  - 3 student questions the buddy answers with mathematical precision.
  - 3 student questions that cannot be answered with current data (and the defensive guardrails triggered).
  - The dual-layer architecture combining deterministic graph algorithms with conversational summarization.
