---
title: Amazon SDE Online Assessment Questions
date: 2023-12-13 12:48:51
tags: [Amazon亚马逊]
categories: Front-end Interview
---
## Code Question1
水果消消乐

![image](https://files.oaiusercontent.com/file-XoAwZ3GS8kQVPsrGnma8YawH?se=2023-12-13T21%3A30%3A02Z&sp=r&sv=2021-08-06&sr=b&rscc=max-age%3D3599%2C%20immutable&rscd=attachment%3B%20filename%3Dphoto-83348BFD-5B31-4F58-9C05-7D4A4378D67E.png&sig=HxEkhSdMnhjZywnn4apDqO9l3uX/lpefTWZon7TBWmc%3D)
题目给出一个包含数字的数组，每个数字代表一种水果，每次可以从数组中选出任意两个不相同的水果并消除。返回经过任意多次消除操作后，数组中还剩余的最少水果的数量。

例1：fruits = [3,3,1,1,2]
先挑出水果1和2消除 -> [3,3,1] -> 挑出水果1和3消除 -> [3]
最后只剩下1个水果，返回1

例2: fruits = [2 2 2 5 1 2]
先消除水果2和水果5 -> [2,2,1,2] -> 消除水果1和水果2 -> [2,2] 
最后剩下2个水果，返回2

### 思路
我的思路是利用栈结构保存未被消除的水果，如果当前元素和栈顶元素相同，则入栈，否则移除栈顶元素，最后返回栈的长度。
但在平台上有一个test case跑不过，也看不了，不知道大家有什么解法和思路吗？

## Code Question2
Delivery Center

![image](https://files.oaiusercontent.com/file-3hJg1oNsu4AAGQiyr2AFPHOY?se=2023-12-13T21%3A30%3A02Z&sp=r&sv=2021-08-06&sr=b&rscc=max-age%3D3599%2C%20immutable&rscd=attachment%3B%20filename%3Dphoto-35435DBE-E2D1-4DEB-A65C-07569A8122E7.jpeg&sig=OQU79mEpg5K89mIvoKolH4H5wDdF5zDLK%2BBcGDBrW80%3D)

题目给出一个数组center，center[i]表示配送中心在坐标上的点。距离d，要求在坐标上找到所以可以建仓库的点，这些点满足，从该点出发到所有的center，并从center返回该点，所有的距离只和不大于距离d。返回满足条件的点的数量。

注意题目没有给可选的仓库的距离范围, 仓库可以和配送中心在同一个点上

例：center = [-2, 1, 0], d = 8
对于点x=-3时，我们可以计算此时所有配送中心距仓库x=-3的来回总距离之和：|-2-(-3)|*2 + |1-(-3)|\*2 + |0-(-3)| * 2 = 2 + 8 + 6 = 16 > 8, 该点不满足。

对于点x=-1, 计算出距离：1*2 + 2\*2 + 1*2 = 8, 满足条件
对于点x=0, 计算出距离：2*2 + 1\*2 + 0 = 6，满足条件
对于点x=1, 计算出距离：3*2 + 0 + 1\*2 = 8， 满足条件
...
最后我们能找到三个点{-1,0,1}满足条件，返回3

### 思路
可以找到满足条件的最左边的点和最右边的点，两点之间的距离就是我们要找的答案。

而查找最左边或者最右边满足条件的点，可以使用二分查找法，具体步骤是：

1. 先确定要查找的左右边界，根据题目描述可以知道，有效范围一定在[min(center)-d, max(center)+d] 之间
2. 确定边界后，从左边查找满足条件的最左边的点 binarySearch(min(center)-d, max(center))。从右边查找满足条件的最右边的点binarySearch(min(center), max(center)+d)
3. 在查找的函数binarySearch(left, right)中，每次取两个边界中间的点mid，判断是否在合法的范围，如果此时distance > d, 则对于查找左边界的情况，我们重新调整范围为[left, mid-1]。即更靠近中心点的那一半
4. 而如果distance < d, 此时不能直接退出循环，因为这只能表示我们找到了一个满足条件的点，但这个点可能不是最左边的点。如果这个点满足条件&&这个点左边的点不满足条件，才能说明这个点是最左边的点。 
5. 这时候我们可以记录下这个点的位置，同时用一个flag标志我们已经找到了至少一个满足条件的点，同时把范围修改为[right, mid-1]继续查找。


## 代码实现
```
function findWarehouseNumber(center, d) {
    let min = Math.min(...center)
    let max = Math.max(...center)
    let start = min - d, end = max + d
    let left = binarySearch(start, max, 'Left')
    let right = binarySearch(min, end, 'Right')
    function binarySearch(left, right, flag) {
        let find = false
       while(left <= right) {
           let mid = Math.floor((right+left)/2)
           if (isDistanceLessD(mid)) {
            find = true
            // 找到之后，尝试往更远的地方查找
            if(flag === 'Left) {
                right = mid-1
            } else {
                left = mid+1
            }
           } else {
            // 没有找到，尝试往更靠近中心的位置查找
            if(flag === 'Left) {
                left = mid+1
            } else {
                right = mid-1
            }
           }
       }
       return find ? left : 0

   }
   function isDistanceLessD(p) {
       let ans = 0
       for(let value of center) {
           ans += Math.abs(p-value) * 2
           if(ans > d) return false
       }
       return ans <= d
   }
    return right - left
}
```

test
```
findWarehouseNumber([-2,1,0],8) //3
findWarehouseNumber([2, 0, 3, -4],22) //5
```