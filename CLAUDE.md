# seeing-or-thinking — agent instructions

## What this is
Model-agnostic harness to run, evaluate, fine-tune and serve video VLMs
(Qwen3-VL first, then Cosmos Reason2). Results back a research paper
("Seeing or Thinking?"), so reproducibility beats speed.

## Layout
- src/vlmlab/            library code
- src/vlmlab/backends/   one file per model family (the only place model code lives)
- tests/                 pytest; GPU tests marked @pytest.mark.gpu
- docs/specs/            numbered specs; every change starts from one

## Architecture rules (never break)
1. Only src/vlmlab/backends/ may import transformers, vllm or model code.
2. Backends turn a Sample into a Result. They never write files or compute scores.
3. Adding a model = one new backend file + one registry entry. No core edits.

## Workflow
- Work from exactly one spec in docs/specs/. If the spec is unclear, stop and ask.
- Never edit a spec without asking.
- One branch per spec; commit messages start with the spec id, e.g. `spec-0001: ...`.
- Plan first, list the files you will touch, then implement in a small diff.
- Done means: `uv run ruff check . && uv run ruff format --check . && uv run pytest` passes.
- Never download models or use a GPU in non-gpu tests.
- Never commit data, runs, checkpoints or weights.

## Commands
- Install:  uv sync --extra hf
- Test:     uv run pytest            (GPU tests: uv run pytest -m gpu)
- Lint/fix: uv run ruff check . --fix && uv run ruff format .

## Conventions
Python 3.11, dataclasses for data, type hints everywhere, no print in library
code (use logging), every result row carries run_info.
