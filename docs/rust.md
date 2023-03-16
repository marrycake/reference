Rust 备忘清单
====

Rust 快速参考备忘单，旨在为编写基本语法和方法提供帮助。

入门
---

### 配置 vscode 调试
<!--rehype:wrap-class=row-span-2-->

[配置参考](https://github.com/vadimcn/vscode-lldb/blob/master/MANUAL.md#source-path-remapping)。下载 CodeLLDB，选择 rust 自动生成 launch.json 文件

```json
{
  "configurations": [
    // 添加一下行，使 vec/hashmap 等类型显示正常
    "sourceLanguages": ["rust"]
  ]
}
```
<!--rehype:className=wrap-text -->

----

将编译文件与标准库的位置进行映射

```json
{
  "lldb.launch.sourceMap": {
    // 你自己的映射 hash 和映射路径
    "/rustc/4b91a6ea7258a947e59c6522cd5898e7c0a6a88f": "/Users/feiwu/.rustup/toolchains/stable-aarch64-apple-darwin/lib/rustlib/src/rust"
  }
}
```
<!--rehype:className=wrap-text -->

### Hello_World.rs

```rust
fn main() {
  println!("Hello, World!");
}
```

#### 编译运行

```shell
$ rustc Hello_World.rs
$ ./Hello_World

Hello, World!
```

### 原始类型

:-| :-
:-| :-
`bool`                    | 布尔值 (`true` _/_ `false`)
`char`                    | 字符
`f32`, `f64`              | 32 位、64 位浮点数
`i64`, `i32`, `i16`, `i8` | 有符号 16- ... 整数
`u64`, `u32`, `u16`, `u8` | 无符号 16 位，... 整数
`isize`                   | 指针大小的有符号整数
`usize`                   | 指针大小的无符号整数

查看: [Rust 类型](#rust-类型)

### 格式化
<!--rehype:wrap-class=row-span-2-->

```rust
// 单个占位符
println!("{}", 1);
// 多个占位符
println!("{} {}", 1, 3);
// 位置参数
println!("{0} 是 {1} {2}，{0} 也是 {3} 编程语言", "Rust", "cool", "language", "safe");
// 命名参数
println!("{country} 是一个团结的国家", country = "China");
// 占位符特征 :b 表示二进制， :0x 表示十六进制， :o 表示八进制
println!("让我们打印 76 是二进制的 {:b} ，十六进制等价物是 {:0x} 八进制等价物是 {:o}", 76, 76, 76);
// 调试特征
println!("使用调试特征 {:?} 在此处打印我们想要的任何内容", (76, 'A', 90));

// 1.58 中的新格式字符串
let x = "world";
println!("Hello {x}!");
```
<!--rehype:className=wrap-text -->

### 打印风格

```rust
// 打印输出
print!("Hello World\n");
// 打印后追加新行
println!("追加新行");
// 打印为错误
eprint!("这是一个错误\n");
// 打印为新行错误
eprintln!("这是新行的错误");
```

### 变量

```rust
// 初始化和声明变量
let some_variable = "This_is_a_variable";
// 使变量可变
let mut mutable_variable = "Mutable";
// 分配多个变量
let (name, age) = ("ElementalX", 20);
// （全局）常量
const SCREAMING_SNAKE_CASE:i64 = 9;
```

### 注释

```rust
// 行注释
/*.............块注释 */
/// 外部文档注释
//! 内部文档评论
```

另见: [注释](https://doc.rust-lang.org/reference/comments.html) _(doc.rust-lang.org)_

### 函数

```rust
fn test(){
  println!("这是一个函数!");
}
fn main(){
  test();
}
```

查看: [Functions](#rust-函数)

### 声明宏

```rust
macro_rules! foo {
  ($l:tt) => { bar!($l); }
}
macro_rules! bar {
  (3) => {}
}
foo!(3);
```

### 元变量
<!--rehype:wrap-class=row-span-2-->

:- | :-
:- | :-
`item` | 程序项
`block` | 块表达式
`stmt` | 语句<br/> _(注意此选择器不匹配句尾的分号)_
`pat` | 模式
`expr` | 表达式
`ty` | 类型
`ident` | 标识符或关键字
`path` | 类型表达式 形式的路径
`tt` | `token` 树<br/> _(单个 `token` 或宏匹配定界符 `()`、`[]` 或 `{}` 中的标记)_
`meta` | 属性，属性中的内容
`lifetime` | 生存期 `token`
`vis` | 可能为空的可见性限定符
`literal` | 匹配 `-?` 字面量表达式

### 结构体

结构体是一个使用关键字 `struct` 定义的标称型(nominal)结构体类型

```rust
struct Point { x: i32, y: i32 }
let p = Point { x: 10, y: 11 };
let px: i32 = p.x;
```

#### 元组结构体

```rust
struct Color (i32, i32, i32);
let black = Color(0,0,0);
```

#### 单元结构体

不关心该类型的内容, 只关心它的行为。

```rust
struct Solution;
impl Solution{
    // ...
}
```

### 语句与表达式

在 rust 中，语句无需返回值，而表达式总要返回值

#### 语句

```rust
let a = "hello".to_string();
let b = a + " world";
println!("{}", b);
```

#### 表达式

```rust
fn main(){
    let x = {
        let a = "hello".to_string();
        a + " world"
    };
    println!("{}", x);
    // hello world
}
```

### 区间表达式
<!--rehype:wrap-class=col-span-2-->

产生式/句法规则         | 句法         | 类型                        | 区间语义
:-                   | :-           | :-                         | :-
RangeExpr            | `start..end` | std::ops::Range            | start ≤ x < end
RangeFromExpr        | `start..`    | std::ops::RangeFrom        | start ≤ x
RangeToExpr          | `..end`      | std::ops::RangeTo          | x < end
RangeFullExpr        | `..`         | std::ops::RangeFull        | -
RangeInclusiveExpr   | `start..=end`| std::ops::RangeInclusive   | start ≤ x ≤ end
RangeToInclusiveExpr | `..=end`     | std::ops::RangeToInclusive | x ≤ end

Rust 类型
--------

### 类型别名

```rust
type Point = (u8, u8);
let p: Point = (41, 68);
```

### 整数

```rust
let mut a: u32 = 8;
let b = 877_u64;
let c = 8999i64;
let d = -90;
```

### 浮点数

```rust
let mut sixty_bit_float: f64 = 89.90;
let thirty_two_bit_float: f32 = 7.90;
let just_a_float = 69.69;
```

### 布尔值

```rust
let true_val: bool = true;
let false_val: bool = false;
let just_a_bool = true;
let is_true = 8 < 5;  // => false
```

### 字符

```rust
let first_letter_of_alphabet = 'a';
let explicit_char: char = 'F';
let implicit_char = '8';
let emoji = "\u{1f600}";   // => 😀
```

### 字符串字面量

```rust
let community_name = "AXIAL";
let no_of_members: &str = "ten";
println!("社区的名称是 {community_name}，它有 {no_of_members} 个成员");
```
<!--rehype:className=wrap-text -->

查看: [字符串](#rust-字符串)

### 数组
<!--rehype:wrap-class=row-span-2-->

这里介绍的是固定长度的数组。rust 中常用的是集合类型 vec 表示的[动态数组](#rust-动态数组)

```rust
┌─────┬─────┬─────┬─────┬─────┬─────┐
| 92  | 97  | 98  | 99  | 98  | 94  |
└─────┴─────┴─────┴─────┴─────┴─────┘
   0     1     2     3     4     5
```

----

```rust
let array: [i64; 6] = [92,97,98,99,98,94];
```

----

```rust
let mut array: [i32 ; 3] = [2,6,10];
array[1] = 4;
array[2] = 6;
```

使用 `mut` 关键字使其可变

### 切片

```rust
let mut array: [ i64; 4] = [1,2,3,4];
// 下限包括在内，上限不包括在内
let mut slices: &[i64] = &array[0..3]
println!("切片的元素是：{slices:?}");
```

### 元组

```rust
let tuple = (1, 'A' , "Cool", 78, true);
```

Rust 字符串
--------------

### 字符串字面量

```rust
let cs:&str = "备忘清单";
// => 为开发者分享备忘单
println!("为开发者分享 {cs}");
```

### 字符串对象

```rust
// 创建一个空字符串对象
let my_string = String::new;
// 转换为字符串对象
let S_string = a_string.to_string()
// 创建一个初始化的字符串对象
let lang = String::from("Rust");  
println!("First language is {lang}");
```

### .capacity()

 ```rust
let rand = String::from("Random String");
rand.capacity()  // => 13
```

以字节为单位计算字符串的容量

### with_capacity()

```rust
let s = String::with_capacity(10);
```

指定一个足够大的容量值,来减少内存拷贝

### .contains()

```rust
let name = String::from("ElementalX");
name.contains("Element") // => true
```

检查子字符串是否包含在原始字符串中

### 添加单个字符

```rust
let mut half_text = String::from("Hal");
half_text.push('f');    // => Half
```

### 添加整个字符串

```rust
let mut hi = String::from("Hey there...");
hi.push_str("How are you doing??");
// => Hey there...How are you doing??
println!("{hi}");
```

### 原生字符串

```rust
let str1 = r#"\hello"#;
println!("{}", str1);
// \hello
```

原生字符串，无需增加转义字符（`\`）转义

### 字节和字节串

```rust
let str2 = b'a';
println!("{}", str2);
// 97
let str3 = b"\\hello";
println!("{:?}", str3);
// [92, 104, 101, 108, 108, 111]
let str4 = br#"\hello"#;
println!("{:?}", str4);
// [92, 104, 101, 108, 108, 111]
```

Rust 动态数组
-----------

### 创建动态数组

```rust
let v: Vec<i32> = Vec::new();
// 使用宏
let v1 = vec![1, 2, 3];
```

### 读取元素

```rust
let v = vec![1, 2, 3, 4, 5];

let element = &v[100];
// panic，越界
let element2 = v.get(100);
println!("{:?}", element2);
//None
```

### 遍历数组

1. 只读取数组中的元素

   ```rust
   let v = vec![1, 2, 3];
   for i in &v {
       println!("{}", i);
   }
   ```

2. 遍历的同时修改数组中的元素

   ```rust
   let mut v = vec![1, 2, 3];
   for i in &mut v {
       *i += 10
   }
   ```

### 多维数组

```rust
     j0   j1   j2   j3   j4   j5
   ┌────┬────┬────┬────┬────┬────┐
i0 | 1  | 2  | 3  | 4  | 5  | 6  |
   ├────┼────┼────┼────┼────┼────┤
i1 | 6  | 5  | 4  | 3  | 2  | 1  |
   └────┴────┴────┴────┴────┴────┘
```

----

```rust
let arr = vec![
    vec![1, 2, 3, 4, 5, 6],
    vec![6, 5, 4, 3, 2, 1]
];
```

### 常用方法
<!--rehype:wrap-class=col-span-2-->

-|:-
-|:-
`len()`                   | 返回 `vec` 的长度
`is_empty()`              | `vec` 是否为空
`push(value)`             | 在 `vec` 尾部插入元素
`pop()`                   | 删除并返回 `vec` 尾部的元素或者返回 `None`
`insert(index,element)`   | 在指定索引处插入元素
`remove(index)`           | 删除指定索引处的元素并返回被删除的元素，索引越界将 panic 报错退出
`clear()`                 | 清空 `vec`
`append(vec)`             | 将另一个 `vec` 中的所有元素追加移入 `vec` 中，移动的 `vec` 变为空
`truncate(len)`           | 将 `vec` 截断到指定长度,多余的元素被删除
`retain(f)`               | 根据给定的函数，保留满足条件的元素
`drain(range)`            | 删除 `vec` 中指定范围的元素,同时返回一个迭代该范围所有元素的迭代器
`split_off(index)`        | 切分 `vec`，索引左边的元素保留在原 `vec` 中(含索引)，索引右边的元素(不含索引)在返回的 `vec` 中

Rust HashMap\<K,V>
--------

### 使用

```rust
use std::collections::HashMap;

fn main() {
  let mut map: HashMap<String, i32> = HashMap::new();
  map.insert(String::from("blue"), 100);
  // 查询Yellow对应的值，若不存在则插入默认值
  let v: &mut i32 =
    map.entry("Yellow".to_string()).or_insert(5);
  let v: &mut i32 = 
    map.entry("Yellow".to_string()).or_insert(50); // 不会修改值
}
```

### 获取元素

```rust
let mut scores = HashMap::new();

scores.insert(String::from("Blue"), 10);
scores.insert(String::from("Yellow"), 50);

let team_name = String::from("Blue");
let score: Option<&i32> = scores.get(&team_name);
```

### 遍历

```rust
let mut scores = HashMap::new();

scores.insert(String::from("Blue"), 10);
scores.insert(String::from("Yellow"), 50);

for (key, value) in &scores {
    println!("{}: {}", key, value);
}
```

### vec -> HashMap

```rust
let teams_list = vec![
    ("中国队".to_string(), 100),
    ("美国队".to_string(), 10),
    ("日本队".to_string(), 50),
];
let teams_map: HashMap<_,_> =
  teams_list.into_iter().collect();
```

----

```rust
let teams = vec![String::from("blue"),String::from("red")];
let intial_scores = vec![10,50];
let scores:HashMap<_,_> =
  teams.iter().zip(intial_scores.iter()).collect();
```

Option & Result
--------

### Option

```rust
enum Option<T> {
    Some(T),
    None,
}
```

#### 使用

```rust
fn main(){
    let a = Some(5);
    // 直接获取原始值
    println!("{}", a.unwrap());
    // 给出错误信息
    let x: Option<&str> = None;
    x.expect("fruits are healthy"); // panics 带有 `fruits are healthy`
}
```

### Result

```rust
enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

#### 使用

```rust
use std::fs::File;

fn main() {
    let f: Result<File,Error> = File::open("hello.txt");
    let f = match f {
        Ok(file) => file,
        Err(error) => {
            panic!("Problem opening the file: {:?}", error)
        },
    };
}
```

### 宏 `?`

`?` 只能用于返回结果是 Result 或者 Option 的函数,或者实现了 Try 类型

```rust
use std::fs::File;
use std::io::{self, Read};

fn read_username_from_file() -> Result<String, io::Error> {
    let mut s = String::new();
    File::open("hello.txt")?.read_to_string(&mut s)?;
    Ok(s)
}
```

----

```rust
fn first(arr: &[i32]) -> Option<&i32> {
   let v = arr.get(0)?;
   Some(v)
}
```

<!--rehype:className=wrap-text -->

枚举
--------

### 在结构体中使用枚举

```rust
enum IpAddrKind {
  V4,
  V6,
}
struct IpAddr {
  kind: IpAddrKind,
  address: String,
}

fn main(){
    let ip = IpAddr{
        kind: IpAddrKind::V4,
        address: String::from("127.0.0.1")
    };
}
```
<!--rehype:className=wrap-text -->

### 枚举的变体

```rust
enum IpAddrKind {
  V4(u8, u8, u8, u8),
  V6(String),
}

fn main() {
  let home = IpAddrKind::V4(127, 0, 0, 1);
  let loopback = IpAddrKind::V6(String::from("::1"));
}
```
<!--rehype:className=wrap-text -->

----

```rust
enum Message{
  Quit,
  Move {x:i32, y:i32},
  Write(String),
  ChangeColor(i32, i32, i32),
}
fn main(){
  let q = Message::Quit;
  let m = Message::Move {x:10, y:20};
  let w = Message:: Write(String::from("hello"));
  let c = Message::ChangeColor(10, 20, 30);
}
```
<!--rehype:className=wrap-text -->

### 模式匹配结构体

```rust
#[derive(Debug)]
enum Grade {
    A,
    B,
    C,
}
enum Subject {
    Math(Grade),
    English(Grade),
}

fn subject_grade(sub: Subject) {
  match sub {
    Subject::Math(grade) => println!("The Math is {:?}", grade),
    Subject::English(grade) => println!("The Math is {:?}", grade),
  }
}

fn main() {
    subject_grade(Subject::Math(Grade::A));
}
```
<!--rehype:className=wrap-text -->

Rust 运算符
-----------

### 比较运算符

:-|:-
:-|:-
`e == f` | `e` 等于 `f`
`e != f` | `e` 不等于 `f`
`e < f`  | `e` 小于 `f`
`e > f`  | `e` 大于 `f`
`e <= f` | `e` 小于或等于 `f`
`e >= f` | `e` 大于或等于 `f`

----

```rust
let (e, f) = (1, 100);
let greater = f > e;        // => true
let less = f < e;           // => false
let greater_equal = f >= e; // => true
let less_equal = e <= f;    // => true
let equal_to = e == f;      // => false
let not_equal_to = e != f;  // => true
```

### 算术运算符

:-|:-
:-|:-
`a +  b` | `a` 被添加到 `b`
`a -  b` | 从 `a` 中减去`b`
`a /  b` | `a` 除以 `b`
`a % b`  | 通过与 `b` 相除得到 `a` 的余数
`a * b`  | `a` 与 `b` 相乘

----

```rust
let (a, b) = (4, 5);
let sum: i32 = a + b;            // => 9
let subtractions: i32 = a - b;   // => -1
let multiplication: i32 = a * b; // => 20
let division: i32 = a / b;       // => 0
let modulus: i32 = a % b;        // => 4
```

### 位运算符

运算符 | 描述
:- | :-
`g & h`  | 二进制与
`g \| h`  | 二进制或
`g ^ h`  | 二进制异或
`g ~ h`  | 二进制补码
`g << h` | 二进制左移
`g >> h` | 二进制右移

----

```rust
let (g, h) = (0x1, 0x2);
let bitwise_and = g & h;  // => 0
let bitwise_or = g | h;   // => 3
let bitwise_xor = g ^ h;  // => 3
let right_shift = g >> 2; // => 0
let left_shift = h << 4;  // => 32 
```

### 逻辑运算符

示例 | 意义
:- | :-
`c && d` | 两者都是真的_(AND)_
`c \|\| d` | 要么是真的_(OR)_
`!c`     | `c` 为假 _(NOT)_

----

```rust
let (c, d) = (true, false);
let and = c && d;  // => false
let or  = c || d;  // => true
let not = !c;      // => false
```

### 复合赋值运算符

```rust
let mut k = 9;
let mut l = k;
```

----

运算符 | 描述
:- | :-
`k += l`  | 添加一个值并赋值，然后 `k=9`
`k -= l`  | `Substrate` 一个值并赋值，然后 `k=18`
`k /= l`  | 除以一个值并赋值，然后 `k=9`
`k *= l`  | 乘一个值并赋值，然后 `k=81`
`k \|= l`  | 按位或并赋值，则 `k=89`

Rust 流程控制
------------

### If 表达式

```rust
let foo = 12;
let bar = 13;
if foo == bar {
  println!("foo 等于 bar");
} else if foo < bar {
  println!("foo 小于 bar");
} else if foo != bar {
  println!("foo 不等于 bar");
} else {
  println!("Nothing");
}
```

### For 循环
<!--rehype:wrap-class=col-span-2-->

```rust
let mut vec = [1, 2, 3];
for v in &mut vec {
  *v -= 1;
  println!("v 的值为：{v}");
}
```

使用方法                      | 等价使用方式                         | 所有权
:-|:-:|:-
for item in collection      | for item in collection.into_iter() | 转移所有权
for item in &collection     | for item in collection.iter()      | 不可变借用
for item in &mut collection | for item in collection.iter_mut()  | 可变借用

### While 循环

```rust
let mut check =  0;
while check < 11{
  println!("check 是：{check}");
  check += 1;
  println!("递增后：{check}");
  if check == 10{
    break; // 停止 while
  }
}
```

### Loop 循环

```rust
loop {
  println!("你好，世界永远！");
}
```

无限循环表示

### Continue 继续声明

```rust
for (v, c) in (0..10+1).enumerate(){
  println!("{c} 数字循环");
  if v == 9{
    println!("我们继续？");
    continue;
  }
  println!{"v 的值为：{v}"};
}
```

### Break 中断语句

`break` 可以单独使用，也可以带一个返回值

```rust
let mut i = 1;
let res = loop {
  println!("i 是 {i}");
  if i > 100 {
    break i - 100;
  }
  i *= 2;
}

println!("{res}"); // 28
```

Rust 模式匹配
--------

### match
<!--rehype:wrap-class=row-span-2-->

match 模式匹配，使用 `a | b` 表示匹配 a **或** b，使用 `_`，表示匹配剩余所有选项

```rust
fn main(){
  let grade = Grade::A;
  match grade {
    Grade::A => println!("Good"),
    Grade::B => println!("Not bad"),
    Grade::C | Grade::D => println!("Come on"),
    _ => println!("emmm"),
  }
}

enum Grade {
    A,
    B,
    C,
    D,
    E,
    F,
}
```
<!--rehype:className=wrap-text -->

#### `matches!` 宏

它可以将一个表达式跟模式进行匹配，然后返回匹配的结果 `true` 或 `false`

```rust
assert!(matches!('x' ',A'..='Z' | 'a'..='z'));
assert!(matches!(Some(101), Some(x) if x > 100));
```
<!--rehype:className=wrap-text -->

### if let 匹配

match 表达式需要匹配所有的枚举才能结束，但通常我们只需要匹配我们需要的值

```rust
let x = 3;
match Some(x) {
  Some(3) => println!("I guess that x is 3"),
  _ => ()
}
```
<!--rehype:className=wrap-text -->

使用 `if let`

```rust
let x = 3;
if let Some(3) = Some(x) {
    println!("I guess that x is 3");
}
```

### while let

```rust
let mut stack = vec![];

stack.push(1);
stack.push(2);
stack.push(3);

while let Some(top) = stack.pop() {
    println!("{}", top);
}
```

### 其它模式匹配

#### for 循环迭代器

```rust
for (i, v) in collection.iter().enumerate(){}
```
<!--rehype:className=wrap-text -->

#### let

```rust
let (x, _, y) = (1, 2, 3);
println!("{x},{y}");
```

----

```rust
fn get_count_item(s: &str) -> (&str, &str) {
    let mut it = s.split(' ');
    let (Some(str1),Some(str2)) = (it.next(),it.next()) else {
        panic!("Can't segment count item pair");
    };
    (str1, str2)
}
```

### 函数中的模式匹配

```rust
fn add((x, y): (i32, i32)) -> i32 {
    x + y
}

fn main(){
  let sum = add(1, 2);
  println!("{sum}");
}
```

### 忽略参数
<!--rehype:wrap-class=row-span-2-->

#### 使用 `..` 忽略剩余参数

```rust
struct Point {
    x: i32,
    y: i32,
    z: i32,
}

let origin = Point { x: 0, y: 0, z: 0 };

match origin {
    Point { x, .. } => println!("x is {}", x),
}
```
<!--rehype:className=wrap-text -->

#### 使用 `_` 忽略部分参数

```rust
let hello = ('h', 'e', 'l', 'l', 'o');

match hello {
    (h, _, _, l, o) => {
        println!("char: {}, {}, {}", h, l, o)
    },
}
```
<!--rehype:className=wrap-text -->

### 匹配命名变量

以下代码，只要给定的 x 是 Some 类型，但 Some 中的值不是 1，都会匹配到 y

```rust
let x = Some(10);
match x {
    Some(1) => println!("x = 1"),
    Some(y) => println!("y = {:?}", y),
    _ => println!("None"),
}// y = 10
```

### `@` 绑定
<!--rehype:wrap-class=row-span-2-->

`@` 运算符允许为一个字段绑定另外一个变量。

```rust
let grade = 'A';
match grade {
    good @ 'A'..='C' => println!("your grade is {}", good),
    _ => println!("Come on"),
}
```
<!--rehype:className=wrap-text -->

----

```rust
#[derive(Debug)]
struct Point {
    x: i32,
    y: i32,
}
fn main(){
    let p @ Point {x: px, y: py } = Point {x: 10, y: 23};
    println!("x: {}, y: {}", px, py);
    println!("{:?}", p);
}
```
<!--rehype:className=wrap-text -->

----

如果使用 `|`，需要使用 `()`，进行多个模式的绑定

```rust
match 1 {
    num @ (1 | 2) => {
        println!("{}", num);
    }
    _ => {}
}
```

### 使用匹配守卫

```rust
let x = Some(2);
match x {
    Some(1) => println!("x = 1"),
    Some(y) if y == 2 => println!("y = {:?}", y),
    _ => println!("No match"),
}// y = 2
```
<!--rehype:className=wrap-text -->

Rust 函数
--------

### 函数命名

rust 的函数使用蛇形命名法（snake case）

```rust
fn print_message(){
  println!("Hello, Quick Reference!");
}
```

### 参数值

rust 需要为函数的参数标明确定的类型

```rust
fn another_fn(a:u8, b: &str){
    println!("我是 u8:{}", a);
    println!("我是 &str:{}", b);
}

fn main(){
    another_fn(10, "hello")
}
```

### 返回值

如果不指定返回值，rust 默认返回 `()` 类型

```rust
// 在 bin 中的入口函数默认返回 ()
fn main(){}
```

----

使用 `->` 指定返回值，如果**表达式**在最后一行，无需使用 return

```rust
fn add(a:i32, b:i32) -> i32 {
    if a + b < 100 {
        return a - b;
    }
    a + b
}
```

### 永不返回 `!`

```rust
fn dead_end() -> ! {
    panic!("panic!!!!!");
}
```
<!--rehype:className=wrap-text -->

惯用转换
-----

### &str -> String

```rust
String::from("str");
"str".to_string();
"str".to_owned();
```

### &str -> &[u8]

```rust
"str".as_bytes();
```

或者你也可以使用 `b""`

```rust
println!("{:?}", b"str");
```

### &str -> Vec<u8>

```rust
"str".as_bytes().to_vec();
"str".as_bytes().to_owned();
```

### String -> &str

```rust
let s = String::from("str");
let r = s.as_str();
```

### String -> &[u8]

```rust
let s = String::from("str");
let v = s.as_bytes();
```

### String -> Vec<u8>

```rust
let s = String::from("str");
let v = s.into_bytes();
```

### &[u8] -> &str

```rust
let b = "str".as_bytes();
let str = std::str::from_utf8(b).unwrap();
```

### &[u8] -> String

```rust
let b = "str".as_bytes();
let str = String::from_utf8(b.to_vec()).unwrap();
```

### &[u8] -> Vec<u8>

```rust
let b = "str".as_bytes();
let str = b.to_vec();
```

----

```rust
let b = "str".as_bytes();
let str = b.to_owned();
```

### Vec<u8> -> &str

```rust
let b = "str".as_bytes().to_vec();
let s = std::str::from_utf8(&b).unwrap();
```

### Vec<u8> -> &[u8]

```rust
let b = "str".as_bytes().to_vec();
let s = b.as_slice();
```

### Vec<u8> -> String

```rust
let b = "str".as_bytes().to_vec();
let s = String::from_utf8(b).unwrap();
```

智能指针
-----

### Box

Box智能指针允许将值存放在堆上
使用场景:

1. 编译时大小未知的类型
2. 当有大量数据并希望数据不被拷贝
3. 当希望拥有一个值并只关心是否是实现了特定trait

```rust
//使用Box<T> 在堆上存储数据
fn main() {
    let b = Box::new(5);    //此时堆上存储了一个i32的值
    println!("b = {}", b);
}
```

```rust
//使用Box<T> 创建递归类型
//因为rust程序语言在编译时需要知道类型的占用空间, 所以类似
//let list = Cons(1, Cons(2, Cons(3, Nil)));这样的递归是不可行的
enum List{
    Cons(i32, Box<List>),
    Nil,
}

use List::{Cons, Nil};

fn main() {
    let list = Cons(1, Box::new(Cons(2, Box::new(3, Nil))));//因为Box只是一个指向内存的指针, 所以类型拥有固定大小
}
```

### Rc

Rc计数智能指针: 注意只能在单线程中使用
使用场景:

1. 当希望堆上的数据供程序的多个部分读取

示例:

```rust
enum List{
    Cons(i32, Rc<List>),
    Nill,
}

use List::{Cons, Nil};
use std::rc::Rc;

fn main() {
    let a = Rc::new(Cons(5, Rc::new(Cons(10, Rc::new(Nil)))));
    let b = Cons(3, Rc::clone(&a));
    let c = Cons(4, Rc::clone(&a));
}

//当调用Rc::clone方法时只会增加堆中数据的引用计数, 实际数据并不会被拷贝
//当计数为0时数据会被清零
//使用Rc引用计数时要注意是否会导致循环引用导致内存泄露
```

### RefCell

RefCell允许在运行时检查借用规则
意味着可以在运行时的得到一个不可变值的可变借用
使用RefCell允许在运行时改变一个不可变的值

```rust
use std::{borrow::Borrow, cell::RefCell};

fn main() {
    let message = RefCell::new(55);
    println!("{}", message.borrow());
    let mut msg = message.borrow_mut();
    *msg = 66;
    println!("{}", msg);
    
}
```

并发编程
-----

### 线程

```rust
use std::thread;
use std::time:Duration;

fn main() {
    let handle = thread::spawn(||{     //使用该方法创建一个线程
        for i in 1..10{
            println!("hi number {} from the spawned thread!", i);
            thread::sleep(Duration::from_millis(1));
        }
    })

    for i in 1..10{
        println!("hi number {} from the spawned thread!", i);
        thread::sleep(Duration::from_millis(1));
    }

    handle.join().unwrap();    //主线程等待该线程运行结束


}
```

### 消息传递
<!--rehype:wrap-class=col-span-2 row-span-2-->
使用观察者模式, 在底层使用channel, 可以拥有多个生产者

```rust
use std::thread;
use std::sync::mpsc;
use std::time:Duration;

fn main() {
    let (tx, rx) = mpsc::channel();

    let tx1 = mpsc::Sender::clone(&tx);
    thread::spawn(move || {
        let vals = vec![
            String::from("hi"),
            String::from("from"),
            String::from("the"),
            String::from("thread");
        ];

        for val in vals {
            tx1.send(val).unwrap();
            thread::sleep(Duration::from_secs(1));
        }
    })

        thread::spawn(move || {
        let vals = vec![
            String::from("1"),
            String::from("2"),
            String::from("3"),
            String::from("4");
        ];

        for val in vals {
            tx.send(val).unwrap();
            thread::sleep(Duration::from_secs(1));
        }
    })

    for received in rx {
        println!("Got: {}", received);
    }
}

```

### 共享状态

```rust
use std::sync::{Mutex, Arc};
use std::thread;

fn main() {
    let counter = Arc::new(Mutex::new(5)); //使用引用计数的原因是进入线程会移交所有权
    let mut handles = vec![];

    for _ in 0..10 {
        let counter = Arc::clone(&counter);  
        let handle = thread::spawn(move || {
            let mut num = counter.lock.unwrap();
            *mut += 1;
        });
        handles.push(handle);
    }

    for handle in handles {
        handle.join().unwrap();
    }
}
```

测试
-----

### 编写一个测试

这是一个测试模块的标准写法

```rust

#[cfg(test)]    //注解, 表示此模块为测试模块
mod tests {
    #[test]     //注解, 表示函数为测试函数
    fn it_works() {{
        assert_eq!(2 + 2, 4);
    }}
}
```

运行

```rust
使用命令cargo test
cargo test {1} -- {2} // {1}处指定cargo命令, {2}处指定对测试文件的命令
                      // 可以使用cargo test --help 和 cargo test -- --help 分别查看
```

使用宏来检查结果

```rust
assert!(param)   // ---> param 需要为一个bool类型
assert_eq!(param1, param2)  // --->检查param1, param2是否相等, 不通过时分别打印param1, param2的值
assert_ne!(param1, param2)  // --->检查param1, param2是否不相等

使用过程中一些注意点:
1. 程序中的输出, 例如print!("something")会被捕获, 只有在测试失败时该捕获才会被输出, 可以使用 cargo test --nocapture 来禁用

自定义错误信息:
assert!(result(), "Greeting did not contain name, value was {}", value);

```

测试中使用注解

```rust
#[should_panic]     // ---> 如果程序中不产生panic测试失败
```

### 控制如何测试

并行或连续的测试

```rust
在rust中, 默认所有测试并行进行
可以使用cargo test -- --test-threads=1 指定串行测试
```

显示函数输出

```rust
1. 成功测试的输出将配捕获, 不会出现在测试总结中
2. 失败测试总结中, 不仅会出现失败测试的输出, 也会显示失败原因
3. 可以使用cargo test --nocapture 指定禁止捕获
```

测试过滤

```rust
运行单个测试
cargo test test_name    //注意不能使用此方法指定多个测试

过滤运行多个测试
cargo test param   //通过给定参数匹配运行匹配字符串的测试, 例如param = add, test有add_1, add_2, sum_1, sub_1, 则只有add_1,, add2会运行

忽略某些测试
#[ignore]   // 通过对注解添加此测试表示当cargo test时此测试不会被运行, 除非 cargo test --ignored表示只运行ignore测试
```

### 测试模块

测试模块

```rust
//使用#[cfg(test)]表示此模块为测试模块
#[cfg(test)]
mod test{   //为什么要使用测试模块, 测试模块可以更好的帮助我们跟踪错误

}
```

测试私有函数

```rust
fn function ();
mod test{
    use super::*;     //对于私有函数, 需要在此处将测试包引入
    #[test]
    fn test_function() {
        function();
    }
}
```

目录测试

```rust
tree: 
--project
    --test
    --adder

//test目录是一个测试目录
//当我们需要在test目录中测试adder中相关功能时
extern crate adder;
#[test]
fn it_adds_two() {
    assert_eq!(4, adder::add_two(2));
}
```

杂项
-----

### 类型断言 type-casting

```rust
let a_int = 90; // int
// int 到 float
let mut type_cast = (a_int as f64);
```

----

```rust
let orginal: char = 'I';
// char 到 int => 73
let type_casted: i64 = orginal as i64;
```

要在 `Rust` 中执行类型转换，必须使用 `as` 关键字

### 生命周期
<!--rehype:wrap-class=row-span-2-->

自动赋予生命周期规则

```rust
对于rust来说, 所有参数都需要指定生命周期, 但是rust给与了三条规则可以自动帮助我们赋予生命周期
1. 为函数的每一个变量各自赋予生命周期
    df function<'a, 'b>(param1: &'a str, param2: &'b str)
2. 当函数参数只有一个时, 为返回值自动赋予与传入值一样的生命周期
    df function<'a>(param1: &'a str) -> result: &'a str
3. 当函数参数包含&self, &mut self 时, 为返回值赋予与self一致的生命周期
    df function<'a, 'b>(&self: &'a, param2: &str: &'b str) -> result: &'a str
```

手动生命周期

```rust
当违反上述规则时需手动赋予
//例如
//作用域'a表示取参数参数作用域中最小的一个
fn the_bigger_one_withlabel<'a, T>(str1: &'a str, str2: &'a str, label: T) -> &'a str
where
    T: Display,
{
    println!("{}", label);
    if str1 > str2 {
        str1
    } else {
        str2
    }
}

```

### 借用

```rust
let mut foo = 4;
let mut borrowed_foo = &foo;
println!("{borrowed_foo}");
```

----

```rust
let mut bar = 3;
let mut mutable_borrowed_bar = &mut bar;
println!("{mutable_borrowed_bar}");
```

这里借用的值使用 `&` 运算符从值一中借用值

### 解引用

```rust
let mut borrow = 10;
let deref = &mut borrow;
println!("{}", *deref);
```

`*` 操作符用于解引用

### 作用域

```rust
{
  // 范围仅限于此大括号
  let a_number = 1;
}
println!("{a_number}");
```

这将产生错误，因为变量 `a_number` 的生命周期在大括号处结束

另见
--------

- [Cargo 备忘清单](./cargo.md) _(jaywcjlove.github.io)_
- [The Rust Document](https://doc.rust-lang.org/book/ch00-00-introduction.html) _(doc.rust-lang.org)_
- [The Rust Reference](https://doc.rust-lang.org/reference/introduction.html) _(doc.rust-lang.org)_
- [Rust Cheatsheet](https://phaiax.github.io/rust-cheatsheet/) _(phaiax.github.io)_
- [Rust 标准库文档中文版](https://jaywcjlove.github.io/rust-cn-document-for-docker/std/std/) _(jaywcjlove.github.io)_
- [Rust 程序设计语言 中文版](https://jaywcjlove.github.io/rust-cn-document-for-docker/book/) _(jaywcjlove.github.io)_
- [Rust By Example 中文版](https://jaywcjlove.github.io/rust-cn-document-for-docker/rust-by-example-cn/) _(jaywcjlove.github.io)_
- [Rust 参考手册中文版](https://jaywcjlove.github.io/rust-cn-document-for-docker/reference/) _(jaywcjlove.github.io)_
- [RustDoc 手册中文版](https://jaywcjlove.github.io/rust-cn-document-for-docker/rustdoc/) _(jaywcjlove.github.io)_
- [Rust Cookbook 中文版](https://jaywcjlove.github.io/rust-cn-document-for-docker/rust-cookbook/) _(jaywcjlove.github.io)_
