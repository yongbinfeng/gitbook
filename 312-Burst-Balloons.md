---
description: 'Dynamic Programming, Merge Interval'
---

# 312. Burst Balloons

[Problem](https://leetcode.com/problems/burst-balloons/)

## DP Solution

Bursting $$i$$ depends on both the left and right sides $$nums[i-1]$$ and $$nums[i+1]$$. In cases like this, we 
use the interval dp definition. Since it is assumed there are two 1s on both ends, we fill in another array, with 
two ends 1 and the central is the same as $$nums$$.
- $$dp[i][j]$$ is the points gained by bursting ballons $$nums(i,j)$$, with $$i$$ and $$j$$ excluded;
- $$dp[i][j] = max(dp[i][k] + dp[k][j] + nums[i]*nums[k]*nums[j])$$, for $$k$$ in $$(i,j)$$; no $$k\pm1$$ in indices because $$dp$$ definition does not include bursting the two ends
- final result is looking for $$dp[0][N+1]$$;

```cpp
int maxCoins(vector<int>& nums) {
    int N = nums.size();
    
    vector<int> values(N + 2, 1);
    // values: copy nums and insert in the front and end with 1
    // such that no special treatment needed in the two edges
    for (int i = 0; i < N; ++i) {
        values[i + 1] = nums[i];
    }
    
    vector<vector<int>> dp(N + 2, vector<int>(N + 2, 0));
    // dp[i][j]: burst ballons in values(i:j)
    for (int l = 2; l < N + 2; ++l) {
        for (int i = 0; i < N + 2 - l; ++i) {
            int j = i + l;
            for (int k = i + 1; k < j; ++k){
                dp[i][j] = max(dp[i][j], dp[i][k] + values[i]*values[k]*values[j] + dp[k][j]);
            }
        }
    }
    
    return dp[0][N+1];
    
}
```

## Notes
- In the relations, it is $$dp[i][k]$$, not $$dp[i][k-1]$$ since the $$dp[i][j]$$ definition is excluding $$i$$ and $$j$$;
