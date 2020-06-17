---
description: 'Dynamic Programming, LIS, DW'
---

# 673 Number of Longest Increasing Subsequence

[Problem](https://leetcode.com/problems/number-of-longest-increasing-subsequence/)

## DP Solution

Modified problem from longest increasing subsequence. In the LIS problem, we define $$dp[i]$$ as the size of the longest increasing subsequence ended with $$nums[i]$$. And the LIS is the maximum of all $$dp[i]$$s.

Here what the problem wants is the number of these LISs. Following the same idea, we define

* $$dp[i]$$ as the size of LISs ended with $$nums[i]$$;
* another vector $$count[i]$$ as the number of LISs ended with $$nums[i]$$;
* For each $$i$$, we loop over all $$j$$s in $$[0,i)$$. If $$nums[j]<nums[i]$$:
  * if $$dp[i]==dp[j]+1$$, $$nums[j]$$ could **also** be used as the last element before $$nums[i]$$ in the subsequence, $$count[i]+=count[j]$$;
  * if $$dp[i]<dp[j]+1$$, $$nums[j]$$ should be the last element before $$nums[i]$$ in the subsequence, $$count[i]=count[j]$$;
* Final result is the maximum $$dp[i]$$. And the sum of all $$counts[i]$$ where $$dp[i]==maxL$$.

```cpp
int findNumberOfLIS(vector<int>& nums) {
    vector<int> dp(nums.size(), 1);
    // dp[i]: LIS ended with nums[i]
    vector<int> count(nums.size(),1);
    // count[i]: number of LIS ended with nums[i]

    for(int i=1; i<nums.size(); i++){
        for(int j=0; j<i; j++){
            if(nums[j]<nums[i]){
                if(dp[i]<dp[j]+1){
                    // j will be used as the last element before i
                    dp[i]=dp[j]+1;
                    count[i]=count[j];
                }
                else if(dp[i]==dp[j]+1){
                    // j can also be used as the last element before i
                    count[i]+=count[j]; 
                }
            }
        }
    }

    int maxL = 0;
    int result = 0;
    for(int i=0; i<nums.size(); i++){
        //cout << "i " << " dp " << dp[i] << " count " << count[i] << endl;
        if(maxL<dp[i]){
            // LIS got changed
            maxL = dp[i];
            result = count[i];
        }
        else if(maxL==dp[i]){
            // another LIS with the same size but different end with the longest one
            result += count[i];
        }
    }

    return result;
}
```

**Notes:**

* $$dp$$ and $$count$$ should be initialized with 1s.

