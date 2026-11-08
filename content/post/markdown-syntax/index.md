---
title: Markdown Syntax Guide
date: 2026-11-07
description: 学习markdown语法和文件头语法，这是我们hugo搭建Blog的基础知识，影响文章的显示和语法，必须先学会
tags: 
    - skill
categories:
    - syntax of markdown
    - syntax of Front Matter
draft: false
weight: 2 
slug: markdown-learning
image: OADD2.jpg
--- 

This article offers a sample of basic Markdown syntax that can be used in Hugo content files, also it shows whether basic HTML elements are decorated with CSS in a Hugo theme.

<!--正文-->
## markdown语法
**便捷好用的文本文件类型，后缀改成md就能启用**

### 文章分界线

***
【***独占一行】

### 文本标题化

##### H5
###### H6

【文本前面加#】           
  * 只有##二级标题 ~ ####四级标题会在右侧标题栏显示     
    -> 我们可以设计成目录

### 文本处理

我是第一行     
我是第二行

换行：【结尾空4格】    

* 我是无序标注     
  1. 我是有序标注     
  2. 我是有序标注     
  * 我是内嵌无序
    1. 我是内嵌有序    

【文本前面加*/1.】

**被加粗字体**    
【文本用**框住】  
*被斜体字体*    
【文本用*框住】

`被引用内容`      
【文本用``框住】，我们一般对文本中的命令/代码使用

### Tables表格

当你需要使用时，请打开Blog的hugo源文件查看语法

   Name | Age
--------|------
    Bob | 27
  Alice | 23

### Code block with backticks

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Example HTML5 Document</title>
</head>
<body>
  <p>Test</p>
</body>
</html>
```
代码块会帮你做高亮处理      
当你需要使用时，请打开Blog的hugo源文件查看语法

### Diff code block

```diff
[dependencies.bevy]
git = "https://github.com/bevyengine/bevy"
rev = "11f52b8c72fc3a568e8bb4a4cd1f3eb025ac2e13"
- features = ["dynamic"]
+ features = ["jpeg", "dynamic"]
```
也许你会用到吧     
当你需要使用时，请打开Blog的hugo源文件查看语法

## Other Elements — abbr, sub, sup, kbd, mark
**hugo支持部分html的元素语法**

<abbr title="Graphics Interchange Format">GIF</abbr> is a bitmap image format.

H<sub>2</sub>O

X<sup>n</sup> + Y<sup>n</sup> = Z<sup>n</sup>

Press <kbd>CTRL</kbd> + <kbd>ALT</kbd> + <kbd>Delete</kbd> to end the session.

Most <mark>salamanders</mark> are nocturnal, and hunt for insects, worms, and other small creatures.


## Front Matter文件头的语法
**hugo支持设计文件文件头渲染**

【`---`框住，放在文件最上方】     

``` 
---     
title: Markdown Syntax Guide 
date: 202-11-07 # 发布时间     
description: Sample article showcasing basic Markdown syntax and formatting for HTML elements. 
tags: 
    - skill
categories:
    - syntax of markdown
    - syntax of Front Matter
weight: 1       # 权重，数字越小，文章越靠前，间隔设置的大一下，留下容错空间

<!-- 上述的是头文件基础模板，基本上都需要 -->

slug: hello-world # 覆盖这篇文章的URL路径的最后一段
image: cover.jpg # 文章的背景，选择文章同目录下的图片的相对路径，大小1000x677
--- 
```