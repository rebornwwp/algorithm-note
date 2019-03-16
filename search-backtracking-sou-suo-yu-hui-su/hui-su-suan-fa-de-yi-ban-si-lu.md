# 回溯算法的一般思路

1. 前一步保存的状态添加现在搜索点的状态
2. 对搜索点之后的搜索点进行dfs
3. 删除之前添加的状态

## 对Tree的DFS

一般dfs传入的参数有三个状态当前节点，从root节点搜索到当前节点的父节点之后的状态，和满足问题解结果的状态。

[Binary Tree Paths](https://leetcode.com/problems/binary-tree-paths/)

```python
class Solution:
    def binaryTreePaths(self, root):
        """
        :type root: TreeNode
        :rtype: List[str]
        """
        if not root:
            return []

        def dfs(node, cur, ans):
            if not node:
                return
            cur.append(node.val)
            if not node.left and not node.right:
                ans.append(cur[:])
            dfs(node.left, cur, ans)
            dfs(node.right, cur, ans)
            cur.pop()
            
        ans = []
        dfs(root, [], ans)
        return ['->'.join(map(str, l)) for l in ans]
```

## 对List的DFS

一般dfs传入的参数有四个状态传入的list，搜索到的节点的index，从root节点搜索到当前节点的父节点之后的状态，和满足问题解结果的状态。

[Permutations](https://www.lintcode.com/problem/permutations/description)

```python
class Solution:
    """
    @param: nums: A list of integers.
    @return: A list of permutations.
    """

    def permute(self, nums):
        if not nums:
            return [[]]

        def dfs(nums, cur, ans):
            if len(cur) == len(nums):
                ans.append(cur[:])
                return
            for n in nums:
                if n in cur:
                    continue
                cur.append(n)
                dfs(nums, cur, ans)
                cur.pop()
        ans = []
        dfs(nums, [], ans)
        return ans
```

 [Print Numbers by Recursion](https://www.lintcode.com/problem/print-numbers-by-recursion/description)

对dfs的一般应用，从中体会思想。其实这道题把整个过程想成一个树，树的根节点都是0。对于某一节点，如果其值为val，则叶子节点从左到右分别为父子节点的值值为 **val\*10 + 0, val \* 10 + 1, val \* 10 + 2,...,val \* 10 + 9** 。这样我们就会得到的9个分叉的完全树的叶子节点就是我们所要求的的0，1，2，3，4... 序列。

```python
class Solution:
    """
    @param n: An integer
    @return: An array storing 1 to the largest number with n digits.
    """

    def numbersByRecursion(self, n):

        def dfs(n, temp, ans):
            if n == 0:
                if temp > 0: # 去掉0
                    ans.append(temp)
                return
            for i in range(0, 10):
                dfs(n - 1, temp * 10 + i, ans)

        ans = []
        dfs(n, 0, ans)
        return ans
```



### 

