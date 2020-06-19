---
description: 'Stack, Circular Array'
---

# 503. Next Greater Element II

[Problem](https://leetcode.com/problems/next-greater-element-ii/)

## Brutal Force

The typical ways to deal with circular arrays are:
- Copy the array for another time: $$temps=nums+nums$$. And loop over $$temps$$
- Use the module: loop over index `i` from `0` to `2*N-1`, and read the value using `nums[i%N]`. This would save a bit space

Using the brutal force, for each `nums[i]`, we check in `[i+1, 2*N-1]`, and find the next greater value

```cpp
vector<int> nextGreaterElements(vector<int>& nums) {
    int N = nums.size();
    
    vector<int> results(N, -1);
    
    for(int i=0; i<N; i++){
        for(int j=i+1; j<2*N; j++){
            if(nums[j%N]>nums[i]){
                results[i]=nums[j%N];
                break;
            }
        }
    }
    
    return results;
}
```

Space complexity is $$O(N)$$. Time complexity $$O(N^2)$$.

## Stack

Thinking about this problem, if $$nums[i]<nums[i+1]$$, we can immediately find the next greater value of $$nums[i]$$ should be $$nums[i+1]$$.
But what should we do if $$nums[i]\geq nums[i+1]$$? We can use a stack to save these indices. For a new value, we check if the new value could
be the next greater value of these in the stack.

```cpp
vector<int> nextGreaterElements(vector<int>& nums) {
    int N = nums.size();
    vector<int> results(N, -1);
    
    stack<int> s;
    for(int i=0; i<2*N; i++){
        while(!s.empty() && nums[s.top()]<nums[i%N]){
            results[s.top()]=nums[i%N];
            s.pop();
        }
        if(i<N){
            s.push(i);
        }
    }
    
    return results;
}
```

Time complexity is reduced to $$O(N)$$ in this case. Space complexity is still $$O(N)$$.

## Notes
- Remember typical ways to deal with circular arrays
- Using stacks could sometimes greatly help solving problems. Here is a interesting [list](https://leetcode.com/problems/next-greater-element-ii/discuss/98270/JavaC%2B%2BPython-Loop-Twice)
