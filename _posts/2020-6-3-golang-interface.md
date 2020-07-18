---
title: golang interface
layout: posts
tags: programing_language
---

A methed can use interface as it's parameter type, and we can use a struct (or pointer to this struct) which implemnents this interface as parameter.

```golang
type I interface {
    a()
}

type S struct {
}
//not *S
func (p S) a() {}

func test1(b I) {
}

func test2(c *S) {
    test1(c)
}

func test3(c S) {
    test1(c)
}
```

Struct `S` implements interface `I`, method `test1`'s parameter is interface `I`,we can use `*S` and `S` as interface `I` ,shown in method `test2` and `test3`

If we modify the code a litte

```golang
type I interface {
    a()
}

type S struct {
}
//not S, we modified it
func (p *S) a() {}

func test1(b I) {
}

func test2(c *S) {
    test1(c)
}
//error
func test3(c S) {
    test1(c)
}
```

If we modify the method receiver to `*S`,then `test3` won't compile,`S` now is not interface `I`.

why?

[Method Sets](https://golang.org/ref/spec#Method_sets)

we already known that all method is equals to functions but use method receiver as it's first argument.

in golang a struct `T` and it's pointer `*T` and it has some methods. if the method receiver is `T` such as

```golang
func (t T) anyMethod(){}
```

then we can call `anyMethod()` on both `T` and `*T`. because `*T` has all methods that `T` has.

but if the method receiver is `*T`
such as

```golang
func (t *T) anyMethod(){}
```

then the method `anyMethod()` only receive `*T` , so `T` doesn't implements interface `I` then `test3` won't compile.
