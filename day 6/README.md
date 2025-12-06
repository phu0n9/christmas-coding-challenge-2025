# Day 6
- 2 tree problems
- 3 trie problems

### [Problem 124: Binary Tree Maximum Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum/description/)
Hints:
- the maximum path can start every where:
    - entirely on the left subtree
    - entirely on the right subtree
    - go down one side
    - or pass through the current node (left → node → right)
- we need to get the maximum of root.val + left + right or maximum of root.val + max(left, right)
- At a node N, you can form a path:
    ``` left_subtree_best + N.val + right_subtree_best ```
    - This is a candidate for `global maximum`, because it is a full “V-shaped” path. But this path cannot be given to the parent, because:
        - parents only accept one direction, left or right
        - parents do not accept “V-shapes”
    - So we update the global max using:
    ``` left_gain + root.val + right_gain ```
- A parent can only extend the path down one side, so the return value is: `root.val + max(left_gain, right_gain)`. But! If left or right gains are negative, they only reduce the sum. 
```
left_gain = max(0, dfs(root.left))
right_gain = max(0, dfs(root.right))
```

```
def maxPathSum(self, root: Optional[TreeNode]) -> int:
    self.max_value = float('-inf')

    def dfs(root):
        if not root:
            return 0
        
        left = max(0, dfs(root.left))
        right = max(0, dfs(root.right))

        self.max_value = max(self.max_value, root.val + left + right)

        return root.val + max(left, right)
    dfs(root)

    return self.max_value
```

### [Problem 297: Serialize and Deserialize Binary Tree](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/description/)
Hints:
- if we have a tree like:
```
    1
   / \
  2   3
     / \
    4   5
```
- we use preorder: root -> left -> right
- for serialize: we can use something like "1,2,N,N,3,4,N,N,5,N,N", where N is null and we need the comma to separate numbers
- for deserialize: keep track of current index with global variable, then if it hits "N", increase 1 and only number as node and also increase the current index by 1, then recursive node left and node right

```
class Codec:
    def serialize(self, root):
        """Encodes a tree to a single string.
        
        :type root: TreeNode
        :rtype: str
        """
        def dfs(root):
            if not root:
                return "N"
            
            return str(root.val) + "," +dfs(root.left) + "," + dfs(root.right)

        return dfs(root)
            
        

    def deserialize(self, data):
        """Decodes your encoded data to tree.
        
        :type data: str
        :rtype: TreeNode
        """
        data = data.split(",")
        self.index = 0
        def dfs():
            if data[self.index] == 'N':
                self.index += 1
                return

            node = TreeNode(int(data[self.index]))
            self.index += 1
            node.left = dfs()
            node.right = dfs()
            return node

        return dfs()
```

### [Problem 208: Implement Trie (Prefix Tree)](https://leetcode.com/problems/implement-trie-prefix-tree/description/)
Hints:
- We can use like 'apple' -> {'a': {'p': {'p': {'l': {'e': {'.'}}}}}}
- the '.' represent it has been completed
- insert: if not found in trie then add {} to current character of that trie and remember to end it with '.'
- search: if current char not in trie, return False, else iterate the word and check whether current char is '.' or not
- startWith: iterate through the prefix and if the current char is not in trie, then return false, continue to go deep into trie until it is the end of prefix character, if it reaches the end of prefix string then it must start with the prefix, return true

```
class Trie:

    def __init__(self):
        self.trie = {}

    def insert(self, word: str) -> None:
        trie = self.trie

        for c in word:
            if c not in trie:
                trie[c] = {}
            
            trie = trie[c]
        
        trie['.'] = '.'

    def search(self, word: str) -> bool:
        trie = self.trie

        for c in word:
            if c not in trie:
                return False
            trie = trie[c]
        
        return '.' in trie

    def startsWith(self, prefix: str) -> bool:
        trie = self.trie

        for c in prefix:
            if c not in trie:
                return False
            
            trie = trie[c]
        
        return True

```


### []()

### []()

