---
title: Leetcode 39. Combination Sum
date: 2023-12-11 22:29:21
tags: Backtracking
categories: Algorithm
---
练习地址：https://leetcode.com/problems/combination-sum/description/
## 题目描述
Given an array of distinct integers candidates and a target integer target, return a list of all unique combinations of candidates where the chosen numbers sum to target. You may return the combinations in any order.

The same number may be chosen from candidates an unlimited number of times. Two combinations are unique if the 
frequency
 of at least one of the chosen numbers is different.

The test cases are generated such that the number of unique combinations that sum up to target is less than 150 combinations for the given input.

 

Example 1:

Input: candidates = [2,3,6,7], target = 7
Output: [[2,2,3],[7]]
Explanation:
2 and 3 are candidates, and 2 + 2 + 3 = 7. Note that 2 can be used multiple times.
7 is a candidate, and 7 = 7.
These are the only two combinations.
Example 2:

Input: candidates = [2,3,5], target = 8
Output: [[2,2,2,2],[2,3,3],[3,5]]
Example 3:

Input: candidates = [2], target = 1
Output: []
 

Constraints:

1 <= candidates.length <= 30
2 <= candidates[i] <= 40
All elements of candidates are distinct.
1 <= target <= 40

**理解**
题目给出一个值范围在[1, 40]之间的数组和数字，要求从数组中任意组合元素，且每一项元素都能重复使用，元素之和等于目标值target，返回能组成target的各种组合。

## 思路：
这个问题可以通过回溯算法来解决，具体步骤如下：

1. 排序（可选）：

首先对数组进行排序。这不是必需的，但有助于优化，因为它可以让我们更早地结束当前递归。
2. 回溯：

递归地构建组合并尝试数组中的每个数字。
对于每个数字，我们可以选择“使用它”或“不使用它”：
“使用它”意味着将它加入当前组合中，然后基于新的总和继续探索。
“不使用它”意味着跳过当前数字，尝试下一个数字。
3. 退出递归中的循环：

如果当前组合的总和超过了目标数 target，则不再继续探索当前分支。
4. 保存结果：

当当前组合的总和等于目标数 target 时，将其添加到结果列表中。

## BackTracking 算法

回溯算法（Backtracking）是一种解决问题的方法，它涉及到递归地探索和尝试不同的解决方案，直到找到满足特定条件的解或确定没有解为止。这种算法经常用于解决组合问题、排列问题、划分问题，以及在某些搜索和优化问题中。它是深度优先搜索（DFS）的一个重要变体。

**基本原理**
1. 选择和探索：

在每一步，从可能的选项中选择一个选项，并继续向前探索。
2. 回溯：

如果当前选择不能达到目标或者不是有效解，就“回溯”到上一步，撤销当前选择，并尝试其他选项。
3. 递归结构：

回溯通常通过递归函数实现，因为每次选择后都进入一个新的状态，需要进一步探索。
4. 关键特点
系统地搜索：回溯算法会尝试所有可能的路径，以找到所有解或者特定的解。
剪枝优化：在搜索过程中，经常使用剪枝技巧来避免不必要的搜索，这是优化回溯算法性能的关键。
无后效性：当前选择的状态不受未来选择的影响。

**应用实例**
- 组合问题：比如求解集合的所有子集。
- 排列问题：比如求解所有可能的排列方式。
- 数独：填充数独板以满足特定规则。
- 八皇后问题：在8×8棋盘上放置八个皇后，使得它们互不攻击。
- 路径问题：在给定的图或网格中找到从起点到终点的路径。

**实现方法**
回溯算法通常可以使用以下伪代码结构来实现：

```
function backtrack(path, choiceList):
    if meet_end_condition(path):
        save_result(path)
        return

    for choice in choiceList:
        make_choice(choice)
        backtrack(path, choiceList)
        undo_choice(choice)
```
这个结构体现了回溯算法的核心：尝试每个可能的选择，继续前进，如果遇到死胡同，就回溯。

总的来说，回溯算法是一种非常强大的解决问题的方法，尤其适用于需要探索所有可能性的情况。由于其通常涉及大量的递归和可能的路径，因此在实际应用中，如何有效地剪枝以减少不必要的搜索成为优化这类算法的关键。

## Code
```
var combinationSum = function(candidates, target) {
    let ans = []
    candidates.sort((a, b) => a - b)
    recursive(0, target, [])
    function recursive(index, target, arr) {
        if(target === 0) {
            ans.push([...arr])
            return
        }
        if(target < 0) {
            return;
        }
        for(let i = index; i < candidates.length; i++) {
            if(candidates[i] > target) break;
            arr.push(candidates[i])
            recursive(i, target-candidates[i], arr)
            arr.pop()
        }
    }
    return ans
}
```
## 复杂度
**时间复杂度**
排序：O(n*logn)
回溯：最坏的情况下是O(2^n)，但由于添加了排序优化，实际复杂度远小于该值
整体时间复杂度：O(n*logn+2^n)

**空间复杂度**
这个题目的空间复杂度不是很好估计，可以看出临时数组和二维数组长度都取决于target大小和candidates的各项元素的值，长度和输入长度/大小没有直接的表示关系。
但递归占用的空间是明确的：O(n)