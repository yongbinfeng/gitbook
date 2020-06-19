---
description: 'Stack'
---

# 496. Next Greater Element I

[Problem](https://leetcode.com/problems/next-greater-element-i/)

## Brutal Force

This problem is basically the same as [Problem 503](https://leetcode.com/problems/next-greater-element-ii/), except
in Problem 503 the vector is circular and it needs the `i%N` module treatment or the copy-another-time treatment.

Brutal force is for one value in `nums1`, loop over `nums2` and find it position, and find its next greater number

```cpp
vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
    vector<int> results(nums1.size(), -1);
    
    for(int i=0; i<nums1.size(); i++){
        int num=nums1[i];
        bool found=false;
        for(int j=0; j<nums2.size(); j++){
            if(nums2[j]==num)
                found=true;
            if(found && nums2[j]>num){
                results[i]=nums2[j];
                break;
            }
        }
    }
    
    return results;
}
```

Time complexity is $$O(n1*n2)$$. Space complexity $O(n1)$$.

## Stack

Similar to Problem 503, we use a stack and the time complexity would be reduced to $$O(n2)$$. Here `nums1` is 
a subset of `nums2`. We find all the next greater values of `nums2`, and use a map to save them.

```cpp
vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
    unordered_map<int, int> m;
    // m[i]=j: the next greater number of i is j, for i,j in nums2
    stack<int> s;
    
    for(int i=0; i<nums2.size(); i++){
        int num=nums2[i];
        while(!s.empty() && num>s.top()){
            m[s.top()]=num;
            s.pop();
        }
        s.push(num); // num is the smallest in stack
    }
    
    vector<int> results(nums1.size(), -1);
    for(int i=0; i<nums1.size(); i++){
        int num=nums1[i];
        if(m.find(num)!=m.end())
            results[i]=m[num];
    }
    
    return results;
}
```

Space complexity is slightly increase to $$O(n2)$$, but the time complexity is $$O(n2)$$ now.
