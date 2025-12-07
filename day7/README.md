# Day 7
- 5 Backtracking problems

### [Problem 78: Subsets](https://leetcode.com/problems/subsets/description/)
Hints:
- Add the current subset to the result
- Iterate over the remaining choices
- Choose a number --> go deeper
- Undo the choice (backtrack)
- Try next number

Example: nums = [1,2,3]
--> [1,2,3]
--> [1,2]
--> [1,3]
--> [1]
--> [2,3]
--> [2]
--> [3]
--> []

```
def subsets(self, nums: List[int]) -> List[List[int]]:
    n = len(nums)
    res = []
    def backtrack(arr, i):
        if i == n:
            res.append([*arr])
            return
        
        arr.append(nums[i])
        backtrack(arr, i + 1)
        arr.pop()
        backtrack(arr, i + 1)
    
    backtrack([], 0)

    return res
```

### [Problem 39: Combination Sum](https://leetcode.com/problems/combination-sum/description/)
Hints:
- Since it only has 150 combinations, we can use backtracking
- sum > target or i == n: then return nothing
- sum == target: then add array to resuslt
- append candidates of current index to arr
- then backtracking with the same value
- then pop that value out in array
- then backtracking with the value of next index (i + 1)

```
def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
    n = len(candidates)
    res = []
    def backtrack(arr, sum, i):
        if sum == target:
            res.append([*arr])
            return
        
        if sum > target or i == n:
            return
        
        arr.append(candidates[i])
        backtrack(arr, sum + candidates[i], i)
        arr.pop()
        backtrack(arr, sum, i + 1)

    backtrack([], 0, 0)
    return res
```

### [Problem 40: Combination Sum II](https://leetcode.com/problems/combination-sum-ii/description/)
Hints:
- The same like the Combination Sum 1 but we need to sort the input array
- Index should always be incrementing 1
- Then before backtracking the second time, we use another loop to skip for the duplicates value so that it can be unique

```
def combinationSum2(self, candidates: List[int], target: int) -> List[List[int]]:
    n = len(candidates)
    res = []
    candidates.sort()
    def backtracking(arr, sum, i):
        if sum == target:
            res.append([*arr])
            return
        
        if i == n or sum > target:
            return
        
        arr.append(candidates[i])
        backtracking(arr, sum + candidates[i], i + 1)
        arr.pop()
        while i + 1 < n and candidates[i] == candidates[i+1]: 
            i += 1
        backtracking(arr, sum, i + 1)

    backtracking([], 0, 0)
    return res
```

### [Problem 46: Permutations](https://leetcode.com/problems/permutations/description/)
Hints:
- we use another array to know which number in which index has been used so that every numbers are unique and unused
- every iteration, assign current index of the used array to be True before recursion and after poping out, set it to be false again
- only add number with the index of the used array

```
def permute(self, nums: List[int]) -> List[List[int]]:
    res = []
    n = len(nums)
    used = [False] * n
    def backtracking(arr, i):
        if i == n:
            res.append([*arr])
            return
        
        for idx in range(n):
            if used[idx]:
                continue
            
            used[idx] = True
            arr.append(nums[idx])
            backtracking(arr, i + 1)
            arr.pop()
            used[idx] = False

    backtracking([], 0)
    return res
```

### [Problem 90: Subsets II](https://leetcode.com/problems/subsets-ii/description/)
Hints:
- with duplicates subset, need to sort first
- because it is to filter out duplicates, need to move index by 1 in every recursion to not to repeat
- before starting another recursion, move the index to the index that is not duplicates

```
def subsetsWithDup(self, nums: List[int]) -> List[List[int]]:
    res = []
    n = len(nums)
    nums.sort()

    def backtracking(arr, i):
        if i == n:
            res.append([*arr])
            return
        
        arr.append(nums[i])
        backtracking(arr, i + 1)
        arr.pop()
        while i + 1 < n and nums[i] == nums[i+1]:
            i += 1
        backtracking(arr, i + 1)
    
    backtracking([], 0)

    return res
```