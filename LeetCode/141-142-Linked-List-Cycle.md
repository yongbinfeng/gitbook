---
description: 'LinkedList'
---

# 141. Linked List Cycle

[Problem](https://leetcode.com/problems/linked-list-cycle/)

## Two pointer solution

Use two pointers: `fast` and `slow`. Every time `fast` moves two steps and `slow` moves one step. If there
is a cycle, eventually they will meet: `fast==slow`. Otherwise `fast` will arrive at the end first.

```cpp
bool hasCycle(ListNode *head) {
    ListNode* fast = head;
    ListNode* slow = head;
    
    while (fast && fast->next) {
        fast = fast->next->next;
        slow = slow->next;
        if (fast == slow)
            return true;
    }
    
    return false;
}
```

# 142. Linked List Cycle II

[Problem](https://leetcode.com/problems/linked-list-cycle-ii/)

## Two pointer solution

When `fast` and `slow` meet, `slow` moves `k` steps, `fast` moves `2k` steps: `2k-k=nr`, where `n` is 
the integer, `r` is the size of the loop.

Suppose the distance from the `head` to the begin of the loop is `m`, the distance from the begin of the 
loop to the meeting node is `x`, then we have `k=x+m+n2r`. So we know

`x+m=n1r`

and set the other half of the distance between the meeting node to the begin of the loop is `y`, we have `x+y=r`.
So,

`m=y+n3r`.

**Once `fast` and `slow` meet, we set `slow` back to `head`, and both `fast` and `slow` move step by step, they will meet
at the begining of the loop.** (`fast` travels `y+n4r`, `slow` travels `m+n5r`)

```cpp
ListNode *detectCycle(ListNode *head) {
    ListNode* fast = head;
    ListNode* slow = head;
    
    while (fast && fast->next) {
        fast = fast->next->next;
        slow = slow->next;
        if (fast == slow) {
            // there is a cycle
            slow = head;
            while (fast != slow ) {
                fast = fast->next;
                slow = slow->next;
            }
            return fast;
        }
    }
    
    return NULL;
}
```
