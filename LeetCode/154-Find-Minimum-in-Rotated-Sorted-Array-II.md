---
description: 'Binary Search'
---

# 154. Find Minimum in Rotated Sorted Array II

[Problem](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array-ii/)

## Binary search solution

Similar to Problem 153, except here there might be duplicates. Start with `left=0` and `right=nums.size()-1`.
- if `nums[mid]<nums[right]`, then the pivot point should be in `(mid,right]`, we set `left=mid+1`;
- else if `nums[mid]<nums[right]`, then the pivot point should be in `[left,mid]`, we set `right=mid`;
- else it would be `nums[mid]==nums[right]`, we check if `right` itself is the pivot point or not, by comparing `nums[right-1]` with `nums[right]`, if `right` is not the pivot point, we do `--right` and redo the comparisons; 

The end condition should be `left<right`, since there is no return inside the loop.

```cpp
int findMin(vector<int>& nums) {
    int left = 0;
    int right = nums.size() - 1;
    
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] < nums[right]) 
            right = mid;
        else if (nums[mid] > nums[right])
            left = mid + 1;
        else if (nums[right - 1] <= nums[right])
            --right;  // right is not the pivot point
        else
            left = right; // right is the pivot point
    }
    
    return nums[left];
}
```

## Notes
Actually, the check if `right` is the pivot point is not necessary: even if it is, since we have `nums[mid]==nums[right]`, the pivot value is still in the interval when we do `--right`, we can still get the pivot value, but not the pivot index. With the special check, we could get both the value and the index correct.
