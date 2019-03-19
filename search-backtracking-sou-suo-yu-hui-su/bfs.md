# BFS

## 问题描述以及问题能解决的问题

假设你经营着一家芒果农场，需要寻找芒果销售商，以便将芒果卖给他。为此，我们可以通过广度优先搜索算法，在朋友中查找出符合条件的芒果销售商。

广度优先搜索是一种用于图的查找算法，可帮助我们回答两类问题：

* 第一类问题：从节点A出发，有前往节点B的路径吗？（在你的人际关系网中，有芒果销售商吗？）
* 第二类问题：从节点A出发，前往节点B的哪条路径最短？（哪个芒果销售商与你的关系最近？）

### [Word Ladder I](https://leetcode.com/problems/word-ladder/) <a id="word-ladder-i"></a>

这道题比较难，需要注意一些东西。 感谢花花酱的[解答](https://zxi.mytechroad.com/blog/searching/127-word-ladder/).

```text
# 注意的一点，只要我们遍历过的就可以删除了，不用再变了。
# 如果不删除遍历，比如 beginWord = "hit"
# endWord = "cog"
# wordList = ["hot","hxt", "dot", "dog", "lot", "log", "cog"]
# bfs的遍历情况可以是这样的
# hit -> hot -> hxt ->...
# hit -> hot -> ...
# hit -> hxt ->...
# hit -> hxt -> hot
# 这样重复的遍历是没有必要，本题求的是最短路径,上面重复了
# 如果删除之后遍历将会是
# hit -> hot ->
# hit -> hxt ->
剪支减少遍历次数。
```

```python
class Solution(object):
    def ladderLength(self, beginWord, endWord, wordList):
        """
        :type beginWord: str
        :type endWord: str
        :type wordList: List[str]
        :rtype: int
        """
        d = set(wordList)
        if endWord not in d:
            return 0

        ascii_lowercase = 'abcdefghijklmnopqrstuvwxyz'

        queue = []
        queue.append(beginWord)

        length = len(beginWord)
        step = 1
        while len(queue) > 0:
            step += 1

            l = len(queue)
            for _ in range(l):
                first_ele = queue.pop(0)
                for i in range(length):
                    for x in ascii_lowercase:
                        new_elem = first_ele[:i] + x + first_ele[i + 1:]
                        if new_elem == endWord:
                            return step
                        elif new_elem not in d:
                            continue
                        # 这里用过就可以删除了
                        d.remove(new_elem)
                        queue.append(new_elem)
        return 0
```

