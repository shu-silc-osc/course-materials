---
tags:
  - FIA/bond-basics
  - FIA/floating-rate
aliases:
  - Floater
  - Floating Rate Bond
---
links:
- "[[Inverse Floaters]]"
- "[[Discount Margin]]"
- "[[Spread for Life]]"
- "[[Interest Rate Risk]]"


# Floating Rate Securities

## 核心定义

Floating-rate securities 的 coupon rate 会随 reference rate 重设。

```
Coupon Rate = Reference Rate + Quoted Margin
```

常见 reference rate：

- LIBOR
- Treasury bill rate
- SOFR 类参考利率

## 特征

- coupon reset 使其价格对利率变化不如 fixed-rate bond 敏感。
- reset date 越远，价格波动越大。
- quoted margin 变化也会影响价格。
- cap 对投资者不利，因为限制 coupon 上升空间。
- floor 对投资者有利，因为限制 coupon 下行空间。

## 与风险的关系

连接：

- [[Interest Rate Risk]]
- [[Discount Margin]]
- [[Spread for Life]]
- [[Floater Pricing]]

## 高频考法

如果题目给：

```
Reference Rate = 6.5%
Quoted Margin = 125 bps
Coupon paid semiannually
```

则：

```
Annual Coupon Rate = 6.5% + 1.25% = 7.75%
Semiannual Coupon Rate = 7.75% / 2 = 3.875%
```

---