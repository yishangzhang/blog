## match 
1. 基本匹配
```rust
fn main() {
    let number = 13;

    match number {
        1 => println!("One"),
        2 => println!("Two"),
        3 => println!("Three"),
        13 => println!("Lucky 13"),
        _ => println!("Other"),
    }
}
```
2. 匹配范围
```rust
fn main() {
    let number = 25;

    match number {
        1..=10 => println!("Between 1 and 10"),
        11..=20 => println!("Between 11 and 20"),
        21..=30 => println!("Between 21 and 30"),
        _ => println!("Other"),
    }
}
```
3. 匹配元组
```rust
fn main() {
    let pair = (0, -2);

    match pair {
        (0, y) => println!("First is 0 and y is {}", y),
        (x, 0) => println!("x is {} and last is 0", x),
        _ => println!("It doesn't matter what they are"),
    }
}
```
4. 匹配结构体
```rust
struct Point {
    x: i32,
    y: i32,
}

fn main() {
    let point = Point { x: 0, y: 7 };

    match point {
        Point { x, y: 0 } => println!("On the x axis at {}", x),
        Point { x: 0, y } => println!("On the y axis at {}", y),
        Point { x, y } => println!("On neither axis: ({}, {})", x, y),
    }
}
```
5. 匹配枚举
```rust
enum Message {
    Quit,
    Move { x: i32, y: i32 },
    Write(String),
    ChangeColor(i32, i32, i32),
}

fn main() {
    let msg = Message::Move { x: 10, y: 20 };

    match msg {
        Message::Quit => println!("The Quit variant has no data to destructure."),
        Message::Move { x, y } => println!("Move in the x direction {} and in the y direction {}", x, y),
        Message::Write(text) => println!("Text message: {}", text),
        Message::ChangeColor(r, g, b) => println!("Change the color to red {}, green {}, and blue {}", r, g, b),
    }
}
```
6. 匹配引用
```rust
fn main() {
    let reference = &4;

    match reference {
        &val => println!("Got a value via destructuring: {:?}", val),
    }

    match *reference {
        val => println!("Got a value via dereferencing: {:?}", val),
    }
}
```
7. 匹配守卫
```rust
fn main() {
    let pair = (2, -2);

    match pair {
        (x, y) if x == y => println!("These are twins"),
        (x, y) if x + y == 0 => println!("Antimatter, kaboom!"),
        (x, _) if x % 2 == 1 => println!("The first one is odd"),
        _ => println!("No correlation..."),
    }
}
```
8. 匹配集合
```rust
fn main() {
    let numbers = (1, 2, 3, 4, 5);

    match numbers {
        (first, .., last) => println!("Some numbers: {}, {}", first, last),
    }
}
```


## for
- ==用于迭代器循环（for-in-loops）：==  跟在in 后边的如果是集合，会自动转型为迭代器 【语法糖？没找到对应的文档】

    for 用于迭代器循环，例如 for i in 0..5。
    这种循环是 Rust 中常见的一种语法糖，用于遍历任何实现了 IntoIterator trait 的集合，直到迭代器返回 None 或循环体中使用 break 为止。
    用于实现 trait（impl Trait for Type）：

- ==for 用于实现 trait，例如 impl Trait for Type。==
    
    这种用法表示为一个类型实现某个 trait。
    用于高阶 trait 边界（higher-ranked trait bounds）：

- ==for 用于高阶 trait 边界，例如 for<'a> &'a T: PartialEq<i32>。==
    
    这种用法表示对生命周期参数的高阶约束。



## 闭包
基本语法
|args| { body }
args 是闭包的参数列表。
body 是闭包的函数体。
示例
1. 捕获环境变量
```rust
fn main() {
    let x = 42;
    let closure = |y: i32| x + y;

    println!("Result: {}", closure(10)); // 输出: Result: 52
}
```
在这个示例中，闭包 closure 捕获了变量 x，并且不需要显式声明捕获方式。编译器会自动推断出 x 是以不可变借用的方式捕获的。

2. 捕获可变变量
```rust
fn main() {
    let mut x = 42;
    let mut closure = |y: i32| {
        x += y;
        x
    };

    println!("Result: {}", closure(10)); // 输出: Result: 52
    println!("Result: {}", closure(10)); // 输出: Result: 62
}
```
在这个示例中，闭包 closure 捕获了可变变量 x，并且不需要显式声明捕获方式。编译器会自动推断出 x 是以可变借用的方式捕获的。

3. 使用 move 关键字
```rust
fn main() {
    let x = String::from("Hello");
    let closure = move |y: &str| format!("{} {}", x, y);

    println!("Result: {}", closure("world")); // 输出: Result: Hello world
}
```
在这个示例中，闭包 closure 捕获了变量 x 的所有权。使用 move 关键字显式地声明了所有权转移。


## map and set

### 使用 HashMap
HashMap 是一个键值对的集合，其中每个键都是唯一的。

#### 创建 HashMap
```rust
use std::collections::HashMap;

fn main() {
    // 创建一个空的 HashMap
    let mut map: HashMap<String, i32> = HashMap::new();

    // 插入键值对
    map.insert(String::from("apple"), 1);
    map.insert(String::from("banana"), 2);
    map.insert(String::from("cherry"), 3);

    // 访问元素
    let apple_count = map.get("apple");
    match apple_count {
        Some(&count) => println!("Apple count: {}", count),
        None => println!("No apple found"),
    }

    // 遍历 HashMap
    for (fruit, count) in &map {
        println!("{}: {}", fruit, count);
    }
}
```

#### 修改 HashMap
```rust
use std::collections::HashMap;

fn main() {
    let mut map: HashMap<String, i32> = HashMap::new();

    map.insert(String::from("apple"), 1);
    map.insert(String::from("banana"), 2);

    // 更新值
    let apple_count = map.entry(String::from("apple")).or_insert(0);
    *apple_count += 1;

    println!("{:?}", map);
}
```
### 使用 HashSet
HashSet 是一个存储唯一值的集合。

#### 创建 HashSet
```rust
use std::collections::HashSet;

fn main() {
    // 创建一个空的 HashSet
    let mut set: HashSet<i32> = HashSet::new();

    // 插入元素
    set.insert(1);
    set.insert(2);
    set.insert(3);

    // 检查元素是否存在
    if set.contains(&1) {
        println!("Set contains 1");
    }

    // 遍历 HashSet
    for &num in &set {
        println!("{}", num);
    }
}
```
#### 修改 HashSet
```rust
use std::collections::HashSet;

fn main() {
    let mut set: HashSet<i32> = HashSet::new();

    set.insert(1);
    set.insert(2);

    // 删除元素
    set.remove(&1);

    println!("{:?}", set.contains(& 2));
}
```