---
description: 'Dynamic Programming, Greedy, Math'
---

# 343. Integer Break

[Problem](https://leetcode.com/problems/integer-break/)

## DP Solution

Use DP to solve this problem. Set `dp[i]` as the maximum product when `n=i`.
- Relation: For `dp[i]`, it can be cut at `[1, i-1]`: if the cut point is at `j`, then `dp[i]=dp[j]*dp[i-j]`, where `j` is in `[1, i-1]`.
- Actually, since it is symmetric, there is no difference cutting at `j` and `i-j`, so we only need to loop over the first half of `j`: `[1, i/2]`;
- Some special cases: if `n=2`, we have to cut at 1, so `dp[i]` can actually be smaller than `i`; but when `i>2`, we can choose not to cut at 1, and the max could be `i`, so every cut we have to compare `i` and `dp[i]` and take the maximum;

```cpp
int integerBreak(int n) {
    vector<int> dp(n + 1, 0);
    // dp[i]: the maximum product when n = i
    
    for (int i = 2; i <= n; ++i) {
        for (int j = 1; j <= i/2; ++j) {
            dp[i] = max(dp[i], max(j, dp[j]) * max(i - j, dp[i - j]));
        }
    }
    
    return dp[n];
}
```

Space complexity: $$O(N)$$; Time complexity: $$O(N^2)$$.

## Math (Greedy)

If we check first few cases more carefully and think. If the length of one segment is `f`, and if `f` is larger than 4, we can break it into `3` and `f-3`, the product would be `3(f-3)`, while for not breaking, the product is `f`: for `f>4`, `3(f-3)>f`. i.e.

** For `f>=5`, it is always better to break it into `3` and `f-3`**

> Then what about break it into `2` and `f-2`: `2(f-2)>3(f-3)` only when `f<5`. i.e.

** For `f<5`, it is always better to break it into `2` and `f-2` **

So, we 
- keep breaking `n` into `n-3` and `3`, until `n<5`;
- if the remaining is 4, we break it into `2*2`, which is still 4;
- if the remaining is 3, if we do the break, the product is 2, otherwise it is 3; so it is better not to break it anymore, unless we have to (i.e., when `n=3`);

```cpp
int integerBreak(int n) {
    if (n == 2)
        return 1;
    if (n == 3)
        return 2;
    
    int result = 1;
    while (n > 4) {
        n = n - 3;
        result = result * 3;
    }
    result = result * n;
    return result;
}
```

Space complexity is $$O(1)$$; Time complexity: $$O(N)$$.
