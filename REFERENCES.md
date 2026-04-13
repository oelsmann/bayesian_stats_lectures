# References and data sources

## Notebook topic references

### Bayesian Inference with MCMC explained
The notebook cites the following source as inspiration:

- Thomas Wiecki, *MCMC Sampling for Dummies*  
  https://twiecki.io/blog/2015/11/10/mcmc-sampling/

### Simple Linear Regression / Robust Linear Regression
The notebook cites these PyMC references:

- PyMC documentation: GLM linear example  
  https://www.pymc.io/projects/docs/en/stable/learn/core_notebooks/GLM_linear.html#glm-linear

- PyMC examples: robust generalized linear model  
  https://www.pymc.io/projects/examples/en/latest/generalized_linear_models/GLM-robust.html

## Data source used in the GNSS example

The GNSS station time-series section reads data directly from:

- University of Nevada, Reno Geodesy Lab  
  https://geodesy.unr.edu/gps_timeseries/IGS20/tenv3/IGS20/AC14.tenv3

This appears in the notebook as a remote text file loaded with `pandas.read_csv(...)`.

## Recommendation for reproducibility

For a public teaching repository, consider one of the following:

1. Keep the remote URL and note that internet access is required.
2. Download the source file into a local `data/` folder and cite its origin in the notebook and README.
3. Add a short paragraph describing:
   - station identifier
   - provider
   - access date
   - processing frame / product version if relevant

## Suggested citation note for the notebook

You may want to add a short markdown cell such as:

> The GNSS time-series data used in this example were obtained from the University of Nevada, Reno Geodesy Lab time-series archive.

## Suggested extra files you may want later

These are not strictly required, but are often useful for public repositories:

- `LICENSE`
- `CITATION.cff`
- `data/README.md` if local datasets are added
- `slides/` if you export lecture slides
