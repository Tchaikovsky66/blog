---
layout: '../../layouts/MarkdownPost.astro'
title: '[ROS笔记]launch文件'
pubDate: 2023-4-8
description: 'launch文件用法'
author: 'shi'
cover:
    url: '/melodic.jpg'
    square: '/melodic.jpg'
    alt: 'cover'
tags: ["笔记","ros","launch","xml"] 
theme: 'light'
featured: true
---
  ### launch文件

通过xml文件实现配置和启动(自动启动ros master)

### launch文件语法

#### 启动< launch > / < node >

~~~xml
<launch> #launch文件根元素
	<node pkg="turtlesim" name"sim1" type="turtlesim_node">
	<node pkg="turtlesim" name"sim1" type="turtlesim_node">
</launch>
~~~

+ pkg: 节点所在功能包名称
+ type: 节点的可执行文件名称
+ name: 节点运行时的名称
  + 会取代原程序中节点名称
+ Output,respawn,requirnargs

**参数设置**

#### 参数< param > / < rosparam >

~~~xml
#设置ros系统运行中的参数,储存在参数服务器中
<param name="output_frame" value="odom"/>
#加载参数文件中的多个参数
<rosparam file="params.yaml" command="loadns="params"/>
~~~

+ Name: 参数名
+ value: 参数值

#### 参数< arg >

~~~xml
#launch文件内部的局部变量,仅限于launch文件内使用
<arg name="arg-name" default="arg-value"/>
~~~

+ Name: 参数名
+ value: 参数值

调用:
~~~xml
<param name="foo" value="$(arg arg-name)"/>
<node name="node" pkg="pkckage" type="type" arg="$(arg arg-name)"/>
~~~

#### 重映射< remap >

重映射ros计算图资源的命名

~~~xml
<remap from="turtlebot/cmd_vel" to= "/cmd_vel"/>
~~~

+ From: 原命名
+ to: 映射只后的命名

#### 嵌套

包含其他launch文件,类似c中的include

~~~xml
<include file="$(dirname)/other.launch"/>
~~~

+ File: 包含其他launch文件路径

### 创建功能包

~~~
$ catkin_create learning_launch
$ vim learning_launch/simple.launch
~~~

![代码示例](/launch/image-20230407180153585.png)

~~~
$ catkin_make
$ roslaunch learning_launch simple.launch
~~~

![运行结果](/launch/image-20230407174755417.png)


