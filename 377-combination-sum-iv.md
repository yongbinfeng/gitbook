---
description: 'Dynamic Programming, Knapsack, DW'
---

# 377 Combination Sum IV

[Problem](https://leetcode.com/problems/combination-sum-iv/)

## DP Solution

This is a very interesting problem. It looks similar to a full knapsack problem \([Problem 518 Coin Exchange 2](https://leetcode.com/problems/coin-change-2/)\). But after careful looking, the two problems are totally different.

Problem 518 is a combination sum. Here in problem 377, the order matters, so it is a permutation sum \(yes the description and the title is shity.\)

Define the dp array as usual:

* $$dp[i][j]$$ means the number of unique ways to get sum j with \[0,i\) coins.
* $$dp[i][0]=1$$ since all coins are positive. There is only one way to get 0 sum.
* Relation
  * This is interesting. In the 01 knapsack case, $$dp[i][j]=dp[i-1][j]+dp[i-1][j-nums[i]]$$. 

    In the full knapsack case, the relation is $$dp[i][j]=dp[i-1][j]+dp[i][j-nums[i]]$$ 

    if $$j>=nums[i]$$, since $$nums[i]$$ could be reused. 

  * Here it is different since the order matters. For a given $$dp[i][j]$$, we should loop over all the $$nums$$. 

    $$dp[i][j]=\sum_k dp[i][j-nums[k]]$$, for all the $$k$$s that satisfies $$j>=nums[k]$$.

```cpp
int combinationSum4(vector<int>& nums, int target) {
    vector<vector<unsigned int>> dp(nums.size()+1, vector<unsigned int>(target+1, 0));

    for(int i=0; i<=nums.size(); i++)
        dp[i][0]=1;

    for(int i=0; i<nums.size(); i++){
        for(int j=0; j<=target; j++){
            for(int k=0; k<=i; k++){
                if(j>=nums[k])
                    dp[i+1][j]+=dp[i+1][j-nums[k]];
            }
        }
    }

    return dp[nums.size()][target];
}
```

Space complexity is $$O(target*N)$$. Time complexity is $$O(target*N^{2})$$. Different from the full knapsack case.

## DP reduced dimension

Look at the previous loop, the loop over $$i$$ could be simply removed. We just need the last loop where $$i=nums.size()-1$$.

```cpp
int combinationSum4(vector<int>& nums, int target) {
    vector<unsigned int> dp(target+1, 0);

    dp[0]=1;

    for(int j=0; j<=target; j++){
        for(int k=0; k<nums.size(); k++){
            if(j>=nums[k])
                dp[j]+=dp[j-nums[k]];
        }
    }

    return dp[target];
}
```

Space complexity is $$O(target)$$. Time complexity is $$O(target*N)$$. Both have been reduced.

## Notes:

* This looks pretty similar to the reduced loop of full knapsack, except the loop over target is done before loop over nums. 

  But the ideas behind this are totally different.

* The question also asks if $$nums$$ could be negative. Need to think about it.

