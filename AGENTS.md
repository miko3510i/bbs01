# Repository Guidelines

## Project Structure & Module Organization
This repository is greenfield; start by creating `src/bbs01/` for application modules and `tests/` for automated coverage. Within `src/bbs01/`, separate HTTP interface handlers under `api/`, domain services under `core/`, and persistence adapters under `infra/`. Reusable helpers belong in `shared/`. Configuration files such as `pyproject.toml`, `.env.example`, and `Makefile` stay at the root. Store architectural notes in `docs/` and seed JSON/YAML fixtures in `assets/`.

## Build, Test, and Development Commands
We target Python 3.12. Create a virtual environment with `python -m venv .venv && source .venv/bin/activate`. Install dependencies declared in `pyproject.toml` using `pip install -e .[dev]`. Keep thin Make targets: `make dev` should launch `uvicorn src.bbs01.api.app:app --reload`, `make lint` should wrap `ruff format && ruff check`, and `make test` should run `pytest`. Document any additional commands in `Makefile` comments.

## Coding Style & Naming Conventions
Adopt Ruff defaults; format code with `ruff format` (PEP 8, 4-space indentation). Name modules with snake_case, classes with PascalCase, and async coroutine functions with snake_case verbs such as `post_message`. Type hints are required for public functions. Configuration constants live in `settings.py` and use UPPER_SNAKE_CASE. Namespace environment variables with the `BBS01_` prefix.

## Testing Guidelines
Use Pytest with `tests/` mirroring the `src/` tree. Name test files `test_<unit>.py`; place reusable fixtures under `tests/fixtures/`. Target ≥90% branch coverage, measured with `pytest --cov=src/bbs01 --cov-report=term-missing`. Add regression tests before fixing bugs and include integration tests hitting HTTP endpoints by exercising `httpx.AsyncClient`.

## Commit & Pull Request Guidelines
Write commit subjects in the imperative present tense (e.g., `Add message pagination`) and reference linked issue IDs in the footer (`Refs #12`). Keep commits focused; rebase onto `main` before opening a PR. Pull requests must provide a concise summary, testing checklist, and screenshots or curl snippets for API-facing changes. Call out database migrations or destructive ops explicitly. Request at least one reviewer and wait for CI to pass before merging.

## Security & Configuration
Never commit `.env` or other secrets; provide sanitized examples via `.env.example`. Validate user input at the API layer and sanitize markdown output before rendering. Rotate API keys quarterly and document secret storage expectations in `docs/security.md`.
