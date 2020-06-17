---
description: 'Dynamic Programming, Knapsack, DW'
---

# 494 Target Sum

[Problem](https://leetcode.com/problems/target-sum/)

## DP Solution

The problem is asking for number of combinations of $$+$$ and $$-$$ such that the sum is equal to target. Put the problem in the other way round, we can treat it as finding some numbers with $$+$$ and the rest with $$-$$, such that the sum is equal to the target.

We know the sum given the known arrays $$nums$$. We need to find some numbers with the sum equals to $$\frac{sum+target}{2}$$. And the rest sum equals to $$\frac{sum-target}{2}$$. Then the sum would be $$target$$.

This becomes a 01 knapsack problem where we want to find some coins with the sum equals to $$\frac{sum+target}{2}$$.

* $$dp[i][j]$$ is the number of ways with $$[0,i)$$ coins and sum is $$j$$. \(standard DP array definition in 01 knapsack problem\).
* $$dp[0][0]=0$$ for the boundary condition.
* Relation:
  * $$dp[i+1][j]=dp[i][j-nums[i]]+dp[i][j]$$ if $$j>=nums[i]$$. \(we can decide if we take this coin in or not if the value of the coin is not greater than the value of the target.\)
  * $$dp[i+1][j]=dp[i][j]$$ if $$j<nums[i]$$. This coin can not be included if the value itself is larger than the target.

```cpp
int findTargetSumWays(vector<int>& nums, int S) {
    int sum=0;
    for(auto num: nums)
        sum+=num;

    if(abs(S)>abs(sum))
        return 0;

    if((S-sum)%2!=0)
        // S and sum must be both odd or even
        return 0;

    vector<vector<int>> dp(nums.size()+1, vector<int>((S+sum)/2+1, 0));
    // dp[i][j]: number of ways with numbers [0,i) and total sum is j.
    dp[0][0]=1;
    for(int i=0; i<nums.size(); i++){
        for(int j=0; j<=(S+sum)/2; j++){
            if(j>=nums[i])
                dp[i+1][j]= dp[i][j-nums[i]]+dp[i][j];
            else
                dp[i+1][j]=dp[i][j];
        }
    }

    return dp[nums.size()][(S+sum)/2];
}
```

Space complexity: $$O(target*N)$$. Time complexity: $$O(target*N)$$.

## DP reduced dimension

The space complexity can be reduced to target, by reverting the $$j$$ loop from $$(S+sum)/2$$ to 0, since $$dp[j]$$ gets updated first before $$dp[j-nums[i]]$$.

```cpp
int findTargetSumWays(vector<int>& nums, int S) {
    int sum=0;
    for(auto num: nums)
        sum+=num;

    if(abs(S)>abs(sum))
        return 0;

    if((S-sum)%2!=0)
        // S and sum must be both odd or even
        return 0;

    vector<int> dp((S+sum)/2+1, 0);
    // dp[j]: number of ways with total sum is j.
    dp[0]=1;
    for(int i=0; i<nums.size(); i++){
        for(int j=(S+sum)/2; j>=0; j--){
            if(j>=nums[i])
                dp[j]+=dp[j-nums[i]];
        }
    }

    return dp[(S+sum)/2];
}
```

Space complexity: $$O(target)$$. Time complexity: $$O(target*N)$$.

**Notes:**

* Understand better the boundary conditions: sometimes i tend to write $$dp[0]=0$$ at the beginning of each $$i$$ loop, but this is not true, as the members of nums\[i\] could be zero.

