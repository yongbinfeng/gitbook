---
description: 'Binary Search'
---

# 33. Search in Rotated Sorted Array

[Problem](https://leetcode.com/problems/search-in-rotated-sorted-array/)

## Two binary searches

We could use the results from Problem 153: first to find the pivot point `pivot`.
Then do two binary searches: one in `[0, pivot-1]`, the other in `[pivot, nums.size()-1]`;

```cpp
int search(vector<int>& nums, int target) {
    // find the pivot index
    int left = 0;
    int right = nums.size() - 1;
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] > nums[right])
            left = mid + 1;
        else
            right = mid;
    }
    int pivot = left;
    int result = binarySearch(nums, 0, pivot - 1, target);
    if (result >= 0)
        return result;
    return binarySearch(nums, pivot, nums.size() - 1, target);
}

int binarySearch(const vector<int>& nums, int low, int high, int target) {
    int left = low;
    int right = high;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] == target)
            return mid;
        else if (nums[mid] > target)
            right = mid - 1;
        else
            left = mid + 1;
    }
    return -1;
}
```

## One binary search

We could merge the search for pivot point and search for target processes into one process, but 
we need to consider more cases this way:

```cpp
int search(vector<int>& nums, int target) {
    int left = 0;
    int right = nums.size() - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (target == nums[mid])
            return mid;
        if (nums[mid] < nums[right]) {
            if (target > nums[mid] && target <= nums[right])
                // target should fall in (mid, right]
                left = mid + 1;
            else
                right = mid - 1;
        } else {
            // nums[mid] > nums[right]
            if (target < nums[mid] && target >= nums[left])
                right = mid - 1;
            else
                left = mid + 1;
        }
    }
    
    return -1;
}
```
