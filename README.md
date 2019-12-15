# [Golang 视频教程](https://pegasuswang.github.io/LetsGo/)

![](./golang.png)

## 目的

通过连载短视频和文章的形式帮助有一定其他语言编程基础的人快速学习和入门 Golang。
内容包括 Golang 基础、内置库、web 开发、并发编程等，均来自笔者日常学习和开发经验总结。
教程中会有一些和 Python 等语言特性的对比，方便读者理解。希望教程有如下特色：

- 每一小节均包含视频和文章，演示笔者日常开发的工作流（当然未必是最佳方式，读者朋友可以分享下自己的开发经验）
- 全部代码视频中现场编写，避免教科书式枯燥讲解代码
- 主次分明，快速上手，主要分享日常业务开发中最常用的特性
- 结合实战，笔者自己边踩坑边总结业务开发中遇到的一些痛点和解决方案。比如如何做单测、代码如何分层、如何排查性能问题等
- 最佳实践。总结业务开发中一些好的实践分享出来，贴地气

主要面向有一定开发经验的开发者，不会涉及到一些非常具体和细节的问题。 比如如何下载 IDE，如何导出环境变量等，
编程新手可以先补一补开发基础。

阅读地址：[ https://pegasuswang.github.io/LetsGo/ ](https://pegasuswang.github.io/LetsGo/)

## 如何快速上手新语言

笔者的经验就是『学以致用』，如果光学不练，很快就会忘记。学一门新语言的最好方式就是在熟悉了基本语法以后，
通过**大量**的编码和项目练习来熟悉它。期间你还需要频繁借助文档/搜索引擎等工具，边写边查，很快就可以上手。
之后再去考虑一些具体的语言细节和深入的特性，


新语言学习十步法：

- 1.安装并且搭建开发环境。
- 2.基础类型和符合类型。基础数据类型(数值类型，字符串）和符合类型(map/set/list)
- 3.控制流语句。判断、循环、选择
- 4.函数。定义方式，传值和返回值。
- 5.面向对象。数据和方法，组合and继承
- 6.内置库（文件、网络、时间、日志等）
- 7.找一个广泛使用的第三方库开始写（抄）项目。
- 8.边写边查，常用代码片段总结成文档。比如 golang 里边各种转换
- 9.第三方库快速编写 demo 代码。从 github 搜索关键词。
- 10.最佳实践：遇到的坑；代码分层；单元测试；静态检查

总结起来就是：

- 多写多练。光看书是学不会编程。练习到手熟悉。书上的例子自己尝试编写和实现
- 照葫芦画瓢，一开始学会模仿别人的写法。看源码，学习优秀的设计和写法
- 总结，分享，与输出。巩固所学知识


[学习一门新编程语言，真没那么难![视频]](https://www.bilibili.com/video/av79283035)

## 工具

笔者使用 when-changed 来监控文件变动并且执行 go 代码，这样你可以边写代码，保存后自动运行观察结果，
在写代码验证你的想法的时候会比较方便，视频里笔者会详细演示。

```sh
pip install when-changed
# 监控当前文件夹变动并且执行命令
when-changed -v -r -1 -s ./ go run main.go
```

## 本电子书制作和写作方式
使用 mkdocs 和 markdown 构建，使用 Python-Markdown-Math 完成数学公式。
markdown 语法参考：http://xianbai.me/learn-md/article/about/readme.html

安装依赖：

```sh
pip install mkdocs    # 制作电子书, http://markdown-docs-zh.readthedocs.io/zh_CN/latest/
# https://stackoverflow.com/questions/27882261/mkdocs-and-mathjax/31874157
pip install https://github.com/mitya57/python-markdown-math/archive/master.zip

# 或者直接
pip install -r requirements.txt

# 如果你 fork 了本项目，可以定期拉取主仓库的代码来获取更新，目前还在不断更新相关章节
```

你可以 clone 本项目后在本地编写和查看电子书：

```sh
mkdocs serve     # 修改自动更新，浏览器打开 http://localhost:8000 访问
# 数学公式参考 https://www.zybuluo.com/codeep/note/163962
mkdocs gh-deploy    # 部署到自己的 github pages
```
