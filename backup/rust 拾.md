# mut 的不同位置
```rs
fn fill_vec(mut vec: Vec<i32>) -> Vec<i32> {
    //      ^^^ added
    vec.push(88);

    vec
}

```
注意 mut加的 位置所代表的不同含义