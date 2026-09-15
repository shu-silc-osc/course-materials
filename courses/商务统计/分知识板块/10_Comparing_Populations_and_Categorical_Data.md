# Chapter 10: Comparing Populations and Categorical Data

#### Multiple-Choice Questions

1. **[Key Question]** A manager studies whether greeting-card sales differ across soft, medium, and bright lighting, assigning six pharmacies to each level. The null hypothesis is:

A) $\mu_1=\mu_2=\mu_3$

B) $\mu_1\ne\mu_2\ne\mu_3$

C) $\mu_1\ge\mu_2\ge\mu_3$

D) $\mu_1\le\mu_2\le\mu_3$

> Difficulty: Basic

<details>
<summary>Answer</summary>

A

</details>

#### Calculation and Application Questions

1. **[Key Question]** Company A employees submit a mean of 5.8 suggestions per month and Company B employees submit 5.0. Independent random samples have $n_A=36$, $n_B=45$, and known population standard deviations $\sigma_A=1.7$, $\sigma_B=1.4$. Test for a difference in population means at $\alpha=0.05$.

> Difficulty: Advanced

<details>
<summary>Answer</summary>

$H_0:\mu_A-\mu_B=0$ and $H_a:\mu_A-\mu_B\ne0$. Use a two-sample z-test.

$z=(5.8-5.0)/\sqrt{1.7^2/36+1.4^2/45}=2.27$. The critical values are $\pm1.96$. Reject $H_0$; there is sufficient evidence of a difference in mean monthly suggestions.

</details>

2. **[Key Question]** Eight Melbourne car-rental companies have mean daily rate USD 47 and standard deviation USD 3. Nine Sydney companies have mean USD 44 and standard deviation USD 3. Assuming normal populations with equal variances, test at $\alpha=0.01$ whether Melbourne's mean rate is higher.

> Difficulty: Advanced

<details>
<summary>Answer</summary>

$H_0:\mu_M-\mu_S\le0$ and $H_a:\mu_M-\mu_S>0$. Use a pooled two-sample t-test with 15 degrees of freedom.

The test statistic is $t=2.06$ and the critical value is $t_{0.01,15}=2.602$. Do not reject $H_0$; there is insufficient evidence at the 1% level that Melbourne's mean daily rate is higher.

</details>

3. **[Key Question]** For 27 clients, the mean paired difference in audit delay, defined as after merger minus before merger, is -3.71 days with standard deviation 5 days. Assuming normally distributed differences, test at $\alpha=0.01$ whether audit delay decreased after merger.

> Difficulty: Advanced

<details>
<summary>Answer</summary>

$H_0:\mu_D\ge0$ and $H_a:\mu_D<0$. Use a paired t-test with 26 degrees of freedom.

$t=(-3.71-0)/(5/\sqrt{27})=-3.8555$. The critical value is -2.479. Reject $H_0$; there is evidence that audit delays decreased after merger.

</details>

#### Comprehensive Questions

1. **[Key Question]** A survey of 1,000 iPad users records the task performed most often.

| User type | Web surfing | Social networking | Media consumption | Total |
|---|---:|---:|---:|---:|
| Business people | 170 | 80 | 110 | 360 |
| Students | 60 | 170 | 70 | 300 |
| General public | 80 | 100 | 160 | 340 |
| Total | 310 | 350 | 340 | 1,000 |

At $\alpha=0.05$, test whether the most-performed task depends on user type.

> Difficulty: Advanced

<details>
<summary>Answer</summary>

$H_0$: task and user type are independent. $H_a$: they are not independent.

The expected-frequency table is:

| User type | Web surfing | Social networking | Media consumption |
|---|---:|---:|---:|
| Business people | 111.6 | 126.0 | 122.4 |
| Students | 93.0 | 105.0 | 102.0 |
| General public | 105.4 | 119.0 | 115.6 |

$\chi^2=136.8055$ with $(3-1)(3-1)=4$ degrees of freedom. The critical value is 9.4877. Reject $H_0$; task and user type are associated.

</details>

This section contains 5 questions.
