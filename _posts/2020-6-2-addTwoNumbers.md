---
title: addTwoNumbers
layout: posts
tags: algorithms
---

给出两个 非空 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 逆序 的方式存储的，并且它们的每个节点只能存储 一位 数字。

如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。

您可以假设除了数字 0 之外，这两个数都不会以 0 开头。

解法1：

依次将列表中对应的元素相加

```golang
func addTwoNumbers(l1 *ListNode, l2 *ListNode) *ListNode {
    result := &ListNode{}
    current := result
    over, l1value, l2value := 0, 0, 0
    for l1 != nil || l2 != nil || over != 0 {
        if l1 != nil {
            l1value = l1.Val
            l1 = l1.Next
        }
        if l2 != nil {
            l2value = l2.Val
            l2 = l2.Next
        }
        current.Val = (l1value + l2value + over) % 10
        over = (l1value + l2value + over) / 10
        if l1 != nil || l2 != nil || over != 0 {
            next := &ListNode{}
            current.Next = next
            current = next
        }
        l1value, l2value = 0, 0
    }
    return result
}
```

解法2：

先算出两个所代表的的值，然后相加，然后再存到列表中

测试未通过：原因：参数太大，转换时会溢出导致结果不正确

```golang
func addTwoNumbers(l1 *ListNode, l2 *ListNode) *ListNode {
    result := &ListNode{}
    current := result
    l1val := getActualNumber(l1)
    l2val := getActualNumber(l2)
    total := l1val + l2val
    for total > 0 {
        current.Val = total % 10
        total = total / 10
        if total > 0 {
            next := &ListNode{}
            current.Next = next
            current = next
        }
    }
    return result
}

func getActualNumber(l *ListNode) int {
    pow := 1
    lvalue := 0
    for l != nil {
        lvalue += l.Val * pow
        pow = pow * 10
        l = l.Next
    }
    return lvalue
}

```
