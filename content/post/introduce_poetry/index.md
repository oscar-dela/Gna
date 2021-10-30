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

一般來說，不管是哪種語言的project，開發過程大概包含幾點

1. environment setup up
2. dependency install
3. development
4. test
5. build
6. publish artifact

當然後續還有 CI/CD，operation等等的步驟，但那不在本文的範疇。

如果對應到python來說，大概會是這樣的

1. environment setup up <-> virtualenv
2. dependency install <-> pip
3. development
4. test <-> pytest, unittest
5. build
6. publish artifact

在過去的工作經驗中，Python讓我們困擾的點大概有幾點

1. 就算有好好的寫死 (==) dependency version，但實際上在安裝的時候仍然可能因為dependency的dependency沒有寫死，導致兩次build出來的結果不一致。
2. 同樣是virtualenv，文件也有寫，但有人就是眼殘會裝到不同版本的Python。
3. 寫Python很快，跑起來很簡單。但如果今天是要寫python的package常常會花很多時間在查如何設定。

## 正文

### Why Poetry

為什麽需要Poetry? Poetry本身算是一個開發工具的集合，他把上述的6點做了統一的封裝，開發者只要專注在程式邏輯開發即可，不用煩惱環境問題。

### Poetry 101

我們直接實際試著用Poetry寫一個小小的爬蟲程式，用yfinance來抓NASDAQ 100過去20年的歷史資料。

#### Install Poetry

官方有提供一鍵安裝

```
curl -sSL https://raw.githubusercontent.com/python-poetry/poetry/master/install-poetry.py | python3 - 
```

#### Init a project

開啟新的project

```
poetry new poetry_101
```

會看到原本的目錄下新增了一個folder

```
➜  Desktop ls -al | grep poetry_101
drwxr-xr-x    6 bifro  staff       192 Oct 30 20:16 poetry_101
```

裡面包含了以下的檔案

```
➜  Desktop ls -al poetry_101
total 8
drwxr-xr-x    6 bifro  staff   192 Oct 30 20:16 .
drwx------@ 107 bifro  staff  3424 Oct 30 20:16 ..
-rw-r--r--    1 bifro  staff     0 Oct 30 20:16 README.rst
drwxr-xr-x    3 bifro  staff    96 Oct 30 20:16 poetry_101     <- 主程式
-rw-r--r--    1 bifro  staff   305 Oct 30 20:16 pyproject.toml <- Dependency放在這裡
drwxr-xr-x    4 bifro  staff   128 Oct 30 20:16 tests          <- unittest
```

可以看到現在 `pyproject.toml` 裡面已經有一些基本的環境設置了

```
[tool.poetry]
name = "poetry_101"
version = "0.1.0"
description = ""
authors = ["Mock Chern <mock.chern@gmail.com>"]

[tool.poetry.dependencies]
python = "^3.9"

[tool.poetry.dev-dependencies]
pytest = "^5.2"

[build-system]
requires = ["poetry-core>=1.0.0"]
build-backend = "poetry.core.masonry.api"
```

如果想指定python版本，可以藉由設定

```
poetry env use /full/path/to/python
```

#### Setup

我們先安裝yfinance

```
poetry add yfinance
```

可以看到poetry會幫忙create virtual environment，也會將dependency寫入到lock file

```
➜  poetry_101 poetry add yfinance
Creating virtualenv poetry-101-fNOwJ5Wf-py3.9 in /Users/oscar/Library/Caches/pypoetry/virtualenvs
Using version ^0.1.64 for yfinance

Updating dependencies
Resolving dependencies... (24.6s)

Writing lock file

Package operations: 21 installs, 0 updates, 0 removals

  • Installing six (1.16.0)
  • Installing certifi (2021.10.8)
  • Installing charset-normalizer (2.0.7)
  • Installing idna (3.3)
  • Installing numpy (1.21.1)
  • Installing pyparsing (2.4.7)
  • Installing python-dateutil (2.8.2)
  • Installing pytz (2021.3)
  • Installing urllib3 (1.26.7)
  • Installing attrs (21.2.0)
  • Installing lxml (4.6.3)
  • Installing more-itertools (8.10.0)
  • Installing multitasking (0.0.9)
  • Installing packaging (21.2)
  • Installing pandas (1.3.4)
  • Installing pluggy (0.13.1)
  • Installing py (1.10.0)
  • Installing requests (2.26.0)
  • Installing wcwidth (0.2.5)
  • Installing pytest (5.4.3)
  • Installing yfinance (0.1.64)
```

安裝完成後，會發現目錄下多了`poetry.lock`

```
➜  poetry_101 ls
README.rst     poetry.lock    poetry_101     pyproject.toml tests
```

這個檔案裡面會寫入所有用到的python dependencies，在不同環境安裝的時候是預設是根據這份設定，所以說更能確保環境的一致性。
