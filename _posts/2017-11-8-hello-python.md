---
layout: post
title: python 的安装
---


从官网下载最新版的Python安装即可，x86-64（64位系统），选择web-based installer，Windows x86-64 executable installer，Windows x86-64 embeddable zip file都可以，它们分别是在线安装版，程序版和压缩版，安装后的效果是一样的，安装时把添加到PATH打钩。
新建一个以.py结尾的文件，输入print（'hello world'）,用sublime打开按Ctrl+b可查看结果。
sublime python插件，1.SublimeREPL 配置
```json 
{
    "keys": ["f5"],
    "caption": "SublimeREPL: Python - RUN current file",
    "command": "run_existing_window_command",
    "args":
    {
        "id": "repl_python_run",
        "file": "config/Python/Main.sublime-menu"
    }
}
```
按F5可进行交互，例如input（）,
2.AutoPep8
自动将 Python 代码按 PEP8 规范格式化，安装完添加如下配置可自动在保存文件的时候格式化：
```json
{
	"format_on_save": true,
}
```
3.选择一个代码提示工具，比如SublimeCodeIntel，Jedi。

2017.11.8