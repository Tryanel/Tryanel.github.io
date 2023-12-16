---
title: "百科"
aliases: [Wiki]
tags: [default]
date: 2023-10-21
lastmod: 2023-12-10 19:27:38
time: 16:47
---

<hr style="border: none; border-top: 1px dashed #000; margin: 20px 0;">

### 文件夹变成 exe 怎么办？

首先到出问题的文件目录下创建一个.reg 文件和.bat 文件，

.reg 文件输入以下内容

```bash
Windows Registry Editor Version 5.00
[HKEY_LOCAL_ MACHINE \SOFTWARE\Microsoft\Windows\CurrentVersion
\Explorer\Advanced\Folder\Hidden
\SHOWALL] "CheckedValue"=dword:00000001
```

> 这个文件用于控制文件和文件夹的显示选项，特别是隐藏文件和文件夹的显示。通过在 HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Advanced\Folder\Hidden\SHOWALL 下创建一个名为"CheckedValue"的 DWORD（双字）值，其数值为`0x00000001`，也就是 1

.bat 文件输入以下内容

```bash
for /f “delims=” %%i in (‘dir /ah /s/b’) do attrib “%%i” -s -h
```

> 该脚本的功能是扫描系统中所有的隐藏文件，并将它们的"系统"和"隐藏"属性移除，使这些文件在资源管理器中可见。
>
> - `for /f "delims=" %%i in ('dir /ah /s/b') do`: 这是一个 for 循环，用于迭代执行后面的命令。在这里遍历所有的隐藏文件。
> - `dir /ah /s/b`: 这个命令用于列出当前目录及其子目录中的所有隐藏文件的完整路径。其中，`/ah`表示只显示隐藏文件，`/s`表示包括子目录，`/b`表示以简短的形式显示结果（仅路径）。
> - `attrib "%%i" -s -h`: 对于每个找到的隐藏文件，该命令将其"系统"和"隐藏"属性移除。`"%%i"`表示当前文件的路径。

分别双击运行它们即可恢复

<hr style="border: none; border-top: 1px dashed #000; margin: 20px 0;">

### PR如何给视频补帧？

> 使用慢镜头和没有场景切换的镜头

0. 新建序列

   设置

   - 编辑模式改为自定义
   - 时基改为60FPS
   - 帧大小根据情况
   - 像素宽高比：方形像素（1.0） 
   - 确定，导入素材

1. 查看所选视频画面是2/3帧或7/8帧或5帧一动

2. 确定之后选择视频按CTRL+R或者鼠标右键视频点击速度/持续时间……将视频加速

   加速表：

   |   帧率    |        加速值        |
   | :-------: | :------------------: |
   | 2/3帧一动 |         250%         |
   |  5帧一动  |         500%         |
   | 7/8帧一动 |         750%         |
   | n/m帧一动 | 平均值(n/2+m/2)×100% |

   原理：假设视频是每2或3帧一动，将视频进行加速，就等于是对视频中间不动的帧进行删除的过程，达到了删除重复帧的效果

3. 复制加速后的视频，复制数量由加速值决定，250%就复制3个，750%就复制8个

4. 复制完成后嵌套，并将Twixtor pro插件拖到嵌套序列上

5. 进入到效果设置

   speed：调整值为1/加速值，比如加速值250%则调40%降速回去

   warping选项的inverse选项改为Forward

   选中视频按回车键渲染（不是小数字键盘的回车）

6. 渲染完成，导出，格式为H.264

