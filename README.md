# Manual

-----------

### Function `PS(S.formula, Y.formula, Y.family, data, monotonicity = "default", ER = c(), trunc = TRUE)`

- `S.formula`: the treatment variable `Z`, the intervention variable `D`, and the (softmax) model for the stratum that a unit belongs to.
  - Example: `Z + D ~ X1 + X2`
  - The treatment variable is `Z`.
  - The intervention variable is `D`.
  - $\mathrm{Pr}(S = s \mid X) \propto \exp(\beta_{s0} + \beta_{s1}X_1 + \beta_{s2}X_2)$ where $\beta_{s0}, \beta_{s1}, \beta_{s2}$ are parameters.
- `Y.formula`: the outcome variable `Y`, and the model (specified by `Y.family`) for the outcome within each stratum.
  - Example: `Y ~ X1 + X2`
  - The outcome variable is `Y`.
  - $\eta(\mathbb{E}[Y \mid X, S = s, Z = z]) = \gamma_{sz0} + \gamma_{sz1}X_1 + \gamma_{sz2}X_2$, where $\gamma_{sz0}, \gamma_{sz1}, \gamma_{sz2}$ are parameters and $\eta$ is the link function specified by `Y.family`.
- `Y.family`: a `family` object in R. The supported options include
  - `gaussian(link = "identity")` with `link` being one of
    - `identity`
    - `log`
    - `inverse`
  - `binomial(link = "logit")` with `link` being one of
    - `logit`
    - `probit`
    - `cauchit`
    - `log`
    - `cloglog`
  - `Gamma(link = "inverse")` with `link` being one of
    - `inverse`
    - `identity`
    - `log`
  - `poisson(link = "log")` with `link` being one of
    - `log`
    - `identity`
    - `sqrt`
  - `inverse.gaussian(link = "1/mu^2")` with link being one of 
    - `1/mu^2`
    - `inverse`
    - `identity`
    - `log`
- `data`: a `data.frame` object, should contain columns with names included in `S.formula` and `Y.formula`. All columns with these names should be numeric.
- `monotonicity`: options include `default`, `strong` and `none` (case not sensitive). Let "xy" denote the stratum where $D(0) = x, D(1) = y$, i.e., "00" being the never-taker stratum, "01" being the complier stratum, "10" being the defier stratum, and "11" being the always-taker stratum. `monotonicity` specifies the strata in consideration.
  - `default`: include "00", "01" and "11".
  - `strong`: include "00" and "01".
  - `none`: include "00", "01", "10" and "11".
- `ER`: a vector with possible values "00", "01", "10" and "11", the strata where exclusion-restriction is assumed.
- `trunc`: bool. If `trunc = TRUE`, we assume that outcomes where $D = 0$ are not observed. This can be used in the case where $D = 0$ denotes death where the outcome variable cannot be measured.

### Output:
A `PStrata` object, including
- `PSobject`: contains information of the model;
- `PSsummary`: summary information of the ACEs and strata proportion.

### S3 Methods:
`print.PStrata`, `summary.PStrata`, `plot.PStrata`.

-------------
# Known issues:

1. `Error: Can't add 'p2' to a ggplot object.` when calling `plot` on `PStrata` objects.

*Solution*: manually run `library(patchwork)` and try again.
