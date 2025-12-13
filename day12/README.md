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