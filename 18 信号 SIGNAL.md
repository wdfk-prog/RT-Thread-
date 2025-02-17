# 18 信号 SIGNAL

- 信号（又称为软中断信号），在软件层次上是对中断机制的一种模拟，在原理上，一个线程收到一个信号与处理器收到一个中断请求可以说是类似的。
- 信号的工作机制

信号在 RT-Thread 中用作异步通信，POSIX 标准定义了 sigset_t 类型来定义一个信号集，然而 sigset_t 类型在不同的系统可能有不同的定义方式，在 RT-Thread 中，将 sigset_t 定义成了 unsigned long 型，并命名为 rt_sigset_t，应用程序能够使用的信号为 SIGUSR1（10）和 SIGUSR2（12）。

- 信号本质是软中断，用来通知线程发生了异步事件，用做线程之间的异常通知、应急处理。一个线程不必通过任何操作来等待信号的到达，事实上，线程也不知道信号到底什么时候到达，线程之间可以互相通过调用 rt_thread_kill() 发送软中断信号。
- 收到信号的线程对各种信号有不同的处理方法，处理方法可以分为三类：

  1. 第一种是类似中断的处理程序，对于需要处理的信号，线程可以指定处理函数，由该函数来处理。
  2. 第二种方法是，忽略某个信号，对该信号不做任何处理，就像未发生过一样。
  3. 第三种方法是，对该信号的处理保留系统的默认值。
- 信号工作机制

当信号被传递给线程 1 时，如果它正处于挂起状态，那会把状态改为就绪状态去处理对应的信号。如果它正处于运行状态，那么会在它当前的线程栈基础上建立新栈帧空间去处理对应的信号，需要注意的是使用的线程栈大小也会相应增加。

## 18.0 系统初始化

1. 创建一个内存池用于信号内存分配

## 18.1 初始化

1. 分配信号内存,用于回调函数
2. 通过 `rt_sighandler_t handler`选项,设置信号处理函数

## 18.2 解除信号阻塞

1. 信号屏蔽字 `sigmask`中的信号被解除阻塞
2. 当前线程处于挂起状态,则从挂起链表中移除,插入就绪链表中,执行一次调度

## 18.3 发送信号(rt_thread_kill)
