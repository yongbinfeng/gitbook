---
description: 'LinkedList'
---

# 203. Remove Linked List Elements

[Problem](https://leetcode.com/problems/remove-linked-list-elements/)

## Solution

Use a pseudo node to point to `head`, such that we don't have to deal with the special case where
the `head` needs to be deleted. (Don't forget to `delete` the pseudo node in the end.)

```cpp
ListNode* removeElements(ListNode* head, int val) {
    ListNode* pseudoN = new ListNode(0, head);
    ListNode* p = pseudoN;
    
    while (p) {
        if (p->next && p->next->val == val) {
            ListNode* tmp = p->next;
            p->next = p->next->next;
            delete tmp;
        } else {
            p = p->next;
        }
    }
    
    p = pseudoN->next;
    delete pseudoN;
    return p;
}
```

## Notes
- `delete` all the deleted nodes;
- if one node is deleted, we do **NOT** do the `p=p->next` operation
