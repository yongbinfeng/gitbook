---
description: 'LinkedList'
---

# 143. Reorder List

[Problem](https://leetcode.com/problems/reorder-list/)

## Iterative solution

Three procedures to solve this problem:
- find the mid node, break the LinkedList into two from the mid, `left` and `right`;
- **revert** the `right`;
- **merge** the `left` with the `right`;

```cpp
void reorderList(ListNode* head) {
    if (!head || !head->next)
        return;
    
    // break the list by half
    // revert the right list
    // and insert it to the left
    ListNode* fast = head;
    ListNode* slow = head;
    while (fast && fast->next) {
        fast = fast->next->next;
        slow = slow->next;
    }
    ListNode* right = slow->next;
    slow->next = NULL;
    
    // invert the right list
    ListNode* pre = NULL;
    ListNode* cur = right;
    while (cur) {
        ListNode* tmp = cur->next;
        cur->next = pre;
        pre = cur;
        cur = tmp;
    }
    right = pre;
    
    // insert the right into the left
    ListNode* pseudoN = new ListNode(0);
    ListNode* p = pseudoN;
    ListNode* left = head;
    int count = 0;
    while (left && right) {
        if (count % 2 == 0) {
            p->next = left;
            left = left->next;
        } else {
            p->next = right;
            right = right->next;
        }
        p = p->next;
        ++count;
    }
    p->next = left ? left : right;
    return ;
}
```
