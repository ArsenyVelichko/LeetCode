# Lists

+ [Remove Nth Node From End of List](#remove-nth-node-from-end-of-list)

## Remove Nth Node From End of List

https://leetcode.com/problems/remove-nth-node-from-end-of-list/

```cpp
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode tmp(0), *toDelete = &tmp;
        ListNode* endOfList = head;
        ListNode* buf;
        toDelete->next = head;
        while (n--)
          endOfList = endOfList->next;
        while (endOfList) {
          endOfList = endOfList->next;
          toDelete = toDelete->next;
        }
        buf = toDelete->next->next;
        delete toDelete->next;
        toDelete->next = buf;
        return tmp.next;
    }
};
```
