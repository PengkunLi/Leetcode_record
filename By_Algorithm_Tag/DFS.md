# DFS Algorithm

## TOPICS
including tree, graph

## Tree

### [111. Minimum-Depth-of-Binary-Tree](https://leetcode-cn.com/problems/minimum-depth-of-binary-tree/)

#### Method 1 :penguin: Divide & conquer/DFS Recursion
```python
class Solution:
    def minDepth(self, root: TreeNode) -> int:
        if not root: return 0
        if not root.left and not root.right: return 1
        left_minDepth = self.minDepth(root.left)
        right_minDepth = self.minDepth(root.right)
        if min(left_minDepth, right_minDepth):
            return min(left_minDepth, right_minDepth) + 1
        else:
            return max(left_minDepth, right_minDepth) + 1
```

#### Method 2 :penguin: DFS Iteration
```python

```
