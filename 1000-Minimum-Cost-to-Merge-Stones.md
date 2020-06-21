---
description: 'Dynamic Programming, Merge Interval'
---

# 1000. Minimum Cost to Merge Stones

[Problem](https://leetcode.com/problems/minimum-cost-to-merge-stones/)

## DP Solution

A very hard problem. Can not think of a solution by myself. And i still cant't fully understand
the solution yet.

- First, we figure out that every merge will reduce the number of stones by $$(K-1)$$; we want to make sure there is ONE stone after all merges, so $$(N-1)%(K-1)==0$$. Otherwise the task is impossible;
- $$dp[i][j]$$ is the minimum cost to merge $$stones[i:j]$$, to the minimal number of possible piles. For example, if $$K=3$$, $$i=1$$, $$j=4$$, we could merge $$[1,2,3]$$, or $$[2,3,4]$$, but 4 or 1 will not be merged and left there. $$dp$$ will be the minimum cost between $$[1,2,3]$$ and $$[2,3,4]$$;
- Given $$dp[i][j]$$, we loop over all possible mergable combinations: $$dp[i][j]=min(dp[i][k]+dp[k+1][j])$$ for $$k=i$$, $$i+(K-1)$$, $$i+2*(K-1)$$... with $$i<j$$; 
- When $$(j-i)%(K-1)==0$$, it means $$stones[i:j]$$ can be merged into 1 pile, and the cost of this merge is $$sum(stones[i:j])$$.

```cpp
int mergeStones(vector<int>& stones, int K) {
    // after every merge, the number of stones will decrease by K-1
    // we need one stone left eventually
    int N = stones.size();
    if ((N - 1) % (K - 1) != 0)
        return -1;
    
    vector<int> sums(N + 1, 0);
    // sum[i]: the sum of stones [0,i)
    for (int i = 1; i <= N; ++i) {
        sums[i] += sums[i - 1] + stones[i - 1];
    }
    
    vector<vector<int>> dp(N, vector<int>(N, 0));
    // dp[i][j]: minimum cost to remove stones in [i:j]
    
    for (int l = K - 1; l < N; ++l) {
        for (int i = 0; i < N - l; ++i) {
            int j = i + l;
            dp[i][j] = INT_MAX;
            for (int k = i; k < j; k += K - 1) {
                dp[i][j] = min(dp[i][j], dp[i][k] + dp[k+1][j]);  
            }
            if (l % (K - 1) == 0)
                // stones[i:j] can be merged into 1 pile
                dp[i][j] += sums[j+1] - sums[i];
        }
    }
        
        
    return dp[0][N-1];
}
```
