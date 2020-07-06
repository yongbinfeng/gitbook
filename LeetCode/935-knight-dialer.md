---
description: 'Dynamic Programming, DW, FSP, Grid'
---

# 935 Knight Dialer

[Problem](https://leetcode.com/problems/knight-dialer/)

## DP Solution

The problem is a little poor-described. Nevertheless, it is a problems with movements on 2D grids. For these irregular movements with a relatively small size, I usually save all the possible movements in a vector:

```cpp
vector<vector<int>> jumps={
    {4,6},{6,8},{7,9},{4,8},{0,3,9},{},
    {0,1,7},{2,6},{1,3},{2,4}
};
```

Then it becomes a standard DP problem counting distinct ways:

* $$dp[i][j]$$ is distinct numbers it could dail with $$i-1$$ movements starting from $$j$$.
* $$dp[i][j]=\sum_{move}dp[i-1][j-move]$$. 
* Final result is the sum over $$j$$ of all $$dp[N][j]$$.

```cpp
int knightDialer(int N) {
    if(!N)
        return 10;

    vector<vector<int>> jumps={
        {4,6},
        {6,8},
        {7,9},
        {4,8},
        {0,3,9},
        {},
        {0,1,7},
        {2,6},
        {1,3},
        {2,4}
    };

    vector<vector<unsigned int>> dp(N+1, vector<unsigned int>(10, 0));
    // dp[i][j]: distinct numbers could be dialed with i-1 hops starting from number j
    for(int j=0; j<10; j++)
        dp[1][j]=1;

    for(int i=2; i<=N; i++){
        for(int j=0; j<10; j++){
            for(auto jump: jumps[j]){
                dp[i][j]+=dp[i-1][jump];
            }
            dp[i][j]=dp[i][j]%(int(1e9)+7);
        }
    }

    unsigned int sum=0;
    for(int j=0; j<10; j++){
        sum+=dp[N][j];
        sum=sum%(int(1e9)+7);
    }

    return sum;
}
```

Space complexity: $$O(N)$$. Time complexity: $$O(N)$$.

## DP reduced dimension

The space complexity can be reduced to $$O(1)$$.

```cpp
int knightDialer(int N) {
    if(!N)
        return 10;

    vector<vector<int>> jumps={
        {4,6},{6,8},{7,9},{4,8},{0,3,9},{},
        {0,1,7},{2,6},{1,3},{2,4}
    };

    vector<unsigned int> dp(10, 1);
    // dp[j]: distinct numbers could be dialed starting from number j

    vector<unsigned int> dp_old = dp;

    for(int i=2; i<=N; i++){
        for(int j=0; j<10; j++){
            dp[j]=0;
            for(auto jump: jumps[j]){
                dp[j]+=dp_old[jump];
            }
            dp[j]=dp[j]%(int(1e9)+7);
        }
        dp_old = dp;
    }

    unsigned int sum=0;
    for(int j=0; j<10; j++){
        sum+=dp[j];
        sum=sum%(int(1e9)+7);
    }

    return sum;
}
```

Space complexity is $$O(1)$$. Time complexity: $$O(N)$$.

## Notes:

* In the $$O(1)$$ case, remember to set $$dp$$ to 0 since it is counting. \(standard in DW problems\).
* In the discussion section, there is the solution with $$O(\lg N)$$ complexity using adjacent matrix.
* Interesting article: [https://medium.com/hackernoon/google-interview-questions-deconstructed-the-knights-dialer-f780d516f029](https://medium.com/hackernoon/google-interview-questions-deconstructed-the-knights-dialer-f780d516f029)

