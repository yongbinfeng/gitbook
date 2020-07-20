---
description: 'Math'
---

# 50. Pow(x, n)

[Problem](https://leetcode.com/problems/powx-n/)

## Recursive solution

Think of the few following cases:
- If `n` is even, then `pow(x, n)=pow(x*x, n/2)`;
- If `n` is odd, then `pow(x, n)=x*pow(x, n-1)`;
- If `n` is negative, then `pow(x, n)=pow(1/x, -n)`;
- If `n` is 0, then `pow(x, n)=0` assuming `x!=0`;
- Special case: if `n=INT_MIN`, `-n` will overflow, treat the case specifically.

```cpp
double myPow(double x, int n) {
    if (n == INT_MIN)
        return myPow(x * x, n / 2);
    if (n < 0)
        return myPow(1 / x, -n);
    if (n == 0)
        return 1;
    if (n % 2 == 0)
        return myPow(x * x, n / 2);
    else
        return x * myPow(x, n - 1); 
}
```

Time complexity is $$O(\log n)$$; Space complexity is also $$O(\log n)$$(?), since we have used the recursive implementation;

## Iterative solution

We can change the recursive solution to iterative solution, and the space complexity would be reduced to $$O(1)$$.

```cpp
double myPow(double x, int n) {
    if (n == INT_MIN)
        return 1 / x * myPow(x, n + 1);
    if (n < 0)
        return myPow( 1 / x, -n);
    if (n == 0)
        return 1;
    double result = 1.0;
    while (n > 0) {
        if (n % 2 == 1) {
            result = result * x;
            --n;
        }
        x = x * x;
        n = n / 2;
    }
    return result;
}
```
