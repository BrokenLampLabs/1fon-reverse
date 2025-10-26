<img heigth="8" src="https://i.imgur.com/YPrwggD.png" alt="banner">

<h1 align="center">🧠 1fon-reverse 💹</h1>

<p align="center">An exploration into training foundational AI models using financial markets as the ultimate training ground, inspired by the vision of 1fon.</p>

<p align="center">
  <a href="https://joefavergel.github.io/">joefavergel.github.io</a>
  <br> <br>
  <a href="#about">About</a> •
  <a href="#features">Features</a> •
  <a href="#life-cycle">Life Cycle</a> •
  <a href="#contribute">Contribute</a> •
  <a href="#license">License</a>
  <br> <br>
  <a target="_blank">
    <img src="https://github.com/QData/TextAttack/workflows/Github%20PyTest/badge.svg" alt="Github Runner Covergae Status">
  </a>
  <a href="https://img.shields.io/badge/version-0.0.1-blue.svg?cacheSeconds=2592000">
    <img src="https://img.shields.io/badge/version-0.0.1-blue.svg?cacheSeconds=2592000" alt="Version" height="18">
  </a>
  <a href="https://www.linkedin.com/in/joseph-fabricio-vergel-becerra/" target="_blank">
    <img src="https://img.shields.io/badge/linkedin-%230077B5.svg?style=for-the-badge&logo=linkedin&logoColor=white" alt="LinkedIn">
  </a>
  <a href="https://twitter.com/joefavergel" target="_blank">
    <img alt="Twitter: joefavergel" src="https://img.shields.io/twitter/follow/joefavergel.svg?style=social"/>
  </a>
</p>


---

## About

`1fon-reverse` is a Python repository for exploring and reverse-engineering the concepts proposed by 1fon. The core thesis is that financial markets, with their inherent complexity and adversarial nature, are the ideal environment for training the next generation of AI models.

Instead of games, this project will use market data to research and develop new base models, employing techniques like open-ended learning and large-scale reinforcement learning (RL) to model and navigate market dynamics—the "final boss" of AI benchmarks. The goal is to build a practical framework for what could be considered an "AlphaZero for the real world."


---

## Features

`1fon-reverse` is built on `Python 3.12` with [pandas](https://pandas.pydata.org/), [numpy](https://numpy.org/), [scikit-learn](https://scikit-learn.org/stable/), [matplotlib](https://matplotlib.org/), [seaborn](https://seaborn.pydata.org/), and [plotly](https://plotly.com/python/), to acquire and process complex financial data, build and train machine learning and reinforcement learning models, and visualize market dynamics and agent performance.

For development, the library uses:

- Dependency management with [uv](https://docs.astral.sh/uv/)
- Formatting, import sorting, and linting with [ruff](https://docs.astral.sh/ruff/)
- Git hooks that run all the above with [pre-commit](https://pre-commit.com/)
- Testing with [pytest](https://docs.pytest.org/en/latest/)


---

## Life Cycle

As a proposal for the data science life cycle, [OSEMN](https://towardsdatascience.com/5-steps-of-a-data-science-project-lifecycle-26c50372b492) is mainly proposed. Standing for Obtain, Scrub, Explore, Model, and iNterpret, OSEMN is a five-phase life cycle.

Another good option is [Microsoft TDSP: The Team Data Science Process](https://learn.microsoft.com/en-us/azure/architecture/data-science-process/overview), which combines many modern agile practices with the life cycle. It has five steps: Business Understanding, Data Acquisition and Understanding, Modeling, Deployment, and Customer Acceptance.

The important thing is that if you think they should be combined to form your own life cycle, feel free to do so.


---

## Contribute

First, make sure you have `Python 3.12` installed. If your installed version does not match, you can create a conda environment with:

```sh
# Create and activate a Python 3.12 virtual environment
$ conda create -n py312 python=3.12
$ conda activate py312
```

Now, you can manage the project dependencies with `uv`. To create the virtual environment and install all dependencies, follow these steps:

```sh
# Install uv using pip
$ pip install uv

# Install development dependencies
$ uv sync --all-packages

# Activate the virtual environment
$ source .venv/bin/activate
```

Once the dependencies are installed, you need to let `Jupyter` know about this new `Python` environment by creating a kernel:

```sh
$ ipython kernel install --user --name KERNEL_NAME
```

Finally, before making any changes to the library, be sure to review the [GitFlow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow) guide and make any changes outside of the `main` branch.
