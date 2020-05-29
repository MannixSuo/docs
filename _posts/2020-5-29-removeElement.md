---
title: removeElemets
layout: posts
tags: algorithms
---

## removeElemets

给你一个数组 nums 和一个值 val，你需要 原地 移除所有数值等于 val 的元素，并返回移除后数组的新长度。

不要使用额外的数组空间，你必须仅使用 O(1) 额外空间并 原地 修改输入数组。

元素的顺序可以改变。你不需要考虑数组中超出新长度后面的元素。

解法1：

两个动点currentIndex ,nextIndex 其中nextIndex依次往后遍历，判断位于nextIndex的元素是否和val相等，如果相等nextIndex++在判断，如果不相等将该元素放到currentIndex的位置然后currentIndex++，nextIndex++

`_____currentIndex>__________nextIndex>________`

```go
func removeElement(nums []int, val int) int {
    var currentIndex, nextIndex int
    currentIndex = 0
    for nextIndex = 0; nextIndex < len(nums); nextIndex++ {
        if nums[nextIndex] == val {
            continue
        }
        nums[currentIndex] = nums[nextIndex]
        currentIndex++
    }
    return currentIndex
}
```

## strStr

给定一个 haystack 字符串和一个 needle 字符串，在 haystack 字符串中找出 needle 字符串出现的第一个位置 (从0开始)。如果不存在，则返回  -1。

解法1:

遍历haystack中的每个字符，如果和needle的第一个相等，在判断下一个，如果不相等haystackIndex++

```golang

func strStr(haystack string, needle string) int {
    if needle == "" {
        return 0
    }
    var haystackIndex, needleIndex, haystackIndexCopy int
    for haystackIndex = 0; haystackIndex < len(haystack); haystackIndex++ {
        haystackIndexCopy = haystackIndex
        for needleIndex = 0; needleIndex < len(needle) && haystackIndexCopy < len(haystack); needleIndex++ {
            if haystack[haystackIndexCopy] != needle[needleIndex] {
                break
            }
            haystackIndexCopy++
            if needleIndex == len(needle)-1 {
                return haystackIndex
            }
        }
    }
    return -1
}
```

解法2：

字符串前后两端都进行判断。

```golang
func strStr(haystack string, needle string) int {
    if needle == "" {
        return 0
    }
    haystackLen, needleLen := len(haystack), len(needle)
    var haystackIndex, needleIndex, haystackIndexCopy, needleLenCopy int
    for haystackIndex = 0; haystackIndex <= haystackLen-needleLen; haystackIndex++ {
        haystackIndexCopy = haystackIndex
        needleLenCopy = needleLen - 1
        for needleIndex = 0; needleIndex < needleLen && haystackIndexCopy < haystackLen; needleIndex++ {
            if haystack[haystackIndexCopy] != needle[needleIndex] {
                break
            }
            if haystack[haystackIndexCopy+needleLenCopy] != needle[needleLenCopy] {
                break
            }
            haystackIndexCopy++
            needleLenCopy--
            if haystackIndexCopy > needleLenCopy {
                return haystackIndex
            }
        }
    }
    return -1
}
```
