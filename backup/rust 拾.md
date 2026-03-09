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