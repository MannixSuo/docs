---
title: removeDuplicates
layout: posts
tags: algorithms
---

给定一个排序数组，你需要在 原地 删除重复出现的元素，使得每个元素只出现一次，返回移除后数组的新长度。

不要使用额外的数组空间，你必须在 原地 修改输入数组 并在使用 O(1) 额外空间的条件下完成。

解法1：
三个动点：i j k.
i是要扫描的点,j是数组前面不重复元素的坐标,k是判断0-j中的元素是否和i相等。
`___k__j_i_____`

```golang

func removeDuplicates(nums []int) int {
    j := 1
    for i := j; i < len(nums); i++ {
        for k := 0; k < j; k++ {
            if nums[k] == nums[i] {
                break
            }
            // 如果到最后还没有相等,说明不是重复元素
            if k == j-1 {
                nums[j] = nums[i]
                j++
            }
        }
    }
    return j
}

```

```golang
func removeDuplicates(nums []int) int {
    j := 0
    for i := 0; i < len(nums); i++ {
        if !contains(nums, j, nums[i]) {
            nums[j] = nums[i]
            j++
        }
    }
    return j
}

func contains(nums []int, length int, number int) bool {
    for i := 0; i < length; i++ {
        if nums[i] == number {
            return true
        }
    }
    return false
}
```

解法2：

解法一的改进版：

四个动点 nextIndex是要扫描并判断是否重复的点
currentIndex是数组前面不重复元素的个数

start end

`0_____start>_____<end_____currentIndex>__nextIndex>___len`中的坐标,应该比解法一快

```golang

func removeDuplicates(nums []int) int {
    nextIndex := 0
    currentIndex := 0
    start := 0
    end := 0
    for nextIndex = currentIndex; nextIndex < len(nums); nextIndex++ {
        for start = 0; start <= currentIndex; start++ {
            end = currentIndex - start - 1
            if start > end {
                nums[currentIndex] = nums[nextIndex]
                currentIndex++
                break
            }
            if nums[end] == nums[nextIndex] || nums[start] == nums[nextIndex] {
                break
            }
        }
    }
    return currentIndex
}
```
