# Intro to Bayesian Statistics

Lecture materials and worked examples for an introduction to Bayesian statistics with PyMC.

This repository is designed so that the tutorial notebook can be added directly to the repo root (or to a `notebooks/` folder) without any other required setup files needing to be created manually.

## Repository contents

- `README.md` — project overview and setup instructions
- `environment.yml` — recommended Conda environment
- `requirements.txt` — optional pip-based installation
- `.gitignore` — ignores Python, Jupyter, cache, and environment artifacts
- `REFERENCES.md` — tutorial source links and dataset references


## Topics covered

The notebook currently includes:

`1_linear_regression_examples.ipynb`

- Bayesian inference with MCMC
- Simple linear regression
- Robust linear regression
- Time-dependent / heteroscedastic uncertainty
- Hierarchical models with observation and process uncertainty
- Application to GNSS station time series

## Quick start

### 1. Clone the repository

```bash
git clone [<your-repo-url>](https://github.com/oelsmann/bayesian_stats_lectures)
cd bayesian_stats_lectures
```

### 2. Create the Conda environment

```bash
conda env create -f environment.yml
conda activate bayes-stats
```

### 3. Launch Jupyter

```bash
jupyter lab
```

or

```bash
jupyter notebook
```

Open `1_linear_regression_examples.ipynb`.

## Installation guide

### Windows

1. Install Miniconda or Anaconda.
2. Open **Anaconda Prompt**.
3. Run:

```bash
conda env create -f environment.yml
conda activate bayes-stats
jupyter lab
```

If `conda` is not recognized, reopen the Anaconda Prompt instead of PowerShell or Command Prompt.

### macOS

1. Install Miniconda or Anaconda.
2. Open **Terminal**.
3. Run:

```bash
conda env create -f environment.yml
conda activate bayes-stats
jupyter lab
```

If your shell has not been initialized for Conda yet, run:

```bash
conda init
```

Then restart Terminal and activate the environment again.

### Linux

1. Install Miniconda or Anaconda.
2. Open a terminal.
3. Run:

```bash
conda env create -f environment.yml
conda activate bayes-stats
jupyter lab
```

If needed, initialize Conda first:

```bash
conda init bash
```

Then restart the shell.

## Included packages

The environment includes the main packages used in the notebook:

- `python`
- `numpy`
- `scipy`
- `pandas`
- `matplotlib`
- `seaborn`
- `jupyterlab`
- `notebook`
- `ipykernel`
- `arviz`
- `xarray`
- `pymc`
- `statsmodels`
- `pillow`

## Optional pip installation

Conda is recommended because PyMC and its numerical dependencies are usually smoother to install this way.

If you prefer pip inside an existing environment:

```bash
pip install -r requirements.txt
```

## Exporting the notebook as slides

If you want to convert the notebook into a Reveal.js slideshow:

```bash
jupyter nbconvert `1_linear_regression_examples.ipynb` --to slides
```

This creates:

```bash
bayesian_examples.slides.html
```

## Data and source references

See [`REFERENCES.md`](REFERENCES.md) for:

- tutorial inspiration and source links
- PyMC example references
- the GNSS time-series data source used in the notebook

