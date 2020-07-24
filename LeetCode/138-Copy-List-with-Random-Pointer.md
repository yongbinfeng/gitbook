---
description: 'LinkedList'
---

# 138. Copy List with Random Pointer

[Problem](https://leetcode.com/problems/copy-list-with-random-pointer/)

## Solution

This is a new linked list problem. I can not think of the solution myself. 

The idea is to first copy all the nodes and link it right after the orignal one. For example, the `1->2->3`
becomes `1->1c->2->2c->3->3c`, in which `1c`, `2c`, and `3c` are the copied nodes.

Then link all the `random` links: for the new nodes, the link shoud be `old->next`, since all the new nodes
are right after the orignal nodes.

Finally we break the two entangled linked lists into two.

```cpp
Node* copyRandomList(Node* head) {
    if (!head)
        return NULL;
    
    // copy the nodes
    // 1->2->3 -> 1->1c->2->2c->3->3c;
    Node* p1 = head;
    Node* p2 = NULL;
    while (p1) {
        p2 = new Node(p1->val);
        p2->next = p1->next;
        p1->next = p2;
        p1 = p2->next;
    }
    
    // copy the random links
    p1 = head;
    while (p1) {
        p2 = p1->next;
        if (p1->random) 
            p2->random = p1->random->next;
        p1 = p2->next;
    }
    
    // disentangle the two lists
    Node* newhead = head->next;
    p1 = head;
    while (p1) {
        p2 = p1->next;
        p1->next = p2->next;
        if (p1->next)
            p2->next = p1->next->next;
        p1 = p1->next;
    }
    return newhead;
}
```
