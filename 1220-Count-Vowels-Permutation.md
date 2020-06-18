---
description: 'Dynamic Programming, FSP, DW'
---

# 1220 Count Vowels Permutation

[Problem](https://leetcode.com/problems/count-vowels-permutation/)

## DP Solution

This is a standard FSP. Should not be hard actually. More close to medium level.
Close to [Problem 801 Minimum Swaps To Make Sequences Increasing](https:////leetcode.com/problems/minimum-swaps-to-make-sequences-increasing/).

We have 5 states here. Each state will have different consequenes:
- $$dp[i][j]$$ is the number of different strings starting with $$j$$ with length $$i+1$$. (j: 0 represents `a`; 1 represents `e`; 2 represents `i`, etc. It could also be $$dp[i][j]$$ represents the number with length $$i$$. Here since $$n\geq 1$$, we use $$i+1$$ to save one space. Not a big deal.)
- `a` will only be followed by `e`: $$dp[i][0]=dp[i-1][1]$$;
- `e` will only be followed by `a` or `i`: $$dp[i][1]=dp[i-1][0]+dp[i-1][2]$$;
- similar for `i`, `o`, `u`
- Boundary conditions: $$dp[0][j]=1$$ for all `j`s.

Workable code:
```cpp
int countVowelPermutation(int n) {
    int MOD = int(1e9)+7;
    
    vector<vector<unsigned>> dp(n, vector<unsigned>(5,0));
    // dp[i][0]: number of strings with length i+1 and start with a
    // dp[i][1]: number of strings with length i+1 and start with e
    // dp[i][2]: number of strings with length i+1 and start with i
    // dp[i][3]: number of strings with length i+1 and start with o
    // dp[i][4]: number of strings with length i+1 and start with u
    
    for(int j=0; j<5; j++)
        dp[0][j]=1;
    
    for(int i=1; i<n; i++){
        dp[i][0]=dp[i-1][1];
        dp[i][1]=dp[i-1][0]+dp[i-1][2];
        dp[i][2]=dp[i-1][0]+dp[i-1][1]+dp[i-1][3]+dp[i-1][4];
        dp[i][3]=dp[i-1][2]+dp[i-1][4];
        dp[i][4]=dp[i-1][0];
        
        for(int j=0; j<5; j++){
            dp[i][j]=dp[i][j]%MOD;
        }
    }
    
    int sum=0;
    for(int j=0; j<5; j++){
        sum+=dp[n-1][j];
        sum=sum%MOD;
    }
    return sum;

}
```

## DP reduced dimension

Space complexity can be reduced from $$O(N)$$ to $$O(1)$$, since $$dp[i]$$ only depends on $$dp[i-1]$$.

```cpp
int countVowelPermutation(int n) {
    int MOD = int(1e9)+7;
    
    vector<unsigned> dp(5,0);
    // dp[0]: number of strings start with a
    // dp[1]: number of strings start with e
    // dp[2]: number of strings start with i
    // dp[3]: number of strings start with o
    // dp[4]: number of strings start with u
    vector<unsigned> dp_pre(5,0);
    
    for(int j=0; j<5; j++)
        dp[j]=1;
    
    for(int i=1; i<n; i++){
        for(int j=0; j<5; j++){
            dp[j]=dp[j]%MOD;
            dp_pre[j] = dp[j];
        }
        
        dp[0]=dp_pre[1];
        dp[1]=dp_pre[0]+dp_pre[2];
        dp[2]=dp_pre[0]+dp_pre[1]+dp_pre[3]+dp_pre[4];
        dp[3]=dp_pre[2]+dp_pre[4];
        dp[4]=dp_pre[0];
        
    }
    
    int sum=0;
    for(int j=0; j<5; j++){
        dp[j]=dp[j]%MOD;
        sum+=dp[j];
        sum=sum%MOD;
    }
    return sum;

}
```

**Notes:**
- Now i realized [Problem 935 Knight Dialer](https://leetcode.com/problems/knight-dialer/) can also be treated as a FSP, if considering the numbers as different states.
- There are $$O(\log N)$$ solutions in dicussion section using Graph theorems. Might be interesting to take a look.
