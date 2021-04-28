《[Rust 权威指南](https://kaisery.github.io/trpl-zh-cn/)》读书笔记
=====================
- [《Rust 权威指南》读书笔记](#rust-权威指南读书笔记)
  - [注释](#注释)
  - [RefCell<T> 在运行时记录借用](#refcellt-在运行时记录借用)
    - [RefCellT> vs Cell<T>](#refcellt-vs-cellt)
  - [解引用运算符](#解引用运算符)
    - [基本使用](#基本使用)
    - [作用于Box<T>智能指针](#作用于boxt智能指针)
      - [Box<T> 使用场景](#boxt-使用场景)
  - [为类型实现trait](#为类型实现trait)
  - [String VS str](#string-vs-str)
  - [并发](#并发)
    - [Mutex(Mutual exclusion)](#mutexmutual-exclusion)
    - [Rc VS Arc](#rc-vs-arc)
    - [Rc(Reference counter), 引用计数智能指针](#rcreference-counter-引用计数智能指针)
    - [Arc(Atomic counter), 原子引用计数](#arcatomic-counter-原子引用计数)
    - [Send & Sync trait](#send--sync-trait)
  - [函数指针与闭包](#函数指针与闭包)
    - [函数指针(function pointer)](#函数指针function-pointer)
    - [闭包](#闭包)
  - [使用trait对象来存储不同类型的值](#使用trait对象来存储不同类型的值)
  - [生命周期](#生命周期)
  - [迷惑人的Package layout](#迷惑人的package-layout)
  - [Integration test](#integration-test)
  - [Rust clippy](#rust-clippy)

## 注释

* // 描述实现细节
* /// 描述使用方法，支持Markdown语法
* //! 为整个包括整个模块提供文档

## RefCell<T> 在运行时记录借用

当创建不可变和可变引用时，我们分别使用 & 和 &mut 语法，然而，对于 RefCell<T> 来说，则是 borrow 和 borrow_mut 方法。borrow 方法返回 Ref<T> 类型的智能指针，borrow_mut 方法返回 RefMut 类型的智能指针。这两个类型都实现了 Deref，所以可以当作常规引用对待。
RefCell<T> 记录当前有多少个活动的 Ref<T> 和 RefMut<T> 智能指针。每次调用 borrow，RefCell<T> 将活动的不可变借用计数加一。当 Ref<T> 值离开作用域时，不可变借用计数减一。记住：RefCell<T>在运行时确保借用规则，即“任何时候只允许有多个不可变借用或一个可变借用”。

### [RefCellT> vs Cell<T>](https://blog.iany.me/2019/02/rust-cell-and-refcell/)

## 解引用运算符
### 基本使用
常规引用是一个指针类型，一种理解指针的方式是将其看成指向储存在其他某处值的箭头. 示例
```
fn main() {
    let x = 5;
    let y = &x;

    assert_eq!(5, x);
    assert_eq!(5, *y);
}
```
变量 x 存放了一个 i32 值 5。y 等于 x 的一个引用。可以断言 x 等于 5。然而，如果希望对 y 的值做出断言，必须使用 *y 来追踪引用所指向的值（也就是 解引用）
如果尝试编写 assert_eq!(5, y);，则会得到如下编译错误
```
error[E0277]: can't compare `{integer}` with `&{integer}`
 --> src/main.rs:6:5
  |
6 |     assert_eq!(5, y);
  |     ^^^^^^^^^^^^^^^^^ no implementation for `{integer} == &{integer}`
  |
  = help: the trait `std::cmp::PartialEq<&{integer}>` is not implemented for
  `{integer}`
```
### 作用于Box<T>智能指针
```
fn main() {
    let x = 5;
    let y = Box::new(x);

    assert_eq!(5, x);
    assert_eq!(5, *y);
}
```
一样能工作，因为Box实现了Deref trait.
没有 Deref trait 的话，编译器只会解引用 & 引用类型。deref 方法向编译器提供了获取任何实现了 Deref trait 的类型的值，并且调用这个类型的 deref 方法来获取一个它知道如何解引用的 & 引用的能力。

#### Box<T> 使用场景

* 当有一个在编译时未知大小的类型，而又想要在需要确切大小的上下文中使用这个类型值的时候
* 当有大量数据并希望在确保数据不被拷贝的情况下转移所有权的时候
* 当希望拥有一个值并只关心它的类型是否实现了特定 trait 而不是其具体类型的时候，又被称为 trait 对象（trait object)

## 为类型实现trait


孤儿规则： 只有当trait或类型定义于我们的库中时，我们才能为类型实现对应的trait。

比如，我们不能在自己的库中，为Vec<T>实现Display trait，因为Vec<T>与Display都定义在外部（标准库中）。
## [String VS str](https://www.ameyalokare.com/rust/2017/10/12/rust-str-vs-String.html)

String在堆上分配，str不可变，固定长度，在二进制文件中。

## 并发

### Mutex(Mutual exclusion)

在任意时刻只允许一个线程访问数据
* 必须在使用数据前获取锁
* 在使用完互斥体守护的数据后释放锁。
  

### Rc VS Arc
### Rc(Reference counter), 引用计数智能指针

在Clone时，增加引用计数，在克隆出的实例被丢弃时，减少引用计数。

1. 为单个值赋予多个所有者
2. 非线程安全

### Arc(Atomic counter), 原子引用计数

* 线程安全
### Send & Sync trait 

1. Send trait 允许在线程间转移所有权

* 除了Rc<T>等极少数类型外，几乎所有类型都实现了Send Trait

2. Sync trait 允许被多个线程以引用的方式访问

* 使用实现了Sync trait的类型才能安全的被多个线程引用。
  
##  函数指针与闭包

### 函数指针(function pointer)

1. 函数在传递过程中会被强制转换为fn类型，fn类型是所谓的函数指针，函数指针与Fn trait是不一样的。 如
```
fn add_one(x: i32) -> i32 {
    x + 1
}

fn do_twice(f: fn(i32) -> i32, arg: i32) -> i32 {
    f(arg) + f(arg)
}

fn main() {
    let answer = do_twice(add_one, 5);

    println!("The answer is: {}", answer);
}
```

2. 函数指针实现了3中闭包Trait（Fn, FnMut,FnOnce）, 所以总是可以将函数指针作为参数传递给一个接收闭包的函数。

* Fn 从环境中不可变的借用值
* FnMut 从环境中可变的借用值。
* FnOnce 获取在环境的变量所有权。

### 闭包

闭包是通过Trait来表达的。

[无法在函数中直接返回一个闭包](https://doc.rust-lang.org/book/ch19-05-advanced-functions-and-closures.html#returning-closures)。如
```rust
fn returns_closure() -> dyn Fn(i32) -> i32 {
    |x| x + 1
}
```

Note: dyn - dynamic dispatch to a trait object

解决这问题可以[使用trait对象](#使用trait对象来存储不同类型的值)。
```rust
fn returns_closure() -> Box<dyn Fn(i32) -> i32> {
    Box::new(|x| x + 1)
}
```
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

## [Rust clippy](https://rust-lang.github.io/rust-clippy/master/)
