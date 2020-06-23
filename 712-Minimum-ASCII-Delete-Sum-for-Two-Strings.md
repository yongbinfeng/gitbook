---
description: 'Dynamic Programming, String, LCS'
---

# 712. Minimum ASCII Delete Sum for Two Strings

[Problem](https://leetcode.com/problems/minimum-ascii-delete-sum-for-two-strings/)

## DP Solution

Another mutant from the LCS problem. 
- $$dp[i][j]$$ is the minimum number of ASCII deletions needed to convert $$s1[0,i)$$ to $$s2[0,j)$$;
- If $$s1[i]==s2[j]$$, we don't need to delete $$s1[i]$$ or $$s2[j]$$, they have no contributions to the dp array; so $$dp[i+1][j+1]=dp[i][j]$$;
- If $$s1[i]!=s2[j]$$, we could copy the deletions from $$dp[i][j+1]$$, then delete $$s1[i]$$; or copy the deletions from $$dp[i+1][j]$$, and then delete $$s2[j]$$; so $$dp[i+1][j+1]=min(dp[i][j+1]+s1[i], dp[i+1][j]+s2[j])$$;

```cpp
int minimumDeleteSum(string s1, string s2) {
    int N1 = s1.size();
    int N2 = s2.size();
    
    vector<vector<int>> dp(N1 + 1, vector<int>(N2 + 1, 0));
    // dp[i][j]: minimum ascii delete sum to make s1[0,i) and s2[0,j) equal
    
    for (int i = 0; i < N1; ++i)
        // when j ==0, all chars in s1 needs to be deleted
        dp[i + 1][0] = dp[i][0] + s1[i]; 
    
    for (int j = 0; j < N2; ++j)
        dp[0][j + 1] = dp[0][j] + s2[j];
    
    for (int i = 0; i < N1; ++i) {
        for (int j = 0; j < N2; ++j) {
            if(s1[i] == s2[j])
                // s1[i] and s2[j] would not contribute to the deletion, both 
                // will be ignored
                dp[i + 1][j + 1] = dp[i][j];
            else
                dp[i + 1][j + 1] = min(dp[i][j + 1] + s1[i], dp[i + 1][j] + s2[j]);
        }
    }
    
    return dp[N1][N2];
}
```

## DP reduced dimension

Space dimension can be reduced to 1D.

```cpp
int minimumDeleteSum(string s1, string s2) {
    int N1 = s1.size();
    int N2 = s2.size();
    
    vector<int> dp(N2 + 1, 0);
    // dp[j]: minimum ascii delete sum to make s1 and s2[0,j) equal
    
    for (int j = 0; j < N2; ++j)
        dp[j + 1] = dp[j] + s2[j];
    
    int prev = 0;
    int curr = 0;
    
    for (int i = 0; i < N1; ++i) {
        curr = dp[0];
        dp[0] += s1[i];
        for (int j = 0; j < N2; ++j) {
            prev = curr; // dp[i][j]
            curr = dp[j + 1]; // dp[i][j + 1]
            if(s1[i] == s2[j])
                // s1[i] and s2[j] would not contribute to the deletion, both 
                // will be ignored
                dp[j + 1] = prev;
            else
                dp[j + 1] = min(dp[j + 1] + s1[i], dp[j] + s2[j]);
        }
    }
    
    return dp[N2];
}
```
