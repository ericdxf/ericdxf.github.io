---
title: Git自动提交代码
category: Git
date: 2022-05-16 18:45:53
tag: 脚本
---

### Git自动提交代码

```
@echo off
@title bat execute git auto commit
F:
cd F:/project/mdbook
git add .
git commit -m "Auto commit."
git push
```

如果不想看到黑窗口弹出，则在`@echo off`下一行添加如下代码即可

```
if "%1"=="h" goto begin
start mshta vbscript:createobject("wscript.shell").run("""%~nx0"" h",0)(window.close)&&exit
:begin
```

