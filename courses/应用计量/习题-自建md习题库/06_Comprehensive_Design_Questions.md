# Comprehensive Design Questions

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


This section contains 4 questions.
