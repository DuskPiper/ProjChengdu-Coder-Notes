# Shortest Path 最短路径

从图中的某个顶点出发到达另外一个顶点的所经过的边的权重和最小的一条路径，称为最短路径。算法具体的形式包括：

- **确定起点的最短路径问题**：即已知起始结点，求最短路径的问题。适合使用[Dijkstra算法](https://zh.wikipedia.org/wiki/Dijkstra%E7%AE%97%E6%B3%95)。
- **确定终点的最短路径问题**：与确定起点的问题相反，该问题是已知终结结点，求最短路径的问题。在[无向图](https://zh.wikipedia.org/wiki/%E7%84%A1%E5%90%91%E5%9C%96)中该问题与确定起点的问题完全等同，在[有向图](https://zh.wikipedia.org/wiki/%E6%9C%89%E5%90%91%E5%9B%BE)中该问题等同于把所有路径方向反转的确定起点的问题。
- **确定起点终点的最短路径问题**：即已知起点和终点，求两结点之间的最短路径。
- **全局最短路径问题**：求图中所有的最短路径。适合使用[Floyd-Warshall算法](https://zh.wikipedia.org/wiki/Floyd-Warshall%E7%AE%97%E6%B3%95)。

本文讨论以下几种算法：

- **Floyd-Warshall 算法**：是解决任意两点间的最短路径，可以处理有向图或负权(不允许负权回路)。时间复杂度高达 $O(V^3)$。
- **Dijkstra算法**：BFS解决赋权有向图的单源最短路径问题。
- **Bellman-Ford算法**：对图进行 $|V|-1$ 次松弛操作，得到所有可能的最短路径。优于Dijkstra算法的是允许负权边，缺点是时间复杂度高达 $O(|V||E|)$。



## Floyd-Warshall 算法

pass

##Dijkstra 算法

使用了广度优先搜索解决赋权有向图或者无向图的单源最短路径问题，算法最终得到一个最短路径树。

1. 初始时，设置集合S只包含源点，v的距离为0。U包含除v外的其他顶点，若v与U中顶点u有边，则正常有权值，若u不是v的出边邻接点，则权值为$∞$。
2. 从U中选距离v最小的顶点k，加入S中（该选定的距离就是v到k的最短路径长度）。
3. 以k为新中间点，修改U中各顶点的距离；若从源点v到顶点u的距离（经过顶点k）比原来距离（不经过顶点k）短，则修改顶点u的距离值，修改后的距离值的顶点k的距离加上边上的权。
4. 重复b和c直到所有顶点都包含在S中。

### 思路





## References

- [WikiPedia](https://en.wikipedia.org/wiki/Shortest_path_problem)
- [最短路径问题---Dijkstra算法详解](https://blog.csdn.net/qq_35644234/article/details/60870719)
- [数据结构与算法(14) 最短路算法](https://plushunter.github.io/2017/07/28/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E4%B8%8E%E7%AE%97%E6%B3%95%EF%BC%8814%EF%BC%89%EF%BC%9A%E6%9C%80%E7%9F%AD%E8%B7%AF%E7%AE%97%E6%B3%95/)

