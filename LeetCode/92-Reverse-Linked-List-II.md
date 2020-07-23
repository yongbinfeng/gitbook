---
description: 'LinkedList'
---

# 92. Reverse Linked List II

[Problem](https://leetcode.com/problems/reverse-linked-list-ii/)

## Iterative solution

It might be better to solve Problem 206 first before this one. We need to do the in-place reverting.

Use the `front` pointer pointing to the `m-1` node, and `pre` points to `m` node. During the reversion, `cur` node should always be placed
right after the `front` node, and `pre` node should not change, while kept being moved to the end.

```cpp
ListNode* reverseBetween(ListNode* head, int m, int n) {
    if (m == n)
        return head;
    
    ListNode* pseudoN = new ListNode(0, head);
    ListNode* cur = pseudoN;
    
    for (int i = 0; i < m - 1; ++i)
        cur = cur->next;
    ListNode* front = cur;  // the node right before the reversion starts
    ListNode* pre = front->next;  // first node during the reversion, which will be moved to m+n eventually
    cur = pre->next;
    
    for (int i = 0; i < n - m; ++i) {
        pre->next = cur->next;
        cur->next = front->next;
        front->next = cur;
        cur = pre->next;
    }
    
    head = pseudoN->next;
    delete pseudoN;
    return head;
    
}
```

It is kind of weird to write the recursive solution for this problem. I'll skip.
