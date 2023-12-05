---
title: Sort an Array
date: 2023-12-05 14:24:49
tags:
categories: Algorithms
---
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
