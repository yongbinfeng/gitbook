---
description: 'Dynamic Programming, DW, Grid'
---

# 1269. Number of Ways to Stay in the Same Place After Some Steps

[Problem](https://leetcode.com/problems/number-of-ways-to-stay-in-the-same-place-after-some-steps/)

## DP Solution

Not like a hard problem. More close to a medium problem to me. Standard DP problem in 1D grid.
Slightly tricky part is to find that since it can only move steps, the arrays that are further than steps
can not be reached at all. (Even more, since it needs to return to 0, the maximum it could arrive is 
actually $$\frac{steps}{2}$$.)

- $$dp[i][j]$$ is the number of ways it could return to 0, with i steps from index j
- $$dp[i][j]=dp[i-1][j]+dp[i-1][j+1]+dp[i-1][j-1]$$.
- Boundary conditions are: with i steps, there is only one way to return from index j to 0, i.e.
  - $$dp[i][i]=1$$

```cpp
int numWays(int steps, int arrLen) {
    int MOD = (int)1e9+7;
    
    int maxL = min(steps, arrLen); // furthest it could reach
    
    vector<vector<unsigned>> dp(steps+1, vector<unsigned>(maxL, 0));
    // dp[i][j]: number of ways to return to index 0 from index j with i steps;
    
    for(int step=0; step<=steps; step++){
        if(step<maxL)
            dp[step][step]=1;
    }
    
    for(int step=1; step<=steps; step++){
        for(int i=0; i<maxL; i++){
            dp[step][i]=dp[step-1][i];
            if(i<maxL-1)
                dp[step][i]+=dp[step-1][i+1];
            if(i>0)
                dp[step][i]+=dp[step-1][i-1];
            dp[step][i]=dp[step][i]%MOD;
        }
    }
    
    return dp[steps][0];
}
```

## DP reduced dimension

space complexity can be reduced to $$O(min(steps, arrLen))$$, since the current step only
depends on the status of the previous one.

```cpp
int numWays(int steps, int arrLen) {
    int MOD = (int)1e9+7;
    
    int maxL = min(steps, arrLen); // furthest it could reach
    
    vector<unsigned> dp(maxL, 0);
    // dp[j]: number of ways to return to index 0 from index j;
    vector<unsigned> dpold(maxL, 0);
            
    for(int step=1; step<=steps; step++){
        
        dpold = dp;
        if(step<maxL)
            // the previous step
            dpold[step-1]=1;
        
        for(int i=0; i<maxL; i++){
            dp[i]=dpold[i];
            if(i<maxL-1)
                dp[i]+=dpold[i+1];
            if(i>0)
                dp[i]+=dpold[i-1];
            dp[i]=dp[i]%MOD;
        }
    }
    
    return dp[0];
}
```

The coding style might be able to get optimized. My brain is not very clear when writing this page...
