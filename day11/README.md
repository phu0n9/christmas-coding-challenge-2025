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

### [Problem 973: K Closest Points to Origin](https://leetcode.com/problems/k-closest-points-to-origin/description/)
Hints:
- Use max heap to do it
- pop out when it reaches limit more than k

```
def kClosest(self, points: List[List[int]], k: int) -> List[List[int]]:
    heap = []

    for x, y in points:
        distance = x*x + y*y
        heapq.heappush(heap, (-distance, [x, y]))
        if len(heap) > k:
            heapq.heappop(heap)
    
    return [item[1] for item in heap]
```

### [Problem 215: Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/description/)
Hints:
- Use min heap
- The top of min heap is the k-th largest number

```
def findKthLargest(self, nums: List[int], k: int) -> int:
    heap = nums
    heapq.heapify(heap)
    while len(heap) > k:
        heapq.heappop(heap)
    
    return heap[0]
```

### [Problem 621: Task Scheduler](https://leetcode.com/problems/task-scheduler/description/)
Hints:
- This one the intuition is to keep track of the current time + cool down time
- And we have to do in such a way that we need to do the tasks that has the most appearance first so that we can pair it to another
- So we need to use max heap and queue to it
- if the current time == first element of time in the queue then we can push it back to the heap
- keep increasing the time until queue or heap is empty

```
def leastInterval(self, tasks: List[str], n: int) -> int:
    count = Counter(tasks)
    heap = []
    heap = [-cnt for cnt in count.values()]
    heapq.heapify(heap)

    queue = collections.deque()
    time = 0
    while heap or queue:
        time += 1
        if heap:
            cnt = 1 + heapq.heappop(heap)
            if cnt:
                queue.append([cnt, time + n])
            
        if queue and queue[0][1] == time:
            heapq.heappush(heap, queue.popleft()[0])
    return time
```

### [Problem 295: Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/description/)
Hints:
- have 2 heaps: min heap and max heap
- we should always keep numbers in min heap always smaller than max heap and try to balance the numbers of both heaps
- add number:
    - keep adding number to min heap
    - if there are min and max heap and first number of min heap > first number of max heap:
        - pop it out of min heap and then add it to max heap
    - if len of min heap > len max heap: pop it out of min heap and add it to max heap
    - if len of max heap > len min heap: pop it out of max heap and add it to min heap
- find median:
    - if len min heap > len max heap: return first number of min heap
    - if len max heap > len min heap: return first number of max heap
    - if len max heap == len min heap: return average of numbers from both heap

```
class MedianFinder:

    def __init__(self):
        self.small = []
        self.large = []

    def addNum(self, num: int) -> None:
        heapq.heappush(self.small, -1 * num)

        if self.small and self.large and -1 * self.small[0] > self.large[0]:
            val = heapq.heappop(self.small)
            heapq.heappush(self.large, -1 * val)
        
        if len(self.small) > len(self.large) + 1:
            val = heapq.heappop(self.small)
            heapq.heappush(self.large, -1 * val)
        
        if len(self.large) > len(self.small) + 1:
            val = heapq.heappop(self.large)
            heapq.heappush(self.small, -1 * val)

    def findMedian(self) -> float:
        if len(self.small) > len(self.large):
            return self.small[0] * -1
        if len(self.small) < len(self.large):
            return self.large[0]
        return ((-1 * self.small[0]) + self.large[0]) / 2
```

### [Problem 355: Design Twitter](https://leetcode.com/problems/design-twitter/description/)
Hints:
- use defalut dict of hashset for followers and default dict of list for tweets
- Add the time for max heap to distinguish
- get news feed: 
    - remember to add currnet userId to followers set before the loop
    - only adding the latest tweet of each users in the max heap and store the latest index of the newest tweet corespondingly to the latest tweet of that user in the max heap (-time, uid, tweet_id, idx)
    - second for loop is to add the latest tweet to res and then add if the current index > 0 then add that user older tweet to the max heap

```
class Twitter:

    def __init__(self):
        self.follows = defaultdict(set)
        self.tweets = defaultdict(list)
        self.time = 0

    def postTweet(self, userId: int, tweetId: int) -> None:
        self.time += 1
        self.tweets[userId].append((self.time, tweetId))

    def getNewsFeed(self, userId: int) -> List[int]:
        res = []
        max_heap = []
        self.follows[userId].add(userId)

        for uid in self.follows[userId]:
            if self.tweets[uid]:
                time, tweet_id = self.tweets[uid][-1]
                idx = len(self.tweets[uid]) - 1
                heapq.heappush(max_heap, (-time, uid, tweet_id, idx))
        
        while len(res) < 10 and max_heap:
            _, uid, tweet_id, idx = heapq.heappop(max_heap)
            res.append(tweet_id)

            if idx > 0:
                time, tweet_id = self.tweets[uid][idx - 1]
                heapq.heappush(max_heap, (-time, uid, tweet_id, idx - 1))

        return res

    def follow(self, followerId: int, followeeId: int) -> None:
        self.follows[followerId].add(followeeId)

    def unfollow(self, followerId: int, followeeId: int) -> None:
        if followeeId in self.follows[followerId]:
            self.follows[followerId].remove(followeeId)
```