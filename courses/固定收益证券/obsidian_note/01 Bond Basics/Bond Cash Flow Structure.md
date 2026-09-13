---
tags:
  - FIA/bond-basics
  - FIA/core
aliases:
  - 债券特征
  - Features of Debt Securities
  - Bond Characteristics
---
links:
- "[[Bond Cash Flow Structure]]"
- "[[Embedded Options Overview]]"
- "[[Floating Rate Securities]]"
- "[[Risk Map of Bond Investing]]"

# Bond Features

## 核心定义

Bond 是发行人向投资者借款形成的债务证券。投资者购买债券，获得未来现金流；发行人承担按约定支付利息和本金的义务。

## 主要特征

### 1. Type of Issuer

发行人类别决定债券风险属性：

- central government
- agency / semi-government
- municipal government
- corporation
- structured finance issuer

关联：

- [[Bond Market Sectors]]
- [[Credit Risk]]
- [[Sovereign Bonds]]

### 2. Bond Indenture

Bond indenture 是发行人与债券持有人之间的法律契约，规定：

- coupon payment
- maturity
- redemption provision
- covenants
- embedded options

关联：

- [[Embedded Options Overview]]
- [[Callable Bonds]]
- [[Putable Bonds]]

### 3. Term to Maturity

到期日决定本金偿还时间。一般而言：

- maturity 越长，[[Interest Rate Risk]] 越高
- maturity 越长，[[Bond Price Volatility]] 越大

关联：

- [[Maturity Spread]]
- [[Duration Overview]]

### 4. Par Value

Par value 也叫：

- face value
- principal value
- maturity value
- redemption value

债券报价通常按 par 的百分比报价：

```
Quoted Price = 100 表示 100% of par
Quoted Price < 100 表示 discount bond
Quoted Price > 100 表示 premium bond
```

关联：

- [[Traditional Bond Pricing]]
- [[Price-Time Relationship]]

### 5. Coupon Rate

Coupon rate 是发行人承诺每年支付的名义利率。

```
Annual Coupon = Coupon Rate × Par Value
Semiannual Coupon = Annual Coupon / 2
```

关联：

- [[Bond Cash Flow Structure]]
- [[Current Yield]]
- [[Yield to Maturity]]

### 6. Provisions for Paying off Bonds

偿还结构包括：

- bullet maturity
- amortizing securities
- sinking fund
- call provision
- put provision

关联：

- [[Mortgage Loans and MBS]]
- [[Callable Bonds]]
- [[Putable Bonds]]

### 7. Embedded Options

债券可包含嵌入期权：

- call option：对 issuer 有利
- put option：对 investor 有利
- conversion option：对 investor 有利

关联：

- [[Embedded Options Overview]]
- [[Option-Adjusted Spread]]
- [[Convertible Bond]]

## 考试提醒

判断 premium / discount 的核心：

```
coupon rate > market yield → premium
coupon rate = market yield → par
coupon rate < market yield → discount
```