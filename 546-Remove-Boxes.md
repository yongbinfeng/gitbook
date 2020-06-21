---
description: 'Dynamic Programming, Merge Interval'
---

# 546. Remove Boxes

[Problem](https://leetcode.com/problems/remove-boxes/)

## DP Solution

Very hard problem. I can't solve it myself.

- $$dp[i][j][k]$$ is the maximum points we could get by removing $$boxes[i:j]$$, with $$k$$ boxes attached to $$boxes[i]$$ on the left, same color as $$boxes[i]$$;
- If we remove $$boxes[i]$$ first, then the points would be $$dp[i+1][j][0]+(k+1)^2$$;
- If we check if there are boxes inside $$[i:j]$$, with the same color as $$boxes[i]$$, the points would be $$dp[i+1][m-1][0]+dp[m][j][k+1]$$, where $$m$$ is from $$[i+1, j]$$ and satisfies $$boxes[i]==boxes[m]$$;
- So $$dp[i][j][k]$$ should be the maximum between these two cases;
- Boundary conditions are $$dp[i][i][k]=(k+1)*(k+1)$$, where $$k$$ is from $$[0,i]$$
- Final result is looking for $$dp[0][N-1][0]$$

```cpp
int removeBoxes(vector<int>& boxes) {
    int N = boxes.size();
    vector<vector<vector<int>>> dp(N, vector<vector<int>>(N, vector<int>(N,0)));
    // dp[i][j][k]: points gained by removing boxes[i:j], with k elements before boxes[i] that has the same color as boxes[i];
    
    for(int i=0; i<N; i++)
        for(int k=0; k<=i; k++)
            dp[i][i][k]=(k+1)*(k+1);
    
    for(int l=1; l<N; l++){
        for(int j=l; j<N; j++){
            int i=j-l;
            for(int k=0; k<=i; k++){
                dp[i][j][k]=(k+1)*(k+1)+dp[i+1][j][0];
                for(int m=i+1; m<=j; m++){
                    if(boxes[i]==boxes[m])
                        dp[i][j][k]=max(dp[i][j][k], dp[i+1][m-1][0]+dp[m][j][k+1]);
                }
            }
        }
    }
    
    return dp[0][N-1][0];
}
```

Time complexity $$O(N^4)$$; Space complexity $$O(N^3)$$. 

There might be better ways to solve this, but with my current skills this is the only way i could understand...
