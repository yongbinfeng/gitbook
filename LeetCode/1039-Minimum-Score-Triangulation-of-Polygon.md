---
description: 'Dynamic Programming, Merge Interval'
---

# 1039. Minimum Score Triangulation of Polygon

[Problem](https://leetcode.com/problems/minimum-score-triangulation-of-polygon/)

## DP Solution

For one N-sided polygon, $$N-2$$ lines need to be connected to make $$N-2$$ triangles.
After one-line cut, one polygon will be broken into 2 polygons. Cutting at $$(0,k)$$, $$(0,k+1)$$,
we will have two polygons plus one triangle. And this is the relation we are interested in.

- $$dp[i][j]$$ is the minimum score for $$A[i:j]$$;
- $$dp[i][j]=min(dp[i][j], dp[i][k]+dp[k][j]+A[i]*A[k]*A[j])$$;
- boundary conditions: $$dp[i][i]=0$$, $$dp[i][i+1]=0$$

```cpp
int minScoreTriangulation(vector<int>& A) {
    int N = A.size();
    vector<vector<int>> dp(N, vector<int>(N, 0));
    // dp[i][j]: smallest score for A[i:j]
    
    for(int l=2; l<N; l++){
        for(int i=0; i<N-l; i++){
            int j=i+l;
            dp[i][j]=INT_MAX;
            for(int k=i+1; k<j; k++){
                dp[i][j]=min(dp[i][j], dp[i][k]+dp[k][j]+A[i]*A[k]*A[j]);
            }
        }
    }
    
    return dp[0][N-1];
}
```
- 
