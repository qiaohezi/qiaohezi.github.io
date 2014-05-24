---
layout: post
title: [Lua]require的用法
description: Github本身就是不错的代码社区，他也提供了一些其他的服务，比如Github Pages，使用它可以很方便的建立自己的独立博客，并且免费。
category: blog
---

require函数的搜索路径由LUA_PATH决定。若要更改或者增加，则如下
```lua
package.path = package.path .. ";?.lua"
```
搜索时会将?替换为require的参数，若搜索的文件存在，则返回true，若搜索完所有的路径皆失败，则返回false

require加载文件成功后