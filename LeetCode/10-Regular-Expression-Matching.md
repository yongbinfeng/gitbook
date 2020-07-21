---
description: 'Dynamic Programming, String'
---

# 10. Regular Expression Matching

[Problem](https://leetcode.com/problems/regular-expression-matching/)

## DP Solution

Use `dp[i][j]` to represent the match result for `s[0,i)` and `p[0,j)`:
- if `s[i]==p[j]`, then `dp[i+1][j+1]=dp[i][j]`, equivalent to ignoring `s[i]` and `p[j]`;
- if `p[j]==.`, then it is the same as the above case, `dp[i+1][j+1]=dp[i][j]`;
- if `p[j]==*`, `*` could represent the preceding element matched zero time, one time and multiple times:
    - `dp[i+1][j+1]|=dp[i+1][j-1]`: for the preceding element matched zero time;
    - `dp[i+1][j+1]|=dp[i][j+1] || dp[i+1][j]`: for the preceding element matched one time or multiple times, but this condition requires `s[i]==p[j-1]` or `p[j-1]=='.'`;

Boundary conditions:
- `dp[0][0]=1`;
- `dp[0][j+1]=dp[0][j]||dp[0][j-1]` if `s[j]==*` (the preceding element matched zero or one time).

```cpp
bool isMatch(string s, string p) {
    int N1 = s.size();
    int N2 = p.size();
    
    vector<vector<bool>> dp(N1 + 1, vector<bool>(N2 + 1, 0));
    // dp[i][j]: isMatch of s[0, i) and p[0, j)
    
    dp[0][0] = 1;
    for (int j = 0; j < N2; ++j) {
        if (p[j] == '*')
            dp[0][j + 1] = dp[0][j] || dp[0][j - 1];
    }
    
    for (int i = 0; i < N1; ++i) {
        for (int j = 0; j < N2; ++j) {
            if (s[i] == p[j]) {
                dp[i + 1][j + 1] = dp[i][j];
            } else if(p[j] == '.') {
                dp[i + 1][j + 1] = dp[i][j];
            } else if(p[j] == '*') {
                dp[i + 1][j + 1] = dp[i + 1][j - 1];  // * count as zero of  the preceding element
                if (s[i] == p[j - 1] || p[j - 1] == '.') {
                    dp[i + 1][j + 1] = dp[i + 1][j + 1] || dp[i][j + 1] || dp[i + 1][j];
                }
            }
        }
    }
    
    return dp[N1][N2];
}
```
