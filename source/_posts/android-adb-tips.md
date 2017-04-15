---
layout: post
title:  "Adb笔记"
date:   2016-12-06
categories: Android
excerpt: Android
---

### **adb**
1. 通过adb保持屏幕常亮(当USB连接时)

	`svc power stayon usb`

2. 查看应用列表

	格式：

	`adb shell pm list packages [-f] [-d] [-e] [-s] [-3] [-i] [-u] [--user USER_ID] [FILTER]`

	| 参数       | 显示列表                   |
	|------------|----------------------------|
	| 无         | 所有应用                   |
	| -f         | 显示应用关联的 apk 文件    |
	| -d         | 只显示 disabled 的应用     |
	| -e         | 只显示 enabled 的应用      |
	| -s         | 只显示系统应用             |
	| -3         | 只显示第三方应用           |
	| -i         | 显示应用的 installer       |
	| -u         | 包含已卸载应用             |
	| `<FILTER>` | 包名包含 `<FILTER>` 字符串 |







