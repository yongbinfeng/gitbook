---
description: 'Dynamic Programming, String, Subsequence, Interval'
---

# 516. Longest Palindromic Subsequence

[Problem](https://leetcode.com/problems/longest-palindromic-subsequence/)

## DP Solution

String subsequence problem. Kind of similar to [Problem 1143 LCS](https://leetcode.com/problems/longest-common-subsequence/).
Except in that case there are two strings. Here we use intervals and make it 2D.
- $$dp[i][j]$$ is the longest palindromic subsequence of $$s[i:j]$$;
- $$dp[i][j]=dp[i+1][j-1]+2$$ if $$s[i]==s[j]$$ and $$j-i\geq 1$$;
- Boundary conditions $$dp[i][i]=1$$;

```cpp
int longestPalindromeSubseq(string s) {
    int N = s.size();
    
    vector<vector<int>> dp(N, vector<int>(N, 0));
    // dp[i][j]: longest palindromic subsequence in s[i,j]
    
    for (int i = 0; i < N; ++i)
        dp[i][i] = 1;
    
    for (int l = 1; l < N; ++l) {
        for (int i = 0; i < N - l; ++i) {
            int j = i + l;
            if (s[i] == s[j])
                dp[i][j] = dp[i+1][j-1] + 2;
            else
                dp[i][j] = max(dp[i + 1][j], dp[i][j - 1]);
        }
    }
    
    return dp[0][N-1];
}
```

Time and space complexity are both $$O(N^2)$$
