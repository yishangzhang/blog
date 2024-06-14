# 尖括号

## Rust 中尖括号的使用

在 Rust 中，尖括号`<>`用途广泛，主要用于表示泛型类型参数、trait 约束和生命周期注解。以下是具体用途的总结：

### 泛型类型参数

- **结构体或枚举**：
  定义一个泛型结构体或枚举时，在其名称后紧接尖括号，内部包含泛型类型参数。
  ```rust
  struct Point<T> {
      x: T,
      y: T,
  }
  ```

- **函数、方法或 trait**：
  定义泛型函数、方法或 trait 时，在其名称后紧接尖括号，内部声明泛型参数。
  ```rust
  fn my_function<T>(param: T) {}
  ```

### Trait 约束

- **`impl` 关键字后面**：
  在对泛型类型实现某个 trait 时，使用`impl<T: SomeTrait>`来表示类型参数`T`带有某些约束。
  ```rust
  impl<T: Display> ToString for T {}
  ```

- **函数或方法参数中**：
  在泛型函数或方法中，可以通过`where`子句在尖括号内施加更复杂的 trait 约束。
  ```rust
  fn some_function<T>(param: T)
  where
      T: SomeTrait + AnotherTrait,
  {}
  ```

### 生命周期（Lifetimes）

- **结构体、函数、方法定义中**：
  生命周期注解也使用尖括号，常见形式为撇号开始的小写字母，如`<'a>`，用于指定引用的有效期。
  ```rust
  fn borrow_something<'a>(item: &'a T) {}
  ```

- **`impl` 块中**：
  在对泛型类型进行方法实现时，如果方法涉及到引用，可能需要声明生命周期参数。
  ```rust
  impl<'a> SomeStruct<'a> {}
  ```

### 综合说明

Rust 中的尖括号`<>`广泛应用于声明：

- 泛型参数（类型、trait 约束）
- 生命周期注解

通过上下文来理解尖括号的具体含义是关键，尖括号可以出现在类型名、trait 名、`impl` 关键字后面，或其内部，以提供关于泛型参数、trait 约束或生命周期的信息。

# {==Dynamically sized types==}

str is a dynamically sized type (DST).
A DST is a type whose size is not known at compile time. Whenever you have a reference to a DST, like &str, it has to include additional information about the data it points to. It is a fat pointer.
In the case of &str, it stores the length of the slice it points to. We'll see more examples of DSTs in the rest of the course.