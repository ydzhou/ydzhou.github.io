---
layout: default
title: 浅谈 Go 的并发
---

## 为什么并发是一个难题？

并发的 bug 往往隐蔽，罕见且难以在非生产环境中复现。这就造成了涉及并发的编程本身是一个难题。

### Race Condition

当多个操作必须严格按照先后顺序执行的时候，这个顺序却无法被保证。比如同时写数据库中的一项。

### Atomicity

一个操作是不能被再分割，不能被打断的。但是很多时候，我们以后的 atomic 操作，实际却并非 atomic，这个时候就出现问题了。比如
```
i := i + 1
```
看似只是一个 atomic 的加法操作，但实际涉及到读取 `i` 的 value，增加 `i` 的 value 和储存 `i` 的 value 这三个 atomic 操作。

### Deadlocks

并发中 lock 的概念是指未避免 data race 而只允许一个 process 操作这个 resource。
Deadlocks 指的是多个 process 互相等待，无法完成操作。比如 A 在等待 B 释放它 lock 住的 resource，B 也在等待 A 的释放。但是只有当 B 释放了 resource，A 完成操作才能释放 B 所需要的 resource。

### Starvation

Starvation 指的是一个 process 一直无法得到必须的 resource 进行操作。大概率这是 scheduling 或者 mutual exclusion 导致的问题。比如之前提到的 Deadlocks，就会导致 Starvation。

## Go 对于并发编程的独特性

Go 提供了 lock 和 [channel](https://gobyexample.com/channels) 两个主要方法应对并发。

> Aim for simplicity, use channels when possible, and treat coroutines like a free resource

Channel 是 Go 独有的 language-level concurrency primitive 或者说 high-level way of tackling concurrency。他的目的是简化对于并发的处理，让我们不再去烦恼上面提到的那些问题。

当然 Channel 并不适用于所有场合，以下情况，更推荐使用 lock：
1. 性能非常重要
2. 你需要保证 `struct` 内部某些范围内的 atomicity

> Share memory by communicating, do not communicate by sharing memory

Go 的一句名言讲出了 channel 的设计理念，提供一个安全的渠道来在不同 process 间交换信息。channel 让不同的 goroutine 可以相互交流，且确保了进列和出列的 atomicity。

下面是一段实例代码
```
package main

import (
	"fmt"
)

func main() {
    inputc := make(chan int)
    outputc := make(chan int)

    go func() {
        inputc <- 1
        inputc <- 2
    } ()

    go func() {
        for i := 0; i < 2; i +=1 {
            num := <-inputc
            outputc <- (num*num)
        }
    }()
    
    for i := 0; i < 2; i +=1 {
        fmt.Println(<-outputc)
    }
}
```
 

