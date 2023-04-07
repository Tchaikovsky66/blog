---
layout: '../../layouts/MarkdownPost.astro'
title: '编译器compiler'
pubDate: 2022-9-12
description: '[学习记录]'
author: 'shi'
cover:
    url: '/computer.png'
    alt: 'cover'
tags: ["笔记", “学习”] 
theme: 'light'
featured: true
---
# 编译原理[入门]--编译器compiler
参考：https://www.youtube.com/watch?v=cxNlb2GTKIc&list=PLTd6ceoshpreZuklA7RBMubSmhE0OHWh_编译原理入门
#### 编译器作用
+ 将高级语言转化为机器语言
+ 正确运行
+ 发现所有错误
+ 快速编译
+ 易用

#### 编译过程
1. 词法分析Lexical Analysis(scanner)：将词句分割
2. 语法分析Syntax Analysis(parser):  分析语义
3. 机器码生成Code Generation and Optimization: 翻译成另一个语言


## 1. 词法分析(scanner)
### 编译器进行扫描
==目的：明确源码中每个词的含义==
```
if temperayion <= 0 then\n
	\t Description = "Freeziong"\n
end if 
```

![](https://mg-blog.csdnimg.cn/32af60e322fb4ad58fa0da6fc81c38d1.png)


### 符号表
==对比---->建立符号表：对信息进行快速访问==
![](https://img-blog.csdnimg.cn/9fff66b0694d4dbd847b13c0064de134.png)


### Lexical Analysis summary
+ Done by the lexer,also know as the scanner
+ identifies lexemes within the source code
+ White space and comments are discarded
+ Generates a stream of tokens
+ Each token passed to the parser on request
+ creates a symbol table of  'names' used in the source code
+ may also create a string table



## 2.语法分析(parser)
==作用：发现已标记化的程序中的任何语法错误==
### 上下无关法（BNF）

```
<if_statement>::=If<condition>Then<statement>
				|If<condition>Then<statement>End If
				|If<condition>Then<statement>Else<statement>Enf If
```

```
if temperayion <= 0 then\n
	\t Description = "Freeziong"\n
end if 
```

### 构造数据结构
词法分析>>token>>分析>>调用>>插入分析树>>分析语法是否正确

![](https://img-blog.csdnimg.cn/b5e42d39b7b04cafa9765529f14efe95.png)

+ 

### 语义分析(Semantic Analysis)
==对句子的意思进行分析==



![](https://img-blog.csdnimg.cn/874d7fe70ee643eab5dd678888169f92.png)
抽象语法树

+ 简化语法树

+ 访问\修改表

+ 分配地址(堆)

### 数学表达式

![](https://img-blog.csdnimg.cn/085d559bccf64f049dcf259ca61e5152.png)


### parser summary

+ Undeclared variables
  未定义变量
+ Multiple declarations within the same scope
  重复定义
+ Misuse of reserved identifiers 
  滥用保留关键字
+ Attempting to access a variable that is out of scope
  尝试在超出变量作用域范围访问
+ Type mismatches 
  类型不匹配
+ Parameter type mismatches when calling functions and procedures 
  执行函数和程序时参数类型不匹配

![](https://img-blog.csdnimg.cn/78ee455197b3406da2db47b00be2db45.png)


## 3.中间代码生成(Intermediate Code)
Intermediate code generation
+ 在抽象语法树生成或者生成完之后生成中间代码
+ Three Address Code(TAC) 三位址码
+ Allows for significant machine independent optimizations

### 三位址码(Three Address Code)
![](https://img-blog.csdnimg.cn/245e048c8722479c8cb71a14a8f0928b.png)

+ Common intermediate representation of a program
  - 中间表达形式
  - Front Ende 
  
+ Each assignment instruction can only have one operator
  - 每条赋值指令只能有一个操作符，总共最多三个地址
  - e.g.    x = (y+z) * w ---> t1 = y+z, t2 = t1 * w , x = t2
  
+ Instructions may be implemented as objects or records
  - 可以以面向对象的方式实现三位指令、不同类型如int、long
  - 作为记录包含地址字段的数据结构进行实现
  - 使用三元组和四元组（运算子，算子1，算子2 ，结果）进行记录
  
+ Each name actually a pointer to an entry to a symbol table 
  - 变量名被替换成指向符号表中某个条目的指针
  


```
//使用跳转到行的方式来替代跳转到代码标签
If x<= 0 Then	--->	    If x<= 0 Then goto L1	
	y = z					goto L2
End If					L1:y=z
						L2:...
```

### 反向修补(Backpatching)
![](https://img-blog.csdnimg.cn/eb4eeb6263da4b50ae89c0b5b3fdec46.png)

+ 维护暂时没确定目标的跳转指令列表
+ 编译器只需通过遍历一次代码，就能生成三位指令序列


### 基本块(Basic Blocks)

+ 始终在一起执行的指令所组成的序列
+ 为编译器提供各种类型优化的机会，不依赖于目标机器的体系结构
+ 分析改进程序中的流程控制
  - e.g.从三位码中消除多余的跳转指令
+ 让寄存器分配和将变量分配给寄存器更容易
  - 将变量分配给cpu寄存器或者主内存
  [外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-lHTdITqI-1662963358142)(C:\Users\Shiwe\AppData\Roaming\Typora\typora-user-images\image-20220911193811333.png)]
+ 扫描中间代码查找块边界,跳转到基本块而不是标签或者行号
+ 从第一个块进入,从最后一个块退出

### 基本块优化(Basic Blocks Optimization)

+ 减少cpu寄存器中保存的定时器变量值,释放这些以方便寄存器获取其他内容
  - 在某时访问另一个块,优化调整后就可能不需要了
+ 消除重复子表达式
  - t = x + y, u = x + y  ----->  t = x + y, u = t
+ 复写传播
  - y = 12 + 5能直接由编译器计算
  - 复写传播和常数折叠能一直继续计算
  - 仅适用于常量值的赋值任务
+ 删除dead code(死码)
  + e.g.    x = x +1,但是从未使用
+ 代数变换
  + e.g.   t = x + 0 ----> t = x;	t = x^2 -----> t = x * x;	t = x * 8 ---->t = x << 3
+ 优化块之间的信息流动方式
  + 一个块无法被跳转或者使用，那么可以被删除 
  + ```goto L1\n L1:goto L2```---->	```goto L2\n L1:goto L2```

### Intermediate Code Summary

+ 中间码是高级源码的低级线性表示
+ 中间代码独立于目标机器(target machine)
  - 由编译器前端创建
+ 三位址码是编译器实现中间代码的一种形式
+ Backpatching用于生成前跳指令
+ 中间代码被分为基本块
+ 窥孔优化(Peephole optimization)用于改进基本块的代码
+ 中间代码会被优化多次，但中间代码也不太可能是完美的。但以接近有效的目标代码
  + 已为最后编译阶段做好准备，即生成可执行的二进制机器码

## 4.目标代码生成(Object Code)

+ 指令选择
+ 寄存器分配和赋值
+ 指令顺序
  + 大部分由中间代码完成
### RISC VS CISC
![](https://img-blog.csdnimg.cn/15be782394e547eab713969fa6c9afa9.png)


### 指令选择(Instruction selection)
+ 将三址码转换为汇编代码
  + LD :load;  ST: store;  ...
+ 会出现不必要的冗余(红框内多余)
![](https://img-blog.csdnimg.cn/e1b4e286c7a14f16bd7e044f55cf4229.png)




### 寄存器分配和赋值(Register allocation and assignment)
+ ==通过数据结构跟踪程序变量在寄存器中的值，以避免不必要的加载(load)和储存(store)==
+ 寄存器描述符:保存当前在寄存器中所储存值对应变量的名称
+ 地址描述符：跟踪变量更新后的位置; 储存在符号表(symbol table)中

![](https://img-blog.csdnimg.cn/77c95ad2c967452091dd5b87f6999d2a.png)


### 窥孔优化(Peephole optimization)

![](https://img-blog.csdnimg.cn/2f1569e48bb34c7a8a0e2abab9783b20.png)


+ Code examined through a sliding window
+ 去除冗余标识符
+ 常量评估
+ 删除无法识别的代码段
+ 识别常见子表达式
+ 展开循环
+ Eliminating procedures 消除过程

### Object Code Generation Summary
+ 依赖于机器架构(machine architecture) [RISC or CISC]
  + 对精简指令集(RISC)相对简单
+ 通过简单的替换来选择指令
+ 通过抽象语法树指定匹配树重写规则来得到更高效的代码
+ Instruction selection based on processing cost
+ 寄存器分配和赋值通过寄存器描述符和地址描述符来完成
+ Peephole optomization
  + 在中间码生成时也可能使用
  (machine architecture) [RISC or CISC]
  + 对精简指令集(RISC)相对简单
+ 通过简单的替换来选择指令
+ 通过抽象语法树指定匹配树重写规则来得到更高效的代码
+ Instruction selection based on processing cost
+ 寄存器分配和赋值通过寄存器描述符和地址描述符来完成
+ Peephole optimization
  + 在中间码生成时也可能使用
