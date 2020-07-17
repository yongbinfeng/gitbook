---
description: 'Binary Search'
---

# 74. Search a 2D Matrix

[Problem](https://leetcode.com/problems/search-a-2d-matrix/)

## Binary search solution

Binary search in 2D arrays: think of the $$M\timesN$$ array as a 1D array with $$[0, M*N-1]$$.
Start with `left=0`, `right=M*N-1`. Convert the `mid` to the `(i,j)` index. Then everything is the same as
a normal binary search problem.

```cpp
bool searchMatrix(vector<vector<int>>& matrix, int target) {
    if (!matrix.size() || !matrix[0].size())
        return false;
    
    int m = matrix.size();
    int n = matrix[0].size();
    int left = 0;
    int right = m * n - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        // the [i][j] index of the mid in the matrix
        int mid_i = mid / n; 
        int mid_j = mid % n;
        if (matrix[mid_i][mid_j] == target)
            return true;
        else if (matrix[mid_i][mid_j] > target)
            right = mid - 1;
        else
            left = mid + 1;
    }
    
    return false;
}
```

## Notes
Compare this problem with [Problem 240. Search a 2D Matrix II](https://leetcode.com/problems/search-a-2d-matrix-ii/).
In that problem, if we want to do the binary search, we have to do it line by line, and the time complexity is $$O(M\log N)$$;
Instead we use its properties and search from the bottom left or the top right corner, and each comparison we remove one line
or one column, the space complexity in that case is $$O(M+N)$$;
