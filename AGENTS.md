# Agent Notes

## Repo Shape
- This repo is a Copier template, not a runnable Python package. The generated project lives under `{{project_name}}/` and most source files are `*.jinja` templates.
- The generated package layout is `src/{{module_name}}`, with the console entrypoint declared in `{{project_name}}/pyproject.toml.jinja` as `{{ project_name }} = "{{ module_name }}:main"`.

## Project Intent
- This repo is a Copier template for a Python application that aids the creating new Python projects.

## Coding Rules
- Coding rules are defined in `{{project_name}}/AGENTS.md.jinja` and should be followed in both the template and the generated project.

## Source Of Truth
- Treat `{{project_name}}/pyproject.toml.jinja` as the main source for tooling and test behavior.

## Generated Tooling
- Target Python is `3.12` from `{{project_name}}/.python-version` and `requires-python = ">=3.12"`.
- The generated project uses a `dependency-groups.dev` section, so prefer `uv`-style commands when giving setup or verification steps.
- Verified generated dev tools are `pytest`, `pytest-cov`, `mypy`, `ruff`, and `pre-commit`.

## Verification Commands
- Install dev dependencies in the generated project with `uv sync --dev`.
- Run unit tests with `uv run pytest`.
- Run component tests with `uv run pytest -m component`.
- Run slow tests with `uv run pytest -m slow`.
- Run type checking with `uv run mypy src`.
- Run the same repo hooks users will hit with `uv run pre-commit run --all-files`.

## Test Quirks
- Default pytest options in `pyproject.toml.jinja` exclude integration tests: `-m 'not component and not slow'`. Plain `pytest` will not run them.
- `testpaths` includes both `tests` and `integration`, so focused test commands may need an explicit marker to avoid surprises.
- `test/compontent_tests/conftest.py.jinja` auto-adds markers from the test node id: names containing `cc` become `component`

## Template Gotcha
- `copier.yaml` currently declares only `project_name` and `module_name`, but `README.md.jinja` and `pyproject.toml.jinja` also reference `short_description`. If you touch Copier questions or template rendering, reconcile that mismatch instead of assuming all variables are declared.
