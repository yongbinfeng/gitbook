---
description: 'Dynamic Programming, Knapsack, DW'
---

# 1155 Number of Dice Rolls With Target Sum

[Problem](https://leetcode.com/problems/number-of-dice-rolls-with-target-sum/)

#### DP Solution:

This is similar to [Problem 518 Coin change 2](https://leetcode.com/problems/coin-change-2/):

- 2D DP array where $$dp[i][j]$$ represents the number of different ways using i dices with sum equal to j
- Relation: $$dp[i][j]+=dp[i-1][j-k]$$ where $$k$$ is the number from the (i-1)-th dice. (Dice indices from 0 to d-1.)
- Boundary: $$dp[0][0]=1$$, others all start with 0.

```cpp
int numRollsToTarget(int d, int f, int target) {
    vector<vector<int>> dp(d+1, vector<int>(target+1, 0));
    // dp[i][j]: number of possible ways using [0,i) dices with the sum equals to j
    dp[0][0]=1;
    for(int i=1; i<=d; i++){
        for(int j=1; j<=target; j++){
            for(int k=1; k<=min(j,f); k++){
                dp[i][j]+=dp[i-1][j-k];
                dp[i][j]=dp[i][j]%(int(1e9+7));
            }
        }
    }
    
    return dp[d][target];
}
```
Space complexity: $$O(d*target)$$. Time complexity $$O(d*f*target)$$.

#### DP reduced dimension
DP can be reduced from 2D to 1D:
  $$dp[j]$$ represents the number of different ways with sum equals to j.
And we could loop j from target to 0, since dp[j] gets modified first before dp[j-k], similar to previous approaches.

```cpp
int numRollsToTarget(int d, int f, int target) {
    vector<int> dp(target+1, 0);
    // dp[j]: number of possible ways with the sum equals to j
    dp[0]=1;
    for(int i=1; i<=d; i++){
        for(int j=target; j>=0; j--){
            dp[j]=0;
            for(int k=1; k<=min(j,f); k++){
                dp[j]+=dp[j-k];
                dp[j]=dp[j]%(int(1e9+7));
            }
        }
    }
    
    return dp[target];
}
```
Space complexity: $$O(target)$$. Time complexity: $$O(d*f*target)$$.
