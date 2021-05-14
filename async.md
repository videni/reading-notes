A series of resources about Rust async
======================================

## 理解Linux epoll

在学习Tokio之前，你应该至少理解linux epoll, 这是所有event loop的最基础的知识点。

* [epoll入门](https://zhuanlan.zhihu.com/p/36764771)
  
  这篇文章简单形象的解释epoll由来，傻瓜也能看懂。但缺少了一些高级概念，如触发模式。

* [epoll 触发模式](https://blog.csdn.net/lihao21/article/details/67631516)
  
* [epoll API](https://zh.wikipedia.org/wiki/Epoll)

* [How to use epoll? A complete example in C](https://web.archive.org/web/20111116155750/https://banu.com/blog/2/how-to-use-epoll-a-complete-example-in-c/)
  
  解释了select, poll, epoll的差异，以及ET,LT触发模式的区别，相当于对上面的文章的总结。如果你有C背景的话，可以看这篇文章代码演示，对我而言，阅读了上面的文章以后，这篇文章，只需要看一下上部分的解释即可，不需要关心代码实现。

## Tokio

* [Tokio internals](https://cafbit.com/post/tokio_internals/)
  