---
title: Ubuntu 安装
date: 2020-10-17 21:01:11
categories: learn
tags: ubuntu
---

# Ubuntu 安装使用

## Ubuntu 安装

### Ubuntu下载

* 推荐使用[USTC](https://mirrors.ustc.edu.cn/)下载镜像。

## Ubuntu 基础配置

* 更换软件源

  使用 USTC 的 Ubuntu 源，配置方式见[官网](https://mirrors.ustc.edu.cn/help/ubuntu.html)

  简单的配置方式如下：

  ```shell
  sudo sed -i 's/archive.ubuntu.com/mirrors.ustc.edu.cn/g' /etc/apt/sources.list
  
  # 更新软件索引
  sudo apt update
  # 更新软件包
  sudo apt upgrade
  ```

* 删除一些不需要的预装软件

  ```shell
  sudo apt autoremove 
  ```

* 安装 HomeBrew

  ```shell
  # 先安装 curl 和 git
  sudo apt install curl git -y
  # 安装 HomeBrew
  /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
  ```

  *如果提示网络问题，可以将`install.sh`文件复制到本地后运行`bash install.sh`*

## Ubuntu 基础软件

* 安装搜狗输入法

  下载Ubuntu搜狗输入法：https://pinyin.sogou.com/linux/?r=pinyin

  ```shell
  sudo dpkg -i sogoupinyin_2.3.2.07_amd64-831.deb
  
  # 提示缺少依赖时，使用如下命令
  sudo apt install -f
  ```

  