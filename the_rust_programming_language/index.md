《Rust 权威指南 》读书笔记
=====================
- [《Rust 权威指南 》读书笔记](#rust-权威指南-读书笔记)
  - [使用trait对象来存储不同类型的值](#使用trait对象来存储不同类型的值)
  - [生命周期](#生命周期)
  - [迷惑人的Package layout](#迷惑人的package-layout)
  - [Integration test](#integration-test)


## 使用trait对象来存储不同类型的值

```rust
Bod<dyn Draw>
```
动态分发
静态分发

动态分发的Trait对象，必须是对象安全的

* 方法的返回函数类型不能是Self
* 方法中不包含任何泛型参数


## 生命周期

任何引用都有一个生命周期，并且需要为使用引用的函数或结构体指定生命周期参数。

1. 声明周期语法
   
* 对函数而言：
关联一个函数中不同参数及返回值的生命周期的，
* 对结构体而言

2. 生命周期省略
* 每一个引用参数都会有自己的生命周期参数
* 当只存在一个输入生命周期参数是，这个生命周期会被赋予给所有的输出生命周期参数。
* 当拥有多个输入生命周期参数，而其中一个是&self或&mute self时，self的生命周期会被赋予给素有的输出生命周期参数。

## 迷惑人的Package layout

[lib.rs vs main.rs](https://stackoverflow.com/questions/57756927/rust-modules-confusion-when-there-is-main-rs-and-lib-rs)


## Integration test

目录结构，与src目录并列， 如
```
src
tests
```

1. tests目录每个文件都是一个包，如果不想被rust视作独立的包，可采用mod.rs命名规范。
2. 二进制包如src/main.rs无法导入作用域，只有库代码包（library crate）才可以。




