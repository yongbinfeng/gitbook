---
description: 'Dynamic Programming, DW, Grid'
---

# 576. Out of Boundary Paths

[Problem](https://leetcode.com/problems/out-of-boundary-paths/)

## DP Solution

Standard DW/MPRT problem. Similar to [Problem 935 Knight Dialer](https://leetcode.com/problems/knight-dialer/).
Same idea.

```cpp
int findPaths(int m, int n, int N, int i, int j) {
    vector<vector<vector<unsigned>>> dp(N+1, vector<vector<unsigned>>(m, vector<unsigned>(n,0)));
    
    vector<vector<int>> moves = {{-1,0}, {1,0}, {0,1}, {0,-1}};
    
    for(int step=1; step<=N; step++){
        for(int p=0; p<m; p++){
            for(int q=0; q<n; q++){
                for(auto move:moves){
                    int pnew = p+move[0];
                    int qnew = q+move[1];
                    if(pnew<0 || pnew>=m || qnew<0 || qnew>=n)
                        dp[step][p][q]+=1;
                    else
                        dp[step][p][q]+=dp[step-1][pnew][qnew];
                }
                dp[step][p][q]=(dp[step][p][q]%(int(1e9+7)));
            }
        }
    }
    
    return dp[N][i][j];
}
```

## DP reduced dimension

```cpp
int findPaths(int m, int n, int N, int i, int j) {
    vector<vector<unsigned>> dp(m, vector<unsigned>(n,0));
    
    vector<vector<int>> moves = {{-1,0}, {1,0}, {0,1}, {0,-1}};
    
    for(int step=1; step<=N; step++){
        vector<vector<unsigned>> dpold = dp;
        for(int p=0; p<m; p++){
            for(int q=0; q<n; q++){
                dp[p][q]=0;
                for(auto move:moves){
                    int pnew = p+move[0];
                    int qnew = q+move[1];
                    if(pnew<0 || pnew>=m || qnew<0 || qnew>=n)
                        dp[p][q]+=1;
                    else
                        dp[p][q]+=dpold[pnew][qnew];
                }
                dp[p][q]=(dp[p][q]%(int(1e9+7)));
            }
        }
    }
    
    return dp[i][j];
}
```

**Notes:**
- Same as usual DW, remember to reset $$dp[p][q]$$ to 0 at the begining of counting;
- The running time and space needed seem a lot in the leetcode submission result. Might be interesting to understand why.
