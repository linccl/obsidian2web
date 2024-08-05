---
title: Obsidian 基础
authot: lincc
date: 2024-07-10 周三
time: 15:32
tags:
---

## 语法
#### 双链
使用两个`[]` 符号形成 `[[]]` 

[[Markdown]]

如果只需要链接文章中某一个段落, 则再`[[]]`中的标题末位输入`#`即可选择标题

[[Markdown#14 待办列表]]

如果要链接的内容没有标题, 或是不需要链接标题内完整的内容,只需要链接标题内一部分内容, 则在`#`后面输入`^`即可选择段落。并且会在被链接的内容添加标记链接位置的角标。角标被修改后，链接会出错。

[[Markdown#^3ba269]]

可以在想要被链接的段落后输入`^自定义的名称`， 然后就可以使用更方便记忆和理解的名称来进行链接

[[Markdown#^bbb]]

链接别名可以在链接末位输入`|`来进行命名

[[Markdown#^bbb|bbb]]

想要将链接的内容直接呈现在文章中，只需要在`[[]]`前加上`!`

![[Markdown#14 待办列表]]

## 搜索
所有的搜索技巧都是可以组合使用的
- 多关键词搜索
	- 搜索包含多个关键词的笔记: 关键词之间使用空格间隔
	- 搜索包含某一个关键词的笔记: 关键词之间用` OR `来连接: `列表 OR 笔记`
	- 搜索包含某个关键词, 但不包含另一个关键词: 关键词之间用` - `来连接: `列表 - 笔记`
- 指定搜索范围
	- 搜索文件名: `file:word`
	- 搜索文本内容: `content:word`
	- 搜索标签: `tag:#word`
	- 搜索同一行中的多个关键词: `line:word1 word2`
	- 搜索同一章节中的多个关键词: `section:word1 word2`
	- 搜索同一段落(块)中的多个关键词: `block:word1 word2`
- 搜索任务
	- 搜索任务关键词: `task:word`
	- 搜索所有任务: `task:""`
	- 搜索所有未任务: `task-todo:""`
	- 搜索所有已任务: `task-done:""`
- 保存查询结果
	- 代码 `query`: 在指定语言为 `query` 在代码框中输入搜索条件
```query
列表 OR 笔记
```
## 使用 Dataview 进行查询
- 查询依据: YAML 数据 / Meateinfo
- YAML
	- 位于 Markdown 文件开头
	- 首尾三个`-`   [[template_metainfo_yaml|yaml模板]]
	- Obsidian 自带的 YMAL 字段
		- tags
		- publish
		- cssclass
		- aliases
	- 自定义字段
		- category
		- data
		- time
		- title
		- rating
	- 行内标记
		- One Field:: Value
		- 这份文档可以打 [rating:: 5] 分
	- [ ] `todo`: 增量式学习(等要用到的时候再学习并完善剩下的)
## 快捷键 

根据自己使用习惯去设置

