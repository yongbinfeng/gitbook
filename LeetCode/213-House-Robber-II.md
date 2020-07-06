---
description: 'Dynamic Programming, FSP, House Robber'
---

# 213. House Robber II

[Problem](https://leetcode.com/problems/house-robber-ii/)

## DP Solution

The problem is very similar to [Problem 198. House Robber](https://leetcode.com/problems/house-robber/), except there is one ring 
connecting the begining and the end. We break the ring into 2 subsets: the first is we could rob the 0th but not the end; the other 
is we could rob the end but not the beginning 0th.

```cpp
int rob(vector<int>& nums) {
    int N = nums.size();
    if (N==1)
        return nums[0];
    
    int prof1 = robhouse(nums, 0, N - 2); // rob the beginning, not the end
    int prof2 = robhouse(nums, 1, N - 1); // rob the end, not the beginning
    
    return max(prof1, prof2);
}

int robhouse(const vector<int>& nums, int nfront, int nback){
    int dp_0 = 0; // maximum profit without robbing i
    int dp_1 = 0; // maximum profit with robbing i
    
    int i = nfront;
    while (i <= nback){
        int temp = dp_0;
        dp_0 = max(dp_0, dp_1);
        dp_1 = temp + nums[i];
        ++i;
    }
    
    return max(dp_0, dp_1);
}
```

## Notes
- Remember to deal with the case where $$N=1$$;
