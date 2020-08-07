---
layout:     post
title:      WSL-Gfortran
subtitle:   WSL配置Fortran所需的环境
date:       2020-08-07
author:     Euler-37
header-img: 
catalog: true
tags:
    - Fortran
    - WSL
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
    * gcc是c语言的编译器,还带了c++的编译器g++,gfortran 是Fortran的编译器

* blas,lapack
``` shell
  sudo apt-get install libopenblas-dev
  sudo apt-get install liblapack-dev
```
    * blas,lapack是线性代数的库函数，其中的openblas是blas的升级版，有并行和汇编加速，速度优于普通的blas库
* mpi,openmpi
``` shell
  sudo apt-get install mpich
  sudo apt-get install libopenmpi-dev
```
    * 安装并行支持，主要是基于mpi协议，因为openmp已经被gcc，gfortran编译器支持了
* coarray
``` shell
  sudo apt-get install libcoarrays-dev
  sudo apt-get install libcoarrays-openmpi-dev
```
    * 安装coarray的支持，coarray是Fortran2008的扩展语法，可以理解为mpi的一种抽象，利用Fortran 原生语法来写并行程序
