---
description: 'Dynamic Programming, Binary Tree, BST, Catalan number'
---

# 96. Unique Binary Search Trees

[Problem](https://leetcode.com/problems/unique-binary-search-trees/)

## DP solution

Thinking about the process, when $$n=1$$, there is one BST; when $$n=2$$, there are two BSTs.
When $$n=3$$, the root could be 1,2,3: 
- if root is 1, then $$[2,3]$$ must be on the right side of 1, and the number of ways is the same as $$n=2$$, since ways to place $$[2,3]$$ is the same as $$[1,2]$$;
- if root is 2; then 1 must be on the left side, 3 must be on the right side;
- if root is 3; then $$[1,2]$$ must be on the left side, and the number of ways is the same as $$n=2$$;

One more example: when $$n=5$$,
- if root is 1, then $$[2,5]$$ must be on the right side, and the number of ways is the same as $$n=4$$;
- if root is 2, then $$[3,5]$$ must be on the right side, 1 must be on the left side;
- if root is 3, then $$[1,2]$$ must be on the left side, $$[4,5]$$ must be on the right side, the number of ways is $$ways(n=2)*ways(n=2)$$;

Now we have figured out: for n:
- $$dp[i]$$ is the number of unique BSTs with number 1,2,3,...i;
- $$dp[i]=\sum dp[i-root][root-1]$$, where $$root$$ is a value in $$[1,i]$$, because of unique BSTs on the left side is $$dp[root-1]$$, and the BSTs on the right side is $$dp[i-root]$$;
- Boundary condition: $$dp[1]=1$$, and we set $$dp[0]=1$$;

```cpp
int numTrees(int n) {
    vector<int> dp(n+1, 0);
    // dp[i]: number of unique BSTs that sore values 1...i;
    dp[0]=1;
    dp[1]=1;
    
    for(int i=2; i<=n; i++){
        for(int root=1; root<=i; root++){
            dp[i]+=dp[root-1]*dp[i-root];
            // root used as the root of BST, left side: root-1; right side: i-root
        }
    }
    
    return dp[n];
}
```
This is actually *catalan number*.

Space complexity is $$O(N)$$; Time complexity is $$O(N^2)$$.
