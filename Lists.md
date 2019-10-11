# Linked Lists

+ [Remove Nth Node From End of List](#remove-nth-node-from-end-of-list)
+ [Merge Two Sorted Lists](#merge-two-sorted-lists)
+ [Linked List Cycle](#linked-list-cycle)
+ [Linked List Cycle II](#linked-list-cycle-ii)
+ [Reorder List](#reorder-list)
+ [Sort List](#sort-list)
+ [Remove Linked List Elements](#remove-linked-list-elements)
+ [Reverse Linked List](#reverse-linked-list)
+ [Palindrome Linked List](#palindrome-linked-list)
+ [Delete Node in a Linked List](#delete-node-in-a-linked-list)
+ [Middle of the Linked List](#middle-of-the-linked-list)
+ [Intersection of Two Linked Lists](#intersection-of-two-linked-lists)

## Remove Nth Node From End of List

https://leetcode.com/problems/remove-nth-node-from-end-of-list/

```C++
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

## Merge Two Sorted Lists

https://leetcode.com/problems/merge-two-sorted-lists/

```C++
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
      ListNode tmp(0), *addTo = &tmp;
      while (l1 != NULL && l2 != NULL) {
        if (l1->val < l2->val) {
          addTo->next = l1;
          l1 = l1->next;
        } else {
          addTo->next = l2;
          l2 = l2->next;
        }
        addTo = addTo->next;
      }
      if (l1 != NULL) {
        addTo->next = l1;
      } else {
        addTo->next = l2;
      }
      return tmp.next;
    }
};
```

## Linked List Cycle

https://leetcode.com/problems/linked-list-cycle/

```C++
class Solution {
public:
    bool hasCycle(ListNode *head) {
        ListNode* fastPtr, *slowPtr = head;
        if (!head)
            return false;
        fastPtr = head->next;
        while (slowPtr != fastPtr) {
          if (fastPtr == NULL || fastPtr->next == NULL)
            return false;
          slowPtr = slowPtr->next;
          fastPtr = fastPtr->next->next;
        }
        return true;
    }
};
```

## Linked List Cycle II

https://leetcode.com/problems/linked-list-cycle-ii/

```C++
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode* fastPtr, *slowPtr = head;
        if (!head)
            return NULL;
        fastPtr = head->next;
        while (slowPtr != fastPtr) {
          if (fastPtr == NULL || fastPtr->next == NULL)
            return NULL;
          slowPtr = slowPtr->next;
          fastPtr = fastPtr->next->next;
        }
      slowPtr = head;
      fastPtr = fastPtr->next;
      while (slowPtr != fastPtr) {
        slowPtr = slowPtr->next;
        fastPtr = fastPtr->next;
      }
      return slowPtr;
    }
};
```

## Reorder List

https://leetcode.com/problems/reorder-list/

### Iteratively

```C++
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
      ListNode* prev = NULL,*buf;
      while (head) { 
        buf = head->next;
        head->next = prev;
        prev = head;
        head = buf;  
      }
      return prev;
    }
   ListNode* middleNode(ListNode* head) {
        ListNode* fastPtr = head->next;
        while (fastPtr) {
          head = head->next;    
          if (fastPtr->next)
            fastPtr = fastPtr->next->next;
          else
            break;        
        }
      return head;
    }
  void reorderList(ListNode* head) {
    ListNode* buf, *tmp;
    if (head != NULL) {
      buf = reverseList(middleNode(head));
      while ((tmp = buf->next) != NULL) {
        buf->next = head->next;
        head->next = buf;
        head = buf->next;
        buf = tmp;
      }
    }
  }
};
```

### Recursively

```C++
class Solution {
public:
  void RecursiveReorder(ListNode* currNode, ListNode** addTo) {
    if (currNode->next != NULL)
      RecursiveReorder(currNode->next, addTo);
    if (*addTo != NULL) {     
      currNode->next = (*addTo)->next;
      (*addTo)->next = currNode;
      *addTo = currNode->next;
      if (*addTo == currNode) {  //Дошли до середины 
        (*addTo)->next = NULL;
        *addTo = NULL;  //Зануляем, чтобы прекратить добавление
      }
    }      
  }
  void reorderList(ListNode* head) {
    ListNode* addTo = head;
    if (head != NULL)
      RecursiveReorder(head, &addTo);
  }
};
```

## Sort List

https://leetcode.com/problems/sort-list/

```C++
class Solution {
public:
    ListNode* middleNode(ListNode* head) {
        ListNode* fastPtr = head->next;
        while (fastPtr) {              
          if (fastPtr->next)
            fastPtr = fastPtr->next->next;
          else
            break; 
          head = head->next;
        }
      fastPtr = head->next;
      head->next = NULL;
      return fastPtr;
    }
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        ListNode tmp(0), *addTo = &tmp;
      while (l1 != NULL && l2 != NULL) {
        if (l1->val < l2->val) {
          addTo->next = l1;
          l1 = l1->next;
        } else {
          addTo->next = l2;
          l2 = l2->next;
        }
        addTo = ptr->next;
      }
      if (l1 != NULL) {
        addTo->next = l1;
      } else {
        addTo->next = l2;
      }
      return tmp.next;
    }
    ListNode* sortList(ListNode* head) {
        if (head == NULL)
          return head;
        if (head->next == NULL)
          return head;
        return mergeTwoLists(sortList(head), sortList(middleNode(head)));
    }
};
```

## Remove Linked List Elements

https://leetcode.com/problems/remove-linked-list-elements/

```C++
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        ListNode tmp(0), *toDelete = &tmp;
        ListNode* buf;
        tmp.next = head;
         while (toDelete->next) {
          if (toDelete->next->val == val) {
            buf = toDelete->next->next;
            delete toDelete->next;
            toDelete->next = buf;
          }
           else
             toDelete = toDelete->next;
          }  
        return tmp.next;
    }
};
```

## Reverse Linked List

https://leetcode.com/problems/reverse-linked-list/

```C++
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
      ListNode* prev = NULL,*buf;
      while (head) { 
        buf = head->next;
        head->next = prev;
        prev = head;
        head = buf;  
      }
      return prev;
    }
};
```
## Palindrome Linked List

https://leetcode.com/problems/palindrome-linked-list/

```C++
class Solution {
public:
  void RecursiveReorder(ListNode* currNode, ListNode** addTo) {
    if (currNode->next != NULL)
      RecursiveReorder(currNode->next, addTo);
    if (*addTo != NULL) {     
      currNode->next = (*addTo)->next;
      (*addTo)->next = currNode;
      *addTo = currNode->next;
      if (*addTo == currNode) {  //Дошли до середины 
        (*addTo)->next = NULL;
        *addTo = NULL;  //Зануляем, чтобы прекратить добавление
      }
    }      
  }
  bool isPalindrome(ListNode* head) {
    ListNode* addTo = head;
    if (head == NULL)
      return true;    
    RecursiveReorder(head, &addTo);
    do {
    if (head->next == NULL)
      return true;
    else if (head->val != head->next->val)
      return false;
    } while ((head = head->next->next) != NULL);
    return true;
  }     
};
```

## Delete Node in a Linked List

https://leetcode.com/problems/delete-node-in-a-linked-list/

```C++
class Solution {
public:
    void deleteNode(ListNode* node) {
        ListNode* buf;
        node->val = node->next->val;
        buf = node->next;
        node->next = node->next->next;
        delete buf;
    }
};
```

## Middle of the Linked List

https://leetcode.com/problems/middle-of-the-linked-list/

```C++
class Solution {
public:
    ListNode* middleNode(ListNode* head) {
        ListNode* fastPtr = head->next;
        while (fastPtr) {
          head = head->next;    
          if (fastPtr->next)
            fastPtr = fastPtr->next->next;
          else
            break;        
        }
      return head;
    }
};
```

## Intersection of Two Linked Lists

https://leetcode.com/problems/intersection-of-two-linked-lists/

```C++
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        ListNode* buf1 = headA,* buf2 = headB;
        ListNode* lastNode = NULL;
        if (buf1 == NULL || buf2 == NULL)
          return NULL;
        while (buf1 != buf2) {
          if (buf1->next == NULL) {
            if (lastNode) {
              if (buf1 != lastNode)
                return NULL;
            } else
              lastNode = buf1;
            buf1 = headB;
          } else
            buf1 = buf1->next;
          if (buf2->next == NULL) {
            if (lastNode) {
              if (buf2 != lastNode)
                return NULL;
            } else
              lastNode = buf2;
            buf2 = headA;
          } else             
              buf2 = buf2->next;
        }        
        return buf1;
    }
};
```
