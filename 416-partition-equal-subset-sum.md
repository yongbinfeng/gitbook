---
description: 'Dynamic Programming, Knapsack, DW'
---

# 416 Partition Equal Subset Sum

[Problem](https://leetcode.com/problems/partition-equal-subset-sum/)

## DP Solution

This is a standard knapsack problem. Similar to [Problem 518 Coin change 2](https://leetcode.com/problems/coin-change-2/).

We caclulate the sum of $$nums$$. Then the problem is asking if there is at least one way to get the sum equals to $$\frac{sum}{2}$$ with $$nums$$:

* $$dp[i][j]$$ is if there is at least one way to get sum equals $$j$$ with nums $$[0,i)$$.
* $$dp[i][j]=dp[i-1][j] || dp[i-1][j-nums[i]]$$ if $$j>=nums[i]$$.
* Otherwise $$dp[i][j]=dp[i-1][j]$$.
* If number of each values are unlimited, the relation becomes $$dp[i][j]=dp[i-1][j] || dp[i][j-nums[i]]$$. Slightly different.

```cpp
bool canPartition(vector<int>& nums) {
    int sum=0;
    for(auto num: nums)
        sum+=num;

    if(sum%2)
        return false;

    vector<vector<bool>> dp(nums.size()+1, vector<bool>(sum/2+1,0));
    // dp[i][j]: if the sum of some elements from [0,i) could be j

    for(int i=0; i<=nums.size(); i++)
        dp[i][0]=1;

    for(int i=0; i<nums.size(); i++){
        for(int j=1; j<=sum/2; j++){
            if(j>=nums[i])
                dp[i+1][j]= dp[i][j] || dp[i][j-nums[i]];
            else
                dp[i+1][j]= dp[i][j];
        }
    }

    return dp[nums.size()][sum/2];
}
```

## DP reduced dimension

The space complexity can be reduced to $$O(sum)$$, by inverting the j calculations from $$\frac{sum}{2}$$ to 0.

```cpp
bool canPartition(vector<int>& nums) {
    int sum=0;
    for(auto num: nums)
        sum+=num;

    if(sum%2)
        return false;

    vector<bool> dp(sum/2+1,0);
    // dp[j]: if the sum of some elements could be j

    dp[0]=1;

    for(int i=0; i<nums.size(); i++){
        for(int j=sum/2; j>=1; j--){
            if(j>=nums[i])
                dp[j]= dp[j] || dp[j-nums[i]];
        }
    }

    return dp[sum/2];
}
```

## Notes:

* There are ways to further reduce the space complexity using bits. But i personally prefer to stick to the standard method, more

  straightforward and easy to understand.

