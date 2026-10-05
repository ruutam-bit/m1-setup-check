# What `make test` does

`make test` runs a single command, defined in `Makefile`:

```make
test:
	pytest -q
```

It runs pytest in quiet mode (`-q`) from the repo root. The repo has no `pytest.ini`, `pyproject.toml` or `conftest.py`, so pytest uses its default discovery and finds every `test_*.py` file. Right now that's only `tests/test_smoke.py`, which has two tests:

1. **`test_openapi_document_can_be_loaded`** loads `docs/openapi.yaml` as YAML. It checks that the `openapi` version starts with `3.` and that `paths` isn't empty. This is a quick check that the API contract file is valid, not a full review of the contract.
2. **`test_participant_files_are_present`** checks that these required files exist: `.claude/settings.json`, `.devcontainer/devcontainer.json`, `CLAUDE.md`, `Makefile`, `tracker/CR-2.md` and `tracker/README.md`. If any are missing, it fails and lists them (the message is in Latvian: "Trūkst faili: …").

## What it doesn't do

- It doesn't check the OpenAPI contract in depth. That's `make lint-contract`, which runs `tools/lint_contract.py docs/openapi.yaml`. Per `CLAUDE.md`, run it as well whenever you change the contract.
- It doesn't check your environment. That's `make verify-setup`: Python and package versions (including Pydantic v2), the `claude` CLI, and that `setup/claude-answer.md` exists. It also runs the smoke tests.

Any new `test_*.py` files you add later will be picked up by `make test` automatically.
