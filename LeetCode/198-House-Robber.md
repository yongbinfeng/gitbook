---
description: 'Dynamic Programming, FSP, House Robber'
---

# 198. House Robber

[Problem](https://leetcode.com/problems/house-robber/)

## DP Solution

Standard FSP problem. At each $$nums[i]$$ there are two states: robbing or not robbing
- $$dp[i+1][0]$$ means the maximum profit for $$nums[0,i]$$, **without** robbing $$nums[i]$$;
- $$dp[i+1][1]$$ means the maximum profit for $$nums[0,i]$$, **with** robbing $$nums[i]$$;
- So for the relations:
  - $$dp[i+1][0]=max(dp[i][1], dp[i][0])$$, is the same as the maximum profit from $$nums[0,i-1]$$, since $$nums[i]$$ is not robbed;
  - $$dp[i+1][1]=dp[i][0]+nums[i]$$, is the maximum profit without robbing $$nums[i-1]$$, plus robbing $$nums[i]$$;

```cpp
int rob(vector<int>& nums) {
    int N = nums.size();
    
    vector<vector<int>> dp(N + 1, vector<int>(2, 0));
    // dp[i][0]: maximum profit for nums[0,i) without robbing house i-1
    // dp[i][1]: maximum profit for nums[0,i), WITH robbing house i-1
    
    for (int i = 0; i < N; ++i) {
        dp[i + 1][0] = max(dp[i][0], dp[i][1]);
        dp[i + 1][1] = dp[i][0] + nums[i];
    }
    
    return max(dp[N][0], dp[N][1]);
}
```

## DP reduced dimension

Space complexity can be reduced to $$O(1)$$

```cpp
int rob(vector<int>& nums) {
    int N = nums.size();
    
    int dp_0 = 0; // maximum profit without robbing house i
    int dp_1 = 0; // maximum profit with robbing house i
    
    for (int i = 0; i < N; ++i) {
        int temp = dp_0;
        dp_0 = max(dp_0, dp_1);
        dp_1 = temp + nums[i];
    }
    
    return max(dp_0, dp_1);
}
```
