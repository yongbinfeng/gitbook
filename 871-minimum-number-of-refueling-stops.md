---
description: 'Dynamic Programming, Knapsack, Top K'
---

# 871 Minimum Number of Refueling Stops

#### DP Solution:

kind of similar to knapsack problem but not exactly the same:

 $$dp[i][j]$$ is the maximum distance the car could arrive. 

Dp\[i\]\[j\]=max\(dp\[i-1\]\[j\], dp\[i-1\]\[j-1\]+gas\[i\]\) if dp\[i-1\]\[j-1\]&gt;=distance\[i\]. otherwise dp\[i\]\[j\]=dp\[i-1\]\[j\].

```text

The final result is to find the minimum j where dp[N][j]>=target.

Space complexity can be reduced to 1D by inverting the j calculation from i to 0. (since dp[i][j] depends on dp[i-1][j-1], we need to modify dp[j] first before dp[j-1]. Second way is to use a priorityQueue: we want to arrive at target with minimum refills, so we should try to find the station with maximum gas within its reaching ability.
```





