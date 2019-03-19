# 枚举法

DFS + Backtracking 的搜索量很大。如果每次 DFS 之前都要进行条件检查的话，优化条件函数可以显著提升程序速度。

### [Generate Parentheses](https://leetcode.com/problems/generate-parentheses/) <a id="generate-parentheses"></a>

```python
class Solution(object):
    def generateParenthesis(self, n):
        """
        :type n: int
        :rtype: List[str]
        """
        if n == 0:
            return ['']

        def dfs(left, right, temp, ans):
            if left == 0 and right == 0:
                ans.append(temp[:])
            if left > 0:
                dfs(left - 1, right, temp + ['('], ans)

            if right > left:
                dfs(left, right - 1, temp + [')'], ans)

        ans = []
        dfs(n, n, [], ans)
        return [''.join(x) for x in ans]
```

### [Restore IP Addresses](https://leetcode.com/problems/restore-ip-addresses/) <a id="restore-ip-addresses"></a>

遍历的时候分三种情况 遍历1个字符，2个字符，3个字符。

需要注意的是边界时候，也就是遍历到最后几个字符的时候，如果长度不够三，情况就不会是三种了，比如“12“字符串就只能有两种情况：1， 12。

```python
class Solution(object):
    def restoreIpAddresses(self, s):
        """
        :type s: str
        :rtype: List[str]
        """
        if len(s) == 0:
            return []

        def dfs(s, temp, ans):
            if len(temp) == 4 and len(s) == 0:
                ans.append(temp[:])
                return

            if len(temp) == 4 and len(s) > 0:
                return
            if len(temp) < 4 and len(s) == 0:
                return

            # for i in range(1, 4):
            # 这里需要注意的是考虑最后一次搜索时，s的长度小于3的时候，当s长度为2的时候比如s='35'
            # s[:2] 和s[:3]是取的相同的长度 都为35，这样搜索过程中多搜索了一次
            r = min(4, len(s) + 1)
            for i in range(1, r):
                if int(s[:i]) < 256 and (i == 1 or (i > 1 and s[0] != '0')):
                    temp.append(s[:i])
                    dfs(s[i:], temp, ans)
                    temp.pop()
        ans = []
        dfs(s, [], ans)
        return ['.'.join(l) for l in ans]
```

### [Palindrome Partitioning](https://leetcode.com/problems/palindrome-partitioning/) <a id="palindrome-partitioning"></a>

没啥好说的，DFS and backtracking

```python
class Solution(object):
    def partition(self, s):
        """
        :type s: str
        :rtype: List[List[str]]
        """
        if len(s) == 0:
            return []

        def dfs(s, temp, ans, store):
            if len(s) == 0:
                ans.append(temp[:])
                return

            for i in range(len(s)):
                if s[:i + 1] in store:
                    temp.append(s[:i + 1])
                    dfs(s[i + 1:], temp, ans, store)
                    temp.pop()
                elif s[:i + 1] == s[:i + 1][::-1]:
                    store.add(s[:i + 1])
                    temp.append(s[:i + 1])
                    dfs(s[i + 1:], temp, ans, store)
                    temp.pop()
        ans = []
        dfs(s, [], ans, set())
        return ans
```





