# Day 3

- 2 Linked Lists

## Linked List part 2

### [Problem 2: Add Two Numbers](https://leetcode.com/problems/add-two-numbers/description/)
Hints:
- Since it is from left to right, we can just add and remember to add remaining
- Create a new dummy node:
    - loop can only exist when l1 OR l2 OR remaining is not 0 or not None
    - curr.next = new Node
    - curr = curr.next
    - only iterate if one of the l1 or l2 has value
    - if l1 or l2 is None then value is 0

```
def addTwoNumbers(self, l1: Optional[ListNode], l2: Optional[ListNode]) -> Optional[ListNode]:
    remaining = 0
    dummy = ListNode()
    curr = dummy

    while l1 or l2 or remaining:
        v1 = l1.val if l1 else 0
        v2 = l2.val if l2 else 0

        total = v1 + v2 + remaining
        res = total % 10
        remaining = total // 10

        curr.next = ListNode(res)
        curr = curr.next

        if l1: l1 = l1.next
        if l2: l2 = l2.next
    
    return dummy.next
```

### [Problem 287: Find the Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number/description/)

Hints:
- Since the constrant is `n+1` values and it ranges from `1, n`
- while loop without condition We can use turtle and rabbit algorithm to detect the cycle: slow = nums[slow], fast = nums[nums[fast]], if it is the same then we break --> detect the cycle and the number is in the cycle
- while loop without condition, use either one of index and then use different index = 0, do it again and return the number again
- `slow` starts from the meeting point inside the cycle
- `slow2` starts from the beginning of the array

```
def findDuplicate(self, nums: List[int]) -> int:
    slow = fast = 0
    while True:
        slow = nums[slow]
        fast = nums[nums[fast]]
        if slow == fast:
            break
    
    slow2 = 0
    while True:
        slow = nums[slow]
        slow2 = nums[slow2]
        if slow == slow2:
            return slow
```
