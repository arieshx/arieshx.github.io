---
layout: post
title: cuda编程
category: cuda
description: cuda 指南
---

## cuda架构特点

- CPU：擅长流程控制和逻辑处理，不规则数据结构，不可预测存储结构，单线程程序，分支密集型算法
- GPU：擅长数据并行计算，规则数据结构，可预测存储模式

## cuda线程模型

下面我们介绍CUDA的线程组织结构。首先我们都知道，线程是程序执行的最基本单元，CUDA的并行计算就是通过成千上万个线程的并行执行来实现的。下面的机构图说明了GPU的不同层次的结构。

从大到小是： Grid，Block，Thread

Grid
- 以1，2维组织
- 共享全局内存
Block 包含相互合作的线程块

-允许彼此同步
- 可以通过共享内存块交换数据
- 1,2,3维组织

Thread
- 线程，并行的基本单位

**kernel**

在GPU上执行的核心程序，这个kernel函数是运行在某个Grid上的。
- One kernel <-> One Grid

理解kernel：
- kernel在device上执行时启动多个线程，一个kernel启动的所有线程称为一个网格（grid），同一个网格上的线程共享相同的全局内存空间
- 网格又可以分为很多线程块（block），一个线程块里面包含很多线程，这是第二个层次。
- kernel调用时也必须通过执行配置<<<grid, block>>>来指定kernel所使用的网格维度和线程块维度

**SP,SM**
- SP：最基本的处理单元，streaming processor，也称为CUDA core。最后具体的指令和任务都是在SP上处理的。GPU进行并行计算，也就是很多个SP同时做处理
- SM：多个SP加上其他的一些资源组成一个streaming multiprocessor。也叫GPU大核，其他资源如：warp scheduler，register，shared memory等。SM可以看做GPU的心脏（对比CPU核心），register和shared memory是SM的稀缺资源。CUDA将这些资源分配给所有驻留在SM中的threads。因此，这些有限的资源就使每个SM中active warps有非常严格的限制，也就限制了并行能力

SP是线程执行的硬件单位，SM中包含多个SP，一个GPU可以有多个SM（比如16个），最终一个GPU可能包含有上千个SP。

软硬件之间的对应关系

thread ------  SP

Block  ------  SM

Grid   ------  GPU

- 每个线程由每个线程处理器（SP）执行
- 线程块由多核处理器（SM）执行
- 一个kernel其实由一个grid来执行，一个kernel一次只能在一个GPU上执行
- 一个block只会由一个sm调度，一个sm可以同时拥有多个block，但是要序列执行。

## cuda内存模型

CUDA中的内存模型分为以下几个层次：

- 每个线程都用自己的registers（寄存器）
- 每个线程都有自己的local memory（局部内存）
- 每个线程块内都有自己的shared memory（共享内存），所有线程块内的所有线程共享这段内存资源
- 每个grid都有自己的global memory（全局内存），不同线程块的线程都可使用
- 每个grid都有自己的constant memory（常量内存）和texture memory（纹理内存），），不同线程块的线程都可使用

线程访问这几类存储器的速度是register > local memory >shared memory > global memory

## cuda编程模型

**host和device**

我们用host指代CPU及其内存，而用device指代GPU及其内存。CUDA程序中既包含host程序，又包含device程序，它们分别在CPU和GPU上运行。同时，host与device之间可以进行通信，这样它们之间可以进行数据拷贝

典型的cuda程序的执行流程如下

- 分配host内存，并进行数据初始化；
- 分配device内存，并从host将数据拷贝到device上；
- 调用CUDA的核函数在device上完成指定的运算；
- 将device上的运算结果拷贝到host上；
- 释放device和host上分配的内存。

关键字表示程序在cpu还是在gpu上跑
- __global__ 定义在host，运行在device
- __device__ 定义在device，运行在device
- __host__  定义在host，运行在host

## 写代码

cu文件是用nvcc去编译执行的，因为其中有kernel函数，所有一定要用nvcc。





https://www.cnblogs.com/skyfsm/p/9673960.html