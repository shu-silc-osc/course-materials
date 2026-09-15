# Chapter 11: Regression Analysis

#### Multiple-Choice Questions

1. **[Key Question]** A multiple regression produces the following ANOVA results.

| Source | SS | df | MS | F | Significance F |
|---|---:|---:|---:|---:|---:|
| Regression | 3,340.7 | 4 | 849.4 | 3.65 | 0.029 |
| Residual | 3,493.1 | 15 | 232.9 | | |
| Total | 6,890.8 | 19 | | | |

Which conclusion follows?

A) At $\alpha=0.01$, at least one regression coefficient differs from zero.

B) At $\alpha=0.001$, at least one regression coefficient differs from zero.

C) At $\alpha=0.05$, at least one regression coefficient differs from zero.

D) None of the regression coefficients differs from zero.

> Difficulty: Intermediate

<details>
<summary>Answer</summary>

C

</details>

2. **[Key Question]** A manager predicts annual car cost $y$ from kilometres driven $x$ using $\hat y=1,550+0.36x$. What is the predicted annual cost when a car is driven 20,000 km?

A) USD 7,200

B) USD 8,750

C) USD 5,650

D) USD 2,270

> Difficulty: Intermediate

<details>
<summary>Answer</summary>

B

</details>

#### Calculation and Application Questions

1. **[Key Question]** A moving-company owner models labour hours required for a job using the volume of goods moved, measured in cubic feet. The fitted model is $\hat y=-2.3697+0.0501x$, and the volume coefficient has a p-value below 0.05.

(a) Identify the dependent and independent variables.

(b) Is volume significant at $\alpha=0.05$?

(c) Interpret the slope.

(d) Predict labour hours for 100 cubic feet.

> Difficulty: Advanced

<details>
<summary>Answer</summary>

(a) Dependent variable: labour hours required. Independent variable: volume of goods moved in cubic feet.

(b) Yes. Its p-value is below 0.05.

(c) Each additional cubic foot is associated with an average increase of 0.0501 labour hours.

(d) $\hat y=-2.3697+0.0501(100)=2.6403$ hours.

</details>

#### Comprehensive Questions

1. **[Key Question]** A regression predicting $y$ from $x_1$ through $x_5$ gives the following output.

| Statistic | Value |
|---|---:|
| $R^2$ | 0.9409 |
| Adjusted $R^2$ | 0.9224 |
| Overall F | 50.9118 |
| Significance F | $2.92\times10^{-9}$ |

| Term | Coefficient | p-value |
|---|---:|---:|
| Intercept | -121.2460 | 0.00002992 |
| $x_1$ | 0.7142 | 0.1447 |
| $x_2$ | -0.0287 | 0.8997 |
| $x_3$ | 0.7308 | 0.000092356 |
| $x_4$ | 0.3919 | 0.00496 |
| $x_5$ | 0.8416 | 0.0208 |

(a) Test the overall model at $\alpha=0.05$.

(b) Identify significant predictors at $\alpha=0.05$.

(c) Interpret the coefficient of determination.

> Difficulty: Advanced

<details>
<summary>Answer</summary>

(a) $H_0:\beta_1=\beta_2=\beta_3=\beta_4=\beta_5=0$; $H_a$: at least one slope is nonzero. Since Significance F is below 0.05, reject $H_0$.

(b) $x_3$, $x_4$, and $x_5$ are significant because their p-values are below 0.05.

(c) $R^2=0.9409$: 94.09% of the sample variation in $y$ is explained by the five predictors. Adjusted $R^2=0.9224$ after accounting for model size.

</details>

This section contains 4 questions.
