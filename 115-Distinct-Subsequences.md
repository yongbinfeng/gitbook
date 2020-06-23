---
description: 'Dynamic Programming, String, Subsequence'
---

# 115. Distinct Subsequences

[Problem](https://leetcode.com/problems/distinct-subsequences/)

## DP Solution

This is similar to the LCS problem.
- $$dp[i][j]$$ is the number of distinct subsequences of $$s[0,i)$$ that is same as $$t[0,j)$$;
- if $$s[i]==t[j]$$, then we could include $$s[i]$$ in the subsequence, or not including it; if included $$s[i]$$, the number of distinct subsequences are the same as $$dp[i-1][j-1]$$; if not including $$s[i]$$, the number of distinct subsequences is $$dp[i-1][j]$$; so in this case $$dp[i][j]=dp[i-1][j]+dp[i-1][j-1]$$;
- if $$s[i]!=t[j]$$, then $$dp[i][j]=dp[i-1][j]$$, i.e., adding $$s[i]$$ does not make any difference to the number of distinct subsequences as $$dp[i-1][j]$$;
- Boundary conditions are when $$j=0$$, which is empty, there is always one subsequence;

```cpp
int numDistinct(string s, string t) {
    int N1 = s.size();
    int N2 = t.size();
    
    vector<vector<long>> dp(N1 + 1, vector<long>(N2 + 1, 0));
    // dp[i][j]: number of distint subsequences of s[0,i) which equals t[0,j)
    
    for (int i = 0; i <= N1; ++i)
        // when j = 0, there is always one empty subsequence of s 
        dp[i][0] = 1;
    
    for (int i = 0; i < N1; ++i) {
        for (int j = 0; j < N2; ++j) {
            if (s[i] == t[j])
                // in this case we could include s[i] or not include s[i]
                // in the subsequence
                dp[i + 1][j + 1] = dp[i][j + 1] + dp[i][j];
            else
                // include s[i] is the same as not including it;
                dp[i + 1][j + 1] = dp[i][j + 1];
        }
    }
    
    return dp[N1][N2];
}
```

## DP reduced dimension

Since $$dp[i][j]$$ only depends on the previous line, we can reduce the space complexity to $$O(N2)$$;

```cpp
int numDistinct(string s, string t) {
    int N1 = s.size();
    int N2 = t.size();
    
    vector<long> dp(N2 + 1, 0);
    // dp[j]: number of distint subsequences of s which equals t[0,j)
    
    dp[0] = 1; // when t is empty, there is always one subsequence
    
    for (int i = 0; i < N1; ++i) {
        for (int j = N2 - 1; j >= 0; --j) {
            if (s[i] == t[j])
                // in this case we could include s[i] or not include s[i]
                // in the subsequence
                dp[j + 1] += dp[j];
        }
    }
    
    return dp[N2];
}
```
