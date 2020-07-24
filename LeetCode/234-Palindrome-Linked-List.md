---
description: 'LinkedList'
---

# 234. Palindrome Linked List

[Problem](https://leetcode.com/problems/palindrome-linked-list/)

## Stack solution

Use a stack to save the values of the first half, and compare with the second half. Be careful with the case when number of nodes is odd,
in which we have to pop out the last element.

```cpp
bool isPalindrome(ListNode* head) {
    if (!head || !head->next)
        return true;
    
    ListNode* fast = head->next;
    ListNode* slow = head;
    while (fast && fast->next) {
        fast = fast->next->next;
        slow = slow->next;
    }
    // if the linked list has even number of nodes
    bool nnodes_even = true;
    if (!fast)
        nnodes_even = false;
    
    stack<int> s;
    ListNode* p = head;
    while (p != slow->next) {
        s.push(p->val);
        p = p->next;
    }
    if (!nnodes_even)
        s.pop();
    while (p) {
        int val = s.top();
        s.pop();
        if (val != p->val)
            return false;
        p = p->next;
    }
    return true;
}
```

Space complexity is $$O(n)$$.

## Revert the right half

For the $$O(1)$$ space complexity solution, we cut the linkedlist by the middle, revert the second half, and compare the left with
the reverted right.

```cpp
bool isPalindrome(ListNode* head) {
    if (!head || !head->next)
        return true;
    
    ListNode* fast = head->next;
    ListNode* slow = head;
    while (fast && fast->next) {
        fast = fast->next->next;
        slow = slow->next;
    }
    ListNode* right = slow->next;
    slow->next = NULL;  // cut the list into 2
    ListNode* left = head;
    
    // revert the right list
    ListNode* pre = NULL;
    ListNode* cur = right;
    while (cur) {
        ListNode* tmp = cur->next;
        cur->next = pre;
        pre = cur;
        cur = tmp;
    }
    right = pre;
    
    // compare the left with the reverted right
    while (right) {
        if (left->val != right->val) 
            return false;
        left = left->next;
        right = right->next;
    }
    return true;
}
```
