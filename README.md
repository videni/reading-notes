Rust笔记
=======
一些列Rust学习笔记

## [《Rust权威指南》](./the_rust_programming_language/index.md)

  Rust的学习曲线还是比较陡峭的，虽然这本书看了好几遍，但里面知识点总是记不住。

## [Rust by Examples](https://rustwiki.org/zh-CN/rust-by-example/index.html)

## [Rust reference](https://minstrel1977.gitee.io/rust-reference/introduction.html)
## [Async Rust](./async.md)


## 如何可视化debug rust程序?

[CodeLLDB](https://marketplace.visualstudio.com/items?itemName=vadimcn.vscode-lldb)
[rust-analyzer](https://marketplace.visualstudio.com/items?itemName=matklad.rust-analyzer)

## 如何编写Macro?

[quote](https://github.com/dtolnay/quote)

## [Rust design pattern](https://rust-unofficial.github.io/patterns/)


## [impl dyn ](https://radicle.community/t/rust-s-impl-dyn-trait-syntax/102)

## [Rust 中常见的有关生命周期的误解](https://github.com/pretzelhammer/rust-blog/blob/master/posts/translations/zh-hans/common-rust-lifetime-misconceptions.md#2-%E5%A6%82%E6%9E%9C-t-static-%E9%82%A3%E4%B9%88-t-%E7%9B%B4%E5%88%B0%E7%A8%8B%E5%BA%8F%E7%BB%93%E6%9D%9F%E4%B8%BA%E6%AD%A2%E9%83%BD%E4%B8%80%E5%AE%9A%E6%98%AF%E6%9C%89%E6%95%88%E7%9A%84)

生命周期有许多误区，多实践，多看慢慢消化。


## [Rust 标准库特性指南](https://github.com/pretzelhammer/rust-blog/blob/master/posts/translations/zh-hans/tour-of-rusts-standard-library-traits.md)

Rust有很多特性，阅读完《Rust权威指南》后必读。

## Rust中的锁

* 理解Linux中的 [spinlock](https://zhuanlan.zhihu.com/p/80727111) ,[rwlock和seqlock](https://zhuanlan.zhihu.com/p/94713372)
  
  rwlock的全称是"reader-writer spin lock"，和普通的spinlock不同，它对"read"和"write"的操作进行了区分。如果当前没有writer，那么多个reader可以同时获取这个rwlock。如果当前没有任何的reader，那么一个writer可以获取这个rwlock。

  seqlock, 其全称是"sequential lock"。相比起rwlock，它进一步解除了reader与writer之间的互斥，只保留了writer与writer之间的互斥。只要没有其他的writer持有这个seqlock（即便当前存在reader持有该seqlock），那么第一个试图获取该seqlock的writer就可以成功地持有。
* [spinlock vs Mutext](https://stackoverflow.com/questions/5869825/when-should-one-use-a-spinlock-instead-of-mutex)

* 最popular的库-[parking_lot](https://docs.rs/parking_lot/0.11.1/parking_lot/)