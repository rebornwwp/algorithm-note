# N-Queens



### [N-Queens I](https://leetcode.com/problems/n-queens/) <a id="n-queens-i"></a>

这里需要注意的就是过滤条件，我们在temp保存了之前摆放的皇后的位置，所以之后再摆放必须符合要求。要求就是

```python
not any([col - x == 0 or i - y == 0 or abs(col - x) == abs(i - y) for x, y in temp])
```

```python
class Solution(object):
    def solveNQueens(self, n):
        """
        :type n: int
        :rtype: List[List[str]]
        """

        def dfs(n, col, temp, ans):

            if len(temp) == n:
                ans.append(temp[:])

            for i in range(n):
                if not any([col - x == 0 or i - y == 0 or abs(col - x) == abs(i - y) for x, y in temp]):
                    temp.append((col, i))
                    dfs(n, col + 1, temp, ans)
                    temp.pop()

        locations = []
        dfs(n, 0, [], locations)

        ans = []
        for i in range(len(locations)):
            l = [['.'] * n for _ in range(n)]
            for col, row in locations[i]:
                l[col][row] = 'Q'
            ans.append(l)
        return [[''.join(row) for row in one] for one in ans]
```

### [N-Queens II](https://leetcode.com/problems/n-queens-ii/) <a id="n-queens-ii"></a>

和上面的问题使用了相同的方法解答，走了一个投机取巧的方法， 说是Knuth神的 [Dancing Links](https://en.wikipedia.org/wiki/Dancing_Links) 下次研究

```python
class Solution(object):
    def totalNQueens(self, n):
        """
        :type n: int
        :rtype: int
        """

        def dfs(n, col, temp, ans):

            if len(temp) == n:
                ans.append(temp[:])

            for i in range(n):
                if not any([col - x == 0 or i - y == 0 or abs(col - x) == abs(i - y) for x, y in temp]):
                    temp.append((col, i))
                    dfs(n, col + 1, temp, ans)
                    temp.pop()

        locations = []
        dfs(n, 0, [], locations)

        return len(locations)
```

