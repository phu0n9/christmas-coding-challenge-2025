# Day 11:

### [Problem 703: Kth Largest Element in a Stream](https://leetcode.com/problems/kth-largest-element-in-a-stream/description/)
Hints:
- We use heap to do this
- in init, we just add it to the heap and if it exceed k then we pop out
- in add we just add to the heap and if it exceed we pop out and return first element in heap --> smallest number

```
class KthLargest:

    def __init__(self, k: int, nums: List[int]):
        self.k = k
        self.heap = nums
        heapq.heapify(self.heap)
        while len(self.heap) > k:
            heapq.heappop(self.heap)

    def add(self, val: int) -> int:
        heapq.heappush(self.heap, val)
        if len(self.heap) > self.k:
            heapq.heappop(self.heap)
        
        return self.heap[0]
```

### [Problem 1046: Last Stone Weight](https://leetcode.com/problems/last-stone-weight/description/)
Hints:
- Use max heap
- Mashing it by poping it out of heap and then add the difference to the heap
- Return the smallest value

```
def lastStoneWeight(self, stones: List[int]) -> int:
    heap = [-stone for stone in stones]
    heapq.heapify(heap)

    while len(heap) > 1:
        x = -heapq.heappop(heap)
        y = -heapq.heappop(heap)
        if x != y:
            heapq.heappush(heap, -abs(y-x))
    
    return -heap[0] if heap else 0
```