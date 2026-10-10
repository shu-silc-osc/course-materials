# Applied Econometrics: Causal Inference and Research Design

> [!abstract] Learning path
> Begin with regression, define potential outcomes and causal estimands, assess how treatment is assigned, and choose a design—randomisation, adjustment/matching, difference-in-differences, instrumental variables, regression discontinuity, or synthetic control—to construct a credible counterfactual.

## 0. Method map

| Assignment setting | Method | Core conditions |
|---|---|---|
| Treatment is as-if random | Randomised experiment; encouragement design | Random assignment; IV additionally needs exclusion and monotonicity |
| Selection is explained by observed covariates | Regression adjustment, stratification, matching, propensity scores | Conditional independence/ignorability, common support, pre-treatment covariates |
| A threshold assigns treatment | Regression discontinuity (RDD) | Continuous potential outcomes near the threshold; no precise manipulation |
| Treated and comparison groups are observed before and after | Difference-in-differences (DiD) | Parallel trends and no differential anticipation |
| An external instrument shifts treatment | Two-stage least squares (2SLS) | Relevance, independence, exclusion; monotonicity for a LATE interpretation |
| A single aggregate unit receives treatment | Synthetic control | Good pre-treatment fit, untreated donor pool, no spillovers |

## 1. Statistics and regression foundations

The linear population model is $Y_i=\beta_0+\beta_1X_i+u_i$. $Y$ is the outcome, $X$ an explanatory, treatment, or control variable, and $u$ collects factors omitted from the model. The conditional expectation function $E(Y\mid X=x)$ is the mean outcome at a given value of $X$. Linear regression represents this function linearly or provides its best linear prediction.

Simple OLS estimates are:

$$\hat\beta_1=\frac{\sum_i(X_i-\bar X)(Y_i-\bar Y)}{\sum_i(X_i-\bar X)^2},\qquad \hat\beta_0=\bar Y-\hat\beta_1\bar X$$

Fitted values $\hat Y_i$ and residuals $\hat u_i=Y_i-\hat Y_i$ are not the unobserved population errors $u_i$. With an intercept, OLS residuals sum to zero, are sample-orthogonal to regressors, and the fitted line passes through the sample means. Random sampling, variation in $X$, and zero conditional mean $E(u\mid X)=0$ support unbiasedness. Homoskedasticity is needed for the classical variance formula; use robust or clustered standard errors under heteroskedasticity or within-group dependence.

Omitted-variable bias arises when an omitted determinant of the outcome is correlated with an included regressor. A multiple-regression coefficient is a linear relationship conditional on the included regressors. Adding controls does not by itself create a causal interpretation; identification still comes from the design and valid adjustment for confounding.

## 2. Potential outcomes and causal effects

For unit $i$, define treatment $D_i\in\{0,1\}$ and potential outcomes $Y_i(1),Y_i(0)$. The individual effect is $Y_i(1)-Y_i(0)$, but only one outcome can be observed for the same unit at the same time. The average treatment effect is $ATE=E[Y(1)-Y(0)]$; the average treatment effect on the treated is $ATT=E[Y(1)-Y(0)\mid D=1]$.

The observed difference in means, $E[Y\mid D=1]-E[Y\mid D=0]$, combines a causal effect with selection bias. Random assignment makes treatment independent of potential outcomes and removes this selection bias, but does not guarantee no attrition, perfect compliance, or homogeneous effects.

SUTVA includes consistency/no-interference restrictions: one unit’s potential outcome should not depend on other units’ treatment, and treatment versions or doses must be well defined. Peer effects, market-equilibrium effects, or multiple treatment versions can invalidate the simple individual-treatment model. A potential-outcomes diagram places $Y(1)$ and $Y(0)$ side by side and marks that only one is observed; a causal graph uses arrows to show treatment, outcomes, common causes, and backdoor paths.

## 3. Randomised experiments and implementation

In a randomised trial, the treatment–control mean difference estimates the ATE. Regression adjustment for pre-treatment covariates can improve precision but is not required for randomisation to identify the effect. In the STAR class-size example, report the treatment definition, assignment mechanism, outcome, clustering or stratification, and standard-error structure.

Threats include attrition, non-compliance or switching, heterogeneous effects, supply-side changes when a programme scales up, Hawthorne/observer effects, spillovers, and limits to external validity. Assignment-based analysis estimates the intention-to-treat effect (ITT). Random encouragement can instrument for treatment receipt and, under additional assumptions, identify a local effect for compliers.

## 4. Selection on observables: regression, stratification, and matching

If treatment is conditionally independent of potential outcomes given pre-treatment covariates $X$, and treated and control units share support in $X$, conditional mean comparisons can identify treatment effects. Stratification compares units within similar $X$ groups and aggregates with weights. Regression adjusts conditional means through a model. Matching finds control units with covariates close to each treated unit.

The propensity score is $p(X)=P(D=1\mid X)$, the probability of treatment given covariates. Under ignorability, common support, and appropriately measured pre-treatment covariates, propensity-score matching or stratification can balance observed variables but cannot remove unobserved confounding. Check balance, overlap, distance, replacement rules, and effective sample size. High-dimensional matching faces the curse of dimensionality.

Causal graphs guide covariate selection: block backdoor paths from treatment to outcome. Conditioning on a collider can open a non-causal path; controlling for a post-treatment mediator may change the estimand. The before/after age-distribution graphs in the lecture should show closer covariate distributions after matching. A single mean difference is not a complete balance diagnostic.

## 5. Regression discontinuity design

Let $R_i$ be a running variable and $c$ a cutoff, with treatment assigned when $R_i\geq c$. In a sharp RDD, treatment probability jumps from zero to one; in a fuzzy RDD, the probability only jumps. If potential outcomes are continuous at the cutoff and units cannot precisely manipulate $R$, the outcome discontinuity identifies a local causal effect.

Local-linear estimation fits separate local regressions on either side of the cutoff, often with kernel weights. The fuzzy-RDD estimate is the outcome jump divided by the treatment-probability jump. Bandwidth trades bias against variance. Report bandwidth sensitivity, alternative trend specifications, covariate continuity, and density/manipulation checks. In the graph, the running variable is on the horizontal axis and the outcome or treatment rate on the vertical axis; the vertical jump at the cutoff is the estimand.

Local-randomisation methods treat a narrow window around the cutoff as approximately random and require window-selection and randomisation diagnostics. They are distinct from standard local-polynomial RDD, which relies on continuity and local extrapolation.

## 6. Panel data, difference-in-differences, and event studies

For two groups and two periods:

$$\hat\tau_{DiD}=(\bar Y_{T,after}-\bar Y_{T,before})-(\bar Y_{C,after}-\bar Y_{C,before})$$

The key assumption is parallel trends: absent treatment, treated and comparison outcomes would have changed similarly. Group fixed effects remove time-invariant group differences; time fixed effects remove common shocks. Cluster standard errors at the treatment-assignment level. With staggered timing and heterogeneous effects, a conventional two-way-fixed-effects coefficient can combine unsuitable comparisons; report group-time effects appropriate to the design.

An event-study plot shows coefficients and confidence intervals by relative time. Pre-treatment coefficients help assess pre-trends, but statistical non-significance does not prove parallel trends. Also show raw group means, sample composition, and policy timing. Robustness checks include placebo dates, alternative control groups, trend specifications, and concurrent-policy checks.

## 7. Instrumental variables and natural experiments

An instrument $Z$ supplies exogenous variation in treatment $D$. In 2SLS, the first stage regresses $D$ on $Z$ and exogenous controls; the second stage regresses $Y$ on predicted treatment. A valid instrument requires relevance ($Z$ changes $D$), independence ($Z$ is independent of potential outcomes/unobserved confounding), and exclusion (it affects $Y$ only through $D$). A local-average-treatment-effect interpretation also requires monotonicity.

Encouragement designs, lotteries, policy eligibility, and Bartik/shift-share instruments need a substantive argument for the source of instrument variation. Weak instruments cause finite-sample bias and unreliable inference; report the first stage, weak-instrument-robust inference, and institutional support for exclusion. The first-stage graph shows the association between $Z$ and $D$; a reduced-form estimate shows $Z$’s effect on $Y$. With one instrument and one endogenous treatment, 2SLS is their ratio. IV generally identifies an effect for compliers affected by the instrument, not necessarily the population ATE.

## 8. Synthetic control

Synthetic control is suited to a small number of treated aggregate units and a long pre-treatment period. Non-negative weights on untreated donor units, summing to one, create a synthetic comparison that matches the treated unit’s pre-treatment outcomes and predictors. The post-treatment gap between the actual unit and synthetic path estimates the effect.

In the plot, the solid line is the treated unit and the dashed line is the synthetic control. Better pre-treatment fit increases credibility; persistent post-treatment divergence is consistent with an effect. Report the donor pool, predictors, weights, pre-treatment fit error, placebo/permutation tests, and possible spillovers or concurrent treatments. Synthetic control cannot fix poor pre-treatment fit or a contaminated donor pool.

## 9. Research-design workflow

1. Define treatment, outcome, target population, time period, and estimand (ATE/ATT/LATE).
2. Draw a causal graph and explain treatment assignment; identify confounding, selection, spillovers, and attrition risks.
3. State identifying assumptions and their observable implications; show data, descriptive statistics, and graphical diagnostics.
4. Estimate the baseline model with an appropriate standard-error structure; conduct prespecified robustness and placebo analyses.
5. Report effect size, units, uncertainty, local scope, and limitations. Do not describe correlation as causation.

## Review checklist

- What parameter does the design identify: ATE, ATT, LATE, or a local cutoff effect?
- What comparison group, instrument, cutoff, or synthetic weighting constructs the counterfactual?
- What is the identifying assumption, and what evidence diagnoses—but cannot fully prove—it?
- Do figures show treatment timing, cutoff, uncertainty, trends, and sample range?
## Slide-by-slide courseware text

### Slide 1

Applied Econometrics:
Causal Inference and Research Design,
Professor Scott Cunningham, Spring 2017
Figure: xkcd

### Slide 2

Basic information
Professor Scott Cunningham’s contact information:
scunning@gmail.com or scott cunningham@baylor.edu
phone: (254) 537-2239
Oﬃce hours: MW: 11:00am to 12:15am in my oﬃce

### Slide 3

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Administrative
Empiricism in Microeconomics
Structure and Assessment
The fundamental theme linking all lectures will be the estimation
of causal eﬀects
Part 1 (8 lectures) covers “the core” of applied econometrics
Part 2 (10 lectures) covers “selection on observables”
methodologies
Part 3 (10 lectures) covers “selection on unobservables”
methodologies
Cunningham Causal Inference

### Slide 4

Assessment
Your grade will be based on three things:
4 problem sets (includes replications) weight: 10 percent
each.
You will need to purchase a copy of STATA (grad plan) or use
the school’s licensed version on the network as the problem
sets require programming.
1 presentation weight: 20 percent
2 exams weight: 20 percent each . Final is cumulative. No
make up exams.

### Slide 5

Required readings
Textbooks
1 Angrist and Pischke (2009) Mostly Harmless Econometrics
(MHE)
2 Morgan and Winship (2014) Counterfactuals and Causal
Inference (MW)
Readings:
We will also discuss a number of papers in each lecture, each
of which you will need to learn inside and out.
Lecture slides and reading lists are available
The exam will cover material from all required readings.

### Slide 6

Content of Part 1: The Core
Part 1 includes 2 lectures on each of the following topics:
1 Statistics and Econometrics Review
Expectation operator, mean, covariance, variance
Ordinary Least Squares (derivation, properties, intuition, etc.)
Conditional expectation function and linear regression
2 Modern theories of causality
Rubin causal model
Pearl’s directed acyclical graphical (DAG) model
3 Randomized experiments

### Slide 7

Content of Part 2: Selection on observables
Part 2 includes lectures on each of the following topics
1 Regression
2 Stratiﬁcation
3 Matching
4 Regression discontinuity design

### Slide 8

Content of Part 3: Selection on unobservables
Part 3 includes lectures on the following topics
1 Panel methods, diﬀerences-in-diﬀerences and synthetic control
2 Instrumental variables

### Slide 9

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Administrative
Empiricism in Microeconomics
Introduction: OLS Review
Derivation of the OLS estimator
Algebraic properties of OLS
Statistical Properties of OLS
Variance of OLS and standard errors
Cunningham Causal Inference

### Slide 10

Terminology
y x
Dependent Variable Independent Variable
Explained Variable Explanatory Variable
Response Variable Control Variable
Predicted Variable Predictor Variable
Regressand Regressor
LHS RHS
The terms “explained” and “explanatory” are probably best, as
they are the most descriptive and widely applicable. But
“dependent” and “independent” are used often. (The
“independence” here is not really statistical independence.)

### Slide 11

We said we must confront three issues:
1 How do we allow factors other than x to aﬀect y?
2 What is the functional relationship between y and x?
3 How can we be sure we are capturing a ceteris paribus
relationship between y and x?
We will argue that the simple regression model
y = β0 + β1x + u (1)
addresses each of them.

### Slide 12

The SLR model is a population model. When it comes to
estimating β1 (and β0) using a random sample of data, we
must restrict how u and x are related to each other.
What we must do is restrict the way u and x relate to each
other in the population.

### Slide 13

First, we make a simplifying assumption (without loss of
generality): the average, or expected, value of u is zero in the
population:
E (u) = 0 (2)
where E (·) is the expected value operator.

### Slide 14

The presence of β0 in
y = β0 + β1x + u (3)
allows us to assume E (u) = 0. If the average of u is diﬀerent from
zero, say α0, we just adjust the intercept, leaving the slope the
same:
y = (β0 + α0) + β1x + (u − α0) (4)
where α0 = E (u). The new error is u − α0 and the new intercept
is β0 + α0. The important point is that the slope, β1, has not
changed.

### Slide 15

An assumption that meshes well with our introductory treatment
involves the mean of the error term for each “slice” of the
population determined by values of x:
E (u|x) = E (u), all values x (5)
where E (u|x) means “the expected value of u given x”.
Then, we say u is mean independent of x.

### Slide 16

Suppose u is “ability” and x is years of education. We need,
for example,
E (ability|x = 8) = E (ability|x = 12) = E (ability|x = 16)
so that the average ability is the same in the diﬀerent portions
of the population with an 8 th grade education, a 12 th grade
education, and a four-year college education.
Because people choose education levels partly based on
ability, this assumption is almost certainly false.
Suppose u is “land quality” and x is fertilizer amount. Then
E (u|x) = E (u) if fertilizer amounts are chosen independently
of quality.

### Slide 17

Combining E (u|x) = E (u) (the substantive assumption) with
E (u) = 0 (a normalization) gives
E (u|x) = 0, all values x (6)
Called the zero conditional mean assumption . Because the
conditional expected value is a linear operator, E (u|x) = 0 implies
E (y |x) = β0 + β1x (7)
which shows the population regression function is a linear
function of x.

### Slide 18

The straight line in the graph on the next page is what
Wooldridge calls the population regression function , and
what Angrist and Pischke call the conditional expectation
function
E (y |x) = β0 + β1x
The conditional distribution of y at three diﬀerent values of x
are superimposed. for a given value of x, we see a range of y
values: remember, y = β0 + β1x + u, and u has a distribution
in the population.

### Slide 19

[No extractable text in PDF text layer; page may contain an image or diagram.]

### Slide 20

Deriving the Ordinary Least Squares Estimates
Given data on x and y, how can we estimate the population
parameters, β0 and β1?
Let {(xi , yi) : i = 1, 2, ..., n} be a random sample of size n
(the number of observations) from the population.
Plug any observation into the population equation:
yi = β0 + β1xi + ui (8)
where the i subscript indicates a particular observation.
We observe yi and xi, but not ui (but we know it is there).

### Slide 21

We use the two population restrictions:
E (u) = 0 (9)
Cov(x, u) = 0 (10)
to obtain estimating equations for β0 and β1. We talked about the
ﬁrst condition. The second condition means that x and u are
uncorrelated. Both conditions are implied by
E (u|x) = 0 (11)

### Slide 22

With E (u) = 0, Cov(x, u) = 0 is the same as E (xu) = 0. Next we
plug in for u:
E (y − β0 − β1x) = 0 (12)
E [x(y − β0 − β1x)] = 0 (13)
These are the two conditions in the population that eﬀectively
determine β0 and β1.

### Slide 23

So we use their sample counterparts (which is a method of
moments approach to estimation):
n−1
n∑
i=1
(yi − ˆβ0 − ˆβ1xi) = 0 (14)
n−1
n∑
i=1
xi(yi − ˆβ0 − ˆβ1xi) = 0 (15)
where ˆβ0 and ˆβ1 are the estimates from the data.
These are two linear equations in the two unknowns ˆβ0 and ˆβ1.

### Slide 24

Pass the summation operator through the ﬁrst equation:
n−1
n∑
i=1
(yi − ˆβ0 − ˆβ1xi) (16)
= n−1
n∑
i=1
yi − n−1
n∑
i=1
ˆβ0 − n−1
n∑
i=1
ˆβ1xi (17)
= n−1
n∑
i=1
yi − ˆβ0 − ˆβ1
(
n−1
n∑
i=1
xi
)
(18)
= y − ˆβ0 − ˆβ1x (19)
We use the standard notation y = n−1 ∑n
i=1 yi for the average of
the n numbers {yi : i = 1, 2, ..., n}. For emphasis, we call y a
sample average.

### Slide 25

We have shown that the ﬁrst equation,
n−1
n∑
i=1
(yi − ˆβ0 − ˆβ1xi) = 0 (20)
implies
y = ˆβ0 + ˆβ1x (21)
Now, use this equation to write the intercept in terms of the slope:
ˆβ0 = y − ˆβ1x (22)

### Slide 26

Plug this into the second equation (but where we take away the
division by n):
n∑
i=1
xi(yi − ˆβ0 − ˆβ1xi) = 0 (23)
so
n∑
i=1
xi[yi − (y − ˆβ1x) − ˆβ1xi] = 0 (24)
Simple algebra gives
n∑
i=1
xi(yi − y) = ˆβ1
[ n∑
i=1
xi(xi − x)
]
(25)

### Slide 27

So, the equation to solve is
n∑
i=1
(xi − x)(yi − y) = ˆβ1
[ n∑
i=1
(xi − x)2
]
(26)
If ∑n
i=1(xi − x)2 > 0, we can write
ˆβ1 =
∑n
i=1(xi − x)(yi − y)∑n
i=1(xi − x)2 = Sample Covariance(xi , yi)
Sample Variance(xi) (27)

### Slide 28

The previous formula for ˆβ1 is important. It shows us how to
take the data we have and compute the slope estimate.
ˆβ1 is called the ordinary least squares (OLS) slope estimate.
It can be computed whenever the sample variance of the xi is
not zero, which only rules out the case where each xi has the
same value.
The intuition is that the variation in x is what permits us to
identify its impact on y.

### Slide 29

Once we have ˆβ1, we compute ˆβ0 = y − ˆβ1x. This is the OLS
intercept estimate.
These days, we let the computer do the calculations, which
are tedious even if n is small.

### Slide 30

For any candidates ˆβ0 and ˆβ1, deﬁne a ﬁtted value for each i
as
ˆyi = ˆβ0 + ˆβ1xi (28)
We have n of these. This is the value we predict for yi given
that x = xi.

### Slide 31

The “mistake” we make is the residual:
ˆui = yi − ˆyi = yi − ˆβ0 − ˆβ1xi (29)
Suppose we measure the size of the mistake, for each i, by
squaring it. Then we add them all up:
n∑
i=1
ˆu2
i =
n∑
i=1
(yi − ˆβ0 − ˆβ1xi)2 (30)
This is called the sum of squared residuals . Finally, choose
ˆβ0 and ˆβ1 to minimize the sum of squared residuals. Using
calculus (or other arguments), it can be shown that the
solutions are the same we obtained before.

### Slide 32

Algebraic Properties of OLS Statistics
Remeber how we obtained ˆβ0 and ˆβ1. When an intercept is
included, we will have:
n∑
i=1
(yi − ˆβ0 − ˆβ1xi) = 0 (31)
The OLS residuals always add up to zero, by construction,
n∑
i=1
ˆui = 0 (32)
Because yi = ˆyi + ˆui by deﬁnition,
n−1
n∑
i=1
yi = n−1
n∑
i=1
ˆyi + n−1
n∑
i=1
ˆui (33)
and so y = ˆy.

### Slide 33

Similarly the way we obtained our estimates,
n−1
n∑
i=1
xi(yi − ˆβ0 − ˆβ1xi) = 0 (34)
The sample covariance (and therefore the sample correlation)
between the explanatory variables and the residuals is always zero:
n−1
n∑
i=1
xi ˆui = 0 (35)
Because the ˆyi are linear functions of the xi, the ﬁtted values and
residuals are uncorrelated, too:
n−1
n∑
i=1
ˆyi ˆui = 0 (36)

### Slide 34

Both properties hold by construction. ˆβ0 and ˆβ1 were chosen to
make them true.
A third property is that the point ( x, y) is always on the OLS
regression line. That is, if we plug in the average for x, we predict
the sample average for y:
y = ˆβ0 + ˆβ1x (37)
Again, we chose the estimates to make this true.

### Slide 35

Expected Value of OLS
Mathematical statistics: How do our estimators behave across
diﬀerent samples of data? On average, would we get the right
answer if we could repeatedly sample?
We need to ﬁnd the expected value of the OLS estimators –
in eﬀect, the average outcome across all possible random
samples – and determine if we are right on average.
Leads to the notion of unbiasedness, which is a “desirable”
characteristic for estimators.
E ( ˆβ) = β (38)

### Slide 36

Remember, our objective is to estimate β1, the slope
population parameter that describes the relationship between
y and x.
ˆβ1 is an estimator of that parameter obtained for a speciﬁc
sample.
Diﬀerent samples will generate diﬀerent estimates ( ˆβ1) for the
“true” β1, i.e. ( ˆβ1) is a random variable.
Unbiasedness is the idea that if we could take as many
random samples on Y as we want from the population, and
compute an estimate each time, the average of these
estimates would be equal to β1.

### Slide 37

Assumption SLR.1 (Linear in Parameters)
The population model can be written as
y = β0 + β1x + u (39)
where β0 and β1 are the (unknown) population parameters.
We view x and u as outcomes of random variables; thus, y is
random.
Stating this assumption formally shows that our goal is to
estimate β0 and β1.

### Slide 38

Assumption SLR.2 (Random Sampling)
We have a random sample of size n, {(xi , yi) : i = 1, ..., n},
following the population model.
We know how to use this data to estimate β0 and β1 by OLS.
Because each i is a draw from the population, we can write,
for each i,
yi = β0 + β1xi + ui (40)
Notice that ui here is the unobserved error for observation i.
It is not the residual that we compute from the data!

### Slide 39

Assumption SLR.3 (Sample Variation in the Explanatory
Variable)
The sample outcomes on xi are not all the same value.
This is the same as saying the sample variance of
{xi : i = 1, ..., n} is not zero.
In practice, this is no assumption at all. If the xi are all the
same value, we cannot learn how x aﬀects y in the population.

### Slide 40

Assumption SLR.4 (Zero Conditional Mean)
In the population, the error term has zero mean given any
value of the explanatory variable:
E (u|x) = E (u) = 0. (41)
This is the key assumption for showing that OLS is unbiased,
with the zero value not being important once we assume
E (u|x) does not change with x.
Note that we can compute the OLS estimates whether or not
this assumption holds, or even if there is an underlying
population model.

### Slide 41

Showing OLS is unbiased
How do we show ˆβ1 is unbiased for β1? What we need to show is
E ( ˆβ1) = β1 (42)
where the expected value means averaging across random samples.
Step 1: Write down a formula for ˆβ1. It is convenient to use
ˆβ1 =
∑n
i=1(xi − x)yi∑n
i=1(xi − x)2 (43)
which is one of several equivalent forms.

### Slide 42

It is convenient to deﬁne SSTx = ∑n
i=1(xi − x)2, to total variation
in the xi, and write
ˆβ1 =
∑n
i=1(xi − x)yi
SSTx
(44)
Remember, SSTx is just some positive number. The existence of
ˆβ1 is guaranteed by SLR.3.
Step 2: Replace each yi with yi = β0 + β1xi + ui (which uses
SLR.1 and the fact that we have data from SLR.2).

### Slide 43

The numerator becomes
n∑
i=1
(xi − x)yi =
n∑
i=1
(xi − x)(β0 + β1xi + ui) (45)
= β0
n∑
i=1
(xi − x) + β1
n∑
i=1
(xi − x)xi +
n∑
i=1
(xi − x)ui (46)
= 0 + β1
n∑
i=1
(xi − x)2 +
n∑
i=1
(xi − x)ui (47)
= β1SSTx +
n∑
i=1
(xi − x)ui (48)
We used ∑n
i=1(xi − x) = 0 and ∑n
i=1(xi − x)xi = ∑n
i=1(xi − x)2.

### Slide 44

We have shown
ˆβ1 = β1SSTx + ∑n
i=1(xi − x)ui
SSTx
= β1 +
∑n
i=1(xi − x)ui
SSTx
(49)
Note how the last piece is the slope coeﬃcient from the OLS
regression of ui on xi, i = 1, ..., n. We cannot do this regression
because the ui are not observed.

### Slide 45

Now deﬁne
wi = (xi − x)
SSTx
(50)
so we have
ˆβ1 = β1 +
n∑
i=1
wi ui (51)
ˆβ1 is a linear function of the unobserved errors, ui. The wi are
all functions of {x1, x2, ..., xn}.
The (random) diﬀerence between ˆβ1 and β1 is due to this
linear function of the unobservables.

### Slide 46

Step 3: Find E ( ˆβ1).
Under Assumptions SLR.2 and SLR.4, E (ui |x1, x2, ..., xn) = 0.
That means, conditional on {x1, x2, ..., xn},
E (wi ui |x1, x2, ..., xn) = wi E (ui |x1, x2, ..., xn) = 0
because wi is a function of {x1, x2, ..., xn}. (In the next slides I
omit the conditioning in the expectations)
This would not be true if, in the population, u and x are
correlated.

### Slide 47

Now we can complete the proof: conditional on {x1, x2, ..., xn},
E ( ˆβ1) = E
(
β1 +
n∑
i=1
wi ui
)
(52)
= β1 +
n∑
i=1
E (wi ui) = β1 +
n∑
i=1
wi E (ui) (53)
= β1 (54)
Remember, β1 is the ﬁxed constant in the population. The
estimator, ˆβ1, varies across samples and is the random outcome:
before we collect our data, we do not know what ˆβ1 will be.

### Slide 48

THEOREM (Unbiasedness of OLS)
Under Assumptions SLR.1 through SLR.4
E ( ˆβ0) = β0 and E ( ˆβ1) = β1. (55)
Omit the proof for ˆβ0.

### Slide 49

Each sample leads to a diﬀerent estimate, ˆβ0 and ˆβ1. Some
will be very close to the true values β0 = 3 and β1 = 2.
Nevertheless, some could be very far from those values.
If we repeat the experiment again and again, and average the
estimates, we would get very close to 2.
The problem is, we do not know which kind of sample we
have. We can never know whether we are close to the
population value.
We hope that our sample is ”typical” and produces a slope
estimate close to β1 but we can never know.

### Slide 50

Reminder
Errors are the vertical distances between observations and the
unknown Conditional Expectation Function. Therefore, they
are unknown.
Residuals are the vertical distances between observations and
the estimated regression function. Therefore, they are known.

### Slide 51

SE and the data
The correct SE estimation procedure is given by the underlying
structure of the data
It is very unlikely that all observations in a dataset are
unrelated, but drawn from identical distributions
(homoskedasticity)
For instance, the variance of income is often greater in
families belonging to top deciles than among poorer families
(heteroskedasticity)
Some phenomena do not aﬀect observations individually, but
they do aﬀect groups of observations uniformly within each
group (clustered data)

### Slide 52

Variance of the OLS Estimators
Under SLR.1 to SLR.4, the OLS estimators are unbiased. This
tells us that, on average, the estimates will equal the
population values.
But we need a measure of dispersion (spread) in the sampling
distribution of the estimators. We use the variance (and,
ultimately, the standard deviation).
We could characterize the variance of the OLS estimators
under SLR.1 to SLR.4 (and we will later). For now, it is
easiest to introduce an assumption that simpliﬁes the
calculations.

### Slide 53

Assumption SLR.5 (Homoskedasticity, or Constant Variance)
The error has the same variance given any value of the explanatory
variable x:
Var(u|x) = σ2 > 0 (56)
where σ2 is (virtually always) unknown.
Because we assume SLR.4, that is, E (u|x) = 0 whenever we
assume SLR.5, we can also write
E (u2|x) = σ2 = E (u2) (57)

### Slide 54

Under the population Assumptions SLR.1 ( y = β0 + β1x + u),
SRL.4 (E (u|x) = 0) and SLR.5 ( Var(u|x) = σ2),
E (y |x) = β0 + β1x (58)
Var(y |x) = σ2 (59)
So the average or expected value of y is allowed to change with x
– in fact, this is what interests us – but the variance does not
change with x. (See Graphs on next two slides)

### Slide 55

[No extractable text in PDF text layer; page may contain an image or diagram.]

### Slide 56

THEOREM (Sampling Variances of OLS)
Under Assumptions SLR.1 to SLR.2,
Var( ˆβ1|x) = σ2
∑n
i=1(xi − x)2 = σ2
SSTx
(60)
Var( ˆβ0|x) = σ2 (
n−1 ∑n
i=1 x2
i
)
SSTx
(61)
(conditional on the outcomes {x1, x2, ..., xn}).

### Slide 57

To show this, write, as before,
ˆβ1 = β1 +
n∑
i=1
wi ui (62)
where wi = (xi − x)/SSTx. We are treating this as nonrandom in
the derivation. Because β1 is a constant, it does not aﬀect
Var( ˆβ1). Now, we need to use the fact that, for uncorrelated
random variables, the variance of the sum is the sum of the
variances.

### Slide 58

The {ui : i = 1, 2, ..., n} are actually independent across i, and so
they are uncorrelated. So (remember that if we know x, we know
w)
Var( ˆβ1|x) = Var
( n∑
i=1
wi ui |x
)
(63)
=
n∑
i=1
Var(wi ui |x) =
n∑
i=1
w 2
i Var(ui |x) (64)
=
n∑
i=1
w 2
i σ2 = σ2
n∑
i=1
w 2
i (65)
where the second-to-last equality uses Assumption SLR.5, so that
the variance of ui does not depend on xi.

### Slide 59

Now we have
n∑
i=1
w 2
i =
n∑
i=1
(xi − x)2
(SSTx)2 =
∑n
i=1(xi − x)2
(SSTx)2 (66)
= SSTx
(SSTx)2 = 1
SSTx
(67)
We have shown
Var( ˆβ1) = σ2
SSTx
(68)

### Slide 60

Usually we are interested in β1. We can easily study the two
factors that aﬀect its variance.
Var( ˆβ1) = σ2
SSTx
(69)
1 As the error variance increases, i.e, as σ2 increases, so does
Var( ˆβ1). The more “noise” in the relationship between y and
x – that is, the larger variability in u – the harder it is to learn
about β1.
2 By contrast, more variation in {xi } is a good thing:
SSTx ↑ implies Var( ˆβ1) ↓ (70)

### Slide 61

Notice that SSTx /n is the sample variance in x. We can think of
this as getting close to the population variance of x, σ2
x, as n gets
large. This means
SSTx ≈ nσ2
x (71)
which means, as n grows, Var( ˆβ1) shrinks at the rate 1 /n. This is
why more data is a good thing: it shrinks the sampling variance of
our estimators.

### Slide 62

The standard deviation of ˆβ1 is the square root of the variance. So
sd( ˆβ1) = σ√SSTx
(72)
This turns out to be the measure of variation that appears in
conﬁdence intervals and test statistics.

### Slide 63

Estimating the Error Variance
In the formula
Var( ˆβ1) = σ2
SSTx
(73)
we can compute SSTx from {xi : i = 1, ..., n}. But we need to
estimate σ2.
Recall that
σ2 = E (u2). (74)

### Slide 64

Therefore, if we could observe a sample on the errors,
{ui : i = 1, 2, ..., n}, an unbiased estimator of σ2 would be the
sample average
n−1
n∑
i=1
u2
i (75)
But this not an estimator because we cannot compute it from the
data we observe, since ui are unobserved.
How about replacing each ui with its “estimate”, the OLS residual
ˆui?
ui = yi − β0 − β1xi (76)
ˆui = yi − ˆβ0 − ˆβ1xi (77)

### Slide 65

ˆui can be computed from the data because it depends on the
estimators ˆβ0 and ˆβ1. Except by ﬂuke,
ˆui ̸= ui (78)
for any i.
ˆui = yi − ˆβ0 − ˆβ1xi = (β0 + β1xi + ui) − ˆβ0 − ˆβ1xi (79)
= ui − ( ˆβ0 − β0) − ( ˆβ1 − β1)xi (80)
E ( ˆβ0) = β0 and E ( ˆβ1) = β1, but the estimators almost always
diﬀer from the population values in a sample.

### Slide 66

Now, what about this as an estimator of σ2?
n−1
n∑
i=1
ˆu2
i = SSR/n (81)
It is a true estimator and easily computed from the data after OLS.
As it turns out, this estimator is slightly biased: its expected value
is a little less than σ2.

### Slide 67

The estimator does not account for the two restrictions on the
residuals, used to obtain ˆβ0 and ˆβ1:
n∑
i=1
ˆui = 0 (82)
n∑
i=1
xi ˆui = 0 (83)
There is no such restriction on the unobserved errors.

### Slide 68

The unbiased estimator of σ2 uses a degrees-of-freedom
adjustment. The residuals have only n − 2 degrees-of-freedom, not
n.
ˆσ2 = SSR
(n − 2) (84)
THEOREM: Unbiased Estimator of σ2
Under Assumptions SLR.1 to SLR.5,
E (ˆσ2) = σ2 (85)

### Slide 69

In regression output, it is
ˆσ =
√
ˆσ2 =
√
SSR
(n − 2) (86)
that is usually reported. This is an estimator of sd(u), the
standard deviation of the population error. And SSR = ∑n
i=1 ˆu2.

### Slide 70

ˆσ is called the standard error of the regression , which
means it is an estimate of the standard deviation of the error
in the regression. Stata calls it the root mean squared error .
Given ˆσ, we can now estimate sd( ˆβ1) and sd( ˆβ0). The
estimates of these are called the standard errors of the ˆβj.

### Slide 71

We just plug ˆσ in for σ:
se( ˆβ1) = ˆσ√SSTx
(87)
where both the numerator and denominator are computed
from the data.
For reasons we will see, it is useful to report the standard
errors below the corresponding coeﬃcient, usually in
parentheses.

### Slide 72

OLS inference is generally faulty in the presence of
heteroskedasticity

### Slide 73

Fortunately, OLS is still useful
Assume SLR.1-4 hold, but not SLR.5. Therefore
Var(ui |xi) = σ2
i
The variance of our estimator, ˆβ1 equals:
Var( ˆβ1) =
∑n
i=1(xi − x)2σ2
i
SST 2x
When σ2
i = σ2 for all i, this formula reduces to the usual
form, σ2
SST 2x

### Slide 74

A valid estimator of Var( ˆβ1) for heteroskedasticity of any form
(including homoskedasticity) is
Var( ˆβ1) =
∑n
i=1(xi − x)2 ˆui
2
SST 2x
which is easily computed from the data after the OLS
regression
As a rule, you should always use the , robust command in
STATA.

### Slide 75

Clustered data
Observations are related to each other within certain groups
You want to regress kids’ grades on class size to determine the
eﬀect of class size on grades
The unobservables of kids belonging to the same classroom
will be correlated (e.g., teacher quality, recess routines) while
will not be correlated with kids in far away classrooms
This means we are assuming independence across “clusters”
but correlation within clusters.

### Slide 76

Cluster robust standard errors
Using cluster-robust standard errors changes somewhat
Instead of summing over each individual, we ﬁrst sum over
groups

### Slide 77

Clustered data
Let’s stack the observations and use matrix notation. Stack
observations by cluster
yg = xg β + ug
The OLS estimator of β is:
ˆβ = [X ′X ]−1X ′y
we just stacked the data is all
The variance is given by:
Var(β) = E [[X ′X ]−1X ′ΩX [X ′X ]−1]

### Slide 78

Clustered data
With this in mind, we can now write the variance-covariance
matrix for clustered data
Var( ˆβ) = [X ′X ]−1[
G∑
i=1
x ′
g ˆug ˆu′
g xg ][X ′X ]−1
In STATA: vce(cluster clustervar) . Where clustervar
is a variable that identiﬁes the groups in which unobservables
are allowed to correlate

### Slide 79

The importance of knowing your data
In real world you should never go with the “independent and
identically distributed” (i.e., homoskedasticity) case. Life is
not that simple.
You need to know your data in order to choose the correct
error structure and then infer the required SE calculation
If you have aggregate variables, like class size, clustering at
that level is required

### Slide 80

Outline: CEF and Linear regression
Properties of the conditional expectation function (CEF)
Reasons for using linear regression
Regression anatomy theorem
Omitted variable bias

### Slide 81

Properties of the conditional expectation function
Now we relax the assumption that x is ﬁxed across samples
and instead assume it is a random variable
Assume we are interested in the returns to schooling in a
wage regression. We can summarize the predictive power of
schooling’s eﬀect on wages with the conditional expectation
function
E (yi |xi) (88)
The CEF for a dependent variable, yi, given covariates Xi, is
the expectation, or population average, of yi with xi held
constant.

### Slide 82

E (yi |xi) gives the expected value of y for given values of x
It provides a reasonable representation of how y changes with
x
If x is random, then E (yi |xi) is a random function

### Slide 83

We write the CEF as E (yi |xi) and note that it’s explicitly a
function of xi, and because xi is a random variable, so is the
CEF
We’re often interested in CEFs that are functions of many
variables, conveniently subsumed in the vector xi, and for a
speciﬁc value of xi, we will write
E (yi |xi = x)

### Slide 84

Helpful result: Law of Iterated Expectations
One result we will use in the following theorems is called law of
iterated expectations . It’s fairly intuitive.
Deﬁnition of Law of Iterated Expectations (LIE)
The unconditional expectation of a random variable is equal to the
expectation of the conditional expectation of the random variable
conditional on some other random variable
E (Y ) = E (E [Y |X ])
.

### Slide 85

Say that the population is divided by gender. We could take
conditional expectations by gender and combine them
(properly weighted) to get the unconditional expectation
E [IQ] = E (E [IQ|Sex])
=
∑
Sexi
Pr(Sexi) · E [IQ|Sexi]
= Pr(Male) · E [IQ|Male]
+Pr(Female) · E [IQ|Female]
In other words, the average of the conditional averages is the
unconditional average.

### Slide 86

Proof.
For the continuous case:
E [E (Y |X )] =
∫
E (Y |X = u)gx(u)du
=
∫ [∫
tfy |x(t|X = u)dt
]
gx(u)du
=
∫ ∫
tfy |x(t|X = u)gx(u)dudt
=
∫
t
[∫
fy |x(t|X = u)gx(u)du
]
dt
=
∫
t [fx,y du] dt
=
∫
tgy (t)dt
= E (y)

### Slide 87

Proof.
For the discrete case,
E (E [Y |X ]) =
∑
x
E [Y |X = x]p(x)
=
∑
x
(∑
y
yp(y |x)
)
p(x)
=
∑
x
∑
y
yp(x, y)
=
∑
y
y
∑
x
p(x, y)
=
∑
y
yp(y)
= E (Y )

### Slide 88

Property 1: CEF Decomposition Property
Theorem 3.1.1: The CEF Decomposition Property
yi = E (yi |xi) + ui
where
1 ui is mean independent of xi; that is
E (ui |xi) = 0
2 ui is uncorrelated with any function of xi
In words: The theorem says that any random variable, yi, can be
decomposed into two parts: the part that can be explained by xi
and the part left over that can’t be which is by deﬁnition
unexplained by xi. Proof is in Angrist and Pischke (ch. 3)

### Slide 89

Property 2: CEF Prediction Property
Theorem 3.1.2: The CEF Prediction Property
Let m(xi) be any function of xi. The CEF solves
E (yi |xi) = arg min m(xi )E [(yi − m(xi))2].
In words: The CEF is the minimum mean squared error predictor
of yi given xi.
Proof is in Angrist and Pischke (ch. 3)

### Slide 90

Property 3: ANOVA Theorem
Theorem 3.1.3: The ANOVA (Analysis of variance) Theorem
The ﬁnal property of the CEF is the ANOVA theorem which states
V (yi) = V [E (yi |xi)] + E [V (yi |xi)]
where V (·) is the variance and V (yi |xi) is the conditional variance
of yi given xi.
Proof is in Angrist and Pischke (ch. 3).

### Slide 91

3 reasons why linear regression may be of interest
Angrist and Pischke argue that the linear regression may be of
interest to you – even if the underlying CEF is not linear! We
review some of the linear theorems now. These are merely to
justify the use of linear models to approximate the CEF.

### Slide 92

Theorem 3.1.4 - The Linear CEF Theorem
Suppose the CEF is linear. Then the population regression is it.
Comment: Trivial theorem imho because if the population CEF is
linear, then of course you should linear regression to estimate it.
Duh.
Proof in Angrist and Pischke (ch. 3). Proof uses the CEF
Decomposition Property from earlier.

### Slide 93

Theorem 3.1.5 - The Best Linear Predictor Theorem
1 The CEF, E (yi |xi), is the minimum mean squared error
(MMSE) predictor of yi given xi in the class of all functions xi
by the CEF prediction property
2 The population regression function, E (xi yi)E (xi x ′
i )−1, is the
best we can do in the class of all linear functions
Proof is in Angrist and Pischke (ch. 3).

### Slide 94

Theorem 3.1.6: The Regression CEF Theorem
The function xi β provides the minimum mean squared error
(MMSE) linear approximation to E (yi |xi), that is
β = arg min bE {(E (yi |xi) − x ′
i b)2}
Again, proof in Angrist and Pischke (ch. 3).

### Slide 95

[No extractable text in PDF text layer; page may contain an image or diagram.]

### Slide 96

Theorem 3.1.7: Regression anatomy theorem
Regression anatomy theorem is maybe more intuitive with an
example and some data visualization. It concerns multiple
linear regression.
Can we estimate the causal eﬀect of family size on labor
supply by regressing labor supply ( workforpay) on family size
(numkids)?
workforpayi = β0 + β1numkidsi + ui
. regress workforpay numkids
where the ﬁrst line is the causal / econometric model, and the
second line is the regression command in STATA

### Slide 97

If family size is random, then number of kids is uncorrelated
with the unobserved error term, which means we can interpret
ˆβ1 as the causal eﬀect.
Example: if Melissa has no children in reality (i.e.,
numkids= 0) and we wanted to know what the eﬀect on labor
supply will be if we surgically manipulated her family size (i.e.,
numkids = 1) then ˆβ1 would be our answer
Visual: Even better, we could just plot the regression
coeﬃcient in a scatter plot showing all i (workforpay,
numkids) pairs and the slope coeﬃcient would be the best ﬁt
of the data through these points, as well as tell us the average
causal eﬀect of family size on labor supply
But how do we interpret ˆβ1 if numkids is non-random?

### Slide 98

Assume that family size is random once we condition on race,
age, marital status and employment. Then the model is:
Workforpayi = β0 + β1Numkidsi + γ1Whitei + γ2Marriedi
+γ3Agei + γ4Employedi + ui
If we want to estimate average causal eﬀect of family size on
labor supply, we will need two things:
1 a data set with all 6 variables;
2 numkids must be randomly assigned conditional on the other
4 variables
Now how do we interpret ˆβ1? And can we visualize ˆβ1 when
there’s multiple dimensions to the data? Yes, using the
regression anatomy theorem, we can.

### Slide 99

Theorem 3.1.7: Regression Anatomy Theorem
Assume your main multiple regression model of interest:
yi = β0 + β1x1i + · · · + βk xki + · · · + βK xKi + ei
and an auxiliary regression in which the variable x1i is regressed on
all the remaining independent variables
x1i = γ0 + γk−1xk−1i + γk+1xk+1i + · · · + γK xKi + fi
and ˜x1i = x1i − ˆx1i being the residual from the auxiliary regression.
The parameter β1 can be rewritten as:
β1 = Cov(yi , ˜x1i)
Var(˜x1i)
In words: The regression anatomy theorem is about interpretation.
It says that ˆβ1 is simply a scaled covariance with the ˜ x1 residual
used instead of the actual data x.

### Slide 100

I think a more detailed proof could be helpful, so I’m leaving it in the slides for now.
Regression Anatomy Proof
To prove the theorem, noteE[˜xki] =E[xki] −E[ˆxki] =E[fi], and plugyi and
residual ˜xki fromxki auxiliary regression into the covariancecov(yi, ˜xki)
βk = cov(yi, ˜xki)
var(˜xki)
= cov(β0 +β1x1i +· · ·+βkxki +· · ·+βKxKi +ei, ˜xki)
var(˜xki)
= cov(β0 +β1x1i +· · ·+βkxki +· · ·+βKxKi +ei,fi)
var(fi)
1 Since by constructionE[fi] = 0, it follows that the termβ0E[fi] = 0.
2 Sincefi is a linear combination of all the independent variables with the
exception ofxki, it must be that
β1E[fix1i] =· · ·=βk−1E[fixk−1i] =βk+1E[fixk+1i] =· · ·=βKE[fixKI] = 0

### Slide 101

Regression Anatomy Proof (cont.)
3 Consider now the termE[eifi]. This can be written as:
E[eifi] = E[eifi]
= E[ei˜xki]
= E[ei(xki −ˆxki)]
= E[eixki] −E[ei˜xki]
Sinceei is uncorrelated with any independent variable, it is also uncorrelated
withxki: accordingly, we haveE[eixki] = 0. With regard to the second term of
the subtraction, substituting the predicted value from thexki auxiliary
regression, we get
E[ei˜xki] =E[ei(ˆγ0 + ˆγ1x1i +· · ·+ˆγk−1xk−1i +ˆγk+1xk+1i +· · ·+ˆγKxKi)]
Once again, sinceei is uncorrelated with any independent variable, the
expected value of the terms is equal to zero. Then, it followsE[eifi] = 0.

### Slide 102

Regression Anatomy Proof (cont.)
4 The only remaining term isE[βkxkifi] which equalsE[βkxki˜xki] sincefi = ˜xki. The
termxki can be substituted using a rewriting of the auxiliary regression model,xki,
such that
xki =E[xki|X−k] + ˜xki
This gives
E[βkxki˜xki] = E[βkE[˜xki(E[xki|X−k] + ˜xki)]]
= βkE[˜xki(E[xki|X−k] + ˜xki)]
= βk{E[˜x2ki] +E[(E[xki|X−k]˜xki)]}
= βkvar(˜xki)
which follows directly from the orthogonoality betweenE[xki|X−k] and ˜xki. From
previous derivations we ﬁnally get
cov(yi,˜xki) =βkvar(˜xki)
which completes the proof.

### Slide 103

STATA command:reganat(i.e., regression anatomy)
. ssc install reganat, replace
. sysuse auto
. regress price length weight headroom mpg
. reganat price length weight headroom mpg, dis(weight length) biline

### Slide 104

[No extractable text in PDF text layer; page may contain an image or diagram.]

### Slide 105

Big picture
1 Regression provides the best linear predictor for the dependent
variable in the same way that the CEF is the best unrestricted
predictor of the dependent variable
2 If we prefer to think of approximating E (yi |xi) as opposed to
predicting yi, the regression CEF theorem tells us that even if
the CEF is nonlinear, regression provides the best linear
approximation to it.
3 Regression anatomy theorem helps us interpret a single slope
coeﬃcient in a multiple regression model by the
aforementioned decomposition.

### Slide 106

Omitted Variable Bias
A typical problem is when a key variable is omitted. Assume
schooling causes earnings to rise:
Yi = β0 + β1Si + β2Ai + ui
Yi = log of earnings
Si = schooling measured in years
Ai = individual ability
Typically the econometrician cannot observeAi; for instance, the
Current Population Survey doesn’t present adult respondents’ family
background, intelligence, or motivation.

### Slide 107

What are the consequences of leaving ability out of the
regression? Suppose you estimated this short regression
instead:
Yi = β0 + β1Si + ηi
where ηi = β2Ai + ui; β0, β1, and β2 are population
regression coeﬃcients; Si is correlated with ηi through Ai
only; and ui is a regression residual uncorrelated with all
regressors by deﬁnition.

### Slide 108

Derivation of Ability Bias
Suppressing thei subscripts, the OLS estimator forβ1 is:
ˆβ1 = Cov(Y,S)
Var(S) = E[YS] −E[Y]E[S]
Var(S)
Plugging in the true model forY, we get:
ˆβ1 = Cov[(β0 +β1S +β2A+u),S]
Var(S)
= E[(β0S +β1S2 +β2SA+uS)]−E(S)E[β0 +β1S +β2A+u]
Var(S)
= β1E(S2) −β1E(S)2 +β2E(AS) −β2E(S)E(A) +E(uS) −E(S)E(u)
Var(S)
= β1 +β2
Cov(A,S)
Var(S)
If β2 >0 and Cov(A,S)>0 the coeﬃcient on schooling in the shortened
regression (without controlling forA) would be upward biased

### Slide 109

Summary
When Cov(A, S) > 0 then ability and schooling are correlated.
When ability is unobserved, then not even multiple regression
will identify the causal eﬀect of schooling on wages.
Here we see one of the main justiﬁcations for this class – what
will we do when the treatment variable is endogenous?
Because endogeneity means the causal eﬀect has not been
identiﬁed.

### Slide 110

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Regression fundamentals
Randomized Experiments
Directed Acyclical Graphs
Introduction to Selection and Randomization
Potential outcomes concepts and deﬁnitions
Random assignment and selection bias
Krueger (1999)
Cunningham Causal Inference

### Slide 111

Introduction to the Selection Problem
A lot of the language we use in the class is from the medical
literature (e.g., “treatment”, “control”)
Simple example introducing potential outcomes notation and
the selection problem: “Do hospitals make people healthier?”
National Health Interview Survey (NHIS) 2005
Health status measured from 1 (poor health) to 5 (excellent
health)
Group Sample Size Mean Health Status Std. Error
Hospital 7,774 3.21 0.014
No hospital 90,049 3.93 0.003

### Slide 112

Introductory Notation
Think of hospitalization as a “treatment” and health status as
an “outcome”.
Let the treatment (hospitalization) be a binary variable:
Di =
{
1 if hospitalized
0 if not hospitalized
where i indexes an individual observation, such as a person
Observed outcomes (health status) is Yi
Causal question: D → Y , “Does hospitalization cause health
to improve?”
Note: we did not ask “is hospitalization correlated with
health?” 1
n
Cov(D,Y )√VarD
√VarY
What’s the diﬀerence between the two questions? What does
cause mean you think? What does correlation mean?

### Slide 113

Introduction to Rubin Causal Model
Observed variables:
Treatment, Di , is observed as either 0 or 1 for each i unit.
Actual outcomes, Y , are observed for each unit i
Unobserved counterfactual variables:
Each individual i has one counterfactual (potential) outcome
that in principle exists but is not observed :
Potential outcome =
{
Y 1
i if Di = 1
Y 0
i if Di = 0
Y 1
i is the health of individual i (e.g. Jill) in the hospital
Y 0
i is the health of the same individual i (e.g. also Jill) not in
the hospital

### Slide 114

Important deﬁnitions
Deﬁnition 1: Individual treatment
eﬀect
The individual treatment eﬀect, δi ,
equals Y 1
i − Y 0
i
Deﬁnition 3: Switching equation
An individual’s observed health
outcomes, Y , is determined by
treatment assignment, Di , and
corresponding potential outcomes:
Yi = Di Y 1
i + (1 − Di )Y 0
i
Yi =
{
Y 1
i if Di = 1
Y 0
i if Di = 0
Deﬁnition 2: Average treatment eﬀect
(ATE)
The average treatment eﬀect is the
population average of all i individual
treatment eﬀects
E[δi ] = E[Y 1
i − Y 0
i ]
= E[Y 1
i ] − E[Y 0
i ]
Deﬁnition 4: Fundamental problem of
causal inference
It is impossible to observe both Y 1
i
and Y 0
i for the same individual and so
individual causal eﬀects, δi , are
unknowable.

### Slide 115

Counterfactuals
Consider the following two situations:
1 Jack is in the hospital ( DJack = 1) and his health is a 2
(YJack = 2).
2 Jill is not in the hospital ( DJill = 0) and her health is a 4
(YJill = 4)
According to deﬁnition 3 (switching equation):
1 Jack’s observed health outcome, YJack = 2 equals his potential
health outcome under treatment, Y 1
Jack = 2
2 Jill’s observed health outcome, YJill = 4 equals her potential
health outcome under control, Y 0
Jill = 4

### Slide 116

According to deﬁnitions 1, 2 and 4:
1 We do not know Jack’s potential health outcome under control
(i.e., Y 0
Jack) so therefore we do not know his treatment eﬀect
since δJack = Y 1
Jack − Y 0
Jack
2 We do not know Jill’s potential health outcome under
treatment (i.e., Y 1
Jill) so therefore we do not know her
treatment eﬀect since δJill = Y 1
Jill − Y 0
Jill
3 Therefore we do not know the average treatment eﬀect (i.e.,
E [δ]) because we are missing Y 1
Jill and Y 0
Jack – the missing
counterfactuals (deﬁnition 4) – which are needed to calculate
ATE
We cannot calculate the average treatment eﬀect because we
are missing individual treatment eﬀects, and we are missing
individual treatment eﬀects because we are missing
counterfactuals for each unit i

### Slide 117

Conditional Average Treatment Eﬀects
Deﬁnition 5: Average Treatment Eﬀect on the Treated (ATT)
The average treatment eﬀect on the treatment group is equal to
the average treatment eﬀect conditional on being a treatment
group member:
E [δ|D = 1] = E [Y 1 − Y 0|D = 1]
= E [Y 1|D = 1] − E [Y 0|D = 1]
Deﬁnition 6: Average Treatment Eﬀect on the Untreated (ATU)
The average treatment eﬀect on the untreated group is equal to
the average treatment eﬀect conditional on being untreated:
E [δ|D = 0] = E [Y 1 − Y 0|D = 0]
= E [Y 1|D = 0] − E [Y 0|D = 0]

### Slide 118

Simple diﬀerence in means
The fundamental problem of causal inference states that since
we cannot observe both Y 1
Jack and Y 0
Jack (or any i for that
matter) at the same moment in time, then we cannot
calculate the causal eﬀect of hospitalization on health
outcomes.
But we have to make due with observable health outcomes,
Y , since that’s all we got
If we subtract Jill’s observed health outcome ( YJill = 4) from
Jack’s observed health outcome, ( YJack = 2), we get:
(YJack |DJack = 1) − (YJill |DJill = 0) = 2 − 4 = −2
which implies that hospitalization caused health to go from
good to poor which is probably ridiculous.

### Slide 119

Do hospitalizations make people sick? Or do sick people go to
hospitals? This is the “selection problem” – without the full
potential outcomes, the simple diﬀerence between treatment
and control group won’t tell us the causal eﬀect of
hospitalization on health outcomes.
Same problem carries over to the simple diﬀerence in means,
E [Yi |Di = 1] − E [Yi |Di = 0], too
So what are we actually measuring if we compare average
health status for the hospitalized with that of the
non-hospitalized?

### Slide 120

Deﬁnition 7: Simple diﬀerence in mean outcomes (SDO)
A simple diﬀerence in mean outcomes (SDO) is the diﬀerence
between the population average outcome for the treatment and
control groups, and can be approximated by the sample averages:
E [Y 1|D = 1] − E [Y 0|D = 0] = EN[yi |di = 1] − EN[yi |di = 0]
in large samples. It is called the naive average treatment eﬀect in
Morgan and Winship.
Notice that . . . :
All individuals in the population contribute twice to ATE,
whereas a sampled individual is used only once to estimate
SDO by contributing to either EN[yi |di = 1] or EN[yi |di = 0].

### Slide 121

Statistical models, such as SDO, are valuable insofar as they
can provide unbiased and/or consistent estimates of the
parameter of interest (i.e., ATE). But notice the subtle
diﬀerence between the LHS and RHS:
SDO <
> ATE
E [y |d = 1] − E [y |d = 0] <
> E [Y 1] − E [Y 0]
The LHS term is the estimator of the RHS term which is a
parameter, and estimators can be biased.

### Slide 122

Biased simple diﬀerence in mean outcomes
Decomposition of the simple diﬀerence in mean outcomes (SDO)
The simple diﬀerence in mean outcomes can be decomposed into three parts
(ignoring sample average notation):
E[Y1|D = 1]−E[Y0|D = 0] = ATE
+E[Y0|D = 1]−E[Y0|D = 0]
+(1−π)(ATT−ATU) (89)
How do we interpret this?
First line is the parameter of interest: the average treatment eﬀect,E[Y1 −Y0]
Second line is one of the most important sources of bias: the “selection bias”
which is the diﬀerence in potential health outcome for the treatment and control
group had neither received any hospitalization. In other words, how did Jack and
Jill’s health status compare to one another had Jacknotgone to the hospital?
Was he sicker than her already?
Third line notes that part of the bias is due to heterogeneous treatment eﬀects
weighted by the share of the population in the control group
Proof from Morgan and Winship, p. 26 on following slides

### Slide 123

Decomposition of SDO
ATE is equal to sum of conditional average expectations by LIE
ATE = E[Y1] −E[Y0]
= {πE[Y1|D = 1] + (1−π)E[Y1|D = 0]}
−{πE[Y0|D = 1] + (1−π)E[Y0|D = 0]}
Use simpliﬁed notations
E[Y1|D = 1] = a
E[Y1|D = 0] = b
E[Y0|D = 1] = c
E[Y0|D = 0] = d
ATE = e
Rewrite ATE
e = {πa+ (1−π)b} − {πc + (1−π)d}

### Slide 124

Move SDO terms to LHS
e = {πa+ (1−π)b} − {πc + (1−π)d}
e = πa+b−πb−πc −d +πd
e = πa+b−πb−πc −d +πd + (a−a) + (c−c) + (d−d)
= e −πa−b+πb+πc +d −πd −a+a−c+c−d+d
a−d = e −πa−b+πb+πc +d −πd +a−c+c−d
a−d = e + (c−d) +a−πa−b+πb−c+πc +d −πd
a−d = e + (c−d) + (1−π)a−(1−π)b+ (1−π)d −(1−π)c
a−d = e + (c−d) + (1−π)(a−c) −(1−π)(b−d)
Substitute conditional means
E[Y1|D = 1]−E[Y0|D = 0] = ATE
+(E[Y0|D = 1]−E[Y0|D = 0])
+(1−π)({E[Y1|D = 1]−E[Y0|D = 1]}
−(1−π){E[Y1|D = 0]−E[Y0|D = 0]})
E[Y1|D = 1]−E[Y0|D = 0] = ATE
+(E[Y0|D = 1]−E[Y0|D = 0])
+(1−π)(ATT−ATU)

### Slide 125

Decomposition of diﬀerence in means
EN[yi |di = 1] − EN[yi |di = 0]  
SDO
= E [Y 1] − E [Y 0]  
Average Treatment Eﬀect
+ E [Y 0|D = 1] − E [Y 0|D = 0]  
Selection bias
+ (1 − π)(ATT − ATU)  
Heterogenous treatment eﬀect bias
where EN[yi |di = 1] → E [Y 1|D = 1],
EN[yi |di = 0] → E [Y 0|D = 0] and (1 − π) is the share of the
population in the control group.

### Slide 126

Independence assumption
Independence assumption
Treatment is independent of potential outcomes
(Y 0, Y 1) ⊥ ⊥D
In words: Random assignment means that the treatment has been
assigned to units independent of their potential outcomes. Thus,
mean potential outcomes for the treatment group and control
group are the same for a given state of the world
E [Y 1|D = 1] = E [Y 1|D = 0]
E [Y 0|D = 1] = E [Y 0|D = 0]

### Slide 127

Random Assignment Solves the Selection Problem
EN[yi |di = 1] − EN[yi |di = 0]  
SDO
= E [Y 1] − E [Y 0]  
Average Treatment Eﬀect
+ E [Y 0|D = 1] − E [Y 0|D = 0]  
Selection bias
+ (1 − π)(ATT − ATU)  
Heterogenous treatment eﬀect bias
Notice that the selection bias from the second line of the
decomposition of SDO was:
E [Y 0|D = 1] − E [Y 0|D = 0]
If treatment is independent of potential outcomes, then swap
out equations and selection bias zeroes out:
E [Y 0|D = 1] − E [Y 0|D = 0] = E [Y 0|D = 0] − E [Y 0|D = 0]
= 0

### Slide 128

Random Assignment Solves the Heterogenous Treatment Eﬀects
How does randomization aﬀect heterogeneity treatment eﬀects bias from the
third line? Rewrite deﬁnitions for ATT and ATU:
ATT =E[Y1|D = 1]−E[Y0|D = 1]
ATU =E[Y1|D = 0]−E[Y0|D = 0]
Rewrite the third row bias after 1−π:
ATT−ATU = E[Y1 | D=1]−E[Y0|D = 1]
−E[Y1 | D=0]+E[Y0|D = 0]
= 0
If treatment is independent of potential outcomes, then:
EN[yi|di = 1]−EN[yi|di = 0] = E[Y1] −E[Y0]
SDO = ATE

### Slide 129

What independence does not mean
Notice – independent treatment assignment means that
E [Y 1|D = 1] = E [Y 1|D = 0]
and the equivalent for Y 0.
But that does not in any way imply that there is no causal
eﬀect. Independence does not imply, in other words, that
E [Y 1|D = 1] is equal to E [Y 0|D = 0].
Independence only implies that the the average values for a
given potential outcome (i.e., Y 1 or Y 0) are the same for the
groups who did receive the treatment as those who did not

### Slide 130

SUTVA
The potential outcomes model presupposes a set of bundled
assumptions called the “stable unit-treatment value
assumption”
1 S: stable
2 U: across all units, or the population
3 TV: treatment-value (“treatment eﬀect”, “causal eﬀect”)
4 A: assumption
SUTVA means that average treatment eﬀects are parameters
that assume (1) homogenous dosage and (2) potential
outcomes are invariant to who else (and how many) is treated.

### Slide 131

SUTVA: Homogenous dose
SUTVA constrains what the treatment can be.
Individuals are receiving the same treatment – i.e., the “dose”
of the treatment to each member of the treatment group is
the same. That’s the “stable unit” part.
If we are estimating the eﬀect of hospitalization on health
status, we assume everyone is getting the same dose of the
hospitalization treatment.
Easy to imagine violations if hospital quality varies, though,
across individuals. But, that just means we have to be careful
what we are and are not deﬁning as the treatment

### Slide 132

SUTVA: No externalities
What if hospitalizing Jack (hospitalized, D = 1) is actually
about vaccinating Jack from small pox?
If Jack is vaccinated for small pox, then Jill’s potential health
status (without vaccination) may be higher than when he isn’t
vaccinated.
In other words, Y 0
Jill, may vary with what Jack does regardless
of whether she herself receives treatment .
SUTVA means that you don’t have a problem like this.
If there are no externalities from treatment, then δi is stable
for each i unit regardless of whether someone else receives the
treatment too.

### Slide 133

SUTVA: Partial equilibrium only
Easier to imagine this with a diﬀerent example.
Let’s say we are estimating the eﬀect of some technological
innovation that lowers the cost functions to ﬁrms in
competitive markets.
A decrease in cost raises proﬁts in the short-run, but positive
proﬁts leads to ﬁrm entry in the long-run.
Firm entry in the long-run causes the supply curve to shift
right, pushing market prices down until price equals average
total cost.
The ﬁrst eﬀect – short-run responses to decreases in cost –
are the only things we can estimate with potential outcomes.

### Slide 134

Example 1: Krueger (1999)
Krueger (1999) econometrically re-analyzes a randomized
experiment to determine the causal eﬀect of class size on
student achievement
The project is the Tennessee Student/Teacher Achievement
Ratio (STAR) experiment from the 1980s
11,600 students and their teachers were randomly assigned to
one of the following three groups:
1 Small class of 13-17 students
2 Regular class of 22-25 students
3 Regular class of 22-25 students with a full-time teacher’s aide
After the assignment, the design called for students to remain
in the same class type for four years
Randomization occurred within schools

### Slide 135

Regression analysis of experiments
With randomization one could simply calculate SDO (simple diﬀerence in mean
outcomes) for the treatment and control group and know that SDO=ATE
because of independence
Nonetheless, it is often useful to analyze experimental data with regression
analysis (see MW section 3.2.2; MHE ch. 2)
Assume that treatment eﬀects are constant – i.e.,Y1i −Y0i =δ ∀i
Substitute into a rearranged switching equation (Deﬁnition 2):
Yi = DiY1i + (1−Di)Y0i
Yi = Y0i + (Y1i −Y0i )Di
Yi = Y0i +δDi
Yi = E[Y0i ] +δDi +Y0i −E[Y0i ]
Yi = α+δDi +ηi
whereηi is the random part ofY0i
This is a regression equation that could be used to estimate the causal eﬀect of
D onY

### Slide 136

Regression analysis of experiments (cont.)
The conditional expectation, E [Yi |Di], with treatment status
switched on and oﬀ gives:
E [Yi |Di = 1] = α + δ + E [ηi |Di = 1]
E [Yi |Di = 0] = α + E [ηi |Di = 0]
Subtracting the latter from the former, we get:
E [Yi |Di = 1] − E [Yi |Di = 0]  
SDO
= δ
Treatment Eﬀect
+ E [ηi |Di = 1] − E [ηi |Di = 0]  
Selection bias

### Slide 137

We can estimate SDO using least squares but there’s other
options as well
In the STAR experiment, Di, equalled one if the student was
enrolled in a small class and had been randomly assigned
Recall that randomization implies that treatment is
independent of potential outcomes, and therefore the selection
bias vanishes

### Slide 138

Why Include Control Variables?
To evaluate experimental data, one may want to add
additional controls in the multivariate regression model. So,
instead of estimating the prior equation, we might estimate:
Yi = α + δDi + X ′
i γ + ηi
There are 2 main reasons for including additional controls in
the regression models:
1 Conditional random assignment. Sometimes randomization is
done conditional on some observable. (here that’s the school).
We’ll discuss “conditional independence assumption” when we
cover matching.
2 Additional controls increase precision. Although control
variables Xi are uncorrelated with Di , they may have
substantial explanatory power for Yi . Including controls thus
reduces variance in the residuals which lowers the standard
errors of the regression estimates.

### Slide 139

Regression in Krueger (1999)
Krueger estimates the following econometric model
Yics = β0 + β1SMALLcs + β2REG/Acs + αs + εics
i indexes a student, c indexes a class, and s indexes a school
Yics is the student’s percentile score
SMALLcs is a dummy equalling 1 if she is assigned to a small
class.
REG /Acs is a dummy equalling 1 if she was assigned to a
regular class with an aide
α is a “school ﬁxed eﬀect” which is a vector of school-speciﬁc
dummy variables. He conditions on school ﬁxed eﬀects
because randomized classroom assignment occurred within
schools.

### Slide 140

Regression results Kindergarten
whethertheteacherhasamaster’sdegree—doesnothaveasystematiceffect.Hardlyanyoftheteachersaremale,sothegenderresultsarenotverymeaningful.Teacherexperiencehasasmall,positiveeffect.Experimentationwithaquadraticinexperi-enceindicatedthattheexperienceproéletendstopeakatabout
TABLEVOLSANDREDUCED-FORMESTIMATESOFEFFECTOFCLASS-SIZEASSIGNMENTONAVERAGEPERCENTILEOFSTANFORDACHIEVEMENTTEST
ExplanatoryvariableOLS:actualclasssizeReducedform:initialclasssize(1)(2)(3)(4)(5)(6)(7)(8)A.KindergartenSmallclass4.825.375.365.374.825.375.365.37(2.19)(1.26)(1.21)(1.19)(2.19)(1.25)(1.21)(1.19)Regular/aideclass.12.29.53.31.12.29.53.31(2.23)(1.13)(1.09)(1.07)(2.23)(1.13)(1.09)(1.07)White/Asian(15——8.358.44——8.358.44yes (1.35)(1.36) (1.35)(1.36)Girl(15yes)——4.484.39——4.484.39(.63)(.63)(.63)(.63)Freelunch(15——213.15213.07——213.15213.07yes) (.77)(.77)(.77)(.77)Whiteteacher———2.57———2.57(2.10)(2.10)Teacherexperience———.26———.26(.10) (.10)Master’sdegree———2.51———2.51(1.06)(1.06)SchooléxedeffectsNoYesYesYesNoYesYesYesR2 .01.25.31.31.01.25.31.31B.FirstgradeSmallclass8.578.437.917.407.547.176.796.37(1.97)(1.21)(1.17)(1.18)(1.76)(1.14)(1.10)(1.11)Regular/aideclass3.442.222.231.781.921.691.641.48(2.05)(1.00)(0.98)(0.98)(1.12)(0.80)(0.76)(0.76)White/Asian(15——6.976.97——6.866.85yes) (1.18)(1.19) (1.18)(1.18)Girl(15yes)——3.803.85——3.763.82(.56)(.56)(.56)(.56)Freelunch(15——213.49213.61——213.65213.77yes) (.87)(.87)(.88)(.87)Whiteteacher———24.28———24.40(1.96)(1.97)Maleteacher———11.82———13.06(3.33)(3.38)Teacherexperience———.05———.06(0.06)(.06)Master’sdegree———.48———.63(1.07)(1.09)SchooléxedeffectsNoYesYesYesNoYesYesYesR2 .02.24.30.30.01.23.29.30
QUARTERLYJOURNALOFECONOMICS512

### Slide 141

Regression results 1st grade

### Slide 142

Problem 1: Attrition
A common problem in randomized experiments on humans is
attrition – i.e., people leaving the experiment
If attrition is random, then attrition aﬀects the treatment and
control groups in the same way and our estimates remain
unbiased
But in this application, attrition is probably non-random:
especially good students from large classes may have enrolled
in private schools creating a selection bias problem
Krueger addresses this concern by imputing the test scores
(from their earlier test scores) for all children who leave the
sample and then re-estimates the model including students
with imputed test scores.

### Slide 143

Problem 1: Attrition
hadhighertestscores,onaverage,thanstudentsassignedtosmallclasseswhoalsoleftthesample,thenthesmallclasseffectswillbebiasedupwards.Onereasonwhythispatternofattritionmightoccuristhathigh-incomeparentsofchildreninlargerclassesmighthavebeenmorelikelytosubsequentlyenrolltheirchildreninprivateschoolsovertimethansimilarparentsofchildreninsmallclasses.Atheart,adjustingforpossiblenonran-domattritionisamatterofimputingtestscoresforstudentswhoexitedthesample.Withlongitudinaldata,thiscanbedonecrudelybyassigningthestudent’smostrecenttestpercentiletothatstudentinyearswhenthestudentwasabsentfromthesample.15ThesampleusedintheérstpanelofTableVIincludesthelargestnumberofstudentswithnonmissingdataavailableeachgrade.Theseresultscorrespondtothemodelestimatedincolumn7ofTableV,exceptthefreelunchvariableisomittedbecauseit15.Inthecaseofastudentwholeftthesamplebutlaterreturned,theaveragetestscoreintheyearssurroundingthestudent’sabsencewasused.Testscoreswerealsoimputedforstudentswhohadamissingtestscorebutdidnotexitthesample(e.g.,becausetheywereabsentwhenthetestwasconducted).Thistechniqueiscloselyrelatedtothe‘‘last-observation-carry-forward’’methodthathasbeenusedinclinicalstudies.
TABLEVIEXPLORATIONOFEFFECTOFATTRITIONDEPENDENTVARIABLE:AVERAGEPERCENTILESCOREONSAT
Grade
Actualtestdata ActualandimputedtestdataCoefficientonsmallclassdum.SamplesizeCoefficientonsmallclassdum.SamplesizeK5.3259005.325900(.76) (.76)1 6.9566326.308328(.74) (.68)2 5.5962825.649773(.76) (.65)3 5.5863395.4910919(.79) (.63)Estimatesofreduced-formmodelsarepresented.Eachregressionincludesthefollowingexplanatoryvariables:adummyvariableindicatinginitialassignmenttoasmallclass;adummyvariableindicatinginitialassignmenttoaregular/aideclass,unrestrictedschooleffects;adummyvariableforstudentgender;andadummyvariableforstudentrace.Thereportedcoefficientonsmallclassdummyisrelativetoregularclasses.Standarderrorsareinparentheses.
EXPERIMENTALESTIMATES 515
Non-random attrition hardly biases the results.

### Slide 144

Problem 2: Switch Classrooms after Random Assignment
“It is virtually impossible to prevent some students from switching
between class types over time.” (Krueger 1999, p. 506)
Toaddressthispotentialproblem,andthevariabilityofclasssizeforagiventypeofassignment,insomeoftheanalysisthatfollowsinitialrandomassignmentisusedasaninstrumentalvariableforactualclasssize.B.DataandStandardizedTestsStudentsweretestedattheendofMarchorbeginningofAprilofeachyear.ThetestsconsistedoftheStanfordAchieve-mentTest(SAT),whichmeasuredachievementinreading,wordrecognition,andmathingradesK–3,andtheTennesseeBasicSkillsFirst(BSF)test,whichmeasuredachievementinreadingandmathingrades1–3.Thetestsweretailoredtoeachgradelevel.Becausetherearenonaturalunitsforthetestresults,Iscaledthetestscoresintopercentileranks.Speciécally,ineachgradeleveltheregularandregular/aidestudentswerepooled
TABLEIVTRANSITIONSBETWEENCLASS-SIZEINADJACENTGRADESNUMBEROFSTUDENTSINEACHTYPEOFCLASSA.Kindergartentoérstgrade FirstgradeKindergartenSmallRegularReg/aideAllSmall129260481400Regular1267376631526Aide 1227617061589All 1540155814174515B.Firstgradetosecondgrade SecondgradeFirstgradeSmallRegularReg/aideAllSmall143523241482Regular15214982021852Aide 4011515601715All 1627163617865049C.Secondgradetothirdgrade ThirdgradeSecondgrade SmallRegularReg/aideAllSmall156437351636Regular16714851521804Aide 407618571973All 1771159820445413
EXPERIMENTALESTIMATES 507
Interpreting Krueger’s“transition matrix” (above)
If students remained in their same class type over time, all the
oﬀ-diagonal elements would be zero
Interpretation: Of the 1,482 ﬁrst graders assigned to small
classrooms, 1,435 remained in small classes; 23 and 24
switched into regular and regular with aide classes in the
second grade
If students with stronger expected academic potential were
more likely to move into the small classes, then these
transitions would bias a simple comparison of outcomes across
class types.

### Slide 145

Problem 2: Switch Classrooms after Random Assignment
Subjects moved between treatment and control groups. How
to address this?
A common solution to this problem is to use initial classroom
assignment (i.e., small or regular classes) as an instrument for
actual assignment. We will discuss instrumental variables
later, so I will hold oﬀ on that now.
Krueger reports regression results where instead of the
student’s actual status as the treatment variable, he regresses
performance against their randomly assigned class size . This
is called the “reduced form” model, and we learn more about
this when we cover IV.
In Kindergarten, OLS and reduced form are the same because
students remained in their initial class for at least one year.
From grade 1 onwards, OLS and reduced form results diﬀer.

### Slide 146

Problem 2: Switch Classrooms after Random Assignment

### Slide 147

Problem 3: Heterogeneous Treatment Eﬀects
Heterogeneous treatment eﬀect bias occurs if treatment
eﬀects diﬀer across the population (i.e., ATT ̸= ATU)
If people selecting to take part of the randomized trial have
diﬀerent returns compared to the population average, then
the experiment will only identify a localized average treatment
eﬀect for the sub-population participants in the experiment.

### Slide 148

Problem 4 and 5
Supply Side Changes
If programs are scaled up, the supply side implementing the
treatment may be diﬀerent
In the trial phase, the supply side may be more motivated than
during the large scale roll-out of a program
Attrition bias
Attrition rates (i.e., leaving the sample between the baseline
and the follow-up surveys) may be diﬀerent in treatment and
control groups
The estimated treatment eﬀect may therefore be biased

### Slide 149

Problem 6 and 7
Hawthorne eﬀects
People behave diﬀerently if they are being observed in an
experiment. Similar to “placebo eﬀects” in that this is a false
positive result.
If they operate diﬀerently on treatment and control groups,
then they may introduce biases
If people from the control group behave diﬀerently, these
eﬀects are sometimes called “John Henry” eﬀects
Substitution bias
Control group members may seek substitutes for treatment
This would bias the estimated treatment eﬀects downward.
Can you see why?
Can also occur if the experiment frees up resources that can
now be concentrated on the control group.

### Slide 150

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Regression fundamentals
Randomized Experiments
Directed Acyclical Graphs
History of graphical causal modeling in science
Path diagrams were developed by Sewell Wright, early 20th
century geneticist, for causal inference
Sewell Wright’s father, Phillip Wright, used them to prove the
existence of the “instrumental variable” estimator (see Stock
and Trebbi 2003)
Judea Pearl and colleagues in Artiﬁcial Intelligence at UCLA
developed DAG modeling to create a formalized causal
inference methodology
Cunningham Causal Inference

### Slide 151

Three biases in observational studies DAGs help illustrate
1 Omitted variable bias
Something causes the outcome which is correlated with the
treatment
2 Reverse causality
Your logic about cause and eﬀect is backwards
3 Conditioning on collider
Conditioning on some variables can introduce spurious
correlations

### Slide 152

Simple DAG
A Directed Acyclic Graph (DAG) is a set of nodes and arrows
with no directed cycles
Z X
U
Y
Nodes represent variables
Arrows represent direct causal eﬀects (“direct” means not
mediated by other variables in the graph)

### Slide 153

Complete DAGs
A DAG is a complete set of all causal relationships, but we
emphasize the causal relationships that surround D and Y
All direct causal eﬀects among the variables in the graph
All common causes of any pair of variables in the graph
Do not leave out anything, even if you can’t measure it or
don’t have data on it.
DAGs are based on theory, a model, observation, experience,
prior studies, intuition
Try to get into the habit of accompanying every regression
you will ever run with a DAG of your own creation to help
guide your identiﬁcation strategy

### Slide 154

Some DAG concepts
Z X
U
Y
U is a parent of X and Y
X and Y are descendants of Z
There is a directed path from Z to Y
There are two paths from Z to U (but no directed path)
X is a collider of the path Z → X ← U
X is a noncollider of the path Z → X → Y

### Slide 155

Confounding
Confounding occurs when when the treatment and the
outcomes have a common cause or parent which creates
spurious correlation between D and Y
D
X
Y
The correlation between D and Y no longer reﬂects the
causal eﬀect of D on Y

### Slide 156

Backdoor Paths
Confounding creates backdoor paths between treatment and
outcome (D ← X → Y )
However, we can “block” the backdoor path by conditioning
on the common cause X
Once we condition on X , the correlation between D and Y
estimates the causal eﬀect of D on Y
Conditioning means calculating
E [Y |D = 1, X ] − E [Y |D = 0, X ] for each value of X
D
X
Y

### Slide 157

Blocked backdoor paths
A backdoor path is blocked if and only if:
It contains a noncollider that has been conditioned on
Or it contains a collider that has not been conditioned on and
has no descendents that have been conditioned on

### Slide 158

Blocked paths
Examples:
1 Conditioning on a noncollider blocks a path:
X Z Y
2 Conditioning on a collider opens a path:
Z X Y
3 Not conditioning on a collider blocks a path:
Z X Y

### Slide 159

Backdoor criterion
Backdoor criterion
Conditioning on X satisﬁes the backdoor criterion with respect to
(D, Y ) directed path if:
1 All backdoor paths are blocked by X
2 No element of X is a collider
In words: If X satisﬁes the backdoor criterion with respect to
(D, Y ), then matching on X identiﬁes the causal eﬀect of D on Y

### Slide 160

Matching on all common causes is suﬃcient
There are two backdoor paths from D to Y
X1 D
X2
Y
Conditioning on X1 and X2 blocks both backdoor paths

### Slide 161

Matching may work even if not all common causes are
observed
U and X1 are common causes
U X2
X1
D Y
Conditioning on X1 and X2 is enough

### Slide 162

There may be more than one set of conditioning
variables that satisfy the backdoor criterion
X3
X1
X2
D Y
X3
X1
X2
D Y
Conditioning on the common causes, X1 and X2, is suﬃcient
. . . but so is conditioning on X3

### Slide 163

Collider bias
Matching on an outcome may create bias
There is only one backdoor path from D to Y
X1 D
X2
Y
Conditioning on X1 blocks the backdoor path
But what if we also condition on X2?
X1 D
X2
Y
Conditioning on X2 opens up a new path!

### Slide 164

Matching on all pretreatment covariates can create bias
There is one backdoor path and it is closed
X
U1
U2
D Y
No confounding – D is identiﬁed
But what if we condition on X ? Now a backdoor path opens.
X
U1
U2
D Y

### Slide 165

Ultimately, we can’t know if we have a collider bias problem,
or whether we’ve satisﬁed the backdoor criterion, without a
model
There’s no getting around it – all empirical work requires
theory to guide the work. Otherwise how do you know if
you’ve conditioning on a noncollider or a collider?
Put diﬀerently, you cannot identify treatment eﬀects without
making assumptions about the process that generated your
data

### Slide 166

Simple example of collider bias
Important: Since unconditioned colliders block back-door paths,
what exactly does conditioning on a collider do? Let’s illustrate
with a fun example and some made-up data
CNN.com headline: Megan Fox voted worst – but sexiest –
actress of 2009 (link)
Assume talent and beauty are independent, but each causes
someone to become a movie star. What’s the correlation
between talent and beauty for a sample of movie stars
compared to the population as a whole (stars and non-stars)?

### Slide 167

What if the sample consists only of movie stars?
Movie Star
Talent Beauty

### Slide 168

STATA code
clear all
set seed 3444
* 2500 independent draws from standard normal distribution
set obs 2500
generate beauty=rnormal()
generate talent=rnormal()
* Creating the collider variable (star)
gen score=(beauty+talent)
egen c85=pctile(score), p(85)
gen star=(score>=c85)
label variable star "Movie star"
* Conditioning on the top 15%
twoway (scatter beauty talent, mcolor(black) msize(small) msymbol(smx)),
ytitle(Beauty) xtitle(Talent) subtitle(Aspiring actors and actresses)
by(star, total)

### Slide 169

-4-2024-4-2024-4-2024-4-202401TotalBeautyTalentGraphs by Movie starAspiring actors and actresses
Figure: Top left ﬁgure: Non-star sample scatter plot of beauty (vertical axis) and talent
(horizontal axis). Top right right ﬁgure: Star sample scatter plot of beauty and talent.
Bottom left ﬁgure: Entire (stars and non-stars combined) sample scatter plot of beauty and
talent.

### Slide 170

Final Remarks
The backdoor criterion provides a useful graphical guide that
can be used to select matching variables
The backdoor criterion is only a set of suﬃcient conditions,
but covers most of the interesting cases
Applying the backdoor criterion requires knowledge of the
DAG
Suppose that there is a set of observed pretreatment covariates
and that we know if they are causes of the treatment and/or
the outcomes
Suppose that there exists a subset of the observed covariates
such that matching on them is enough to control for
confounding
Then, matching on the subset of the observed covariates that
are either a cause of the treatment or a cause of the outcome
or both is also enough to control for confounding

### Slide 171

What if you can’t conduct randomized experiment?
Problems with the experimental design itself:
non-compliance by administrators
non-compliance by members of the treatment group
non-compliance by members of the control group
Experiments may be impractical due to:
Too expensive
Unethical
Not feasible for some other reason

### Slide 172

Observational alternatives to experiments
1 Selection on observables : Treatment and control groups diﬀer
from each other on observable characteristics only
Stratiﬁcation/subclassiﬁcation
Matching
Propensity score matching
2 Selection on unobservables : treatment and control groups
diﬀer from each other on unobservable characteristics
exogenous variable induces variation in treatment –
instrumental variables
selection mechanism is known – regression discontinuity
designs
treatment and controls are observed before and after
treatment – panel estimators, diﬀerences-in-diﬀerences,
synthetic control, even studies

### Slide 173

Readings
Textbook readings
Read MW chapter 3-4
Read MHE chapter 3
Article readings
See website under Matching estimation (Job Trainings Papers)

### Slide 174

GERRY B. HILL, WAYNE MILLAR and JAMES CONNELLY 
Figure 1 
Lung Cancer at Autopsy: Combined Results from 18 Studies 
Per cent of autopsies 
8 * 
1860 1870 1880 1890 1900 l910 1920 1930 1940 1950 
Year 
Observed + fitted 
Mortality Statistics 
The Registrar General of England and Wales began publishing the num- 
bers of deaths for specific cancer sites in 1911.W The death rates for can- 
cer of the lung from 1911 to 1955 were published by Percy Stocks.26 The 
rates increased exponentially over the period: 10% per year in males 
and 6% per year in females. Canadian rates for the period 1931-52 were 
published by A. J. Phillips.27 The rates were consistently lower in Canada 
than in England and Wales, but also increased exponentially at 8% per 
year in males and 4% per year in females. 
The British and Canadian rates are shown in Figure 2. The rates (a) for 
males, and (b) for females have been age-standardized,28 and the trends 
extended to 1990, using data published by Richard Peto and colleagues, 29 
and by Statistics Canada.30 In British males the rates reached a maxi- 
I mum in the mid-1970's and then declined. In Canadian males the initial 
rise was more prolonged, reaching a maximum in 1990. Among females 
the age-standardized rates continue to climb in both countries, the rise 
being steeper in Canada than in Britain. 
The fact that mortality was lower at first in Canada than in Britain 
may be explained by the difference in smoking in the two countries. 
Percy Stocks31 cited data on the annual consumption per adult of ciga- 
rettes in various countries between 1939 and 1957. In 1939 the con- 
sumption in Canada was only half that in the United Kingdom, while in 
1957 the consumption in Canada was 5% higher than in the United 
Kingdom. 
The trends in the age-standardized rates in most developed countries 
between 1953 and 1992 have been published recently by the International 
"The Great Debate" 371 
Figure 2(a) 
Mortality from Cancer of the Lung in Males 
Rate per 100,000 
120 
l00 - 
80 - 
60 - 
40 - 
20 - 
0 
l910 1920 1930 1940 1960 1960 1970 l980 l990 2000 
Year 
- England & Wales + Canada + United Kingdom 
Standardlzed to European Population 
Figure 2(b) 
Mortality from Cancer of the Lung in Females 
Rate per 100,000 
36 
30 - 
26 - 
20 - 
l6 - 
l0 - 
0 
1910 1920 1930 1940 1960 l960 1970 1980 1990 2000 
Year 
- England (L Wales + Canada -+- United Kingdom 
Standardlzed to European Population

### Slide 175

"The Great Debate" 375 
to the publication of the London results later that year73 and the com- 
plete study, involving 1,465 pairs of lung cancer patients and controls, in 
1952.54 The results were consistent with earlier studies, but the large 
number of patients involved, and the more detailed questions asked, 
led to a more searching analysis and the elimination of possible con- 
founding factors. 
After 1950 the number of case-coytrol studies mushroomed. The accu- 
mulating evidence from these studies was reviewed successively by 
Richard Doll75 Harold Dorn,56 and the Surgeon General of the United 
States.57 Initially the results of the case-control studies were expressed 
simply in terms of the proportions of smokers among cases and con- 
trols (p1 and p2,say), but in a seminal paper in 1951 Jerome Cornfield58 
showed that the odds ratio, pl/(l - pl): p241 - p2), in a case-control 
study is an estimate of the ratio of the risk of lung cancer among smok- 
' ers to that among non-smokers. This paper was the first major contri- 
bution to epidemiological methods produced by the problem of lung 
cancer and smoking. 
Figure 4 summarizes the results of 24 case-control studies in males and 
12 such studies in females. Many of the studies were small so that the 
estimates of the odds ratio varied considerably. In Figure 4 a weighted 
mean is shown, calculated by a method described by Barnet Woolf in 
1955.59 The risk of lung cancer is greater in males than females, and 
increases with the amount smoked. 
Figure 4 
Smoking and Lung Cancer Case-control Studies 
Odds Ratio* 
'0 1 
" 
Lees than 20 20 or more All 
Male8 m Females 
Weighted mean 
376 GERRY B. HILL, WAYNE MILLAR and JAMES CONNELLY 
Cohort Studies 60 
Cohort studies, though less prone to bias, are much more difficult to 
perform than case-control studies, since it is necessary to assemble many 
thousands of individuals, determine their smoking status, and follow 
them up for several years to determine how many develop lung cancer. 
Four such studies were mounted in the 1950s. The subjects used were 
British doctors,61 United States veterans,62 Canadian veterans,63 and vol- 
unteers assembled by the American Cancer Society.@ All four used mor- 
tality as the end-point. 
Figure 5 shows the combined mortality ratios for cancer of the lung in 
males by level of cigarette smoking. Two of the studies involved females, 
but the numbers of lung cancer deaths were too small to provide precise 
estimates. Since all causes of death were recorded in the cohort studies it 
was possible to determine the relationship between smoking and dis- 
eases other than lung cancer. Sigruficant associations were found in rela- ' 
tion to several types of cancer (e.g. mouth, pharynx, larynx, esophagus, 
bladder) and with chronic respiratory disease and cardiovascular disease. 
Figure 5 
Smoking and Lung cancer Cohort Studies in Males 
Mortality Ratio* 
25 
-1 
Less than 10 10 to 19 , 20 or more 
welghied mean of 4 etudles 
The epidemiological studies of smoking and cancer of the lung stim- 
ulated debates within two areas of discourse: (1) the scientific literature, 
and (2) the medical establishment and government agencies. The first of 
these is most relevant to the development of epidemiology as a scientific 
discipline.

### Slide 176

The Great Debate
Smoking,S, causeslung cancer,C (S →C) versus spurious correlation due to
backdoor path:
Z
CS
Criticisms from Joseph Berkson, Jerzy Neyman and Ronald Fisher: (Hill, Millar
and Connelly 2003)
1 Correlation b/w smoking and lung cancer is spurious due to biased selection of
subjects (e.g., conditioning on collider problem)
2 Functional form complaints about using “risk ratios” and “odds ratios”
3 Confounder,Z, creates backdoor path between smoking and cancer
4 Implausible magnitudes
5 No experimental evidence to incriminate smoking as a cause of lung cancer
Fisher’s confounding theory
Fisher, equally famous as a geneticist, argued from logic, statistics and genetic
evidence for a hypothetical confounding genome,Z, and therefore smokers and
non-smokers were not exchangeable (violation of independence assumption)
Other studies showed that cigarette smokers and non-smokers were diﬀerent on
observables – more extraverted than non-smokers and pipe smokers, diﬀered in
age, diﬀered in income, diﬀered in education, etc.

### Slide 177

Uh, I thought you told me Fisher was smart?
Fisher’s arguments were actually based on sound science.
It may be too easy for us to criticize Fisher for his stance
because we look back to a diﬀerent time when the
smoking/lung cancer link was not universally accepted, and
evidence for the causal link was shallow:
“the [the epidemiologists] turned out to be right,
but only because bad logic does not necessarily lead
to wrong conclusions.” Robert Hooke’s (1983), How
to Tell the Liars from the Statisticians.
But FWIW, Fisher was a chain smoking pipe smoker, he died
of lung cancer, and he was a paid expert witness for the
tobacco industry.

### Slide 178

Motivation: Smoking and Mortality
Table: Death rates per 1,000 person-years (Cochran 1968)
Smoking group Canada U.K. U.S.
Non-smokers 20.2 11.3 13.5
Cigarettes 20.5 14.1 13.5
Cigars/pipes 35.5 20.7 17.4
Are cigars dangerous?

### Slide 179

Non-smokers and smokers diﬀer in mortality and age
Table: Mean ages, years (Cochran 1968)
Smoking group Canada U.K. U.S.
Non-smokers 54.9 49.1 57.0
Cigarettes 50.5 49.8 53.2
Cigars/pipes 65.9 55.7 59.7
Older people die at a higher rate, and for reasons other than
just smoking cigars
Maybe cigar smokers higher observed death rates is because
they’re older on average

### Slide 180

Subclassiﬁcation
One way to think about the problem is that the covariates are
not balanced – their mean values diﬀer for treatment and
control group. So let’s try to balance them.
Subclassiﬁcation (also called stratiﬁcation): Compare
mortality rates across the diﬀerent smoking groups within age
groups so as to neutralize covariate imbalances in the
observed sample

### Slide 181

Subclassiﬁcation
Divide the smoking group samples into age groups
For each of the smoking group samples, calculate the
mortality rates for the age group
Construct probability weights for each age group as the
proportion of the sample with a given age
Compute the weighted averages of the age groups mortality
rates for each smoking group using the probability weights

### Slide 182

Subclassiﬁcation: example
Death rates Number of
Pipe-smokers Pipe-smokers Non-smokers
Age 20-50 15 11 29
Age 50-70 35 13 9
Age +70 50 16 2
Total 40 40
Question: What is the average death rate for pipe smokers?

### Slide 183

Subclassiﬁcation: example
Death rates Number of
Pipe-smokers Pipe-smokers Non-smokers
Age 20-50 15 11 29
Age 50-70 35 13 9
Age +70 50 16 2
Total 40 40
Question: What is the average death rate for pipe smokers?
15 ·
(11
40
)
+ 35 ·
(13
40
)
+ 50 ·
(16
40
)
= 35.5

### Slide 184

Subclassiﬁcation: example
Death rates Number of
Pipe-smokers Pipe-smokers Non-smokers
Age 20-50 15 11 29
Age 50-70 35 13 9
Age +70 50 16 2
Total 40 40
Question: What would the average mortality rate be for pipe smokers
if they had the same age distribution as the non-smokers?

### Slide 185

Subclassiﬁcation: example
Death rates Number of
Pipe-smokers Pipe-smokers Non-smokers
Age 20-50 15 11 29
Age 50-70 35 13 9
Age +70 50 16 2
Total 40 40
Question: What would the average mortality rate be for pipe smokers
if they had the same age distribution as the non-smokers?
15 ·
(29
40
)
+ 35 ·
( 9
40
)
+ 50 ·
( 2
40
)
= 21.2

### Slide 186

Table: Adjusted death rates using 3 age groups (Cochran 1968)
Smoking group Canada U.K. U.S.
Non-smokers 20.2 11.3 13.5
Cigarettes 28.3 12.8 17.7
Cigars/pipes 21.2 12.0 14.2

### Slide 187

Covariates
Deﬁnition: Predetermined Covariates
Variable X is predetermined with respect to the treatment D (also
called “pretreatment”) if for each individual i, X 0
i = X 1
i , i.e., the
value of Xi does not depend on the value of Di. Such
characteristics are called covariates.
Comment I: Does not imply X and D are independent
Comment II: Predetermined variables are often time invariant (e.g.,
sex, race), but time invariance is not a necessary condition

### Slide 188

Outcomes
Deﬁnition: Outcomes
Those variables, Y , that are (possibly) not predetermined are
called outcomes (for some individual i, Y 0
i ̸= Y 1
i )
Comment: Recall the “collider bias”; in general, one shouldn’t
condition on outcomes because it may induce bias

### Slide 189

Adjustment for Observables
Subclassiﬁcation
Matching
Propensity score methods
Regression

### Slide 190

Identiﬁcation under independence
Recall that randomization implies
(Y 0, Y 1) ⊥ ⊥D
and therefore:
E [Y |D = 1] − E [Y |D = 0] = E [Y 1|D = 1] − E [Y 0|D = 0]  
by the switching equation
= E [Y 1] − E [Y 0]  
by independence
= E [Y 1 − Y 0]  
ATE
As well as that ATT = ATE:
E [Y 1 − Y 0] = E [Y 1 − Y 0|D = 1]

### Slide 191

Identiﬁcation under conditional independence
Identiﬁcation assumptions:
1 (Y 1, Y 0) ⊥ ⊥D|X (conditional independence)
2 0 < Pr(D = 1|X ) < 1 with probability one (common support)
Identiﬁcation result:
Given assumption 1:
E [Y 1 − Y 0|X ] = E [Y 1 − Y 0|X , D = 1]
= E [Y |X , D = 1] − E [Y |X , D = 0]
Given assumption 2:
δATE = E [Y 1 − Y 0]
=
∫
E [Y 1 − Y 0|X , D = 1]dPr(X )
=
∫
(E [Y |X , D = 1] − E [Y |X , D = 0]) dPr(X )

### Slide 192

Identiﬁcation under conditional independence
Identiﬁcation assumptions:
1 (Y 1, Y 0) ⊥ ⊥D|X (conditional independence)
2 0 < Pr(D = 1|X ) < 1 with probability one (common support)
Identiﬁcation result:
Similarly
δATT = E [Y 1 − Y 0|D = 1]
=
∫
(E [Y |X , D = 1] − E [Y |X , D = 0]) dPr(X |D = 1)
To identify δATT the conditional independence and common
support assumptions can be relaxed to:
1 Y 0 ⊥ ⊥D|X
2 Pr(D = 1|X ) < 1 (with Pr(D = 1) > 0)

### Slide 193

Subclassiﬁcation estimator
The identiﬁcation result is:
δATE =
∫
(E [Y |X , D = 1] − E [Y |X , D = 0]) dPr(X )
δATT =
∫
(E [Y |X , D = 1] − E [Y |X , D = 0]) dPr(X |D = 1)
Assume X takes on K diﬀerent cells {X 1, . . . ,X k , . . . ,X K }.
Then the analogy principle suggests the following estimators:
ˆδATE =
K∑
k=1
(Y
1,k
− Y
0,k
) ·
(N k
N
)
ˆδATT =
K∑
k=1
(Y
1,k
− Y
0,k
) ·
(N k
T
NT
)
where N k is the number of obs. and N K
T is the number of
treatment observations in cell k; Y
1,k
is the mean outcome
for the treated in cell k; Y
0,k
is the mean outcome for the
control in cell k

### Slide 194

Subclassiﬁcation by Age ( K = 2)
Death Rate Number of
Xk Smokers Non-smokers Diﬀ. Smokers Non-smokers
Old 28 24 4 3 10
Young 22 16 6 7 10
Total 10 20
Question: What is ˆδATE =
K∑
k=1
(Y
1,k
− Y
0,k
) ·
(N k
N
)
?

### Slide 195

Subclassiﬁcation by Age ( K = 2)
Death Rate Number of
Xk Smokers Non-smokers Diﬀ. Smokers Non-smokers
Old 28 24 4 3 10
Young 22 16 6 7 10
Total 10 20
Question: What is ˆδATE =
K∑
k=1
(Y
1,k
− Y
0,k
) ·
(N k
N
)
?
4 ·
(13
30
)
+ 6 ·
(17
30
)
= 5.13

### Slide 196

Subclassiﬁcation by Age ( K = 2)
Death Rate Number of
Xk Smokers Non-smokers Diﬀ. Smokers Non-smokers
Old 28 24 4 3 10
Young 22 16 6 7 10
Total 10 20
Question: What is ˆδATT =
K∑
k=1
(Y
1,k
− Y
0,k
) ·
(N k
T
NT
)
?

### Slide 197

Subclassiﬁcation by Age ( K = 2)
Death Rate Number of
Xk Smokers Non-smokers Diﬀ. Smokers Non-smokers
Old 28 24 4 3 10
Young 22 16 6 7 10
Total 10 20
Question: What is ˆδATT =
K∑
k=1
(Y
1,k
− Y
0,k
) ·
(N k
T
NT
)
?
4 ·
( 3
10
)
+ 6 ·
( 7
10
)
= 5.4

### Slide 198

Subclassiﬁcation by Age and Gender (K = 4)
Death Rate Number of
Xk Smokers Non-smokers Diﬀ. Smokers Non-smokers
Old Males 28 22 4 3 7
Old Females 24 3
Young Males 21 16 5 3 4
Young Females 23 17 6 4 6
Total 10 20
Problem: What isˆδATE=
K∑
k=1
(Y1,k −Y0,k) ·
(Nk
N
)
?

### Slide 199

Subclassiﬁcation by Age and Gender (K = 4)
Death Rate Number of
Xk Smokers Non-smokers Diﬀ. Smokers Non-smokers
Old Males 28 22 4 3 7
Old Females 24 3
Young Males 21 16 5 3 4
Young Females 23 17 6 4 6
Total 10 20
Problem: What isˆδATE=
K∑
k=1
(Y1,k −Y0,k) ·
(Nk
N
)
?
Not identiﬁed!

### Slide 200

Subclassiﬁcation by Age and Gender (K = 4)
Death Rate Number of
Xk Smokers Non-smokers Diﬀ. Smokers Non-smokers
Old Males 28 22 4 3 7
Old Females 24 3
Young Males 21 16 5 3 4
Young Females 23 17 6 4 6
Total 10 20
Question: What isˆδATT=
K∑
k=1
(Y1,k −Y0,k) ·
(NkT
NT
)
?

### Slide 201

Subclassiﬁcation by Age and Gender (K = 4)
Death Rate Number of
Xk Smokers Non-smokers Diﬀ. Smokers Non-smokers
Old Males 28 22 4 3 7
Old Females 24 3
Young Males 21 16 5 3 4
Young Females 23 17 6 4 6
Total 10 20
Question: What isˆδATT=
K∑
k=1
(Y1,k −Y0,k) ·
(NkT
NT
)
?
4·
(3
10
)
+ 5·
(3
10
)
+ 6·
(4
10
)
= 5.1

### Slide 202

Curse of Dimensionality
Subclassiﬁcation may become less feasible in ﬁnite samples as
the number of covariates grows (e.g., K = 4 was too many for
this sample)
Assume we have k covariates and we divide each into 3 coarse
categories (e.g., age: young, middle age, old; income: low,
medium, high, etc.)
The number of sub classiﬁcation cells (or “strata”) is 3 k. For
k = 10, then it’s 3 10 = 59, 049

### Slide 203

Curse of Dimensionality
If sparseness occurs, it means many cells may contain either
only treatment units or only control units but not both. If so,
we cannot use sub classiﬁcation.
Subclassiﬁcation is also a problem if the cells are “too
coarse”. We can always use “ﬁner” classiﬁcations, but ﬁner
cells worsens the dimensional problem, so we don’t gain much
from that. ex: using 10 variables and 5 categories for each, we
get 5 10 = 9, 765, 625.

### Slide 204

Matching
Alternatively, we could estimate δATT by imputing the missing
potential outcome of each treatment unit using the observed
outcome from that outcome’s “closest” control unit
δATT = 1
NT
∑
Di =1
(Yi − Yj(i))
where Yj(i) is the observed outcome of a control unit such
that Xj(i) is the closest value to Xi among all of the control
observations

### Slide 205

Matching
We could also use the average observed outcome over M
closest matches:
δATT = 1
NT
∑
Di =1
(
Yi −
[
1
M
M∑
m=1
Yjm(1)
])
Works well when we can ﬁnd good matches for each
treatment group unit, so M is usually deﬁned to be small (i.e.,
M = 1 or M = 2)

### Slide 206

Matching
We can also use matching to estimate δATE. In that case, we
match in both directions:
1 If observation i is treated, we impute Y 0
i using the control
matches, {Yj1(i), . . . ,YjM (i)}
2 If observation i is control, we impute Y 1
i using the treatment
matches, {Yj1(i), . . . ,YjM (i)}
The estimator is:
ˆδATE = 1
N
N∑
i=1
(2Di − 1)
[
Yi −
(
1
M
M∑
m=1
Yjm(i)
)]

### Slide 207

Matching example with single covariate
Potential Outcome
unit under Treatment under Control
i Y 1
i Y 0
i DI Xi
1 6 ? 1 3
2 1 ? 1 1
3 0 ? 1 10
4 0 0 2
5 9 0 3
6 1 0 -2
7 1 0 -4
Question: What is ˆδATT = 1
NT
∑
Di =1
(Yi − Yj(i))?

### Slide 208

Matching example with single covariate
Potential Outcome
unit under Treatment under Control
i Y 1
i Y 0
i DI Xi
1 6 ? 1 3
2 1 ? 1 1
3 0 ? 1 10
4 0 0 2
5 9 0 3
6 1 0 -2
7 1 0 -4
Question: What is ˆδATT = 1
NT
∑
Di =1
(Yi − Yj(i))?
Match and plug in!

### Slide 209

Matching example with single covariate
Potential Outcome
unit under Treatment under Control
i Y 1
i Y 0
i DI Xi
1 6 9 1 3
2 1 0 1 1
3 0 9 1 10
4 0 0 2
5 9 0 3
6 1 0 -2
7 1 0 -4
Question: What is ˆδATT = 1
NT
∑
Di =1
(Yi − Yj(i))?
ˆδATT = 1
3 · (6 − 9) + 1
3 · (1 − 0) + 1
3 · (0 − 9) = −3.7

### Slide 210

A Training Example
Trainees Non-Trainees
unit age earnings unit age earnings
12 8 1 7 7 0 0 1 4 3 2 0 9 0 0
23 4 1 0 2 0 0 2 5 0 3 1 0 0 0
32 9 1 4 4 0 0 3 3 0 2 1 0 0 0
42 5 2 0 8 0 0 4 2 7 9 3 0 0
52 9 6 1 0 0 5 5 4 4 1 1 0 0
62 3 2 8 6 0 0 6 4 8 2 9 8 0 0
73 3 2 1 9 0 0 7 3 9 4 2 0 0 0
82 7 2 8 8 0 0 8 2 8 8 8 0 0
93 1 2 0 3 0 0 9 2 4 2 5 5 0 0
10 26 28100 10 33 15500
11 25 9400 11 26 400
12 27 14300 12 31 26600
13 29 12500 13 26 16500
14 24 19700 14 34 24200
15 25 10100 15 25 23300
16 43 10700 16 24 9700
17 28 11500 17 29 6200
18 27 10700 18 35 30200
19 28 16300 19 32 17800
Average: 28.5 16426 20 23 9500
21 32 25900
Average: 33 20724

### Slide 211

A Training Example
Trainees Non-Trainees Matched Sample
unit age earnings unit age earnings unit age earnings
12 8 1 7 7 0 0 1 4 3 2 0 9 0 0
23 4 1 0 2 0 0 2 5 0 3 1 0 0 0
32 9 1 4 4 0 0 3 3 0 2 1 0 0 0
42 5 2 0 8 0 0 4 2 7 9 3 0 0
52 9 6 1 0 0 5 5 4 4 1 1 0 0
62 3 2 8 6 0 0 6 4 8 2 9 8 0 0
73 3 2 1 9 0 0 7 3 9 4 2 0 0 0
82 7 2 8 8 0 0 8 2 8 8 8 0 0
93 1 2 0 3 0 0 9 2 4 2 5 5 0 0
10 26 28100 10 33 15500
11 25 9400 11 26 400
12 27 14300 12 31 26600
13 29 12500 13 26 16500
14 24 19700 14 34 24200
15 25 10100 15 25 23300
16 43 10700 16 24 9700
17 28 11500 17 29 6200
18 27 10700 18 35 30200
19 28 16300 19 32 17800
Average: 28.5 16426 20 23 9500 Average:
21 32 25900
Average: 33 20724

### Slide 212

A Training Example
Trainees Non-Trainees Matched Sample
unit age earnings unit age earnings unit age earnings
1 28 17700 14 3 2 0 9 0 0
23 4 1 0 2 0 0 2 5 0 3 1 0 0 0
32 9 1 4 4 0 0 3 3 0 2 1 0 0 0
42 5 2 0 8 0 0 4 2 7 9 3 0 0
52 9 6 1 0 0 5 5 4 4 1 1 0 0
62 3 2 8 6 0 0 6 4 8 2 9 8 0 0
73 3 2 1 9 0 0 7 3 9 4 2 0 0 0
82 7 2 8 8 0 0 8 2 8 8 8 0 0
93 1 2 0 3 0 0 9 2 4 2 5 5 0 0
10 26 28100 10 33 15500
11 25 9400 11 26 400
12 27 14300 12 31 26600
13 29 12500 13 26 16500
14 24 19700 14 34 24200
15 25 10100 15 25 23300
16 43 10700 16 24 9700
17 28 11500 17 29 6200
18 27 10700 18 35 30200
19 28 16300 19 32 17800
Average: 28.5 16426 20 23 9500 Average:
21 32 25900
Average: 33 20724

### Slide 213

A Training Example
Trainees Non-Trainees Matched Sample
unit age earnings unit age earnings unit age earnings
1 28 17700 14 3 2 0 9 0 0
23 4 1 0 2 0 0 2 5 0 3 1 0 0 0
32 9 1 4 4 0 0 3 3 0 2 1 0 0 0
42 5 2 0 8 0 0 4 2 7 9 3 0 0
52 9 6 1 0 0 5 5 4 4 1 1 0 0
62 3 2 8 6 0 0 6 4 8 2 9 8 0 0
73 3 2 1 9 0 0 7 3 9 4 2 0 0 0
82 7 2 8 8 0 0 8 28 8800
93 1 2 0 3 0 0 9 2 4 2 5 5 0 0
10 26 28100 10 33 15500
11 25 9400 11 26 400
12 27 14300 12 31 26600
13 29 12500 13 26 16500
14 24 19700 14 34 24200
15 25 10100 15 25 23300
16 43 10700 16 24 9700
17 28 11500 17 29 6200
18 27 10700 18 35 30200
19 28 16300 19 32 17800
Average: 28.5 16426 20 23 9500 Average:
21 32 25900
Average: 33 20724

### Slide 214

A Training Example
Trainees Non-Trainees Matched Sample
unit age earnings unit age earnings unit age earnings
1 28 17700 14 3 2 0 9 0 0 8 28 8800
23 4 1 0 2 0 0 2 5 0 3 1 0 0 0
32 9 1 4 4 0 0 3 3 0 2 1 0 0 0
42 5 2 0 8 0 0 4 2 7 9 3 0 0
52 9 6 1 0 0 5 5 4 4 1 1 0 0
62 3 2 8 6 0 0 6 4 8 2 9 8 0 0
73 3 2 1 9 0 0 7 3 9 4 2 0 0 0
82 7 2 8 8 0 0 8 28 8800
93 1 2 0 3 0 0 9 2 4 2 5 5 0 0
10 26 28100 10 33 15500
11 25 9400 11 26 400
12 27 14300 12 31 26600
13 29 12500 13 26 16500
14 24 19700 14 34 24200
15 25 10100 15 25 23300
16 43 10700 16 24 9700
17 28 11500 17 29 6200
18 27 10700 18 35 30200
19 28 16300 19 32 17800
Average: 28.5 16426 20 23 9500 Average:
21 32 25900
Average: 33 20724

### Slide 215

A Training Example
Trainees Non-Trainees Matched Sample
unit age earnings unit age earnings unit age earnings
12 8 1 7 7 0 0 1 4 3 2 0 9 0 0 82 8 8 8 0 0
2 34 10200 25 0 3 1 0 0 0 14 34 24200
32 9 1 4 4 0 0 3 3 0 2 1 0 0 0
42 5 2 0 8 0 0 4 2 7 9 3 0 0
52 9 6 1 0 0 5 5 4 4 1 1 0 0
62 3 2 8 6 0 0 6 4 8 2 9 8 0 0
73 3 2 1 9 0 0 7 3 9 4 2 0 0 0
82 7 2 8 8 0 0 8 2 8 8 8 0 0
93 1 2 0 3 0 0 9 2 4 2 5 5 0 0
10 26 28100 10 33 15500
11 25 9400 11 26 400
12 27 14300 12 31 26600
13 29 12500 13 26 16500
14 24 19700 14 34 24200
15 25 10100 15 25 23300
16 43 10700 16 24 9700
17 28 11500 17 29 6200
18 27 10700 18 35 30200
19 28 16300 19 32 17800
Average: 28.5 16426 20 23 9500 Average:
21 32 25900
Average: 33 20724

### Slide 216

A Training Example
Trainees Non-Trainees Matched Sample
unit age earnings unit age earnings unit age earnings
12 8 1 7 7 0 0 1 4 3 2 0 9 0 0 82 8 8 8 0 0
23 4 1 0 2 0 0 2 5 0 3 1 0 0 0 1 4 3 4 2 4 2 0 0
3 29 14400 33 0 2 1 0 0 0 17 29 6200
42 5 2 0 8 0 0 4 2 7 9 3 0 0
52 9 6 1 0 0 5 5 4 4 1 1 0 0
62 3 2 8 6 0 0 6 4 8 2 9 8 0 0
73 3 2 1 9 0 0 7 3 9 4 2 0 0 0
82 7 2 8 8 0 0 8 2 8 8 8 0 0
93 1 2 0 3 0 0 9 2 4 2 5 5 0 0
10 26 28100 10 33 15500
11 25 9400 11 26 400
12 27 14300 12 31 26600
13 29 12500 13 26 16500
14 24 19700 14 34 24200
15 25 10100 15 25 23300
16 43 10700 16 24 9700
17 28 11500 17 29 6200
18 27 10700 18 35 30200
19 28 16300 19 32 17800
Average: 28.5 16426 20 23 9500 Average:
21 32 25900
Average: 33 20724

### Slide 217

A Training Example
Trainees Non-Trainees Matched Sample
unit age earnings unit age earnings unit age earnings
12 8 1 7 7 0 0 1 4 3 2 0 9 0 0 82 8 8 8 0 0
23 4 1 0 2 0 0 2 5 0 3 1 0 0 0 1 4 3 4 2 4 2 0 0
32 9 1 4 4 0 0 3 3 0 2 1 0 0 0 1 7 2 9 6 2 0 0
4 25 20800 42 7 9 3 0 0 15 25 23300
52 9 6 1 0 0 5 5 4 4 1 1 0 0
62 3 2 8 6 0 0 6 4 8 2 9 8 0 0
73 3 2 1 9 0 0 7 3 9 4 2 0 0 0
82 7 2 8 8 0 0 8 2 8 8 8 0 0
93 1 2 0 3 0 0 9 2 4 2 5 5 0 0
10 26 28100 10 33 15500
11 25 9400 11 26 400
12 27 14300 12 31 26600
13 29 12500 13 26 16500
14 24 19700 14 34 24200
15 25 10100 15 25 23300
16 43 10700 16 24 9700
17 28 11500 17 29 6200
18 27 10700 18 35 30200
19 28 16300 19 32 17800
Average: 28.5 16426 20 23 9500 Average:
21 32 25900
Average: 33 20724

### Slide 218

A Training Example
Trainees Non-Trainees Matched Sample
unit age earnings unit age earnings unit age earnings
12 8 1 7 7 0 0 1 4 3 2 0 9 0 0 82 8 8 8 0 0
23 4 1 0 2 0 0 2 5 0 3 1 0 0 0 1 4 3 4 2 4 2 0 0
32 9 1 4 4 0 0 3 3 0 2 1 0 0 0 1 7 2 9 6 2 0 0
42 5 2 0 8 0 0 4 2 7 9 3 0 0 1 5 2 5 2 3 3 0 0
5 29 6100 55 4 4 1 1 0 0 17 29 6200
62 3 2 8 6 0 0 6 4 8 2 9 8 0 0
73 3 2 1 9 0 0 7 3 9 4 2 0 0 0
82 7 2 8 8 0 0 8 2 8 8 8 0 0
93 1 2 0 3 0 0 9 2 4 2 5 5 0 0
10 26 28100 10 33 15500
11 25 9400 11 26 400
12 27 14300 12 31 26600
13 29 12500 13 26 16500
14 24 19700 14 34 24200
15 25 10100 15 25 23300
16 43 10700 16 24 9700
17 28 11500 17 29 6200
18 27 10700 18 35 30200
19 28 16300 19 32 17800
Average: 28.5 16426 20 23 9500 Average:
21 32 25900
Average: 33 20724

### Slide 219

A Training Example
Trainees Non-Trainees Matched Sample
unit age earnings unit age earnings unit age earnings
12 8 1 7 7 0 0 1 4 3 2 0 9 0 0 82 8 8 8 0 0
23 4 1 0 2 0 0 2 5 0 3 1 0 0 0 1 4 3 4 2 4 2 0 0
32 9 1 4 4 0 0 3 3 0 2 1 0 0 0 1 7 2 9 6 2 0 0
42 5 2 0 8 0 0 4 2 7 9 3 0 0 1 5 2 5 2 3 3 0 0
52 9 6 1 0 0 5 5 4 4 1 1 0 0 1 7 2 9 6 2 0 0
6 23 28600 64 8 2 9 8 0 0 20 23 9500
73 3 2 1 9 0 0 7 3 9 4 2 0 0 0
82 7 2 8 8 0 0 8 2 8 8 8 0 0
93 1 2 0 3 0 0 9 2 4 2 5 5 0 0
10 26 28100 10 33 15500
11 25 9400 11 26 400
12 27 14300 12 31 26600
13 29 12500 13 26 16500
14 24 19700 14 34 24200
15 25 10100 15 25 23300
16 43 10700 16 24 9700
17 28 11500 17 29 6200
18 27 10700 18 35 30200
19 28 16300 19 32 17800
Average: 28.5 16426 20 23 9500 Average:
21 32 25900
Average: 33 20724

### Slide 220

A Training Example
Trainees Non-Trainees Matched Sample
unit age earnings unit age earnings unit age earnings
12 8 1 7 7 0 0 1 4 3 2 0 9 0 0 82 8 8 8 0 0
23 4 1 0 2 0 0 2 5 0 3 1 0 0 0 1 4 3 4 2 4 2 0 0
32 9 1 4 4 0 0 3 3 0 2 1 0 0 0 1 7 2 9 6 2 0 0
42 5 2 0 8 0 0 4 2 7 9 3 0 0 1 5 2 5 2 3 3 0 0
52 9 6 1 0 0 5 5 4 4 1 1 0 0 1 7 2 9 6 2 0 0
62 3 2 8 6 0 0 6 4 8 2 9 8 0 0 2 0 2 3 9 5 0 0
7 33 21900 73 9 4 2 0 0 0 10 33 15500
82 7 2 8 8 0 0 8 2 8 8 8 0 0
93 1 2 0 3 0 0 9 2 4 2 5 5 0 0
10 26 28100 10 33 15500
11 25 9400 11 26 400
12 27 14300 12 31 26600
13 29 12500 13 26 16500
14 24 19700 14 34 24200
15 25 10100 15 25 23300
16 43 10700 16 24 9700
17 28 11500 17 29 6200
18 27 10700 18 35 30200
19 28 16300 19 32 17800
Average: 28.5 16426 20 23 9500 Average:
21 32 25900
Average: 33 20724

### Slide 221

A Training Example
Trainees Non-Trainees Matched Sample
unit age earnings unit age earnings unit age earnings
12 8 1 7 7 0 0 1 4 3 2 0 9 0 0 82 8 8 8 0 0
23 4 1 0 2 0 0 2 5 0 3 1 0 0 0 1 4 3 4 2 4 2 0 0
32 9 1 4 4 0 0 3 3 0 2 1 0 0 0 1 7 2 9 6 2 0 0
42 5 2 0 8 0 0 4 27 9300 15 25 23300
52 9 6 1 0 0 5 5 4 4 1 1 0 0 1 7 2 9 6 2 0 0
62 3 2 8 6 0 0 6 4 8 2 9 8 0 0 2 0 2 3 9 5 0 0
73 3 2 1 9 0 0 7 3 9 4 2 0 0 0 1 0 3 3 1 5 5 0 0
8 27 28800 82 8 8 8 0 0 4 27 9300
93 1 2 0 3 0 0 9 2 4 2 5 5 0 0
10 26 28100 10 33 15500
11 25 9400 11 26 400
12 27 14300 12 31 26600
13 29 12500 13 26 16500
14 24 19700 14 34 24200
15 25 10100 15 25 23300
16 43 10700 16 24 9700
17 28 11500 17 29 6200
18 27 10700 18 35 30200
19 28 16300 19 32 17800
Average: 28.5 16426 20 23 9500 Average:
21 32 25900
Average: 33 20724

### Slide 222

A Training Example
Trainees Non-Trainees Matched Sample
unit age earnings unit age earnings unit age earnings
12 8 1 7 7 0 0 1 4 3 2 0 9 0 0 82 8 8 8 0 0
23 4 1 0 2 0 0 2 5 0 3 1 0 0 0 1 4 3 4 2 4 2 0 0
32 9 1 4 4 0 0 3 3 0 2 1 0 0 0 1 7 2 9 6 2 0 0
42 5 2 0 8 0 0 4 2 7 9 3 0 0 1 5 2 5 2 3 3 0 0
52 9 6 1 0 0 5 5 4 4 1 1 0 0 1 7 2 9 6 2 0 0
62 3 2 8 6 0 0 6 4 8 2 9 8 0 0 2 0 2 3 9 5 0 0
73 3 2 1 9 0 0 7 3 9 4 2 0 0 0 1 0 3 3 1 5 5 0 0
82 7 2 8 8 0 0 8 2 8 8 8 0 0 42 7 9 3 0 0
9 31 20300 92 4 2 5 5 0 0 12 31 26600
10 26 28100 10 33 15500
11 25 9400 11 26 400
12 27 14300 12 31 26600
13 29 12500 13 26 16500
14 24 19700 14 34 24200
15 25 10100 15 25 23300
16 43 10700 16 24 9700
17 28 11500 17 29 6200
18 27 10700 18 35 30200
19 28 16300 19 32 17800
Average: 28.5 16426 20 23 9500 Average:
21 32 25900
Average: 33 20724

### Slide 223

A Training Example
Trainees Non-Trainees Matched Sample
unit age earnings unit age earnings unit age earnings
12 8 1 7 7 0 0 1 4 3 2 0 9 0 0 82 8 8 8 0 0
23 4 1 0 2 0 0 2 5 0 3 1 0 0 0 1 4 3 4 2 4 2 0 0
32 9 1 4 4 0 0 3 3 0 2 1 0 0 0 1 7 2 9 6 2 0 0
42 5 2 0 8 0 0 4 2 7 9 3 0 0 1 5 2 5 2 3 3 0 0
52 9 6 1 0 0 5 5 4 4 1 1 0 0 1 7 2 9 6 2 0 0
62 3 2 8 6 0 0 6 4 8 2 9 8 0 0 2 0 2 3 9 5 0 0
73 3 2 1 9 0 0 7 3 9 4 2 0 0 0 1 0 3 3 1 5 5 0 0
82 7 2 8 8 0 0 8 2 8 8 8 0 0 42 7 9 3 0 0
93 1 2 0 3 0 0 9 2 4 2 5 5 0 0 1 2 3 1 2 6 6 0 0
10 26 28100 10 33 15500 11,13 26 8450
11 25 9400 11 26 400
12 27 14300 12 31 26600
13 29 12500 13 26 16500
14 24 19700 14 34 24200
15 25 10100 15 25 23300
16 43 10700 16 24 9700
17 28 11500 17 29 6200
18 27 10700 18 35 30200
19 28 16300 19 32 17800
Average: 28.5 16426 20 23 9500 Average:
21 32 25900
Average: 33 20724

### Slide 224

A Training Example
Trainees Non-Trainees Matched Sample
unit age earnings unit age earnings unit age earnings
12 8 1 7 7 0 0 1 4 3 2 0 9 0 0 82 8 8 8 0 0
23 4 1 0 2 0 0 2 5 0 3 1 0 0 0 1 4 3 4 2 4 2 0 0
32 9 1 4 4 0 0 3 3 0 2 1 0 0 0 1 7 2 9 6 2 0 0
42 5 2 0 8 0 0 4 2 7 9 3 0 0 1 5 2 5 2 3 3 0 0
52 9 6 1 0 0 5 5 4 4 1 1 0 0 1 7 2 9 6 2 0 0
62 3 2 8 6 0 0 6 4 8 2 9 8 0 0 2 0 2 3 9 5 0 0
73 3 2 1 9 0 0 7 3 9 4 2 0 0 0 1 0 3 3 1 5 5 0 0
82 7 2 8 8 0 0 8 2 8 8 8 0 0 42 7 9 3 0 0
93 1 2 0 3 0 0 9 2 4 2 5 5 0 0 1 2 3 1 2 6 6 0 0
10 26 28100 10 33 15500 11,13 26 8450
11 25 9400 11 26 400 15 25 23300
12 27 14300 12 31 26600
13 29 12500 13 26 16500
14 24 19700 14 34 24200
15 25 10100 15 25 23300
16 43 10700 16 24 9700
17 28 11500 17 29 6200
18 27 10700 18 35 30200
19 28 16300 19 32 17800
Average: 28.5 16426 20 23 9500 Average:
21 32 25900
Average: 33 20724

### Slide 225

A Training Example
Trainees Non-Trainees Matched Sample
unit age earnings unit age earnings unit age earnings
12 8 1 7 7 0 0 1 4 3 2 0 9 0 0 82 8 8 8 0 0
23 4 1 0 2 0 0 2 5 0 3 1 0 0 0 1 4 3 4 2 4 2 0 0
32 9 1 4 4 0 0 3 3 0 2 1 0 0 0 1 7 2 9 6 2 0 0
42 5 2 0 8 0 0 4 27 9300 15 25 23300
52 9 6 1 0 0 5 5 4 4 1 1 0 0 1 7 2 9 6 2 0 0
62 3 2 8 6 0 0 6 4 8 2 9 8 0 0 2 0 2 3 9 5 0 0
73 3 2 1 9 0 0 7 3 9 4 2 0 0 0 1 0 3 3 1 5 5 0 0
82 7 2 8 8 0 0 8 2 8 8 8 0 0 42 7 9 3 0 0
93 1 2 0 3 0 0 9 2 4 2 5 5 0 0 1 2 3 1 2 6 6 0 0
10 26 28100 10 33 15500 11,13 26 8450
11 25 9400 11 26 400 15 25 23300
12 27 14300 12 31 26600 4 27 9300
13 29 12500 13 26 16500
14 24 19700 14 34 24200
15 25 10100 15 25 23300
16 43 10700 16 24 9700
17 28 11500 17 29 6200
18 27 10700 18 35 30200
19 28 16300 19 32 17800
Average: 28.5 16426 20 23 9500 Average:
21 32 25900
Average: 33 20724

### Slide 226

A Training Example
Trainees Non-Trainees Matched Sample
unit age earnings unit age earnings unit age earnings
12 8 1 7 7 0 0 1 4 3 2 0 9 0 0 82 8 8 8 0 0
23 4 1 0 2 0 0 2 5 0 3 1 0 0 0 1 4 3 4 2 4 2 0 0
32 9 1 4 4 0 0 3 3 0 2 1 0 0 0 1 7 2 9 6 2 0 0
42 5 2 0 8 0 0 4 2 7 9 3 0 0 1 5 2 5 2 3 3 0 0
52 9 6 1 0 0 5 5 4 4 1 1 0 0 1 7 2 9 6 2 0 0
62 3 2 8 6 0 0 6 4 8 2 9 8 0 0 2 0 2 3 9 5 0 0
73 3 2 1 9 0 0 7 3 9 4 2 0 0 0 1 0 3 3 1 5 5 0 0
82 7 2 8 8 0 0 8 2 8 8 8 0 0 42 7 9 3 0 0
93 1 2 0 3 0 0 9 2 4 2 5 5 0 0 1 2 3 1 2 6 6 0 0
10 26 28100 10 33 15500 11,13 26 8450
11 25 9400 11 26 400 15 25 23300
12 27 14300 12 31 26600 4 27 9300
13 29 12500 13 26 16500 17 29 6200
14 24 19700 14 34 24200
15 25 10100 15 25 23300
16 43 10700 16 24 9700
17 28 11500 17 29 6200
18 27 10700 18 35 30200
19 28 16300 19 32 17800
Average: 28.5 16426 20 23 9500 Average:
21 32 25900
Average: 33 20724

### Slide 227

A Training Example
Trainees Non-Trainees Matched Sample
unit age earnings unit age earnings unit age earnings
12 8 1 7 7 0 0 1 4 3 2 0 9 0 0 82 8 8 8 0 0
23 4 1 0 2 0 0 2 5 0 3 1 0 0 0 1 4 3 4 2 4 2 0 0
32 9 1 4 4 0 0 3 3 0 2 1 0 0 0 1 7 2 9 6 2 0 0
42 5 2 0 8 0 0 4 2 7 9 3 0 0 1 5 2 5 2 3 3 0 0
52 9 6 1 0 0 5 5 4 4 1 1 0 0 1 7 2 9 6 2 0 0
62 3 2 8 6 0 0 6 4 8 2 9 8 0 0 2 0 2 3 9 5 0 0
73 3 2 1 9 0 0 7 3 9 4 2 0 0 0 1 0 3 3 1 5 5 0 0
82 7 2 8 8 0 0 8 2 8 8 8 0 0 42 7 9 3 0 0
93 1 2 0 3 0 0 9 24 25500 12 31 26600
10 26 28100 10 33 15500 11,13 26 8450
11 25 9400 11 26 400 15 25 23300
12 27 14300 12 31 26600 4 27 9300
13 29 12500 13 26 16500 17 29 6200
14 24 19700 14 34 24200 9,16 24 17700
15 25 10100 15 25 23300
16 43 10700 16 24 9700
17 28 11500 17 29 6200
18 27 10700 18 35 30200
19 28 16300 19 32 17800
Average: 28.5 16426 20 23 9500 Average:
21 32 25900
Average: 33 20724

### Slide 228

A Training Example
Trainees Non-Trainees Matched Sample
unit age earnings unit age earnings unit age earnings
12 8 1 7 7 0 0 1 4 3 2 0 9 0 0 82 8 8 8 0 0
23 4 1 0 2 0 0 2 5 0 3 1 0 0 0 1 4 3 4 2 4 2 0 0
32 9 1 4 4 0 0 3 3 0 2 1 0 0 0 1 7 2 9 6 2 0 0
42 5 2 0 8 0 0 4 2 7 9 3 0 0 1 5 2 5 2 3 3 0 0
52 9 6 1 0 0 5 5 4 4 1 1 0 0 1 7 2 9 6 2 0 0
62 3 2 8 6 0 0 6 4 8 2 9 8 0 0 2 0 2 3 9 5 0 0
73 3 2 1 9 0 0 7 3 9 4 2 0 0 0 1 0 3 3 1 5 5 0 0
82 7 2 8 8 0 0 8 2 8 8 8 0 0 42 7 9 3 0 0
93 1 2 0 3 0 0 9 2 4 2 5 5 0 0 1 2 3 1 2 6 6 0 0
10 26 28100 10 33 15500 11,13 26 8450
11 25 9400 11 26 400 15 25 23300
12 27 14300 12 31 26600 4 27 9300
13 29 12500 13 26 16500 17 29 6200
14 24 19700 14 34 24200 9,16 24 17700
15 25 10100 15 25 23300 15 25 23300
16 43 10700 16 24 9700
17 28 11500 17 29 6200
18 27 10700 18 35 30200
19 28 16300 19 32 17800
Average: 28.5 16426 20 23 9500 Average:
21 32 25900
Average: 33 20724

### Slide 229

A Training Example
Trainees Non-Trainees Matched Sample
unit age earnings unit age earnings unit age earnings
12 8 1 7 7 0 0 1 43 20900 82 8 8 8 0 0
23 4 1 0 2 0 0 2 5 0 3 1 0 0 0 1 4 3 4 2 4 2 0 0
32 9 1 4 4 0 0 3 3 0 2 1 0 0 0 1 7 2 9 6 2 0 0
42 5 2 0 8 0 0 4 2 7 9 3 0 0 1 5 2 5 2 3 3 0 0
52 9 6 1 0 0 5 5 4 4 1 1 0 0 1 7 2 9 6 2 0 0
62 3 2 8 6 0 0 6 4 8 2 9 8 0 0 2 0 2 3 9 5 0 0
73 3 2 1 9 0 0 7 3 9 4 2 0 0 0 1 0 3 3 1 5 5 0 0
82 7 2 8 8 0 0 8 2 8 8 8 0 0 42 7 9 3 0 0
93 1 2 0 3 0 0 9 2 4 2 5 5 0 0 1 2 3 1 2 6 6 0 0
10 26 28100 10 33 15500 11,13 26 8450
11 25 9400 11 26 400 15 25 23300
12 27 14300 12 31 26600 4 27 9300
13 29 12500 13 26 16500 17 29 6200
14 24 19700 14 34 24200 9,16 24 17700
15 25 10100 15 25 23300 15 25 23300
16 43 10700 16 24 9700 1 43 20900
17 28 11500 17 29 6200
18 27 10700 18 35 30200
19 28 16300 19 32 17800
Average: 28.5 16426 20 23 9500 Average:
21 32 25900
Average: 33 20724

### Slide 230

A Training Example
Trainees Non-Trainees Matched Sample
unit age earnings unit age earnings unit age earnings
12 8 1 7 7 0 0 1 4 3 2 0 9 0 0 82 8 8 8 0 0
23 4 1 0 2 0 0 2 5 0 3 1 0 0 0 1 4 3 4 2 4 2 0 0
32 9 1 4 4 0 0 3 3 0 2 1 0 0 0 1 7 2 9 6 2 0 0
42 5 2 0 8 0 0 4 2 7 9 3 0 0 1 5 2 5 2 3 3 0 0
52 9 6 1 0 0 5 5 4 4 1 1 0 0 1 7 2 9 6 2 0 0
62 3 2 8 6 0 0 6 4 8 2 9 8 0 0 2 0 2 3 9 5 0 0
73 3 2 1 9 0 0 7 3 9 4 2 0 0 0 1 0 3 3 1 5 5 0 0
82 7 2 8 8 0 0 8 28 8800 42 7 9 3 0 0
93 1 2 0 3 0 0 9 2 4 2 5 5 0 0 1 2 3 1 2 6 6 0 0
10 26 28100 10 33 15500 11,13 26 8450
11 25 9400 11 26 400 15 25 23300
12 27 14300 12 31 26600 4 27 9300
13 29 12500 13 26 16500 17 29 6200
14 24 19700 14 34 24200 9,16 24 17700
15 25 10100 15 25 23300 15 25 23300
16 43 10700 16 24 9700 1 43 20900
17 28 11500 17 29 6200 8 28 8800
18 27 10700 18 35 30200
19 28 16300 19 32 17800
Average: 28.5 16426 20 23 9500 Average:
21 32 25900
Average: 33 20724

### Slide 231

A Training Example
Trainees Non-Trainees Matched Sample
unit age earnings unit age earnings unit age earnings
12 8 1 7 7 0 0 1 4 3 2 0 9 0 0 82 8 8 8 0 0
23 4 1 0 2 0 0 2 5 0 3 1 0 0 0 1 4 3 4 2 4 2 0 0
32 9 1 4 4 0 0 3 3 0 2 1 0 0 0 1 7 2 9 6 2 0 0
42 5 2 0 8 0 0 4 27 9300 15 25 23300
52 9 6 1 0 0 5 5 4 4 1 1 0 0 1 7 2 9 6 2 0 0
62 3 2 8 6 0 0 6 4 8 2 9 8 0 0 2 0 2 3 9 5 0 0
73 3 2 1 9 0 0 7 3 9 4 2 0 0 0 1 0 3 3 1 5 5 0 0
82 7 2 8 8 0 0 8 2 8 8 8 0 0 42 7 9 3 0 0
93 1 2 0 3 0 0 9 2 4 2 5 5 0 0 1 2 3 1 2 6 6 0 0
10 26 28100 10 33 15500 11,13 26 8450
11 25 9400 11 26 400 15 25 23300
12 27 14300 12 31 26600 4 27 9300
13 29 12500 13 26 16500 17 29 6200
14 24 19700 14 34 24200 9,16 24 17700
15 25 10100 15 25 23300 15 25 23300
16 43 10700 16 24 9700 1 43 20900
17 28 11500 17 29 6200 8 28 8800
18 27 10700 18 35 30200 4 27 9300
19 28 16300 19 32 17800
Average: 28.5 16426 20 23 9500 Average:
21 32 25900
Average: 33 20724

### Slide 232

A Training Example
Trainees Non-Trainees Matched Sample
unit age earnings unit age earnings unit age earnings
12 8 1 7 7 0 0 1 4 3 2 0 9 0 0 82 8 8 8 0 0
23 4 1 0 2 0 0 2 5 0 3 1 0 0 0 1 4 3 4 2 4 2 0 0
32 9 1 4 4 0 0 3 3 0 2 1 0 0 0 1 7 2 9 6 2 0 0
42 5 2 0 8 0 0 4 2 7 9 3 0 0 1 5 2 5 2 3 3 0 0
52 9 6 1 0 0 5 5 4 4 1 1 0 0 1 7 2 9 6 2 0 0
62 3 2 8 6 0 0 6 4 8 2 9 8 0 0 2 0 2 3 9 5 0 0
73 3 2 1 9 0 0 7 3 9 4 2 0 0 0 1 0 3 3 1 5 5 0 0
82 7 2 8 8 0 0 8 28 8800 42 7 9 3 0 0
93 1 2 0 3 0 0 9 2 4 2 5 5 0 0 1 2 3 1 2 6 6 0 0
10 26 28100 10 33 15500 11,13 26 8450
11 25 9400 11 26 400 15 25 23300
12 27 14300 12 31 26600 4 27 9300
13 29 12500 13 26 16500 17 29 6200
14 24 19700 14 34 24200 9,16 24 17700
15 25 10100 15 25 23300 15 25 23300
16 43 10700 16 24 9700 1 43 20900
17 28 11500 17 29 6200 8 28 8800
18 27 10700 18 35 30200 4 27 9300
19 28 16300 19 32 17800 8 28 8800
Average: 28.5 16426 20 23 9500 Average:
21 32 25900
Average: 33 20724

### Slide 233

A Training Example
Trainees Non-Trainees Matched Sample
unit age earnings unit age earnings unit age earnings
12 8 1 7 7 0 0 1 4 3 2 0 9 0 0 82 8 8 8 0 0
23 4 1 0 2 0 0 2 5 0 3 1 0 0 0 1 4 3 4 2 4 2 0 0
32 9 1 4 4 0 0 3 3 0 2 1 0 0 0 1 7 2 9 6 2 0 0
42 5 2 0 8 0 0 4 2 7 9 3 0 0 1 5 2 5 2 3 3 0 0
52 9 6 1 0 0 5 5 4 4 1 1 0 0 1 7 2 9 6 2 0 0
62 3 2 8 6 0 0 6 4 8 2 9 8 0 0 2 0 2 3 9 5 0 0
73 3 2 1 9 0 0 7 3 9 4 2 0 0 0 1 0 3 3 1 5 5 0 0
82 7 2 8 8 0 0 8 2 8 8 8 0 0 42 7 9 3 0 0
93 1 2 0 3 0 0 9 2 4 2 5 5 0 0 1 2 3 1 2 6 6 0 0
10 26 28100 10 33 15500 11,13 26 8450
11 25 9400 11 26 400 15 25 23300
12 27 14300 12 31 26600 4 27 9300
13 29 12500 13 26 16500 17 29 6200
14 24 19700 14 34 24200 9,16 24 17700
15 25 10100 15 25 23300 15 25 23300
16 43 10700 16 24 9700 1 43 20900
17 28 11500 17 29 6200 8 28 8800
18 27 10700 18 35 30200 4 27 9300
19 28 16300 19 32 17800 8 28 8800
Average: 28.5 16426 20 23 9500 Average: 28.5 13982
21 32 25900
Average: 33 20724

### Slide 234

Age Distribution: Before Matching
0 1 2 30 1 2 3
20 30 40 50 60
A: Trainees
B: Non−Trainees
frequency
age
Graphs by group

### Slide 235

Age Distribution: After Matching
0 1 2 30 1 2 3
20 30 40 50 60
A: Trainees
B: Non−Trainees
frequency
age
Graphs by group

### Slide 236

Training E↵ect Estimates
Di↵erence in average earnings between trainees and non-trainees
Before matching
16426   20724 =  4298
After matching:
16426   13982 = 2444

### Slide 237

Alternative distance metric: Euclidean distance
When the vector of matching covariates, X =


X1
X2
...
Xk

 has more
than one dimension ( k > 1) we will need a new deﬁnition of
distance to measure “closeness”.
Deﬁnition: Euclidean distance
||Xi − Xj || =
√
(Xi − Xj)′(Xi − Xj)
=
√
k∑
n=1
(Xni − Xnj)2
Comment: The Euclidean distance is not invariant to changes in
the scale of the X ’s. For this reason, alternative distance metrics
that are invariant to changes in scale are used

### Slide 238

Normalized Euclidean distance
Deﬁnition: Normalized Euclidean distance
A commonly used distance is the normalized Euclidean distance:
||Xi −Xj||=
√
(Xi −Xj)′ ˆV−1(Xi −Xj)
where
ˆV−1 =


ˆσ21 0 . . . 0
0 ˆσ22 . . . 0
... ... ... ...
0 0 . . . ˆσ2k


Notice that the normalized Euclidean distance is equal to:
||Xi −Xj||=
√
k∑
n=1
(Xni −Xnj)
ˆσ2n
Thus, if there are changes in the scale ofXni, these changes also aﬀectˆσ2n, and
the normalized Euclidean distance does not change

### Slide 239

Mahalanobis distance
Deﬁnition: Mahalanobis distance
The Mahalanobis distance is the scale-invariant distance metric:
||Xi − Xj ||=
√
(Xi − Xj)′ ˆΣ−1
X (Xi − Xj)
where ˆΣX is the sample variance-covariance matrix of X .
Comments: Related, you can create arbitrary distances like:
||Xi − Xj ||=
√
k∑
n=1
ωn · (Xni − Xnj)2
(with all ωn ≥ 0) so that we assign large ωn’s to those covariates
that we want to match particularly well.

### Slide 240

Matching and the Curse of Dimensionality
Dimensionality creates headaches for us in matching.
Bad news: Matching discrepancies ||Xi − Xj(i)|| tend to
increase with k, the dimension of X
Good news: Matching discrepancies converge to zero . . .
Bad news: . . . but they converge very slow if k is large
Good news?: Mathematically, it can be shown that
||Xi − Xj(i)|| converges to zero at the same rate as 1
N
1
k
Bad news: It’s hard to ﬁnd good matches when X has a large
dimension: you need many observations if k is big.

### Slide 241

Deriving the matching bias
Derive the matching bias by ﬁrst writing out the sample ATT
estimate:
ˆδATT = 1
NT
∑
Di =1
(Yi − Yj(i)),
where each i and j(i) units are matched, Xi ≈ Xj(i) and Dj(i) = 0.
Deﬁne potential outcomes and switching eq.
µ0(x) = E [Y |X = x, D = 0] = E [Y 0|X = x],
µ1(x) = E [Y |X = x, D = 1] = E [Y 1|X = x],
Yi = µDi (Xi) + εi
Substitute and distribute terms
ˆδATT = 1
NT
∑
Di =1
[
(µ1(Xi) + εi) − (µ0(Xj(i)) + εj(i))
]
= 1
NT
∑
Di =1
(µ1(Xi) − µ0(Xj(i))) + 1
NT
∑
Di =1
(εi − εj(i))

### Slide 242

Deriving the matching bias
Diﬀerence between sample estimate and population parameter is:
ˆδATT − δATT = 1
NT
∑
Di =1
(
µ1(Xi) − µ0(Xj(i)) − δATT
)
+ 1
NT
∑
Di =1
(εi − εj(i))
Algebraic manipulation and simpliﬁcation:
ˆδATT − δATT = 1
NT
∑
Di =1
(
µ1(Xi) − µ0(Xi) − δATT
)
+ 1
NT
∑
Di =1
(εi − εj(i))
+ 1
NT
∑
Di =1
(
µ0(Xi) − µ0(Xj(i))
)
.

### Slide 243

Deriving the matching bias
Apply the Central Limit Theorem and the diﬀerence,
√
1
N (ˆδATT − δATT),
converges to a Normal distribution with zero mean. But, however,
E [
√
1
N (ˆδATT − δATT)] = E [
√
1
N (µ0(Xi) − µ0(Xj(i)))|D = 1].
Now consider the implications if k is large:
The diﬀerence between Xi and Xj(i) converges to zero very
slowly
The diﬀerence µ0(Xi) − µ0(Xj(i)) converges to zero very slowly
E [
√
1
N (µ0(Xi) − µ0(Xj(i)))|D = 1] may not converge to zero
and an be very large!
E [
√
1
N (ˆδATT − δATT)] may not converge to zero because the
bias of the matching discrepancy is dominating the matching
estimator!
Bias is often an issue when we match in many dimensions

### Slide 244

Solutions to matching bias problem
The bias of the matching estimator is caused by large matching
discrepancies ||Xi − Xj(i)||. The curse of dimensionality virtually
guarantees this. However:
1 But the matching discrepancies are observed. We can always
check in the data how well we’re matching the covariates.
2 For ˆδATT we can always make the matching discrepancies
small by using a large reservoir of untreated units to select the
matches (that is, by making NC large).
3 If the matching discrepancies are large, so we are worried
about potential biases, we can apply bias correction techniques
4 Partial solution: propensity score methods (coming soon. . . )

### Slide 245

Matching with bias correction
Each treated observation contributes
µ0(Xi) − µ0(Xj(i))
to the bias.
Bias-corrected (BC) matching:
ˆδBC
ATT = 1
NT
∑
Di =1
[
(Yi − Yj(i)) − (ˆµ0(Xi) − ˆµ0(Xj(i)))
]
where ˆµ0(X ) is an estimate of E [Y |X = x, D = 0]. For
example using OLS.
Under some conditions, the bias correction eliminates the bias
of the matching estimator without aﬀecting the variance.

### Slide 246

Bias adjustment in matched data
Potential Outcome
unit under Treatment under Control
i Y 1
i Y 0
i Di Xi
1 10 8 1 3
2 4 1 1 1
3 10 9 1 10
4 8 0 4
5 1 0 0
6 9 0 8
ˆδATT = 10 − 8
3 + 4 − 1
3 + 10 − 9
3 = 2

### Slide 247

Bias adjustment in matched data
Potential Outcome
unit under Treatment under Control
i Y 1
i Y 0
i Di Xi
1 10 8 1 3
2 4 1 1 1
3 10 9 1 10
4 8 0 4
5 1 0 0
6 9 0 8
ˆδATT = 10 − 8
3 + 4 − 1
3 + 10 − 9
3 = 2
For the bias correction, estimate ˆµ0(X ) = ˆβ0 + ˆβ1X = 2 + X

### Slide 248

Bias adjustment in matched data
Potential Outcome
unit under Treatment under Control
i Y1i Y0i Di Xi
1 10 8 1 3
2 4 1 1 1
3 10 9 1 10
4 8 0 4
5 1 0 0
6 9 0 8
ˆδATT = 10−8
3 + 4 −1
3 + 10−9
3 = 2
For the bias correction, estimateˆµ0(X) = ˆβ0 + ˆβ1X = 2 +X
ˆδATT = (10−8)−(ˆµ0(3)− ˆµ0(4))
3 + (4−1)−(ˆµ0(1)− ˆµ0(0))
3
+ (10−9)−(ˆµ0(10)− ˆµ0(8))
3 = 1.33

### Slide 249

Matching bias: Implications for practice
Bias arises because of the eﬀect of large matching discrepancies on
µ0(Xi) − µ0(Xj(i)). To minimize matching discrepancies:
1 Use a small M (e.g., M = 1). Larger values of M produce
large matching discrepancies.
2 Use matching with replacement. Because matching with
replacement can use untreated units as a match more than
once, matching with replacement produces smaller matching
discrepancies than matching without replacement.
3 Try to match covariates with a large eﬀect on µ0(·)
particularly well.

### Slide 250

Large sample distribution for matching estimators
Matching estimators have a Normal distribution in large
samples (provided the bias is small):
√
NT (ˆδATT − δATT) d− →N(0, σ2
ATT)
For matching without replacement, the “usual” variance
estimator:
ˆσ2
ATT = 1
NT
∑
Di =1
(
Yi − 1
M
M∑
m=1
Yjm(i) − ˆδATT
)2
,
is valid.

### Slide 251

Large sample distribution for matching estimators
For matching with replacement:
ˆσ2
ATT = 1
NT
∑
Di =1
(
Yi − 1
M
M∑
m=1
Yjm(i) − ˆδATT
)2
+ 1
NT
∑
Di =0
(Ki(Ki − 1)
M2
)
ˆvar(ε|Xi , Di = 0)
where Ki is the number of times observation i is used as a
match.
ˆvar(Yi |Xi , Di = 0) can be estimated also by matching. For
example, take two observations with Di = Dj = 0 and
Xi ≈ Xj, then
ˆvar(Yi |Xi , Di = 0) = (Yi − Yj)2
2
is an unbiased estimator of ˆvar(εi |Xi , Di = 0))
The bootstrap doesn’t work!

### Slide 252

Propensity score
Overview
What do we use a propensity score for?
How do we construct the propensity score?
How do we implement propensity score estimation in STATA?
Discuss several articles using propensity score matching

### Slide 253

Joke (sort of. . . )
Two heart surgeons walk into a bar.
Jack: “I just ﬁnished my 100th heart surgery!”
Jill: “I ﬁnished my 100th heart surgery last week. So I have
more experience than you, and am therefore probably a better
surgeon. How many of your patients died within 3 months of
surgery? I’ve only had 10 die.”
Jack: “Five. Which means I’m probably the better surgery.”
Jill: “Or maybe my patients are older and have a higher risk of
mortality than your patients.”
I didn’t say it was a good joke.
They are debating about who is the better surgeon – the
treatment of interest. But if the patients’ characteristics diﬀer
between Jack and Jill, and those characteristics are correlated
with mortality, then it confounds the inference
To really judge who is the better surgeon, we want them to
each see the same groups of patients of equivalent
characteristics

### Slide 254

Purpose of propensity scores
When treatment is non-random, you can use propensity score
matching to compare a treatment and control group who are
equivalent on observable characteristics
Propensity score matching also provides a way to summarize
covariate information about treatment selection into a single
scalar
Can be used to adjust for diﬀerences via study design, or
matching, or during estimation of the treatment eﬀect (e.g.,
subclassiﬁcation or regression)

### Slide 255

Some caveats
This is only relevant for selection on observables
If you cannot write down a DAG such that conditioning on
some covariate(s) meets the backdoor criterion, then
propensity score matching is not the appropriate methodology
You need to exhaustively identify the variables, X , that will
block all back door paths between the treatment, D, and the
outcome, Y using your knowledge of theory and the
institutional details

### Slide 256

Motivation
If the treatment is randomly assigned, then we use the simple
diﬀerence in outcomes (SDO) estimator
1
NT
∑
(Y |D = 1) − 1
NC
∑
(Y |D = 0)
This is called “ignobility” when we have randomized
treatment assignment
But if what if ignobility is violated?
Propensity score matching has a weaker assumption called
conditional independence which we will discuss later

### Slide 257

OLS
In principle you could estimate the treatment eﬀect using
regression and control for X
Y = δD + βX + ε
where X is a matrix of covariates that you think aﬀect the
probability of receiving a scholarship
OLS consistently estimates the conditional mean, but if
probability of treatment is nonlinear, this conditional mean
may be less informative
Usually, we won’t know how selection depended on X – only
that it did (e.g., DAG and the backdoor criterion)

### Slide 258

Propensity score
The idea with matching on X ’s was to compare units who
were ”close” to one another based on some distance to the
nearest X neighbor. But we had some issue with matching
discrepancies and the the curse of dimensionality.
The idea with propensity score matching is to compare units
who, based solely on their observables, had very similar
probabilities of being placed into treatment
If conditional on X , two units have a similar probability of
treatment, then we say they have similar propensity scores
If two units have equivalent propensity scores, then diﬀerences
between their outcomes is attributable to the treatment
If we compare a unit in the treatment group to a control
group unit with two similar propensity scores, then conditional
on the propensity score, all remaining variation between these
two is randomness—if selection on observables is a correct
assumption.

### Slide 259

First: How do we practically do this?
Estimation using propensity score matching is a two-step
procedure
1 First step: estimate the propensity score
2 Second step: calculate the average causal eﬀect of interest by
averaging diﬀerences in outcomes over units with similar
propensity scores
Estimating the propensity score
Estimate the following equation with binary treatment D on
the left-hand-side and observables X related to selection
determination (e.g., backdoor criterion) on the right-hand-side
using probit or logit model
Prob(D = 1|X ) = γX + ω
Second using the estimated coeﬃcients, calculate the
predicted probability of treatment
ˆρ = ˆγX
The propensity score is just the predicted conditional
probability of treatment, or the ﬁtted value for each unit

### Slide 260

Deﬁnition
Deﬁnition of Propensity score
Propensity score is deﬁned as the selection probability conditional
on the confounding variables: p(X ) = Pr(D = 1|X )
Identiﬁcation Assumptions:
1 (Y 0, Y 1) ⊥ ⊥D|X (conditional independence)
2 0 < Pr(D = 1|X ) < 1 (common support)

### Slide 261

Identifying assumption I: Conditional independence (Rosenbaum and Rubin 1983)
(Y0i , Y1i ) ⊥ ⊥D|Xi. There exists a setX of observable covariates such that after
controlling for these covariates, treatment assignment is independent of potential
outcomes.
Interpretation: After controlling forX, the assignment of units to the
treatment is ‘as good as random’.
Why?: Conditional independence allows us the ability to construct an unbiased
counterfactual for the treatment group using the non-experimental control
group units.
Also called ‘ignorable treatment assignment’ or ‘unconfoundedness’ (statistics),
backdoor criterion (Pearl), ‘selection on observables’, or ‘exogeneity’
(economics). Exogeneity in traditional econometric pedagogy
Y0i = α+β′Xi +ε, and
Y1i = Y0i +δ
then we can write:
Yi =α+δDi +β′Xi +εi
and conditional independence⇐ ⇒εi ⊥ ⊥Di|Xi (exogeneity)
Conditional independence isnot testable

### Slide 262

Identifying assumption II: Common support
For each value of X , there is a positive probability of being both
treated and untreated
0 < Pr(Di = 1|Xi) < 1
Interpretation: Implies that the probability of receiving
treatment for every value of the vector X is strictly within the
unit interval: as is the probability of not receiving treatment.
Why?: Common support ensures there is suﬃcient overlap in
the characteristics of treated and untreated units to ﬁnd
adequate matches
Common support is testable

### Slide 263

Identiﬁcation given strong ignobility
When both assumptions are satisﬁed, the treatment
assignment is said to be strongly ignorable in the
terminology of Rosenbaum and Rubin (1983).
By deﬁnition
δi(Xi) = E [Y 1
i − Y 0
i |Xi = x]
= E [Y 1
i |Xi = x] − E [Y 0
i |Xi = x]
Unconfoundedness allow us to substitute
E [Y 1
i |Di = 1, Xi = x] = E [Yi |Di = 1, Xi = x]
and similar for other term.
Common support allows us to estimate both terms
Then, δ = E [δ(Xi)]

### Slide 264

Propensity score theorem (Rosenbaum and Rubin 1983)
If (Y 1, Y 0) ⊥ ⊥D|X (conditional independence), then
(Y 1, Y 0) ⊥ ⊥D|p(X ) where p(X ) = Pr(D = 1|X ), the propensity
score
Suﬃcient conditions: Conditioning on the propensity score is
enough to have independence between the treatment indicator, D,
and the potential outcomes, ( Y 1, Y 0)
Dimension reduction: Extremely valuable theorem. Here’s why
Stratifying on X tends to run into sparseness-related problems
(i.e., empty cells) in ﬁnite samples for even moderate number
of covariates
But the propensity score, p(X ), is just a scalar, and therefore
stratifying across that probability is going to be a lot easier.
Proof: Straightforward application of the law of iterated
expectations (LIE) with nested conditioning from MHE (pp. 80-81)

### Slide 265

Proof
If we can show that the probability an individual receives
treatment conditional on potential outcomes and the
propensity score is not a function of potential outcomes
Pr(D = 1|Y 0, Y 1, p(X )) ̸= f (Y 0, Y 1)
then we will have proven that there is an independence
between Y 0, Y 1 and D conditional on X
Before diving into the proof, ﬁrst recognize that
Pr(D = 1|Y 0, Y 1, p(X )) = E [D|Y 0, Y 1, p(X )]
because
E [D|Y 0, Y 1, p(X )] = 1 · Pr(D = 1|Y 0, Y 1, p(X ))
+0 · Pr(D = 0|Y 0, Y 1, p(X ))
and the second term cancels out.

### Slide 266

Proof.
Assume that (Y1,Y0)⊥ ⊥D|X. Then:
Pr(D= 1|Y1,Y0,p(X)) = E[D|Y1,Y0,p(X)]  
See previous slide
= E[E[D|Y1,Y0,p(X),X]|Y1,Y0,p(X)]  
by LIE
= E[E[D|Y1,Y0,X]|Y1,Y0,p(X)]  
GivenX, we knowp(X)
= E[E[D|X]|Y1,Y0,p(X)]  
by conditional independence
= E[p(X)|Y1,Y0,p(X)]  
propensity score deﬁnition
= p(X)
Using a similar argument, we obtain:
Pr(D= 1|p(X)) = E[D|p(X)]  
Previous slide
=E[E[D|X]|p(X)]  
LIE
= E[p(X)|p(X)]  
deﬁnition
=p(X)
andPr(D= 1|Y1,Y0,p(X)) =Pr(D= 1|p(X)) by conditional independence

### Slide 267

Propensity score theorem
Like the omitted variable bias formula for regression, the
propensity score theorem says that you need only control for
covariates that aﬀect the probability of treatment
But it also says something more
The only covariate you really need to control for is the
probability of treatment itself
Direct propensity score matching works in the same way as
covariate matching (e.g., nearest neighbor matching) except
that we match on the score instead of the covariates directly

### Slide 268

Corollary
If (Y 1, Y 0) ⊥ ⊥D|X , we can estimate average treatment eﬀects:
E [Y 1 − Y 0|p(X )] = E [Y |D = 1, p(X )] − E [Y |D = 0, p(X )]
Three-step procedure:
1 Estimate the conditional probability of treatment, or
propensity score, p(X ) = Pr(D = 1|X ), using any standard
probability model (logit or probit)
2 Do matching, sub classiﬁcation (stratiﬁcation), inverse
probability weighting or some other algorithmic procedures to
estimate the average causal eﬀect conditional on the
estimated propensity score
3 Compute standard errors

### Slide 269

Balancing property
Because the propensity score is a function of X, we know:
Pr(D = 1|X , p(X )) = Pr(D = 1|X )
= p(X )
∴ conditional on p(X ), the probability that D = 1 does not
depend on X .
D and X are independent conditional on p(X ):
D ⊥ ⊥X |p(X )
So we obtain the balancing property of the propensity score:
Pr(X |D = 1, p(X )) = Pr(X |D = 0, p(X ))
conditional on the property score, the distribution of the
covariates is the same for treatment and control group units
We can use this to check if our estimated propensity score
actually produces balance:
Pr(X |D = 1, ˆp(X )) = Pr(X |D = 0, ˆp(X ))

### Slide 270

Balancing Property
D
p(X )
X
Y

### Slide 271

Inverse Probability Weighting
Proposition
If Y1,Y0 ⊥ ⊥D|X, then
δATE = E[Y1 −Y0] =E
[
Y · D−p(X)
p(X) · (1−p(X))
]
δATT = E[Y1 −Y0|D = 1] = 1
Pr(D = 1)· E
[
Y · D−p(X)
1−p(X)
]
Proof.
E
[
Y D−p(X)
p(X)(1−p(X))
⏐⏐⏐X
]
= E
[ Y
p(X)
⏐⏐⏐X,D = 1
]
p(X)
+E
[ −Y
1−p(X)
⏐⏐⏐X,D = 0
]
(1−p(X))
= E[Y|X,D = 1]−E[Y|X,D = 0]
and the results follow from integrating overP(X) andP(X|D = 1).

### Slide 272

Weighting on the propensity score
ATE and ATT parameters (previous slide):
δATE = E[Y1 −Y0] =E
[
Y · D−p(X)
p(X) · (1−p(X))
]
δATT = E[Y1 −Y0|D = 1] = 1
Pr(D = 1)· E
[
Y · D−p(X)
1−p(X)
]
The analogy principle suggests a two-step estimator:
1 Estimate the propensity score:ˆp(X)
2 Use estimated score to produce analog estimators. LetˆδATEandˆδATTbe an
estimate of theATEandATTparameter:
ˆδATE = 1
N
N∑
i=1
Yi · Di −ˆp(Xi)
ˆp(Xi) · (1−ˆp(Xi))
ˆδATT = 1
NT
N∑
i=1
Yi · Di −ˆp(Xi)
1−ˆp(Xi)

### Slide 273

Weighting on the propensity score
Sample analogs of the population parameters (previous slide):
ˆδATE = 1
N
N∑
i=1
Yi · Di − ˆp(Xi)
ˆp(Xi) · (1 − ˆp(Xi))
ˆδATT = 1
NT
N∑
i=1
Yi · Di − ˆp(Xi)
1 − ˆp(Xi)
Standard errors:
We need to adjust the standard errors for ﬁrst-step estimation
of p(X )
Parameteric ﬁrst step: Newey and McFadden (1994)
Non-parametric ﬁrst step: Newey (1994)
Or bootstrap the entire two-step procedure.

### Slide 274

Other algorithmic methods
1 Estimate propensity score
2 Choose algorithm
3 Check overlap/common support
4 Matching Quality/Eﬀect estimation
5 Sensitivity Analysis

### Slide 275

Estimation
Once you have determined the propensity score with the
procedure above, there are several ways to use it to estimate
average treatment eﬀects
1 Stratiﬁcation. Run the second step within each group.
Calculate the weighted mean of the within-group estimates to
get the average treatment eﬀect
2 Matching. Match each treatment observation to its nearest
neighbor(s) based on similar propensity scores.

### Slide 276

Stratiﬁcation: Achieving Balance
Stratiﬁcation requires imposing balance by grouping the data
and testing for diﬀerences in covariate means
1 Sort the data by propensity score and divide into groups of
observations with similar propensity scores (e.g., percentiles)
2 Within each group, test (using a t-test) whether the means of
the covariates ( X ) are equal between treatment and control
3 If so, then stop. If not, it means the covariates aren’t balanced
within that group . Divide the group in half and repeat
4 If a particular covariate is unbalanced for multiple groups,
modify the initial logit or probit equation by including higher
order terms and/or interactions with that covariate and repeat
This is done to satisfy the balancing property.
Notice: we never even look at outcomes in this step

### Slide 277

Matching strategy and ATT estimation
The standard matching strategy is the following:
Pair each treatment unit i with one or more comparable
control group unit j, where comparability is in terms of
proximity to the estimated propensity score
Associate to the treatment unit’s outcome Yi a matched
outcome of Yi(j) given by the weighted outcomes of its
neighbors in the control group
Yi(j) =
∑
j∈C(i)
wij Yj
where
C (i) is the set of neighbors with W = 0 of the treatment unit i
and wij is the weight of control group units j with∑
j∈C(i) wij = 1

### Slide 278

Estimation (cont.)
The ATT equals
E [Y 1
i |Di = 1] − E [Y 0
i |Di = 1]
We estimate it as follows
ˆATT = 1
NT
=
∑
i:Wi =1
[
Yi − Yi(j)
]
where NT is the number of matched treatment units in the sample.

### Slide 279

Matching methods
An estimate of the propensity score is not enough to estimate
the ATT
In fact, the probability of observing two units with exactly the
same propensity score is in principle zero because p(x) is
continuous
Several matching methods have been proposed in the
literature, but the most widely used are:
Nearest-neighbor matching (with or without caliper)
Radius matching
Kernel matching
Stratiﬁcation matching
Typically, one treatment unit i is matched to several control
units j, but sometimes one-to-one matching is used

### Slide 280

STATA
The STATA command psmatch2 will perform propensity
score matching
Rich suite of options: nearest neighbor (with or without
caliper, with or without replacement), k-nearest neighbors,
radius, kernel, local linear regression, and Mahalanobis
matching
It includes routes for common support graphing ( psgraph)
and covariate imbalance testing ( pstest)
But standard errors are incorrect. They use bootstrapping
methods or variance approximation.
Has an excellent help ﬁle
Type in ssc install psmatch2

### Slide 281

STATA
Another useful command in STATA is pscore
Various matching methods for estimating ATT
Standard errors are still incorrect: bootstrapping, and variance
approximation
Balancing tests based on stratiﬁcation
Have to use net search pscore to install it.

### Slide 282

STATA
As of STATA 14, built-in suite of options called teffects
Fewer options. No sampling without replacement
Standard errors are correct
I’m less familiar with it

### Slide 283

Nearest Neighbor
Pretty similar to covariate matching. Formula is
ATTNN = 1
NT
∑
i:Wi =1
[
Yi −
∑
j∈C(i)M
wij Yj
]
NT is the number of Treatment units i
wij is equal to 1
NC
if j is a control unit and zero otherwise; NC
is number of control units j

### Slide 284

NN Matching: Bias vs. Variance
How many nearest neighbors should we use?
Matching just one nearest neighbor minimizes bias at the cost
of larger variance
Matching using additional nearest neighbors increases the bias
but decreases the variance
Matching with or without replacement
with replacement keeps bias low at the cost of larger variance
without replacement keeps variance low but at the cost of
potential bias

### Slide 285

Caliper matching
Caliper matching is a variation on NN matching that tries to
build into the algorithm stops against bad matches
It does this by imposing a tolerable maximum distance (e.g.,
0.2 units in the propensity score away from a treatment unit
i’s propensity score)
If there doesn’t exist any control group unit j within that
“caliper”, then treatment unit i is discarded
It’s diﬃcult to know what this caliper should be ex ante

### Slide 286

Radius matching
Each treatment unit i is matched only with the control group unit
j whose propensity score falls into a predeﬁned neighborhood of
the propensity score of the treatment unit.
All the control units with ˆρj falling within a radius r from ˆρi
are matched to the treatment unit i
The smaller the radius, the better the quality of the matches,
but the higher the possibility some treatment units are not
matched because the neighborhood does not contain control
group units j

### Slide 287

Checking the common support assumption
We can summarize the propensity scores in the treatment and
control group and count how many units are oﬀ-support
For instance, let’s say that the treatment group propensity
scores range from [0 .1, 0.85]. That means the min and max
scores are in that range.
We could check what the distribution is for the control group,
and drop all the units before 0 .1 and above 0 .85. In other
words, we’re throwing away observations that don’t satisfy the
common support assumption.
A histogram of propensity scores by treatment and control
group also highlights the overlap problem

### Slide 288

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Stratiﬁcation
Matching
Propensity score matching
Regression discontinuity designs
Examples of propensity score matching
Example: Job Trainings Program (NSW) (LaLonde 1986,
Dehejia and Wahba 1999; 2002)
Cunningham Causal Inference

### Slide 289

Description of NSW Job Trainings Program
The National Supported Work Demonstration (NSW), operated by
Manpower Demonstration Research Corp in the mid-1970s:
was a temporary employment program designed to help
disadvantaged workers lacking basic job skills move into the
labor market by giving them work experience and counseling
in a sheltered environment
was also unique in that it randomly assigned qualiﬁed
applicants to training positions:
Treatment group: received all the beneﬁts of NSW program
Control group: left to fend for themselves
admitted AFDC females, ex-drug addicts, ex-criminal
oﬀenders, and high school dropouts of both sexes

### Slide 290

NSW Program
Treatment group members were:
guaranteed a job for 9-18 months depending on the target
group and site
divided into crews of 3-5 participants who worked together and
met frequently with an NSW counselor to discuss grievances
and performance
paid for their work
Other details about the NSW program:
Wages: NSW oﬀered the trainees lower wage rates than they
would’ve received on a regular job, but allowed their earnings
to increase for satisfactory performance and attendance
Post-treatment: after their term expired, they were forced to
ﬁnd regular employment
Job types: varied within sites – gas station attendant, working
at a printer shop – and males and females were frequently
performing diﬀerent kinds of work

### Slide 291

NSW Data
NSW data collection :
MDRC collected earnings and demographic information from
both treatment and control at baseline and every 9 months
thereafter
Conducted up to 4 post-baseline interviews
Diﬀerent sample sizes from study to study can be confusing,
but has simple explanations
Estimation:
NSW was a randomized job trainings program; therefore
estimating the average treatment eﬀect is straightforward:
1
Nt
∑
Di =1
Yi − 1
Nc
∑
Di =0
Yi ≈ E [Y 1 − Y 0]
in large samples assuming treatment selection is independent
of potential outcomes (randomization) – i.e., ( Y 0, Y 1) ⊥ ⊥D.
NSW worked: Treatment group participants’ real earnings
post-treatment (1978) was positive and economically
meaningful – ≈ $900 (LaLonde 1986) to $1,800 (Dehejia and
Wahba 2002) depending on the sample used

### Slide 292

LaLonde, Robert J. (1986). “Evaluating the Econometric
Evaluations of Training Programs with Experimental Data”.
American Economic Review .
LaLonde’s study: was not an evaluation of the NSW program,
but rather an evaluation of econometric models done by:
replacing the experimental NSW control group with
non-experimental control group drawn from two public use
datasets: Current Population Survey (CPS) and Panel Study
of Income Dynamics (PSID)
estimating the average eﬀect using non-experimental workers
as controls for the NSW trainees
comparing his non-experimental estimates to the experimental
estimates of $900
LaLonde’s conclusion: available econometric approaches were
biased and inconsistent
His estimates were way oﬀ and usually the wrong sign
Conclusion was inﬂuential in policy circles and led to greater
push for more experimental evaluations

### Slide 293

[No extractable text in PDF text layer; page may contain an image or diagram.]

### Slide 294

[No extractable text in PDF text layer; page may contain an image or diagram.]

### Slide 295

Imbalanced covariates for experimental and non-experimental
samples
CPS NSW
All Controls Trainees
Nc = 15, 992 Nt = 297
covariate mean (s.d.) mean mean t-stat diﬀ
Black 0.09 0.28 0.07 0.80 47.04 -0.73
Hispanic 0.07 0.26 0.07 0.94 1.47 -0.02
Age 33.07 11.04 33.2 24.63 13.37 8.6
Married 0.70 0.46 0.71 0.17 20.54 0.54
No degree 0.30 0.46 0.30 0.73 16.27 -0.43
Education 12.0 2.86 12.03 10.38 9.85 1.65
1975 Earnings 13.51 9.31 13.65 3.1 19.63 10.6
1975 Unemp 0.11 0.32 0.11 0.37 14.29 -0.26

### Slide 296

Dehija and Wahba (1999)
Dehejia and Wahba (DW) update LaLonde’s original study
using propensity score matching
1 Dehejia, Rajeev H. and Sadek Wahba (1999). “Causal Eﬀects
in Nonexperimental Studies: Reevaluating the Evaluation of
Training Programs”. Journal of the American Statistical
Association, vol. 94(448): 1053-1062 (pdf)
Can propensity score matching improve over the estimators
that LaLonde examined?

### Slide 297

'HKHMLDDQG:DKED&DXVDO(IIHFWVLQ1RQH[SHULPHQWDO6WXGLHV 
7DEOH6DPSOH0HDQVRI&KDUDFWHULVWLFVIRU16:DQG&RPSDULVRQ6DPSOHV
1RRIREVHUYDWLRQV$JH(GXFDWLRQ%ODFN+LVSDQLF1RGHJUHH0DUULHG5(865(86
16:/DORQGHD
7UHDWHG      
 
&RQWURO      
 
5(VXEVHWE
7UHDWHG       
 
&RQWURO       
 
&RPSDULVRQJURXSVF
36,'       
>@>@>@>@>@>@>@ >@
36,'       
>@>@>@>@>@>@>@ >@
36,'       
>@>@>@>@>@>@ >@
&36       
>@>@>@>@>@>@>@ >@
&36       
>@>@>@>@>@>@>@ >@
&36       
>@>@>@>@>@>@>@ >@
127(6WDQGDUGHUURUVDUHLQSDUHQWKHVHV6WDQGDUGHUURURQGLIIHUHQFHLQPHDQVZLWK5(VXEVHWWUHDWHGLVJLYHQLQEUDFNHWV$JH DJHLQ\HDUV(GXFDWLRQ QXPEHURI\HDUVRIVFKRROLQJ
%ODFN LIEODFNRWKHUZLVH+LVSDQLF LI+LVSDQLFRWKHUZLVH1RGHJUHH LIQRKLJKVFKRROGHJUHHRWKHUZLVH0DUULHG LIPDUULHGRWKHUZLVH5([ HDUQLQJVLQFDOHQGDU\HDU
[
D16:VDPSOHDVFRQVWUXFWHGE\/DORQGH
E7KHVXEVHWRIWKH/DORQGHVDPSOHIRUZKLFK5(LVDYDLODEOH
F'HILQLWLRQRIFRPSDULVRQJURXSV/DORQGH
36,'$OOPDOHKRXVHKROGKHDGVXQGHUDJHZKRGLGQRWFODVVLI\WKHPVHOYHVDVUHWLUHGLQ
36,'6HOHFWVIURP36,'DOOPHQZKRZHUHQRWZRUNLQJZKHQVXUYH\HGLQWKHVSULQJRI
36,'6HOHFWVIURP36,'DOOPHQZKRZHUHQRWZRUNLQJLQ
&36$OO&36PDOHVXQGHUDJH
&366HOHFWVIURP&36DOOPDOHVZKRZHUHQRWZRUNLQJZKHQVXUYH\HGLQ0DUFK
&366HOHFWVIURP&36DOOWKHXQHPSOR\HGPDOHVLQZKRVHLQFRPHLQZDVEHORZWKHSRYHUW\OHYHO
36,'DQG&36DUHLGHQWLFDOWRWKRVHXVHGE\/DORQGH&36DUHVLPLODUWRWKRVHXVHGE\/DORQGHEXW/DORQGH
VRULJLQDOVXEVHWFRXOGQRWEHUHFUHDWHG
RIWKHWUHDWPHQWLPSDFW%HFDXVHRXUDQDO\VLVIRFXVHVRQ
WKHLPSRUWDQFHRISUHLQWHUYHQWLRQYDULDEOHVZHIRFXVRQ
WKHILUVWRIWKHVH
7DEOHSDQHO$UHSURGXFHVWKHUHVXOWVRI/DORQGH
7DEOH&RPSDULQJ3DQHOV$DQG%ZHQRWHWKDWWKHWUHDW
PHQWHIIHFWDVHVWLPDWHGIURPWKHUDQGRPL]HGH[SHULPHQW
LVKLJKHULQWKHODWWHUFRPSDUHGWR7KLVUH
IOHFWVGLIIHUHQFHVLQWKHFRPSRVLWLRQRIWKHWZRVDPSOHVDV
GLVFXVVHGLQWKHSUHYLRXVVHFWLRQ$KLJKHUWUHDWPHQWHI
IHFWLVREWDLQHGIRUWKRVHZKRMRLQHGWKHSURJUDPHDUOLHURU
ZKRZHUHXQHPSOR\HGSULRUWRSURJUDPSDUWLFLSDWLRQ7KH
UHVXOWVLQWHUPVRIWKHVXFFHVVRIQRQH[SHULPHQWDOHVWLPDWHV
DUHTXDOLWDWLYHO\VLPLODUDFURVVWKHWZRVDPSOHV7KHVLP
SOHGLIIHUHQFHLQPHDQVUHSRUWHGLQFROXPQ\LHOGVQHJ
DWLYHWUHDWPHQWHIIHFWVIRUWKH&36DQG36,'FRPSDULVRQ
JURXSVLQERWKVDPSOHVH[FHSW36,'7KHIL[HGHIIHFWV
W\SHGLIIHUHQFLQJHVWLPDWRULQWKHWKLUGFROXPQIDUHVVRPH
ZKDWEHWWHUDOWKRXJKPDQ\HVWLPDWHVDUHVWLOOQHJDWLYHRU
GHWHULRUDWHZKHQZHFRQWUROIRUFRYDULDWHVLQERWKSDQHOV
7KHHVWLPDWHVLQWKHILIWKFROXPQDUHFORVHVWWRWKHH[SHU
LPHQWDOHVWLPDWHFRQVLVWHQWO\FORVHUWKDQWKRVHLQWKHVHF
RQGFROXPQZKLFKGRQRWFRQWUROIRUHDUQLQJVLQ7KH
WUHDWPHQWHIIHFWLVXQGHUHVWLPDWHGE\DERXWIRUWKH
&36FRPSDULVRQJURXSVDQGE\IRUWKH36,'JURXSV
/DORQGH
VFRQFOXVLRQIURPSDQHO$ZKLFKDOVRKROGVLQ
SDQHO%LVWKDWWKHUHJUHVVLRQVSHFLILFDWLRQVDQGFRPSDUL
VRQJURXSVIDLOWRUHSOLFDWHWKHWUHDWPHQWLPSDFW
,QFOXGLQJHDUQLQJVDVDQDGGLWLRQDOYDULDEOHLQWKH
UHJUHVVLRQVLQ7DEOHSDQHO&GRHVQRWDOWHU/DORQGH
V
EDVLFPHVVDJHDOWKRXJKWKHHVWLPDWHVLPSURYHFRPSDUHG
WRWKRVHLQSDQHO%,QFROXPQVDQGPDQ\HVWLPDWHV
UHPDLQQHJDWLYHEXWOHVVVRWKDQLQSDQHO%,QFROXPQ
WKHHVWLPDWHVIRU36,'DQG&36DUHQHJDWLYHEXWWKH
HVWLPDWHVIRUWKHVXEVHWVLPSURYH,QFROXPQVDQG
WKHHVWLPDWHVDUHFORVHUWRWKHH[SHULPHQWDOEHQFKPDUNWKDQ
LQSDQHO%RIIE\DERXWIRU36,'DQG&36
DQGE\IRU&362YHUDOOWKHUHVXOWVFORVHVWWRWKH
H[SHULPHQWDOEHQFKPDUNLQ7DEOHDUHIRU&36SDQHO&
7KLVUDLVHVDQXPEHURILVVXHV7KHVWUDWHJ\RIFRQVLGHULQJ
VXEVHWVRIWKHFRPSDULVRQJURXSLPSURYHVHVWLPDWHVRIWKH
WUHDWPHQWHIIHFWUHODWLYHWRWKHEHQFKPDUN+RZHYHU7DEOH
UHYHDOVWKDWVLJQLILFDQWGLIIHUHQFHVUHPDLQEHWZHHQWKH
FRPSDULVRQJURXSVDQGWKHWUHDWPHQWJURXS7KHVHVXEVHWV
DUHFUHDWHGEDVHGRQRQHRUWZRSUHLQWHUYHQWLRQYDULDEOHV
,Q6HFWLRQVDQGZHVKRZWKDWSURSHQVLW\VFRUHPHWKRGV
SURYLGHDV\VWHPDWLFPHDQVRIFUHDWLQJVXFKVXEVHWV
,'(17,)<,1*$1'(67,0$7,1*7+($9(5$*(
75($70(17())(&7
,GHQWLILFDWLRQ
/HW<LUHSUHVHQWWKHYDOXHRIWKHRXWFRPHZKHQXQLWLLV
H[SRVHGWRUHJLPHFDOOHGWUHDWPHQWDQGOHW<LRUHSUHVHQW

### Slide 298

-RXUQDORIWKH$PHULFDQ6WDWLVWLFDO$VVRFLDWLRQ'HFHPEHU
4/R/2FR/R
,FP
TT
,W&,&&&&11U/2/R&

FR
I
8
&&U
1Q& 
8&
1
XLZPZZ'&
L
(
8

8
8
&&0( 8/R&
'FR ,&
&/
8 8
8
&=
&2
&0&&
&///@FRWW:1&2
2&,FR
&=

&&&/
E
/,
LFR

/2FR&0/R
&/1/22
&=,& 
&//UOR/RR 
'&&&'
8 FR
R/RD
ZR&
&'&

2) 1U
/2&2FRFR


//@
U':/2 &,&?M
1UO' FR&0&0/2/2&,'&2
FR 4&"8 ]L]]WL2
&4/ 
FR
&&?MBD LD&L&&L&R
&
W
/2FR)LFRUOUO WRU0/R,

&&&0U
W8 RUO -5XLZ
&/
FR( FR8
&UO&( &0M=
,W/2&0
&/RZ
U

U/2
W,U &,&&,&
T&
M&= 'U11UFR

/88 
&=8
FR /8&PFR
MDD
/22FR UFR&0/2&2
JW21WDDP1 /2'W5&,8 U
&R'&U'& 
,
&
''FRD
FOL(8
8


,W2
&2
UORW 
W,
&,/
&RFTR&,=/2WR/2/2&?-U8
:W:/2&?M 45
2D(
&22FR
8
&

&0
(8DFR
&0FO(  &0
/2L28 &'&?LWU/2'/2/2&0( FRU/2 &=DFR
&/4"R ]WM(SXLZ

D8FR8
&&,
DUR4S(U
&&RL]]WW&U 
URZ0P&
O11/R/41T/R&?/&,
RR&?MRU
RU/RCR R /RD&U" 6Q8D8 
&2 8

R
(
(XL&
] U WZ
&?M2&&
2R FR
FR
&'
'
9&F
QU
TPW&R/RRU
&
O&?MFRQ&R
U&2/R&?M66D/
8
9
ZW
= LDFR
 UU

### Slide 299

Proposition 2
X ⊥ ⊥D|p(X )
Conditional on the propensity score, the covariates are
independent of the treatment, suggesting that the distribution
of covariate values should be the same for both treatment and
control groups

### Slide 300

Trimming the data
“Trimming” throws away units from control which do not
overlap with the treatment in terms of estimated propensity
score
In STATA:
. su pscore if treat==1
. drop if pscore<r(mean) & treat==0

### Slide 301

Overlap
 -RXUQDORIWKH$PHULFDQ6WDWLVWLFDO$VVRFLDWLRQ'HFHPEHU
ZKHWKHUZHVXFFHHGLQEDODQFLQJWKHFRYDULDWHVZLWKLQHDFK
VWUDWXP:HXVHWHVWVIRUWKHVWDWLVWLFDOVLJQLILFDQFHRIGLI
IHUHQFHVLQWKHGLVWULEXWLRQRIFRYDULDWHVIRFXVLQJRQILUVW
DQGVHFRQGPRPHQWVVHH5RVHQEDXPDQG5XELQ,I
WKHUHDUHQRVLJQLILFDQWGLIIHUHQFHVEHWZHHQWKHWZRJURXSV
ZLWKLQHDFKVWUDWXPWKHQZHDFFHSWWKHVSHFLILFDWLRQ,I
WKHUHDUHVLJQLILFDQWGLIIHUHQFHVWKHQZHDGGKLJKHURUGHU
WHUPVDQGLQWHUDFWLRQVRIWKHFRYDULDWHVXQWLOWKLVFRQGLWLRQ
LVVDWLVILHG,Q6HFWLRQZHGHPRQVWUDWHWKDWWKHUHVXOWVDUH
QRWVHQVLWLYHWRWKHVHOHFWLRQRIKLJKHURUGHUDQGLQWHUDFWLRQ
YDULDEOHV
,QWKHVHFRQGVWHSJLYHQWKHHVWLPDWHGSURSHQVLW\VFRUH
ZHQHHGWRHVWLPDWHDXQLYDULDWHQRQSDUDPHWULFUHJUHV
VLRQ(<M7L 7 MS;LIRUM  :HIRFXVRQ
VLPSOHPHWKRGVIRUREWDLQLQJDIOH[LEOHIXQFWLRQDOIRUP
VWUDWLILFDWLRQDQGPDWFKLQJEXWLQSULQFLSOHRQHFRXOGXVH
DQ\RIWKHVWDQGDUGDUUD\RIQRQSDUDPHWULFWHFKQLTXHVVHH
HJ+DUGOHDQG/LQWRQ+HFNPDQ,FKLPXUDDQG7RGG

:LWKVWUDWLILFDWLRQREVHUYDWLRQVDUHVRUWHGIURPORZHVWWR
KLJKHVWHVWLPDWHGSURSHQVLW\VFRUH:HGLVFDUGWKHFRPSDU
LVRQXQLWVZLWKDQHVWLPDWHGSURSHQVLW\VFRUHOHVVWKDQWKH
PLQLPXPRUJUHDWHUWKDQWKHPD[LPXPHVWLPDWHGSURSHQ
VLW\VFRUHIRUWUHDWHGXQLWV7KHVWUDWDGHILQHGRQWKHHV
WLPDWHGSURSHQVLW\VFRUHDUHFKRVHQVRWKDWWKHFRYDULDWHV
ZLWKLQHDFKVWUDWXPDUHEDODQFHGDFURVVWKHWUHDWPHQWDQG
FRPSDULVRQXQLWV:HNQRZWKDWVXFKVWUDWDH[LVWIURPVWHS
%DVHGRQZLWKLQHDFKVWUDWXPZHWDNHDGLIIHUHQFH
LQPHDQVRIWKHRXWFRPHEHWZHHQWKHWUHDWPHQWDQGFRP
SDULVRQJURXSVWKHQZHLJKWWKHVHE\WKHQXPEHURIWUHDWHG
REVHUYDWLRQVLQHDFKVWUDWXP:HDOVRFRQVLGHUPDWFKLQJRQ
WKHSURSHQVLW\VFRUH(DFKWUHDWPHQWXQLWLVPDWFKHGZLWK
UHSODFHPHQWWRWKHFRPSDULVRQXQLWZLWKWKHFORVHVWSURSHQ
VLW\VFRUHWKHXQPDWFKHGFRPSDULVRQXQLWVDUHGLVFDUGHG
VHH'HKHMLDDQG:DKEDIRUPRUHGHWDLOVDOVR+HFN
PDQ,FKLPXUD6PLWKDQG7RGG+HFNPDQ,FKLPXUD
DQG7RGG5XELQ
7KHUHDUHDQXPEHURIUHDVRQVIRUSUHIHUULQJWKLVWZR
VWHSDSSURDFKWRGLUHFWHVWLPDWLRQRI)LUVWWDFNOLQJ
GLUHFWO\ZLWKDQRQSDUDPHWULFUHJUHVVLRQZRXOGHQFRXQWHU
WKHFXUVHRIGLPHQVLRQDOLW\DVDSUREOHPLQPDQ\GDWDVHWV
VXFKDVRXUVWKDWKDYHDODUJHQXPEHURIFRYDULDWHV7KLV
ZRXOGDOVRRFFXUZKHQHVWLPDWLQJWKHSURSHQVLW\VFRUHXV
LQJQRQSDUDPHWULFWHFKQLTXHV+HQFHZHXVHDSDUDPHWULF
PRGHOIRUWKHSURSHQVLW\VFRUH7KLVLVSUHIHUDEOHWRDS
SO\LQJDSDUDPHWULFPRGHOGLUHFWO\WREHFDXVHDVZH
ZLOOVHHWKHUHVXOWVDUHOHVVVHQVLWLYHWRWKHORJLWVSHFLIL
FDWLRQWKDQUHJUHVVLRQPRGHOVVXFKDVWKRVHLQ7DEOH
)LQDOO\GHSHQGLQJRQWKHHVWLPDWRUWKDWRQHDGRSWVHJ
VWUDWLILFDWLRQDSUHFLVHHVWLPDWHRIWKHSURSHQVLW\VFRUHLV
QRWUHTXLUHG7KHSURFHVVRIYDOLGDWLQJWKHSURSHQVLW\VFRUH
HVWLPDWHSURGXFHVDWOHDVWRQHSDUWLWLRQVWUXFWXUHWKDWEDO
DQFHVSUHLQWHUYHQWLRQFRYDULDWHVDFURVVWKHWUHDWPHQWDQG
FRPSDULVRQJURXSVZLWKLQHDFKVWUDWXPZKLFKE\LV
DOOWKDWLVQHHGHGIRUDQXQELDVHGHVWLPDWHRIWKHWUHDWPHQW
LPSDFW
5(68/7686,1*7+(3523(16,7<6&25(
8VLQJWKHPHWKRGRXWOLQHGLQWKHSUHYLRXVVHFWLRQZH
VHSDUDWHO\HVWLPDWHWKHSURSHQVLW\VFRUHIRUHDFKVDPSOH
RIFRPSDULVRQXQLWVDQGWUHDWPHQWXQLWV)LJXUHVDQG
SUHVHQWKLVWRJUDPVRIWKHHVWLPDWHGSURSHQVLW\VFRUHVIRU
WKHWUHDWPHQWDQG36,'DQG&36FRPSDULVRQJURXSV
0RVWRIWKHFRPSDULVRQXQLWVRIDWRWDORI
36,'XQLWVDQGRI&36XQLWVDUHGLV
FDUGHGEHFDXVHWKHLUHVWLPDWHGSURSHQVLW\VFRUHVDUHOHVV
WKDQWKHPLQLPXPIRUWKHWUHDWPHQWXQLWV(YHQWKHQWKH
ILUVWELQXQLWVZLWKDQHVWLPDWHGSURSHQVLW\VFRUHRI
FRQWDLQVPRVWRIWKHUHPDLQLQJFRPSDULVRQXQLWVDQG
IHZWUHDWPHQWXQLWV$QLPSRUWDQWGLIIHUHQFHEHWZHHQWKH
ILJXUHVLVWKDW)LJXUHKDVPDQ\ELQVLQZKLFKWKHWUHDW
PHQWXQLWVJUHDWO\RXWQXPEHUWKHFRPSDULVRQXQLWV,Q
GHHGIRUWKUHHELQVWKHUHDUHQRFRPSDULVRQXQLWV,QFRQ
WUDVWLQ)LJXUHIRU&36HDFKELQFRQWDLQVDWOHDVWD
IHZFRPSDULVRQXQLWV2YHUDOOIRU36,'WKHUHDUH
PRUHWKDQKDOIWKHWRWDOQXPEHUWUHDWHGXQLWVZLWKDQHVWL
PDWHGSURSHQVLW\VFRUHLQH[FHVVRIDQGRQO\FRPSDU
LVRQXQLWVFRPSDUHGWRWUHDWHGDQGFRPSDULVRQXQLWV
IRU&36
)LJXUHVDQGLOOXVWUDWHWKHGLDJQRVWLFYDOXHRIWKH
SURSHQVLW\VFRUH7KH\UHYHDOWKDWDOWKRXJKWKHFRPSDUL
VRQJURXSVDUHODUJHUHODWLYHWRWKHWUHDWPHQWJURXSWKHUH
LVOLPLWHGRYHUODSLQWHUPVRISUHLQWHUYHQWLRQFKDUDFWHULV
WLFV+DGWKHUHEHHQQRFRPSDULVRQXQLWVRYHUODSSLQJZLWK
DEURDGUDQJHRIWKHWUHDWPHQWXQLWVWKHQLWZRXOGQRWKDYH
EHHQSRVVLEOHWRHVWLPDWHWKHDYHUDJHWUHDWPHQWHIIHFWRQWKH
LL ,O ,O O O
R LM
&D;P O)OQ
          
(VWLPDWHGS;LFRPSDULVRQXQLWVGLVFDUGHGILUVWELQFRQWDLQVFRPSDULVRQXQLWV
)LJXUH+LVWRJUDPRIWKH(VWLPDWHG3URSHQVLW\6FRUHIRU16:7UHDWHG8QLWVDQG36,'&RPSDULVRQ8QLWV7KH36,'XQLWVZKRVH
HVWLPDWHGSURSHQVLW\VFRUHLVOHVVWKDQWKHPLQLPXPHVWLPDWHGSURSHQVLW\VFRUHIRUWKHWUHDWPHQWJURXSDUHGLVFDUGHG7KHILUVWELQFRQWDLQV
36,'XQLWV7KHUHLVPLQLPDORYHUODSEHWZHHQWKHWZRJURXSV7KUHHELQVDQGFRQWDLQQRFRPSDULVRQXQLWV7KHUHDUH
WUHDWHGXQLWVZLWKDQHVWLPDWHGSURSHQVLW\VFRUHJUHDWHUWKDQDQGRQO\FRPSDULVRQXQLWV

### Slide 302

Overlap
'HKHMLDDQG:DKED&DXVDO(IIHFWVLQ1RQH[SHULPHQWDO6WXGLHV 
LL ,O O O ,O
^
R
&D

FR ;
  
(VWLPDWHGS;LFRPSDULVRQXQLWVGLVFDUGHGILUVWELQFRQWDLQVFRPSDULVRQXQLWV
)LJXUH+LVWRJUDPRIWKH(VWLPDWHG3URSHQVLW\6FRUHIRU16:7UHDWHG8QLWVDQG&36&RPSDULVRQ8QLWV7KH&36XQLWVZKRVH
HVWLPDWHGSURSHQVLW\VFRUHLVOHVVWKDQWKHPLQLPXPHVWLPDWHGSURSHQVLW\VFRUHIRUWKHWUHDWPHQWJURXSDUHGLVFDUGHG7KHILUVWELQFRQWDLQV
&36XQLWV7KHUHLVPLQLPDORYHUODSEHWZHHQWKHWZRJURXSVEXWWKHRYHUODSLVJUHDWHUWKDQLQ)LJXUHRQO\RQHELQFRQWDLQVQR
FRPSDULVRQXQLWVDQGWKHUHDUHWUHDWHGDQGFRPSDULVRQXQLWVZLWKDQHVWLPDWHGSURSHQVLW\VFRUHJUHDWHUWKDQ
WUHDWPHQWJURXSDOWKRXJKWKHWUHDWPHQWLPSDFWVWLOOFRXOG
EHHVWLPDWHGLQWKHUDQJHRIRYHUODS:LWKOLPLWHGRYHUODS
ZHFDQSURFHHGFDXWLRXVO\ZLWKHVWLPDWLRQ%HFDXVHLQRXU
DSSOLFDWLRQZHKDYHWKHEHQFKPDUNH[SHULPHQWDOHVWLPDWH
ZHDUHDEOHWRHYDOXDWHWKHDFFXUDF\RIWKHHVWLPDWHV(YHQ
LQWKHDEVHQFHRIDQH[SHULPHQWDOHVWLPDWHZHVKRZLQ6HF
WLRQWKDWWKHXVHRIPXOWLSOHFRPSDULVRQJURXSVSURYLGHV
DQRWKHUPHDQVRIHYDOXDWLQJWKHHVWLPDWHV
:HXVHVWUDWLILFDWLRQDQGPDWFKLQJRQWKHSURSHQVLW\
VFRUHWRJURXSWKHWUHDWPHQWXQLWVZLWKWKHVPDOOQXPEHU
RIFRPSDULVRQXQLWVZKRVHHVWLPDWHGSURSHQVLW\VFRUHVDUH
JUHDWHUWKDQWKHPLQLPXPRUOHVVWKDQWKHPD[LPXP
SURSHQVLW\VFRUHIRUWUHDWPHQWXQLWV:HHVWLPDWHWKHWUHDW
PHQWHIIHFWE\VXPPLQJWKHZLWKLQVWUDWXPGLIIHUHQFHLQ
PHDQVEHWZHHQWKHWUHDWPHQWDQGFRPSDULVRQREVHUYDWLRQV
RIHDUQLQJVLQZKHUHWKHVXPLVZHLJKWHGE\WKH
QXPEHURIWUHDWHGREVHUYDWLRQVZLWKLQHDFKVWUDWXP>7DEOH
FROXPQ@$QDOWHUQDWLYHLVDZLWKLQEORFNUHJUHVVLRQ
DJDLQWDNLQJDZHLJKWHGVXPRYHUWKHVWUDWD>7DEOHFRO
XPQ@:KHQWKHFRYDULDWHVDUHZHOOEDODQFHGVXFKD
UHJUHVVLRQVKRXOGKDYHOLWWOHHIIHFWEXWLWFDQKHOSHOLPL
QDWHWKHUHPDLQLQJZLWKLQEORFNGLIIHUHQFHV/LNHZLVHIRU
PDWFKLQJZHFDQHVWLPDWHDGLIIHUHQFHLQPHDQVEHWZHHQ
WKHWUHDWPHQWDQGPDWFKHGFRPSDULVRQJURXSVIRUHDUQLQJV
LQ>FROXPQ@DQGDOVRSHUIRUPDUHJUHVVLRQRI
HDUQLQJVRQFRYDULDWHV>FROXPQ@
7DEOHSUHVHQWVWKHUHVXOWV)RUWKH36,'VDPSOHWKH
VWUDWLILFDWLRQHVWLPDWHLVDQGWKHPDWFKLQJHVWL
PDWHLVFRPSDUHGWRWKHEHQFKPDUNUDQGRPL]HG
H[SHULPHQWHVWLPDWHRI7KHHVWLPDWHVIURPDGLI
IHUHQFHLQPHDQVDQGUHJUHVVLRQRQWKHIXOOVDPSOHDUH
DQG,QFROXPQVDQGFRQWUROOLQJ
IRUFRYDULDWHVKDVOLWWOHLPSDFWRQWKHVWUDWLILFDWLRQDQG
7DEOH(VWLPDWHG7UDLQLQJ(IIHFWVIRUWKH16:0DOH3DUWLFLSDQWV8VLQJ&RPSDULVRQ*URXSV)URP36,'DQG&36
16:WUHDWPHQWHDUQLQJVOHVVFRPSDULVRQJURXSHDUQLQJV
16:HDUQLQJVOHVV FRQGLWLRQDORQWKHHVWLPDWHGSURSHQVLW\VFRUH
FRPSDULVRQJURXS
HDUQLQJV 4XDGUDWLF 6WUDWLI\LQJRQWKHVFRUH 0DWFKLQJRQWKHVFRUH
  LQVFRUH
     
8QDGMXVWHG$GMXVWHGD 8QDGMXVWHG$GMXVWHG2EVHUYDWLRQVF8QDGMXVWHG$GMXVWHGG
16:  
 
36,'OH       
  
36,'       
 
36,'        
 
&36J        
    
&36        
    
&36        
    
D/HDVWVTXDUHVUHJUHVVLRQ5(RQDFRQVWDQWDWUHDWPHQWLQGLFDWRUDJHDJHHGXFDWLRQQRGHJUHHEODFN+LVSDQLF5(5(
E/HDVWVTXDUHVUHJUHVVLRQRI5(RQDTXDGUDWLFRQWKHHVWLPDWHGSURSHQVLW\VFRUHDQGDWUHDWPHQWLQGLFDWRUIRUREVHUYDWLRQVXVHGXQGHUVWUDWLILFDWLRQVHHQRWHJ
F1XPEHURIREVHUYDWLRQVUHIHUVWRWKHDFWXDOQXPEHURIFRPSDULVRQDQGWUHDWPHQWXQLWVXVHGIRUQDPHO\DOOWUHDWPHQWXQLWVDQGWKRVHFRPSDULVRQXQLWVZKRVHHVWLPDWHGSURSHQVLW\
VFRUHLVJUHDWHUWKDQWKHPLQLPXPDQGOHVVWKDQWKHPD[LPXPHVWLPDWHGSURSHQVLW\VFRUHIRUWKHWUHDWPHQWJURXS
G:HLJKWHGOHDVWVTXDUHVWUHDWPHQWREVHUYDWLRQVZHLJKWHGDVDQGFRQWUROREVHUYDWLRQVZHLJKWHGE\WKHQXPEHURIWLPHVWKH\DUHPDWFKHGWRDWUHDWPHQWREVHUYDWLRQ>VDPHFRYDULDWHVDVD@
3URSHQVLW\VFRUHVDUHHVWLPDWHGXVLQJWKHORJLVWLFPRGHOZLWKVSHFLILFDWLRQVDVIROORZVH36,'3UEL  )DH    36,'3URE7)DJHDJHHGXFDWLRQHGXFDWLRQPDUULHGQRGHJUHHEODFN+LVSDQLF5(5(5(5(X
EODFN
I3,DG3'3R   )JD HGDLHXQGJHDUHEON+LDL  
36,'DQG36,'3URE7L  )DJHDJHHGXFDWLRQHGXFDWLRQQRGHJUHHPDUULHGEODFN+LVSDQLF5(5(5(5(XX
&36&36DQG&363URE7L  )DJHDJHHGXFDWLRQHGXFDWLRQQRGHJUHHPDUULHGEODFN+LVSDQLF5(5(XXHGXFDWLRQ
5(DJH

### Slide 303

Results
'HKHMLDDQG:DKED&DXVDO(IIHFWVLQ1RQH[SHULPHQWDO6WXGLHV 
LL ,O O O ,O
^
R
&D

FR ;
  
(VWLPDWHGS;LFRPSDULVRQXQLWVGLVFDUGHGILUVWELQFRQWDLQVFRPSDULVRQXQLWV
)LJXUH+LVWRJUDPRIWKH(VWLPDWHG3URSHQVLW\6FRUHIRU16:7UHDWHG8QLWVDQG&36&RPSDULVRQ8QLWV7KH&36XQLWVZKRVH
HVWLPDWHGSURSHQVLW\VFRUHLVOHVVWKDQWKHPLQLPXPHVWLPDWHGSURSHQVLW\VFRUHIRUWKHWUHDWPHQWJURXSDUHGLVFDUGHG7KHILUVWELQFRQWDLQV
&36XQLWV7KHUHLVPLQLPDORYHUODSEHWZHHQWKHWZRJURXSVEXWWKHRYHUODSLVJUHDWHUWKDQLQ)LJXUHRQO\RQHELQFRQWDLQVQR
FRPSDULVRQXQLWVDQGWKHUHDUHWUHDWHGDQGFRPSDULVRQXQLWVZLWKDQHVWLPDWHGSURSHQVLW\VFRUHJUHDWHUWKDQ
WUHDWPHQWJURXSDOWKRXJKWKHWUHDWPHQWLPSDFWVWLOOFRXOG
EHHVWLPDWHGLQWKHUDQJHRIRYHUODS:LWKOLPLWHGRYHUODS
ZHFDQSURFHHGFDXWLRXVO\ZLWKHVWLPDWLRQ%HFDXVHLQRXU
DSSOLFDWLRQZHKDYHWKHEHQFKPDUNH[SHULPHQWDOHVWLPDWH
ZHDUHDEOHWRHYDOXDWHWKHDFFXUDF\RIWKHHVWLPDWHV(YHQ
LQWKHDEVHQFHRIDQH[SHULPHQWDOHVWLPDWHZHVKRZLQ6HF
WLRQWKDWWKHXVHRIPXOWLSOHFRPSDULVRQJURXSVSURYLGHV
DQRWKHUPHDQVRIHYDOXDWLQJWKHHVWLPDWHV
:HXVHVWUDWLILFDWLRQDQGPDWFKLQJRQWKHSURSHQVLW\
VFRUHWRJURXSWKHWUHDWPHQWXQLWVZLWKWKHVPDOOQXPEHU
RIFRPSDULVRQXQLWVZKRVHHVWLPDWHGSURSHQVLW\VFRUHVDUH
JUHDWHUWKDQWKHPLQLPXPRUOHVVWKDQWKHPD[LPXP
SURSHQVLW\VFRUHIRUWUHDWPHQWXQLWV:HHVWLPDWHWKHWUHDW
PHQWHIIHFWE\VXPPLQJWKHZLWKLQVWUDWXPGLIIHUHQFHLQ
PHDQVEHWZHHQWKHWUHDWPHQWDQGFRPSDULVRQREVHUYDWLRQV
RIHDUQLQJVLQZKHUHWKHVXPLVZHLJKWHGE\WKH
QXPEHURIWUHDWHGREVHUYDWLRQVZLWKLQHDFKVWUDWXP>7DEOH
FROXPQ@$QDOWHUQDWLYHLVDZLWKLQEORFNUHJUHVVLRQ
DJDLQWDNLQJDZHLJKWHGVXPRYHUWKHVWUDWD>7DEOHFRO
XPQ@:KHQWKHFRYDULDWHVDUHZHOOEDODQFHGVXFKD
UHJUHVVLRQVKRXOGKDYHOLWWOHHIIHFWEXWLWFDQKHOSHOLPL
QDWHWKHUHPDLQLQJZLWKLQEORFNGLIIHUHQFHV/LNHZLVHIRU
PDWFKLQJZHFDQHVWLPDWHDGLIIHUHQFHLQPHDQVEHWZHHQ
WKHWUHDWPHQWDQGPDWFKHGFRPSDULVRQJURXSVIRUHDUQLQJV
LQ>FROXPQ@DQGDOVRSHUIRUPDUHJUHVVLRQRI
HDUQLQJVRQFRYDULDWHV>FROXPQ@
7DEOHSUHVHQWVWKHUHVXOWV)RUWKH36,'VDPSOHWKH
VWUDWLILFDWLRQHVWLPDWHLVDQGWKHPDWFKLQJHVWL
PDWHLVFRPSDUHGWRWKHEHQFKPDUNUDQGRPL]HG
H[SHULPHQWHVWLPDWHRI7KHHVWLPDWHVIURPDGLI
IHUHQFHLQPHDQVDQGUHJUHVVLRQRQWKHIXOOVDPSOHDUH
DQG,QFROXPQVDQGFRQWUROOLQJ
IRUFRYDULDWHVKDVOLWWOHLPSDFWRQWKHVWUDWLILFDWLRQDQG
7DEOH(VWLPDWHG7UDLQLQJ(IIHFWVIRUWKH16:0DOH3DUWLFLSDQWV8VLQJ&RPSDULVRQ*URXSV)URP36,'DQG&36
16:WUHDWPHQWHDUQLQJVOHVVFRPSDULVRQJURXSHDUQLQJV
16:HDUQLQJVOHVV FRQGLWLRQDORQWKHHVWLPDWHGSURSHQVLW\VFRUH
FRPSDULVRQJURXS
HDUQLQJV 4XDGUDWLF 6WUDWLI\LQJRQWKHVFRUH 0DWFKLQJRQWKHVFRUH
  LQVFRUH
     
8QDGMXVWHG$GMXVWHGD 8QDGMXVWHG$GMXVWHG2EVHUYDWLRQVF8QDGMXVWHG$GMXVWHGG
16:  
 
36,'OH       
  
36,'       
 
36,'        
 
&36J        
    
&36        
    
&36        
    
D/HDVWVTXDUHVUHJUHVVLRQ5(RQDFRQVWDQWDWUHDWPHQWLQGLFDWRUDJHDJHHGXFDWLRQQRGHJUHHEODFN+LVSDQLF5(5(
E/HDVWVTXDUHVUHJUHVVLRQRI5(RQDTXDGUDWLFRQWKHHVWLPDWHGSURSHQVLW\VFRUHDQGDWUHDWPHQWLQGLFDWRUIRUREVHUYDWLRQVXVHGXQGHUVWUDWLILFDWLRQVHHQRWHJ
F1XPEHURIREVHUYDWLRQVUHIHUVWRWKHDFWXDOQXPEHURIFRPSDULVRQDQGWUHDWPHQWXQLWVXVHGIRUQDPHO\DOOWUHDWPHQWXQLWVDQGWKRVHFRPSDULVRQXQLWVZKRVHHVWLPDWHGSURSHQVLW\
VFRUHLVJUHDWHUWKDQWKHPLQLPXPDQGOHVVWKDQWKHPD[LPXPHVWLPDWHGSURSHQVLW\VFRUHIRUWKHWUHDWPHQWJURXS
G:HLJKWHGOHDVWVTXDUHVWUHDWPHQWREVHUYDWLRQVZHLJKWHGDVDQGFRQWUROREVHUYDWLRQVZHLJKWHGE\WKHQXPEHURIWLPHVWKH\DUHPDWFKHGWRDWUHDWPHQWREVHUYDWLRQ>VDPHFRYDULDWHVDVD@
3URSHQVLW\VFRUHVDUHHVWLPDWHGXVLQJWKHORJLVWLFPRGHOZLWKVSHFLILFDWLRQVDVIROORZVH36,'3UEL  )DH    36,'3URE7)DJHDJHHGXFDWLRQHGXFDWLRQPDUULHGQRGHJUHHEODFN+LVSDQLF5(5(5(5(X
EODFN
I3,DG3'3R   )JD HGDLHXQGJHDUHEON+LDL  
36,'DQG36,'3URE7L  )DJHDJHHGXFDWLRQHGXFDWLRQQRGHJUHHPDUULHGEODFN+LVSDQLF5(5(5(5(XX
&36&36DQG&363URE7L  )DJHDJHHGXFDWLRQHGXFDWLRQQRGHJUHHPDUULHGEODFN+LVSDQLF5(5(XXHGXFDWLRQ
5(DJH

### Slide 304

Covariate balance
 -RXUQDORIWKH$PHULFDQ6WDWLVWLFDO$VVRFLDWLRQ'HFHPEHU
PDWFKLQJHVWLPDWHV/LNHZLVHIRUWKH&36WKHSURSHQVLW\
VFRUHEDVHGHVWLPDWHVIURPWKH&36DQG
DUHPXFKFORVHUWRWKHH[SHULPHQWDOEHQFKPDUNWKDQ
HVWLPDWHVIURPWKHIXOOFRPSDULVRQVDPSOH
DQG
:HDOVRFRQVLGHUHVWLPDWHVIURPWKHVXEVHWVRIWKH36,'
DQG&36,Q7DEOHWKHHVWLPDWHVWHQGWRLPSURYHZKHQ
DSSOLHGWRQDUURZHUVXEVHWV+RZHYHUWKHHVWLPDWHVVWLOO
UDQJHIURPWR,Q7DEOHWKHHVWLPDWHVGR
QRWLPSURYHIRUWKHVXEVHWVDOWKRXJKWKHUDQJHRIIOXFWXD
WLRQLVQDUURZHUIURPWR7DEOHVDQGVKHG
OLJKWRQWKLV
7DEOHSUHVHQWVWKHSUHLQWHUYHQWLRQFKDUDFWHULVWLFVRI
WKHYDULRXVFRPSDULVRQJURXSV:HQRWHWKDWWKHVXEVHWV
36,'DQGDQG&36DQGDOWKRXJKPRUHFORVHO\
UHVHPEOLQJWKHWUHDWPHQWJURXSDUHVWLOOFRQVLGHUDEO\GLIIHU
HQWLQDQXPEHURILPSRUWDQWGLPHQVLRQVLQFOXGLQJHWKQLF
LW\PDULWDOVWDWXVDQGHVSHFLDOO\HDUQLQJV7DEOHSUHVHQWV
WKHFKDUDFWHULVWLFVRIWKHPDWFKHGVXEVDPSOHVIURPWKH
FRPSDULVRQJURXSV7KHFKDUDFWHULVWLFVRIWKHPDWFKHGVXE
VHWVRI&36DQG36,'FRUUHVSRQGFORVHO\WRWKHWUHDW
PHQWJURXSQRQHRIWKHGLIIHUHQFHVLVVWDWLVWLFDOO\VLJQLI
LFDQW%XWDVZHFUHDWHVXEVHWVRIWKHFRPSDULVRQJURXSV
WKHTXDOLW\RIWKHPDWFKHVGHFOLQHVPRVWGUDPDWLFDOO\IRU
WKH36,'36,'DQGHDUQLQJVQRZLQFUHDVHIURP
WRZKHUHDVWKH\GHFOLQHIRUWKHWUHDWPHQWJURXS7KH
WUDLQLQJOLWHUDWXUHKDVLGHQWLILHGWKHGLSLQHDUQLQJVDVDQ
LPSRUWDQWFKDUDFWHULVWLFRISDUWLFLSDQWVLQWUDLQLQJSURJUDPV
VHH$VKHQIHOWHU7KH&36VXEVDPSOHVUHWDLQ
WKHGLSEXWHDUQLQJVDUHVXEVWDQWLDOO\KLJKHUIRUWKH
PDWFKHGVXEVHWRI&36WKDQIRUWKHWUHDWPHQWJURXS
7KLVLOOXVWUDWHVRQHRIWKHLPSRUWDQWIHDWXUHVRISURSHQ
VLW\VFRUHPHWKRGVQDPHO\WKDWFUHDWLRQRIDGKRFVXEVDP
SOHVIURPWKHQRQH[SHULPHQWDOFRPSDULVRQJURXSLVQHL
WKHUQHFHVVDU\QRUGHVLUDEOHVXEVDPSOHVEDVHGRQVLQJOH
SUHLQWHUYHQWLRQFKDUDFWHULVWLFVPD\GLVSRVHRIFRPSDULVRQ
XQLWVWKDWVWLOOSURYLGHJRRGRYHUDOOFRPSDULVRQVZLWKWUHDW
PHQWXQLWV7KHSURSHQVLW\VFRUHVRUWVRXWZKLFKFRPSDUL
VRQXQLWVDUHPRVWUHOHYDQWFRQVLGHULQJDOOSUHLQWHUYHQWLRQ
FKDUDFWHULVWLFVVLPXOWDQHRXVO\QRWMXVWRQHFKDUDFWHULVWLFDW
QWLPH
&ROXPQLQ7DEOHLOOXVWUDWHVWKHYDOXHRIDOORZLQJ
ERWKIRUDKHWHURJHQHRXVWUHDWPHQWHIIHFWDQGIRUDQRQ
OLQHDUIXQFWLRQDOIRUPLQWKHSURSHQVLW\VFRUH7KHHVWLPD
WRUVLQFROXPQVKDYHERWKRIWKHVHFKDUDFWHULVWLFV
ZKHUHDVFROXPQUHJUHVVHVHDUQLQJVRQDOHVVQRQ
OLQHDUIXQFWLRQ>TXDGUDWLFDVRSSRVHGWRWKHVWHSIXQFWLRQ
LQFROXPQVDQG@RIWKHHVWLPDWHGSURSHQVLW\VFRUH
DQGDWUHDWPHQWLQGLFDWRU7KHHVWLPDWHVDUHFRPSDUDEOH
WRWKRVHLQFROXPQZKHUHZHUHJUHVVWKHRXWFRPHRQ
DOOSUHLQWHUYHQWLRQFKDUDFWHULVWLFVDQGDUHIDUWKHUIURPWKH
H[SHULPHQWDOEHQFKPDUNWKDQWKHHVWLPDWHVLQFROXPQV
7KLVGHPRQVWUDWHVWKHDELOLW\RIWKHSURSHQVLW\VFRUHWR
VXPPDUL]HDOOSUHLQWHUYHQWLRQYDULDEOHVEXWXQGHUOLQHVWKH
LPSRUWDQFHRIXVLQJWKHSURSHQVLW\VFRUHLQDVXIILFLHQWO\
QRQOLQHDUIXQFWLRQDOIRUP
)LQDOO\LWPXVWEHQRWHGWKDWHYHQWKRXJKWKHHVWLPDWHV
SUHVHQWHGLQ7DEOHDUHFORVHUWRWKHH[SHULPHQWDOEHQFK
PDUNWKDQWKRVHSUHVHQWHGLQ7DEOHZLWKWKHH[FHSWLRQ
RIWKHDGMXVWHGPDWFKLQJHVWLPDWRUWKHLUVWDQGDUGHUURUV
DUHKLJKHU,Q7DEOHFROXPQWKHVWDQGDUGHUURUVDUH
DQGIRUWKH&36DQG36,'FRPSDUHGWR
DQGLQ7DEOH3DQHO&FROXPQ7KLVLVEHFDXVHWKH
SURSHQVLW\VFRUHHVWLPDWRUVXVHIHZHUREVHUYDWLRQV:KHQ
VWUDWLI\LQJRQWKHSURSHQVLW\VFRUHZHGLVFDUGLUUHOHYDQW
FRQWUROVVRWKDWWKHVWUDWDPD\FRQWDLQDVIHZDVVHYHQ
WUHDWHGREVHUYDWLRQV+RZHYHUWKHVWDQGDUGHUURUVIRUWKH
DGMXVWHGPDWFKLQJHVWLPDWRUDQGDUHVLPLODUWR
WKRVHLQ7DEOH
%\VXPPDUL]LQJDOORIWKHFRYDULDWHVLQDVLQJOHQXPEHU
WKHSURSHQVLW\VFRUHPHWKRGDOORZVXVWRIRFXVRQWKHFRP
SDUDELOLW\RIWKHFRPSDULVRQJURXSWRWKHWUHDWPHQWJURXS
+HQFHLWDOORZVXVWRDGGUHVVWKHLVVXHVRIIXQFWLRQDOIRUP
DQGWUHDWPHQWHIIHFWKHWHURJHQHLW\PXFKPRUHHDVLO\
6(16,7,9,7<$1$/<6,6
6HQVLWLYLW\WRWKH6SHFLILFDWLRQRIWKH
3URSHQVLW\6FRUH
7KHXSSHUKDOIRI7DEOHGHPRQVWUDWHVWKDWWKHHVWLPDWHV
RIWKHWUHDWPHQWLPSDFWDUHQRWSDUWLFXODUO\VHQVLWLYHWRWKH
VSHFLILFDWLRQXVHGIRUWKHSURSHQVLW\VFRUH6SHFLILFDWLRQV
DQGDUHWKHVDPHDVWKRVHLQ7DEOHDQGKHQFHWKH\EDO
7DEOH6DPSOH0HDQVRI&KDUDFWHULVWLFVIRU0DWFKHG&RQWURO6DPSOHV
0DWFKHG1RRI
VDPSOHVREVHUYDWLRQV$JH (GXFDWLRQ%ODFN+LVSDQLF1RGHJUHH0DUULHG5(86 5(86
16:        
036,'        
>@>@>@>@ >@ >@ >@ >@
036,'        
>@>@>@>@ >@ >@ >@ >@
036,'        
>@>@>@>@ >@ >@ >@ >@
0&36        
>@>@>@>@ >@ >@ >@ >@
0&36        
>@>@>@>@ >@  >@ >@
0&36        
>@>@>@>@ >@ >@ >@ >@
127(6WDQGDUGHUURURQWKHGLIIHUHQFHLQPHDQVZLWK16:VDPSOHLVJLYHQLQEUDFNHWV
036,'DQG0&36DUHWKHVXEVDPSOHVRI36,'DQG&36WKDWDUHPDWFKHGWRWKHWUHDWPHQWJURXS

### Slide 305

Estimation in STATA
We will replicate some of DW (1999) using the experimental
treatment and non-experimental PSID sample
You will need to download the following zipped dataset from
Barbara Sianesi’s presentation given to the 2010 German
STATA Users Group in Berlin.
Note you will need to install psmatch2
ssc install psmatch2, replace

### Slide 306

. use ./nsw_psid, clear(NSW treated and PSID non-treated)
. qui probit treated age black hispanic married educ nodegree re75
. margins, dydx(_all)
Average marginal effects Number of obs = 2787Model VCE : OIM
Expression : Pr(treated), predict()dy/dx w.r.t. : age black hispanic married education nodegree re75
------------------------------------------------------------------------------| Delta-method| dy/dx Std. Err. z P>|z| [95% Conf. Interval]-------------+----------------------------------------------------------------age | -.0035844 .000462 -7.76 0.000 -.0044899 -.002679black | .0766501 .0088228 8.69 0.000 .0593577 .0939426hispanic | .0831734 .0157648 5.28 0.000 .0522751 .1140718married | -.0850743 .0070274 -12.11 0.000 -.0988478 -.0713009education | .0003458 .0023048 0.15 0.881 -.0041716 .0048633nodegree | .0418875 .0108642 3.86 0.000 .0205942 .0631809re75 | -6.89e-06 5.89e-07 -11.71 0.000 -8.04e-06 -5.74e-06------------------------------------------------------------------------------
. // compute the propensity score. predict double score(option pr assumed; Pr(treated))

### Slide 307

. // compare the densities of the estimated propensity score over groups
. density2 score, group(treated) saving(./psm2a, replace)
(file ./psm2a saved)
. graph export psm2a.pdf, replace
(file psm2a.pdf written in PDF format)
. psgraph, treated(treated) pscore(score) bin(50) saving(psm2b, replace)
(file psm2b.gph saved)
. graph export psm2b.pdf, replace
(file psm2b.pdf written in PDF format)

### Slide 308

02460.2.4.6.81Pr(treated)treated=1treated=0

### Slide 309

0.2.4.6.81Propensity ScoreUntreatedTreated

### Slide 310

. // compute nearest neighbor matching with caliper and replacement
. psmatch2 treated, pscore(score) outcome(re78) caliper(0.01)
There are observations with identical propensity score values.
The sort order of the data could affect your results.
Make sure that the sort order is random before calling psmatch2.
----------------------------------------------------------------------------------------
Variable Sample | Treated Controls Difference S.E. T-stat
----------------------------+-----------------------------------------------------------
re78 Unmatched | 5976.35202 21553.9209 -15577.5689 913.328457 -17.06
ATT | 6067.8117 5758.47686 309.334834 1080.935 0.29
----------------------------+-----------------------------------------------------------
Note: S.E. does not take into account that the propensity score is estimated.
psmatch2: | psmatch2: Common
Treatment | support
assignment | Off suppo On suppor | Total
-----------+----------------------+----------
Untreated | 0 2,490 | 2,490
Treated | 26 271 | 297
-----------+----------------------+----------
Total | 26 2,761 | 2,787
. //evaluate common support
. summarize _support if treated
Variable | Obs Mean Std. Dev. Min Max
-------------+--------------------------------------------------------
_support | 297 .9124579 .2831048 0 1

### Slide 311

. //evaluate quality of matching
. pstest2 age black hispanic married educ nodegree re75, sum graph
----------------------------------------------------------------------------
| Mean %reduct | t-test
Variable Sample | Treated Control %bias |bias| | t p>|t|
------------------------+----------------------------------+----------------
age Unmatched | 24.626 34.851 -116.6 | -16.48 0.000
Matched | 25.052 25.443 -4.5 96.2 | -0.61 0.545
| |
black Unmatched | .80135 .2506 132.1 | 20.86 0.000
Matched | .78967 .78967 0.0 100.0 | -0.00 1.000
| |
hispanic Unmatched | .09428 .03253 25.5 | 5.21 0.000
Matched | .09594 .08856 3.0 88.0 | 0.30 0.767
| |
married Unmatched | .16835 .86627 -194.9 | -33.02 0.000
Matched | .1845 .14022 12.4 93.7 | 1.40 0.163
| |
education Unmatched | 10.38 12.117 -68.6 | -9.51 0.000
Matched | 10.465 10.166 11.8 82.8 | 1.54 0.125
| |
nodegree Unmatched | .73064 .30522 94.0 | 15.10 0.000
Matched | .71587 .69373 4.9 94.8 | 0.56 0.573
| |
re75 Unmatched | 3066.1 19063 -156.6 | -20.12 0.000
Matched | 3197.4 3307.8 -1.1 99.3 | -0.28 0.778
| |
----------------------------------------------------------------------------

### Slide 312

-200-1000100200marriedre75ageeducationhispanicnodegreeblackUnmatchedMatched

### Slide 313

Kernel matching
Alternatively we can perform propensity score matching with a
kernel-based method.
Notice on the next slide that the estimate of the ATT switches
sign relative to that produced by the NN matching algorithm

### Slide 314

. // compute kernel-based matching with normal kernel. psmatch2 treated, pscore(score) outcome(re78) kernel k(normal) bw(0.01)----------------------------------------------------------------------------------------Variable Sample | Treated Controls Difference S.E. T-stat----------------------------+-----------------------------------------------------------re78 Unmatched | 5976.35202 21553.9209 -15577.5689 913.328457 -17.06ATT | 5976.35202 6882.18396 -905.831935 2151.26377 -0.42----------------------------+-----------------------------------------------------------Note: S.E. does not take into account that the propensity score is estimated.
| psmatch2:psmatch2: | CommonTreatment | supportassignment | On support | Total-----------+-----------+----------Untreated | 2,490 | 2,490Treated | 297 | 297-----------+-----------+----------Total | 2,787 | 2,787

### Slide 315

. //evaluate quality of matching
. pstest2 age black hispanic married educ nodegree re75, sum graph
----------------------------------------------------------------------------
| Mean %reduct | t-test
Variable Sample | Treated Control %bias |bias| | t p>|t|
------------------------+----------------------------------+----------------
age Unmatched | 24.626 34.851 -116.6 | -16.48 0.000
Matched | 24.626 24.572 0.6 99.5 | 0.09 0.926
| |
black Unmatched | .80135 .2506 132.1 | 20.86 0.000
Matched | .80135 .81763 -3.9 97.0 | -0.50 0.614
| |
hispanic Unmatched | .09428 .03253 25.5 | 5.21 0.000
Matched | .09428 .08306 4.6 81.8 | 0.48 0.631
| |
married Unmatched | .16835 .86627 -194.9 | -33.02 0.000
Matched | .16835 .1439 6.8 96.5 | 0.82 0.413
| |
education Unmatched | 10.38 12.117 -68.6 | -9.51 0.000
Matched | 10.38 10.238 5.6 91.8 | 0.81 0.415
| |
nodegree Unmatched | .73064 .30522 94.0 | 15.10 0.000
Matched | .73064 .72101 2.1 97.7 | 0.26 0.793
| |
re75 Unmatched | 3066.1 19063 -156.6 | -20.12 0.000
Matched | 3066.1 3905.8 -8.2 94.8 | -1.99 0.047
| |
----------------------------------------------------------------------------

### Slide 316

-200-1000100200marriedre75ageeducationhispanicnodegreeblackUnmatchedMatched

### Slide 317

Matchings vs. Propensity score
6 Journal of Business & Economic Statistics, January 2011
tistics for the experimental treatment group. The second pair
of columns presents summary statistics for the experimental
controls. The third pair of columns presents summary statistics
for the nonexperimental comparison group constructed from
the PSID. The last two columns present normalized differences
between the covariate distributions, between the experimental
treated and controls, and between the experimental treated and
the PSID comparison group, respectively. These normalized
differences are calculated as
nor-dif = X1 − X0√
(S2
0 + S2
1)/2
,
where Xw = ∑
i:Wi=w Xi/Nw and S2
w = ∑
i:Wi=w(Xi − Xw)2/
(Nw − 1).N o t et h a tt h i sd i f f e r sf r o mt h et-statistic for the test of
the null hypothesis that E[X|W = 0]= E[X|W = 1],w h i c hw i l l
be
t-stat = X1 − X0√
S2
0/N0 + S2
1/N1
.
The normalized difference provides a scale-free measure of the
difference in the location of the two distributions, and is useful
for assessing the degree of difﬁculty in adjusting for differences
in covariates.
Panel A contains the results for pretreatment variables and
Panel B for outcomes. Notice the large differences in back-
ground characteristics between the program participants and the
PSID sample. This is what makes drawing causal inferences
from comparisons between the PSID sample and the treatment
group a tenuous task. From Panel B, we can obtain an unbi-
ased estimate of the effect of the NSW program on earnings in
1978 by comparing the averages for the experimental treated
and controls, 6 .35 − 4.55 = 1.80, with a standard error of 0 .67
(earnings are measured in thousands of dollars). Using a nor-
mal approximation to the limiting distribution of the effect of
the program on earnings in 1978, we obtain a 95% conﬁdence
interval, which is [0.49, 3.10].
Table 2 presents estimates of the causal effect of the NSW
program on earnings using various matching, regression, and
weighting estimators. Panel A reports estimates for the exper-
imental data (treated and controls). Panel B reports estimates
based on the experimental treated and the PSID comparison
group. The ﬁrst set of rows in each case reports matching es-
timates for M equal to 1, 4, 16, 64, and 2490 (the size of the
PSID comparison group). The matching estimates include sim-
ple matching with no-bias adjustment and bias-adjusted match-
ing, ﬁrst by matching on the covariates and then by matching
on the estimated propensity score. The covariate matching es-
timators use the matrix Ane (the diagional matrix with inverse
sample variances on the diagonal) as the distance measure. Be-
cause we are focused on the average effect for the treated, the
bias correction only requires an estimate of µ0(Xi). We esti-
mate this regression function using linear regression on all nine
pretreatment covariates in Table 1,p a n e lA ,b u td on o ti n c l u d e
any higher order terms or interactions, with only the control
units that are used as a match [the units j such that Wj = 0a n d
Table 2. Experimental and nonexperimental estimates for the NSW data
M = 1 M = 4 M = 16 M = 64 M = 2490
Est. (SE) Est. (SE) Est. (SE) Est. (SE) Est. (SE)
Panel A:
Experimental estimates
Covariate matching 1 .22 (0.84) 1.99 (0.74) 1.75 (0.74) 2 .20 (0.70) 1 .79 (0.67)
Bias-adjusted cov matching 1 .16 (0.84) 1.84 (0.74) 1.54 (0.75) 1 .74 (0.71) 1 .72 (0.68)
Pscore matching 1 .43 (0.81) 1.95 (0.69) 1.85 (0.69) 1 .85 (0.68) 1 .79 (0.67)
Bias-adjusted pscore matching 1 .22 (0.81) 1.89 (0.71) 1.78 (0.70) 1 .67 (0.69) 1 .72 (0.68)
Regression estimates
Mean difference 1 .79 (0.67)
Linear 1 .72 (0.68)
Quadratic 2 .27 (0.80)
Weighting on pscore 1 .79 (0.67)
Weighting and linear regression 1 .69 (0.66)
Panel B:
Nonexperimental estimates
Simple matching 2 .07 (1.13) 1.62 (0.91) 0.47 (0.85) −0.11 (0.75) −15.20 (0.61)
Bias-adjusted matching 2 .42 (1.13) 2.51 (0.90) 2.48 (0.83) 2 .26 (0.71) 0 .84 (0.63)
Pscore matching 2 .32 (1.21) 2.06 (1.01) 0.79 (1.25) −0.18 (0.92) −1.55 (0.80)
Bias-adjusted pscore matching 3 .10 (1.21) 2.61 (1.03) 2.37 (1.28) 2 .32 (0.94) 2 .00 (0.84)
Regression estimates
Mean difference −15.20 (0.66)
Linear 0 .84 (0.88)
Quadratic 3 .26 (1.04)
Weighting on pscore 1 .77 (0.67)
Weighting and linear regression 1 .65 (0.66)
NOTE: The outcome is earnings in 1978 in thousands of dollars.

### Slide 318

Conclusions
Propensity scores are awesome to check the balance and
overlap of covariates – an under appreciated diagnostic, and
one that you won’t probably do if you only run regressions
There are extensions for more than two treatments (Cattaneo
2010)
The propensity score can make groups comparable but only
on the variables used to estimate the propensity score in the
ﬁrst place. There is NO guarantee you are balancing on
unobserved covariates.
If you know that there are important unobservable variables,
you may need another tool.
Randomization ensure that both observable and unobservable
variables are balanced

### Slide 319

Consider this
One thing to ruminate over is that in invoking CIA, the researcher
is basically saying that there exists some unknown randomness
determining treatment once she conditions on X . But if there is
some randomization,
1 how does she know that exactly, and
2 why not use that randomization itself as the treatment, such
as one often does in a reduced form instrumental variables
approach (more to come on that)? This is basically what
Krueger (1999) does when he regresses test scores onto
original room assignment (rather than actual room
assignment).
HT to Rodney Andrews for making this point to me in a
conversation

### Slide 320

Conclusions
Think hard about this question: Do you really need a
propensity score analysis? Why do you believe that CIA holds?
Why not use the conditional randomization process as the
explanatory variable? If you can’t do that, how conﬁdent are
you in CIA?

### Slide 321

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Stratiﬁcation
Matching
Propensity score matching
Regression discontinuity designs
What is regression discontinuity design?
Recently there has been an increase in a particular type of research design known as
regression discontinuity design(RDD). Cook (2008) has a fascinating history of
thought on how and why.
Donald Campbell is the originator of regression discontinuity design. First
study is Thistlethwaite and Campbell (1960). Merit awards were given to
students whose test scores were over some cutoﬀ point. They compared award
winners to non-winners just around the threshold to identify the causal eﬀect of
merit awards on future academic outcomes.
Pictures are helpful for understanding the RDD research design. Tell me what
you think these are saying.
Cunningham Causal Inference

### Slide 322

RDD Visual Example
V . Results
A. Earnings Discontinuities at the Admission Cutoff
To the extent that there are economic returns to attend-
ing the ﬂagship state university, one should observe a
discontinuity in earnings at the admission cutoff. This
is shown for white men in ﬁgure 2, which shows a
regression of residual earnings on a cubic polynomial of
adjusted SA T score. Table 1 shows the discontinuity
estimates that result from varying functional form
FIGURE 1.—FRACTION ENROLLED AT THE FLAGSHIP STATE UNIVERSITY
0 .1 .2 .3 .4 .5 .6 .7 .8 .9 1
-300 -250 -200 -150 -100 -50 0 50 100 150 200 250 300 350
Local Average
FIGURE 2.—NATURAL LOG OF ANNUAL EARNINGS FOR WHITE MEN TEN TO FIFTEEN YEARS AFTER HIGH SCHOOL GRADUATION (FIT WITH A CUBIC
POLYNOMIAL OF ADJUSTED SA T SCORE)
-.4 -.3 -.2 -.1 0 .1 .2
(Residual) Natural Log of Earnings
-300 -250 -200 -150 -100 -50 0 50 100 150 200 250 300 350
SAT Points Above the Admission Cutoff
Predicted Earnings Local Average
Estimated Discontinuity = 0.095 (z = 3.01) 
THE EFFECT OF A TTENDING THE FLAGSHIP STA TE UNIVERSITY ON EARNINGS 721

### Slide 323

RDD Visual Example
V . Results
A. Earnings Discontinuities at the Admission Cutoff
To the extent that there are economic returns to attend-
ing the ﬂagship state university, one should observe a
discontinuity in earnings at the admission cutoff. This
is shown for white men in ﬁgure 2, which shows a
regression of residual earnings on a cubic polynomial of
adjusted SA T score. Table 1 shows the discontinuity
estimates that result from varying functional form
FIGURE 1.—FRACTION ENROLLED AT THE FLAGSHIP STATE UNIVERSITY
0 .1 .2 .3 .4 .5 .6 .7 .8 .9 1
-300 -250 -200 -150 -100 -50 0 50 100 150 200 250 300 350
Local Average
FIGURE 2.—NATURAL LOG OF ANNUAL EARNINGS FOR WHITE MEN TEN TO FIFTEEN YEARS AFTER HIGH SCHOOL GRADUATION (FIT WITH A CUBIC
POLYNOMIAL OF ADJUSTED SA T SCORE)
-.4 -.3 -.2 -.1 0 .1 .2
(Residual) Natural Log of Earnings
-300 -250 -200 -150 -100 -50 0 50 100 150 200 250 300 350
SAT Points Above the Admission Cutoff
Predicted Earnings Local Average
Estimated Discontinuity = 0.095 (z = 3.01) 
THE EFFECT OF A TTENDING THE FLAGSHIP STA TE UNIVERSITY ON EARNINGS 721

### Slide 324

What is a regression discontinuity design?
RDD is based on a simple idea: the assignment of some
treatment to observational units is completely known.
We know that the probability of treatment “jumps” when an
applicant’s test scores meet university’s admission rule
If the students accepted and the students rejected are similar
near the cutoﬀ point, then data can be analyzed as though it
were a conditionally randomized experiment .
RDD is appropriate in situations where treatment assignment
is sharply discontinuous in the values of a variable
It’s best understood with an example. We’ll look at Angrist
and Lavy (1999) which was one of a few papers around the
same time to bring this methodology into economics

### Slide 325

Angrist, Joshua D. and Victor Lavy (1999). “Using Maimonides’
Rule to Estimate the Eﬀect of Class Size on Scholastic
Achievement”. Quarterly Journal of Economics .

### Slide 326

Summary
Angrist and Lavy (1999) are interested in estimating the
causal eﬀect of class size on educational outcomes
So like Krueger (1999), Angrist and Lavy (1999) are
interested in the eﬀect of class size on pupil achievement.

### Slide 327

Maimonedes Rule and class size
One of the earliest references to class size is the Babylonian
Talmud, completed around 6th century, which discusses rules
for the determination of class size and pupil-teacher ratios in
bible study.
Maimonides was a 12th century Rabbinic scholar who
interpreted the Talmud’s discussion of class size as follows
“Twenty-ﬁve children may be put in charge of one teacher. If
the number in the class exceeds twenty-ﬁve but is not more
than forty, two teachers must be appointed.”

### Slide 328

Maimonedes Rule and Class size
“The importance of Maimonedes’ rule for our purposes is that
since 1969, it has been used to determine the division of
enrollment cohorts into classes in Israeli public schools.”
(Angrist and Lavy 1999)

### Slide 329

Identiﬁcation
Class size in most places and most times is strongly associated
with unobservable diﬀerences in variables that also predict
educational performance
Poverty, aﬄuence, enthusiasm/skepticism about the value of
education, special needs of students for remedial or advanced
instruction, obscure and barely intelligible obsessions of
bureaucracies
Each of these determines (i) class size and (ii) clouds the
eﬀect of class size on academic performance because (iii) each
is also correlated independently with academic performance
However—if adherence to Maimonides’ rule is perfectly rigid,
then what would separate a school with a single class of size
40 from the same school with two classes whose average size
is 20.5 ( 41
2 = 20.5) is the enrollment of a single student

### Slide 330

Exogenous variation in class size
Maimonides’ rule has the largest impact on a school with
about 40 students in a grade cohort
With cohorts of size 40, 80 and 120 students, the steps down
in average class size required by Maimonides’ rule when an
additional student enrolls are from 40 to 20.5 ( 41
2 ), 40 to 27
( 81
3 ), and 40 to 30.25 ( 121
4 )
School also use the percent disadvantaged in a school “to
allocate supplementary hours of instruction and other school
resources” which is why Angrist and Lavy control for it

### Slide 331

Data
Test data came from short-lived national testing program in
Israeli elementary schools
June 1991 and 1992 near end of school year all 4-5th graders
were given achievement tests to measure math and (Hebrew)
reading skills
Average math and reading test scores (scaled from 1-100)
linked with data on school characteristics and class size from
other sources
Beginning of year (BOY) enrollment data was collected and
linked
Class size was collected around the time of the testing and
obtained from administrative source

### Slide 332

Data
The unit of observation in the linked data sets is the class
(remember the example comparing micro and aggregate data
regressions)
linked class-level data includes average test scores in each class,
spring class size, BOY enrollment for each school and grade, a
town identiﬁer, school-level index of student socioeconomic
status called “percent disadvantaged” (PD) and variables
identifying the ethnic and religious composition of the school
Study was limited to Jewish public schools, both secular and
religious schools, which account for the vast majority of
school children in Israel
Arab schools weren’t given the tests; ultra-orthodox Jewish
schools were omitted because they use a curriculum that
diﬀers considerably from public schools

### Slide 333

TABLE I
UNWEIGHTED DE SCRIP TIVE STATISTICS
Va r i a b l e M e a n S . D .
Quantiles
0.10 0.25 0.50 0.75 0.90
A. F u ll sa m ple
5t h gr a de (2019 cla sses, 1002 sch ools, t est ed in 1991)
Class size 29.9 6.5 21 26 31 35 38
Enro llment 77.7 38.8 31 50 72 100 128
Perc ent disadvantaged 14.1 13.5 2 4 10 20 35
Reading size 27.3 6.6 19 23 28 32 36
Math size 27.7 6.6 19 23 28 33 36
Av e r a g e v e r b a l 7 4 . 4 7 . 7 6 4 . 2 6 9 . 9 7 5 . 4 7 9 . 8 8 3 . 3
Av e r a g e m a t h 6 7 . 3 9 . 6 5 4 . 8 6 1 . 1 6 7 . 8 7 4 . 1 7 9 . 4
4t h gr a de (2049 cla sses, 1013 sch ools, t est ed in 1991)
Class size 30.3 6.3 22 26 31 35 38
Enro llment 78.3 37.7 30 51 74 101 127
Perc ent disadvantaged 13.8 13.4 2491 93 5
Reading size 27.7 6.5 19 24 28 32 36
Math size 28.1 6.5 19 24 29 33 36
Av e r a g e v e r b a l 7 2 . 5 8 . 0 6 2 . 1 6 7 . 7 7 3 . 3 7 8 . 2 8 2 . 0
Av e r a g e m a t h 6 8 . 9 8 . 8 5 7 . 5 6 3 . 6 6 9 . 3 7 5 . 0 7 9 . 4
3r d gr a de (2111 cla sses, 1011 sch ools, t est ed in 1992)
Class size 30.5 6.2 22 26 31 35 38
Enro llment 79.6 37.3 34 52 74 104 129
Perc ent disadvantaged 13.8 13.4 2491 93 5
Reading size 24.5 5.4 17 21 25 29 31
Math size 24.7 5.4 18 21 25 29 31
Av e r a g e v e r b a l 8 6 . 3 6 . 1 7 8 . 4 8 3 . 0 8 7 . 2 9 0 . 7 9 3 . 1
Av e r a g e m a t h 8 4 . 1 6 . 8 7 5 . 0 8 0 . 2 8 4 . 7 8 9 . 0 9 1 . 9
B. /H11001//H110025D i s c o n t i n u i t ys a m p l e( e n r o l l m e n t3 6 – 4 5 ,7 6 – 8 5 ,1 1 6 – 1 2 4 )
5th grade 4th grade 3rd grade
Mean S.D. Mean S.D. Mean S.D.
(471 cla sses,
224 sch ools)
(415 cla sses,
195 sch ools
(441 cla sses,
206 sch ools)
Class size 30.8 7.4 31.1 7.2 30.6 7.4
Enro llment 76.4 29.5 78.5 30.0 75.7 28.2
Perc ent disadvantaged 13.6 13.2 12.9 12.3 14.5 14.6
Reading size 28.1 7.3 28.3 7.7 24.6 6.2
Math size 28.5 7.4 28.7 7.7 24.8 6.3
Av e r a g e v e r b a l 7 4 . 5 8 . 2 7 2 . 5 7 . 8 8 6 . 2 6 . 3
Av e r a g e m a t h 6 7 . 0 1 0 . 2 6 8 . 7 9 . 1 8 4 . 2 7 . 0
Va r i a b l e d e ﬁ n i t i on s a r e a s fol l ow s : C l a s s s i z e/H11005number o f students in c lass in the spring, Enro llment /H11005
September grade enrollment, Percent disadvantaged /H11005percent of students in the school from ‘ ‘ disadvantaged
backgrounds,’ ’Reading size /H11005number o f students who to o k the reading test, Math size /H11005number o f students
who took the math test, A verage verbal /H11005average c o mpo site reading sc o re in the c lass, A verage math /H11005
average c o mpo site math sc o re in the c lass.
USING MAIMONIDES’RULE 539
Page 539
@xyserv1/disk4/CLS_jrnlkz/GRP_qjec/JOB_qjec114-2/DIV_064a01 dans
 at Baylor University on February 27, 2013http://qje.oxfordjournals.org/Downloaded from

### Slide 334

Discontinuity sample
Maimonides’ rule can be used to identify the eﬀects of class
size because the rule induces a discontinuity in the
relationship between enrollment and class size at enrollment
multiples of 40
Angrist and Lavy (1999) present descriptive statistics for one
such “discontinuity sample” deﬁned as only schools with
enrollments ± 5 students: 36,45; 76,85; 116,125
Slightly fewer than 25% of classes come from schools with
enrollments of this range
Average class size is a bit larger in this ± discontinuity sample
than in the overall sample
But otherwise remarkably similar to the full sample

### Slide 335

TABLE I
UNWEIGHTED DE SCRIP TIVE STATISTICS
Va r i a b l e M e a n S . D .
Quantiles
0.10 0.25 0.50 0.75 0.90
A. F u ll sa m ple
5t h gr a de (2019 cla sses, 1002 sch ools, t est ed in 1991)
Class size 29.9 6.5 21 26 31 35 38
Enro llment 77.7 38.8 31 50 72 100 128
Perc ent disadvantaged 14.1 13.5 2 4 10 20 35
Reading size 27.3 6.6 19 23 28 32 36
Math size 27.7 6.6 19 23 28 33 36
Av e r a g e v e r b a l 7 4 . 4 7 . 7 6 4 . 2 6 9 . 9 7 5 . 4 7 9 . 8 8 3 . 3
Av e r a g e m a t h 6 7 . 3 9 . 6 5 4 . 8 6 1 . 1 6 7 . 8 7 4 . 1 7 9 . 4
4t h gr a de (2049 cla sses, 1013 sch ools, t est ed in 1991)
Class size 30.3 6.3 22 26 31 35 38
Enro llment 78.3 37.7 30 51 74 101 127
Perc ent disadvantaged 13.8 13.4 2491 93 5
Reading size 27.7 6.5 19 24 28 32 36
Math size 28.1 6.5 19 24 29 33 36
Av e r a g e v e r b a l 7 2 . 5 8 . 0 6 2 . 1 6 7 . 7 7 3 . 3 7 8 . 2 8 2 . 0
Av e r a g e m a t h 6 8 . 9 8 . 8 5 7 . 5 6 3 . 6 6 9 . 3 7 5 . 0 7 9 . 4
3r d gr a de (2111 cla sses, 1011 sch ools, t est ed in 1992)
Class size 30.5 6.2 22 26 31 35 38
Enro llment 79.6 37.3 34 52 74 104 129
Perc ent disadvantaged 13.8 13.4 2491 93 5
Reading size 24.5 5.4 17 21 25 29 31
Math size 24.7 5.4 18 21 25 29 31
Av e r a g e v e r b a l 8 6 . 3 6 . 1 7 8 . 4 8 3 . 0 8 7 . 2 9 0 . 7 9 3 . 1
Av e r a g e m a t h 8 4 . 1 6 . 8 7 5 . 0 8 0 . 2 8 4 . 7 8 9 . 0 9 1 . 9
B. /H11001//H110025D i s c o n t i n u i t ys a m p l e( e n r o l l m e n t3 6 – 4 5 ,7 6 – 8 5 ,1 1 6 – 1 2 4 )
5th grade 4th grade 3rd grade
Mean S.D. Mean S.D. Mean S.D.
(471 cla sses,
224 sch ools)
(415 cla sses,
195 sch ools
(441 cla sses,
206 sch ools)
Class size 30.8 7.4 31.1 7.2 30.6 7.4
Enro llment 76.4 29.5 78.5 30.0 75.7 28.2
Perc ent disadvantaged 13.6 13.2 12.9 12.3 14.5 14.6
Reading size 28.1 7.3 28.3 7.7 24.6 6.2
Math size 28.5 7.4 28.7 7.7 24.8 6.3
Av e r a g e v e r b a l 7 4 . 5 8 . 2 7 2 . 5 7 . 8 8 6 . 2 6 . 3
Av e r a g e m a t h 6 7 . 0 1 0 . 2 6 8 . 7 9 . 1 8 4 . 2 7 . 0
Va r i a b l e d e ﬁ n i t i on s a r e a s fol l ow s : C l a s s s i z e/H11005number o f students in c lass in the spring, Enro llment /H11005
September grade enrollment, Percent disadvantaged /H11005percent of students in the school from ‘ ‘ disadvantaged
backgrounds,’ ’Reading size /H11005number o f students who to o k the reading test, Math size /H11005number o f students
who took the math test, A verage verbal /H11005average c o mpo site reading sc o re in the c lass, A verage math /H11005
average c o mpo site math sc o re in the c lass.
USING MAIMONIDES’RULE 539
Page 539
@xyserv1/disk4/CLS_jrnlkz/GRP_qjec/JOB_qjec114-2/DIV_064a01 dans
 at Baylor University on February 27, 2013http://qje.oxfordjournals.org/Downloaded from

### Slide 336

Class size function and enrollment size
fsc = es
intes−1
40 + 1 (90)
wherees is the beginning-of-year enrollment in schools in a given grade
(e.g., 5th grade);fsc is class size assigned to classc in schools for that
grade; int(n) is the largest integer less than or equal ton
This equation captures the fact that Maimonides’ rule allows
enrollment cohorts of 1-40 to be grouped in a single class, but
enrollment cohorts of 41-80 are split into two classes of average size
20.5-40, enrollment cohorts of 81-120 are split into three classes of
average size 27-40, and so on.

### Slide 337

Class size function and enrollment size
Although fsc is ﬁxed within schools, in practice enrollment
cohorts are not necessarily divided into classes of equal size
But, even though the actual relationship between class size
and enrollment size involves many factors, in Israel it clearly
has a lot to do with fsc as we’ll see

### Slide 338

on e-qu a r t er of t h e cla sses a r e of equ a l size. On t h e ot h er h a n d,
even t h ou gh t h e a ct u a l r ela t ion sh ip bet ween cla ss size a n d
en r ollm en t size in volves m a n y fa ct or s, in Isr a el it clea r ly h a s a lot
to do with fsc. Th is ca n be seen in F igu r es Ia a n d Ib, wh ich plot t h e
a ver a ge cla ss size by en r ollm en t size for ﬁft h a n d fou r t h gr a de
pu pils, a lon g wit h t h e cla ss-size fu n ct ion . Th e da sh ed h or izon t a l
F IGURE I
Cla ss Size in 1991 by In it ia l E n r ollm en t Cou n t , Act u a l Aver a ge Size a n d a s
P r edict ed by Ma im on ides’Ru le
USING MAIMONIDES’RULE 541
Page 541
@xyserv2/disk4/CLS_jrnlkz/GRP_qjec/JOB_qjec114-2/DIV_064a01 dawn
 at Baylor University on February 27, 2013http://qje.oxfordjournals.org/Downloaded from

### Slide 339

Class size function and test scores
The class size function is correlated with average class size as
well as average test scores of fourth and ﬁfth graders
The following picture plots average reading test scores and
average values of fsc by enrollment size in enrollment intervals
of ten for ﬁfth graders
The ﬁgure shows that test scores are generally higher in
schools with larger enrollments and larger predicted class sizes
But, it also shows an up-and-down pattern in which average
scores by enrollment size mirror the class-size function

### Slide 340

In a ddit ion t o exh ibit in g a st r on g a ssocia t ion wit h a ver a ge
cla ss size, t h e cla ss-size fu n ct ion is a lso cor r ela t ed wit h t h e
a ver a ge t est scor es of fou r t h a n d ﬁft h gr a der s (a lt h ou gh n ot t h ir d
gr a der s). Th is ca n be seen in F igu r es IIa a n d IIb, wh ich plot
a ver a ge r ea din g t est scor es a n d a ver a ge va lu es of fsc by en r ollm en t
size, in en r ollm en t in t er va ls of t en . F igu r e IIa plot s t h e scor es of
F IGURE II
Av e r a g e R e a d i n g S cor e s b y E n r ol l m e n t C ou n t , a n d t h e C or r e s p on d i n g Av e r a g e
Cla ss Size P r edict ed by Ma im on ides’Ru le
USING MAIMONIDES’RULE 543
Page 543
@xyserv1/disk4/CLS_jrnlkz/GRP_qjec/JOB_qjec114-2/DIV_064a01 dans
 at Baylor University on February 27, 2013http://qje.oxfordjournals.org/Downloaded from

### Slide 341

Class-size function and test scores
The overall positive correlation between test scores and enrollment is partly
attributed to larger schools in Israel being geographically concentrated in
larger, more aﬄuent cities (smaller schools in poorer “developmental towns”
outside the major urban centers)
They note that the enrollment size and the PD index measuring proportion of
students from disadvantaged background is negatively correlated
They control for the “trend” association between test scores and enrollment
size and plot the residuals from regressions of average scores and the average
of fsc on average enrollment and PD index for each interval
The estimates for ﬁfth graders imply a reduction inpredictedclass size of ten
students is associated with a 2.2 point increase in average reading scores – a
little more than one-quarter of a standard deviation in the distribution of class
averages

### Slide 342

do n ot pr ovide a fr a m ewor k for for m a l st a t ist ica l in fer en ce.
Alt h ou gh t h e m icr o da t a for fou r t h a n d ﬁft h gr a der s a r e u n -
a va ila ble, a m odel for in dividu a l pu pils’ t est scor es is u sed t o
descr ibe t h e ca u sa l r ela t ion sh ips t o be est im a t ed. F or t h e ith
F IGURE III
Aver a ge Test (Rea din g/Ma t h ) Scor es a n d P r edict ed Cla ss Size by E n r ollm en t ,
Residu a ls fr om Regr ession s on P er cen t Disa dva n t a ged a n d E n r ollm en t
USING MAIMONIDES’RULE 545
Page 545
@xyserv2/disk4/CLS_jrnlkz/GRP_qjec/JOB_qjec114-2/DIV_064a01 dawn
 at Baylor University on February 27, 2013http://qje.oxfordjournals.org/Downloaded from

### Slide 343

do n ot pr ovide a fr a m ewor k for for m a l st a t ist ica l in fer en ce.
Alt h ou gh t h e m icr o da t a for fou r t h a n d ﬁft h gr a der s a r e u n -
a va ila ble, a m odel for in dividu a l pu pils’ t est scor es is u sed t o
descr ibe t h e ca u sa l r ela t ion sh ips t o be est im a t ed. F or t h e ith
F IGURE III
Aver a ge Test (Rea din g/Ma t h ) Scor es a n d P r edict ed Cla ss Size by E n r ollm en t ,
Residu a ls fr om Regr ession s on P er cen t Disa dva n t a ged a n d E n r ollm en t
USING MAIMONIDES’RULE 545
Page 545
@xyserv2/disk4/CLS_jrnlkz/GRP_qjec/JOB_qjec114-2/DIV_064a01 dawn
 at Baylor University on February 27, 2013http://qje.oxfordjournals.org/Downloaded from

### Slide 344

OLS model
yisc = Xs β + nsc δ + µc ηs + εsc (91)
where yisc is pupil i’s score, Xs is a vector of school characteristics,
sometimes including functions of enrollment and nsc is the size of
class c in school s.
The term µc is an i.i.d. random class component, and the
term ηs is an i.i.d. random school component
The class-size coeﬃcient, δ is the primary parameter of
interest.

### Slide 345

OLS model
This equation describes the average potential outcomes of
students under alternative assignments of nsc controlling for
any eﬀects of Xs
If nsc was randomly assigned conditional on Xs, then δ would
be the weighted average response to random variation in class
size along the length of the individual causal response
functions connecting class size and pupil scores
Since nsc is not randomly assigned, in practice it is likely that
it is correlated with potential outcomes – in this case, the
error components in the equation
But while OLS may not have a causal interpretation, using
regression discontinuity design might

### Slide 346

Grouped regression
¯ysc = Xs β + nsc δ + ηs + [µc + ¯εsc] (92)
where overbars denote averages. The bracketed term is the
class-level error term.
Step 1: “ﬁrst stage”
nsc = Xs π0 + fsc π1 + ψsc (93)
where πj are parameter and the error term is deﬁned as the
residual from the population regression of ηs onto Xs and fsc and
captures other things associated with enrollment

### Slide 347

TABLE II
OLS ESTIMATE S F OR1991
5th Grade 4th Grade
Rea din g com pr eh en sion Math Rea din g com pr eh en sion Math
(1) (2) (3) (4) (5) (6) (7) (8) (9) (10) (11) (12)
M ean score 74.3 67.3 72.5 69.9
(s.d .) (8.1) (9.9) (8.0) (8.8)
R egressors
Class size .221 /H11002.031 /H11002.025 .322 .076 .019 0.141 /H11002.053 /H11002.040 .221 .055 .009
(.031) (.026) (.031) (.039) (.036) (.044) (.033) (.028) (.033) (.036) (.033) (.039)
Perc ent disadvantaged /H11002.350 /H11002.351 /H11002.340 /H11002.332 /H11002.339 /H11002.341 /H11002.289 /H11002.281
(.012) (.013) (.018) (.018) (.013) (.014) (.016) (.016)
E n r ollm en t /H11002.002 .017 /H11002.004 .014
(.006) (.009) (.007) (.008)
Root MSE 7.54 6.10 6.10 9.36 8.32 8.30 7.94 6.65 6.65 8.66 7.82 7.81
R2 .036 .369 .369 .048 .249 .252 .013 .309 .309 .025 .204 .207
N2 , 0 1 9 2 , 0 1 8 2 , 0 4 9 2 , 0 4 9
The unit of observation is the average score in the class. Standard errors are reported in parentheses. Standard errors were corrected for within-school correlation between classes.
USING MAIMONIDES’RULE551
Page551
@xyserv1/disk4/CLS_jrnlkz/GRP_qjec/JOB_qjec114-2/DIV_064a01 dans
 at Baylor University on February 27, 2013http://qje.oxfordjournals.org/Downloaded from

### Slide 348

TABLE III
REDUCED-FORMESTIMATE S F OR1991
5th Graders 4th Graders
Class size
Reading
com pr eh en sion Math Class size
Reading
com pr eh en sion Math
(1) (2) (3) (4) (5) (6) (7) (8) (9) (10) (11) (12)
A. F u ll sa m ple
Me ans 29.9 74.4 67.3 30.3 72.5 68.9
(s.d .) (6.5) (7.7) (9.6) (6.3) (8.0) (8.8)
Re gre sso rs
fsc .704 .542 /H11002.111 /H11002.149 /H11002.009 /H11002.124 .772 .670 /H11002.085 /H11002.089 .038 /H11002.033
(.022) (.027) (.028) (.035) (.039) (.049) (.020) (.025) (.031) (.040) (.037) (.047)
Perc ent disadvantaged /H11002.076 /H11002.053 /H11002.360 /H11002.355 /H11002.354 /H11002.338 /H11002.054 /H11002.039 /H11002.340 /H11002.340 /H11002.292 /H11002.282
(.010) (.009) (.012) (.013) (.017) (.018) (.008) (.009) (.013) (.014) (.016) (.016)
Enro llment .043 .010 .031 .027 .001 .019
(.005) (.006) (.009) (.005) (.007) (.009)
Root MSE 4.56 4.38 6.07 6.07 8.33 8.28 4.20 4.13 6.64 6.64 7.83 7.81
R2 .516 .553 .375 .377 .247 .255 .561 .575 .311 .311 .204 .207
N2 , 0 1 9 2 , 0 1 9 2 , 0 1 8 2 , 0 4 9 2 , 0 4 9 2 , 0 4 9
B. Discontinuity sample
Me ans 30.8 74.5 67.0 31.1 72.5 68.7
(s.d .) (7.4) (8.2) (10.2) (7.2) (7.8) (9.1)
Re gre sso rs
fsc .481 .346 /H11002.197 /H11002.202 /H11002.089 /H11002.154 .625 .503 /H11002.061 /H11002.075 .059 .012
(.053) (.052) (.050) (.054) (.071) (.077) (.050) (.053) (.056) (.063) (.072) (.080)
Perc ent disadvantaged /H11002.130 /H11002.067 /H11002.424 /H11002.422 /H11002.435 /H11002.405 /H11002.068 /H11002.029 /H11002.348 /H11002.343 /H11002.306 /H11002.291
(.029) (.028) (.027) (.029) (.039) (.042) (.029) (.028) (.032) (.034) (.041) (.043)
Enro llment .086 .003 .041 .063 .007 .024
(.015) (.015) (.022) (.014) (.017) (.022)
Root MSE 5.95 5.58 6.24 6.24 8.58 8.53 5.49 5.26 6.57 6.57 8.26 8.25
R2 .360 .437 .421 .421 .296 .305 .428 .475 .299 .299 .178 .182
N4 7 1 4 7 1 4 7 1 4 1 5 4 1 5 4 1 5
The functionfscis equal to enrollment/[int((enrollment /H110021)/40)/H110011]. Standard errors are reported in parentheses. Standard errors were corrected for within-school correlation
between classes. The unit of observation is the average score in the class.
USING MAIMONIDES’RULE553
Page553
@xyserv1/disk4/CLS_jrnlkz/GRP_qjec/JOB_qjec114-2/DIV_064a01 dans
 at Baylor University on February 27, 2013http://qje.oxfordjournals.org/Downloaded from

### Slide 349

Second step: use ﬁtted values in grouped regression
¯ysc = Xs β + ˆnsc δ + ηs + [µc + ¯εsc] (94)
where ˆnsc is the predicted class size from the previous regression

### Slide 350

TABLE IV
2SLS ESTIMATE S F OR1991 (FIFTHGRADE RS)
Rea din g com pr eh en sion Math
Full sample
/H11001//H110025
Discon t in u it y
sample Full sample
/H11001//H110025
Discon t in u it y
sample
(1) (2) (3) (4) (5) (6) (7) (8) (9) (10) (11) (12)
M ean score 74.4 74.5 67.3 67.0
(s.d .) (7.7) (8.2) (9.6) (10.2)
R egressors
Class size /H11002.158 /H11002.275 /H11002.260 /H11002.186 /H11002.410 /H11002.582 /H11002.013 /H11002.230 /H11002.261 /H11002.202 /H11002.185 /H11002.443
(.040) (.066) (.081) (.104) (.113) (.181) (.056) (.092) (.113) (.131) (.151) (.236)
Perc ent disadvantaged /H11002.372 /H11002.369 /H11002.369 /H11002.477 /H11002.461 /H11002.355 /H11002.350 /H11002.350 /H11002.459 /H11002.435
(.014) (.014) (.013) (.037) (.037) (.019) (.019) (.019) (.049) (.049)
Enro llment .022 .012 .053 .041 .062 .079
(.009) (.026) (.028) (.012) (.037) (.036)
Enro llment squared/ 100 .005 /H11002.010
(.011) (.016)
Piec ewise linear trend .136 .193
(.032) (.040)
Root MSE 6.15 6.23 6.22 7.71 6.79 7.15 8.34 8.40 8.42 9.49 8.79 9.10
N 2019 1961 471 2018 1960 471
The unit of observation is the average score in the class. Standard errors are reported in parentheses. Standard errors were corrected for within-school correlation between classes.
All est im a t es u sefscas an instrument f o r c lass size.
QUARTERLY J OURNAL OF ECONOMICS554
Page554
@xyserv1/disk4/CLS_jrnlkz/GRP_qjec/JOB_qjec114-2/DIV_064a01 dans
 at Baylor University on February 27, 2013http://qje.oxfordjournals.org/Downloaded from

### Slide 351

How large is their eﬀects?
The Tennessee STAR experiment yielded eﬀect sizes of about 0.13-0.27
standard deviation among pupils and about 0.32 - 0.66 standard deviation in
the distribution of class means
Angrist and Lavy (1999) compare their results by calculating their eﬀect size
associated with reducing class size by eight pupils (same as STAR)
They then multiply this times their second step estimate for reading scores for
ﬁfth graders (-0.275) gives an eﬀect size of around 2.2 points or 0.29 standard
deviation
Their estimates of eﬀect size for ﬁfth graders are at the low end of the range of
those found in the Tennessee experiment
Observational studies are often confounded by a failure to isolate a credible
source of exogenous variation in school inputs, leading one researcher
(Hanushek 1997) to conclude that school inputs don’t matter in academic
performance
RDD overcomes problems of confounding by exploiting exogenous variation
that originates inadministrative rules
As with STAR experiment, their study shows that smaller classes appear
beneﬁcial to student academic achievement

### Slide 352

RDD Data Requirements
Question: So, where can I ﬁnd these “jumps”? Answer: Humans are
embedding “jumps” in their rules all the time. Dumb rules – while usually bad
policy – aregreatfor research.
Validity doesn’t require the assignment rule be arbitrary, only that it is known,
precise and free of manipulation. The most eﬀective RDD studies involve
programs whereX has a “hair trigger” that is not tightly related to the
outcome being studied. Examples:
Probability of being tried as an adult (higher penalties for a given crime) “jumps” at age 18
Probability of being arrested for DWI “jumps” at blood alcohol content>0.08
Probability of receiving universal healthcare insurance “jumps” at age 65
Probability of receiving medical attention “jumps” when birthweight falls below 1,500 grams
Probability of having to attend summer school “jumps” when grade falls below 60
Data requirements can be substantial. Large sample sizes are characteristic
features of the RDD
If there are strong trends, one typically needs a lot of data
Researchers are typically using administrative data or settings such as birth
records where there aremanyobservations

### Slide 353

More recent examples
Almond et al. (2010): Infants with low birthweight have both
higher mortality and receive more medical treatment. Medical
treatment becomes more likely when infant weight falls below
1,500 grams (or ˜3 pounds). What’s the eﬀect of medical
treatment on infant mortality?
Anderson and Magruder (2012): At Yelp.com, individuals
reviewed restaurants on a scale of 1 to 5. The aggregate score
was a weighted average of all reviews. Yelp assigned an
integer stars based on which quintile a restaurant was in the
distribution of reviews. What’s the eﬀect of a star on
reservations and revenue?

### Slide 354

Sharp vs. Fuzzy RDD
RDD estimates the causal eﬀect by distinguishing the discontinuous
function, 1( X ≥ X0), from the smooth selection function, f (X )
(though Van der Klaauw uses S instead of X notation)
COLLEGE AID AND ENROLLMENT 
II 
i- 
H 
o Q) 
v) 
o 
I, 
0) 
Selection variable S 
FIGURE 2 
ASSIGNMENT IN THE SHARP (DASHED) AND FUZZY (SOLID) RD DESIGN 
manner but in such a way that the propensity score function Pr(T = 1 I S) is again 
known to have a discontinuity at S. The fuzzy design can occur in case of misassign- 
ment relative to the cutoff value in a sharp design, with values of S near the cutoff 
appearing in both treatment and control groups. Alternatively, in addition to the 
position of the individual's score relative to the cutoff value, assignment may be 
based on additional variables observed by the administrator, but unobserved by 
the evaluator. Compared to the sharp design, selection here is both on observables 
and unobservables. Instead of having the step function Pr(T = 1 I S) = 1{S > 3), 
the selection probability as a function of S may now appear as the S-shaped func- 
tion shown in Figure 2. 
As in the sharp RD design case, it is again possible to exploit the discontinuity in 
the selection rule to identify a treatment effect under continuity assumption Al. 
To see this, note that if the conditional mean function E[u I S] is continuous at S = 
5, then lims, E[YI S] - limst E[YI S] = a(limsts E[T I S] - lims E[T I S]). It 
follows that the treatment effect a is identified by 
limss E[YI S] - limsts E[YI S] 
lims,s E[TI S] - limsts E[TI S] 
where the denominator in (9) is nonzero because of the known discontinuity of 
E[TIS] at S. 
(9) 
1261 
Figure: from Van der Klaauw (2002)

### Slide 355

Sharp vs. Fuzzy RDD
1 Sharp RDD: Treatment is a deterministic function of running
variable, X . Example: Medicare beneﬁts.
2 Fuzzy RDD: Discontinuous “jump” in the probability of
treatment when X > X0. Cutoﬀ is used as an instrumental
variable for treatment. Example: Maimonides Rule (Angrist
and Lavy 1999)

### Slide 356

Treatment assignment in the sharp RDD
Deterministic treatment assignment (“sharp RDD”)
In Sharp RDD, treatment status is a deterministic and
discontinuous function of a covariate, Xi:
Di =
{
1 if Xi ≥ X0
0 if Xi < X0
where X0 is a known threshold or cutoﬀ. In other words, if you
know the value of Xi for a unit i, you know treatment assignment
for unit i with certainty.
Example: Let X be age. Americans aged 64 are not eligible for
Medicare, but Americans aged 65 ( X ≥ X65) are eligible for
Medicare (ignoring disability exemptions)

### Slide 357

Treatment eﬀect: deﬁnition and estimation
Deﬁnition of treatment eﬀect
Assume constant treatment eﬀects potential outcomes model linear inX:
Y0i = α+βXi
Y1i = Y0i +δ
Use the switching equation and write in terms ofYi:
Yi = Y0i + (Y1i −Y0i )Di
Yi = α+βXi +δDi +εi
The treatment eﬀect parameter,δ, is the discontinuity in the conditional
expectation function:
δ = limXi→X0E[Y1i |Xi =X0] −limX0←XiE[Y0i |Xi =X0]
= limXi→X0E[Yi|Xi =X0] −limX0←XiE[Yi|Xi =X0]
The sharp RDD estimation is interpreted as an average causal eﬀect of the
treatment at the discontinuity
δSRD=E[Y1i −Y0i |Xi =X0]

### Slide 358

Notice the role of extrapolation in estimating treatment eﬀects
when sharp RDD
Left of cutoﬀ, only non-treated observations, Di = 0 for
X < X0
Right of cutoﬀ, only treated observations, Di = 1 for X ≥ X0
Figure: Dashed lines are extrapolations

### Slide 359

Key identifying assumption
Continuity of conditional regression functions (Hahn, Todd and Van der Klaauw
2001; Lee 2008)
E[Y0i |X =X0] andE[Y1i |X =X0] are continuous (smooth) inX atX0.
Remark Alfred Marshall quotes Darwin in Principles of Economics(1890), “Natura
non fact saltum”, which means“nature does not make jumps”. Jumps are
unnatural, so when they occur, they require an explanation
Meaning If population averagepotential outcomes, Y1 andY0, are continuous
functions ofX at the cutoﬀ,X0, then potential average outcomesdo notjump at
X0. All other unobserved determinants ofY are continuously related to the running
variable,X
Implication This assumption allows us to use average outcome of units right below
the cutoﬀ as a valid counterfactual for units right above the cutoﬀ. The causal
eﬀect of the treatment will be based on extrapolation from the trend,
E[Y0i |X <X0], to those values ofX >X0 for theE[Y0i |X >X0].

### Slide 360

/// --- Examples using simulated data
set obs 1000
set seed 1234567
* Generate running variable
gen x = rnormal(100, 50)
replace x=0 if x < 0
drop if x > 280
sum x, det
* Set the cutoff at X=140. Treated if X > 140
gen D = 0
replace D = 1 if x > 140

### Slide 361

Graphical example of continuous assumption
gen y1 = 100 + 0*D + 2*x + rnormal(0, 20)
scatter y1 x if D==0, msize(vsmall) || scatter y1 x if D==1, msize(vsmall)
legend(off) xline(140, lstyle(foreground)) || lfit y1 x if D ==0, color(red)
|| lfit y1 x if D ==1, color(red) ytitle("Outcome (Y)") xtitle("Test
Score (X)")

### Slide 362

Graphical example with treatment eﬀect, cutoﬀ=140
gen y = 100 + 80*D + 2*x + rnormal(0, 20)
scatter y x if D==0, msize(vsmall) || scatter y x if D==1, ///
msize(vsmall) legend(off) xline(140, lstyle(foreground)) || /// lfit
y x if D ==0, color(red) || lfit y x if D ==1, color(red) /// ytitle("Outcome
(Y)") xtitle("Test Score (X)")
Vertical distance is the treatment effect0200400600800Outcome (Y)050100150200250Test Score (X)

### Slide 363

Tangent: centering at the cutoﬀ point
It is common for authors to transformX by “centering” atX0:
Yi =α+β(Xi −X0) +δDi +εi
This doesn’t change the interpretation of the treatment eﬀect – only the
interpretation of the intercept.
Example: Medicare and age 65. Center the running variable (age) by
subtracting 65:
Y = β0 +β1(Age−65) +β2Edu
= β0 +β1Age−β165 +β2Edu
= α+β1Age+β2Edu
whereα=β0 −β165. All other coeﬃcients, notice, have the same
interpretation, except for the intercept.

### Slide 364

[No extractable text in PDF text layer; page may contain an image or diagram.]

### Slide 365

Nonlinearity bias
Smoothness inE[Y0i |Xi] and linearity are diﬀerent things. What if the trend relation
E[Y0i |Xi] does not jump atX0 but rather is simply nonlinear?
gen x2 = x*x
gen x3 = x*x*x
gen y = 10000 + 0*D - 100*x +x2 + rnormal(0, 1000)
scatter y x if D==0, msize(vsmall) || scatter y x if D==1, msize(vsmall) legend(off) xline(140,
lstyle(foreground)) ylabel(none) || lfit y x if D ==0, color(red) || lfit y x if D ==1, color(red)
xtitle("Test Score (X)") ytitle("Outcome (Y)")

### Slide 366

Sharp RDD: Nonlinear Case
Suppose the nonlinear relationship is E [Y 0
i |Xi] = f (Xi) for
some reasonably smooth function f (Xi). In that case we’d ﬁt
the regression model:
Yi = f (Xi) + δDi + ηi
Since f (Xi) is counterfactual for values of Xi > X0, how will
we model the nonlinearity? There are 2 ways of approximating
f (Xi):
1 Let f (Xi ) equal a pth order polynomial:
Yi = α + β1xi + β2x 2
i + · · · + βpx p
i + δDi + ηi
2 Use a nonparametric kernel method (later)

### Slide 367

Diﬀerent polynomials on the 2 sides of the discontinuity
We can generalize the function,f(xi), by allowing thexi terms to diﬀer on
both sides of the cutoﬀ by including them both individually and interacting
them withDi. In that case we have:
E[Y0i |Xi] = α+β01˜Xi +β02˜X2i +· · ·+β0p ˜Xp
i
E[Y1i |Xi] = α+δ +β11˜Xi +β12˜X2i +· · ·+β1p ˜Xp
i
where˜Xi is the centered running variable (i.e.,Xi −X0). Centering atX0
ensures that the treatment eﬀect atXi =X0 is the coeﬃcient onDi in a
regression model with interaction terms
As Lee and Lemieux (2010) note, allowing diﬀerent functions on both sides of
the discontinuity should be the main results in an RDD paper as otherwise we
use values from both sides of the cutoﬀ to estimation the function on each side

### Slide 368

Diﬀerent polynomials on the 2 sides of the discontinuity
To derive a regression model, ﬁrst note that the observed values must be used
in place of the potential outcomes:
E[Y|X] =E[Y0|X] +(E[Y1|X] −E[Y0|X])D
Regression model you estimate is:
Yi = α+β01˜xi +β02˜x2i +· · ·+β0p˜xp
i
+δDi +β∗1Di˜xi +β∗2Di˜x2i +· · ·+β∗pDi˜xp
i +εi
whereβ∗1 =β11−β01, β∗2 =β21−β21 andβ∗p =β1p −β0p
The equation we looked at earlier a few slides back was just a special case of
the above equation withβ∗1 =β∗2 =β∗p = 0
The treatment eﬀect atx0 is δ
The treatment eﬀect atXi −X0 =c >0 is: δ +β∗1c +β∗2c2 +· · ·+β∗pcp

### Slide 369

Polynomial simulation example
capture drop y x2 x3gen x2 = x*xgen x3 = x*x*xgen y = 10000 + 0*D - 100*x +x2 + rnormal(0, 1000)
reg y D x x2 x3
predict yhat
scatter y x if D==0, msize(vsmall) || scatter y x if D==1, msize(vsmall)legend(off) xline(140, lstyle(foreground)) ylabel(none) || line yhatx if D ==0, color(red) sort || line yhat x if D ==1, sort color(red)xtitle("Test Score (X)") ytitle("Outcome (Y)")
Outcome (Y)050100150200250Test Score (X)
Figure: Third degree polynomial. Actual model second degree polynomial.

### Slide 370

Figure: P-order polynomial regressions with and without centering. Only intercept changes.

### Slide 371

Kernel regression
In addition to usingpth order polynomials to model the nonlinearities, we can
use kernel regression. Neither is right or wrong – they have advantages and
disadvantages.
The nonparametric kernel method has problems because you are trying to
estimate regressions at the cutoﬀ point which results in a “boundary problem”
(Hahn, Todd and Van der Klaauw 2001)
The Kernel Method
The nonparametric kernel method has its problems in this case
because you are trying to estimate regressions at the cuto § point.
This results in a "boundary problem".
While the "true" e §ect is AB, with a certain bandwidth a rectangular
kernel would estimate the e §ect as A’B’.
There is therefore systematic bias with the kernel method if the f (X )
is upwards or downwards sloping.
Waldinger (Warwick) 21 / 48
While the “true” eﬀect isAB, with a certain bandwidth a rectangular kernel
would estimate the eﬀect asA′B′
There is therefore systematic bias with the kernel method if thef(X) is
upwards or downwards sloping

### Slide 372

Kernel Method - Local linear regression
The standard solution to this problem is to run local linear nonparametric
regression (Hahn, Todd and Van der Klaauw 2001). In the case described by
the previous slide, this would substantially reduce the bias
Think of it as a weighted regression restricted to a window (hence “local”).
The kernel provides the weights to that regression. STATA’spolycommand
estimates kernel-weighted local polynomial regression.
A rectangular kernel would give the same result as takingE[Y] at a given bin
onX. The triangular kernel gives more importance to the observations closer
to the center.
While estimating this in a given window of widthh around the cutoﬀ is
straightforward, it’s more diﬃcult to choose this bandwidth (or window), and
the method is sensitive to the choice of bandwidth. There is essentially a
tradeoﬀ between bias and eﬃciency. See Lee and Lemieux (2010) for two
methods to choose on the bandwidth. This is an active area of research in
econometrics.

### Slide 373

Example of sharp RDD: David Card, Carlos Dobkin and Nicole
Maestas (2008), “The Impact of Nearly Universal Insurance
Coverage on Health Care Utilization: Evidence from Medicare”,
American Economic Review .

### Slide 374

Non-elderly health insurance information
20% of non elderly adults in US lacked health insurance
coverage in 2005
Most were from lower-income families, nearly one-half were
African American or Hispanic
Many analysts have argued that unequal insurance coverage
contributes to disparities in health care utilization and health
outcomes across SES
Even among the insured, there are diﬀerences: copayments,
deductibles, other features that aﬀect use
Evidence that better insurance causes better health outcomes
is limited
Both supply and demand for insurance depend on health
status, confounding observational comparisons between people
with diﬀerent insurance characteristics

### Slide 375

Elderly health insurance information
< 1% of the elderly population are uninsured
Most have fee-for-service Medicare coverage
The transition occurs abruptly at age 65, which is the
threshold for Medicare eligibility

### Slide 376

Estimation
Reduced form measure of causal eﬀect of health insurance
status on health care use, y:
yija = Xijaα + fj(α; β) +
∑
k
C k
ijaδk + uija
where i indexes individuals, j indexes a socioeconomic group,
a indexes an age, uija the unobserved error component, Xija a
set of covariates (e.g., gender and region), fj(α; β) a smooth
function representing the age proﬁle of outcome y for group j,
and C k
ija(k = 1, 2, . . . ,K ) are characteristics of the insurance
coverage held by the individual such as copayment rates
Problem: Insurance coverage is endogenous
→ cov(uija, Cija) ̸= 0.
Identiﬁcation: age threshold for Medicare eligibility at 65 used
as credibly exogenous variation in insurance status.

### Slide 377

Insurance and age
DECEMBER 20082244 THE AMERICAN ECONOMIC REVIEW
where hj 1a2 5 fj 1a2 1 d1g1
j 1a2 1 d2
 g2
j 1a2 represents the reduced-form age pro/f_ile for group j, p y
j  
5  p1
j d1 1 p2
j d2, and vy
ija 5  uija 1 v1
ija d1
  1 v2
ija d2  is an error term. Assuming that the pro /f_iles 
fj 1a2 , g1
j 1a2 , and g2
j 1a2 are all continuous at age 65, any discontinuity in y can be attributed to 
discontinuities in insurance. The magnitude depends on the size of the insurance changes at 65 
1p1
j and p2
j2 , and on the associated causal effects 1d1 and d22.
For some basic health care services—for example, routine doctor visits—it is arguable that 
only the presence of insurance matters. In this case, the implied discontinuity in y at age 65 for 
group j will be proportional to the change in insurance coverage experienced by the group. For 
more expensive or elective services, the generosity of coverage may also matter, if patients are 
unwilling to cover the required copayment or if managed care programs will not cover the ser -
vice. This creates a potential identi/f_ication problem in interpreting the discontinuity in y for any 
one group. Since py
j is a linear combination of the discontinuities in coverage and generosity, d1 
and d2 can be estimated by a regression across groups:
(4)  py
j  5  d 0 1 d1
 p1
j 1 d2
 p2
j 1 ej ,
where ej is an error term re/f_lecting a combination of the sampling errors in py
j , p1
j , and p2
j .
This framework can be extended to include additional dimensions of insurance coverage. In 
practice, however, a key limitation is the lack of information on the insurance packages held by dif-
ferent individuals. In the absence of more complete data, we use the presence of at least two forms 
of coverage as an indicator of “generous” coverage. We also explore a simple measure of gatekeeper 
limitations, based on whether an individual’s primary insurer is a managed care provider.
In our empirical analysis, we /f_it regression discontinuity (RD) models like (2a), (2b), and (3) by 
demographic subgroup to individual data using OLS estimators. We then combine the estimates 






                   
"HFJORVBSUFST
'SBDUJPOXJUIDPWFSBHF
"OZDPWFSBHFIJHIFEXIJUFT
"OZDPWFSBHFBMMHSPVQT
"OZDPWFSBHFMPXFENJOPSJUJFT
5XPQPMJDJFTIJHIFEXIJUFT
5XPQPMJDJFTBMMHSPVQT
5XPQPMJDJFTMPXFENJOPSJUJFT
F/i.sc/g.sc/u.sc/r.sc/e.sc 1. C/o.sc/v.sc/e.sc/r.sc/a.sc/g.sc/e.sc /b.sc/y.sc A/n.sc/y.sc I/n.sc/s.sc/u.sc/r.sc/a.sc/n.sc/c.sc/e.sc /a.sc/n.sc/d.sc /b.sc/y.sc T/w.sc/o.sc /o.sc/r.sc M/o.sc/r.sc/e.sc P/o.sc/l.sc/i.sc/c.sc/i.sc/e.sc/s.sc, /b.sc/y.sc A/g.sc/e.sc /a.sc/n.sc/d.sc D/e.sc/m.sc/o.sc/g.sc/r.sc/a.sc/p.sc/h.sc/i.sc/c.sc G/r.sc/o.sc/u.sc/p.sc

### Slide 378

Estimation
Suppose health insurance coverage can be summarized by two dummy variables
– C1ija (any coverage) andC2ija (generous insurance). Linear probability model:
C1ija = Xijaβ1j +g1j (a) +Daπ1j +v1ija
C2ija = Xijaβ2j +g2j (a) +Daπ2j +v2ija
whereβ1j andβ2j are group-speciﬁc coeﬃcients,g1j (a) andg2j (a) are smooth
age proﬁles for groupj, andDa is a dummy for being age≥65.
Reduced form from couple slides back
yija=Xijaα+fj(α;β) +∑
k
Ckijaδk +uija
CombiningCija equations, and rewrite reduced for model
yija=Xija(αj +β1j δ1j +β2j δ2j ) +hj(a) +Daπy
j +vy
ija
whereh(a) =fj(a) +δ1g1j (a) +δ2g2j (a) is reduced form age proﬁle for groupj,
πy
j =π1j δ1 +π2j δ2 andvy
ija=uija+v1ijaδ1 +v2ijaδ2 is the error term.
Assuming that the proﬁlesfj(a), g1j (a) andg2j (a) are continuous at age 65
(i.e., continuous assumption), then any discontinuity iny is due to insurance
and magnitudes depend on the size of the insurance changes at 65 (π1j andπ2j )
and on the associated causal eﬀects (δ1 andδ2)

### Slide 379

Estimation
For some basic health care services (e.g., routine doctor visits), it may be the
only thing that matters is insurance.
But, in those situations, the implied discontinuity iny at 65 for groupj will be
proportional to the change in insurance status experienced by that group
For more expensive (or elective) services, the generosity of the coverage may
matter (for instance, if patients are unwilling to cover the required copay or if
the managed care program won’t cover the service).
This creates a potential identiﬁcation problem in interpreting the discontinuity
in y for any one group
Sinceπy
j is a linear combination of the discontinuities in coverage and
generosity,δ1 andδ2 can be estimated by a regression across groups:
πy
j =δ0 +δ1π1j +δ2j +ej
whereej is an error term reﬂecting a combination of the sampling errors inπy
j ,
π1j andπ2j
RDD models like the ones on the previous slide are ﬁt with OLS by
demographic subgroup to individual data
Estimates are combined across groups to estimate the above model

### Slide 380

Data
1992-2003 National Health Interview Survey (NHIS)
NHIS reports respondents’ birth year and birth month, and the calendar quarter
of the interview. Authors construct an estimate of age in quarters at date of
interview.
A person reaching 65 in the interview quarter is age 65 and 0 quarters
Assuming a uniform distribution of interview dates, one-half of these people will
be 0-6 weeks younger than 65 and one-half will be 0-6 weeks older
Analysis is limited to people>55 and<75. Final sample size isn =160,821
(although some outcomes are not available for earlier years)
1992-2003 hospital discharge records for California, Florida and New York
Discharge ﬁles represent a complete census of discharges from all hospitals in the
three states (except federally regulated institutions).
Data ﬁles include information on age in months at the time of admission
Sample selection criteria: drop records for people admitted as transfers from
other institutions, and limit people between 60 and 70 years of age at admission
Sample sizes are 4,017,325 (California), 2,793,547 (Florida), 3,121,721 (New
York)

### Slide 381

Medicare description
Description of Medicare eligibility
Medicare is available to people who are at least 65 and have
worked 40 quarters or more in covered employment or have a
spouse who did.
Coverage is available to younger people with severe kidney
disease and recipients of Social Security Disability Insurance
Eligible individuals can obtain Medicare hospital insurance
(Part A) free of charge, and medical insurance (Part B) for a
modest monthly premium
Individuals receive notice of their impending eligibility for
Medicare shortly before 65 th birthday and are informed they
have to enroll in it and choose whether to accept Part B
coverage.
Coverage begins on the ﬁrst day of the month in which they
turn 65.

### Slide 382

Medicare description
Five insurance-related variables: probability of Medicare
coverage, any health insurance coverage, private coverage,
two or more forms of coverage, and individual’s primary health
insurance is managed care program.
Data are drawn from 1999-2003 NHIS and for each
characteristic, authors show the incidence rate at ages 63-64
and the change at age 65 based on a version of the CK
equations that include a quadratic in age, fully interacted with
a post-65 dummy as well as controls for gender, education,
race/ethnicity, region and sample year
Alternative speciﬁcations were also used – a parametric model
ﬁt to a narrower age window (ages 63-67) and a local linear
regression speciﬁcation using a chosen bandwidth. Both show
show similar estimates of the change at age 65.

### Slide 383

Changes in insurance coverage at age 65
DECEMBER 20082246 THE AMERICAN ECONOMIC REVIEW
Medicare coverage rises by 60 percentage points at age 65, from a base level of 12 percent 
among 63- 64-year-olds. Consistent with DI enrollment patterns (David H. Autor and Mark G. 
Duggan 2003), Medicare enrollment prior to 65 is higher for minorities and people with below-
average schooling, and these groups experience relatively smaller gains at age 65 (see rows 2–7). 
The pattern is reversed for the probability of any insurance coverage (columns 3 and 4): groups 
with lower insurance coverage rates prior to 65 experience larger gains at age 65. There is still 
some disparity in insurance coverage after 65, but the 28-point gap between more educated whites 
and less  educated minorities narrows to about 10 points. Similarly, as shown in rows 8–10, the 
21-point gap in coverage between whites and Hispanics before age 65 closes to only 12 points after. 
Thus, the onset of Medicare eligibility dramatically reduces disparities in insurance coverage.
Columns 5 and 6 present information on the prevalence of private insurance coverage (i.e., 
employer-provided or purchased coverage). Prior to age 65 private coverage rates range from 33 
percent for less educated minorities to 86 percent for better educated whites. The RD estimates 
in column 6 show that these differences are hardly affected by the onset of Medicare eligibility. 
This stability re /f_lects the fact that most people who hold private coverage before 65 transition 
to a combination of Medicare and supplemental coverage, either through an employer-provided 
plan or an individually purchased Medigap policy. 3 Columns 7 and 8 of Table 1 analyze the 
age patterns of multiple coverage (i.e., reporting two or more policies). Prior to age 65, the rate 
3  Across the six groups in rows 2–7 of Table 1, for example, the correlation between the private coverage rate at ages 
63–64 shown in column 5 and the fraction of 65- 66-year-olds with private supplemental Medicare coverage is 0.97.
T/a.sc/b.sc/l.sc/e.sc 1—I/n.sc/s.sc/u.sc/r.sc/a.sc/n.sc/c.sc/e.sc C/h.sc/a.sc/r.sc/a.sc/c.sc/t.sc/e.sc/r.sc/i.sc/s.sc/t.sc/i.sc/c.sc/s.sc J/u.sc/s.sc/t.sc /b.sc/e.sc/f.sc/o.sc/r.sc/e.sc A/g.sc/e.sc 65 /a.sc/n.sc/d.sc E/s.sc/t.sc/i.sc/m.sc/a.sc/t.sc/e.sc/d.sc D/i.sc/s.sc/c.sc/o.sc/n.sc/t.sc/i.sc/n.sc/u.sc/i.sc/t.sc/i.sc/e.sc/s.sc /a.sc/t.sc A/g.sc/e.sc 65
On Medicare Any insurance Private coverage 21 Forms coverage Managed care
Age
63–4
RD
 at 65
Age
 63–4
RD
 at 65
Age 
63–4
RD 
at 65
Age 
63–4
RD 
at 65
Age
 63–4
RD
 at 65
112 122 132 142 152 162 172 182 192 1102
Overall sample 12.3 59.7 87.9 9.5 71.8 22.9 10.8 44.1 59.4 228.4
14.12 10.62 11.12 12.82 12.12
Classi/f_ied by ethnicity and education:
White non-Hispanic:
 High school dropout 21.1 58.5 84.1 13.0 63.5 26.2 15.0 44.5 48.1 225.0
14.62 12.72 13.32 14.02 14.52
 High school graduate 11.4 64.7 92.0 7.6 80.5 21.9 10.1 51.8 58.9 230.3
15.02 10.72 11.62 13.82 12.62
 At least some college 6.1 68.4 94.6 4.4 85.6 22.3 8.8 55.1 69.1 240.1
14.72 10.52 11.82 14.02 12.62
Minority:
 High school dropout 19.5 44.5 66.8 21.5 33.2 21.2 11.4 19.4 39.1 28.3
13.12 12.12 12.52 11.92 13.12
 High school graduate 16.7 44.6 85.2 8.9 60.9 25.8 13.6 23.4 54.2 215.4
14.72 12.82 15.12 14.82 13.52
 At least some college 10.3 52.1 89.1 5.8 73.3 25.4 11.1 38.4 66.2 222.3
14.92 12.02 14.32 13.82 17.22
Classi/f_ied by ethnicity only:
White non-Hispanic 10.8 65.2 91.8 7.3 79.7 22.8 10.4 51.9 61.9 233.6
 1all 2 14.62 10.52 11.42 13.52 12.32
Black non-Hispanic 17.9 48.5 84.6 11.9 57.1 24.2 13.4 27.8 48.2 213.5
 1all 2 13.62 12.02 12.82 13.72 13.72
Hispanic 1all 2 16.0 44.4 70.0 17.3 42.5 22.0 10.8 21.7 52.9 212.1
13.72 13.02 11.72 12.12 13.72
Note: Entries in odd-numbered columns are percentages of age 63- 64-year-olds in group with insurance characteristic 
shown in column heading. Entries in even-numbered columns are estimated regression discontinuties at age 65, from 
models that include quadratic control for age, fully interacted with dummy for age 65 or older. Other controls include 
indicators for gender, race/ethnicity, education, region, and sample year. Estimates are based on linear probability 
 models /f_it to pooled samples of 1999–2003 NHIS.

### Slide 384

Other changes at age 65
Formal identiﬁcation of an RD model relating an outcomey (insurance
coverage) to a treatment (Medicare age-eligibility) that depends on age (a)
relies on assumption that the conditional expectation functions for both
potential outcomes is continuous ata = 65. That is,E[Y0|a] andE[Y1|a] are
continuous ata = 65. If so, then the average treatment eﬀect at age 65 is
identiﬁed as
lim65←aE[y1|a] −lima→65E[y0|a]
Continuity requires that all other factors that might aﬀect the outcome of
interest (insurance coverage) also trend smoothly at the cutoﬀa = 65.
What else changes, though, at 65?
Employment. 65 is the traditional age of retirements. Any abrupt change in
employment could lead to diﬀerences in health care utilization if non workers have
more time to visit doctors.
Test for potential discontinuities at age 65 for confounding variables using the
1996-2004 March CPS with age measured in years. No evidence for large
discontinuities in employment (next slide).

### Slide 385

Other changes at age 65
VOL. 98 NO. 5 2249CARD ET AL: THE EFFECT OF MEDICARE ON HEALTH CARE UTILIZATION
 gender, and show no large discontinuities for either men or women. 6 As an additional check, 
we used longitudinal data from the 2001 Survey of Income and Program Participation to esti -
mate month-to-month changes in individual employment (see the online Appendix). Consistent 
with the results here, we found no evidence of a discontinuity in employment at age 65. We also 
investigated the age pro/f_iles of marriage, being classi /f_ied as poor, and receiving food stamps in 
the NHIS, as well as residential mobility, marital status, and the incidence of low income in the 
CPS. As summarized in the online Appendix to this paper, none of these outcomes shows sig -
ni/f_icant discontinuities at age 65 for the overall sample or the subgroups used in Tables 1 and 2. 
We conclude that employment, family structure, family income, and location, taken as a whole,  
all trend relatively smoothly at age 65, and are unlikely to confound our analysis of the impact 
of Medicare eligibility.
III. Changes in Health Care Access and Utilization at Age 65
We now turn to an analysis of the effects of reaching age 65 on access to care and utilization 
of health care services. Since 1997 the NHIS has asked two questions: (1) “During the past 12 
months has medical care been delayed for this person because of worry about the cost?” and (2) 
“During the past 12 months was there any time when this person needed medical care but did not 
get it because (this person) could not afford it?” Columns 1 and 3 of Table 2 show the fractions of 
people ages 63–64 in the pooled 1997–2003 NHIS who responded positively to these two ques-
tions. Overall, about 7 percent of the near-elderly reported delaying care, and 5 percent reported 
not getting care, with relatively higher rates for less educated minorities and for Hispanics. Our 
RD estimates in columns 2 and 4 imply signi/f_icant declines at age 65 in both measures of access 
6 Graphs similar to Figure 2 by gender are available in our online Appendix.










                   
"HF
'SBDUJPOFNQMPZFE
)JHIFEXIJUFTBDUVBM
)JHIFEXIJUFTQSFEJDUFE
0WFSBMMTBNQMFBDUVBM
0WFSBMMTBNQMFQSFEJDUFE
-PXFENJOPSJUJFTBDUVBM
-PXFENJOPSJUJFTQSFEJDUFE
F/i.sc/g.sc/u.sc/r.sc/e.sc 2. E/m.sc/p.sc/l.sc/o.sc/y.sc/m.sc/e.sc/n.sc/t.sc R/a.sc/t.sc/e.sc/s.sc /b.sc/y.sc A/g.sc/e.sc /a.sc/n.sc/d.sc D/e.sc/m.sc/o.sc/g.sc/r.sc/a.sc/p.sc/h.sc/i.sc/c.sc G/r.sc/o.sc/u.sc/p.sc (1992–2003 NHIS)

### Slide 386

Eﬀect of cutoﬀ on access to care and utilization
Since 1997, NHIS has asked two questions: (1) “During the past 12 months has medical care been delayed
for this person because of worry about the cost?” and (2) “‘During the past 12 months was there any time
when this person needed medical care but did not get it because (this person) could not aﬀord it?”
DECEMBER 20082250 THE AMERICAN ECONOMIC REVIEW
problems, especially for less educated minorities and Hispanics. The onset of Medicare eligibil-
ity leads to a fall in cost-related access problems and a narrowing of intergroup disparities in 
access.7
The right-hand columns of Table 3 present results for two key measures of health care utili -
zation: (1) “Did the individual have at least one doctor visit in the past year?” and (2) “Did the 
individual have one or more overnight hospital stays in the past year?” based on pooled samples 
of the 1992–2003 NHIS. Inspection of the utilization rates among 63- 64-year-olds shows a well-
known fact: less educated and minority groups are less likely to have a routine doctor visit than 
better educated and nonminority groups, but more likely to have had a hospital spell. The RD 
estimates in column 6 suggest that the age 65 threshold is associated with a (modest) increase in 
routine doctor visits, with relatively larger gains for the groups with lower rates before 65. 8 For 
example, among the near-elderly there is a 7.4 percentage point gap in the probability of a routine 
7 Because the questions refer to the previous year, our estimates of the effect of reaching 65 on access problems may 
be attenuated. Speci /f_ically, people who recently turned 65 could have had problems in the past year, but before their 
birthday. Such attenuation may be reduced if people tend to recall only their most recent experiences.
8  Lichtenberg (2002) also found a discontinuous rise in physician visits in the National Ambulatory Medical Care 
Surveys, but did not disaggregate visits by race/ethnic group.
T/a.sc/b.sc/l.sc/e.sc 3—M/e.sc/a.sc/s.sc/u.sc/r.sc/e.sc/s.sc /o.sc/f.sc A/c.sc/c.sc/e.sc/s.sc/s.sc /t.sc/o.sc C/a.sc/r.sc/e.sc J/u.sc/s.sc/t.sc /b.sc/e.sc/f.sc/o.sc/r.sc/e.sc 65 /a.sc/n.sc/d.sc E/s.sc/t.sc/i.sc/m.sc/a.sc/t.sc/e.sc/d.sc D/i.sc/s.sc/c.sc/o.sc/n.sc/t.sc/i.sc/n.sc/u.sc/i.sc/t.sc/i.sc/e.sc/s.sc /a.sc/t.sc 65
1997–2003 NHIS 1992–2003 NHIS
Delayed care last year Did not get care last year Saw doctor last year Hospital stay last year
Age 63–64 RD at 65 Age 63–64 RD at 65 Age 63264 RD at 65 Age 63264 RD at 65
112 122 132 142 152 162 172 182
Overall sample 7.2 21.8 4.9 21.3 84.8 1.3 11.8 1.2
10.42 10.32 10.72 10.42
Classi/f_ied by ethnicity and education:
White non-Hispanic:
 High school dropout 11.6 21.5 7.9 20.2 81.7 3.1 14.4 1.6
11.12 11.02 11.32 11.32
 High school graduate 7.1 0.3 5.5 21.3 85.1 20.4 12.0 0.3
12.82 12.82 11.52 10.72
 At least some college 6.0 21.5 3.7 21.4 87.6 0.0 9.8 2.1
10.42 10.32 11.32 10.72
Minority:
 High school dropout 13.6 25.3 11.7 24.2 80.2 5.0 14.5 0.0
11.02 10.92 12.22 11.42
 High school graduate 4.3 23.8 1.2 1.5 84.8 1.9 11.4 1.8
13.22 13.72 12.72 11.42
 At least some college 5.4 20.6 4.8 20.2 85.0 3.7 9.5 0.7
11.12 10.82 13.92 12.02
Classi/f_ied by ethnicity only:
White non-Hispanic 6.9 21.6 4.4 21.2 85.3 0.6 11.6 1.3
10.42 10.32 10.82 10.52
Black non-Hispanic 1all 2 7.3 21.9 6.4 20.3 84.2 3.6 14.4 0.5
11.12 11.12 11.92 11.12
Hispanic 1all 2 11.1 24.9 9.3 23.8 79.4 8.2 11.8 1.0
10.82 10.72 10.82 11.62
Note: Entries in odd numbered columns are mean of variable in column heading among people ages 63–64. Entries in 
even numbered columns are estimated regression discontinuties at age 65, from models that include linear control for 
age interacted with dummy for age 65 or older (columns 2 and 4) or quadratic control for age, interacted with dummy 
for age 65 and older (columns 6 and 8). Other controls in models include indicators for gender, race/ethnicity, educa -
tion, region, and sample year. Sample in columns 1–4 is pooled 1997–2003 NHIS. Sample in columns 5–8 is pooled 
1992–2003 NHIS. Samples for regression models include people ages 55–75 only. Standard errors (in parentheses) are 
clustered by quarter of age.

### Slide 387

Eﬀect of cutoﬀ on access to care and utilization
The right-hand columns of Table 3 present results for two key measures of healthcare utilization: (1) “Did
the individual have at least one doctor visit in the past year?” and (2) “Did the individual have one or more
overnight hospital stays in the past year?”
DECEMBER 20082250 THE AMERICAN ECONOMIC REVIEW
problems, especially for less educated minorities and Hispanics. The onset of Medicare eligibil-
ity leads to a fall in cost-related access problems and a narrowing of intergroup disparities in 
access.7
The right-hand columns of Table 3 present results for two key measures of health care utili -
zation: (1) “Did the individual have at least one doctor visit in the past year?” and (2) “Did the 
individual have one or more overnight hospital stays in the past year?” based on pooled samples 
of the 1992–2003 NHIS. Inspection of the utilization rates among 63- 64-year-olds shows a well-
known fact: less educated and minority groups are less likely to have a routine doctor visit than 
better educated and nonminority groups, but more likely to have had a hospital spell. The RD 
estimates in column 6 suggest that the age 65 threshold is associated with a (modest) increase in 
routine doctor visits, with relatively larger gains for the groups with lower rates before 65. 8 For 
example, among the near-elderly there is a 7.4 percentage point gap in the probability of a routine 
7 Because the questions refer to the previous year, our estimates of the effect of reaching 65 on access problems may 
be attenuated. Speci /f_ically, people who recently turned 65 could have had problems in the past year, but before their 
birthday. Such attenuation may be reduced if people tend to recall only their most recent experiences.
8  Lichtenberg (2002) also found a discontinuous rise in physician visits in the National Ambulatory Medical Care 
Surveys, but did not disaggregate visits by race/ethnic group.
T/a.sc/b.sc/l.sc/e.sc 3—M/e.sc/a.sc/s.sc/u.sc/r.sc/e.sc/s.sc /o.sc/f.sc A/c.sc/c.sc/e.sc/s.sc/s.sc /t.sc/o.sc C/a.sc/r.sc/e.sc J/u.sc/s.sc/t.sc /b.sc/e.sc/f.sc/o.sc/r.sc/e.sc 65 /a.sc/n.sc/d.sc E/s.sc/t.sc/i.sc/m.sc/a.sc/t.sc/e.sc/d.sc D/i.sc/s.sc/c.sc/o.sc/n.sc/t.sc/i.sc/n.sc/u.sc/i.sc/t.sc/i.sc/e.sc/s.sc /a.sc/t.sc 65
1997–2003 NHIS 1992–2003 NHIS
Delayed care last year Did not get care last year Saw doctor last year Hospital stay last year
Age 63–64 RD at 65 Age 63–64 RD at 65 Age 63264 RD at 65 Age 63264 RD at 65
112 122 132 142 152 162 172 182
Overall sample 7.2 21.8 4.9 21.3 84.8 1.3 11.8 1.2
10.42 10.32 10.72 10.42
Classi/f_ied by ethnicity and education:
White non-Hispanic:
 High school dropout 11.6 21.5 7.9 20.2 81.7 3.1 14.4 1.6
11.12 11.02 11.32 11.32
 High school graduate 7.1 0.3 5.5 21.3 85.1 20.4 12.0 0.3
12.82 12.82 11.52 10.72
 At least some college 6.0 21.5 3.7 21.4 87.6 0.0 9.8 2.1
10.42 10.32 11.32 10.72
Minority:
 High school dropout 13.6 25.3 11.7 24.2 80.2 5.0 14.5 0.0
11.02 10.92 12.22 11.42
 High school graduate 4.3 23.8 1.2 1.5 84.8 1.9 11.4 1.8
13.22 13.72 12.72 11.42
 At least some college 5.4 20.6 4.8 20.2 85.0 3.7 9.5 0.7
11.12 10.82 13.92 12.02
Classi/f_ied by ethnicity only:
White non-Hispanic 6.9 21.6 4.4 21.2 85.3 0.6 11.6 1.3
10.42 10.32 10.82 10.52
Black non-Hispanic 1all 2 7.3 21.9 6.4 20.3 84.2 3.6 14.4 0.5
11.12 11.12 11.92 11.12
Hispanic 1all 2 11.1 24.9 9.3 23.8 79.4 8.2 11.8 1.0
10.82 10.72 10.82 11.62
Note: Entries in odd numbered columns are mean of variable in column heading among people ages 63–64. Entries in 
even numbered columns are estimated regression discontinuties at age 65, from models that include linear control for 
age interacted with dummy for age 65 or older (columns 2 and 4) or quadratic control for age, interacted with dummy 
for age 65 and older (columns 6 and 8). Other controls in models include indicators for gender, race/ethnicity, educa -
tion, region, and sample year. Sample in columns 1–4 is pooled 1997–2003 NHIS. Sample in columns 5–8 is pooled 
1992–2003 NHIS. Samples for regression models include people ages 55–75 only. Standard errors (in parentheses) are 
clustered by quarter of age.

### Slide 388

Changes in hospitalizations
DECEMBER 20082252 THE AMERICAN ECONOMIC REVIEW
Hispanic-white differences in admission rates narrow at age 65, as whites gain relative to blacks 
and Hispanics gain relative to whites.
For reference, the bottom row of Table 4 shows insurance coverage rates among 60- 64-year-
olds in the three states, along with estimated jumps in insurance coverage at 65.11 Coverage rates 
in the three states are below the national average prior to 65, but rise by more (15 percent versus 
a national average of about 10 percent). Consistent with the national data in Table 1, the gains in 
insurance coverage in the three states are largest for Hispanics (20.3 versus 17.3 percent nation-
ally), a little smaller for blacks (17.6 versus 11.9 percent nationally) and smallest for whites (12.7 
versus 7.3 percent nationally).
A key advantage of our hospital data is that we can break down admissions by route into 
the hospital, and by admission diagnosis and primary procedure. A comparison of rows 2 and 
3 in Table 4 shows that most of the jump in admissions at age 65 is driven by non–emergency 
room admissions, although for each race/ethnic group there is also some increase in ER admis -
sions.12 Further insights can be gleaned from the admissions patterns across diagnoses. The most 
common admission diagnosis for near-elderly patients is chronic ischemic heart disease (IHD), 
which is often treated by coronary artery bypass surgery. There are substantial disparities in IHD 
11 These data are drawn from the 1996–2004 CPS data for California, New York, and Florida. Given the small 
sample sizes and the coarseness of the age measure in the CPS, we estimated the insurance RDs assuming a linear age 
pro/f_ile but allowing a different slope before and after 65.
12 ER admissions include extremely urgent cases (which one might expect to be unresponsive to insurance status) 
as well as patients who have presented at the ER without being referred by a physician. Some analysts have argued that 
provision of health insurance would reduce ER use and shift patients to outpatient care. Nevertheless, our results are 
consistent with the RAND Health Insurance Experiment, which found ER use as responsive to copayment rates as use 
of outpatient care (Joseph P. Newhouse 1993). 






          
"HF
5PUBMIPTQJUBMBENJTTJPOTQFS
QFSTPOZFBST







)PTQJUBMBENJTTJPOTGPSIJQBOELOFFSFQMBDFNFOUQFS

QFSTPOZFBST
"MMBENJTTJPOT	MFGUTDBMF
"MMBENJTTJPOTQSFEJDUFE
8IJUFIJQBOELOFFSFQMBDFNFOU8IJUFIJQBOELOFFSFQMBDFNFOUGJUUFE
)JTQBOJDIJQBOELOFFSFQMBDFNFOU)JTQBOJDIJQBOELOFFSFQMBDFNFOUGJUUFE
#MBDLIJQBOELOFFSFQMBDFNFOU#MBDLIJQBOELOFFSFQMBDFNFOUGJUUFE
F/i.sc/g.sc/u.sc/r.sc/e.sc 3. H/o.sc/s.sc/p.sc/i.sc/t.sc/a.sc/l.sc A/d.sc/m.sc/i.sc/s.sc/s.sc/i.sc/o.sc/n.sc R/a.sc/t.sc/e.sc/s.sc /b.sc/y.sc R/a.sc/c.sc/e.sc/E/t.sc/h.sc/n.sc/i.sc/c.sc/i.sc/t.sc/y.sc

### Slide 389

Eﬀect of cutoﬀ on access to care and utilization
VOL. 98 NO. 5 2253CARD ET AL: THE EFFECT OF MEDICARE ON HEALTH CARE UTILIZATION
admission rates prior to age 65, with a black-white relative admission rate of only 72 percent (see 
row 4). Overall admissions for this diagnosis rise by 12 percent at age 65, with the largest pro -
portional rise for Hispanics (who have the lowest base rate) and the smallest gain for blacks. The 
onset of Medicare eligibility therefore leads to a surprising widening of the black-white disparity 
in admission rates for IHD, but a slight narrowing of the Hispanic-white disparity.
The second most common admission diagnosis is acute myocardial infarction (AMI or heart 
attack). AMI admission rates among 60- 64-year-olds are fairly similar for whites and blacks, 
T/a.sc/b.sc/l.sc/e.sc 4—H/o.sc/s.sc/p.sc/i.sc/t.sc/a.sc/l.sc A/d.sc/m.sc/i.sc/s.sc/s.sc/i.sc/o.sc/n.sc/s.sc /a.sc/n.sc/d.sc I/n.sc/s.sc/u.sc/r.sc/a.sc/n.sc/c.sc/e.sc C/o.sc/v.sc/e.sc/r.sc/a.sc/g.sc/e.sc /a.sc/t.sc A/g.sc/e.sc 65: C/a.sc/l.sc/i.sc/f.sc/o.sc/r.sc/n.sc/i.sc/a.sc, F/l.sc/o.sc/r.sc/i.sc/d.sc/a.sc, /a.sc/n.sc/d.sc N/e.sc/w.sc Y/o.sc/r.sc/k.sc 
All Whites Hispanics Blacks
Rate age 
60–64
RD at 65 Rate age 
60–64
RD at 65 Rate age 
60–64
RD at 65 Rate age 
60–64
RD at 65
112 122 132 142 152 162 172 182
Hospital admissions
All admissions 1,443 7.57 1,407 7.74 1,262 9.47 2,008 4.39
10.292 10.332 10.552 10.712
By route into hospital
 ER admission 761 3.30 688 3.70 774 2.63 1,313 1.93
 10.392 10.402 10.922 10.952
 Non–ER admission 682 12.16 718 11.51 488 19.89 695 8.92
10.462 10.492 11.052 11.042
By admission diagnosis
 Chronic ischemic heart disease 83 11.58 90 11.05 59 18.45 66 8.29
10.962 11.162 12.452 12.782
 AMI 48 4.41 50 5.31 38 3.90 45 23.43
11.432 11.652 13.332 14.782
 Heart failure 56 0.44 45 2.33 62 24.85 130 21.47
11.112 11.242 12.632 12.432
 Chronic bronchitis 34 7.50 36 6.50 19 9.76 38 13.05
11.512 11.522 15.582 14.432
 Osteoarthrosis 34 26.97 38 27.16 18 29.27 27 22.08
11.392 11.642 15.052 14.012
 Pneumonia 34 2.44 32 2.05 30 3.39 51 3.81
11.422 11.742 14.342 13.212
By primary procedure
 None 419 5.70 400 5.73 388 7.23 614 3.86
10.332 10.402 11.232 11.252
 Diagnostic procedures on heart 51 9.18 53 8.17 40 16.78 58 8.76
11.032 11.202 13.212 13.322
 Removal of coronary artery obstruction 38 10.67 43 10.49 23 18.77 22 0.49
11.462 11.602 13.942 15.332
 Bypass anastomosis of heart 26 15.91 29 16.17 17 18.97 13 5.15
11.392 11.442 15.622 16.092
 Joint replacement lower extremity 41 22.69 46 23.16 22 26.40 33 12.14
11.472 11.602 14.692 14.202
 Diagnostic procedure on small intestine 35 7.35 31 6.60 37 13.07 58 4.09
11.272 11.472 13.272 13.132
 Cholecystectomy 1gall bladder removal 2 26 17.93 26 16.00 29 29.25 18 12.27
12.102 11.842 15.112 17.502
Insurance coverage
 Probability of coverage 82.7 15.0 86.7 12.7 69.1 20.3 79.0 17.6
 1March CPS data 2 10.82 10.82 12.72 12.72
Notes: Insurance estimates are based on pooled March CPS 1996–2004 data for California, Florida, and New York. 
Entries in top row columns 1, 3, 5, 7 are fractions of 60- and 64-year-olds with insurance coverage. Entries in top row 
columns 2, 4, 6, 8 represent regression discontinuity estimates 131002 of the increase in coverage at age 65 from a 
model with a quadratic in age, fully interacted with a post-65 dummy. Entries in lower rows columns 1, 3, 5, 7 are hospi-
tal admission rates 1 per 10,000 person years for 60- to 64-year-olds 2 for California, Florida, and New York 1992–2002.  
Entries in lower rows columns 2, 4, 6, 8 are regression discontinuity estimates 131002 of the increase in the log of the 
number of admissions at age 65, from models with a quadratic in age, fully interacted with a post-65 dummy. Standard 
errors are in parentheses.

### Slide 390

Changes in ownership
VOL. 98 NO. 5 2255CARD ET AL: THE EFFECT OF MEDICARE ON HEALTH CARE UTILIZATION
gall bladder removal). The /f_irst group of admissions tends to increase only slightly at age 65, with 
similar gains across groups. The second group rises by more, with a pattern that often widens 
existing racial/ethnic disparities—especially between blacks and whites. This contrast suggests 
there is an interaction between the availability and generosity of insurance, on one hand, and 
the existence of speci /f_ic surgical procedures, on the other, that lead to differences in rises in 
hospital admissions once Medicare becomes available. In this light, it is interesting to note that 
the increase in admissions for people who receive no procedures is below the overall growth in 
admissions (5.7 percent versus an overall average of 7.6 percent).
Further evidence that supply-side reactions to the changes in insurance status at age 65 play 
a role is presented in Figure 4, which plots the age pro /f_iles of hospital admissions by hospital 
ownership type in California. 14 Private nonpro/f_it hospitals (the largest category) and private for-
 pro/f_its (the second largest category) experience relatively large increases in admissions at age 
65. By comparison, hospitals owned by Kaiser Permanente (a large and long-established HMO) 
show little change in admissions at age 65, and county hospitals experience a sharp decline. 
These patterns point to two important conclusions. First, the jump at age 65 in overall hospital 
admissions masks a signi /f_icant redistribution of caseloads across hospitals. Once Medicare is 
available, some patients no longer have to use county hospitals and choose a private alternative. 
Second, the lack of any discontinuity at the Kaiser hospitals suggests that changes in managed 
care status may have some role in explaining the rise in admissions in the hospital system as a 
whole. In particular, Kaiser patients remain under a similar managed care regime before and 
after 65, and these patients show no rise in hospitalization at 65, whereas other patients appear to 
14 Hospital ownership data are available in our California /f_iles only. 

















          
"HF
1SJWBUFOPOQSPGJUBENJTTJPOT















'PSQSPGJU
DPVOUZ
,BJTFS
DIVSDI
BENJTTJPOT
$PVOUZ
)PTQJUBMEJTUSJDU
,BJTFS
1SJWBUFOPOQSPGJU
1SJWBUFGPSQSPGJU
$IVSDI3VO
F/i.sc/g.sc/u.sc/r.sc/e.sc 4. H/o.sc/s.sc/p.sc/i.sc/t.sc/a.sc/l.sc A/d.sc/m.sc/i.sc/s.sc/s.sc/i.sc/o.sc/n.sc /i.sc/n.sc C/a.sc/l.sc/i.sc/f.sc/o.sc/r.sc/n.sc/i.sc/a.sc /b.sc/y.sc O/w.sc/n.sc/e.sc/r.sc/s.sc/h.sc/i.sc/p.sc T/y.sc/p.sc/e.sc (1992–2002)

### Slide 391

Socioeconomic disparities
VOL. 98 NO. 5 2257CARD ET AL: THE EFFECT OF MEDICARE ON HEALTH CARE UTILIZATION
nonminority groups, as people with insurance prior to 65 enroll in fee-for-service Medicare and 
supplementary coverage plans.
We /f_ind that the onset of Medicare eligibility leads to increases in the use of medical care ser-
vices, with a pattern of gains across groups that varies with the type of service. Routine doctor 
visits and access to care increase more for groups that previously lacked coverage, and experi -
ence the largest gains in coverage at age 65. Overall hospitalizations increase sharply, but the pat-
terns of gains across groups differ by type of admission. For certain elective hospital admissions, 
including hip and knee replacements and bypass surgery, the increases are larger for groups that 
are more likely to have Medicare combined with supplemental coverage after 65. For other con-
ditions like heart failure that are mainly treated by drugs, all groups show very small increases 
in hospitalization rates at 65. Coupled with evidence of a redistribution of the caseload across 
hospitals of different ownership types, these patterns suggest that the rise in hospitalization at 65 
is driven by an interaction between the increase in insurance “generosity” at 65—speci/f_ically for 
groups who move to fee-for-service Medicare with supplemental coverage—and the existence of 
pro/f_itable treatments (like bypass surgery) for certain diagnoses.
T/a.sc/b.sc/l.sc/e.sc 5—S/u.sc/m.sc/m.sc/a.sc/r.sc/y.sc /o.sc/f.sc E/f.sc/f.sc/e.sc/c.sc/t.sc/s.sc /o.sc/f.sc I/n.sc/s.sc/u.sc/r.sc/a.sc/n.sc/c.sc/e.sc C/o.sc/v.sc/e.sc/r.sc/a.sc/g.sc/e.sc /o.sc/n.sc S/o.sc/c.sc/i.sc/o.sc/e.sc/c.sc/o.sc/n.sc/o.sc/m.sc/i.sc/c.sc D/i.sc/s.sc/p.sc/a.sc/r.sc/i.sc/t.sc/i.sc/e.sc/s.sc
Disparities at ages 63–64
Percent change in disparity due to 
change in coverage at 65
Coef/f_icient on 
coverage RD R2
Low-ed minority-
Hi-ed Whites
Black- 
White
Hispanic-
White
Low-ed minority-
Hi-ed Whites
Black- 
White
Hispanic-
White
Outcome (1) (2) (3) (4) (5) (6) (7) (8)
Panel A: Based on change in insurance coverage at 65
Delay in care last year 20.19 0.72 7.6 — — 242.8 — —
(0.06)
No care last year 20.12 0.39 8.0 — — 225.6 — —
(0.06)
Regular doctor visit last year 0.32 0.77 27.4 — — 273.9 — —
(0.09)
Hospital stay last year 20.09 0.26 4.7 — — 232.7 — —
(0.08)
Total hospital admissions 0.06 0.74 — 724 2193 — 1.8 20.5
(0.18)
Diagnostic procedures 0.59 0.62 — 9 225 — 35.6 4.5
 of the heart (0.31)
Bypass anastomosis of heart 20.54 0.54 — 218 214 — 24.9 24.8
(0.93)
Joint replacement of lower 20.19 0.89 — 27 228 — 2.7 22.3
 extremity (0.64)
Panel B: Based on change in incidence of multiple coverage at 65
Total hospital admissions 0.03 0.74 — 724 2193 — 20.1 5.8
(0.08)
Diagnostic procedures 20.21 0.55 — 9 225 — 22.3 218.7
 of the heart (0.14)
Bypass anastomosis 0.46 0.64 — 218 214 — 29.4 37.1
 of heart (0.34)
Joint replacement lower 0.42 0.94 — 27 228 — 59.4 25.7
 of extremity (0.21)
Notes: Each entry in panel A, column 1, is estimated coef /f_icient from regression of RDs in listed health outcome on 
RDs in insurance coverage over six ethnicity/education groups (rows 1–4) or nine state-ethnicity groups (rows 5–8).  
All regressions weighted by the inverse sampling variance of the estimated discontinuity in each outcome, and regres -
sions in rows 5–8 include state dummies. Entries in column 2 are corresponding R-squared coef /f_icients from each 
regression.  Entries in columns 3, 4, and 5 are the observed disparities in each health outcome at ages 63–64, and entries 
in columns 6, 7, and 8 are the percent change in the disparity attributable to the change in insurance coverage based on 
the coef/f_icient in column 1. Health disparities measured in the NHIS are characterized in terms of low-ed minorities 
versus hi-ed whites, whereas health disparities measured in the hospital discharge data are characterized in terms of 
black-white or hispanic-white differences.  Panel B is similar to panel A except that the RDs in each health outcome are 
regressed on the RDs in the incidence of multiple coverage at 65. Panel B regressions are based on data for New York 
and Florida only (i.e., six state-ethnicity groups).

### Slide 392

Summary of Card, et al. (2009)
David Card, Carlos Dobkin and Nicole Maestas (2009), “Does
Medicare Save Lives?”, American Economic Review

### Slide 393

Impact of Medicare on admissions
DOES MEDICARE SAVE LIVES? 605
5.90
5.95
6.00
6.05
6.10
6.15
6.20
6.25
6.30
6.35
6.40
60 61 62 63 64 65 66 67 68 69 70
Age in months
Log(admissions)
Unplanned ED admission Unplanned ED admissions fitted
Other admissions Other admissions fitted
FIGURE II
Number of Admissions by Route into Hospital, California, 1992–2002
The lines are ﬁtted values from regressions that include a second-order polyno-
mial in age fully interacted with a dummy for age ≧65 and a dummy variable for
the month before people turn 65. The dependent variable is the log of the number
of admissions by patient’s age (in days) at admission, for patients between 60 and
70 years of age. The count of admissions is based on hospital discharge records for
California and includes admissions from January 1, 1992, to November 30, 2002.
The points represent means of the dependent variable for 30-day cells. The age
proﬁle for unplanned ED admissions includes admissions that occurred through
the emergency department and were unplanned. The category “Other Admissions”
includes all other admissions.
leads to an increase in hospitalization rates (Card, Dobkin, and
Maestas 2008), this is a serious threat to an RD analysis of the
health outcomes of patients. Figure II illustrates the difﬁculty
in using counts of hospital admissions based on California dis-
charge records from 1992 to 2002. (The sample is described more
precisely below .) At age 65 the number of non-ED admissions
jumps by approximately 12%, whereas the number of ED admis-
sions rises by 3%. Assuming that the additional patients are not
as sick as those who would enter the hospital regardless of Medi-
care eligibility , the average health of patients rises discretely at
age 65.
In this paper we attempt to solve the sample selection prob-
lem by focusing on a subset of patients who are admitted through
the ED for a relatively severe set of conditions that require imme-
diate hospitalization. Speciﬁcally , we identify a set of admission
 at University of Kentucky Libraries on November 29, 2011http://qje.oxfordjournals.org/Downloaded from

### Slide 394

Proportion with coverage
DOES MEDICARE SAVE LIVES? 615
0
0.1
0.2
0.3
0.4
0.5
0.6
0.7
0.8
0.9
1
60 61 62 63 64 65 66 67 68 69 70
Age at admission
Proportion with coverage
Private Private fitted
Medicare Medicare fitted
Medicaid Medicaid fitted
Other insurance Other insurance fitted
Uninsured Uninsured fitted
FIGURE IV
Primary Insurance Coverage of Admitted Patients
See notes for Figure II. In this ﬁgure the y-axis represents the fraction of
patients with different classes of primary insurance coverage. Sample includes
425,315 patients with nondeferrable primary diagnoses, deﬁned as unplanned
admissions through the emergency department for diagnoses with a t-statistic for
the test of equal weekday and weekend admission rates of 0.965 or less. Medicare
eligibility status of patients within one month of their 65th birthdays is uncertain
and we have excluded these observations.
other , and none) in the nondeferrable admissions subsample. Con-
sistent with the patterns in Figure I for the overall population, we
see a big increase in the fraction of patients with Medicare as their
primary insurer at 65, coupled with a decline in the fraction with
no insurance. RD models for health insurance outcomes are pre-
sented in Table III. The models follow the same speciﬁcation as in
Table II, although we now include a set of covariates (year, month
and day of admission, race/ethnicity , sex, and admission diagno-
sis ﬁxed effects) in the speciﬁcations shown in even-numbered
columns. For reference, the speciﬁcations in the odd-numbered
columns exclude these controls and also exclude the dummy for
admissions in the 31 days before a patient’s 65th birthday .
The regression results conﬁrm the visual impressions con-
veyed in Figure IV . At age 65, the fraction of patients with
Medicare as their primary insurer rises by about 47 percentage
points, whereas the fractions with private insurance and Medicaid
 at University of Kentucky Libraries on November 29, 2011http://qje.oxfordjournals.org/Downloaded from

### Slide 395

Impact of Medicare on type of coverage
616QUARTERL Y JOURNAL OF ECONOMICSTABLE III
REGRESSIONDISCONTINUITYMODELS FORPROBABILITY OFDIFFERENTFORMS OFPRIMARYINSURANCECOVERAGE
Medicare Private Medicaid Uninsured
(1) (2) (3) (4) (5) (6) (7) (8)
Age over 65 (×100) 43.94 7 .5 −24.8 −26.8 −10.1 −10.8 −7.4 −8.0
(0.4) (0 .4) (0 .4) (0 .4) (0 .3) (0 .3) (0 .2) (0 .2)
Additional controls No Y es No Y es No Y es No Y es
Mean of dependent variable 24.0 43.3 43.3 9.7
for patients aged 64–65 (×100)
Notes.Standard errors in parentheses. Dependent variable is indicator for type of insurance listed as “primary insurer” on discharge record. Sample includes 425,315 observations
on patients between the ages of 60 and 70 admitted to California hospitals between January 1, 1992, and November 30, 2002 for an unplanned admission thr ough the emergency
department, with a diagnosis (ICD-9) for which thet-test for equality of weekend and weekday admission rates is less than 0.96 in absolute value. All models include second-order
polynomial in age (in days) fully interacted with dummy for age over 65 and are ﬁt by OLS. Models in even-numbered columns include the following additional controls: a dummy for
people who are within one month of their 65th birthday; dummies for month, year , sex, race/ethnicity , and admission on Saturday or Sunday; and a complete set of unrestricted ﬁxed
effects for each ICD-9 admission diagnosis. In columns (1)–(8) the coefﬁcient on “age over 65” and its standard error have been multiplied by 100.
 at University of Kentucky Libraries on November 29, 2011http://qje.oxfordjournals.org/Downloaded from

### Slide 396

Treatment intensity
618 QUARTERL Y JOURNAL OF ECONOMICS
9.70
9.75
9.80
9.85
9.90
9.95
10.00
10.05
10.10
10.15
10.20
60 61 62 63 64 65 66 67 68 69
Age at admission
Log (list charges)
2
3
4
5
6
7
8
9
10
Days and number of procedures
Log list charges Log charges fitted
Length of stay fitted Length of stay (days)
Number of procedures Procedures fitted
FIGURE V
Three Measures of Inpatient Treatment Intensity
See notes to Figure IV . Sample includes unplanned admissions through the
emergency department for diagnoses with a t-statistic for the test of equal weekday
and weekend admission rates of 0.965 or less. In this ﬁgure the sample is further
restricted to patients with valid SSNs (407,386 observations). Sample for log list
charges excludes patients admitted to Kaiser hospitals. Length of stay , number of
procedures, and list charges are cumulated over all consecutive hospitalizations.
List charges are measured in 2002 dollars.
health spending). To evaluate this concern we ﬁt RD models to the
standard deviation of log list charges for patients in each age-at-
admission cell (with age measured in days). These models showed
no large or statistically signiﬁcant effect on the dispersion in list
charges at age 65. We also ﬁt RD models to the 75th and 90th
percentiles of list charges for each age-at-admission cell. These
models showed that both the 75th and 90th percentiles of list
charges increase by about 2%–3% at age 65. (The regression mod-
els, and associated graphs, are presented in the Online Appendix,
Table C and Figures F and G.)
Overall, we conclude that there are modest but statistically
signiﬁcant increases in the intensity of treatment at age 65 for pa-
tients in our nondeferrable admissions sample, on the order of 3%.
These increases are much smaller than the 10%–25% increases
in rates of elective procedures such as hip and knee replacements
observed in the overall population (Card, Dobkin, and Maestas
 at University of Kentucky Libraries on November 29, 2011http://qje.oxfordjournals.org/Downloaded from

### Slide 397

Treatment intensity
DOES MEDICARE SAVE LIVES?619
TABLE IV
REGRESSIONDISCONTINUITYMODELS FORCHANGES INTREATMENTINTENSITY
Length of stay (days) Number of procedures Log list charges (×100)
(1) (2) (3) (4) (5) (6)
Age over 65 0.37 0.35 0.09 0.11 2.52 .6
(0.24) (0.26) (0.03) (0.03) (1.1) (1.0)
Additional controls No Y es No Y es No Y es
Mean of dependent variable 8.12 2.50 9.87
for patients aged 63 or 64
Notes.Standard errors in parentheses. Dependent variable is length of stay in days (columns (1) and (2)), number of procedures performed (columns (3) and (4 )), and log of total
list charges (columns (5) and (6)). Sample includes 407,386 (352,652 in columns (5) and (6)) observations on patients with valid SSNs between the ages of 60 and 70 admitted to
California hospitals between January 1, 1992, and November 30, 2002 for an unplanned admission through the ED. Data on list charges are missing for Kai ser hospitals. See note to
Table III for additional details on sample, and list of additional covariates included in even-numbered columns. In columns (5) and (6) the coefﬁciento n“ a g eo v e r6 5 ”a n di t ss t a n d a r d
error have been multiplied by 100.
 at University of Kentucky Libraries on November 29, 2011http://qje.oxfordjournals.org/Downloaded from

### Slide 398

Mortality
DOES MEDICARE SAVE LIVES? 621
0.00
0.05
0.10
0.15
0.20
0.25
0.30
60 61 62 63 64 65 66 67 68 69 70
Age at admission
Death rate (fraction)
Death 365 days Death 365 days fitted
Death 180 days Death 180 days fitted
Death 90 days Death 90 days fitted
Death 28 days 
Death 28 days fitted
Death 14 days
Death 14 days fitted
Death 7 days 
Death 7 days fitted
FIGURE VI
Patient Mortality Rates over Different Follow-Up Intervals
See notes to Figure IV . Sample includes unplanned admissions through the
emergency department for diagnoses with a t-statistic for the test of equal week-
day and weekend admission rates of 0.965 or less. In this ﬁgure the sample is
further restricted to patients with valid SSNs (407,386 observations). Deaths in-
clude include in-hospital and out-of-hospital deaths.
between-hospital transfer rates rise at age 65, with a particu-
larly large rise in within-hospital transfers (25%) that appears
to be driven by a jump in the rate of transfer to skilled nursing
facilities within the same hospital. We also ﬁnd a marginally sig-
niﬁcant reduction in the probability that patients are readmitted
to a California hospital within 28 days after their initial admis-
sion (point estimate = −0.6-percentage-point reduction on a base
readmission rate of 17.0% for 64-year-olds, t = 1.70). Graphs and
estimates for transfers and readmissions are presented in the
Online Appendix (Figures H and I and Table E).
V .D. Mortality
Figure VI plots the age proﬁles for the probability of death
within 7, 14, 28, 90, 180, and 365 days of admission to the hos-
pital, whereas the ﬁrst two rows of Table V present estimates
from RD regression models corresponding to each of these out-
comes. Inspection of Figure VI shows that each of the mortality
 at University of Kentucky Libraries on November 29, 2011http://qje.oxfordjournals.org/Downloaded from

### Slide 399

Mortality
622QUARTERL Y JOURNAL OF ECONOMICS
TABLE V
REGRESSIONDISCONTINUITYESTIMATES OFCHANGES INMORTALITYRATES
Death rate in
7d a y s 1 4d a y s 2 8d a y s 9 0d a y s 1 8 0d a y s 3 6 5d a y s
Estimated discontinuity at age 65 (×100)
Fully interacted quadratic with no −1.1 −1.0 −1.1 −1.1 −1.2 −1.0
additional controls (0.2) (0.2) (0.3) (0.3) (0.4) (0.4)
Fully interacted quadratic plus −1.0 −0.8 −0.9 −0.9 −0.8 −0.7
additional controls (0.2) (0.2) (0.3) (0.3) (0.3) (0.4)
Fully interacted cubic plus additional −0.7 −0.7 −0.6 −0.9 −0.9 −0.4
controls (0.3) (0.2) (0.4) (0.4) (0.5) (0.5)
Local linear regression procedure ﬁt −0.8 −0.8 −0.8 −0.9 −1.1 −0.8
separately to left and right with (0.2) (0.2) (0.2) (0.2) (0.3) (0.3)
rule-of-thumb bandwidths
Mean of dependent variable (%) 5.17 .19 .81 4.71 8.42 3.0
Notes.Standard errors in parentheses. Dependent variable is indicator for death within interval indicated by column heading. Entries in rows (1)–(3) are estimated coefﬁcients of
dummy for age over 65 from models that include a quadratic polynomial in age (rows (1) and (2)) or a cubic polynomial in age (row (3)) fully interacted with a dummy for age over 65.
Models in rows (2) and (3) include the following additional controls: a dummy for people who are within 1 month of their 65 birthdays, dummies for year , month, sex, race/ethnicity ,
and Saturday or Sunday admissions, and unrestricted ﬁxed effects for each ICD-9 admission diagnosis. Entries in row (4) are estimated discontinuiti es from a local linear regression
procedure, ﬁt separately to the left and right, with independently selected bandwidths from a rule-of-thumb procedure suggested by Fan and Gijbels ( 1996). Sample includes 407,386
observations on patients between the ages of 60 and 70 admitted to California hospitals between January 1, 1992, and November 30, 2002, for unplanned a dmission through the ED
who have nonmissing Social Security numbers. All coefﬁcients and their SEs have been multiplied by 100.
 at University of Kentucky Libraries on November 29, 2011http://qje.oxfordjournals.org/Downloaded from

### Slide 400

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Stratiﬁcation
Matching
Propensity score matching
Regression discontinuity designs
Probabilistic treatment assignment (i.e. “fuzzy RDD”)
The probability of receiving treatment changes discontinuously at the cutoﬀ,X0,
but need not go from 0 to 1
limXi→X0Pr(Di = 1|Xi =X0) ̸=limX0←XiPr(Di = 1|Xi =X0)
Examples: Incentives to participate in some program may change discontinuously at
the cutoﬀ but are not powerful enough to move everyone from non participation to
participation.
In the sharp RDD,Di wasdeterminedbyXi ≥X0; in the fuzzy RDD, the
conditional probability of treatmentjumpsatX0.
The relationship between the probability of treatment andXi can be written as:
P[Di = 1|Xi] =g0(Xi) + [g1(Xi) −g0(Xi)]Ti
whereTi = 1 if (Xi ≥X0) and 0 otherwise.
Cunningham Causal Inference

### Slide 401

Visualization of Fuzzy RDD identiﬁcation strategy
 
 
 
 
Figure:Imbens and Lemieux (2007), ﬁgure 3. Horizontal axis is the running variable. Vertical axis is the
conditional probability of treatment at each value of the running variable.

### Slide 402

Visualization of identiﬁcation strategy (i.e. smoothness)
Conditional expectation of the 2 potential outcomes givenX forD = 0,1
Partly dashed, partly solid→continuous functions of the covariate
Conditional expectation of the observed outcome,Y
Solid→jumps atX =X0
 
 
 
 
Figure: Potential and observed outcome regressions (Imbens and Lemieux 2007)

### Slide 403

Use the discontinuity as IV
Wald estimator of treatment eﬀect under Fuzzy RDD
Average causal eﬀect of the treatment is the Wald IV parameter
δFuzzy RDD= limX→X0E[Y|X =X0] −limX0←XE[Y|X =X0]
limX→X0E[D|X =X0] −limX0←XE[D|X =X0]
Fuzzy RDD is numerically equivalent and conceptually similar to instrumental
variables
Numerator: “jump” in the regression of the outcome on the running variable,X.
“Reduced form”.
Denominator: “jump” in the regression of the treatment indicator on the running
variableX. “First stage”.
Use software package to estimate (e.g.,ivregress 2slsin STATA).
Same IV assumptions, caveats about compliers vs. deﬁers, and statistical tests
that we discussed with instrumental variables apply here – e.g., check for weak
instruments usingF test on instrument in ﬁrst stage, etc.

### Slide 404

First stage relationship betweenX andD
One can use bothTi as well as the interaction terms as instruments forDi. If
one uses onlyTi as IV, then the it is a “just identiﬁed” model which usually
has good ﬁnite sample properties.
In the just identiﬁed case, the ﬁrst stage would be:
Di =γ0 +γ1Xi +γ2X2i +· · ·+γpXp
i +πTi +ζ1i
whereπ is the causal eﬀect ofT on the conditional probability of treatment.
The fuzzy RD reduced form is:
Yi =µ+κ1Xi +κ2X2i +· · ·+κpXp
i +ρπTi +ζ2i

### Slide 405

Fuzzy RDD with varying Treatment Eﬀects - Second Stage
As in the sharp RDD case one can allow the smooth function
to be diﬀerent on both sides of the discontinuity.
The second stage model with interaction terms would be the
same as before:
Yi = α + β01˜xi + β02˜x2
i + · · · + β0p ˜x p
i
+ρDi + β∗
1Di ˜xi + β∗
2Di ˜x2
i + · · · + β∗
pDi ˜x p
i + ηi
Where ˜x are now not only normalized with respect to x0 but
are also ﬁtted values obtained from the ﬁrst stage regression.

### Slide 406

Fuzzy RDD with Varying Treatment Eﬀects - First Stages
Again one can use both Ti as well as the interaction terms as
instruments for Di
Only using T the estimated ﬁrst stages would be:
Di = γ00 + γ01 ˜Xi + γ02 ˜X 2
i + · · · + γ0p ˜X p
i
+πTi + γ∗
1 ˜Xi Ti + γ∗
2 ˜X 2
i Ti + · · · + γ∗
pTi + ζ1i
We would also construct analogous ﬁrst stages for ˜Xi Di,
˜X 2
i Di, . . ., ˜X p
i Di.

### Slide 407

What does Fuzzy RDD Estimate?
As Hahn, Todd and van der Klaauw (2001) point out, one
needs the same assumptions as in the standard IV framework
As with other binary IVs, the fuzzy RDD is estimating LATE:
the average treatment eﬀect for the compliers
In RDD, the compliers are those whose treatment status
changed as we moved the value of xi from just to the left of
x0 to just to the right of x0

### Slide 408

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Stratiﬁcation
Matching
Propensity score matching
Regression discontinuity designs
Challenges to RDD
Treatment is not as good as randomly assigned around the cutoﬀ,X0, when
agents are able to manipulate their running variable scores. This happens
when:
1 the assignment rule is known in advance
2 agents are interested in adjusting
3 agents have time to adjust
4 Examples: re-take an exam, self-reported income, etc.
Some other unobservable characteristic changes at the threshold, and this has a
direct eﬀect on the outcome.
In other words, the cutoﬀ is endogenous
Example: Age thresholds used for policy (i.e., person turns 18, and faces more
severe penalties for crime) is correlated with other variables that aﬀect the
outcome (i.e., graduation, voting rights, etc.)
Econometricians and applied social scientists have developed several formalized tests
to evaluate the severity of these problems which we now discuss.
Cunningham Causal Inference

### Slide 409

Test 1: Manipulation of the running variable
Sorting on the running variable (i.e., Manipulation)
Assume a desirable treatment,D, and an assignment ruleX ≥X0. If individuals
sort intoD by choosingX such thatX ≥X0, then we say individuals are sorting on
the running variable.
Motivating example: Suppose a doctor plans to randomly assign heart patients
to a statin and a placebo to study the eﬀect of the statin on heart attacks
within 10 years. The doctor randomly assigns patients to two diﬀerent waiting
rooms,A andB, and plans to give those inA the statin and those inB the
placebo. If some of the patents learn of the planned treatment assignment
mechanism, what would we expect to happen? And how would you check for
it?

### Slide 410

McCrary Density Test
We would expect waiting roomA to becomecrowded. In the RDD context, sorting
on the running variable implies heaping on the “good side” ofX0
McCrary (2008) suggests a formal test. Under the null the density should be
continuous at the cutoﬀ point. Under the alternative hypothesis, the density
should increase at the kink (whereD is viewed as good)
1 Partition the assignment variable into bins and calculate frequencies (i.e., number
of observations) in each bin
2 Treat those frequency counts as dependent variable in a local linear regression
The McCrary Density Test has becomemandatoryfor every analysis using
RDD.
If you can estimate the conditional expectations, you evidently have data on the
running variable. So in principle you can always do a density test
You can download the (no longer supported) STATA ado package,DCdensity, to
implement McCrary’s density test
(http://eml.berkeley.edu/~jmccrary/DCdensity/)
You can installrddfor R too
(http://cran.r-project.org/web/packages/rdd/rdd.pdf)

### Slide 411

Caveats about McCrary Density Test
For RDD to be useful, you already need to know something about the
mechanism generating the assignment variable and how susceptible it could be
to manipulation. Note the rationality of economic actors that this test is built
on.
A discontinuity in the density is “suspicious” – itsuggestsmanipulation ofX
around the cutoﬀ is probably going on. In principle one doesn’t need continuity.
This is a high-powered test. You need a lot of observations atX0 to distinguish
a discontinuity in the density from noise.
also necessary, and we may characterize those who reduce their labor supply as those with coaipc=f i and
bi4aið1 /C0f iÞ=d.
Fig. 2 shows the implications of these behavioral effects using a simulated data set on 50,000 agents with
linear utility. The simulation takes ðai; biÞ to be distributed as independent normals, with E ½ai/C138¼12, V ½ai/C138¼9,
E½bi/C138¼0, and V ½bi/C138¼1, and the f i distribution to be uniform on ½0; 1/C138and independent of ðai; biÞ. The
earnings threshold is set at c ¼ 14.
This data generating process is consistent with (A0). If the program did not exist, then period 1 earnings
would be Ri0 ¼ ai. The conditional expectation of ai given Ri0 is thus just the 45 /C14line, which is continuous; the
conditional expectation of bi given Ri0 is ﬂat, which is likewise continuous; and the density of Ri0 is the normal
density, hence continuous. Panel A of Fig. 2 is a local linear regression estimate of the conditional expectation
of bi given Ri0. The smoothness of the conditional expectation indicates the validity of (A0).
However, even though (A0) is satisﬁed, agents’ endogenous labor supply creates an identiﬁcation problem.
The actual running variable is not Ri0, but Ri, which is manipulated by those agents who ﬁnd it worthwhile to
do so. Panel B gives a local linear regression estimate of the conditional expectation of bi given Ri. This panel
ARTICLE IN PRESS
Fig. 1. The agent’s problem.
-0.50
-0.30
-0.10
0.10
0.30
0.50
5 10 15 20
Income
Conditional Expectation
Estimate
-0.50
-0.30
-0.10
0.10
0.30
0.50
5 10 15 20
Income
Conditional Expectation
Estimate
`
0.00
0.02
0.04
0.06
0.08
0.10
0.12
0.14
0.16
5 10 15 20
Income
Density Estimate
0.00
0.02
0.04
0.06
0.08
0.10
0.12
0.14
0.16
5 10 15 20
Income
Density Estimate
Fig. 2. Hypothetical example: gaming the system with an income-tested job training program: (A) conditional expectation of returns to
treatment with no pre-announcement and no manipulation; (B) conditional expectation of returns to treatment with pre-announcement
and manipulation; (C) density of income with no pre-announcement and no manipulation; (D) density of income with pre-announcement
and manipulation.
J. McCrary / Journal of Econometrics 142 (2008) 698–714706
Figure:Panel C is density of income when there is no pre-announcement and no manipulation. Panel D is
the density of income when there is pre-announcement and manipulation. From McCrary (2008).

### Slide 412

Visualizing manipulation
Figure 2: Distribution of marathon ﬁnishing times ( n =9 , 378, 546)
NOTE: The dark bars highlight the density in the minute bin just prior to each 30 minute threshold.
12
Figure 3: Running McCrary z-statistic
-50050100150McCrary z2:303:003:304:004:305:005:306:006:307:00thresholdNon-categorical10 minute threshold30 minute thresholdNOTE: The McCrary test is run at each minute threshold from 2:40 to 7:00 to test whether there is a signiﬁcant discontinuity
in the density function at that threshold.
14
Figure:Figures 2 and 3 from Eric Allen, Patricia Dechow, Devin Pope and George Wu’s (2013)
“Reference-Dependent Preferences: Evidence from Marathon Runners”.
http://faculty.chicagobooth.edu/devin.pope/research/pdf/Website_Marathons.pdf

### Slide 413

More examples of testing for manipulation:
Lee (2008) Incumbency Eﬀect
David Lee’s (2008) “Randomized Experiments from non-random selection in
US House Elections”,Journal of Econometricsanalyzes the incumbency eﬀect
using Democratic incumbents for US congressional elections.
A large political science literature on the “incumbency advantage” – having won
an election once helps you win subsequent elections.
Empirical challenge: how to separate incumbency advantage from selection –
(i.e., candidates win multiple elections because they are better)
Identiﬁcation: Incumbency isassignedto a candidate discontinuously at 50%
voteshare under two-party democracy with majority rule.
Lee (2008) analyzes the probability of winning the election in yeart + 1 by
comparing candidates who just won to candidates who just lost the election in
yeart.

### Slide 414

Density test
Popular Elections to the U.S. House of Representatives, 1900-1990:
A Smooth Density!"#$%&'()'*&+,-%./"-'0,/&'12.%&'3&4./"5&'/,'6$/,778
9,:$4.%';4&-/",<='/,'/2&'>,$=&',7'3&:%&=&</./"5&=?'@ABBC@AAB
!
"!
#!
$!
%&!
%'!
(% (!)* (!)# (!)+ (!)& ! !)& !)+ !)# !)* %
*&+,-%./"-'D.%#"<
!%&E$&<-F'6,$</
!)!!
!)&!
!)+!
!)#!
!)*!
%)!!
%)&!
%)+!
%)#!
*&<="/F';=/"+./&
,
!"#$%&'G)'9&%-&</'0,/"<#'H&.F8
3,44'6.44'0,/&=?'I)1)'>,$=&',7'3&:%&=&</./"5&=?'@JGKCLBB(
!
'!
%!!
%'!
&!!
&'!
"!!
!! ) % ! ) & ! ) " ! ) + ! ) ' ! ) # ! ) - ! ) * ! ) $%
9&%-&</'0,/"<#'"<'!.5,%',7'9%,:,=&M'N"44
!%&E$&<-F'6,$</
!)!!
!)'!
%)!!
%)'!
&)!!
&)'!
*&<="/F';=/"+./&
,
Figure: Democratic vote share relative to cutoﬀ: popular elections to the
House of Representatives, 1900-1990 (McCrary 2008).

### Slide 415

More evidence of manipulation
Contrast this with roll call voting in the US House of Representatives
Coordination is expected because these are repeated games, votes are public
records, and side payments are possible in the form of future votes
Bills around the cutoﬀ are more likely to be passed than not. Seems like a good
candidate for RDD
Fails McCrary Density Test;cannotuse RDD because policy decisions are not
quasi-randomly assigned around cutoﬀ
Roll Call Votes in the House, 1857-2008:
A Not Very Smooth Density
!"#$%&'()'*&+,-%./"-'0,/&'12.%&'3&4./"5&'/,'6$/,778
9,:$4.%';4&-/",<='/,'/2&'>,$=&',7'3&:%&=&</./"5&=?'@ABBC@AAB
!
"!
#!
$!
%&!
%'!
(% (!)* (!)# (!)+ (!)& ! !)& !)+ !)# !)* %
*&+,-%./"-'D.%#"<
!%&E$&<-F'6,$</
!)!!
!)&!
!)+!
!)#!
!)*!
%)!!
%)&!
%)+!
%)#!
*&<="/F';=/"+./&
,
!"#$%&'G)'9&%-&</'0,/"<#'H&.F8
3,44'6.44'0,/&=?'I)1)'>,$=&',7'3&:%&=&</./"5&=?'@JGKCLBB(
!
'!
%!!
%'!
&!!
&'!
"!!
!! ) % ! ) & ! ) " ! ) + ! ) ' ! ) # ! ) - ! ) * ! ) $%
9&%-&</'0,/"<#'"<'!.5,%',7'9%,:,=&M'N"44
!%&E$&<-F'6,$</
!)!!
!)'!
%)!!
%)'!
&)!!
&)'!
*&<="/F';=/"+./&
,
Figure: Percent voting Yeay: Roll Call Votes, US House of Representatives, 1857-2004
(McCrary 2008)

### Slide 416

Test 2: Balance test on covariates
This is a type of placebo test. For RDD to be valid in your
study, you do not want to observe a discontinuity around the
cutoﬀ, X0, for average values of covariates that should not be
aﬀected by the treatment – e.g., pretreatment characteristics
Question: What does a jump in the average values of
pre-treatment characteristics have to do with the continuous
(smoothness) assumption?
Choose other pre-treatment covariates, Z and do a similar
graphical plot as you did for Y
You do not want to see a jump around the cutoﬀ, X0
A formal balance test involves the same procedure used to
estimate the treatment eﬀect, only use Z instead of Y as a
LHS variable
Can combine test on multiple covariates into a single test
statistic using seemingly unrelated regression ( SUR) or a single
“stacked” regression

### Slide 417

Visualizing Placebo Test
share. The coef ﬁcient reported in column (6) is the predicted
difference at 50 percent. The table con ﬁrms that, for many ob-
servable characteristics, there is no signi ﬁcant difference in a
close neighborhood of 50 percent. One important exception is the
percentage black, for which the magnitude of the discontinuity is
statistically signi ﬁcant.23
As a consequence, estimates of the coef ﬁcients in Table I from
regressions that include these covariates would be expected to
produce similar results —as in a randomized experiment —since
23. This is due to few outliers in the outer part of the vote share range. When
the polynomial is estimated including only districts with vote share between 25
percent and 75 percent, the coef ﬁcients becomes insigni ﬁcant. The gap for percent
urban and open seats, while not statistically signi ﬁcant at the 5 percent level, is
signiﬁcant at the 10 percent level.
FIGURE III
Similarity of Constituents ’ Characteristics in Bare Democrat and Republican
Districts–Part 1
Panels refer to (from top left to bottom right) the following district character-
istics: real income, percentage with high-school degree, percentage black, percent-
age eligible to vote. Circles represent the average characteristic within intervals
of 0.01 in Democrat vote share. The continuous line represents the predicted
values from a fourth-order polynomial in vote share ﬁtted separately for points
above and below the 50 percent threshold. The dotted line represents the 95
percent con ﬁdence interval.
835DO VOTERS AFFECT OR ELECT POLICIES?
 at Baylor University on April 22, 2014http://qje.oxfordjournals.org/Downloaded from 
Figure:Figure 3 from Lee, Moretti and Butler (2004), “Do Voters Aﬀect or Elect Policies?”Quarterly
Journal of Economcis. Panels refer to (top left to bottom right) the following district characteristics: real
income, percentage with high-school degree, percentage black, percentage eligible to vote. Circles represent
the average characteristic within intervals of 0.01 in Democratic vote share. The continuous line represents
the predicted values from a fourth-order polynomial in vote share ﬁtted separately for points above and
below the 50 percent threshold. The dotted line represents the 95 percent conﬁdence interval.

### Slide 418

Test 3: Jumps at non-discontinuous points
Imbens and Lemieux (2008) suggest to look at one side of the
discontinuity (e.g., X < X0), take the median value of the
running variable in that section, and pretend it was a
discontinuity, X ′
0
Then test whether in reality there is a discontinuity at X ′
0.
You do not want to ﬁnd anything.
Another kind of placebo test. Similar to synthetic control
falsiﬁcation exercises where you look for non-eﬀects in
non-treatment periods. (See Abadie, Diamond and
Hainmueller 2014)

### Slide 419

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Stratiﬁcation
Matching
Propensity score matching
Regression discontinuity designs
RDD graphs
A graphical analysis should be an integral part of any RDD.
1 Outcome by running variable,(Xi):
The standard graph showing the discontinuity in the outcome variable
Construct bins and average the outcome within bins on both sides of the cutoﬀ
You should also look at diﬀerent bin sizes when constructing these graphs (see
Lee and Lemieux, 2010, for details)
Plot the running variables,Xi, on the horizontal axis and the average ofYi for
each bin on the vertical axis
You may also want to plot a relatively ﬂexible regression line on top of the bin
means
Inspect whether there is a discontinuity atx0
Inspect whether there are other unexpected discontinuities
Cunningham Causal Inference

### Slide 420

Example: Outcomes by Running Variables
Example: Outcomes by Forcing Variable
From Lee and Lemieux (2010) based on Lee (2008)
Waldinger (Warwick) 26 / 48

### Slide 421

Example: Outcomes by Running Variables with smaller bins
Example: Outcomes by Forcing Variable - Smaller Bins
From Lee and Lemieux (2010) based on Lee (2008)
Waldinger (Warwick) 27 / 48

### Slide 422

More RDD Graphs
2 Probability of treatment by running variable if fuzzy
RDD
In a fuzzy RDD, you also want to see that the treatment
variable jumps at x0
This tells you whether you have a ﬁrst stage
3 Covariates by a running variable
Construct a similar graph to the one before but using a
covariate as the “outcome”
There should be no jump in other covariates at the
discontinuity, x0.
If the covariates jump at the discontinuity, one would doubt
the identifying assumption

### Slide 423

Example: Covariates by Running Variable
Example Covariates by Forcing Variable
From Lee and Lemieux (2010) based on Lee (2008)
Waldinger (Warwick) 29 / 48

### Slide 424

More RDD graphs!
4 The density of the running variable
One should plot the number of observations in each bin.
This plot allows to investigate whether there is a discontinuity
in the distribution of the running variable at the threshold
This would suggest that people can manipulate the running
variable around the threshold.
This is an indirect test of the identifying assumption that each
individual has imprecise control over the assignment variable

### Slide 425

Density of the running variable
Density of the forcing variable
From Lee & Lemieux (2010) based on Lee (2008)
Waldinger (Warwick) 31 / 48

### Slide 426

“Do Voters Aﬀect or Elect Policies?”
by Lee, Moretti and Butler (2004)
How do voters aﬀect policy? There are two fundamentally diﬀerent views of the role
of elections in a representative democracy.
1 Convergence: Heterogenous voter ideology forces each candidates to
moderate their positions (e.g., median voter theorem).
“Competition for votes can force even the most partisan Republicans
and Democrats to moderate their policy choices. In the extreme case,
competition may be so strong that it leads to ‘full policy
convergence’: opposing parties are forced to adopt identical policies”
(Lee, Moretti, and Butler 2004; Downs 1957).
2 Divergence: When partisan politicians cannot credibly commit to certain
policies, then convergence is undermined. The result can be full policy
divergence. Divergence is when the winning candidate, after taking oﬃce,
simply pursues his most-preferred policy. In this case, voters fail to compel
candidates to reach any kind of policy compromise.

### Slide 427

Simpliﬁed model
Candidates: R andD are candidates in a Congressional race. Policy space is
unidimensional whereD’s andR’s per-period policy preferences are quadratic
loss functions,u(l) andv(l) andl is the policy variable
D’s bliss point:l∗ =c(>0)
maxl u(l) = −1
2(l −c)2
∂u(l)
∂l = 2 × −1
2(l −c) = 0
l∗ = c(>0)
R’s bliss point:l∗ = 0
maxl v(l) = −1
2l2
∂v(l)
∂l = −1
2 ×2l = 0
l∗ = 0
Voters: Ex ante, voters expectD (R) to choose policyxe (ye), and expectD
to win with probabilityP(xe,ye)
Whenxe >ye then ∂P
∂xe >0, ∂P
∂ye <0. Democrats could gain more votes by
moderating the policy position.
P∗ represents the underlying popularity ofD party, or put diﬀerently, the
probabilityD would win ifx =c andy = 0.
if D (R) wins, policyx (y) will be implemented; rational expectations
equilibrium assumed (x =xe; y =ye), game repeats in periodst andt + 1
where a period includes the election and the Congressional session.

### Slide 428

Multiple Nash equilibria
1 Partial/Complete Convergence: Voters aﬀect policies
Key result: ∂x ∗
∂P ∗ > 0 and ∂y ∗
∂P ∗ > 0
If we did a “helicopter drop” of more Democrats in the district
to exogenously increase P∗ and this resulted in candidates
changing their policy positions, then ∂x ∗
∂P ∗ > 0 and ∂y ∗
∂P ∗ > 0
2 Complete divergence: Voters elect politicians with ﬁxed
policies
Key result: ∂x ∗
∂P ∗ = ∂x ∗
∂P ∗ = 0
An exogenous shock to P∗ (e.g., the helicopter drop of
Democrats) does nothing to equilibrium policies. Voters elect
politicians’ ﬁxed policies.

### Slide 429

Estimation
Potential roll-call voting record outcomes of the representative in the district
following electiont is
RCt =Dtxt + (1−Dt)yt
whereDt is an indicator for whetherD won the election. That is, only the
winning candidate’s policy is observed.
This expression can be transformed into regression equations:
RCt = α0 +π0P∗t +π1Dt +εt (95)
RCt+1 = β0 +π0P∗t+1+π1Dt+1+εt+1 (96)
whereα0 andβ0 are constants
They parameterized the derivatives from the prior slide asπ0, the coeﬃcient on
the probability,P. See their Appendix 1, p. 849 for details.
This also allows an independent eﬀect of the party,π1

### Slide 430

Estimation (cont.)
Equation 198 cannot be directly estimated because we don’t observeP∗. But
suppose we could randomizeDt. ThenDt would be independent ofP∗t andεt.
Then taking conditional expectations with respect toDt we get:
E[RCt+1|Dt = 1]−E[RCt+1|Dt = 0]  
Observable
= π0[P∗Dt+1−P∗Rt+1]
+ π1[PDt+1−PRt+1]  
Observable
= γ
Total eﬀect of initial win on future roll call votes
(97)
E[RCt|Dt = 1]−E[RCt|Dt = 0]  
Observable
= π1 (98)
E[Dt+1|Dt = 1]−E[Dt+1|Dt = 0]  
Observable
= PDt+1−PRt+1 (99)

### Slide 431

Why this works
The “elect” component isπ1[PDt+1−PRt+1] and it’s estimated as the diﬀerence
in mean voting records between the parties at timet
The fraction of districts won by Democrats int + 1 is an estimate of
[PDt+1−PRt+1]
Because we can estimate the total eﬀect,γ, of a Democrat victory int on
RCt+1, we can net out the elect component to implicitly get the “aﬀect”
component
Random assignment ofDt is crucial. Without it, equation 206 would reﬂectπ1
andselection (i.e., that Dem districts have more liberal bliss points).

### Slide 432

Data and RDD Jargon
Two main datasets: liberal voting score from the Americans for Democratic
Action (ADA) linked with House of Representatives election results for
1946-1995
Authors use the ADA score for all US House Representatives from 1946 to 1995
as their voting record index
For each Congress, ADA chooses about twenty high-proﬁle roll-call votes and
creates an index varying 0 and 100 for each Representative of the House. Higher
scores correspond to a more “liberal” voting record.
RDD Jargon
The running variable isvotesharewhich is the share of all votes that went to a
Democrat. ADA scores are linked to election returns data during that period.
They use exogenous variation in Democratic wins to check whether convergence
or divergence is correct.
Discontinuity in the running variable occurs atvoteshare= 0.5. When
voteshare>0.5, the Democratic candidate wins.
Download STATA do ﬁle from my website:http://business.baylor.edu/
scott_cunningham/teaching/causalinf/lmb2004.txt, save it on your
computer, and rename the extension to saylmb2004.do. Open it in your
STATA do-editor.

### Slide 433

Statistical results
primarily elect policies (full divergence) rather than affect poli-
cies (partial convergence).
Here we quantify our estimates more precisely. In the analy-
sis that follows, we restrict our attention to “close elections ”—
where the Democrat vote share in time t is strictly between 48
and 52 percent. As Figures I and II show, the difference between
barely elected Democrat and Republican districts among these
elections will provide a reasonable approximation to the discon-
tinuity gaps. There are 915 observations, where each observation
is a district-year. 20
Table I, column (1), reports the estimated total effect /H9253, the
size of the jump in Figure I. Speci ﬁcally, column (1) shows the
difference in the average ADA t/H110011 for districts for which the
Democrat vote share at time t is strictly between 50 percent and
52 percent and districts for which the Democrat vote share at
time t is strictly between 48 percent and 50 percent. The esti-
mated difference is 21.2.
In column (2) we estimate the coef ﬁcient /H92661, which is equal to
the size of the jump in Figure IIa. The estimate is the difference
in the average ADA t for districts for which the Democrat vote
20. In 68 percent of cases, the representative in period t /H110011 is the same as
the representative in period t. The distribution of close elections is fairly uniform
across the years. In a typical year there are about 40 close elections. The year with
the smallest number is 1988, with twelve close elections. The year with the largest
number is 1966, with 92 close elections.
TABLE I
RESULTS BASED ON ADA S CORES—CLOSE ELECTIONS SAMPLE
Variable
Total effect Elect component Affect component
/H9253/H92661 (Pt/H110011
D /H11002Pt/H110011
R ) /H92661[(Pt/H110011
D /H11002Pt/H110011
R )] /H92660[P*t /H110011
D /H11002P*t /H110011
R ]
ADA t/H110011 ADAt DEMt/H110011 (col. (2)*(col. (3)) (col. (1)) /H11002(col. (4))
(1) (2) (3) (4) (5)
Estimated gap 21.2 47.6 0.48
(1.9) (1.3) (0.02)
22.84 /H110021.64
(2.2) (2.0)
Standard errors are in parentheses. The unit of observation is a district-congressional session. The
sample includes only observations where the Democrat vote share at time t is strictly between 48 percent and
52 percent. The estimated gap is the difference in the average of the relevant variable for observations for
which the Democrat vote share at time t is strictly between 50 percent and 52 percent and observations for
which the Democrat vote share at time t is strictly between 48 percent and 50 percent. Time t and t /H110011 refer
to congressional sessions. ADA t is the adjusted ADA voting score. Higher ADA scores correspond to more
liberal roll-call voting records. Sample size is 915.
832 QUARTERLY JOURNAL OF ECONOMICS
 at Baylor University on April 7, 2014http://qje.oxfordjournals.org/Downloaded from 
Figure: Lee, Moretti, and Butler 2004, Table 1.

### Slide 434

. ** Tables - using OLS. * Table 1: Results Based on ADA Scores. reg score lagdemocrat if lagdemvoteshare>.48 & lagdemvoteshare<.52, cluster(id)
Linear regression Number of obs = 915<snip>------------------------------------------------------------------------------| Robustscore | Coef. Std. Err. t P>|t| [95% Conf. Interval]-------------+----------------------------------------------------------------lagdemocrat | 21.28387 1.951234 10.91 0.000 17.45445 25.11329cons | 31.19576 1.333983 23.39 0.000 28.57773 33.81378------------------------------------------------------------------------------
. reg score democrat if lagdemvoteshare>.48 & lagdemvoteshare<.52, cluster(id)
Linear regression Number of obs = 915<snip>------------------------------------------------------------------------------| Robustscore | Coef. Std. Err. t P>|t| [95% Conf. Interval]-------------+----------------------------------------------------------------democrat | 47.7056 1.356011 35.18 0.000 45.04434 50.36686cons | 18.7469 .8432428 22.23 0.000 17.09198 20.40182------------------------------------------------------------------------------
. reg democrat lagdemocrat if lagdemvoteshare>.48 & lagdemvoteshare<.52, cluster(id)
Linear regression Number of obs = 915<snip>------------------------------------------------------------------------------| Robustdemocrat | Coef. Std. Err. t P>|t| [95% Conf. Interval]-------------+----------------------------------------------------------------lagdemocrat | .4843287 .0289322 16.74 0.000 .4275475 .5411099cons | .2417582 .0200939 12.03 0.000 .2023227 .2811938------------------------------------------------------------------------------

### Slide 435

Bandwidth, bias and eﬃciency
Relaxing bandwidth requirement is about trading off bias and efficiency
. * Table 1 (continued): Examine variations in model specification
. reg score democrat , cluster(id2)
Linear regression Number of obs = 13588
<snip>
(Std. Err. adjusted for 505 clusters in id2)
------------------------------------------------------------------------------
| Robust
score | Coef. Std. Err. t P>|t| [95% Conf. Interval]
-------------+----------------------------------------------------------------
democrat | 40.76266 1.495659 27.25 0.000 37.82416 43.70115
cons | 17.5756 .8225887 21.37 0.000 15.95947 19.19172
------------------------------------------------------------------------------
. reg score democrat if lagdemvoteshare>.48 & lagdemvoteshare<.52, cluster(id2)
Linear regression Number of obs = 915
<snip>
(Std. Err. adjusted for 250 clusters in id2)
------------------------------------------------------------------------------
| Robust
score | Coef. Std. Err. t P>|t| [95% Conf. Interval]
-------------+----------------------------------------------------------------
democrat | 47.7056 2.043658 23.34 0.000 43.68054 51.73066
cons | 18.7469 1.38312 13.55 0.000 16.02279 21.47101
------------------------------------------------------------------------------

### Slide 436

Control for the running variable?
. * Center vote share at 0.5 to improve interpretation in interaction models. gen demvoteshare_c = demvoteshare - 0.5(11 missing values generated)
. * Next, control for the (centered) running variable as a linear control variable.. * This is the simplest RDD.. reg score democrat demvoteshare_c, cluster(id2)
Linear regression Number of obs = 13577<snip> (Std. Err. adjusted for 505 clusters in id2)--------------------------------------------------------------------------------| Robustscore | Coef. Std. Err. t P>|t| [95% Conf. Interval]---------------+----------------------------------------------------------------democrat | 58.50236 1.555847 37.60 0.000 55.44561 61.5591demvoteshare_c | -48.93761 4.441693 -11.02 0.000 -57.66412 -40.21109cons | 11.03413 .9602657 11.49 0.000 9.147518 12.92075--------------------------------------------------------------------------------
. * Modeling the linearity such that slopes can differ above vs. below discontinuity. xi: reg score i.democrat*demvoteshare_c, cluster(id2)i.democrat _Idemocrat_0-1 (naturally coded; _Idemocrat_0 omitted)i.demo~t*demv~c _IdemXdemvo_# (coded as above)
Linear regression Number of obs = 13577<snip> (Std. Err. adjusted for 505 clusters in id2)--------------------------------------------------------------------------------| Robustscore | Coef. Std. Err. t P>|t| [95% Conf. Interval]---------------+----------------------------------------------------------------_Idemocrat_1 | 55.43136 1.448568 38.27 0.000 52.58538 58.27734demvoteshare_c | -5.682785 5.939863 -0.96 0.339 -17.35273 5.987156_IdemXdemvo_1 | -55.15188 8.236231 -6.70 0.000 -71.33346 -38.97031cons | 16.81598 .9050024 18.58 0.000 15.03794 18.59403--------------------------------------------------------------------------------

### Slide 437

Other bandwidths
. * Use +/-0.1 from cutoff (i.e., .4 to .6). xi: reg score i.democrat*demvoteshare_c if demvoteshare>.40 & demvoteshare<.60, cluster(id2)i.democrat _Idemocrat_0-1 (naturally coded; _Idemocrat_0 omitted)i.demo~t*demv~c _IdemXdemvo_# (coded as above)
Linear regression Number of obs = 4632<snip> (Std. Err. adjusted for 428 clusters in id2)--------------------------------------------------------------------------------| Robustscore | Coef. Std. Err. t P>|t| [95% Conf. Interval]---------------+----------------------------------------------------------------_Idemocrat_1 | 47.15915 1.810413 26.05 0.000 43.60072 50.71758demvoteshare_c | 9.421594 18.33431 0.51 0.608 -26.61514 45.45832_IdemXdemvo_1 | -9.127629 31.8722 -0.29 0.775 -71.77357 53.51831cons | 17.22656 1.239458 13.90 0.000 14.79036 19.66276--------------------------------------------------------------------------------
* Use +/- 0.05. xi: reg score i.democrat*demvoteshare_c if demvoteshare>.45 & demvoteshare<.55, cluster(id2)i.democrat _Idemocrat_0-1 (naturally coded; _Idemocrat_0 omitted)i.demo~t*demv~c _IdemXdemvo_# (coded as above)
Linear regression Number of obs = 2387<snip> (Std. Err. adjusted for 367 clusters in id2)--------------------------------------------------------------------------------| Robustscore | Coef. Std. Err. t P>|t| [95% Conf. Interval]---------------+----------------------------------------------------------------_Idemocrat_1 | 46.77845 2.491464 18.78 0.000 41.87906 51.67783demvoteshare_c | 54.82604 50.12314 1.09 0.275 -43.73946 153.3915_IdemXdemvo_1 | -91.1152 81.05893 -1.12 0.262 -250.5149 68.28449cons | 18.09713 1.693516 10.69 0.000 14.76689 21.42737--------------------------------------------------------------------------------
* Use +/- 0.01. xi: reg score i.democrat*demvoteshare_c if demvoteshare>.49 & demvoteshare<.51, cluster(id2)i.democrat _Idemocrat_0-1 (naturally coded; _Idemocrat_0 omitted)i.demo~t*demv~c _IdemXdemvo_# (coded as above)
Linear regression Number of obs = 441<snip> (Std. Err. adjusted for 160 clusters in id2)--------------------------------------------------------------------------------| Robustscore | Coef. Std. Err. t P>|t| [95% Conf. Interval]---------------+----------------------------------------------------------------_Idemocrat_1 | 45.27375 5.813497 7.79 0.000 33.79212 56.75539demvoteshare_c | 55.75797 716.0422 0.08 0.938 -1358.423 1469.939_IdemXdemvo_1 | -88.56438 955.1282 -0.09 0.926 -1974.939 1797.81cons | 20.60166 4.514799 4.56 0.000 11.68495 29.51837--------------------------------------------------------------------------------

### Slide 438

No voteshare control vs. polynomials
. * Narrow bandwidth, no vote share control (this is comparing means close to cutoff). reg score democrat if demvoteshare>.49 & demvoteshare<.51, cluster(id2)
Linear regression Number of obs = 441F( 1, 159) = 244.90Prob > F = 0.0000R-squared = 0.5525Root MSE = 20.456
(Std. Err. adjusted for 160 clusters in id2)------------------------------------------------------------------------------| Robustscore | Coef. Std. Err. t P>|t| [95% Conf. Interval]-------------+----------------------------------------------------------------democrat | 45.3931 2.900628 15.65 0.000 39.66438 51.12183cons | 20.31812 2.026848 10.02 0.000 16.3151 24.32114------------------------------------------------------------------------------
. * What about a polynomial of degree 5?. gen demvoteshare2=demvoteshare^2(11 missing values generated)
. gen demvoteshare3=demvoteshare^3(11 missing values generated)
. gen demvoteshare4=demvoteshare^4(11 missing values generated)
. gen demvoteshare5=demvoteshare^5(11 missing values generated)
. reg score demvoteshare demvoteshare2 demvoteshare3 demvoteshare4 demvoteshare5 demvoteshare5 democrat, cluster(id2)note: demvoteshare5 omitted because of collinearity
Linear regression Number of obs = 13577<snip> (Std. Err. adjusted for 505 clusters in id2)-------------------------------------------------------------------------------| Robustscore | Coef. Std. Err. t P>|t| [95% Conf. Interval]--------------+----------------------------------------------------------------demvoteshare | -19.30281 71.97002 -0.27 0.789 -160.701 122.0954demvoteshare2 | 424.0805 543.3939 0.78 0.436 -643.5157 1491.677demvoteshare3 | -1746.451 1475.699 -1.18 0.237 -4645.731 1152.828demvoteshare4 | 2559.367 1666.599 1.54 0.125 -714.9694 5833.704demvoteshare5 | -1248.399 664.662 -1.88 0.061 -2554.249 57.45029demvoteshare5 | 0 (omitted)democrat | 49.13039 1.764906 27.84 0.000 45.66291 52.59787cons | 16.36452 3.07522 5.32 0.000 10.32269 22.40635-------------------------------------------------------------------------------

### Slide 439

Polynomial with interactions
. * Center the running variable and use polynomials and interactions to model* the nonlinearities below and above discontinuity. gen xc=demvoteshare-0.5(11 missing values generated)
. gen x2c=xc^2(11 missing values generated)
. gen x3c=xc^3(11 missing values generated)
. gen x4c=xc^4(11 missing values generated)
. gen x5c=xc^5(11 missing values generated)
.. reg score i.democrat##(c.xc c.x2c c.x3c c.x4c c.x5c)
Source | SS df MS Number of obs = 13577-------------+------------------------------ F( 11, 13565) = 1058.32Model | 6677033.81 11 607003.073 Prob > F = 0.0000Residual | 7780253.69 13565 573.553534 R-squared = 0.4618-------------+------------------------------ Adj R-squared = 0.4614Total | 14457287.5 13576 1064.91511 Root MSE = 23.949
---------------------------------------------------------------------------------score | Coef. Std. Err. t P>|t| [95% Conf. Interval]----------------+----------------------------------------------------------------1.democrat | 47.73325 2.042906 23.37 0.000 43.72887 51.73763xc | -28.79765 74.09167 -0.39 0.698 -174.0276 116.4323x2c | -1138.232 1144.497 -0.99 0.320 -3381.605 1105.141x3c | -10681.29 7137.315 -1.50 0.135 -24671.42 3308.839x4c | -33490.23 18844.92 -1.78 0.076 -70428.88 3448.424x5c | -32873.77 17212.08 -1.91 0.056 -66611.83 864.302|democrat#c.xc |1 | -5.793828 97.79088 -0.06 0.953 -197.4775 185.8899|democrat#c.x2c |1 | 1768.225 1433.252 1.23 0.217 -1041.149 4577.599|democrat#c.x3c |1 | 6279.346 8553.95 0.73 0.463 -10487.58 23046.28|democrat#c.x4c |1 | 47111.44 21834.51 2.16 0.031 4312.767 89910.11|democrat#c.x5c |1 | 17786.85 19488.24 0.91 0.361 -20412.81 55986.51|cons | 17.05847 1.469785 11.61 0.000 14.17749 19.93946---------------------------------------------------------------------------------

### Slide 440

More polynomial and bandwidth regressions
. * We could limit the regressions to a window using a 2nd degree polynomial
. * interacted with the treatment variable
. reg score i.democrat##(c.xc c.x2c) if (demvoteshare>0.4 & demvoteshare<0.6)
Source | SS df MS Number of obs = 4632
-------------+------------------------------ F( 5, 4626) = 1153.29
Model | 2622762.02 5 524552.404 Prob > F = 0.0000
Residual | 2104043.2 4626 454.829918 R-squared = 0.5549
-------------+------------------------------ Adj R-squared = 0.5544
Total | 4726805.22 4631 1020.6878 Root MSE = 21.327
---------------------------------------------------------------------------------
score | Coef. Std. Err. t P>|t| [95% Conf. Interval]
----------------+----------------------------------------------------------------
1.democrat | 45.9283 1.892566 24.27 0.000 42.21797 49.63863
xc | 38.63988 60.77525 0.64 0.525 -80.5086 157.7884
x2c | 295.1723 594.3159 0.50 0.619 -869.9704 1460.315
|
democrat#c.xc |
1 | 6.507415 88.51418 0.07 0.941 -167.0226 180.0374
|
democrat#c.x2c |
1 | -744.0247 862.0435 -0.86 0.388 -2434.041 945.9916
|
cons | 17.71198 1.310861 13.51 0.000 15.14207 20.28189
---------------------------------------------------------------------------------

### Slide 441

Nonparametric estimation
Hahn, Todd and Van der Klaauw (2001) clariﬁed assumptions about RDD (i.e.,
continuity in conditional expectation regression functions)
Also framed estimation as a non-parametric problem and emphasized using
local polynomial regressions
Nonparametric methods mean a lot of diﬀerent things to diﬀerent people in
statistics.
In RDD context, the idea is to estimate a model that doesn’t assume a
functional form for the relationship betweenY (outcome variable) andX
(running variable)
That model would be something general like
Y =f(X) +ε
A very basic method is to calculateE[Y] for each bin onX – like a histogram.
STATA has an option to do this calledcmogramand it has a lot of useful
options. We can recreate Figures I, IIA and IIB using it

### Slide 442

Graphical estimate of equation 205
be a continuous and smooth function of vote shares everywhere,
except at the threshold that determines party membership. There
is a large discontinuous jump in ADA scores at the 50 percent
threshold. Compare districts where the Democrat candidate
barely lost in period t (for example, vote share is 49.5 percent),
with districts where the Democrat candidate barely won (for
example, vote share is 50.5 percent). If the regression disconti-
nuity design is valid, the two groups of districts should appear ex
ante similar in every respect — on average. The difference will be
that in one group, the Democrats will be the incumbent for the
next election ( t /H110011), and in the other it will be the Republicans.
Districts where the Democrats are the incumbent party for elec-
tion t /H110011 elect representatives who have much higher ADA
scores, compared with districts where the Republican candidate
FIGURE I
Total Effect of Initial Win on Future ADA Scores: /H9253
This ﬁgure plots ADA scores after the election at time t /H110011 against the
Democrat vote share, time t. Each circle is the average ADA score within 0.01
intervals of the Democrat vote share. Solid lines are ﬁtted values from fourth-
order polynomial regressions on either side of the discontinuity. Dotted lines are
pointwise 95 percent con ﬁdence intervals. The discontinuity gap estimates
/H9253/H11005/H92660/H20849P*t /H110011
D /H11002P*t /H110011
R /H20850/H11001/H92661/H20849P*t /H110011
D /H11002P*t /H110011
R /H20850.
“Affect”“ Elect”
828 QUARTERLY JOURNAL OF ECONOMICS
 at Baylor University on April 7, 2014http://qje.oxfordjournals.org/Downloaded from 
Figure: Lee, Moretti, and Butler 2004, Figure I. γ ≈ 20

### Slide 443

Nonparametric estimation withcmogram. cmogram score lagdemvoteshare , cut(0.5) scatter line(0.5) qfitciPlotting mean of score, conditional on lagdemvoteshare.n = 13577Bin #1: [0,.0135101361854656] (n = 181) (mean = 18.90983436977007)Bin #2: (.0135101361854656,.0270202723709312] (n = 0) (mean = .)Bin #3: (.0270202723709312,.0405304085563968] (n = 0) (mean = .)Bin #4: (.0405304085563968,.0540405447418624] (n = 0) (mean = .)Bin #5: (.0540405447418624,.067550680927328] (n = 0) (mean = .)Bin #6: (.067550680927328,.0810608171127936] (n = 2) (mean = 11.2350001335144)<snip>Bin #36: (.9605263157894751,.9736842105263173] (n = 2) (mean = 96.58000183105469)Bin #37: (.9736842105263173,.9868421052631594] (n = 0) (mean = .)Bin #38: (.9868421052631594,1] (n = 1646) (mean = 33.014483642933)

### Slide 444

Linear and lowess ﬁts
. cmogram score lagdemvoteshare , cut(0.5) scatter line(0.5) lfit
Plotting mean of score, conditional on lagdemvoteshare.
. cmogram score lagdemvoteshare , cut(0.5) scatter line(0.5) lowess
Plotting mean of score, conditional on lagdemvoteshare.

### Slide 445

Graphical estimate of equation 206
FIGURE IIb
Effect of Initial Win on Winning Next Election: ( Pt/H110011
D /H11002Pt/H110011
R )
Top panel plots ADA scores after the election at time t against the Democrat
vote share, time t. Bottom panel plots probability of Democrat victory at t /H110011
against Democrat vote share, time t. See caption of Figure III for more details.
FIGURE IIa
Effect of Party Af ﬁliation: /H92661
830 QUARTERLY JOURNAL OF ECONOMICS
 at Baylor University on April 7, 2014http://qje.oxfordjournals.org/Downloaded from 
Figure: Lee, Moretti, and Butler 2004, Figure IIa. π1 ≈ 45

### Slide 446

* Lowesscmogram score demvoteshare, cut(.5) scatter line(.5) lowess
Figure: Cubic feet with conﬁdence intervals

### Slide 447

Graphical estimate of equation 207
FIGURE IIb
Effect of Initial Win on Winning Next Election: ( Pt/H110011
D /H11002Pt/H110011
R )
Top panel plots ADA scores after the election at time t against the Democrat
vote share, time t. Bottom panel plots probability of Democrat victory at t /H110011
against Democrat vote share, time t. See caption of Figure III for more details.
FIGURE IIa
Effect of Party Af ﬁliation: /H92661
830 QUARTERLY JOURNAL OF ECONOMICS
 at Baylor University on April 7, 2014http://qje.oxfordjournals.org/Downloaded from 
Figure: Lee, Moretti, and Butler 2004, Figure IIb. ( P D
t+1 − P R
t+1) ≈ 0.50

### Slide 448

Kernel weighted local polynomial regression
Hahn, Todd and Van der Klaauw (2001) showed that the one-sided kernel
estimation (such aslowess) may have poor properties because the point of
interest is at a boundary (i.e., the discontinuity), called the “boundary problem”
They proposed to use “local linear nonparametric regressions” instead
STATA’spolyestimates kernel-weighted local polynomial regressions. Think of
it as a weighted regression restricted to a window like we’ve been doing (hence
“local”) where the kernel provides the weights
A rectangular kernel would give the same result asE[Y] at a given bin onX.
The triangular kernel gives more importance to observations close to the center.
This method will be sensitive to how large the bandwidth (window) you choose

### Slide 449

. * Note kernel-weighted local polynomial regression is a smoothing method.. lpoly score demvoteshare if democrat == 0, nograph kernel(triangle) gen(x0 sdem0) ///> bwidth(0.1)
. lpoly score demvoteshare if democrat == 1, nograph kernel(triangle) gen(x1 sdem1) ///> bwidth(0.1)
. scatter sdem1 x1, color(red) msize(small) || scatter sdem0 x0, msize(small) color(red) ///> xline(0.5,lstyle(dot)) legend(off) xtitle("Democratic vote share") ytitle("ADA score")
. * Next, let’s get the treatment effect at the cutoff where demvoteshare=0.5. capture drop sdem0 sdem1. gen forat=0.5 in 1. lpoly score demvoteshare if democrat==0, nograph kernel(triangle) gen(sdem0) at(forat) bwidth(0.1). lpoly score demvoteshare if democrat==1, nograph kernel(triangle) gen(sdem1) at(forat) bwidth(0.1). gen late=sdem1 - sdem0. list sdem1 sdem0 late in 1/1+----------------------------------+| sdem1 sdem0 late ||----------------------------------|1. | 64.395204 16.908821 47.48639 |+----------------------------------+

### Slide 450

. * What happens when we change the bandwidth? Use 0.01, 0.05, 0.2, 0.3, 0.4. capture drop smoothdem0* smoothdem1* x0* x1*. local co 0. foreach i in 0.01 0.05 0.1 0.20 0.30 0.402. local co = ‘co’ +13. lpoly score demvoteshare if democrat == 0, nograph kernel(triangle) gen(x0‘co’ smoothdem0‘co’) ///> bwidth(‘i’)4. lpoly score demvoteshare if democrat == 1, nograph kernel(triangle) gen(x1‘co’ smoothdem1‘co’) ///> bwidth(‘i’)5.
. line smoothdem01 x01, msize(small) color(gray) sort || line smoothdem11 x11, sort color(gray) || ///> line smoothdem02 x02, color(black) sort || line smoothdem12 x12, sort color(black) || ///> line smoothdem03 x03, color(red) sort || line smoothdem13 x13, sort color(red) || ///> line smoothdem04 x04, color(blue) sort || line smoothdem14 x14, sort color(blue) || ///> line smoothdem05 x05, color(green)sort || line smoothdem15 x15, sort color(green)|| ///> line smoothdem06 x06, color(orange) sort || line smoothdem16 x16, sort color(orange) ///> xline(0.5,lstyle(dot)) legend(off) xtitle("Democratic vote share") ytitle("ADA score") ///> title("Bandwidths: 0.01, 0.05, 0.1, 0.2, 0.3, 0.4")

### Slide 451

Several methods for choosing the optimal bandwidth (window), but it’s always
a trade oﬀ between bias and variance
In practical applications, you want to check for balance around that window
Standard error of the treatment eﬀects can be bootstrapped but there are also
other alternatives
You could add other variables to nonparametric methods.
Calonico, Cattaneao and Titiunik (2013b) propose local-polynomial regression
discontinuity estimators with robust conﬁdence intervals
STATA ado package and R package are both calledrdrobust

### Slide 452

. rdrobust score demvoteshare, c(0.5) all bwselect(IK)Preparing data.Computing Bandwidth Selectors.Computing Variance-Covariance Matrix.Computing RD Estimates.Estimation Completed.
Sharp RD Estimates using Local Polynomial Regression.
Cutoff c = .5 | Left of c Right of c Number of obs = 13577----------------------+---------------------- NN Matches = 3Number of obs | 3535 3318 BW Type = IKOrder Loc. Poly. (p) | 1 1 Kernel Type = TriangularOrder Bias (q) | 2 2BW Loc. Poly. (h) | 0.152 0.152BW Bias (b) | 0.191 0.191rho (h/b) | 0.795 0.795
Outcome: score. Running Variable: demvoteshare.--------------------------------------------------------------------------------------Method | Coef. Std. Err. z P>|z| [95% Conf. Interval]----------------------+---------------------------------------------------------------Conventional | 47.171 .98058 48.1046 0.000 45.2488 49.0926Robust | - - 36.4839 0.000 44.0965 49.1034--------------------------------------------------------------------------------------
All Estimates.--------------------------------------------------------------------------------------Method | Coef. Std. Err. z P>|z| [95% Conf. Interval]----------------------+---------------------------------------------------------------Conventional | 47.171 .98058 48.1046 0.000 45.2488 49.0926Bias-Corrected | 46.6 .98058 47.5226 0.000 44.678 48.5219Robust | 46.6 1.2773 36.4839 0.000 44.0965 49.1034--------------------------------------------------------------------------------------

### Slide 453

McCrary Density Test
. * McCrary (2008) density test to check for manipulation of the running variable (DCdensity). DCdensity demvoteshare_c if (demvoteshare_c>-0.5 & demvoteshare_c<0.5), breakpoint(0) generate(Xj Yj r0 fhat se_fhat)Using default bin size calculation, bin size = .003047982Using default bandwidth calculation, bandwidth = .104944836
Discontinuity estimate (log difference in height): .011195629(.061618519)Performing LLR smoothing.296 iterations will be performed

### Slide 454

“Pioneers take the arrows in the back”
– Jim West during a faculty meeting
Lee (2008) is important for the econometric theory, but this
particular application turned out to be wrong:
“Contrary to the assumptions of RD, we show that bare winners and bare
losers in US House elections (1942-2008) diﬀer markedly on pretreatment
ovariates. Bare winners possess large ex ante ﬁnancial, experience, and
incumbency advantages over their opponents and are usually the
candidates predicted to win by Congressional Quarterly—’s pre-election
ratings. Covariate imbalance actually worsens in the closest House
election. National partisan tides help explain these patterns. We present
evidence suggesting that sorting in close House elections is due mainly to
activities on or before Election Day rather than post election recounts or
other manipulation. The sorting is so strong that it is impossible to
achieve covariate balance between matched treated and control
observations, making covariate adjustment a dubious enterprise. Although
RD is problematic for postwar House elections, this example does
highlight the design’s advantages over alternatives: RD’s assumptions are
clear and weaker than model-based alternatives, and their implications are
empirically testable.” (Caughey and Sekhon, 2011).

### Slide 455

Regression Kink Design
Card, Lee, Pei and Weber (2012) introduces a variant of the
RDD which they call regression kink design (RKD)
They essentially use a kink in some policy rule to identify the
causal eﬀect of the policy
Instead of a jump in the outcome you now expect a jump in
the ﬁrst derivative

### Slide 456

Unemployment beneﬁts in Austria
They apply their design to answer the question whether the
level of unemployment beneﬁts aﬀects the length of
unemployment in Austria
Unemployment beneﬁts are based on income in a base period
The beneﬁt formula for unemployment exhibits 2 kinks
There is a minimum beneﬁt level that isn’t binding for people
with low earnings
Then beneﬁts are 55% of the earnings in the base period
There is a maximum beneﬁt level that is adjusted every year
People with dependents get small supplements (which is the
reason why one can distinguish ﬁve “solid” lines in the
following graph)
Not everyone receives beneﬁts that correspond one to one to
the formula because of mistakes in the administrative data

### Slide 457

Base Year Earnings and Unemployment Beneﬁts
Base Year Earnings and Unemployment Beneﬁts
The graph shows unemployment beneﬁts (vertical axis) as a function of
pre-unemployment earnings (horizontal axis).
Waldinger (Warwick) 46 / 48
The graph shows unemployment beneﬁts (vertical axis) as a
function of pre-unemployment earnings (horizontal axis)

### Slide 458

Base Year Earnings and Beneﬁts for Single Individuals
Base Year Earnings and Beneﬁts for Single Individuals
Bin-Size: 100 Euros
For single individuals UI beneﬁts are ﬂat below the cuto §.T h e
relationship is still upward sloping because of family beneﬁts.
Waldinger (Warwick) 47 / 48
Bin-size: 100 euros
For single individuals UI beneﬁts are ﬂat below the cutoﬀ. The
relationship is still upward sloping because of family beneﬁts.

### Slide 459

Time to Next Job for Single Individuals
Time to Next Job For Single Individuals
People with higher base earnings have less trouble ﬁnding a job
(negative slope).
There is a kink: the relationship becomes shallower once beneﬁts
increase more.
Waldinger (Warwick) 48 / 48
People with higher base earnings have less trouble ﬁnding a
job (negative slope)
There is a kink: the relationship becomes shallower once
beneﬁts increase more.

### Slide 460

Selection on unobservables
Recall the identifying assumption when estimating causal
eﬀects using a conditioning strategy
Independence: ( Y 0, Y 1) ⊥ ⊥D
Use methods like propensity score matching and condition on
p(x), or linear regression
What if treatment assignment to units is based on some
unobserved variable, u, which is correlated with the outcome
Conditioning strategies are invalid
Selection on unobservable methods, though, may work
Natural experiments may be useful

### Slide 461

Natural experiments
Natural experiments are neither an estimator or an experiment
(natural or otherwise). Rather, it is simply an event that
occurs naturally which causes exogenous variation in some
treatment variable of interest
An attempt to ﬁnd in the world some rare circumstance such
that a consequential treatment was handed to some people
and denied to others haphazardly
“The word natural has various connotations, but a natural
experiment is a wild experiment not a wholesome experiment ;
nature in the way that a tiger is natural, not in the way that
oatmeal is natural.” (Rosenbaum 2005)
Haphazard variation in treatment is not like the variation
induced by a randomized experiment

### Slide 462

Natural experiments
Naive matching – “people who look comparable are
comparable”. False.
Matching can create matched samples of units that look
similar on some x
Matching cannot create a matched sample who are similar on
u (unobservables), though
Deﬁnition of naive: someone who believes something because
it’s convenient to believe it.

### Slide 463

Broad Street Pump
John Snow is believed to be the bastard son of Edward Stark
but is actually a Targaryan
But before that John Snow, there was another John Snow – a
practicing anesthesiologist in 19th century London
This Snow is considered the father of epidemiology
He is famous for providing convincing evidence that cholera
was a waterborne disease
He lived from 1813-1858

### Slide 464

19th century science
Microscopes were around but had horrible resolution
Most human pathogens couldn’t be seen
Isolating these microorganisms wouldn’t occur for half a
century
Two views of what caused disease:
Majority view - “Miasmas”. Minute, inanimate poison particles
in the air
Minority view - “infection theory”.

### Slide 465

Cholera background
Cholera arrives in the early 1800s and exhibits “epidemic
waves”.
Cholera attacked victims suddenly, was usually fatal, and
symptoms included vomiting and acute diarrhea
Snow observed the clinical course of the disease and made the
following conjecture
The active agent was a living organism that entered the body,
got into the alimentary canal with food or drink, multiplied in
the body, and generated some poison that caused the body to
expel water
The organism passed out of the body with these evacuations,
entered the water supply and infected new victims
The process repeated itself, growing rapidly through the
common water supply, causing an epidemic

### Slide 466

History
There were three main epidemics in London
1831-1832
1848-1849 (>15,000 deaths)
1853-1854 (>30,000 deaths)

### Slide 467

Background
Snow is an early advocate for the infection theory – cholera is
being spread person-to-person through some unknown
mechanism
His earlier evidence was based on years of observations
including
Cholera transmission tended to follow human commerce
A sailor on a ship from a cholera-free country who arrived at a
cholera-stricken port would only get sick after landing or
taking on supplies
Cholera hit the poor communities the worst, who also lived in
the most crowded housing with the worst hygeine
Facts were diﬃcult to reconcile with the miasma theory, but
are consistent with infection theory

### Slide 468

More evidence
Snow identiﬁes Patient Zero: the ﬁrst case of an early
epidemic
“a seaman named John Harnold, who had arrived by the Elbe
steamer from Hamburg, where the disease was prevailing”
The second case? John Harnold’s roommate

### Slide 469

More evidence
Snow studied two apartment buildings. The ﬁrst was heavily
hit with cholera but the second wasn’t.
He found the water supply in the ﬁrst building was
contaminated by runoﬀ from privies but the water supply in
the second was cleaner
Earlier water supply studies
In the London of the 1800s, there were many diﬀerent water
companies serving diﬀerent areas of the city
Some were served by more than one company
Several took their water from the Thames, which was heavily
polluted by sewage
The service areas of such companies had much higher rates of
cholera
The Chelsea water company was an exception, but it had an
exceptionally good ﬁltration system

### Slide 470

Broad Street Water Pump
1849: Lambeth water company moves the intake point
upstream along the Thames, above the main sewage discharge
point (i.e., purer water)
1849: Southwark and Vauxhall water company left their
intake point downstream from where the sewage discharged
(gross) (i.e., infected water)
Comparisons of data on cholera deaths from the 1853-1854
year showed the epidemic hit the Southwark and Vauxhall
services areas harder, and largely spared the Lambeth areas

### Slide 471

Snow’s Table IX
No. houses Cholera deaths Deaths / 10,000 houses
Southwark and Vauxhall 40,046 1,263 315
Lambeth 26,107 98 37
Rest of London 256,423 1,422 59

### Slide 472

Shoe leather research
“As a piece of statistical technology, [Snow’s Table IX] is
by no means remarkable. But the story it tells is very
persuasive. The force of the argument results from the
clarity of the prior reasoning, the bringing together of
many diﬀerent lines of evidence, and the amount of shoe
leather Snow was willing to use to get the data.”
“Snow did some brilliant detective work on
nonexperimental data. What is impressive is not the
statistical technique but the handling of the scientiﬁc
issues. He made steady progress from shrewd observation
through case studies to analyze ecological data. In the
end, he found and analyzed a natural experiment.”

### Slide 473

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
Introduction to instrumental variables
Pearl argues that there are three ways to estimate causal eﬀects: the backdoor
criterion (“selection on observables”), instrumental variables, and the front
door criterion.
Now we move into the instrumental variables research design
Illustrious history – discovered by Philip Wright and published as an appendix
in his 1928 book
One of the most powerful methods for identifying causal eﬀects in the social
sciences
Cunningham Causal Inference

### Slide 474

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
Sometimes, the researchercanﬁnd the ﬂag. That is, the researcher knows of a
variable (Z) that actuallyis randomly assigned and that aﬀects fertility
decisions. Such a variable is called an “instrument”.
Example: Angrist and Evans (1998), “Children and their parents’ labor supply”
American Economic Review,
Z is a dummy variable indicating whether the ﬁrst two children born were of the
same gender
Many parents have a preference for having at least one child of each gender
Consider a couple whose ﬁrst two kids were both boys; they will often have a
third, hoping to have a girl
Consider a couple whose ﬁrst two kids were girls; they will often have a third,
hoping for a boy
Consider a couple with one boy and one girls; they will often not have a third kid
The gender if your kids is arguably randomly assigned (maybe not exactly, but
close enough)
Cunningham Causal Inference

### Slide 475

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
“No causation without manipulation” (Holland, 1985). If you want to use IV,
then ask:
What moves around the covariate of interest that might be plausibly viewed as random?
“Focusing on the selection process”
How was the covariate of interest selected?
Do researchers select randomly, as in drawing balls from an urn?
Or do subjects choose the covariate partially based on random factors and
partially based on non-random factors?
If the random factors underlying the decision can beobserved by the researcher,
then you can use IV to estimate the eﬀect of the covariate of interest on the
outcome
Angrist and Evans (1988) example:
Families with at least two kids are the subject population
The covariate of interest is the number of kids (numkids).
There are several outcomes of interest measuring labor market decisions of the
couple, such as whether the mother worked for pay in the last year (workforpay).
Once you have identiﬁed such a variable, begin to think about what data sets
might have information on an outcome of interest, the covariate of interest,
and the instrument you have put your ﬁnger on.
Cunningham Causal Inference

### Slide 476

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
In a pinch, you can even get by with two diﬀerent data sets,
one of which has information on the outcome and the
instrument, and the other of which has information on the
covariate of interest and the instrument.
This is known as “Two sample IV” because there are two
samples involved, rather than the traditional one sample.
Once we deﬁne what IV is measuring carefully, you will see
why this works.
Cunningham Causal Inference

### Slide 477

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
Instrumental variables strategies formalizeuntainted inference, which is the
inference drawn by an intelligent layperson with no particular training or
background in statistics.
Example of what I mean by “untainted inference”:
The researchers tell a layperson that the gender of a woman’s ﬁrst two children is
predictive of her labor force attachment. In particular, women whose ﬁrst two
children are of the same gender work substantially less than women whose ﬁrst
two children are of diﬀerent genders
On its face, this is a puzzling fact – without further information, it is hard to see
why the gender of your children would be so predictive of labor market
participation
The researchers additionally point out that women whose ﬁrst two children are of
the same gender are more likely to have additional children than women whose
ﬁrst two children are of diﬀerent genders
The layperson then wonders whether the labor market diﬀerences are duesolely
to the diﬀerences in the number of kids the woman has
Cunningham Causal Inference

### Slide 478

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
Traditional and Contemporary IV Pedagogy
We want to learn the IV framework in two iterations:
1 Constant treatment eﬀects (i.e.,β is constant across all individual units)
Constant treatment eﬀects is the traditional econometric pedagogy when ﬁrst
learning instrumental variables, and is not based explicitly on the potential
outcomes model or notation
Constant treatment eﬀects is identical to assuming that ATE=ATT=ATU
because constant treatment eﬀects assumesβi =β−i =β for all units
2 Heterogeneous treatment eﬀects (i.e.,βi varies across individual units)
This is the “modern IV pedagogy”, and you may not have learned it in
econometrics if only because potential outcomes is not ordinarily the basis of
most ﬁrst year econometrics sequences
Heterogeneous treatment eﬀects means that theATE̸=ATT̸=ATUbecauseβi
diﬀers across the population
This is equivalent to assuming the coeﬃcient,βi, is a random variable that varies
across the population
Heterogenous treatment eﬀects is based on work by Angrist, Imbens and Rubin
(1996) and Imbens and Angrist (1994) which introduced the “local average
treatment eﬀect” (LATE) concept
Cunningham Causal Inference

### Slide 479

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
When should you think of using instrumental variables?
Instrumental variables methods are typically used to address
the following kinds of problems encountered in OLS
regressions:
1 Omitted variable bias
2 Measurement error
3 Simultaneity bias, or “reverse causality”
Cunningham Causal Inference

### Slide 480

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
Omitted Variable Bias (Angrist and Pischke, 2009)
Labor economists have been studying the returns to schooling a long time –typically some version of a “Mincer regression”:
Yi =α+ρSi +γAi +νi
Yi = log of earnings
Si = schooling measured in years
Ai = individual ability
Typically the econometrician cannot observeAi; for instance, the CPS tells usnothing about adult respondents’ family background, intelligence, ormotivation.
What are the consequences of leaving ability out of the regression? Supposeyou estimated this short regression instead:
Yi =α+ρSi +ηi
whereηi =γAi +νi; α, ρ, andγare population regression coeﬃcients;Si iscorrelated withηi throughAi only; andvi is a regression residual uncorrelatedwith all regressors by deﬁnition.
Cunningham Causal Inference

### Slide 481

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
Derivation of Ability Bias
Suppressing thei subscripts, the OLS estimator forρis:
ˆρ=Cov(Y,S)
Var(S) =E[YS]−E[Y]E[S]
Var(S)
Plugging in the true model forY, we get:
ˆρ = Cov[(α+ρS+γA+ν),S]
Var(S)
= E[(αS+S2ρ+γSA+νS)]−E(S)E[α+ρS+Aγ+ν]
Var(S)
= ρE(S2)−ρE(S)2+γE(AS)−γE(S)E(A) +E(νS)−E(S)E(ν)
Var(S)
= ρ+γCov(AS)
Var(S)
Ifγ >0 and Cov(A,S)>0 the coeﬃcient on schooling in the shortened
regression (without controlling forA) would be upward biased
Cunningham Causal Inference

### Slide 482

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
How can IV be used to obtain unbiased estimates?
Suppose there exists a variable,Zi, that is correlated withSi .
We can estimateρwith this variable,Z:
Cov(Y,Z) = Cov(α+ρS+γA+ν,Z)
= E[(α+ρS+γA+ν),Z]−E[α+ρS+γA+ν]E[Z]
= {αE(Z)−αE(Z)}+ρ{E(SZ)−E(S)E(Z)}
+γ{E(AZ)−E(A)E(Z)}+E(νZ)−E(ν)E(Z)
Cov(Y,Z) = ρCov(S,Z) +γCov(A,Z) +Cov(ν,Z)
What conditions must hold for a valid instrumental variable?
Cov(S,Z)̸= 0 – “ﬁrst stage” exists.SandZ are correlated
Cov(A,Z) =Cov(ν,Z) = 0 – “exclusion restriction”.Z is orthogonal to the
factors inη, such as unobserved ability or the structural disturbance term,η
Assuming the ﬁrst stage exists and that the exclusion restriction holds, then we
can estimateρwithρIV:
ρIV=Cov(Y,Z)
Cov(S,Z) =ρ
Cunningham Causal Inference

### Slide 483

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
IV is Consistent if IV Assumptions are Satisﬁed
The IV estimator is consistent if the IV assumptions are satisﬁed. Substitute
true model forY:
ρIV = Cov([α+ρS+γA+ν],Z)
Cov(S,Z)
= ρCov([S],Z)
Cov(S,Z) +γCov([A],Z)
Cov(S,Z) +Cov([ν],Z)
Cov(S,Z)
= ρ+γCov(η,Z)
Cov(S,Z)
Taking the plim:
plimˆρIV =ρ
becauseCov([A],Z) = 0 andCov([ν],Z) = 0 due to the exclusion restriction,
andCov(S,Z)̸= 0 (due to the ﬁrst stage)
But, ifZ isnotindependent ofη (either correlated withAorν),andif the
correlation betweenS andZ is “weak”, then the second term blows up. We
will return to this later when we discuss the problem of “weak instruments”.
Cunningham Causal Inference

### Slide 484

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
In which DAG is Z a valid instrument for D?
e
YD
Z
(a)
e
YD
Z
(b)
Cunningham Causal Inference

### Slide 485

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
Reviewing some of the IV Jargon
Causal model. Sometimes called the structural model:
Yi =α+ρSi +ηi
First-stage regression. Gets the name because of two-stage least squares:
Si =α+ρZi +ζi
Second-stage regression. Notice the ﬁtted values,ˆS:
Yi =α+ρˆSi +νi
Reduced form. Notice this is just a regression ofY onto the instrument:
Yi =α+πZi +εi
Cunningham Causal Inference

### Slide 486

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
Two-stages least squares
Suppose you have a sample of data onY, X, andZ. For each observationi weassume the data are generated according to
Yi = α+βXi +εi
Xi = γ+δZi +νi
whereCov(Z, ε) = 0 andδ̸= 0.
Plug in covariance, and using the result that∑ni=1(xi −x) = 0, write out theIV estimator:
ˆβIV=Cov(Z,Y)
Cov(Z,X)=
1n
∑ni=1(Zi −Z)(Yi −Y)
1n
∑ni=1(Zi −Z)(Xi −X)=
1n
∑ni=1(Zi −Z)Yi
1n
∑ni=1(Zi −Z)Xi
Substitute the causal model deﬁnition ofYto get:
ˆβIV =
1n
∑ni=1(Zi −Z){α+βX+ε}
1n
∑ni=1(Zi −Z)Xi
= β+
1n(Zi −Z)εi
1n
∑ni=1(Zi −Z)Xi
= β+ ”small ifnis large”
Cunningham Causal Inference

### Slide 487

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
Two-stages least squares
NoteβIV is ratio of “reduced form” (π) to “ﬁrst stage” coeﬃcient (δ):
ˆβIV = Cov(Z,Y)
Cov(Z,X) =
Cov(Z,Y)
Var(Z)
Cov(Z,X)
Var(Z)
= ˆπ
ˆδ
Rewriteˆδ as
ˆδ = Cov(Z,X)
Var(Z) ⇔Cov(Z,X) =ˆδVar(Z) (100)
Then rewriteβIV
ˆβIV = Cov(Z,Y)
Cov(Z,X) = ˆδCov(Z,Y)
ˆδCov(Z,X) = ˆδCov(Z,Y)
ˆδ2Var(Z)
= Cov(ˆδZ,Y)
Var(ˆδZ) (101)
Cunningham Causal Inference

### Slide 488

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
Two-stage least squares
RecallX=γ+δZ+ν;ˆβIV=Cov(ˆδZ,Y)
Var(ˆδZ) and letˆX=ˆγ+ˆδZ.
Then the two-stage least squares (2SLS) estimator is
ˆβIV = Cov(ˆδZ,Y)
Var(ˆδZ) =Cov(ˆX,Y)
Var(ˆX)
Proof.
We will show thatˆδCov(Y,Z) =Cov(ˆX,Y). I will leave it to you to show thatVar(ˆδZ) =Var(ˆX)
Cov(ˆX,Y) = E[ˆX Y]−E[ˆX]E[Y]
= E(Y[ˆγ+ˆδZ])−E(Y)E(ˆγ+ˆδZ)
= ˆγE(Y) +ˆδE(YZ)−ˆγE(Y)−ˆδE(Y)E(Z)
= ˆδ[E(YZ)−E(Y)E(Z)]
Cov(ˆX,Y) = ˆδCov(Y,Z)
The 2SLS estimator replacesXwith the ﬁtted values ofX(i.e.,ˆX) from theﬁrst stage regression ofXontoZand all other covariates.
Cunningham Causal Inference

### Slide 489

Intuition of 2SLS
I’ve said that learning about instrumental variables through
the “intuition” of two-stage least squares is valuable, but
what do I mean exactly?
2SLS emphasizes the use of the ﬁtted values of the
endogenous regressor – what has that transformation done?
By using the ﬁtted values of the endogenous regressor from
the ﬁrst stage regression, our regression now uses only the
exogenous variation in the regressor due to the instrumental
variable itself
We recover exogenous variation in other words
. . . but think about it – that variation was there before, but
was just a subset of all the variation in the regressor
Instrumental variables therefore reduces the variation in the
data, but that variation which is left is exogenous

### Slide 490

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
Implementing 2SLS in STATA
In a sample of data, you could get the reduced form and ﬁrst stage coeﬃcients
manually by the following two regression commands in STATA:
. reg Y Z
. reg X Z
While it is always a good idea to run these two regressions, don’t compute yourIV estimate this way
Example: It is often the case that a pattern of missing data will diﬀer betweenY
andX; in such a case, the usual procedure of “casewise deletion” is to keep the
subsample with non-missing data onY, X, andZ.
But the reduced form and ﬁrst stage regressions would be estimated oﬀ of
diﬀerent sub-samples if you used the two step method above
The standard errors from the second stage regression are also wrong
Best practice is to use your built-in procedure (which also gives standard
errors):
. ivregress Y (X=Z)
Cunningham Causal Inference

### Slide 491

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
Implementing 2SLS in STATA
You can also estimate 2SLS using the auxiliary regression approach we just
covered:
. reg X Z
. predict Xhat
. reg Y Xhat
For the same reasons that you shouldn’t actually implement 2SLS manually
using the ratio of the reduced form and ﬁrst stage coeﬃcients, you shouldn’t
manually use the auxiliary regression approach because, again, the standard
errors are incorrect, and any complex missing patterns may leave you with
diﬀerent samples
This “two stage least squares” interpretation of IV – called aninterpretation,
because it is not the actual suggested procedure – is useful for understanding
what IV does, but stick withivregress
Cunningham Causal Inference

### Slide 492

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
Instrument for Education using Compulsory Schooling Laws
In practice, it is often diﬃcult to ﬁnd convincing instruments – usually because
potential instruments don’t satisfy the exclusion restriction
In the returns to education literature, Angrist and Krueger (1991) had a very
inﬂuential study where they used quarter of birth as an instrumental variable
for schooling
In the US, you could drop out of school once you turned 16
“School districts typically require a student to have turned age six by January 1
of the year in which he or she enters school” (Angrist and Krueger 1991, p.
980)
Children have diﬀerent ages when they start school, though, and this creates
diﬀerent lengths of schooling at the time they turn 16 (potential drop out age):
Instrument for Education using Compulsory Schooling Laws
In practice it is often di ¢cult to ﬁnd convincing instruments (in
particular because many potential IVs do not satisfy the exclusion
restriction).
In the returns to education literature Angrist and Krueger (1991) had
a very inﬂuential study where they used quarter of birth as an
instrumental variable for schooling.
In the US you could drop out of school once you turned 16.
Children have di §erent ages when they start school and thus di §erent
lengths of schooling at the time they turn 16 when they can
potentially drop out.
Waldinger (Warwick) 9/4 5
Cunningham Causal Inference

### Slide 493

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
First Stage
Men born earlier in the year have lower schooling. This indicates that there is a
ﬁrst stage.
First Stages
Men born earlier in the year have lower schooling. This indicates that
there is a ﬁrst stage.
Waldinger (Warwick) 10 / 45
Cunningham Causal Inference

### Slide 494

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
Reduced Form
Do diﬀerences in schooling due to diﬀerent quarter of birth translate into
diﬀerent earnings?
Reduced Form
Do di §erences in schooling due to di §erent quarter of birth translate
into di §erent earnings?
Waldinger (Warwick) 11 / 45
Cunningham Causal Inference

### Slide 495

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
Two Stage Least Squares model
The ﬁrst stage regression is:
Si =Xπ10+π11Zi +η1i
The reduced form regression is:
Yi =Xπ20+π21Zi +η2i
The covariate adjusted IV estimator is the sample analog of the ratio,π21
π11
Again, how was this estimator calculated?
1 Obtain the ﬁrst stage ﬁtted values:
ˆSi =Xˆπ10+ˆπ11Zi
whereˆπ1j forj = 1,2 are OLS estimates of the ﬁrst stage regression
2 Plug the ﬁrst stage ﬁtted values into the “second-stage equation” to then
estimate
Yi =αX+ˆSρ+ error
Cunningham Causal Inference

### Slide 496

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
Two Stage Least Squares
But as we note, they don’t actually manually do this – I remind you of this
because the 2SLS intuition is very useful. 2SLS only retains the variation inS
generated by the quasi-experimental variation, which we hope is exogenous
Angrist and Krueger use more than one instrumental variable to instrument for
schooling: they include a dummy for each quarter of birth. Their estimated
ﬁrst-stage regression is therefore:
Si =Xπ10+Z1iπ11+Z2iπ12+Z3iπ13+η1
The second stage is the same as before, but the ﬁtted values are from the new
ﬁrst stage
Cunningham Causal Inference

### Slide 497

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
First stage regression results
First Stage Regressions in Angrist & Krueger (1991)
Waldinger (Warwick) 14 / 45
Cunningham Causal Inference

### Slide 498

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
First stage regression results
Quarter of birth is a strong predictor of total years of education
First Stage Regressions in Angrist & Krueger (1991)
Quarter of birth is a strong predictor of total years of education.
Waldinger (Warwick) 15 / 45
Cunningham Causal Inference

### Slide 499

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
First stage regression results: Placebos
Cunningham Causal Inference

### Slide 500

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
IV Estimates Birth Cohorts 20-29, 1980 Census
IV Results
IV Estimates Birth Cohorts 20-29, 1980 Census
Waldinger (Warwick) 17 / 45
Cunningham Causal Inference

### Slide 501

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
IV Estimates - including some covariates
IV Results - Including Some Covariates
IV Estimates Birth Cohorts 20-29, 1980 Census
Waldinger (Warwick) 18 / 45
Cunningham Causal Inference

### Slide 502

Wald estimator
They also present an alternative to 2SLS called the Wald
estimator – both are versions of instrumental variables
Recall that 2SLS uses the predicted values from a ﬁrst stage
regression – but we showed that the 2SLS method was
equivalent to Cov(Y,Z)
Cov(X,Z)
The Wald estimator simply calculates the return to education
as the ratio of the diﬀerence in earnings by quarter of birth to
the diﬀerence in years of education by quarter of birth – it’s a
version of the above
Formally, IVWald = E(Y |Z=1)−E(Y |Z=0)
E(D|Z=1)−E(D|Z=0)

### Slide 503

[No extractable text in PDF text layer; page may contain an image or diagram.]

### Slide 504

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
IV Estimates - more covariates and interacting quarter of birth
They also include speciﬁcations where they use 30 (quarter of birth×year)dummy variables and 150 (quarter of birth×state) dummies as instrumentalvariables
What’s the intuition here? The eﬀect of quarter of birth may vary by birth year or
by state
It reduced the standard errors, but that comes at a cost of potentially having a
weak instruments problem
IV Results - Including More Covariates and Interacting
Quarter of Birth
They also include speciﬁcations where they use 30 (quarter of birth x
year) dummies and 150 (quarter of birth x state) dummies as IVs
(intuition: the e §ect of quarter of birth may vary by birth year or
state).
This reduces standard errors.
But also comes at the cost of potentially having a weak instruments
problem (see below).
Waldinger (Warwick) 19 / 45
Cunningham Causal Inference

### Slide 505

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
plain
Mechanism
In addition to log weekly wage, they examined the impact of
compulsory schooling on log annual salary and weeks worked
The main impact of compulsory schooling is on the log weekly
wage – not on weeks worked
Cunningham Causal Inference

### Slide 506

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
Weak Instruments
As we mentioned earlier, IV is consistent but biased
For a long time, researchers were not attentive to this subtle diﬀerence and
didn’t care much about the small sample bias of IV
But in the early 1990s, a number of papers highlighted that IV can beseverely
biased – in particular, when instruments have only a weak correlation with the
endogenous variable of interest and when many instruments are used to
instrument for one endogenous variable (i.e., there are many overidentifying
restrictions).
In the worst case, if the instruments are so weak that there is no ﬁrst stage,
then the 2SLS sampling distribution is centered on the probability limit of OLS
Cunningham Causal Inference

### Slide 507

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
Weak instruments and bias towards OLS
Let’s consider a model with a single endogenous regressor and a simple
constant treatment eﬀect
The causal model of interest is:
y =βx +ν
The matrix of instrumental variables is Z with the ﬁrst stage equation:
x =Z’π+η
If νi andηi are correlated, estimating the ﬁrst equation by OLS would lead to
biased results, wherein the OLS bias is:
E[βOLS−β] =Cov(ν,x)
Var(x)
If νi andηi are correlated the OLS bias is therefore:σνη
σ2x
Cunningham Causal Inference

### Slide 508

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
Weak instruments and bias towards OLS
It can be shown that the bias of 2SLS is approximately:
E[ˆβ2SLS−β] ≈ σνη
σ2η
1
F + 1
whereF is the population analogue of theF-statistic for the joint signiﬁcance
of the instruments in the ﬁrst stage regression. See Angrist and Pischke pp.
206-208 for a derivation.
If the ﬁrst stage is weak (i.e,F →0), then the bias of 2SLS approachesσνη
σ2η
This is the same as the OLS bias as forπ = 0 in the second equation on the
earlier slide (i.e., there is no ﬁrst stage relationship betweenZ andD) σ2x =σ2η
and therefore the OLS biasσνη
σ2x
becomesσνη
σ2x
.
But if the ﬁrst stage is very strong (F → ∞) then the IV bias goes to 0.
Cunningham Causal Inference

### Slide 509

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
Weak Instruments - Adding More Instruments
Adding more weak instruments will increase the bias of 2SLS
By adding further instruments without predictive power, the ﬁrst stageF-statistic
goes toward zero and the bias increases
If the model is “just identiﬁed” – mean the same number of instrumental
variables as there are endogenous covariates – weak instrument bias is less of a
problem
See Angrist and Pischke, p. 209 where they write that IV is “approximately
biased” – this is only true if the ﬁrst stage is not zero
Seehttp:
//econ.lse.ac.uk/staff/spischke/mhe/josh/solon_justid_April14.pdf
Bound, Jaeger and Baker (1995) highlighted this problem for the Angrist and
Krueger study. AK present ﬁndings from using diﬀerent sets of instruments
1 Quarter of birth dummies→3 instruments
2 Quarter of birth dummies + (quarter of birth)×(year of birth) + (quarter of
birth)×(state of birth)→180 instruments
Cunningham Causal Inference

### Slide 510

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
Adding instruments in Angrist and Krueger
Adding Instruments in Angrist & Krueger
Table from Bound, Jaeger, and Baker (1995) - 3 and 30 IVs
Adding more weak instruments reduced the ﬁrst stage F-statistic and
moves the coe ¢cient towards the OLS coe ¢cient.
Waldinger (Warwick) 41 / 45
Adding more weak instruments reduced the ﬁrst stageF-statistic and moves
the coeﬃcient towards the OLS coeﬃcient
Cunningham Causal Inference

### Slide 511

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
Adding instruments in Angrist and Krueger
Adding Instruments in Angrist & Krueger
Table from Bound, Jaeger, and Baker (1995) - 180 IVs
Adding more weak instruments reduced the ﬁrst stage F-statistic and
moves the coe ¢cient towards the OLS coe ¢cient.
Waldinger (Warwick) 42 / 45
Adding more weak instruments reduced the ﬁrst stageF-statistic and moves
the coeﬃcient towards the OLS coeﬃcient
Cunningham Causal Inference

### Slide 512

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
What can you do if you have weak instruments?
WIth weak instruments, you have the following options:
1 Use a just identiﬁed model with your strongest IV
2 Use a limited information maximum likelihood estimator (LIML). This is
approximately median unbiased for over identiﬁed constant eﬀects models. It
provides the same asymptotic distribution as 2SLS (under constant eﬀects) but
provides a ﬁnite-sample bias reduction. (LIML is programmed for STATA in the
ivregresscommand.)
3 Find stronger instruments.
Cunningham Causal Inference

### Slide 513

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
Practical Tips for IV Papers
1 Report the ﬁrst stage
Does it make sense?
Do the coeﬃcients have the right magnitude and sign?
2 Report theF-statistic on the excluded instrument(s).
Stock, Wright and Yogo (2002) suggest thatF-statistics>10 indicate that you
do not have a weak instrument problem – this is not a proof, but more like a rule
of thumb
If you have more than one endogenous regressor for which you want to
instrument, reporting the ﬁrst stageF-statistic is not enough (because 1
instrument could aﬀect both endogenous variables and the other could have no
eﬀect – the model would be under identiﬁed). In that case, you want to report
the Cragg-Donald EV statistic.
Cunningham Causal Inference

### Slide 514

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
Practical Tips for IV Papers
3 If you have many IVs, pick your best instrument and report the just identiﬁed
model (weak instrument problem is much less problematic)
4 Check over identiﬁed 2SLS models with LIML
5 Look at the reduced form
The reduced form is estimated with OLS and is therefore unbiased
If you can’t see the causal relationship of interest in the reduced form, it is
probably not there
Cunningham Causal Inference

### Slide 515

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
Angrist (1990) Veteran Draft Lottery
Angrist (1990) uses the Vietnam draft lottery as an instrumental variable for
military service
In the 1960s and 1970s, young American men were drafted for military service
to serve in Vietnam
Concerns about the fairness of the conscription policy led to the introduction of
a draftlotteryin 1970
From 1970 to 1972, random sequence numbers were randomly assigned to each
birth date in cohorts of 19-year-olds
Men with lottery numbers below a cutoﬀ were drafted; in other words
Higher numbers werelesslikely to be drafted;
Lower numbers weremorelikely to be drafted.
The draft did not perfectly determine military service:
Many draft-eligible men were exempt for health and other reasons
Exempt men would sometimes volunteer
Cunningham Causal Inference

### Slide 516

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
Summary of Findings on Vietnam Draft Lottery
1 First stage results : Having a low lottery number (i.e., being
eligible for the draft) increased veteran status by about 16
percentage points (the mean of veteran status was 27 percent)
2 Second stage results : Serving in the army lowers earnings by
between $2,050 and $2,741 per year
Cunningham Causal Inference

### Slide 517

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
IV with Heterogenous Treatment Eﬀects
Up to this point, we only considered models where the causal eﬀect was the
same for all individuals (i.e., homogenous treatment eﬀects whereY1i −Y0i =δ
for alli units)
Let’s now try to understand what instrumental variables estimation is
measuring if treatment eﬀects areheterogenous(i.e,Y1i −Y0i =δi which varies
across the population)
Why do we care?
1 We care about internal validity: Does the design successfully uncover causal
eﬀects for the population that we are studying?
2 We care about external validity: Does the study’s results inform us about
diﬀerent populations?
Cunningham Causal Inference

### Slide 518

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
IV with Heterogenous Treatment Eﬀects
Similar potential outcomes notation and terminology:
Causal chain isZi →Di →Yi
Yi(Di = 0,Zi = 1) is represented asYi(0,1)
“Potential treatment status” versus “observed treatment status
D1i =i’s treatment status whenZi = 1
D0i =i’s treatment status whenZi = 0
Observed treatment status switching equation:
Di = D0i + (D1i −D0i )Zi
= π0 +π1Zi +ζi
π0i = E[D0i ]
π1i = ( D1i −D0i ) is the heterogenous causal eﬀect of the IV onDi.
E[π1i] = The average causal eﬀect ofZi onDi
Cunningham Causal Inference

### Slide 519

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
Identifying assumptions under heterogenous treatment eﬀects
1 Stable Unit Treatment Value Assumption (SUTVA)
2 Random Assignment
3 Exclusion Restriction
4 Nonzero First Stage
5 Monotonicity
Cunningham Causal Inference

### Slide 520

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
Stable Unit Treatment Value Assumption (SUTVA)
Stable Unit Treatment Value Assumption (SUTVA)
If Zi =Z′i, thenDi(Z) =Di(Z′)
If Zi =Z′i andDi =D′i, thenYi(D,Z) =Yi(D’,Z’)
Interpretation Potential outcomes for each personi are unrelated to the treatment
status of other individuals.
Example Veteran status of person at risk of being drafted is not aﬀected by the
draft status of others at risk of being drafted.
Implication RewriteYi(D,Z) asYi(Di,Zi) andDi(Z) asDi(Zi).
Deﬁnition 1: Intention-to-treat eﬀects
Causal eﬀect ofZ onD is D1i −D0i
Causal eﬀect ofZ onY is Yi(D1i ,1)−Yi(D0i ,0)
Cunningham Causal Inference

### Slide 521

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
Independence assumption
Independence assumption (e.g., “as good as random assignment”)
{Yi(D1i ,1),Yi(D0i ,0),D1i ,D0i} ⊥ ⊥Zi
Interpretation The IV is independent of the vector of potential outcomes and
potential treatment assignments (i.e. “as good as randomly assigned”)
The independence assumption is suﬃcient for a causal interpretation of the reduced
form:
E[Yi|Zi = 1]−E[Yi|Zi = 0] = E[Yi(D1i ,1)|Zi = 1]−E[Yi(D0i ,0)|Zi = 0]
= E[Yi(D1i ,1)]−E[Yi(D0i ,0)]
Independence means that the ﬁrst stage measures the causal eﬀect ofZi onDi:
E[Di|Zi = 1]−E[Di|Zi = 0] = E[D1i |Zi = 1]−E[D0i |Zi = 0]
= E[D1i −D0i ]
Example Vietnam conscription for military service was based on randomly generated
draft lottery numbers. The assignment of draft lottery number was independent of
potential earnings or potential military service – as good as random.
Cunningham Causal Inference

### Slide 522

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
Exclusion Restriction
Exclusion Restriction
Y(D,Z)=Y(D,Z’)for allZ, Z’, and for allD
Interpretation Any eﬀect ofZ onY must be via the eﬀect ofZ onD. In other
words,Yi(Di,Zi) is a function ofD only. Or formally:
Yi(Di,0) =Yi(Di,1) forD= 0,1
Example In the Vietnam draft lottery example, an individual’s earnings potential as
a veteran or a non-veteran are assumed to be the same regardless of draft eligibility
status. The exclusion restriction would be violated if low lottery numbers may have
aﬀected schooling (e.g., to avoid the draft). If this was the case, the lottery number
would be correlated with earnings for at least two cases:
1 through its eﬀect on military service
2 through its eﬀect on educational attainment
Implication Random lottery numbers (independence) does not imply that the
exclusion restriction is satisﬁed
Cunningham Causal Inference

### Slide 523

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
Exclusion restriction
Use the exclusion restriction to deﬁne potential outcomes indexed solely against
treatment status:
Y1i = Yi(1,1) =Yi(1,0)
Y0i = Yi(0,1) =Yi(0,0)
Rewrite the switching equation:
Yi = Yi(0,Zi) + [Yi(1,Zi) −Yi(0,Zi)]Di
Yi = Y0i + [Y1i −Y0i ]Di
Random coeﬃcients notation for this is:
Yi =α0 +ρiDi
withα0 =E[Y0i ] andρi =Y1i −Y0i
Cunningham Causal Inference

### Slide 524

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
First stage
Nonzero Average Causal Eﬀect ofZ onD
E[D1i −D0i ] ̸= 0
InterpretationZ has to have some statistically signiﬁcant eﬀect on the average
probability of treatment
Example Having a low lottery number increases the average probability of service.
Cunningham Causal Inference

### Slide 525

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
Monotonicity
Monotonicity
Eitherπ1i ≥0 for alli orπ1i ≤0 for alli = 1, . . . ,N
Interpretation Recall thatπ+ 1i is the reduced form causal eﬀect of the
instrumental variable on an individuali’s treatment status. Monotonicity requires
that the instrumental variable (weakly) operate in the same direction on all
individual units. In other words, while the instrument may have no eﬀect on some
people, all those who are aﬀected are aﬀectedin the same direction(i.e., positively
or negatively, but not both).
Draft example While draft eligibility may have had no eﬀect on the probability of
military service for some, monotonicity means the draft lottery either shifted people
into service, or it shifted people out of service – but it did not do both.
Schooling example In the quarter of birth example for schooling, this assumption
may not be satisﬁed (see Barua and Lang 2009). Being born in the 4thquarter
(which typically increases schooling) may have reduced schooling for some because
their school enrollment was held back by their parents
Implication Without monotonicity, IV estimators are not guaranteed to estimate a
weighted average of the underlying causal eﬀects of the aﬀected group,Y1i −Y0i .
Cunningham Causal Inference

### Slide 526

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
Local average treatment eﬀect
If all 1-5 assumptions are satisﬁed, then IV estimates thelocal average treatment
eﬀect (LATE)of D onY:
δIV,LATE= Eﬀect of Z on Y
Eﬀect of Z on D
Instrumental variables (IV) estimand:
δIV,LATE= E[Yi(D1i ,1)−Yi(D0i ,0)]
E[D1i −D0i ]
=E[(Y1i −Y0i )|D1i −D0i = 1]
Cunningham Causal Inference

### Slide 527

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
Local Average Treatment Eﬀect
The LATE parameters is the average causal eﬀect ofD onY for those whose
treatment status was changed by the instrument,Z
Vietnam draft lottery example: IV estimates the average eﬀect of military
service on earnings for the subpopulation who enrolled in military service
because of the draft but would not have served otherwise.
In other words, LATE would not tell us what the causal eﬀect of military service
was for volunteers or those who were exempted from military service for medical
reasons
We have reviewed the properties of IV with heterogenous treatment eﬀects
using a very simple dummy endogenous variable, dummy IV, and no additional
controls example.
The intuition of LATE generalizes to most cases where we have continuous
endogenous variables and instruments, and additional control variables.
Cunningham Causal Inference

### Slide 528

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
More IV Jargon!
The LATE framework partitions any population with an instrument into
potentially 4 groups:
1 Compliers: The subpopulation withD1i = 1 andD0i = 0. Their treatment status
is aﬀected by the instrument in the “correct direction”.
2 Always takers: The subpopulation withD1i =D0i = 1. They always take the
treatment independently ofZ.
3 Never takers: The subpopulation withD1i =D0i = 0. They never take the
treatment independently ofZ.
4 Deﬁers: The subpopulation withD1i = 0 andD0i = 1. Their treatment status is
aﬀected by the instrument in the “wrong direction”.
(You’ll notice that increasingly our terms are borrowing from the medical
literature where the treatment is taking a pill. Same here.)
Cunningham Causal Inference

### Slide 529

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
Never-Takers
D1i −D0i = 0
Yi(0,1)−Yi(0,0) = 0
By Exclusion Restriction, causal eﬀect ofZ
onY is zero.
Deﬁer
D1i −D0i =−1
Yi(0,1)−Yi(1,0) =Yi(0)−Yi(1)
By Monotonicity, no one in this group
Complier
D1i −D0i = 1
Yi(1,1)−Yi(0,0) =Yi(1)−Yi(0)
Average Treatment Eﬀect among
Compliers
Always-taker
D1i −D0i = 0
Yi(1,1)−Yi(1,0) = 0
By Exclusion Restriction, causal eﬀect ofZ
onY is zero.
Cunningham Causal Inference

### Slide 530

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
Monotonicity Ensures that there are no deﬁers
Monotonicity ensures that there are no deﬁers
Why is it important to not have deﬁers?
If there were deﬁers, eﬀects on compliers could be (partly) canceled out by
opposite eﬀects on deﬁers
One could then observe a reduced form which is close to zero even though
treatment eﬀects are positive for everyone (but the compliers are pushed in one
direction by the instrument and the deﬁers in the other direction)
Cunningham Causal Inference

### Slide 531

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
What Does IV (Not) Estimate?
As outlined above, with all 5 assumptions satisﬁed, IV estimates the average
treatment eﬀect forcompliers
Contrast this with the traditional IV pedagogy with constant treatment eﬀects
(i.e.,δi =δ for alli units).
Question: What does IV estimate when treatment eﬀects are assumed to be
constant?
Without further assumptions (e.g., constant causal eﬀects), LATE is not
informative about eﬀects on never-takers or always-takers because the
instrument does not aﬀect their treatment status
So what? Well, it matters because in most applications, we would be mostly
interested in estimating the average treatment eﬀect on the whole population:
ATE=E[Y1i −Y0i ]
But that’s not possible usually with IV
Cunningham Causal Inference

### Slide 532

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
Sensitivity to assumptions: exclusion restriction
Example Someone at risk of draft (low lottery number) changes education plans to
retain draft deferments and avoid conscription.
Implication Increased bias to IV estimand through two channels:
Average direct eﬀect ofZ onY for compliers
Average direct eﬀect ofZ onY for noncompliers multiplied by odds of being a
non-complier
Severity Depends on:
Odds of noncompliance (smaller→less bias)
“Strength” of instrument (stronger→less bias)
Eﬀect of the alternative channel onY
Cunningham Causal Inference

### Slide 533

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
Sensitivity to assumptions: Monotonicity violations
Example Someone who would have volunteered for Army when not at risk of draft
(high lottery number) chooses to avoid military service when at risk of being drafted
(low lottery number)
Implication Bias to IV estimand (multiplication of 2 terms):
Proportion deﬁers relative to compliers
Diﬀerence in average causal eﬀects ofD onY for compliers and deﬁers
Severity Depends on:
Proportion of deﬁers (small→less bias)
“Strength” of instrument (stronger→less bias)
Variation in eﬀect ofD onY (less→less bias)
Cunningham Causal Inference

### Slide 534

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
Summarizing
The potential outcomes framework gives a more subtle interpretation of what
IV is measuring
In the constant coeﬃcients world (i.e., traditional pedagogy), IV measuresδ which
is “the” causal eﬀect ofDi onYi, and assumed to be the same for alli units
In the random coeﬃcients world, IV measures instead anaverageof heterogeneous
causal eﬀects across a particular population –E[δi] for some group ofi units
IV, therefore, measures thelocal average treatment eﬀector LATE parameter,
which is the average of causal eﬀects across the subpopulation ofcompliers, or
those units whose covariate of interest,Di, is inﬂuenced by the instrument.
Angrist and Evans (1996) example: not every woman whose ﬁrst two kids
share gender will go on to have a third kid.
Under heterogenous treatment eﬀects, Angrist and Evans (1996) identify the
causal eﬀect of the gender composition of the ﬁrst two kids on labor supply
Remark: That is not the same thing as identifying the causal eﬀect of children on
labor supply; the former is a LATE whereas the latter might be better described
as an ATE
Ex postthis is probably obvious, butex antethis was a real breakthrough (see
Angrist, Imbens and Rubin 1996; Imbens and Angrist 1994)
Cunningham Causal Inference

### Slide 535

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
Other potentially interesting treatment eﬀects
Another eﬀect which we may be potentially interested in estimating is the
familiar estimand, the average treatment eﬀect on the treatment group (ATT)
Remark: LATE isnotthe same as ATT, though.
E[Y1i −Y0i |Di = 1]  
ATT
= E[Y1i −Y0i |D0i = 1]  
Eﬀect on always takers
P[D0i = 1|Di = 1]
+E[Y1i −Y0i |D1i >D0i ]  
Eﬀect on compliers
P[D1i >D0i ,Zi = 1|Di = 1]
The average treatment eﬀect on the treated, ATT, is a weighted average of the
eﬀects on always-takers and compliers.
If there are no always takers we can, however, estimate ATT which is equal to
LATE in that case.
Cunningham Causal Inference

### Slide 536

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
Discussions and questions
When might wenotbe interested in the local average treatment eﬀect?
Romneycare in Massachusetts examples: If the compliance rate or treatment
eﬀects diﬀer in the community than during some quasi-experimental expansion,
are we more interested in LATE, ATT or ATE?
What might be other examples of this in economics? In education?
This has a similar ﬂavor to methodological questions regarding a study’s
“external” vs. “internal validity”
What do we make of the fact that LATE is deﬁned for anunobservable
sub-population(i.e., can’t label all units in the population as compliers or
noncompliers)?
What do we make of the fact that IV identiﬁcation is based on a set of
untestable assumptions?
For example: colonial settler mortality (Z) inﬂuences economic development (Y)
only throughZ’s association with human capital accumulation rather than
institutions,D (Acemoglu, Johnson and Robinson, 2001).
Cunningham Causal Inference

### Slide 537

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
IV in Randomized Trials
The use of IV methods may be helpful when evaluating a randomized trial
In many randomized trials, participation is nonetheless voluntary among those
randomly assigned to treatment
On the other hand, persons in the control group usually do not have access to
treatment
only those who are particularly likely to beneﬁt from treatment therefore will
probably take up treatment which almost always leads to positive selection bias
if you just compare means between treated and untreated individuals using OLS,
you will obtain biased treatment eﬀectseven for the randomized trialdue
non-compliance
Solution: instrument for treatment with whether you were oﬀered treatment
and estimate LATE
Cunningham Causal Inference

### Slide 538

Dean Eckles (Facebok), Rene Kizilcec and Eytan Bakshy
(2015). “Identifying Peer Eﬀects in Social Networks with
Peer Encouragement Designs”

### Slide 539

Peer encouragement designs
Dean Eckles, et al. (2015) randomly assign vertices in
Facebook network to encouragement to behavior of interest
and examine how this spills over to others
Indirectly aﬀect behaviors of existing peers
Eckles peer encouragement design allows them to estimate
the eﬀect of peer behaviors itself through instrumental
variables – not just the encouragement itself
An alternative view: assign peers to behaviors, see eﬀect it
has on the person.

### Slide 540

Eﬀects of receiving feedback
Motivation – when an individual shares content in social
media, what are the eﬀects of receiving additional feedback
like comments and “likes”?
Generating further conversation – ego replies to comments
In-kind peer eﬀects in giving feedback (reciprocity)
Creating and sharing more content in the future
How do these eﬀects vary by prior ego behaviors?
How can minor user interface changes aﬀect user experiences
and engagement? How should we make these tradeoﬀs?

### Slide 541

Goals
Goal 1: Estimate peer eﬀects
How does a marginal peer adopting aﬀect your adoption?
Ideal experiment: directly assign behaviors of existing peers
Goal 2: Estimate eﬀects of global treatment
What would happen if we gave everyone the treatment?
Average treatment eﬀect (ATE) of global treatment Z1 vs. Z0
δ(z1, z0) = 1
N
∑
i
E [Yi (Z = z1) − Yi (Z = z0)]
Ideal experiment: assign connected components to treatment
This is an active area of research (Eckles, Karrer, Ugander
2014; Ugander, et al 2013)

### Slide 542

What are “Encouragement designs”?
Randomly assign units to encouragement Z to a focal
behavior D
Examples: randomly encourage students to study (Powers and
Swinton 1984)
Assign to take a drug or not (but they may not take it)
Formal analysis using potential outcomes model
Total eﬀect of encouragement: intent-to-treat (ITT)
Yi (Zi = 1) − Yi (Zi = 0)
Eﬀect of behavior (i.e., eﬀect of D on Y )
Yi (Di = 1) − Yi (Di = 0)
Can we use Z to estimate the eﬀect of D on Y ?

### Slide 543

Encouragement designs
The best instrumental variables are randomly assigned
Find a variable Z that aﬀects D but is otherwise unrelated to
Y
Use this exogenous variation in D to estimate the eﬀect of D
on Y
e
YD
Z

### Slide 544

IV analysis
Number of encouraged peers is an instrumental variable for
peer eﬀects
Complete mediation (i.e., “exclusion restriction”) – Peer
encouragement only aﬀects ego behavior via peer behavior
Even if eﬀects are heterogenous, IV analysis of encouragement
designs identiﬁes the LATE for the complier subpopulation
Eckles, et al (2015) argue that this is likely an advantage over
other instruments that aren’t encouragements

### Slide 545

So what exactly is the instrument??
Facebook claim as fact: Pre-expanding vs. not pre-expanding
comment boxes ( Z ) increases feedback ( D)
Fact: Pre-expanding vs. not pre-expanding 
comment boxes modulates feedback 
Unexpanded comment box Expanded comment box 
Higher interaction rate "
Lower interaction rate

### Slide 546

Binary encouragement designs
Four types of people by potential outcomes
Compliers - treatment if encouraged, control if not
Always-takers - treatment whether encouraged or not
Never-takers - control whether encouraged or not
Deﬁers - control if encouraged, treatment if not encouraged
Not all of these may exist in a particular study
In a trial of a new drug or oﬀering (removing) a new
(existing) feature, there are neither always-takers nor deﬁers

### Slide 547

Binary encouragement designs
D=0 D=1
Z=0 Compliers and Never-takers Deﬁers and Always-takers
Z=1 Deﬁers and Never-takers Compliers and Always-takers

### Slide 548

Heterogenous treatment eﬀects
Monotonicity: With probability 1, D z
i ≥ D z′
i for all z ≥ z ′ and
all i
Then local average treatment eﬀect (LATE) is identiﬁed
In binary Z , D case, LATE is the average treatment eﬀect for
the population of compliers
We always have to ask ourselves: are we interested in the
LATE?

### Slide 549

Peer encouragement designs with a single behavior of
interest
Assign to encouragement to:
enroll in a retirement savings account (Duﬂo and Saez 2003)
post a thankful status update on Thanksgiving Day
Summarize peer assignments (e.g., number of peers assigned)
Summarize peer behaviors (e.g., number of adopter peers)
Compute average ego behaviors as a function of these

### Slide 550

Peer encouragement with dyads
1 Randomly encourage j or not
2 Observe j’s behavior (endogenous treatment for i)
3 Observe i’s behavior (outcome)
Z-i
W-i
Yi
Z-i
W-i
Yi
Z-i
W-i
Z-i
W-i
Always taker
Never taker
Never taker
Complier
Complier
Zj
Wj
Yi
Zj
Wj
Yi
Zj
Wj
Yi
Zj
Wj
Yi
Z-i
W-i
Yi
Peer encouragements for dyads
Non-compliance
Encourage all peers Encourage  no peers Encourage  some peers
Peer encouragement designs with dyads 
Binary encouragement, peer behavior, and ego outcome 
1. Randomly encourage j or not"
2. Observe j’s behavior (endogenous treatment for i)"
3. Observe i’s behavior (outcome)"
complier peer: 
Z-i
W-i
Yi
Z-i
W-i
Yi
Z-i
W-i
Z-i
W-i
Always taker
Never taker
Never taker
Complier
Complier
Zj
Wj
Yi
Zj
Wj
Yi
Zj
Wj
Yi
Zj
Wj
Yi
Z-i
W-i
Yi
Peer encouragements for dyads
Non-compliance
Encourage all peers Encourage  no peers Encourage  some peers
always-taker peer: 
All variables represented by circles may have other common causes not shown. Variables represented by squares are root nodes."

### Slide 551

Peer encouragement with multiple peers
Z-i
W-i
Yi
Z-i
W-i
Yi
Z-i
W-i
Z-i
W-i
Always taker
Never taker
Never taker
Complier
Complier
Zj
Wj
Yi
Zj
Wj
Yi
Zj
Wj
Yi
Zj
Wj
Yi
Z-i
W-i
Yi
Peer encouragements for dyads
Non-compliance
Encourage all peers Encourage  no peers Encourage  some peers
Peer encouragement designs 
With multiple peers 
All variables represented by circles may have other common causes not shown. Variables represented by squares are root nodes."

### Slide 552

Noncompliance with multiple peers
Z-i
W-i
Yi
Z-i
W-i
Yi
Z-i
W-i
Z-i
W-i
Always taker
Never taker
Never taker
Complier
Complier
Zj
Wj
Yi
Zj
Wj
Yi
Zj
Wj
Yi
Zj
Wj
Yi
Z-i
W-i
Yi
Peer encouragements for dyads
Non-compliance
Encourage all peers Encourage  no peers Encourage  some peers
Peer encouragement designs 
Noncompliance with multiple peers 
▪ An ego’s peer may be a mix of compliance types"

### Slide 553

Reduced form: Intent-to-treat (ITT)
One option is forego the IV analysis and instead estimate the
ITT
In other words, analyze ego outcome Y as a function of the
number of peers assigned Z
What else?
Compute probabilities of assignment, Pr(Z )
Use inverse probability weighting to estimate average outcomes

### Slide 554

Statistical inference for spillovers
Other issue is statistical inference.
Brieﬂy: statistical inference in a network application needs to
account for network autocorrelation in regressors (i.e.,
regressors are correlated within a network)
Number of peers treated has high auto-correlation
“Randomization inference” – increasingly used. Possible to do
exact testing for spillovers using new versions of conditional
randomization inference (Athey, Barrios, Eckles and Imbens
2015)

### Slide 555

Instrumental variables analysis
Ego’s outcome caused by own assignment D and peer
behaviors Y
The number of adopter peers is what matters – how many
peers will be “encouraged”
Eckles, et al (2015) suggest two-stage least squares or other
IV methods (e.g., LIML)
See Bramoulle et al (2009) “Identiﬁcation of Peer Eﬀects
through Social Networks”, Journal of Econometrics for exact
formulation of using “peers of peers IV”
Exclusion restriction requires that peer of peer behavior or
covariates only aﬀect ego behavior through peer behavior

### Slide 556

Results
Unfortunately, empirical results are currently not being
circulated from this study so this is from memory
First stage: the eﬀect of encouragement on feedback was very
large. Receiving expanding boxes resulted in peers leaving
more comments.
Eﬀects on behavior Y : the IV analysis showed that peers
inﬂuence our behavior – we are more likely to leave new
comments, reciprocal comments, likes, and even post new
updates
These eﬀects were very large – in both stages

### Slide 557

Lottery designs
Another design is to use a randomized lottery as an
instrumental variable for some treatment
Examples might be randomized lottery for attending charter
schools to study eﬀect of charter schools on educational
outcomes
We’ll discuss two papers from 2012 and 2014 evaluating a
lottery-based expansion of Medicaid health insurance on
Oregon on numerous health and ﬁnancial outcomes

### Slide 558

Overarching question
What are the eﬀects of expanding access to public health
insurance for low income adults?
Magnitudes, and even the signs, associated with that question
were uncertain
Limited existing evidence
Institute of Medicine review of evidence was suggestive, but a
lot of uncertainty
Observational studies are confounded by selection into health
insurance
Quasi-experimental work often focuses on elderly and children
Only one randomized experiment in a developed country: the
RAND health insurance experiment
1970s experiment on a general population
Randomized cost-sharing, not coverage itself

### Slide 559

The Oregon Health Insurance Experiment
Setting: Oregon Health Plan Standard
Oregon’s Medicaid expansion program for poor adults
Eligibility
Poor (<100% federal poverty line) adults 19-64
Not eligible for other programs
Uninsured > 6 months
Legal residents
Comprehensive coverage (no dental or vision)
Minimum cost-sharing
Similar to other states in payments, management
Closed to new enrollment in 2004

### Slide 560

The Oregon Medicaid Experiment
Lottery
Waiver to operate lottery
5-week sign-up period, heavy advertising (January to February
2008)
Low barriers to sign up, no eligibility pre-screening
Limited information on list
Randomly drew 30,000 out of 85,000 on list (March-October
2008)
Those selected given chance to apply
Treatment at household level
Had to return application within 45 days
60% applied; 50% of those deemed eligible → 10,000 enrollees

### Slide 561

Oregon Health Insurance Experiment
Evaluate eﬀects of Medicaid using lottery as randomized
controlled trial (RCT)
Intent-to-treat: Reduced form comparison of outcomes
between treatment group (lottery selected individuals) and
controls (not selected)
LATE: IV using lottery as instrument for insurance coverage
First stage: about a 25 percentage point increase in insurance
coverage
Archived analysis plan
Massive data collect eﬀort – primary and secondary
Similar to ACA expansion but limits to generalizability
Partial equilibrium vs. General equilibrium
Mandate and external validity
Oregon vs. other states
Short vs. Long-run

### Slide 562

Examine Broad Range of Outcomes
Costs: Health care utilization
Insurance increases resources (income) and lowers price,
increasing utilization
But improved eﬃciency (and improved health), decreasing
utilization (“oﬀset”)
Additional uncertainty when comparing Medicaid to no
insurance
Beneﬁts I: Financial risk exposure
Insurance supposed to smooth consumption
But for very low income, is most care de jure or de facto free?
Beneﬁts II: Health
Expected to improve (via increased quantity / quality of care)
But could discourage health investments (“ ex ante moral
hazard”)

### Slide 563

Data
Pre-randomization demographic information
From lottery sign-up
State administrative records on Medicaid enrollment
Primary measure of ﬁrst stage (i.e., insurance coverage)
Outcomes
Administrative data ( ∼16 months post-notiﬁcation): Hospital
discharge data, mortality, credit reports
Mail surveys ( ∼15 months): some questions ask 6-month
look-back; some ask current
In-person survey and measurements ( ∼25 months): Detailed
questionnaires, blood samples, blood pressure, body mass index

### Slide 564

Study Population
10

### Slide 565

Empirical Framework
Reduced form – estimates of the causal eﬀect of lottery
selection
Yihj = β0 + β1LOTTERYh + Xihβ2 + Vihβ3 + εihj
Validity of experimental design: randomization; balance on
treatment and control
Instrumental variables – eﬀect of insurance coverage
INSURANCEihj = δ0 + δ1LOTTERYih + Xihδ2 + Vihδ3 + µihj
yihj = π0 + π1INSURANCEih + Xihπ2 + Vihπ3 + vihj
Eﬀect of lottery on coverage: about 25 percentage points
Additional assumption for causality: primary pathway
Could aﬀect participation in other programs, but actually small
“Warm glow” of winning – especially early
Analysis plan, multiple inference adjustment

### Slide 566

Eﬀect of lottery on coverage (ﬁrst stage)
Effects of Lottery on Coverage (1stStage)
12
Control EstimatedControl EstimatedControl Estimated
mean FS mean FS mean FS
Ever on Medicaid 0.141 0.256 0.135 0.255 0.135 0.290
(0.004) (0.004) (0.007)
Ever on OHP Standard 0.027 0.264 0.028 0.264 0.026 0.302
(0.003) (0.004) (0.005)
# of Months on Medicaid 1.408 3.355 1.352 3.366 1.509 3.943
(0.045) (0.055) -0.09
On Medicaid, end of study period 0.106 0.148 0.101 0.151 0.105 0.189
(0.003) (0.004) (0.006)
0.325 0.179
(0.008)
0.128 -0.008
(0.005)
Currently on Medicaid (self report) 0.117 0.197
(0.006)
Currently on Medicaid 0.093 0.177
(0.006)
FullsampleC redit  subsampleS urvey respondents
Currently have any insurance (self report)
Currenty have private ins. (self report)

### Slide 567

Amy Finkelstein, et al. (2012). “The Oregon Health
Insurance Experiment: Evidence from the First Year”,
Quarterly Journal of Economics, vol. 127, issue 3, August.

### Slide 568

Eﬀects of Medicaid
Use primary and secondary data to gauge 1-year eﬀects
Mail surveys: 70,000 surveys at baseline, 12 months
Administrative data
Medicaid enrollment records
Statewide Hospital discharge data, 2007-2010
Credit report data, 2007-2010
Mortality data, 2007-2010

### Slide 569

Mail survey data
Fielding protocol
∼70,000 people, surveyed at baseline and 12 months later
Basic protocol: three-stage male survey protocol,
English/Spanish
Intensive protocol on a 30% subsample included additional
tracking, mailings, phone attempts (done to adjust for
non-response bias)
Response rate
Eﬀective response rate = 50%
Non-response bias aways possible, but response rate and
pre-randomization measures in administrative data were
balanced between treatment and control

### Slide 570

Administrative data
Medicaid records
Pre-randomization demographics from list
Enrollment records to assess “ﬁrst stage” (how many of the
selected got insurance coverage)
Hospital discharge data
Probabilistically matched to list, de-identiﬁed at Oregon
Health Plan
Includes dates and source of admissions, diagnoses,
procedures, length of stay, hospital identiﬁer
Includes years before and after randomization
Other data
Mortality data from Oregon death records
Credit report data, probabilistically matched, de-identiﬁed

### Slide 571

Sample
89,824 unique individuals on the waiting list
Sample exclusions (based on pre-randomization data only)
Ineligible for OHP Standard (out of state address, age, etc.)
Individuals with institutional addresses on list
Final sample: 79,922 individuals out of 66,385 households
29,834 treated individuals (surveyed 29,589)
40,088 control individuals (surveyed 28,816)

### Slide 572

Sample characteristics
Sample Characteristics
19

### Slide 573

Outcomes
Access and use of care
Is access to care improved? Do the insured use more care? Is
there a shift in the types of care being used?
Mail surveys and hospital discharge data
Financial strain
How much does insurance protect against ﬁnancial strain?
What are the out-of-pocket implications?
Mail surveys and credit reports
Health
What are the short-term impacts on self-reported physical and
mental health?
Mail surveys and vital statistics (mortality)

### Slide 574

Eﬀect of lottery on coverage (ﬁrst stage)
Gaining insurance resulted in better access to care and higher
satisfaction with care (conditional on actually getting care)
Results: Access & Use of Care
21
Gaining insurance resulted in better access to care and higher 
satisfaction with care (conditional on actually getting care).
CONTROL RF Model
(ITT)
IV Model
(LATE)
P-Value
Have a usual place of  care 49.9% +9.9% +33.9% .0001
Have a personal doctor 49.0% +8.1% +28.0% .0001
Got all needed health care 68.4% +6.9% +23.9% .0001
Got all needed prescriptions 76.5% +5.6% +19.5% .0001
Satisfied with quality of  care 70.8% +4.3% +14.2% .001
SOURCE: Survey data

### Slide 575

22

### Slide 576

Eﬀect of lottery on coverage (ﬁrst stage)
Gaining insurance resulted in increased probability of hospital
admissions, primarily driven by non-emergency department
admissions
Results: Access & Use of Care
23
Gaining insurance resulted in increased probability of  hospital 
admissions, primarily driven by non-ED admissions.
CONTROL RF Model
(ITT)
IV Model
(LATE)
P-Value
Any hospital admission 6.7% +.50% +2.1% .004
--Admits through ED 4.8% +.2% +.7% .265
--Admits NOT through 
ED
2.9% +.4% +1.6% .002
Overall, this represents a 30% higher probability of  admission, although admissions 
are still rare events.
SOURCE: Hospital Discharge Data
Overall, this represents a 30% higher probability of admission,
although admissions are still rare events

### Slide 577

Total Use By Condition
24

### Slide 578

Summary: Access and use of care
Overall, utilization and costs went up relative to controls
30% increase in probability of an inpatient admission
35% increase in probability of an outpatient visit
15% increase in probability of taking prescription medications
Total $777 increase in average spending (a 25% increase)
With this increased spending, those who gained insurance
were
35% more likely to get all needed care
25% more likely to get all needed medications
Far more likely to follow preventive care guidelines, such as
mammograms (60%) and PAP tests (45%)

### Slide 579

Results: Financial Strain
Gaining insurance resulted in a reduced probability of having
medical collections in credit reports, and in lower amounts
owed
Gaining insurance resulted in a reduced probability of  having medical 
collections in credit reports, and in lower amounts owed.
CONTROL RF Model
(ITT)
IV Model
(LATE)
P-Value
Had a bankruptcy 1.4% +0.2% +0.9% .358
Had a collection 50.0% -1.2% -4.8% .013
--Medical collections 28.1% -1.6% -6.4% .0001
--Non-medical collections 39.2% -0.5 -1.8% .455
$ owed medical collections $1,999 -$99 -$390 .025
SOURCE: Credit report data
Results: Financial Strain
26
Source: Credit report data

### Slide 580

27

### Slide 581

Summary: Financial Strain
Overall, reductions in collections on credit reports were
evident
25% decrease in probability of a medical collection
Those with a collection owed signiﬁcantly less
Household ﬁnancial strain related to medical costs was
mitigated
Substantial reduction across all ﬁnancial strain measures
Captures “informal channels” people use to make it work
Implications for both patients and providers
Only 2% of bills sent to collections are ever paid

### Slide 582

Results: Self-reported health
Self-reported measures showed signiﬁcant improvements one
year after randomization
Self-reported measures showed significant improvements one year 
after randomization
CONTROL RF Model
(ITT)
IV Model
(LATE)
P-Value
Health good, v good, excellent 54.8% +3.9% +13.3% .0001
Health stable or improving 71.4% +3.3% +11.3% .0001
Depression screen NEGATIVE 67.1% +2.3% +7.8% .003
CDC Healthy Days (physical) 21.86 +.381 +1.31 .018
CDC Healthy Days (mental) 18.73 +.603 +2.08 .003
SOURCE: Survey data
Results: Self-Reported Health
29
Source: Survey data

### Slide 583

Summary: Self-reported health
Overall, big improvements in self-reported physical and mental
health
25% increase in probability of good, very good or excellent
health
10% decrease in probability of screening for depression
Physical health measures open to several interpretations
Improvements consistent with ﬁndings of increased utilization,
better access, and improved quality
BUT in their baseline surveys, results appeared shortly after
coverage (∼2/3rds magnitude of full result)
May suggest increase in perception of well-being rather than
physical health
Biomarker data can shed light on this issue

### Slide 584

Discussion
At 1 year, found increases in utilization, reductions in ﬁnancial
strain, and improvements in self-reported health
Medicaid expansion had beneﬁts and costs – didn’t “pay for
itself”
Conﬁrmed biases inherent in observational studies – would
have estimated bigger increases in use and smaller
improvements in outcomes
Policy-makers may have diﬀerent views on value of diﬀerent
aspects of improved well-being
“I have an incredible amount of fear because I don’t know if
the cancer has spread or not.”
“A lot of times I wanted to rob a bank so I could pay for the
medicine I was just so scared . . . People with cancer either
have a good chance or no chance. In my case it’s hard to
recover from lung cancer but it’s possible. Insurance took so
long to kick in that I didn’t think I would get it. Now there is
a big bright light shining on me.” (Anecdotes)
Important to have broad evidence on multifaceted eﬀects of
Medicaid expansions

### Slide 585

Baicker, Katherine, et al. (2014). “The Oregon Experiment
– Eﬀects of Medicaid on Clinical Outcomes”, The New
England Journal of Medicine.

### Slide 586

In-person data collection
Questionnaire and health examination including
Survey questions
Anthropometric and blood pressure measurement
Dried blood spot collection
Catalog of all medications
Fielded between September 2009 and December 2010
Average response ∼25 months after lottery began
Limited to Portland area: 20,745 person sample
12,229 interviews for eﬀective response rate of 73%

### Slide 587

Analytic approach
Intent to treat eﬀect of lottery selection
Comparing all selected with all not selected
Random treatment assignment
No diﬀerential selection for outcome measurement
Local average treatment eﬀect on Medicaid coverage
Using lottery selection as an instrument for coverage
∼24 percentage point increase in Medicaid enrollment
No change in private insurance (no crowd-out)
No eﬀect of lottery except via Medicaid coverage
Statistical inference is the same for both

### Slide 588

Results
1 Health care use
2 Financial strain
3 Clinical health outcomes

### Slide 589

36

### Slide 590

37

### Slide 591

Health care use results
Increases in use in various settings
Increases in probability and number of outpatient visits
Increases in probability and number of prescription drugs
No discernible change in hospital or ED use (imprecise)
Increases in preventive care across range of services
Increases in perceived access and quality
Implied 35% increase in spending for insured

### Slide 592

Results
1 Health care use
2 Financial strain
3 Clinical health outcomes

### Slide 593

40

### Slide 594

Financial Hardship Results
Reduction in strain, out-of-pocket (OOP), money owed
Substantial reduction across measures
Elimination of catastrophic OOP health spending
Implications for distribution of burden/beneﬁts
Some borne by patients, some by providers
Non-ﬁnancial burden of medical expenses and debt

### Slide 595

Results
1 Health care use
2 Financial strain
3 Clinical health outcomes

### Slide 596

Focusing on speciﬁc conditions
Measured:
Blood pressure
Cholesterol levels
Glycated hemoglobin
Depression
Reasons for selecting these:
Reasonably prevalent conditions
Clinically eﬀective medications exist
Markers of longer term risk of cardiovascular disease
Can be measured by trained interviewers and lab tests
A limited window into health status

### Slide 597

44

### Slide 598

45

### Slide 599

46

### Slide 600

47

### Slide 601

Results on speciﬁc conditions
Large reductions in depression
Increases in diagnoses and medication
In-person estimate of −9 percentage points in being depressed
Glycated hemoglobin
Increases in diagnosis and medication
No signiﬁcant eﬀect on HbA1c; wide conﬁdence intervals
Blood pressure and cholesterol
No signiﬁcant eﬀects on diagnosis or medication
No signiﬁcant eﬀects on outcomes
Framingham risk score
No signiﬁcant eﬀect (in general or sub-poplulations)

### Slide 602

49

### Slide 603

Summary
One to two years after expanded access to Medicaid:
Increases in health care use and associated costs
Increases in compliance with recommended preventive care
Improvements in quality and access
Reductions in ﬁnancial strain
Improvements in self-reported health
Improvements in depression
No signiﬁcant change in speciﬁc physical measures
Sense of the relative magnitude of the eﬀects
Use and access, ﬁnancial beneﬁts, general health, depression
Physical measures of speciﬁc chronic conditions

### Slide 604

Extrapolation to Obamacare (ACA) Expansion
Context quite relevant for health care reform:
States can choose to cover a similar population in planned
2014 Medicaid expansions (up to 138% of federal poverty line)
But important caveats to bear in mind
Oregon and Portland vs. US generally
Voluntary enrollment vs. mandate
Partial vs. general equilibrium eﬀects
Short-run (1-2 years) vs. medium or long run

### Slide 605

Updating Priors based on Study’s Findings
“Medicaid is worthless or worse than no insurance”’
Studies found increases in utilization and perceived access and
quality
Reductions in ﬁnancial strain, improvement in self-reported
health
Improvement in depression
Can reject large declines in several physical measures
“Health insurance expansion saves money”
In short run, studies showed increases in utilization and cost
and no change in ED use
Increases in preventive care, improvements in self-reported
health, improvements in depression

### Slide 606

Conclusion
Eﬀects of expanding Medicaid likely to be manifold
Hard to establish with observational data and often misleading
Expanding Medicaid generates both costs and beneﬁts
Increased spending
Measurably improves some aspects of health but not others
Important caveats about generalizability
Weighing them depends on policy priorities
Further research on alternative policies needed
Many steps in pathway between insurance and outcome
Role for innovation in insurance coverage
Complements to health care (e.g., social determinants)

### Slide 607

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
Topics covered
1 Review ﬁxed eﬀects regression models
2 Diﬀerences-in-diﬀerences basics: Card and Krueger (1994)
3 Regression diﬀerences-in-diﬀerences
4 Synthetic control: Abadie, Diamond and Hainmueller (2010)
5 Combining diﬀerences-in-diﬀerences with IV: Waldinger (2010)
Cunningham Causal Inference

### Slide 608

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Natural Experiments
Instrumental variables
Panel data
Diﬀerences-in-diﬀerences
Synthetic Control
Panel Methods
Panel data: we observe the same units (individuals, ﬁrms,
countries, schools, etc.) over several time periods
Often our outcome variable depends on unobserved factors
which are also correlated with our explanatory variable of
interest
If these omitted variables are constant over time, we can use
panel data estimators to consistently estimate the eﬀect of
our explanatory variable
Main estimators for panel data:
Pooled OLS
Fixed eﬀects estimator
Random eﬀects estimator
Cunningham Causal Inference

### Slide 609

Panel setup
Let y and x ≡ (x1, x2, . . . ,xk) be observable random variables
and c be an unobservable random variable
We are interested in the partial eﬀects of variable xj in the
population regression function
E [y |x1, x2, . . . ,xk , c]
We observe a sample of i = 1, 2, . . . ,N cross-sectional units
for t = 1, 2, . . . ,T time periods (a balanced panel)
For each unit i, we denote the observable variables for all time
periods as {(yit , xit) : t = 1, 2, . . . ,T }
xit ≡ (xit1, xit2, . . . ,xitk) is a 1 × K vector
Typically assume that cross-sectional units are i.i.d. draws
from the population: {yi , xi , ci }N
i=1 ∼ i.i.d. (cross-sectional
independence)
yi ≡ (yi1, yi2, . . . ,yiT )′ and xi ≡ (xi1, xi2, . . . ,xiT )
Consider asymptotic properties with T ﬁxed and N → ∞

### Slide 610

Panel setup
Single unit:
yi =


yi1
...
yit
...
yiT


T ×1
Xi =


Xi,1,1 Xi,1,2 Xi,1,j . . . Xi,1,K
... ... ... ...
Xi,t,1 Xi,t,2 Xi,t,j . . . Xi,t,K
... ... ... ...
Xi,T,1 Xi,T,2 Xi,T,j . . . Xi,T,K


T ×K
Panel with all units:
y =


y1
...
yi
...
yN


NT ×1
X =


X1
...
Xi
...
XN


NT ×K

### Slide 611

Unobserved eﬀects model: Farm output
For a randomly drawn cross-sectional unit i, the model is
given by
yit = xitβ + ci + εit, t = 1, 2, . . . ,T
yit: output of farm i in year t
xit : 1 × K vector of variable inputs for farm i in year t, such as
labor, fertilizer, etc. plus an intercept
β : K × 1 vector of marginal eﬀects of variable inputs
ci : sum of all time-invariant inputs known to farmer i (but
unobserved for the researcher), e.g., soil quality, managerial
ability, etc.
often called the unobserved eﬀect, unobserved heterogeneity,
etc
εit: time-varying unobserved inputs, such as rainfall, unknown
to the farmer at the time the decision on the variable inputs xit
is made
often called the idiosyncratic error
What happens when we regress yit on xit?

### Slide 612

Pooled OLS
When we ignore the panel structure and regress yit on xit we
get
yit = xitβ + vit; t = 1, 2, . . . ,T
with composite error vit ≡ ci + εit
Main assumption to obtain consistent estimates for β is:
E [vit |xi1, xi2, . . . ,xiT ] = E [vit |xit] = 0 for t = 1, 2, . . . ,T
xit are strictly exogenous: the composite error vit in each time
period is uncorrelated with the past, current and future
regressors
But: labour input xit likely depends on soil quality ci and so
we have omitted variable bias and ˆβ is not consistent
No correlation between xit and vit implies no correlation
between unobserved eﬀect ci and xit for all t
Violations are common: whenever we omit a time-constant
variable that is correlated with the regressors (heterogeneity
bias)
Additional problem: vit are serially correlated for same i since
ci is present in each t and thus pooled OLS standard errors are
invalid

### Slide 613

Unobserved eﬀects model: program evaluation
Program evaluation model:
yit = progitβ + ci + εit; t = 1, 2, . . . ,T
yit: log wage of individual i in year t
progit: indicator coded 1 if individual i participants in training
program at t and 0 otherwise
β: eﬀect of program
ci : sum of all time-invariant unobserved characteristics that
aﬀect wages, such as ability, etc.
What happens when we regress yit on progit? ˆβ not
consistent since progit is likely correlated with ci (e.g., ability)
Always ask: is there a time-constant unobserved variable ( ci)
that is correlated with the regressors? If yes, then pooled OLS
is problematic

### Slide 614

Fixed eﬀect regression
Our unobserved eﬀects model is:
yit = xitβ + ci + εit; t = 1, 2, . . . ,T
If we have data on multiple time periods, we can think of ci
as ﬁxed eﬀects or “nuisance parameters” to be estimated
OLS estimation with ﬁxed eﬀects yields
( ˆβ, ˆc1, . . . ,ˆcN) = argmin
b,m1,...,mN
N∑
i=1
T∑
t=1
(yit − xitb − mi)2
this amounts to including N farm dummies in regression of yit
on xit

### Slide 615

Derivation: ﬁxed eﬀects regression
( ˆβ, ˆc1, . . . ,ˆcN) = argmin
b,m1,...,mN
N∑
i=1
T∑
t=1
(yit − xitb − mi)2
The ﬁrst-order conditions (FOC) for this minimization problem are:
N∑
i=1
T∑
t=1
x ′
it(yit − xit ˆβ − ˆci) = 0
and
T∑
t=1
(yit − xit ˆβ − ˆci) = 0
for i = 1, . . . ,N.

### Slide 616

Derivation: ﬁxed eﬀects regression
Therefore, fori = 1, . . . ,N,
ˆci = 1
T
T∑
t=1
(yit −xitˆβ) = ¯yi −¯xi ˆβ,
where
¯xi ≡ 1
T
T∑
t=1
xit; ¯yi ≡ 1
T
T∑
t=1
yit
Plug this result into the ﬁrst FOC to obtain:
ˆβ =
( N∑
i=1
T∑
t=1
(xit −¯xi)′(xit −¯xi)
)−1( N∑
i=1
T∑
t=1
(xit −¯xi)′(yit −¯y)
)
ˆβ =
( N∑
i=1
T∑
t=1
¨x′it¨xit
)−1( N∑
i=1
T∑
t=1
¨x′it¨xit
)
with time-demeaned variables ¨xit ≡xit −¯x, ¨yit ≡yit −¯yi

### Slide 617

Fixed eﬀects regression
Running a regression with the time-demeaned variables
¨yit ≡ yit − ¯yi and ¨xit ≡ xit − ¯x is numerically equivalent to a
regression of yit on xit and unit speciﬁc dummy variables.
Even better, the regression with the time demeaned variables is
consistent for β even when Cov[xit, ci] ̸= 0 because
time-demeaning eliminates the unobserved eﬀects
yit = xitβ + ci + εit
¯yi = ¯ xi β + ci + ¯εi
(yit − ¯yi) = ( xit − ¯x)β + (ci − ci) + (εit − ¯εi)
¨yit = ¨ xitβ + ¨εit

### Slide 618

Fixed eﬀects regression: main results
Identiﬁcation assumptions:
1 E [εit |xi1, x + i2, . . . ,xiT , ci ] = 0; t = 1, 2, . . . ,T
regressors are strictly exogenous conditional on the unobserved
eﬀect
allows xit to be arbitrarily related to ci
2 rank
( ∑T
t=1 E [¨x′
it ¨xit]
)
= K
regressors vary over time for at least some i and not collinear
Fixed eﬀects estimator
1 Demean and regress ¨yit on ¨xit (need to correct degrees of
freedom)
2 Regress yit on xit and unit dummies (dummy variable
regression)
3 Regress yit on xit with canned ﬁxed eﬀects routine
STATA:xtreg y x, fe i(PanelID)
Properties (under assumptions 1-2):
ˆβFE is consistent: plim
N→∞
ˆβFE ,N = β
ˆβFE is unbiased conditional on X

### Slide 619

Fixed eﬀects regression: main issues
Inference:
Standard errors have to be “clustered” by panel unit (e.g.,
farm) to allow correlation in the εit’s for the same i.
STATA:xtreg , fe i(PanelID) cluster( PanelID )
Yields valid inference as long as number of clusters is
reasonably large
Typically we care about β, but unit ﬁxed eﬀects ci could be of
interest
ˆci from dummy variable regression is unbiased but not
consistent for ci (based on ﬁxed T and N → ∞)
xtreg , fe routine demeans the data before running the
regression and therefore does not estimate ˆci
intercept shows average ˆci across units
we can recover ˆci using ˆci = ¯yi− ¯xi ˆβ
predict c_i, u

### Slide 620

Example: Direct Democracy and Naturalizations
Do minorities fare worse under direct democracy than under
representative democracy?
Hainmueller and Hangartner (2012) examine data on
naturalization requests of immigrants in Switzerland, where
municipalities vote on naturalization applications in:
referendums (direct democracy)
elected municipality councils (representative democracy)
Annual panel data from 1,400 municipalities for the
1991-2009 period
yit : naturalization rate = no.naturalizationsit
eligibleforeignpopulationi,t−1
xit : 1 if municipality used representative democracy, 0 if
municipality used direct democracy in year t

### Slide 621

Naturalization Panel Data
Fixed E ↵ects Regression
Naturalization Panel Data
repdem          float  %9.0g                  1 representative democracy, 0 direct 
nat_rate        float  %9.0g                  naturalization rate (percent)
year            float  %ty                    year
muni_name       str43  %43s                   municipality name
muniID          float  %8.0g                  municipality code
                                                                                   
variable name   type   format      label      variable label
              storage  display     value
. des muniID muni_name year nat_rate repdem 
J. Hainmueller (MIT) 20 / 64

### Slide 622

Panel Data Long Format
Fixed E ↵ects Regression
Panel Data Long Format
                                                           
 40.        3       Bonstetten   1992   .8403362        0  
 39.        3       Bonstetten   1991   .8333334        0  
 38.        2   Affoltern A.A.   2009   6.119704        1  
 37.        2   Affoltern A.A.   2008   8.977719        1  
 36.        2   Affoltern A.A.   2007   7.067371        1  
                                                           
 35.        2   Affoltern A.A.   2006   8.115358        1  
 34.        2   Affoltern A.A.   2005   4.387827        0  
 33.        2   Affoltern A.A.   2004   5.621302        0  
 32.        2   Affoltern A.A.   2003   4.844814        0  
 31.        2   Affoltern A.A.   2002   4.638365        0  
                                                           
       muniID        muni_name   year   nat_rate   repdem  
                                                           
. list muniID muni_name year nat_rate repdem in 31/40
J. Hainmueller (MIT) 21 / 64

### Slide 623

Pooled OLS
Fixed E ↵ects Regression
Pooled OLS
                                                                              
       _cons     2.222683     .10088    22.03   0.000     2.023976    2.421389
      repdem     2.503318   .2195202    11.40   0.000     2.070921    2.935714
                                                                              
    nat_rate        Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
                             Robust
                                                                              
                               (Std. Err. adjusted for 245 clusters in muniID)
                                                       Root MSE      =    3.98
                                                       R-squared     =  0.0748
                                                       Prob > F      =  0.0000
                                                       F(  1,   244) =  130.04
Linear regression                                      Number of obs =    4655
. reg nat_rate repdem , cl(muniID)
. ** pooled ols
J. Hainmueller (MIT) 22 / 64

### Slide 624

Decompose within and between variation
Fixed E ↵ects Regression
Decompose Within and Between Variation
         within                3.807039  -3.711323   24.80134       T =      19
         between               1.622939          0   7.567746       n =     245
nat_rate overall    2.938992   4.137305          0   24.13793       N =    4655
                                                                               
Variable                Mean   Std. Dev.       Min        Max      Observations
. xtsum  nat_rate
                delta:  1 year
        time variable:  year, 1991 to 2009
       panel variable:  muniID (strongly balanced)
. tsset  muniID year ,  yearly
J. Hainmueller (MIT) 23 / 64

### Slide 625

Time-demeaning for ﬁxed eﬀects: yit → ¨yit
Fixed E ↵ects Regression
Time-Demeaning for Fixed E ↵ects: yit ! ¨yit
 29.        2   Affoltern A.A.   2000   2.195946         3.595932     -1.399986  
 28.        2   Affoltern A.A.   1999   2.402135         3.595932     -1.193796  
 27.        2   Affoltern A.A.   1998   2.054155         3.595932     -1.541776  
 26.        2   Affoltern A.A.   1997   1.862745         3.595932     -1.733186  
 25.        2   Affoltern A.A.   1996     1.7769         3.595932     -1.819031  
                                                                                 
 24.        2   Affoltern A.A.   1995   2.002002         3.595932      -1.59393  
 23.        2   Affoltern A.A.   1994   .8342023         3.595932     -2.761729  
 22.        2   Affoltern A.A.   1993    1.04712         3.595932     -2.548811  
 21.        2   Affoltern A.A.   1992   .9473684         3.595932     -2.648563  
 20.        2   Affoltern A.A.   1991   .2173913         3.595932      -3.37854  
                                                                                 
       muniID        muni_name   year   nat_rate   means_nat_rate   dm_nat_rate  
                                                                                 
. list muniID muni_name year nat_rate means_nat_rate dm_nat_rate in 20/40 ,ab(20)
. gen  dm_nat_rate = nat_rate - means_nat_rate
. * compute deviations from means
. egen means_nat_rate = mean(nat_rate) , by(muniID)
. * get municipality means  
J. Hainmueller (MIT) 24 / 64

### Slide 626

Fixed eﬀects regression with demeaned data
Fixed E ↵ects Regression
Fixed E ↵ects Regression with Demeaned Data
                                                                              
       _cons     6.65e-10   5.81e-09     0.11   0.909    -1.08e-08    1.21e-08
   dm_repdem       3.0228   .1856244    16.28   0.000     2.657169    3.388431
                                                                              
 dm_nat_rate        Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
                             Robust
                                                                              
                               (Std. Err. adjusted for 245 clusters in muniID)
                                                       Root MSE      =  3.6017
                                                       R-squared     =  0.1052
                                                       Prob > F      =  0.0000
                                                       F(  1,   244) =  265.18
Linear regression                                      Number of obs =    4655
. reg dm_nat_rate dm_repdem , cl(muniID)
. * regression with demeaned data
. 
. gen  dm_repdem   = repdem   - means_repdem 
. egen means_repdem   = mean(repdem) , by(muniID)
. * demean direct demoracy dummy
J. Hainmueller (MIT) 25 / 64

### Slide 627

Fixed eﬀects regression with canned routine
Fixed E ↵ects Regression
Fixed E ↵ects Regression with Canned Routine
                                                                              
         rho    .17650677   (fraction of variance due to u_i)
     sigma_e      3.69998
     sigma_u    1.7129711
                                                                              
       _cons     2.074036   .0531153    39.05   0.000     1.969413    2.178659
      repdem       3.0228   .1856244    16.28   0.000     2.657169    3.388431
                                                                              
    nat_rate        Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
                             Robust
                                                                              
                               (Std. Err. adjusted for 245 clusters in muniID)
corr(u_i, Xb)  = -0.1373                        Prob > F           =    0.0000
                                                F(1,244)           =    265.18
       overall = 0.0748                                        max =        19
       between = 0.0005                                        avg =      19.0
R-sq:  within  = 0.1052                         Obs per group: min =        19
Group variable: muniID                          Number of groups   =       245
Fixed-effects (within) regression               Number of obs      =      4655
. xtreg nat_rate repdem , fe cl(muniID) i(muniID)
J. Hainmueller (MIT) 26 / 64

### Slide 628

Fixed eﬀects regression with dummies
Fixed E ↵ects Regression
Fixed E ↵ects Regression with Dummies
         13      .3265469   5.17e-14  6.3e+12   0.000     .3265469    .3265469
         10      1.271783   5.17e-14  2.5e+13   0.000     1.271783    1.271783
          9      1.284652   5.17e-14  2.5e+13   0.000     1.284652    1.284652
          3      1.292252   5.17e-14  2.5e+13   0.000     1.292252    1.292252
          2      1.367365   5.17e-14  2.6e+13   0.000     1.367365    1.367365
      muniID  
              
      repdem       3.0228   .1906916    15.85   0.000     2.647188    3.398412
                                                                              
    nat_rate        Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
                             Robust
                                                                              
                               (Std. Err. adjusted for 245 clusters in muniID)
                                                       Root MSE      =     3.7
                                                       R-squared     =  0.2423
                                                       Prob > F      =       .
                                                       F(  0,   244) =       .
Linear regression                                      Number of obs =    4655
. reg nat_rate repdem i.muniID, cl(muniID) 
J. Hainmueller (MIT) 27 / 64

### Slide 629

Applying ﬁxed eﬀects
We can use ﬁxed eﬀects for other data structures to restrict
comparisons to within unit variation
Matched pairs
Twin ﬁxed eﬀects to control for unobserved eﬀects of family
background
Cluster ﬁxed eﬀects in hierarchical data
School ﬁxed eﬀects to control for unobserved eﬀects of school

### Slide 630

Problems that even ﬁxed eﬀects do not solve
yit =xitβ+ci +εit; t = 1,2, . . . ,T
Whereyit is murder rate andxit is police spending per capita
What happens when we regressy onx and city ﬁxed eﬀects?
ˆβFE inconsistent unless strict exogeneity conditional onci holds
E[εit|xi1,xi2,..., xiT,ci] = 0;t = 1,2,..., T
impliesεit uncorrelated with past, current and future regressors
Most common violations
1 Time-varying omitted variables
Economic boom leads to more police spending and less murders
Can include time-varying controls, but avoid post-treatment bias (i.e., collider)
2 Simultaneity
if city adjusts police based on past murder rate, then spendingt+1 is correlated
withεt (since higherεt leads to higher murder rate att)
strictly exogenousx cannot react to what happens toy in the past or the future!
Fixed eﬀects do not obviate need for good research design!

### Slide 631

Random Eﬀects
Reconsider our unobserved eﬀects model:
yit = xitβ + ci + εit, t = 1, 2, . . . ,T
Cannot use the ﬁxed eﬀects regression to estimate eﬀects of
time-constant regressors in xit (eg., soil quality, farm location,
etc.)
Since ﬁxed eﬀect estimator allows ci to be correlated with xit,
we cannot distinguish the eﬀects of time-invariant regressors
from the time-invariant unobserved eﬀect ci
Need orthogonality assumption: Cov[xit , ci ] = 0;
t = 1, . . . ,T
Strong assumption: Unobserved eﬀects ci are uncorrelated
with each explanatory variable in xit in each time period
For example if we include soil quality in xit we have to assume
it is uncorrelated with all other time-invariant inputs

### Slide 632

Random eﬀects assumptions
yit = xitβ + ci + εit; t = 1, . . . ,T
1 E [εit|xi , ci] = 0; t = 1, 2, . . . ,T : explanatory variables are
strictly exogenous conditional on the unobserved eﬀect
2 E [ci |xi] = E [ci] = 0: unobserved eﬀects ci are uncorrelated
with regressors
3 rank E [X ′
i ΩXi] = K : no collinearity among regressors
Ω = E [vi v′
i ] : the variance matrix of the composite error
vit = ci + εit
4 We typically also assume that Ω takes a special form:
E [εi ε′
i |xi ] = σ2
εIT : idiosyncratic errors are homoskedastic for
all t and serially uncorrelated
E [c2
i |xi ] = σ2
c : unobserved eﬀect ci is homoscedastic

### Slide 633

Random eﬀects assumptions
yit =xitβ+ci +εit; t = 1, . . . ,T
1 E[εit|xi,ci] = 0;t = 1,2, . . . ,T: explanatory variables are strictly exogenous
conditional on the unobserved eﬀect
2 E[ci|xi] =E[ci] = 0: unobserved eﬀectsci are uncorrelated with regressors
3 rankE[X′i ΩXi] =K: no collinearity among regressors
Ω =E[viv′i ] : the variance matrix of the composite errorvit =ci +εit
4 We typically also assume that Ω takes a special form:
E[εiε′i|xi] =σ2εIT : idiosyncratic errors are homoskedastic for allt and serially
uncorrelated
E[c2i |xi] =σ2c: unobserved eﬀectci is homoscedastic
Assumption 4 implies Ω =E[viv′i |xi] =


σ2c +σ2ε σ2c . . . σ 2c
σ2c σ2c +σ2ε . . . ...
... ... σ2c
σ2c σ2c +σ2ε


T×T

### Slide 634

Random eﬀects assumptions
yit =xitβ+ci +εit; t = 1, . . . ,T
1 E[εit|xi,ci] = 0;t = 1,2, . . . ,T: explanatory variables are strictly exogenous
conditional on the unobserved eﬀect
2 E[ci|xi] =E[ci] = 0: unobserved eﬀectsci are uncorrelated with regressors
3 rankE[X′i ΩXi] =K: no collinearity among regressors
Ω =E[viv′i ] : the variance matrix of the composite errorvit =ci +εit
4 We typically also assume that Ω takes a special form:
E[εiε′i|xi] =σ2εIT : idiosyncratic errors are homoskedastic for allt and serially
uncorrelated
E[c2i |xi] =σ2c: unobserved eﬀectci is homoscedastic
Given assumptions 1-3, pooled OLS is consistent, since composite errorvit is
uncorrelated withxit for allt
However, pooled OLS ignores the serial correlation invit

### Slide 635

Random eﬀects assumptions
yit =xitβ+ci +εit; t = 1, . . . ,T
1 E[εit|xi,ci] = 0;t = 1,2, . . . ,T: explanatory variables are strictly exogenous
conditional on the unobserved eﬀect
2 E[ci|xi] =E[ci] = 0: unobserved eﬀectsci are uncorrelated with regressors
3 rankE[X′i ΩXi] =K: no collinearity among regressors
Ω =E[viv′i ] : the variance matrix of the composite errorvit =ci +εit
4 We typically also assume that Ω takes a special form:
E[εiε′i|xi] =σ2εIT : idiosyncratic errors are homoskedastic for allt and serially
uncorrelated
E[c2i |xi] =σ2c: unobserved eﬀectci is homoscedastic
Random eﬀects estimatorˆβRE exploits this serial correlation in a generalized
least squares (GLS) framework
ˆβRE is consistent under assumption 1-3:plim
N→∞
ˆβRE,N =β
ˆβRE is asymptotically eﬃcient given assumption 4 (in the class of estimators
consistent underE[vi|xi] = 0)

### Slide 636

Random eﬀects estimator
Consider the transformation parameter
λ = 1 −
( σ2
ε
σ2ε + T σ2c
)1
2
with 0 ≤ λ ≤ 1
σ2
ε = Var[εit]: variance of idiosyncratic error
σ2
c = Var(ci ): Variance of unobserved eﬀect
ˆβRE is equivalent to pooled OLS on:
yit − ¯yi = ( xit − λ¯xi)β + (vit − λ¯vi), ∀i, t
˜yit = ˜ xitβ + ˜vit
As λ → 1, ˆβRE → ˆβFE
As λ → 0, ˆβRE → ˆβPooled OLS
λ→ 1 as T→∞ or if variance of ci is large relative to
variance of εit
λ can be estimated from data ˆλ = 1 − (ˆσ2
ε /(ˆσ2
ε + T ˆε2
c))
1
2
Usually wise to cluster the standard errors since assumption 4
is strong

### Slide 637

Random eﬀects regression
Random E ↵ects
Random E ↵ects Regression
                                                                              
         rho     .1231606   (fraction of variance due to u_i)
     sigma_e      3.69998
     sigma_u    1.3866768
                                                                              
       _cons     2.120793   .0972959    21.80   0.000     1.930096    2.311489
      repdem     2.859397   .1893742    15.10   0.000      2.48823    3.230564
                                                                              
    nat_rate        Coef.   Std. Err.      z    P>|z|     [95% Conf. Interval]
                             Robust
                                                                              
                               (Std. Err. adjusted for 245 clusters in muniID)
corr(u_i, X)   = 0 (assumed)                    Prob > chi2        =    0.0000
                                                Wald chi2(1)       =    227.99
       overall = 0.0748                                        max =        19
       between = 0.0005                                        avg =      19.0
R-sq:  within  = 0.1052                         Obs per group: min =        19
Group variable: muniID                          Number of groups   =       245
Random-effects GLS regression                   Number of obs      =      4655
. xtreg nat_rate repdem , re cl(muniID) i(muniID)
J. Hainmueller (MIT) 34 / 64

### Slide 638

Summary: Fixed eﬀects, random eﬀects, Pooled OLS
Main assumptions
1 Regressors are strictly exogenous conditional on the
time-invariant unobserved eﬀects
2 Regressors are uncorrelated with the time-invariant unobserved
eﬀects
Results
Fixed eﬀects estimator is consistent given assumption 1, but
rules out time-invariant regressors
Random eﬀects estimators and pooled OLS are consistent
under assumptions 1-2, and allow for time-invariant regressors
Given homoskedasticity assumptions (random eﬀects
assumption 4), the random eﬀects estimator is asymptotically
eﬃcient
Assumption 2 is strong so ﬁxed eﬀects are typically more
credible
Often the main reason for using panel data is to rule out all
time-invariant unobserved confounders!

### Slide 639

Hausman test
ˆβRE ˆβFE
H0 : Cov[xit, ci] = 0 Consistent and eﬃcient Consistent
H1 : Cov[xit, ci] ̸= 0 Inconsistent Consistent
Then,
Under H0, ˆβRE − ˆβFE should be close to zero
Under H1, ˆβRE − ˆβFE should be diﬀerent from zero
It can be shown that in large samples, under H0, the test
statistic
( ˆβFE − ˆβRE )′(ˆVar[βFE ] − ˆVar[βRE ])−1( ˆβFE − ˆβRE ) d→ χ2
k
where k is the number of time-varying regressors.
We may reject the null hypothesis of “random eﬀects” and
stick with the less eﬃcient, but consistent ﬁxed eﬀects
speciﬁcation

### Slide 640

Random eﬀects regression
Random E ↵ects
Hausman Test
                Prob>chi2 =      0.0000
                          =       28.79
                  chi2(1) = (b-B)'[(V_b-V_B)^(-1)](b-B)
    Test:  Ho:  difference in coefficients not systematic
            B = inconsistent under Ha, efficient under Ho; obtained from xtreg
                           b = consistent under Ho and Ha; obtained from xtreg
                                                                              
      repdem        3.0228     2.859397        .1634027        .0304517
                                                                              
                     FE           RE         Difference          S.E.
                    (b)          (B)            (b-B)     sqrt(diag(V_b-V_B))
                      Coefficients     
. hausman FE RE 
. estimates store RE 
. quietly: xtreg nat_rate repdem , re i(muniID)
. 
. estimates store FE 
. quietly: xtreg nat_rate repdem , fe i(muniID)
. ** hausman test
J. Hainmueller (MIT) 37 / 64

### Slide 641

Hausman test
Hausman test does not test if the ﬁxed eﬀect model is correct;
the test assumes that the ﬁxed eﬀects estimator is consistent!
Conventional Hausman test assumes homoskedastic model
and does not allow for clustering
There are Haumsman like tests that allow for clustered
standard errors
Random E ↵ects
Hausman Test
Hausman test does not test if the ﬁxed e ↵ects model is correct, the test
assumes that the ﬁxed e ↵ects estimator is consistent!
Conventional Hausman test assumes homoscedastic model and does not
allow for clustering
There are Hausman like tests that allow for clustered standard errors
Sargan-Hansen statistic  26.560  Chi-sq(1)    P-value = 0.0000
Cross-section time-series model: xtreg re  robust cluster(muniID)
Test of overidentifying restrictions: fixed vs random effects
. xtoverid 
. quietly: xtreg nat_rate repdem , re i(muniID) cl(muniID)
. * hausman test with clustering
J. Hainmueller (MIT) 38 / 64

### Slide 642

Adding Time Eﬀects
Reconsider our unobserved eﬀects model:
yit = xitβ + ci + εit, t = 1, 2, . . . ,T
Fixed eﬀects assumption: E [εit |xi , ci ] = 0; t = 1, 2, . . . ,T :
regressors are strictly exogenous conditional on the unobserved
eﬀect
Typical violation: Common shocks that aﬀect all units in the
same way and are correlated with xit
Trends in farming technology or climate aﬀect productivity
Trends in immigration inﬂows aﬀect naturalization rates
We can allow for such common shocks by including time
eﬀects into the model

### Slide 643

Random eﬀects regression
Modeling Time
Naturalization Rate Over Time
0246naturalization rate (percent)19901995200020052010yearnaturalization rate (percent)lb/ubxtgraph nat rate
J. Hainmueller (MIT) 41 / 64
xtgraph nat_rate

### Slide 644

Random eﬀects regression
Modeling Time
Representative Democracy Over Time
0.2.4.6.81representative democracy (1/0)19901995200020052010year representative democracylb/ubxtgraph repdem
J. Hainmueller (MIT) 42 / 64
xtgraph repdem

### Slide 645

Fixed eﬀects: adding time eﬀects
Linear time trend:
yit = xitβ + ci + t + εit; t = 1, 2, . . . ,T
Linear time trend common to all units
Time ﬁxed eﬀects:
yit = xitβ + ci + tt + εit; t = 1, 2, . . . ,T
Common shock in each time period
Generalized diﬀerence-in-diﬀerence model
Unit speciﬁc linear time trends:
yit = xitβ + ci + gi · t + tt + εit; t = 1, 2, . . . ,T
Linear time trends that vary by unit

### Slide 646

Modeling time eﬀects
Modeling Time
Modeling Time E ↵ects
J. Hainmueller (MIT) 44 / 64

### Slide 647

Fixed eﬀects: adding time eﬀects
Modeling Time
Fixed E ↵ects Regression: Adding Time E ↵ects
 30.        2   Affoltern A.A.   2001     11  
                                              
 29.        2   Affoltern A.A.   2000     10  
 28.        2   Affoltern A.A.   1999      9  
 27.        2   Affoltern A.A.   1998      8  
 26.        2   Affoltern A.A.   1997      7  
 25.        2   Affoltern A.A.   1996      6  
                                              
 24.        2   Affoltern A.A.   1995      5  
 23.        2   Affoltern A.A.   1994      4  
 22.        2   Affoltern A.A.   1993      3  
 21.        2   Affoltern A.A.   1992      2  
 20.        2   Affoltern A.A.   1991      1  
                                              
       muniID        muni_name   year   time  
                                              
. list muniID muni_name year time in 20/40 ,ab(20)
. egen time = group(year)
J. Hainmueller (MIT) 45 / 64

### Slide 648

Fixed eﬀects: linear time trend
Modeling Time
Fixed E ↵ects Regression: Linear Time Trend
                                                                              
         rho    .17086519   (fraction of variance due to u_i)
     sigma_e     3.584409
     sigma_u    1.6271657
                                                                              
       _cons     .3892908   .1309232     2.97   0.003     .1314069    .6471747
        time     .2313692   .0171752    13.47   0.000     .1975386    .2651997
      repdem     .8247928   .2590615     3.18   0.002     .3145106    1.335075
                                                                              
    nat_rate        Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
                             Robust
                                                                              
                               (Std. Err. adjusted for 245 clusters in muniID)
corr(u_i, Xb)  = -0.0079                        Prob > F           =    0.0000
                                                F(2,244)           =    247.57
       overall = 0.1350                                        max =        19
       between = 0.0005                                        avg =      19.0
R-sq:  within  = 0.1604                         Obs per group: min =        19
Group variable: muniID                          Number of groups   =       245
Fixed-effects (within) regression               Number of obs      =      4655
. xtreg nat_rate repdem time , fe cl(muniID) i(muniID)
J. Hainmueller (MIT) 46 / 64

### Slide 649

Fixed eﬀects: year ﬁxed eﬀects
Modeling Time
Fixed E ↵ects Regression: Year Fixed E ↵ects
          4      .7034078    .167466     4.20   0.000     .3735443    1.033271
          3      .2789777   .1514124     1.84   0.067    -.0192644    .5772198
          2      .3829173   .1723225     2.22   0.027     .0434879    .7223468
        time  
              
      repdem     1.203658   .3031499     3.97   0.000     .6065335    1.800783
                                                                              
    nat_rate        Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
                             Robust
                                                                              
                               (Std. Err. adjusted for 245 clusters in muniID)
corr(u_i, Xb)  = -0.0168                        Prob > F           =    0.0000
                                                F(19,244)          =     31.48
       overall = 0.1575                                        max =        19
       between = 0.0005                                        avg =      19.0
R-sq:  within  = 0.1885                         Obs per group: min =        19
Group variable: muniID                          Number of groups   =       245
Fixed-effects (within) regression               Number of obs      =      4655
. xtreg nat_rate repdem i.time , fe cl(muniID) i(muniID)
J. Hainmueller (MIT) 47 / 64

### Slide 650

Fixed eﬀects: unit speciﬁc time trends
Modeling Time
Fixed E ↵ects Regression: Unit Speciﬁc Time Trends
           3       .248985    .024298    10.25   0.000     .2011244    .2968457
           2      .2914274    .024298    11.99   0.000     .2435667     .339288
           1       .333343    .024298    13.72   0.000     .2854823    .3812036
muniID#c.time  
               
       repdem     .9865241    .322868     3.06   0.002     .3505601    1.622488
                                                                               
     nat_rate        Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
                              Robust
                                                                               
                                (Std. Err. adjusted for 245 clusters in muniID)
corr(u_i, Xb)  = -0.3963                        Prob > F           =         .
                                                F(18,244)          =         .
       overall = 0.2864                                        max =        19
       between = 0.5185                                        avg =      19.0
R-sq:  within  = 0.2650                         Obs per group: min =        19
Group variable: muniID                          Number of groups   =       245
Fixed-effects (within) regression               Number of obs      =      4655
note: 19.time omitted because of collinearity
. xtreg nat_rate repdem muniID#c.time i.time  , fe cl(muniID) i(muniID)
J. Hainmueller (MIT) 48 / 64

### Slide 651

Unit speciﬁc time trends often eliminate “results”
Modeling Time
Unit Speciﬁc Time Trends Often Eliminate “Results”
240 Chapter 5 
TABLE 5.2.3  Estimated effects of labor regulation on the performance of firms  in Indian states  
(1) (2) (3) (4) 
Labor regulation (lagged) -.186 (.064) -.185 (.051) -.104 (.039) .0002 (.020) 
Log development expenditure per capita .240 (.128) .184 (.119) .241 (.106) 
Log installed electricity capacity per capita .089 (.061) .082 (.054) .023 (.033) 
Log state population .720 (.96) 0.310 (1.192) -1.419 (2.326) 
Congress majority -.0009 (.01) .020 (.010) 
Hard left majority -.050 (.017) -.007 (.009) 
Janata majority .008 (.026) -.020 (.033) 
Regional majority .006 (.009) .026 (.023) 
State-specific trends Adjusted R2 No .93 No .93 No .94 Yes .95 
Notes: Adapted from Besley and Burgess (2004), table IV. The table reports regression DD estimates of the effects of labor regulation on productivity. The dependent variable is log manufacturing output per capita. All models include state and year effects. Robust standard errors clustered at the state level are reported in parentheses. State amendments to the Industrial Disputes Act are coded 1 = pro-worker, 0 = neutral, -1 = pro-employer and then cumulated over the period to generate the labor regulation measure. Log of installed electrical capacity is measured in kilowatts, and log development expenditure is real per capita state spending on social and economic services. Congress, hard left, Janata, and regional majority are counts of the number of years for which these political groupings held a majority of the seats in the state legislatures. The data are for the sixteen main states for the period 1958-92. There are 552 observations. 
" AOt"'I( , Fixed Effects, DD, and Panel Data 2 
the estimates in columns 2 and 3 add tI specific covariates, such as government exptl and state population. This is in the spirit addition of state-level adult employment ra the minimum wage study. The addition of c Besley and Burgess estimates little. But the specific trends kills the labor regulation effl in column 4. Apparently, labor regulation in states where output was declining anywa trend therefore drives the estimated regulat 
I Picking Controls 
We've labeled the two dimensions in the D time because this is the archetypical DD e econometrics. But the DD idea is much mo of states, the subscript s might denote del some of which are affected by a policy a For example, Kugler, Jimeno, and Hernanz effects of age-specific employment protectlc Likewise, instead of time, we might group other types ofcharacteristics. An example II (1999), who studied the effect of chan .. laws on teen pregnancy using variation b' birth. Regardless of the group labels, hoe always set up an implicit treatment-contre question of whether this comparison il • careful consideration. One potential pitfall in this context IfI 
position of the treatment and control II result of treatment. Going back to a del and time comparisons, suppose we're i. of the generosity of public assistance Oft • 
cally, U.S. states have offered widely VI'" to poor unmarried mothers. Labor ecoDlli interested in the effects of such inc0n:.1 how much of an increase in living I 
and whether they make work less ann-. and Rosenbaum, 2001, for a recent eM 
“labor regulation increased in states where output was declining anyway”
J. Hainmueller (MIT) 49 / 64

### Slide 652

Distributed Lag model
yit = xitβ0 + xi,t−1β1 + xi,t−2β2 + ci + εit, t = 1, 2, . . . ,T
Model recognizes the eﬀect of change in x may occur with a
late
eﬀect of new tax credit for children on fertility rate
Interpretation of coeﬃcients:
Consider temporary increase in xit from level m to m + 1 at t
which lasts only one period
yt−1 = mβ0 + mβ1 + mβ2 + ci
yt = (m + 1)β0 + mβ1 + mβ2 + ci
yt+1 = mβ0 + (m + 1)β1 + mβ2 + ci
yt+2 = mβ0 + mβ1 + (m + 1)β2 + ci
yt+3 = mβ0 + mβ1 + mβ2 + ci

### Slide 653

Distributed Lag model
yit = xitβ0 + xi,t−1β1 + xi,t−2β2 + ci + εit, t = 1, 2, . . . ,T
Model recognizes the eﬀect of change in x may occur with a
late
eﬀect of new tax credit for children on fertility rate
Interpretation of coeﬃcients:
Consider temporary increase in xit from level m to m + 1 at t
which lasts only one period
yt−1 = mβ0 + mβ1 + mβ2 + ci
yt = (m + 1)β0 + mβ1 + mβ2 + ci
yt+1 = mβ0 + (m + 1)β1 + mβ2 + ci
yt+2 = mβ0 + mβ1 + (m + 1)β2 + ci
yt+3 = mβ0 + mβ1 + mβ2 + ci
β0 = yt − yt−1 immediate change in y due to temporary
one-unit increase in x (impact propensity)

### Slide 654

Distributed Lag model
yit = xitβ0 + xi,t−1β1 + xi,t−2β2 + ci + εit, t = 1, 2, . . . ,T
Model recognizes the eﬀect of change in x may occur with a
late
eﬀect of new tax credit for children on fertility rate
Interpretation of coeﬃcients:
Consider temporary increase in xit from level m to m + 1 at t
which lasts only one period
yt−1 = mβ0 + mβ1 + mβ2 + ci
yt = (m + 1)β0 + mβ1 + mβ2 + ci
yt+1 = mβ0 + (m + 1)β1 + mβ2 + ci
yt+2 = mβ0 + mβ1 + (m + 1)β2 + ci
yt+3 = mβ0 + mβ1 + mβ2 + ci
β1 = yt+1 − yt change in y one period after temporary
one-unit increase in x

### Slide 655

Distributed Lag model
yit = xitβ0 + xi,t−1β1 + xi,t−2β2 + ci + εit, t = 1, 2, . . . ,T
Model recognizes the eﬀect of change in x may occur with a
late
eﬀect of new tax credit for children on fertility rate
Interpretation of coeﬃcients:
Consider temporary increase in xit from level m to m + 1 at t
which lasts only one period
yt−1 = mβ0 + mβ1 + mβ2 + ci
yt = (m + 1)β0 + mβ1 + mβ2 + ci
yt+1 = mβ0 + (m + 1)β1 + mβ2 + ci
yt+2 = mβ0 + mβ1 + (m + 1)β2 + ci
yt+3 = mβ0 + mβ1 + mβ2 + ci
β2 = yt+2 − yt−1 change in y two periods after temporary
one-unit increase in x

### Slide 656

Distributed Lag model
yit = xitβ0 + xi,t−1β1 + xi,t−2β2 + ci + εit, t = 1, 2, . . . ,T
Model recognizes the eﬀect of change in x may occur with a
late
eﬀect of new tax credit for children on fertility rate
Interpretation of coeﬃcients:
Consider temporary increase in xit from level m to m + 1 at t
which lasts only one period
yt−1 = mβ0 + mβ1 + mβ2 + ci
yt = (m + 1)β0 + mβ1 + mβ2 + ci
yt+1 = mβ0 + (m + 1)β1 + mβ2 + ci
yt+2 = mβ0 + mβ1 + (m + 1)β2 + ci
yt+3 = mβ0 + mβ1 + mβ2 + ci
β3 = yt−1 change in y is zero three periods after temporary
one-unit increase in x

### Slide 657

Distributed Lag model
yit = xitβ0 + xi,t−1β1 + xi,t−2β2 + ci + εit, t = 1, 2, . . . ,T
Interpretation of coeﬃcients:
Consider permanent increase in xit from level m to m + 1 at
t, i.e., ( xs = m, s < t and xs = m + 1, s ≥ t)
yt−1 = mβ0 + mβ1 + mβ2 + ci
yt = (m + 1)β0 + mβ1 + mβ2 + ci
yt+1 = (m + 1)β0 + (m + 1)β1 + mβ2 + ci
yt+2 = (m + 1)β0 + (m + 1)β1 + (m + 1)β2 + ci
yt+3 = (m + 1)β0 + (m + 1)β1 + (m + 1)β2 + ci
After one period y has increased by β0 + β1, after two periods
y has increased by β0 + β1 + β2 and there are no further
increases after two periods
Long-run increase in y : β0 + β1 + β2 (long-run propensity)

### Slide 658

Lagged eﬀects of direct democracy
Modeling Dynamic E ↵ects
Lagged E ↵ects of Direct Democracy
         L3.    -.5245206   .4109918    -1.28   0.203    -1.334065    .2850239
         L2.    -.1648692   .4697434    -0.35   0.726    -1.090139    .7604003
         L1.     1.201065   .4233731     2.84   0.005      .367133    2.034998
         --.     .6364802   .3593924     1.77   0.078    -.0714272    1.344388
      repdem  
                                                                              
    nat_rate        Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
                             Robust
                                                                              
                               (Std. Err. adjusted for 245 clusters in muniID)
corr(u_i, Xb)  = -0.0206                        Prob > F           =    0.0000
                                                F(19,244)          =     21.63
       overall = 0.1235                                        max =        16
       between = 0.0012                                        avg =      16.0
R-sq:  within  = 0.1536                         Obs per group: min =        16
Group variable: muniID                          Number of groups   =       245
Fixed-effects (within) regression               Number of obs      =      3920
. xtreg nat_rate repdem L1.repdem L2.repdem  L3.repdem i.year, fe cl(muniID) i(muniID)
J. Hainmueller (MIT) 53 / 64

### Slide 659

Long-run eﬀect of direct democracy
Modeling Dynamic E ↵ects
Long-run E ↵ect of Direct Democracy
                                                                              
         (1)     1.294485   .4426322     2.92   0.004     .4226175    2.166353
                                                                              
    nat_rate        Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
                                                                              
 ( 1)  repdem + L.repdem + L2.repdem + L3.repdem = 0
. lincom repdem + L1.repdem + L2.repdem + L3.repdem 
J. Hainmueller (MIT) 54 / 64

### Slide 660

Lags and Leads model
yit = xi,t+1β−1+xitβ0+xi,t−1β1+xi,t−2β2+ci +εit; t = 1, 2, . . . ,T
Can use estimate of β−1 to test for anticipation eﬀects
Consider temporary increase in xit from level m to m + 1 at t
yt−2 =β−1m + mβ0 + mβ1 + mβ2 + ci
yt−1 =β−1(m + 1) + mβ0 + mβ1 + mβ2 + ci
Anticipation eﬀect: β−1 = yt−1 − yt−2 change in y in period
t − 1, the period before the temporary one-unit increase in x
Placebo test: if x causes y, but y does not cause x, then β−1
should be close to zero

### Slide 661

Leads and Lags
Modeling Dynamic E ↵ects
Leads and Lags
         L3.    -.2904252   .4108244    -0.71   0.480     -1.09964    .5187895
         L2.      .014941   .4583628     0.03   0.974    -.8879119    .9177939
         L1.     .8723962   .4619322     1.89   0.060    -.0374873     1.78228
         --.     .6975731   .4397095     1.59   0.114    -.1685376    1.563684
         F1.     .1707685   .3212906     0.53   0.596    -.4620886    .8036255
      repdem  
                                                                              
    nat_rate        Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
                             Robust
                                                                              
                               (Std. Err. adjusted for 245 clusters in muniID)
corr(u_i, Xb)  = -0.0353                        Prob > F           =    0.0000
                                                F(19,244)          =     20.34
       overall = 0.1269                                        max =        15
       between = 0.0010                                        avg =      15.0
R-sq:  within  = 0.1621                         Obs per group: min =        15
Group variable: muniID                          Number of groups   =       245
Fixed-effects (within) regression               Number of obs      =      3675
. xtreg nat_rate F1.repdem repdem L1.repdem L2.repdem L3.repdem  i.year, fe cl(muniID) i(muniID)
J. Hainmueller (MIT) 56 / 64

### Slide 662

The Autor Test
Let Dit be a binary indicator equaling 1 if unit i switched
from control to treatment between t and t − 1; 0 otherwise
Lags: Di,t−1: unit switched between t − 1 and t − 2
Leads: Di,t+1: unit switches between t + 1 and t
Include lags and leads into the ﬁxed eﬀects model:
yit = Di,t+2β−2+Di,t+1β−1+Ditβ0+Di,t−1β1+Di,t−2β2+ci+εit
Interpretation of coeﬃcients:
Leads β−1, β−2, etc. test for anticipation eﬀects
Switch β0 tests for immediate eﬀect
Lags β1, β2, etc. test for long-run eﬀects
highest lag dummy can be coded 1 for all post-switch years

### Slide 663

Lags and Leads of Switch to Representative Democracy
Modeling Dynamic E ↵ects
Lags and Leads of Switch to Representative Democracy
                                                                                                           817.      Stäfa   2009        1          0         0         0         1          0          0          0  816.      Stäfa   2008        1          0         0         0         1          0          0          0                                                                                                             815.      Stäfa   2007        1          0         0         1         0          0          0          0  814.      Stäfa   2006        1          0         1         0         0          0          0          0  813.      Stäfa   2005        1          1         0         0         0          0          0          0  812.      Stäfa   2004        0          0         0         0         0          1          0          0  811.      Stäfa   2003        0          0         0         0         0          0          1          0                                                                                                             810.      Stäfa   2002        0          0         0         0         0          0          0          1  809.      Stäfa   2001        0          0         0         0         0          0          0          0  808.      Stäfa   2000        0          0         0         0         0          0          0          0  807.      Stäfa   1999        0          0         0         0         0          0          0          0  806.      Stäfa   1998        0          0         0         0         0          0          0          0                                                                                                                    muni_n~e   year   repdem   switch_t   sw_lag1   sw_lag2   sw_lag3   sw_lead1   sw_lead2   sw_lead3                                                                                                             
>                         sw_lead1 sw_lead2 sw_lead3 in 806/817. list muni_name year repdem switch_t sw_lag1 sw_lag2 sw_lag3  ///
J. Hainmueller (MIT) 58 / 64

### Slide 664

Dynamic Eﬀect of Switching to Representative Democracy
Modeling Dynamic E ↵ects
Dynamic E ↵ect of Switching to Representative Democracy
    sw_lead5    -.0942415   .2599789    -0.36   0.717    -.6063307    .4178477    sw_lead4     .0750194   .2990359     0.25   0.802    -.5140018    .6640405    sw_lead3     .1440446   .3194086     0.45   0.652    -.4851054    .7731945    sw_lead2     .0843676   .3575292     0.24   0.814      -.61987    .7886051    sw_lead1     .2138757   .3899881     0.55   0.584    -.5542971    .9820485    switch_t     .7564792    .428627     1.76   0.079    -.0878019     1.60076     sw_lag1     1.881663   .4880099     3.86   0.000     .9204133    2.842913     sw_lag2     1.743682   .5395212     3.23   0.001      .680969    2.806396     sw_lag3     1.160345   .5080271     2.28   0.023     .1596665    2.161023                                                                                  nat_rate        Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]                             Robust                                                                                                             (Std. Err. adjusted for 245 clusters in muniID)
corr(u_i, Xb)  = -0.0162                        Prob > F           =    0.0000                                                F(27,244)          =     23.76
       overall = 0.1601                                        max =        19       between = 0.0011                                        avg =      19.0R-sq:  within  = 0.1913                         Obs per group: min =        19
Group variable: muniID                          Number of groups   =       245Fixed-effects (within) regression               Number of obs      =      4655
>        sw_lead1 sw_lead2 sw_lead3 sw_lead4 sw_lead5 i.year, fe cluster(muniID) i(muniID). xtreg  nat_rate sw_lag3 sw_lag2 sw_lag1 switch_t ///
J. Hainmueller (MIT) 59 / 64

### Slide 665

Dynamic Eﬀect of Switching to Representative Democracy
Modeling Dynamic E ↵ects
Dynamic E ↵ect of Switching to Representative Democracy
J. Hainmueller (MIT) 60 / 64

### Slide 666

Lagged Dependent Variable
yit = αyi,t−1 + ci + εit, t = 1, 2, . . . ,T
yit could be capital stock of ﬁrm i at time t and α the capital
depreciation rate
For simplicity, we assume that εit are uncorrelated in time (as
well as across individuals)
Note that
yi,t−1 = αyi,t−2 + ci + εi,t−1
So we have Cov[yi,t−1, ci] ̸= 0 and therefore we need to
include ﬁxed eﬀects ci into the regression
Does this work though?

### Slide 667

Lagged dependent variable
With T = 3 we have
yi3 = αyi2 + ci + εi3
yi2 = αyi1 + ci + εi2
and we can take time diﬀerences to eliminate ci (similar to ﬁxed
eﬀects)
yi3 − yi2 = α(yi2 − yi1) + (ci − ci) + (εi3 − εi2)
∆yi3 = α∆yi2 + ∆εi3
Since εi2 aﬀects both ∆ yi2 = yi2 − yi1 and ∆εi3 = εi3 − εi2 we get
Cov[∆yi2, ∆εi3] ̸= 0 and thus still have endogeneity
Models with ﬁxed eﬀects and lagged y do not produce consistent
estimators Might use past levels yi1 as an instrument for ∆ yi2, but
this requires strong assumptions (e.g., no serial correlation in εit)

### Slide 668

Cheng, Cheng and Mark Hoestra 2013. “Does Strengthening
Self-Defense Law Deter Crime or Escalate Violence?
Evidence from Expansions to Castle Doctrine”. Journal of
Human Resources, 48(3), pp. 821-854.

### Slide 669

Summary
Cheng and Hoekstra (2013) are interested in whether
expansions to “castle doctrine statutes” at the state level
increase or decrease gun violence.
Prior to these expansions, English common law principle
required “duty to retreat” before using lethal force against an
assailant except when the assailant is an intruder in the home
The home is one’s “castle” – hence, “castle doctrine”
When intruders threatened the victim in the home, the duty to
retreat was waived and lethal force in self-defense was allowed

### Slide 670

Castle doctrine law explained
In 2005, Florida passed a law that expanded self-defense
protections beyond the house
2000 to 2010, 21 states explicitly put ?castle doctrine? into
statute, and (more importantly) extended it to places outside
the home
In other words, 21 states removed the duty to retreat in
speciﬁed circumstances
Other changes:
Presumption of reasonable fear is added
Civil liability for those acting under the law is removed

### Slide 671

Texas example
Duty to retreat
“For purposes of subsection (a), in determining whether an actor described by
subsection (e) reasonably believed that the use of force was necessary, a ﬁnder of
fact may not consider whether the actor failed to retreat.”
Also: Language stating a person is justiﬁed using deadly force against another “if
a reasonable person in the actor’s situation would not have retreated” is removed
from the statute
Presumption of reasonableness
“Except as provided in subsection (b), a person is justiﬁed in using force against
another when and to the degree the actor [he] reasonably believes the force is
immediately necessary to protect the actor [himself] against the other’s use or
attempted use of unlawful force. The actor’s belief that the force was
immediately necessary as described by this subsection is presumed to be
reasonable if the actor . . . ”
Civil Liability
“A defendant who uses force or deadly force that is justiﬁed under Chapter 9
Penal code is immune from civil liability for personal injury or death that results
from the defendant’s use of force or deadly force, as applicable.”

### Slide 672

Economic theory
Workers supply legal or illegal labor
Costs: foregone wages, risk of arrest, risk of conviction,
penalties conditional on conviction
Beneﬁts: valuable goods
Castle doctrine expansions lowered the (expected) cost of
killing someone in self-defense
If people are rational, then lowering the price of lethal
self-defense should increase lethal homicides

### Slide 673

Economic theory
Two types of lethal force in self-defense:
True positive: use of lethal force against criminals committing
serious crimes
False positive: arguments escalate to lethal force that could
have been averted with “duty to retreat”; cases of mistaken
identity caused by lower expected penalties for being wrong,
etc.
Caveat:
Although deterrence is a theoretical possibility, note that the
goal of the laws was to protect/enhance victim rights, not
deter crime

### Slide 674

Castle Doctrine Laws in US, 2000-2010
Alabama 2006 Yes Yes No Yes
Alaska 2006 Yes No Yes Yes
Arizona 2006 Yes Yes Yes Yes
Florida 2005 Yes Yes Yes Yes
Georgia 2006 Yes Yes No Yes
Indiana 2006 Yes Yes No Yes
Kansas 2006 Yes Yes No Yes
Kentucky 2006 Yes Yes Yes Yes
Louisiana 2006 Yes Yes Yes Yes
Michigan 2006 Yes Yes No Yes
Mississippi 2006 Yes Yes Yes Yes
Missouri 2007 Yes No No Yes
Montana 2009 Yes Yes Yes No
North Dakota 2007 Yes No Yes Yes
Ohio 2008 Yes No Yes Yes
Oklahoma 2006 Yes Yes Yes Yes
South Carolina 2006 Yes Yes Yes Yes
South Dakota 2006 Yes Yes No No
Tennessee 2007 Yes Yes Yes Yes
Texas 2007 Yes Yes Yes Yes
West Virginia 2008 Yes Yes No No
State
Effective 
Year
Removes duty to retreat 
in any place one has a 
legal right to be
Presumption of 
reasonable fear
Removes 
civil 
liability
Removes duty to 
retreat somewhere 
outside home

### Slide 675

Economic theory concluded
Summary:
21 states passed laws removing “duty to retreat” in places
outside the home
17 states removed “duty to retreat” in any place one had a
legal right to be
13 states include a presumption of reasonable fear
18 states remove civil liability when force was justiﬁed under
law

### Slide 676

Cheng and Hoekstra’s identiﬁcation strategy
Research design:
Estimate the state-level causal eﬀect: what would’ve happened
to these same states had they not passed the law?
Compare the changes in outcomes after castle doctrine law
adoption to changes in the outcomes in other states in the
same region of the country
Estimation: Panel ﬁxed eﬀects estimation
Yit = β1Di + β2Tt + β3(CDLit) + α1Xit + ci + ut + εit
CDL is a dummy variable equalling 1 if the state has a castle
doctrine law and zero otherwise
Note: most of their speciﬁcations will include region dummies
interacted with year dummies or “region-by-year ﬁxed eﬀects”
which means the estimated coeﬃcient must originate from
variation within a given region (but across states within that
region) in a year

### Slide 677

Data
FBI Uniform Crime Reports (2000-2010)
State-level crime rates, or “oﬀenses per 100,000 population”
Falsiﬁcation outcomes: motor vehicle theft and larceny
Deterrence outcomes:
Burglary: the unlawful entry of a structure to commit a felony
or a theft
Robbery: the talking or attempting to take anything of value
from the care, custody or control of a person or persons by
force or threat of force or violence and/or putting the victim in
fear
Aggravated assault: unlawful attack by one person upon
another for the purpose of inﬂicting severe or aggravated
bodily injury
Homicide categories
1 Total homicides – murder plus non-negligent manslaughter
(∼14,000 per year)
2 Justiﬁable homicides by private citizens ( ∼250/year)

### Slide 678

Standard Errors
Statistical inference:
Cluster the standard errors at the state level
Are disturbances random draws from individually identical
distribution?
It’s likely that within a state, unobserved determinants of
crime are serially correlated
Bertand, Duﬂo and Mullainathan (2004) recommend adjusting
for serial correlation in unobserved disturbances within states
at the level of the treatment
“Randomization inference”
“how likely is it that we estimate eﬀects of this magnitude
when using randomly chosen pre-treatment time periods and
randomly assigning placebo treatments?”
Becoming increasingly commonly done

### Slide 679

Region-by-year ﬁxed eﬀects
Absent passing castle doctrine laws, outcomes in these 21
states would have changed similar to other states in their
same region
Recall the “region-by-year ﬁxed eﬀects” in the X term
By including “region-by-year ﬁxed eﬀects”, they are arguing
that unobserved changes in crime are running “parallel” to the
treatment states within region over time
Need not hold across regions since the across region variation
is not being used in this analysis due to the saturation of the
model with “region-by-year ﬁxed eﬀects”

### Slide 680

Testing the identiﬁcation strategy
Assess how including time-varying controls aﬀected estimates
Examine the eﬀect of the laws on “placebo” outcomes as a
falsiﬁcation (e.g., automobile thefts and larceny)
Include state-speciﬁc linear time trends
Alabama, et al. dummy interacted with TREND which equals
1 in 2000, 2 in 2001, . . . , 11 in 2010
Forces the identiﬁcation to come from variation in outcomes
around the state-speciﬁc linear trend
Stronger requirements, in other words
Outcomes must be large enough and diﬀerent enough from a
state-speciﬁc linear trend otherwise it is collinear with the
state-trend
Include “leads” to test for divergence in the year prior to
adoption
Test for historical tendency for outcomes in adopting states to
diverge from control states

### Slide 681

Control variables
Controls (X matrix in earlier equation)
Full-time police employment per 100,000 state residents from
the LEKOA data (FBI data)
Persons incarcerated in state prison per 100,000 residents
Shares of white/black men in 15-24 and 25-44 age groups
State per capita spending on public assistance
State per capita spending on public welfare

### Slide 682

Summary Statistics
Table 2: Descriptive Statistics
Dependent Variables
Homicides per 100,000 Population 4.8 5.5
(2.5) (1.9)
Justifiable Homicide by Private Citizens (count) 5.1 11.8
(8.2) (12.9)
Justifiable Homicide by Police (count) 8.0 23.4
(16.9) (34.3)
Robberies per 100,000 Population 107.2 143.1
(59.6) (47.5)
Aggravated Assault per 100,000 Population 267 296
(131) (114)
Burglary per 100,000 Population 710 744
(240) (235)
Larceny per 100,000 Population 2,334 2,328
(533) (532)
Motor Theft per 100,000 Population 331 381
(178) (174)
Proportion of Robberies in Which a Gun Was Used 0.35 0.37
(0.13) (0.13)
Mean (Unweighted) Mean (Weighted by 
Population)

### Slide 683

Step one: Falsiﬁcation test
Cheng and Hoekstra (2013) present falsiﬁcation ﬁrst to show the reader that
they ﬁnd no association within region over time in the passage of these laws
and either larceny rate or motor vehicle theft rate
The idea here is to immediately address concerns that what they show you later is
due to generic crime trends in those states that pass the laws
It’s a useful way to assuage doubt people may have, as remember, policy-makers
are not just randomly ﬂipping coins when passing laws, but presumably do so
because of things they observe on the ground
Results will be presented separately under six diﬀerent speciﬁcations
Each new speciﬁcation adds more controls
What should you expect to ﬁnd on key variables of interest?
No statistically signiﬁcant association between the CDL passage and the
placebos; small magnitudes preferably too
No association on the one-year lead either
How do you interpret coeﬃcients?
His model is “log outcomes” regressed onto a dummy variable (level), so these
are semi-elasticities and approximate percentage changes – but you should
transform them by taking the exponential of each coeﬃcient and then
diﬀerencing it from one to ﬁnd the actual percentage change
Ex: CDL = -0.0137 (column 12, Table 3, “Log (larceny rate)” outcome.)
Exp(-0.0137) = 0.986, and so 1-0.986 = 1.4. Thus, CDL reduced larceny rates
by 1.4 percent, which is not statistically signiﬁcant.

### Slide 684

Results – Falsiﬁcation Exercise
Table 3: Placebo Tests
7 8 9 10 11 12
Panel A: Larceny
Castle Doctrine Law 0.00745 0.00145 -0.00188 -0.00445 -0.00361 -0.0137
(0.0227) (0.0205) (0.0210) (0.0226) (0.0201) (0.0228)
-0.0103
(0.0114)
Observation 550 550 550 550 550 550
Panel B: Motor Vehicle Theft
Castle Doctrine Law 0.0767* 0.0138 0.00814 0.00775 0.00977 -0.00373
(0.0413) (0.0444) (0.0407) (0.0462) (0.0391) (0.0361)
-0.00155
(0.0287)
Observation 550 550 550 550 550 550
State and Year Fixed Effects Yes Yes Yes Yes Yes Yes
Region-by-Year Fixed Effects Yes Yes Yes Yes Yes
Time-Varying Controls Yes Yes Yes Yes
Controls for Larceny or Motor Theft Yes
State-Specific Linear Time Trends Yes
OLS - Unweighted
Log (Larceny Rate)
Notes: Each column in each panel represents a separate regression. The unit of observation is state-year. Robust standard errors are 
clustered at the state level. Time-varying controls include policing and incarceration rates, welfare and public assistance spending, median 
income, poverty rate, unemployment rate, and demographics.  
Log (Motor Vehicle Theft Rate)
One Year Before Adoption of 
Castle Doctrine Law
One Year Before Adoption of 
Castle Doctrine Law

### Slide 685

Step two: testing the deterrence hypothesis
Having found no eﬀect on their placebos, Cheng and Hoekstra
(2013) examine the eﬀect of CDL on three deterrence
outcomes: burglary, robbery and aggravated assault
They will, again, have six speciﬁcations per outcome in the
“weighted” regression, and then another ﬁve for the
“unweighted” regression
What does deterrence look like?
Negative signs on the CDL variable is consistent with
deterrence – these crimes were “deterred”, in other words
Bounds on the magnitudes from the standard errors are used
to provide some conﬁdence about the estimates as well

### Slide 686

Results – Deterrence
1 2 3 4 5 6 7 8 9 10 11 12
Panel A: Burglary
Castle Doctrine Law 0.0780***0.02900.02230.01640.0327*0.0237 0.0572**0.009610.006630.002770.006830.0207
(0.0255)(0.0236)(0.0223)(0.0247)(0.0165)(0.0207)(0.0272)(0.0291)(0.0268)(0.0304)(0.0222)(0.0259)
-0.0201 -0.0154
(0.0139) (0.0214)
Panel B: Robbery
Castle Doctrine Law 0.04080.03440.02620.02160.0376**0.0515* 0.04480.03200.008390.005520.008740.0267
(0.0254)(0.0224)(0.0229)(0.0246)(0.0181)(0.0274)(0.0331)(0.0421)(0.0387)(0.0437)(0.0339)(0.0299)
-0.0156 -0.0115
(0.0167) (0.0283)
Panel C: Aggravated Assault
Castle Doctrine Law 0.04340.03970.03720.03620.04240.0414 0.05550.06980.03430.03050.03410.0317
(0.0387)(0.0407)(0.0319)(0.0349)(0.0291)(0.0285)(0.0604)(0.0630)(0.0433)(0.0478)(0.0405)(0.0380)
-0.00343 -0.0150
(0.0161) (0.0251)
Observations 550 550 550 550 550 550 550 550 550 550 550 550
State and Year Fixed Effects Yes Yes Yes Yes Yes Yes Yes Yes Yes Yes Yes Yes
Region-by-Year Fixed Effects Yes Yes Yes Yes Yes Yes Yes Yes Yes Yes
Time-Varying Controls Yes Yes Yes Yes Yes Yes Yes Yes
Contemporaneous Crime Rates Yes Yes
State-Specific Linear Time Trends Yes Yes
OLS - Weighted by State Population OLS - Unweighted
Log (Burglary Rate) Log (Burglary Rate)
Log (Robbery Rate) Log (Robbery Rate)
Log (Aggravated Assault Rate) Log (Aggravated Assault Rate)
One Year Before Adoption of 
Castle Doctrine Law
One Year Before Adoption of 
Castle Doctrine Law
One Year Before Adoption of 
Castle Doctrine Law

### Slide 687

Conclusion
“In short, these estimates provide strong evidence against the
possibility that castle doctrine laws cause economically
meaningful deterrence eﬀects” (p. 17)
Translation: They can’t ﬁnd evidence of large deterrence
eﬀects
“Thus, while castle doctrine law may well have beneﬁts to
those legally justiﬁed in protecting themselves in self-defense,
there is no evidence that the law provides positive spillovers by
deterring crime more generally” (p. 17)
They note in footnote 24 that they cannot measure the
beneﬁts to victims whose crimes were deterred, or the beneﬁts
from lower legal costs; their focus is limited to whether it
deterred the crimes, not whether the net beneﬁts from the
laws were positive
Obviously, if there is no deterrence, though, then the net
beneﬁts are lower from CDL than they would be if they did
deter

### Slide 688

Step 3: Homicides
The key ﬁnding in this study is the very large eﬀect that CDL
had on homicides and non-negligent manslaughter
As the eﬀects are quite large, their strategy is ﬁrst to present
pictures
The pictures are a bit tricky, though, since they’re going to
also present pictures for the control and treatment group units
This is going to be useful for eye-balling the parallel trends
pre-treatment.
Remember, though – he needs parallel trends within-region –
these ﬁgures don’t show that
But you should start with pictures; don’t fetishize regression

### Slide 689

Log Homicide Rates – 2005 Adopter = Florida
1
1.1
1.2
1.3
1.4
1.5
1.6
1.7
1.8
1.9
2
Log Homicide Rate
2000 2001 2002 2003 2004 2005 2006 2007 2008 2009 2010
Year
Treatment: Florida (law enacted in October 2005)
Control: States that did not enact a law 2000 - 2010

### Slide 690

Log Homicide Rates – 2006 Adopter (13 states)
1
1.1
1.2
1.3
1.4
1.5
1.6
1.7
1.8
1.9
2
Log Homicide Rate
2000 2001 2002 2003 2004 2005 2006 2007 2008 2009 2010
Year
Treatment: States that enacted the law in 2006
Control: States that did not enact a law 2000 - 2010

### Slide 691

Log Homicide Rates – 2007 Adopter (4 states)
1
1.1
1.2
1.3
1.4
1.5
1.6
1.7
1.8
1.9
2
Log Homicide Rate
2000 2001 2002 2003 2004 2005 2006 2007 2008 2009 2010
Year
Treatment: States that enacted the law in 2007
Control: States that did not enact a law 2000 - 2010

### Slide 692

Log Homicide Rates – 2008 Adopter (2 states)
1
1.1
1.2
1.3
1.4
1.5
1.6
1.7
1.8
1.9
2
Log Homicide Rate
2000 2001 2002 2003 2004 2005 2006 2007 2008 2009 2010
Year
Treatment: States that enacted the law in 2008
Control: States that did not enact a law 2000 - 2010

### Slide 693

Log Homicide Rates – 2009 Adopter = Montana
.5
.6
.7
.8
.9
1
1.1
1.2
1.3
1.4
1.5
Log Homicide Rate
2000 2001 2002 2003 2004 2005 2006 2007 2008 2009 2010
Year
Treatment: State that enacted the law in 2009 (Montana)
Control: States that did not enact a law 2000 - 2010

### Slide 694

Residual log homicide rates
-.1
-.08
-.06
-.04
-.02
0
.02
.04
.06
.08
.1
Residual Log Homicide Rate
-6 -5 -4 -3 -2 -1 0 1 2 3 4
Years after passing castle doctrine/stand-your-ground
Treatment: Castle Doctrine/Stand-Your-Ground States
Control: Other States

### Slide 695

Estimation results
Before going into the estimation results, here?s what you are
looking for
This second hypothesis wherein reductions in the expected
penalties and costs associated with self-defense due to CDL
causes lethal violence to increase (non-deterrence escalation of
violence) should exhibit a positive association on the DD
variable
It should be diﬀerent from zero statistically and economically
meaningful
He will estimate the model using panel ﬁxed eﬀects estimation
and “negative binomial count models”
Because of the smaller number of annual homicides each year
in a state, he moves away from homicide rates in some
speciﬁcations and looks at “count” outcomes
He uses a class of estimators more appropriate for “counts”
called “count models”, like the negative binomial estimated
with maximum likelihood
Results are robust to least squares and count models

### Slide 696

Homicide – OLS
1 2 3 4 5 6
Panel A: Log Homicide Rate (OLS - Weighted)
Castle Doctrine Law 0.0801** 0.0946*** 0.0937*** 0.0875** 0.0985*** 0.100**
(0.0342) (0.0279) (0.0290) (0.0337) (0.0299) (0.0388)
-0.0212
(0.0246)
Observations 550 550 550 550 550 550
Panel B: Log Homicide Rate (OLS - Unweighted)
Castle Doctrine Law 0.0877 0.0811 0.0600 0.0461 0.0580 0.0672
(0.0638) (0.0769) (0.0684) (0.0764) (0.0662) (0.0450)
-0.0557
(0.0494)
Observations 550 550 550 550 550 550
State and Year Fixed Effects Yes Yes Yes Yes Yes Yes
Region-by-Year Fixed Effects Yes Yes Yes Yes Yes
Time-Varying Controls Yes Yes Yes Yes
Contemporaneous Crime Rates Yes
State-Specific Linear Time Trends Yes
One Year Before Adoption of Castle Doctrine 
Law
One Year Before Adoption of Castle Doctrine 
Law

### Slide 697

Homicide – Negative Binomial; Murder – OLS
1 2 3 4 5 6
Panel C: Homicide (Negative Binomial - Unweighted)
Castle Doctrine Law 0.0565* 0.0734** 0.0879*** 0.0783** 0.0937*** 0.108***
(0.0331) (0.0305) (0.0313) (0.0355) (0.0302) (0.0346)
-0.0352
(0.0260)
Observations 550 550 550 550 550 550
Panel D: Log Murder Rate (OLS - Weighted)
Castle Doctrine Law 0.0906** 0.0955** 0.0916** 0.0884** 0.0981** 0.0813
(0.0424) (0.0389) (0.0382) (0.0404) (0.0391) (0.0520)
-0.0110
(0.0230)
Observations 550 550 550 550 550 550
State and Year Fixed Effects Yes Yes Yes Yes Yes Yes
Region-by-Year Fixed Effects Yes Yes Yes Yes Yes
Time-Varying Controls Yes Yes Yes Yes
Contemporaneous Crime Rates Yes
State-Specific Linear Time Trends Yes
One Year Before Adoption of Castle Doctrine 
Law
One Year Before Adoption of Castle Doctrine 
Law

### Slide 698

Homicide – identiﬁcation test
Question: Did homicide rates of adopting states show a general historical
tendency to increase over time relative to other non-adopting states from the
same region?
Method: Move the 11-year panel back one year at a time (covering 1960-2009)
and estimate 40 placebo “eﬀects” of passing CDL 1 to 40 years earlier
Findings
Method Average estimate Estimates larger than actual estimate
Weighted OLS * –0.003 0/40
Unweighted OLS 0.001 1/40
Negative binomial 0.001 0/40

### Slide 699

Interpretation
No evidence that Castle Doctrine/Stand Your Ground Laws
deter violent crimes such as burglary, robbery and aggravated
assault
These laws do lead to an 8% net increase in homicide rates,
translating to around 600 additional homicides per year across
the 21 adopting states
Unlikely that all of the additional homicides were legally
justiﬁed
Economics of crime and behavior: incentives matter in some
contexts but not others

### Slide 700

Diﬀerences-in-diﬀerences: Card and Krueger (1994)
Suppose you are interested in the eﬀect of minimum wages on
employment (a classic and controversial question in labor
economics).
In a competitive labor market, increases in the minimum wage
would move us up a downward sloping labor demand curve →
employment would fall

### Slide 701

DD: Card and Krueger (1994)
Card and Krueger (1994) analyzed the eﬀect of a minimum
wage increase in New Jersey using a diﬀerences-in-diﬀerences
(DD) methodology
In February 1992, New Jersey increased the state minimum
wage from $4.25 to $5.05. Pennsylvania’s minimum wage
stayed at $4.25.
Locations of Restaurants (Card and Krueger 2000)
J. Hainmueller (MIT) 5/5 0
They surveyed about 400 fast food stores both in New Jersey
and Pennsylvania before and after the minimum wage increase
in New Jersey

### Slide 702

DD Strategy
DD is a version of ﬁxed eﬀects estimation. To see this
formally:
Y 1
ist : employment at restaurant i, state s, time t with a high w min
Y 0
ist : employment at restaurant i, state s, time t with a low w min
In practice of course we only see one or the other. We then
assume that:
E [Y 0
its |s, t] = γs + λt
In the absence of a minimum wage change, employment is
determined by the sum of a time-invariant state eﬀect, γs and
a year eﬀect, λt that is common across states
Let Dst be a dummy for high-minimum wage states and
periods
Assuming E [Y 1
its − Y 0
its |s, t] = δ is the treatment eﬀect,
observed employment can be written:
Yits = γs + λt + δDst + εits

### Slide 703

Wages After Rise in Minimum Wage
J. Hainmueller (MIT) 7/5 0

### Slide 704

DD Strategy II
In New Jersey
Employment in February is
E (Yist |s = NJ, t = Feb) = γNJ + λFeb
Employment in November is:
E (Yist |s = NJ, t = Nov) = γNJ + λNov + δ
Diﬀerence between November and February
E (Yist |s = NJ, t = Nov)−E (Yist |s = NJ, t = Feb) = λN −λF +δ
In Pennsylvania
Employment in February is
E (Yist |s = PA, t = Feb) = γPA + λFeb
Employment in November is:
E (Yist |s = PA, t = Nov) = γPA + λNov
Diﬀerence between November and February
E (Yist |s = PA, t = Nov) − E (Yist |s = PA, t = Feb) = λN − λF

### Slide 705

DD Strategy III
The DD strategy amounts to comparing the change in
employment in NJ to the change in employment in PA.
The population DD are:
(
E (Yist|s = NJ, t = Nov) − E (Yist|s = NJ, t = Feb)
)
−
(
E (Yist|s = PA, t = Nov) − E (Yist|s = PA, t = Feb)
)
= δ
This is estimated using the sample analog of the population
means

### Slide 706

Di↵erence-in-Di↵erences: Estimation
Sample Means: Minimum wage laws and employment
THE AMERICAN ECONOMIC REVlEW SEPTEMBER 1994 
TABLE 3-AVERAGE EMPLOYMENT  THE RISE PER STORE BEFORE AND I~ER  
IN NEW JERSEY MINIMUM WAGE  
Stores by state Stores in New Jersey a Differences within NJ~ 
Variable 
PA 
(i) 
NJ 
(ii) 
Difference, 
NJ-PA 
(iii) 
Wage = 
$4.25 
(iv) 
Wage = 
$4.26-$4.99 
(v) 
Wage r 
$5.00 
(vi) 
Low-
high 
(vii) 
Midrange-
high 
(viii) 
1. FTE employment before, 
all available observations 
2. FTE employment after, 
all available observations 
3. Change in mean FTE 
employment 
4. Change in mean FTE 
employment, balanced 
sample of storesC 
5. Change in mean FTE 
employment, setting 
FTE at temporarily 
closed stores to Od 
Notes: Standard errors are shown in parentheses. The sample consists of all stores with available data on employment. FTE 
(full-time-equivalent) employment counts each part-time worker as half a full-time worker. Employment at six closed stores 
is set to zero. Employment at four temporarily closed stores is treated as missing. 
astares in New Jersey were classified by whether starting wage in wave 1 equals $4.25 per hour (N = 101), is between 
$4.26 and $4.99 per hour (N = 140), or is $5.00 per hour or higher (N = 73). 
b~ifference in employment between low-wage ($4.25 per hour) and high-wage (2$5.00 per hour) stores; and difference 
in employment between midrange ($4.26-$4.99 per hour) and high-wage stores. 
'Subset of stores with available employment data in wave 1 and wave 2. 
this row only, wave-2 employment at four temporarily closed stores is set to 0. Employment changes are based on the 
subset of stores with available employment data in wave 1 and wave 2. 
TABLE 4-REDUCED-FORM MODELS FOR CHANGE IN EMPLOYMENT 
Model 
Independent variable (i) (ii) (iii) (iv) (v) 
1. New Jersey dummy 2.33 2.30 - - -
(1.19) (1.20) 
2. Initial wage gapa --15.65 14.92 11.91 
(6.08) (6.21) (7.39) 
3. Controls for chain and  no yes no yes yes 
ownershipb 
4. Controls for regionC 
5. Standard error of regression 
6. Probability value for controlsd 
Notes: Standard errors are given in parentheses. The sample consists of 357 stores 
with available data on employment and starting wages in waves 1 and 2. The 
dependent variable in all models is change in FTE employment. The mean and 
standard deviation of the dependent variable are -0.237 and 8.825, respectively. All 
models include an unrestricted constant (not reported). 
aProportional increase in starting wage necessary to raise starting wage to new 
minimum rate. For stores in Pennsylvania the wage gap is 0. 
b~hree dummy variables for chain type and whether or not the store is company- 
owned are included. 
'Dummy variables for two regions of New Jersey and two regions of eastern 
Pennsylvania are included. 
d~robability value of joint F test for exclusion of all control variables. 
J. Hainmueller (MIT) 17 / 50
Surprisingly, employment rose in NJ relative to PA after the
minimum wage change

### Slide 707

Regression DD
We can estimate the DD estimator in a regression framework
Advantages:
It’s easy to calculate the standard errors
We can control for other variables which may reduce the
residual variance (lead to smaller standard errors)
It’s easy to include multiple periods
We can study treatments with diﬀerent treatment intensity.
(e.g., varying increases in the minimum wage for diﬀerent
states)
The typical regression model we estimate is
Outcomeit = β1 + β2Treati + β3Postt + β4(Treat× Post)it + εit
where Treat is a dummy if the observation is in the treatment
group and Post is a post treatment dummy

### Slide 708

Regression DD - Card and Krueger
In the Card and Krueger case, the equivalent regression would
be:
Yits = α + γNJs + λdt + δ(NJ × d)st + εits
NJ is a dummy equal to 1 if the observation is from NJ
d is a dummy equal to 1 if the observation is from November
(the post period)
This equation takes the following values
PA Pre: α
PA Post: α + λ
NJ Pre: α + γ
NJ Post: α + γ + λ + δ
DD estimate: (NJ Post - NJ Pre) - (PA Post - PA Pre) = δ

### Slide 709

Graph - Observed Data
Waldinger (Warwick) 19 / 55

### Slide 710

Yist = α + γNJs + λdt + δ(NJ × d)st + εist
Graph - DD
Yist = a + g NJs + l dt + d(NJs  dt )+ #ist
Waldinger (Warwick) 20 / 55

### Slide 711

Yist = α + γNJs + λdt + δ(NJ × d)st + εist
Graph - DD
Yist = a + g NJs + l dt + d(NJs  dt )+ #ist
Waldinger (Warwick) 21 / 55

### Slide 712

Yist = α + γNJs + λdt + δ(NJ × d)st + εist
Graph - DD
Yist = a + g NJs + l dt + d(NJs  dt )+ #ist
Waldinger (Warwick) 22 / 55

### Slide 713

Yist = α + γNJs + λdt + δ(NJ × d)st + εist
Graph - DD
Yist = a + g NJs + l dt + d(NJs  dt )+ #ist
Waldinger (Warwick) 23 / 55

### Slide 714

Key assumption of any DD strategy: Parallel trends
The key assumption for any DD strategy is that the outcome
in treatment and control group would follow the same time
trend in the absence of the treatment
This doesn’t mean that they have to have the same mean of
the outcome
Parallel trends are diﬃcult to verify because technically one of
the parallel trends is an unobserved counterfactual
But one often will check using pre-treatment data to show
that the trends are the same
Even if pre-trends are the same one still has to worry about
other policies changing at the same time (omitted variable
bias)
Key Assumption of Any DD Strategy: Common Trends
The key assumption for any DD strategy is that the outcome in
treatment and control group would follow the same time trend in the
absence of the treatment.
This does not mean that they have to have the same mean of the
outcome!
Common trend assumption is di ¢cult to verify but one often uses
pre-treatment data to show that the trends are the same.
Even if pre-trends are the same one still has to worry about other
policies changing at the same time.
Waldinger (Warwick) 24 / 55

### Slide 715

Regression DD Including Leads and Lags
Including leads into the DD model is an easy way to analyze
pre-treatment trends
Lags can be included to analyze whether the treatment eﬀect
changes over time after assignment
The estimated regression would be:
Yits = γs + λt +
−1∑
τ=−q
γτ Dsτ +
m∑
τ=0
δτ Dsτ + xist + εist
Treatment occurs in year 0
Includes q leads or anticipatory eﬀects
Includes m leads or post treatment eﬀects

### Slide 716

Study including leads and lags – Autor (2003)
Autor (2003) includes both leads and lags in a DD model
analyzing the eﬀect of increased employment protection on
the ﬁrm’s use of temporary help workers
In the US employers can usually hire and ﬁre workers at will
Some states courts have made some exceptions to this
employment at will rule and have thus increased employment
protection
The standard thing to do is normalize the adoption year to 0
Autor then analyzes the eﬀect of these exemptions on the use
of temporary help workers.

### Slide 717

Results
Results
The leads are very close to 0. ! no evidence for anticipatory e §ects
(good news for the common trends assumption).
The lags show that the e §ect increases during the ﬁrst years of the
treatment and then remains relatively constant.
Waldinger (Warwick) 27 / 55
The leads are very close to 0. → no evidence for anticipatory
eﬀects (good news for the parallel trends assumption).
The lags show the eﬀect increases during the ﬁrst years of the
treatment and then remains relatively constant.

### Slide 718

Standard errors in DD strategies
Many paper using DD strategies use data from many years –
not just 1 pre and 1 post period
The variables of interest in many of these setups only vary at
a group level (say a state level) and outcome variables are
often serially correlated
In the Card and Krueger study, it is very likely that
employment in each state is not only correlated within the
state but also serially correlated
As Bertrand, Duﬂo and Mullainathan (2004) point out,
conventional standard errors often severely understate the
standard deviation of the estimators – standard errors are
biased downward

### Slide 719

Standard errors in DD – practical solutions
Bertrand, Duﬂo and Mullainathan propose the following
solutions:
1 Block bootstrapping standard errors (if you analyze states the
block should be the states and you would sample whole states
with replacement for bootstrapping)
2 Clustering standard errors at the group level (in STATA one
would simply add , cluster(state) to the regression
equation if one analyzes state level variation)
3 Aggregating the data into one pre and one post period.
Literally works if there is only one treatment data. With
staggered treatment dates one should adopt the following
procedure:
Regress Yst onto state FE, year FE and relevant covariates
Obtain residuals from the treatment states only and divide
them into 2 groups: pre and post treatment
Then regress the two groups of residuals onto a post dummy
Correct treatment of standard errors sometimes makes the
number of groups very small: in the Card and Krueger study
the number of groups is only 2.

### Slide 720

Threats to validity
1 Non-parallel trends
2 Compositional diﬀerences
3 Long-term eﬀects vs. reliability
4 Functional form dependence

### Slide 721

Non-parallel trends
Often policymakers will select the treatment and controls
based on pre-existing diﬀerences in outcomes – practically
guaranteeing the parallel trends assumption will be violated.
“Ashenfelter dip”
Named after Orley Ashenfelter, labor economist at Princeton
Participants in job trainings program often experience a “dip”
in earnings just prior to entering the program
Since wages have a natural tendency to mean reversion,
comparing wages of participants and non-participants using
DD leads to an upward biased estimate of the program eﬀect.
Regional targeting. NGOs may target villages that appear
most promising, or worse oﬀ, which is a form of selection bias
and violates parallel trends

### Slide 722

Checks for DD Design
Very common for readers and others to request a variety of
“robustness checks” from a DD design
Think of these as along the same lines as the leads and lags
we already discussed
Falsiﬁcation test using data for prior periods (already
discussed)
Falsiﬁcation test using data for alternative control group
Falsiﬁcation test using alternative “placebo” outcome that
should not be aﬀected by the treatment

### Slide 723

Alternative control group – DDD
Di↵erence-in-Di↵erences: Threats to Validity
Triple DDD: Mandated Maternity Beneﬁts (Grub er, 1994)
J. Hainmueller (MIT) 37 / 50

### Slide 724

DDD in Regression
Wijt = α + β1Xijt + β2τt + β3δj + β4Di + β5(δ × τ)jt
+ β6(τ × D)ti + β7(δ × D)ij + β8(δ × τ × D)ijt + εijt
The DDD estimate is the diﬀerence between the DD of
interest and a placebo DD (which is supposed to be zero)
If the placebo DD is non-zero, it might be diﬃcult to convince
the reviewer that the DDD removed all the bias
If the placebo DD is zero, then DD and DDD give the same
results but DD is preferable because standard errors are
smaller for DD than DDD

### Slide 725

Falsiﬁcation test with placebo outcome
Cheng and Hoekstra (2013) examine the eﬀect of castle
doctrine gun laws on homicides. They investigate the eﬀect of
the laws on non-gun related oﬀenses like grand theft auto and
ﬁnd no evidence of an eﬀect (“second order outcomes”)
Auld and Grootendorst (2004) estimated standard “rational
addiction” models (Becker and Murphy 1988) on data with
milk, eggs, oranges and apples. They ﬁnd these plausibly
non-addictive goods are addictive, which casts doubt on the
research design of all the rational addiction models.
Several studies found signiﬁcant network eﬀects on outcomes
like obesity, smoking, alcohol use and happiness. Cohen-Cole
and Fletcher (2008) use similar models and data and ﬁnd
similar network “eﬀects” for things that aren’t contagious like
acne, height and headaches

### Slide 726

Threats to validity – compositional diﬀerences
One of the risks of a repeated cross-section is that the
composition of the sample may have changed between the pre
and post period
Hong (2011) uses repeated cross-sectional data from the
Consumer Expenditure Survey (CEX) containing music
expenditure and internet use for a random sample of
households
Study exploits the emergence of Napter (ﬁrst ﬁle sharing
software widely used by Internet users) in June 1999 as a
natural experiment
Study compares internet users and internet non-users before
and after emergence of Napster

### Slide 727

Di↵erence-in-Di↵erences: Threats to Validity
Compositional di ↵erences?
Table 8: 2SIV Estimates for Age and Family Groupsa
HHs w/childrenAge 15-34 Aged 6-17
(1) (2)
A. DD Estimates
✓ -3.432 (1.284) -3.258 (1.203)
B. 2SIV Estimates
✓0 -2.427 (0.949) -0.120 (1.092)
✓1 -2.719 (2.079) -22.510 (6.889)
the mean of imputed
downloading probability 0.346 0.140
aStandard errors in parentheses. The dependent variable is music expenditure
in 1998 dollar. All regressions are estimated by weighted least squares using the
CEX weights. Panel A reports the coe cient estimates for✓in the DD regression
(16) that includes controls such as age, education, income, appliance, occupation,
family composition, and region. Panel B reports the coe cient estimates for✓0
and✓1in the regression (17) that includes various covariates. Bootstrap is used
to estimated standard errors.
Figure 1: Internet Di↵usion and Average Quarterly Music Expenditure in the CEX
0510152025303540
199619971998199920002001Year
Average Music Expenditure (in 1998 dollars)0102030405060708090100% of HHs w/Internet connection
Non-user GroupInternet User GroupInternet Diffusion
44
J. Hainmueller (MIT) 41 / 50

### Slide 728

Di↵erence-in-Di↵erences: Threats to Validity
Compositional di ↵erences?
Table 1: Descriptive Statistics for Internet User and Non-user Groupsa
Year 1997 1998 1999 2000
Internet User Non-user Internet User Non-user Internet User Non-user Internet User Non-user
Average Expenditure
Recorded Music $25.73 $10.90 $24.18 $9.97 $20.92 $9.37 $17.42 $8.22
Entertainment $195.03 $96.71 $193.38 $84.92 $182.42 $80.19 $164.88 $71.44
Zero Expenditure
Recorded Music .56 .79 .60 .80 .64 .81 .68 .83
Entertainment .08 .32 .09 .35 .14 .39 .17 .44
Demographics
Age 40.2 49.0 42.3 49.0 44.1 49.4 44.3 49.9
Income $52,887 $30,459 $51,995 $28,169 $49,970 $26,649 $47,510 $26,336
High School Grad. .18 .31 .17 .32 .21 .32 .22 .33
Some College .37 .28 .35 .27 .34 .27 .36 .27
College Grad. .43 .21 .45 .21 .42 .20 .37 .20
Manager .16 .08 .16 .08 .14 .08 .14 .07
Professional .23 .11 .22 .10 .21 .10 .19 .10
Living in a Dorm .12 0 .08 0 .05 0 .05 0
Urban .93 .87 .93 .86 .91 .87 .89 .86
Inside a MSA .84 .78 .83 .78 .83 .78 .81 .78
Pop. Size>4 million .34 .26 .30 .26 .31 .25 .28 .25
Appliance Ownership
Computer .79 .27 .81 .28 .80 .28 .81 .32
Sound System .81 .57 .79 .58 .78 .56 .76 .56
VCR .83 .72 .86 .74 .86 .72 .85 .72
Total Households
(in million) 15 91 22 86 28 80 34 76
Observations 3,163 19,052 5,624 21,550 8,191 22,810 9,606 20,919
aAll the statistics are weighted using the weights provided by the CEX. Years refer to the period from June of the year to May of the next year. Total
households are computed by summing the CEX weights.
40
Di↵usion of the internet changes samples (e.g. younger music fans are
early adopters)
J. Hainmueller (MIT) 42 / 50
Diﬀusion of the Internet changes samples (e.g., younger music fans are early
adopters)

### Slide 729

Comparative case studies
In qualitative comparative case studies, the goal is to reason
inductively the causal eﬀects of events or characteristics of a
single unit on some outcome, but oftentimes through logic
and historical analysis.
May not answer the causal questions at all because there is
rarely a counterfactual, or if so, it’s ad hoc.
Classic example of comparative case study approach is Alexis
de Toqueville’s Democracy in America
Quantitative comparative case studies are more explicitly
causal designs. Usually a natural experiment applied to a
single aggregate unit (e.g., city, school, ﬁrm, state, country)
Comparative case studies: compare the evolution of an
aggregate outcome for the unit aﬀected by the intervention to
the evolution of the same aggregate for some control group
(Card 1990; Card and Krueger 1994; Abadie and Gardeazabal
2003)

### Slide 730

Motivating example: The Mariel Boatlift
How do inﬂows of immigrants aﬀect the wages and
employment of natives in local labor markets?
Card (1990) uses the Mariel boatlift of 1980 as a natural
experiment to measure the eﬀect of a sudden inﬂux of
immigrants on unemployment among less-skilled natives
Motivating Example: The Mariel Boatlift
How do inﬂows of immigrants a ↵ect the wages and
employment of natives in local labor markets?
Card (1990) uses the Mariel Boatlift of 1980 as a natural
experiment to measure the e ↵ect of a sudden inﬂux of
immigrants on unemployment among less-skilled natives

### Slide 731

Motivating example: The Mariel Boatlift
How do inﬂows of immigrants aﬀect the wages and
employment of natives in local labor markets?
Card (1990) uses the Mariel boatlift of 1980 as a natural
experiment to measure the eﬀect of a sudden inﬂux of
immigrants on unemployment among less-skilled natives
The Mariel Boatlift increased the Miami labor force by 7%
Individual-level data on unemployment from the Current
Population Survey (CPS) for Miami and four comparison
cities (Atlanta, Los Angeles, Houston, Tampa-St. Petersburg)

### Slide 732

Motivating example: the Mariel Boatlift
Motivating Example: The Mariel Boatlift
 (~\ 
i
-..~
--- 
--~
0\ 
V 
Vii'} 
1J')NOO
~ 
~ci- 
N"':N
V' 
"'"'" 
"'"'" 
-I 
/ 
C'i~~ 
C1C1o.
- 
-0- 
-N-
~O\C;:) 
I 
I 
I 
I
,
viu~u.r:c
00 
~ - r;::; 
~ 
~ 
~ 
~
O\MO\ 
000\00.
ft 
ddd 
"';:dN 
c
'-' 
'-' 
'-" 
,-
~ 
O\MV 
\0\00 
C
E 
- 
M~d 
o\NM 
~
~ 
OO~ 
- 
0
0 
O\N 
I 
I.r:
- 
-- 
~
0. 
u
a 
~
U, 
d
C 
~
=' 
L..0
C 
t:
0 
u
',--- 
--~ 
0
c: 
-M- 
r-..000\"d
0 
... 
...
~
.- 
- 0 
- 
- 0 
-
-""""" 
K 
~ 
. 
-vr-.. 
MMO 
C
\~ 
00 
. 
.. 
'"
'/ 
.- 
~ 0\ 
1J') 
V 
0 
00 
0 
N 
g
au 
t-- 
- 
C/)
e 
>c 
~c.s 
I 
"""
.- 
\0
10.., 
"d
~ 
~
u~ 
M
U 
rl
U 
C 
c: 
:0
.I:: 
0 
0 
~
... 
~ 
~ 
L.
10.., 
.~.~ 
~
0 
0. 
0.. 
0
'a 
a 
E 
0\
0 
0 
0\
~ 
U 
-
~ 
I 
Y 
-
~ 
(I) 
.6
~ 
';j 
u 
u ~ 
'-'
II) 
'-.- 
~ 
a
u 
.~ 
~ 
.~ 
~ 
r \
U 
,- 
u'- 
'-'
~ 
0.2-: 
c.2-: 
a
g 
°u 
OGJ 
0
U 
'~~U 
.~~U 
.L..
L.. 
C 
C 
~
u,.. 
II) 
.- 
U 
~ 
,;... 
U 
I£: 
- 
U~o..~ 
~F;0..L.. 
U
~ 
=' 
,~ 
~ a 
~ 
u 
;a 
a 
~ 
"0 
R 
e ,-
0~ 
~ .-
0~ 
0..
t!: 
(,5 
~~UQ 
~~UQ 
~
';, 
<
II)
: 
GJ 
,.
; 
U 
II)
0 
C 
U
' 
U 
-0
' ~
Z, 
, 
GJ 
or
!~ 
, 
--- 
---"
'I 
.- 
N
l~ 
Q 
~_c 
~~~ 
.
,
\:)

### Slide 733

Comparative case studies
Advantages:
Policy interventions often take place at an aggregate level
Aggregate/macro data are often available
Problems:
Selection of control group is often ambiguous
Standard errors do not reﬂect uncertainty about the ability of
the control group to reproduce the counterfactual of interest

### Slide 734

Synthetic Control method
A distinctive feature of comparative case studies is that units
of analysis are usually aggregate entities, like countries or
regions, for which suitable single comparisons often do not
exist
The synthetic control method is based on the observation
that, when the units of analysis are a few aggregate entities, a
combination of comparison units (a “synthetic control”) often
does a better job reproducing the characteristics of a treated
unit than any single comparison unit alone.
Motivated by this consideration, the comparison unit in the
synthetic control method is selected as the weighted average
of all potential comparison units that best resembles the
characteristics of the treated unit(s)

### Slide 735

Synthetic control method: advantages
Precludes extrapolation
Does not require access to post-treatment outcomes in the
“design” phase of the study, when synthetic controls are
calculated
Makes explicit the contribution of each comparison unit to the
counterfactual of interest
Allows researchers to use quantitative and qualitative
techniques to analyze the similarities and diﬀerences between
the units representing the case of interest and the synthetic
control
Formalizing the way comparison units are chosen not only
represents a way of systemizing comparative case studies, it
also has direct implications for inference

### Slide 736

Synthetic control method: estimation
Suppose that we observe J + 1 units in periods 1 , 2, . . . ,T
Unit “one” is exposed to the intervention of interest (that is,
“treated”) during periods T0 + 1, . . . ,T
The remaining J are an untreated reservoir of potential
controls (a “donor pool”)
Let Y N
it be the outcome that would be observed for unit i at
time t in the absence of the intervention
Let Y I
it be the outcome that would be observed for unit i at
time t if unit i is exposed to the intervention in periods
T0 + 1 to T .
We aim to estimate the eﬀect of the intervention on the
treated unit ( α1T0+1, . . . , α1T ) where
α1t = Y I
1t − Y N
1t = Y1t − Y N
1t
for t > T0 and Y1t is the outcome for unit one at time t

### Slide 737

Synthetic control method: implementation
Let W = (w2, . . . ,wJ+1)′ with wj ≥ 0 for j = 2, . . . ,J + 1
and w2 + · · · + wj+1 = 1. Each value of W represents a
potential synthetic control
Let X1 be a ( k × 1) vector of pre-intervention characteristics
for the treated unit. Similarly, let X0 be a ( k × J) matrix
which contains the same variables for the unaﬀected units.
The vector W ∗ = (w ∗
2 , . . . ,w ∗
J+1)′ is chosen to minimize
||X1 − X0W ||, subject to our weight constraints
Let Yjt be the value of the outcome for unit j at time t. For a
post-intervention period t (with t ≥ T0) the synthetic control
estimator is:
ˆα1t = Y1t −
J+1∑
j=2
w ∗
j Yjt

### Slide 738

Synthetic control method: estimation
That is to reproduce the counterfactual Y N
it we ﬁnd the
combination of untreated units that best resembles the
treated unit before the intervention in terms of the values of k
relevant covariates (predictors of the outcome of interest)
Example: 1988 California’s tobacco control program
(Proposition 99):
Treated unit: California
Outcome of interest: tobacco consumption in California after
1988
Potential controls: other US states
Covariates: predictors of state-level tobacco consumption
measured before 1988

### Slide 739

Synthetic control method: estimation
Abadie, et al. consider
||X1 − X0W ||=
√
(X1 − X0W )′V (X1 − X0W ) where V is
some (k × k) symmetric and positive semideﬁnite matrix
Let Xjm be the value of the m-th covariates for unit j
Typically, V is diagonal with main diagonal v1, . . . ,vk. Then,
the synthetic control weights w ∗
2 , . . . ,w ∗
J+1 minimize:
k∑
m=1
vm
(
X1m −
J+1∑
j=2
wj Xjm
)2
where vm is a weight that reﬂects the relative importance that
we assign to the m-th variable when we measure the
discrepancy between the treated unit and the synthetic
controls

### Slide 740

Synthetic control estimation: estimation
The choice of V is important
W∗ depends on the choice of V
The synthetic control W∗(V ) is meant to reproduce the
behavior of the outcome variable for the treated unit in the
absence of the treatment
Therefore, the weights v1, . . . ,vk should reﬂect the predictive
value of the covariates

### Slide 741

Synthetic control estimation: estimation
Choice of v1, . . . ,vk can be based on
Subjective assessment of the predictive power of each of the
covariates, or calibration inspecting how diﬀerent values for
v1, . . . ,vk aﬀect the discrepancies between the treated unit
and the synthetic control
Use regression to assess the predictive power of the covariates
Minimize mean square prediction error (MSPE):
T0∑
t=1
(
Y1t −
J∑
j=2
w∗
j (V )Yjt
)2
Cross-validation
Divide the pre-treatment period into an initial training period
and a subsequent validation period
For any given V , calculate W∗(V ) in the training period.
Minimize the MSPE of W∗(V ) in the validation period

### Slide 742

What about unobserved factors?
Comparative case studies are complicated by unmeasured
factors aﬀecting the outcome variables as well as
heterogeneity in the eﬀect of observed and unobserved factors
However, as we will see, if the number of pre-intervention
periods in the data is large, matching on pre-intervention
outcomes allows us to control for heterogeneous responses to
multiple unobserved factors
Intuition: only units that are alike in observed and unobserved
determinants of the outcome variable as well as in the eﬀect
of those determinants on the outcome variable should produce
similar trajectories of the outcome variable over extended
periods of time

### Slide 743

The Application: California’s Proposition 99
In 1988, California ﬁrst passed comprehensive tobacco control
legislation:
increased cigarette tax by 25 cents/pack
earmarked tax revenues to health and anti-smoking budgets
funded anti-smoking media campaigns
spurred clean-air ordinances throughout the state
produced more than $100 million per year in anti-tobacco
projects
Other states that subsequently passed control programs are
excluded from donor pool of controls (AK, AZ, FL, HI, MA,
MD, MI, NJ, OR, WA, DC)

### Slide 744

Cigarette Consumption: CA and the Rest of the US
Cigarette Consumption: CA and the Rest of the U.S.
1970197519801985199019952000
020406080100120140
year
per−capita cigarette sales (in packs)
Californiarest of the U.S.
Passage of Proposition 99

### Slide 745

Cigarette Consumption: CA and synthetic CA
Cigarette Consumption: CA and synthetic CA
1970197519801985199019952000
020406080100120140
year
per−capita cigarette sales (in packs)
California
synthetic California
Passage of Proposition 99

### Slide 746

Predictor Means: Actual vs. Synthetic California
Predictor Means: Actual vs. Synthetic California
California Average of
Variables Real Synthetic 38 control states
Ln(GDP per capita) 10 .08 9 .86 9 .86
Percent aged 15-24 17 .40 17 .40 17 .29
Retail price 89 .42 89 .41 87 .27
Beer consumption per capita 24 .28 24 .20 23 .75
Cigarette sales per capita 1988 90 .10 91 .62 114 .20
Cigarette sales per capita 1980 120 .20 120 .43 136 .58
Cigarette sales per capita 1975 127 .10 126 .99 132 .81
Note: All variables except lagged cigarette sales are averaged for the 1980-
1988 period (beer consumption is averaged 1984-1988).

### Slide 747

Smoking Gap between CA and synthetic CA
Smoking Gap Between CA and synthetic CA
1970197519801985199019952000
−30−20−100102030
year
gap in per−capita cigarette sales (in packs)
Passage of Proposition 99

### Slide 748

Inference
To evaluate the signiﬁcance of our results, we conduct a series
of placebo experiments where we reassign the tobacco control
program to states other than California
We proceed as follows:
Iteratively apply the synthetic method to each country/state in
the donor pool and obtain a distribution of placebo eﬀects
Compare the gap (RMSPE) for California to the distribution of
the placebo gaps. For example the post-Prop. 99 RMSPE is:
RMSPE =
( 1
T − T0
T∑
t=T0+1
(
Y1t −
J+1∑
j=2
w∗
j Yjt
)2)1
2
Question is whether the eﬀect estimated by the synthetic
control for California is large relative to the eﬀect estimated for
a state chosen at random

### Slide 749

Smoking Gap for CA and 38 control states
Smoking Gap for CA and 38 control states
(All States in Donor Pool)
1970197519801985199019952000
−30−20−100102030
year
gap in per−capita cigarette sales (in packs)
Californiacontrol states
Passage of Proposition 99

### Slide 750

Smoking Gap for CA and 34 control states
Smoking Gap for CA and 34 control states
(Pre-Prop. 99 MSPE  20 Times Pre-Prop. 99 MSPE for CA)
1970197519801985199019952000
−30−20−100102030
year
gap in per−capita cigarette sales (in packs)
Californiacontrol states
Passage of Proposition 99

### Slide 751

Smoking Gap for CA and 29 control states
Smoking Gap for CA and 29 control states
(Pre-Prop. 99 MSPE  5T i m e sP r e - P r o p . 9 9M S P Ef o rC A )
1970197519801985199019952000
−30−20−100102030
year
gap in per−capita cigarette sales (in packs)
Californiacontrol states
Passage of Proposition 99

### Slide 752

Smoking Gap for CA and 19 control states
Smoking Gap for CA and 19 control states
(Pre-Prop. 99 MSPE  2T i m e sP r e - P r o p . 9 9M S P Ef o rC A )
1970197519801985199019952000
−30−20−100102030
year
gap in per−capita cigarette sales (in packs)
Californiacontrol states
Passage of Proposition 99

### Slide 753

Ratio Post-Prop. 99 RMSPE to Pre-Prop. 99 RMSPE
Ratio Post-Prop. 99 MSPE to Pre-Prop. 99 MSPE
(All 38 States in Donor Pool)
0 20 40 60 80100120
0 1 2 3 4 5
post/pre−Proposition 99 mean squared prediction error
frequency California

### Slide 754

The Application: The 1990 German Reuniﬁcation
Cross-country regressions are often criticized because they put
side-by-side countries of very diﬀerent characteristics
“What do Thailand, the Dominican Republic, Zimbabwe,
Greece and Bolivia have in common that merits their being
put in the same regression analysis? Answer: For most
purposes, nothing at all.” (Harberger 1987)
The synthetic control method provides a data-driven
procedure to select a comparison unit
Application: the economic impact of the 1990 German
reuniﬁcation in West Germany
Donor pool is restricted to 16 OECD countries

### Slide 755

West Germany and the OECD sample
West Germany and the OECD sample
1960 1970 1980 1990 2000
050001000015000200002500030000
year
per−capita GDP (PPP, 2002 USD)
West Germanyrest of the OECD sample
reunification

### Slide 756

Covariate averages before 1990
Covariate Averages before 1990
West Synthetic OECD
Germany West Germany Sample
GDP per-capita 15808.9 15800.9 8021.1
Trade openness 56.8 56.9 31.9
Inﬂation rate 2.6 3.5 7.4
Industry share 34.5 34.4 34.2
Schooling 55.5 55.2 44.1
Investment rate 27.0 27.0 25.9

### Slide 757

West Germany and synthetic West Germany
West Germany and synthetic West Germany
1960 1970 1980 1990 2000
050001000015000200002500030000
year
per−capita GDP (PPP, 2002 USD)
West Germanysynthetic West Germany
reunification

### Slide 758

Country Weights in the Synthetic West Germany
Country Weights in the Synthetic West Germany
Country Weight Country Weight
Australia 0 Netherlands 0.10
Austria 0.42 New Zealand 0
Belgium 0 Norway 0
Denmark 0 Portugal 0
France 0 Spain 0
Greece 0 Switzerland 0.11
Italy 0 United Kingdom 0
Japan 0.16 United States 0.22

### Slide 759

Post-Reuniﬁcation to Pre-Reuniﬁcation RMSPE Ratio
Post-Reuniﬁcation to Pre-Reuniﬁcation RMSE Ratio
Portugal
Austria
Denmark
France
Japan
Switzerland
UK
Belgium
Spain
New Zealand
USA
Netherlands
Greece
Norway
Australia
Italy
West Germany
●
●
●
●
●
●
●
●
●
●
●
●
●
●
●
●
●
5 10 15
Post−Period RMSE / Pre−Period RMSE

### Slide 760

Placebo Reuniﬁcation: 1975
Placebo Reuniﬁcation: 1975
1960196519701975198019851990
050001000015000200002500030000
year
per−capita GDP (PPP, 2002 USD)
West Germanysynthetic West Germany
placebo reunification

### Slide 761

Comparison to Regression
Constructing a synthetic comparison as a linear combination
of the untreated units with coeﬃcients that sum to one may
appear unusual
However in fact a regression-based approach does exactly this,
albeit in an implicit way
In contrast to the synthetic control method, the regression
approach does not restrict the coeﬃcients of the linear
combination that deﬁne the comparison unit to be in between
zero and one, therefore allowing extrapolation outside the
support of the data

### Slide 762

Comparison to regression
LetT1 be the number of post-intervention periods and:
X1 : (k ×1)-matrix of covariates for treated unit
X0 : (k ×J)-matrix of covariates for control units
Y0 : (T1 ×J)-matrix of post-intervention outcomes for control units
Let ˆB = (X0X′0)−1X0Y′0
be the (k ×T1) matrix of regression coeﬃcients ofY0 onX0
That is, each column ofˆB contains the regression coeﬃcients ofY0 onX0 for a
post-intervention period.
A regression-based counterfactual of the outcome for the treated unit in
absence of the treatment is given by the (T1 ×1) vectorˆB′X1

### Slide 763

Comparison to regression
Notice that
ˆB ′X1 = Y0W reg
where
W reg = X ′
0(X0X ′
0)−1X1
As a result, the regression-based estimate of the
counterfactual of interest is a linear combination of
post-treatment outcomes for the untreated units, with weights
W reg
Let ι be a ( J × 1)vector of ones. The sum of the regression
weights is ι′W reg . It can be proven that
ι′W reg = 1

### Slide 764

Synthetic vs. Regression Weights
Synthetic vs. Regression Weights
Synthetic Regression Synthetic Regression
Country Control Weight Weight Country Control Weight Weight
Australia 0 0.12 Netherlands 0.10 0.14
Austria 0.42 0.26 New Zealand 0 0.12
Belgium 0 0 Norway 0 0.04
Denmark 0 0.08 Portugal 0 -0.08
France 0 0.04 Spain 0 -0.01
Greece 0 -0.09 Switzerland 0.11 0.05
Italy 0 -0.05 UK 0 0.06
Japan 0.16 0.19 USA 0.22 0.13

### Slide 765

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Concluding remarks
Final thoughts
A good research design is one you are excited to tell people about – that’s
basically what characterizesallresearch designs, whether propensity score
matching or regression discontinuity designs, in some respects
Most important thing is to be honest with yourself and the reader
Always check for covariate balance in everything that you do
Causality is easy and hard. Don’t get confused which is the hard part and
which is the easy part. Don’t get enamored by statistical modeling that
obscures the identiﬁcation problem from plain sight. Always understand what
assumptions youmustmake, be clear which parameters you are and are not
identifying, and don’t be afraid of your answers.
Cunningham Causal Inference

### Slide 766

Overview
Part 1: The Core
Part 2: Selection on observables
Part 3: Selection on unobservables
Part 4: Advanced material
Conclusion
References
Articles and Books on the Syllabus I
Cunningham Causal Inference

Text-layer check: 766 slides; 8 slides without extractable text.

## Visual records for image-only slides

- **Slide 19 — Conditional expectation function.** The horizontal axis is $x$ and the vertical axis is $y$. Conditional distributions at $x_1,x_2,x_3$ vary around a linear conditional-mean line, illustrating that regression fits the mean relationship at each $x$, not every observation.
- **Slide 55 — Homoskedastic regression model.** The conditional distributions have approximately equal width at three values of $x$, illustrating the constant-variance assumption $Var(u\mid x)=\sigma^2$.
- **Slide 95 — Schooling and the conditional mean of wages.** The horizontal axis is years of completed education (about 0–20); the vertical axis is log weekly earnings in 2003. Dots are the conditional expectation function and the dashed line is the linear regression. The association is positive but not perfectly linear.
- **Slide 104 — Regression anatomy.** The dependent variable is Price. Separate panels plot Price against Weight and Length. Dashed lines are bivariate regressions; solid lines are multivariate regressions controlling for Length, Weight, Headroom, and Mileage. The slope differences illustrate how omitted covariates can change an apparent relationship.
- **Slides 293–294 — Training-earnings comparison table.** The table compares pre- and post-training earnings for male NSW programme participants using alternative comparison groups from PSID and CPS-SSA. Columns progress from unadjusted differences to age-adjusted differences, growth differences, and richer pre-treatment controls. Estimates depend on the comparison group and adjustment strategy; one column should not be selected without justification.
- **Slide 364 — Stata regression output.** The page shows an outcome regression on treatment $D$ and a second specification with a centred covariate. The treatment coefficient is about 80.014 (standard error about 2.145) and remains essentially unchanged after adding the control, illustrating the precision role of controls under random assignment. Other displayed regression statistics remain in the page image/text-layer record.
- **Slide 503 — Quarter-of-birth IV estimates.** For men born in 1920–1929 in the 1970 Census, the log-weekly-wage difference between first-quarter births and other-quarter births is about −0.00898 (SE 0.00301), and the education difference is −0.1256 years (SE 0.0155); the Wald return-to-education estimate is 0.0715 versus OLS 0.0809. For men born in 1930–1939 in the 1980 Census, the corresponding differences are about −0.01110 (SE 0.00274) and −0.1084 years (SE 0.0132); the Wald estimate is 0.1020 versus OLS 0.0709. The table compares the instrument’s first stage and IV/OLS estimates.
## Supplementary weekly courseware text

### lectures_dags.pdf — 26 pages (0 without PDF text)

#### Page 1

Directed Acyclical Graphs
Yumin Li
SILC, SHU
September 14, 2021

#### Page 2

Outline
Introduction
Backdoor criterion
Collider bias

#### Page 3

Outline
Introduction
Backdoor criterion
Collider bias

#### Page 4

History of graphical causal modeling in science
▶ Path diagrams were developed by Sewell Wright, early 20th
century geneticist, for causal inference
▶ Sewell Wright’s father, Phillip Wright, used them to prove the
existence of the “instrumental variable” estimator (see Stock
and Trebbi 2003)
▶ Judea Pearl and colleagues in Artiﬁcial Intelligence at UCLA
developed DAG modeling to create a formalized causal
inference methodology

#### Page 5

Three biases in observational studies DAGs help illustrate
1. Omitted variable bias
▶ Something causes the outcome which is correlated with the
treatment
2. Reverse causality
▶ Your logic about cause and eﬀect is backwards
3. Conditioning on collider
▶ Conditioning on some variables can introduce spurious
correlations

#### Page 6

Simple DAG
A Directed Acyclic Graph (DAG) is a set of nodes and arrows
with no directed cycles
Z X
U
Y
▶ Nodes represent variables
▶ Arrows represent direct causal eﬀects (“direct” means not
mediated by other variables in the graph)

#### Page 7

Complete DAGs
▶ A DAG is a complete set of all causal relationships, but we
emphasize the causal relationships that surround D and Y
▶ All direct causal eﬀects among the variables in the graph
▶ All common causes of any pair of variables in the graph
▶ Do not leave out anything, even if you can’t measure it or
don’t have data on it.
▶ DAGs are based on theory, a model, observation, experience,
prior studies, intuition
▶ DAGs can be intuitive representation. Try to get into the
habit of accompanying every regression you will ever run with
a DAG of your own creation to help guide your identiﬁcation
strategy

#### Page 8

Some DAG concepts
Z X
U
Y
▶ U is a parent of X and Y
▶ X and Y are descendants of Z
▶ There is a directed path from Z to Y
▶ There are two paths from Z to U (but no directed path)
▶ X is a collider of the path Z → X ← U; collider path is
blocked, or closed.
▶ X is a noncollider of the path Z → X → Y ; noncollider path
is open.

#### Page 9

Confounding
▶ Confounding occurs when the treatment and the outcomes
have a common cause or parent which creates spurious
correlation between D and Y
D
X
Y
The correlation between D and Y no longer reﬂects the
causal eﬀect of D on Y

#### Page 10

Backdoor Paths
▶ Confounding creates backdoor paths between treatment and
outcome (D ← X → Y )
▶ If we can close all of the otherwise open backdoor paths (by
conditioning on the common cause X ), then we can isolate
the causal eﬀect.
▶ Once we condition on X , the correlation between D and Y
estimates the causal eﬀect of D on Y
▶ Conditioning means calculating
E [Y |D = 1, X ] − E [Y |D = 0, X ] for each value of X
D
X
Y

#### Page 11

Blocked backdoor paths
A backdoor path is blocked if and only if:
▶ It contains a noncollider that has been conditioned on
▶ Or it contains a collider that has not been conditioned on and
has no descendents that have been conditioned on

#### Page 12

Blocked paths
Examples:
1. Conditioning on a noncollider blocks a path:
X Z Y
2. Conditioning on a collider opens a path:
Z X Y
3. Not conditioning on a collider blocks a path:
Z X Y

#### Page 13

Outline
Introduction
Backdoor criterion
Collider bias

#### Page 14

Backdoor criterion
Backdoor criterion
Conditioning on X satisﬁes the backdoor criterion with respect to
(D, Y ) directed path if:
1. All backdoor paths are blocked by X
2. No element of X is a collider
In words: If X satisﬁes the backdoor criterion with respect to
(D, Y ), then matching on X identiﬁes the causal eﬀect of D on Y

#### Page 15

▶ Matching on all common causes is suﬃcient
▶ There are two backdoor paths from D to Y
X1 D
X2
Y
▶ Conditioning on X1 and X2 blocks both backdoor paths

#### Page 16

▶ Matching may work even if not all common causes are
observed
▶ U and X1 are common causes
U X2
X1
D Y
▶ Conditioning on X1 and X2 is enough

#### Page 17

▶ There may be more than one set of conditioning
variables that satisfy the backdoor criterion
X3
X1
X2
D Y
X3
X1
X2
D Y
▶ Conditioning on the common causes, X1 and X2, is suﬃcient
▶ . . . but so is conditioning on X3

#### Page 18

Outline
Introduction
Backdoor criterion
Collider bias

#### Page 19

Collider bias
▶ Matching on an outcome may create bias
▶ There is only one open backdoor path from D to Y
X1 D
X2
Y
▶ Conditioning on X1 blocks the backdoor path
▶ But what if we also condition on X2?
X1 D
X2
Y
▶ Conditioning on X2 opens up a new path!

#### Page 20

▶ Matching on all pretreatment covariates can create bias
▶ There is one backdoor path and it is closed
X
U1
U2
D Y
▶ No confounding – D is identiﬁed
▶ But what if we condition on X ? Now a backdoor path opens.
X
U1
U2
D Y

#### Page 21

▶ Ultimately, we can’t know if we have a collider bias problem,
or whether we’ve satisﬁed the backdoor criterion, without a
model
▶ There’s no getting around it – all empirical work requires
theory to guide the work. Otherwise how do you know if
you’ve conditioning on a noncollider or a collider?
▶ Put diﬀerently, you cannot identify treatment eﬀects without
making assumptions about the process that generated your
data

#### Page 22

Simple example of collider bias
Important: Since unconditioned colliders block back-door paths,
what exactly does conditioning on a collider do? Let’s illustrate
with a fun example and some made-up data
▶ CNN.com headline: Megan Fox voted worst – but sexiest –
actress of 2009 (link)
▶ Assume talent and beauty are independent, but each causes
someone to become a movie star. What’s the correlation
between talent and beauty for a sample of movie stars
compared to the population as a whole (stars and non-stars)?

#### Page 23

▶ What if the sample consists only of movie stars?
Movie Star
Talent Beauty

#### Page 24

STATA code
clear all
set seed 3444
* 2500 independent draws from standard normal distribution
set obs 2500
generate beauty=rnormal()
generate talent=rnormal()
* Creating the collider variable (star)
gen score=(beauty+talent)
egen c85=pctile(score), p(85)
gen star=(score >=c85)
label variable star "Movie star"
* Conditioning on the top 15%
twoway (scatter beauty talent, mcolor(black) msize(small)
msymbol(smx)), ytitle(Beauty) xtitle(Talent) subtitle(Aspiring actors
and actresses) by(star, total)

#### Page 25

-4-2024-4-2024-4-2024-4-202401TotalBeautyTalentGraphs by Movie starAspiring actors and actresses
Figure: Top left ﬁgure: Non-star sample scatter plot of beauty (vertical
axis) and talent (horizontal axis). Top right right ﬁgure: Star sample
scatter plot of beauty and talent. Bottom left ﬁgure: Entire (stars and
non-stars combined) sample scatter plot of beauty and talent.

#### Page 26

Final Remarks
▶ The backdoor criterion provides a useful graphical guide that
can be used to select matching variables
▶ The backdoor criterion is only a set of suﬃcient conditions,
but covers most of the interesting cases
▶ Applying the backdoor criterion requires knowledge of the
DAG
▶ Suppose that there is a set of observed pretreatment covariates
and that we know if they are causes of the treatment and/or
the outcomes
▶ Suppose that there exists a subset of the observed covariates
such that matching on them is enough to control for
confounding
▶ Then, matching on the subset of the observed covariates that
are either a cause of the treatment or a cause of the outcome
or both is also enough to control for confounding

### lectures_overview.pdf — 19 pages (0 without PDF text)

#### Page 1

Causal Inference
Yumin Li
SILC, SHU
September 6, 2021

#### Page 2

Outline
Administrative
Empiricism in Microeconomics

#### Page 3

Administrative
▶ Dr. Yumin Li’s contact information:
▶ yuminli@shu.edu.cn
▶ Wechat: 18817679863 (use it don’t abuse it)
▶ Oﬃce hours: Friday: 10am to 12pm in Wenshang 405
▶ TA: Fangying Yao, mxmy0103@163.com

#### Page 4

Content
▶ Introduction and Review;
▶ Pearl’s directed acyclical graphical (DAG) model;
▶ Potential outcomes model;
▶ Regression discontinuity;
▶ Instrumental variables;
▶ Panel data
▶ Diﬀerence-in-diﬀerences
▶ Synthetic Control

#### Page 5

References and Readings
▶ References:
1. Scott Cunningham (2021) Causal Inference: The Mixtape
2. Morgan and Winship (2014) Counterfactuals and Causal
Inference (MW)
3. Angrist and Pischke (2009) Mostly Harmless Econometrics
(MHE)
▶ Readings:
▶ We will also discuss a number of papers in each lecture, each
of which you will need to learn inside and out.
▶ Lecture slides and reading lists are available online.
▶ The exam will cover material from all required readings.
▶ Lecture notes can be downloaded from our elearning site.

#### Page 6

Assessment
Your grade will be based on four things:
▶ 3 problem sets (one problem set each for parts 1-3 of the
course) weight: 10 percent each .
▶ Deadline for submission of 1st problem set: TBA. You will
have one week to work on the problem set.
▶ 1 in-class programming class exercise weight: 10 percent
▶ You will use STATA, Python or R as the problem sets require
programming.
▶ 1 research proposal on week 10 weight: 10 percent
▶ Research question, literature, research strategy, data, model
etc.
▶ 1 ﬁnal exam weight: 50 percent . Cumulative.

#### Page 7

The Journey Begins
Questions?

#### Page 8

Outline
Administrative
Empiricism in Microeconomics

#### Page 9

Introduction of Research
▶ Do we need research?
▶ Do you want to do research?
▶ How to do research?
▶ Courses, advisors, mentors etc.
▶ Books, papers and seminars.
▶ Math, English and coding etc.
▶ “Hard work beats talent when talent doesn’t work hard”

#### Page 10

Foundations of scientiﬁc knowledge
▶ Scientiﬁc methodologies are the epistemological foundation of
scientiﬁc knowledge
▶ Science does not collect evidence in order to “prove” what
people already believe or want others to believe.
▶ Science accepts unexpected and even undesirable answers.
▶ Science is process oriented, not outcome oriented.
▶ The example of Wechat public accounts vs papers.

#### Page 11

Empirical analysis in economics
▶ Theoretical vs. empirical research.
▶ Empirical Analysis uses data to test a theory or to estimate
a relationship between variables.
▶ The ﬁrst step in conducting an empirical economic analysis is
the (careful) formulation of the question we would like to
answer.
▶ In some cases we would like to develop and test a formal
Economic Model that describe mathematically a certain
relationship or behavior we are interested. Those models can
provide us with testable predictions that we can, then,
evaluate using data.
▶ You can also obtain a starting point for empirical analysis less
formally through a intuitive, somewhat more informal
reasoning.
▶ The Economic Model is the framework with which we describe
the relationships we are interested. We would like to test the
intuition of our results (or the hypotheses) or the whole
economic model.

#### Page 12

Economic models and econometrics
▶ After we have speciﬁed an Economic Model, we turn it into
what is called an Econometric Model that we could estimate
directly with data.
▶ One clear issue is about the functional form of the model, or
how to describe the relationships of the variables we are
interested through a equation which is simple to work.
▶ Other important issue is how to deal with variables that
cannot be reasonably observed or measured which show up in
our economic model.
▶ We will focus our studies starting the econometric analysis by
specifying an econometric model, given how time consuming
it can be to derive an Economic Model.

#### Page 13

Example: Job market training’s eﬀect on wages
▶ An (informal) Economic Model given by:
wage = f (educ, experience, training) (1)
▶ An Econometric Model given by:
wage =β0 +β1educ +β2experience +β3training + u (2)
▶ The term u is the error term or disturbance term contains
factors such as ”innate” ability, quality of education, family
background and any other factor that could impact someone’s
wage.
▶ Dealing with u is the fundamental aspect of any econometric
analysis as we will see throughout the semester.

#### Page 14

Econometric example (cont.)
▶ β0, β1, β2 and β3 are called the parameters of the
econometric model and they describe the directions and
intensities of the relationship between wage and each of the
variables.
▶ Finally, given our Econometric Model, we can test hypotheses
of interest in a form similar to what is covered in a statistics
or econometrics course. For example, one could test if the job
training program has no eﬀect in wages by testing if β3 = 0.
▶ To actually carry on with the Empirical Analysis we would
need data. Once data on the relevant variables has been
collected, we estimate the parameter values using a statistical
model.

#### Page 15

Data
▶ Experimental data: Collected at ”laboratory” environment,
common in the Natural Sciences. Researcher participates
actively on the process being ”recorded.” Much more diﬃcult
to obtain in the Social Sciences due to feasibility, ﬁnancial
cost or moral objections though it is more common now than
was once the case (e.g., JPAL, RAND health insurance
experiment)
▶ Non-Experimental (observational) data: Usually collected
through surveys on a retrospective fashion (you collect data
about what has happened to people) or by-product of some
business activity. The researcher is a passive actor, observing
people, ﬁrms etc. actions and results without interfering with
the outcomes. Most common data used in all of the social
sciences.

#### Page 16

Causality and Ceteris Paribus
▶ In most cases when testing an economic theory and especially
when evaluating public policy, our goal is to infer about the
causal eﬀect of one variable on another variable, say the
eﬀect of education on wages.
▶ Simply ﬁnding an association between two variables might be
suggestive of a causal eﬀect, but it might not mean that there
is a causal eﬀect linking them or that the causal eﬀect was
correctly estimated.
▶ Correlation does not mean causality: rooster and sun rise;
chocolate consumption and Nobel laureates; red compaign
and high demand etc.
▶ Sometimes there are causal relationships and yet no
observable correlation: endogeneity.
▶ The idea of Ceteris Paribus - “Other (relevant) factors being
equal” - is an important concept in causal analysis.

#### Page 17

Ceteris paribus (cont)
▶ The concept of ceteris paribus is very natural in economics
questions:
▶ When analyzing consumer’s demand, we want to know how a
change in price aﬀect the quantity holding other factors like
income, price of other goods, and individual tastes ﬁxed.
▶ In the evaluation of the job training program we want to know
its eﬀect on wages holding education, experience and other
factors ﬁxed.
▶ The gold standard in causal analysis would be able to carry
out an appropriate controlled experiment.
▶ Usually it is not possible to literally hold “all else equal” . The
key question is have we held enough factors ﬁxed to make a
case about causality in the analysis we are performing.
▶ Most times the experiment that we would like to perform is
not possible or practical to perform, but it is still useful to
think hard about what would be the experiment.
▶ Careful econometric analysis can provide us with a “simulated”
ceteris paribus experiment.

#### Page 18

Examples
▶ Eﬀects of Fertilizer in Crop Yield : It is diﬃcult to be sure
that some factors are equal between diﬀerent plots of land. In
fact land quality is diﬃcult to observe, for example. We could
get away with it if we allocate the levels of fertilizer randomly
(ignoring other characteristics that could aﬀect plot yield).
▶ Returns of Education : We would like to know how much the
wage of a random person would be increased if we gave her
one extra year or education keeping everything else ﬁxed.
Obviously, it would not be feasible (or morally acceptable) to
design an experiment that would randomly give more
education to people in the way that we dealt with the fertilizer
experiment. The big issue to infer causality here is that
people actually choose their level of education, and that
choice might depend on other factors that impact their ability
to earn higher wages (“ability”).

#### Page 19

Causal Inference is widely used
▶ The questions my friend studies in Kuaishou:
▶ How to design incentive strategy to attract new users?
▶ Can exposure of elite streamers promote users’ using time and
rewards?
▶ How can imrpovement of streamer PK promote users’ using
time and rewards?
▶ Can we change the product or service to increase streaming
demand and platform’s revenue?
▶ The methods they use:
▶ Causal inference of observational data; A/B test.

### lectures_review(2).pdf — 123 pages (0 without PDF text)

#### Page 1

Probability and Regression Review
Yumin Li
SILC, SHU
September 13, 2021

#### Page 2

Outline
Probability and statistics review
Deﬁnition of the simple regression model
Deriving the Ordinary Least Squares Estimates
Properties of OLS on any Sample of Data
Units of Measurement and Functional Form
Expected Values and Variance of OLS

#### Page 3

Outline
Probability and statistics review
Deﬁnition of the simple regression model
Deriving the Ordinary Least Squares Estimates
Properties of OLS on any Sample of Data
Units of Measurement and Functional Form
Expected Values and Variance of OLS

#### Page 4

Summation Operator
n∑
i=1
xi≡ x1 + x2 +... + xn (1)
Properties of the Summation operator
1. For any constant c:
n∑
i=1
c = nc (2)
2. n∑
i=1
cxi = c
n∑
i=1
xi (3)
3. For any constant a and b:
n∑
i=1
(axi + byi) = a
n∑
i=1
xi + b
n∑
i=1
yi (4)

#### Page 5

Note that: n∑
i=1
xi
yi
̸=
∑n
i=1 xi∑n
i=1 yi
(5)
and
n∑
i=1
x 2
i ̸=
( n∑
i=1
xi
)2
(6)
The average (mean) can be computed as
x = 1
n
n∑
i=1
xi = x1 + x2 +... + xn
n (7)
and the sum of the deviations from the mean:
n∑
i=1
(xi− x) = 0 (8)

#### Page 6

One result that will be very useful throughout the semester is:
n∑
i=1
(xi− x)2 =
n∑
i=1
x 2
i − n(x)2 (9)
and more generally,
n∑
i=1
(xi−x)(yi−y) =
n∑
i=1
xi(yi−y) =
n∑
i=1
(xi−x)yi =
n∑
i=1
xiyi−n(xy)
(10)

#### Page 7

Linear Functions
Remember that you can describe a straight line in a graph as:
y =β0 +β1x (11)
where β0 is the intercept (where the line crosses the y-axis) and
β1 is the slope of the line.
▶ A change in y is always β1 times a change in x:
∆y =β1∆x (12)
▶ It is simple to extend this to the case with several variables:
y =β0 +β1x1 +β2x2 (13)
∆y =β1∆x1 +β2∆x2 (14)

#### Page 8

Note that:
▶ β0 is still the intercept (when x1=0 and x2 = 0)
▶ If x2 is held ﬁxed (∆x2 = 0), the change in y is given by
β1∆x1
▶ β1 is called the partial eﬀect of x1 on y since it measures
how y changes with x1, holding x2 ﬁxed. Hence, the partial
eﬀect is very closely related to the notion of ceteris paribus.
β1 = ∆y
∆x1
(15)
if ∆x2 = 0.

#### Page 9

Expected Value
The expected value of a random variable, also called the
expectation and sometimes the population mean is simply the
weighted average of the possible values that variable can take, with
the weights being given by the probability of each value
”happening” in the population.
▶ Suppose the variable X can take values x1, x2,..., xk with
probability f (x1), f (x2),..., f (xk), respectively.
E (X ) = x1f (x1) +x2f (x2) +... + xkf (xk) =
k∑
j=1
xjf (xj) (16)

#### Page 10

For Example: if X takes on values −1, 0, and 2 with probabilities
0.3, 0.3, and 0.4, respectively,
E (X ) = ( −1)(0.3) + (0)(0.3) + (2)(0.4)
= 0 .5 (17)
In fact you could take the expectation of a function of that
variable, say X 2. Note that X 2 take only the values 0, 1 and 4
with probabilities 0.3, 0.3, and 0.4,
E (X 2) = ( −1)2(0.3) + (0)2(0.3) + (2)2(0.4)
= 0 .3 + 0 + 1.6
= 1 .9 (18)

#### Page 11

Properties of Expected Values
1. For any constant c, E (c) = c
2. For any constants a and b,
E (aX + b) = E (aX ) + E (b) = aE (X ) + b
3. If we have many constants, a1, a2,..., an, and many random
variables, X1, X2,..., Xn, then
E (a1X1+a2X2+... +anXn) = a1E (X1)+a2E (X2)+... +anE (Xn)
(19)
Using summation notation,
E
( n∑
i=1
aiXi
)
=
n∑
i=1
aiE (Xi) (20)
In the special case in which each ai = 1:
E
( n∑
i=1
Xi
)
=
n∑
i=1
E (Xi) (21)

#### Page 12

Outline
Probability and statistics review
Deﬁnition of the simple regression model
Deriving the Ordinary Least Squares Estimates
Properties of OLS on any Sample of Data
Units of Measurement and Functional Form
Expected Values and Variance of OLS

#### Page 13

OLS review
▶ We begin with cross-sectional analysis and will (eventually)
assume we can collect a random sample from the population
of interest.
▶ We begin with the following premise, once we have a
population in mind. There are two variables, x and y, and we
would like to “study how y varies with changes in x”.
▶ We have seen examples: x is amount of fertilizer, and y is
soybean yield. x is years of schooling, y is hourly wage.

#### Page 14

We must confront three issues:
1. How do we allow factors other than x to aﬀect y? There is
never an exact relationship between two variables (in
interesting cases).
2. What is the functional relationship between y and x?
3. How can we be sure we are capturing a ceteris paribus
relationship between y and x (as is so often the goal)?

#### Page 15

Consider the following equation relating y to x:
y =β0 +β1x + u, (22)
which is assumed to hold in the population of interest .
This equation deﬁnes the simple linear regression model (or
two-variable regression model).
▶ y and x are not treated symmetrically. We want to explain y
in terms of x. From a causality standpoint, it makes no sense
to “explain” past educational attainment in terms of future
labor earnings. We want to explain student performance ( y)
in terms of class size ( x).

#### Page 16

Terminology
y x
Dependent Variable Independent Variable
Explained Variable Explanatory Variable
Response Variable Control Variable
Predicted Variable Predictor Variable
Regressand Regressor
The terms “explained” and “explanatory” are probably best, as
they are the most descriptive and widely applicable. But
“dependent” and “independent” are used often. (The
“independence” here is not really statistical independence.)

#### Page 17

We mentioned the error term or disturbance, u, before. The
equation
y =β0 +β1x + u, (23)
explicitly allows for other factors, contained in u, to aﬀect y.
▶ This equation also addresses the functional form issue (in a
simple way). Namely, y is assumed to be linearly related to x.
We call β0 the intercept parameter and β1 the slope
parameter. These describe a population, and our ultimate
goal is to estimate them.
▶ In the simple regression framework, we eﬀectively treat all
factors aﬀecting y,other than x as being unobserved.

#### Page 18

The equation also addresses the ceteris paribus issue. In
y =β0 +β1x + u, (24)
all other factors that aﬀect y are in u. We want to know how y
changes when x changes, holding u ﬁxed . Let ∆ denote “change”.
Then holding u ﬁxed means ∆ u = 0. So
∆y =β1∆x + ∆u =β1∆x when ∆u = 0 (25)
This equation eﬀectively deﬁnes β1 as a slope, with the only
diﬀerence being the (very important) caveat ∆ u = 0.

#### Page 19

EXAMPLE: Yield and Fertilizer
yield =β0 +β1fertilizer + u (26)
where u contains things such as land quality, rainfall on a plot of
land, and so on. The slope parameter, β1, is of primary interest: it
tells us how yield will change when the amount of fertilizer
changes, holding all else ﬁxed.
Note: The linear function is probably not realistic here. The eﬀect
of fertilizer is likely to diminish at large amounts of fertilizer.

#### Page 20

EXAMPLE: Wage and Education
wage =β0 +β1educ + u (27)
where u contains somewhat nebulous factors (“ability”) but, also,
past workforce experience and tenure on the current job.
∆wage =β1∆educ when ∆u = 0 (28)
Is each year of education really worth the same dollar amount no
matter how much education one starts with?
▶ The linear form of the equation being used implies that a
one-unit change in x has the same impact on y regardless of
the initial value of x. That is not very realistic in several
applied cases.

#### Page 21

▶ This seems too easy! How can we hope to generally estimate
the ceteris paribus eﬀect of y on x when we have assumed all
other factors aﬀecting y are unobserved and lumped into u?
▶ The key is that the SLR model is a population model. When
it comes to estimating β1 (and β0) using a random sample of
data, we must restrict how u and x are related to each other.
▶ But x and u are properly viewed as having distributions in the
population. For example, if x = educ then, in principle, we
could ﬁgure out its distribution in the population just over,
say, 30 years old. Suppose u is cognitive ability. Assuming we
can measure what that means, it also has a distribution in the
population.
▶ What we must do is restrict the way u and x relate to each
other in the population.

#### Page 22

First, we make a simplifying assumption (without loss of
generality): the average, or expected, value of u is zero in the
population:
E (u) = 0 (29)
where E (·) is the expected value operator.
Normalizing land quality or “ability” to be zero in the population is
harmless.

#### Page 23

The presence of β0 in
y =β0 +β1x + u (30)
allows us to assume E (u) = 0. If the average of u is diﬀerent from
zero, say α0, we just adjust the intercept, leaving the slope the
same:
y = (β0 +α0) +β1x + (u−α0) (31)
where α0 = E (u). The new error is u−α0 and the new intercept
is β0 +α0. The important point is that the slope, β1, has not
changed.

#### Page 24

▶ The expectation operator, E (·), is a population concept, i.e.,
it refers to the whole group of interest, not only the sample
we have available to us.
▶ Its intuition is (loosely) similar to the average of a random
variable, in the population.
▶ Useful facts about the expectation operator for W and H any
random variables: Note that E (W ) is constant.
E (c) = c for any constant c (32)
E (aW + b) = aE (W ) + b for any constants a, b. (33)
E (W + H) = E (W ) + E (H) (34)
E (W− E (W )) = 0 (35)

#### Page 25

As you have seen before, we use the variance of a random variable,
say W as a measure of its dispersion.
Var(W ) =σ2 = E
[
(W− E (W ))2]
in the population (36)
Var(aW + b) = a2Var(W ) for any constants a, b. (37)
We can show that:
Var(W ) = E
(
W 2)
− [E (W )]2 (38)
In a given sample, we estimate the variance by:
S 2 = (n− 1)−1
n∑
i=1
(xi− x)2 (39)

#### Page 26

Usually, we are interested in how two variables are related to each
other. One important measure of the (linear) connection between
them is given by the covariance.
▶ In the population the covariance between X and Y is given by:
Cov(X, Y ) = E (XY )− E (X )E (Y ) (40)
▶ Then, if E (X ) or E (Y ) equals zero, Cov(X, Y ) = E (XY ).
▶ If X and Y are independent Cov(X, Y ) = 0, also,
Cov(aX + b, cY + d) = acCov(X, Y )

#### Page 27

▶ The covariance measures the amount of linear dependence
between two random variables. Cov(X, Y )> 0 indicates that
the random variables move in the same direction.
Cov(X, Y )< 0 that they move in opposite directions.
▶ Zero covariance does NOT imply that the variables are not
related (there could be a nonlinear connection between them)
▶ Interpreting the magnitude of the covariance can be tricky.
For that, we are better served by looking at correlation:
Corr(X, Y ) = Cov(X, Y )√
Var(X )Var(Y )
(41)
▶ The Correlation coeﬃcient is between −1 and 1. A positive
(negative) correlation indicates that the variables move in the
same (opposite) ways. The closer to 1 or −1 the strongest
the linear relationship is.

#### Page 28

An assumption that meshes well with our introductory treatment
involves the mean of the error term for each “slice” of the
population determined by values of x:
E (u|x) = E (u), all values x (42)
where E (u|x) means “the expected value of u given x”.
Then, we say u is mean independent of x.

#### Page 29

▶ Suppose u is “ability” and x is years of education. We need,
for example,
E (ability|x = 8) = E (ability|x = 12) = E (ability|x = 16)
so that the average ability is the same in the diﬀerent portions
of the population with an 8 th grade education, a 12 th grade
education, and a four-year college education.
Because people choose education levels partly based on
ability, this assumption is almost certainly false.
▶ Suppose u is “land quality” and x is fertilizer amount. Then
E (u|x) = E (u) if fertilizer amounts are chosen independently
of quality.

#### Page 30

Combining E (u|x) = E (u) (the substantive assumption) with
E (u) = 0 (a normalization) gives
E (u|x) = 0, all values x (43)
Called the zero conditional mean assumption . Because the
conditional expected value is a linear operator, E (u|x) = 0 implies
E (y|x) =β0 +β1x (44)
which shows the population regression function (or population
expectation function) is a linear function of x.

#### Page 31

EXAMPLE: College versus High School GPA.
Consider the population of students attending a university. We
assume β0 = 1.5 and β1 = 0.5 are known (which is unrealistic).
E (colGPA|hsGPA) = 1.5 + 0.5 hsGPA (45)
If hsGPA = 3.6 then the average colGPA among students with this
particular high school GPA is
1.5 + 0.5(3.6) = 3.3 (46)
▶ Anyone with hsGPA = 3.6 most likely will not have colGPA
= 3.3. This is the expected college GPA based on averages
within that slice of the population.
▶ Regression analysis is essentially about explaining eﬀects on
averages.

#### Page 32

Outline
Probability and statistics review
Deﬁnition of the simple regression model
Deriving the Ordinary Least Squares Estimates
Properties of OLS on any Sample of Data
Units of Measurement and Functional Form
Expected Values and Variance of OLS

#### Page 33

Deriving the Ordinary Least Squares Estimates
▶ Given data on x and y, how can we estimate the population
parameters,β0 and β1?
▶ Let{(xi, yi) : i = 1, 2,..., n} be a random sample of size n
(the number of observations) from the population.
▶ Plug any observation into the population equation:
yi =β0 +β1xi + ui (47)
where the i subscript indicates a particular observation.
▶ We observe yi and xi, but not ui (but we know it is there).

#### Page 34

We use the two population restrictions:
E (u) = 0 (48)
Cov(x, u) = 0 (49)
to obtain estimating equations for β0 and β1. We talked about the
ﬁrst condition. The second condition means that x and u are
uncorrelated. Both conditions are implied by
E (u|x) = 0 (50)

#### Page 35

With E (u) = 0, Cov(x, u) = 0 is the same as E (xu) = 0. Next we
plug in for u:
E (y−β0−β1x) = 0 (51)
E [x(y−β0−β1x)] = 0 (52)
These are the two conditions in the population that eﬀectively
determine β0 and β1.

#### Page 36

So we use their sample counterparts (which is a method of
moments approach to estimation):
n−1
n∑
i=1
(yi− ˆβ0− ˆβ1xi) = 0 (53)
n−1
n∑
i=1
xi(yi− ˆβ0− ˆβ1xi) = 0 (54)
where ˆβ0 and ˆβ1 are the estimates from the data.
These are two linear equations in the two unknowns ˆβ0 and ˆβ1.

#### Page 37

Pass the summation operator through the ﬁrst equation:
n−1
n∑
i=1
(yi− ˆβ0− ˆβ1xi) (55)
= n−1
n∑
i=1
yi− n−1
n∑
i=1
ˆβ0− n−1
n∑
i=1
ˆβ1xi (56)
= n−1
n∑
i=1
yi− ˆβ0− ˆβ1
(
n−1
n∑
i=1
xi
)
(57)
= y− ˆβ0− ˆβ1x (58)
We use the standard notation y = n−1 ∑n
i=1 yi for the average of
the n numbers{yi : i = 1, 2,..., n}. For emphasis, we call y a
sample average.

#### Page 38

We have shown that the ﬁrst equation,
n−1
n∑
i=1
(yi− ˆβ0− ˆβ1xi) = 0 (59)
implies
y = ˆβ0 + ˆβ1x (60)
Now, use this equation to write the intercept in terms of the slope:
ˆβ0 = y− ˆβ1x (61)

#### Page 39

Plug this into the second equation (but where we take away the
division by n):
n∑
i=1
xi(yi− ˆβ0− ˆβ1xi) = 0 (62)
so
n∑
i=1
xi[yi− (y− ˆβ1x)− ˆβ1xi] = 0 (63)
Simple algebra gives
n∑
i=1
xi(yi− y) = ˆβ1
[n∑
i=1
xi(xi− x)
]
(64)

#### Page 40

So, the equation to solve is
n∑
i=1
(xi− x)(yi− y) = ˆβ1
[n∑
i=1
(xi− x)2
]
(65)
If ∑n
i=1(xi− x)2̸= 0, we can write
ˆβ1 =
∑n
i=1(xi− x)(yi− y)∑n
i=1(xi− x)2 = Sample Covariance(xi, yi)
Sample Variance(xi) (66)

#### Page 41

▶ The previous formula for ˆβ1 is important. It shows us how to
take the data we have and compute the slope estimate. For
reasons we will see, ˆβ1 is called the ordinary least squares
(OLS) slope estimate.
▶ It can be computed whenever the sample variance of the xi is
not zero, which only rules out the case where each xi has the
same value. The intuition is that the variation in x is what
permits us to identify its impact in y.

#### Page 42

▶ Once we have ˆβ1, we compute ˆβ0 = y− ˆβ1x. This is the OLS
intercept estimate.
▶ These days, we let the computer do the calculations, which
are tedious even if n is small.
▶ Where does the name “ordinary least squares” come from?
For any candidates ˆβ0 and ˆβ1, deﬁne a ﬁtted value for each i
as
ˆyi = ˆβ0 + ˆβ1xi (67)
We have n of these. This is the value we predict for yi given
that x = xi.

#### Page 43

▶ The “mistake” we make is the residual:
ˆui = yi− ˆyi = yi− ˆβ0− ˆβ1xi (68)
Suppose we measure the size of the mistake, for each i, by
squaring it. Then we add them all up:
n∑
i=1
ˆu2
i =
n∑
i=1
(yi− ˆβ0− ˆβ1xi)2 (69)
This is called the sum of squared residuals . Finally, choose
ˆβ0 and ˆβ1 to minimize the sum of squared residuals. Using
calculus (or other arguments), it can be shown that the
solutions are the same we obtained before.

#### Page 44

Once we have the numbers ˆβ0 and ˆβ1 for a given data set, we
write the OLS regression line :
ˆy = ˆβ0 + ˆβ1x (70)
which allows us to predict y for any (sensible) value of x. Also
called the sample regression function .
▶ The intercept, ˆβ0, is the predicted y when x = 0, if x = 0
makes sense.
▶ The slope, ˆβ1, allows us to predict changes in y for any
(reasonable) change in x:
∆ˆy = ˆβ1∆x (71)
If ∆x = 1, so x increases by one unit, ∆ˆy = ˆβ1.

#### Page 45

EXAMPLE: Eﬀects of Education on Hourly Wage (WAGE2.DTA)
▶ wage is in dollars per hour. Data are from 1991. educ is
highest grade completed. Only men.
▶ At ﬁrst the negative intercept is a puzzle: wage is predicted to
be $− 0.90 when educ = 0! But the smallest amount of
education in the sample is educ = 8.
▶ Each additional year of schooling is estimated to be worth
about $0.54 per hour in increased wages.
ˆwage =−0.90 + 0.54 educ
n = 526

#### Page 46

▶ In the Stata output, ˆβ0 =−0.90 is the coeﬃcient labeled
“cons.” ˆβ1 = 0.54 is the coeﬃcient labeled “educ”.
▶ We will learn about the other numbers as we go.
▶ General form of the Stata command:
reg y x
The order of y and x is critical! The ﬁrst variable in the
command is taken to be the dependent variable, while the
subsequent are the independent variables.

#### Page 47

Reminder: When we write
wage =β0 +β1educ + u (72)
in the population, we do not know β0 and β1. ˆβ0 =−0.90 and
ˆβ1 = 0.54 are our estimates from this sample of 526 men. They
may or may not be close to the population values.
The function
ˆwage =−0.90 + 0.54 educ (73)
is the OLS regression line. When educ = 8,
ˆwage =−0.90 + 0.54(8) = $3.42per hour (74)

#### Page 48

EXAMPLE: Eﬀects of Campaign Spending on Voting Outcomes
(VOTE.DTA)
▶ Two-party vote, Congressional elections. voteA is the
percentage of votes received by candidate A; shareA is the
percentage share of campaign spending.
▶ Then, y = voteA, x = shareA.

#### Page 49

ˆvoteA = 26.81 + 0.464 shareA (75)
n = 173 (76)
▶ If shareA = 50, ˆvoteA = 26.81 +.0464(50) = 50.01,or
essentially 50 percent of the votes.
▶ If shareA increases by 10 - which means 10 percentage points
- voteA is predicted to increase by 4.64 percentage points in
votes cast to that candidate.
▶ Is this eﬀect causal? Or, do better candidates spend more
(more contributions?)? As with simple correlation analysis,
these simple regression estimates do not allow us to tell.

#### Page 50

Outline
Probability and statistics review
Deﬁnition of the simple regression model
Deriving the Ordinary Least Squares Estimates
Properties of OLS on any Sample of Data
Units of Measurement and Functional Form
Expected Values and Variance of OLS

#### Page 51

▶ Most residuals will be diﬀerent from zero (they do not lie on
the regression line). Some will be positive, others negative.
▶ Positive residuals indicate that the regression line (and, hence,
the predicted values) underestimate the value of yi, similarly if
ˆui is negative, the line overestimate.

#### Page 52

Algebraic Properties of OLS Statistics
Remeber how we obtained ˆβ0 and ˆβ1. When an intercept is
included, we will have:
n∑
i=1
(yi− ˆβ0− ˆβ1xi) = 0 (80)
The OLS residuals always add up to zero, by construction,
n∑
i=1
ˆui = 0 (81)
Because yi = ˆyi + ˆui by deﬁnition,
n−1
n∑
i=1
yi = n−1
n∑
i=1
ˆyi + n−1
n∑
i=1
ˆui (82)
and so y = ˆy.

#### Page 53

Similarly y the way we obtained our estimates,
n∑
i=1
xi(yi− ˆβ0− ˆβ1xi) = 0 (83)
The sample covariance (and therefore the sample correlation)
between the explanatory variables and the residuals is always zero:
n∑
i=1
xi ˆui = 0 (84)
Because the ˆyi are linear functions of the xi, the ﬁtted values and
residuals are uncorrelated, too:
n∑
i=1
ˆyi ˆui = 0 (85)

#### Page 54

Both properties hold by construction. ˆβ0 and ˆβ1 were chosen to
make them true.
A third property is that the point ( x, y) is always on the OLS
regression line. That is, if we plug in the average for x, we predict
the sample average for y:
y = ˆβ0 + ˆβ1x (86)
Again, we chose the estimates to make this true.

#### Page 55

Goodness-of-Fit
▶ For each observation, write
yi = ˆyi + ˆui (87)
▶ Deﬁne the total (SST), explained (SSE) and residual (SSR)
sum of squares as
SST =
n∑
i=1
(yi− y)2 (88)
SSE =
n∑
i=1
(ˆyi− y)2 (89)
SSR =
n∑
i=1
ˆu2
i (90)

#### Page 56

These are sample variances when divided by n (or n− 1). SST
n−1 is
the sample variance of yi, SSE
n−1 is the sample variance of ˆyi, and
SSR
n−1 is the sample variance of ˆui.
By writing
SST =
n∑
i=1
(yi− y)2 =
n∑
i=1
[(yi− ˆyi) + (ˆyi− y)]2 (91)
=
n∑
i=1
[ˆui + (ˆyi− y)]2 (92)
and using that the ﬁtted values and residuals are uncorrelated, can
show
SST = SSE + SSR (93)

#### Page 57

Assuming SST > 0, we can deﬁne the fraction of the total
variation in yi that is explained by xi (or the OLS regression line) as
R 2 = SSE
SST = 1− SSR
SST (94)
Called the R-squared of the regression.
It can be shown to be equal to the square of the correlation
coeﬃcient between yi and ˆyi. Therefore,
0≤ R 2≤ 1 (95)

#### Page 58

▶ R 2 = 0 means no linear relationship between yi and xi.
R 2 = 1 means a perfect linear relationship.
▶ As R 2 increases, the yi are closer and closer to falling on the
OLS regression line.
▶ Do not want to ﬁxate on R 2. It is a useful summary measure
but does not tell us about causality.

#### Page 59

Outline
Probability and statistics review
Deﬁnition of the simple regression model
Deriving the Ordinary Least Squares Estimates
Properties of OLS on any Sample of Data
Units of Measurement and Functional Form
Expected Values and Variance of OLS

#### Page 60

Units of Measurement and Functional Form
Two important issues need to be understood:
▶ How changing the units of measurement of the dependent
and/or independent variable aﬀects OLS estimates? It is very
important to know how y and x are measured in order to
interpret regression functions.
▶ How to incorporate functional forms usually used in economics
into regression analysis.

#### Page 61

Consider an equation estimated from CEOSAL.DTA, where annual
CEO salary is in thousands of dollars and the return on equity is a
percent:
ˆsalary = 963.191 + 18.501 roe (96)
n = 209, R 2 =.0132 (97)
When roe = 0 (it never is in the data), ˆsalary = 963.191. But
salary is in thousands of dollars, so $963,191.
A one percentage point (unit) increase in roe increases predicted
salary by 18.501, or $18,501.

#### Page 62

What if we measure roe as a decimal, rather than a percent?
Deﬁne
roedec = roe/100 (98)
What will happen to the intercept, slope, and R 2 when we regress
salary on roedec?
Nothing should happen to the intercept: roedec = 0 is the same as
roe = 0. But the slope will increase by 100. The goodness-of-ﬁt
should not change, and it does not.

#### Page 63

The new regression is
ˆsalary = 963.191 + 1, 850.1 roedec (99)
n = 209, R 2 =.0132 (100)
Now a one percentage point change in roe is the same as
∆roedec =.01, and so we get the same eﬀect as before. Note
that,
y =β0 +β1x + u⇒ y =β0 +
(β1
c
)
(cx) + u (101)
then the slope is scaled so to preserve the relationship between
variables.

#### Page 64

What if we measure salary in dollars, rather than thousands of
dollars?
salarydol = 1, 000· salary (102)
Both the intercept and slope get multiplied by 1,000.
ˆsalarydol = 963, 191 + 18, 501 roe (103)
n = 209, R 2 =.0132 (104)
Note that,
y =β0 +β1x + u⇒ cy = cβ0 + (cβ1)x + cu (105)

#### Page 65

Using the Natural Logarithm in Simple Regression
Recall the wage example:
ˆwage =−0.90 + 0.54 educ (106)
n = 526, R 2 =.133 (107)
Might be an okay approximation, but unsatisfying for a couple of
reasons:
▶ The negative intercept is a bit strange (even though the
equation gives sensible predictions for education ranging from
8 to 20).
▶ More importantly, the dollar value of another year of schooling
is constant. So the 16 th year of education is worth the same
as the second. We expect additional years of schooling to be
worth more, in dollar terms, than previous years.

#### Page 66

How can we incorporate an increasing eﬀect? One way is to
postulate a constant percentage eﬀect. We can approximate this
using the natural log (ln, but many uses log).
Let the dependent variable be ln( wage):
ln(wage) =β0 +β1educ + u (108)
Holding u ﬁxed,
∆ ln(wage) =β1∆educ (109)
so
β1 = ∆ ln(wage)
∆educ (110)

#### Page 67

Useful result from calculus:
100· ∆ ln(wage)≈ %∆wage (111)
This means when ln( wage) =β0 +β1educ + u, we have a simple
interpretation of β1:
100β1≈ %∆wage when ∆educ = 1 (112)
If we do not multiply by 100, we have the decimal version (the
proportionate change).
In this example, 100β1 is often called the return to education (just
like an investment). This measure is free of units of measurement
(currency, price level).

#### Page 68

▶ We still want to explain wage in terms of educ! This gives us
a way to get a (rough) constant percentage eﬀect.
▶ Predicting wage is more complicated but not usually the
interesting question.
Using WAGE2.DTA. Have to deﬁne ln( wage) in general.
ˆln(wage) = 0.58 +.082 educ (113)
n = 526, R 2 =.1858 (114)
The estimated return to each year of education is about 8 .2%.
Warning: This R-squared is not directly comparable to the
R-squared when wage is the dependent variable.

#### Page 69

We can use the ln on both sides of the equation to get constant
elasticity models. For example, if
ln(salary) =β0 +β1 ln(sales) + u (115)
then,
β1≈ %∆salary
%∆sales (116)
The elasticity is free of units of salary and sales.
A constant elasticity model for salary and sales makes more sense
than a constant dollar eﬀect.

#### Page 70

▶ The estimated elasticity of CEO salary with respect to ﬁrms
sales is about 0.257. (Note that we do not need to even know
salary is in thousands of dollars while sales is in millions.)
▶ A 10 percent increase in sales is associated with (causes?)
about a
0.257(10) = 2.57 (117)
percent increase in salary.

#### Page 71

Model Dep. Var. Indep. Var. β1
Level-Level y x ∆y =β1∆x
Level-Log y ln(x) ∆y = (β1/100)%∆x
Log-Level ln(y) x %∆y = (100β1)∆x
Log-Log ln(y) ln(x) %∆y =β1%∆x
The possibility of using the natural log to get nonlinear
relationships between y and x raises a question: What do we mean
now by ”linear” regression? The answer is that the model is linear
in the parameters, β0 and β1. We can use any transformations of
the dependent and independent variables to get interesting
interpretations for the parameters.

#### Page 72

Outline
Probability and statistics review
Deﬁnition of the simple regression model
Deriving the Ordinary Least Squares Estimates
Properties of OLS on any Sample of Data
Units of Measurement and Functional Form
Expected Values and Variance of OLS

#### Page 73

▶ Mathematical statistics: How do our estimators behave across
diﬀerent samples of data? On average, would we get the right
answer if we could repeatedly sample?
▶ We need to ﬁnd the expected value of the OLS estimators –
in eﬀect, the average outcome across all possible random
samples – and determine if we are right on average.
▶ Leads to the notion of unbiasedness, which is a “desirable”
characteristic for estimators.
E ( ˆβ) =β (118)

#### Page 74

▶ Remember, our objective is to estimate β1, the slope
population parameter that describes the relationship between
y and x.
▶ ˆβ1 is an estimator of that parameter obtained for a speciﬁc
sample.
▶ Diﬀerent samples will generate diﬀerent estimates ( ˆβ1) for the
“true” β1, i.e. ( ˆβ1) is a random variable.
▶ Unbiasedness is the idea that if we could take as many
random samples on Y as we want from the population, and
compute an estimate each time, the average of these
estimates would be equal to β1.

#### Page 75

Assumption SLR.1 (Linear in Parameters)
▶ The population model can be written as
y =β0 +β1x + u (119)
where β0 and β1 are the (unknown) population parameters.
▶ We view x and u as outcomes of random variables; thus, y is
random.
▶ Stating this assumption formally shows that our goal is to
estimate β0 and β1.

#### Page 76

Assumption SLR.2 (Random Sampling)
▶ We have a random sample of size n,{(xi, yi) : i = 1,..., n},
following the population model.
▶ We know how to use this data to estimate β0 and β1 by OLS.
▶ Because each i is a draw from the population, we can write,
for each i,
yi =β0 +β1xi + ui (120)
▶ Notice that ui here is the unobserved error for observation i.
It is not the residual that we compute from the data!

#### Page 77

Assumption SLR.3 (Sample Variation in the Explanatory
Variable)
▶ The sample outcomes on xi are not all the same value.
▶ This is the same as saying the sample variance of
{xi : i = 1,..., n} is not zero.
▶ In practice, this is no assumption at all. If the xi are all the
same value, we cannot learn how x aﬀects y in the population.

#### Page 78

Assumption SLR.4 (Zero Conditional Mean)
▶ In the population, the error term has zero mean given any
value of the explanatory variable:
E (u|x) = E (u) = 0. (121)
▶ This is the key assumption for showing that OLS is unbiased,
with the zero value not being important once we assume
E (u|x) does not change with x.
▶ Note that we can compute the OLS estimates whether or not
this assumption holds, or even if there is an underlying
population model.

#### Page 79

How do we show ˆβ1 is unbiased for β1? What we need to show is
E ( ˆβ1) =β1 (122)
where the expected value means averaging across random samples.
Step 1: Write down a formula for ˆβ1. It is convenient to use
ˆβ1 =
∑n
i=1(xi− x)yi∑n
i=1(xi− x)2 (123)
which is one of several equivalent forms.

#### Page 80

It is convenient to deﬁne SSTx = ∑n
i=1(xi− x)2, to total variation
in the xi, and write
ˆβ1 =
∑n
i=1(xi− x)yi
SSTx
(124)
Remember, SSTx is just some positive number. The existence of
ˆβ1 is guaranteed by SLR.3.
Step 2: Replace each yi with yi =β0 +β1xi + ui (which uses
SLR.1 and the fact that we have data from SLR.2).

#### Page 81

The numerator becomes
n∑
i=1
(xi− x)yi =
n∑
i=1
(xi− x)(β0 +β1xi + ui) (125)
=β0
n∑
i=1
(xi− x) +β1
n∑
i=1
(xi− x)xi +
n∑
i=1
(xi− x)ui (126)
= 0 +β1
n∑
i=1
(xi− x)2 +
n∑
i=1
(xi− x)ui (127)
=β1SSTx +
n∑
i=1
(xi− x)ui (128)
We used ∑n
i=1(xi− x) = 0 and ∑n
i=1(xi− x)xi = ∑n
i=1(xi− x)2.

#### Page 82

We have shown
ˆβ1 = β1SSTx + ∑n
i=1(xi− x)ui
SSTx
=β1 +
∑n
i=1(xi− x)ui
SSTx
(129)
Note how the last piece is the slope coeﬃcient from the OLS
regression of ui on xi, i = 1,..., n. We cannot do this regression
because the ui are not observed.

#### Page 83

Now deﬁne
wi = (xi− x)
SSTx
(130)
so we have
ˆβ1 =β1 +
n∑
i=1
wiui (131)
▶ ˆβ1 is a linear function of the unobserved errors, ui. The wi are
all functions of {x1, x2,..., xn}.
▶ The (random) diﬀerence between ˆβ1 and β1 is due to this
linear function of the unobservables.

#### Page 84

Step 3: Find E ( ˆβ1).
▶ Under Assumptions SLR.2 and SLR.4, E (ui|x1, x2,..., xn) = 0.
That means, conditional on{x1, x2,..., xn},
E (wiui|x1, x2,..., xn) = wiE (ui|x1, x2,..., xn) = 0
because wi is a function of {x1, x2,..., xn}. (In the next slides I
omit the conditioning in the expectations)
▶ This would not be true if, in the population, u and x are
correlated.

#### Page 85

Now we can complete the proof: conditional on {x1, x2,..., xn},
E ( ˆβ1) = E
(
β1 +
n∑
i=1
wiui
)
(132)
=β1 +
n∑
i=1
E (wiui) =β1 +
n∑
i=1
wiE (ui) (133)
=β1 (134)
Remember, β1 is the ﬁxed constant in the population. The
estimator, ˆβ1, varies across samples and is the random outcome:
before we collect our data, we do not know what ˆβ1 will be.

#### Page 86

THEOREM (Unbiasedness of OLS)
Under Assumptions SLR.1 through SLR.4
E ( ˆβ0) =β0 and E ( ˆβ1) =β1. (135)
▶ Omit the proof for ˆβ0.
▶ Now simulate data in Stata:
y = 3 + 2x + u (136)
▶ x∼ Normal(0, 9), u∼ Normal(0, 36), and they are
independent.

#### Page 87

▶ Each sample leads to a diﬀerent estimate, ˆβ0 and ˆβ1. Some
will be very close to the true values β0 = 3 and β1 = 2.
Nevertheless, some could be very far from those values.
▶ If we repeat the experiment again and again, and average the
estimates, we would get very close to 2.
▶ The problem is, we do not know which kind of sample we
have. We can never know whether we are close to the
population value.
▶ We hope that our sample is ”typical” and produces a slope
estimate close to β1 but we can never know.

#### Page 88

Unbiasedness is a property of the procedure or rule. After
estimating an equation like
ˆlwage = 0.58 +.082 educ (137)
n = 526, R 2 =.1858 (138)
it is tempting to say 8 .2% is an ”unbiased estimate” of the return
to education. Technically, this statement is incorrect. The rule
used to get ˆβ0 = 0.58 and ˆβ1 =.082 is unbiased (if we even
believe u is unrelated to educ).

#### Page 89

We use four assumptions:
▶ SLR.1: y =β0 +β1x + u
▶ SLR.2: random sampling
▶ SLR.3: sample variation in the xi
▶ SLR.4: E (u|x) = 0
The focus should mainly be on the last of these. What are the
omitted factors? Are they likely to be correlated with x? If so,
SLR.4 fails and OLS will be biased.

#### Page 90

Variance of the OLS Estimators
▶ Under SLR.1 to SLR.4, the OLS estimators are unbiased. This
tells us that, on average, the estimates will equal the
population values.
▶ But we need a measure of dispersion (spread) in the sampling
distribution of the estimators. We use the variance (and,
ultimately, the standard deviation).
▶ We could characterize the variance of the OLS estimators
under SLR.1 to SLR.4 (and we will later). For now, it is
easiest to introduce an assumption that simpliﬁes the
calculations.

#### Page 91

Assumption SLR.5 (Homoskedasticity, or Constant Variance)
The error has the same variance given any value of the explanatory
variable x:
Var(u|x) =σ2> 0 (139)
where σ2 is (virtually always) unknown.
Because we assume SLR.4, that is, E (u|x) = 0 whenever we
assume SLR.5, we can also write
E (u2|x) =σ2 = E (u2) (140)

#### Page 92

Under the population Assumptions SLR.1 ( y =β0 +β1x + u),
SRL.4 (E (u|x) = 0) and SLR.5 ( Var(u|x) =σ2),
E (y|x) =β0 +β1x (141)
Var(y|x) =σ2 (142)
So the average or expected value of y is allowed to change with x
– in fact, this is what interests us – but the variance does not
change with x.

#### Page 93

The constant variance assumption may not be realistic; it must be
determined on a case-by-case basis.
EXAMPLE: Suppose that we would like to analyse the relationship
between savings and income; y = sav, x = inc; and we think
E (sav|inc) =β0 +β1inc (143)
with β1> 0. This means average family saving increases with
income. If we impose SLR.5 then
Var(sav|inc) =σ2 (144)
which means the variability in saving does not change with income.
There are reasons to think savings would be more variable as
income increases. So SLR.5 is not realistic on this case.

#### Page 94

THEOREM (Sampling Variances of OLS)
Under Assumptions SLR.1 to SLR.2,
Var( ˆβ1|x) = σ2
∑n
i=1(xi− x)2 = σ2
SSTx
(145)
Var( ˆβ0|x) = σ2 (
n−1 ∑n
i=1 x 2
i
)
SSTx
(146)
(conditional on the outcomes {x1, x2,..., xn}).

#### Page 95

To show this, write, as before,
ˆβ1 =β1 +
n∑
i=1
wiui (147)
where wi = (xi− x)/SSTx. We are treating this as nonrandom in
the derivation. Because β1 is a constant, it does not aﬀect
Var( ˆβ1). Now, we need to use the fact that, for uncorrelated
random variables, the variance of the sum is the sum of the
variances.

#### Page 96

The{ui : i = 1, 2,..., n} are actually independent across i, and so
they are uncorrelated. So (remember that if we know x, we know
w)
Var( ˆβ1|x) = Var
( n∑
i=1
wiui|x
)
(148)
=
n∑
i=1
Var(wiui|x) =
n∑
i=1
w 2
i Var(ui|x) (149)
=
n∑
i=1
w 2
i σ2 =σ2
n∑
i=1
w 2
i (150)
where the second-to-last equality uses Assumption SLR.5, so that
the variance of ui does not depend on xi.

#### Page 97

Now we have
n∑
i=1
w 2
i =
n∑
i=1
(xi− x)2
(SSTx)2 =
∑n
i=1(xi− x)2
(SSTx)2 (151)
= SSTx
(SSTx)2 = 1
SSTx
(152)
We have shown
Var( ˆβ1) = σ2
SSTx
(153)

#### Page 98

▶ This is the “standard” formula for the variance of the OLS
slope estimator. It is not valid if Assumption SLR.5 does not
hold.
▶ The homoskedasticity assumption was not used to show
unbiasedness of the OLS estimators. That requires only SLR.1
to SLR.4.

#### Page 99

Usually we are interested in β1. We can easily study the two
factors that aﬀect its variance.
Var( ˆβ1) = σ2
SSTx
(154)
1. As the error variance increases, i.e, as σ2 increases, so does
Var( ˆβ1). The more “noise” in the relationship between y and
x – that is, the larger variability in u – the harder it is to learn
about β1.
2. By contrast, more variation in {xi} is a good thing:
SSTx↑ implies Var( ˆβ1)↓ (155)

#### Page 100

Notice that SSTx/n is the sample variance in x. We can think of
this as getting close to the population variance of x, σ2
x, as n gets
large. This means
SSTx≈ nσ2
x (156)
which means, as n grows, Var( ˆβ1) shrinks at the rate 1 /n. This is
why more data is a good thing: it shrinks the sampling variance of
our estimators.

#### Page 101

The standard deviation of ˆβ1 is the square root of the variance. So
sd( ˆβ1) = σ√SSTx
(157)
This turns out to be the measure of variation that appears in
conﬁdence intervals and test statistics.

#### Page 102

Estimating the Error Variance
In the formula
Var( ˆβ1) = σ2
SSTx
(158)
we can compute SSTx from{xi : i = 1,..., n}. But we need to
estimate σ2.
Recall that
σ2 = E (u2). (159)

#### Page 103

Therefore, if we could observe a sample on the errors,
{ui : i = 1, 2,..., n}, an unbiased estimator of σ2 would be the
sample average
n−1
n∑
i=1
u2
i (160)
But this not an estimator because we cannot compute it from the
data we observe, since ui are unobserved.
How about replacing each ui with its “estimate”, the OLS residual
ˆui?
ui = yi−β0−β1xi (161)
ˆui = yi− ˆβ0− ˆβ1xi (162)

#### Page 104

ˆui can be computed from the data because it depends on the
estimators ˆβ0 and ˆβ1. Except by ﬂuke,
ˆui̸= ui (163)
for any i.
ˆui = yi− ˆβ0− ˆβ1xi = (β0 +β1xi + ui)− ˆβ0− ˆβ1xi (164)
= ui− ( ˆβ0−β0)− ( ˆβ1−β1)xi (165)
E ( ˆβ0) =β0 and E ( ˆβ1) =β1, but the estimators almost always
diﬀer from the population values in a sample.

#### Page 105

Now, what about this as an estimator of σ2?
n−1
n∑
i=1
ˆu2
i = SSR/n (166)
It is a true estimator and easily computed from the data after OLS.
As it turns out, this estimator is slightly biased: its expected value
is a little less than σ2.

#### Page 106

The estimator does not account for the two restrictions on the
residuals, used to obtain ˆβ0 and ˆβ1:
n∑
i=1
ˆui = 0 (167)
n∑
i=1
xi ˆui = 0 (168)
There is no such restriction on the unobserved errors.

#### Page 107

The unbiased estimator of σ2 uses a degrees-of-freedom
adjustment. The residuals have only n− 2 degrees-of-freedom, not
n.
ˆσ2 = SSR
(n− 2) (169)
THEOREM: Unbiased Estimator of σ2
Under Assumptions SLR.1 to SLR.5,
E (ˆσ2) =σ2 (170)

#### Page 108

In regression output, it is
ˆσ =
√
ˆσ2 =
√
SSR
(n− 2) (171)
that is usually reported. This is an estimator of sd(u), the
standard deviation of the population error.
One small glitch is that ˆσ is not unbiased for σ. This will not
matter for our purposes (and there is no unbiased estimator of σ).

#### Page 109

▶ ˆσ is called the standard error of the regression , which
means it is an estimate of the standard deviation of the error
in the regression. Stata calls it the root mean squared error .
▶ Given ˆσ, we can now estimate sd( ˆβ1) and sd( ˆβ0). The
estimates of these are called the standard errors of the ˆβj.
We will use these a lot.
▶ Almost all regression packages report the standard errors in a
column next to the coeﬃcient estimates.

#### Page 110

▶ We just plug ˆσ in for σ:
se( ˆβ1) = ˆσ√SSTx
(172)
where both the numerator and denominator are computed
from the data.
▶ For reasons we will see, it is useful to report the standard
errors below the corresponding coeﬃcient, usually in
parentheses.

#### Page 111

SE and the data
The correct SE estimation procedure is given by the underlying
structure of the data
▶ It is very unlikely that all observations in a dataset are
unrelated, but drawn from identical distributions
(homoskedasticity)
▶ For instance, the variance of income is often greater in
families belonging to top deciles than among poorer families
(heteroskedasticity)
▶ Some phenomena do not aﬀect observations individually, but
they do aﬀect groups of observations uniformly within each
group (clustered data)

#### Page 112

▶ OLS inference is generally faulty in the presence of
heteroskedasticity

#### Page 113

▶ Fortunately, OLS is still useful
▶ Assume SLR.1-4 hold, but not SLR.5. Therefore
Var(ui|xi) =σ2
i
▶ The variance of our estimator, ˆβ1 equals:
Var( ˆβ1) =
∑n
i=1(xi− x)2σ2
i
SST 2x
▶ When σ2
i =σ2 for all i, this formula reduces to the usual
form, σ2
SST 2x

#### Page 114

▶ A valid estimator of Var( ˆβ1) for heteroskedasticity of any form
(including homoskedasticity) is
Var( ˆβ1) =
∑n
i=1(xi− x)2 ˆui
2
SST 2x
which is easily computed from the data after the OLS
regression. It’s called robust standard error.
▶ As a rule, you should always use the , robust command in
STATA.

#### Page 115

Clustered data
▶ Observations are related to each other within certain groups
▶ You want to regress kids’ grades on class size to determine the
eﬀect of class size on grades
▶ The unobservables of kids belonging to the same classroom
will be correlated (e.g., teacher quality, recess routines) while
will not be correlated with kids in far away classrooms
▶ This means we are assuming independence across “clusters”
but correlation within clusters.

#### Page 116

Cluster robust standard errors
▶ Using cluster-robust standard errors changes our estimation
results.
▶ Instead of summing over each individual, we ﬁrst sum over
groups

#### Page 117

Clustered data
▶ Let’s stack the observations and use matrix notation. Stack
observations by cluster
yg = xgβ + ug
▶ The OLS estimator of β is:
ˆβ = [X′X ]−1X′y
we just stacked the data is all
▶ The variance is given by:
Var(β) = E [[X′X ]−1X′ΩX [X′X ]−1]

#### Page 118

Clustered data
With this in mind, we can now write the variance-covariance
matrix for clustered data
Var( ˆβ) = [X′X ]−1[G∑
i=1
x′
g ˆug ˆu′
g xg ][X′X ]−1
▶ In STATA: vce(cluster clustervar) . Where clustervar
is a variable that identiﬁes the groups in which unobservables
are allowed to correlate

#### Page 119

The importance of knowing your data
▶ In real world you should never go with the “independent and
identically distributed” (i.e., homoskedasticity) case. Life is
not that simple.
▶ You need to know your data in order to choose the correct
error structure and then infer the required SE calculation
▶ If you have aggregate variables, like class size, clustering at
that level is required

#### Page 120

Summary and Tasks Left Undone
▶ In the table of Stata output, we have talked about the
coeﬃcients (Coef.), and how to interpret them. It is critical to
become comfortable with that.
▶ Of course, we know what the sample size (Number of obs)
means. We have talked about the R-squared and how it
depends on the total, explained (model) and residual sum of
squares.
▶ We just talked about ˆσ, the standard error of the regression
(Root MSE). Its main importance, for now, is how it appears
in the standard errors of the intercept and slope estimates.
The standard errors are in the column labeled ”Std. Err.”

#### Page 121

▶ We have talked about conditions under which OLS is
unbiased, and we have derived the sampling variance of the
OLS estimators(which underlies the standard error
calculations).
▶ SLR.1: y =β0 +β1x + u
▶ SLR.2: random sampling
▶ SLR.3: sample variation in the xi
▶ SLR.4: E (u|x) = 0
▶ SLR.5: Var(u|x) = Var(u) =σ2.

#### Page 122

▶ The ﬁrst four assumptions are used for unbiasedness and we
add the ﬁfth, homoskedasticity, for the ”standard” variance
calculation.
▶ We could continue by discussing statistical inference and
conﬁdence intervals for the population parameters. But the
mechanics are very similar to multiple regression, and so we
hold oﬀ.
▶ Also, there are a lot of ways to combine data to estimate a
slope and intercept. How do we know the OLS approach is a
good one? We will argue, in the context of multiple
regression, that OLS is ”best” in a certain sense under the
ﬁve given assumptions. These are often called the
Gauss-Markov Assumptions (for cross-sectional regression).

#### Page 123

Basics of regression
▶ A key tool in multivariate statistical inference is linear
regression in which we specify the conditional mean of some
response variable, Y , as a linear function of k independent
variables
E [Y|X1, X2,..., XK ] =β1X1 +β2X2 +··· +βK XK
▶ Note that the conditional mean of Y is a function of
observable X1, X2,..., XK covariates with ﬁxed (i.e.,
constant) parameters, β1, β2, . . . ,βK .
▶ Given values for these parameters, then our regression model
predicts the average value of the outcome variable, Y , for
diﬀerent values of X1, X2, . . . , XK

### lectures_review.pdf — 72 pages (0 without PDF text)

#### Page 1

Probability and Regression Review
Yumin Li
SILC, SHU
September 6, 2021

#### Page 2

Outline
Probability and statistics review
Deﬁnition of the simple regression model
Deriving the Ordinary Least Squares Estimates
Properties of OLS on any Sample of Data
Units of Measurement and Functional Form
Expected Values and Variance of OLS

#### Page 3

Outline
Probability and statistics review
Deﬁnition of the simple regression model
Deriving the Ordinary Least Squares Estimates
Properties of OLS on any Sample of Data
Units of Measurement and Functional Form
Expected Values and Variance of OLS

#### Page 4

Summation Operator
n∑
i=1
xi≡ x1 + x2 +... + xn (1)
Properties of the Summation operator
1. For any constant c:
n∑
i=1
c = nc (2)
2. n∑
i=1
cxi = c
n∑
i=1
xi (3)
3. For any constant a and b:
n∑
i=1
(axi + byi) = a
n∑
i=1
xi + b
n∑
i=1
yi (4)

#### Page 5

Note that: n∑
i=1
xi
yi
̸=
∑n
i=1 xi∑n
i=1 yi
(5)
and
n∑
i=1
x 2
i ̸=
( n∑
i=1
xi
)2
(6)
The average (mean) can be computed as
x = 1
n
n∑
i=1
xi = x1 + x2 +... + xn
n (7)
and the sum of the deviations from the mean:
n∑
i=1
(xi− x) = 0 (8)

#### Page 6

One result that will be very useful throughout the semester is:
n∑
i=1
(xi− x)2 =
n∑
i=1
x 2
i − n(x)2 (9)
and more generally,
n∑
i=1
(xi−x)(yi−y) =
n∑
i=1
xi(yi−y) =
n∑
i=1
(xi−x)yi =
n∑
i=1
xiyi−n(xy)
(10)

#### Page 7

Linear Functions
Remember that you can describe a straight line in a graph as:
y =β0 +β1x (11)
where β0 is the intercept (where the line crosses the y-axis) and
β1 is the slope of the line.
▶ A change in y is always β1 times a change in x:
∆y =β1∆x (12)
▶ It is simple to extend this to the case with several variables:
y =β0 +β1x1 +β2x2 (13)
∆y =β1∆x1 +β2∆x2 (14)

#### Page 8

Note that:
▶ β0 is still the intercept (when x1=0 and x2 = 0)
▶ If x2 is held ﬁxed (∆x2 = 0), the change in y is given by
β1∆x1
▶ β1 is called the partial eﬀect of x1 on y since it measures
how y changes with x1, holding x2 ﬁxed. Hence, the partial
eﬀect is very closely related to the notion of ceteris paribus.
β1 = ∆y
∆x1
(15)
if ∆x2 = 0.

#### Page 9

Expected Value
The expected value of a random variable, also called the
expectation and sometimes the population mean is simply the
weighted average of the possible values that variable can take, with
the weights being given by the probability of each value
”happening” in the population.
▶ Suppose the variable X can take values x1, x2,..., xk with
probability f (x1), f (x2),..., f (xk), respectively.
E (X ) = x1f (x1) +x2f (x2) +... + xkf (xk) =
k∑
j=1
xjf (xj) (16)

#### Page 10

For Example: if X takes on values −1, 0, and 2 with probabilities
0.3, 0.3, and 0.4, respectively,
E (X ) = ( −1)(0.3) + (0)(0.3) + (2)(0.4)
= 0 .5 (17)
In fact you could take the expectation of a function of that
variable, say X 2. Note that X 2 take only the values 0, 1 and 4
with probabilities 0.3, 0.3, and 0.4,
E (X 2) = ( −1)2(0.3) + (0)2(0.3) + (2)2(0.4)
= 0 .3 + 0 + 1.6
= 1 .9 (18)

#### Page 11

Properties of Expected Values
1. For any constant c, E (c) = c
2. For any constants a and b,
E (aX + b) = E (aX ) + E (b) = aE (X ) + b
3. If we have many constants, a1, a2,..., an, and many random
variables, X1, X2,..., Xn, then
E (a1X1+a2X2+... +anXn) = a1E (X1)+a2E (X2)+... +anE (Xn)
(19)
Using summation notation,
E
( n∑
i=1
aiXi
)
=
n∑
i=1
aiE (Xi) (20)
In the special case in which each ai = 1:
E
( n∑
i=1
Xi
)
=
n∑
i=1
E (Xi) (21)

#### Page 12

Outline
Probability and statistics review
Deﬁnition of the simple regression model
Deriving the Ordinary Least Squares Estimates
Properties of OLS on any Sample of Data
Units of Measurement and Functional Form
Expected Values and Variance of OLS

#### Page 13

OLS review
▶ We begin with cross-sectional analysis and will (eventually)
assume we can collect a random sample from the population
of interest.
▶ We begin with the following premise, once we have a
population in mind. There are two variables, x and y, and we
would like to “study how y varies with changes in x”.
▶ We have seen examples: x is amount of fertilizer, and y is
soybean yield. x is years of schooling, y is hourly wage.

#### Page 14

We must confront three issues:
1. How do we allow factors other than x to aﬀect y? There is
never an exact relationship between two variables (in
interesting cases).
2. What is the functional relationship between y and x?
3. How can we be sure we are capturing a ceteris paribus
relationship between y and x (as is so often the goal)?

#### Page 15

Consider the following equation relating y to x:
y =β0 +β1x + u, (22)
which is assumed to hold in the population of interest .
This equation deﬁnes the simple linear regression model (or
two-variable regression model).
▶ y and x are not treated symmetrically. We want to explain y
in terms of x. From a causality standpoint, it makes no sense
to “explain” past educational attainment in terms of future
labor earnings. We want to explain student performance ( y)
in terms of class size ( x).

#### Page 16

Terminology
y x
Dependent Variable Independent Variable
Explained Variable Explanatory Variable
Response Variable Control Variable
Predicted Variable Predictor Variable
Regressand Regressor
The terms “explained” and “explanatory” are probably best, as
they are the most descriptive and widely applicable. But
“dependent” and “independent” are used often. (The
“independence” here is not really statistical independence.)

#### Page 17

We mentioned the error term or disturbance, u, before. The
equation
y =β0 +β1x + u, (23)
explicitly allows for other factors, contained in u, to aﬀect y.
▶ This equation also addresses the functional form issue (in a
simple way). Namely, y is assumed to be linearly related to x.
We call β0 the intercept parameter and β1 the slope
parameter. These describe a population, and our ultimate
goal is to estimate them.
▶ In the simple regression framework, we eﬀectively treat all
factors aﬀecting y,other than x as being unobserved.

#### Page 18

The equation also addresses the ceteris paribus issue. In
y =β0 +β1x + u, (24)
all other factors that aﬀect y are in u. We want to know how y
changes when x changes, holding u ﬁxed . Let ∆ denote “change”.
Then holding u ﬁxed means ∆ u = 0. So
∆y =β1∆x + ∆u =β1∆x when ∆u = 0 (25)
This equation eﬀectively deﬁnes β1 as a slope, with the only
diﬀerence being the (very important) caveat ∆ u = 0.

#### Page 19

EXAMPLE: Yield and Fertilizer
yield =β0 +β1fertilizer + u (26)
where u contains things such as land quality, rainfall on a plot of
land, and so on. The slope parameter, β1, is of primary interest: it
tells us how yield will change when the amount of fertilizer
changes, holding all else ﬁxed.
Note: The linear function is probably not realistic here. The eﬀect
of fertilizer is likely to diminish at large amounts of fertilizer.

#### Page 20

EXAMPLE: Wage and Education
wage =β0 +β1educ + u (27)
where u contains somewhat nebulous factors (“ability”) but, also,
past workforce experience and tenure on the current job.
∆wage =β1∆educ when ∆u = 0 (28)
Is each year of education really worth the same dollar amount no
matter how much education one starts with?
▶ The linear form of the equation being used implies that a
one-unit change in x has the same impact on y regardless of
the initial value of x. That is not very realistic in several
applied cases.

#### Page 21

▶ This seems too easy! How can we hope to generally estimate
the ceteris paribus eﬀect of y on x when we have assumed all
other factors aﬀecting y are unobserved and lumped into u?
▶ The key is that the SLR model is a population model. When
it comes to estimating β1 (and β0) using a random sample of
data, we must restrict how u and x are related to each other.
▶ But x and u are properly viewed as having distributions in the
population. For example, if x = educ then, in principle, we
could ﬁgure out its distribution in the population just over,
say, 30 years old. Suppose u is cognitive ability. Assuming we
can measure what that means, it also has a distribution in the
population.
▶ What we must do is restrict the way u and x relate to each
other in the population.

#### Page 22

First, we make a simplifying assumption (without loss of
generality): the average, or expected, value of u is zero in the
population:
E (u) = 0 (29)
where E (·) is the expected value operator.
Normalizing land quality or “ability” to be zero in the population is
harmless.

#### Page 23

The presence of β0 in
y =β0 +β1x + u (30)
allows us to assume E (u) = 0. If the average of u is diﬀerent from
zero, say α0, we just adjust the intercept, leaving the slope the
same:
y = (β0 +α0) +β1x + (u−α0) (31)
where α0 = E (u). The new error is u−α0 and the new intercept
is β0 +α0. The important point is that the slope, β1, has not
changed.

#### Page 24

▶ The expectation operator, E (·), is a population concept, i.e.,
it refers to the whole group of interest, not only the sample
we have available to us.
▶ Its intuition is (loosely) similar to the average of a random
variable, in the population.
▶ Useful facts about the expectation operator for W and H any
random variables: Note that E (W ) is constant.
E (c) = c for any constant c (32)
E (aW + b) = aE (W ) + b for any constants a, b. (33)
E (W + H) = E (W ) + E (H) (34)
E (W− E (W )) = 0 (35)

#### Page 25

As you have seen before, we use the variance of a random variable,
say W as a measure of its dispersion.
Var(W ) =σ2 = E
[
(W− E (W ))2]
in the population (36)
Var(aW + b) = a2Var(W ) for any constants a, b. (37)
We can show that:
Var(W ) = E
(
W 2)
− [E (W )]2 (38)
In a given sample, we estimate the variance by:
S 2 = (n− 1)−1
n∑
i=1
(xi− x)2 (39)

#### Page 26

Usually, we are interested in how two variables are related to each
other. One important measure of the (linear) connection between
them is given by the covariance.
▶ In the population the covariance between X and Y is given by:
Cov(X, Y ) = E (XY )− E (X )E (Y ) (40)
▶ Then, if E (X ) or E (Y ) equals zero, Cov(X, Y ) = E (XY ).
▶ If X and Y are independent Cov(X, Y ) = 0, also,
Cov(aX + b, cY + d) = acCov(X, Y )

#### Page 27

▶ The covariance measures the amount of linear dependence
between two random variables. Cov(X, Y )> 0 indicates that
the random variables move in the same direction.
Cov(X, Y )< 0 that they move in opposite directions.
▶ Zero covariance does NOT imply that the variables are not
related (there could be a nonlinear connection between them)
▶ Interpreting the magnitude of the covariance can be tricky.
For that, we are better served by looking at correlation:
Corr(X, Y ) = Cov(X, Y )√
Var(X )Var(Y )
(41)
▶ The Correlation coeﬃcient is between −1 and 1. A positive
(negative) correlation indicates that the variables move in the
same (opposite) ways. The closer to 1 or −1 the strongest
the linear relationship is.

#### Page 28

An assumption that meshes well with our introductory treatment
involves the mean of the error term for each “slice” of the
population determined by values of x:
E (u|x) = E (u), all values x (42)
where E (u|x) means “the expected value of u given x”.
Then, we say u is mean independent of x.

#### Page 29

▶ Suppose u is “ability” and x is years of education. We need,
for example,
E (ability|x = 8) = E (ability|x = 12) = E (ability|x = 16)
so that the average ability is the same in the diﬀerent portions
of the population with an 8 th grade education, a 12 th grade
education, and a four-year college education.
Because people choose education levels partly based on
ability, this assumption is almost certainly false.
▶ Suppose u is “land quality” and x is fertilizer amount. Then
E (u|x) = E (u) if fertilizer amounts are chosen independently
of quality.

#### Page 30

Combining E (u|x) = E (u) (the substantive assumption) with
E (u) = 0 (a normalization) gives
E (u|x) = 0, all values x (43)
Called the zero conditional mean assumption . Because the
conditional expected value is a linear operator, E (u|x) = 0 implies
E (y|x) =β0 +β1x (44)
which shows the population regression function (or population
expectation function) is a linear function of x.

#### Page 31

EXAMPLE: College versus High School GPA.
Consider the population of students attending a university. We
assume β0 = 1.5 and β1 = 0.5 are known (which is unrealistic).
E (colGPA|hsGPA) = 1.5 + 0.5 hsGPA (45)
If hsGPA = 3.6 then the average colGPA among students with this
particular high school GPA is
1.5 + 0.5(3.6) = 3.3 (46)
▶ Anyone with hsGPA = 3.6 most likely will not have colGPA
= 3.3. This is the expected college GPA based on averages
within that slice of the population.
▶ Regression analysis is essentially about explaining eﬀects on
averages.

#### Page 32

Outline
Probability and statistics review
Deﬁnition of the simple regression model
Deriving the Ordinary Least Squares Estimates
Properties of OLS on any Sample of Data
Units of Measurement and Functional Form
Expected Values and Variance of OLS

#### Page 33

Deriving the Ordinary Least Squares Estimates
▶ Given data on x and y, how can we estimate the population
parameters,β0 and β1?
▶ Let{(xi, yi) : i = 1, 2,..., n} be a random sample of size n
(the number of observations) from the population.
▶ Plug any observation into the population equation:
yi =β0 +β1xi + ui (47)
where the i subscript indicates a particular observation.
▶ We observe yi and xi, but not ui (but we know it is there).

#### Page 34

We use the two population restrictions:
E (u) = 0 (48)
Cov(x, u) = 0 (49)
to obtain estimating equations for β0 and β1. We talked about the
ﬁrst condition. The second condition means that x and u are
uncorrelated. Both conditions are implied by
E (u|x) = 0 (50)

#### Page 35

With E (u) = 0, Cov(x, u) = 0 is the same as E (xu) = 0. Next we
plug in for u:
E (y−β0−β1x) = 0 (51)
E [x(y−β0−β1x)] = 0 (52)
These are the two conditions in the population that eﬀectively
determine β0 and β1.

#### Page 36

So we use their sample counterparts (which is a method of
moments approach to estimation):
n−1
n∑
i=1
(yi− ˆβ0− ˆβ1xi) = 0 (53)
n−1
n∑
i=1
xi(yi− ˆβ0− ˆβ1xi) = 0 (54)
where ˆβ0 and ˆβ1 are the estimates from the data.
These are two linear equations in the two unknowns ˆβ0 and ˆβ1.

#### Page 37

Pass the summation operator through the ﬁrst equation:
n−1
n∑
i=1
(yi− ˆβ0− ˆβ1xi) (55)
= n−1
n∑
i=1
yi− n−1
n∑
i=1
ˆβ0− n−1
n∑
i=1
ˆβ1xi (56)
= n−1
n∑
i=1
yi− ˆβ0− ˆβ1
(
n−1
n∑
i=1
xi
)
(57)
= y− ˆβ0− ˆβ1x (58)
We use the standard notation y = n−1 ∑n
i=1 yi for the average of
the n numbers{yi : i = 1, 2,..., n}. For emphasis, we call y a
sample average.

#### Page 38

We have shown that the ﬁrst equation,
n−1
n∑
i=1
(yi− ˆβ0− ˆβ1xi) = 0 (59)
implies
y = ˆβ0 + ˆβ1x (60)
Now, use this equation to write the intercept in terms of the slope:
ˆβ0 = y− ˆβ1x (61)

#### Page 39

Plug this into the second equation (but where we take away the
division by n):
n∑
i=1
xi(yi− ˆβ0− ˆβ1xi) = 0 (62)
so
n∑
i=1
xi[yi− (y− ˆβ1x)− ˆβ1xi] = 0 (63)
Simple algebra gives
n∑
i=1
xi(yi− y) = ˆβ1
[n∑
i=1
xi(xi− x)
]
(64)

#### Page 40

So, the equation to solve is
n∑
i=1
(xi− x)(yi− y) = ˆβ1
[n∑
i=1
(xi− x)2
]
(65)
If ∑n
i=1(xi− x)2̸= 0, we can write
ˆβ1 =
∑n
i=1(xi− x)(yi− y)∑n
i=1(xi− x)2 = Sample Covariance(xi, yi)
Sample Variance(xi) (66)

#### Page 41

▶ The previous formula for ˆβ1 is important. It shows us how to
take the data we have and compute the slope estimate. For
reasons we will see, ˆβ1 is called the ordinary least squares
(OLS) slope estimate.
▶ It can be computed whenever the sample variance of the xi is
not zero, which only rules out the case where each xi has the
same value. The intuition is that the variation in x is what
permits us to identify its impact in y.

#### Page 42

▶ Once we have ˆβ1, we compute ˆβ0 = y− ˆβ1x. This is the OLS
intercept estimate.
▶ These days, we let the computer do the calculations, which
are tedious even if n is small.
▶ Where does the name “ordinary least squares” come from?
For any candidates ˆβ0 and ˆβ1, deﬁne a ﬁtted value for each i
as
ˆyi = ˆβ0 + ˆβ1xi (67)
We have n of these. This is the value we predict for yi given
that x = xi.

#### Page 43

▶ The “mistake” we make is the residual:
ˆui = yi− ˆyi = yi− ˆβ0− ˆβ1xi (68)
Suppose we measure the size of the mistake, for each i, by
squaring it. Then we add them all up:
n∑
i=1
ˆu2
i =
n∑
i=1
(yi− ˆβ0− ˆβ1xi)2 (69)
This is called the sum of squared residuals . Finally, choose
ˆβ0 and ˆβ1 to minimize the sum of squared residuals. Using
calculus (or other arguments), it can be shown that the
solutions are the same we obtained before.

#### Page 44

Once we have the numbers ˆβ0 and ˆβ1 for a given data set, we
write the OLS regression line :
ˆy = ˆβ0 + ˆβ1x (70)
which allows us to predict y for any (sensible) value of x. Also
called the sample regression function .
▶ The intercept, ˆβ0, is the predicted y when x = 0, if x = 0
makes sense.
▶ The slope, ˆβ1, allows us to predict changes in y for any
(reasonable) change in x:
∆ˆy = ˆβ1∆x (71)
If ∆x = 1, so x increases by one unit, ∆ˆy = ˆβ1.

#### Page 45

EXAMPLE: Eﬀects of Education on Hourly Wage (WAGE2.DTA)
▶ wage is in dollars per hour. Data are from 1991. educ is
highest grade completed. Only men.
▶ At ﬁrst the negative intercept is a puzzle: wage is predicted to
be $− 0.90 when educ = 0! But the smallest amount of
education in the sample is educ = 8.
▶ Each additional year of schooling is estimated to be worth
about $0.54 per hour in increased wages.
ˆwage =−0.90 + 0.54 educ
n = 526

#### Page 46

▶ In the Stata output, ˆβ0 =−0.90 is the coeﬃcient labeled
“cons.” ˆβ1 = 0.54 is the coeﬃcient labeled “educ”.
▶ We will learn about the other numbers as we go.
▶ General form of the Stata command:
reg y x
The order of y and x is critical! The ﬁrst variable in the
command is taken to be the dependent variable, while the
subsequent are the independent variables.

#### Page 47

Reminder: When we write
wage =β0 +β1educ + u (72)
in the population, we do not know β0 and β1. ˆβ0 =−0.90 and
ˆβ1 = 0.54 are our estimates from this sample of 526 men. They
may or may not be close to the population values.
The function
ˆwage =−0.90 + 0.54 educ (73)
is the OLS regression line. When educ = 8,
ˆwage =−0.90 + 0.54(8) = $3.42per hour (74)

#### Page 48

EXAMPLE: Eﬀects of Campaign Spending on Voting Outcomes
(VOTE.DTA)
▶ Two-party vote, Congressional elections. voteA is the
percentage of votes received by candidate A; shareA is the
percentage share of campaign spending.
▶ Then, y = voteA, x = shareA.

#### Page 49

ˆvoteA = 26.81 + 0.464 shareA (75)
n = 173 (76)
▶ If shareA = 50, ˆvoteA = 26.81 +.0464(50) = 50.01,or
essentially 50 percent of the votes.
▶ If shareA increases by 10 - which means 10 percentage points
- voteA is predicted to increase by 4.64 percentage points in
votes cast to that candidate.
▶ Is this eﬀect causal? Or, do better candidates spend more
(more contributions?)? As with simple correlation analysis,
these simple regression estimates do not allow us to tell.

#### Page 50

Outline
Probability and statistics review
Deﬁnition of the simple regression model
Deriving the Ordinary Least Squares Estimates
Properties of OLS on any Sample of Data
Units of Measurement and Functional Form
Expected Values and Variance of OLS

#### Page 51

▶ Most residuals will be diﬀerent from zero (they do not lie on
the regression line). Some will be positive, others negative.
▶ Positive residuals indicate that the regression line (and, hence,
the predicted values) underestimate the value of yi, similarly if
ˆui is negative, the line overestimate.

#### Page 52

Algebraic Properties of OLS Statistics
Remeber how we obtained ˆβ0 and ˆβ1. When an intercept is
included, we will have:
n∑
i=1
(yi− ˆβ0− ˆβ1xi) = 0 (80)
The OLS residuals always add up to zero, by construction,
n∑
i=1
ˆui = 0 (81)
Because yi = ˆyi + ˆui by deﬁnition,
n−1
n∑
i=1
yi = n−1
n∑
i=1
ˆyi + n−1
n∑
i=1
ˆui (82)
and so y = ˆy.

#### Page 53

Similarly y the way we obtained our estimates,
n∑
i=1
xi(yi− ˆβ0− ˆβ1xi) = 0 (83)
The sample covariance (and therefore the sample correlation)
between the explanatory variables and the residuals is always zero:
n∑
i=1
xi ˆui = 0 (84)
Because the ˆyi are linear functions of the xi, the ﬁtted values and
residuals are uncorrelated, too:
n∑
i=1
ˆyi ˆui = 0 (85)

#### Page 54

Both properties hold by construction. ˆβ0 and ˆβ1 were chosen to
make them true.
A third property is that the point ( x, y) is always on the OLS
regression line. That is, if we plug in the average for x, we predict
the sample average for y:
y = ˆβ0 + ˆβ1x (86)
Again, we chose the estimates to make this true.

#### Page 55

Goodness-of-Fit
▶ For each observation, write
yi = ˆyi + ˆui (87)
▶ Deﬁne the total (SST), explained (SSE) and residual (SSR)
sum of squares as
SST =
n∑
i=1
(yi− y)2 (88)
SSE =
n∑
i=1
(ˆyi− y)2 (89)
SSR =
n∑
i=1
ˆu2
i (90)

#### Page 56

These are sample variances when divided by n (or n− 1). SST
n−1 is
the sample variance of yi, SSE
n−1 is the sample variance of ˆyi, and
SSR
n−1 is the sample variance of ˆui.
By writing
SST =
n∑
i=1
(yi− y)2 =
n∑
i=1
[(yi− ˆyi) + (ˆyi− y)]2 (91)
=
n∑
i=1
[ˆui + (ˆyi− y)]2 (92)
and using that the ﬁtted values and residuals are uncorrelated, can
show
SST = SSE + SSR (93)

#### Page 57

Assuming SST > 0, we can deﬁne the fraction of the total
variation in yi that is explained by xi (or the OLS regression line) as
R 2 = SSE
SST = 1− SSR
SST (94)
Called the R-squared of the regression.
It can be shown to be equal to the square of the correlation
coeﬃcient between yi and ˆyi. Therefore,
0≤ R 2≤ 1 (95)

#### Page 58

▶ R 2 = 0 means no linear relationship between yi and xi.
R 2 = 1 means a perfect linear relationship.
▶ As R 2 increases, the yi are closer and closer to falling on the
OLS regression line.
▶ Do not want to ﬁxate on R 2. It is a useful summary measure
but does not tell us about causality.

#### Page 59

Outline
Probability and statistics review
Deﬁnition of the simple regression model
Deriving the Ordinary Least Squares Estimates
Properties of OLS on any Sample of Data
Units of Measurement and Functional Form
Expected Values and Variance of OLS

#### Page 60

Units of Measurement and Functional Form
Two important issues need to be understood:
▶ How changing the units of measurement of the dependent
and/or independent variable aﬀects OLS estimates? It is very
important to know how y and x are measured in order to
interpret regression functions.
▶ How to incorporate functional forms usually used in economics
into regression analysis.

#### Page 61

Consider an equation estimated from CEOSAL.DTA, where annual
CEO salary is in thousands of dollars and the return on equity is a
percent:
ˆsalary = 963.191 + 18.501 roe (96)
n = 209, R 2 =.0132 (97)
When roe = 0 (it never is in the data), ˆsalary = 963.191. But
salary is in thousands of dollars, so $963,191.
A one percentage point (unit) increase in roe increases predicted
salary by 18.501, or $18,501.

#### Page 62

What if we measure roe as a decimal, rather than a percent?
Deﬁne
roedec = roe/100 (98)
What will happen to the intercept, slope, and R 2 when we regress
salary on roedec?
Nothing should happen to the intercept: roedec = 0 is the same as
roe = 0. But the slope will increase by 100. The goodness-of-ﬁt
should not change, and it does not.

#### Page 63

The new regression is
ˆsalary = 963.191 + 1, 850.1 roedec (99)
n = 209, R 2 =.0132 (100)
Now a one percentage point change in roe is the same as
∆roedec =.01, and so we get the same eﬀect as before. Note
that,
y =β0 +β1x + u⇒ y =β0 +
(β1
c
)
(cx) + u (101)
then the slope is scaled so to preserve the relationship between
variables.

#### Page 64

What if we measure salary in dollars, rather than thousands of
dollars?
salarydol = 1, 000· salary (102)
Both the intercept and slope get multiplied by 1,000.
ˆsalarydol = 963, 191 + 18, 501 roe (103)
n = 209, R 2 =.0132 (104)
Note that,
y =β0 +β1x + u⇒ cy = cβ0 + (cβ1)x + cu (105)

#### Page 65

Using the Natural Logarithm in Simple Regression
Recall the wage example:
ˆwage =−0.90 + 0.54 educ (106)
n = 526, R 2 =.133 (107)
Might be an okay approximation, but unsatisfying for a couple of
reasons:
▶ The negative intercept is a bit strange (even though the
equation gives sensible predictions for education ranging from
8 to 20).
▶ More importantly, the dollar value of another year of schooling
is constant. So the 16 th year of education is worth the same
as the second. We expect additional years of schooling to be
worth more, in dollar terms, than previous years.

#### Page 66

How can we incorporate an increasing eﬀect? One way is to
postulate a constant percentage eﬀect. We can approximate this
using the natural log (ln, but many uses log).
Let the dependent variable be ln( wage):
ln(wage) =β0 +β1educ + u (108)
Holding u ﬁxed,
∆ ln(wage) =β1∆educ (109)
so
β1 = ∆ ln(wage)
∆educ (110)

#### Page 67

Useful result from calculus:
100· ∆ ln(wage)≈ %∆wage (111)
This means when ln( wage) =β0 +β1educ + u, we have a simple
interpretation of β1:
100β1≈ %∆wage when ∆educ = 1 (112)
If we do not multiply by 100, we have the decimal version (the
proportionate change).
In this example, 100β1 is often called the return to education (just
like an investment). This measure is free of units of measurement
(currency, price level).

#### Page 68

▶ We still want to explain wage in terms of educ! This gives us
a way to get a (rough) constant percentage eﬀect.
▶ Predicting wage is more complicated but not usually the
interesting question.
Using WAGE2.DTA. Have to deﬁne ln( wage) in general.
ˆln(wage) = 0.58 +.082 educ (113)
n = 526, R 2 =.1858 (114)
The estimated return to each year of education is about 8 .2%.
Warning: This R-squared is not directly comparable to the
R-squared when wage is the dependent variable.

#### Page 69

We can use the ln on both sides of the equation to get constant
elasticity models. For example, if
ln(salary) =β0 +β1 ln(sales) + u (115)
then,
β1≈ %∆salary
%∆sales (116)
The elasticity is free of units of salary and sales.
A constant elasticity model for salary and sales makes more sense
than a constant dollar eﬀect.

#### Page 70

▶ The estimated elasticity of CEO salary with respect to ﬁrms
sales is about 0.257. (Note that we do not need to even know
salary is in thousands of dollars while sales is in millions.)
▶ A 10 percent increase in sales is associated with (causes?)
about a
0.257(10) = 2.57 (117)
percent increase in salary.

#### Page 71

Model Dep. Var. Indep. Var. β1
Level-Level y x ∆y =β1∆x
Level-Log y ln(x) ∆y = (β1/100)%∆x
Log-Level ln(y) x %∆y = (100β1)∆x
Log-Log ln(y) ln(x) %∆y =β1%∆x
The possibility of using the natural log to get nonlinear
relationships between y and x raises a question: What do we mean
now by ”linear” regression? The answer is that the model is linear
in the parameters, β0 and β1. We can use any transformations of
the dependent and independent variables to get interesting
interpretations for the parameters.

#### Page 72

Outline
Probability and statistics review
Deﬁnition of the simple regression model
Deriving the Ordinary Least Squares Estimates
Properties of OLS on any Sample of Data
Units of Measurement and Functional Form
Expected Values and Variance of OLS

### RDD.pdf — 60 pages (0 without PDF text)

#### Page 1

Regression Discontinuity Designs
Yumin Li
Shanghai University
Yumin Li 1 / 60

#### Page 2

Outline
1 Introduction to Regression Discontinuity Designs
2 Graphical illustration of RD models
3 Local Polynomial Analysis
4 Falsification Analysis for RD designs
Yumin Li Introduction to Regression Discontinuity Designs 2 / 60

#### Page 3

Causal Inference
We are interested in treatment effect of policy or intervention
If treatment randomization available → easy to estimate effects
If treatment randomization not available → observational studies
Selection on observables
DID
Instrumental variables, etc.
Regression discontinuity (RD) design
Simple assignment, based on known external factors
Objective basis to evaluate assumptions
Careful: very local!
Yumin Li Introduction to Regression Discontinuity Designs 3 / 60

#### Page 4

Regression Discontinuity Design
Defined by the triplet: score, cutoff, treatment.
Units receive a score.
A treatment is assigned based on the score and a known cutoff.
The treatment is:
given to units whose score is greater than the cutoff.
withheld from units whose score is less than the cutoff.
Under assumptions, the abrupt change in the probability of treatment assignment
allows us to learn about the effect of the treatment.
Some examples:
Xi Yi
Education entry test score test score, enrollment, performance, etc
Development poverty index educ, labor, health, etc
Health age / birthdate insurance coverage, mortality, etc.
Yumin Li Introduction to Regression Discontinuity Designs 4 / 60

#### Page 5

Treatment Assignment in (Sharp) RD Design
Yumin Li Introduction to Regression Discontinuity Designs 5 / 60

#### Page 6

Sharp Regression Discontinuity Design
n units, indexed by i = 1, 2, ..., n
Unit’s score (or running variable) is Xi, treatment is Ti = 1(Xi ≥ c)
Each unit has two potential outcomes:
Yi(1): outcome that would be observed if i received treatment
Yi(0): outcome that would be observed if i received control
The observed outcome is
Yi =
(
Yi(1) if Xi ≥ c
Yi(0) if Xi < c
Fundamental problem of causal inference: only observe Yi(0) for units below cutoff
and only observe Yi(1) for units above cutoff
Yumin Li Introduction to Regression Discontinuity Designs 6 / 60

#### Page 7

RD Treatment Effect in Sharp RD Design
Yumin Li Introduction to Regression Discontinuity Designs 7 / 60

#### Page 8

Fundamental Missing Data Problem
A special situation occurs at the cutoff X = c, the only point at which we may
“almost” observe both curves
Imagine two groups of units:
with score equal to c, Xi = c → treated
with with score barely below c, Xi = c − ε → control
Yet if values of the average potential outcomes at c are not abruptly different from
their values at points near c, these two sets of units would be identical except for
their treatment status
Vertical distance at c: the average treatment effect at this point
This is the feature on which all RD designs are based
Yumin Li Introduction to Regression Discontinuity Designs 8 / 60

#### Page 9

τSRD = E[Yi(1) − Yi(0) | Xi = c]
Yumin Li Introduction to Regression Discontinuity Designs 9 / 60

#### Page 10

Continuity Conditions
τSRD = E[Yi(1) − Yi(0) | Xi = c]| {z }
Unobservable
= lim
x↓c
E[Yi | Xi = x]
| {z }
Observable
− lim
x↑c
E[Yi | Xi = x]
| {z }
Observable
Yumin Li Introduction to Regression Discontinuity Designs 10 / 60

#### Page 11

Sharp RD design: Summary
Canonical Parameter:
τSRD = E[Yi(1) − Yi(0) | Xi = c] = lim
x↓c
E[Yi | Xi = x] − lim
x↑c
E[Yi | Xi = x]
Perfect compliance:
every unit with score above c receives treatment.
every unit with score below c receives control.
Yumin Li Introduction to Regression Discontinuity Designs 11 / 60

#### Page 12

Local Nature of RD Effects
RD parameters can be interpreted as causal in the sense that they are based on a
comparison of potential outcomes— Yi(1) and Yi(0).
But, in contrast to other parameters, average treatment effect is calculated at a
single point on support of continuous random variable ( Xi).
This results in RD treatment effects having limited external validity:
τSRD, the average treatment effect at c, may not be informative about treatment
effect at values of Xi ̸= c.
Absent specific assumptions about global shape of regression functions, RD effects
are average treatment effects local to the cutoff.
How much can be learned from such local treatment effects will depend on each
particular application.
Yumin Li Introduction to Regression Discontinuity Designs 12 / 60

#### Page 13

The RD Parameter: No Heterogeneity
Yumin Li Introduction to Regression Discontinuity Designs 13 / 60

#### Page 14

The RD Parameter: Mild Heterogeneity
Yumin Li Introduction to Regression Discontinuity Designs 14 / 60

#### Page 15

The RD Parameter: Wild Heterogeneity
Yumin Li Introduction to Regression Discontinuity Designs 15 / 60

#### Page 16

Outline
1 Introduction to Regression Discontinuity Designs
2 Graphical illustration of RD models
3 Local Polynomial Analysis
4 Falsification Analysis for RD designs
Yumin Li Graphical illustration of RD models 16 / 60

#### Page 17

RD Packages
https://rdpackages.github.io/
rdrobust: estimation, inference and graphical presentation using local polynomials,
partitioning, and spacings estimators; bandwidth selection.
rdlocrand package: covariate balance, binomial tests, randomization inference
methods (window selection & inference).
rddensity: discontinuity in density test at cutoff (a.k.a. manipulation testing) using
novel local polynomial density estimator.
rdmulti: RD plots, estimation, inference, and extrapolation with multiple cutoffs
and multiple scores.
rdpower: power calculation and sample selection for local polynomial methods.
Yumin Li Graphical illustration of RD models 17 / 60

#### Page 18

Graphical illustration of RD models
Appealing feature of RDD: it can be illustrated graphically.
Combined with formal approaches to estimation and inference, adds transparency
to the analysis.
Scatter plot: limited effectiveness for visualizing RD design.
Usually useful to aggregate or smooth the data before plotting.
Yumin Li Graphical illustration of RD models 18 / 60

#### Page 19

Example: Meyersson (2014)
Meyersson (2014) employs a Sharp RD design, based on close elections in Turkey,
to study the impact of having a mayor from an Islamic party on various outcomes.
What is the effect of Islamic parties’ control of local governments on women’s
rights, in particular on the educational attain- ment of young women?
Can we just compare the municipalities controlled by an Islamic versus a secular
mayor?
Yumin Li Graphical illustration of RD models 19 / 60

#### Page 20

Raw scatter plot
Yumin Li Graphical illustration of RD models 20 / 60

#### Page 21

Islamic vs Secular
Yumin Li Graphical illustration of RD models 21 / 60

#### Page 22

Graphical illustration of RD models
In (a), raw comparison of the overall sample mean for each group reveals a
negative average difference
In (b), the scatter plot for the subset of municipalities where the Islamic margin of
victory is within 50 percentage points restricts the range to include 83% of the
total observations. Use a fourth-order polynomial fit separately on either side of the
cutoff.
There is an overall negative relationship between Islamic vote share and educational
attainment of women.
A comparison of citeis where the Islamic party barely wins the election versus cities
where the Islamic party barely loses will reveal the causal (local) effect of Islamic
party control of the local government on the educational attainment of women.
Yumin Li Graphical illustration of RD models 22 / 60

#### Page 23

Graphical illustration of RD models
Typical RD plot:
global polynomial fit: smooth approximation to the unknown regression functions;
4th or 5th order polynomials, separately above and below the cutoff.
local sample means: disjoint intervals (bins) of the score, calculating the mean of
the outcome within each bin.
Combination of (i) and (ii) allows for:
visualize the overall shape of the regression functions for T and C
retain information about local behavior of the data
Yumin Li Graphical illustration of RD models 23 / 60

#### Page 24

Using 40 Bins of Equal Length
In Stata: rdplot Y X, nbins(20 20) binselect(es) graph options(graphregion(color(white))
xtitle(Score) ytitle(Outcome))
Yumin Li Graphical illustration of RD models 24 / 60

#### Page 25

Using 40 Bins of Equal Obs
In Stata: rdplot Y X, nbins(20 20) binselect(qs) graph options(graphregion(color(white))
xtitle(Score) ytitle(Outcome))
Yumin Li Graphical illustration of RD models 25 / 60

#### Page 26

Graphical illustration of RD models
Two types of bins:
Evenly-spaced: bins that have equal length
Quantile-spaced: bins contain the same number of observations
How to choose the number of bins optimally:
Tracing out the regression function: Integrated Mean Squared Error (IMSE)
Method (balances bias and variance)
Mimicking Variance
Yumin Li Graphical illustration of RD models 26 / 60

#### Page 27

IMSE Method
Yumin Li Graphical illustration of RD models 27 / 60

#### Page 28

IMSE Method
rdplot Y X, binselect(es) graph options(graphregion(color(white)) xtitle(Score)
ytitle(Outcome))
Can produce an RD plot that uses an IMSE-optimal number of quantile-spaced bins if set
binselect = ” qs”
Yumin Li Graphical illustration of RD models 28 / 60

#### Page 29

Mimicking Variance Method
It selects the number of bins so that the binned means have an asymptotic variability
that is about equal to the variability of the raw data.
It leads to more bins than IMSE, thus giving a better sense of the variability of the data.
rdplot Y X, binselect(esmv) graph options(graphregion(color(white)) xtitle(Score)
ytitle(Outcome))
Yumin Li Graphical illustration of RD models 29 / 60

#### Page 30

Graphical illustration of RD models
Start with MV bins to better illustrate the variability of the outcome as a function
of the score, ideally comparing ES to QS bins to highlight the distributional
features of the score.
Then select the number of bins to be IMSE-optimal to explore the global features
of the regression function
Yumin Li Graphical illustration of RD models 30 / 60

#### Page 31

Outline
1 Introduction to Regression Discontinuity Designs
2 Graphical illustration of RD models
3 Local Polynomial Analysis
4 Falsification Analysis for RD designs
Yumin Li Local Polynomial Analysis 31 / 60

#### Page 32

Standard RD: Treatment Effect Estimation
E[Yi | Xi = x] approximated in neighborhood of x0 by polynomial function, so the
fit is local, not global. Then compute the estimated treatment effect and/or the
statistical inference procedure.
This global approach is still used in RD plots. However, a global polynomial
approach does not deliver point estimators and inference procedures with good
properties for the RD treatment effect.
Global polynomial approximations tend to deliver a poor fit at boundary points –
Runge’s phenomenon in approximation theory.
Since the RD point estimator is defined at a boundary point, a global parametric
RD analysis can be highly misleading.
Yumin Li Local Polynomial Analysis 32 / 60

#### Page 33

Continuity Conditions
τSRD = E[Yi(1) − Yi(0) | Xi = c]| {z }
Unobservable
= lim
x↓c
E[Yi | Xi = x]
| {z }
Observable
− lim
x↑c
E[Yi | Xi = x]
| {z }
Observable
Yumin Li Local Polynomial Analysis 33 / 60

#### Page 34

Local Polynomial Estimation
Yumin Li Local Polynomial Analysis 34 / 60

#### Page 35

Local Polynomial Estimation
Yumin Li Local Polynomial Analysis 35 / 60

#### Page 36

Local Polynomial Estimation
Yumin Li Local Polynomial Analysis 36 / 60

#### Page 37

RD Local Polynomial Estimation and Inference
Local lower-order polynomial estimation is are less sensitive to outliers or data far
from the cutoff:
Choose order of polynomial p: p = 1
Choose bandwidth h to keep observations in [ x0 − h, x0 + h]: MSE (mean squared
error) -optimal or CER (coverage error) -optimal
Choose kernel function to weigh observations, wi = K( x−x0
h ): a weighting scheme to
ensure that the observations closer to c receive more weight than those further away
Fit a weighted least-squares regression of Yi on a constant and ( Xi − c), ...,
(Xi − c)p for both sides of the cutoff.
Construct point estimator: bτn = ˆµ+ − ˆµ−
Yumin Li Local Polynomial Analysis 37 / 60

#### Page 38

Choice of Kernel Weights
The estimation and inference results are typically not very sensitive to the choice of
kernel.
Yumin Li Local Polynomial Analysis 38 / 60

#### Page 39

Choice of Polynomial Order ( p)
The higher p, the more flexible the approximation
However, since approximation is local, p should be low to avoid overfitting
Given p, approximation can be improved by focusing on a smaller neighborhood
around the cutoff
Standard practice is to choose p = 1 ( “local linear” )
Yumin Li Local Polynomial Analysis 39 / 60

#### Page 40

Bandwidth Selection
A linear polynomial seems not flexible.
An appropriately chosen bandwidth will adjust to the chosen p so that the linear
approximation to the unknown regression functions is reliable.
The accuracy of the approximation can always be improved by reducing the
bandwidth.
Yumin Li Local Polynomial Analysis 40 / 60

#### Page 41

Approximation for fixed p = 1
Yumin Li Local Polynomial Analysis 41 / 60

#### Page 42

Approximation for fixed p = 1
Yumin Li Local Polynomial Analysis 42 / 60

#### Page 43

Bandwidth Selection
Choosing a smaller h will reduce the misspecification error (also known as
“smoothing bias” ), but will tend to increase the variance of the coefficients because
fewer observations will be available.
The choice of bandwidth involves a “bias-variance trade-off.”
Since RD results are often sensitive to the choice of h, it is important to select h in
a data-driven, automatic way to avoid specification searching and ad hoc decisions.
The most popular approach minimizes the mean squared error of the local RD point
estimator, bτSRD. In the context of MSE-optimal h, triangular kernel is optimal
Yumin Li Local Polynomial Analysis 43 / 60

#### Page 44

Conventional Local Polynomial Point Estimation
“Local-linear” estimator (w/ weights K( · )):
Yi = α0 + (Xi − c)β0 + ε0,i for −hn ≤ Xi < c
Yi = α1 + (Xi − c)β1 + ε1,i for c ≤ Xi ≤ hn
RD effect: bτn = ˆα1 − ˆα0
When choosing MSE-optimal h, this point estimator bτn is optimal (also consistent)
Yumin Li Local Polynomial Analysis 44 / 60

#### Page 45

Meyersson application
We start by choosing an ad hoc bandwidth h = 20. ( bτn = 2.9271)
The same point estimator can be obtained by fitting a single linear regression that
includes an interaction between the treatment indicator and the score.
Then we estimate the same point estimation with a triangular kernel.( bτn = 2.9373)
The the confidence intervals and standard errors of local polynomial point
estimation are generally invalid
rdrobust: tailored to RD designs and includes several functions to conduct local
polynomial bandwidth selection, RD point estimation, and RD inference using a
fully non-parametric and internally coherent methodology.
Yumin Li Local Polynomial Analysis 45 / 60

#### Page 46

Robust Local Polynomial Inference
Our bandwidth selection addresses bias-variance trade-off.
Valid inference should take into account the bias in specification.
Two approaches:
Use the bandwidth hMSE for both estimation and inference, but modify the usual
t-statistic to account for the effects of misspecification due to the large bandwidth,
as well as for the additional sampling error introduced by such modification.
Use hMSE only for point estimation, and then choose a different bandwidth for
inference purposes.
rdrobust command reports both conventional and robust bias-corrected inference.
Yumin Li Local Polynomial Analysis 46 / 60

#### Page 47

Adding Covariates to the Analysis
You can implement RD local polynomial analysis to fit the outcome on the score
alone. Some may want to include other covariates.
If covariates differ systematically at the cutoff, the assumption of continuity of the
potential outcomes is implausible.
Two natural approaches:
conditioning or subsetting, which makes the most sense when only a few discrete
covariates are used
partialling out via local polynomial methods: weighted least squares regression of Yi
on a constant, Ti, (Xi − c), ..., ( Xi − c)p and interaction terms Ti(Xi − c)p.
When we have covariate balance, the covariate-adjusted estimator estimates the
same parameter as the unadjusted estimator. Valid justification for including
covariates in RD analysis is the potential for efficiency gains (shorter confidence
intervals).
Yumin Li Local Polynomial Analysis 47 / 60

#### Page 48

Outline
1 Introduction to Regression Discontinuity Designs
2 Graphical illustration of RD models
3 Local Polynomial Analysis
4 Falsification Analysis for RD designs
Yumin Li Falsification Analysis for RD designs 48 / 60

#### Page 49

Falsification Methods
Falsification Analysis in empirical research is testing the validity of a research
design. It involves conducting tests in settings where no effect should theoretically
exist, thereby ” falsifying” the research design if an effect is found.
RD rule of treatment assignment is not by itself enough to guarantee that
continuity or local randomizations are met
Scholarship based on exam scores, we are interested on the effect of scholarship.
Qualitative information and quantitative falsification tests play crucial role
Qualitative information: were there mechanisms to appeal score? did people change
their score? Administrative process info is useful to validate the design.
Falsification: various statistical tests
Yumin Li Falsification Analysis for RD designs 49 / 60

#### Page 50

Falsification Methods
Density test of “sorting” : is number of observations below the cutoff surprisingly
different from number of observations above it?
Treatment effect on
Predetermined covariates (variables that are determined before the treatment is
assigned)
Placebo outcomes (variables that are determined after the treatment is assigned,
but are not affected by the treatment): e.g. T: access to clean water; Y: mortality
due to water-borne illnesses. Placebo outcome: mortality from road accidents.
Also: effect at different cutoffs, effect at different bandwidths, dougnout hole
Yumin Li Falsification Analysis for RD designs 50 / 60

#### Page 51

Density Test
If units do not have the ability to precisely manipulate the value of the score that
they receive, the number of treated observations just above the cutoff should be
similar to the number of control. You should show a histogram of the running
variable.
Yumin Li Falsification Analysis for RD designs 51 / 60

#### Page 52

Density Test
A more formal test would choose a small neighborhood around the cutoff, and
perform a Bernoulli test within that neighborhood with a probability of “success”
equal to 1/2
In Meyersson application, if we choose Xi ∈ [−2, 2], there are 47 control and 53
treated observations.
In STATA: bitesti 100 53 1/2
The p-value is 0.6173, so no evidence of “sorting” .
You should conduct this test for different neighborhoods around the cutoff.
Yumin Li Falsification Analysis for RD designs 52 / 60

#### Page 53

Density Test
A complementary approach is to conduct a test of the null hypothesis that the
density of the running variable is continuous at the cutoff.
This test requires the estimation of the density of observations near the cutoff,
separately for observations above and below the cutoff.
In STATA: rddensity X
The value of the statistic is = 1.394 and the associated p-value is 0.1633. We fail
to reject the null hypothesis of no difference in the density of treated and control.
Yumin Li Falsification Analysis for RD designs 53 / 60

#### Page 54

Density Test
Yumin Li Falsification Analysis for RD designs 54 / 60

#### Page 55

Tests on Predetermined Covariates and Placebo Outcomes
Continuity-based falsification:
Local polynomial effects with optimal banwidth
Robust Inference
CRUCIAL: each covariate/placebo outcome must have its own optimal bandwidth
Yumin Li Falsification Analysis for RD designs 55 / 60

#### Page 56

Placebo Cutoffs
Examines treatment effects at artificial cutoff values.
The presence of discontinuities away from the cutoff can potentially cast doubt on
the RD design.
This test replaces the true cutoff value by another value at which the treatment
status does not really change and performs estimation and inference using this
artificial cutoff point.
To avoid “contamination” due to real treatment effects, for artificial cutoffs above
the real cutoff we should use only treated units, and for artificial cutoffs below the
real cutoff we use only control observations.
Report the results for several placebo cutoffs.
Yumin Li Falsification Analysis for RD designs 56 / 60

#### Page 57

Placebo Cutoffs
Yumin Li Falsification Analysis for RD designs 57 / 60

#### Page 58

Sensitivity to Observations near the Cutoff
This analysis investigates how sensitive the results are to the response of units who
are located very close to the cutoff (most likely to have engaged in manipulation)
A “donut hole” approach: exclude such units and then repeat the estimation and
inference analysis using the remaining sample
In STATA: rdrobust Y X if abs(X) >= 0.3
It is natural to repeat this exercise a few times to assess the actual sensitivity for
different amounts of excluded units.
Yumin Li Falsification Analysis for RD designs 58 / 60

#### Page 59

Sensitivity to Bandwidth Choice
As the bandwidth increases, the bias of the local polynomial estimator increases
and its variance decreases. The confidence intervals will decrease in length but will
also be displaced because of the bias.
This test is only useful over small ranges around the MSE-optimal bandwidth as
bandwidths much larger than the MSE-optimal bandwidth will have too much bias,
and bandwidths much smaller than the MSE-optimal choice will have too much
variance.
We can do this test for some bandwidth choices close to the hMSE and hCER, i.e.,
hMSE , hCER, 2hMSE , and 2 hCER.
Yumin Li Falsification Analysis for RD designs 59 / 60

#### Page 60

Sensitivity to Bandwidth Choice
Yumin Li Falsification Analysis for RD designs 60 / 60

### iv.pdf — 87 pages (2 without PDF text)

#### Page 1

Instrumental Variables (IV)
Yumin Li
Shanghai University

#### Page 2

Outline
Regression Review
Models vs. Estimands vs. Estimators
Regression Identification and Endogeneity
Introduction to IV
Instrument Validity and Relevance
The 2SLS Estimator
Where do (Good) Instruments Come From?
True Lotteries
Natural Experiments
Panel Data
2SLS Mechanics
Just-Identified IV
Overidentification
Weak and Many Instruments
Weak IVs
Many IVs

#### Page 3

Models vs. Estimands vs. Estimators
Three distinct objects (though not always clearly distinguished)
▶ Parameters come from models of how observed data are generated
▶ E.g. a structural supply/demand elasticity, an ATE, or a CEF
▶ They set the target for an empirical analysis: what we want to
know
▶ Estimands are functions of the population data distribution
▶ E.g. a difference in means or ratio of population regression
coef’s
▶ Make assumptions to link parameters & estimands
(“identification”)
▶ Estimators are functions of the observed data itself (the “sample”)
▶ E.g. a difference in sample means or ratio of OLS coefficients
▶ Since data are random, so are estimators. Each has a
distribution
▶ Use knowledge of estimator distributions to learn about
estimands (“inference”) and—hopefully—identified parameters

#### Page 4

The Lay of the Land
We will mostly focus on identification, but we’ll cover some IV
estimation / inference issues as well

#### Page 5

Let’s Get Specific
Human capital theory (e.g. Becker, 1957) tells us that taking
applied metrics are likely to boost productivity
▶ Parameter: returns to taking this class β, measured in some
outcome Yi (e.g. lifetime pubs / earnings / weibo followers)
▶ Simple causal/structural model: Yi = α + βDi + εi, where
Di ∈ {0, 1} indicates taking this class
We see a sample of Yi, Di, and some other covariates
W1i , . . . ,WKi
▶ We fire up Stata and type “reg y d w*, r”
▶ How do we interpret the output?

#### Page 6

Let’s Get Specific
Human capital theory (e.g. Becker, 1957) tells us that taking
applied metrics are likely to boost productivity
▶ Parameter: returns to taking this class β, measured in some
outcome Yi (e.g. lifetime pubs / earnings / weibo followers)
▶ Simple causal/structural model: Yi = α + βDi + εi, where
Di ∈ {0, 1} indicates taking this class
We see a sample of Yi, Di, and some other covariates
W1i , . . . ,WKi
▶ We fire up Stata and type “reg y d w*, r”
▶ How do we interpret the output?

#### Page 7

Let’s Get Specific
Human capital theory (e.g. Becker, 1957) tells us that taking
applied metrics are likely to boost productivity
▶ Parameter: returns to taking this class β, measured in some
outcome Yi (e.g. lifetime pubs / earnings / weibo followers)
▶ Simple causal/structural model: Yi = α + βDi + εi, where
Di ∈ {0, 1} indicates taking this class
We see a sample of Yi, Di, and some other covariates
W1i , . . . ,WKi
▶ We fire up Stata and type “reg y d w*, r”
▶ How do we interpret the output?

#### Page 8

Population Regression
The OLS estimator bβOLS consistently estimates the regression
estimand βOLS under some conditions (e.g. i.i.d. data)
▶ Stata tells us bβOLS and what we can infer about βOLS from it
▶ It doesn’t directly tell us about the relationship between βOLS
and β

#### Page 9

Population Regression
The population regression of Yi on Xi = [1, Di , W1i , . . . ,WKi]′ is
given by Yi = X′
i βOLS + Ui where E [Xi Ui] = 0
▶ Equivalently, βOLS = E [Xi X′
i]−1E [Xi Yi] and
Ui = Yi − X′
i βOLS
▶ βOLS contains regression coefficients; Ui is the regression
residual
Key point: we can always define βOLS for any Yi and Xi (assuming
no perfect collinearity); this is what Stata estimates
▶ Specifically it computes bβOLS = ( 1
N
P
i Xi X′
i)−1( 1
N
P
i Xi Yi)
and uses large-sample asymptotics (LLN/CLT) to get a
standard error

#### Page 10

Population Regression
The population regression of Yi on Xi = [1, Di , W1i , . . . ,WKi]′ is
given by Yi = X′
i βOLS + Ui where E [Xi Ui] = 0
▶ Equivalently, βOLS = E [Xi X′
i]−1E [Xi Yi] and
Ui = Yi − X′
i βOLS
▶ βOLS contains regression coefficients; Ui is the regression
residual
Key point: we can always define βOLS for any Yi and Xi (assuming
no perfect collinearity); this is what Stata estimates
▶ Specifically it computes bβOLS = ( 1
N
P
i Xi X′
i)−1( 1
N
P
i Xi Yi)
and uses large-sample asymptotics (LLN/CLT) to get a
standard error

#### Page 11

Population Regression
The population regression of Yi on Xi = [1, Di , W1i , . . . ,WKi]′ is
given by Yi = X′
i βOLS + Ui where E [Xi Ui] = 0
▶ Equivalently, βOLS = E [Xi X′
i]−1E [Xi Yi] and
Ui = Yi − X′
i βOLS
▶ βOLS contains regression coefficients; Ui is the regression
residual
Key point: we can always define βOLS for any Yi and Xi (assuming
no perfect collinearity); this is what Stata estimates
▶ Specifically it computes bβOLS = ( 1
N
P
i Xi X′
i)−1( 1
N
P
i Xi Yi)
and uses large-sample asymptotics (LLN/CLT) to get a
standard error

#### Page 12

You Can’t Always Get What you Want...
But what if this estimand is not what we want?
▶ What if βOLS fails to coincide with our economic parameter of
interest (e.g. returns to metric classes)?

#### Page 13

You Can’t Always Get What you Want...
The model parameter in Yi = α + βDi + εi need not coincide with
the regression coefficient in Yi = αOLS + βOLS Di + Ui
▶ I.e. we may not have Cov(Di , εi) = 0 (always have
Cov(Di , Ui) = 0)
Selection bias (a.k.a. omitted variables bias): students with higher
latent earnings potential εi are more likely to take this class Di
▶ Cov(Di , εi) > 0 means βOLS > β : overstate the
returns-to-metric-classes

#### Page 14

Can I just Control My Way Out of This?
Adding more controls (e.g. demographics) may or may not help
▶ Projecting εi on Xi, we get Yi = α + βDi + γXi + ˜εi,
Cov(Xi , ˜εi) = 0
▶ Whether or not Cov(Di , ˜εi) = 0 depends on whether Xi
sufficiently accounts for the confounding relationship with
Cov(Di , εi) ̸= 0

#### Page 15

Regression “Exogeneity”

#### Page 16

Regression “Endogeneity”

#### Page 17

...But Sometimes, You Get What you Need
Imagine this course was “oversubscribed,” and admission was
determined by lottery
▶ Among those interested in taking the course, a random
sample denoted by Zi = 1 was given access
▶ The rest, with Zi = 0 not initially given access (maybe got in
later)
Intuitively, this external shock Zi should be helpful for identifying β
▶ Affects Di, so relevant to the “treatment” of interest
▶ Randomly assigned, so unconfounded by selection (unlike Di)
Indeed, this leads us to IV estimands (and estimators)

#### Page 18

...But Sometimes, You Get What you Need
Imagine this course was “oversubscribed,” and admission was
determined by lottery
▶ Among those interested in taking the course, a random
sample denoted by Zi = 1 was given access
▶ The rest, with Zi = 0 not initially given access (maybe got in
later)
Intuitively, this external shock Zi should be helpful for identifying β
▶ Affects Di, so relevant to the “treatment” of interest
▶ Randomly assigned, so unconfounded by selection (unlike Di)
Indeed, this leads us to IV estimands (and estimators)

#### Page 19

...But Sometimes, You Get What you Need
Imagine this course was “oversubscribed,” and admission was
determined by lottery
▶ Among those interested in taking the course, a random
sample denoted by Zi = 1 was given access
▶ The rest, with Zi = 0 not initially given access (maybe got in
later)
Intuitively, this external shock Zi should be helpful for identifying β
▶ Affects Di, so relevant to the “treatment” of interest
▶ Randomly assigned, so unconfounded by selection (unlike Di)
Indeed, this leads us to IV estimands (and estimators)

#### Page 20

The IV Solution

#### Page 21

The Intuition
▶ The effect of Zi on Yi (ρ) is composed of the effect of Zi on
Di (π) and the effect of Di on Yi (β): ρ = πβ
▶ Therefore, we might be able to find β by estimating ρ
π

#### Page 22

Outline
Regression Review
Models vs. Estimands vs. Estimators
Regression Identification and Endogeneity
Introduction to IV
Instrument Validity and Relevance
The 2SLS Estimator
Where do (Good) Instruments Come From?
True Lotteries
Natural Experiments
Panel Data
2SLS Mechanics
Just-Identified IV
Overidentification
Weak and Many Instruments
Weak IVs
Many IVs

#### Page 23

Instrument Validity and Relevance
Causal/structural model Yi = α + βDi + εi and a candidate IV Zi
▶ Single Di and Zi and no further controls, for now
Two key assumptions:
▶ Relevance: Zi and Di are correlated: Cov(Zi , Di) ̸= 0
(testable)
▶ Validity: Zi and εi are uncorrelated: Cov(Zi , εi) = 0
(untestable, find convincing economic arguments)
We then have identification:
Cov(Zi , Yi) = Cov(Zi , α + βDi + εi) = βCov(Zi , Di) + Cov(Zi , εi)
= βCov(Zi , Di) = ⇒ β = Cov(Zi , Yi)
Cov(Zi , Di)

#### Page 24

The IV Estimand
The (simple) IV estimand is:
βIV = Cov(Zi , Yi)
Cov(Zi , Di)
▶ Compare to the OLS estimand: βOLS = Cov (Di ,Yi )
Var(Di )

#### Page 25

“Reduced Form” and “First Stage”
Note that we can write:
βIV = Cov(Zi , Yi)
Cov(Zi , Di) = Cov(Zi , Yi)/Var(Zi)
Cov(Zi , Di)/Var(Zi) = ρOLS
πOLS
where ρOLS and πOLS are two OLS estimands:
Yi = κOLS + ρOLS Zi + Vi “reduced form”
Di = µOLS + πOLS Zi + Wi “first stage”

#### Page 26

“Reduced Form” and “First Stage”
Note that we can write:
βIV = Cov(Zi , Yi)
Cov(Zi , Di) = Cov(Zi , Yi)/Var(Zi)
Cov(Zi , Di)/Var(Zi) = ρOLS
πOLS
where ρOLS and πOLS are two OLS estimands:
Yi = κOLS + ρOLS Zi + Vi “reduced form”
Di = µOLS + πOLS Zi + Wi “first stage”

#### Page 27

IV estimand as the “Second Stage”
Sometimes we refer to the IV estimand as the “second stage”:
Yi = αIV + βIV Di + Ui
where now Cov(Zi , Ui) = 0. Thus “IV=RF/FS”
(βIV = ρOLS /πOLS)

#### Page 28

The 2SLS Estimator
▶ As with OLS, we estimate IV by sample analog:
bβIV =
dCov(Zi , Yi)
dCov(Zi , Di)
= bρOLS
bπOLS
where ˆρOLS = dCov(Zi , Yi)/dVar(Zi), and
ˆπOLS = dCov(Zi , Di)/dVar(Zi)
▶ This is what Stata does when you type “ivreg2 y (d=z), r”
▶ Standard errors come from the usual large-sample asymptotics
▶ We will soon consider extensions of all of this, with controls /
multiple instruments / etc

#### Page 29

Finkelstein et al. (2012)- the effects of Medicaid
▶ Important question for policy: what is the effect of Medicaid
on health outcomes?
▶ The ideal situation for learning about the causal effects of
Medicaid would be to randomize who gets it
▶ Not possible for moral / budgetary reasons
▶ However, we have the Oregon Health Insurance Experiment
(OHIE) which randomized eligibility for Medicaid for some
people (i.e. those with earnings between 100 and 138% of the
FPL)

#### Page 30

Background about OHIE

#### Page 31

Winning the lottery vs Medicaid
▶ Finkelstein et al. (2012) used the OHIE to estimate the causal
effect of winning the eligibility lottery
▶ But what if we actually care about the causal effect of
enrolling in Medicaid ?
▶ Winning the lottery is not the same as enrolling in Medicaid:
▶ Treatment Control
Ever on Medicaid 0.397 0.141
▶ Some people who win the lottery do not enroll (don’t return
form; no longer eligible)
▶ Some people who lose the lottery enroll anyway (become
eligible by other criteria)

#### Page 32

The effect of Medicaid enrollement on depression
Treatment Control
Ever on Medicaid 0.397 0.141
Depression symptoms 0.306 0.329
▶ What is the estimated effect of winning the lottery on
Medicaid enrollment?
0.397 − 0.141 = 0.256
▶ What is the estimated effect of winning the lottery on
depression?
0.306 − 0.329 = −0.023
▶ Say you want to estimate the effect of Medicaid enrollment
on depression? What would you do?
▶ A natural estimate is to divide the effect on depression by the
effect on enrollment: −0.023/0.256 ≈ −0.09 → a 9 pp
reduction per enrollee

#### Page 33

Angrist (1990): The “Draft Lottery Paper”
Angrist famously used Vietnam-era draft eligibility as an
instrument to estimate the earnings effects of military service
▶ Let Zi ∈ {0, 1} be an indicator for draft eligibility, Di ∈ {0, 1}
be an indicator for military service, and Yi measure later-life
earnings
Here βIV = Cov (Zi ,Yi )/Var(Zi )
Cov (Zi ,Di )/Var(Zi ) = E [Yi |Zi =1]−E [Yi |Zi =0]
E [Di |Zi =1]−E [Di |Zi =0] has a special
name, because Zi is binary: the Wald estimand
▶ First stage E [Di | Zi = 1] − E [Di | Zi = 0]: effect of eligibility
on the probability of military service (b/c Di is binary)
▶ Reduced form E [Yi | Zi = 1] − E [Yi | Zi = 0]: effect of
eligibility on adult earnings (measured in 1971, 1981...)
IV interprets the latter causal effect in terms of the former

#### Page 34

Angrist (1990)

#### Page 35

Outline
Regression Review
Models vs. Estimands vs. Estimators
Regression Identification and Endogeneity
Introduction to IV
Instrument Validity and Relevance
The 2SLS Estimator
Where do (Good) Instruments Come From?
True Lotteries
Natural Experiments
Panel Data
2SLS Mechanics
Just-Identified IV
Overidentification
Weak and Many Instruments
Weak IVs
Many IVs

#### Page 36

Subtlties of the Validity Condition
To apply IV, we need to make a good case for instrument validity
Consider our simple causal model, Yi = α + βDi + εi. Validity,
Cov(Zi , εi) = 0, intuitively requires two distinct assumptions:
▶ As-good-as-random assignment / Indenpedence: individuals
with higher/lower potential earnings face the same
distribution of Zi
▶ Exclusion: the “assignment” of Zi only affects Yi through Di
Confusingly, old-school econometrics texts sometimes refer to
Cov(Zi , εi) = 0 as the “exclusion restriction”
▶ More modern IV texts take care to distinguish between these
two conceptually distinct requirements...

#### Page 37

A Valid Instrument

#### Page 38

A Violation of As-Good-As-Random Assignment

#### Page 39

A Violation of Exclusion

#### Page 40

Where do IVs Come From?
1. True Lotteries
One sure-fire way to ensure that Zi is as-good-as-randomly
assigned is... to randomly assign it!
▶ Some of the best IVs come from lotteries, either run by the
researcher (e.g. an RCT) or so-called “natural experiments”
▶ We still need to worry about violations of the exclusion
restriction
▶ Relevance holds when Zi has some effect on Di
“Gold standard” IV: a randomized offer to participate in a
program, with Di recording program participation
▶ Exclusion restriction likely to hold for any Yi, by construction
▶ Relevance almost guaranteed (provided people want the
program!)

#### Page 41

Where do IVs Come From?
1. True Lotteries
One sure-fire way to ensure that Zi is as-good-as-randomly
assigned is... to randomly assign it!
▶ Some of the best IVs come from lotteries, either run by the
researcher (e.g. an RCT) or so-called “natural experiments”
▶ We still need to worry about violations of the exclusion
restriction
▶ Relevance holds when Zi has some effect on Di
“Gold standard” IV: a randomized offer to participate in a
program, with Di recording program participation
▶ Exclusion restriction likely to hold for any Yi, by construction
▶ Relevance almost guaranteed (provided people want the
program!)

#### Page 42

Where do IVs Come From?
1. True Lotteries
One sure-fire way to ensure that Zi is as-good-as-randomly
assigned is... to randomly assign it!
▶ Some of the best IVs come from lotteries, either run by the
researcher (e.g. an RCT) or so-called “natural experiments”
▶ We still need to worry about violations of the exclusion
restriction
▶ Relevance holds when Zi has some effect on Di
“Gold standard” IV: a randomized offer to participate in a
program, with Di recording program participation
▶ Exclusion restriction likely to hold for any Yi, by construction
▶ Relevance almost guaranteed (provided people want the
program!)

#### Page 43

Example
Charter School Lotteries
Abdulkadiroglu et al. (2016) are interested in whether going to a
“charter” middle school increases standardized test scores
▶ Charter students tend to score better, but they worry about
selection
▶ History of doubting educational inputs, since Coleman (1966)

#### Page 44

Example
Charter School Lotteries
They leverage an institutional feature of charters: admission
lotteries
▶ When more kids want to enroll than there are seats, admission
offers Zi ∈ {0, 1} are effectively drawn from a hat
▶ Offers plausibly only affect later test scores Yi by changing
charter enrollment Di ∈ {0, 1}, so are plausibly valid
instruments
They study a particular charter (UP Academy), which is
“takeover”
▶ Two offer IVs: “immediate” (on lottery night) and from a
waitlist

#### Page 45

Example
Charter School Lotteries
They leverage an institutional feature of charters: admission
lotteries
▶ When more kids want to enroll than there are seats, admission
offers Zi ∈ {0, 1} are effectively drawn from a hat
▶ Offers plausibly only affect later test scores Yi by changing
charter enrollment Di ∈ {0, 1}, so are plausibly valid
instruments
They study a particular charter (UP Academy), which is
“takeover”
▶ Two offer IVs: “immediate” (on lottery night) and from a
waitlist

#### Page 46

Example
Charter School Lotteries
They leverage an institutional feature of charters: admission
lotteries
▶ When more kids want to enroll than there are seats, admission
offers Zi ∈ {0, 1} are effectively drawn from a hat
▶ Offers plausibly only affect later test scores Yi by changing
charter enrollment Di ∈ {0, 1}, so are plausibly valid
instruments
They study a particular charter (UP Academy), which is
“takeover”
▶ Two offer IVs: “immediate” (on lottery night) and from a
waitlist

#### Page 47

Lottery IV Estimates of UP Test Score Effects

#### Page 48

Where do IVs Come From?
2. Natural Experiments
Without appealing to literal randomization, we may credibly argue
Zi is as-good-as-randomly assigned conditional on some Wi
▶ Such “natural experiments” rely on a selection-on-observables
argument (for Zi, instead Di)
▶ Still worry about exclusion: Zi cannot affect Yi except
through Di

#### Page 49

Example
Quarter-of-Birth
▶ Angrist and Krueger (1991) are interested in a classic question
in labor economics: what are the labor market returns to an
additional year of schooling?
▶ Why can’t we just compare earnings for people who get more
or less schooling?
▶ Schooling is not randomly assigned! Choice of schooling may
depend on many confounding factors, such as ability or family
background, that would directly affect earnings as well.
▶ What do we need if we want to estimate the effects of
schooling on earnings using IV?
▶ We need to find an instrument that is as-good-as-randomly
assigned, and affects earnings only through years of schooling.

#### Page 50

Compulsory Schooling Laws
▶ Most states have compulsory schooling laws that require
people to stay in school until their 16th or 17th birthday
▶ With fixed school start dates, these laws will lead people
planning to drop out to have less schooling if they are born
earlier in the year
▶ If school start in Jan 1, if the oldest kid in a class is born on
Jan 1 and the youngest kid is born on December 31, then the
oldest kid can legally drop out after getting 1 fewer year of
education than the youngest kid
▶ AK argue that the part of the year in which you are born is
effectively random, and therefore instrument for years of
schooling with quarter of birth

#### Page 51

The Quarter-of-Birth Natural Experiment: Visualized

#### Page 52

Quarter-of-Birth IV Estimates of Returns to Schooling

#### Page 53

Evaluating the assumptions
▶ Relevance:
▶ Need quarter of birth to be correlated with years of education.
▶ This one we can check and indeed is the case ( t-statistic of
12).
▶ Independence:
▶ Need quarter of birth to be independent of determinants of
wages or responses to compulsory schooling laws
(Zi ⊥ (Y (.), D(.)))).
▶ Independence seems plausible if parents can’t time the births
of their children exactly.
▶ But...

#### Page 54

▶ Quarter of birth is correlated with some demographic
characteristics
▶ Suggests some possible violations of independence

#### Page 55

Evaluating the assumptions...
▶ Exclusion: Quarter of birth affects earnings only through
number of years of education
▶ Seems generally plausible but...
▶ being older or younger in your grade may affect the quality of
your education directly

#### Page 56

[No extractable text in PDF text layer; image page.]

#### Page 57

[No extractable text in PDF text layer; image page.]

#### Page 58

Where do IVs Come From?
3. Panel Data
We might also combine IV + difference-in-differences identification
▶ E.g. instrument with Zi × Postt, controlling for Zi and Postt
FEs
▶ This requires two parallel trends assumptions, for the RF and
FS
▶ Still need to worry about the exclusion restriction, as always

#### Page 59

Example
Charter School Takeovers
In Abdulkadiroglu et al. (2016), they complement the lottery
analysis of takeover charters with an instrumented diff-in-diff
analysis
▶ Students enrolled in the “legacy” public school were eligible
for being “grandfathered” into UP, without having to apply to
the charter
▶ We compare their trends in test scores & enrollment to a
matched comparison group of observably-similar students at
other schools

#### Page 60

Grandfathering IV: Visualized

#### Page 61

Grandfathering IV Estimates of UP Test Score Effects

#### Page 62

Outline
Regression Review
Models vs. Estimands vs. Estimators
Regression Identification and Endogeneity
Introduction to IV
Instrument Validity and Relevance
The 2SLS Estimator
Where do (Good) Instruments Come From?
True Lotteries
Natural Experiments
Panel Data
2SLS Mechanics
Just-Identified IV
Overidentification
Weak and Many Instruments
Weak IVs
Many IVs

#### Page 63

Just-Identified IV
Tthe Stata ivregress/ivreg2 commands (or fixest::feols in R) allows
for controls and multiple treatments / instruments
▶ When # treatment > # instruments, it is “under-identified”,
there is no consistent IV estimator.
▶ When # treatment = # instruments, we say the IV is
“just-identified” or “exact-identified”:
Yi = βDi + W′
i γ + εi (second stage)
Di = πZi + W′
i µ + ηi (first stage)
where Wi includes a constant.

#### Page 64

Just-Identified IV
The reduced form is:
Yi = ρZi + W′
i κ + νi
Same identification logic as before:
▶ Validity: Cov(Zi , εi) = 0, allowing Cov(Zi , Wi) ̸= 0
▶ Relevance: π ̸= 0, so Zi and Di are correlated controlling for
Wi
IV is still “reduced form over first stage”: ( βIV = ρOLS /πOLS)
▶ Can use Frisch-Waugh-Lovell to “partial out” Wi from Yi, Di,
Zi, and so get back to an IV regression without controls

#### Page 65

Overidentification
Sometimes we have more than one instrument Ziℓ, for
ℓ = 1, . . . ,L. This leads to an “overidentified” IV regression:
Yi = βDi + W′
i γ + εi (second stage)
Di = Z′
i π + W′
i µ + ηi (first stage)
where Zi = [Zi1, . . . ,ZiL]′. Reduced form: Yi = Z′
i ρ + W′
i κ + νi
Validity: Cov(Ziℓ, εi) = 0 for all ℓ
▶ “Overidentified” b/c we could use any Ziℓ to identify
β = ρℓ/πℓ
▶ Relevance: πℓ ̸= 0 for at least some ℓ
Overidentification can yield tests of IV validity
▶ Intuitively, 2SLS checks whether all the Ziℓ yields the same IV
estimate, which is sensible in a constant-effects model...

#### Page 66

Overidentification
Sometimes we have more than one instrument Ziℓ, for
ℓ = 1, . . . ,L. This leads to an “overidentified” IV regression:
Yi = βDi + W′
i γ + εi (second stage)
Di = Z′
i π + W′
i µ + ηi (first stage)
where Zi = [Zi1, . . . ,ZiL]′. Reduced form: Yi = Z′
i ρ + W′
i κ + νi
Validity: Cov(Ziℓ, εi) = 0 for all ℓ
▶ “Overidentified” b/c we could use any Ziℓ to identify
β = ρℓ/πℓ
▶ Relevance: πℓ ̸= 0 for at least some ℓ
Overidentification can yield tests of IV validity
▶ Intuitively, 2SLS checks whether all the Ziℓ yields the same IV
estimate, which is sensible in a constant-effects model...

#### Page 67

Overidentification
Sometimes we have more than one instrument Ziℓ, for
ℓ = 1, . . . ,L. This leads to an “overidentified” IV regression:
Yi = βDi + W′
i γ + εi (second stage)
Di = Z′
i π + W′
i µ + ηi (first stage)
where Zi = [Zi1, . . . ,ZiL]′. Reduced form: Yi = Z′
i ρ + W′
i κ + νi
Validity: Cov(Ziℓ, εi) = 0 for all ℓ
▶ “Overidentified” b/c we could use any Ziℓ to identify
β = ρℓ/πℓ
▶ Relevance: πℓ ̸= 0 for at least some ℓ
Overidentification can yield tests of IV validity
▶ Intuitively, 2SLS checks whether all the Ziℓ yields the same IV
estimate, which is sensible in a constant-effects model...

#### Page 68

Overidentification
Sometimes we have more than one instrument Ziℓ, for
ℓ = 1, . . . ,L. This leads to an “overidentified” IV regression:
Yi = βDi + W′
i γ + εi (second stage)
Di = Z′
i π + W′
i µ + ηi (first stage)
where Zi = [Zi1, . . . ,ZiL]′. Reduced form: Yi = Z′
i ρ + W′
i κ + νi
Validity: Cov(Ziℓ, εi) = 0 for all ℓ
▶ “Overidentified” b/c we could use any Ziℓ to identify
β = ρℓ/πℓ
▶ Relevance: πℓ ̸= 0 for at least some ℓ
Overidentification can yield tests of IV validity
▶ Intuitively, 2SLS checks whether all the Ziℓ yields the same IV
estimate, which is sensible in a constant-effects model...

#### Page 69

Putting the “2S” in “2SLS”
You’ll notice I haven’t actually defined 2SLS beyond the simple
case
▶ Before we had βIV = Cov (Zi ,Yi )
Cov (Zi ,Di ) leading to bβIV =
dCov (Zi ,Yi )
dCov (Zi ,Di )
▶ General form follows similarly (as a sample analog) but is
notation-heavy, so we won’t go into it here
A more useful way to define 2SLS is by a two-step procedure:
▶ First regress Di on all instruments Ziℓ and controls Wik
▶ Then regress Yi on the “fitted values” bDi and controls Wik
The proof of this follows from some (simple) linear algebra
▶ Intuitively, regressing Yi on bπOLS Zi gives a scaled RF:
bβIV = bρOLS
bπOLS

#### Page 70

Putting the “2S” in “2SLS”
You’ll notice I haven’t actually defined 2SLS beyond the simple
case
▶ Before we had βIV = Cov (Zi ,Yi )
Cov (Zi ,Di ) leading to bβIV =
dCov (Zi ,Yi )
dCov (Zi ,Di )
▶ General form follows similarly (as a sample analog) but is
notation-heavy, so we won’t go into it here
A more useful way to define 2SLS is by a two-step procedure:
▶ First regress Di on all instruments Ziℓ and controls Wik
▶ Then regress Yi on the “fitted values” bDi and controls Wik
The proof of this follows from some (simple) linear algebra
▶ Intuitively, regressing Yi on bπOLS Zi gives a scaled RF:
bβIV = bρOLS
bπOLS

#### Page 71

Putting the “2S” in “2SLS”
You’ll notice I haven’t actually defined 2SLS beyond the simple
case
▶ Before we had βIV = Cov (Zi ,Yi )
Cov (Zi ,Di ) leading to bβIV =
dCov (Zi ,Yi )
dCov (Zi ,Di )
▶ General form follows similarly (as a sample analog) but is
notation-heavy, so we won’t go into it here
A more useful way to define 2SLS is by a two-step procedure:
▶ First regress Di on all instruments Ziℓ and controls Wik
▶ Then regress Yi on the “fitted values” bDi and controls Wik
The proof of this follows from some (simple) linear algebra
▶ Intuitively, regressing Yi on bπOLS Zi gives a scaled RF:
bβIV = bρOLS
bπOLS

#### Page 72

Avoid Manual 2SLS!
Although easy, you should never do such “manual 2SLS” yourself!
▶ Your point estimates will be right, but your SEs won’t be!
▶ Also might forget to include some controls in the second
stage, etc
Just let Stata/R do everything for you...

#### Page 73

2SLS Done Right

#### Page 74

Outline
Regression Review
Models vs. Estimands vs. Estimators
Regression Identification and Endogeneity
Introduction to IV
Instrument Validity and Relevance
The 2SLS Estimator
Where do (Good) Instruments Come From?
True Lotteries
Natural Experiments
Panel Data
2SLS Mechanics
Just-Identified IV
Overidentification
Weak and Many Instruments
Weak IVs
Many IVs

#### Page 75

Weak Instruments
When running just-identified IV, you should always worry about
the “strength” of your instrument
▶ Specifically the first stage F-statistic, which tests πOLS = 0
If πOLS is small relative to its standard error, the IV is “weak”
▶ Typically use the rule-of-thumb of F < 10 (Staiger and Stock
1997). This F-test should always be reported with your IV
estimates.
▶ In this case the second-stage SEs will be large and the 2SLS
estimate will tend to be biased towards the corresponding OLS
Much made of this over the years, but Angrist and Koles´ ar (2022)
argue recently that we shouldn’t worry too much
▶ The SE increase tends to be large enough to “cover up” the
bias
▶ Just-id. 2SLS is “approximately median-unbiased”

#### Page 76

Weak Instruments
When running just-identified IV, you should always worry about
the “strength” of your instrument
▶ Specifically the first stage F-statistic, which tests πOLS = 0
If πOLS is small relative to its standard error, the IV is “weak”
▶ Typically use the rule-of-thumb of F < 10 (Staiger and Stock
1997). This F-test should always be reported with your IV
estimates.
▶ In this case the second-stage SEs will be large and the 2SLS
estimate will tend to be biased towards the corresponding OLS
Much made of this over the years, but Angrist and Koles´ ar (2022)
argue recently that we shouldn’t worry too much
▶ The SE increase tends to be large enough to “cover up” the
bias
▶ Just-id. 2SLS is “approximately median-unbiased”

#### Page 77

Weak Instruments
When running just-identified IV, you should always worry about
the “strength” of your instrument
▶ Specifically the first stage F-statistic, which tests πOLS = 0
If πOLS is small relative to its standard error, the IV is “weak”
▶ Typically use the rule-of-thumb of F < 10 (Staiger and Stock
1997). This F-test should always be reported with your IV
estimates.
▶ In this case the second-stage SEs will be large and the 2SLS
estimate will tend to be biased towards the corresponding OLS
Much made of this over the years, but Angrist and Koles´ ar (2022)
argue recently that we shouldn’t worry too much
▶ The SE increase tends to be large enough to “cover up” the
bias
▶ Just-id. 2SLS is “approximately median-unbiased”

#### Page 78

Weak Instruments: Visualized
Monte Carlo: Yi = εi, Di = ΠZi + ηi: Π = Var(εi) = Var(ηi) = 1

#### Page 79

Weak Instruments: Visualized
Monte Carlo: Yi = εi, Di = ΠZi + ηi: Π = 0 .1 (Weaker)

#### Page 80

Weak Instruments: Visualized
Monte Carlo: Yi = εi, Di = ΠZi + ηi: Π = 0 .01 (Very Weak)

#### Page 81

Many IVs
A more pernicious problem is many-instrument bias, when overid
▶ Also tends to manifest in low first-stage F’s, so also good to
check
Many-IV bias is also towards OLS. But unlike before, the SEs go
down
▶ Intuitively, a more flexible FS tends to fit Di better → more
power
▶ But we can have overfitting with lots of Zi → essentially
recreate Di
As we’ll see, this bias is especially relevant in judge IV designs
▶ Potentially many judge assignment indicators as the
instrument
▶ Leave-out corrections (e.g. Angrist et al. 1999) have been
adapted to this setting in recent years (e.g. Koles´ ar 2013)

#### Page 82

Many IVs
A more pernicious problem is many-instrument bias, when overid
▶ Also tends to manifest in low first-stage F’s, so also good to
check
Many-IV bias is also towards OLS. But unlike before, the SEs go
down
▶ Intuitively, a more flexible FS tends to fit Di better → more
power
▶ But we can have overfitting with lots of Zi → essentially
recreate Di
As we’ll see, this bias is especially relevant in judge IV designs
▶ Potentially many judge assignment indicators as the
instrument
▶ Leave-out corrections (e.g. Angrist et al. 1999) have been
adapted to this setting in recent years (e.g. Koles´ ar 2013)

#### Page 83

Many IVs
A more pernicious problem is many-instrument bias, when overid
▶ Also tends to manifest in low first-stage F’s, so also good to
check
Many-IV bias is also towards OLS. But unlike before, the SEs go
down
▶ Intuitively, a more flexible FS tends to fit Di better → more
power
▶ But we can have overfitting with lots of Zi → essentially
recreate Di
As we’ll see, this bias is especially relevant in judge IV designs
▶ Potentially many judge assignment indicators as the
instrument
▶ Leave-out corrections (e.g. Angrist et al. 1999) have been
adapted to this setting in recent years (e.g. Koles´ ar 2013)

#### Page 84

Many Instruments: Visualized
Monte Carlo: Yi = εi, Di = ΠZi1 + ηi: IV with one Zi1

#### Page 85

Many Instruments: Visualized
Monte Carlo: Yi = εi, Di = ΠZi1 + ηi: IV with ten Zij

#### Page 86

Many Instruments: Visualized
Monte Carlo: Yi = εi, Di = ΠZi1 + ηi: IV with 100 Zij

#### Page 87

What to Do?
Check your F’s after every IV regression
▶ State of the art: Montiel Olea and Pflueger ’15; weakivtest in
Stata
▶ Staiger-Stock rule-of-thumb ( F > 10) still seems widely held
▶ See Lee et al. (2020) and Keane and Neal (2022) for some
discussions of additional subtleties
If your F is small, some things to consider:
▶ Is there a different instrument that’s stronger?
▶ Is there a better functional form for the instrument you have?
▶ Do interactions with covariates help? (note: beware
many-weak!)
▶ Does changing the covariate set help? (note: beware
invalidity!)

### Introduction.pptx — 13 slides

#### Slide 1

Applied Econometrics

Yumin Li

#### Slide 2

Who am I?

Yumin Li (Associate Prof. of Economics, SILC, SHU) 
Applied microeconomist working primarily at the intersection of industrial organization and environmental and energy economics
User of structural models and methods such as DID and IV

Recently, I have written on topics such as…
Solar subsidies in the U.S.
Electricity cross-subsidies in China
The causal effects of COVID-19 lockdown on air quality
The causal effects of  U.S. trade restrictions on innovations
Exclusive dealing and subsidies on platforms

#### Slide 3

Econometrics II

This is the second course of Econometrics and I am going to mainly cover applied econometric methods, especially the design tradition in causal inference. 
Causal inference is an old field but has increasingly drawn people to it (2021 Nobel Prize in Economic Sciences was awarded to David Card, Joshua Angrist and Guido Imbens).
Hope to cover potential outcomes, randomization, unconfoundedness, instrumental variables and difference-in-differences, regression discontinuity etc.
I hope to cover intuition, mechanics, calculations, meaning, assumptions, code including actually taking time to code, advocate for data visualization – in other words the art and the science.

#### Slide 4

Why do we need research?

This course is about doing research. Why do we need it?
Knowledge is power, and social science could be valuable: Head Start in the US; the New Deal in the US as a way to address the Great Depression etc.
Economics could be valuable: Bretton Woods system; congestion pricing; Conditional Cash Transfer etc.

#### Slide 5

The Polymarket Trump 'Whale'

On the cryptocurrency betting platform Polymarket, a French trader named Theo splurged $30 million on betting Trump defeating Harris to win the 2024 US presidential election. This trader earned $85 million from this US election.
The mainstream polls often cater to biases
The 'shy Trump voter' phenomenon
Instead of asking pollsters whom they intended to vote for, he asked them whom they believed their neighbor would vote for.

#### Slide 6

Why do we need research?

Scientific methodologies are the epistemological foundation of scientific knowledge 
Science does not collect evidence in order to “prove” what people already believe or want others to believe. 
Science accepts unexpected and even undesirable answers. 
Science is process oriented, not outcome oriented. 
Knowledge is built upon the contributions of many individuals through iterative research, writing, and debate. The scientific method and the rigor of peer review establish a framework for validating knowledge. This process is intentionally neither easy nor quick. What often begins as an idea or a hunch evolves through stages—from supposition to proof, and ultimately to consensus.

#### Slide 7

Why do we need research?

Research skills are applicable in various fields, not just academia: critical thinking, data analysis, and evidence-based reasoning are valuable in business, healthcare, policy-making, and more.
More research skills could give us better understanding of the world and enriching our intellectual lives.

#### Slide 8

How to obtain quality information?

In the era of information overload and information cocoons: Promoting media literacy; Finding reliable sources; Engaging with contradictory viewpoints
To ensure AI outputs are scientifically sound
Answer the following question in rigorous academic style, adhering to: 
Core conclusions must derive from systematic reviews/meta-analyses or authoritative guidelines (≤5 years old); specify evidence type. 
Clearly distinguish: verified consensus (with consensus level), academic controversies (cite evidence from multiple perspectives), and knowledge gaps. 
Maintain neutral, objective language; exercise precision with metrics (e.g., effect sizes, confidence intervals). 
If reliable evidence is insufficient, state: "Current high-quality evidence is limited" — do not speculate. 
Question: [Your query]
Always treat AI as a supportive starting point—not an authoritative endpoint.

#### Slide 9

Tentative Plan of the Course

A Hidden Curriculum of Applied Research
Potential Outcomes
Difference-in-differences
Instrumental Variables
Regression Discontinuity

#### Slide 10

Assessment

Your grade will be based on three things: 
Paper Replication: 20%. 
Find a research paper on top journals. 
Week 4 first draft, week 12 final version
Research proposal: 30%
Research question, literature, research strategy, data, model etc.
Week 8 first draft, week 16 final version
Final exam: 50%. Cumulative. 
You can form a study group (2-3 persons) to do the replication and proposal.

#### Slide 11

About using AI

AI anxiety: humans who use AI vs humans who don’t
AI can teach you everything I teach, like internet/library has everything I teach. All the knowledge, equations, methods and even codes. What can you learn from this class?
How to ask good questions? Or how to write good econometrics related prompts?

#### Slide 12

Materials and Reference

Cunningham, S., 2021. Causal inference: The mixtape. Yale university press.
Chernozhukov, V., Hansen, C., Kallus, N., Spindler, M. and Syrgkanis, V., 2024. Applied causal inference powered by ML and AI. arXiv preprint arXiv:2403.02467.
Facure, M., 2023. Causal Inference in Python.  O'Reilly Media, Inc.
邱嘉平, 2020. 因果推断实用计量方法. 上海财经大学出版社.

https://github.com/matteocourthoud/awesome-causal-inference/blob/main/src/books.md

#### Slide 13

More Importantly

Online Resources: books, courses, codes
Learn to search for academic papers and books: 
https://scholar.google.com/
https://sci-hub.se/
https://annas-archive.org/
Learn to find data and code 
Administrative data and statistics (CSMAR etc.)
Harvard Dataverse
Journals and economists’ personal sites

### HiddenCurriculum.pptx — 85 slides

#### Slide 1

A Hidden Curriculum of Applied Research

Yumin Li
Shanghai University

#### Slide 2

Outline

Introduction
Section 1. Starting Your Research Project
Section 2. Practical Tips for Writing Your Applied Paper
Section 3. Presentation

#### Slide 3

What I wish I would have learned as a student on…
What’s my future career
How to prepare my career
In this course, we discuss
Research process
Writing papers
Publishing and other stuff (refereeing, presenting research, etc.)

There is a template for doing good applied research, and I hope to share that with you

What is a hidden curriculum?

#### Slide 4

Section 1: Starting Your Research Project

What makes a research question viable?

The three hurdles:
(i) Is it interesting/important/policy relevant? 
(ii) Are you filling a clearly defined gap in the literature?  
(iii) Are the data available? Does a well-defined natural experiment exist?

#### Slide 5

Hurdle #1. Is it interesting/important/policy relevant?

How do I know if my idea is interesting?
Seems interesting to me, but…
Pitch your idea broadly, especially to other economists who you know will be critical
Try-to-kill-my-idea sessions
Pitch your idea to non-economists.

If it is interesting, does that mean it is “important”?
Yes! Which leads to…

Does my idea have to be policy relevant? 
It depends 
Sometimes explain an important phenomenon is good enough.

#### Slide 6

Is there policy relevance to explaining the urban mortality transition?
Sure, maybe. But, trying to better understand what has been called “one of the most significant developments in the history of human welfare” is interesting in and of itself!
Don’t force a project to be policy relevant if it isn’t or, more importantly, doesn’t need to be

[Embedded image/diagram; no text alternative in the slide text layer.]

#### Slide 7

AER Vol. 115, No. 3, March 2025

Land Rental Markets: Experimental Evidence from Kenya
Do Ordeals Work for Selection Markets? Evidence from Health Insurance Auto-Enrollment
Mortgage Pricing and Monetary Policy
Internationalizing Like China
Ownership Concentration and Strategic Supply Reduction
The Decline of Too Big to Fail
How Competitive Is the Stock Market? Theory, Evidence from Portfolios, and Implications for the Rise of Passive Investing
Corruption as a Local Advantage: Evidence from the Indigenization of Nigerian Oil

#### Slide 8

《经济研究》No. 8, 2024

[Embedded image/diagram; no text alternative in the slide text layer.]

[Embedded image/diagram; no text alternative in the slide text layer.]

#### Slide 9

Become an expert of the relevant literature. 

How do I do this?
For each project, I create a literature-review binder
1.) Start with econ-specific search engines (e.g., JSTOR, Econ Lit, NBER WP, etc.) before doing more general searches on     
     engines such as Google Scholar
2.) Sort by theoretical vs. empirical
3.) Sort by how well the papers published
4.) Sort empirical papers by the credibility or type of research design
Cross-sectional vs. panel
IV vs. DD vs. RD vs. Structural Model Estimation

If you do this, you will know exactly how you are building on existing studies.

Hurdle #2. Are you filling a clearly defined gap in the literature?

#### Slide 10

[Embedded image/diagram; no text alternative in the slide text layer.]

#### Slide 11

Hardest hurdle. Do not assume the data exist…
Even if data exist, collecting/cleaning/etc. can be a heavy lift.

Data on X and Y are available, but I do not have a well-defined natural experiment. Should I dump the idea?
It will be difficult to publish in economics. But you may try management journals, or other interdisciplinary journals. 
You can become a structural model economist, but then you won’t have any friends.
Just kidding!
Not really

Hurdle #3. Are the data available? Does a well-defined natural experiment exist?

#### Slide 12

Start with hypothesis of interest and then try to identify a natural experiment
vs.
Observe a natural experiment and then come up with a question

Some “old-school” folks think the pure form of research is to do the former.
Impractical
Miss out on a lot of good topics

Always have your natural experiment radar on…”one does not know where an inspiration will come from.”

But, most importantly, become an expert of institutional details, the historical context, etc. of your areas of interest.
Do this and ideas will naturally come!

Hurdle #3 (continued)

#### Slide 13

If you can get over these 3 hurdles, you are off to the races!






If you can’t, get back up, and go back to the drawing board.

[Embedded image/diagram; no text alternative in the slide text layer.]

[Embedded image/diagram; no text alternative in the slide text layer.]

#### Slide 14

When to cut the cord on a project

You’ve cleared the 3 hurdles, but when do I call it quits?  
As a graduate student trying to come up with a great JMP or a jr. prof. trying to get tenure, this can be a make-it or break-it decision.

When does this come up?
1.) Null results
2.) Cannot identify mechanisms
3.) Something better comes along
4.) Someone comes up with a well-executed working paper with similar topic.

#### Slide 15

The Null Result

Zero is a number too! But, is it always interesting?
No effect of minimum wage hike on unemployment
Interesting and policy relevant zero.
What about the relationship between prenatal exposure to the Super Bowl and low birth weight (Duncan et al. 2017, JHR) or the relationship between minimum wages and traffic fatalities (Adams et al. 2012, RESTAT)?
Probably only interesting if you find an effect.
High risk-high reward projects
Null effect would not make for a good JMP
Null effect would not publish well.
Does this cause a type of file-drawer bias?  Sure.
Be practical when weighing the trade-offs between which projects to pursue and which ones to quit.

Zero is a number too, but only if it is precise!
Big difference between pitching a tight vs. noisy zero.
If 95% CI contains large values on either side of zero, we do not learn much.

#### Slide 16

The Null Result

[Embedded image/diagram; no text alternative in the slide text layer.]

[Embedded image/diagram; no text alternative in the slide text layer.]

Precise zeros can publish well

Imprecise zeros, not as well

If null is interesting/policy relevant and precise, then proceed!
	-But, know that the burden of proof can be much higher.

#### Slide 17

Identifying Mechanisms

Suppose you have a great natural experiment, but can’t pin down mechanism(s).
Death sentence?
Not necessarily!  It depends on the topic…

Medical marijuana laws (MMLs) → traffic fatalities (Anderson et al. 2013, J of Law and Econ)
MMLs → 8 to 11% decrease in traffic fatalities
Effect driven by alcohol-related crashes
Plus, beer sales and self-reports of binge drinking fall
Without shedding light on mechanism, would not have published very well.

#### Slide 18

Identifying Mechanisms

Military deployments → crime (Anderson and Rees 2015, J of Law and Econ)
Increases in the number of never-deployed combat brigades at Fort Carson, CO → more violent crime
In contrast, previously deployed brigades do not cause crime
Never-deployed units represent a greater threat to public safety…why?  
Unable to say much
But, (interesting) results ran counter to common perception and paper published well

Medical marijuana laws (MMLs) → suicide (Anderson et al. 2014, American J of Public Health)
MMLs → reductions in suicides among young adult males
Policy relevant?  Yes.
General interest (within econ) without mechanism(s)?  Probably not.
Low ceiling within econ journals
Not a great JMP
Recommendation:  Move on or pivot to a non-economics journal
Flagship journal in public health

#### Slide 19

Try to gauge early on whether you will be able to address underlying mechanisms.
Maybe consider this hurdle #4.

Try to gauge early on just how crucial it will be to address underlying mechanisms.

If you end up sinking a lot of time into a project and cannot address mechanisms, you can pivot to a non-economics journal.

Identifying Mechanisms

#### Slide 20

A Similar Paper Shows Up

[Embedded image/diagram; no text alternative in the slide text layer.]

#### Slide 21

Targeting journals
FAQ: “Where should I send paper X?”
If you become a master of the literature, the set of relevant journals will be clear 

When to target a non-economics journal
You need to pivot because…
Estimates are correlations only
You cannot identify mechanisms
You just don’t have enough “meat” for a full-length paper
You need a mental health break!

Managing your research projects

#### Slide 22

Section 2: Practical Tips for Writing Your Applied Paper

What are you targeting?
A journal, graduation thesis, or just a course paper.
The classic way to structure an applied economic paper should include these sections: 
Introduction
Background
Data Summary
Empirical Strategy
Results
Conclusion.

#### Slide 23

Length

Aim for 18-23 pages of text, 5-6 figures and tables (combined total)
More figures than tables
Extra tables and figures can go in an appendix
The revision process is usually going to add text, tables, and figures
Thirty pages of text and 10 figures/tables is too much!  Referees and editors have short attention spans.

#### Slide 24

Grammar

Use LLM to revise you paper for typos and grammatical errors
Make sure there is a one-to-one correspondence between in-text citations and references
Use a consistent style for the references
Include the first names of the authors in the reference section. The editor is looking at your references for potential referees.

#### Slide 25

Writing the first half of your applied paper

The Introduction
Perhaps the most important part of your paper.
Why should someone keep reading your paper if they aren’t “hooked” by the end of the introduction?
Don’t reinvent the wheel. Read classics.
There is a template for writing a good introduction.

#### Slide 26

An (applied micro) introduction should contain:
(i) General motivation.  Captivate economists and non-economists alike.
(ii) Specific motivation.  Appeal to policy being evaluated or theory being tested.
(iii) Brief description of background 
(iv) Data description
(v) Description of identification strategy.
(vi) Description of principal results.
(vii) Roadmap (optional)

I like to approach writing the introduction as a box-checking exercise

Let’s walk through an example…

The Introduction

-You are NOT writing a mystery novel!

-Reader should know exactly what to expect by the end of the introduction.

#### Slide 27

(i) General motivation
(ii) Specific motivation that appeals to scholars working in this area.
Previous explanations for the TB mortality decline 
Highlight explanation we are interested in testing (i.e., public health)
Highlight that this remains an open question, which also highlights our contribution

[Embedded image/diagram; no text alternative in the slide text layer.]

#### Slide 28

(iii) Important background details
Historical background
Relevance to modern day public health
What constituted the anti-TB campaign?

(iv) Data description

(v) Identification

(vi) Results

[Embedded image/diagram; no text alternative in the slide text layer.]

#### Slide 29

Three Common FAQs

How many contributions should I list?  
A good paper probably only makes one or two real contributions. 
If you must list more, clearly indicate which contribution you think is most important.
Big mistake:  List too many contributions and do not rank them in order of importance
Big mistake:  Claim to advance a number of different fields.  
Signals you aren’t sure where your major contributions lie.
Is this a labor paper or a behavioral paper or a econ history paper…or something else?  Avoid creating this confusion
 Do not confuse editors in search of referees

How long should my introduction be?
No rule of thumb.  
But, avoid lumping your intro and background sections together.
Error on side of brevity.  Do not ask reader to wade through a 10-page introduction.
Make it less than 5 pages.

#### Slide 30

How much does my writing matter?
This applies to the paper in its entirety, but it matters a lot!!!
Grammar
From my perspective, blatant grammatical, spelling, sentence structure errors, etc. are a death sentence for a paper.
Why should I trust that you were any less careless regarding the unobserved part of your research project (e.g., data collection, cleaning, and analysis)?
If I am refereeing a paper and the introduction is riddled with these types of mistakes, I generally quit reading and recommend a “strong rejection.”
AI can be helpful.
Style and tone
Be engaging without being chatty or informal.  
More formal than the language you hear being used in seminars
For instance, avoid words like “get” and “got” and avoid using contractions (e.g., won’t, don’t, can’t)
Read what you wrote out load
Does each sentence set up the next?
Does each paragraph set up the next?
Again, go read some classics
Wright, Gavin. 1981. “Cheap Labor and Southern Textiles, 1880-1930.” Quarterly Journal of Economics, 96(4): 605-629.
Charles, Kerwin et al.. 2009. “Conspicuous Consumption and Race.” Quarterly Journal of Economics, 124(2): 425-467.

Three Common FAQs

#### Slide 31

Background

Break into subsections (not necessarily in this order):
(i) Background details regarding your right-hand side variable of interest
Include information relevant to your identification strategy
How many policy changes do you observe?
For how many units (e.g., cities, counties, states, etc.) do you observe pre- and post-treatment data?
(ii) Background on your relationship of interest
(iii) Background specific to the theory being tested (if any)
(iv) Literature review can be a separate section or woven into the above.
I prefer the latter approach.
Flows better
But, harder to pull off
Include subsection headers within background.  
Very helpful for reader

#### Slide 32

Example 1. “The Effects of Pretrial Detention on Conviction, Future Crime, and Employment: Evidence from Randomly Assigned Judges” (Dobbie et al. 2018, American Economic Review)

Goal of paper: Use detention tendencies of quasi-randomly assigned bail judges to estimate the causal effects of pretrial detention on subsequent defendant outcomes.

Background, Section A. Overview of the bail system in the United States
General overview on how bail system works

Background, Section B. Description of setting specific to their data (i.e., Philadelphia and Miami-Dade Counties)
Broken into 3 subsections: 
(i) Philadelphia County bail system
(ii) Miami-Dade County bail system
(iii) Describe how the features of these bail systems make them an ideal setting for their research design.
Subsection B. (iii) is a great example of how one can describe the intuition behind the identification strategy before regression equations.

#### Slide 33

Goal of paper: Examine how state-level nonmedical exemptions from school-entry vaccine mandates affect vaccination rates in early childhood and academic achievement in middle school.

Straightforward (and well-written) paper that does not require a lengthy background section.

Background, Section 1. Description of policy changes in treated states (i.e., AR and TX).

Background, Section 2. Discussion of how exemptions might affect a downstream outcomes.
Description of possible mechanisms.
Great discussion on mechanisms before results are even presented.

Example 2. “Personal Belief Exemptions for School-Entry Vaccinations, Vaccination Rates, and Academic Achievement” (Hair et al. 2018, Journal of Health Economics)

#### Slide 34

Goal of paper: Estimate the effect of municipal-level milk inspections on infant and waterborne mortality.

Background, Section 1. General history of municipal milk inspections in the United States.
Describe exactly how inspections worked.

Background, Section 2. History of other efforts to provide pure milk.
Give readers a sense of what other public health policies and efforts were being mandated.
Relevant for the empirical strategy.  Telling reader what they should be worried about!

Background, Section 3. Literature review on minimum quality standards (MQSs).
Review the competing empirical literature and describe the theoretical contributions
Of the 3 examples, this paper tests a classic economic theory, whereas the others do not.  The literature review must tackle both empirical and theoretical studies.

Example 3. “Safeguarding Consumers Through Minimum Quality Standards: Milk Inspections and Urban Mortality, 1880-1910” (Anderson et al. 2022, NBER Working Paper No. 30063)

#### Slide 35

In one’s literature review, not all previous studies should be treated equally.

Researchers should provide careful and accurate evaluations of previous studies.
Most policymakers have never heard of fixed effects, have no idea what a regression discontinuity is, and do not care whether state-specific time trends were included on the right-hand side.
Policymakers and non-economists count on us to communicate which studies should be taken seriously and which should not (or be ignored).
For instance, giving studies that rely on cross-sectional policy variation the same weight as those that rely on within-state policy variation comes with the risk of leading policymakers astray.

For example, researchers interested in the relationship between marijuana use and alcohol consumption all too often declare that the literature is “mixed”, citing Pacula (1998a) and Williams et al. (2004) as evidence of complementarity and Kelly and Resul (2014) and Sabia et al. (2017) as evidence of substitutability.  
This is a LAZY  characterization of the literature.
Pacula (1998a) and Williams et al. (2004) relied on cross-sectional policy variation.  Their estimates could simply reflect unobserved factors at the state level such as preferences and attitudes.
On the other hand, Kelly and Resul (2014) and Sabia et al. (2017) used panel data and exploited well-defined natural experiments…much more credible research design!

Background: A common mistake!

#### Slide 36

Two separate sections or just one?
Both ways common
I prefer combining into one section.
Empirical strategy section before data section?  
I’m not a huge fan 
I want to understand the data before thinking about the empirical strategy in detail

All in one section, I prefer:
First, begin by describing your outcome data.
Second, describe your primary estimating equation.
Lastly, describe the data for your right-hand side variables.

Let’s go back to the milk inspections paper (Anderson et al. 2022)…

Data and empirical strategy

#### Slide 37

First, we describe the data sources for outcomes of interest (i.e., mortality rates).
If possible, emphasize data collection efforts!
Shed light on where identification is coming from.  It should never be a black box.
List your sources in detail.  Roadmap for replication.





Show descriptive figures, such as trends in your outcomes (Figure 2)
Spices things up a bit.
Avoid a long boring data description
Highlight interesting trends
Highlight differences in means between groups
Etc.

#### Slide 38

[Embedded image/diagram; no text alternative in the slide text layer.]

Next, describe estimating equation.
Use descriptive figures/statistics as motivation for your regression equation.
Don’t screw up your subscripts!!!  




Finally, use your regression equation as a jumping off point to discuss right-hand side variables
First, describe primary right-hand side variable of interest.  
After that, discuss the other Xs.
List their sources and other pertinent information in footnotes.  
Sometimes, Xs are not that important.  Presenting them in this manner allows the reader to “choose their own adventure.”
If you want the details, read footnote.
If not, skip it.
Common to see long descriptions of the Xs in a data section before empirical strategy.
I do not recommend this.
Putting this info in main text can disrupt flow of the manuscript

#### Slide 39

Collecting and cleaning data sets

The data collection and cleaning process is where the empirical component of your project begins.

It is also one of the most important.

It does not matter if you are using the newest and fanciest estimator, when you are using it on a data set filled with systematic errors.
This could happen in the data collection phase.
Or in the data cleaning phase

You don’t want to publish a paper, have it cited 1,500 times, and find out 15 years later that your estimates were a product of data transcription or coding errors…

#### Slide 40

[Embedded image/diagram; no text alternative in the slide text layer.]

This table is from a recent submission to a top field journal.  What can be improved?

Producing Effective Tables

#### Slide 41

[Embedded image/diagram; no text alternative in the slide text layer.]

The title could certainly be more informative.

Were these variable names copy and pasted from the Stata output!?

#### Slide 42

[Embedded image/diagram; no text alternative in the slide text layer.]

What is the treatment?  Use the notes to provide some sense of what is being estimated and how.  Notes to tables should allow them to “stand alone.”

In general, the table is too cluttered.  There is too much going on...

Measure in 1000s of $s.  
I don’t need to know the effect down to the penny

#### Slide 43

Table 1. Years of schooling and hourly wages: Sample restricted to Danish respondents |  | 
 |   
              Men                                          Women 
   wages        ln(wages)                wages           ln(wages) | 
Schooling |    5.64            .054*
  (5.11)           (.030) |         7.83**            .094***
    (3.55)            (.007)

Observations | 
   12,385 | 
      12,012
 |  | 
Notes: The sample is restricted to CHAT respondents ages 25-45 from Colorado, Nebraska, and Iowa whose wage was greater than or equal to $3.25 per hour.  Each column reports an estimate from a separate OLS regression of wages on years of schooling.  Controls include race, ethnicity, marital status, union status, years of experience, tenure at current job, and firm size.  Standard errors clustered at the state level are reported in parentheses.  

* Statistically significant at the 10% level; ** at the 5% level; *** at the 1% level.

 |  | 

Period here instead of colon

Now you can use a colon here

Keep the table uncluttered.  Don’t report the estimated coefficients of the controls.

Ideally, you provide enough information in the notes to the table so that the reader can interpret the estimates without having to read the text.

#### Slide 44

Useful Phrases When Describing Regression Results

I find evidence of a negative relationship between years of schooling and earnings. 

The results suggest that schooling is negatively related to earnings. 

Consistent with the hypothesis that educational attainment affects earnings, I find that...

A one-year increase in schooling is associated with a 6.8 percent increase in earnings.

The estimate of β is .068, suggesting that an additional year of schooling leads to a 6.8 percent increase in earnings. 

The estimated effect of an additional year of schooling on earnings is positive 6.8 percent.

#### Slide 45

Not being clear about the units

Focusing solely on statistical significance (i.e., stargazing)
	 It is essential not to confuse the statistical usage of "significant" with 	the everyday usage. In everyday usage, 	"significant" means "of 	practical importance," or simply "important." In statistical usage, 	"significant" means "signifying a characteristic of the population 	from which the sample is drawn," regardless of whether the 	characteristic is important. (Wallis and Roberts 1965, p. 385)

Not characterizing magnitudes
This can be hard, but we will provide some suggestions

Careless interpretations (e.g., do not confuse “percent” and “percentage point”)

Common Pitfalls When Describing Regression Results

#### Slide 46

OLS estimates of the relationship between years of schooling and ln(wages)

                                       Insignificant                                                                          Significant               | 
 
.15 
(.131) |  
.15 
(.033)
 
.0012 
(.003) |  
.0012 
(.00001)
Note: Standard errors are reported in parentheses. 
  | 

BIG

Small

Magnitude and Significance

#### Slide 47

OLS estimates of the relationship between years of schooling and ln(wages)

                                       Insignificant                                                                          Significant               | 
 
.15 
(.131) |  
.15 
(.033)
 
.0012 
(.0033) |  
.0012 
(.00001)
Note: Standard errors are reported in parentheses. 
  | 

BIG

Small

Why would we characterize these estimates as “BIG”?  Big relative to what?

You need some way to anchor your estimate and putting its magnitude into context...

#### Slide 48

OLS estimates of the relationship between years of schooling and ln(wages)

                                       Insignificant                                                                          Significant               | 
 
.15 
(.131) |  
.15 
(.033)
 
.0012 
(.0033) |  
.0012 
(.00001)
Note: Standard errors are reported in parentheses. 
  | 

BIG

Small

Why would we characterize these estimates as “BIG”?  Big relative to what? 

1. Relative to previous estimates of the effect of schooling on wages…Cite the most important studies in this area (e.g., Angrist and Krueger 1991) or refer to a “stylized fact.”

#### Slide 49

OLS estimates of the relationship between years of schooling and ln(wages)

                                       Insignificant                                                                          Significant               | 
 
.15 
(.131) |  
.15 
(.033)
 
.0012 
(.0033) |  
.0012 
(.00001)
Note: Standard errors are reported in parentheses. 
  | 

BIG

Small

Why would we characterize these estimates as “BIG”?  Big relative to what? 

2. Relative to the effect of other policies or to an established benchmark/goal.  For instance, you could note that, according to your estimate, an additional year of schooling for Blacks would reduce the Black-White wage gap by 4 percentage points.

#### Slide 50

OLS estimates of the relationship between years of schooling and ln(wages)

                                       Insignificant                                                                          Significant               | 
 
.15 
(.131) |  
.15 
(.033)
 
.0012 
(.0033) |  
.0012 
(.00001)
Note: Standard errors are reported in parentheses. 
  | 

BIG

Small

Why would we characterize these estimates as “BIG”?  Big relative to what? 

3.  Relative to the mean wage in your sample, or to the change in wages for some time period of interest.

#### Slide 51

OLS estimates of the relationship between years of schooling and ln(wages)

                                       Insignificant                                                                          Significant               | 
 
.15 
(.131) |  
.15 
(.033)
 
.0012 
(.0033) |  
.0012 
(.00001)
Note: Standard errors are reported in parentheses. 
  | 

BIG

Small

Why would we characterize these estimates as “small”?  Small relative to what? 

The above strategies still apply.  Often a good idea to point reader to the upper bound of the 95% CI.  Even the upper bound of the 95% CI is small relative to…

#### Slide 52

Using data on 19- through 22-year-olds and an RD design, Carpenter and Dobkin (2009) found that reaching the minimum legal drinking age was associated with a 21% increase in alcohol consumption and a 15% increase in traffic fatalities

The implied elasticity of traffic fatalities with respect to alcohol consumption from these estimates is 0.71 (i.e., 0.15/0.21)

Restricting our sample to 19- through 22-year-olds, Anderson et al. (2013) found that legalizing medical marijuana was associated with a 15% decrease in alcohol consumption and a 12% decrease in traffic fatalities

The implied elasticity from these estimates is 0.80 (i.e., 0.12/0.15)

References 
Anderson, D. Mark, Benjamin Hansen, and Daniel I. Rees. 2013. “Medical Marijuana Laws, Traffic Fatalities, and Alcohol Consumption.” Journal of Law and Economics, 56 (2): 333-369.

Carpenter, Christopher and Carlos Dobkin. 2009. “The Effect of Alcohol Consumption on Mortality: Regression Discontinuity Evidence from the Minimum Drinking Age.”  AEJ: Applied, 1(1): 164-182.

Sometimes you just want to confirm that your estimates are reasonable…

#### Slide 53

If you torture the data long enough, nature will confess.
			          --Ronald Coase








This is a sad and decidedly unscientific state of affairs we find ourselves in. Hardly anyone takes data analyses seriously. Or perhaps more accurately, hardly anyone takes anyone else's data analyses seriously. Like elaborately plumed birds who have long since lost the ability to procreate but not the desire, we preen and strut and display our t-values.
	                                                       --Edward Leamer (1983, p. 37)

Data Mining

[Embedded image/diagram; no text alternative in the slide text layer.]

[Embedded image/diagram; no text alternative in the slide text layer.]

#### Slide 54

The terminology “data mining” is often used in the context of pre-test bias.  In particular, researchers often run a large number of different regressions on  a body of data looking for significant t statistics (at ,say, the 5% level).  Using this approach invalidates traditional hypothesis-testing procedures because such data mining is likely by chance to uncover significant t statistics; i.e., the final results chosen are much more likely to embody Type I error than the claimed 5%.	
		--From Peter Kennedy’s Guide to Econometrics (4th edtion)

“Classic” data mining or p-hacking

[Embedded image/diagram; no text alternative in the slide text layer.]

#### Slide 55

1.   You only report estimates of the following equation:

      yit = α + β1X1it + β2X2it +… βkXkit + εit .

      However, estimating this equation produces very different results:	
       
      ln(yit)= α + β1X1it + β2X2it +… βkXkit + εit .

To be clear, data mining is not the same thing as doing robustness checks

Let’s consider three signs you may be data mining

#### Slide 56

2.   You only report estimates of the following equation:

      yit = α0 + α1Zit-1 + β1X1it + β2X2it +… βkXkit + εit .

       However, estimating any of these equations produces very different
       results:	

      yit = α0 + α1Zit + β1X1it + β2X2it +… βkXkit + εit 
      yit = α0 + α1Zit-2 + β1X1it + β2X2it +… βkXkit + εit 
      yit = α0 + α1Zit-1+ α2Zit-2 + β1X1it +β2X2it +… βkXkit + εit 
      yit = α0 + α1Zit + α2Zit-1+ α3Zit-2 + β1X1it +β2X2it +… βkXkit + εit .

#### Slide 57

3.   You only report estimates of the following equation for respondents
      ages 12-15:

      yit = α0 + α1Zit-1 + β1X1it + β2X2it +… βkXkit + εit .

       However, you get very different results if your sample includes 
       16-year-olds. 


A few more behaviors that can be described as data mining:

Stop project because of null results
Decide against submitting to a journal  
Modifying your original hypothesis to better match results

#### Slide 58

Why do a sensitivity analysis?
Reassure the reader that you’re not p-hacking
Good empirical work is transparent and easily replicated
Provide a range of estimates
Unless one particular specification is preferred, readers will expect a range of estimates
Often, theory does not dictate specification 
Facilitate comparison of your estimates to those reported by previous researchers

Sensitive to what?
Choice of controls 
Period under examination                 
To weight or not to weight? 
Solon, Gary, Steven J. Haider, and Jeffrey M. Wooldridge. 2015. “What Are We Weighting For?” JHR, 50(2): 301-316.
Unit-specific linear trends
TWFEs with staggered treatment 
Etc. 

The goal is to be thorough without getting bogged down.  It is easy to get bogged down…

Sensitivity Analysis

#### Slide 59

Goal
The goal is to convince the reader that your estimate is causal
Describe the threat to identification 
Describing a variable as “potentially endogenous” is not enough
Describing the threat to identification sets up the falsification test or alternative identification strategy
Be specific!

Falsification tests

[Embedded image/diagram; no text alternative in the slide text layer.]

#### Slide 60

Anderson et al. (2021) estimate a large and precisely measured β1
What might be some threats to identification?  Name the omitted variables!
Maybe CAP laws were passed alongside other gun-related policies
Maybe CAP laws were passed alongside other policies designed to curb general violence among youths
Based on these two potential omitted variables, what might be some outcomes we could consider as falsification tests?

Falsification tests from Anderson et al. (2021) J of Urban Economics

[Embedded image/diagram; no text alternative in the slide text layer.]

[Embedded image/diagram; no text alternative in the slide text layer.]

#### Slide 61

Structure of the Results

When it comes to structing the results section of your paper, you probably should not try to reinvent the wheel.
Descriptive figures/tables
Main results
Extensions
Robustness/sensitivity checks
Heterogeneity analyses
Analyses of supplemental data sets
Etc.

A working paper I just came across…

[Embedded image/diagram; no text alternative in the slide text layer.]

[Embedded image/diagram; no text alternative in the slide text layer.]

5 pages later…

[Embedded image/diagram; no text alternative in the slide text layer.]

2 pages after that…

Main estimates were shown in Table 4 and Figure 10 of the paper

#### Slide 62

The Conclusion

Repeat yourself, but don’t repeat yourself!
Conclusion contains much of what is in the intro, but should be shorter
(i) Open with a general motivation
Use new eye-catching stats to start conclusion
(ii) Reiterate specific motivation for scholars working in this area.
Remind readers of your contribution  
(iii) Describe setting, data, and identification strategy
Briefer than intro 
(vi) Results discussion. 
The one place where I will go into more detail than in the introduction
Feel free to remind the reader of interesting sub-analyses
Heterogeneous effects
Particularly convincing robustness checks
Supplementary data analyses
Etc.

#### Slide 63

The last paragraph of the conclusion is really important. Finish with a bang!
One of the more difficult paragraphs to write in a paper.  It can feel daunting, but you have options…
(i) Opine about future research 
(ii) Highlight a contribution not in your introduction
Historical U.S. studies that relate estimates to developing countries today
“Infant Health and Later-Life Labor Market Outcomes,” Lazuka (2018, J of Human Resources)
(iii) Policy relevance 
“The Impact of Mass Shootings on Gun Policy,” Luca et al. (2020, J of Public Economics)
“Accidents Will Happen? Unintentional Childhood Injuries and the Effects of Child Care Regulations,” Currie and Hotz (2004, J of Health Economics)
(iv) List caveats
Not as big a fan of this.  I like including caveats in the conclusion, but maybe not as your final word.
This paper pulls it off:
“Is Legal Pot Crippling Mexican Drug Trafficking Organisations? The Effect of Medical Marijuana Laws on U.S. Crime,” Gavrilova et al. (2017, Economic Journal)
Or some combination of (i)-(iv)

The Conclusion

#### Slide 64

How long should it be?
Shorter than your introduction.
Usually no more than 2 pages.

The Conclusion

#### Slide 65

Section 3: Presentation

Presentation is a common way of job interview.
Know your audience
Practice
Presenting is 50% entertainment and 50% teaching

Keep your audience engaged and they will remember you

Ask them questions
Teach them something new — anything new — and they will remember you
Show them that you are passionate about the topic.

#### Slide 66

General tips

If you’re overly technical and aim your presentation at a narrow audience, then your presentation will be forgotten

Think about aiming your presentation at the person in the audience LEAST familiar with your area of research 

“Data without context is meaningless; for data to have an impact, it requires a story.” —Schröder et al.(2023)

#### Slide 67

The power of stories

[Embedded image/diagram; no text alternative in the slide text layer.]

During your presentation, you’re going to present more information than anyone can possibly remember

Think of yourself as a storyteller

Numbers and facts w/o context are difficult to remember

Stories are easy to remember

 Your story is the scaffolding upon 
which you fasten numbers and  facts

#### Slide 68

How to effectively convey your story

Limit what you present.  Less is more.

Prime your listeners

Briefly describe your indentification strategy early in your presentation 

Preview your main results

What comes next?  What’s on the next slide?  What is the goal of the next section?

“Next, I am going to try to convince you of X…It’s important that I convince you of X, because…”

#### Slide 69

Why do we use slides?

Don’t use slides to “prove” that you’ve read every paper on…


Don’t use slides to “prove” you did 27 separate robustness checks
Don’t use slides to show descriptive statistics and/or coefficients for each and every variable in your analysis

#### Slide 70

Why do we use slides?

Prompts for you

Numbers that you do not want to memorize

Specific language/phrasing that you might forget

Visual aids for your audience

Figures are, in general, better than tables

Each slide should convey one main point.

#### Slide 71

What should be on your slide?

Try for three lines on each slide

At most one or two sub-points under each line

Keep your language simple

Avoid using jargon
Avoid using too many acronyms 

 “We explore the heterogenous effects of NOTDA and/or YATES exposure by…”  Yuk!!! 

Keep your slides clean and easy to read

#### Slide 72

[Embedded image/diagram; no text alternative in the slide text layer.]

Clean? Easy to read?

NS

#### Slide 73

NS

Pregnant women in these communities are particularly vulnerable

Evidence that in utero exposure to maternal stress can adversely affect infant health (Black et al. 2016)

Short- and long-term negative effects 
Health (Currie et al. 2010)
Labor market outcomes (Black et al. 2007; Oreopoulos et al. 2008)

#### Slide 74

Your introduction

The first five minutes are the most important

Talk about yourself with the title slide up 
How did you become interested in this topic?
Why did this topic engage you?
This is a chance to create your brand
 Every talk is a job talk…

Begin with the “big picture” 
Why should we care about this general topic?

#### Slide 75

Background

Remember, most audience members are not specialists in this area. You know more than they do. 
This is a chance to teach them something new

Try to briefly characterize the state of the literature 
What do we know and what don’t we know?
Show that you’re an expert in this field, broadly defined

#### Slide 76

Data

Collecting unique or difficult-to-find data is rewarded by the profession

Spend some time talking about the data
It is not easy to get the data.

#### Slide 77

Avoid showing a long table full of descriptive statistics

[Embedded image/diagram; no text alternative in the slide text layer.]

#### Slide 78

Tables

Don’t show estimates that you’re not going to talk about

Go through the columns one by one 

Pay attention to the units 
Is that a percentage point change?  Is that a percent change?

Try to mention magnitude  

Take your time!
Leave enough time to savior your results

#### Slide 79

This table is from a seminar I attended before. What can be improved?

[Embedded image/diagram; no text alternative in the slide text layer.]

NS

#### Slide 80

[Embedded image/diagram; no text alternative in the slide text layer.]

This is what it looked like from the back of the room.

#### Slide 81

What can be improved?

[Embedded image/diagram; no text alternative in the slide text layer.]

#### Slide 82

Series First Trimester: -11.06, -38.0, -11.24

Series Column1: None, None, None

Series Third Trimester: None, None, None

1st trimester

[Embedded image/diagram; no text alternative in the slide text layer.]

Plots or charts are usually better than tables

#### Slide 83

Series First Trimester: -11.06, -38.0, -11.24

Series Column1: None, None, None

Series Third Trimester: None, None, None

1st trimester

[Embedded image/diagram; no text alternative in the slide text layer.]

#### Slide 84

Giving the presentation

Grab people’s attention
Convey the most important information right away
Pausing is important
Try to end with something actionable

#### Slide 85

Handling questions

Don’t be afraid to say “I don’t know”

Don’t be afraid to ask if you don’t understand a question
“I agree that Z is a determinant of Y, but why would it be systematically correlated with treatment in my context?”

### lectures_review(1).pdf — 114 pages (0 without PDF text)

#### Page 1

Probability and Regression Review
Yumin Li
SILC, SHU
September 9, 2021

#### Page 2

Outline
Probability and statistics review
Deﬁnition of the simple regression model
Deriving the Ordinary Least Squares Estimates
Properties of OLS on any Sample of Data
Units of Measurement and Functional Form
Expected Values and Variance of OLS

#### Page 3

Outline
Probability and statistics review
Deﬁnition of the simple regression model
Deriving the Ordinary Least Squares Estimates
Properties of OLS on any Sample of Data
Units of Measurement and Functional Form
Expected Values and Variance of OLS

#### Page 4

Summation Operator
n∑
i=1
xi≡ x1 + x2 +... + xn (1)
Properties of the Summation operator
1. For any constant c:
n∑
i=1
c = nc (2)
2. n∑
i=1
cxi = c
n∑
i=1
xi (3)
3. For any constant a and b:
n∑
i=1
(axi + byi) = a
n∑
i=1
xi + b
n∑
i=1
yi (4)

#### Page 5

Note that: n∑
i=1
xi
yi
̸=
∑n
i=1 xi∑n
i=1 yi
(5)
and
n∑
i=1
x 2
i ̸=
( n∑
i=1
xi
)2
(6)
The average (mean) can be computed as
x = 1
n
n∑
i=1
xi = x1 + x2 +... + xn
n (7)
and the sum of the deviations from the mean:
n∑
i=1
(xi− x) = 0 (8)

#### Page 6

One result that will be very useful throughout the semester is:
n∑
i=1
(xi− x)2 =
n∑
i=1
x 2
i − n(x)2 (9)
and more generally,
n∑
i=1
(xi−x)(yi−y) =
n∑
i=1
xi(yi−y) =
n∑
i=1
(xi−x)yi =
n∑
i=1
xiyi−n(xy)
(10)

#### Page 7

Linear Functions
Remember that you can describe a straight line in a graph as:
y =β0 +β1x (11)
where β0 is the intercept (where the line crosses the y-axis) and
β1 is the slope of the line.
▶ A change in y is always β1 times a change in x:
∆y =β1∆x (12)
▶ It is simple to extend this to the case with several variables:
y =β0 +β1x1 +β2x2 (13)
∆y =β1∆x1 +β2∆x2 (14)

#### Page 8

Note that:
▶ β0 is still the intercept (when x1=0 and x2 = 0)
▶ If x2 is held ﬁxed (∆x2 = 0), the change in y is given by
β1∆x1
▶ β1 is called the partial eﬀect of x1 on y since it measures
how y changes with x1, holding x2 ﬁxed. Hence, the partial
eﬀect is very closely related to the notion of ceteris paribus.
β1 = ∆y
∆x1
(15)
if ∆x2 = 0.

#### Page 9

Expected Value
The expected value of a random variable, also called the
expectation and sometimes the population mean is simply the
weighted average of the possible values that variable can take, with
the weights being given by the probability of each value
”happening” in the population.
▶ Suppose the variable X can take values x1, x2,..., xk with
probability f (x1), f (x2),..., f (xk), respectively.
E (X ) = x1f (x1) +x2f (x2) +... + xkf (xk) =
k∑
j=1
xjf (xj) (16)

#### Page 10

For Example: if X takes on values −1, 0, and 2 with probabilities
0.3, 0.3, and 0.4, respectively,
E (X ) = ( −1)(0.3) + (0)(0.3) + (2)(0.4)
= 0 .5 (17)
In fact you could take the expectation of a function of that
variable, say X 2. Note that X 2 take only the values 0, 1 and 4
with probabilities 0.3, 0.3, and 0.4,
E (X 2) = ( −1)2(0.3) + (0)2(0.3) + (2)2(0.4)
= 0 .3 + 0 + 1.6
= 1 .9 (18)

#### Page 11

Properties of Expected Values
1. For any constant c, E (c) = c
2. For any constants a and b,
E (aX + b) = E (aX ) + E (b) = aE (X ) + b
3. If we have many constants, a1, a2,..., an, and many random
variables, X1, X2,..., Xn, then
E (a1X1+a2X2+... +anXn) = a1E (X1)+a2E (X2)+... +anE (Xn)
(19)
Using summation notation,
E
( n∑
i=1
aiXi
)
=
n∑
i=1
aiE (Xi) (20)
In the special case in which each ai = 1:
E
( n∑
i=1
Xi
)
=
n∑
i=1
E (Xi) (21)

#### Page 12

Outline
Probability and statistics review
Deﬁnition of the simple regression model
Deriving the Ordinary Least Squares Estimates
Properties of OLS on any Sample of Data
Units of Measurement and Functional Form
Expected Values and Variance of OLS

#### Page 13

OLS review
▶ We begin with cross-sectional analysis and will (eventually)
assume we can collect a random sample from the population
of interest.
▶ We begin with the following premise, once we have a
population in mind. There are two variables, x and y, and we
would like to “study how y varies with changes in x”.
▶ We have seen examples: x is amount of fertilizer, and y is
soybean yield. x is years of schooling, y is hourly wage.

#### Page 14

We must confront three issues:
1. How do we allow factors other than x to aﬀect y? There is
never an exact relationship between two variables (in
interesting cases).
2. What is the functional relationship between y and x?
3. How can we be sure we are capturing a ceteris paribus
relationship between y and x (as is so often the goal)?

#### Page 15

Consider the following equation relating y to x:
y =β0 +β1x + u, (22)
which is assumed to hold in the population of interest .
This equation deﬁnes the simple linear regression model (or
two-variable regression model).
▶ y and x are not treated symmetrically. We want to explain y
in terms of x. From a causality standpoint, it makes no sense
to “explain” past educational attainment in terms of future
labor earnings. We want to explain student performance ( y)
in terms of class size ( x).

#### Page 16

Terminology
y x
Dependent Variable Independent Variable
Explained Variable Explanatory Variable
Response Variable Control Variable
Predicted Variable Predictor Variable
Regressand Regressor
The terms “explained” and “explanatory” are probably best, as
they are the most descriptive and widely applicable. But
“dependent” and “independent” are used often. (The
“independence” here is not really statistical independence.)

#### Page 17

We mentioned the error term or disturbance, u, before. The
equation
y =β0 +β1x + u, (23)
explicitly allows for other factors, contained in u, to aﬀect y.
▶ This equation also addresses the functional form issue (in a
simple way). Namely, y is assumed to be linearly related to x.
We call β0 the intercept parameter and β1 the slope
parameter. These describe a population, and our ultimate
goal is to estimate them.
▶ In the simple regression framework, we eﬀectively treat all
factors aﬀecting y,other than x as being unobserved.

#### Page 18

The equation also addresses the ceteris paribus issue. In
y =β0 +β1x + u, (24)
all other factors that aﬀect y are in u. We want to know how y
changes when x changes, holding u ﬁxed . Let ∆ denote “change”.
Then holding u ﬁxed means ∆ u = 0. So
∆y =β1∆x + ∆u =β1∆x when ∆u = 0 (25)
This equation eﬀectively deﬁnes β1 as a slope, with the only
diﬀerence being the (very important) caveat ∆ u = 0.

#### Page 19

EXAMPLE: Yield and Fertilizer
yield =β0 +β1fertilizer + u (26)
where u contains things such as land quality, rainfall on a plot of
land, and so on. The slope parameter, β1, is of primary interest: it
tells us how yield will change when the amount of fertilizer
changes, holding all else ﬁxed.
Note: The linear function is probably not realistic here. The eﬀect
of fertilizer is likely to diminish at large amounts of fertilizer.

#### Page 20

EXAMPLE: Wage and Education
wage =β0 +β1educ + u (27)
where u contains somewhat nebulous factors (“ability”) but, also,
past workforce experience and tenure on the current job.
∆wage =β1∆educ when ∆u = 0 (28)
Is each year of education really worth the same dollar amount no
matter how much education one starts with?
▶ The linear form of the equation being used implies that a
one-unit change in x has the same impact on y regardless of
the initial value of x. That is not very realistic in several
applied cases.

#### Page 21

▶ This seems too easy! How can we hope to generally estimate
the ceteris paribus eﬀect of y on x when we have assumed all
other factors aﬀecting y are unobserved and lumped into u?
▶ The key is that the SLR model is a population model. When
it comes to estimating β1 (and β0) using a random sample of
data, we must restrict how u and x are related to each other.
▶ But x and u are properly viewed as having distributions in the
population. For example, if x = educ then, in principle, we
could ﬁgure out its distribution in the population just over,
say, 30 years old. Suppose u is cognitive ability. Assuming we
can measure what that means, it also has a distribution in the
population.
▶ What we must do is restrict the way u and x relate to each
other in the population.

#### Page 22

First, we make a simplifying assumption (without loss of
generality): the average, or expected, value of u is zero in the
population:
E (u) = 0 (29)
where E (·) is the expected value operator.
Normalizing land quality or “ability” to be zero in the population is
harmless.

#### Page 23

The presence of β0 in
y =β0 +β1x + u (30)
allows us to assume E (u) = 0. If the average of u is diﬀerent from
zero, say α0, we just adjust the intercept, leaving the slope the
same:
y = (β0 +α0) +β1x + (u−α0) (31)
where α0 = E (u). The new error is u−α0 and the new intercept
is β0 +α0. The important point is that the slope, β1, has not
changed.

#### Page 24

▶ The expectation operator, E (·), is a population concept, i.e.,
it refers to the whole group of interest, not only the sample
we have available to us.
▶ Its intuition is (loosely) similar to the average of a random
variable, in the population.
▶ Useful facts about the expectation operator for W and H any
random variables: Note that E (W ) is constant.
E (c) = c for any constant c (32)
E (aW + b) = aE (W ) + b for any constants a, b. (33)
E (W + H) = E (W ) + E (H) (34)
E (W− E (W )) = 0 (35)

#### Page 25

As you have seen before, we use the variance of a random variable,
say W as a measure of its dispersion.
Var(W ) =σ2 = E
[
(W− E (W ))2]
in the population (36)
Var(aW + b) = a2Var(W ) for any constants a, b. (37)
We can show that:
Var(W ) = E
(
W 2)
− [E (W )]2 (38)
In a given sample, we estimate the variance by:
S 2 = (n− 1)−1
n∑
i=1
(xi− x)2 (39)

#### Page 26

Usually, we are interested in how two variables are related to each
other. One important measure of the (linear) connection between
them is given by the covariance.
▶ In the population the covariance between X and Y is given by:
Cov(X, Y ) = E (XY )− E (X )E (Y ) (40)
▶ Then, if E (X ) or E (Y ) equals zero, Cov(X, Y ) = E (XY ).
▶ If X and Y are independent Cov(X, Y ) = 0, also,
Cov(aX + b, cY + d) = acCov(X, Y )

#### Page 27

▶ The covariance measures the amount of linear dependence
between two random variables. Cov(X, Y )> 0 indicates that
the random variables move in the same direction.
Cov(X, Y )< 0 that they move in opposite directions.
▶ Zero covariance does NOT imply that the variables are not
related (there could be a nonlinear connection between them)
▶ Interpreting the magnitude of the covariance can be tricky.
For that, we are better served by looking at correlation:
Corr(X, Y ) = Cov(X, Y )√
Var(X )Var(Y )
(41)
▶ The Correlation coeﬃcient is between −1 and 1. A positive
(negative) correlation indicates that the variables move in the
same (opposite) ways. The closer to 1 or −1 the strongest
the linear relationship is.

#### Page 28

An assumption that meshes well with our introductory treatment
involves the mean of the error term for each “slice” of the
population determined by values of x:
E (u|x) = E (u), all values x (42)
where E (u|x) means “the expected value of u given x”.
Then, we say u is mean independent of x.

#### Page 29

▶ Suppose u is “ability” and x is years of education. We need,
for example,
E (ability|x = 8) = E (ability|x = 12) = E (ability|x = 16)
so that the average ability is the same in the diﬀerent portions
of the population with an 8 th grade education, a 12 th grade
education, and a four-year college education.
Because people choose education levels partly based on
ability, this assumption is almost certainly false.
▶ Suppose u is “land quality” and x is fertilizer amount. Then
E (u|x) = E (u) if fertilizer amounts are chosen independently
of quality.

#### Page 30

Combining E (u|x) = E (u) (the substantive assumption) with
E (u) = 0 (a normalization) gives
E (u|x) = 0, all values x (43)
Called the zero conditional mean assumption . Because the
conditional expected value is a linear operator, E (u|x) = 0 implies
E (y|x) =β0 +β1x (44)
which shows the population regression function (or population
expectation function) is a linear function of x.

#### Page 31

EXAMPLE: College versus High School GPA.
Consider the population of students attending a university. We
assume β0 = 1.5 and β1 = 0.5 are known (which is unrealistic).
E (colGPA|hsGPA) = 1.5 + 0.5 hsGPA (45)
If hsGPA = 3.6 then the average colGPA among students with this
particular high school GPA is
1.5 + 0.5(3.6) = 3.3 (46)
▶ Anyone with hsGPA = 3.6 most likely will not have colGPA
= 3.3. This is the expected college GPA based on averages
within that slice of the population.
▶ Regression analysis is essentially about explaining eﬀects on
averages.

#### Page 32

Outline
Probability and statistics review
Deﬁnition of the simple regression model
Deriving the Ordinary Least Squares Estimates
Properties of OLS on any Sample of Data
Units of Measurement and Functional Form
Expected Values and Variance of OLS

#### Page 33

Deriving the Ordinary Least Squares Estimates
▶ Given data on x and y, how can we estimate the population
parameters,β0 and β1?
▶ Let{(xi, yi) : i = 1, 2,..., n} be a random sample of size n
(the number of observations) from the population.
▶ Plug any observation into the population equation:
yi =β0 +β1xi + ui (47)
where the i subscript indicates a particular observation.
▶ We observe yi and xi, but not ui (but we know it is there).

#### Page 34

We use the two population restrictions:
E (u) = 0 (48)
Cov(x, u) = 0 (49)
to obtain estimating equations for β0 and β1. We talked about the
ﬁrst condition. The second condition means that x and u are
uncorrelated. Both conditions are implied by
E (u|x) = 0 (50)

#### Page 35

With E (u) = 0, Cov(x, u) = 0 is the same as E (xu) = 0. Next we
plug in for u:
E (y−β0−β1x) = 0 (51)
E [x(y−β0−β1x)] = 0 (52)
These are the two conditions in the population that eﬀectively
determine β0 and β1.

#### Page 36

So we use their sample counterparts (which is a method of
moments approach to estimation):
n−1
n∑
i=1
(yi− ˆβ0− ˆβ1xi) = 0 (53)
n−1
n∑
i=1
xi(yi− ˆβ0− ˆβ1xi) = 0 (54)
where ˆβ0 and ˆβ1 are the estimates from the data.
These are two linear equations in the two unknowns ˆβ0 and ˆβ1.

#### Page 37

Pass the summation operator through the ﬁrst equation:
n−1
n∑
i=1
(yi− ˆβ0− ˆβ1xi) (55)
= n−1
n∑
i=1
yi− n−1
n∑
i=1
ˆβ0− n−1
n∑
i=1
ˆβ1xi (56)
= n−1
n∑
i=1
yi− ˆβ0− ˆβ1
(
n−1
n∑
i=1
xi
)
(57)
= y− ˆβ0− ˆβ1x (58)
We use the standard notation y = n−1 ∑n
i=1 yi for the average of
the n numbers{yi : i = 1, 2,..., n}. For emphasis, we call y a
sample average.

#### Page 38

We have shown that the ﬁrst equation,
n−1
n∑
i=1
(yi− ˆβ0− ˆβ1xi) = 0 (59)
implies
y = ˆβ0 + ˆβ1x (60)
Now, use this equation to write the intercept in terms of the slope:
ˆβ0 = y− ˆβ1x (61)

#### Page 39

Plug this into the second equation (but where we take away the
division by n):
n∑
i=1
xi(yi− ˆβ0− ˆβ1xi) = 0 (62)
so
n∑
i=1
xi[yi− (y− ˆβ1x)− ˆβ1xi] = 0 (63)
Simple algebra gives
n∑
i=1
xi(yi− y) = ˆβ1
[n∑
i=1
xi(xi− x)
]
(64)

#### Page 40

So, the equation to solve is
n∑
i=1
(xi− x)(yi− y) = ˆβ1
[n∑
i=1
(xi− x)2
]
(65)
If ∑n
i=1(xi− x)2̸= 0, we can write
ˆβ1 =
∑n
i=1(xi− x)(yi− y)∑n
i=1(xi− x)2 = Sample Covariance(xi, yi)
Sample Variance(xi) (66)

#### Page 41

▶ The previous formula for ˆβ1 is important. It shows us how to
take the data we have and compute the slope estimate. For
reasons we will see, ˆβ1 is called the ordinary least squares
(OLS) slope estimate.
▶ It can be computed whenever the sample variance of the xi is
not zero, which only rules out the case where each xi has the
same value. The intuition is that the variation in x is what
permits us to identify its impact in y.

#### Page 42

▶ Once we have ˆβ1, we compute ˆβ0 = y− ˆβ1x. This is the OLS
intercept estimate.
▶ These days, we let the computer do the calculations, which
are tedious even if n is small.
▶ Where does the name “ordinary least squares” come from?
For any candidates ˆβ0 and ˆβ1, deﬁne a ﬁtted value for each i
as
ˆyi = ˆβ0 + ˆβ1xi (67)
We have n of these. This is the value we predict for yi given
that x = xi.

#### Page 43

▶ The “mistake” we make is the residual:
ˆui = yi− ˆyi = yi− ˆβ0− ˆβ1xi (68)
Suppose we measure the size of the mistake, for each i, by
squaring it. Then we add them all up:
n∑
i=1
ˆu2
i =
n∑
i=1
(yi− ˆβ0− ˆβ1xi)2 (69)
This is called the sum of squared residuals . Finally, choose
ˆβ0 and ˆβ1 to minimize the sum of squared residuals. Using
calculus (or other arguments), it can be shown that the
solutions are the same we obtained before.

#### Page 44

Once we have the numbers ˆβ0 and ˆβ1 for a given data set, we
write the OLS regression line :
ˆy = ˆβ0 + ˆβ1x (70)
which allows us to predict y for any (sensible) value of x. Also
called the sample regression function .
▶ The intercept, ˆβ0, is the predicted y when x = 0, if x = 0
makes sense.
▶ The slope, ˆβ1, allows us to predict changes in y for any
(reasonable) change in x:
∆ˆy = ˆβ1∆x (71)
If ∆x = 1, so x increases by one unit, ∆ˆy = ˆβ1.

#### Page 45

EXAMPLE: Eﬀects of Education on Hourly Wage (WAGE2.DTA)
▶ wage is in dollars per hour. Data are from 1991. educ is
highest grade completed. Only men.
▶ At ﬁrst the negative intercept is a puzzle: wage is predicted to
be $− 0.90 when educ = 0! But the smallest amount of
education in the sample is educ = 8.
▶ Each additional year of schooling is estimated to be worth
about $0.54 per hour in increased wages.
ˆwage =−0.90 + 0.54 educ
n = 526

#### Page 46

▶ In the Stata output, ˆβ0 =−0.90 is the coeﬃcient labeled
“cons.” ˆβ1 = 0.54 is the coeﬃcient labeled “educ”.
▶ We will learn about the other numbers as we go.
▶ General form of the Stata command:
reg y x
The order of y and x is critical! The ﬁrst variable in the
command is taken to be the dependent variable, while the
subsequent are the independent variables.

#### Page 47

Reminder: When we write
wage =β0 +β1educ + u (72)
in the population, we do not know β0 and β1. ˆβ0 =−0.90 and
ˆβ1 = 0.54 are our estimates from this sample of 526 men. They
may or may not be close to the population values.
The function
ˆwage =−0.90 + 0.54 educ (73)
is the OLS regression line. When educ = 8,
ˆwage =−0.90 + 0.54(8) = $3.42per hour (74)

#### Page 48

EXAMPLE: Eﬀects of Campaign Spending on Voting Outcomes
(VOTE.DTA)
▶ Two-party vote, Congressional elections. voteA is the
percentage of votes received by candidate A; shareA is the
percentage share of campaign spending.
▶ Then, y = voteA, x = shareA.

#### Page 49

ˆvoteA = 26.81 + 0.464 shareA (75)
n = 173 (76)
▶ If shareA = 50, ˆvoteA = 26.81 +.0464(50) = 50.01,or
essentially 50 percent of the votes.
▶ If shareA increases by 10 - which means 10 percentage points
- voteA is predicted to increase by 4.64 percentage points in
votes cast to that candidate.
▶ Is this eﬀect causal? Or, do better candidates spend more
(more contributions?)? As with simple correlation analysis,
these simple regression estimates do not allow us to tell.

#### Page 50

Outline
Probability and statistics review
Deﬁnition of the simple regression model
Deriving the Ordinary Least Squares Estimates
Properties of OLS on any Sample of Data
Units of Measurement and Functional Form
Expected Values and Variance of OLS

#### Page 51

▶ Most residuals will be diﬀerent from zero (they do not lie on
the regression line). Some will be positive, others negative.
▶ Positive residuals indicate that the regression line (and, hence,
the predicted values) underestimate the value of yi, similarly if
ˆui is negative, the line overestimate.

#### Page 52

Algebraic Properties of OLS Statistics
Remeber how we obtained ˆβ0 and ˆβ1. When an intercept is
included, we will have:
n∑
i=1
(yi− ˆβ0− ˆβ1xi) = 0 (80)
The OLS residuals always add up to zero, by construction,
n∑
i=1
ˆui = 0 (81)
Because yi = ˆyi + ˆui by deﬁnition,
n−1
n∑
i=1
yi = n−1
n∑
i=1
ˆyi + n−1
n∑
i=1
ˆui (82)
and so y = ˆy.

#### Page 53

Similarly y the way we obtained our estimates,
n∑
i=1
xi(yi− ˆβ0− ˆβ1xi) = 0 (83)
The sample covariance (and therefore the sample correlation)
between the explanatory variables and the residuals is always zero:
n∑
i=1
xi ˆui = 0 (84)
Because the ˆyi are linear functions of the xi, the ﬁtted values and
residuals are uncorrelated, too:
n∑
i=1
ˆyi ˆui = 0 (85)

#### Page 54

Both properties hold by construction. ˆβ0 and ˆβ1 were chosen to
make them true.
A third property is that the point ( x, y) is always on the OLS
regression line. That is, if we plug in the average for x, we predict
the sample average for y:
y = ˆβ0 + ˆβ1x (86)
Again, we chose the estimates to make this true.

#### Page 55

Goodness-of-Fit
▶ For each observation, write
yi = ˆyi + ˆui (87)
▶ Deﬁne the total (SST), explained (SSE) and residual (SSR)
sum of squares as
SST =
n∑
i=1
(yi− y)2 (88)
SSE =
n∑
i=1
(ˆyi− y)2 (89)
SSR =
n∑
i=1
ˆu2
i (90)

#### Page 56

These are sample variances when divided by n (or n− 1). SST
n−1 is
the sample variance of yi, SSE
n−1 is the sample variance of ˆyi, and
SSR
n−1 is the sample variance of ˆui.
By writing
SST =
n∑
i=1
(yi− y)2 =
n∑
i=1
[(yi− ˆyi) + (ˆyi− y)]2 (91)
=
n∑
i=1
[ˆui + (ˆyi− y)]2 (92)
and using that the ﬁtted values and residuals are uncorrelated, can
show
SST = SSE + SSR (93)

#### Page 57

Assuming SST > 0, we can deﬁne the fraction of the total
variation in yi that is explained by xi (or the OLS regression line) as
R 2 = SSE
SST = 1− SSR
SST (94)
Called the R-squared of the regression.
It can be shown to be equal to the square of the correlation
coeﬃcient between yi and ˆyi. Therefore,
0≤ R 2≤ 1 (95)

#### Page 58

▶ R 2 = 0 means no linear relationship between yi and xi.
R 2 = 1 means a perfect linear relationship.
▶ As R 2 increases, the yi are closer and closer to falling on the
OLS regression line.
▶ Do not want to ﬁxate on R 2. It is a useful summary measure
but does not tell us about causality.

#### Page 59

Outline
Probability and statistics review
Deﬁnition of the simple regression model
Deriving the Ordinary Least Squares Estimates
Properties of OLS on any Sample of Data
Units of Measurement and Functional Form
Expected Values and Variance of OLS

#### Page 60

Units of Measurement and Functional Form
Two important issues need to be understood:
▶ How changing the units of measurement of the dependent
and/or independent variable aﬀects OLS estimates? It is very
important to know how y and x are measured in order to
interpret regression functions.
▶ How to incorporate functional forms usually used in economics
into regression analysis.

#### Page 61

Consider an equation estimated from CEOSAL.DTA, where annual
CEO salary is in thousands of dollars and the return on equity is a
percent:
ˆsalary = 963.191 + 18.501 roe (96)
n = 209, R 2 =.0132 (97)
When roe = 0 (it never is in the data), ˆsalary = 963.191. But
salary is in thousands of dollars, so $963,191.
A one percentage point (unit) increase in roe increases predicted
salary by 18.501, or $18,501.

#### Page 62

What if we measure roe as a decimal, rather than a percent?
Deﬁne
roedec = roe/100 (98)
What will happen to the intercept, slope, and R 2 when we regress
salary on roedec?
Nothing should happen to the intercept: roedec = 0 is the same as
roe = 0. But the slope will increase by 100. The goodness-of-ﬁt
should not change, and it does not.

#### Page 63

The new regression is
ˆsalary = 963.191 + 1, 850.1 roedec (99)
n = 209, R 2 =.0132 (100)
Now a one percentage point change in roe is the same as
∆roedec =.01, and so we get the same eﬀect as before. Note
that,
y =β0 +β1x + u⇒ y =β0 +
(β1
c
)
(cx) + u (101)
then the slope is scaled so to preserve the relationship between
variables.

#### Page 64

What if we measure salary in dollars, rather than thousands of
dollars?
salarydol = 1, 000· salary (102)
Both the intercept and slope get multiplied by 1,000.
ˆsalarydol = 963, 191 + 18, 501 roe (103)
n = 209, R 2 =.0132 (104)
Note that,
y =β0 +β1x + u⇒ cy = cβ0 + (cβ1)x + cu (105)

#### Page 65

Using the Natural Logarithm in Simple Regression
Recall the wage example:
ˆwage =−0.90 + 0.54 educ (106)
n = 526, R 2 =.133 (107)
Might be an okay approximation, but unsatisfying for a couple of
reasons:
▶ The negative intercept is a bit strange (even though the
equation gives sensible predictions for education ranging from
8 to 20).
▶ More importantly, the dollar value of another year of schooling
is constant. So the 16 th year of education is worth the same
as the second. We expect additional years of schooling to be
worth more, in dollar terms, than previous years.

#### Page 66

How can we incorporate an increasing eﬀect? One way is to
postulate a constant percentage eﬀect. We can approximate this
using the natural log (ln, but many uses log).
Let the dependent variable be ln( wage):
ln(wage) =β0 +β1educ + u (108)
Holding u ﬁxed,
∆ ln(wage) =β1∆educ (109)
so
β1 = ∆ ln(wage)
∆educ (110)

#### Page 67

Useful result from calculus:
100· ∆ ln(wage)≈ %∆wage (111)
This means when ln( wage) =β0 +β1educ + u, we have a simple
interpretation of β1:
100β1≈ %∆wage when ∆educ = 1 (112)
If we do not multiply by 100, we have the decimal version (the
proportionate change).
In this example, 100β1 is often called the return to education (just
like an investment). This measure is free of units of measurement
(currency, price level).

#### Page 68

▶ We still want to explain wage in terms of educ! This gives us
a way to get a (rough) constant percentage eﬀect.
▶ Predicting wage is more complicated but not usually the
interesting question.
Using WAGE2.DTA. Have to deﬁne ln( wage) in general.
ˆln(wage) = 0.58 +.082 educ (113)
n = 526, R 2 =.1858 (114)
The estimated return to each year of education is about 8 .2%.
Warning: This R-squared is not directly comparable to the
R-squared when wage is the dependent variable.

#### Page 69

We can use the ln on both sides of the equation to get constant
elasticity models. For example, if
ln(salary) =β0 +β1 ln(sales) + u (115)
then,
β1≈ %∆salary
%∆sales (116)
The elasticity is free of units of salary and sales.
A constant elasticity model for salary and sales makes more sense
than a constant dollar eﬀect.

#### Page 70

▶ The estimated elasticity of CEO salary with respect to ﬁrms
sales is about 0.257. (Note that we do not need to even know
salary is in thousands of dollars while sales is in millions.)
▶ A 10 percent increase in sales is associated with (causes?)
about a
0.257(10) = 2.57 (117)
percent increase in salary.

#### Page 71

Model Dep. Var. Indep. Var. β1
Level-Level y x ∆y =β1∆x
Level-Log y ln(x) ∆y = (β1/100)%∆x
Log-Level ln(y) x %∆y = (100β1)∆x
Log-Log ln(y) ln(x) %∆y =β1%∆x
The possibility of using the natural log to get nonlinear
relationships between y and x raises a question: What do we mean
now by ”linear” regression? The answer is that the model is linear
in the parameters, β0 and β1. We can use any transformations of
the dependent and independent variables to get interesting
interpretations for the parameters.

#### Page 72

Outline
Probability and statistics review
Deﬁnition of the simple regression model
Deriving the Ordinary Least Squares Estimates
Properties of OLS on any Sample of Data
Units of Measurement and Functional Form
Expected Values and Variance of OLS

#### Page 73

▶ Mathematical statistics: How do our estimators behave across
diﬀerent samples of data? On average, would we get the right
answer if we could repeatedly sample?
▶ We need to ﬁnd the expected value of the OLS estimators –
in eﬀect, the average outcome across all possible random
samples – and determine if we are right on average.
▶ Leads to the notion of unbiasedness, which is a “desirable”
characteristic for estimators.
E ( ˆβ) =β (118)

#### Page 74

▶ Remember, our objective is to estimate β1, the slope
population parameter that describes the relationship between
y and x.
▶ ˆβ1 is an estimator of that parameter obtained for a speciﬁc
sample.
▶ Diﬀerent samples will generate diﬀerent estimates ( ˆβ1) for the
“true” β1, i.e. ( ˆβ1) is a random variable.
▶ Unbiasedness is the idea that if we could take as many
random samples on Y as we want from the population, and
compute an estimate each time, the average of these
estimates would be equal to β1.

#### Page 75

Assumption SLR.1 (Linear in Parameters)
▶ The population model can be written as
y =β0 +β1x + u (119)
where β0 and β1 are the (unknown) population parameters.
▶ We view x and u as outcomes of random variables; thus, y is
random.
▶ Stating this assumption formally shows that our goal is to
estimate β0 and β1.

#### Page 76

Assumption SLR.2 (Random Sampling)
▶ We have a random sample of size n,{(xi, yi) : i = 1,..., n},
following the population model.
▶ We know how to use this data to estimate β0 and β1 by OLS.
▶ Because each i is a draw from the population, we can write,
for each i,
yi =β0 +β1xi + ui (120)
▶ Notice that ui here is the unobserved error for observation i.
It is not the residual that we compute from the data!

#### Page 77

Assumption SLR.3 (Sample Variation in the Explanatory
Variable)
▶ The sample outcomes on xi are not all the same value.
▶ This is the same as saying the sample variance of
{xi : i = 1,..., n} is not zero.
▶ In practice, this is no assumption at all. If the xi are all the
same value, we cannot learn how x aﬀects y in the population.

#### Page 78

Assumption SLR.4 (Zero Conditional Mean)
▶ In the population, the error term has zero mean given any
value of the explanatory variable:
E (u|x) = E (u) = 0. (121)
▶ This is the key assumption for showing that OLS is unbiased,
with the zero value not being important once we assume
E (u|x) does not change with x.
▶ Note that we can compute the OLS estimates whether or not
this assumption holds, or even if there is an underlying
population model.

#### Page 79

How do we show ˆβ1 is unbiased for β1? What we need to show is
E ( ˆβ1) =β1 (122)
where the expected value means averaging across random samples.
Step 1: Write down a formula for ˆβ1. It is convenient to use
ˆβ1 =
∑n
i=1(xi− x)yi∑n
i=1(xi− x)2 (123)
which is one of several equivalent forms.

#### Page 80

It is convenient to deﬁne SSTx = ∑n
i=1(xi− x)2, to total variation
in the xi, and write
ˆβ1 =
∑n
i=1(xi− x)yi
SSTx
(124)
Remember, SSTx is just some positive number. The existence of
ˆβ1 is guaranteed by SLR.3.
Step 2: Replace each yi with yi =β0 +β1xi + ui (which uses
SLR.1 and the fact that we have data from SLR.2).

#### Page 81

The numerator becomes
n∑
i=1
(xi− x)yi =
n∑
i=1
(xi− x)(β0 +β1xi + ui) (125)
=β0
n∑
i=1
(xi− x) +β1
n∑
i=1
(xi− x)xi +
n∑
i=1
(xi− x)ui (126)
= 0 +β1
n∑
i=1
(xi− x)2 +
n∑
i=1
(xi− x)ui (127)
=β1SSTx +
n∑
i=1
(xi− x)ui (128)
We used ∑n
i=1(xi− x) = 0 and ∑n
i=1(xi− x)xi = ∑n
i=1(xi− x)2.

#### Page 82

We have shown
ˆβ1 = β1SSTx + ∑n
i=1(xi− x)ui
SSTx
=β1 +
∑n
i=1(xi− x)ui
SSTx
(129)
Note how the last piece is the slope coeﬃcient from the OLS
regression of ui on xi, i = 1,..., n. We cannot do this regression
because the ui are not observed.

#### Page 83

Now deﬁne
wi = (xi− x)
SSTx
(130)
so we have
ˆβ1 =β1 +
n∑
i=1
wiui (131)
▶ ˆβ1 is a linear function of the unobserved errors, ui. The wi are
all functions of {x1, x2,..., xn}.
▶ The (random) diﬀerence between ˆβ1 and β1 is due to this
linear function of the unobservables.

#### Page 84

Step 3: Find E ( ˆβ1).
▶ Under Assumptions SLR.2 and SLR.4, E (ui|x1, x2,..., xn) = 0.
That means, conditional on{x1, x2,..., xn},
E (wiui|x1, x2,..., xn) = wiE (ui|x1, x2,..., xn) = 0
because wi is a function of {x1, x2,..., xn}. (In the next slides I
omit the conditioning in the expectations)
▶ This would not be true if, in the population, u and x are
correlated.

#### Page 85

Now we can complete the proof: conditional on {x1, x2,..., xn},
E ( ˆβ1) = E
(
β1 +
n∑
i=1
wiui
)
(132)
=β1 +
n∑
i=1
E (wiui) =β1 +
n∑
i=1
wiE (ui) (133)
=β1 (134)
Remember, β1 is the ﬁxed constant in the population. The
estimator, ˆβ1, varies across samples and is the random outcome:
before we collect our data, we do not know what ˆβ1 will be.

#### Page 86

THEOREM (Unbiasedness of OLS)
Under Assumptions SLR.1 through SLR.4
E ( ˆβ0) =β0 and E ( ˆβ1) =β1. (135)
▶ Omit the proof for ˆβ0.
▶ Now simulate data in Stata:
y = 3 + 2x + u (136)
▶ x∼ Normal(0, 9), u∼ Normal(0, 36), and they are
independent.

#### Page 87

▶ Each sample leads to a diﬀerent estimate, ˆβ0 and ˆβ1. Some
will be very close to the true values β0 = 3 and β1 = 2.
Nevertheless, some could be very far from those values.
▶ If we repeat the experiment again and again, and average the
estimates, we would get very close to 2.
▶ The problem is, we do not know which kind of sample we
have. We can never know whether we are close to the
population value.
▶ We hope that our sample is ”typical” and produces a slope
estimate close to β1 but we can never know.

#### Page 88

Unbiasedness is a property of the procedure or rule. After
estimating an equation like
ˆlwage = 0.58 +.082 educ (137)
n = 526, R 2 =.1858 (138)
it is tempting to say 8 .2% is an ”unbiased estimate” of the return
to education. Technically, this statement is incorrect. The rule
used to get ˆβ0 = 0.58 and ˆβ1 =.082 is unbiased (if we even
believe u is unrelated to educ).

#### Page 89

We use four assumptions:
▶ SLR.1: y =β0 +β1x + u
▶ SLR.2: random sampling
▶ SLR.3: sample variation in the xi
▶ SLR.4: E (u|x) = 0
The focus should mainly be on the last of these. What are the
omitted factors? Are they likely to be correlated with x? If so,
SLR.4 fails and OLS will be biased.

#### Page 90

Variance of the OLS Estimators
▶ Under SLR.1 to SLR.4, the OLS estimators are unbiased. This
tells us that, on average, the estimates will equal the
population values.
▶ But we need a measure of dispersion (spread) in the sampling
distribution of the estimators. We use the variance (and,
ultimately, the standard deviation).
▶ We could characterize the variance of the OLS estimators
under SLR.1 to SLR.4 (and we will later). For now, it is
easiest to introduce an assumption that simpliﬁes the
calculations.

#### Page 91

Assumption SLR.5 (Homoskedasticity, or Constant Variance)
The error has the same variance given any value of the explanatory
variable x:
Var(u|x) =σ2> 0 (139)
where σ2 is (virtually always) unknown.
Because we assume SLR.4, that is, E (u|x) = 0 whenever we
assume SLR.5, we can also write
E (u2|x) =σ2 = E (u2) (140)

#### Page 92

Under the population Assumptions SLR.1 ( y =β0 +β1x + u),
SRL.4 (E (u|x) = 0) and SLR.5 ( Var(u|x) =σ2),
E (y|x) =β0 +β1x (141)
Var(y|x) =σ2 (142)
So the average or expected value of y is allowed to change with x
– in fact, this is what interests us – but the variance does not
change with x.

#### Page 93

The constant variance assumption may not be realistic; it must be
determined on a case-by-case basis.
EXAMPLE: Suppose that we would like to analyse the relationship
between savings and income; y = sav, x = inc; and we think
E (sav|inc) =β0 +β1inc (143)
with β1> 0. This means average family saving increases with
income. If we impose SLR.5 then
Var(sav|inc) =σ2 (144)
which means the variability in saving does not change with income.
There are reasons to think savings would be more variable as
income increases. So SLR.5 is not realistic on this case.

#### Page 94

THEOREM (Sampling Variances of OLS)
Under Assumptions SLR.1 to SLR.2,
Var( ˆβ1|x) = σ2
∑n
i=1(xi− x)2 = σ2
SSTx
(145)
Var( ˆβ0|x) = σ2 (
n−1 ∑n
i=1 x 2
i
)
SSTx
(146)
(conditional on the outcomes {x1, x2,..., xn}).

#### Page 95

To show this, write, as before,
ˆβ1 =β1 +
n∑
i=1
wiui (147)
where wi = (xi− x)/SSTx. We are treating this as nonrandom in
the derivation. Because β1 is a constant, it does not aﬀect
Var( ˆβ1). Now, we need to use the fact that, for uncorrelated
random variables, the variance of the sum is the sum of the
variances.

#### Page 96

The{ui : i = 1, 2,..., n} are actually independent across i, and so
they are uncorrelated. So (remember that if we know x, we know
w)
Var( ˆβ1|x) = Var
( n∑
i=1
wiui|x
)
(148)
=
n∑
i=1
Var(wiui|x) =
n∑
i=1
w 2
i Var(ui|x) (149)
=
n∑
i=1
w 2
i σ2 =σ2
n∑
i=1
w 2
i (150)
where the second-to-last equality uses Assumption SLR.5, so that
the variance of ui does not depend on xi.

#### Page 97

Now we have
n∑
i=1
w 2
i =
n∑
i=1
(xi− x)2
(SSTx)2 =
∑n
i=1(xi− x)2
(SSTx)2 (151)
= SSTx
(SSTx)2 = 1
SSTx
(152)
We have shown
Var( ˆβ1) = σ2
SSTx
(153)

#### Page 98

▶ This is the “standard” formula for the variance of the OLS
slope estimator. It is not valid if Assumption SLR.5 does not
hold.
▶ The homoskedasticity assumption was not used to show
unbiasedness of the OLS estimators. That requires only SLR.1
to SLR.4.

#### Page 99

Usually we are interested in β1. We can easily study the two
factors that aﬀect its variance.
Var( ˆβ1) = σ2
SSTx
(154)
1. As the error variance increases, i.e, as σ2 increases, so does
Var( ˆβ1). The more “noise” in the relationship between y and
x – that is, the larger variability in u – the harder it is to learn
about β1.
2. By contrast, more variation in {xi} is a good thing:
SSTx↑ implies Var( ˆβ1)↓ (155)

#### Page 100

Notice that SSTx/n is the sample variance in x. We can think of
this as getting close to the population variance of x, σ2
x, as n gets
large. This means
SSTx≈ nσ2
x (156)
which means, as n grows, Var( ˆβ1) shrinks at the rate 1 /n. This is
why more data is a good thing: it shrinks the sampling variance of
our estimators.

#### Page 101

The standard deviation of ˆβ1 is the square root of the variance. So
sd( ˆβ1) = σ√SSTx
(157)
This turns out to be the measure of variation that appears in
conﬁdence intervals and test statistics.

#### Page 102

Estimating the Error Variance
In the formula
Var( ˆβ1) = σ2
SSTx
(158)
we can compute SSTx from{xi : i = 1,..., n}. But we need to
estimate σ2.
Recall that
σ2 = E (u2). (159)

#### Page 103

Therefore, if we could observe a sample on the errors,
{ui : i = 1, 2,..., n}, an unbiased estimator of σ2 would be the
sample average
n−1
n∑
i=1
u2
i (160)
But this not an estimator because we cannot compute it from the
data we observe, since ui are unobserved.
How about replacing each ui with its “estimate”, the OLS residual
ˆui?
ui = yi−β0−β1xi (161)
ˆui = yi− ˆβ0− ˆβ1xi (162)

#### Page 104

ˆui can be computed from the data because it depends on the
estimators ˆβ0 and ˆβ1. Except by ﬂuke,
ˆui̸= ui (163)
for any i.
ˆui = yi− ˆβ0− ˆβ1xi = (β0 +β1xi + ui)− ˆβ0− ˆβ1xi (164)
= ui− ( ˆβ0−β0)− ( ˆβ1−β1)xi (165)
E ( ˆβ0) =β0 and E ( ˆβ1) =β1, but the estimators almost always
diﬀer from the population values in a sample.

#### Page 105

Now, what about this as an estimator of σ2?
n−1
n∑
i=1
ˆu2
i = SSR/n (166)
It is a true estimator and easily computed from the data after OLS.
As it turns out, this estimator is slightly biased: its expected value
is a little less than σ2.

#### Page 106

The estimator does not account for the two restrictions on the
residuals, used to obtain ˆβ0 and ˆβ1:
n∑
i=1
ˆui = 0 (167)
n∑
i=1
xi ˆui = 0 (168)
There is no such restriction on the unobserved errors.

#### Page 107

The unbiased estimator of σ2 uses a degrees-of-freedom
adjustment. The residuals have only n− 2 degrees-of-freedom, not
n.
ˆσ2 = SSR
(n− 2) (169)
THEOREM: Unbiased Estimator of σ2
Under Assumptions SLR.1 to SLR.5,
E (ˆσ2) =σ2 (170)

#### Page 108

In regression output, it is
ˆσ =
√
ˆσ2 =
√
SSR
(n− 2) (171)
that is usually reported. This is an estimator of sd(u), the
standard deviation of the population error.
One small glitch is that ˆσ is not unbiased for σ. This will not
matter for our purposes (and there is no unbiased estimator of σ).

#### Page 109

▶ ˆσ is called the standard error of the regression , which
means it is an estimate of the standard deviation of the error
in the regression. Stata calls it the root mean squared error .
▶ Given ˆσ, we can now estimate sd( ˆβ1) and sd( ˆβ0). The
estimates of these are called the standard errors of the ˆβj.
We will use these a lot.
▶ Almost all regression packages report the standard errors in a
column next to the coeﬃcient estimates.

#### Page 110

▶ We just plug ˆσ in for σ:
se( ˆβ1) = ˆσ√SSTx
(172)
where both the numerator and denominator are computed
from the data.
▶ For reasons we will see, it is useful to report the standard
errors below the corresponding coeﬃcient, usually in
parentheses.

#### Page 111

Summary and Tasks Left Undone
▶ In the table of Stata output, we have talked about the
coeﬃcients (Coef.), and how to interpret them. It is critical to
become comfortable with that.
▶ Of course, we know what the sample size (Number of obs)
means. We have talked about the R-squared and how it
depends on the total, explained (model) and residual sum of
squares.
▶ We just talked about ˆσ, the standard error of the regression
(Root MSE). Its main importance, for now, is how it appears
in the standard errors of the intercept and slope estimates.
The standard errors are in the column labeled ”Std. Err.”

#### Page 112

▶ We have talked about conditions under which OLS is
unbiased, and we have derived the sampling variance of the
OLS estimators(which underlies the standard error
calculations).
▶ SLR.1: y =β0 +β1x + u
▶ SLR.2: random sampling
▶ SLR.3: sample variation in the xi
▶ SLR.4: E (u|x) = 0
▶ SLR.5: Var(u|x) = Var(u) =σ2.

#### Page 113

▶ The ﬁrst four assumptions are used for unbiasedness and we
add the ﬁfth, homoskedasticity, for the ”standard” variance
calculation.
▶ We could continue by discussing statistical inference and
conﬁdence intervals for the population parameters. But the
mechanics are very similar to multiple regression, and so we
hold oﬀ.
▶ Also, there are a lot of ways to combine data to estimate a
slope and intercept. How do we know the OLS approach is a
good one? We will argue, in the context of multiple
regression, that OLS is ”best” in a certain sense under the
ﬁve given assumptions. These are often called the
Gauss-Markov Assumptions (for cross-sectional regression).

#### Page 114

Basics of regression
▶ A key tool in multivariate statistical inference is linear
regression in which we specify the conditional mean of some
response variable, Y , as a linear function of k independent
variables
E [Y|X1, X2,..., XK ] =β1X1 +β2X2 +··· +βK XK
▶ Note that the conditional mean of Y is a function of
observable X1, X2,..., XK covariates with ﬁxed (i.e.,
constant) parameters, β1, β2, . . . ,βK .
▶ Given values for these parameters, then our regression model
predicts the average value of the outcome variable, Y , for
diﬀerent values of X1, X2, . . . , XK

### bartik_iv.pdf — 24 pages (0 without PDF text)

#### Page 1

Bartik Instruments
Yumin Li
Shanghai University
1 / 21

#### Page 2

Roadmap
- In some cases, the source of exogeneous variation (either in an IV setting, or just OLS)
is straightforward
- There is a single policy or source of variation
- However, in other settings, there are more complicated sources of variation exploited
to identify eﬀects. T oday we’ll focus on Bartik (shift-share) instruments
- Key historical feature of this approach is that they had an “intuitive” feature of
identiﬁcation, but formal properties were not established for several decades
- Analagous to staggered DiD literature!
2 / 21

#### Page 3

Bartik instruments are used everywhere
Labor supply + immigration Trade Finance, innovation,
others...
Bartik
- Thread that links all Bartik applications:
- local markets composed of many “categories”
- need for identiﬁcation
- Approach has been used since the early 90:
- sometimes called “shift-share” or “industry mix” instruments
3 / 21

#### Page 4

Examples of Bartik instruments in many subﬁelds
Immigration: Altonji and Card (1991), Card (2001)
Bank Lending: Amiti and Weinstein (2018), Greenstone, Mas and Nguyen (2015)
Market Size + Demography: Acemoglu and Linn (2004), Jaravel (2018)
Labor Supply Elasticity: Blanchard and Katz (1992), Bartik (1991)
Fiscal Multipliers: Nakamura and Steinsson (2014)
Trade + Labor: Autor, Dorn, and Hanson (2013), Autor, Dorn and Hanson (2018), etc.
Foreign Aid: Nunn and Qian (2014)
Portfolio Allocation: Calvet, Campbell, and Sodini (2009)
Trade + Prices: Piveteau and Smagghue (2017), de Roux et al. (2017)
Automation: Acemoglu and Restrepo (2017)
4 / 21

#### Page 5

Many paths lead to Bartik
- Diverse literature leads to many motivations and justiﬁcations for Bartik approach
- Two distinct approaches in the literature:
1. Applied micro statistical approach: interested in a reduced form causal relationship; need
an instrument that is uncorrelated with error term; make argument that Bartik instrument
is defensible
2. Structural approach: interested in particular parameters from model; assumptions of
model motivate certain estimating equations
- So what is the Bartik approach anyway?
5 / 21

#### Page 6

Motivation: local labor market approaches + reduced form
Consider a local labor market regression like the following:
yl = β0 + βxl + ϵl
- E[xl ϵl ] ̸= 0 ⇒ need an instrument to estimate β
- E.g. Autor, Dorn and Hanson (2013) setting:
- l: location (commuting zone)
- yl: manufacturing employment growth
- xl: import exposure to China growth
- β: eﬀect of rise of China on manufacturing employment
- an instrument for location-level exposure to trade with China
6 / 21

#### Page 7

The Bartik instrument
Accounting identity #1:
xl =
K
∑
k =1
zlk glk
- zlk: location-industry shares (Zl)
- glk: location-industry growth (in imports) rates (Gl)
Accounting identity #2:
glk|{z}
location-industry
= gk|{z}
industry
+ ˜glk|{z}
idiosyncratic
location- industry
Bartik:
Bl =
K
∑
k =1
zlk gk
7 / 21

#### Page 8

This gives us a simple 2SLS structure
yl = β0 + βxl + ϵl
xl = π0 + π1Bl + ul
Bl =
K
∑
k =1
zlk gk
glk = gk + ˜glk
China shock: e.g., Autor, Dorn and Hanson (2013)
- zlk: location (l) industry (k) composition
- glk: location (l) industry (k) growth in imports from China
- gk: industry k growth of imports (from China)
8 / 21

#### Page 9

Other instruments have this structure
yl = β0 + βxl + ϵl
xl = π0 + π1Bl + ul
Bl =
K
∑
k =1
zlk gk
glk = gk + ˜glk
Immigrant enclave: e.g., Altonji and Card (1991)
- zlk: share of people from foreign k living in l (in a base period)
- glk: growth in number of people from k to l
- gk: growth in people from k nationally
8 / 21

#### Page 10

Other instruments have this structure
yl = β0 + βxl + ϵl
xl = π0 + π1Bl + ul
Bl =
K
∑
k =1
zlk gk
glk = gk + ˜glk
Bank-lending relationships: e.g., Greenstone, Mas and Nguyen (2015)
- zlk: location (l) share of loan origination from bank k
- glk: loan growth in location l by bank k
- gk: part of loan growth due to bank supply shock
8 / 21

#### Page 11

Other instruments have this structure
yl = β0 + βxl + ϵl
xl = π0 + π1Bl + ul
Bl =
K
∑
k =1
zlk gk
glk = gk + ˜glk
Market size and demography: e.g., Acemoglu and Linn (2004)
- zlk: spending share on drug l from age group k
- glk: growth in spending of group k on drug l
- gk: growth in spending of group k (due to population aging)
8 / 21

#### Page 12

What’s necessary for consistency?
yl = β0 + βxl + ϵl
xl = π0 + π1Bl + ul
Bl =
K
∑
k =1
zlk gk
glk = gk + ˜glk
- We need Bl to be a valid instrument
- Requires two conditions with constant eﬀects:
1. Relevance: π1 ̸= 0, e.g. Cov(Bl , xl ) ̸= 0
2. Validity: E (Bl ϵl ) = 0
- Key ﬂaw in this literature until recently: economic + statistical content of exclusion
has been vague and sometimes confused
9 / 21

#### Page 13

Key thing to remember from today
- Assuming independence or exogeneity on the basis of a model does not necessarily
make it true
- E.g. Hausman instruments in IO models – model may assume that exclusion restriction is
satisﬁed, but not necessarily true in reality
- Assuming that two things are independent because they don’t seem “related” doesn’t
make it true
- Bartik literature many times argues that national nature of shocks “decouples” the
instrument from local market conditions. However, it still exploits local characteristics.
Need to make very speciﬁc arguments to validate claim.
- When evaluating an identiﬁcation strategy, you should be able to describe
counterfactual claims using the measure. This is typically not concrete in Bartik – try
to make it concrete! What is exactly changing in China? Why is it random?
10 / 21

#### Page 14

More general econometric set-up
ylt = Dlt β0 + xlt β + ϵlt ,
xlt = Dlt τ + Blt γ + ηlt
Dlt = controls, f.e.
glkt = gkt + ˜glkt
Blt =
K
∑
k =1
zlk 0gkt ,
{{xlt , Dlt , ϵlt }T
t=1}L
l=1, iid, L → ∞
Assumptions for IV in terms of Blt:
- Exogeneity: E [Blt ϵlt |Dlt ] = 0
- Relevance: Cov [Blt , xlt |Dlt ] ̸= 0
Question:
- What do these statements about Blt imply about zlk0 and gkt?
11 / 21

#### Page 15

Recent Literature on this topic
- Three papers addressed this question, and can be split into two grouping
- The division between papers can be split based on focus on zlk0 vs. gkt
1. Goldsmith-Pinkham, Sorkin and Swift (2020) focus on zlk 0 and make an analogy to
diﬀerence-in-diﬀerences
2. Adao, Kolesar and Morales (2019) and Borusyak, Hull and Jaravel (2020) focus on gkt, and
make a strong connection to the design based approach (e.g. these are as-if random
shocks)
- Key problem, historically, in this literature, was the lack of a coherent defense of the
identifying variation
- These papers provide a way of doing this! But you have to pick one approach
12 / 21

#### Page 16

Understanding the identifying assumption in GPSS: Three special
cases
1. One time period, two industries
2. T time periods, two industries
3. One time period, K industries
13 / 21

#### Page 17

Special case #1: One time period, two industries
- zl2 = 1 − zl1
- Bartik:
Bl = zl1g1 + zl2g2 = zl1g1 + (1 − zl1)g2
= g2 + (g1 − g2)zl1
First-stage:
xl = γ0 + γBl + ηl
xl = γ0 + γg2| {z }
constant
+ γ(g1 − g2)| {z }
coeﬃcient
zl1 + ηl
The instrument is zl1, while gk aﬀects relevance
14 / 21

#### Page 18

Special case #2: T time periods, two industries
Panel Bartik:
Blt = zl10g1t + zl20g2t = g2t + ∆gt|{z}
g1t −g2t
zl10
First stage:
xlt = τl + τt + γBlt + ηlt
xlt = τl + (τt + γg2t )| {z }
˜τt
+ γ∆gt| {z }
˜γt
zl10 + ηlt
15 / 21

#### Page 19

Special case #2: T time periods, two industries
- Analogy to continuous diﬀerence-in-diﬀerences
- ∆gt is size of policy
- zl10 is exposure to policy
- Sometimes a “pre-period” before policy: test for parallel pre-trends
- E.g., in ADH, what happens from 1970 to 1990?
16 / 21

#### Page 20

Special case #3: One time period, K industries
- G: K × 1 vector of gk
- Z : L × K , matrix of Zl
- Y ⊥, X ⊥, B = ( Z G): L × 1, vectors of y ⊥
l , x ⊥
l and Bl
- Ω: K × K
ˆβBartik = B′Y ⊥
B′X ⊥
ˆβGMM = (X ⊥′Z )Ω(Z ′Y ⊥)
(X ⊥′Z )Ω(Z ′X ⊥)
If Ω = ( GG′), then ˆβBartik = ˆβGMM
17 / 21

#### Page 21

Full general result with T time periods and K industries
Two estimators are numerically identical:
- TSLS with Bartik instrument
- GMM with industry shares × time period as instruments and a particular weight
matrix
ˆβBartik = B′ eY⊥
B′ eX⊥
ˆβGMM = (X⊥′ eZ)Ω(eZ′Y⊥)
(X⊥′ eZ)Ω(eZ′X⊥)
Ω = ( GG′), and eZ is an L T× KT stacked vector of Z0 interacted with time ﬁxed eﬀects
and G is a KT × 1 stacked vector of growth rates gkt.
18 / 21

#### Page 22

When is the estimator consistent for the estimand of interest?
What is the identiﬁcation condition?
ˆβBartik = ∑L
l=1 ∑T
t=1 ∑K
k =1 zlkt gkt y ⊥
lt
∑L
l=1 ∑T
t=1 ∑K
k =1 zlkt gkt x ⊥
lt
Two ideas:
- “Shares” : talk about properties of zlkt
- Conditional exogeneity
- model based – diﬀ-in-diﬀ style approach
- “Shocks” (Borusyak, Hull and Jaravel (2018)): talk about properties of gkt
- Random, and a large number (equivalent industry-level regression)
- design-based (in spirit) – IV strategy
19 / 21

#### Page 23

When are these views plausible? What do they mean?
Shares
Conditional exogeneity:
- Typically: exogenous to changes in error term, not levels of outcome
- Standard in diﬀ-in-diﬀ (exclusion): in a period, exposure to an industry matters for
outcome only through x
Shocks
- Large number of industries (shares are misspeciﬁed, need it to average out)
- Random shocks across industries – need the shocks to be conditionally random
How do we choose?
- The shocks approach is more design-based (which can be appealing), but requires an
argument why shocks are randomly assigned
- The shares approach is model-based, so suﬀers from same issues as DiD, but may
more naturally work in your setting.
20 / 21

#### Page 24

Is ADH about shocks or shares?
Shocks:
- Explains why ghigh−income
kt rather than gUS
kt (hard to rationalize under shares)
- Natural in a trade model: why would imports from China rise (in a trade model)?
Independent industry-speciﬁc shocks
Shares:
- Explains why zlkt−1 rather than zlkt (hard to rationalize under shocks)
- Explains why it is important for identiﬁcation to study local labor markets (as opposed
to parameter of interest where we want to think about spillovers)
Bottom line: a little hard to tell what exactly ADH are assuming; ADH approach does not
appear to satisfy testable assumptions under GPSS, but do appear to under BHJ.
21 / 21

### HeterogeneousFX.pdf — 13 pages (0 without PDF text)

#### Page 1

IV: Heterogeneous Effects
Yumin Li
Shanghai University

#### Page 2

Outline
The LATE Theorem
Potential Outcome Setup
Theorem and Extensions

#### Page 3

From Constant to Heterogeneous Effects
So far we have implicitly been considering models w/ constant
effects
▶ Yi = α + βDi + εi implies ∂Y /∂D = β for all observations i
▶ What if this model is misspecified? I.e. what if
Yi = α + βi Di + εi ?
Intuitively, different “research designs” (e.g. instruments) may
capture different effects of the same treatment — even when all
are valid
▶ Recall charter lottery vs. takeover IVs: very different setups!
Formalized in the (Nobel-winning!) Imbens and Angrist ’94 LATE
thm.
▶ Using a general potential outcomes framework...

#### Page 4

Potential Outcome Setup
Let Yi(0) and Yi(1) denote individual i’s potential outcomes given
a binary treatment Di ∈ {0, 1}
▶ Observed outcomes:
Yi = (1 − Di)Yi(0) + Di Yi(1) = αi + βi Di
▶ Only observe one of these POs for each i; other is a
counterfactual
▶ Interested in the treatment effects βi = Yi(1) − Yi(0)
Imbens-Angrist’ insight: we can also do this for an IV first stage:
▶ Let Di(0) and Di(1) denote individual i’s potential treatment
given a binary instrument Zi ∈ {0, 1}:
Di = (1 − Zi)Di(0) + Zi Di(1)
Under what assumptions can we causally interpret “ivreg2 Y
(D=Z)”?

#### Page 5

Four types of people – With Math
▶ Let Di be an indicator for whether you are on Medicaid
(treatment). Let Zi be an indicator for whether you won the
lottery (instrument)
▶ Introduce potential treatments, Di(1) and Di(0), analogous to
the potential outcomes
▶ Di (1) is treatment status if Zi = 1 (win the lottery)
▶ Di (0) is treatment status if Zi = 0 (lost the lottery)
▶ Always takers: people who will enroll in Medicaid regardless
of the outcome of the lottery. Always takers have
Di(1) = Di(0) = 1
▶ Never takers: people who will never enroll in Medicaid
regardless of the lottery. Never takers have Di(1) = Di(0) = 0
▶ Compliers: people who enroll in Medicaid only if they win the
lottery. Compliers have Di(1) = 1 and Di(0) = 0
▶ Defiers: people who enroll in Medicaid only if they lose the
lottery. Defiers have Di(1) = 0 and Di(0) = 1

#### Page 6

Imbens and Angrist (1994) Assumptions
1. As-good-as-random assignment :
Zi ⊥ (Yi(0), Yi(1), Di(0), Di(1))
▶ Consider the Angrist draft lottery, or Angrist-Krueger’s QoB IV
2. Exclusion: Zi only affects Yi through its effect on Di
▶ Implicit in our potential outcomes notation: Yi (d) not indexed
by Zi
3. Relevance: Zi is correlated with Di
▶ Equivalently, given Assumption 1, E [Di (1) − Di (0)] ̸= 0
4. Monotonicity: Di(1) ≥ Di(0) for all i (i.e., almost-surely)
▶ The instrument can only shift the treatment in one direction

#### Page 7

The LATE Theorem
▶ If Independence, Relevance, Exclusion, and Monotonicity hold,
then
▶
E [Yi |Zi = 1] − E [Yi |Zi = 0]
E [Di |Zi = 1] − E [Di |Zi = 0] = E [Yi(1) − Yi(0)|Di(1) = 1, Di(0) = 0]
▶ or in words:
▶
Effect of Z on Y
Effect of Z on D = Local average treatment effect for compliers
▶ This result won Joshua Angrist and Guido Imbens the 2021
Nobel Prize in Economics!
▶ Different (valid) IVs can identify different LATEs!

#### Page 8

Intuition for the LATE theorem
▶ Remember the theorem says that
▶
E [Yi |Zi = 1] − E [Yi |Zi = 0]
E [Di |Zi = 1] − E [Di |Zi = 0] = E [Yi(1) − Yi(0)|Di(1) = 1, Di(0) = 0]
▶ By independence, the numerato is the causal effect of Z on Y
▶ However, the exclusion restriction says that the effect of Z on
Y is zero for always-takers and never-takers (their treatment
doesn’t change!).
▶ Further, we’ve assumed that there are no defiers
▶ Thus, the numerator will be the average effect for compliers
times the fraction of compliers in the population, i.e.
LATE × P(Complier)
▶ But the denominator is the effect of Z on D. This effect is
zero for ATs and NTs, so the denominator is the share of
compliers, P(Complier)

#### Page 9

More formal derivation
▶ Remember that we can divide the population into three
groups, always-takers (ATs), never-takers (NTs), and
compliers (Cs) b/c we assumed no defiers!
▶ Let αAT = P(AT), αNT = P(NT ), αC = P(C ) be the shares
of ATs, NTs, and Cs
▶ Since Z is randomly assigned (independence), we have that
P(AT|Z = 1) = P(AT) = αAT.
Likewise, P(AT|Z = 0) = P(AT) = αAT
▶ By similar arguments, share of NTs and Cs is the same in the
Z = 1 and Z = 0 groups.

#### Page 10

More formal derivation
▶ Next step: show that the numerato is αC × LATE
▶ By the Law of Iterated Expectations,
E [Yi |Zi = 1] =
αC E [Yi |Zi = 1, C ] + αATE [Yi |Zi = 1, AT] + αNT E [Yi |Zi = 1, NT ] =
αC E [Yi(1)|Zi = 1, C ] + αATE [Yi(1)|Zi = 1, AT] + αNT E [Yi(0)|Zi = 1, NT ] =
αC E [Yi(1)|C ] + αATE [Yi(1)|AT] + αNT E [Yi(0)|NT ]
where the last line uses independence
▶ Similarly,
E [Yi |Zi = 0] = αC E [Yi(0)|C ] + αATE [Yi(1)|AT] + αNT E [Yi(0)|NT ]
▶ Thus, the numerator in our ratio is
E [Yi |Zi = 1] − E [Yi |Zi = 0] = αC (E [Yi(1)|C ] − E [Yi(0)|C ])| {z }
LATE

#### Page 11

More formal derivation
We showed that the numerator is αC × LATE
To finish proof, we show that the denominator is αC
The denominator is
E [Di |Zi = 1] − E [Di |Zi = 0]
Hence,
numerator
denominator = αC × LATE
αC
= LATE

#### Page 12

What Does This Mean Practically?
Two conceptually distinct considerations: internal vs. external
validity
▶ Context of an IV, and who the compliers likely are, may
matter
▶ Usual “overidentification” test logic fails: two valid IVs may
have different estimands! (see Kitagawa (2015) for alternative
tests)
In addition to as-good-as-random assignment / exclusion, we may
need to worry about monotonicity when we do IV
▶ Sensible in earlier lottery / natural experiment / panel
examples
▶ Maybe questionable in judge IVs

#### Page 13

Extensions
Angrist and Imbens worked out the original LATE theroem for
binary Di, discrete Zi, and no included controls
... but it extends
▶ Angrist/Imbens ’95: multivalued (ordered) Di, saturated
covariates
▶ Angrist/Graddy/Imbens ’00: continuous Di (supply/demand
setup)
▶ Heckman/Vytlicil ’05: continuous Zi (more on this soon)
▶ Multiple unordered treatments is harder (e.g. Behaghel et al.
2013)
Recent discussions highlight importance of including flexible
controls
▶ E.g. Sloczy´ nski ’20, Borusyak and Hull ’21, Mogstad et al. ’22
▶ If monotonicity only holds conditional on Xi, may need
Zi-by-Xi interactions (which may lead to many-weak
problems...)

### RDD2.pdf — 39 pages (0 without PDF text)

#### Page 1

Regression Discontinuity Designs II
Yumin Li
Shanghai University
Yumin Li 1 / 39

#### Page 2

Outline
1 RD Local Randomization Methods
2 The Fuzzy RD Design
Yumin Li RD Local Randomization Methods 2 / 39

#### Page 3

Recap: Continuity-based Approach
Assume regression functions are continuous to obtain
τSRD = E[Yi(1) − Yi(0) | Xi = c] = lim
x↓c
E[Yi | Xi = x] − lim
x↑c
E[Yi | Xi = x]
Approximates regression function and relies on continuity assumptions.
Requires: choosing weights, bandwidth and polynomial order.
Alternative: local randomization approach can be used as a complement and
robustness check to the continuity-based analysis when the running variable is
continuous, and is a natural framework for analysis when the running variable is
discrete and has few mass points.
Yumin Li RD Local Randomization Methods 3 / 39

#### Page 4

Overview of Local Randomization Approach
Local randomization and randomization inference methods.
Interpreting RD as a local randomization in a window around the cutoff
Conceptual differences with local polynomial estimation
Window selection
Estimation and inference using randomization-based methods
Yumin Li RD Local Randomization Methods 4 / 39

#### Page 5

Analogies with experiments
Lee (2008): RD design can be as credible as a randomized experiment for units
very near cutoff
Imagine that score depends on each unit’s unobservables characteristics and choices
If the two following conditions hold:
1 there is a random chance element to score that unit receives
2 probability of this random “error” doesn’t change abruptly at cutoff
Then the RD design can be seen as an experiment: units barely above the cutoff
as-if randomly assigned to treatment; units barely above the cutoff as-if randomly
assigned to control
This fails if individuals have ability to exactly control their score
Yumin Li RD Local Randomization Methods 5 / 39

#### Page 6

Analogies with experiments
Consider an RD Design where treatment is assigned based on score exceeding
cutoff, and units lack ability to manipulate score (continuity holds)
Crucial distinction:
Experiment  no need to make assumptions about shape of the average potential
outcomes
RD design  inferences depend crucially on assumptions regarding functional form
of regression functions
Any experiment can be recast as an RD design where
score is a uniform random variable
cutoff chosen to ensure a given probability of treatment
E.g. each student assigned uniform random number between 0 and 100, scolarship
given to students whose score is above 50
Yumin Li RD Local Randomization Methods 6 / 39

#### Page 7

Randomized Experiment
Yumin Li RD Local Randomization Methods 7 / 39

#### Page 8

Experiment versus RD Design
Yumin Li RD Local Randomization Methods 8 / 39

#### Page 9

Local Randomization RD
Yumin Li RD Local Randomization Methods 9 / 39

#### Page 10

Local Randomization Approach to RD Design
Key assumption: exists window W = [−w , w] around cutoff ( −w < c < w) where
(assuming random potential outcomes)
Ti independent of (Yi(0), Yi(1)) ( for all Xi ∈ W )
Thus, inside W subjects are as-if randomly assigned to either side of cutoff
The distribution of running variable same for all units inside W
Potential outcomes in W depend on running variable only through threshold
indicators within W (i.e. Yi(0) = Yi(0, Xi) and Yi(1) = Yi(1, Xi))
Stronger than Continuity-Based Approach ⇒ Relevant population functions are not
only continuous at x0, but also completely unaffected by the running variable in W
Yumin Li RD Local Randomization Methods 10 / 39

#### Page 11

Local Randomization Approach to RD Design
In many RD applications, a local randomization assumption will only be plausible in
a very small window around the cutoff, and it will likely contain very few
observations. It is natural to employ a Fisherian inference approach.
Under Fisherian framework, with W = [c − w , c + w], local randomization
assumption is:
The distribution of the running variable in the window W , FXi|Xi∈W (x), is known, is
the same for all units, and does not depend on the potential outcomes:
FXi|Xi∈W (x) = F (x)
Inside W , the potential outcomes depend on the running variable solely through the
treatment indicator Ti = 1(Xi ≥ c) but not directly: Yi(Xi , Ti) = Yi(Ti) for all i
such that Xi ∈ W
Under both conditions, inside W , placement above/below cutoff is unrelated to
potential outcomes and potential outcomes unrelated to score
Yumin Li RD Local Randomization Methods 11 / 39

#### Page 12

Local Randomization RD
Yumin Li RD Local Randomization Methods 12 / 39

#### Page 13

Local Randomization Approach to RD Design
In window W , subjects randomly assigned to either side of cutoff:
Window W
Assignment mechanism
If assignment mechanism and W are known, RD becomes an experiment in W
If few units inside W , adopt a Fisherian setup: potential outcomes are fixed, only
randomness is in the assignment of subjects
Yumin Li RD Local Randomization Methods 13 / 39

#### Page 14

Local Randomization Approach Using Fisherian Methods
Approach has two steps:
Step 1: Choose window around cutoff where randomization holds
Step 2: Apply randomization inference tools, given a hypothesized treatment
assignment, within W
Yumin Li RD Local Randomization Methods 14 / 39

#### Page 15

Step 1: Choose the window W
How to choose window? An ad-hoc method lacks transparency and objectivity. We
should use a principled data-driven procedure.
Use balance tests on pre-determined/exogenous covariates.
Very intuitive, easy to implement. Assume there exists at least one predetermined
covariate, Zi, that is associated with the running variable only outside the window
W where the local randomization assumptions hold. So H0 is that there is no
treatment effect in W . The procedure starts with the smallest possible window W1
and tests the null. If not rejected, a larger window W2 is selected, and H0 is tested
again inside W2. The procedure keeps increasing the length of the window and
re-testing H0 in each larger window, until a window is reached where H0 is rejected.
Yumin Li RD Local Randomization Methods 15 / 39

#### Page 16

Window Selector Based on Covariate Balance
Yumin Li RD Local Randomization Methods 16 / 39

#### Page 17

Step 2: Use Randomization Inference Tools within W
Under this framework, we can treat observations within the window W as if
generated by a randomized experiment
One possible randomization mechanism:
Ti is Bernoulli with parameter π: for all vectors t in Ω W ,
Pr(TW = t) = πt
′
1(1 − π)(1−t)
′
1
Since π is unknown, we estimate it π = T
′
w1
nW
Another possible randomization mechanism (known as a complete randomization
mechanism or a fixed margins randomization):
Fix number of treated units within the window at mW , which leads to
Pr(TW = t) = 1
 nW
mW


for all t ∈ ΩW
Yumin Li RD Local Randomization Methods 17 / 39

#### Page 18

Example of a Complete Randomization Mechanism
Yumin Li RD Local Randomization Methods 18 / 39

#### Page 19

Randomization Inference
To calculate a p-value, we can ask how extreme the observed real treatment effect
(3 in the example) was compared with all the possible permuted treatment effects
if the treatment status had been simply assigned at random to the same number of
observations in this group.
The observed difference-in-means (3) is the largest of the ten, and the exact
p-value is therefore 1 /10 = 0.10
This p-value is finite-sample exact, because the null distribution was derived
directly from the randomization distribution of the treatment assignment, and does
not rely on any statistical model or large sample approximations.
Yumin Li RD Local Randomization Methods 19 / 39

#### Page 20

Example: The Effect of Winning Elections on Future Vote Shares
In the U.S., there are two U.S. Senate seats in each of the 50 states. Each seat is
up for election every six years, but the seats are staggered so that one third of seats
is up for election every two years, and the two seats in the same state are never up
for election simultaneously.
Cattaneo, Frandsen, and Titiunik (2015) estimate the RD effect of the Democratic
party winning a Senate seat on its vote share in the following election for that seat.
The unit of analysis is the U.S. state
The score is the Democratic party’s margin of victory at election t—defined as the
difference between the vote share obtained by the Democratic party minus the vote
share obtained by its strongest opponent.
The outcome of interest is the vote share of the Democratic party in the following
election for that same seat; we denote this election t+2 because the election
immediately following election t is for the other Senate seat in the same state.
Yumin Li RD Local Randomization Methods 20 / 39

#### Page 21

Validation and Falsification Analysis
Tests of a null treatment effect on pre-treatment covariates or placebo outcomes
Tests to assess the density of the score around the cutoff
Treatment effect estimation at artificial cutoffs values
Sensitivity to neighborhood choices
Yumin Li RD Local Randomization Methods 21 / 39

#### Page 22

Outline
1 RD Local Randomization Methods
2 The Fuzzy RD Design
Yumin Li The Fuzzy RD Design 22 / 39

#### Page 23

Treatment Assignment in (Sharp) RD Design
Yumin Li The Fuzzy RD Design 23 / 39

#### Page 24

Sharp Regression Discontinuity Design
n units, indexed by i = 1, 2, ..., n
Unit’s score (or running variable) is Xi, treatment is Ti = 1(Xi ≥ c)
Each unit has two potential outcomes:
Yi(1): outcome that would be observed if i received treatment
Yi(0): outcome that would be observed if i received control
The observed outcome is
Yi =
(
Yi(1) if Xi ≥ c
Yi(0) if Xi < c
Fundamental problem of causal inference : only observe Yi(0) for units below
cutoff and only observe Yi(1) for units above cutoff
Yumin Li The Fuzzy RD Design 24 / 39

#### Page 25

RD Treatment Effect in Sharp RD Design
Yumin Li The Fuzzy RD Design 25 / 39

#### Page 26

Fuzzy RD Design
Imperfect compliance:
Probability of treatment changes at c, but not necessarily from 0 to 1
Some units with score above c may decide not to take up treatment
Example: voting eligibility at 18
Ti = 1(Xi ≥ c) is the treatment assigned, Di is treatment taken. For some units
Ti ̸= Di
Treatment taken has two potential values, Di(1) and Di(0), and observed
treatment taken is Di = Ti · Di(1) + (1 − Ti) · Di(0)
Four potential outcomes instead of two:
Yi(1, D(1)) = Di(1)Yi(1, 1) + (1 − Di(1))Yi(1, 0)
Yi(0, D(0)) = Di(0)Yi(0, 1) + (1 − Di(0))Yi(0, 0)
Yumin Li The Fuzzy RD Design 26 / 39

#### Page 27

Conditional Probability of Receiving Treatment
Yumin Li The Fuzzy RD Design 27 / 39

#### Page 28

Fuzzy RD Design
Interest in both the effect of being assigned to treatment (i.e., the effect of T) and
the effect of actually receiving treatment (i.e., the effect of D): when households
with income below a cutoff are eligible to receive a cash transfer, T: eligibility; D:
Cash transfer; Y: income.
Since treatment assignment cannot be changed, compliance with the assignment is
always perfect. Thus, analysis of the effect of T follows a Sharp RD design
In contrast, the study of the effect of D requires modifications and additional
assumptions
Yumin Li The Fuzzy RD Design 28 / 39

#### Page 29

Fuzzy RD Design: Continuity-based parameters
The Sharp RD estimator of the effect of Ti on Yi consistently estimates the
quantity
τY := lim
x↓c
E[Yi | Xi = x] − lim
x↑c
E[Yi | Xi = x]
= lim
x↓c
E[Yi(1, D(1)) | Xi = x] − lim
x↑c
E[Yi(0, D(0)) | Xi = x]
where the equality follows from the more general definition of the observed
outcome as Yi = Ti · Yi(1, D(1)) + (1 − Ti) · Yi(0, D(0)), and thus requires no
special assumptions.
Yumin Li The Fuzzy RD Design 29 / 39

#### Page 30

Fuzzy RD Design: Intention-to-treat
Assuming continuity of E[Yi(1, D(1)) | Xi = x] and E[Yi(0, D(0)) | Xi = x], seen
as functions of x, at the cutoff c, we have
τY = τITT , τ ITT := E[Yi(1, D(1)) − Yi(0, D(0)) | Xi = c]
and thus estimated jump in the average observed outcome at the cutoff recovers
the average effect of T on Y at c.
τITT is usually called average “intention-to-treat” effect, and it captures effect (at
the cutoff) of being assigned to treatment
This parameter is different from Sharp RD parameter τSRD under perfect
compliance τSRD = E[Yi(1) − Yi(0) | Xi = c]
Yumin Li The Fuzzy RD Design 30 / 39

#### Page 31

Fuzzy RD Design: Intention-to-treat
Perfect compliance is a particular case where
P[D(0) = 0 | Xi = x] = 1 for x < c and P[D(1) = 1 | Xi = x] = 1 for x > c
Di = Ti = 1(Xi ≥ c)
Yi(1, 1) := Yi(1) and Yi(0, 0) := Yi(0)
Thus, when compliance is perfect, the RD ITT effect of the treatment assignment
on the outcome is equivalent to the Sharp RD effect of the treatment received:
τITT = E[Yi(1) − Yi(0) | Xi = c]
But when some units are non-compliers, τITT captures the effect of the treatment
assignment, which will be in general different from the effect of actually receiving
the treatment
Yumin Li The Fuzzy RD Design 31 / 39

#### Page 32

Fuzzy RD Design: First Stage
Fuzzy analysis includes study of how the RD assignment rule affects the probability
of receiving the treatment.
Treating Di as the outcome, a Sharp RD strategy estimates
τD := lim
x↓c
E[Di | Xi = x] − lim
x↑c
E[Di | Xi = x]
Since Di is binary, τD captures the difference in the probability of receiving the
treatment between units assigned to treatment vs. assigned to control, at the
cutoff.
Yumin Li The Fuzzy RD Design 32 / 39

#### Page 33

Fuzzy RD Design: First Stage
Assuming continuity at c of E[Di(1) | Xi = x] and E[Di(0) | Xi = x], seen as
functions of x, we have
τD = τFS , τ FS := E[D(1) − D(0) | Xi = c]
and thus can interpret τD as the causal effect of Ti on Di.
τFS captures the effect of assigning the treatment on receiving the treatment for
units with scores near or at the cutoff, usually called “first-stage” effect.
Yumin Li The Fuzzy RD Design 33 / 39

#### Page 34

Fuzzy RD Design: Estimation of FS and ITT effects
Since both τFS and τITT are Sharp RD parameters, analysis follows standard
continuity-based Sharp RD methods, using Xi as running variable, Ti = 1(Xi ≥ c)
as treatment of interest, and Di and Yi as outcomes:
ˆτITT = lim
x↓c
ˆE[Yi | Xi = x] − lim
x↑c
ˆE[Yi | Xi = x]
ˆτFS = lim
x↓c
ˆE[Di | Xi = x] − lim
x↑c
ˆE[Di | Xi = x]
with bandwidth selection and inference methods as discussed before.
Yumin Li The Fuzzy RD Design 34 / 39

#### Page 35

Fuzzy RD Design: Effect of Actual Treatment
When interest is on the effect of the treatment received, it is common to focus on
τFRD := τY
τD
We call τFRD the “fuzzy RD parameter” .
Under the augmented continuity conditions for ITT effects, τFRD = τITT
τFS
. This
interpretation of the Fuzzy RD parameter as ratio of two ITT effects is analogous
to result in IV literature.
Yumin Li The Fuzzy RD Design 35 / 39

#### Page 36

Fuzzy RD Design: Effect of Actual Treatment
Explore conditions under which τFRD can be directly interpreted as the average
treatment effect of the treatment for some subpopulations.
Non-zero first stage: τFS must be nonzero—ideally, well-separated from zero:
Moving above/below the cutoff must induce some units to actually take the
treatment.
Exclusion Restriction: the treatment assignment must affect the potential
outcomes and potential treatments only via the treatment received, but not
directly: E[Yi(Ti , 0)|Xi = x] and E[Yi(Ti , 1)|Xi = x] must be continuous in x at c.
Compliance Restriction: many possibilities, including
Local independence: potential outcomes independent of potential treatments near
the cutoff (Hahn, Todd, and vanderKlaauw, 2001).
Monotonicity: there are no units who receive the opposite treatment to the one
they are assigned near the cutoff (i.e., no “defiers” ).
Yumin Li The Fuzzy RD Design 36 / 39

#### Page 37

Fuzzy RD Design
Interpretation of τFRD will differ according to which assumptions we are willing to
make. For example,
Under local independence
τFRD = E[Yi(1, 1) − Yi(0, 0) | Xi = c]
Under monotonicity
τFRD = E[Yi(1, 1) − Yi(0, 0) | Xi = c, i is a complier ]
Yumin Li The Fuzzy RD Design 37 / 39

#### Page 38

Implementation of Fuzzy RD analysis
Falsification: density test and covariates effects should focus on intention-to-treat
effects.
Bandwidth Selection: two bandwidths if focus on ITT and FS effects, single
bandwidth if focus on Fuzzy RD effect.
Weak Assignment: Avoid analyzing Fuzzy RD effects when the RD assignment rule
has weak effect on the adoption of the treatment.
Yumin Li The Fuzzy RD Design 38 / 39

#### Page 39

Empirical Example
Study by Londo˜ no-V´ elez, Rodr ´ ıguez, and S´ anchez (AEJ, 2020) on the effects Ser
Pilo Paga (SPP), a governmental program in Colombia that funds full tuition to
attend higher education institutions (HEIs).
To be eligible, students must score in top 9% of scores in national high school exit
exam ( “SABER 11” score), and must come from a household with wealth index
below a region-specific threshold ( “SISBEN” wealth score).
Transform this two-dimensional RD design into one-dimensional design: only
students whose SABER 11 score is above cutoff.
Score: difference between student’s SISBEN wealth index and respective cutoff.
Cutoff: normalized to zero.
Treatment assignment (T): an indicator equal to 1 if score below zero. Treatment
received (D): indicator equal to 1 if student received subsidy.
Yumin Li The Fuzzy RD Design 39 / 39

### Potential Outcomes Causal Model.pdf — 100 pages (2 without PDF text)

#### Page 1

Potential Outcomes Causal Model
Yumin Li
Shanghai University
March 24, 2025

#### Page 2

Outline
Foundational ideas
Brief (and selected) history of causal inference
Independence and Selection Bias
Industry example of RCT: eBay advertising
Tennessee’s small class size RCT: STAR

#### Page 3

What is Causality?
▶ Causality is metaphysics; causal inference is epistemology
(what is knowledge?) IOW, what makes a causal belief a
“warranted belief”?
▶ Causality and causal inference have common sources (e.g.,
Aristotle, David Hume, John Stuart Mill, David Lewis) but we
will focus primarily on the experimental design tradition.
▶ We use the potential outcomes framework of causal inference
to build and discuss methods that estimate unbiased and
meaningful average causal parameters as defined by that
framework.

#### Page 4

Causal Inference vs Prediction

#### Page 5

Causal Inference vs Prediction
Traditional prediction
▶ Traditional prediction seeks
to detect patterns in data
and fit functional
relationships between
variables with a high
degree of accuracy
▶ “Does this person have
heart disease?”, “How
many books will I sell?”
▶ It is not predictions of
what effect a choice will
have, though
Causal inference
▶ Causal inference is also a
type of prediction, but it’s
a prediction of a
counterfactual associated
with a particular choice
taken
▶ Causal inference takes that
predicted (or imputed)
counterfactual and
constructs a causal effect
that we hope tells us about
a future in the event of a
similar choice taken

#### Page 6

Causal Inference is widely used in business
▶ The questions economists study in Kuaishou or Douyin:
▶ How to design incentive strategy to attract new users?
▶ Can exposure of elite streamers promote users’ using time and
rewards?
▶ How can imrpovement of streamer PK promote users’ using
time and rewards?
▶ Can we change the product or service to increase streaming
demand and platform’s revenue?
▶ The methods they use:
▶ Causal inference of observational data; A/B test.
▶ The name “Douyin” is actually determined by a A/B test.

#### Page 7

Naive causal inference
▶ Aliens estimate a model showing a systematic correlation
between COVID deaths and ventilators. They conclude
ventilators are killing patients.
▶ Or aliens see supermarkets can sell a lot when the
supermarkets are in red. They conclude that red is important
in advertising.
▶ Their error was they confused correlation with causality, but
deeper than that, they didn’t understand how the world
worked

#### Page 8

Correlation vs Causality

#### Page 9

Causality and Ceteris Paribus
▶ In most cases when testing an economic theory and especially
when evaluating public policy, our goal is to infer about the
causal effect of one variable on another variable
▶ the effect of deworming medication on children’s cognitive
outcomes
▶ the effect of having been involved in war on labour market
earnings
▶ the effect of microfinance receipt on small business profit
▶ Simply finding an association between two variables might be
suggestive of a causal effect, but it might not be causal or the
causal effect may be not correctly estimated.
▶ Correlation does not mean causality: rooster and sun rise;
chocolate consumption and Nobel laureates; red compaign
and high demand etc.
▶ The idea of Ceteris Paribus - “Other (relevant) factors being
equal” - is an important concept in causal analysis.

#### Page 10

Ceteris paribus (cont)
▶ The concept of ceteris paribus is very natural in economics
questions:
▶ When analyzing consumer’s demand, we want to know how a
change in price affect the quantity holding other factors like
income, price of other goods, and individual tastes fixed.
▶ In the evaluation of the job training program we want to know
its effect on wages holding education, experience and other
factors fixed.
▶ The gold standard in causal analysis would be able to carry
out an appropriate controlled experiment.
▶ Usually it is not possible to literally hold “all else equal” . The
key question is have we held enough factors fixed to make a
case about causality in the analysis we are performing.
▶ Most times the experiment that we would like to perform is
not possible or practical to perform, but it is still useful to
think hard about what would be the experiment.
▶ Careful econometric analysis can provide us with a “simulated”
ceteris paribus experiment.

#### Page 11

The effects of good schools
▶ The top 3 in the Shanghai college entrance examination in
2024 come from the same middle school class, Class 6 of
Lansheng Middle School. Do good schools improve students’
academic performance?
▶ In 2020, Shanghai’s private middle school cannot admit
students based on test scores, but only through lotteries. In
2024, the first year of the middle school exam in the lottery
era, the difference of test scores between public schools and
private schools is indeed closer. The private schools are still
better in terms of scores.
▶ Does it mean that private schools can indeed bring about
better student performance?
▶ A better way is to compare those who got in and those who
didn’t - not those who didn’t take part in the lottery. For this
comparison, schools do not have a significant impact.

#### Page 12

#1: Correlation and causality are different concepts
Causal is one unit, correlation is many units
▶ Causal question: “If a doctor puts a patient on a ventilator
(D), will her covid symptoms (Y) improve?”
▶ Correlation question:
Corr(D, Y ) = Cov(D, Y )√VarD
√VarY

#### Page 13

#2: Coming first may not mean causality!
▶ Every morning the rooster crows and then the sun rises
▶ Did the rooster cause the sun to rise? Or did the sun cause
the rooster to crow?
▶ What if cat killed the rooster?
▶ Post hoc ergo propter hoc : “after this, therefore, because of
this”

#### Page 14

#3: Causality may mask correlations! Endogeneity!

#### Page 15

Modeling is Not the First Step
▶ In our research question , what is our target parameter :
averaged treatment effects over a specific population
▶ Another is the idea of treatment assignment mechanisms –
how and why units are and are not chosen into the treatment
versus control status matters for everything
▶ The design stage should precede the stage of estimating with
data (i.e., coding) and come after collecting the data

#### Page 16

Definition and Identification Come First
1. Turn the research question (“what is the causal effect of an
advertising campaign on sales?”) into a specific aggregate
causal parameter
2. Describe the narrow set of beliefs that make that parameter
obtainable with data
3. Build a model that uses the data and the beliefs to estimate
the causal parameter
Most of us skip (1) and many skip (2) and go straight to (3).

#### Page 17

Outline
Foundational ideas
Brief (and selected) history of causal inference
Independence and Selection Bias
Industry example of RCT: eBay advertising
Tennessee’s small class size RCT: STAR

#### Page 18

Three New Ideas
1. Counterfactual: Philosophers come to it first and its central
role in causal inference makes causality unknowable that the
project is nearly derailed
2. Treatment assignment mechanism : Neyman and Fisher
solve the counterfactual problem in statistics and lay the
foundation of the modern randomized controlled trial (RCT)
with their focus on the selection process
3. No One Causal Effect : There is no such thing as “the
causal effect”; there’s many and your first step is to pick a
parameter (not as easy as it sounds)

#### Page 19

Modern Philosophers Introduce Counterfactual
Comparisons
“If a person eats of a particular dish, and dies in conse-
quence, that is, would not have died if he had not eaten
it, people would be apt to say that eating of that dish was
the source of his death.” – John Stuart Mill (19th century
moral philosopher and economist)
“Causation is something that makes a difference, and the
difference it makes must be a difference from what would
have happened without it.” – David Lewis (20th century
philosopher)

#### Page 20

Statistical origins
“Yet, although the seeds of the idea that [causal effects
are comparisons of potential outcomes] can be traced back
at least to the 18th century [most likely he means David
Hume], the formal notation for potential outcomes was not
introduced until 1923 by Neyman.” – Don Rubin (1990)

#### Page 21

Jerzy Neyman’s Notation
▶ Jerzy Neyman’s 1923 masters thesis describes a field
experiment with differing plots of land (imagine hundreds of
square gardens) and many different “varieties” of fertilizer
that farmers could apply to the land
▶ “Uik is the yield of the ith variety on the kth plot...”
(Neyman 1923)
▶ He calls Uik “potential yield”, as opposed to the realized yield
because i (the fertilizer type) described all possible fertilizers
that could be assigned to each k square garden
▶ Though only one fertilizer will be assigned to the land, many
possible fertilizer assignments were possible beforehand, each
with their own outcome

#### Page 22

Jerzy Neyman’s Notation
▶ For each fertilizer there is an associated “potential yield” that
he collapses into U which he considers to be “a priori fixed
but unknown” (Rubin 1990)
▶ Farmers draw fertilizer from an urn, like a bingo ball from a
bingo ball machine, with replacement and apply it to each
square garden
▶ Fertilizer assignment moves us from “all possible outcomes”
to “realized outcome” terminology
▶ Neyman’s urn model was a classic thought experiment, but it
was also stochastically identical to the completely randomized
experiment
▶ His arch-rival, Ronald Fisher, realizes this and publishes a
book two years later calling for randomizization as the basis
for causal inference

#### Page 23

Treatment assignment mechanism
“Before the 20th century, there appears to have been only
limited awareness of the concept of the assignment mech-
anism. Although by the 1930s, randomized experiments
were firmly established in some areas of scientific investi-
gation, notably in agricultural experiments, there was no
formal statement for a general assignment mechanism and,
moreover, not even formal arguments in favor of random-
ization until Fisher (1925).” (Imbens and Rubin 2015)

#### Page 24

Progress is made and progress is not made
▶ Econometrics traditionally modeled causality in terms of
realized outcomes until recently (with some exceptions)
▶ We need to make a distinction between now the idea of data
(“realized outcomes”) and these hypothetical concepts
represented by Neyman’s notation (“potential outcomes”)
▶ Let’s introduce potential outcomes and assume an
intervention from the pandemic –
▶ Patient is either placed on a ventilator or not (”treatment” is
the ventilator)
▶ What effect will it have on their health (e.g., mortality)?
▶ What is an ”effect” and how will we define it?

#### Page 25

Potential outcomes notation
Let the treatment be a binary variable:
Di,t =
(
1 if placed on ventilator at time t
0 if not placed on ventilator at time t
where i indexes an individual observation, such as a person

#### Page 26

Potential outcomes notation
Potential outcomes:
Y j
i,t =
(
Y 1
i,t health status if placed on ventilator at time t
Y 0
i,t health status if not placed on ventilator at time t
where j indexes a potential treatment status for the same i person
at the same t point in time

#### Page 27

Realized vs potential outcomes
▶ Potential outcome Y j refers to the “a priori fixed but
unknown” outcomes associated with different possible
treatment assignments
▶ Realized outcome Y refers to the “posterior and known”
outcome associated with a specific treatment assignment
▶ Potential outcomes become realized outcomes through
treatment assignment generated by an assignment mechanism
like randomization or rationality

#### Page 28

Models vs Treatment Assignment
▶ Treatment assignment mechanism drives the entire effort to
identify causal effects as some make it easy and some make it
potentially impossible
▶ Did a doctor choose to do it? Did the patient choose to do
it? Did an algorithm choose to do it?
▶ But it’s more than that – why was the choice made, not who
made it
▶ For instance, a doctor might always make it, but it matters if
she flipped a coin vs read the chart and made the ”best
decision”

#### Page 29

Important definitions
Definition 1: Individual treatment effect
The individual treatment effect, δi, associated with a ventilator is
equal to Y 1
i − Y 0
i .

#### Page 30

Important definitions
Definition 2: Switching equation
An individual’s realized health outcome, Yi, is determined by
treatment assignment, Di which selects one of the potential
outcomes:
Yi = Di Y 1
i + (1 − Di)Y 0
i
Yi =
(
Y 1
i if Di = 1
Y 0
i if Di = 0
Not the same as treatment assignment mechanism. Treatment
assignment mechanism describes how D was assigned, not whether
it was assigned.

#### Page 31

Missing data problem
Definition 3: Fundamental problem of causal inference
If you need both potential outcomes to know causality with
certainty, then since it is impossible to observe both Y 1
i and Y 0
i for
the same individual, δi, is unknowable.
This is my reason from saying Mill’s counterfactual framework
derailed the quest for causal effects given counterfactuals are
fictional

#### Page 32

Missing data problem
▶ Fundamental problem of causal inference is deep and
impossible to overcome – not even with more data (you will
always with more data be missing one of the potential
outcomes)
▶ Causal inference is a missing data problem
▶ All of causal inference involves imputing missing
counterfactuals and not all imputations are equal

#### Page 33

Average Treatment Effects
Definition 4: Average treatment effect (ATE)
The average treatment effect is the population average of all i
individual treatment effects
E [δ] = E [Y 1 − Y 0]
= E [Y 1] − E [Y 0]
Aggregate parameters based on individual treatment effects are
summaries of individual treatment effects
Cannot be calculated because Y 1
i and Y 0
i do not exist for the
same unit i

#### Page 34

Conditional Average Treatment Effects
Definition 5: Average Treatment Effect on the Treated (ATT)
The average treatment effect on the treatment group is equal to
the average treatment effect conditional on being a treatment
group member:
E [δ|D = 1] = E [Y 1 − Y 0|D = 1]
= E [Y 1|D = 1] − E [Y 0|D = 1]
Cannot be calculated because Y 1
i and Y 0
i do not exist for the
same unit i .
Why the ATT and ATE will often not be the same? Consider
analyzing the effect of obtaining secondary schooling on
subsequent income. Those with greater natural ability or
connections in the employment market may be better placed to
benefit from additional schooling.

#### Page 35

Conditional Average Treatment Effects
Definition 6: Average Treatment Effect on the Untreated
(ATU)
The average treatment effect on the untreated group is equal to
the average treatment effect conditional on being untreated:
E [δ|D = 0] = E [Y 1 − Y 0|D = 0]
= E [Y 1|D = 0] − E [Y 0|D = 0]
Cannot be calculated because Y 1
i and Y 0
i do not exist for the
same unit i due to switching equation

#### Page 36

Average Treatment Effects are Simple Summaries
▶ Notice how in all three of these, all we did was take the
defined treatment effect at the individual and aggregate
▶ Because aggregate causal parameters are summaries of
individual treatment effects, each of which cannot be
calculated, the aggregates cannot be calculated either
▶ Missing data in this context isn’t missing your earbuds – it’s
missing unicorns and fire breathing dragons (fictional vs real
data)
▶ While we cannot measure average causal effects, we can
estimate them, but only in situations and we review one –
randomization

#### Page 37

Simple Comparisons
Definition 7: Simple difference in mean outcomes (SDO)
A simple difference in mean outcomes (SDO) can be approximated
by comparing the sample average outcome for the treatment group
(D = 1) with a comparison group ( D = 0)
SDO = E [Y 1|D = 1] − E [Y 0|D = 0]
SDO is not a causal parameter because it’s comparing Y 1 and Y 0
for different units, not the same units, so what is it measuring?

#### Page 38

Decomposition of the SDO
Decomposition of the SDO
The SDO is made up of three things:
E [Y 1|D = 1] − E [Y 0|D = 0] = ATE
+E [Y 0|D = 1] − E [Y 0|D = 0]
+(1 − π)(ATT − ATU)
where π is the share of units in the treatment group. Now let’s see
how we get here.

#### Page 39

Rewrite the ATE
▶ Before we get started, let’s look closely at the definition of the
ATE
▶ We can express the ATE as the weighted average of the ATT
and the ATU, and . . .
▶ We can express the ATE as the sum of four conditional means
multiplied by corresponding weights (Law of iterated
expectations)

#### Page 40

Begin with ATE definition
Rewrite the definition of the ATE
ATE = E [Y 1] − E [Y 0]
= πATT + (1 − π)ATU
= πE [Y 1|D = 1] − πE [Y 0|D = 1]
+(1 − π)E [Y 1|D = 0] − (1 − π)E [Y 0|D = 0]
ATE = {πE [Y 1|D = 1] + (1 − π)E [Y 1|D = 0]}
−{πE [Y 0|D = 1] + (1 − π)E [Y 0|D = 0]}
Let’s make this easier to read by replacing the last row with letters

#### Page 41

Change notation
Substitute letters for expectations
E [Y 1|D = 1] = a
E [Y 1|D = 0] = b
E [Y 0|D = 1] = c
E [Y 0|D = 0] = d
ATE = e

#### Page 42

Rewrite ATE definition
Rewrite ATE
e = {πa + (1 − π)b} − {πc + (1 − π)d}

#### Page 43

Simple manipulation of ATE definition
e = {πa + (1 − π)b} − {πc + (1 − π)d}
e = πa + b − πb − πc − d + πd
e = πa + b − πb − πc − d + πd + (a − a) + (c − c) + (d − d)
0 = e − πa − b + πb + πc + d − πd − a + a − c + c − d + d
a − d = e − πa − b + πb + πc + d − πd + a − c + c − d
a − d = e + (c − d) + a − πa − b + πb − c + πc + d − πd
a − d = e + (c − d) + (1 − π)a − (1 − π)b + (1 − π)d − (1 − π)c
a − d = e + (c − d) + (1 − π)(a − c) − (1 − π)(b − d)

#### Page 44

Replace letters with original terms
a−d = e + (c−d) + (1−π)(a−c) −(1−π)(b−d)
E[Y1|D = 1]−E[Y0|D = 0] = ATE
+(E[Y0|D = 1]−E[Y0|D = 0])
+(1−π)({E[Y1|D = 1]−E[Y0|D = 1]}| {z }
ATT
−(1−π)({E[Y1|D = 0]−E[Y0|D = 0]}| {z }
ATU
Red terms are based on missing counterfactuals and therefore cannot be calculated.
This is anidentity

#### Page 45

Decomposition of the SDO
E [Y 1|D = 1] − E [Y 0|D = 0] = ATE
+(E [Y 0|D = 1] − E [Y 0|D = 0])
+(1 − π)(ATT − ATU)
Although we started with π (the share of units in treatment), note
we have weighted the heterogeneity bias term by 1 − π (the share
of units in control)

#### Page 46

Estimate SDO with sample averages
EN[Y |D = 1] − EN[Y |D = 0]| {z }
Estimate of SDO
= E [Y 1] − E [Y 0]| {z }
Average Treatment Effect
+ E [Y 0|D = 1] − E [Y 0|D = 0]| {z }
Selection bias
+ (1 − π)(ATT − ATU)| {z }
Heterogenous treatment effect bias
Using the switching equation and sample averages, we can
calculate EN[Y |D = 1] → E [Y 1|D = 1],
EN[Y |D = 0] → E [Y 0|D = 0] and (1 − π) is the share of the
population in the control group.

#### Page 47

Illustrating selection bias with spreadsheets
▶ Eliminating selection bias requires understanding the selection
mechanism – why did units end up treated but not others?
▶ Perfect Doctor exercise: assume a doctor sees patients, knows
each person’s treatment effects, despite counterfactuals, and
assigns treatment based on whether gains are positive or not
▶ Illustrate decomposition together using numerical example:
https://docs.qq.com/sheet/DUmRWbm1Ec0tJSk9u?no_
promotion=1&tab=BB08J2
▶ We can do it in Stata as well.

#### Page 48

Outline
Foundational ideas
Brief (and selected) history of causal inference
Independence and Selection Bias
Industry example of RCT: eBay advertising
Tennessee’s small class size RCT: STAR

#### Page 49

The “Section”
▶ Hull, Kolesar and Walters (2022) tell the economic history of
causal inference rooted in the Princeton Industrial Relations
Section in the 70s and 80s responding to an “empirical crisis”
in labor
▶ Key players are Orley Ashenfelter (head of the Section), David
Card (his student and 2021 Nobel Laureate), Bob LaLonde
(both men’s student), Josh Angrist (both men’s student and
2021 Nobel Laureate), and numerous others (e.g., Janet
Currie, John DiNardo, Steven Pischke, and many more)
▶ We tend to associate with them the credibility revolution, but
more recently it’s also called a design based approach to
causal inference

#### Page 50

Design vs model based approaches to selection bias
▶ Historically two ways that this selection bias was addressed:
modeling it directly (Heckman, and others) and by design
1. Design-based methods. Think of the randomized experiment.
As we will see randomization will force selection bias to zero
2. Model-based methods. Model the selection bias and then
remove it mechanically (e.g.
https://econ.pages.code.wm.edu/407/notes/docs/mle heckman.html)
▶ Both have been highly influential, but constitute different
approaches, and our course largely focuses on the former not
the latter

#### Page 51

Selection bias and Design
▶ Selection bias in causal inference is when one or both mean
potential outcome differ by treatment status
▶ Source of the bias is caused by the why people get treated, or
what’s called the “treatment assignment mechanism”
▶ But as we will see, if the treatment was assigned randomly
(exogeneity), the implications differ than if they were assigned
”optimally” (endogeneity)

#### Page 52

Treatment assignment mechanisms
▶ Two extreme examples of a treatment assignment mechanism:
1. randomization (i.e., taking the medicine because a coin flip
told you to take it) versus
2. sorting on one or both potential outcome (i.e., taking the
medicine because it’ll help you) which I call the “Perfect
Doctor” but which Heckman and others call a “Roy model”
▶ Bias comes from how treatment is assigned and that
mechanism dictates the direction we have to take

#### Page 53

Three forms of selection bias
▶ In causal inference, selection bias is caused by different mean
potential outcomes by treatment status, of which there are
three possibilities:
1. Selection on Y 0: You chose the treatment because of what
will happen if you didn’t
2. Selection on Y 1: You chose the treatment because of what
will happen if you do
3. Selection on gains, δ: You chose the treatment because the
net benefits were positive
▶ All three cause biased estimates of the ATE, though the
degree to which it fully distorts the estimates depends on
those different reasons for sorting into treatment

#### Page 54

Humans cause selection bias, not models
▶ An unusual paradox in causal inference is that our successful
programs are the hardest to study:
▶ The better our programs are, the better humans get at
assigning treatments to people, the more interventions target
people correctly based on treatment gains, the worse the
selection bias is
▶ But the less efficient our programs, the more erratic the
assignment mechanism is, the more mistakes there are in
targeting people with their best options, the less the selection
bias is
▶ The reason for the “natural experiment” drive in causal
inference was based on what we could deduce about selection
bias from designs, which was more like the latter
▶ But solely depending on the latter would be limiting if our aim
was to study our most successful programs (e.g., food stamps
or the Supplemental Nutrition Assistance Program (SNAP))

#### Page 55

Summarizing the goals of causal inference
Our goal in causal inference is to estimate aggregate causal
parameters with data using treatment assignment mechanisms that
plausibly eliminate selection bias
Depending on the treatment assignment mechanism, certain
procedures are allowed and others are prohibited
Let’s look what happens in an RCT and why this addresses
selection bias term E [Y 0|D = 1] and E [Y 0|D = 0] to see why
Fisher (1925) recommended it

#### Page 56

Independence
Independence assumption
Treatment is assigned to a population independent of that
population’s potential outcomes
(Y 0, Y 1) ⊥ ⊥D
This is random or quasi-random assignment and ensures mean
potential outcomes for the treatment group and control group are
the same. Also ensures other variables are distributed the same for
a large sample.
E [Y 0|D = 1] = E [Y 0|D = 0]
E [Y 1|D = 1] = E [Y 1|D = 0]

#### Page 57

Random Assignment Solves the Selection Problem
EN[yi |di = 1] − EN[yi |di = 0]| {z }
SDO
= E [Y 1] − E [Y 0]| {z }
Average Treatment Effect
+ E [Y 0|D = 1] − E [Y 0|D = 0]| {z }
Selection bias
+ (1 − π)(ATT − ATU)| {z }
Heterogenous treatment effect bias
▶ If treatment is independent of potential outcomes, then
selection bias zeroes out:
E [Y 0|D = 1] − E [Y 0|D = 0] = 0

#### Page 58

Random Assignment Solves the Heterogenous Treatment Effects
▶ How does randomization affect heterogeneity treatment effects bias from the
third line? Rewrite definitions for ATT and ATU:
ATT =E[Y1|D = 1]−E[Y0|D = 1]
ATU =E[Y1|D = 0]−E[Y0|D = 0]
▶ Rewrite the third row bias after 1−π:
ATT−ATU = E[Y1 | D=1]−E[Y0|D = 1]
−E[Y1 | D=0]+E[Y0|D = 0]
= 0
▶ If treatment is independent of potential outcomes, then:
EN[yi|di = 1]−EN[yi|di = 0] = E[Y1] −E[Y0]
SDO = ATE

#### Page 59

Identification with Full Independence
EN[Yi |Di = 1] − EN[Yi |Di = 0]| {z }
Estimate of SDO
= E [Y 1] − E [Y 0]| {z }
Average Treatment Effect
+ 0|{z}
Selection bias
+ 0|{z}
Heterogenous treatment effect bias
SDO is unbiased estimate of ATE with randomized treatment
assignment because it sets selection bias to zero and ATT = ATU.

#### Page 60

Interference when aggregating units
▶ While treatment effects are defined at individual level,
aggregate parameters combine units
▶ This therefore means that for the aggregate parameters to be
stable, one unit’s treatment choice cannot “interfere” with
another unit’s potential outcomes
▶ Placing limits on those possibilities creates challenges for
definitions and estimation that are probably huge headaches,
even in the RCT
▶ Violations are an active area of scholarship and important for
social networks, peer effects and various platforms (e.g.,
Weibo)
▶ The typical responses are either to change the unit of
randomiza tion/analysis, so as to internalize the externality; or
to estimate the externalities direcly. See in particular Miguel
and Kremer (2004) for a paper that grapples with such
externalities.

#### Page 61

SUTVA
▶ SUTVA stands for “stable unit-treatment value assumption”
1. S: stable
2. U: across all units, or the population
3. TV: treatment-value (“treatment effect”, “causal effect”)
4. A: assumption
▶ Largely about interference when aggregating but also poorly
defined treatments and scale

#### Page 62

SUTVA: No spillovers to other units
▶ What if we impose a treatment at one neighborhood but not
a contiguous one?
▶ Treatment may spill over causing Y = Y 1 even for the control
units because of spillovers from treatment group
▶ Can be mitigated with careful delineation of treatment and
control units so that interference is impossible, may even
require aggregation (e.g., classroom becomes the unit, not
students)

#### Page 63

SUTVA: No Hidden Variation in Treatment
▶ SUTVA requires each unit receive the same treatment dosage;
this is what it means by “stable” (i.e., notice that the super
scripts contain either 0 or 1, not 0.55, 0.27)
▶ If we are estimating the effect of aspirin on headaches, we
assume treatment is 200mg per person in the treatment
▶ Easy to imagine violations if hospital quality, staffing or even
the vents themselves vary across treatment group
▶ Be careful what we are and are not defining as the treatment;
you may have to think of it as multiple arms

#### Page 64

SUTVA: Scale can affect stability of treatment effects
Easier to imagine this with a different example.
▶ Let’s say we estimate a causal effect of early childhood
intervention in Sichuan
▶ Now President Xi wants to roll it out for the whole China –
will it have the same effect as we found?
▶ Scaling up a policy can be challenging to predict if there are
rising costs of production
▶ What if expansion requires hiring lower quality teachers just to
make classes?
▶ That’s a general equilibrium effect; we only estimated a
partial equilibrium effect (external versus internal validity)

#### Page 65

Outline
Foundational ideas
Brief (and selected) history of causal inference
Independence and Selection Bias
Industry example of RCT: eBay advertising
Tennessee’s small class size RCT: STAR

#### Page 66

[No extractable text in PDF text layer; image page.]

#### Page 67

Internet advertising facts
▶ In 2012, revenues from Internet advertising was $ 36.6 billion
and has only grown since
▶ Paid search (“search engine marketing”) is the largest format
by revenue (46.3% of 2012 revenues, or $ 16.9 billion)
▶ Google is leading provider (registered $ 46 billion in global
revenues in 2012 of which 95% was attributed to advertising)

#### Page 68

Selection bias
▶ Treatment was targeted ads at particular people conducting
particular types of keyword search
▶ Consumers who choose to click on ads are loyal and already
informed about products with high likelihood to buy already
▶ Problem is ads are targeting people at the end of their search,
so the question is whether they would’ve found it already (i.e.,
E [Y 0|D = 1] ̸= E [Y 0|D = 0])

#### Page 69

Selection bias
▶ Estimated return on investment using OLS found ROI of over
1600%
▶ Compared this to experimental methods and found ROI of
-63% with a 95% CI of [ −124%, −3%], rejecting the
hypothesis that the channel yielded short-run positive returns
▶ Think back to perfect doctor – Even without the treatment
(Y 0), the treated group observationally would’ve still found a
way

#### Page 70

Natural experiment
▶ Study began with a naturally occurring and somewhat
fortuitous event at eBay
▶ eBay halted SEM queries for brand words (i.e., queries that
included the term eBay) on Yahoo! and Microsoft but
continued to pay for these terms on Google
▶ Blake, Nosky and Tadelis (2015) showed almost all of the
foregone click traffic and attributed sales were captured by
natural search
▶ Substitution between paid and unpaid traffic was nearly one
to one complete

#### Page 71

[No extractable text in PDF text layer; image page.]

#### Page 72

Interpretation of natural experiment
“The evidence strongly supports the intuitive notion that
for brand keywords, natural search is close to a perfect
substitute for paid search, making brand keyword SEM
ineffective for short-term sales. After all, the users who
type the brand keyword in the search query intend to reach
the company’s website, and most likely will execute on
their intent regardless of the appearance of a paid search
ad.”

#### Page 73

Selection bias
Observational data masked causal effect (recall the decomposition
of the any non-designed estimation strategy)
“Advertising may appear to attract these consumers, when
in reality they would have found other channels to visit
the company’s website. We overcome this endogeneity
challenge with our controlled experiments.”

#### Page 74

RCT
Natural experiment was valuable, but eBay could run a large scale
RCT.
Use this finding of a nearly one-to-one substitution once paid
search was dropped to convince eBay to field a large scale RCT
discontinuing non-band key words

#### Page 75

Design of the experiment
▶ Randomly assigned 30 percent of eBay’s US traffic to stop all
bidding for all non-brand keywords for 60 days
▶ Some random group of users, in other words, were exposed to
ads; a control group did not see the ads
▶ Used Google’s geographic bid feature that can accurately
identify geographic market of the user conducting the search
▶ Ads were suspended in 30 percent of markets to reduce the
scope of the test and minimize the potential cost and impact
to the business

#### Page 76

Figure: Attributed sales due to clicking on a Google link (treatment
group)

#### Page 77

Figure: Differences in total sales by market (treatment to control)

#### Page 78

Figure: Spending effect on revenue using OLS but not the randomization.
Effects are gigantic.

#### Page 79

Figure: Spending effect on revenue using the randomization DID. Effects
are negative.

#### Page 80

Heterogenous treatment effects
▶ Recall how the potential outcomes model explicitly models
individual treatment effects could be unique and that the
perfect doctor showed selection on gains masked treatment
effects, perhaps even reversing sign
▶ Search advertising in this RCT only worked if the consumer
had no idea that the company had the desired product
▶ Large firms like eBay with powerful brands will see little
benefit from paid search advertising because most consumers
already know that they exist, as well as what they have to
offer

#### Page 81

Figure: Effects on new users are positive and large, but not others.

#### Page 82

Figure: Effects are largest for “least active” customers.

#### Page 83

Why are causal effects small?
▶ They suggest that the brand query tests found small causal
returns because users simply substituted from the paid search
clicks to the natural search clicks
▶ If that’s the case, then it’s explicitly a selection bias story
E [Y 0|D = 1] ̸= E [Y 0|D = 0]
where D is being shown the branded advertisement based on
search (i.e., they were already going there)
▶ They weren’t using branded search for information; they were
using to navigate

#### Page 84

Self selection based on gains
▶ Potential outcomes is the foundation of the physical
experiment because the physical experiment assigns units to
treatments independent of potential outcomes, Y 0, Y 1
▶ This is important because outside of the physical experiment,
we expect people select those important treatments based on
whether, subjectively, they think Y 1 > Y 0 or Y 1 ≤ Y 0.
▶ Rational actors almost by definition are thought to “self-select
into treatment” making non-designed comparisons potentially
misleading – sometimes by a little, sometimes by a lot

#### Page 85

Outline
Foundational ideas
Brief (and selected) history of causal inference
Independence and Selection Bias
Industry example of RCT: eBay advertising
Tennessee’s small class size RCT: STAR

#### Page 86

Example 1: Krueger (1999)
▶ Krueger (1999) econometrically re-analyzes a randomized
experiment to determine the causal effect of class size on
student achievement
▶ The project is the Tennessee Student/Teacher Achievement
Ratio (STAR) experiment from the 1980s
▶ 11,600 students and their teachers were randomly assigned to
one of the following three groups:
1. Small class of 13-17 students
2. Regular class of 22-25 students
3. Regular class of 22-25 students with a full-time teacher’s aide
▶ After the assignment, the design called for students to remain
in the same class type for four years
▶ Randomization occurred within schools

#### Page 87

Regression analysis of experiments
▶ With randomization one could simply calculate SDO (simple difference in mean
outcomes) for the treatment and control group and know that SDO=ATE
because of independence
▶ Nonetheless, it is often useful to analyze experimental data with regression
analysis (see MW section 3.2.2; MHE ch. 2)
▶ Assume that treatment effects are constant – i.e.,Y1i −Y0i =δ ∀i
▶ Substitute into a rearranged switching equation (Definition 2):
Yi = DiY1i + (1−Di)Y0i
Yi = Y0i + (Y1i −Y0i )Di
Yi = Y0i +δDi
Yi = E[Y0i ] +δDi +Y0i −E[Y0i ]
Yi = α+δDi +ηi
whereηi is the random part ofY0i
▶ This is a regression equation that could be used to estimate the causal effect of
D onY

#### Page 88

Regression analysis of experiments (cont.)
▶ The conditional expectation, E [Yi |Di], with treatment status
switched on and off gives:
E [Yi |Di = 1] = α + δ + E [ηi |Di = 1]
E [Yi |Di = 0] = α + E [ηi |Di = 0]
▶ Subtracting the latter from the former, we get:
E [Yi |Di = 1] − E [Yi |Di = 0]| {z }
SDO
= δ|{z}
Treatment Effect
+ E [ηi |Di = 1] − E [ηi |Di = 0]| {z }
Selection bias

#### Page 89

▶ We can estimate SDO using least squares but there’s other
options as well
▶ In the STAR experiment, Di, equalled one if the student was
enrolled in a small class and had been randomly assigned
▶ Recall that randomization implies that treatment is
independent of potential outcomes, and therefore the selection
bias vanishes

#### Page 90

Why Include Control Variables?
▶ To evaluate experimental data, one may want to add
additional controls in the multivariate regression model. So,
instead of estimating the prior equation, we might estimate:
Yi = α + δDi + X ′
i γ + ηi
▶ There are 2 main reasons for including additional controls in
the regression models:
1. Conditional random assignment. Sometimes randomization is
done conditional on some observable. (here that’s the school).
2. Additional controls increase precision. Although control
variables Xi are uncorrelated with Di , they may have
substantial explanatory power for Yi . Including controls thus
reduces variance in the residuals which lowers the standard
errors of the regression estimates.

#### Page 91

Regression in Krueger (1999)
Krueger estimates the following econometric model
Yics = β0 + β1SMALLcs + β2REG/Acs + X ′
i γ + αs + εics
▶ i indexes a student, c indexes a class, and s indexes a school
▶ Yics is the student’s percentile score
▶ SMALLcs is a dummy equalling 1 if she is assigned to a small
class.
▶ REG /Acs is a dummy equalling 1 if she was assigned to a
regular class with an aide
▶ X includes other controls
▶ α is a “school fixed effect” which is a vector of school-specific
dummy variables. He conditions on school fixed effects
because randomized classroom assignment occurred within
schools.

#### Page 92

Regression results Kindergarten
whethertheteacherhasamaster’sdegree—doesnothaveasystematiceffect.Hardlyanyoftheteachersaremale,sothegenderresultsarenotverymeaningful.Teacherexperiencehasasmall,positiveeffect.Experimentationwithaquadraticinexperi-enceindicatedthattheexperienceproéletendstopeakatabout
TABLEVOLSANDREDUCED-FORMESTIMATESOFEFFECTOFCLASS-SIZEASSIGNMENTONAVERAGEPERCENTILEOFSTANFORDACHIEVEMENTTEST
ExplanatoryvariableOLS:actualclasssizeReducedform:initialclasssize(1)(2)(3)(4)(5)(6)(7)(8)A.KindergartenSmallclass4.825.375.365.374.825.375.365.37(2.19)(1.26)(1.21)(1.19)(2.19)(1.25)(1.21)(1.19)Regular/aideclass.12.29.53.31.12.29.53.31(2.23)(1.13)(1.09)(1.07)(2.23)(1.13)(1.09)(1.07)White/Asian(15——8.358.44——8.358.44yes (1.35)(1.36) (1.35)(1.36)Girl(15yes)——4.484.39——4.484.39(.63)(.63)(.63)(.63)Freelunch(15——213.15213.07——213.15213.07yes) (.77)(.77)(.77)(.77)Whiteteacher———2.57———2.57(2.10)(2.10)Teacherexperience———.26———.26(.10) (.10)Master’sdegree———2.51———2.51(1.06)(1.06)SchooléxedeffectsNoYesYesYesNoYesYesYesR2 .01.25.31.31.01.25.31.31B.FirstgradeSmallclass8.578.437.917.407.547.176.796.37(1.97)(1.21)(1.17)(1.18)(1.76)(1.14)(1.10)(1.11)Regular/aideclass3.442.222.231.781.921.691.641.48(2.05)(1.00)(0.98)(0.98)(1.12)(0.80)(0.76)(0.76)White/Asian(15——6.976.97——6.866.85yes) (1.18)(1.19) (1.18)(1.18)Girl(15yes)——3.803.85——3.763.82(.56)(.56)(.56)(.56)Freelunch(15——213.49213.61——213.65213.77yes) (.87)(.87)(.88)(.87)Whiteteacher———24.28———24.40(1.96)(1.97)Maleteacher———11.82———13.06(3.33)(3.38)Teacherexperience———.05———.06(0.06)(.06)Master’sdegree———.48———.63(1.07)(1.09)SchooléxedeffectsNoYesYesYesNoYesYesYesR2 .02.24.30.30.01.23.29.30
QUARTERLYJOURNALOFECONOMICS512

#### Page 93

Regression results 1st grade

#### Page 94

Problem 1: Attrition
A common problem in randomized experiments on humans is
attrition – i.e., people leaving the experiment
▶ If attrition is random, then attrition affects the treatment and
control groups in the same way and our estimates remain
unbiased
▶ But in this application, attrition is probably non-random:
especially good students from large classes may have enrolled
in private schools creating a selection bias problem
▶ Krueger addresses this concern by imputing the test scores
(from their earlier test scores) for all children who leave the
sample and then re-estimates the model including students
with imputed test scores.

#### Page 95

Problem 1: Attrition
hadhighertestscores,onaverage,thanstudentsassignedtosmallclasseswhoalsoleftthesample,thenthesmallclasseffectswillbebiasedupwards.Onereasonwhythispatternofattritionmightoccuristhathigh-incomeparentsofchildreninlargerclassesmighthavebeenmorelikelytosubsequentlyenrolltheirchildreninprivateschoolsovertimethansimilarparentsofchildreninsmallclasses.Atheart,adjustingforpossiblenonran-domattritionisamatterofimputingtestscoresforstudentswhoexitedthesample.Withlongitudinaldata,thiscanbedonecrudelybyassigningthestudent’smostrecenttestpercentiletothatstudentinyearswhenthestudentwasabsentfromthesample.15ThesampleusedintheérstpanelofTableVIincludesthelargestnumberofstudentswithnonmissingdataavailableeachgrade.Theseresultscorrespondtothemodelestimatedincolumn7ofTableV,exceptthefreelunchvariableisomittedbecauseit15.Inthecaseofastudentwholeftthesamplebutlaterreturned,theaveragetestscoreintheyearssurroundingthestudent’sabsencewasused.Testscoreswerealsoimputedforstudentswhohadamissingtestscorebutdidnotexitthesample(e.g.,becausetheywereabsentwhenthetestwasconducted).Thistechniqueiscloselyrelatedtothe‘‘last-observation-carry-forward’’methodthathasbeenusedinclinicalstudies.
TABLEVIEXPLORATIONOFEFFECTOFATTRITIONDEPENDENTVARIABLE:AVERAGEPERCENTILESCOREONSAT
Grade
Actualtestdata ActualandimputedtestdataCoefficientonsmallclassdum.SamplesizeCoefficientonsmallclassdum.SamplesizeK5.3259005.325900(.76) (.76)1 6.9566326.308328(.74) (.68)2 5.5962825.649773(.76) (.65)3 5.5863395.4910919(.79) (.63)Estimatesofreduced-formmodelsarepresented.Eachregressionincludesthefollowingexplanatoryvariables:adummyvariableindicatinginitialassignmenttoasmallclass;adummyvariableindicatinginitialassignmenttoaregular/aideclass,unrestrictedschooleffects;adummyvariableforstudentgender;andadummyvariableforstudentrace.Thereportedcoefficientonsmallclassdummyisrelativetoregularclasses.Standarderrorsareinparentheses.
EXPERIMENTALESTIMATES 515
▶ Non-random attrition hardly biases the results.

#### Page 96

Problem 2: Switch Classrooms after Random Assignment
“It is virtually impossible to prevent some students from switching
between class types over time.” (Krueger 1999, p. 506)
Toaddressthispotentialproblem,andthevariabilityofclasssizeforagiventypeofassignment,insomeoftheanalysisthatfollowsinitialrandomassignmentisusedasaninstrumentalvariableforactualclasssize.B.DataandStandardizedTestsStudentsweretestedattheendofMarchorbeginningofAprilofeachyear.ThetestsconsistedoftheStanfordAchieve-mentTest(SAT),whichmeasuredachievementinreading,wordrecognition,andmathingradesK–3,andtheTennesseeBasicSkillsFirst(BSF)test,whichmeasuredachievementinreadingandmathingrades1–3.Thetestsweretailoredtoeachgradelevel.Becausetherearenonaturalunitsforthetestresults,Iscaledthetestscoresintopercentileranks.Speciécally,ineachgradeleveltheregularandregular/aidestudentswerepooled
TABLEIVTRANSITIONSBETWEENCLASS-SIZEINADJACENTGRADESNUMBEROFSTUDENTSINEACHTYPEOFCLASSA.Kindergartentoérstgrade FirstgradeKindergartenSmallRegularReg/aideAllSmall129260481400Regular1267376631526Aide 1227617061589All 1540155814174515B.Firstgradetosecondgrade SecondgradeFirstgradeSmallRegularReg/aideAllSmall143523241482Regular15214982021852Aide 4011515601715All 1627163617865049C.Secondgradetothirdgrade ThirdgradeSecondgrade SmallRegularReg/aideAllSmall156437351636Regular16714851521804Aide 407618571973All 1771159820445413
EXPERIMENTALESTIMATES 507
▶ Interpreting Krueger’s“transition matrix” (above)
▶ If students remained in their same class type over time, all the
off-diagonal elements would be zero
▶ Interpretation: Of the 1,482 first graders assigned to small
classrooms, 1,435 remained in small classes; 23 and 24
switched into regular and regular with aide classes in the
second grade
▶ If students with stronger expected academic potential were
more likely to move into the small classes, then these
transitions would bias a simple comparison of outcomes across
class types.

#### Page 97

Problem 2: Switch Classrooms after Random Assignment
▶ Subjects moved between treatment and control groups. How
to address this?
▶ A common solution to this problem is to use initial classroom
assignment (i.e., small or regular classes) as an instrument for
actual assignment. We will discuss instrumental variables
later, so I will hold off on that now.
▶ Krueger reports regression results where instead of the
student’s actual status as the treatment variable, he regresses
performance against their randomly assigned class size . This
is called the “reduced form” model, and we learn more about
this when we cover IV.
▶ In Kindergarten, OLS and reduced form are the same because
students remained in their initial class for at least one year.
▶ From grade 1 onwards, OLS and reduced form results differ.

#### Page 98

Problem 2: Switch Classrooms after Random Assignment

#### Page 99

Comments
▶ Natural experiments are valuable, but they don’t always have
the same certainty the way an RCT does
▶ We use natural experiments when people won’t let us run the
RCTs we want to run!
▶ Findings from natural experiments often push others to run
RCTs – like at eBay
▶ But RCTs are expensive and timeintensive to run.

#### Page 100

How to run RCT
▶ How to randomise?
▶ Test for balance of covariates between treatment and control
groups
▶ The use of stratified or blocked randomisation
▶ Power calculations
▶ Glennerster and Takavarasha’s 2013 “Running Randomized
Evaluations: A Practical Guide”, a comprehensive applied
manual with an accompanying webpage:
http://runningres.com/.

### DID_PT Violations.pdf — 33 pages (0 without PDF text)

#### Page 1

Difference-in-Differences
Yumin Li
Shanghai University
December 17, 2024

#### Page 2

Violations of PT
▶ Remember that in the canonical DiD model we had:
▶ Two periods and a common treatment date
▶ Identification from parallel trends and no anticipation
▶ A large number of clusters for inference
▶ A second literature has focused on relaxing the second assumption: what if
parallel trends may be violated?
▶ The ideas from this literature apply even if there is non-staggered timing,
although as we’ll see, many of the tools can be applied with staggered timing as
well. Large clusters is maintained throughout.

#### Page 3

Violations of parallel trends
▶ Three substrands of this literature:
▶ Parallel trends only conditional on covariates
▶ Testing for violations of (conditional) parallel trends
▶ Sensitivity analysis and bounding exercises
▶ I will focus on the latter two

#### Page 4

Why might we be skeptical of PT?
▶ Recall PT requires the selection bias to be constant over time. Why might we be
skeptical of this?
▶ E.g. states that pass a minimum wage increase might also change unemployment
insurance at the same time
▶ Then UI is a confound in period 1 but not in period 0
▶ The same confounding factors may have different effects on the outcome in
different time periods
▶ Suppose people who enroll in a job training program are more motivated to find a job
▶ Motivation might matter more in a bad economy than in a good economy

#### Page 5

Why might we be skeptical of PT? Part 2
▶ Another reason to be skeptical of parallel trends is that its validity will often be
functional form dependent
▶ Consider an example:
▶ In period 0, all control units have outcome 10; all treated units have outcome 5.
▶ In period 1, all control units have outcome 15.
▶ If treatment hadn’t occurred, would treated units’ outcome have increased by 5 also
(PT in levels)?
▶ Or would they have increased by 50% ( ∼ PT in logs)?

#### Page 6

▶ Roth and Sant’Anna (2023) show that PT will depend on functional form unless:
▶ Randomization: treated and control group have same dist. of Y (0) in each period
▶ No time effects: distribution of Y (0) doesn’t change over time for either group
▶ A hybrid: θ fraction of the population is as good as randomized; the other 1 − θ
fraction has no time effects.
▶ Absent these conditions, PT will be violated for at least some functional form;
often hard to know if we chose the right one!

#### Page 7

Pre-trends to the rescue...
▶ Luckily, in most DiD applications we have several periods before anyone was
treated
▶ We can test whether the groups were moving in parallel prior to the treatment
▶ If so, then assumption that confounding factors are stable seems more plausible
▶ If not, then it’s relatively implausible that would have magically started moving in
parallel after treatment date
▶ Testing for pre-trends provides a natural plausibility check on the parallel trends
assumption

#### Page 8

Carey, Miller, and Wherry (2020) do a DiD
comparing states who expanded Medicaid in
2014 to states that didn’t.
Report results from “event-study” regres-
sion:
Yits = ϕt+λs+
X
r ̸=−1
Di ×1[t = 2014+r]·βr +ϵit
where Yits is insurance for person i in year
t in state s, and Di = 1 if in an expansion
state.

#### Page 9

▶ Testing for pre-existing trends is a very natural way to assess the plausibility of the
PT assumption
▶ But it also has several limitations, highlighted in recent work (Freyaldenhoven et
al., 2019; Kahn-Lang and Lang, 2020; Roth, 2022)
▶ We will focus on these issues, as well as some solutions. We will focus mainly on
two papers:
▶ Roth (2022 AER:I, “Pre-test with Caution: Event-study Estimates After Testing for
Parallel Trends”)
▶ Rambachan and Roth (2023 RES, “A More Credible Approach to Parallel Trends”)

#### Page 10

Overview of Limitations
▶ Parallel pre-trends doesn’t necessarily imply parallel (counterfactual)
post-treatment trends
▶ If other policies change at the same time as the one of interest — e.g. min wage and
UI reform together — can produce parallel pre-trends but non-parallel post-trends
▶ Likewise, could be that treated/control groups are differentially exposed to
recessions, but there is only a recession in the post-treatment period
▶ Low power: even if pre-trends are non-zero, we may fail to detect it statistically.
(power is a measure of the ability of a hypothesis testing setup to detect a
particular effect if it is truly present.)
▶ Pre-testing issues: if we only analyze cases without statistically significant
pre-trends, this introduces a form of selection bias (which can make things worse)
▶ If we fail the pre-test, what next? May still want to write a paper (especially if
violation is “small”)

#### Page 11

Issue 1 - Low Power
▶ He & Wang (2017) study impacts of placing college grads as village officials in
China
▶ Use an “event-study” approach comparing treated and untreated villages
Yit =
X
k̸=−1
D k
itβk + αi + ϕt + ϵit

#### Page 12

Issue 1 - Low Power
“The estimated coefficients on the leads of treatment ... are statistically indif-
ferent from 0. ... We conclude that the pretreatment trends in the outcomes
in both groups of villages are similar, and villages without CGVOs can serve
as a suitable control group for villages with CGVOs in the treatment period.”
(He and Wang, 2017)

#### Page 13

Issue 1 - Low Power
▶ P-value for H0 : βpre = green dots (no pre-trend): 0.81

#### Page 14

Issue 1 - Low Power
▶ P-value for H0 : βpre = green dots (no pre-trend): 0.81
▶ P-value for H0 : βpre = red dots: 0.81

#### Page 15

Issue 1 - Low Power
▶ P-value for H0 : βpre = green dots (no pre-trend): 0.81
▶ P-value for H0 : βpre = red dots: 0.81

#### Page 16

Issue 1 - Low Power
▶ P-value for H0 : βpre = green dots (no pre-trend): 0.81
▶ P-value for H0 : βpre = red dots: 0.81
▶ P-value for H0 : βpre = blue dots: 0.81

#### Page 17

Issue 1 - Low Power
▶ P-value for H0 : βpre = green dots (no pre-trend): 0.81
▶ P-value for H0 : βpre = red dots: 0.81
▶ P-value for H0 : βpre = blue dots: 0.81
▶ We can’t reject zero pre-trend, but we also can’t reject pre-trends that under smooth
extrapolations to the post-treatment period would produce substantial bias

#### Page 18

More systematic evidence
▶ Roth (2022): simulations calibrated to papers published in AER, AEJ: Applied,
and AEJ: Policy between 2014 and mid-2018
▶ 70 total papers contain an event-study plot; focus on 12 with available data
▶ Evaluate properties of standard estimates/CIs under linear violations of parallel
trends against which conventional tests have limited power (50 or 80%):
▶ Bias often of magnitude similar to estimated treatment effect
▶ Confidence intervals substantially undercover in many cases
▶ Distortions from pre-testing can further exacerbate these issues

#### Page 19

Issue 2 - Distortions from Pre-testing
▶ When parallel trends is violated, we will sometimes fail to find a significant
pre-trend
▶ But the draws of data where this happens are a selected sample. This is known
as pre-test bias.
▶ Analyzing this selected sample introduces additional statistical issues, and can
make things worse!

#### Page 20

Stylized Three-Period DiD Example
▶ Consider a 3-period model ( t = −1, 0, 1) where treatment occurs in last period
▶ No causal effect of treatment: Yit(0) = Yit(1) in all periods
▶ In population, treatment group is on a linear trend relative to the control group
with slope δ
▶ Control group mean in period t: E [Yit(0) | Control group] = 0
▶ Treatment group mean in period t: E [Yit(0) | Treated group] = δ · t
▶ Simulate from this model with Yit equal to the group mean plus independent
normal errors

#### Page 21

Issue 2 - Distortions from Pre-testing
▶ Example: In population, there is a linear difference in trend with slope 3

#### Page 22

Issue 2 - Distortions from Pre-testing
▶ Example: In population, there is a linear difference in trend with slope 3
▶ In actual draws of data, there will be noise around this line

#### Page 23

Issue 2 - Distortions from Pre-testing
▶ In some of the draws of the data, highlighted in blue, the difference between
period -1 and 0 will be insignificant
▶ In the insignificant draws, we tend to underestimate the difference between
treatment and control at t=0

#### Page 24

Issue 2 - Distortions from Pre-testing
▶ When we underestimate the difference between treatment and control at t=0, the
DiD between period 0 and 1 tends to be particularly large when we get an
insignificant pre-trend

#### Page 25

To Summarize
What are the Limitations of Pre-trends Testing?
1. Low Power – May not find significant pre-trend even if PT is violated
2. Pre-testing Issues – Selection bias from only analyzing cases with insignificant
pre-trend
3. If reject pre-trends test, what comes next?
What Can We Do About It?
1. Diagnostics of power and distortions from pre-testing (Roth, 2022, “Pre-Test with
Caution”). See pretrends package.
2. Formal sensitivity analysis that avoids pre-testing (Rambachan and Roth, 2023,
“A More Credible Approach to Parellel Trends”). See HonestDiD package.

#### Page 26

“A More Credible Approach to Parallel Trends”
▶ The intuition motivating pre-trends testing is that the pre-trends are informative
about counterfactual post-treatment trends
▶ Formalize this by imposing the restriction that the counterfactual difference in
trends can’t be “too different” than the pre-trend
▶ This allows us to bound the treatment effect and obtain uniformly valid
(“honest”) confidence sets under the imposed restrictions
▶ Enables sensitivity analysis: How different would the counterfactual trend have
to be from the pre-trends to negate a conclusion (e.g. a positive effect)?

#### Page 27

Restrictions on Violations of PT
▶ Consider the 3-period model ( t = −1, 0, 1) where treatment occurs in last period
▶ Let δ1 be the violation of PT:
δ1 = E [Yi,t=1(0) − Yi,t=0(0) | Di = 1] − E [Yi,t=1(0) − Yi,t=0(0) | Di = 0]
▶ We don’t directly identify δ1, but we do identify its pre-treatment analog, δ−1:
δ−1 = E [Yi,t=−1(0) − Yi,t=0(0) | Di = 1] − E [Yi,t=−1(0) − Yi,t=0(0) | Di = 0]
▶ Key idea: restrict possible values of δ1 given δ−1, or δ−1, ..., δ −k. Intuitively,
counterfactual trend can’t be too different from pre-trend

#### Page 28

Examples of Restrictions on δ
▶ Bounds on relative magnitudes: Require that |δ1| ≤ ¯M|δ−1|
▶ Smoothness restriction: Bound how far δ1 can deviate from a linear
extrapolation of the pre-trend: δ1 ∈ [−δ−1 − M, −δ1 + M]

#### Page 29

Robust confidence intervals
▶ Rambachan and Roth (2023) develop confidence intervals for the treatment effect
of interest under the assumptions on δ discussed above
▶ The CIs account for the fact that we don’t observe the true (population) pre-trend
δpre, only our estimate ˆδpre.
▶ The robust CIs tend to be wider the larger are the confidence intervals on the
pre-trends — intuitive, since if we know less about the pre-trends, we should have
more uncertainty
▶ This contrasts with pre-trends tests, where you’re less likely to reject the null that
δpre = 0 when the SEs are larger!

#### Page 30

Benzarti & Carloni (2019)
▶ BC study the incidence of a cut in the value-added tax on sit-down restaurants in
France. France reduced the VAT on restaurants from 19.6 to 5.5 percent in July
of 2009.
▶ BC analyze the impact of this change using a difference-in-differences design
comparing restaurants to a control group of other market services firms
Yirt =
2012X
s=2004
βs × 1[t = s] × Dir + ϕi + λt + ϵirt, (1)
▶ Yirt = outcome of interest for firm i in region r
▶ Dir = indicator if firm i in region r is a restaurant
▶ Φi , λt = firm and year FEs
▶ Outcomes of interest include firm profits, prices, wage bill & employment.
We focus on impact on profits in first year after reform.

#### Page 31

Event-study coefficients for log profits

#### Page 32

▶ “Breakdown” ¯M for null effect is ∼ 2
▶ Can rule out a null effect unless allow for violations of PT 2x larger than the max
in pre-period

#### Page 33

So to summarize!
▶ Tests of pre-trends are intuitive but not a panacea!
▶ In particular, they may suffer from low power and introduce pre-test bias
▶ Roth (2022) and Rambachan and Roth (2023) provide tools for diagnostics and
sensitivity analysis
▶ And these tools play nicely with recent estimators developed for heterogeneous
treatment effects

### panelanalysis.pdf — 61 pages (0 without PDF text)

#### Page 1

Panel Data Analysis
Yumin Li
Shanghai University
Yumin Li 1 / 61

#### Page 2

Outline
1 Introduction
2 Panel Data Management
3 Regression analysis
4 Hypothesis Testing
5 Extensions
Yumin Li Introduction 2 / 61

#### Page 3

What is Panel Data?
A cross-section (of people, firms, countries, etc.) is observed over time.
Panel data provides observations on the same units in several time periods
(unlike independently pooled cross sections).
Panel data often consist of a very large number of cross-sections over a small
number of time periods.
Yumin Li Introduction 3 / 61

#### Page 4

What Advantages Do Panel Data Offer?
Panel data allows us to...
...examine issues that cannot be studied using either time series or
cross-sectional data.
...deal with unobserved heterogeneity in the micro units.
...analyze dynamics with only a short time series.
...increase the efficiency of estimation.
Yumin Li Introduction 4 / 61

#### Page 5

Outline
1 Introduction
2 Panel Data Management
3 Regression analysis
4 Hypothesis Testing
5 Extensions
Yumin Li Panel Data Management 5 / 61

#### Page 6

Getting Started...
We now consider data from the Panel Study of Income Dynamics.
You can install the relevant files from within Stata. Type:
. n e t from h t t p : / /www. s t a t a = p r e s s . com/ data /mus
. n e t i n s t a l l mus
. n e t g e t mus
You can also download the data from www.stata-press.com/data/mus.html.
Yumin Li Panel Data Management 6 / 61

#### Page 7

The Dataset
Open the data set:
. us e m u s 0 8 p s i d e x t r a c t . dta , c l e a r
The data set contains information on 595 individuals (the cross-sectional
units) over 7 years (1976-1982).
The total number of observations is thus 595 ×7 = 4165.
There are no missing observations (so the data set is balanced).
“Describe” the data
Yumin Li Panel Data Management 7 / 61

#### Page 8

Panel Data Organization (ctd.)
Panel data is usually organized in the long form, with each observation a distinct
individual-time pair.
Inform Stata about the panel and time variables id and t by typing:
. x t s e t i d t
You can now use the time-series operators of Stata ( L., D.,...) and all the xt
commands.
“xtdescribe”
Yumin Li Panel Data Management 8 / 61

#### Page 9

Outline
1 Introduction
2 Panel Data Management
3 Regression analysis
4 Hypothesis Testing
5 Extensions
Yumin Li Regression analysis 9 / 61

#### Page 10

Estimating the Union Wage Premium
We now use the panel data to estimate the union wage premium.
In our case, the premium measures the degree to which wages are higher if
set by a union contract. In general, the vast empirical literature on the issue
finds that union bargaining increase wages above the market rate.
We will see how panel data can be used to overcome some of the difficulties
associated with estimating the wage premium. We restrict the analysis to
men (drop if fem == 1)!
Yumin Li Regression analysis 10 / 61

#### Page 11

The Basic Linear Panel Model
yit =α+ xitβ+ ai +ϵit (1)
i denotes the cross-sectional unit and t the time period.
yit is the dependent variable.
αis a common intercept.
xit are explanatory variables.
ai are unobserved individual-specific (fixed) effects.
ϵit is an error term.
Yumin Li Regression analysis 11 / 61

#### Page 12

The Basic Linear Panel Model
wit =α+ xitβ+γUnionit + ai +ϵit (2)
i denotes the cross-sectional unit and t the time period.
wit is the log of hourly wage.
αis a common intercept.
xit are explanatory variables.
Unionit indicates whether wage is set by a union contract.
ϵit is an error term.
Yumin Li Regression analysis 12 / 61

#### Page 13

The Unobserved Fixed Effect
wit =α+ xitβ+γUnionit + ai +ϵit (3)
The ai captures all unobserved, time constant factors that affect wit.
The unobserved fixed effect is specific to an individual and does not vary over
time.
The unobserved fixed effect is unknown to the researcher.
Examples: ability, ambition...
Yumin Li Regression analysis 13 / 61

#### Page 14

Pooled OLS
wit =α+ xitβ+γUnionit + ai +ϵit (4)
How to estimate the parameter of interest, γ, given our seven years of panel
data?
One possibility is just to pool the data and use OLS.
Do this in Stata using exp, exp2, wks, ed, ind and occ as controls.
Yumin Li Regression analysis 14 / 61

#### Page 15

Pooled OLS
Yumin Li Regression analysis 15 / 61

#### Page 16

What is Wrong with Pooled OLS?
Let us re-write the linear basic panel model as follows:
wit =α+ xitβ+γUnionit + (ai +ϵit) (5)
=α+ xitβ+γUnionit + vit (6)
where vit = ai +ϵit is referred to as the composite error term.
Yumin Li Regression analysis 16 / 61

#### Page 17

Problem 1: Serially Correlated Errors
If w is overpredicted in one year for a given person, then it is likely to be
overpredicted in other years.
The composite error vit = ai +ϵit is serially correlated even if ϵit is i.i.d. with
a variance of σ2
ϵ.
A worker that is more able today will be more able tomorrow and we thus
have:
Cor(vit, vis)̸= 0 for t̸= s if ai̸= 0. (7)
We can easily show this in Stata...
Yumin Li Regression analysis 17 / 61

#### Page 18

Problem 1: Serially Correlated Errors
Yumin Li Regression analysis 18 / 61

#### Page 19

Solution: Cluster-Robust Standard Errors
With serially correlated errors, standard errors are thus biased.
Calculate cluster-robust standard errors that allow for correlation within
clusters (cross-sections).
Cluster-robust standard errors only require that errors are independent
between cross-sections.
Use the vce(cluster) option in Stata...
Yumin Li Regression analysis 19 / 61

#### Page 20

Problem 2: Omitted Variables
wit =α+ xitβ+γUnionit + vit (8)
We must assume that vit is uncorrelated with Union it for OLS to consistently
estimate γ.
So even if ϵit is uncorrelated with Union it, pooled OLS is biased and
inconsistent if ai and Union it are correlated.
The resulting heterogeneity bias is caused from omitting a time-constant
variable.
Yumin Li Regression analysis 20 / 61

#### Page 21

Problem 2: Omitted Variables
Why should ai and Union it be correlated?
Unobserved factors that affect wages may also affect workers’ selection into
the union.
Wage standardization policy of unions might be most appealing to workers
with low underlying earnings potential.
Unionised employers might pick workers from the queue, as not all workers
who desire union employment can find union jobs.
⇒Unionit might be positively or negatively correlated with ability.
Yumin Li Regression analysis 21 / 61

#### Page 22

The Fixed Effects Model
The fixed effects model allows the unobserved effects to be correlated with
the explanatory variables.
In fact, it uses a transformation to remove the unobserved effect prior to
estimation.
Yumin Li Regression analysis 22 / 61

#### Page 23

The Fixed Effects Transformation
Consider our basic linear panel model:
yit =α+ xitβ+ ai +ϵit (9)
For each cross-section, average this equation over time:
yi =α+ x iβ+ ai +ϵi, (10)
where the average of some variable z is given by z i = T −1 ∑T
t=1 yit.
Yumin Li Regression analysis 23 / 61

#### Page 24

The Fixed Effects Transformation (ctd.)
Now subtract equation (10) from (9) to get rid of the fixed effect:
(yit−yi) = (α−α) + (xit−x i)β+ (ai−ai) + (ϵit−ϵi)
= (xit−x i)β+ (ϵit−ϵi). (11)
Because ai has been eliminated, OLS leads to consistent estimates of βeven if xit
is correlated with ai!
Yumin Li Regression analysis 24 / 61

#### Page 25

Re-Estimate the Union Wage Premium
Now use xtreg, fe in STATA to re-estimate the union wage premium using the
fixed effects model:
wit−wi = (xit−xi)β+γ(Unionit−Unioni) + (ϵit−ϵi) (12)
Yumin Li Regression analysis 25 / 61

#### Page 26

The Fixed Effects Estimates
Yumin Li Regression analysis 26 / 61

#### Page 27

Caveats of the Fixed-Effects Estimator
The fixed effects estimator uses the time variation in y and x within
cross-sectional units only.
It discards variation across cross-sections (between variation).
It does not allow us to estimate the coefficients of time-invariant regressors
(gender, education...).
Differenced regressors may be more susceptible to measurement error.
Does not solve the problem of time-varying omitted variables.
Yumin Li Regression analysis 27 / 61

#### Page 28

Within- and Between-Variation
The STATA command xtsum decomposes the overall variation in a variable as
follows (where s 2
O = s 2
W + s 2
B):
Within: s 2
W = 1
NT−1
∑
i
∑
t
(xit−x i)2 (13)
Between: s 2
B = 1
N−1
∑
i
∑
t
(x i−x)2 (14)
Overall: s 2
O = 1
NT−1
∑
i
∑
t
(xit−x)2 (15)
Use xtsum (and possibly xttrans) to assess the relative importance of between and
within variation in the data.
Yumin Li Regression analysis 28 / 61

#### Page 29

xtsum the Data
Yumin Li Regression analysis 29 / 61

#### Page 30

xttrans union
Yumin Li Regression analysis 30 / 61

#### Page 31

Within and Between R 2
STATA’sxtreg calculates the following three R 2 measures:
Within R 2:ρ2{(yit−y i), (xit−x i) ˆβ}, (16)
Between R 2:ρ2{y i, x i ˆβ}, (17)
Overall R 2:ρ2{yit, xit ˆβ}, (18)
where ρ2{x, y}denotes the squared correlation between x and y.
Yumin Li Regression analysis 31 / 61

#### Page 32

LSDV and First-Difference Estimators
There are two other estimators that also allow the unobserved fixed-effect to be
correlated with the regressors:
1 Least-squares dummy variables (LSDV) estimator
2 First-difference (FD) estimator
Both estimators are also widely used in practice but share the caveats of the fixed
effects estimator. LSDV and the fixed effects estimator are always algebraically
equivalent, regardless of T . First-difference estimator will produce the exactly
same results as them if T = 2, not the same if T > 2.
Yumin Li Regression analysis 32 / 61

#### Page 33

The Dummy Variables Regression
The LSDV regression considers the unobserved effects ai as parameters to be
estimated.
It directly estimates yit =α+ xitβ+ ∑
i
ai Di +ϵit adding a dummy for each
cross-sectional unit i.
Yumin Li Regression analysis 33 / 61

#### Page 34

The Dummy Variables Regression
The LSDV regression gives us exactly the same estimate of βas the
fixed-effects estimator.
It does not allow us to estimate the coefficients of time-invariant regressors
(why?).
Use areg or reg to estimate the union wage premium using the LSDV regression
Yumin Li Regression analysis 34 / 61

#### Page 35

The LSDV Estimates
Yumin Li Regression analysis 35 / 61

#### Page 36

First-difference Estimator
Consider again our basic linear panel model
yit =α+ xitβ+ ai +ϵit (19)
and lag it one period:
yi,t−1 =α+ xi,t−1β+ ai +ϵi,t−1. (20)
Yumin Li Regression analysis 36 / 61

#### Page 37

First-difference Estimator (ctd.)
Now subtract equation (20) from (19) to obtain the first-differenced equation...
(yit−yi,t−1) = (α−α) + (xit−xi,t−1)β+ (ai−ai)
+ (ϵit−ϵi,t−1)
= (xit−xi,t−1)β+ (ϵit−ϵi,t−1) (21)
Because ai has been eliminated, OLS leads to consistent estimates of βeven if xit
is correlated with ai!
Yumin Li Regression analysis 37 / 61

#### Page 38

Re-Estimate the Union Wage Premium
Now use STATA to re-estimate the union wage premium using the model in first
differences:
wit−wi,t−1 = (xit−xi,t−1)β+γ(Unionit−Unioni,t−1) + (ϵit−ϵi,t−1) (22)
You should use the time-series operator for differences D...
Yumin Li Regression analysis 38 / 61

#### Page 39

The First-difference Estimates
Yumin Li Regression analysis 39 / 61

#### Page 40

The Random Effects Estimator
The FE / LSDV / FD estimators wipe out between variation (i.e., variation
across cross-sections).
If we believe that the ai’s are not correlated with the regressors, we should
resort to more efficient estimators.
One possibility is pooled OLS with clustered-robust standard errors.
A more efficient estimator is the Random Effects (RE) estimator.
Yumin Li Regression analysis 40 / 61

#### Page 41

The Random Effects Estimator (ctd.)
Consider again our basic linear panel model:
yit =α+ xitβ+ ai +ϵit (23)
Now suppose that ai is purely random with mean 0 and variance σ2
α.
Thus, ai is assumed to be uncorrelated with the regressors.
Yumin Li Regression analysis 41 / 61

#### Page 42

The Random Effects Estimator (ctd.)
Consider again our basic linear panel model:
yit =α+ xitβ+ ai +ϵit (24)
Yet, the composite error vit = ai +ϵit is still serially correlated.
Under the random effects assumption, Var(vit) =σ2
α+σ2
ϵand
Cov(vit, vis) =σ2
α, s̸= t.
The intraclass correlation of the error is thus given by
ρv = σ2
α
σ2α+σ2ϵ
(25)
Yumin Li Regression analysis 42 / 61

#### Page 43

The Random Effects Estimator (ctd.)
The Random Effects Model is the OLS estimator in a model transformed to have
serially uncorrelated errors. The transformed model is given by:
(yit−ˆθi yi) = (1−ˆθi)α+ (xit−ˆθi x i)β+ (1−ˆθi)ai
+ (ϵit−ˆθiϵi) (26)
where ˆθi is a consistent estimator of
θ= 1−
√
σ2α/(Tσ2α+σ2ϵ), (27)
which is bounded by 0 and 1.
Yumin Li Regression analysis 43 / 61

#### Page 44

Random Effects or Fixed Effects (ctd.)
It is worth noting that the RE model is a weighted average of the within and the
between model:
(yit−yi) + (1−ˆθi)yi = (xit−x i)β+ (ϵit−ϵi)
+ (1−ˆθi)(α+ x iβ+ ai +ϵi) (28)
Forθ= 0, we obtain pooled OLS.
Forθ= 1, we obtain the within model.
Yumin Li Regression analysis 44 / 61

#### Page 45

Random Effects Estimates
Yumin Li Regression analysis 45 / 61

#### Page 46

Estimates of Variance Components
The composite error vit = ai +ϵit is referred as ui + eit in the Stata output.
Stata provides the estimates of the std. dev. of ai (denoted sigma u) and ϵit
(denoted sigma e).
The output rho is an estimate of the intraclass correlation of the error
ρv = σ2
α
σ2α +σ2ϵ
.
For the RE model, the estimated ˆθcan be obtained using the theta option.
Yumin Li Regression analysis 46 / 61

#### Page 47

Comparison of Estimates
Now recap and store the different estimates by typing:
quietly regress lwage exp exp2 wks ed union ind occ, vce(cluster id)
estimates store OLS rob
quietly xtreg lwage exp exp2 wks ed union ind occ, fe
estimates store FE
quietly xtreg lwage exp exp2 wks ed union ind occ, re
estimates store RE
estimates table OLS rob FE RE, b se stats(N r2 r2 o r2 b r2 w)
Yumin Li Regression analysis 47 / 61

#### Page 48

Comparison of Estimates
Yumin Li Regression analysis 48 / 61

#### Page 49

Outline
1 Introduction
2 Panel Data Management
3 Regression analysis
4 Hypothesis Testing
5 Extensions
Yumin Li Hypothesis Testing 49 / 61

#### Page 50

Hypothesis Testing
In applied work, you may want to test whether
the cross-sectional intercepts differ at all.
to use the random or fixed effects estimators in case intercepts differ.
Yumin Li Hypothesis Testing 50 / 61

#### Page 51

RE/FE or Pooled OLS?
There is an easy way to test whether the cross-sectional intercepts are
different from each other.
Just run xtreg, fe and calculate the corresponding dummy variable estimates.
Do a simple F test to test whether the coefficients are identical.
Stata actually reports this test below the regression output of xtreg, fe .
If the coefficients do not differ, you can use pooled OLS.
Yumin Li Hypothesis Testing 51 / 61

#### Page 52

RE/FE or Pooled OLS?
Yumin Li Hypothesis Testing 52 / 61

#### Page 53

Random Effects or Fixed Effects?
If the ai’s are correlated with the regressors, only the FE estimator is
consistent.
If the ai’s are uncorrelated with the regressors, both the FE and the RE
estimators are consistent but the RE estimator is more efficient.
The reason is simply that the RE estimator uses both the within and the
between variation of the data.
Yumin Li Hypothesis Testing 53 / 61

#### Page 54

Random Effects or Fixed Effects? (ctd.)
The Hausman test uses the fact that both estimators are consistent under the
null hypothesis of no correlation.
Under the null hypothesis, the two estimates of βshould thus not differ
systematically.
We can implement the test in Stata using hausman.
For the technical details of the test, you may consult Greene (2008).
Yumin Li Hypothesis Testing 54 / 61

#### Page 55

Random Effects or Fixed Effects?
Yumin Li Hypothesis Testing 55 / 61

#### Page 56

Fixed Effects or First Differencing?
For T = 2, the two estimators are the same.
For T≥2 , the estimators are not the same but both are unbiased and
consistent.
For large N, and small T , FE is more efficient if the idiosyncratic error ϵit is
serially uncorrelated.
Yet, there is no easy way to choose between the two estimators and both are
widely used in practice.
Yumin Li Hypothesis Testing 56 / 61

#### Page 57

Outline
1 Introduction
2 Panel Data Management
3 Regression analysis
4 Hypothesis Testing
5 Extensions
Yumin Li Extensions 57 / 61

#### Page 58

Extension: Two-way-effects Model
A standard extension of the basic linear panel model is to allow the intercept to
vary not only over individuals but also over time:
yit =α+ xitβ+λt + ai +ϵit (29)
The time effects λt shift the intercept over time and affect all micro-units
uniformly.
Examples: business cycle movements, common trend in wages...
In short panels, we usually include a full set of time dummies among the xit’s.
Yumin Li Extensions 58 / 61

#### Page 59

Extension: Two-way-effects Model
Re-estimate the wage equation including a full set of time dummies.
You can either generate the dummies by hand or use the i. option of Stata.
Yumin Li Extensions 59 / 61

#### Page 60

FE Estimates with Time Dummies
Yumin Li Extensions 60 / 61

#### Page 61

RE Estimates with Time Dummies
Yumin Li Extensions 61 / 61

### DID.pdf — 39 pages (0 without PDF text)

#### Page 1

Difference-in-Differences
Yumin Li
Shanghai University
December 14, 2024

#### Page 2

What is difference-in-differences (DiD)
▶ DiD is when a group of units are assigned some treatment and then
compared to a group of units that weren’t before and after
▶ One of the most widely used quasi-experimental methods in economics and
increasingly in industry
▶ Predates the randomized experiment by 80 years, but uses basic
experimental ideas about treatment and control groups (just not
randomized)
▶ Uses panel or repeated cross section datasets, binary treatments usually, and
often covariates

#### Page 3

Popularity of Difference-in-Differences Methods
Goldsmith-Pinkham (2024) built on Currie, Kleven and Zwiers (2020) and
document the popularity of DiD within economics.

#### Page 4

Camparison with IV

#### Page 5

Camparison with Experiments and RD

#### Page 6

Camparison with Structural Models

#### Page 7

Why So Popular?
▶ Mild data requirements: You need data from before and after the
intervention and groups of units that are and are not exposed to the
intervention in a given post-treatment period.
▶ Allows for selection on unobservables: PT is compatible with some types of
selection on unobservables.
▶ Easy-to-use software: Traditional and modern DiD tools are available in
Stata, R, and Python, including the very recent ones.
▶ Possibility to assess the plausibility of PT: We often observe several
pre-treatment periods that we can use to assess the plausibility of PT.
▶ We can learn about treatment effect dynamics: We can leverage event-study
aggregations to understand better how treatment effects evolve with elapsed
treatment timing.

#### Page 8

Our Focus
The questions about DID:
▶ Should I believe parallel trends holds in this context?
▶ Why do I have pre-trends in some of my specifications but not others?
▶ Is it okay if I focus only on the specifications without pre-trends...?
In Roth, Sant’Anna, Bilinski, and Poe (2023), they synthesize the recent
literature and provide concrete recommendations for practitioners

#### Page 9

(Approximate) Schedule
▶ Preliminaries & The Canonical DiD Model
▶ Staggered treatment timing and heterogeneous treatment effects
▶ Violations of Parallel Trends

#### Page 10

The simplest case
▶ We will start a description of DiD in the simplest “canonical” case
▶ Why? Because recent DiD lit can be viewed as relaxing various components
of the canonical model while preserving others
▶ In the canonical DiD model, we have:
▶ 2 periods: treatment occurs (for some units) in period 2
▶ Identification of the ATT from parallel trends and no anticipation
▶ Estimation using sample analogs, equivalent to OLS with TWFE
▶ A large number of independent observations (or clusters)

#### Page 11

Canonical DiD – with math
▶ Panel data on Yit for t = 1, 2 and i = 1, ..., N
▶ Treatment timing: Some units ( Di = 1) are treated in period 2; everyone
else is untreated ( Di = 0)
▶ Potential outcomes: Observe Yit(1) ≡ Yit(0, 1) for treated units; and
Yit(0) ≡ Yit(0, 0) for comparison

#### Page 12

Key identifying assumptions
▶ Parallel trends:
E (Y (0) − Yi1(0) | Di) = E (Yi2(0) − Yi1(0) | Di = 0) (1)
▶ “The evolution of earnings for our trainees had they not trained is the same
as the evolution of mean earnings for non-trainees”. Is it testable?
▶ No anticipation: Yi1(1) = Yi1(0)
▶ Intuitively, outcome in period 1 isn’t affected by treatment status in period 2
▶ Often left implicit in notation, but important for interpreting DiD estimand
as a causal effect in period 2
▶ Stale Unit Treatment Value Assumption

#### Page 13

Visualizing PT

#### Page 14

What is and is not parallel trends?
▶ Parallel trends does not mean treatments were randomly assigned (though
random assignment guarantees parallel trends)
▶ Parallel trends does not require that the groups be similar at baseline on
outcomes (though random assignment guarantees that would be)
▶ Parallel trends does require that the comparison group follows a trend in
outcomes that is approximately the same as the counterfactual trend of the
treatment group
▶ Parallel trends allows for the presence of selection bias, but the bias from
selecting into treatment must be the same in period t = 1 as it is in period t
= 2.

#### Page 15

No Anticipation
▶ “No anticipation” simply means that the unit is not treated until it is
treated (and that can be violated with rational forward looking agents but
not always)
▶ Example 1: Tomorrow I win the lottery, but don’t get paid yet. I decide to
buy a new house today. That violates NA
▶ Example 2: Next year, a state lets you drive without a driver license and
you know it. But you can’t drive without a driver license today. This
satisfies NA.

#### Page 16

Identification
▶ Target parameter: Average treatment effect on the treated (ATT) in
period 2
▶
τATT = E [Yi2(1) − Yi2(0)|Di = 1]
▶ Under parallel trends and no anticipation, can show that
τATT = (E [Yi2|Di = 1] − E [Yi1|Di = 1])| {z }
Change for treated
− (E [Yi2|Di = 0] − E [Yi1|Di = 0])| {z }
Change for control
,
a “difference-in-differences” of population means

#### Page 17

Proof of Identification Argument
▶ Start with
E [Yi2 − Yi1|Di = 1] − E [Yi2 − Yi1|Di = 0]
▶ Apply definition of POs to obtain:
E [Yi2(1) − Yi1(1)|Di = 1] − E [Yi2(0) − Yi1(0)|Di = 0]
▶ Use No Anticipation to substitute Yi1(0) for Yi1(1):
E [Yi2(1) − Yi1(0)|Di = 1] − E [Yi2(0) − Yi1(0)|Di = 0]
▶ Add and subtract E [Yi2(0)|Di = 1] to obtain:
E [Yi2(1) − Yi2(0)|Di = 1]+
[(E [Yi2(0)|Di = 1] − E [Yi1(0)|Di = 1]) − (E [Yi2(0)|Di = 0] − E [Yi1(0)|Di = 0])]
▶ Cancel the last terms using PT to get E [Yi2(1) − Yi2(0)|Di = 1] = τATT

#### Page 18

Visualizing Identification

#### Page 19

Estimation and Inference
▶ The most conceptually simple estimator replaces population means with
sample analogs:
ˆτDiD = ( ¯Y12 − ¯Y11) − ( ¯Y02 − ¯Y01)
where ¯Ydt is sample mean for group d in period t
▶ Conveniently, ˆτDID is algebraically equal to OLS coefficient ˆβ from
Yit = αi + ϕt + Ditβ + ϵit, (2)
where Dit = Di ∗ 1[t = 2]. Also equivalent to β from ∆Yi = α + ∆Di β + uit.
▶ Inference: And clustered standard errors are valid as number of clusters
grows large

#### Page 20

Characterizing the recent literature
We can group the recent innovations in DiD lit by which elements of the
canonical model they relax:
▶ Multiple periods and staggered treatment timing
▶ Relaxing or allowing PT to be violated
▶ Inference with a small number of clusters

#### Page 21

Staggered Timing
Remember that in the canonical DiD model we had:
▶ Two periods and a common treatment date
▶ Identification from parallel trends and no anticipation
▶ A large number of clusters for inference
A very active recent literature has focused on relaxing the first assumption: what
if there are multiple periods and units adopt treatment at different
times?
This literature typically maintains the remaining ingredients: parallel trends and
many clusters

#### Page 22

Overview of Staggered Timing Literature
1. Negative results: TWFE OLS doesn’t give us what we want with treatment
effect heterogeneity
2. New estimators: perform better under treatment effect heterogeneity

#### Page 23

Staggered timing set-up
Panel of observations for periods t = 1, ..., T
Suppose units adopt a binary treatment at different dates Gi ∈ {1, ..., T } ∪ ∞
(where Gi = ∞ means “never-treated”)
▶ Literature is now starting to consider cases with continuous treatment &
treatments that turn on/off – that lit is still developing (see Section 3.4 of
Roth et al. (2023))
Potential outcomes Yit(g) – depend on time and time you were first-treated

#### Page 24

Extending the Identifying Assumptions
The key identifying assumptions from the canonical model are extended in the
natural way
Parallel trends: Intuitively, says that if treatment hadn’t happened, all
“adoption cohorts” would have parallel average outcomes in all periods
E [Yit(∞) − Yi,t−1(∞)|Gi = g] = E [Yit(∞) − Yi,t−1(∞)|Gi = g ′] for all g , g ′, t
Note: can impose slightly weaker versions (e.g. only require PT post-treatment)
No anticipation: Intuitively, says that treatment has no impact before it is
implemented
Yit(g) = Yit(∞) for all t < g

#### Page 25

Negative results
Suppose we again run the regression
Yit = αi + ϕt + Ditβ + ϵit,
where Dit = 1[t ≥ Gi] is a treatment indicator.
Suppose we’re willing to assume no anticipation and parallel trends across all
adoption cohorts as described above
Good news: if treatment effects are constant across time and units,
Yit(g) − Yit(∞) ≡ τ, then β = τ
Bad news: if treatment effects are heterogeneous, then β may put negative
weights on treatment effects for some units and time periods
▶ E.g., if treatment effect depends on time since treatment,
Yit(t − r) − Yit(∞) = τr , then some τrs may get negative weight

#### Page 26

Where do these negative results come from?
The intuition for these negative results is that the TWFE OLS specification
combines two sources of comparisons:
1. Clean comparisons: DiD’s between treated and not-yet-treated units
2. Forbidden comparisons: DiD’s between two sets of already-treated units
(who began treatment at different times)
These forbidden comparisons can lead to negative weights: the “control group”
is already treated, so we run into problems if their treatment effects change over
time

#### Page 27

Some intuition for forbidden comparisons
Consider the two period model, except suppose now that our two groups are
always-treated units (treated in both periods) and switchers (treated only in
period 2)
With two periods, the coefficient β from Yit = αi + ϕt + Ditβ + ϵit is the same
as from the first-differenced regression ∆ Yi = α + ∆Di β + ui
Observe that ∆ Di is one for switchers and zero for stayers.
That is, the stayers are the control group! Thus,
ˆβ =
  ¯YSwitchers,2 − ¯YSwitchers,1

| {z }
Change for switchers
−
  ¯YAT,2 − ¯YAT,1

| {z }
Change for always treated
Problem: if the treatment effect for the always-treated grows over time, that will
enter ˆβ negatively!
With multiple periods/staggered timing, ˆβ includes both this type of comparisons
and clean comparisons

#### Page 28

Second Intuition for Negative Weights
The Frisch-Waugh-Lovell theorem says that we can obtain the coefficient β in
Yit = αi + ϕt + Ditβ + ϵit by the following two-step procedure.
First, regress the treatment indicator Dit on the FEs (a linear probability model):
Dit = ˜αi + ˜ϕt + ˜ϵit
Then run a univariate regression of Yit on Dit − ˆDit to obtain β.
▶ Thus, β = Cov (Yit ,Dit − ˆDit )
Var(Dit − ˆDit ) = E (Yit (Dit − ˆDit ))
Var(Dit − ˆDit )
However, it’s well known that the linear probability model for Dit may have
predictions outside the unit interval. If ˆDit > 1 even though unit i is treated in
period t, then Dit − ˆDit < 0, and thus Yit gets negative weight.

#### Page 29

Not just negative but weird...
The literature has placed a lot of emphasis on the fact that some treatment
effects may get negative weights
But even if the weights are non-negative, they might not give us the most
intuitive parameter
For example, suppose each unit i has treatment effect τi in every period if they
are treated (no dynamics). Then β gives a weighted average of the τi where the
weights are largest for units treated closest to the middle of the panel
It is not obvious that these weights are relevant for policy, even if they are all
non-negative!

#### Page 30

Issues with dynamic TWFE
Sun and Abraham (2021) show that similar issues arise with dynamic TWFE
specifications:
Yi,t = αi + λt +
X
k̸=0
γkD k
i,t + εi,t,
where D k
i,t = 1 {t − Gi = k} are “event-time” dummies.
This specification yields a sensible estimand when there is heterogeneity only in
time since treatment. When there are heterogeneous dynamic treatment effects
across adoption cohorts, the coefficients become difficult to interpret.
Like for the static spec, γk may put negative weight on treatment effects after k
periods for some units
SA also show that γk may be “contaminated” by treatment effects at lags k ′ ̸= k,
i.e., γ2 may be influenced by the treatment effect 3 periods after treatment.

#### Page 31

Dynamic TWFE - Continued
The results in SA suggest that interpreting the ˆ γk for k = 1, 2, ... as estimates of
the dynamic effects of treatment may be misleading
These results also imply that pre-trends tests of the γk for k < 0 may be
misleading – could be non-zero even if parallel trends holds, since they may be
“contaminated” by post-treatment effects!

#### Page 32

New estimators (and estimands!)
Several new (closely-related) estimators have been proposed to try to address
these negative weighting issues
The key components of all of these are:
1. Be precise about the target parameter (estimand) – i.e., how do we want to
aggregate treatment effects across time/units
2. Estimate the target parameter using only “clean-comparisons”

#### Page 33

Example – Callaway and Sant’Anna (2021)
Define ATT(g , t) to be ATT in period t for units first treated at period g,
ATT(g , t) = E [Yit(g) − Yit(∞)|Gi = g]
Under PT and No Anticipation, ATT(g , t) is identified as
ATT(g , t) = E [Yit − Yi,g −1|Gi = g]| {z }
Change for cohort g
− E [Yit − Yi,g −1|Gi = ∞]| {z }
Change for never-treated units
Why?
This is a two-group two-period comparison, so the argument is the same as in
the canonical case!

#### Page 34

Proof of Identification Argument
▶ Start with
E [Yit − Yi,g −1|Gi = g] − E [Yit − Yi,g −1|Gi = ∞]
▶ Apply definition of POs to obtain:
E [Yit(g) − Yi,g −1(g)|Gi = g] − E [Yig(∞) − Yi,g −1(∞)|Gi = ∞]
▶ Use No Anticipation to substitute Yi,g −1(∞) for Yi,g −1(g):
E [Yit(g) − Yi,g −1(∞)|Gi = g] − E [Yig(∞) − Yi,g −1(∞)|Gi = ∞]
▶ Add and subtract E [Yit(∞)|Gi = g] to obtain:
E [Yit(g) − Yit(∞)|Gi = g]+
[E [Yit(∞) − Yi,g −1(∞)|Gi = g] − E [Yig(∞) − Yi,g −1(∞)|Gi = ∞]]
▶ Cancel the last term using PT to get
E [Yit(g) − Yit(∞)|Gi = g] = ATT(g , t)

#### Page 35

Example – Callaway and Sant’Anna (2021)
If ATT(g , t) is the target parameter,
ATT(g , t) = E [Yit(g) − Yit(∞)|Gi = g]
We can then estimate this with sample analogs:
[ATT(g , t) = bE [Yit − Yi,g −1|Gi = g]| {z }
Sample change for cohort g
−bE [Yit − Yi,g −1|Gi = ∞]| {z }
Sample change for never-treated
where ˆE denotes sample means.

#### Page 36

Aggregation schemes
If have a large number of observations and relatively few groups/periods, can
report [ATT(g , t)’s directly.
If there are many groups/periods, the [ATT(g , t) may be very imprecisely
estimated and/or too numerous to report concisely

#### Page 37

Aggregation schemes
In these cases, it is often desirable to report sensible averages of the [ATT(g , t)’s.
One of the most useful is to report event-study parameters which aggregate
[ATT(g , t)’s at a particular lag since treatment
▶ E.g. ˆθk =P
g [ATT(g , t + k) aggregates effects for cohorts in the kth
period after treatment
▶ Can also construct for k < 0 to estimate “pre-trends”
C&S discuss other sensible aggregations too – e.g., if interested in whether
treatment effects differ across good/bad economies, may want to “calendar
averages” that pool the [ATT(t, g) for the same year

#### Page 38

Comparisons of new estimators
Callaway and Sant’Anna also propose an analogous estimator using
not-yet-treated rather than never-treated units.
Sun and Abraham (2021) propose a similar estimator but with different
comparisons groups (e.g. using last-to-be treated rather than not-yet-treated)
Borusyak et al. (2021), Wooldridge (2021), Gardner (2021) propose
“imputation” estimators that estimate the counterfactual ˆYit(0) using a TWFE
model that is fit using only pre-treatment data
▶ Main difference from C&S is that this uses more pre-treatment periods, not
just period g − 1
▶ This can sometimes be more efficient (if outcome not too serially
correlated), but also relies on a stronger PT assumption that may be more
susceptible to bias
Roth and Sant’Anna (2021) show that you can get even more precise estimates if
you’re willing to assume treatment timing is “as good as random”

#### Page 39

Personal advice
Don’t freak out about this new literature!
In most cases, using the “new” DiD methods will not lead to a big change in
your results (empirically, TE heterogeneity is not that large in most cases)
▶ The exceptions are cases where there are periods where almost all units are
treated – this is when “forbidden comparisons” get the most weight
The most important thing is to be precise about who you want the comparison
group to be and to choose a method that only uses these “clean comparisons”
In my experience, the difference between the new estimators is typically not that
large – can report multiple new methods for robustness

### 02-Known_Observed_Confounders.pdf — 135 pages (0 without PDF text)

#### Page 1

Known and Observed Confounders
Yumin Li
Shanghai University
April 1, 2025

#### Page 2

Outline
Unconfoundedness and Ignorable Treatment Assignment
Design Trumps Analysis
Aggregate target parameters
Assumptions
Estimators
Subclassification
Exact and Inexact Matching
Propensity scores
Concluding remarks

#### Page 3

Ordinary way of work
1. Download the data
2. Clean the data
3. Estimate the effects
There is a step between (2) and (3) that we will call ”design” and
it’s important

#### Page 4

Good and bad designed studies
▶ Often the RCT is called the gold standard because of its
ability to eliminate selection bias
▶ But you can have poorly designed RCTs, and researchers
often spend months trying to get the design stage right
▶ If that’s true, then wouldn’t the lower ranked designs need to
spend at least that much time if not longer to make it as
strong as possible?
▶ We need a step between (2) and (3) and we will call it
”design” to avoid unnecessary biases

#### Page 5

Technical prowess vs. practical wisdom
▶ In practice, there is often an overemphasis on the analysis
phase, where technical skills and complex statistical methods
are valued more than the foundational design.
▶ However, without a sound design, even the most sophisticated
analysis can lead us astray.
▶ Practical wisdom in study design is just as crucial, if not more
so, than technical prowess in analysis.
▶ Practical wisdom involves understanding the real-world
context, anticipating potential issues, and making informed
decisions about the study design.

#### Page 6

Importance of Design in Causal Inference
▶ This encompasses all activities that occur before we even look
at the outcome data.
▶ The idea here is that before we dive into any data analysis, we
need to have a robust design in place.
▶ The design phase includes conceptualizing the study,
identifying relevant covariates, and establishing the
assignment mechanism.

#### Page 7

Always frame your study as an RCT
▶ Think of observational studies and RCTs on a continuum
▶ A well design observational study can dominate a poorly
designed randomized experiment
▶ First main components of the study are potential outcomes
▶ Write down your core question using potential outcomes – if
you cannot, it’s either not a causal study or you’re not ready
to proceed

#### Page 8

Conceptualizing the Hypothetical Randomized Experiment
▶ Defining the Hypothetical Experiment:
▶ The first step in is to conceptualize the hypothetical
randomized experiment that your observational study aims to
approximate.
▶ This involves a clear and detailed definition of what the
experiment would look like if it were possible to conduct it.

#### Page 9

Conceptualizing the Hypothetical Randomized Experiment
▶ Different hypothetical experiments for the same dataset (e.g.,
prenatal exposures).
▶ A single dataset can be conceptualized in multiple ways,
leading to different hypothetical experiments.
▶ For example, a dataset on prenatal exposures could be used to
study the effects of hormone exposure, barbiturate exposure,
or both.
▶ Each conceptualization leads to a different design and
potentially different causal inferences.

#### Page 10

Prenatal Exposure Example
▶ Dataset with prenatal exposures (hormone levels and
barbiturate usage) can be conceptualized in different
hypothetical experiments:
▶ Hormone Exposure Experiment : Randomly assign pregnant
women to different hormone levels to study effects on
outcomes (e.g., child development).
▶ Barbiturate Exposure Experiment : Randomly assign
pregnant women to different barbiturate levels to study effects
on outcomes.
▶ Factorial Experiment: Randomly assign combinations of
hormone and barbiturate levels to study individual and
combined effects.
▶ Each conceptualization frames the study as a randomized
experiment, shaping the design and selection mechanisms.

#### Page 11

Reconstructing the Assignment Mechanism
▶ Understand how units would be assigned in a randomized
experiment.
▶ Mimic this process in your observational study.
▶ Identify who chose the treatment, what they knew, why they
did it, and what influenced treatment assignment.
▶ If you cannot articulate this, it may not be the time to do the
analysis.

#### Page 12

Assess covariate quality and availability
▶ Specifying treatment conditions and outcome variables.
▶ It is essential to be explicit about the treatment conditions and
outcome variables.
▶ What exactly are the treatments or interventions being
compared?
▶ What outcomes are we measuring?
▶ Clear definitions help in designing a study that closely
approximates the randomized experiment.

#### Page 13

Known and Observed Confounder
▶ We need to think about covariates: which covariates to
control for (confounders, affecting D and Y simultaneously),
which ones to avoid (colliders, when you control for them,
create new previously non-existent spurious correlations), and
which ones improve precision
▶ Confounders select units into treatment and left uncontrolled
create selection bias
▶ When they are observed covariates, we can reduce the
selection bias directly in different ways

#### Page 14

Known and Observed Confounders
▶ We’ll be considering four scenarios with different estimators
under different assumptions:
1. Matching and weighting: Unconfoundedness and common
support
2. Bias correction: Unconfoundedness, weak support
3. Standard regression specifications: Unconfoundedness,
constant treatment effects and functional form restrictions
4. Regression adjustment: Unconfoundedness and functional form
restrictions
▶ Goal is to know which ones to use under which scenarios and
their tradeoffs

#### Page 15

Shifting gears
▶ Having discussed RCTs, we will now move into the practical
realm
▶ You have a non-experimental study.
▶ We now discuss parameters, assumptions and estimation

#### Page 16

Pick the causal parameter of interest
▶ Framing your question as an average treatment effect
▶ Which population are you interested in?
▶ Example: vaccines are meant to be used by everyone, so you
want to know the ATE
▶ Example: remedial summer school aimed at poor performing
students are only meant to be taken by poor performing
students, so you don’t want the ATE (you want the ATT)
▶ Mostly we can estimate ATT.
▶ Heterogeneous treatment effects mean results can differ by
unit, and therefore average treatment effects may different for
different groups

#### Page 17

Aggregate causal parameters have missing potential
outcomes
▶ Every aggregate causal parameter is missing some potential
outcome (e.g., ATT is missing E [Y 0|D = 1]
▶ If we are going to estimate the ATT, then it must be we are
estimating E [Y 0|D = 1] somehow, but how?
▶ If we are using controls, then we in some way, shape or form
“filling in” the missing counterfactual using the covariates of
the comparison group
▶ “At some level, all methods for causal inference can be viewed
as imputation methods, although some more explicitly than
others.” – Imbens and Rubin (2015)

#### Page 18

Missing Counterfactuals
▶ Every aggregate causal parameter is missing some potential
outcome (e.g., ATT is missing E [Y 0|D = 1]
▶ If we are going to estimate the ATT, then it must be we are
estimating E [Y 0|D = 1] somehow, but how?
▶ If we are using controls, then we impute the missing
counterfactual using the covariates of the comparison group
▶ “At some level, all methods for causal inference can be viewed
as imputation methods, although some more explicitly than
others.” – Imbens and Rubin (2015)

#### Page 19

Assumptions
▶ When attempting to estimate aggregate causal parameters
using covariate adjustment, there are two assumptions
1. Unconfoundedness: Selection on observable covariates such
that all remaining variation in the treatment is random
2. Common support: Technically requires exact matches
▶ Both assumptions are needed to estimate all causal
parameters like the ATE, ATT and ATU and both
assumptions are very strong

#### Page 20

Identifying assumption I: Unconfoundedness
(Y 0, Y 1) ⊥ ⊥D|X . There exists a set X of known and quantified
confounders such that after adjusting for them, treatment
assignment is independent of potential outcomes .
E [Y 0|D = 1, X = x] = E [Y 0|D = 0, X = x]
E [Y 1|D = 1, X = x] = E [Y 1|D = 0, X = x]
Conditional on X, treatment assignment is randomly distributed. It
is also called ignorability. Unconfoundedness justifies substituting
units in treatment for control based on X = x – but only if there
are exact matches

#### Page 21

Selection on Observables and Random Choices
▶ Ask yourself ”Who chooses the treatment? What do they
know when they did it? Why did they do it? What did they
hope would happen?”
▶ If you are solving causal inference problems with covariates,
you are saying this:
▶ Covariates chose the treatment for people
▶ Exactly identical people were only flipping coins after that
▶ Selection on observables is a ”strong assumption”, so
sometimes we weaken it both because it’s more plausible, and
because weaker assumptions may be all you need to get your
target parameter

#### Page 22

Assumption: Weak Unconfoundedness
(Y 0) ⊥ ⊥D|X . There exists a set X of known and quantified
confounders such that after adjusting for them, treatment
assignment is independent of Y 0
E [Y 0|D = 1, X = x] = E [Y 0|D = 0, X = x]
Weak unconfoundedness will mean we can replace missing
E [Y 0|D = 1, X = x] with observed E [Y 0|D = 0, X = x] so long as
you have exact matches. Weak unconfoundedness matters for the
ATT but ”strong” unconfoundedness matters for ATE, ATT and
the ATU

#### Page 23

Common support visual
Common support is like having a bridge that allows you to move
soldiers and equipment across a moat.

#### Page 24

Identifying assumption II: Common support
For ranges of X , there is a positive probability of being both
treated and untreated
▶ There exists units in treatment and control in every specific
combination of the conditioning set (e.g., not males and old,
but adult males, adult females, youth male, youth female),
you can’t make the substitutions otherwise
▶ Testable because common support is observable unlike
unconfoundedness, but as you can imagine if the dimensions
of X gets large (and with a continuous covariate it’s infinite!)
then it won’t hold in any finite sample!

#### Page 25

Assumptions combined
But if we have them both (represented below), we can estimate
the ATE through nonparametric matching
1. ( Y 1, Y 0) ⊥ ⊥D|X (strong unconfoundedness)
2. 0 < Pr(D = 1|X ) < 1 (common support)
Comparing groups of individuals who have the same values of X ,
treatment is not based gains, δ.
The second term implies we have people in treatment and control
for every strata of X

#### Page 26

Estimating ATE with Assumptions
Unconfoundedness means you can replace the missing
E [Y 0|D = 1, X = x] with the observed E [Y 0|D = 0, X = x]
because they are in fact the same anyway
E [Y 1 − Y 0|X = x] = E [Y 1|X = x] − E [Y 0|X = x]
= E [Y 1|D = 1, X = x]| {z }
E[Y 1] same for both
− E [Y 0|D = 0, X = x]| {z }
E[Y 0] same for both
= E [Y |D = 1, X = x] − E [Y |D = 0, X = x]| {z }
Switching equation from potential outcomes to realized outcomes
Not testable, but if true, those steps lead you to an estimator

#### Page 27

Estimating ATE with Assumptions
▶ Common support acts as a bridge connecting two groups
potential outcomes through common covariates which allows
you to take weighted averages of differences in means
ATE = E [Y 1 − Y 0] = E

E [Y 1 − Y 0 | X ]

=
Z
E [Y 1 − Y 0|X , D = 1]dPr(X )
=
Z
(E [Y |X , D = 1] − E [Y |X , D = 0]) dPr(X )
▶ You’re taking weighted averages of difference in means over
the distribution of X

#### Page 28

Maybe You Want the ATT
▶ In the social sciences, my opinion is it’s rare you want the
ATE, or if you did, you probably can’t get it
▶ ATT parameters are all about the average effect of a program
on the people in the program
▶ Many research questions are actually about the ATT not the
ATE
▶ Labor market discrimination studies are most likely ATT
questions because you want to know the effect of
discrimination on the minority group (i.e., the one
discriminated against)

#### Page 29

ATT Identification
We can modify those assumptions and weaken both which helps a
lot
1. Y 0 ⊥ ⊥D|X (weak unconfoundedness)
2. Pr(D = 1|X ) < 1 (with Pr(D = 1) > 0) (weak support)
We don’t need full common support because we don’t need to find
counterfactuals for the control group – we only need units in the
control group that match with our treatment group
Selection is weaker too, like I said – they are not entirely irrational,
but who knows if it helps you

#### Page 30

Estimating ATT
Weighted averages under both assumptions:
δATT =
Z
(E [Y |X , D = 1] − E [Y |X , D = 0]) dPr(X |D = 1)
We match units in treatment and control because under weak
unconfoundedness they’re substitutable, and we use weak common
support so that we can actually do it, then we take weighted
averages over the differences.
What assumptions and method would you use to estimate the
ATU? (Y 1 ⊥ ⊥D|X and Pr(D = 1|X ) > 0). I’ve never seen a study
estimate the ATU.

#### Page 31

Outline
Unconfoundedness and Ignorable Treatment Assignment
Design Trumps Analysis
Aggregate target parameters
Assumptions
Estimators
Subclassification
Exact and Inexact Matching
Propensity scores
Concluding remarks

#### Page 32

Causal inference using covariates
▶ Now we will explore estimators that for lack of a better word
“use covariates” to estimate average treatment effect
parameters
▶ I will be discussing them around these topics: weighting and
matching, matching biases (and corrections), regression
▶ Themes about heterogeneous treatment effects, common
support and correct (and incorrect) regression specifications
will be common

#### Page 33

Subclassification method
▶ Wanting to show subclassification as an early effort to tackle
covariates, but which ultimately cannot handle large features
due to common support issues before moving into regression
▶ Titanic sank on maiden voyage April 15, 1912 after hitting an
iceberg in North Atlantic
▶ 2200 on board, but only 700 survived, despite 20 lifeboats
with 60 capacity (1200 potential lives could’ve been saved)
▶ Women and children first was a maritime rule to ration
lifeboats, but there were different cabins (1st class, 2nd class,
etc.) on different levels with different proximity to boats
▶ What was the causal effect of 1st class on survival adjusting
for W and C ?

#### Page 34

Titanic layout
First Class: Decks B-C

#### Page 35

Titanic layout
Second Class: Decks D-E

#### Page 36

Titanic layout
Third Class: Decks F and G

#### Page 37

Titanic exercise
1. Stratify the confounders : Our age and sex variables are
both binary, so we can only create four strata: male children,
female children, male adults, female adults
2. Calculate differences within strata : Calculate average
survival rates for each group within each of the four strata
and difference within strata
3. Calculate probability weights : Count the number of people
in each strata and divide by the total number of souls aboard
(crew and passengers)
4. Aggregate differences across strata using weights :
Estimate the ATE by aggregating the difference in survival
rates over the four strata with each strata-specific difference
weighted by that strata’s weight

#### Page 38

Table 1: Stratified sample
Table: Counts and Titanic survival rates by strata and first class status.
First class All other classes
Strata Obs Mean Obs Mean Total
Male adult 175 0.326 1,492 0.188 1,667
Female adult 144 0.972 281 0.626 425
Male child 5 1 59 0.407 64
Female child 1 1 44 0.613 45
Total observations 325 1,876 2,201

#### Page 39

Table 2: Estimates of aggregate parameters
Table: Differences in survival rates, stratification weights, and estimates
of parameters
Strata Differences in Survival Rates Weight k,ATE Weightk,ATT Weightk,ATU
Male adult 0.138 0.76 0.54 0.80
Female adult 0.346 0.19 0.44 0.15
Male child 0.593 0.03 0.02 0.03
Female child 0.387 0.02 0.00 0.02
No stratification Stratification weighted estimates
[SDO dATE dATT [ATU
Estimated coefficient 0.35 0.20 0.24 0.19

#### Page 40

Curse of dimensionality
▶ We were able to estimate all three causal effect parameters
because for all four strata there were units in both treatment
and control
▶ But if we dropped the only female child in first class from the
data, we’d be in trouble bc there wouldn’t be any way to
calculate a difference for that group
▶ This happens when you have many covariates or some are
continuous – it’s called curse of dimensionality
▶ If we do not have at least one observation in both treatment
and control for all covariate strata , then some of parameters
cannot be estimated due to losing support

#### Page 41

Table: Counts and Titanic survival rates by strata and first class status.
First class All other classes
Strata Obs Mean Obs Mean Total
Male adult 175 0.326 1,492 0.188 1,667
Female adult 144 0.972 281 0.626 425
Male child 5 1 59 0.407 64
Female child 0 n/a 44 0.613 44
Total observations 324 1,876 2,200

#### Page 42

ATT is the only one we can get
bδATT = (0 .137 × 0.54) + (0.346 × 0.44) + (0.593 × 0.02)
= 0 .24 or 24 percentage points (1)

#### Page 43

Table: Differences in survival rates, stratification weights, and estimates
of parameters without perfect stratification
Strata Differences in Survival Rates Weight k,ATE Weightk,ATT Weightk,ATU
Male adult 0.137 0.76 0.54 0.80
Female adult 0.346 0.19 0.44 0.15
Male child 0.593 0.03 0.02 0.03
Female child n/a 0.02 0 0.02
No stratification Stratification weighted estimates
[SDO dATE dATT [ATU
Estimated coefficient 0.35 n/a 0.24 n/a
Differences in survival rates, stratification weights, and estimated parameters. All coefficients should be multiplied
by 100 to get a percentage point change in survival rate as a result of having a first class cabin. Note that the
SDO is a simple difference in mean outcomes and therefore not a weighted average over the strata differences. But
the estimated ATE, ATT and ATU parameters are weighted averages in difference in means using corresponding
stratification weights.

#### Page 44

Why did this happen?
▶ Stratification requires having units in both groups for every
value of X to get ATE
▶ If you want the ATT, you have to have units in the control
group for every treated group based on its value of X (female
children weren’t treated, so didn’t matter)
▶ If you want the ATU, you have to have units in the treatment
group for every control group based on its value of X (female
children weren’t treated, so did matter)
▶ This has a technical word we are going to learn more about
called a “lack of common support”

#### Page 45

Curse of Dimensionality
▶ Stratification methods break down in finite samples because
as increase the number of covariates, the ”dimension” grows
even faster
▶ Assume we have k covariates and we divide each into 3 coarse
categories (e.g., age: young, middle age, old; income: low,
medium, high, etc.)
▶ The number of strata is 3 k. For k = 10, then it’s
310 = 59, 049
▶ The curse of dimensionality is based on the slices of all
interactions of the covariates, not just the covariates, and that
explodes fast

#### Page 46

Empty cells
▶ When some slice is empty, it means there’s no one in that cell
– so maybe you don’t have any female children in first class,
but you do have them in other cabins
▶ When you have empty cells, you can’t match within that
dimension of the data, and so the ”curse of dimensionality” is
one of the reasons why this approach breaks down so fast
▶ Matching methods really force us to see these curses, they’re
often hidden from OLS because OLS (as we’ll see) overcomes
the problem by just assuming a linearity (it doesn’t match; it
extrapolates)

#### Page 47

Outline
Unconfoundedness and Ignorable Treatment Assignment
Design Trumps Analysis
Aggregate target parameters
Assumptions
Estimators
Subclassification
Exact and Inexact Matching
Propensity scores
Concluding remarks

#### Page 48

Common support vs no common support
▶ Common support is what will determine unbiasedness in
nonparametric methods that satisfy unconfoundedness
▶ Strictly speaking common support will mean that we have
exact matches
▶ When we have inexact matches, then nonparametric matching
methods may become biased
▶ Some bias adjustment methods can improve estimation
▶ We’ll review these all now

#### Page 49

Exact matching

#### Page 50

Exact Matching
Matching goes back at least to Rubin’s early work on the
propensity score, but we will start with nearest neighbor matching
as there’s ideas there we draw upon later with synthetic control
Matching will match a treated unit to a comparison unit that is
identical on the known and quantified confounders
If we can’t find one, it means common support failed, the estimate
would need to use nearest neighbors (with matching bias), which
we will discuss

#### Page 51

Training example (unmatched)

#### Page 52

Age Imbalance
Figure: Age distribution of a job training program’s trainees (green)
versus a sample of workers who were not enrolled in the trainee program
(white).

#### Page 53

Simple example
Let’s review together nnmatch.do to see how we do this using a
program called teffects nnmatch in Stata.

#### Page 54

Exact matching
▶ Exact matching finds a person in the control group whose
value of Xj is exactly equal to each person in the treatment
group i
▶ Will not work if the conditioning set includes a continuous
variable
▶ Will also not work if K gets large (curse of dimensionality)
▶ Numerically identical to subclassification

#### Page 55

ATT estimator
We will focus on the ATT for the rest of today and the equation is:
bδATT = 1
NT
X
Di =1
(Yi − Yj(i)) (2)
where Yj(i) is the jth unit matched to the i th unit based on the jth
being “exactly equal to” the i th unit with respect to the X
conditioning set

#### Page 56

Number of matches
What if I find two or more M units with the identical X value?
Then what?
bδATT = 1
NT
X
Di =1

Yi −
 1
M
MX
m=1
Yjm(i)

(3)
Notice that we are only dealing with Y 0
i by matching; The Y 1
i is
fine as is.

#### Page 57

Matching algorithm
1. For each unit i in the treatment group with known and
quantified confounder X = xi, find all units j in the control
group (or donor pool) for whom xi = xj. These j units are our
M matches and M can be one or it can be greater than one if
you want it to be.
2. For each unit i, replace its missing potential outcome, Y 0
i ,
with the matched j units’ realized outcomes, 1
M
P Yj(i), from
Step 1. Do this for all i units in the treatment group.
3. For each unit i, calculate the difference between realized
earnings and matched earnings, bδi = Yi − 1
M
P Yj(i).
4. Finally, estimate the sample ATT by averaging over all i
differences in earnings from Step 3 as 1
NT
Pbδi, where NT is
the number of treatment units.

#### Page 58

Training example using exact matching

#### Page 59

Estimated ATT using Exact Matching
▶ Notice how you don’t use all the data with matching (only the
yellow units)
▶ That’s because not all of the data is relevant or helpful
▶ This is an important point – not all of the data in causal
inference has the variation you need for estimating causal
effects
▶ You’re imputing missing counterfactuals, not predicting
outcomes – it’s a different task

#### Page 60

Inexact matching

#### Page 61

Curse of Dimensionality
▶ If no matches can be found, it means many cells may contain
either only treatment units or only control units but not both
(”empty cells”) which violates our common support
assumption
▶ Sometimes people want to ”bin” or ”coarsen” the covariates,
and you can do that, but then it’s no longer exact matching
and any differences in potential outcomes across the units still
create biases
▶ So here we look at inexact matches and then we look at ways
to reduce the matching biases when common support fails

#### Page 62

Nearest Neighbor Matching
▶ EstimatebδATT by imputing the missing potential outcome of
each treatment unit i using the observed outcome from that
outcome’s “nearest” neighbor j in the control set using X for
the matching
bδATT = 1
NT
X
Di =1
(Yi − Yj(i))
where Yj(i) is the observed outcome of a control unit such
that Xj(i) is the closest value to Xi among all of the control
observations (eg match on X )

#### Page 63

Matching
▶ We could also use the average observed outcome over M
closest matches:
bδATT = 1
NT
X
Di =1
 
Yi −
"
1
M
MX
m=1
Yjm(1)
#!
▶ Works well when we can find good matches for each
treatment group unit, so M is usually defined to be small (i.e.,
M = 1 or M = 2)

#### Page 64

Matching example with single covariate
i Y 1
i Y 0
i Di Xi
1 6 ? 1 3
2 1 ? 1 1
3 0 ? 1 10
4 0 0 2
5 9 0 3
6 1 0 -2
7 1 0 -4

#### Page 65

Matching example with single covariate
i Y 1
i Y 0
i DI Xi
1 6 9 1 3
2 1 0 1 1
3 0 9 1 10
4 0 0 2
5 9 0 3
6 1 0 -2
7 1 0 -4
Question: What is [δATT = 1
NT
X
Di =1
(Yi − Yj(i))?
bδATT = 1
3 · (6 − 9) + 1
3 · (1 − 0) + 1
3 · (0 − 9) = −3.7

#### Page 66

Measuring the matching discrepancy
▶ What does it mean to be close when I am working with a
large number of covariates?
▶ What if we had a way of measuring a match in terms of how
“close” each unit’s Xi value was to the matched Xj
▶ Let’s do that and use the square root of the sum of all
squared differences in each unit’s Xi − Xj(i) as a measure of
how bad the match is
▶ This is called the Euclidean distance

#### Page 67

Euclidean distance
Definition: Euclidean distance
||Xi − Xj || =
q
(Xi − Xj)′(Xi − Xj)
=
vuut
kX
n=1
(Xni − Xnj)2
Let’s do this together – sometimes it helps to manually calculate
this
https:
//docs.qq.com/sheet/DUnJlS0dpdmZpZVpx?tab=BB08J2

#### Page 68

Inexact matching: Random match

#### Page 69

Minimizing the Euclidean distance
▶ Abadie and Imbens (2006) show that there exists a unique
solution to the matching problem that minimizes a given
distance metric
▶ We are minimizing Euclidean distance, and if that is not zero,
then there is matching discreprancy
▶ And since it is a minimum, any other match will have more
matching discreprancies
▶ Matching in R and teffects in Stata (not sure in python)

#### Page 70

Inexact matching by minimizing the Euclidean distance

#### Page 71

Other distance metrics
▶ Our example treated a one unit difference in age and one unit
difference in GPA as the same, but those scales are different
and matter a lot
▶ The Euclidean distance is not invariant to changes in the scale
of the X ’s.
▶ Alternative distance metrics that are invariant to changes in
scale are more commonly used
▶ Normalized Euclidean distance and Mahalanobis distance both
try to normalize it so that scale doesn’t matter

#### Page 72

Normalized Euclidean distance
Definition: Normalized Euclidean distance
A commonly used distance is the normalized Euclidean distance:
||Xi − Xj ||=
q
(Xi − Xj)′bV −1(Xi − Xj)
where
bV −1 = diag(bσ2
1,bσ2
2, . . . ,bσ2
k)

#### Page 73

Normalized Euclidean distance
▶ Notice that the normalized Euclidean distance is equal to:
||Xi − Xj ||=
vuut
kX
n=1
(Xni − Xnj)2
bσ2n
▶ Thus, if there are changes in the scale of Xni, these changes
also affect bσ2
n, and the normalized Euclidean distance does not
change

#### Page 74

Mahalanobis distance
Definition: Mahalanobis distance
The Mahalanobis distance is the scale-invariant distance metric:
||Xi − Xj ||=
q
(Xi − Xj)′bΣ−1
X (Xi − Xj)
where bΣX is the sample variance-covariance matrix of X .

#### Page 75

Matching and the Curse of Dimensionality
▶ The larger the dimensions of the conditioning set, the less
likely common support holds, and you can’t do it because you
need these covariate dimensions to satisfy weak
unconfoundedness!
▶ This problem is caused by the finite dataset, and it introduces
a particular type of selection bias
▶ Abadie and Imbens (2011) derived a way to reduce the bias
(bias adjustment or bias correction) based on the
unconfoundedness assumption and an outcome regression
model

#### Page 76

Deriving the matching bias
bδATT = 1
NT
X
Di =1
(Yi − Yj(i)),
where each i and j(i) units are matched, Xi ≈ Xj(i) and Dj(i) = 0.
Define potential outcomes and switching eq.
µ0(x) = E [Y |X = x, D = 0] = E [Y 0|X = x],
µ1(x) = E [Y |X = x, D = 1] = E [Y 1|X = x],
Yi = µDi (Xi) + εi

#### Page 77

Deriving the matching bias
Substitute and distribute terms
bδATT = 1
NT
X
Di =1
(Yi − Yj(i))
= 1
NT
X
Di =1

(µ1(Xi) + εi) − (µ0(Xj(i)) + εj(i))

= 1
NT
X
Di =1
(µ1(Xi) − µ0(Xj(i))) + 1
NT
X
Di =1
(εi − εj(i))

#### Page 78

Deriving the matching bias
Difference between sample estimate and population parameter is:
bδATT − δATT = 1
NT
X
Di =1
 
µ1(Xi) − µ0(Xj(i)) − δATT

+ 1
NT
X
Di =1
(εi − εj(i))
Add a zero, algebraic manipulation and simplification:
bδATT − δATT = 1
NT
X
Di =1
 
µ1(Xi) − µ0(Xi) − δATT

+ 1
NT
X
Di =1
(εi − εj(i))
+ 1
NT
X
Di =1
 
µ0(Xi) − µ0(Xj(i))

.

#### Page 79

Deriving the matching bias
▶ Matching under unconfoundedness is consistent which means
this:
bδATT − δATT → 0 as N → ∞
.
▶ But in a finite sample, we may never reach the necessary
sample size for unbiaseness
E [
r
1
N (bδATT − δATT)] = E [
r
1
N (µ0(Xi) − µ0(Xj(i)))|D = 1]
▶ When might this happen? When the dimensions, k, are very
large (lots of covariates and/or strata dimensions)

#### Page 80

Bias is often an issue when we match in many dimensions
▶ The difference between Xi and Xj(i) converges to zero very
slowly
▶ The selection bias term, µ0(Xi) − µ0(Xj(i)), converges to zero
very slowly
▶ E [
q
1
N (µ0(Xi) − µ0(Xj(i)))|D = 1] may not converge to zero
and be very large
▶ And so, E [
q
1
N (bδATT − δATT)] may not converge to zero
because the bias of the matching discrepancy is dominating
the matching estimator

#### Page 81

Solutions to matching bias problem
The bias of the matching estimator is caused by large matching
discrepancies ||Xi − Xj(i)|| which is virtually guaranteed by the
curse of dimensionality. However:
1. But the matching discrepancies are observed. We can always
check in the data how well we’re matching the covariates.
2. For bδATT we can sometimes make the matching discrepancies
small by using a large reservoir of untreated units to select the
matches (that is, by making NC large).
3. If the matching discrepancies are large, so we are worried
about potential biases, we can apply bias correction techniques

#### Page 82

Matching with bias correction
▶ Each treated observation contributes
µ0(Xi) − µ0(Xj(i))
to the bias.
▶ Bias-corrected (BC) matching:
bδBC
ATT = 1
NT
X
Di =1
h
(Yi − Yj(i)) − (cµ0(Xi) −cµ0(Xj(i)))
i
wherecµ0(X ) is an estimate of E [Y |X = x, D = 0]. For
example using OLS but other maybe too (neural nets?).
▶ Under some conditions, the bias correction eliminates the bias
of the matching estimator without affecting the estimator’s
variance.

#### Page 83

Intuition
▶ Under unconfoundedness, we know that
E [Y 0|D = 1, X = x] = E [Y 0|D = 0, X = x]
▶ This implies that whatever the relationship is between Y 0 and
X in the control group, it’s the same in the treatment group,
even though we cannot observe Y 0 in the treatment group
▶ We will therefore fit a regression of Y 0 onto X in the D = 0
group only and use that to predict cY 0 in both groups

#### Page 84

Bias correction steps

#### Page 85

Bias correction steps

#### Page 86

Steps
1. Regress Y on X with OLS except only use the matched
sample of control group units:
Y 0
j = α + βXj + εj
where j are the units for which Dj = 0.

#### Page 87

Steps
2. Use the estimates bα and bβ to predict bµ0(X ) for both the i
and the matched j(i) units:
bµ0
i = bα +bβXi
bµ0
j(i) = bα +bβXj(i)

#### Page 88

Steps
3. Subtract bµ0
i (Xi) −bµ0
j(i)(Xj(i)), our estimate of the selection
bias caused by matching discrepancies, from the sample
estimate of the ATT:
bδBC
ATT = 1
NT
X
Di =1

(Yi − Yj(i)) −

bµ0(Xi) −bµ0(Xj(i))


#### Page 89

Steps
4. Estimate Abadie-Imbens robust standard error (Abadie and
Imbens 2006; 2008; 2011)

#### Page 90

Matching bias: Implications for practice
Matching bias arises because of the effect of large matching
discrepancies on µ0(Xi) − µ0(Xj(i)) due to a lack of common
support. To minimize matching discrepancies:
1. Use a small M (e.g., M = 1). Larger values of M produce
large matching discrepancies.
2. Use matching with replacement. Because matching with
replacement can use untreated units as a match more than
once, matching with replacement produces smaller matching
discrepancies than matching without replacement.

#### Page 91

Large sample distribution for matching estimators
▶ Cannot use the bootstrap, so Abadie and Imbens derived the
variance (Abadie and Imbens 2008)
▶ Matching estimators have a Normal distribution in large
samples (provided the bias is small):
p
NT (bδATT − δATT) d− →N(0, σ2
ATT)
▶ For matching without replacement, the “usual” variance
estimator:
bσ2
ATT = 1
NT
X
Di =1
 
Yi − 1
M
MX
m=1
Yjm(i) −bδATT
!2
,
is valid.

#### Page 92

Large sample distribution for matching estimators
▶ For matching with replacement:
bσ2
ATT = 1
NT
X
Di =1
 
Yi − 1
M
MX
m=1
Yjm(i) −bδATT
!2
+ 1
NT
X
Di =0
 Ki(Ki − 1)
M2

cvar(ε|Xi , Di = 0)
where Ki is the number of times observation i is used as a
match.
▶ cvar(Yi |Xi , Di = 0) can be estimated also by matching. For
example, take two observations with Di = Dj = 0 and
Xi ≈ Xj, then
cvar(Yi |Xi , Di = 0) = (Yi − Yj)2
2
is an unbiased estimator of cvar(εi |Xi , Di = 0))

#### Page 93

Outline
Unconfoundedness and Ignorable Treatment Assignment
Design Trumps Analysis
Aggregate target parameters
Assumptions
Estimators
Subclassification
Exact and Inexact Matching
Propensity scores
Concluding remarks

#### Page 94

Propensity score as dimension reduction
▶ Alternative to the distance minimization method is to use
something called the ”propensity score” to summarize all of
the covariates into a single scalar and then weight or match
on it
▶ Rosenbaum and Rubin (1983) developed the propensity score
method
▶ They show that if treatment is independent of potential
outcomes conditional on K covariates, then it will be
independent of potential outcomes conditional on propensity
score.

#### Page 95

Propensity score theorem
“We are interested in estimating the average effect of a binary
treatment on a scaler outcome. If assignment to the treatment is
exogenous or unconfounded, that is, independent of the potential
outcomes given covariates, biases associated with simple
treatment-control average comparisons can be removed by
adjusted for differences in the covariates. Rosenbaum and Rubin
(1983) show that adjusting solely for differences between treated
and control units in the propensity score removes all biases
associated with differences in covariates.” – Kirano, Imbens and
Ridder (2003, Econometrica)

#### Page 96

Formal Definition
Definition of Propensity score
A propensity score is a number bounded between 0 and 1
measuring the probability of treatment assignment conditional on a
vector of confounding variables: p(X ) = Pr(D = 1|X )

#### Page 97

Propensity score theorem
Propensity score theorem
If (Y 1, Y 0) ⊥ ⊥D|X (full unconfoundedness), then
(Y 1, Y 0) ⊥ ⊥D|ρ(X ) where ρ(X ) = Pr(D = 1|X ), the propensity
score
▶ Conditioning on the propensity score is enough to have
independence between D and (Y 1, Y 0) (Rosenbaum and
Rubin 1983)
▶ With full unconfoundedness, you can estimate the ATE, but
you can estimate the ATT with weaker assumptions
(independence with respect to ( Y 0) ⊥ ⊥D|ρ(X ))
▶ But remember – propensity scores don’t solve
unconfoundedness; if you don’t have it with respect to K
dimensions of X , you won’t have it when you collapse it to
the propensity score

#### Page 98

True vs Estimating Propensity Score
▶ Outside the randomized experiment, we don’t know the “true
propensity score”
▶ We use theory or hunches to select covariates, careful not to
include outcomes/colliders
▶ But we still won’t know the functional form (i.e., logit, probit,
polynomials, interactions)
▶ Often then people will use higher order terms and interactions
to provide flexibility but it does technically have
misspecification biases
▶ This is an area most likely where machine learning could
greatly enhance the estimation (take your pick)

#### Page 99

Step 1: Pick your parameter ATE vs ATT vs ATU
▶ Which population are you studying? Only those who were
discriminated against? That’s the ATT
▶ Do you want to imagine “what if blacks and whites were both
discriminated against?” That’s the ATE
▶ The more you can focus on one particular causal parameter,
the easier and more justified it gets as it weakens both
assumptions

#### Page 100

Step 2: Estimate the propensity score
▶ Estimate the conditional probability of treatment using probit
or logit model (or ML)
Pr(Di = 1|Xi) = F (βXi)
▶ Note: don’t use OLS because while it will get the mean right,
it will not get correct values in the tails because of its linear
projections
▶ OLS will give propensity scores outside the [0,1] bounds and
probabilities cannot be negative or greater then one

#### Page 101

Step 2: Estimate the propensity score
▶ Use the estimated coefficients to predict the propensity score
for each unit i
bρi(Xi) = bβXi
▶ Note that each unit i now has a predicted probability of
treatment given the values of their covariates relative to
everyone else’s

#### Page 102

Step 2: Estimate the propensity score
▶ Think of the propensity score as a frequentist concept of
probability
▶ “If I drew someone from the sample with these characteristics,
then how many of those are in the treatment group divided by
the total with those characteristics”
▶ Or for each dimension of X , a ratio of NT
(NT +NC )

#### Page 103

Investigating overlap
▶ Once you obtain the propensity score, you can use it for
estimation, but you can also use it for evaluating covariate
balance
▶ It’s an easy way to do it with histograms, and since the
propensity score theorem holds for the dimensions of X ,
there’s no loss of generality in investigating overlap that way
versus one by one
▶ Remember: you need common support, not on X individual
covariates alone, but within the K dimensions, so
investigating with the propensity score is easier to do

#### Page 104

Covariate 1 histograms

#### Page 105

Covariate 2 histograms

#### Page 106

Propensity scores

#### Page 107

Navigating the vastness of estimation
▶ Estimators using propensity score:
1. Match units from one group to another using the propensity
score (with various rules for finding how close to be)
2. Weighting by the inverse propensity score
▶ Variety of techniques to derive standard errors from
parametric methods to bootstrapping
▶ Can even introduce “doubly robust” methods to deal with
matching bias like we did with nearest neighbor bias correction
▶ But all these estimators assume unconfoundedness, so really
it’s all about addressing the lack of overlap; that’s the only
bias that exists when you have unconfoundedness remember

#### Page 108

Step 3a: Estimation with matching
▶ Most common method is to use matching
▶ Matching finds a unit in the comparison group with a similar
bρi(X ) to service as counterfactual for the unit
▶ For the ATE, you’ll need matches on both side; for the ATT,
you’ll need matches for the treatment group among controls
▶ Lack of overlap creates issues for matching which we’ll note
later

#### Page 109

Step 3b: Estimation with stratification
▶ Rare to see this done anymore, though it was one of the
methods that Dehejia and Wahba (2002) tried
▶ Stratification is a kind of weighting method similar to
Cochran’s subclassification method where weights are group
shares within certain ranges of the propensity score
▶ Method uses brute force to achieve the balancing property
discussed earlier, which is then used with weighted differences
in means within propensity score “strata”

#### Page 110

Step 3b: Achieving Balance
1. First create “propensity score strata” inside which you have
balanced covariates
1.1 Sort the data by propensity score and divide into groups of
observations with similar propensity scores (e.g., percentiles)
1.2 Within each strata, test (e.g., t-test) whether the means of the
k covariates are equal between treatment and control
1.3 If so, then stop. If not, it means the covariates aren’t balanced
within that propensity score strata so then divide that strata in
half and repeat step 2
1.4 If a particular covariate is unbalanced for multiple groups,
modify the initial logit or probit equation by including higher
order terms and/or interactions with that covariate and repeat

#### Page 111

Step 3b: Estimation with stratification
2. Next calculate weights for each region depend on the causal
parameter you’re seeking:
▶ ATE: number of units in that region divided by total units in
sample ;
▶ ATT: number of treated units in that region divided by the
number of treated units in the sample
▶ ATU: number of control units in that region divided by the
number of control units in the sample
3. Calculate simple difference in mean outcomes (i.e., without
controls) for each propensity score region (Step 1)
4. Then take weighted average using weights from step 2 and
difference in means from step 3

#### Page 112

Step 3c: IPW by Hirano, Imbens and Ridder
▶ Hirano, Imbens and Ridder (2003, Econometrica) considers
estimation of the ATT
▶ Focus is on the efficient estimation and they show that while
weighting on the inverse of the true propensity score is not an
efficient estimator, paradoxically weighting each observation
by the inverse of a nonparametric estimator of the propensity
score is

#### Page 113

Step 3c: Estimation with inverse probability weighting
▶ IPW uses the estimated propensity score to reweight the
outcomes
▶ IPW is non-parametric – you are just taking averages and
multiplying by the inverse of the propensity score weights
depending on which parameter you want to estimate
▶ There are fewer implementation choices than in matching
(i.e., no choice over distance, number of neighbors)
▶ There are bias adjustment methods called double robust
where you combine imputing counterfactuals with weighting
by the propensity score

#### Page 114

Step 3c: Estimation of ATT with IPW
Estimating ATT with IPW
Given Y 0 ⊥ ⊥D|X and weak common support, then
δATT = E [Y 1 − Y 0|D = 1]
= 1
Pr(D = 1) · E

Y · D − ρ(X )
1 − ρ(X )

Notice that when D = 1, the outcome is not weighted, but when
D = 0 it is. You’re missing the Y 0 for the treatment, not Y 1 so
you use the treatment group Y values alone and weight “up” or
“down” the comparison groups by their propensity scores

#### Page 115

Step 3c: Estimating ATE with IPW
Estimating ATE with IPW
Given Y 1, Y 0 ⊥ ⊥D|X and common support, then
δATE = E [Y 1 − Y 0]
= E

Y · D − ρ(X )
ρ(X ) · (1 − ρ(X ))

Notice that since treated units are missing counterfactuals, but so
are controls, all of the data is weighted for the ATE (not just the
controls for ATT)

#### Page 116

Inverse Probability Weighting
Proof.
E

Y · D − ρ(X )
ρ(X )(1 − ρ(X ))
X

= E
 Y
ρ(X )
X , D = 1

ρ(X )
+E
 −Y
1 − ρ(X )
X , D = 0

(1 − ρ(X ))
= E [Y |X , D = 1] − E [Y |X , D = 0]
and the results follow from integrating over P(X ) and
P(X |D = 1).

#### Page 117

Step 4: Standard Errors
Standard errors can be constructed a few different ways:
▶ We need to adjust the standard errors for first-step estimation
of ρ(X )
▶ Parameteric first step: Newey and McFadden (1994)
▶ Non-parametric first step: Newey (1994)
▶ IPW is a smooth estimator which means the bootstrap is valid
for inference (Adudumilli 2018 and Bodory et al. 2020)

#### Page 118

Other comments about propensity scores
▶ You can get outlier weights when doing propensity scores
▶ If the propensity score is very high for a control group in the
ATT, the weight can explode
▶ Certain normalizations of the way the IPW is constructed to
try and minimize those influences (Hirano and Imbens, 2001)
▶ Stata’s teffects or R’s ipw both let you estimate
parameters using IPW and get standard errors
▶ But you can do it manually using OLS and use the propensity
scores as analytical weights

#### Page 119

Propensity score matching
▶ Matching, or “imputation”, is another way that utilizes the
bρi(Xi)
▶ Matching estimation based on the propensity score has the
same first step as IPW, but not the second and third steps
▶ Common support starts to be more complex with imputation
methods because you will need to decide how far away from a
unit’s own propensity score is a tolerable distance to be
considered a “neighbor”

#### Page 120

Standard matching strategy
▶ Pair each treatment unit i with one or more comparable
control group unit j, where comparability is in terms of
proximity, or distance, to the estimated propensity score
▶ Impute the unit’s missing counterfactual outcome Yi(j) based
on the unit or units chosen in the previous step
▶ If more than one are “nearest neighbors”, then use the
neighbors’ weighted outcomes
Yi(j) =
X
j∈C(i)
wij Yj
where C (i) is the set of neighbors and wij is the weight of
control group units j withP
j∈C(i) wij = 1

#### Page 121

Imputing the counterfactuals
Let the ATT be our parameter of interest:
E [Y 1
i |Di = 1] − E [Y 0
i |Di = 1]
We estimate it as follows
[ATT = 1
NT
X
i:Di =1

Yi − Yi(j)

where NT is the number of matched treatment units in the
sample. Note the difference between imputation and IPW – the
only weight here is 1
NT

#### Page 122

Matching methods
▶ The probability of observing two units with exactly the same
propensity score is in principle zero if Pr(X = x) is continuous
▶ Several matching methods have been proposed in the
literature, but the most widely used are:
▶ Nearest-neighbor matching (with or without caliper)
▶ Radius matching
▶ Kernel matching
▶ Typically, one treatment unit i is matched to several control
units j, but sometimes one-to-one matching is used

#### Page 123

Nearest Neighbor
Pretty similar to covariate matching. Formula is
[ATT = 1
NT
X
i:Di =1

Yi −
X
j∈C(i)M
wij Yj

▶ NT is the number of treated units i and NC is number of
control units j
▶ wij is equal to 1
NC
if j is a control unit and zero otherwise
▶ And unit j is chosen as a control for i if it’s propensity score is
nearest to that of i

#### Page 124

NN Matching: Bias vs. Variance
How far away on the propensity score will you use is what makes
some of the different types of matching proposed differ
▶ Matching just one nearest neighbor minimizes bias at the cost
of larger variance
▶ Matching using additional nearest neighbors increases the bias
but decreases the variance

#### Page 125

NN Matching: Bias vs. Variance
Matching with or without replacement
▶ with replacement keeps bias low at the cost of larger variance
▶ without replacement keeps variance low but at the cost of
potential bias
People usually choose to do matching with replacement.

#### Page 126

Distance between treatment and control units
▶ What was historically done was limiting “distance” through
various ad hoc choices
▶ There were two common ways for creating the circle – caliper
matching and radius matching.

#### Page 127

Caliper matching
▶ Caliper matching is a variation on NN matching that tries to
build brakes into the algorithm as to avoid “bad neighbors” by
imposing a tolerable maximum distance (e.g., 0.2 units in the
propensity score away from a treatment unit i’s propensity
score)
▶ Note – this is a one-to-one imputation, and if there doesn’t
exist anybody in the control group unit j within that
“caliper”, then treatment unit i is discarded which as with all
trimming changes the parameter we are estimating
▶ It’s difficult to know what this caliper should be ex ante ,
hence why I said it is somewhat ad hoc

#### Page 128

Radius matching
▶ Each treatment unit i is matched with the control group units
whose propensity score are in a “predefined neighborhood” of
the propensity score of the treatment unit.
▶ All the control units with bρj(Xj) falling within a radius r from
bρi(Xi) are matched to the treatment unit i – this is what
distinguishes it from calipers, and makes it more similar to
covariate matching (Abadie and Imbens 2006, 2008)
▶ The smaller the radius, the better the quality of the matches,
but the higher the possibility some treatment units are not
matched because the neighborhood does not contain control
group units j

#### Page 129

Critique
▶ King and Nielsen (2019) is a critique of using propensity
scores for matching (i.e., imputation)
▶ Issues raised have to do with forced balance through trimming
and a myriad of other common choices made by the researcher

#### Page 130

Ad hoc user choices introduce bias
“‘[The] more balanced the data, or the more balance it
becomes by [trimming] some of the observations through
matching, the more likely propensity score matching will
degrade inferences.” – King and Nielsen (2019)

#### Page 131

Software
▶ You can use - teffects, psmatch - to get at these two
nearest neighbor approaches by setting the number of
matches
▶ You can use - pscore2- for stratification
▶ You can use the MatchIt package in R

#### Page 132

Outline
Unconfoundedness and Ignorable Treatment Assignment
Design Trumps Analysis
Aggregate target parameters
Assumptions
Estimators
Subclassification
Exact and Inexact Matching
Propensity scores
Concluding remarks

#### Page 133

Comments
▶ Unconfoundedness means that the confounders are known and
quantified, meaning they’re in your data and well measured
▶ Also means that within the dimensions of those covariates is
an RCT (“independence”)
▶ Common support is also needed with matching
▶ Without a prior behavioral model guiding you, it’s very hard
to defend unconfoundedness (identification by convenience)

#### Page 134

When not to use unconfoundedness methods
▶ Individual sorting based on rationality is strong and subtle
▶ College attendance, major, marriage, children, divorce – very
hard to imagine that for people with identical covariate values
they all flipped coins
▶ Unconfoundedness is a strong assumption, and the weaker
ones (like with respect to Y 0) may be easier to defend which
gets you to the ATT

#### Page 135

Common Support
▶ Unconfoundedness says on average,
E [Y 0|D = 1, X ] = E [Y 0|X ] – that is, it doesn’t depend on D
so you can just switch the known for unknown ones
▶ But even if unconfoundedness holds doesn’t mean your
dataset is large enough that the one to one matches can
happen – that’s failure to have enough matches because the
dimensions are too large
▶ So much of the literature is how to handle failed common
support
▶ Bias adjustment methods are one way to address that but
even they can’t work miracles

### Lecture 5.pdf — 57 pages (0 without PDF text)

#### Page 1

Lecture 5 
Panel Data Methods

#### Page 2

Jian WANG (SILC)
What are panel data?
Data Visualization APRIL 27,2015 2Shanghai UniversityJian WANG (UTS&SHU)

#### Page 3

Jian WANG (SILC)
Panel Data
Data Visualization APRIL 27,2015 3Shanghai UniversityJian WANG (UTS&SHU)

#### Page 4

Jian WANG (SILC)
Pooled cross-sectional data
Data Visualization APRIL 27,2015
Shanghai UniversityJian WANG (UTS&SHU)

#### Page 5

Panel Data Sets: Large N or Large T?
Data Visualization APRIL 27,2015 5Jian WANG (SILC) Shanghai UniversityJian WANG (UTS&SHU)

#### Page 6

Panel Analysis: Decompose Variance of variables
Data Visualization APRIL 27,2015 6Jian WANG (SILC) Shanghai UniversityJian WANG (UTS&SHU)

#### Page 7

Jian WANG (SILC)
Decompose the variation: Stata: xtsum
Data Visualization APRIL 27,2015 7Shanghai UniversityJian WANG (UTS&SHU)

#### Page 8

Jian WANG (SILC)
How panel data can help us identify causal effect?
Data Visualization APRIL 27,2015 8Shanghai UniversityJian WANG (UTS&SHU)

#### Page 9

Jian WANG (SILC)
How panel data can help us identify causal effect?
Data Visualization APRIL 27,2015 9Shanghai UniversityJian WANG (UTS&SHU)

#### Page 10

Jian WANG (SILC)
How panel data can help us identify causal effect?
Data Visualization APRIL 27,2015 10Shanghai UniversityJian WANG (UTS&SHU)

#### Page 11

How panel data can help us identify causal effect?
Data Visualization APRIL 27,2015 11Jian WANG (SILC) Shanghai UniversityJian WANG (UTS&SHU)

#### Page 12

Jian WANG (SILC)
Time-invariant unobservable variable
Data Visualization APRIL 27,2015 12Shanghai UniversityJian WANG (UTS&SHU)

#### Page 13

Random Effects vs. Fixed Effects
Data Visualization APRIL 27,2015 13Jian WANG (SILC) Shanghai UniversityJian WANG (UTS&SHU)

#### Page 14

Random Effects vs. Fixed Effects

#### Page 15

Jian WANG (SILC)
Fixed Effects Model
Data Visualization APRIL 27,2015 15Shanghai UniversityJian WANG (UTS&SHU)

#### Page 16

Fixed Effect Within Estimator
Data Visualization APRIL 27,2015 16Jian WANG (SILC) Shanghai UniversityJian WANG (UTS&SHU)

#### Page 17

Fixed Effect Within Estimator
Data Visualization APRIL 27,2015 17Jian WANG (SILC) Shanghai UniversityJian WANG (UTS&SHU)

#### Page 18

Consistency of b
Data Visualization APRIL 27,2015 18Jian WANG (SILC) Shanghai UniversityJian WANG (UTS&SHU)

#### Page 19

Jian WANG (SILC)
Estimator Fixed Effet αi:
Data Visualization APRIL 27,2015 19Shanghai UniversityJian WANG (UTS&SHU)

#### Page 20

Jian WANG (SILC)
Estimator Fixed Effet αi:
Data Visualization APRIL 27,2015 20Shanghai UniversityJian WANG (UTS&SHU)

#### Page 21

Estimate Fixed Effects Model Method 2: LSDV
Data Visualization APRIL 27,2015 21Jian WANG (SILC) Shanghai UniversityJian WANG (UTS&SHU)

#### Page 22

Least Squares dummy variable (LSDV) estimator
Data Visualization APRIL 27,2015 22Jian WANG (SILC) Shanghai UniversityJian WANG (UTS&SHU)

#### Page 23

LSDV Estimator and Within Estimator: b
Data Visualization APRIL 27,2015 23Jian WANG (SILC) Shanghai UniversityJian WANG (UTS&SHU)

#### Page 24

Jian WANG (SILC)
FEM Method 3: First Differences
Data Visualization APRIL 27,2015 24Shanghai UniversityJian WANG (UTS&SHU)

#### Page 25

Jian WANG (SILC)
FE Practical Issue 1: R-square Goodness of Fit
Data Visualization APRIL 27,2015 25Shanghai UniversityJian WANG (UTS&SHU)

#### Page 26

Goodness of Fit (STATA)
Data Visualization APRIL 27,2015 26Jian WANG (SILC) Shanghai UniversityJian WANG (UTS&SHU)

#### Page 27

Jian WANG (SILC)
STATA Implementation: xtreg, fe
Data Visualization APRIL 27,2015 27Shanghai UniversityJian WANG (UTS&SHU)

#### Page 28

FEM Practical Issue 2: Time Invariant Regressors
Data Visualization APRIL 27,2015 28Jian WANG (SILC) Shanghai UniversityJian WANG (UTS&SHU)

#### Page 29

FEM Practical Issue 2: Time Invariant Regressors
Data Visualization APRIL 27,2015 29Jian WANG (SILC) Shanghai UniversityJian WANG (UTS&SHU)

#### Page 30

Jian WANG (SILC)
FEM Practical Issue 3: Regressors with little within-group variation
Data Visualization APRIL 27,2015 30Shanghai UniversityJian WANG (UTS&SHU)

#### Page 31

Jian WANG (SILC)
FEM Practical Issue 4: Multiple types of FEs
Data Visualization 31Shanghai UniversityJian WANG (UTS&SHU)

#### Page 32

Jian WANG (SILC)
FEM Practical Issue 4: Multiple types of FEs
Data Visualization APRIL 27,2015 32Shanghai UniversityJian WANG (UTS&SHU)

#### Page 33

Jian WANG (SILC)
Random Effects Model
Data Visualization APRIL 27,2015 33Shanghai UniversityJian WANG (UTS&SHU)

#### Page 34

Jian WANG (SILC)
Variance Matrix in Random Effects Model
Data Visualization APRIL 27,2015 34Shanghai UniversityJian WANG (UTS&SHU)

#### Page 35

Jian WANG (SILC)
Random effect model: Algebra
Data Visualization 35Shanghai UniversityJian WANG (UTS&SHU)

#### Page 36

Jian WANG (SILC)
GLS Estimator for Random Effects Model
Data Visualization 36Shanghai UniversityJian WANG (UTS&SHU)

#### Page 37

Jian WANG (SILC)
Understand GLS Estimator for Random Effects Model
Data Visualization 37Shanghai UniversityJian WANG (UTS&SHU)

#### Page 38

Jian WANG (SILC)
STATA: Xtreg, re
Data Visualization 38Shanghai UniversityJian WANG (UTS&SHU)

#### Page 39

Jian WANG (SILC)
Fixed effects vs. random effects
Data Visualization 39Shanghai UniversityJian WANG (UTS&SHU)

#### Page 40

Jian WANG (SILC)
Hausman Test for FE vs. RE
Data Visualization 40Shanghai UniversityJian WANG (UTS&SHU)

#### Page 41

Jian WANG (SILC)
The Hausman Test
Data Visualization 41Shanghai UniversityJian WANG (UTS&SHU)

#### Page 42

Jian WANG (SILC)
STATA: Hausman
Data Visualization 42Shanghai UniversityJian WANG (UTS&SHU)

#### Page 43

Jian WANG (SILC)
Example: Wage and Marriage
Data Visualization 43Shanghai UniversityJian WANG (UTS&SHU)

#### Page 44

Jian WANG (SILC)
Data Visualization 44Shanghai UniversityJian WANG (UTS&SHU)

#### Page 45

Jian WANG (SILC)
Data Visualization 45Shanghai UniversityJian WANG (UTS&SHU)

#### Page 46

Jian WANG (SILC)
Cross-Sectional and Pooled Regression
Data Visualization 46Shanghai UniversityJian WANG (UTS&SHU)

#### Page 47

Jian WANG (SILC)
First Difference
Data Visualization 47Shanghai UniversityJian WANG (UTS&SHU)

#### Page 48

Jian WANG (SILC)
First Difference
Data Visualization 48Shanghai UniversityJian WANG (UTS&SHU)

#### Page 49

Jian WANG (SILC)
First Difference
Data Visualization 49Shanghai UniversityJian WANG (UTS&SHU)

#### Page 50

Jian WANG (SILC)
Within Estimator (Individual Fixed Effects)
Data Visualization 50Shanghai UniversityJian WANG (UTS&SHU)

#### Page 51

Jian WANG (SILC)
Data Visualization 51Shanghai UniversityJian WANG (UTS&SHU)

#### Page 52

Jian WANG (SILC)
Fixed Effect Estimation (with individual fixed effect)
Data Visualization 52Shanghai UniversityJian WANG (UTS&SHU)

#### Page 53

Jian WANG (SILC)
LSDV estimator
Data Visualization 53Shanghai UniversityJian WANG (UTS&SHU)

#### Page 54

Jian WANG (SILC)
Random Effect
Data Visualization 54Shanghai UniversityJian WANG (UTS&SHU)

#### Page 55

Jian WANG (SILC)
Difference-in-Difference Estimation Difference-in
Data Visualization 55Shanghai UniversityJian WANG (UTS&SHU)

#### Page 56

Jian WANG (SILC)
Difference-in-Difference Estimation
Data Visualization 56Shanghai UniversityJian WANG (UTS&SHU)

#### Page 57

Jian WANG (SILC)
Fixed effect estimation (individual fixed effect + year fixed effect)
Data Visualization 57Shanghai UniversityJian WANG (UTS&SHU)
