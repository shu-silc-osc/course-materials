# Course Roadmap

## 第一层：债券是什么

核心文件：

- [[Bond Features]]
- [[Bond Cash Flow Structure]]
- [[Bond Market Sectors]]

必须掌握：

- issuer
- indenture
- maturity
- par value
- coupon rate
- redemption provision
- embedded option
- floating-rate structure
- convertible feature

## 第二层：债券为什么有风险

核心文件：

- [[Risk Map of Bond Investing]]
- [[Interest Rate Risk]]
- [[Credit Risk]]
- [[Reinvestment Risk]]
- [[Call Risk]]
- [[Liquidity Risk]]

关键逻辑：

- [[Interest Rate Risk]] 解释价格为什么随利率反向变动。
- [[Reinvestment Risk]] 解释 coupon cash flow 的再投资不确定性。
- [[Call Risk]] 解释发行人为什么会在低利率环境赎回债券。
- [[Credit Risk]] 解释非国债为什么需要风险溢价。
- [[Liquidity Risk]] 解释为什么流动性越高，投资者要求的收益率越低。

## 第三层：债券市场与利差

核心文件：

- [[Treasury Benchmark Rate]]
- [[Yield Spread]]
- [[Factors Affecting Yield Spread]]
- [[Term Structure of Interest Rates]]

关键逻辑：

- Treasury 是 benchmark。
- Non-Treasury bond 的收益率通常等于 benchmark rate + spread。
- spread 反映 credit、liquidity、tax、option、maturity 等因素。
- yield curve 是 maturity 与 yield 的关系。

## 第四层：债券定价与收益率

核心文件：

- [[Traditional Bond Pricing]]
- [[Yield to Maturity]]
- [[Yield to Call]]
- [[Yield to Put]]
- [[Yield to Worst]]
- [[Total Return Horizon Yield]]

关键逻辑：

- 债券价格 = 未来现金流的现值。
- YTM 是让现金流现值等于市场价格的折现率。
- Callable bond 看 YTC/YTW。
- Putable bond 看 YTP/YTW。
- Horizon yield / total return 更接近实际持有期收益。

## 第五层：利率风险量化

核心文件：

- [[Bond Price Volatility]]
- [[Full Valuation Approach]]
- [[Duration Overview]]
- [[Modified Duration]]
- [[Convexity]]

关键逻辑：

- Full valuation 是重新估值，最准确但较繁琐。
- Duration 是一阶近似。
- Convexity 是二阶修正。
- 利率变动越大，越需要 convexity adjustment。

## 第六层：特殊工具与组合应用

核心文件：

- [[Callable Bonds]]
- [[Convertible Bond]]
- [[Securitization Process]]
- [[Eurodollar Futures]]
- [[Treasury Bond Futures]]
- [[Investment Management Process]]

关键逻辑：

- Callable bond = straight bond - call option。
- Putable bond = straight bond + put option。
- Convertible bond = straight value + equity call option。
- ABS = pooled assets + SPV + cash flow waterfall。
- Futures 用于利率风险管理和价格发现。
- Portfolio management 把收益率、久期、曲线、利差策略放到组合层面。