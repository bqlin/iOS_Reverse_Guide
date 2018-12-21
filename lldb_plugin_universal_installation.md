# LLDB 插件通用安装方式

1. 拷贝 LLDB 插件到本地。若是 Git 项目，则 `git clone` 到本地指定目录，若可以 brew 安装，那就更省事了。
2. 获取 LLDB 启动的 `*.py` 文件路径，一般在项目文档里有叙述。
3. 编辑或创建 `~/.lldbinit`，加入以下行：

```
command script import py文件路径
```
