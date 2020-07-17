---
description: 'Binary Search'
---

# 35. Search Insert Position

[Problem](https://leetcode.com/problems/search-insert-position/)

## Binary search solution

Standard binary search problem. Initialize the `left` to `0`, and `right` to `nums.size()-1`. The search interval is `[left, right]`.
Check if `nums[mid]` is the same as target:
- if yes, return `mid`;
- Or if `nums[mid]>target`, then `right` should be `mid-1`;
- Otherwise `left` should be `mid+1`;

The terminate condition is `left=right+1`. In this case the number is not found, the left index should be the index where `target` would be 
inserted.

```cpp
int searchInsert(vector<int>& nums, int target) {
    int left = 0;
    int right = nums.size() - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] == target)
            return mid;
        else if (nums[mid] > target)
            right = mid - 1;
        else
            left = mid + 1;
    }
    return left; 
}
```
