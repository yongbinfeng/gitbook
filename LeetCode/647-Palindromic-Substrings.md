---
description: 'Dynamic Programming, String, Substring, Interval'
---

# 647. Palindromic Substrings

[Problem](https://leetcode.com/problems/palindromic-substrings/)

## Expansion Solution

We start from the central and expand it towards the two ends, until charaters at the two ends are not the same
```cpp
public:
    int countSubstrings(string s) {
        int count = 0;
        for (int i = 0; i < s.size(); ++i) {
            count += expandString(s, i, i);
            count += expandString(s, i, i+1);
        }
        return count;
    }

private:
    int expandString(const string& s, int left, int right){
        int count = 0;
        while(left >= 0 && right < s.size() && s[left]==s[right]){
            count++;
            left--;
            right++;
        }
        return count;
    }
```

Space complexity is $$O(1)$$; Time complexity is $$O(N^2)$$;

## DP Solution

For DP problems with substring/subarray, we usually define DP related with the last chacter. Here, 
- $$dp[i][j]$$ represents if $$s[i:j]$$ is a palindromic string;
- $$dp[i][j] = (s[i]==s[j] && dp[i+1][j-1])$$, if $$j-i\geq2$$; if $$0\leq j-i<2$$, we check if $$s[i]==s[j]$$;

```cpp
int countSubstrings(string s) {
    int N = s.size();
    
    vector<vector<bool>> dp(N, vector<bool>(N, 0));
    // dp[i][j]: if s[i,j] is palindromic string
    
    int count = 0;
    
    for (int l = 0; l < N; ++l) {
        for (int i = 0; i < N - l; ++i) {
            int j = i + l;
            dp[i][j] = 0;
            if ((s[i] == s[j]) && (l < 2 || dp[i+1][j-1]))
                dp[i][j] = 1;
            if(dp[i][j])
                count++;
        }
    }
    
    return count;
}
```

## Notes
- For the expansion method, it could start from the a single charater and expand to the two sides, or from two charaters and expand, so we use `expandString(s,i, i)` and `expandString(s, i, i+1)`;
- For the DP method, follow the typical substring question. Also since there is only one string here, it is either a 1D DP, or a 2D DP with intervals;
