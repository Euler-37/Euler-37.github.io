---
layout:     post
title:      WSL-Gfortran
subtitle:   WSL配置Fortran所需的环境
date:       2020-08-07
author:     Euler-37
header-img: 
catalog: true
tags:
    - Xcode
    - iOS
---
# win10安装Linux子系统
* 具体教程网上查找
# 更换清华源 
* 按照[清华源Ubuntu](https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu/)更换源
``` shell
  sudo apt-get update
  sudo apt-get upgrade
```
# 安装必要的软件
* gcc,gfortran
``` shell
  sudo apt-get install gcc
  sudo apt-get install gfortran
```
* blas,lapack
``` shell
  sudo apt-get install libopenblas-dev
  sudo apt-get install liblapack-dev
```
* mpi,openmpi
``` shell
  sudo apt-get install mpich
  sudo apt-get install libopenmpi-dev
```
* coarray
``` shell
  sudo apt-get install libcoarrays-dev
  sudo apt-get install libcoarrays-openmpi-dev
```
