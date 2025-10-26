# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

`1fon-reverse` is a research project exploring foundational AI models trained using financial markets data. The core thesis is that financial markets, with their inherent complexity and adversarial nature, serve as an ideal training ground for next-generation AI models. This project employs open-ended learning and large-scale reinforcement learning (RL) to model and navigate market dynamics.

The project is built on Python 3.12 and follows a data science lifecycle approach (OSEMN: Obtain, Scrub, Explore, Model, iNterpret).

## Development Environment Setup

### Initial Setup
```sh
# Install uv (package manager)
pip install uv

# Install all dependencies including dev dependencies
uv sync --all-packages

# Activate virtual environment
source .venv/bin/activate

# Create Jupyter kernel for this environment
ipython kernel install --user --name KERNEL_NAME
```

### Python Version
This project requires Python 3.12. If you don't have this version, create a conda environment:
```sh
conda create -n py312 python=3.12
conda activate py312
```

## Common Commands

### Testing
```sh
# Run all tests
pytest

# Run tests with coverage
pytest --cov

# Run a single test file
pytest tests/test_file.py

# Run a specific test function
pytest tests/test_file.py::test_function_name
```

### Linting and Formatting
```sh
# Run ruff linter (with auto-fix)
ruff check --fix .

# Run ruff formatter
ruff format .

# Check without fixing
ruff check .
```

### Pre-commit Hooks
```sh
# Install pre-commit hooks
pre-commit install

# Run pre-commit on all files
pre-commit run --all-files
```

The pre-commit hooks automatically run:
- YAML validation
- End-of-file fixer
- Trailing whitespace removal
- Ruff linting (with auto-fix)
- Ruff formatting

## Code Architecture

### Project Structure
- `src/`: Core library code
  - `utils.py`: Contains utility functions including `set_seed_everything()` for reproducible random number generation (default seed: 20180507)
- `tests/`: Test files using pytest
- `notebooks/`: Jupyter notebooks for exploration and experimentation
- `scripts/`: Placeholder for future scripts

### Key Dependencies
- Data manipulation: pandas, numpy
- Machine learning: scikit-learn
- Visualization: matplotlib, seaborn, plotly
- Jupyter support: ipykernel, ipywidgets
- Dev tools: pytest, ruff, pre-commit

### Code Style
- Line length: 88 characters (enforced by ruff)
- Formatting is handled by ruff (follows Black-compatible style)
- Import sorting is managed by ruff

## Workflow

### Git Branching
This project follows GitFlow:
- `main`: Production-ready code
- `develop`: Integration branch for features
- Feature branches: Branch off from `develop`, never work directly on `main`

### Making Changes
1. Create a feature branch from `develop`
2. Make changes
3. Pre-commit hooks will automatically run on `git commit`
4. Create pull request to merge back into `develop`

## Research Focus

The project explores training AI models using financial markets data as a more challenging alternative to game-based environments. The approach aims to build what could be considered an "AlphaZero for the real world" using market dynamics as the training ground.
