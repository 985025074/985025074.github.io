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

Box 是什么
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
dyn 是什么
dyn Trait 表示：

“某个实现了这个 trait 的具体类型，但先不告诉你它到底是什么。”

例如：


dyn Error
意思是：

它一定实现了 Error
但具体可能是 ParseIntError
也可能是 io::Error
也可能是自定义错误
这叫 trait object。

为什么常一起写：Box<dyn Error>
因为 dyn Error 的具体类型不确定，编译期大小也不确定，不能直接写：


let e: dyn Error;
所以要放到某种指针后面，比如：

&dyn Error
Box<dyn Error>
其中：

&dyn Error：借用
Box<dyn Error>：拥有这个错误对象
所以：


Box<dyn Error>
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

fn main() -> Result<(), Box<dyn Error>>
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

GPT-5.4 • 1x