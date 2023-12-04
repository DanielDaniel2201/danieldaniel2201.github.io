---
permalink: /about/
title: "About"
---

# 王远秋

[Github](https://github.com/DanielDaniel2201)

## 教育经历

* 北京外国语大学 (2022 9月 - )
  * 西班牙语 - 本科(GPA: 3.85)

## 个人技能

* 编程语言：C, Java, Python, RISC-V汇编, SQL, scheme
* 数据结构和算法：用Java实现链表、栈、堆、树等基本数据结构和A star、排序等基本算法([code](https://github.com/DanielDaniel2201/CS61B-spring18))
* 计算机体系结构：RISC-V汇编编程，CPU设计，缓存机制，虚拟内存，多线程编程([code](https://github.com/DanielDaniel2201/CS61C-fall20-labs/tree/main/lab10))等
* 外语技能：英语四级614，西班牙语(年级前10%)

## 项目经历

* CS61CPU([code](https://github.com/DanielDaniel2201/CS61C-fall20-projects/tree/main/proj3/cpu)): 基于RISC-V开源指令集的CPU设计
  * CS61CPU是基于RISC-V开源指令集，通过Logisim数字电路模拟软件设计的通用CPU
  * CS61CPU采用了二级流水线设计，分为指令提取、指令运行两个阶段
  * 实现内容包括：算术逻辑单元(ALU)、寄存器文件(register file)、分支比较单元(branch comparator)、控制逻辑单元(control logic)
  * 此外编写了RISC-V汇编测试文件对CPU准确性进行测试

* numc([code](https://github.com/DanielDaniel2201/CS61C-fall20-projects/blob/main/proj4/src/matrix.c)): OpenMp, SIMD优化的矩阵运算库
  * numc是一个由C语言编写的矩阵运算库，功能包括矩阵乘法、矩阵幂运算、矩阵转置、矩阵加法、矩阵填充等基本运算
  * 在实现基本矩阵运算基础上，使用OpenMp和SIMD进行多线程编程优化
  * 经多线程优化显著提高运算效率

* HackAssembler([code](https://github.com/DanielDaniel2201/Nand2Tetris/tree/main/projects/06/Assembler))：简易的汇编语言编译器
  * HackAssembler是使用Java语言编写的编译器，将一门简易的汇编语言Hack转化为二进制码
  * 设计了Parser部分对输入的汇编语言进行解析，将输入指令分类并拆分指令成分
  * 设计了SymbolTable和Code对输入汇编程序的(预)操作变量和操作类型分配二进制码
  * HackAssembler采用了二次扫描设计来支持含有自定义名称的汇编程序

* BearMaps([code](https://github.com/DanielDaniel2201/CS61B-spring18/tree/master/proj3/src/main/java)): 基于A star算法的简易的导航地图
  * BearMaps根据网页端返回的经纬度信息，在图库中选取大小比例合适的地图板块发送给网页端进行呈现
  * BearMpas读取[OpenStreetMap](https://en.wikipedia.org/wiki/OpenStreetMap)数据集，将Berkeley地区各地点以node形式构成图数据结构进行储存
  * BearMaps基于A star最短路径算法，通过网页端返回的起点和终点经纬度，设计由node组成的最短路径并将信息返回网页端

* ants([code](https://github.com/DanielDaniel2201/CS61A-su20/blob/main/projects/ants/ants.py)): 基于Python面向对象编程的图形化交互游戏
  * ants基于Python面向对象编程模式，在图形化界面上支持玩家选用不同特性的ants与bees进行对抗
  * 采用合适的数据结构(列表、字典等)，对实例类型、数量等信息进行增删改查
  * 对该较大型项目进行拓展、测试

## 个人经历

* 矢量动画设计课 (2023 3月-6月)
  * 助教
* 北外TechLab技术部
  * 参与编写科研工具汇总
  * 组织开展知识图谱讲座
