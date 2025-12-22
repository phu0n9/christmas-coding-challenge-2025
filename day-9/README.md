# Day 9

- 6 Medium problems

### [Problem 200: Number of Islands](https://leetcode.com/problems/number-of-islands/description/)
Hints:
- use dfs and no need to revisit the positions based on setting to '0'
- only use dfs when it is current grid position is '1'

```
def numIslands(self, grid: List[List[str]]) -> int:
    m, n = len(grid), len(grid[0])
    res = 0

    def dfs(i, j):
        if i < 0 or j < 0 or i >= m or j >= n or grid[i][j] == '0':
            return
        
        grid[i][j] = '0'
        dfs(i + 1, j)
        dfs(i - 1, j)
        dfs(i, j + 1)
        dfs(i, j - 1)
    
    for i in range(m):
        for j in range(n):
            if grid[i][j] == '1':
                dfs(i, j)
                res += 1
    
    return res
```

### [Problem 695: Max Area of Island](https://leetcode.com/problems/max-area-of-island/description/)
Hints:
- The same if base condition like numOfIslands and set it to grid[i][j] = 0 to know we have visited there
- with every problem that require to find the area or sum or anything, just return like 1 + dfs(...)

```
def maxAreaOfIsland(self, grid: List[List[int]]) -> int:
    max_area = 0
    m, n = len(grid), len(grid[0])

    def dfs(i, j):
        if i < 0 or j < 0 or i >= m or j >= n or grid[i][j] == 0:
            return 0
        
        grid[i][j] = 0
        return 1 + dfs(i + 1, j) + dfs(i - 1, j) + dfs(i, j + 1) + dfs(i, j - 1)
    
    for i in range(m):
        for j in range(n):
            if grid[i][j] == 1:
                max_area = max(max_area, dfs(i, j))
    
    return max_area
```

### [Problem 133: Clone graph](https://leetcode.com/problems/clone-graph/description/)
Hints:
- Use hashmap to store the node that has been visited
- Use DFS to loop through:
    - if the current node is in hashmap then return that node --> visited[n]
    - if not then create new node: copy = Node(n.val)
    - then add it to hashmap: visited[n] = copy
    - iterate through neighbors of current node:
        - add the copy node neighbors to the neighbor of current node: copy.neighbors.append(dfs(neighbor))
    - return the copy node

```
def cloneGraph(self, node: Optional['Node']) -> Optional['Node']:
    if not node:
        return
    
    visited = {}

    def dfs(n):
        if n in visited:
            return visited[n]

        copy = Node(n.val)
        visited[n] = copy

        for neighbor in n.neighbors:
            copy.neighbors.append(dfs(neighbor))

        return copy
        
    return dfs(node)
```

### [Problem 286: Walls And Gates](https://leetcode.com/problems/walls-and-gates/description/)
Hints:
- It asks to use shortest path, then we need to use BFS
- `REMEMBER`: Since we use BFS, the first visit is always the shortest
- If we tranverse to each cell and then use BFS the complexity can be O(m*n)^2
- Therefore, we can just store the row and cols of the gates in a hashset and then append these positions to the queue
- iterate through the queue and assign it into nested BFS:
- assign distance of current position of the grid is the distance
    - nested BFS: base if condition: out of bound, existing in visited, or hit a wall then return
    - nested BFS: if not base condition: then add the position (row and col) to hashset and queue
- after the nested bfs done: increment the distance to 1

```
def islandsAndTreasure(self, grid: List[List[int]]) -> None:
    ROWS, COLS = len(grid), len(grid[0])

    visit = set()
    q = collections.deque()

    def add_room(r, c):
        if r < 0 or r == ROWS or c < 0 or c == COLS or (r, c) in visit or grid[r][c] == -1:
            return
        
        visit.add((r, c))
        q.append([r, c])

    for i in range(ROWS):
        for j in range(COLS):
            if grid[i][j] == 0:
                visit.add((i, j))
                q.append([i, j])
    
    dist = 0

    while q:
        for i in range(len(q)):
            r, c = q.popleft()
            grid[r][c] = dist

            add_room(r + 1, c)
            add_room(r - 1, c)
            add_room(r, c + 1)
            add_room(r, c - 1)
        dist += 1
```

### [Problem 994: Rotting Oranges](https://leetcode.com/problems/rotting-oranges/description/)
Hints:
- When it is shortest or minimum and start at somewhere that is not 0,0, we should use BFS
- In this problem, even though we iterate through correctly, if there is an orange that is in the bottom left where we cannot reach it, we should keep track of the fresh orange before we do BFS
- Before: append the position of rotten orange in queue, and keep track of fresh oranges
- Doing BFS: decrement fresh orange if we see them and assign it to be rotten orange
- After finishing going through 4 direction, we increment the minute by 1

```
def orangesRotting(self, grid: List[List[int]]) -> int:
    ROWS, COLS = len(grid), len(grid[0])
    fresh = 0
    res = 0
    queue = collections.deque()
    for r in range(ROWS):
        for c in range(COLS):
            if grid[r][c] == 2:
                queue.append((r, c))
            elif grid[r][c] == 1:
                fresh += 1

    def bfs(r, c):
        nonlocal fresh
        if r < 0 or r == ROWS or c < 0 or c == COLS or grid[r][c] != 1:
            return
        
        grid[r][c] = 2
        fresh -= 1
        queue.append((r, c))
    
    while queue and fresh > 0:
        for i in range(len(queue)):
            r, c = queue.popleft()
            bfs(r + 1, c)
            bfs(r - 1, c)
            bfs(r, c + 1)
            bfs(r, c - 1)

        res += 1
    return -1 if fresh > 0 else res
```

### [Problem 417: Pacific Atlantic Water Flow](https://leetcode.com/problems/pacific-atlantic-water-flow/description/)
Hints:
- There is no shortest path or anything so we can use DFS or BFS
- Since we notice that the pacific and atlantic areas that can have squares that can make water overflows are in the edges (the rows and cols which is the borders of the grid in 4 directions)
- Therefore, we need to iterate DFS through these areas first and store the position that has higher height than the previous height area, we need to have two hashset of pacific and atlantic 
- And then after that we iterate through rows and cols and check where are the position both appearing in pacific and atlantic hashset and add to the result

```
def pacificAtlantic(self, heights: List[List[int]]) -> List[List[int]]:
    pacific, atlanta = set(), set()
    res = []
    ROWS, COLS = len(heights), len(heights[0])
    def dfs(r, c, visit, prev_height):
        if r < 0 or c < 0 or r == ROWS or c == COLS or (r, c) in visit or heights[r][c] < prev_height:
            return
        
        visit.add((r, c))
        dfs(r + 1, c, visit, heights[r][c])
        dfs(r - 1, c, visit, heights[r][c])
        dfs(r, c + 1, visit, heights[r][c])
        dfs(r, c - 1, visit, heights[r][c])
    
    for c in range(COLS):
        dfs(0, c, pacific, heights[0][c])
        dfs(ROWS - 1, c, atlanta, heights[ROWS-1][c])
    
    for r in range(ROWS):
        dfs(r, 0, pacific, heights[r][0])
        dfs(r, COLS - 1, atlanta, heights[r][COLS-1])
    
    for r in range(ROWS):
        for c in range(COLS):
            if (r,c) in pacific and (r, c) in atlanta:
                res.append([r, c])
    
    return res
```
