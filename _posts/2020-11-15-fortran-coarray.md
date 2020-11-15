---
layout:     post
title:      Fortran Coarray
subtitle:   Coarray 介绍
date:       2020-11-15
author:     Euler-37
header-img:
catalog: true
tags:
    - Fortran
---
# Coarray
## 简介
Fortran 在2018标准中引入了Coarray,用来抽象mpi,目前的实现有 opencoarrays,ifort-coarray.

## 基本语法
* image

    coarray中,每一个进程称为一个image,普通定义的变量在每一个image都是自主定义的.利用内置函数
    `myid=this_image()`可以给出该操作所进行的image,`proces=num_images()`给出核心数。
    ```fortran
    program main
        implicit none
        integer::myid,proces
        proces=num_images()
        myid=this_image()
        write(*,*)myid
    end program
    ```
* coarray 是为了不同image之间通讯所建立的，如果不需要通讯，直接加编译选项，实际上程序以及默认开启了并行。

    ``` fortran
    program main
        implicit none
        real(8)::r
        call random_seed()
        call random_number(r)
        write(*,*)r
    end program
    ```
* 定义一个Coarray

    ``` fortran
    integer::a(10)[*]
    ```
    如上定义一个一维数组a,且是一个coarray，当然也支持自定义的类型。

    coarray支持高维，用于描述image之间的关系
    ``` fortran
    integer::a(10)[5,5,*]
    integer,allocatable::b(:)[:,:,:]
    allocate(b(10)[5,5,*])
    !allocate(b(this_image())[5,5,*])!wrong
    ```
    但是其中必须有一个维度为`[*]`,不同image分配的数组维度必须相同

* 同步

    `sync all`用于同步不同的image,在需要同步时使用

## 内置函数
* `co_max(a,result_image)`

    找到所有image中最大的，结果返回到`result_image`,a不一定是定义为coarray,只要在这个环境中的变量即可

* `co_min(a,result_image)`

    求最小，参数同上

* `co_sum(a,result_image)`

    求和，参数同上

* `co_broadcast(a,source_image)`

    将`source_image`中的a值广播给每一个image
    ``` fortran
    program main
        implicit none
        integer::val(3)
        if(this_image()==1)then
            val=[1,3,5]
        end if
        call co_broadcast(val,source_image=1)
        !val=[1,3,5]
    end program
    ```
* `co_reduce(a,operation,result_image)`

    对a执行`operation`的迭代操作，结果返回`result_image`
    ``` fortran
    program main
        implicit none
        integer::val[*]
        val=this_image()
        call co_reduce(val,myprod,result_image=1)
        if(this_image()==1)then
            write(*,*)"prod",val
        end if
    contains
        pure function myprod(a,b)result(f1)
            integer,value::a,b
            integer::f1
            f1=a*b
        end function
    end program
    ```












