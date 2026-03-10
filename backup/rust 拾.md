# mut 的不同位置
```rs
fn fill_vec(mut vec: Vec<i32>) -> Vec<i32> {
    //      ^^^ added
    vec.push(88);

    vec
}

```
注意 mut加的 位置所代表的不同含义
# 结构体快速赋值
```rs
        // TODO: Create your own order using the update syntax and template above!
        // 两种方式 一种是 变量名相同的情况下 不需要 额外 声明键值

        // update syntax
        let your_order = Order {
            name: "Hacker in Rust".to_string(),

            ..order_template
        };

```
# enum 给予类型
总共两种 （） 和 「」
```rs
#[derive(Debug)]
enum Message {
    // TODO: Define the different variants used below.
    Resize { width: u32, height: u32 },
    Move(Point),
    Echo(String),
    ChangeColor(u32, u32, u32),
    Quit,
}


```
# Option
Option 的match
```
# move out or not ? derterminate it!
match option_here{
Some or orthers}

```
# dyn and box
可以分开理解：

## Box 是什么
Box<T> 是 堆上分配 的智能指针。

意思是：

值本体放在 heap
栈上只放一个指针
Box 拥有这个值的所有权
例如：


let x = Box::new(123);
这里：

123 放在堆上
x 是一个 Box<i32>
常见用途：

数据太大，不想直接放栈上
需要递归类型
需要装 dyn Trait
## dyn 是什么
dyn Trait 表示：

“某个实现了这个 trait 的具体类型，但先不告诉你它到底是什么。”

例如：


## dyn Error
意思是：

它一定实现了 Error
但具体可能是 ParseIntError
也可能是 io::Error
也可能是自定义错误
这叫 trait object。

## 为什么常一起写：Box<dyn Error>
因为 dyn Error 的具体类型不确定，编译期大小也不确定，不能直接写：


let e: dyn Error;
所以要放到某种指针后面，比如：

&dyn Error
Box<dyn Error>
其中：

&dyn Error：借用
Box<dyn Error>：拥有这个错误对象
所以：


## Box<dyn Error>
就是：

“一个放在堆上的、实现了 Error trait 的某个具体错误值。”

一个直观类比
Box
像“盒子”

盒子里装一个值
盒子拥有它
dyn
像“接口类型”

只要求“符合某个接口”
不要求提前知道具体类型
结合你前面的例子

## fn main() -> Result<(), Box<dyn Error>>
意思是：

成功返回 ()
失败返回“某种错误”
这个错误具体类型不固定
只要实现了 Error 就行
用 Box 把它装起来统一返回
一句话
Box<T>：把 T 放到堆上，并拥有它
dyn Trait：某个实现了这个 trait 的未知具体类型
Box<dyn Trait>：在堆上装一个实现了该 trait 的对象
如果需要，可以继续讲 为什么 dyn 会涉及动态分发。

# rust string的生命周期
注意！ 裸的string 那确实是‘static 但是中间转换如to_str()获得的不是！ 跟随原始变量
```rs
fn main() {
    let raw_str = "hello!";
    let string_str = String::from("hello!");
    // does nothing lsp告诉我们。可以看到 他的生命周期不由我们管辖
    drop(raw_str);
    let get_ref_string = &string_str;
    drop(string_str);
    println!("{}", get_ref_string);

    println!("Hello, world!");
}
```
**tip**: 注意 assert_eq貌似 str和 string都能等
Rust 里字符串“相加”主要是这条规则：
# 字符串相加规则
1. String + &str
+ 的签名本质上是：

所以：

左边必须是 String
右边必须是 &str
左边会被 move
返回新的 String
例子
这里：

a 被移动，之后不能再用
b 是 &str
c 是新的 String
2. 不能直接 String + String
这样不行：

因为右边需要 &str，不是 String。

要改成：

3. &str + &str 也不行
因为左边不是 String：

可以先转成 String：

4. 连续相加
规则还是一样：

每次左边那个 String 被 move
右边借用成 &str
5. 更推荐的方式
如果只是拼接，常用这些：

push_str
format!
format! 不会 move 原来的字符串值本体，通常更直观。

一句话
Rust 字符串相加规则就是：

左 String，右 &str
左边会被 move
结果是新的 String
如果需要，可以继续补一张 String / &String / &str 的对照表。

# trait 以及函数签名
fn test(self)->Self
真正定义的是 类型！返回以及参数 是mut self只是影响内部的可变性 不影响函数签名 也就是trait
```rs
trait test {
fn cc(){
}

}
```
# impl trait
函数签名中输入实现trait的类型 两种区别（与box + dyn）

```rs
impl Licensed 是什么
在参数位置：
`fn compare_license_types(software1: impl Licensed, software2: impl Licensed) -> bool`
基本等价于：
`fn compare_license_types<T1: Licensed, T2: Licensed>(software1: T1, software2: T2) -> bool`
特点：
静态分发
编译期知道具体类型
不需要 Box
不需要堆分配
每种具体类型会生成对应代码
这里两个参数甚至可以是 不同类型：
SomeSoftware
OtherSoftware
都可以。
```
## 而后者是
动态分发
编译期不知道具体类型，只知道“实现了 Licensed”
通常要用 Box 装起来
Box 会涉及堆分配
运行时通过 vtable 调方法
todo 

**支持+语法**

# test 语法
```rs
#[cfg(test)]
mod tests {
    // TODO: Import `is_even`. You can use a wildcard to import everything in
    // the outer module.

    #[test]
#[should_panic] // optional
    fn you_can_assert() {
        // TODO: Test the function `is_even` with some values.
        assert!();
        assert!();
    }
}
```
# 几种写法：
```rs
fn capitalize_first(input: &str) -> String {
    let mut chars = input.chars();
    match chars.next() {
        None => String::new(),
        Some(first) => first.to_ascii_uppercase().to_string() + &chars.collect::<String>(),
    }
}
    match chars.next() {
        None => String::new(),
        Some(first) => first.to_uppercase().chain(chars).collect(),

```
collecy知名类型 不然无法推断
