# minipack 基本流程

# 单个module的解析。
使用的是AST 去解析，而不是简单的匹配语句。
最终返回一个dict
分别代表文件的id 和 对应的dependences   
以及使用babel转译的代码

# 逐个解析。
使用bfs 逐个遍历

# 最后的 打包
注意我们把每个函数都包了起来
```js
function (require,module.exports) {

}
```
这里就是给babel生成的代码里的 require 和 exports调用的。

最后生成的最终函数里就有require函数的实现。大致就是。找到对应的函数 调用。 注意实际传的是localRequire 是 名字->数字的一个小转换。 最后返回exports的内容
