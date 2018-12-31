TITLE: Learning Rust -1
TAG: rust
Learning Rust - 1
================

### 变量
```Rust
fn main() {
    let x = 5;
    let mut y = 7;
    println!("x is {}, y is {}", x, y);
    y = 10;
    println!("y is {}", y);
}
```
变量默认是 **不可变**（*immutable*）的,
