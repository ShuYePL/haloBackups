---
title: git push文件error: src refspec master does not match any
date: 2022-07-07 11:08:28.349
updated: 2022-07-07 11:11:20.025
url: /archives/gitpush-wen-jian-errorsrcrefspecmasterdoesnotmatchany
categories: 
- Git
tags: 
- 报错
---

完整报错信息
~~~
error: src refspec master does not match any
error: failed to push some refs to 'https://github.com/ShuYePL/JavaWeb.git'
~~~
当时我写的 push 命令是 git push demo "master"
但是，在仓库中分支的名字是 main ，写错分支的名字导致发生了这个错误，改正后的命令如下
~~~
git push demo "master"
~~~
改正后就能成功上传了。