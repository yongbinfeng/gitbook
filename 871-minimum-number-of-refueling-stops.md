---
description: 'Dynamic Programming, Knapsack, Top K'
---

# 871 Minimum Number of Refueling Stops

#### DP Solution:

kind of similar to knapsack problem but not exactly the same:

- $$dp[i][j]$$ is the maximum distance the car could arrive. 
- Relation:
  - If $$dp[i-1][j-1]\leq stations[i][0]$$:
     $$dp[i][j]=max(dp[i-1][j], dp[i-1][j-1]+stations[i][1])$$
  - Otherwise: 
     $$dp[i][j]=dp[i-1][j]$$.
- The final result is to find the minimum j where $$dp[N][j]>=target$$.

```
int minRefuelStops(int target, int startFuel, vector<vector<int>>& stations) {
        int N=stations.size();
        vector<vector<long>> dp(N+1, vector<long>(N+1, 0));
        // dp[i][j]: maximum distance the car could reach with j refills from [0,i) stations
        
        dp[0][0]=startFuel;
        for(int i=0; i<N; i++){
            dp[i+1][0]=startFuel;
            for(int j=1; j<=i+1; j++){
                if(dp[i][j-1]>=stations[i][0]){
                    // could refill
                    dp[i+1][j]=max(dp[i][j], dp[i][j-1]+stations[i][1]);
                }
                else{
                    dp[i+1][j]=dp[i][j];
                }
            }
        }
        
        
        for(int j=0; j<=N; j++){
            if(dp[N][j]>=target)
                return j;
        }
        
        return -1;
    }
```
Space complexity: $$O(N^2)$$. Time complexity: $O(N^{2})$

Space complexity can be reduced to 1D by inverting the j calculation from i to 0. (since dp[i][j] depends on dp[i-1][j-1], we need to modify dp[j] first before dp[j-1]. Second way is to use a priorityQueue: we want to arrive at target with minimum refills, so we should try to find the station with maximum gas within its reaching ability.





