---
description: 'Binary Search'
---

# 278. First Bad Version

[Problem](https://leetcode.com/problems/first-bad-version/)

## Binary search solution

Set the `left` to `1`, and the `right` to `n`. Check the `mid`:
- if `mid` is bad, then `right=mid`;
- else `left=mid+1`;

The loop should end when `left=right`, and we know in this case `left` (or `right`)
is the beginning index of the bad version; **If we do not return inside the `while` loop, 
we should use `left<right`, not `left<=right` in the while loop condition**

```cpp
int firstBadVersion(int n) {
    int left = 1;
    int right = n;
    
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (isBadVersion(mid))
            right = mid;
        else
            left = mid + 1;
    }
    
    return left;
}
```
