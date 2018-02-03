---
title: 'vue-fmt: Atom 上 Vue 文件格式化和美化工具'
date: 2018-02-03 22:28:03
tags: [vue,tools]
---

# vue-fmt (https://github.com/voodeng/vue-fmt)

> ✌ Vue文件格式化和美化工具，Atom插件

近期在工作上使用 Vue 的工具链来写一些项目，就近期的感觉来说，vscode的活跃度还是比Atom高挺多的，对于Vue项目来说，在vscode上边有挺多完善的工具插件比如(vetur)，而且在最近都有更新，而Atom上，部分都老旧了点，更新也不够活跃。

Atom 除了慢点，没别的毛病了🤣，外观和强定制性是用下去的动力。

![放张现用的图](http://ww4.sinaimg.cn/large/87c01ec7gy1fo3n3tn8ayj20tv0n4tcw.jpg)

<!-- more -->

## 插件功能

这个插件，参考了vue-format和atom-beautify，根据自身的需求重新写了这一份工具
ps: js为地基的程序就是这点好，有需求什么的，没有现成的可用，那就自己上手改！而且还挺快...

#### 格式化(Format)
格式化Vue文件，如果不是*.vue文件，会有提示，其中
- `template`: 
使用的是我修改过得`js-beautify`工具，lang暂时不支持，这个需要根据lang属性重新进行内容的解析，或者其他支持该语言的工具
- `script`: 
使用的是 `prettier`和`prettier-eslint`，支持 ts
- `style`: 
使用`prettier`, 支持 sass,scss,css,less,postcss


#### 排序(Sort)
顶级标签的排序，主要是最近看了下Vue官方的[风格指南](https://cn.vuejs.org/v2/style-guide/#%E5%8D%95%E6%96%87%E4%BB%B6%E7%BB%84%E4%BB%B6%E7%9A%84%E9%A1%B6%E7%BA%A7%E5%85%83%E7%B4%A0%E7%9A%84%E9%A1%BA%E5%BA%8F-%E6%8E%A8%E8%8D%90)，算是一个整理用的功能。

在保持原有代码顺序不变的情况下，根据设置的顺序先后来重新排列。

> 需要注意的是，开启排序的话，会清除这3个顶级标签以外的东西的。


#### 缩进(Indent)
一些插件都忽略了这个简单的需求了，开启后，会根据设置来给顶级标签内的内容加上一个缩进。

这个应该是很多人都需要的，Atom里边，标签内没有缩进的话，无法折叠...


#### 新行(Newline)
开启后，会在顶级标签的内容上下加上新的一行。

单独用prettier，会给支持的内容`end_with_newline`，这里我给修改了下输出，保持统一。要么上下加要么不加...

#### Html 属性对齐(Html attr wrap)
这个的实现是稍微麻烦点的，更具最新的`js-beautify`修改了部分内容来实现，并且把这个包放在插件里边。

添加了`aligned`的方式，确切的说是cut off align
- 行超出指定宽度，attr换行并对齐
- 已换行的attr，保留第一个属性和标签同行，其他的进行换行
- 未换行的保持不变

#### prettier-eslint
`prettier-eslint`本身如果文件是*.vue结尾的，会找不到`*.eslintrc.*`的设置，因为它暂时解析不了vue单组件文件，但是我们只是需要格式化script里边的内容，本身是没问题的。

这里我对它的 filePath 指定了当前文件后缀加上`.js`来使其找到eslint设置。对，很狂野的方式...等待社区的答案或者作者的修护后，再重新更进。

## 预览和安装
![feature](http://ww1.sinaimg.cn/large/87c01ec7gy1fo30g5rh4ug20qe0owwzu.gif)


```bash
apm install vue-fmt

# or

cd ~/.atom/package
git clone https://github.com/voodeng/vue-fmt && cd vue-fmt
npm install
```

## PR & Issues
问题反馈 ❤ [Github](https://github.com/voodeng/vue-fmt/issues)
<a href="https://github.com/voodeng"><img src='http://ww4.sinaimg.cn/large/87c01ec7gy1fo3p0pnsraj207n0130sj.jpg' height='30' align='left' /></a>