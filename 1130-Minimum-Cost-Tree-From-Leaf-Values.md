---
description: 'Dynamic Programming, Merge Interval, Greedy, Stack'
---

# 1130. Minimum Cost Tree From Leaf Values

[Problem](https://leetcode.com/problems/minimum-cost-tree-from-leaf-values/)

## DP Solution

How the tree is formed in `i` depends on both sides `i-1` and `i+1`. A good example for DP with intervals:
$$dp[i][j]=dp[i][k]+dp[k][j]+...$$:

- $$dp[i][j]$$ is defined as the smallest possible sum of non-leaf nodes, formed using $$arr[i,j]$$;
- $$dp[i][j]=min(dp[i][k]+dp[k+1][j]+min(arr[i:k]*arr[k+1:j]))$$;
- boundary conditions: $$dp[i][i]=0$$; final result is $$dp[0][N-1]$$;

```cpp
int mctFromLeafValues(vector<int>& arr) {
    int N = arr.size();
    
    vector<vector<int>> dp(N, vector<int>(N, INT_MAX));
    // dp[i][j]: smallest possible sum of all non-leaf nodes for arr[i:j]
    
    vector<vector<int>> maxArr(N, vector<int>(N,0));
    // maxArr[i][j]: maximum arr in arr[i:j];
    
    for(int i=0; i<N; i++){
        dp[i][i]=0;
        maxArr[i][i]=arr[i];
    }
    
    for(int l=1; l<N; l++){
        for(int i=0; i<N-l; i++){
            int j= i+l;
            for(int k=i; k<j; k++){
                maxArr[i][j]=max(maxArr[i][k], maxArr[k+1][j]);
                dp[i][j]=min(dp[i][j],dp[i][k]+dp[k+1][j]+maxArr[i][k]*maxArr[k+1][j]);
            }
        }
    }
    
    return dp[0][N-1];
}
```

Time complexity is $$O(N^{3})$$. Space complexity is $$O(N^2)$$.

## Notes
- This is a standard DP merge interval problem. Need to be familiar with the basic ideas and approach. 
- There are greedy approaches that could optmize the time complexity to $$O(N^2)$$, even $$O(N)$$. Need to understand them.
