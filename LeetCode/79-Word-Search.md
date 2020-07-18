---
description: 'DFS, Backtracking'
---

# 79. Word Search

[Problem](https://leetcode.com/problems/word-search/)

## DFS

Standard DFS + Backtracking problem. Use one extra `visited` 2D vector to keep track of the places that has been visited.

```cpp
bool exist(vector<vector<char>>& board, string word) {
    int m = board.size();
    int n = board[0].size();
    // keep track of this point has been visited or not
    vector<vector<bool>> visited(m, vector<bool>(n, 0)); 
    for (int i = 0; i < m; ++i) {
        for (int j = 0; j < n; ++j) {
            bool result = dfs(board, word, i, j, 0, visited);
            if (result)
                return true;
        }
    }
    return false;
}

bool dfs(const vector<vector<char>>& board, const string& word, int i, int j, int k, vector<vector<bool>>& visited) {
    if (i < 0 || i >= board.size() || j < 0 || j >= board[0].size() || visited[i][j] || word[k] != board[i][j])
        // out of boundary or has bee visited before or not matched
        return false;
    if (k == word.size() - 1)
        // sucessfully matched
        return true;

    visited[i][j] = 1;
    bool result = dfs(board, word, i + 1, j, k + 1, visited) || dfs(board, word, i - 1, j, k + 1, visited) || dfs(board, word, i, j + 1, k + 1, visited) || dfs(board, word, i, j - 1, k + 1, visited);
    visited[i][j] = 0;

    return result;

}
```

Time complexity: $$O(3^{K}MN)$$. Space complexity: $$O(MN)$$.
For every char there are 3 choices (should be 4, but one has been visited.). In the worst scenario, we have to do the $$3^{K}$$ DFS $$MN$$ times.

## DFS without extra space

we don't really need the `visited` vector if we change the visited `board` content to something used to mark.

```cpp
bool exist(vector<vector<char>>& board, string word) {
    int m = board.size();
    int n = board[0].size();
    for (int i = 0; i < m; ++i) {
        for (int j = 0; j < n; ++j) {
            bool result = dfs(board, word, i, j, 0);
            if (result)
                return true;
        }
    }
    return false;
}

bool dfs(vector<vector<char>>& board, const string& word, int i, int j, int k) {
    if (i < 0 || i >= board.size() || j < 0 || j >= board[0].size() || board[i][j] == '*' || word[k] != board[i][j])
        // out of boundary or has bee visited before or not matched
        return false;
    if (k == word.size() - 1)
        // sucessfully matched
        return true;

    board[i][j] = '*';
    bool result = dfs(board, word, i + 1, j, k + 1) || dfs(board, word, i - 1, j, k + 1) || dfs(board, word, i, j + 1, k + 1) || dfs(board, word, i, j - 1, k + 1);
    board[i][j] = word[k];

    return result;

}
```

Time complexity is the same: $$O(3^{K}MN)$$. Space complexity: $$O(K)$$, since we only need to do deeper into the recursion for K times, each time with one char from the `word`.
