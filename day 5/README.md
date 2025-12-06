# Day 5

- 6 Tree problems

### [Problem 102: Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/description/)
Hints:
- This will have 2 ways of doing it: DFS and BFS, normally in level order, we use BFS
- deque, add first root node
- loop through queue: check for the size of the queue for the levels
- nested loop with levels size, popleft the current node, add current node to level array, and then assign node.left and node.right to the queue if they are existing
- outside of this second loop, we add array of levels

```
def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
    if not root:
        return []
    
    res = []
    queue = collections.deque([root])

    while queue:
        len_queue = len(queue)
        level = []

        for _ in range(len_queue):
            node = queue.popleft()
            level.append(node.val)
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
        
        res.append(level)
    return res
```

- DFS:
- do like normal DFS but need to keep track with the level
- if the len(current result array) <= level, then we need to add new empty array
- assign the current node value with the res array with the current level value
- loop with node.left and node.right with both level + 1

```
def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
    res = []
    def dfs(root, level):
        if not root:
            return None

        if len(res) <= level:
            res.append([])

        res[level].append(root.val)
        dfs(root.left, level + 1)
        dfs(root.right, level + 1)

    dfs(root, 0)
    return res
```

### [Problem 199: Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/description/)
Hints:
- Use queue, deque and popleft the first one and then only add the last value on the queue
- If there is node.left or node.right, add them to the queue

```
def rightSideView(self, root: Optional[TreeNode]) -> List[int]:
    res = []
    if not root: return res
    queue = deque([root])
    while queue:
        size = len(queue)
        for i in range(size):
            node = queue.popleft()
            if i == size - 1:
                res.append(node.val)
            
            if node.left:
                queue.append(node.left)

            if node.right:
                queue.append(node.right)

    return res
```

### [Problem 98: Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/description/)
Hints:
- if left node < node.val < right node --> valid
- however we cannot compare between just node value because there are some cases like:
```
    5
   / \
  1   4
     / \
    3   6
```
- so 3 is not correct, it should be on the left side, so compare between just node would not be correct
- However, we can compare the min < node.val < max where min is the smallest node on the left and max is the max node on the right
- Use these to iterate through the recursive loop like dfs(root, min, max) --> dfs(root.left, left, root.val) and dfs(root.right, root.val, right)
- In the first iteration we set min and max to be the biggest and smallest number like float('inf') and float('-inf')

```
def isValidBST(self, root: Optional[TreeNode]) -> bool:
    def dfs(root, left, right):
        if not root:
            return True
        
        if not (left < root.val < right): return False
        return dfs(root.left, left, root.val) and dfs(root.right, root.val, right)
    
    return dfs(root, float('-inf'), float('inf'))
```

### [Problem 105: Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)
Hints:
- in preorder: we can get the root since the order is root -> left -> right
- in inorder: we can get the left and right tree since the order is left -> root -> right
- to save time complexisty, we use hashmap to store the position of inorder node val with respective index
- then if base condition is left > right then we stop the recursion, we have to increase the current index to iterate through every index in preorder
- so if we got something like this:
```
preorder = [3, 9, 20, 15, 7]
inorder  = [9, 3, 15, 20, 7]
```
Hashmap:
```
9 → 0
3 → 1
15 → 2
20 → 3
7 → 4
```
Root will be 3 and it is at index 1
```
Left inorder = [9]
Right inorder = [15, 20, 7]
```
Recursive:
```
dfs(left=0, right=0)   → builds left subtree
dfs(left=2, right=4)   → builds right subtree
```

```
def buildTree(self, preorder: List[int], inorder: List[int]) -> Optional[TreeNode]:
    index_map = {val: i for i, val in enumerate(inorder)}
    self.index = 0

    def dfs(left, right):
        if left > right:
            return
        
        root_val = preorder[self.index]
        self.index += 1

        root = TreeNode(root_val)
        mid = index_map[root_val]

        root.left = dfs(left, mid - 1)
        root.right = dfs(mid + 1, right)

        return root

    return dfs(0, len(inorder) - 1)
```

### [Problem 230: Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst/description/)
Hints:
- since inorder is the order has been sorted, we can use this inorder to solve this problem because it needs the k smallest value
- so we can add all the values in an array with in order sorting, and then the value would be arr[k - 1] since the array is 0-indexed

```
def kthSmallest(self, root: Optional[TreeNode], k: int) -> int:
    arr = []
    
    def dfs(root):
        if not root:
            return 
        
        dfs(root.left)
        arr.append(root.val)
        dfs(root.right)
    
    dfs(root)
    return arr[k - 1]
```

### [Problem 1448: Count Good Nodes in Binary Tree](https://leetcode.com/problems/count-good-nodes-in-binary-tree/description/)
Hints:
- A node is good if:
```
On the path from the root to that node,
no node has a value greater than the node’s value.
```
- In other words, `node.val >= max value seen so far on this path`
- At each node:
    - Compare node.val with max_so_far
    - if base condition return 0
    - If node.val >= max_so_far, it is a good node
    - Update max_so_far = max(max_so_far, node.val)
    - Recurse into left and right children with the updated max

```
def goodNodes(self, root: TreeNode) -> int:
    self.cnt = 0
    def dfs(root, max_value):
        if not root:
            return 0
        
        max_value = max(max_value, root.val)
        if root.val >= max_value:
            self.cnt += 1
        
        dfs(root.left, max_value)
        dfs(root.right, max_value)
    dfs(root, float('-inf'))
    return self.cnt
```


