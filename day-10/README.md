# Day 10:
- 6 Medium and 1 Hard: Directed and Undirected graph

### [Problem 130: Surrounded Regions](https://leetcode.com/problems/surrounded-regions/description/)
Hints:
- This one is like the pacific and atlantic problem
- The O at the borders are considered safe
- The O at the inner boxes can be converted to X
- So we need to mark the O at the borders to something else like S and marked as visited using DFS
- We do not need to mark the O at the inner boxes
- Then we loop through again and set marked S to O and other O into X

```
def solve(self, board: List[List[str]]) -> None:
    """
    Do not return anything, modify board in-place instead.
    """        
    ROWS, COLS = len(board), len(board[0])
    
    def dfs(r, c):
        if r < 0 or r == ROWS or c < 0 or c == COLS or board[r][c] != 'O':
            return
        
        board[r][c] = 'S'

        dfs(r + 1, c)
        dfs(r - 1, c)
        dfs(r, c + 1)
        dfs(r, c - 1)
    
    for r in range(ROWS):
        dfs(r, 0)
        dfs(r, COLS - 1)
    
    for c in range(COLS):
        dfs(0, c)
        dfs(ROWS - 1, c)

    for r in range(ROWS):
        for c in range(COLS):
            if board[r][c] == 'S':
                board[r][c] = 'O'
            elif board[r][c] == 'O':
                board[r][c] = 'X'
```

### [Problem 207: Course Schedule](https://leetcode.com/problems/course-schedule/description/)
Hints:
- Store the neighbors in a graph with the node
- Then use array of UNVISTED with the length of numCourses, and then in dfs we have 3 states: UNIVISTED, VISITING, and VISITED
- Store the states array with the status: VISITING and VISITED, if it is VISITED then it will be true, then if it is VISITING, it means that it has been visiting the second time so there is a cycle there, we return false
- in dfs if it is not VISITING or VISITED, then set the node to VISITING, and then loop through for loop of its neighbor and recursive of it
- Loop through the len of numCourses, use dfs to start the recursive

```
def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
    graph = defaultdict(list)

    courses = prerequisites
    for a, b in courses:
        graph[a].append(b)
    
    UNVISITED = 0
    VISITING = 1
    VISITED = 2
    states = [UNVISITED] * numCourses

    def dfs(node):
        state = states[node]
        if state == VISITED: return True
        elif state == VISITING: return False

        states[node] = VISITING

        for nei in graph[node]:
            if not dfs(nei):
                return False
        
        states[node] = VISITED
        return True

    for i in range(numCourses):
        if not dfs(i):
            return False

    return True
```

### [Problem 210: Course Schedule II](https://leetcode.com/problems/course-schedule-ii/description/)
Hints:
- It is the same as Course Schedule but adding a list to store the result

```
def findOrder(self, numCourses: int, prerequisites: List[List[int]]) -> List[int]:
    res = []
    graph = defaultdict(list)
    for a, b in prerequisites:
        graph[a].append(b)

    UNVISITED = 0
    VISITING = 1
    VISITED = 2

    states = [UNVISITED] * numCourses

    def dfs(node):
        if states[node] == VISITED: return True
        elif states[node] == VISITING: return False

        states[node] = VISITING

        for nei in graph[node]:
            if not dfs(nei):
                return False
        
        states[node] = VISITED
        res.append(node)
        return True

    for i in range(numCourses):
        if not dfs(i):
            return []

    return res
```

### [Problem 261: Graph Valid Tree](https://leetcode.com/problems/graph-valid-tree/description/)
Hints:
- This is undirected graph so u -> v can be v -> u
- Therefore, we need to add both cases like u -> v and v -> u
- Use hashset visit, base case of dfs is that if the current node is in visit, then it would be false
- After that add node to visit if it has not been visit, then loop through its neighbor:
    - Remember to add prev node, if the prev node is the same as current node --> since this is a undirected graph, so it is the edge, this would not occur in directed graphs
    - then recursive dfs to check if it is false then return false
    - when it checks all the path successfully, return true
- run this dfs to node is 0 and prev node is -1, and check whether len(visited) == n, that means we have visited all the nodes

```
def validTree(self, n: int, edges: List[List[int]]) -> bool:
    graph = defaultdict(list)
    for edge, neighbor in edges:
        graph[edge].append(neighbor)
        graph[neighbor].append(edge)

    visited = set()

    def dfs(node, prev):
        if node in visited:
            return False
        
        visited.add(node)
        for nei in graph[node]:
            if nei == prev:
                continue
            
            if not dfs(nei, node):
                return False
            
        return True

    return dfs(0, -1) and len(visited) == n
```

### [Problem 323: Number of Connected Components in an Undirected Graph](https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/description/)
Hints:
- Review again with Union Find
- This is undirected graph so need to add the edges both edges into graph
- Then make an array of False visited array with size of n
- Do a dfs only when the current index of visited array is not visited (False) then reset the current index visited state to True
- Loop through for loop with size n and check if it is not visited node then reset to True and do dfs and then increment result

```
def countComponents(self, n: int, edges: List[List[int]]) -> int:
    graph = defaultdict(list)
    for u, v in edges:
        graph[u].append(v)
        graph[v].append(u)

    visit = [False] * n
    res = 0
    def dfs(node):
        for neighbor in graph[node]:
            if not visit[neighbor]:
                visit[neighbor] = True
                dfs(neighbor)
                    
    for node in range(n):
        if not visit[node]:
            visit[node] = True
            dfs(node)
            res += 1
    return res
```

### [Problem 684: Redundant Connection](https://leetcode.com/problems/redundant-connection/description/)
Hints:
- Use union find to do
- One array keep track of parent from 0 to N + 1 (len edges + 1)
- One array keep track of rank with 1 values from 0 to N + 1 
- find function is to assign parent node:
    - if current node != parent node[node]: parent[node] = find(parent[node])
    - else return parent[node]
- union function to check:
    - p1, p2 = find(node 1) and find(node 2)
    - if p1 = p2: meaning that it is redundant connection so need to return False
    - if rank[p1] > rank[p2]: assign parent node2 = p1 and increase the rank[p1] += rank[p2]
    - else rank[p1] <= rank[p2]: assign parent node1 = p2 and increase the rank[p2] += rank[p1]
- The ranking is for building the smaller tree under the big tree
- Then outside of the loop with edges length: and check if the union is false then return the two nodes

```
def findRedundantConnection(self, edges: List[List[int]]) -> List[int]:
    N = len(edges)
    par = [i for i in range(N + 1)]
    rank = [1] * (N + 1)

    def find(n):
        if par[n] != n:
            par[n] = find(par[n])
        return par[n]
    
    def union(n1, n2):
        p1, p2 = find(n1), find(n2)
        if p1 == p2:
            return False
        
        if rank[p1] > rank[p2]:
            par[p2] = p1
            rank[p1] += rank[p2]
        else:
            par[p1] = p2
            rank[p2] += rank[p1]
    
        return True
    
    for n1, n2 in edges:
        if not union(n1, n2):
            return [n1, n2]
```

### [Problem 127: Word Ladder](https://leetcode.com/problems/word-ladder/description/)
Hints:
- Imagine each word is a node and we can only iterate one at a time and we also need to find the shortes path so we need to do BFS and graph in this case
- Trick: add all the variation of the word in the wordlist in graph like: hit -> "$it", "h$t", "hi$" 
- We also need a hashset to add the word in
- Using a queue to add the current word and the level, every time we iterate and add it to the queue, we increase the level by 1 and add it to the hashset
- Rememeber to reset the graph of the current pattern after we finished iteration to [] so that we don't need to visit again

```
def ladderLength(self, beginWord: str, endWord: str, wordList: List[str]) -> int:
    if endWord not in wordList:
        return 0
    graph = defaultdict(list)

    def create_graph(graph, word):
        for i in range(len(word)):
            pattern = word[:i] + "*" + word[i+1:]
            graph[pattern].append(word)
        return graph

    for word in wordList:
        graph = create_graph(graph, word)
    
    visited = set([beginWord])
    queue = collections.deque([(beginWord, 1)])

    while queue:
        word, level = queue.popleft()
        
        if word == endWord:
            return level
        
        for i in range(len(beginWord)):
            pattern = word[:i] + "*" + word[i+1:]

            for neighbor in graph[pattern]:
                if neighbor not in visited:
                    visited.add(neighbor)
                    queue.append((neighbor, level + 1))
            graph[pattern] = []

    return 0 
```