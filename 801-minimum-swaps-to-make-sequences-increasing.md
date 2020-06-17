---
description: 'Dynamic Programming, FSP, DW'
---

# 801 Minimum Swaps To Make Sequences Increasing

[Problem](https://leetcode.com/problems/minimum-swaps-to-make-sequences-increasing/)

## DP Solution

This is a very interesting problem. Similar to [978. Longest Turbulent Subarray](https://leetcode.com/problems/longest-turbulent-subarray/) and the stocks problems, where there are multiple states, and the result of one state depends on the previous state. \(FSP\)

The first time i saw this problem, i mistakenly thought it was a two-pointer problem. My bad.

For each position $$i$$, there are two states:

> swap $$A[i]$$ and $$B[i]$$ or NOT swap.

So we define:

* $$dp[i][0]$$ is the minimum number of swaps to make $$A[i]$$ and $$B[i]$$ strictly increasing, while we do **NOT** swap $$A[i]$$ and $$B[i]$$; 
* $$dp[i][1]$$ is the minimum number of swaps to make $$A[i]$$ and $$B[i]$$ strictly increasing, where we do swap $$A[i]$$ and $$B[i]$$;

$$dp[i][0-1]$$ depends on the relations of $$A[i]$$ and $$B[i]$$. We define:

* `isBothIncreasing = (A[i-1]<A[i]) && (B[i-1]<B[i])`: A and B are increasing from $$i-1$$ to $$i$$; 
* `isChangedIncreasing = (A[i-1]<B[i]) && (B[i-1]<A[i])`: after swapping, A and B will be increasing from $$i-1$$ to $$i$$;

Since the problem states 'it is guaranteed that the given input always makes it possible', we have

`isBothIncreasing || isChangedIncreasing=1`.

Otherwise it would be impossible.

Then the $$dp$$ relations would be pretty clear. Explained in the code below.

```cpp
int minSwap(vector<int>& A, vector<int>& B) {
    int N = A.size();
    vector<vector<int>> dp(N, vector<int>(2,INT_MAX));
    // dp[i][0]: minimum number of swaps to make A[0,i] and B[0,j] strictly increasing, and A[i] and B[i] are NOT swapped
    // dp[i][0]: minimum number of swaps to make A[0,i] and B[0,j] strictly increasing, and A[i] and B[i] are swapped

    dp[0][0]=0;
    dp[0][1]=1;

    for(int i=1; i<N; i++){
        bool isBothIncreasing = (A[i-1]<A[i]) && (B[i-1]<B[i]);
        bool isChangedIncreasing = (A[i-1]<B[i]) && (B[i-1]<A[i]);
        // since it is guaranteed that the actions are feasible, here isBothIncreasing||isChangedIncreasing=1;

        if(isBothIncreasing && isChangedIncreasing){
            // does not matter if A[i-1] and B[i-1] got swapped or not
            dp[i][0]=min(dp[i-1][0], dp[i-1][1]);
            dp[i][1]=min(dp[i-1][0], dp[i-1][1])+1;
        }
        else if(isBothIncreasing){
            // A[i], B[i] and A[i-1], B[i-1] has to be either both swapped or neither swapped
            dp[i][0]=dp[i-1][0];
            dp[i][1]=dp[i-1][1]+1;
        }
        else{
            // isChangedIncreasing
            // it has to be either A[i], B[i] got swapped, or A[i-1], B[i-1] got swapped. But it can not be both are swapped or both are not swapped
            dp[i][0]=dp[i-1][1];
            dp[i][1]=dp[i-1][0]+1;
        }
    }

    return min(dp[N-1][0], dp[N-1][1]);
}
```

Space complexity and time complexity are both $$O(N)$$.

## DP reduced dimension

Since $$dp[i][0-1]$$ only depends on the previous states, the space complexity can be reduced to $$O(1)$$.

```cpp
int minSwap(vector<int>& A, vector<int>& B) {
    int N = A.size();
    int dp_0 = 0;
    int dp_1 = 1;
    // dp[0]: minimum number of swaps to make A and B strictly increasing, and A[i] and B[i] are NOT swapped
    // dp[0]: minimum number of swaps to make A and B strictly increasing, and A[i] and B[i] are swapped

    for(int i=1; i<N; i++){
        bool isBothIncreasing = (A[i-1]<A[i]) && (B[i-1]<B[i]);
        bool isChangedIncreasing = (A[i-1]<B[i]) && (B[i-1]<A[i]);
        // since it is guaranteed that the actions are feasible, here isBothIncreasing||isChangedIncreasing=1;

        int dp_0_old = dp_0;
        int dp_1_old = dp_1;

        if(isBothIncreasing && isChangedIncreasing){
            // does not matter if A[i-1] and B[i-1] got swapped or not
            dp_0=min(dp_0_old, dp_1_old);
            dp_1=min(dp_0_old, dp_1_old)+1;
        }
        else if(isBothIncreasing){
            // A[i], B[i] and A[i-1], B[i-1] has to be either both swapped or neither swapped
            dp_0=dp_0_old;
            dp_1=dp_1_old+1;
        }
        else{
            // isChangedIncreasing
            // it has to be either A[i], B[i] got swapped, or A[i-1], B[i-1] got swapped. But it can not be both are swapped or both are not swapped
            dp_0=dp_1_old;
            dp_1=dp_0_old+1;
        }
    }

    return min(dp_0, dp_1);
}
```

**Notes:**

* This problem can be reviewed with the [maximum turbulent array](https://leetcode.com/problems/longest-turbulent-subarray/) and the stocks problems together.

