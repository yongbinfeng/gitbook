---
description: 'LinkedList, Sort, MergeSort'
---

# 148. Sort List

[Problem](https://leetcode.com/problems/sort-list/)

## Recursive solution

Merge sort

```cpp
ListNode* sortList(ListNode* head) {
    if (!head || !head->next)
        return head;
    
    ListNode* fast = head;
    ListNode* slow = head;
    ListNode* pre = NULL;
    while (fast && fast->next) {
        fast = fast->next->next;
        pre = slow;
        slow = slow->next;
    }
    pre->next = NULL;  // important
    
    head = sortList(head);
    slow = sortList(slow);
    head = mergeList(head, slow);
    return head;
}

ListNode* mergeList(ListNode* p1, ListNode* p2) {
    if (!p1)
        return p2;
    if (!p2)
        return p1;
    
    if (p1->val <= p2->val) {
        p1->next = mergeList(p1->next, p2);
        return p1;
    } else {
        p2->next = mergeList(p1, p2->next);
        return p2;
    }
}
```

## Iterative solution

```cpp
ListNode* sortList(ListNode* head) {
    ListNode* cur = head;
    int length = 0;
    while (cur) {
        cur = cur->next;
        ++length;
    }
    
    ListNode* pseudoN = new ListNode(0, head);
    for (int step = 1; step < length; step = step * 2) {
        ListNode* tail = pseudoN;
        ListNode* left = pseudoN->next;
        while (left) {
            ListNode* right = split(left, step);
            ListNode* nextleft = split(right, step);
            tail = mergeList(left, right, tail);
            left = nextleft;
        }
    }
    
    head = pseudoN->next;
    delete pseudoN;
    return head;
}

// cut the LinkedList at steps, and return the next LinkedList
ListNode* split(ListNode* p, int step) {
    int move = 1;
    while (p && move < step) {
        p = p->next;
        ++move;
    }
    if (!p) 
        return NULL;
    
    ListNode* pnext = p->next;
    p->next = NULL;
    return pnext;
}

// merge the sorted left and right linkedlist into tail,
// and return the new tail
ListNode* mergeList(ListNode* left, ListNode* right, ListNode* head) {
    ListNode* p = head;
    while (left && right) {
        if (left->val <= right->val) {
            p->next = left;
            left = left->next;
        } else {
            p->next = right;
            right = right->next;
        }
        p = p->next;
    }
    p->next = left ? left : right;
    while (p->next) {
        p = p->next;
    }
    return p;
}
```
