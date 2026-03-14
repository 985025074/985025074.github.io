# minipack 基本流程

# 单个module的解析。
使用的是AST 去解析，而不是简单的匹配语句。
最终返回一个dict
分别代表文件的id 和 对应的dependences   
以及使用babel转译的代码

# 逐个解析。
使用bfs 逐个遍历

# 最后的 打包

