---
description: 'Dynamic Programming, String, LCS'
---

# 72. Edit Distance

[Problem](https://leetcode.com/problems/edit-distance/)

## DP Solution

This looks very hard, but after careful thinking it is very similar to LCS problem, or the problem related to the 2D Grids. 

- Following the tradition, define $$dp[i][j]$$ as the minimum number of operations to convert $$s1[0,i)$$ to $$s2[0,j)$$;
- If $$s1[i]==s2[j]$$, then $$dp[i+1][j+1]=dp[i][j]$$, no operation need to be taken;
- If $$s1[i]!=s2[j]$$, then we need to compare: 
  - For the insert operation, we are converting $$dp[i+1][j+1]$$ to $$dp[i+1][j]+1$$, since we are going to insert $$s2[j]$$ after $$s1[0,i]$$;
  - For the delete operation, we are converting $$dp[i+1][j+1]$$ to $$dp[i][j+1]+1$$, since $$s1[i]$$ will be deleted;
  - For the replace operation, we are converting $$dp[i+1][j+1]$$ to $$dp[i][j]+1$$, since $$s1[i]$$ will be replaced to $$s2[j]$$, and we do not need to care about $$s1[i]$$ or $$s2[j]$$ anymore;
  - Now we just need to care about which one is smaller among these three operations, in order to achieve the minimum operations goal. $$dp[i+1][j+1]=min(dp[i][j+1], dp[i][j], dp[i+1][j])+1$$;
- Boundary conditions are when $$i=0$$ or $$j=0$$, we need to take $$j$$ or $$i$$ operations to convert $$s1$$ into $$s2$$: $$dp[0][j]=j$$, $$dp[i][0]=i$$

```cpp
int minDistance(string word1, string word2) {
    int N1 = word1.size();
    int N2 = word2.size();
    
    vector<vector<int>> dp(N1 + 1, vector<int>(N2 + 1, 0));
    // dp[i][j]: minimum number of operations to convert word1[0,i) 
    // to word2[0,j)
    
    //Boundary conditions
    for (int i = 0; i < N1; ++i) 
        dp[i + 1][0] = i + 1;
    
    for (int j = 0; j < N2; ++j)
        dp[0][j + 1] = j + 1;
    
    for (int i = 0; i < N1; ++i) {
        for (int j = 0; j < N2; ++j) {
            if (word1[i] == word2[j])
                dp[i + 1][j + 1] = dp[i][j];
            else{
                dp[i + 1][j + 1] = min(dp[i + 1][j], dp[i][j + 1]) + 1;
                dp[i + 1][j + 1] = min(dp[i + 1][j + 1], dp[i][j] + 1);
            }
        }
    }
    
    return dp[N1][N2];
}
```

## DP reduced dimension

Space complexity can be reduced to $$O(n2)$$:

```cpp
int minDistance(string word1, string word2) {
    int N1 = word1.size();
    int N2 = word2.size();
    
    vector<int> dp(N2 + 1, 0);
    // dp[j]: minimum number of operations to convert word1
    // to word2[0,j)
    
    //Boundary conditions
    for (int j = 0; j < N2; ++j)
        dp[j + 1] = j + 1;
    
    int prev = 0;
    int curr = 0;
    
    for (int i = 0; i < N1; ++i) {
        dp[0] = i + 1; // dp[i+1][0]
        for (int j = 0; j < N2; ++j) {
            prev = curr; // dp[i][j]
            curr = dp[j + 1]; // dp[i][j + 1]
            if (word1[i] == word2[j])
                dp[j + 1] = prev;
            else{
                dp[j + 1] = min(dp[j], dp[j + 1]) + 1;
                dp[j + 1] = min(dp[j + 1], prev + 1);
            }
        }
        curr = i + 1; // dp[i + 1][0]
    }
    
    return dp[N2];
}
```

## Notes
- Do not forget the boundary conditions: different from the LCS case, where the boundary is 0 (no LCS if one of the strings is empty);
- In the reduced dimension case, remember to update $$dp[0]$$ at the beginning of each $$j$$ loop, same as the LCS case;
