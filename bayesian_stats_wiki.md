# Bayesian Statistics Wiki

Author: ChatGPT 5.4 Thinking

## 1. What is Bayesian statistics?

Bayesian statistics is a framework for inference in which uncertainty about unknown quantities is expressed with probability distributions. Instead of producing only a single best estimate, Bayesian analysis updates prior beliefs with observed data and returns a posterior distribution.

At the center of Bayesian inference is **Bayes' theorem**:

$$
p(\theta \mid y) = \frac{p(y \mid \theta) \, p(\theta)}{p(y)}
$$

where:

- $\theta$ denotes the unknown parameter(s)
- $y$ denotes the observed data
- $p(\theta \mid y)$ is the **posterior**
- $p(y \mid \theta)$ is the **likelihood**
- $p(\theta)$ is the **prior**
- $p(y)$ is the **evidence** or **marginal likelihood**

A common proportional form is:

$$
p(\theta \mid y) \propto p(y \mid \theta) \, p(\theta)
$$

This is often the most useful form in practice.

---

## 2. Prior

The **prior** describes what is believed about a parameter before seeing the data.

Examples:

- $\mu \sim \mathcal{N}(0, 10^2)$  
  A normal prior centered at 0 with large uncertainty.
- $\sigma \sim \text{HalfNormal}(1)$  
  A positive prior for a scale parameter.
- $p \sim \text{Beta}(2,2)$  
  A prior for a probability between 0 and 1.

Priors can be:

- **informative**: encode substantial prior knowledge
- **weakly informative**: mildly constrain unrealistic values
- **noninformative / diffuse**: try to let the data dominate

Good priors improve stability, interpretability, and regularization.

---

## 3. Likelihood

The **likelihood** describes how probable the observed data are for a given parameter value.

Examples:

### Normal likelihood
If observations are assumed normally distributed around a mean $\mu$:

$$
y_i \sim \mathcal{N}(\mu, \sigma^2)
$$

### Bernoulli likelihood
For binary observations:

$$
y_i \sim \text{Bernoulli}(p)
$$

### Poisson likelihood
For count data:

$$
y_i \sim \text{Poisson}(\lambda)
$$

The likelihood links the model parameters to the data.

---

## 4. Posterior

The **posterior** is the updated distribution of the parameters after combining prior information with the data.

It contains the full result of the inference:

- plausible parameter values
- uncertainty
- parameter dependence
- derived quantities

Typical summaries of a posterior include:

- posterior mean
- posterior median
- posterior standard deviation
- credible intervals
- highest density intervals

A 95% credible interval means that, under the model, there is a 95% posterior probability that the parameter lies in that interval.

---

## 5. Simple examples

### Example 1: Coin tosses
Suppose a coin is tossed $n$ times and gives $k$ heads.

- Likelihood:

$$
k \sim \text{Binomial}(n, p)
$$

- Prior:

$$
p \sim \text{Beta}(\alpha, \beta)
$$

- Posterior:

$$
p \mid k \sim \text{Beta}(\alpha + k, \beta + n - k)
$$

This is a classic conjugate Bayesian model.

### Example 2: Unknown mean of a normal distribution
Suppose:

$$
y_i \sim \mathcal{N}(\mu, \sigma^2)
$$

with known $\sigma$, and prior:

$$
\mu \sim \mathcal{N}(\mu_0, \tau_0^2)
$$

Then the posterior for $\mu$ is also normal.

### Example 3: Linear regression
A Bayesian linear regression may be written as:

$$
y_i \sim \mathcal{N}(\alpha + \beta x_i, \sigma^2)
$$

with priors such as:

$$
\alpha \sim \mathcal{N}(0, 10^2), \quad
\beta \sim \mathcal{N}(0, 10^2), \quad
\sigma \sim \text{HalfNormal}(1)
$$

The posterior then gives uncertainty not only in the regression line, but also in the parameters and predictions.

---

## 6. Why samplers are needed

For simple conjugate models, the posterior can be written down analytically. For most realistic models, however, the posterior cannot be computed in closed form.

In those cases, we use **sampling algorithms** to generate draws from the posterior distribution.

These draws are then used to estimate:

- parameter summaries
- credible intervals
- predictions
- model comparison quantities

---

## 7. Common samplers

### 7.1 MCMC
**Markov chain Monte Carlo (MCMC)** methods construct a Markov chain whose stationary distribution is the posterior.

The idea is to generate many dependent samples that, after convergence, behave like draws from the posterior.

### 7.2 Metropolis-Hastings
A general MCMC method that proposes a new value and accepts or rejects it according to an acceptance rule.

Pros:
- general
- conceptually simple

Cons:
- can mix slowly
- can be inefficient in high dimensions

### 7.3 Gibbs sampling
Samples each parameter conditionally on the current values of the others.

Pros:
- effective when conditional distributions are easy to sample

Cons:
- limited to models with tractable conditional updates

### 7.4 Hamiltonian Monte Carlo (HMC)
Uses gradients of the log posterior to explore parameter space more efficiently.

Pros:
- often much more efficient than random-walk samplers
- works well for many continuous high-dimensional models

Cons:
- more complex
- requires gradients

### 7.5 NUTS
The **No-U-Turn Sampler (NUTS)** is an adaptive version of HMC and is the default in many modern Bayesian libraries such as PyMC and Stan.

It usually provides efficient posterior exploration with minimal manual tuning.

---

## 8. Trace and trace plots

A **trace** is the sequence of sampled values of a parameter across MCMC iterations.

A **trace plot** shows:

- sampled value on the y-axis
- iteration number on the x-axis

Trace plots are useful for diagnosing sampling quality.

Good trace behavior:
- rapid mixing
- no clear trends
- different chains overlap well
- stable stationary behavior

Poor trace behavior:
- drift
- strong autocorrelation
- chains stuck in different regions
- sudden jumps or pathological behavior

---

## 9. Number of samples and tuning

### `n_samples`
The number of posterior draws retained after warm-up.

More samples generally improve Monte Carlo accuracy, but increase compute time.

### `n_tuning` or `tune`
The number of warm-up or adaptation iterations.

These samples are used to adapt the sampler and are usually discarded.

Typical idea:
- tuning phase: help sampler find efficient step sizes / mass matrix
- sampling phase: retain draws for inference

A run might look like:
- 4 chains
- 1000 tuning draws
- 2000 posterior draws per chain

This would produce 8000 retained posterior draws in total.

---

## 10. Convergence diagnostics

No single statistic proves convergence, but several diagnostics are commonly checked together.

### 10.1 R-hat
The potential scale reduction factor compares within-chain and between-chain variation.

Interpretation:
- close to 1.00 is good
- values noticeably above 1 suggest lack of convergence

A common rule of thumb is that R-hat should be below about 1.01.

### 10.2 Effective sample size (ESS)
Because MCMC samples are autocorrelated, the effective sample size is smaller than the raw number of draws.

Two common ESS summaries:
- **bulk ESS**: quality of estimation in the center of the posterior
- **tail ESS**: quality of estimation in the tails

Higher ESS is better.

### 10.3 Divergences
In HMC / NUTS, divergences can indicate that the sampler struggles with posterior geometry.

Divergences may suggest:
- poor parameterization
- problematic priors
- funnel-shaped geometry
- the need for re-scaling or non-centered parameterization

### 10.4 Monte Carlo standard error (MCSE)
The uncertainty caused by finite posterior sampling.

Smaller MCSE means more precise Monte Carlo estimates.

### 10.5 Energy and BFMI
For HMC-based methods, diagnostics related to energy behavior may reveal poor exploration.

Low BFMI can indicate issues with momentum resampling and inefficient sampling.

---

## 11. Model comparison statistics

Model comparison in Bayesian analysis should be done carefully. Common tools include:

### 11.1 WAIC
The **Widely Applicable Information Criterion** estimates out-of-sample predictive performance while adjusting for model complexity.

Lower WAIC is generally better.

### 11.2 LOO-CV
**Leave-one-out cross-validation** estimates predictive performance by approximating how well the model predicts unseen observations.

A common practical version is **PSIS-LOO**.

Lower expected predictive error is better.

### 11.3 Bayes factors
A Bayes factor compares the marginal likelihoods of two models:

$$
BF_{12} = \frac{p(y \mid M_1)}{p(y \mid M_2)}
$$

Interpretation:
- $BF_{12} > 1$: support for model $M_1$
- $BF_{12} < 1$: support for model $M_2$

Bayes factors can be useful, but they are often sensitive to priors.

### 11.4 Posterior predictive checks
Rather than a single number, posterior predictive checks compare simulated data from the model to the observed data.

These checks are often one of the most informative ways to assess fit.

---

## 12. Hierarchical models

A **hierarchical model** has parameters defined at multiple levels.

Example: students nested within schools.

At the observation level:

$$
y_{ij} \sim \mathcal{N}(\mu_j, \sigma^2)
$$

At the group level:

$$
\mu_j \sim \mathcal{N}(\mu_0, \tau^2)
$$

This structure allows **partial pooling**:
- groups share information
- estimates for sparse groups are stabilized
- uncertainty is handled more realistically

Hierarchical models are especially useful for:
- repeated measurements
- grouped data
- multi-site studies
- station networks
- longitudinal data

---

## 13. Data uncertainty vs process uncertainty

These represent different sources of variability and should often be modeled separately.

### Data uncertainty
Also called **observation uncertainty** or **measurement error**.

This reflects imperfections in the measurement system, for example:
- sensor precision limits
- instrument noise
- reported standard errors
- missing calibration effects

A simple data model is:

$$
y_{\text{obs},i} \sim \mathcal{N}(y_{\text{true},i}, \sigma_{\text{obs},i}^2)
$$

### Process uncertainty
This reflects variability or model mismatch in the underlying system, for example:
- natural variability
- unresolved dynamics
- simplifications in the physical model
- structural model error

A simple process model is:

$$
y_{\text{true},i} \sim \mathcal{N}(\mu_i, \sigma_{\text{proc}}^2)
$$

Together, a hierarchical formulation becomes:

$$
y_{\text{true},i} \sim \mathcal{N}(\mu_i, \sigma_{\text{proc}}^2)
$$

$$
y_{\text{obs},i} \sim \mathcal{N}(y_{\text{true},i}, \sigma_{\text{obs},i}^2)
$$

This separation is important because formal observational uncertainties and model residuals carry different information.

---

## 14. Practical workflow for Bayesian modeling

A typical workflow is:

1. define the scientific question
2. choose a likelihood
3. choose priors
4. fit the model with an appropriate sampler
5. inspect trace plots and diagnostics
6. summarize the posterior
7. perform posterior predictive checks
8. compare models if needed
9. interpret results in the context of the problem

---

## 15. Useful terms at a glance

- **Prior**: belief before seeing the data
- **Likelihood**: how the data arise given parameters
- **Posterior**: updated belief after seeing the data
- **Sampler**: algorithm used to approximate the posterior
- **Trace**: sampled parameter values across iterations
- **Tuning / warm-up**: adaptation phase before retained samples
- **R-hat**: convergence diagnostic
- **ESS**: effective number of independent samples
- **WAIC / LOO**: predictive model comparison tools
- **Hierarchical model**: model with multiple levels of parameters
- **Data uncertainty**: uncertainty from measurement
- **Process uncertainty**: uncertainty from the modeled system

---

## 16. Suggested software

Popular Bayesian software includes:

- PyMC
- Stan
- CmdStanPy
- ArviZ
- NumPyro
- Turing.jl

These tools support posterior sampling, diagnostics, visualization, and model comparison.

---

## 17. Final note

Bayesian statistics is especially powerful when uncertainty matters. Its main strength is that it provides a coherent way to combine prior knowledge, observed data, and model structure while preserving uncertainty throughout the analysis.


## Prompt to generate this file
'write a small wiki as md file for bayesian stats including bayes theorem, pror, posterior, likelihood, samplers, (some examples), trace, n samples, n tuning, convergence stats (overview of some params), model comparison stats, hierarchical models, data and process uncertainties, return as file'
