# basic

## Macro

=== "println!"
    ```{.rust}
    macro_rules! println {
        () => { ... };
        ($($arg:tt)*) => { ... };
    }
    //============================
    #![allow(unused)]
    fn main() {
        println!(); // prints just a newline
        println!("hello there!");
        println!("format {} arguments", "some");
        let local_variable = "some";
        println!("format {local_variable} arguments");
    }
    ```
=== "panic!"
    ```{.rust}
    macro_rules! panic {
        ($($arg:tt)*) => { ... };
    }
    //===========================================
    panic!();
    panic!("this is a terrible mistake!");
    panic!("this is a {} {message}", "fancy", message = "message");
    std::panic::panic_any(4); // panic with the value of 4 to be collected elsewher
    ```
=== "format!"
    ```{.rust}
    macro_rules! format {
        ($($arg:tt)*) => { ... };
    }
    //====================================
    format!("test");                             // => "test"
    format!("hello {}", "world!");               // => "hello world!"
    format!("x = {}, y = {val}", 10, val = 30);  // => "x = 10, y = 30"
    let (x, y) = (1, 2);
    format!("{x} + {y} = 3");                    // => "1 + 2 = 3"
    ```
=== "assert!"
    ```{.rust}
    macro_rules! assert {
        ($cond:expr $(,)?) => { ... };
        ($cond:expr, $($arg:tt)+) => { ... };
    }
    //====================================
    assert!(true);

    fn some_computation() -> bool { true } // a very simple function

    assert!(some_computation());

    // assert with a custom message
    let x = true;
    assert!(x, "x wasn't true!");

    let a = 3; let b = 27;
    assert!(a + b == 30, "a = {}, b = {}", a, b);
    ```

## Memory Layout

=== "String"
    ```{}
            +---------+--------+----------+
      Stack | pointer | length | capacity | 
            |  |      |   5    |    5     |
            +--|------+--------+----------+
               |
               |
               v
            +---+---+---+---+---+
     Heap:  | H | e | l | l | o |
            +---+---+---+---+---+

    ```

=== "&String"
    ```
            --------------------------------------
            |                                    |         
       +----v----+--------+----------+      +----|----+
       | pointer | length | capacity |      | pointer |
       |    |    |   5    |    5     |      |         |
       +----|----+--------+----------+      +---------+
            |        s                          &s 
            |       
            v       
          +---+---+---+---+---+
          | H | e | l | l | o |
          +---+---+---+---+---+
    ```
=== "&str"
    ```
                          s                              slice
            +---------+--------+----------+      +---------+--------+
      Stack | pointer | length | capacity |      | pointer | length |
            |    |    |   5    |    5     |      |    |    |   4    |
            +----|----+--------+----------+      +----|----+--------+
                 |        s                           |  
                 |                                    |
                 v                                    | 
               +---+---+---+---+---+                  |
       Heap:   | H | e | l | l | o |                  |
               +---+---+---+---+---+                  |
                     ^                                |
                     |                                |
                     +--------------------------------+

    ```
    str is a [DST](other.md#dynamically-sized-types) type

## Control Flow

=== "if"
    ```{.rust .linenos}
    let number = 3;
    if number < 5 {
        println!("`number` is smaller than 5");
    }
    ```

=== "if/else"
    ```{.rust}
    let number = 3;

    if number < 5 {
        println!("`number` is smaller than 5");
    } else {
        println!("`number` is greater than or equal to 5");
    }
    ```
=== "match"
    ```{.rust}
    enum Status {
        ToDo,
        InProgress,
        Done
    }

    impl Status {
        fn is_done(&self) -> bool {
            match self {
                Status::Done => true,
                // The `|` operator lets you match multiple patterns.
                // It reads as "either `Status::ToDo` or `Status::InProgress`".
                Status::InProgress | Status::ToDo => false
            }
        }
    }
    ```

## Loop
=== "while"
    ```{.rust}
    let sum = 0;
    let i = 1;
    // "while i is less than or equal to 5"
    while i <= 5 {
        // `+=` is a shorthand for `sum = sum + i`
        sum += i;
        i += 1;
    }    
    ```

=== "for"
    ```{.rust}
    let mut sum = 0;
    for i in 1..=5 {
        sum += i;
    }
    ```

=== "loop"
    ```{.rust}
    loop {
            count += 1;

            if count == 3 {
                println!("three");

                // Skip the rest of this iteration
                continue;
            }

            println!("{}", count);

            if count == 5 {
                println!("OK, that's enough");

                // Exit this loop
                break;      //(1)
            }
        }
    ```

    1. :man_raising_hand:loop familary with while , but only stop when meet break


## Derive macros



## Generic programming
```{.rust}
fn print_if_even<T>(n: T)  //(1)
where
    T: IsEven + Debug  //(2)
{
    if n.is_even() {
        println!("{n:?} is even");
    }
}
```

1. 参数n的类型是T
2. 这里的冒号意味着{==约束==}，约束为类型T必须实现某个trait


Two different trait bounds
=== "where clause"
    ```{.rust} 
    fn print_if_even<T>(n: T)
    where
        T: IsEven + Debug
    //  ^^^^^^^^^^^^^^^^^
    //  This is a `where` clause
    {
        // [...]
    }
    ```
=== "inline"
    ```{.rust}
    fn print_if_even<T: IsEven + Debug>(n: T) {
        //           ^^^^^^^^^^^^^^^^^
        //           This is an inline trait bound
        // [...]
    }
    ```

## Generics and associated types
Example 
```{.rust}
pub trait From<T> {   //(1)
    fn from(value: T) -> Self;
}

pub trait Deref {
    type Target;      //(2)
    
    fn deref(&self) -> &Self::Target;
}
```

1. from 实现了泛型  
2. deref 实现了关联类型

关联类型，对于一种struct ,只可能有一种实现  而对于泛型，在impl时可以传入多种不同的参数

An associated type is uniquely determined by the trait implementation. you'll only be able to specify one Target for a given type and there won't be any ambiguity.

## 尖括号<>
[尖括号](./other.md#尖括号)


## 复合类型（Compound Types）

=== "Struct"


=== "Enum"
    ```
    An enumeration is a type that can have a fixed set of values, called variants.
    In Rust, you define an enumeration using the enum keyword:

    enum Status {
        ToDo,
        InProgress,
        Done,
    }
    enum, just like struct, defines a new Rust type.
    ```


=== "Tuples"


=== "Array"


## Trait 
🌟 The trait is defined in the current crate

🌟 The implementor type is defined in the current crate 

### Operator

=== "Add"
    ```{.rust}
    pub trait Add<Rhs = Self> {
        type Output;

        // Required method
        fn add(self, rhs: Rhs) -> Self::Output;
    }

    //==========================

    use std::ops::Add;

    #[derive(Debug, Copy, Clone, PartialEq)]
    struct Point {
        x: i32,
        y: i32,
    }

    impl Add for Point {
        type Output = Self;

        fn add(self, other: Self) -> Self {
            Self {
                x: self.x + other.x,
                y: self.y + other.y,
            }
        }
    }

    assert_eq!(Point { x: 1, y: 0 } + Point { x: 2, y: 3 },
            Point { x: 3, y: 3 });
    ```
    
=== "Sub"
    ```{.rust}
    pub trait Sub<Rhs = Self> {
        type Output;

        // Required method
        fn sub(self, rhs: Rhs) -> Self::Output;
    }

    //============================================
    use std::ops::Sub;

    #[derive(Debug, Copy, Clone, PartialEq)]
    struct Point {
        x: i32,
        y: i32,
    }

    impl Sub for Point {
        type Output = Self;

        fn sub(self, other: Self) -> Self::Output {
            Self {
                x: self.x - other.x,
                y: self.y - other.y,
            }
        }
    }

    assert_eq!(Point { x: 3, y: 3 } - Point { x: 2, y: 3 },
            Point { x: 1, y: 0 });
    ```
=== "Mul"
    ```{.rust}
    pub trait Mul<Rhs = Self> {
        type Output;

        // Required method
        fn mul(self, rhs: Rhs) -> Self::Output;
    }

    //==========================
    use std::ops::Mul;

    // By the fundamental theorem of arithmetic, rational numbers in lowest
    // terms are unique. So, by keeping `Rational`s in reduced form, we can
    // derive `Eq` and `PartialEq`.
    #[derive(Debug, Eq, PartialEq)]
    struct Rational {
        numerator: usize,
        denominator: usize,
    }

    impl Rational {
        fn new(numerator: usize, denominator: usize) -> Self {
            if denominator == 0 {
                panic!("Zero is an invalid denominator!");
            }

            // Reduce to lowest terms by dividing by the greatest common
            // divisor.
            let gcd = gcd(numerator, denominator);
            Self {
                numerator: numerator / gcd,
                denominator: denominator / gcd,
            }
        }
    }

    impl Mul for Rational {
        // The multiplication of rational numbers is a closed operation.
        type Output = Self;

        fn mul(self, rhs: Self) -> Self {
            let numerator = self.numerator * rhs.numerator;
            let denominator = self.denominator * rhs.denominator;
            Self::new(numerator, denominator)
        }
    }

    // Euclid's two-thousand-year-old algorithm for finding the greatest common
    // divisor.
    fn gcd(x: usize, y: usize) -> usize {
        let mut x = x;
        let mut y = y;
        while y != 0 {
            let t = y;
            y = x % y;
            x = t;
        }
        x
    }

    assert_eq!(Rational::new(1, 2), Rational::new(2, 4));
    assert_eq!(Rational::new(2, 3) * Rational::new(3, 4),
            Rational::new(1, 2));
    ```
=== "Div"
    ```{.rust}
    pub trait Div<Rhs = Self> {
        type Output;

        // Required method
        fn div(self, rhs: Rhs) -> Self::Output;
    }

    //========
    use std::ops::Div;

    // By the fundamental theorem of arithmetic, rational numbers in lowest
    // terms are unique. So, by keeping `Rational`s in reduced form, we can
    // derive `Eq` and `PartialEq`.
    #[derive(Debug, Eq, PartialEq)]
    struct Rational {
        numerator: usize,
        denominator: usize,
    }

    impl Rational {
        fn new(numerator: usize, denominator: usize) -> Self {
            if denominator == 0 {
                panic!("Zero is an invalid denominator!");
            }

            // Reduce to lowest terms by dividing by the greatest common
            // divisor.
            let gcd = gcd(numerator, denominator);
            Self {
                numerator: numerator / gcd,
                denominator: denominator / gcd,
            }
        }
    }

    impl Div for Rational {
        // The division of rational numbers is a closed operation.
        type Output = Self;

        fn div(self, rhs: Self) -> Self::Output {
            if rhs.numerator == 0 {
                panic!("Cannot divide by zero-valued `Rational`!");
            }

            let numerator = self.numerator * rhs.denominator;
            let denominator = self.denominator * rhs.numerator;
            Self::new(numerator, denominator)
        }
    }

    // Euclid's two-thousand-year-old algorithm for finding the greatest common
    // divisor.
    fn gcd(x: usize, y: usize) -> usize {
        let mut x = x;
        let mut y = y;
        while y != 0 {
            let t = y;
            y = x % y;
            x = t;
        }
        x
    }

    assert_eq!(Rational::new(1, 2), Rational::new(2, 4));
    assert_eq!(Rational::new(1, 2) / Rational::new(3, 4),
            Rational::new(2, 3));
    ```
=== "Rem"
    ```{.rust}
    pub trait Rem<Rhs = Self> {
        type Output;

        // Required method
        fn rem(self, rhs: Rhs) -> Self::Output;
    }

    //================================

    use std::ops::Rem;

    #[derive(PartialEq, Debug)]
    struct SplitSlice<'a, T> {
        slice: &'a [T],
    }

    impl<'a, T> Rem<usize> for SplitSlice<'a, T> {
        type Output = Self;

        fn rem(self, modulus: usize) -> Self::Output {
            let len = self.slice.len();
            let rem = len % modulus;
            let start = len - rem;
            Self {slice: &self.slice[start..]}
        }
    }

    // If we were to divide &[0, 1, 2, 3, 4, 5, 6, 7] into slices of size 3,
    // the remainder would be &[6, 7].
    assert_eq!(SplitSlice { slice: &[0, 1, 2, 3, 4, 5, 6, 7] } % 3,
            SplitSlice { slice: &[6, 7] });
    ```
=== "PartialEq"
    ```{.rust}

    pub trait PartialEq {
        // Required method
        //
        // `Self` is a Rust keyword that stands for 
        // "the type that is implementing the trait"
        fn eq(&self, other: &Self) -> bool;

        // Provided method
        fn ne(&self, other: &Self) -> bool { ... }  //(1)
    }

    //====================
    use std::ops::Add;

    #[derive(Debug, Copy, Clone, PartialEq)]
    struct Point {
        x: i32,
        y: i32,
    }

    impl Add for Point {
        type Output = Self;

        fn add(self, other: Self) -> Self {
            Self {
                x: self.x + other.x,
                y: self.y + other.y,
            }
        }
    }

    assert_eq!(Point { x: 1, y: 0 } + Point { x: 2, y: 3 },
            Point { x: 3, y: 3 });
    ```

    1. fn ne is implmented by default

=== "PartialOrd"
    ```{.rust}
    pub trait PartialOrd<Rhs = Self>: PartialEq<Rhs>
    where
        Rhs: ?Sized,
    {
        // Required method
        fn partial_cmp(&self, other: &Rhs) -> Option<Ordering>; //(1)

        // Provided methods
        fn lt(&self, other: &Rhs) -> bool { ... }
        fn le(&self, other: &Rhs) -> bool { ... }
        fn gt(&self, other: &Rhs) -> bool { ... }
        fn ge(&self, other: &Rhs) -> bool { ... }
    }

    //

    ```

    1. the only one need to be done

### Other
=== "Deref"
    ```{.rust}
    //std::ops
    pub trait Deref {
        type Target: ?Sized;

        // Required method
        fn deref(&self) -> &Self::Target;
    }

    //==========================

    use std::ops::Deref;

    struct DerefExample<T> {
        value: T
    }

    impl<T> Deref for DerefExample<T> {
        type Target = T;

        fn deref(&self) -> &Self::Target {
            &self.value
        }
    }

    let x = DerefExample { value: 'a' };
    assert_eq!('a', *x);
    ```

=== "Sized"
    ```{.rust}
    
    pub trait Sized {  //(1)
        // This is an empty trait, no methods to implement.
    }

    ```

    1. no methods to implement just means that it is a flag

    :man_raising_hand: 如果一个结构体是泛型的，那么它会假定结构体所包含的类型T实现了Size 的trait

=== "From"
    ```{.rust}
    pub trait From<T>: Sized {
        // Required method
        fn from(value: T) -> Self;
    }

    //============================

    use std::fs;
    use std::io;
    use std::num;

    enum CliError {
        IoError(io::Error),
        ParseError(num::ParseIntError),
    }

    impl From<io::Error> for CliError {
        fn from(error: io::Error) -> Self {
            CliError::IoError(error)
        }
    }

    impl From<num::ParseIntError> for CliError {
        fn from(error: num::ParseIntError) -> Self {
            CliError::ParseError(error)
        }
    }

    fn open_and_parse_file(file_name: &str) -> Result<i32, CliError> {
        let mut contents = fs::read_to_string(&file_name)?;
        let num: i32 = contents.trim().parse()?;
        Ok(num)
    }
    ```
=== "Into"
    ```{.rust}
    pub trait Into<T>: Sized {
        // Required method
        fn into(self) -> T;
    }

    //============================


    ```
    From实现后，将自动生成一个Into的方法， 在这里sized 是约束From 的trait ,也就是约束self,意味着必须实现了sized trait的类型才能实现sized , super trait??

=== "Clone"
    ```{.rust}
    pub trait Clone {
        fn clone(&self) -> Self;
    }

    //========================


    ```


=== "Copy"
    ```{.rust}
    pub trait Copy: Clone { } //(1)    
    ```

    1. copy is an empty trait , which just a trait（Marker Trait）

=== "Drop"
    ```{.rust}
    pub trait Drop {
        // Required method
        fn drop(&mut self);
    }

    //=======================

    struct HasDrop;

    impl Drop for HasDrop {
        fn drop(&mut self) {
            println!("Dropping HasDrop!");
        }
    }

    struct HasTwoDrops {
        one: HasDrop,
        two: HasDrop,
    }

    impl Drop for HasTwoDrops {
        fn drop(&mut self) {
            println!("Dropping HasTwoDrops!");
        }
    }

    fn main() {
        let _x = HasTwoDrops { one: HasDrop, two: HasDrop };
        println!("Running!");
    }


    //output =========

    Running!
    Dropping HasTwoDrops!
    Dropping HasDrop!
    Dropping HasDrop!
    ```
    那么如果没有实现 Drop trait 会不会导致内存泄漏呢？
    
    - 对于 Rust 的堆内存来说，即使没有显式地实现 Drop trait，Rust 的所有权系统仍然会在对象离开作用域时自动释放它所拥有的内存资源，因此不会发生内存泄漏。
    - 但是，如果你的类型使用了除了堆内存以外的外部资源，比如打开文件、网络连接或者其他需要手动释放的资源，而没有通过实现 Drop trait 来释放这些资源，那么这些资源可能不会被正确清理，从而导致资源泄漏。