# Day 12:
- 6 Problems

### [Problem 57: Insert Interval](https://leetcode.com/problems/insert-interval/description/)
Hints:
- Since they are sorted by order so we don't need to compare each intervals to each other, only compare to new interval
- There are 3 cases:
    - current interval start > new interval end --> we can add the new interval and the rest of the list to the result and return the result
    - current interval end < new interval begin --> add to the result
    - current interval end > new interval begin --> they overlaps so we need to set new interval start = min(current interval start and new interval start) and new interval end = max(current interval end and new interval end)
- in case the new interval is the end of the list, we need to add it after the loop and return the result

```
def insert(self, intervals: List[List[int]], newInterval: List[int]) -> List[List[int]]:
    res = []
    for i in range(len(intervals)):
        if intervals[i][0] > newInterval[1]:
            res.append(newInterval)
            return res + intervals[i:]
        elif intervals[i][1] < newInterval[0]:
            res.append(intervals[i])
        else:
            newInterval = [min(newInterval[0], intervals[i][0]), max(newInterval[1], intervals[i][1])]
    res.append(newInterval)
    return res
```

### [Problem 56: Merge Intervals](https://leetcode.com/problems/merge-intervals/description/)
Hints:
- Need to sort first
- Add first value to result array
- Use for loop to check starting at 1:
    - keep track of the last value in result array 
    - if start time of last value in result array > end time of current time: append it to result array
    - else it is overlaps so only change the end time of the last value in result array assign it to maximum number between current end time and the end time of last value in result array

```
def merge(self, intervals: List[List[int]]) -> List[List[int]]:
    if len(intervals) == 1:
        return intervals
    intervals.sort()
    res = [intervals[0]]
    
    for i in range(1, len(intervals)):
        last = res[-1]
        if intervals[i][0] > last[1]:
            res.append(intervals[i])
        else:
            last[1] = max(intervals[i][1], last[1])
    return res
```

### [Problem 435: Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/description/)
Hints:
- We have to sort first, either by start time or end time
- Then if we sort by end time then the start time of the current value is always bigger or equal to the end time of the previous value so if this is not the case then we increase result by 1

```
def eraseOverlapIntervals(self, intervals: List[List[int]]) -> int:
    intervals.sort(key=lambda x: x[1])
    removed = 0
    end = intervals[0][1]
    for i in range(1, len(intervals)):
        if intervals[i][0] < end:
            removed += 1
        else:
            end = intervals[i][1]
    
    return removed
```

### [Problem 252: Meeting Rooms](https://leetcode.com/problems/meeting-rooms/description/)
Hints:
- sort it and remember start time of current value will always bigger or equal to end time of previous value

```
def canAttendMeetings(self, intervals: List[Interval]) -> bool:
    intervals.sort(key=lambda i: i.start)
    for i in range(1, len(intervals)):
        s1 = intervals[i-1]
        s2 = intervals[i]
        if s1.end > s2.start:
            return False
    return True
```

### [Problem 253: Meeting Rooms II](https://leetcode.com/problems/meeting-rooms-ii/description/)
Hints:
- do 2 separate sorted start and end time
- have 2 pointers for start and end time
- while loop end when current start index < len(start array)
- intuition is that only count when the start time < end time --> shift start index by 1 --> can have meeting, decrement it when start time > end time --> shift end index by 1
- result is the max of result and count time

```
def minMeetingRooms(self, intervals: List[Interval]) -> int:
    start = sorted([i.start for i in intervals])
    end = sorted([i.end for i in intervals])
    res, count = 0, 0
    s, e = 0, 0
    while s < len(intervals):
        if start[s] < end[e]:
            s += 1
            count += 1
        else:
            e += 1
            count -= 1
        res = max(res, count)
    return res
```