---
description: 给定a,b;b>a
---

# 最小公倍数和最大公因数

### 辗转相除法求最大公因数

**一个数学事实**

**GCD(a, b) = GCD(b, a mod b)**

```
long gcd(long a, long b) {
    return b == 0 ? a : gcd(b, a % b);
}

```

### 最小公倍数

```
long lcm(long a, long b) {
    return a * 1L * (b / gcd(a, b));
}
```
