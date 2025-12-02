# Day 2

- 5 Linked List questions

## Linked List part 1:

### [Problem 206: Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/description/)

Hints:
- use dummy = None
- curr = head
- iterate through curr
- assign temp = [curr.next](http://curr.next) for continue iteration
- [curr.next](http://curr.next) = dummy for connection
- dummy = curr for reverse
- curr = temp for iteration

```
def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
    prev = head
    dummy = None
    while prev:
        temp = prev.next
        prev.next = dummy
        dummy = prev
        prev = temp
    return dummy
```

### [Problem 21: Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/description/)

Hints:
- create a dummy node with null value
- curr = dummy
- use while loop with both list1 and list2
    - if list1.val < list2.val: curr.next = list1 and then iterate list1
    - else curr.next = list2 and then iterate list2
    - iterate curr
- if list1 is still existing then use curr.next = list1
- if list2 is still existing then use curr.next = list2
- return dummy.next

```
def mergeTwoLists(self, list1: Optional[ListNode], list2: Optional[ListNode]) -> Optional[ListNode]:
    dummy = ListNode()
    curr = dummy

    while list1 and list2:
        if list1.val < list2.val:
            curr.next = list1
            list1 = list1.next
        else:
            curr.next = list2
            list2 = list2.next
        curr = curr.next
    
    if list1:
        curr.next = list1
    else:
        curr.next = list2
    return dummy.next
```

### [Problem 141: Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/description/)

Hints:
- Use turtle and rabbit approach
- slow = head, fast = head
- while fast and fast.next:
    - slow = slow.next
    - fast = fast.next.next
    - if slow == fast:
        - return it has cycle
- otherwise return false

```
def hasCycle(self, head: Optional[ListNode]) -> bool:
    slow, fast = head, head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow == fast:
            return True
    return False
```

### [Problem 143: Reorder List](https://leetcode.com/problems/reorder-list/description/)

Hints:
- Find the middle point of the list using turtle and rabbit method
- set the next point of middle point to null
- reverse the second half
- while loop iterate using second half: 
    - temp1, temp2 = first.next, second.next
    - first.next = second
    - second.next = temp1
    - first, second = temp1, temp2

--> Example:
```
First half:
1 → 2 → 3

Reversed second half:
6 → 5 → 4

Iteration 1:

first = 1

second = 6

After merge step:
1 → 6 → 2

Pointers move to:

first = 2

second = 5
```


```
def reorderList(self, head: Optional[ListNode]) -> None:
    """
    Do not return anything, modify head in-place instead.
    """
    slow, fast = head, head.next
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next

    # middle node
    second = slow.next
    prev = slow.next = None
    # reverse the nodes
    while second:
        temp = second.next
        second.next = prev
        prev = second
        second = temp
    
    first, second = head, prev

    while second:
        temp1, temp2 = first.next, second.next
        first.next = second
        second.next = temp1
        first, second = temp1, temp2
```

### [Problem 19: Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/description/)

Hints:
- create dummy node
- left node is dummy node
- right node is at head
- while n > 0: move the right node till it reaches n = 0
- while there is right node:
    - move left by 1
    - move right by 1
- the node needs to be deleted is before one node so left.next = left.next.next

--> Why this algorithm works:
because right = length - n 
left = length - current right index - 1 so that is why it always ended up before the node needs to be deleted by 1

```
def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
    dummy = ListNode(0, head)
    left = dummy
    right = head

    while n > 0:
        right = right.next
        n -= 1

    while right:
        left = left.next
        right = right.next

    left.next = left.next.next
    return dummy.next
```