---
title: Leetcode 1466. Reorder Routes to Make All Paths Lead to the City Zero
date: 2023-12-15 11:17:13
tags: Graph
categories: Algorithm
---
练习地址：[1466. Reorder Routes to Make All Paths Lead to the City Zero](https://leetcode.com/problems/reorder-routes-to-make-all-paths-lead-to-the-city-zero/)

## 题目理解

## 解法1 - DFS
- 有向图，每个城市经过一个路径后，可以到达城市0
- 将所有城市和城市间的路线视作一棵树，根节点是城市0，对于任意子树，都必须满足有一条路线能从子树访问到其父节点，层层往上，最终到达根节点。
- 用变量count来保存需要翻转的路径的数量
- 用一个邻接表来存储每个城市和另一个城市的路径，同时保存路径的方向，比如用0代表a通向b，用1代表b通向a。我们用0, 1来保存路径的方向，比如对于[0, 1]，我们会往邻接表里分别加入键为城市0和城市1，值为长度为2的数组。对于城市0，其和城市1联通，且这条路径是由0通往1的，我们计作1. adj[0] = [1, 1]。同理对于城市1，其和城市0联通，但路径方向是反的，我们计作adj[1] = [0, 0]
- 使用深度搜索，遍历城市，每次传入当前节点和父节点，在dfs中，我们遍历当前节点的邻接表元素，即我们前面存储的长度为2的数组arr, 如果数组中的节点等于父节点，表示这个节点我们已经访问过了。否则我们把arr[1]累加到count中。可以看到，如果方向是正向，则arr[1]会是0，不需要翻转也不会影响count的结果。
- 同时我们把arr[0]作为下一次dfs的目标节点，把这一次的目标节点作为下一次的父节点。
- 最后，我们会遍历所有的节点，并得到需要翻转的路径数量
### 代码
```
var minReorder = function(n, connections) {
    let adj = {}
    let count = 0
    for(let item of connections) {
        adj[item[0]] = adj[item[0]] || []
        adj[item[1]] = adj[item[1]] || []
        adj[item[0]].push([item[1], 1])
        adj[item[1]].push([item[0], 0])
    }
    function dfs(node, parent, adj) {
        for(let [child, sign] of adj[node]) {
            // 用child parent比较确保该元素没有被访问过
            if(child !== parent) {
                count += sign
                dfs(child, node, adj)
            }
        }
    }
    dfs(0, -1, adj)
    return count
};
```
### 复杂度
Time complexity: O(n).
我们需要 O(n) 时间来初始化邻接表。
dfs 函数访问每个节点一次，总共花费 O(n) 时间。 因为我们有无向边，所以每条边只能迭代两次（通过最后的节点），导致访问所有节点时总共需要 O(e) 次操作，其中 e 是边的数量。 因为给定的图是一棵树，有 n−1 个无向边，所以 O(n+e)=O(n)。

Space complexity: O(n).

构建邻接表需要 O(n) 空间。
在最坏的情况下，dfs 使用的递归调用堆栈最多可以有 n 个元素。 在这种情况下它将占用 O(n) 空间。O(2n) = O(n)
## 解法2 - BFS