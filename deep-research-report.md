---
---
---

# Estimating Labor Market Transition Probabilities from Eurostat LFS Aggregates

## Executive summary

The strongest conclusion from the literature and from your uploaded draft is that publicly available Eurostat Labour Force Survey cross-sections are **good enough to recover some economically important transition objects**, but **not rich enough to nonparametrically recover the full quarterly** $E/U/I$ transition matrix without extra structure. In particular, public aggregates support a robust two-state $E/U$ exercise in the spirit of Shimer, and they also support a useful three-state **partial-identification** exercise when unemployment duration, job tenure, and inactivity stocks are used together. What they do **not** support, in a fully model-free way, is the full set of destination-specific flows $E\to U$, $E\to I$, $U\to E$, $U\to I$, $I\to E$, and $I\to U$ at quarterly frequency.

That makes your project viable, but it also suggests a particular framing. The most defensible empirical contribution is not “I have fully recovered public Eurostat gross flows,” but rather: **“I recover a transparent, replicable set of labor-market turnover objects from public Eurostat aggregates, and I show what is and is not identified.”** Your uploaded draft is especially valuable here because it already moves beyond the standard two-state setup by using short job tenure from public EU-LFS tables to identify employment inflow and job destruction in a three-state framework. It is one of the clearest attempts to do this with public European aggregates alone.

Methodologically, the literature you need sits at the intersection of three traditions. The first is search-and-matching macroeconomics and stock-flow accounting, where Shimer-style formulas recover job-finding and separation rates from stocks and short-duration unemployment under a two-state structure. The second is duration econometrics, where Lancaster, Salant, Chesher–Lancaster, Heckman–Singer, van den Berg and others show that cross-sectional duration data are **stock-sampled current durations**, not completed spell durations, so identification requires renewal-theoretic and hazard-based reasoning. The third is the smaller European literature using LFS-type information, including OECD and EU applications, plus Hungarian work on stock-flow consistency and your own draft’s tenure-based extension.

My recommendation is a layered strategy. Start with a **baseline public-data estimator** that updates your draft’s three-state partial-identification system country by country, using quarterly public Eurostat aggregates from the first date where unemployment duration, tenure, and inactivity tables overlap consistently. Then add a second layer based on **full grouped duration/tenure inversion** to estimate duration-dependent exit hazards under transparent stationarity assumptions. Only after that should you consider a **constrained matrix-completion** step for a full $3\times 3$ transition matrix; if you do, it should be presented as a regularized completion problem, not as nonparametric identification.

## Eurostat LFS public information set

The Eurostat LFS is a harmonized household-survey framework that Eurostat disseminates for European countries on a quarterly basis; the standard legal and historical description emphasizes quarterly submission from 1998 onward and broad European coverage beyond the EU core, while your draft works with a wide quarterly European sample and shows how far one can get using only public aggregates.

For your specific project, the relevant public information set is narrower than “everything in the LFS” but wider than headline employment and unemployment rates. The draft you uploaded makes particularly effective use of three aggregate table families: unemployment by duration, employment by job tenure, and inactivity. It also shows why the practical start date for a balanced quarterly panel is often later than one might hope: in the draft, many countries effectively begin in 2005, with later starts for several others because the needed tenure series do not overlap earlier in a clean way.

The harmonized LFS framework also uses common classifications such as sex, age, education, occupation, industry, and region, which is why compositionally richer exercises are possible in principle. In practice, however, not every duration or tenure table is available jointly with every classification, so the empirical design usually needs to move from “fully crossed micro-style cells” to “coarse, feasible public cells.”

### Public Eurostat series that matter most

The table below synthesizes the public Eurostat aggregate inputs that matter most for this project. The dataset codes shown are the ones explicitly used in your draft and should be treated as the historical reference point for a current pull; Eurostat dataset aliases do occasionally shift, so they should be checked in the current Data Browser before coding the pipeline. fileciteturn0file0

| Concept | Public Eurostat source used or implied in your draft | What to extract | Why it matters |
|----|----|----|----|
| Employment stock $E_t$ | Quarterly LFS employment aggregates; in the draft, employment detail comes from the same table family used for tenure | Total employed count/share | Base stock equation |
| Unemployment stock $U_t$ and short-duration unemployment $U_t^s$ | `lfsq_ugad` in the draft | Total unemployed; combine the shortest unemployment-duration bins into “under 3 months” | Identifies unemployment inflow/outflow moments in two-state and three-state systems |
| Inactivity stock $I_t$ | `lfsq_igaww` in the draft | Total inactive count/share, and optionally willingness-to-work categories | Needed once inactivity is treated as a distinct state |
| Short-tenure employment $E_t^s$ and tenure distribution | `lfsq_egdn2` in the draft | Jobs started within the last 3 months; optionally fuller tenure distribution | Key public proxy for employment inflow and separation dynamics |
| Composition cells | Sex, age, education, industry, occupation where public overlap exists | Cell-specific stocks and short-duration/tenure shares | Controls composition bias |
| Optional validation benchmark | Eurostat transition tables, if currently disseminated | Do **not** use for estimation; use only for validation if allowed by your research design | External benchmark on plausibility |

A useful practical choice is to run the core quarterly estimators on a common $15\!-\!64$ universe first, because that is the age range used in your draft and tends to keep employment, unemployment, and inactivity concepts aligned. After that, you can test whether $15\!-\!74$ is empirically preferable for countries where duration tables are cleaner in the broader age band.

Two preprocessing details from your draft are especially important. First, the tenure table can contain nonresponse or residual categories, and those residuals need a systematic treatment rather than silent dropping. Second, the draft works with non-seasonally-adjusted public data and then seasonally adjusts derived series for presentation, which is usually the safer choice if you want to preserve accounting identities in the raw identification step.

## Literature and method families

### Search-and-matching stock-flow methods

The first literature family is the search-and-matching tradition associated with Diamond, Mortensen, and Pissarides, and the macro stock-flow implementations built on it. In that tradition, employment and unemployment are connected by job-finding and job-destruction processes, and aggregate labor-market stocks are interpreted as the outcome of underlying matching hazards. Shimer’s contribution was to show that short-duration unemployment, observed in standard headline-type survey data, contains enough information to recover two-state gross flows under a simple timing structure. This is why his approach became so influential in empirical labor-flow work.

In the two-state case, the steady-state benchmark is very simple. If $p_{EU}$ is the employment-to-unemployment probability and $p_{UE}$ the unemployment-to-employment probability, then steady-state unemployment satisfies

$$
u^* = \frac{p_{EU}}{p_{EU} + p_{UE}}.
$$

More generally, if $P$ is a transition matrix and $\pi$ the stationary distribution, then

$$
\pi = \pi P, \qquad \iota' \pi = 1.
$$

This steady-state relation is analytically useful, but it does **not** by itself solve the three-state identification problem, because a $3\times 3$ Markov matrix has too many free off-diagonal elements relative to public stock moments. It is best used as a benchmark or regularizer, not as the whole estimator.

Your draft is very explicit on the limitation of stopping at the two-state approach in Europe: inactivity is too important to be treated as negligible. It therefore extends the standard approach by using job tenure information to recover a richer set of objects, while still acknowledging that full destination-specific identification remains out of reach under weak assumptions. That is exactly the right place to start for a Eurostat-based paper.

### Duration and current-duration econometrics

The second literature family is duration econometrics. This is crucial because unemployment duration and job tenure in public cross-sections are **not complete spell lengths**. They are ages of ongoing spells, observed through **stock sampling**, which overweights long spells. This is the classical stock-versus-flow sampling problem emphasized by Salant, Lancaster, and Chesher–Lancaster, and it is the reason renewal theory sits underneath any serious attempt to infer flows from current-duration data.

Under stationarity, if $T$ is the completed spell duration and $A$ is the current duration observed in a cross-section of ongoing spells, then the age density is proportional to the survivor function of the completed duration distribution:

$$
f_A(a) = \frac{S_T(a)}{\mathbb{E}[T]}, \qquad S_T(a)=\Pr(T>a), \qquad a\ge 0.
$$

In discrete time, with $T\in\{1,2,\dots,J\}$ and current duration $A\in\{0,1,\dots,J-1\}$, the stock-sampling identity can be written as

$$
q_a = \Pr(A=a) = \frac{1}{\mu}\sum_{j=a+1}^{J}\pi_j,
\qquad
\mu = \sum_{j=1}^{J} j\pi_j,
$$

where $\pi_j=\Pr(T=j)$ is the completed-duration distribution. This is the core “hazard inversion” or deconvolution relation that lets you go from cross-sectional duration shares back to an implied spell-distribution or hazard profile.

That same literature also explains why parametric and semiparametric duration models matter here. If you estimate hazards from grouped current-duration data, you immediately face unobserved heterogeneity, grouped intervals, open-ended top bins, and sometimes heaping or preferential reporting. This is why the classic references emphasize mixed proportional hazard models and semiparametric approaches, and why newer grouped-current-duration papers show that censoring and reporting assumptions can drive nontrivial bias.

For your project, this branch of the literature matters less because you want to estimate individual duration dependence per se, and more because it tells you **how much structure is needed** to interpret Eurostat duration and tenure tables. The important implication is: if you use the full grouped duration or tenure distribution rather than just a short-spell bin, you are implicitly entering this literature and should be explicit about stationarity, tail assumptions, and selection.

### European applications and what is missing

The literature that is closest to your actual design is surprisingly thin. Many influential papers on labor-market flows use matched microdata, administrative records, or linked survey waves. In Europe, some papers use the EU-LFS question on labor-market status one year earlier rather than truly quarterly public cross-sections, which is informative but conceptually different. Your draft explicitly notes this and uses that contrast to motivate a public-aggregate quarterly approach.

The closest applied references in your draft include Hobijn and Şahin, who extend Shimer-style logic cross-country while staying essentially in a two-state framework; Casado, Fernández and Jimeno, who infer worker flows in the EU using retrospective information; the Hungarian stock-flow consistency and raking literature; and your own tenure-based three-state system. The recent paper by Fiaschi and Tealdi is not a public-aggregate paper, but it is still useful because it shows how a modern multi-state labor-flow decomposition is done when longitudinal data are available, which makes it a natural validation benchmark for your aggregate estimator.

The clearest hole in the literature, and therefore your strongest opportunity, is this: **there is still little public-data work that combines unemployment duration, job tenure, and inactivity stocks to deliver a transparent quarterly three-state flow decomposition for Eurostat countries.** There is rich theory, rich micro evidence, and some public-data building blocks, but the exact bridge you want is still relatively underdeveloped.

## Identification limits and practical estimation workflows

The natural state vector is

$$
s_t = \begin{pmatrix} e_t & u_t & i_t \end{pmatrix},
$$

with a quarterly transition matrix

$$
P_t =
\begin{pmatrix}
1-p_{EU,t}-p_{EI,t} & p_{EU,t} & p_{EI,t} \\
p_{UE,t} & 1-p_{UE,t}-p_{UI,t} & p_{UI,t} \\
p_{IE,t} & p_{IU,t} & 1-p_{IE,t}-p_{IU,t}
\end{pmatrix},
$$

so that

$$
s_t = s_{t-1} P_t.
$$

With only the three stocks $e_t,u_t,i_t$, there are only two independent accounting equations because the shares sum to one, but there are six off-diagonal transition probabilities. Public duration and tenure moments help, but they do not automatically identify destination-specific flows unless you impose additional structure. This is the central identification fact that should govern your entire empirical design.

### Comparison of candidate methods

The table below is a synthesis of the method families most relevant to your project.

| Method | Extra public moments required | What it identifies well | Main assumptions | Main strengths | Main weaknesses |
|----|----|----|----|----|----|
| Stock-only steady-state Markov inversion | Only $E,U,I$ shares | Almost nothing beyond low-dimensional calibrations | Quasi-steady-state, sparse structure, or strong restrictions on $P_t$ | Very simple | Heavily underidentified in three states |
| Two-state Shimer method | Unemployment stock + short-duration unemployment | $E\to U$ and $U\to E$ in a closed $E/U$ system | No inactivity margin, same-quarter timing, short-duration bin correctly measured | Transparent and easy to replicate | Misses inactivity and job-to-job contamination |
| Three-state partial identification from your draft | $E,U,I$ stocks + short-duration unemployment + short job tenure | Employment inflow $\lambda_t f_t$, job destruction $\rho_t$, search intensity $\lambda_t$, implied finding $f_t$ | Public short-tenure jobs proxy new employment; timing rules; common search probability among nonemployed | Best public-data three-state baseline | Still not a full $3\times 3$ matrix |
| Full duration/tenure hazard inversion | Entire unemployment-duration and tenure distributions | Total exit hazards by elapsed duration; inflow rates under stationarity | Renewal/current-duration logic, stationarity or quasi-stationarity, top-bin tail assumption | Exploits much more information | Does not separate destinations without extra structure |
| Cell-specific hazard estimation and reaggregation | Duration/tenure and stocks by sex/age/education/sector | Composition-adjusted hazards | Sufficient cell coverage; common definitions across cells | Reduces aggregation bias | Public tables often sparse or inconsistent across dimensions |
| Constrained matrix completion or raking | Any of the above, plus stock-consistency constraints | A full matrix as a regularized estimate | Priors or penalties on missing destination-specific pieces | Practical way to get a usable $3\times 3$ matrix | Not nonparametric identification; results depend on priors |

### Baseline workflow

For a public-data paper, the best baseline is still the Shimer-style two-state system plus your draft’s three-state extension.

In the two-state case, let $u_t$ be the unemployment share and $u_t^s$ the share unemployed for less than one quarter. If $f_t$ is the job-finding probability and $s_t$ the separation probability, then

$$
u_t = (1-f_t)\left[u_{t-1}+s_t(1-u_{t-1})\right].
$$

Since those who were already unemployed last quarter and remain unemployed must have duration above one quarter, the short-duration moment implies

$$
u_t-u_t^s = (1-f_t)u_{t-1},
$$

so

$$
f_t = 1-\frac{u_t-u_t^s}{u_{t-1}},
$$

and therefore

$$
s_t = \frac{u_t-(1-f_t)u_{t-1}}{(1-f_t)(1-u_{t-1})}.
$$

These are the workhorse formulas that make short-duration unemployment so useful.

Your draft generalizes this logic by introducing inactivity and a search-intensity parameter. Let $\rho_t$ denote job destruction from employment, $\lambda_t$ the probability that a person without a job searches, and $f_t$ the probability that a searcher finds employment. Then the draft’s system is

$$
e_t = (1-\rho_t)e_{t-1} + f_t s_t,
$$

$$
u_t = (1-f_t)s_t,
$$

$$
i_t = (1-\lambda_t)\left(\rho_t e_{t-1}+u_{t-1}+i_{t-1}\right),
$$

with

$$
s_t = \lambda_t\left(\rho_t e_{t-1}+u_{t-1}+i_{t-1}\right).
$$

If $e_t^s$ denotes employment with job tenure under three months, then the draft shows that the key objects can be identified as

$$
\lambda_t f_t = \frac{e_t^s}{1-e_{t-1}},
$$

$$
\rho_t = \frac{1-\frac{e_t-e_t^s}{e_{t-1}}}{1-\lambda_t f_t},
$$

$$
\lambda_t = 1-\frac{i_t}{\rho_t e_{t-1}+u_{t-1}+i_{t-1}},
\qquad
f_t = \frac{\lambda_t f_t}{\lambda_t}.
$$

This is, in my view, the single most useful public-data system for your project. It converts public quarterly Eurostat aggregates into interpretable three-state turnover objects without pretending to have fully identified all six destination-specific flows.

### Advanced workflow

Once the baseline system is in place, the next step is to use the **full grouped duration and tenure distributions** instead of just the short-spell bins. Under a discrete current-duration setup, if $q_{a,t}$ is the observed share at age $a$ and $\pi_{j,t}$ the latent distribution of completed spell lengths, then

$$
q_{a,t} = \frac{1}{\mu_t}\sum_{j=a+1}^{J}\pi_{j,t},
\qquad
\mu_t = \sum_{j=1}^{J} j\,\pi_{j,t}.
$$

This is a lower-triangular deconvolution problem. If you estimate $\pi_{j,t}$, you can back out duration-dependent exit hazards. With a unit-interval discrete approximation, a simple hazard relation is

$$
h_{a,t} = 1-\frac{q_{a+1,t}}{q_{a,t}}.
$$

For grouped bins, you replace this with a grouped-likelihood or minimum-distance fit.

A practical implementation is to parameterize the hazard as piecewise constant, Weibull, Gompertz, or a spline; estimate those parameters from grouped current-duration shares; and then infer a total exit hazard from unemployment and a total separation hazard from the tenure distribution. The public data then identify **total exits from** $U$ and **total exits from** $E$ by spell age, but not the destination split between employment and inactivity, or unemployment and inactivity, without more assumptions. That is why this should be treated as a second layer on top of the baseline public estimator, not as a substitute for it.

If you ultimately want a usable full $3\times 3$ matrix for simulation or decomposition, the clean way to proceed is a constrained minimum-distance or Bayesian completion problem. In generic form,

$$
\hat P_t
=
\arg\min_{P\in\mathcal{P}}
\left[
w_1 \|s_t-s_{t-1}P\|^2
+
w_2 \|M_t(P)-\hat M_t\|^2
+
w_3 \Omega(P)
\right],
$$

where $M_t(P)$ collects the duration and tenure moments that the public data do identify, and $\Omega(P)$ is a smoothness or shrinkage penalty. This approach is entirely legitimate, but it should be presented honestly as **regularized completion**, not as fully identified gross flows. citeturn49search0turn49search1

## Diagnostics, sensitivity, and validation

A serious public-data paper on labor flows lives or dies on diagnostics. The public moments are noisy, the duration measures are stock-sampled, and the short-tenure series can be contaminated by job-to-job changes or classification quirks. The role of diagnostics is therefore not cosmetic; it is the main credibility device.

### Diagnostics that should be standard

| Diagnostic | How to do it | Why it matters |
|----|----|----|
| Stock reconciliation | Check $\hat s_t = s_{t-1}\hat P_t$ against observed $s_t$ | Verifies accounting consistency |
| Short-spell replication | Compare model-implied $\hat u_t^s$ and $\hat e_t^s$ to observed short-duration/short-tenure shares | Core identification moments must fit |
| Probability bounds | Enforce $0\le p_{ij,t}\le 1$ and row sums equal one | Prevents impossible transitions |
| Tail sensitivity | Vary the treatment of open-ended duration and tenure bins | Mean duration and hazard tails are fragile |
| Timing sensitivity | Compare same-quarter, next-quarter, and continuous-time interpretations | Time aggregation bias can be large |
| Composition sensitivity | Re-estimate in sex-age-education or sector cells and reaggregate | Aggregation bias can mimic duration dependence |
| Seasonal sensitivity | Compare raw, seasonally adjusted, and four-quarter-smoothed estimates | Public quarterly flows are highly seasonal |
| External validation | Compare to Eurostat transition rates or administrative data where available | Most persuasive credibility check |

A useful forecast diagnostic is to evaluate the one-step-ahead stock error

$$
\varepsilon_t = s_t - s_{t-1}\hat P_t,
$$

and summarize it with country-specific RMSEs. A useful duration-fit diagnostic is a grouped Pearson statistic or deviance computed from the observed and fitted duration shares. These checks are especially important if you move beyond the short-bin estimator to a grouped hazard model.

The most important sensitivity checks for your design are, in my opinion, these. First, vary the treatment of nonresponse cells in the tenure table, because your draft already shows that residual categories are nontrivial in practice. Second, test whether short-tenure employment plausibly reflects only nonemployment-to-employment hiring, or whether job-to-job mobility is contaminating it in some countries and sectors. Third, vary the top-bin tail assumption in any hazard inversion using full duration distributions. Fourth, compare aggregate results to results aggregated from coarse cells such as sex by age or sex by education.

For external validation, I would use three layers. The first is internal consistency, meaning you predict next-quarter stocks well. The second is **published transition benchmarks**, including Eurostat transition rates if you allow them only for validation, never for estimation. The third is country-specific administrative evidence where available, such as social-security hires and separations, public-employment-service outflows from unemployment, or national matched-LFS studies. Backing the public-aggregate estimator against at least one non-survey benchmark in a few countries will materially strengthen the paper.

## Recommended project plan

The most credible paper design is an explicitly layered one.

First, replicate and update your draft’s baseline quarterly three-state estimator on the public Eurostat aggregates, country by country, on the longest overlap where all key tables exist consistently. Second, estimate the same objects within coarse demographic cells and aggregate them back up, so that your headline results are not driven by composition shifts. Third, add a grouped duration/tenure inversion layer to estimate duration-dependent total exit hazards from unemployment and employment. Fourth, if you need a full $3\times 3$ matrix for decomposition exercises, estimate it as a constrained completion problem and keep that clearly separate from the baseline identified objects.

### Estimation pipeline

``` mermaid
flowchart TD
    A[Audit Eurostat public tables] --> B[Build quarterly E U I stocks]
    B --> C[Construct short-duration unemployment and short-tenure employment shares]
    C --> D[Estimate two-state Shimer baseline]
    C --> E[Estimate three-state partial-identification system]
    B --> F[Assemble full grouped duration and tenure distributions]
    F --> G[Estimate grouped current-duration hazards]
    D --> H[Compare and reconcile country-level results]
    E --> H
    G --> H
    H --> I[Optional constrained matrix completion]
    I --> J[Diagnostics sensitivity validation]
    J --> K[Country results panel analysis and decomposition]
```

### Suggested timeline

``` mermaid
timeline
    title Suggested research timeline
    Month 1 : Audit current Eurostat dataset aliases
            : Write extraction and cleaning scripts
    Month 2 : Replicate baseline two-state and three-state estimators
            : Build balanced and unbalanced panels
    Month 3 : Add composition-adjusted cell estimators
            : Produce first cross-country stylized facts
    Month 4 : Estimate grouped duration and tenure hazard models
            : Run tail and reporting sensitivity checks
    Month 5 : Add constrained matrix-completion extension
            : Validate against published transitions or administrative benchmarks
    Month 6 : Finalize decomposition exercises
            : Write methods appendix and robustness section
```

### Minimal implementation skeleton

The code below is deliberately schematic. The dataset codes are the ones used in your draft and should be checked against the current Eurostat Data Browser before implementation.

``` r
# R pseudo-code
library(eurostat)
library(dplyr)

u_dur <- get_eurostat("lfsq_ugad", time_format = "date")
e_ten <- get_eurostat("lfsq_egdn2", time_format = "date")
i_pop <- get_eurostat("lfsq_igaww", time_format = "date")

# Filter to common universe, e.g. sex = total, age = 15-64, NSA, persons
# Build:
# u_t      = total unemployment share
# u_t_s    = unemployment duration < 3 months
# e_t      = employment share
# e_t_s    = tenure < 3 months
# i_t      = inactivity share

df <- df %>%
  group_by(country) %>%
  arrange(time) %>%
  mutate(
    f_ue = 1 - (u_t - u_t_s) / lag(u_t),
    s_eu = (u_t - (1 - f_ue) * lag(u_t)) / ((1 - f_ue) * (1 - lag(u_t))),
    lambda_f = e_t_s / (1 - lag(e_t)),
    rho = (1 - (e_t - e_t_s) / lag(e_t)) / (1 - lambda_f),
    lambda = 1 - i_t / (rho * lag(e_t) + lag(u_t) + lag(i_t)),
    f_search = lambda_f / lambda
  )
```

``` python
# Python pseudo-code for grouped duration inversion
import numpy as np
from scipy.optimize import minimize

def backward_probs(pi):
    # pi[j-1] = P(T = j), j = 1,...,J
    j = np.arange(1, len(pi) + 1)
    mu = np.sum(j * pi)
    S = np.flip(np.cumsum(np.flip(pi)))   # survivor at integer durations
    q = S / mu                            # stock-sampled current-duration ages
    return q

def objective(theta, q_obs):
    # Example: softmax(theta) gives a latent completed-duration distribution pi
    ex = np.exp(theta - np.max(theta))
    pi = ex / ex.sum()
    q_fit = backward_probs(pi)
    # KL loss + smoothness penalty
    kl = np.sum(q_obs * (np.log(q_obs + 1e-12) - np.log(q_fit[:len(q_obs)] + 1e-12)))
    smooth = np.sum(np.diff(pi, 2)**2)
    return kl + 1e-3 * smooth

res = minimize(objective, x0=np.zeros(J), args=(q_obs,))
pi_hat = np.exp(res.x - np.max(res.x))
pi_hat = pi_hat / pi_hat.sum()
q_hat = backward_probs(pi_hat)
hazard_hat = 1 - q_hat[1:] / q_hat[:-1]
```

## Open questions and limitations

The largest unresolved issue is still destination-specific identification. Public Eurostat unemployment-duration data tell you about **total exit from unemployment**, and public tenure data tell you about **total exit from employment**. They do not, by themselves, tell you how those exits split across employment, unemployment, and inactivity. Your draft’s three-state system solves this only partially, and I think that is a strength rather than a weakness because it keeps the identifying assumptions visible.

A second unresolved issue is job-to-job contamination in the short-tenure series. If workers who switch directly from one job to another appear in the public tenure table as “tenure under three months,” then $e_t^s$ is not purely a nonemployment-to-employment inflow measure. This is not fatal, but it does mean that the cleanest interpretation of the tenure moment is likely to vary by country, sector, and business-cycle phase. This is exactly the sort of issue that should be addressed empirically with sectoral and demographic robustness checks.

A third practical limitation is that I have high confidence in the **methodological map** and in the **historical Eurostat dataset codes used in your draft**, but not every current Eurostat table alias was directly verified in the present search environment. For implementation, I would therefore treat the codes in your draft as the starting point and verify current aliases in the live Data Browser before binding the scripts.

## Bibliography

### Core literature on labor flows and search-and-matching

-   Casado, J. M., Fernández, C., & Jimeno, J. F. (2015). *Worker flows in the European Union during the Great Recession*. ECB Working Paper No. 1862.
-   Campolmi, A., & Gnocchi, S. (2014). *Labor market participation, unemployment and monetary policy*. *Journal of Monetary Economics*, 65, 17–29.
-   Elsby, M. W. L., Hobijn, B., & Şahin, A. (2013). *Unemployment dynamics in the OECD*. *Review of Economics and Statistics*, 95(2), 530–548.
-   Elsby, M. W. L., Hobijn, B., & Şahin, A. (2015). *On the importance of the participation margin for labor market fluctuations*. *Journal of Monetary Economics*, 72, 64–82.
-   Fiaschi, D., & Tealdi, C. (2021). *A general methodology to measure labour market dynamics*. arXiv preprint. citeturn47academia1
-   Frazis, H. J., Robison, E. L., Evans, T. D., & Duff, M. A. (2005). *Estimating gross flows consistent with stocks in the CPS*. *Monthly Labor Review*, 128(9), 3–9.
-   Hobijn, B., & Şahin, A. (2009). *Job-finding and separation rates in the OECD*. *Economics Letters*, 104(3), 107–111.
-   Mortensen, D. T., & Pissarides, C. A. (1994). *Job creation and job destruction in the theory of unemployment*. *Review of Economic Studies*, 61(3), 397–415.
-   Pissarides, C. A. (1985). *Short-run equilibrium dynamics of unemployment, vacancies, and real wages*. *American Economic Review*, 75(4), 676–690.
-   Pissarides, C. A. (2000). *Equilibrium Unemployment Theory*. MIT Press. citeturn55search1turn55search4
-   Shimer, R. (2005). *The cyclical behavior of equilibrium unemployment and vacancies*. *American Economic Review*, 95(1), 25–49.
-   Shimer, R. (2010). *Labor Markets and Business Cycles*. Princeton University Press. citeturn54search0

### Duration, current-duration, and stock-flow sampling literature

-   Chesher, A., & Lancaster, T. (1981). *Stock and flow sampling*. *Economics Letters*, 8(1), 63–65. citeturn49search0
-   Hausman, J. A., & Woutersen, T. (2014). *Estimating a semiparametric duration model without specifying heterogeneity*. *Journal of Econometrics*, 178, 114–131. citeturn49search0
-   Heckman, J. J., & Singer, B. (1984). *A method for minimizing the impact of distributional assumptions in econometric models for duration data*. *Econometrica*, 52(2), 271–320. citeturn49search1
-   Kiefer, N. M. (1988). *Economic duration data and hazard functions*. *Journal of Economic Literature*, 26(2), 646–679.
-   Lancaster, T. (1979). *Econometric methods for the duration of unemployment*. *Econometrica*, 47(4), 939–956. citeturn49search0
-   Lancaster, T. (1990). *The Econometric Analysis of Transition Data*. Cambridge University Press. citeturn49search1turn55search3
-   Salant, S. W. (1977). *Search theory and duration data: A theory of sorts*. *Quarterly Journal of Economics*, 91(1), 39–57. citeturn49search0turn49search1
-   van den Berg, G. J. (2001). *Duration models: specification, identification and multiple durations*. In J. J. Heckman & E. Leamer (Eds.), *Handbook of Econometrics*, Vol. 5. Elsevier.
-   van Es, B., Klaassen, C. A. J., & Mokveld, P. J. (2006). *A comparison of information concerning the regression parameter in the accelerated failure time model under current duration and length biased sampling: Does it pay to be patient?* arXiv preprint. citeturn49academia3

### Grouped current-duration and related methodological sources

-   Lee, C. H., Susmann, H., & Alkema, L. (2023). *A Bayesian analysis of current duration data with reporting issues*. arXiv preprint. citeturn49academia4
-   McLain, A. C., Sundaram, R., Thoma, M., & Buck Louis, G. M. (2018). *Cautionary note on semiparametric modeling of grouped current duration data with preferential reporting*. arXiv preprint. citeturn50academia0

### European and Hungarian literature directly relevant to your design

-   Cseres-Gergely, Zs. (2011). *Munkapiaci áramlások, konzisztencia és gereblyézés*. *Statisztikai Szemle*, 89, 481–500.
-   Cseres-Gergely, Zs., & Kónya, I. (2016). *Can we measure labour market flows better with a bit more of economics?* Unpublished working-paper draft provided by the user. fileciteturn0file0
-   Mihályffy, L. (2012). *Munkapiaci áramlások, konzisztencia – egy alternatív megoldás*. *Statisztikai Szemle*, 90, 394–423.
-   Morvay, E. (2012). *Sztochasztikus ciklikus munkaerő-áramlás a visegrádi országokban*. *Statisztikai Szemle*, 90, 815–843.

### Supporting web references consulted

-   *Flow sampling* encyclopedia entry, summarizing stock-versus-flow sampling and the relevant econometric references. citeturn49search0
-   *Unobserved heterogeneity in duration models* encyclopedia entry, summarizing mixed-hazard and semiparametric duration references. citeturn49search1
-   *Discrete-time proportional hazards* encyclopedia entry, useful for grouped hazard notation. citeturn45search2
-   *Labour Force Survey* encyclopedia entry, summarizing Eurostat legal basis and quarterly European LFS coverage. citeturn41search0turn21search2
-   *Renewal theory* encyclopedia entry, useful for the inspection-paradox intuition. citeturn25search3

### Eurostat data and documentation entry points

The items below are the main entry points you will likely need. The dataset codes are those used in your uploaded draft and should be verified against the current Eurostat Data Browser before coding. fileciteturn0file0

-   Eurostat Data Browser: `https://ec.europa.eu/eurostat/data/database`
-   Eurostat dissemination API template: `https://ec.europa.eu/eurostat/api/dissemination/statistics/1.0/data/<dataset_code>`
-   Eurostat Labour Force Survey overview: `https://ec.europa.eu/eurostat/web/lfs/overview`
-   Historical public table code used in the draft for unemployment duration: `https://ec.europa.eu/eurostat/api/dissemination/statistics/1.0/data/lfsq_ugad`
-   Historical public table code used in the draft for employment by tenure: `https://ec.europa.eu/eurostat/api/dissemination/statistics/1.0/data/lfsq_egdn2`
-   Historical public table code used in the draft for inactivity: `https://ec.europa.eu/eurostat/api/dissemination/statistics/1.0/data/lfsq_igaww`
