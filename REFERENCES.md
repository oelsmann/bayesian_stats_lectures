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

