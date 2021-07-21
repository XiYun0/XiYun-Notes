# Rust入门

文档：[Rust 官方文档中文教程 ](https://rustwiki.org/)/ 对应视频：https://www.bilibili.com/video/BV1hp4y1k7SV

官网：https://www.rust-lang.org/zh-CN/

## 开始

下载clion

创建main.rs

```rust
fn main() {
    println!("你好");
}
```

```
cargo run
```

```
glong@glongdeMacBook-Air rust-demo % cargo run
   Compiling rust-demo v0.1.0 (/Users/glong/project/rust-demo)
    Finished dev [unoptimized + debuginfo] target(s) in 0.52s
     Running `target/debug/rust-demo`
你好
```

## cargo

> cargo类似Java的maven，帮助我们管理第三方库

```bash
cargo --version
cargo 1.53.0 (4369396ce 2021-04-27)
```

使用cargo创建项目

```bash
cargo new hello_cargo
```

### cargo.toml

```
[package]
name = "hello_cargo"
version = "0.1.0"
edition = "2018"

# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html

[dependencies]
```

其中

```
- name，项目名
- version, 项目版本
- authors， 项目作者
- edition，使用的Rust版本
```

添加依赖，如

```
[dependencies]
rand = "0.3.14"
```

速度非常慢，需要修改cargo镜像源

```
cd /Users/glong/.cargo
```

```
vim config


[source.crates-io]
registry = "https://github.com/rust-lang/crates.io-index"
# 指定镜像
replace-with = 'tuna'

# 清华大学
[source.tuna]
registry = "https://mirrors.tuna.tsinghua.edu.cn/git/crates.io-index.git"
```

### cargo.lock

锁住依赖

### 命令

```bash
cargo build
	cargo build --release	发布使用
cargo check
cargo run

cargo update // 忽略cargo.lock里面的内容，更新依赖
```

## 编写 猜猜看

```rust
use std::io;    // prelude
fn main() {
    println!("猜数字");

    println!("猜测一个数");

    /**
    rust变量默认不可变，所以需要使用mut
    **/

    // let mut foo = 1;
    // let bar = foo;
    // foo = 2;

    let mut guess = String::new();  // ::表示关联函数，类似于java的static静态函数

    io::stdin().read_line(&mut guess).expect("无法读取行");
    // io::Result Ok, Err
    println!("你猜测的数数: {}", guess);
}
```





### 比较大小

```rust
use std::io;    // prelude
use std::cmp::Ordering;
use rand::Rng;  // trait Rng为随机生成数
fn main() {
    println!("猜数字");

    let select_number = rand::thread_rng().gen_range(1, 101);
    println!("神秘数字数 {}", select_number);

    println!("猜测一个数");

    let mut guess = String::new();  // ::表示关联函数，类似于java的static静态函数

    io::stdin().read_line(&mut guess).expect("无法读取行");

    // parse解析，可能是u8，u32等到，所以前面要显示声明变量类型 : u32
    let guess: u32 = guess.trim().parse().expect("Please type a number!");
    // io::Result Ok, Err
    println!("你猜测的数数: {}", guess);

    // match类似于switch，但是它更是一种模式匹配
    match guess.cmp(&select_number) {
        Ordering::Less => println!("Too small!"),
        Ordering::Greater => println!("To big"),
        Ordering::Equal => println!("You win"),
    };
}
```

### 多次猜测

上面我们每猜测一次都要重新开启，现在使用循环

```rust
use std::io;    // prelude
use std::cmp::Ordering;
use rand::Rng;  // trait Rng为随机生成数
fn main() {
    println!("猜数字");

    let select_number = rand::thread_rng().gen_range(1, 101);
    println!("神秘数字数 {}", select_number);


    loop {
        println!("猜测一个数");

        let mut guess = String::new();  // ::表示关联函数，类似于java的static静态函数

        io::stdin().read_line(&mut guess).expect("无法读取行");

        // parse解析，可能是u8，u32等到，所以前面要显示声明变量类型 : u32
        let guess: u32 = match guess.trim().parse(){
            Ok(num) => num,
            Err(_) => continue,
        };
        // io::Result Ok, Err
        println!("你猜测的数数: {}", guess);

        // match类似于switch，但是它更是一种模式匹配
        match guess.cmp(&select_number) {
            Ordering::Less => println!("Too small!"),
            Ordering::Greater => println!("To big"),
            Ordering::Equal => {
                println!("You win");
                break;
            },
        };
    }
}
```

# Rust常见编程概念