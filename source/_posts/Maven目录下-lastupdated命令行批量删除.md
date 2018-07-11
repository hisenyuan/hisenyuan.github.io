---
title: Maven目录下*.lastupdated命令行批量删除
keywords: [maven,maven lastupdated]
tags: [maven,java]
date: 2017-02-20 23:50:48
categories: java
---
windows系统
```
cd %userprofile%\.m2\repository
for /r %i in (*.lastUpdated) do del %i
```
或者新建一个bat文件，批处理。就不用每次都在cmd敲命令了
```
@echo off
echo 确认删除maven仓库下*.lastUpdated文件？
pause
::create by hisenyuan(hisenyuan@gmail.com)

::这里写你的仓库路径
set REPOSITORY_PATH=C:\hisenwork\soft\maven
echo 正在搜索...
for /f "delims=" %%i in ('dir /b /s "%REPOSITORY_PATH%\*lastUpdated*"') do (
    del /s /q %%i
)
echo 完毕
pause
```
linux系统
```
find /app/maven/localRepository -name "*.lastUpdated" -exec grep -q "Could not transfer" {} \; -print -exec rm {} \;
```