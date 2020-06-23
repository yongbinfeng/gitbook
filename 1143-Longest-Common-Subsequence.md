---
description: 'Dynamic Programming, String, Subsequence, LCS'
---

# 1143. Longest Common Subsequence

[Problem](https://leetcode.com/problems/longest-common-subsequence/)

## DP Solution

Very standard longest common subsequence problem. 

- $$dp[i][j]$$ is the longest common subsequence between $$s1[0:i)$$ and $$s2[0:j)$$ 
- Relation:
  - if $$s1[i-1]==s2[j-1]$$, $$dp[i][j]=dp[i-1][j-1]+1$$;
  - Otherwise $$dp[i][j]=max(dp[i][j-1], dp[i-1][j])$$;

```cpp
int longestCommonSubsequence(string text1, string text2) {
    int n1 = text1.size();
    int n2 = text2.size();
    
    vector<vector<int>> dp(n1 + 1, vector<int>(n2 + 1, 0));
    // dp[i][j]: subsequence length between text1[0:i) and text2[0:j)
    
    for (int i = 1; i <= n1; ++i) {
        for (int j = 1; j <= n2; ++j) {
            if (text1[i - 1] == text2[j - 1])
                dp[i][j] = dp[i - 1][j - 1] + 1;
            else
                dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
        }
    }
    
    return dp[n1][n2];
}
```

## DP reduced dimension

Space complexity can be reduced to $$O(n2)$$, instead of $$O(n1*n2)$$.

```cpp
int longestCommonSubsequence(string text1, string text2) {
    int n1 = text1.size();
    int n2 = text2.size();
    
    vector<int> dp(n2 + 1, 0);
    // dp[j]: subsequence length between text1 and text2[0:j)
    
    int prev = 0;
    int curr = 0;
    
    for (int i = 1; i <= n1; ++i) {
        curr = 0; // dp[i-1][0]
        for (int j = 1; j <= n2; ++j) {
            prev = curr;
            curr = dp[j];
            if (text1[i - 1] == text2[j - 1])
                dp[j] = prev + 1;
            else
                dp[j] = max(dp[j], dp[j - 1]);
        }
    }
    
    return dp[n2];
}
```
