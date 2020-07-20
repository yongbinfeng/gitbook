---
description: 'Bit Manipulation'
---

[Problem](https://leetcode.com/problems/number-of-1-bits/)

## Bit Shift Solution

Check if the last digit is 1, shift the number 1 bit to the right, repeat.

```cpp
int hammingWeight(uint32_t n) {
    int result=0;
    while(n){
        if(n&1)
            result++;
        n= n>>1;
    }
    
    return result;
}
```

Space complexity: $$O(1)$$. Time complexity: $$O(\log N)$$.

## n & (n-1)

Every `n&(n-1)` operation will clear one `1` in the number. We repeat it until `n=0`.

```cpp
int hammingWeight(uint32_t n) {
    int result = 0;
    while (n) {
        n = n & (n - 1);
        ++result;
    }
    return result;
}
```

Space complexity: $$O(1)$$. Time complexity: $$O(M)$$, where `M` is the number of `1`s in `n`.
