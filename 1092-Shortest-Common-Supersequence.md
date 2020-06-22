---
description: 'Dynamic Programming, String, LCS'
---

# 1092. Shortest Common Supersequence

[Problem](https://leetcode.com/problems/shortest-common-supersequence/) 

## DP Solution

This is a problem modified from [Problem 1143. Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/).

We first using DP to find the length of the longest common subsequence. By inverting the loop over $$i$$ and $$j$$, we could find the longest common subsequence string, by checking the relations of $$dp[i+1][j+1]$$, $$dp[i+1][j]$$, $$dp[i][j+1]$$ and $$dp[i][j]$$:

- $$dp[i][j]$$ is the length of the longest common subsequence between $$s1[0:i)$$ and $$s2[0:j)$$
- if $$dp[i+1][j+1]==dp[i+1][j]$$, it means $$s1[i]$$ is NOT part of the longest common subsequence;
- else if $$dp[i+1][j+1]==dp[i][j+1]$$, it means $$s2[j]$$ is NOT part of the longest common subsequence;
- else if $$dp[i+1][j+1]==dp[i][j]+1$$, it means $$s1[i]==s2[j]$$, and it is part of the longest common subsequnce;

The procedure of looking for the shortest common supersequence, is basically the same as the above looking for the LCS, except inserting the char into the string if it is NOT part of the longest common subsequece; 

Here is a very nice video explaining the process: [video](https://youtu.be/sSno9rV8Rhg?t=1328)

```cpp
string shortestCommonSupersequence(string str1, string str2) {
    int N1 = str1.size();
    int N2 = str2.size();
    
    vector<vector<int>> dp(N1 + 1, vector<int>(N2 + 1, 0));
    // dp[i][j]: length of LCS with str1[0:N1) and str2[0:N2)
    
    for (int i = 0; i < N1; ++i) {
        for (int j = 0; j < N2; ++j) {
            if (str1[i] == str2[j])
                dp[i + 1][j + 1] = dp[i][j] + 1;
            else
                dp[i + 1][j + 1] = max(dp[i][j + 1], dp[i + 1][j]);
        }
    }
    
    // recover Shortest common supersequence from LCS
    int i = N1;
    int j = N2;
    string s;
    while(i > 0 && j > 0){
        if(dp[i][j] == dp[i - 1][j]){
            s += str1[i - 1];
            --i;
        }
        else if(dp[i][j] == dp[i][j - 1]){
            s += str2[j - 1];
            --j;
        }
        else{
            s += str1[i - 1];
            --i;
            --j;
        }
    }
    while(i > 0){
        s += str1[i - 1];
        --i;
    }
    while(j > 0){
        s += str2[j - 1];
        --j;
    }
    
    reverse(s.begin(), s.end());
    
    return s;
    
}
```
