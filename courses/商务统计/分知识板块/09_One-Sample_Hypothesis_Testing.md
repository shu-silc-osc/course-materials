# Chapter 9: One-Sample Hypothesis Testing

#### Multiple-Choice Questions

1. **[Key Question]** Consider $H_0:\mu\ge67$ and $H_a:\mu<67$. These hypotheses:

A) define a one-tailed test with rejection in the right tail

B) define a one-tailed test with rejection in the left tail

C) define a two-tailed test

D) are established incorrectly

> Difficulty: Basic

<details>
<summary>Answer</summary>

B

</details>

#### Calculation and Application Questions

1. **[Key Question]** A life-insurance salesperson claims that the average Perth worker has no more than USD 25,000 of personal life insurance. A random sample of 100 workers has mean USD 26,650; the population standard deviation is USD 12,000.

(a) Test the claim at $\alpha=0.05$.

(b) Find the p-value and state the conclusion.

> Difficulty: Advanced

<details>
<summary>Answer</summary>

$H_0:\mu\le25,000$ and $H_a:\mu>25,000$. Use a right-tailed z-test.

$z=(26,650-25,000)/(12,000/\sqrt{100})=1.38$. The critical value is 1.645, so do not reject $H_0$. The sample does not provide evidence that the claim is invalid.

The p-value is 0.0838. Since $0.0838>0.05$, the p-value decision is also not to reject $H_0$.

</details>

2. **[Key Question]** Industrial emissions should not exceed 2.5 parts per million. Nine normally distributed air tests have mean 3.4 and sample standard deviation 0.6. At $\alpha=0.01$, is there sufficient evidence that the company exceeds the safe limit?

> Difficulty: Advanced

<details>
<summary>Answer</summary>

$H_0:\mu\le2.5$ and $H_a:\mu>2.5$. Use a right-tailed t-test because $\sigma$ is unknown.

With 8 degrees of freedom, $t_{0.01,8}=2.896$. The test statistic is $t=(3.4-2.5)/(0.6/\sqrt9)=4.5$. Reject $H_0$; there is sufficient evidence that mean emissions exceed the safe limit.

</details>

3. **[Key Question]** A prior study found that 79% of companies offer flexible scheduling. A researcher believes the proportion is lower among accounting companies. In a random sample of 415 accounting companies, 303 offer flexible scheduling.

(a) Test the researcher's belief at $\alpha=0.01$.

(b) Determine the p-value and state the conclusion.

> Difficulty: Advanced

<details>
<summary>Answer</summary>

$H_0:p\ge0.79$ and $H_a:p<0.79$. Here $\hat p=303/415=0.7301$.

$z=(0.7301-0.79)/\sqrt{0.79(0.21)/415}=-3.00$. The critical value is -2.326, so reject $H_0$.

The p-value is 0.0013. Since $0.0013<0.01$, there is sufficient evidence that the accounting-company proportion is lower than 79%.

</details>

This section contains 4 questions.
