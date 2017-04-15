---
layout: post
title:  "Android中的Tasks和Back Stack"
date:   2016-04-23
categories: android
excerpt: android
---


关于Activity的Task和Back Stack，一直比较困惑，这里做一些笔记，大多摘自官网API指南。

### **概念**


> Task是指在执行特定工作时与用户交互的一系列`Activity`。这些`Activity`按照各自打开的顺序排列在堆栈(即“返回栈”)中。

### **工作流程**

设备主屏幕是大多数Task的起点。当用户触摸应用启动器中的图标（或主屏幕上的快捷键）时，该应用的Task将出现在前台。 如果应用不存在Task（应用最近未曾使用），则会创建一个新Task，并且该应用的“主”Activity 将作为堆栈中的根 Activity 打开。

当前 Activity 启动另一个 Activity 时，该新 Activity 会被推送到堆栈顶部，成为焦点所在。 前一个 Activity 仍保留在堆栈中，但是处于停止状态。Activity 停止时，系统会保持其用户界面的当前状态。 用户按“返回”按钮时，当前 Activity 会从堆栈顶部弹出（Activity 被销毁），而前一个 Activity 恢复执行（恢复其 UI 的前一状态）。 堆栈中的 Activity 永远不会重新排列，仅推入和弹出堆栈：由当前 Activity 启动时推入堆栈；用户使用“返回”按钮退出时弹出堆栈。 因此，`返回栈以“后进先出”对象结构运行`。 图 1 通过时间线显示 Activity 之间的进度以及每个时间点的当前返回栈，直观呈现了这种行为。

![](http://developer.android.com/images/fundamentals/diagram_backstack.png)

**图 1**. *显示Task中的每个新 Activity 如何向返回栈添加项目。 用户按“返回”按钮时，当前 Activity 随即被销毁，而前一个 Activity 恢复执行。*

如果用户继续按“返回”，堆栈中的相应 Activity 就会弹出，以显示前一个 Activity，直到用户返回主屏幕为止（或者，返回Task开始时正在运行的任意 Activity）。 当所有 Activity 均从堆栈中删除后，Task即不复存在。

Task是作为一个整体，当用户开启新的Task或者返回Home界面时，该Task会移动到”后台“。尽管在后台时，该Task中的所有Activity全部为停止状态，但是该Task的返回栈仍旧不变，也就是说当另一个Task启动之后，该Task仅失去焦点而已。
然而，Task可以返回到“前台”，用户就能回到离开时的状态。

![](http://developer.android.com/images/fundamentals/diagram_multitasking.png)

**示例1**. *两个Task：Task B 在前台接收用户交互，而Task A 则在后台等待恢复。*

> **注意：** 后台可以同时运行多个Task。但是，如果用户同时运行多个后台Task，则系统可能会开始销毁后台 Activity，以回收内存资源，从而导致 Activity 状态丢失。

由于返回栈中的 Activity 永远不会重新排列，因此如果应用允许用户从多个 Activity 中启动特定 Activity，则会创建该 Activity 的新实例并推入堆栈中（而不是将 Activity 的任一先前实例置于顶部）。 因此，应用中的一个 Activity 可能会多次实例化（即使 Activity 来自不同的任务）。

![](http://developer.android.com/images/fundamentals/diagram_multiple_instances.png)

**示例2**. 一个 Activity 将多次实例化。

**Activity 和任务的默认行为总结如下**

- 当 Activity A 启动 Activity B 时，Activity A 将会停止，但系统会保留其状态（例如，滚动位置和已输入表单中的文本）。如果用户在处于 Activity B 时按“返回”按钮，则 Activity A 将恢复其状态，继续执行。
- 用户通过按“主页”按钮离开任务时，当前 Activity 将停止且其任务会进入后台。 系统将保留任务中每个 Activity 的状态。如果用户稍后通过选择开始任务的启动器图标来恢复任务，则任务将出现在前台并恢复执行堆栈顶部的 Activity。
- 如果用户按“返回”按钮，则当前 Activity 会从堆栈弹出并被销毁。 堆栈中的前一个 Activity 恢复执行。销毁 Activity 时，系统绝对不会保留该 Activity 的状态。
- 即使来自其他Task，Activity 也可以多次实例化。

