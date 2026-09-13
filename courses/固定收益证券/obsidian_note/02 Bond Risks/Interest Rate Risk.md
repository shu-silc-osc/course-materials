---
tags:
  - FIA/risk
  - FIA/interes-rate-risk
---
- links:
- "[[Price-Yield Relationship]]"
- "[[Bond Price Volatility]]"
- "[[Duration Overview]]"
- "[[Convexity]]"


# Interest Rate Risk

## 核心定义

Interest-rate risk 是市场利率变化导致债券价格变化的风险。固定利率债券价格与 required yield 方向相反：

```
Yield ↑ → Bond Price ↓
Yield ↓ → Bond Price ↑
```

## 影响因素

### 1. Maturity

```
Maturity 越长 → price sensitivity 越高
```

关联：

- [[Duration Overview]]
- [[Bond Price Volatility]]

### 2. Coupon Rate

```
Coupon rate 越低 → price sensitivity 越高
```

Zero-coupon bond 通常利率敏感性最高。

关联：

- [[Zero-Coupon Bond Pricing]]
- [[Macaulay Duration]]

### 3. Initial Yield Level

```
Initial yield 越高 → interest rate sensitivity 越低
```

### 4. Embedded Options

嵌入期权会改变债券的利率敏感性。

关联：

- [[Callable Bonds]]
- [[Negative Convexity]]
- [[Option-Adjusted Spread]]

## 与其他风险的关系

- 与 [[Reinvestment Risk]] 有部分 offsetting effect。
- 与 [[Yield Curve Risk]] 不同：interest-rate risk 常假设平行移动，而 yield curve risk 关注曲线形状变化。
- 与 [[Call Risk]] 相连：利率下降时 callable bond 可能被赎回。