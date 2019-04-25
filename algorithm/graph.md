# Graph

## 概念

图的定义一个图（Graph）由顶点集（vertex）V和边集（edge）E构成

路径（path）顶点序列，路径的长（length）N-1 N为路径点的个数

如果一个有向图从任意顶点出发无法经过若干条边回到该点，则这个图是一个**有向无环图**（DAG图）。

如果一个无向图中，每一个顶点到每个其他顶点都能找到路径，则说明图是连通的（connected），具有上述性质的有向图是强连接的（Strong Connected），如果有向图去掉方向所形成的基础图（underlying graph）是连通的，那么这个有向图是弱连通的（weakly connected）

完全图（complete graph）是其每一对顶点间都存在一条边的图。

## 图的表示

如果图稠密的时候，二维数组（adjacent matrix）表示

如果稀疏的时候，邻接表（adjacent list）表示

### [拓扑排序](http://www.bowdoin.edu/~ltoma/teaching/cs231/fall14/Lectures/11-topsort/topsort.pdf)

拓扑排序是对有向无圈图（directed acyclic graph G = \(V, E\)）的一种排序，它使得如果存在一条vi到vj的路径，那么在排序结果中vj出现在vi之后。如果有向图中存在圈，那么拓扑排序是不存在的。

#### DFS

```python
from collections import defaultdict

#Class to represent a graph
class Graph:
    def __init__(self,vertices):
        self.graph = defaultdict(list) #dictionary containing adjacency List
        self.V = vertices #No. of vertices

    # function to add an edge to graph
    def addEdge(self,u,v):
        self.graph[u].append(v)

    # A recursive function used by topologicalSort
    def topologicalSortUtil(self,v,visited,stack):

        # Mark the current node as visited.
        visited[v] = True

        # Recur for all the vertices adjacent to this vertex
        for i in self.graph[v]:
            if visited[i] == False:
                self.topologicalSortUtil(i,visited,stack)

        # Push current vertex to stack which stores result
        stack.insert(0,v)

    # The function to do Topological Sort. It uses recursive
    # topologicalSortUtil()
    def topologicalSort(self):
        print(self.graph)
        # Mark all the vertices as not visited
        visited = [False]*self.V
        stack =[]
        # Call the recursive helper function to store Topological
        # Sort starting from all vertices one by one
        for i in range(self.V-1, -1, -1):
            if visited[i] == False:
                self.topologicalSortUtil(i,visited,stack)
                print(stack)

        # Print contents of the stack
        print stack
g= Graph(7)
g.addEdge(0, 1)
g.addEdge(0, 2)
g.addEdge(0, 3)
g.addEdge(1, 3)
g.addEdge(1, 4)
g.addEdge(2, 5)
g.addEdge(3, 2)
g.addEdge(3, 5)
g.addEdge(3, 6)
g.addEdge(4, 3)
g.addEdge(4, 6)
g.addEdge(6, 5)

print "Following is a Topological Sort of the given graph"
g.topologicalSort()
```



#### BFS

```python
class DirectedGraphNode:
    def __init__(self, x):
        self.label = x
        self.neighbors = []


class Solution:
    """
    @param: graph: A list of Directed graph node
    @return: Any topological order for the given graph.
    """

    def topSort(self, graph):

        def dfs(ans, root, visited):
            if root in visited:
                return

            for n in root.neighbors:
                dfs(ans, n, visited)
            ans.insert(0, root)
            visited.append(root)

        ans = []
        visited = []
        for root in graph:
            dfs(ans, root, visited)
        return ans

    def topSort1(self, graph):
        indegrees = dict()
        for g in graph:
            if g not in indegrees:
                indegrees[g] = 0
            for i in g.neighbors:
                if i not in indegrees:
                    indegrees[i] = 1
                else:
                    indegrees[i] += 1

        def remove(indegrees, e):
            if e in indegrees and indegrees[e] == 0:
                del indegrees[e]

        e_without_indegree = [g for g in graph if indegrees[g] == 0]
        ans = []
        count = 0
        while len(e_without_indegree) > 0:
            first = e_without_indegree.pop(0)
            ans.append(first)
            count += 1
            remove(indegrees, first)
            for e in first.neighbors:
                indegrees[e] -= 1
                if indegrees[e] == 0:
                    e_without_indegree.append(e)
        if count != len(graph):
            raise IndexError
        return ans

connects = [
    (1, (2, 3, 4)),
    (2, (4, 5)),
    (3, (6,)),
    (4, (3, 6, 7)),
    (5, (4, 7)),
    (6, set()),
    (7, (6,))
]
l = [DirectedGraphNode(i) for i in range(1, 8)]


for i, ns in connects:
    for n in ns:
        l[i - 1].neighbors.append(l[n - 1])
s = Solution()

ans = s.topSort1(l)
print([i.label for i in ans])
```

#### 无权最短路径

```python
class DirectedGraphNode:
    def __init__(self, x):
        self.label = x
        self.neighbors = []
        self.known = False
        self.dist = None
        self.path = None

    def __str__(self):
        return "(label: {}, dist: {})".format(self.label, self.dist)

    def __repr__(self):
        return str(self)


class Solution:
    """
    @param: graph: A list of Directed graph node
    @return: Any topological order for the given graph.
    """

    def shortest_path(self, graph, start_label):
        """ start_label代表起始点的label值 """
        start_g = [g for g in graph if g.label == start_label]
        if len(start_g) == 1:
            start_g[0].dist = 0
        else:
            print("wrong")
            return

        for curr_dist in range(len(graph)):
            for g in graph:
                if not g.known and g.dist == curr_dist:
                    g.known = True
                    for n in g.neighbors:
                        if n.dist is None:
                            n.dist = curr_dist + 1
                            n.path = g

    def shortest_path1(self, graph, start_label):
        """ bfs """
        start_g = [g for g in graph if g.label == start_label]
        if len(start_g) == 1:
            start_g[0].dist = 0
        else:
            print("wrong")
            return

        while start_g:
            g = start_g.pop(0)
            # 只是一个遍历过的标志没有其他作用
            g.known = True
            for n in g.neighbors:
                if n.dist is None:
                    n.dist = g.dist + 1
                    n.path = g
                    start_g.append(n)


connects = [
    (1, (2, 4)),
    (2, (4, 5)),
    (3, (1, 6)),
    (4, (3, 5, 6, 7)),
    (5, (7, )),
    (6, set()),
    (7, (6,))
]
l = [DirectedGraphNode(i) for i in range(1, 8)]

for i, ns in connects:
    for n in ns:
        l[i - 1].neighbors.append(l[n - 1])

s = Solution()

s.shortest_path(l, 3)
print(sorted([(i.label, i.dist) for i in l], key=lambda x: x[0]))

s.shortest_path1(l, 3)
print(sorted([(i.label, i.dist) for i in l], key=lambda x: x[0]))

```

