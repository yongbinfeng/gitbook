---
description: 'LinkedList'
---

# 21. Merge Two Sorted Lists

[Problem](https://leetcode.com/problems/merge-two-sorted-lists/)

## Iterative solution

```cpp
ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
    ListNode *pseudoN = new ListNode(0);
    ListNode *p = pseudoN;
    
    while (l1 && l2) {
        if (l1->val <= l2->val) {
            p->next = l1;
            l1 = l1->next;
        } else {
            p->next = l2;
            l2 = l2->next;
        }
        p = p->next;
    }
    p->next = l1 ? l1 : l2;  // remaining
    
    p = pseudoN->next;
    delete pseudoN;
    return p;
}
```

## Recursive solution

```cpp
ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
    if (!l1 || !l2)
        return l1 ? l1 : l2;
    
    if (l1->val <= l2->val) {
        l1->next = mergeTwoLists(l1->next, l2);
        return l1;
    } else {
        l2->next = mergeTwoLists(l1, l2->next);
        return l2;
    }
}
```
