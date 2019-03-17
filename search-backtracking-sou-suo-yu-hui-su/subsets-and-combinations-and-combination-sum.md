# Subsets & Combinations & Combination Sum

巩固对回溯算法中对dfs的应用，多思考。了解

[Subsets](https://leetcode.com/problems/subsets/)

```python
class Solution:
    def subsets(self, nums):
        if not nums:
            return [[]]

        def dfs(nums, temp, ans):
            ans.append(temp[:])

            for i in range(len(nums)):
                temp.append(nums[i])
                dfs(nums[i+1:], temp, ans)
                temp.pop()

        ans = []
        dfs(nums, [], ans)
        return ans
```

#### [Subsets II](https://leetcode.com/problems/subsets-ii/) <a id="subsets-ii"></a>

```python
class Solution(object):
    def subsetsWithDup(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        if not nums:
            return []

        def dfs(nums, temp, ans):
            ans.append(temp[:])
            if not nums:
                return

            visited = []
            for i in range(len(nums)):
                if nums[i] not in visited:
                    visited.append(nums[i])
                    dfs(nums[i+1:], temp + [nums[i]], ans)

        ans = []
        nums = sorted(nums)
        dfs(nums, [], ans)
        return ans
```

#### [Combination Sum II](https://leetcode.com/problems/combination-sum-ii/) <a id="combination-sum-ii-"></a>

```python
class Solution(object):
    def combinationSum2(self, candidates, target):
        """
        :type candidates: List[int]
        :type target: int
        :rtype: List[List[int]]
        """
        if not candidates:
            return []

        def dfs(candidates, target, temp, ans):
            if target == 0:
                ans.append(temp[:])
            if target < 0:
                return

            visited = []
            for i in range(len(candidates)):
                if candidates[i] not in visited:
                    visited.append(candidates[i])
                    temp.append(candidates[i])
                    dfs(candidates[i+1:], target - candidates[i], temp, ans)
                    temp.pop()

        ans = []
        candidates = sorted(candidates)
        dfs(candidates, target, [], ans)
        return ans
```

#### [Combination Sum III](https://leetcode.com/problems/combination-sum-iii/) <a id="combination-sum-iii"></a>

```python
class Solution(object):
    def combinationSum3(self, k, n):
        """
        :type k: int
        :type n: int
        :rtype: List[List[int]]
        """

        def dfs(index, k, target, temp, ans):
            if target == 0 and k == 0:
                ans.append(temp[:])

            if target < 0 or k < 0:
                return

            for i in range(index, 10):
                temp.append(i)
                dfs(i + 1, k - 1, target - i, temp, ans)
                temp.pop()
                # 想函数式一点可以将上面三行改成
                # dfs(i + 1, k - 1, target - i, temp + [i], ans)

        ans = []
        dfs(1, k, n, [], ans)
        return ans
```

#### [Combination Sum IV](https://leetcode.com/problems/combination-sum-iv/) <a id="combination-sum-iv"></a>

这道题需要好好注意，不是就解得集合，而是求得解得个数。

```python
class Solution:
    def combinationSum4(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
        # 上面的第一个解法为啥timelimit是我们dfs中有重复计算了，
        # 需要一个中间做存储的如同lru_cache一样

        def dfs(nums, temp_sum, target, d):
            if target == temp_sum:
                return 1
            if target < temp_sum:
                return 0
            if temp_sum in d:
                return d[temp_sum]

            count = 0
            for n in nums:
                count += dfs(nums, temp_sum + n,  target, d)
                # dfs(nums,temp1, target, d)和dfs(nums, temp1, target2, d)
                # 这里需要注意的是temp1等于temp2的时候肯定结果是相同的
                # 所以加入了一个cache作用的d容器，减少函数的调用次数。

            d[temp_sum] = count
            return count
```

