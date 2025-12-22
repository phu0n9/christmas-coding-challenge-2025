# Day 4

## Trees part 1:

### [Problem 226: Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/description/)
Hints:
- if condition root is not None then return None
- revert root.left = root.right
- revert root.right = root.left
- use recursive for root.left
- use recursive for root.right
- return root

```
def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
    if not root: return None
    root.left, root.right = root.right, root.left
    self.invertTree(root.left)
    self.invertTree(root.right)
    return root
```

### [Problem 104: Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/description/)
Hints:
- use recursive of 1 + max(dfs(root.left), dfs(root.right))
- if there is no node, return 0

```
def maxDepth(self, root: Optional[TreeNode]) -> int:
    def dfs(root):
        if not root: return 0
        return 1 + max(dfs(root.left), dfs(root.right))
    return dfs(root)
```

### [Problem 543: Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/description/)
Hints:
- Since it is diameter of a tree so if we have a tree like this:
```
      1
     / \
    2   3
   / \
  4   5
```
- It would be 4,2,5 and 4,1,3 and 2,1,3, so there are 3 diameters in this tree, so we need to compute the current height for each tree and update the diameter = left_tree_height + right_tree_height

```
def diameterOfBinaryTree(self, root: Optional[TreeNode]) -> int:
    diameter = 0
    def dfs(root):
        nonlocal diameter
        if not root: return 0
        
        left = dfs(root.left)
        right = dfs(root.right)
        diameter = max(diameter, left + right)
        return 1 + max(left, right)
    
    dfs(root)
    return diameter
```

### [Problem 110: Balanced Binary Tree](https://leetcode.com/problems/balanced-binary-tree/description/)
Hints:
- Normally, we can do like find the heigh of each left and right and check whether abs(left height - right height) > 1 then it will be false
- However, do that way the time complexity is O(n^2), use if condition to check root is None or not, if it is None then we return [True, 0] so we will use array value of 2 then first value is to check:
    - first value: check it has left node or not and check it has right node and check abs(left height - right height) <= 1
    - second value is to store the height: 1 + max(left_tree_height, right_tree_height)

```
def isBalanced(self, root: Optional[TreeNode]) -> bool:
        
    def dfs(root):
        if not root:
            return [True, 0]

        left, right = dfs(root.left), dfs(root.right)
        balanced = left[0] and right[0] and abs(left[1] - right[1]) <= 1
        return [balanced, 1 + max(left[1], right[1])]
        
    return dfs(root)[0]
```

### [Problem 100: Same Tree](https://leetcode.com/problems/same-tree/description/)
Hints:
- use dfs to return and rememeber if p tree is none and q tree is none, it can be correct so need to put this if condition check first
- second if is to check if one of the tree has the node or not
- check value is the same or not
- return recursive of both left trees of two trees and recursive of both right trees of two trees

```
def isSameTree(self, p: Optional[TreeNode], q: Optional[TreeNode]) -> bool:
    def dfs(p: Optional[TreeNode], q: Optional[TreeNode]) -> bool:
        if not p and not q:
            return True
        if not p or not q:
            return False
        
        if p.val != q.val: return False
        return dfs(p.left, q.left) and dfs(p.right, q.right)
    
    return dfs(p, q)
```

### [Problem 572: Subtree of Another Tree](https://leetcode.com/problems/subtree-of-another-tree/description/)
Hints:
- it is like a bigger problem of same tree
- it can have 2 possibilities: the subRoot can start at the root of the main tree or it is starting on the left/right of the main tree
- so we need to check it is the same tree or it will start at `root.left` or `root.right`, remain the same `subRoot`

```
def isSubtree(self, root: Optional[TreeNode], subRoot: Optional[TreeNode]) -> bool:
    if not root: return False

    def dfs(root: Optional[TreeNode], subRoot: Optional[TreeNode]) -> bool:
        if not root and not subRoot: return True
        elif not root or not subRoot: return False

        if root.val != subRoot.val: return False
        return dfs(root.left, subRoot.left) and dfs(root.right, subRoot.right)

    return dfs(root, subRoot) or self.isSubtree(root.left, subRoot) or self.isSubtree(root.right, subRoot)
```

### [Problem 235: Lowest Common Ancestor of a Binary Search Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/description/)
Hints:
- Since it is a binary search tree, we have two options like it is smaller then it will be on the left, if it is bigger, it will be on the right
- If the p value and q value both smaller than root val, then it will be on the left
- If the p value and q value both bigger than root val, then it will be on the right
- Otherwise p and q are on opposite sides, or one equals the root, then it will be on the root

```
def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
    def dfs(root, p, q):
        if p.val < root.val and q.val < root.val:
            return dfs(root.left, p, q)
        elif p.val > root.val and q.val > root.val:
            return dfs(root.right, p, q)
        else:
            return root
    return dfs(root, p, q)
```