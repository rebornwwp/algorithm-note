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

### 拓扑排序

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

