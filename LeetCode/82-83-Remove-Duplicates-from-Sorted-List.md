---
description: 'LinkedList'
---

# 83. Remove Duplicates from Sorted List

[Problem](https://leetcode.com/problems/remove-duplicates-from-sorted-list/)

## Iterative solution

Loop from `head`, check `cur->val` with `cur->next->val`, if equal, set `cur->next = cur->next->next`, until the end;

```cpp
ListNode* deleteDuplicates(ListNode* head) {
    ListNode* cur = head;
    
    while (cur && cur->next) {
        if (cur->val == cur->next->val) {
            cur->next = cur->next->next;
        } else {
            cur = cur->next;
        }
    }
    
    return head;
}
```

## Recursive solution

Think of the linklist starting from `cur`, and all the duplicates have been removed; we check if the `head` has the same value as `cur`, if the same, 
we skip `head`.

```cpp
ListNode* deleteDuplicates(ListNode* head) {
    if (!head || !head->next) 
        return head;

    if (head && head->next && head->val == head->next->val) 
        head = deleteDuplicates(head->next);
    else
        head->next = deleteDuplicates(head->next);
    return head;
}
```

# 82. Remove Duplicates from Sorted List II

[Problem](https://leetcode.com/problems/remove-duplicates-from-sorted-list-ii/)

## Iterative solution

Here it requires if a number is duplicated, all the nodes with that number should be removed. 
- Since the `head` node could also be removed, we create a `pseudoN` pointed to the `head`, such that we don't have to deal with the special case where the `head` is removed;
- Check the val of `cur` and `cur->next`, if the same, keep moving `cur` until it is different, all the nodes between `pre` and `cur->next` should be removed, by `pre->next = cur->next`;

```cpp
ListNode* deleteDuplicates(ListNode* head) {
    ListNode* pseudoN = new ListNode(0, head);
    ListNode* pre = pseudoN;
    ListNode* cur = head;
    
    while (cur && cur->next) {
        bool isduplicate = false;
        while (cur && cur->next && cur->val == cur->next->val) {
            cur = cur->next;
            isduplicate = true;
        }
        if (isduplicate) 
            pre->next = cur->next;
        else
            pre = pre->next;
        cur = cur->next;
    }
    
    head = pseudoN->next;
    delete pseudoN;
    return head;
}
```

## Recursive solution

If `cur->val` is duplicated, we skip all the `cur->val`s until the one that is not duplicate, and then we start recursive.

```cpp
ListNode* deleteDuplicates(ListNode* head) {
    if (!head || !head->next)
        return head;
    
    bool isduplicate = false;
    while (head && head->next && head->val == head->next->val) {
        isduplicate = true;
        head = head->next;
    }
    
    if (isduplicate)
        head = deleteDuplicates(head->next);
    else
        head->next = deleteDuplicates(head->next);
    return head;
}
```
