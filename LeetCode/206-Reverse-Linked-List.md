---
description: 'LinkedList'
---

# 206. Reverse Linked List

[Problem](https://leetcode.com/problems/reverse-linked-list/)

## Interative solution 1

Loop from the begining, point `cur->next` to `pre`, and `cur` goes to the original next, until the end. 

```cpp
ListNode* reverseList(ListNode* head) {
    ListNode* pre = NULL;
    ListNode* cur = head;
    
    while (cur) {
        ListNode* temp = cur->next;
        cur->next = pre;
        pre = cur;
        cur = temp;
    }
    
    return pre;
}
```

## Iterative solution 2: in-place revert

For `1->2->3->4->5`, we do it step by step: first change it to `2->1->3->4->5`, then `3->2->1->4->5`, i.e., keep inserting the `cur` to the front. This method also works for Problem 92, where we are asked to revert part of the linkedlist.

```cpp
ListNode* reverseList(ListNode* head) {
    // keep inserting cur to the head position
    // and push the original head to the end
    // 1->2->3->4->5 -----> 
    // 2->1->3->4->5 ----->
    // 3->2->1->4->5 -----> ...
    if (!head || !head->next)
        return head;
    
    ListNode* pseudoN = new ListNode(0, head);
    
    ListNode* pre = head;
    ListNode* cur = head->next;
    while (cur) {
        pre->next = cur->next;
        cur->next = pseudoN->next;
        pseudoN->next = cur;
        cur = pre->next;
    }
    
    head = pseudoN->next;
    delete pseudoN;
    return head;
}
```

## Recursive solution 1
After the `reverseList(head->next)`, `head->next` will be the last element in the linkedlist. we will add the `head` to the next of `head->next`.

```cpp
ListNode* reverseList(ListNode* head) {
    if (!head || !head->next)
        return head;
    
    ListNode* phead = reverseList(head->next); // new head
    head->next->next = head;
    head->next = NULL;
    
    return phead;
}
```

## Recursive in place:

Put the `cur` node to the head place before recursion.

```cpp
ListNode* reverseList(ListNode* head) {
    return reverseList(head, NULL);
}

ListNode* reverseList(ListNode* cur, ListNode* newhead) {
    if (!cur)
        return newhead;
    
    ListNode* temp = cur->next;
    cur->next = newhead;
    return reverseList(temp, cur);
}
```
