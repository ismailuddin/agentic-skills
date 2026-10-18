---
name: migrate-to-uv
description: Migrates a Python package project structure to use uv, ruff and prek
---

# migrate-to-uv

This skills documents how to migrate a Python package from the PEP-517 style
using `setup.cfg` to using `pyproject.toml` with `uv`.

## When to use

This should be used anytime the user requests to migrate a Python package
repository structure to a format compliant with `uv`.

## Instructions

1. Move the contents of `setup.cfg` to `pyproject.toml` and delete `setup.cfg`
2. Add a `.python-version` specifying the version as 3.11
3. Import the dependencies from `requirements-dev.txt` into `pyproject.toml` using the `uv` CLI (i.e. `uv add -r requirements-dev.txt`)
4. Switch the build system to use `uv_build`, and remove `build` from the dev dependencies.
5. Configure the version provider for Commitizen to be `uv`
6. Remove `pylint`, `flake8`, `isort`, `black` from the dev dependencies and replace it with `ruff`. Remove all the associated configuration files i.e. `pylintrc`, `.flake8` as well as any configuration in `setup.cfg` and / or `pyproject.toml`
7. Configure `ruff` to force single line imports using the guidance below.
8. Move the configuration from `pytest.ini` to `pyproject.toml`
9. Replace the pre-commit hooks for `black`, `pylint`, `isort`, `flake8` with `ruff`
10. Remove `pre-commit` from the dependencies, and replace it with `prek`
11. Run `ruff format` and `ruff check --fix` on the codebase.
12. Update any of the CI / CD pipelines to use `uv`.

### Configuring ruff to force single line imports

```toml
[tool.ruff.lint.isort]
force-single-line = true

[tool.ruff.lint]
select = ["F", "I"]
```
