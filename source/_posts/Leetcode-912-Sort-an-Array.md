---
title: Leetcode 912. Sort an Array
date: 2023-12-05 14:24:49
tags: Sort
categories: Algorithm
---
练习地址：
# Merge Sort 归并排序
思路：归并排序实际是分治法，将待排序的数组从中间分割成两部分，直到每个子数组只有一个元素或者没有元素。递归地对两个子数组进行归并排序。将两个已经排序的子数组合并成一个有序的数组。

步骤：
1. 递归地分割数组：

如果数组的长度大于1，则找到数组的中间点并将其分割成两个子数组。
对这两个子数组分别进行归并排序。

2. 合并步骤：

创建一个临时数组，用于存储合并后的有序元素。
比较两个子数组的元素，每次从两个子数组中选取较小的元素放入临时数组。
重复上述过程，直到两个子数组中的所有元素都被合并到临时数组中。
将临时数组中的元素复制回原数组，这样原数组就成为了有序数组。

## 复杂度
时间复杂度：O(n*log n)
空间复杂度：O(n)

## 举例
arr=[5,4,8,1,2,6]
递归1: arr1=sortArray([5,4,8]), arr2=sortArray([1,2,6])
            |
    递归2: arr1=sortArray([5]), arr2=sortArray([4,8])
        递归3: 由于数组[5]长度为1，直接返回数组，所以arr1=[5]. arr2继续递归
            递归4: arr1=sortArray([4]), arr2=sortArray([8]), 两个数组长度都为1，都直接返回数组，所以arr1=[4], arr2=[8]
            接着调用mergeSort([4],[8]), 在mergeSort()中遍历两个数组并声明一个临时数组，依次比较两个数组的元素，每次将较小的那个值加入临时数组中。
            合并完成后得到：[4, 8]并返回
        退出递归3，回到递归2继续调用mergeSort([5],[4,8]), 合并后得到[4,5,8]
    退出递归2，回到递归1。此时递归1中的arr2也会接着执行上面操作，得到[1,2,6]
    再次调用mergeSort([4,5,8], [1,2,6])合并后：[1,2,4,5,6,8]
返回结果
## 代码实现
Go:
```
func sortArray(nums []int) []int {
    if len(nums) <= 1 {
        return nums
    }
    mid := len(nums) / 2
    arr1 := sortArray(nums[:mid])
    arr2 := sortArray(nums[mid:])

    return mergeSort(arr1, arr2)
    
}

func mergeSort(arr1, arr2 []int) []int {
    i := 0
    j := 0
    ans := []int{}
    for i < len(arr1) && j < len(arr2) {
        if arr1[i] <= arr2[j] {
            ans = append(ans, arr1[i])
            i++
        } else {
            ans = append(ans, arr2[j])
            j++
        }
    }
    for i < len(arr1) {
        ans = append(ans, arr1[i])
        i++
    }
    for j < len(arr2) {
        ans = append(ans, arr2[j])
        j++
    }
    return ans
}
```
# Quick Sort 快速排序
思路：快速排序是一种高效的排序算法，也是基于分治法的。它的基本思路是选择一个元素作为“基准”（pivot），然后对数组进行划分，将所有小于基准的元素移动到基准的左边，将所有大于基准的元素移动到基准的右边。这一步骤称为“划分”（partition）。之后，递归地在基准左边和右边的子数组上重复这个过程。
步骤：
选择基准：从数组中选择一个元素作为基准。基准的选择可以有多种方式，比如选择第一个元素、最后一个元素、中间的元素，或者随机选择一个元素。

划分操作：

重新排列数组，使得所有小于基准的元素都移到基准的左边，所有大于基准的元素都移到基准的右边。等于基准的元素可以放在任何一边。
在划分结束后，基准元素所处的位置就是它最终排序后的位置。
递归排序：

递归地对基准左边和右边的子数组进行快速排序。
递归的基本情况是子数组的大小为0或1，这时子数组已经是排序好的。
## 复杂度
时间复杂度：快速排序在平均和最好情况下的时间复杂度是O(n log n)，在最坏情况下是O(n^2)
空间复杂度：空间复杂度取决于递归调用的深度，通常是O(log n)

## 代码实现
```
function sortArray(nums: number[]): number[] {
  quickSort(nums, 0, nums.length-1)
  return nums
}
function quickSort(nums, left, right) {
  if (right <= left) return
  
  const pivotIdx = partition(nums, left, right)
  quickSort(nums, left, pivotIdx-1)
  quickSort(nums, pivotIdx+1, right)
}

function partition(nums, left, right): number {
  // 选取右边第一个元素为基准元素
  const pivot = nums[right]
  // 左子数组的边界
  let j = left
  for (let i = left; i < right; i++) {
    if (nums[i] < pivot) {
      [nums[i], nums[j]] = [nums[j], nums[i]]
      j++
    }
  }
  [nums[j], nums[right]] = [nums[right], nums[j]]
  return j
}
```