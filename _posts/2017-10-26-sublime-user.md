---
layout: post
title: 在sublime text中打开当前文件的所在目录
---


1.点击Preferences菜单,之后点击Key Bindings - User,如果打开的文件为空,那么点击Preferences,之后点击Key Bindings - Default
在打开文件的最下方加上一行代码:
```
{ "keys": ["alt+o"], "command": "open_dir", "args": {"dir": "$file_path", "file":"$file_name"}} 
```
保存后即可使用alt+o快捷键打开当前文件所在的目录.快捷键alt+o是可以修改的。
以下是完整用户配置
```javascript
[
	{ "keys": ["ctrl+alt+s"], "command": "save_all" },
	 { "keys": ["alt+m"], "command": "markdown_preview", "args": {"target": "browser", "parser":"markdown"}  },
	 { "keys": ["alt+o"], "command": "open_dir", "args": {"dir": "$file_path", "file":"$file_name"}} ,
]
```
