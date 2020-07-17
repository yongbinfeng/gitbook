---
description: 'Binary Search'
---

# 153. Find Minimum in Rotated Sorted Array

[Problem](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)

## Binary search solution

Start from `left=0` and `right=nums.size()-1`; compare `nums[mid]` with `nums[right]`
- if `nums[mid]>nums[right]`, then the pivot point must be in `(mid, right]`, set `left=mid+1`;
- else, `nums[mid]<nums[right]`, since there is no duplicate, and the pivot point must be in `[left,mid]`, set `right=mid`; (**note `mid` itself could be the pivot point in this case**)

End condition is when `left=right`, and `left` (or `right`) should be the pivot point. (**if we are not returning from inside the while loop, then it should be `left<right`, not `left<=right`**)

Why would we compare `mid` with `right`, not `mid` with `left`?
> The `left` could be the pivot point: if `nums[mid]<nums[left]`, we know the pivot point should be in `(left, mid]`; but if `nums[mid]>nums[left]`, the pivot point could be in `(mid, right]`, but it could also be the `left`. If compare `mid` with `right`, we dont have this problem.

```cpp
int findMin(vector<int>& nums) {
    int left = 0;
    int right = nums.size() - 1;
    
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] > nums[right])
            left = mid + 1;
        else
            right = mid;
    }
    
    return nums[left];
}
```
