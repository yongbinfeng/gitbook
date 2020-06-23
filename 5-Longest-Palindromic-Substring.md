---
description: 'Dynamic Programming, String, Substring, Interval'
---

# 5. Longest Palindromic Substring

[Problem](https://leetcode.com/problems/longest-palindromic-substring)

## Expansion Solution

This problem is mutant from [Problem 647. Palindromic Substrings](https://leetcode.com/problems/palindromic-substrings/), where
in that case it is counting the number of palindromic substrings. Here we are looking for the longest substring. The idea is the same,
we find all the palindromic substrings, and store the longest substring.

```cpp
string longestPalindrome(string s) {
    int N = s.size();
    int maxL = 0; 
    int sfront = 0;
    
    for (int i = 0; i < N; ++i) {
        expandString(s, i, i, maxL, sfront);
        expandString(s, i, i + 1, maxL, sfront);
    }
            
    return maxL > 0 ? s.substr(sfront, maxL) : "";
    
}

void expandString(const string& s, int i, int j, int& maxL, int& sfront){
    while (i >= 0 && j < s.size() && s[i] == s[j]){
        --i;
        ++j;
    }
    if ((j - i - 1) > maxL){
        maxL = (j - i - 1);
        sfront = i + 1;
    }
}
```

## DP Solution

```cpp
string longestPalindrome(string s) {
    int N = s.size();
    
    vector<vector<bool>> dp(N, vector<bool>(N, 0));
    // if dp[i][j] is a palindromic string
    
    int maxL = 0;
    int sfront = 0;
    
    for (int l = 0; l < N; ++l) {
        for (int i = 0; i < N - l; ++i) {
            int j = i + l;
            dp[i][j] = (s[i] == s[j] && (l<2 || dp[i + 1][j - 1]));
            
            if (dp[i][j] && (l + 1 > maxL)) {
                maxL = l + 1;
                sfront = i;
            }
        }
    }
    
    return maxL > 0 ? s.substr(sfront, maxL) : "";
}
```

## Notes
- The idea for solving this problem, is almost the same as problem 647, and both the expansion and dp method works;
- Might be interesting to cross compare Problem 647, 5, and 516. In Problem 516, where it looks for the length of the longest palindromic subsequence, it is more similar to a LCS problem;
