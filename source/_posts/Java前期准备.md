---
title: Java前期准备
date: 2021/3/11
tags: [初始java]
categories: Java
---

# Java 前期准备

## 一：Java 开发场景

- Java 开发场景 1——SSM
- Java 开发场景 2——Android 核心代码
- Java 开发场景 3——大数据

## 二：Java 应用领域

- 企业级应用
  - 大企业的软件系统
  - 各种类型的网站
- Android 平台的应用
- 移动领域应用
  - 消费和嵌入式领域，是指在各种小型设备上的应用

## 三：Java 技术体系平台

- Java SE 标准版
- Java EE 企业版
- Java ME 小型版

## 四：基础知识点

- 什么是程序：计算机执行某些操作或解决某个问题而编写的一系列有序指令的集合

### Java 的重要特点

- Java 是面向对象的
- 强类型机制，异常处理，垃圾的自动收集
- 跨平台性（Java 虚拟机机制）
  - .java 文件编译成.class 文件，然后可在 windows 和 Linux 等多个系统上运行
- 解释性
  - 编译后的代码，不能直接被机器执行，需要解释器解释才能被机器执行，编译性语言，编译后的代码，可以直接被机器执行（c/c++）

### <b style="color:skyblue">Java 运行机制及运行过程</b>

- 因为有 JVM，才能支撑 Java 程序在三个操作系统中都可以执行，所以需要安装 JVM（存在 jdk 中）
- JVM 是一个虚拟的计算机，具有指令集并使用不同的存储区域，负责执行指令，管理数据，内存，寄存器，不同的平台系统有不同的 JVM，JVN 存在于 jdk 中
- **JDK**：Java 开发工具包，=JRE + java 开发工具（java，javac 等） ，而 JRE（Java 运行环境）=JVM + Java 核心类库

### <b style="color:skyblue">JDK,JRE,JVM 的包含关系</b>

- JDK = JRE + 开发工具包（javac，java 等编译工具）
- JRE = JVM + java SE 标准类库
- JDK = JVM + java SE 标准类库 + 开发工具包
- 如果只需要运行 Java，只需要安装 JRE
- 安装 JDK：官网直接安装即可

### <b style="color:skyblue;">JDK 配置</b>

- 配置环境变量 path
  - 理由：当前执行的程序在当前目录下如果不存在，win10 系统会在系统中已有的一个名为 path 的环境变量指定的目录中查找。如果仍未找到，会出现错误提示
  - 添加一个 JAVA_HOME 的环境变量（指向 JDK 的安装目录）
  - 编辑 PATH 环境变量，增加%JAVA_HOME%\bin，指向 bin 文件
- 配置好之后在任意目录里打开 cmd 窗口，输入 javac 等命令不会报错即可

### Java 开发工具

- sublime （初期推荐）
- IDEA （项目推荐）
