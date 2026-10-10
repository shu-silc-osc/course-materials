# Applied Econometrics Question Bank

## How to Use

All questions, headings, directions, and answers are in English. State the estimand and identification assumption before interpreting a coefficient.

## Regression and Causal Effects

#### Short-Answer Questions

1. Distinguish the population error $u_i$ from the OLS residual $\hat u_i$.

> Difficulty: Basic

<details>
<summary>Answer</summary>

$u_i$ is the unobserved deviation from the population model and is generally unknown. $\hat u_i=Y_i-\hat Y_i$ is computed from the sample after estimating the regression.

</details>

2. State the zero conditional mean assumption and explain its role in OLS.

> Difficulty: Basic

<details>
<summary>Answer</summary>

$E(u\mid X)=0$. It rules out systematic correlation between the included regressor and the conditional mean of omitted determinants, supporting unbiased estimation under the other sampling assumptions.

</details>

3. Why does the observed difference in mean outcomes between treated and untreated units not generally equal the ATE?

> Difficulty: Basic

<details>
<summary>Answer</summary>

The groups may differ in their untreated potential outcomes. The observed difference combines a treatment effect and selection bias.

## Experiments and Matching

</details>

4. What is the fundamental problem of causal inference?

> Difficulty: Basic

<details>
<summary>Answer</summary>

For each unit, only the potential outcome under the treatment actually received is observed; the other potential outcome is counterfactual.

</details>

5. Name two components of SUTVA and give one example of a possible violation.

> Difficulty: Basic

<details>
<summary>Answer</summary>

Consistency/well-defined treatment versions and no interference. A vaccination spillover affecting untreated neighbours violates no interference.

</details>

6. Under what conditions can matching on observed covariates identify a treatment effect?

> Difficulty: Intermediate

<details>
<summary>Answer</summary>

Conditional independence of treatment and potential outcomes given pre-treatment covariates, common support, and adequate measurement/model implementation.

</details>

7. Why can conditioning on a collider create bias?

> Difficulty: Basic

<details>
<summary>Answer</summary>

Conditioning on a common effect of two variables can open a non-causal path between them, inducing association where none existed.

## Difference-in-Differences

</details>

8. A treated group’s outcome rises by 5 units and a comparison group’s outcome rises by 2 units. Compute the two-by-two DiD estimate and state the identifying assumption.

> Difficulty: Intermediate

<details>
<summary>Answer</summary>

$5-2=3$ units. The key assumption is parallel trends absent treatment, with no differential anticipation or confounding shock that invalidates the comparison.

</details>

9. Why are standard errors often clustered in DiD?

> Difficulty: Basic

<details>
<summary>Answer</summary>

Treatment is commonly assigned at a group or policy level, and outcomes within a group may have correlated errors over time. Clustering at the assignment level reflects this dependence.

</details>

10. What does a non-significant pre-treatment event-study coefficient prove?

> Difficulty: Basic

<details>
<summary>Answer</summary>

It does not prove parallel trends. It indicates that the data do not reject a zero pre-treatment difference at the chosen precision; power and design diagnostics still matter.

## Instrumental Variables

</details>

11. State the three core IV conditions.

> Difficulty: Basic

<details>
<summary>Answer</summary>

Relevance, independence/exogeneity, and exclusion restriction. Monotonicity is additionally needed for the standard complier-LATE interpretation.

</details>

12. A policy eligibility instrument strongly predicts programme participation but also directly changes local funding for outcomes. Which condition is threatened?

> Difficulty: Intermediate

<details>
<summary>Answer</summary>

The exclusion restriction, because eligibility may affect the outcome through funding other than programme participation.

</details>

13. In a just-identified one-instrument model, the reduced-form effect is 0.12 and the first-stage effect is 0.40. Compute the Wald estimate.

> Difficulty: Intermediate

<details>
<summary>Answer</summary>

$0.12/0.40=0.30$ outcome units per induced treatment change, under the IV assumptions.

## RDD and Synthetic Control

</details>

14. In a fuzzy RDD, the outcome jumps by 4 and treatment probability jumps by 0.5 at the cutoff. What is the local effect estimate?

> Difficulty: Intermediate

<details>
<summary>Answer</summary>

$4/0.5=8$ outcome units per treatment, subject to the RDD continuity and no-manipulation conditions.

</details>

15. Why is bandwidth choice important in RDD?

> Difficulty: Basic

<details>
<summary>Answer</summary>

A wider bandwidth uses more observations but relies on a less local approximation and may increase bias; a narrower bandwidth is more local but has higher variance.

</details>

16. What does a good pre-treatment fit mean in synthetic control, and what does it not establish?

> Difficulty: Basic

<details>
<summary>Answer</summary>

It shows the weighted donor combination reproduces pre-treatment outcomes/predictors well. It does not establish that the donor pool is uncontaminated or that no post-treatment confounder exists.

## Comprehensive Design Questions

</details>

17. A scholarship is awarded to students above a test-score cutoff. Which quasi-experimental design is natural, and what are two key diagnostics?

> Difficulty: Intermediate

<details>
<summary>Answer</summary>

RDD. Check for precise score manipulation and continuity of pre-treatment covariates/outcomes around the cutoff; assess bandwidth and functional-form sensitivity.

</details>

18. A training programme is voluntary. Researchers observe rich pre-treatment covariates but cannot randomise participation. Which methods could be considered, and what limitation remains?

> Difficulty: Basic

<details>
<summary>Answer</summary>

Regression adjustment, stratification, matching, or propensity-score methods. They require conditional ignorability and common support and cannot remove unobserved confounding.

</details>

19. A single province adopts a policy and several untreated provinces are available for a long pre-policy period. Which method may be useful, and what should be reported?

> Difficulty: Intermediate

<details>
<summary>Answer</summary>

Synthetic control. Report donor pool, predictor and weight choices, pre-treatment fit, post-treatment gap, placebo tests, and possible spillovers.

</details>

20. Explain why “the coefficient is statistically significant” is insufficient for a causal conclusion.

> Difficulty: Intermediate

<details>
<summary>Answer</summary>

Statistical significance does not establish identification. The researcher must justify the design, assumptions, estimand, standard errors, measurement, and threats such as confounding or selection.

</details>
