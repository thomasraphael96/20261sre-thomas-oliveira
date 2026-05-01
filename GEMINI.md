# GEMINI.md - Project Context & Instructions

This document provides foundational context and mandates for working on the **Olist ETL** project. Adhere to these guidelines to ensure consistency and alignment with project goals.

## 1. Project Overview
The **Olist ETL** project is a data engineering initiative to process approximately 100,000 marketplace orders from the [Olist Dataset](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce). The goal is to build a reliable, idempotent, and observable pipeline that loads data into an analytical database for daily dashboarding.

### Main Technologies (Inferred/Planned)
- **Language:** Python
- **Libraries:** SQLAlchemy (for database abstraction), Pandas/Native Python (for transformation).
- **Databases:** SQLite (local/dev), PostgreSQL (target analytical DB).
- **Source Format:** CSV files.

## 2. Core Engineering Mandates
These principles are non-negotiable and must guide all implementation decisions:

1.  **Idempotency (RNF01):** Every execution must be safe to rerun. State is managed via `UPSERT` operations or equivalent mechanisms to prevent data duplication.
2.  **Observability (RNF04, RNF05):** No "silent failures." Every stage (Extract, Transform, Load) must log progress, row counts, and health status.
3.  **Resilience (RNF03):** The pipeline must handle partial failures (e.g., a few corrupt rows) without crashing the entire process.
4.  **Integrity (RNF02):** Zero data loss for valid records.

## 3. Directory Structure
- `specs/`: Contains detailed problem definitions, data mappings, and technical requirements.
    - `00_problem.md`: Context, stakeholders, and failure modes.
    - `01_data_mapping.md`: Detailed CSV schema and consolidation plan.
    - `02_requirements.md`: Functional and non-functional requirements (SWEBOK aligned).
- `README.md`: Basic project entry point.

## 4. Building and Running
*Note: The project is currently in the specification phase. Implementation is pending.*

- **TODO:** Define environment setup (e.g., `venv`, `pip install -r requirements.txt`).
- **TODO:** Define execution command (e.g., `python main.py`).
- **TODO:** Define test command (e.g., `pytest`).

## 5. Development Conventions
- **Modular Design:** Follow the ETL pattern—keep Extraction, Transformation, and Loading logic in separate, testable modules.
- **Error Handling:** Implement dead-letter queues or error-logging tables for records that fail validation.
- **Type Safety:** Use Python type hints to ensure clarity in data flow.
- **Documentation:** Keep the `specs/` directory updated as architectural decisions are finalized.
