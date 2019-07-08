# Golang 线程和协程的区别

#### Golang 线程和协程的区别

　　备注：需要区分进程、线程\(内核级线程\)、协程\(用户级线程\)三个概念。

　**进程、线程 和 协程 之间概念的区别**

　　对于 **进程、线程**，都是有内核进行调度，有 CPU 时间片的概念，进行 **抢占式调度**（有多种调度算法）

　　对于 **协程**\(用户级线程\)，这是对内核透明的，也就是系统并不知道有协程的存在，是完全由用户自己的程序进行调度的，因为是由用户程序自己控制，那么就很难像抢占式调度那样做到强制的 CPU 控制权切换到其他进程/线程，通常只能进行 **协作式调度**，需要协程自己主动把控制权转让出去之后，其他协程才能被执行到。

　**goroutine 和协程区别**

　　**本质上，goroutine 就是协程。** 不同的是，Golang 在 runtime、系统调用等多方面对 goroutine 调度进行了封装和处理，当遇到长时间执行或者进行系统调用时，会主动把当前 goroutine 的CPU \(P\) 转让出去，让其他 goroutine 能被调度并执行，也就是 Golang 从语言层面支持了协程。Golang 的一大特色就是从语言层面原生支持协程，在函数或者方法前面加 go关键字就可创建一个协程。

　**其他方面的比较**

　　1. 内存消耗方面

　　　　每个 goroutine \(协程\) 默认占用内存远比 Java 、C 的线程少。  
　　　　_goroutine：_2KB   
　　　　线程：8MB

　　2. 线程和 goroutine 切换调度开销方面

　　　　线程/goroutine 切换开销方面，goroutine 远比线程小  
　　　　_线程：_涉及模式切换\(从用户态切换到内核态\)、16个寄存器、PC、SP...等寄存器的刷新等。  
　　　　_goroutine：_只有三个寄存器的值修改 - PC / SP / DX.

### 协程特点

1. 协程调度机制无法实现公平调度
2. 协程的资源开销是非常低的，一台普通的服务器就能支持百万协程，（协程kb，线程MB）

### 并发通信模型

共享内存和消息

#### 共享内存

```go
package main

import (
	"fmt"
	"sync"
	"runtime"
)

var counter int = 0

func Count(lock *sync.Mutex) {
	lock.Lock()	// 上锁
	counter++
	fmt.Println("counter =", counter)
	lock.Unlock()	// 解锁
}

func main() {
	lock := &sync.Mutex{}

	for i:=0; i<10; i++ {
		go Count(lock)
	}
	for {
		lock.Lock()	// 上锁
		c := counter
		lock.Unlock()	// 解锁

		runtime.Gosched() // 出让时间片

		if c >= 10 {
			break
		}
	}
}
```

#### 消息

```go
package main

import "fmt"

func Count(ch chan int) {
    ch <- 1
    fmt.Println("Counting")
}

func main() {

    chs := make([] chan int, 10)

    for i:=0; i<10; i++ {
        chs[i] = make(chan int)
        go Count(chs[i])
    }

    for _, ch := range(chs) {
        <-ch
    }
}
```



