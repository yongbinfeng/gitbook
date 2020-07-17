---
description: 'Binary Search'
---

# 81. Search in Rotated Sorted Array II

[Problem](https://leetcode.com/problems/search-in-rotated-sorted-array-ii/)

## Two binary search solution

Similar to Problem 33 and Problem 154, we can first find the pivot index, and do two binary 
searchs in `[0, pivot-1)` and `[pivot, nums.size()-1]` separately, and see if target could be found

```cpp
bool search(vector<int>& nums, int target) {
    int left = 0;
    int right = nums.size() - 1;
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] < nums[right])
            right = mid;
        else if (nums[mid] > nums[right])
            left = mid + 1;
        else if (nums[right] >= nums[right - 1])
            --right;
        else
            left = right;
    }
    int pivot = left;
    
    bool result_left = binarySearch(nums, 0, pivot - 1, target);
    if (result_left)
        return true;
    else
        return binarySearch(nums, pivot, nums.size() - 1, target);
}

bool binarySearch(const vector<int>& nums, int low, int high, int target) {
    int left = low;
    int right = high;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] == target)
            return true;
        else if (nums[mid] > target)
            right = mid - 1;
        else
            left = mid + 1;
    }
    return false;
}
```

## One binary search

Alternatively we do everything in one binary search, with more cases to be considered:

```cpp
bool search(vector<int>& nums, int target) {
    int left = 0;
    int right = nums.size() - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] == target)
            return true;
        if (nums[mid] > nums[right]) {
            // pivot in (mid, right]
            if (nums[mid] > target && nums[left] <= target)
                right = mid - 1;
            else
                left = mid + 1;
        } else if (nums[mid] < nums[right]) {
            // pivot in [left, mid]
            if (nums[mid] < target && nums[right] >= target)
                left = mid + 1;
            else
                right = mid - 1;
        } else {
            --right;
        }
    }
    
    return false;
}
```
