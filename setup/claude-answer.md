# What `make test` does

`make test` runs `pytest -q` from the repo root (`Makefile:10-11`). The `-q` flag just makes the output shorter.

There's no `pytest.ini`, `pyproject.toml` or `conftest.py`, so pytest uses its defaults and collects every `test_*.py` file in the repo. Right now the only one is `tests/test_smoke.py`, which has two tests:

1. **`test_openapi_document_can_be_loaded`**: reads `docs/openapi.yaml` and checks that the YAML is valid, that the `openapi` field starts with `3.`, and that `paths` isn't empty. It doesn't check whether the contract makes sense; that's what `make lint-contract` is for (`tools/lint_contract.py`).
2. **`test_participant_files_are_present`**: checks that these files exist: `.claude/settings.json`, `.devcontainer/devcontainer.json`, `CLAUDE.md`, `Makefile`, `tracker/CR-2.md` and `tracker/README.md`. If any are missing, it fails and lists them (the message is in Latvian, "Trūkst faili").

When I ran it, both tests passed (`2 passed in 0.10s`).

## How it differs from the other targets

- `make verify-setup` runs only the smoke test, but it first checks your environment: the Python version, the required packages (FastAPI, Pydantic v2, httpx, pytest, schemathesis), the `claude` CLI, and that `setup/claude-answer.md` exists and isn't empty.
- `make test` will automatically pick up any new `test_*.py` files you add later, such as API tests for a tracker task.
