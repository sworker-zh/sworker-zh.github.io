---
title: virtualenv（一）：python虚拟环境的安装
date: 2018-03-22 11:30:00
tags: virtualenv  
categories: 技术翻译
---
# 安装

### 警告
我们建议安装virtualenv-1.9或者以后的版本，不过优先选择1.9版本的。它包含在virtualenv中的pip没有通过SSL从PyPI下载。

### 警告
如果通过pip的方式安装virtualenv，我们建议使用pip的1.3版本或者更高的版本。优先选择pip的1.3版本。

### 警告：
当使用的安装工具低于0.9.7版本的时候，我们不建议使用通过easy_install的方式进行安装。因为它没用通过SSL从PyPI进行下载而且会在某些情况下发生不知名错误。

<!---------------more------------------->
通过pip进行全局安装（如果你的pip版本在1.3版本及以上）
```
$ [sudo] pip install virtualenv
```

或者获取最新的版本
```
$ [sudo] pip install https://github.com/pypa/virtualenv/tarball/develop
```

通过源码进行全局安装
```
$ curl -O https://pypi.python.org/packages/source/v/virtualenv/virtualenv-X.X.tar.gz
$ tar xvfz virtualenv-X.X.tar.gz
$ cd virtualenv-X.X
$ [sudo] python setup.py install
```

通过源码在本地指定位置安装
```
$ curl -O https://pypi.python.org/packages/source/v/virtualenv/virtualenv-X.X.tar.gz
$ tar xvfz virtualenv-X.X.tar.gz
$ cd virtualenv-X.X
$ python virtualenv.py myVE
```
































