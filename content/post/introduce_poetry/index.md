---
title: "像詩人一樣寫Python - Poetry"
date: 2021-10-30T17:45:09+08:00
draft: true
tags:
- Python
- Python Tools
categories:
- Tech
---
## 前言

一般講到Python Package管理，80%的人第一個想到的應該是pip。

的確pip很強大，也有`pip freeze > requirements.txt`可以reproduce環境設定。

但最常碰到的問題，就是pip裝了一堆dependency後，發現不知道誰是誰的，甚至會打架。那我們用virtualenv切開總可以了吧？好吧你贏了，的確是一種做法。

但是又有另一個問題是，單純用pip去安裝dependencies，有可能會有前者覆蓋後者的問題。

> 舉例來說，一個project有兩個dependencies(A, B)，會依賴於同一個dependency(C)。
> A depends on C > 1.0 & C < 1.5
> B depends on C > 1.7 & C < 2.0
> 這時候不管誰先裝，都會讓code壞掉








