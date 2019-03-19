# Subsets, Combination and Permutation

1. Subsets问题，从集合中寻找子集。子集的长度最大为S的长度。
2. permutation问题，排列问题。在一个可能有重复元素的集合中，选取出符合条件的所有集合，比如一副牌中，选两张牌排列，这是个区分顺序，可能还包含着重复性元素的问题。比如
3. combination问题，组合问题。在一个可能有重复元素的集合中，选取出符合条件的所有集合，集合中的元素，和permutation想似，其不区分选择顺序，比如一副牌中，选出两个牌，不分顺序。（不分顺序，意味着操作中需要去重）



### [Combinations](http://www.lintcode.com/problem/combinations) <a id="lintcode---combinations"></a>

```python
class Solution(object):
    def combine(self, n, k):
        """
        :type n: int
        :type k: int
        :rtype: List[List[int]]
        """
        if n < k:
            return []
        if k == 0:
            return [[]]

        def dfs(n, i, temp, k, ans):
            if k == len(temp):
                ans.append(temp[:])
                return

            for x in range(i, n):
                temp.append(x + 1)
                dfs(n, x + 1, temp, k, ans)
                temp.pop()

        ans = []
        dfs(n, 0, [], k, ans)
        return ans
```

### [Combination Sum](http://www.lintcode.com/en/problem/combination-sum/#) <a id="lintcode---combination-sum"></a>

这道题需要注意的就是需要先排序。

```python
class Solution(object):
    def combinationSum(self, candidates, target):
        """
        :type candidates: List[int]
        :type target: int
        :rtype: List[List[int]]
        """
        if not candidates:
            return []
        if target == 0:
            return [[]]

        def dfs(candidates, temp, target, ans):
            if target == 0:
                ans.append(temp[:])
                return
            if target < 0:
                return
            for i in range(len(candidates)):
                temp.append(candidates[i])
                dfs(candidates[i:], temp, target - candidates[i], ans)
                temp.pop()

        ans = []
        candidates = sorted(candidates)
        dfs(candidates, [], target, ans)
        return ans
```

### [Permutations](http://www.lintcode.com/en/problem/permutations/#) <a id="lintcode---permutations"></a>

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

### [Permutations II](http://www.lintcode.com/en/problem/permutations-ii/) <a id="lintcode---permutations-ii"></a>

```python
class Solution(object):
    def permuteUnique(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        if not nums:
            return []

        def dfs(nums, temp, ans):
            if not nums:
                ans.append(temp[:])

            visited = []
            for i in range(len(nums)):
                # 过滤掉重复数字的dfs
                if nums[i] in visited:
                    continue
                temp.append(nums[i])
                visited.append(nums[i])
                dfs(nums[0:i] + nums[i + 1:], temp, ans)
                temp.pop()

        ans = []
        dfs(nums, [], ans)
        return ans
```

