---
title: Data Structure Study -- Graph
date: 2023-12-15 10:00:59
tags: Graph
categories: Data Structure
---

## What is Graph Data Structure?

图是一种非线性数据结构，由顶点和边组成。顶点有时也被称为节点，边是连接图中任意两个节点的线或弧。更正式地，图由一组顶点（V）和一组边（E）组成。图用G（E，V）表示。
![image](https://media.geeksforgeeks.org/wp-content/uploads/20200630111809/graph18.jpg)

## 重要的Graph类型
无向图：图中的边没有方向，表示的是双向关系。例如，社交网络中的好友关系就是无向图的一个典型应用。
有向图：图中的边有明确的方向，表示单向关系。例如，网页之间的链接就可以用有向图来表示。
加权图：图的每条边都有一个权重（或者成本），用于表示边的重要性或者通过该边的代价。例如，地图中的路线图，边的权重可以表示距离或者时间。
无权图：图的边没有权重。
连通图：图中任意两个顶点都是连通的，即存在一条路径连接这两个顶点。
非连通图：图中存在至少一对顶点，它们之间没有路径。
完全图：图中任意两个不同的顶点都有一条边相连。
稀疏图：图中的边数远小于顶点数的最大可能边数。
稠密图：图中的边数接近于顶点数的最大可能边数。
树：是一种特殊的图，其中任意两个顶点之间只有一条路径。

## 如何存储Graph?
1. Adjacency Matrix
2. Adjacency List

## Graph基本操作
Insertion of Nodes/Edges in the graph – Insert a node into the graph.
Deletion of Nodes/Edges in the graph – Delete a node from the graph.
Searching on Graphs – Search an entity in the graph.
Traversal of Graphs – Traversing all the nodes in the graph.
## Graph的使用场景
1. 地图可以用图表表示，然后可以被计算机用来提供各种服务，比如两个城市之间的最短路径。
2. 当各种任务彼此依赖时，这种情况可以用有向无环图表示，我们可以使用拓扑排序找到任务可以执行的顺序。
3. 状态转移图表示当前状态下可以进行的合法移动。在井字棋游戏中可以使用这个方法。

## Graph的算法题
[841. Keys and Rooms](https://leetcode.com/problems/keys-and-rooms/?envType=study-plan-v2&envId=leetcode-75)
[1466. Reorder Routes to Make All Paths Lead to the City Zero](https://leetcode.com/problems/reorder-routes-to-make-all-paths-lead-to-the-city-zero/?envType=study-plan-v2&envId=leetcode-75)


## 总结