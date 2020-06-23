---
description: 'Dynamic Programming, FSP, House Robber'
---

# 337. House Robber III

[Problem](https://leetcode.com/problems/house-robber-iii/)

## DP Solution

This is an interesting problem, different from the previous house robber problems.
- `dp_0` is the maximum profit without robbing the current node;
- `dp_1` is the maximum profit with robbing the current node;
- we do the same for left and right, and have `dp_left_0`, `dp_left_1`, `dp_right_0`, `dp_right_1`;
- For `dp_1`, since we are going to rob the current node, we can not rob the left and right node anymore, so `dp_1=val+dp_left_0+dp_right_0`;
- For `dp_0`, we are not going to rob the current node, so we could choose whether to rob the left or the right node or not, depending on its value, so we have `dp_0 = max(dp_left_0, dp_left_1) + max(dp_right_0, dp_right_1)`;

```cpp
int rob(TreeNode* root) {
    int dp_0 = 0;
    int dp_1 = 0;
    dfs(root, dp_0, dp_1);
    return max(dp_0, dp_1);
}

void dfs(TreeNode* root, int& dp_0, int& dp_1) {
    // dp_0: maximum profit without robbing the current layer
    // dp_1: maximum profit with robbing the current layer
    
    if (!root)
        return;
    
    int dp_left_0 = 0; // maximum profit without robbing the left node
    int dp_left_1 = 0; // maximum profit with robbing the left node
    dfs(root->left,  dp_left_0, dp_left_1);
    
    int dp_right_0 = 0; // maximum profit without robbing the right node
    int dp_right_1 = 0; // maximum profit with robbing the right node
    dfs(root->right, dp_right_0, dp_right_1);
    
    dp_1 = root->val + dp_left_0 + dp_right_0;
    dp_0 = max(dp_left_0, dp_left_1) + max(dp_right_0, dp_right_1);
    
    return;
}
```
- 
