---
title: Leetcode 4. Median of Two Sorted Arrays
date: 2023-12-21 08:52:09
tags: Binary Search
categories: Algorithm
---
## 练习地址
Hard [4. Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/description/)

## 题目描述
找两个有序数组的中位数，要求时间复杂度为O(log(m+n))
## 题目类型
Binary Search 即二分搜索

## 思路
理解中位数的性质：

中位数是一组数排序后位于中间的数。如果数的个数是偶数，则中位数是中间两个数的平均值。
在两个排序数组中找中位数，意味着如果我们可以在两个数组中找到一个合适的切分点，使得左侧元素总数等于右侧元素总数（或者左侧元素比右侧元素多一个），那么我们就找到了中位数的位置。
使用二分搜索定位切分点：

假设两个数组分别是 A 和 B。为了简化问题，我们假设 A 的长度小于等于 B 的长度。这样我们只需要在较短的数组 A 上进行二分搜索。
定义两个指针 left 和 right 来表示我们在数组 A 中搜索切分点的范围。开始时，left = 0，right = len(A)。
在二分搜索过程中，我们取 mid = (left + right) / 2 作为数组 A 的切分点，并且根据 mid 来计算数组 B 中的切分点 midB = (len(A) + len(B) + 1) / 2 - mid。
我们需要检查 A[mid-1] <= B[midB] 和 B[midB-1] <= A[mid] 是否成立。如果这两个条件都满足，那么我们已经找到了合适的切分点。
如果 A[mid-1] > B[midB]，说明 A 的切分点需要左移，即调整 right = mid - 1。
如果 B[midB-1] > A[mid]，说明 A 的切分点需要右移，即调整 left = mid + 1。
计算中位数：

找到切分点后，中位数可以通过左侧部分的最大值和右侧部分的最小值来计算。
如果 m+n 是奇数，中位数就是左侧部分的最大值。
如果 m+n 是偶数，中位数是左侧部分的最大值和右侧部分的最小值的平均值。

**B的切分点是如何计算出来的？**
假设 A 和 B 的长度分别是 m 和 n。我们在 A 上进行二分搜索，找到一个切分点 i。基于这个切分点，我们可以计算出 B 中相应的切分点 j，使得左右两边的元素数量相等（或接近相等）。计算 j 的方法是：

`j = (m+n+1) / 2 - i`

这里，(m + n + 1) / 2 表示总元素数量的一半（对于奇数，它会向上取整，确保左边的元素数量总是大于或等于右边的元素数量）。从这个总数中减去 A 的切分点 i，我们得到 B 的切分点 j。

**我们为什么要检查A[mid-1] <= B[midB] 和 B[midB-1] <= A[mid]？**
按上所述，A 和 B 被分成两个部分：

A 的左半部分：A[0] 到 A[i-1]
A 的右半部分：A[i] 到 A[m-1]
B 的左半部分：B[0] 到 B[j-1]
B 的右半部分：B[j] 到 B[n-1]

我们需要确保所有左半部分的元素都小于等于右半部分的元素。这意味着：

A[i-1] <= B[j] 和 B[j-1] <= A[i]
如果这两个条件中的任何一个不满足，我们需要调整 A 的切分点 i 并重新计算 B 的切分点 j。

**如果切分点位于数组边界应该如何处理？**
1. 当 A[i-1] 不存在时：

这种情况发生在 i 等于 0 时，即 A 的切分点在数组的最开始位置。在这种情况下，可以认为 A[i-1] 代表负无穷大，因为它不存在，我们不需要比较 A[i-1] 和 B[j]。

2. 当 B[j-1] 不存在时：

这种情况发生在 j 等于 0 时，即 B 的切分点在数组的最开始位置。同样地，可以认为 B[j-1] 代表负无穷大，因此不需要比较 B[j-1] 和 A[i]。

3. 当 A[i] 不存在时：

如果 i 等于 A 的长度，意味着 A 的切分点在数组的最后位置。这时，A[i] 可以被认为是正无穷大。

4. 当 B[j] 不存在时：

如果 j 等于 B 的长度，意味着 B 的切分点在数组的最后位置。这时，B[j] 也可以被认为是正无穷大。
## 代码
```
function findMedianSortedArrays(nums1: number[], nums2: number[]): number {
    if(nums2.length <= 0 && nums1.length <= 0) {
        return 0
    }
    if(nums1.length > nums2.length) {
        const temp = nums1
        nums1 = nums2
        nums2 = temp
    }
    const len1 = nums1.length, len2 = nums2.length
    if(len1 <= 0) {
        let mid = Math.floor(len2/2)
        return len2 % 2 === 0 ? (nums2[mid] + nums2[mid-1]) / 2 : nums2[mid]
    }
    let left = 0, right = len1
    while(left <= right) {
        let mid1 = Math.floor((left+right)/ 2)
        let mid2 = Math.floor((len1+len2+1)/2 - mid1) 
        
        const nums1Left = mid1 - 1 >= 0 ? nums1[mid1-1] : -Infinity
        const nums1Right = mid1 >= len1 ? Infinity : nums1[mid1]
        const nums2Left = mid2 - 1 >= 0 ? nums2[mid2-1] : -Infinity
        const nums2Right = mid2 >= len2 ? Infinity : nums2[mid2]
        if(nums1Left <= nums2Right && nums2Left <= nums1Right) {
            // terminate loop
            if((len1+len2) % 2 === 0) {
                return (Math.max(nums1Left, nums2Left) + Math.min(nums1Right, nums2Right)) / 2
            }
            return Math.max(nums1Left, nums2Left)
        } else {
            // nums1的左边界比nums2的右边界大，nums1取[left, mid1]这一段
            if(nums1Left > nums2Right) {
                right = mid1 - 1
            } else {
                left = mid1 + 1
            }
        }

    }
};
```