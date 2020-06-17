---
description: 'Dynamic Programming, DW'
---

# 808 Soup Servings

[Problem](https://leetcode.com/problems/soup-servings/)

## DP Solution

First of all, the unit is 25ml: basically all operations are based on every 25ml. So we should convert $$ml$$ to 1 by

`int n=(N+24)/25`

Then it becomes a standard DP problem:

* $$dp[i][j]$$ is the result when A is i, B is j;
* Boundary conditions: $$dp[0][0]=0.5$$; $$dp[0][j]=1$$; $$dp[i][0]=0$$;
* Relation:
  * $$dp[i][j]=(dp[i-4][j]+dp[i-3][j-1]+dp[i-2][j-2]+dp[i-1][j-3])*0.25$$

The kind of cheating part is the space complexity is $$O(N^2)$$. And the problem states $$N$$ could be as large as $$10^{9}$$. Clearly the memory does not allow. But with a few tests we could find

$$dp[N][N]=1$$ for $$N>=5000$$

Kind of funny...But i can't find any code without making use of this relation.

```cpp
double soupServings(int N) {
    if(N>5000)
        return 1.0;

    int n=(N+24)/25; // conver N into 25ml unit

    vector<vector<double>> dp(n+1, vector<double>(n+1, 0));
    // dp[i][j]: result of the result with i A and j B

    dp[0][0]=0.5;
    for(int j=1; j<=n; j++)
        dp[0][j]=1.0;

    for(int i=1; i<=n; i++){
        for(int j=1; j<=n; j++){
            double sum=0;
            sum+=dp[max(i-4,0)][j];
            sum+=dp[max(i-3,0)][j-1];
            sum+=dp[max(i-2,0)][max(j-2,0)];
            sum+=dp[i-1][max(j-3,0)];
            dp[i][j]=sum*0.25;
        }
    }

    return dp[n][n];
}
```

**Notes:**

* The fact is for $$N>5000$$, the result is always 1.. So when one sees N could go as much as $$10^{9}$$, it does not necessarily mean $$O(N^2)$$ space/time complexity is not allowed..

