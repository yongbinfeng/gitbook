---
description: 'Dynamic Programming, DW, Grid'
---

# 688 Knight Probability in Chessboard

[Problem](https://leetcode.com/problems/knight-probability-in-chessboard/)

## DP Solution:

this is a pretty standard DP problem in 2D grids:

* $$dp[k][i][j]$$ is the probability that the knight is still in the chessboard after k jumps starting from $$(i,j)$$.
* when $$k=0$$, $$dp$$ is always 1, since the knight is always in the chessboard at the beginning, with no jumps.
* Relation:
  * $$dp[k+1][i][j]+=dp[k][i+move_i][j+move_j]$$. $$move_i$$ and $$move_j$$ looped in all the 8 moves. \(standard counting\).

```cpp
double knightProbability(int N, int K, int r, int c) {

    vector<vector<int>> moves={{-2,-1},{-1,-2}, 
                               {1,2}, {2,1}, 
                               {2,-1}, {1,-2},
                               {-1,2}, {-2,1}};

    vector<vector<vector<double>>> dp(K+1,vector<vector<double>>(N, vector<double>(N,0.)));
    // dp[k][i][j]: probability of staying inside the board with k jump from (i,j)
    for(int i=0; i<N; i++)
        for(int j=0; j<N; j++)
            dp[0][i][j]=1.0;

    for(int k=1; k<=K; k++){
        for(int i=0; i<N; i++){
            for(int j=0; j<N; j++){
                for(auto move: moves){
                    if(isInside(i+move[0], j+move[1], N)){
                        dp[k][i][j]+=0.125*dp[k-1][i+move[0]][j+move[1]];
                    }
                }
            }
        }
    }

    return dp[K][r][c];
}

bool isInside(int i, int j, int N){
    return (i>=0 && i<N && j>=0 && j<N);
}
```

Space complexity: $$O(K*N^{2})$$. Time complexity: $$O(K*N^2)$$.

## DP reduced dimension

The space complexity can be reduced to 2D, using another 2D array to keep track of the old DP array

```cpp
double knightProbability(int N, int K, int r, int c) {

    vector<vector<int>> moves={{-2,-1},{-1,-2}, 
                               {1,2}, {2,1}, 
                               {2,-1}, {1,-2},
                               {-1,2}, {-2,1}};

    vector<vector<double>> dp(N, vector<double>(N,1.));
    // dp[i][j]: probability of staying inside the board from (i,j)      
    for(int k=1; k<=K; k++){
        vector<vector<double>> dpold = dp;
        for(int i=0; i<N; i++){
            for(int j=0; j<N; j++){
                dp[i][j]=0;
                for(auto move: moves){
                    if(isInside(i+move[0], j+move[1], N)){
                        dp[i][j]+=0.125*dpold[i+move[0]][j+move[1]];
                    }
                }
            }
        }
    }

    return dp[r][c];
}

bool isInside(int i, int j, int N){
    return (i>=0 && i<N && j>=0 && j<N);
}
```

Space complexity: $$O(N^{2})$$. Time complexity: $$O(K*N^2)$$.

**Notes:**

* Don't forget to set $$dp[i][j]$$ to 0 at the beginning of the moves. \(standar procedure in DW.\)

