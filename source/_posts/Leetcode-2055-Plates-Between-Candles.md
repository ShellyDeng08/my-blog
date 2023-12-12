---
title: Leetcode 2055. Plates Between Candles
date: 2023-12-11 14:10:10
tags: Prefix Sum
categories: Algorithm
---

## Code
```
var platesBetweenCandles = function(s, queries) {
    // 记录从0 -> i位置的蜡烛总数
    let prefixSum = [0]
    for(let i = 0; i < s.length; i++) {
        prefixSum[i+1] = s[i] === '|' ? prefixSum[i]+1 : prefixSum[i]
    }

    let leftCandle = [], rightCandle = []
    // 记录每个位置，最靠近它的左边的蜡烛位置
    for(let i = 0, lastCandle = -1; i < s.length; i++) {
        if (s[i] === '|') {
            lastCandle =  i
        }
        
        leftCandle[i] = lastCandle
    }

    for(let i = s.length - 1, lastCandle = -1; i >= 0; i--) {
        if (s[i] === '|') {
            lastCandle =  i
        }

        rightCandle[i] = lastCandle
    }
    let ans = []
    for(let item of queries) {
        // 合法区间，即把给的区间转换成由左右蜡烛包围的新区间，新区间一定是小于等于原区间的。
        let left = rightCandle[item[0]], right = leftCandle[item[1]]

        // 在这个范围中，左右都有蜡烛，且左蜡烛在右蜡烛左边，表示这个区间内是有盘子的
        if(left < right && left > -1 && right > -1) {
            // 盘子数量 = 区间长度 - 区间内蜡烛的数量
            // 区间内蜡烛的数量 = 前缀和[右边界]-前缀和[左边界]
            ans.push(right-left+1 - (prefixSum[right + 1] - prefixSum[left]))
        } else {
            ans.push(0)
        }
    }
    return ans
};
```