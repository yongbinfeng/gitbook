---
description: 'LinkedList'
---

# 86. Partition List

[Problem](https://leetcode.com/problems/partition-list/)

## Solution

We create two linked lists: one is to save the ones below `x`, the other for equal and above `x`. And merge the two lists together afterwards.

```cpp
ListNode* partition(ListNode* head, int x) {
    ListNode pseudoSmall = ListNode(0);
    ListNode pseudoBig = ListNode(0);
    
    ListNode* psmall = &pseudoSmall;
    ListNode* pbig = &pseudoBig;
    ListNode* p = head;
    
    while (p) {
        if (p->val < x) {
            psmall->next = p;
            psmall = psmall->next;
        } else {
            pbig->next = p;
            pbig = pbig->next;
        }
        p = p->next;
    }
    
    pbig->next = NULL;
    psmall->next = pseudoBig.next;
    p = pseudoSmall.next;
    return p;
    
}
```

## Notes
Do NOT forget to set `pbig->next = NULL` in the end.
