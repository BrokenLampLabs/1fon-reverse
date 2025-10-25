<img heigth="8" src="https://i.imgur.com/5VTYUPE.png" alt="banner">

<h1 align="center">💻 Data/ML Project Template 🤖</h1>

<p align="center">A chill starting point for whipping up data projects that actually work, stay sane, and won’t break when you look at them funny.</p>

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

`data-project-template` is a Python repository template for developing data science and machine learning projects in early stages. This `Python` library is designed to training and generate the machine and deep learning models that predicts base transportation cost of FTL modality in United States & Canada.


---

## Features

`data-project-template` is built on `Python 3.12` with [pandas](https://pandas.pydata.org/), [numpy](https://numpy.org/) and [scikit-learn](https://scikit-learn.org/stable/), [matplotlib](https://matplotlib.org/), [seaborn](https://seaborn.pydata.org/), [plotly](https://plotly.com/python/)  among others, to preprocess the data, build the machine learning models, and visualize the results.

For development, the library use:

- Dependency mangament with [uv](https://docs.astral.sh/uv/)
- Formatting, import sorting and linting with [ruff](https://docs.astral.sh/ruff/)
- Git hooks that run all the above with [pre-commit](https://pre-commit.com/)
- Testing with [pytest](https://docs.pytest.org/en/latest/)


---

## Life Cycle

As a proposal for the data science life cycle, [OSEMN](https://towardsdatascience.com/5-steps-of-a-data-science-project-lifecycle-26c50372b492) is mainly proposed. Standing for Obtain, Scrub, Explore, Model, and iNterpret, OSEMN is a five-phase life cycle.

Other good option is [Microsoft TDSP: The Team Data Science Process](https://learn.microsoft.com/en-us/azure/architecture/data-science-process/overview) combines many modern agile practices with the life cycle. It has five steps: Business Understanding, Data Acquisition and Understanding, Modeling, Deployment, and Customer Acceptance.

The important thing is that if you think they should be combined and form their own life cycle, feel free to do so.


---

## Contribute

First, make sure that before enabling pipenv, you must have `Python 3.12` installed. If it does not correspond to the version you have installed, you can create a conda environment with:

```sh
# Create and activate python 3.12 virutal environment
$ conda create -n py312 python=3.12
$ conda activate py312
```

Now, you can managament the project dependencies with `Pipenv`. To create de virtual environment and install all dependencies follow:

```sh
# Install uv using pip
$ pip install uv

# Install development dependencies
$ uv sync --all-packages

# Activate virtual environment
$ source .venv/bin/activate
```

Once the dependencies are installed, we need to notify `Jupyter` of this new `Python` environment by creating a kernel:

```sh
$ ipython kernel install --user --name KERNEL_NAME
```

Finally, before making any changes to the library, be sure to review the [GitFlow](https://www.atlassian.com/es/git/tutorials/comparing-workflows/gitflow-workflow) guide and make any changes outside of the `master` branch.
