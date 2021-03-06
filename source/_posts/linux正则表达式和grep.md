title: linux正则表达式和grep
date: 2015-05-26 07:27:44
category: [linux]
tags: [正则表达式,linux命令]
---

##正则表达式

正则表达式描述了一种字符串匹配的模式，常用于：

- grep:从特定的文件中或从标准输入中查找含有某个字符串的行
- sed :从输入中读取信息，经过编辑后输出
- awk: 伪装成实用程序的强大编程语言，主要用于文本处理

###常用符号

####基本元字符

`^` 行首定位符，表示以..开始

`$` 行尾定位符，表示以..结束

`.`  匹配单个字符

`*` 匹配0个或任意多个字符

`[]` 匹配[]中出现字符范围内的一个字符

`\` 用来转义元字符，如\{m,n\},启用扩展元字符\? \+


####扩展元字符

`?` 匹配0个或者1个

`+` 匹配1个或者多个

`|` 或者

`()`分组符号

####特殊匹配字符

`[:alnum:]` 字母与数字字符

`[:alpha:]` 字母

`[:ascii:]` ASCII字符

`[:blank:]` **空格或制表符**

`[:cntrl:]` ASCII控制字符

`[:digit:]` 数字

`[:graph:]` 非控制、空格字符

`[:lower:]` 小写字母

`[:print:]` 可打印字符

`[:punct:]` 标点符号字符

`[:space:]` **空白字符，包括垂直制表符**

`[:upper:]` 大写字母

`[:xdigit:]` 十六进制数字

###实例

* ^ $ 
`ls -l | grep ^d`

匹配以d开头的所有内容

*	`ls -l | grep d$`

匹配以d结束的所有内容

*	`^$` 

匹配空行

*	`^.$`

只包含一个字符的

*	`* ？ +`

`compu*ter` 匹配u，重复0次或多次

`compu?ter` 匹配0个或者1个u

`compu+ter` 匹配1个或多个u

**需要注意的是+和？是扩展字符，需要看具体使用正则表达式的环境
如果使用grep需要使用-E指定为扩展模式才能正常使用+和？**

*	 `\`可以屏蔽一些特殊字符，如`$  .  ‘  “  *  [  ]  ^  |  (  )  \  +  ?`

`\.pass`  匹配`*.pass`

* `[]`
 
`[1234]` 匹配1,2,3,4中的一个

`[1-9]` 数字1-9中的一个

`[A-Za-z]` 所有字母

`[^0-9]` 一个非数字的字符

* `\{\}`

`A\{2\}B`   匹配AAB

`A\{4,\}B`   匹配A出现至少4次B

`A\{2,4\}B`  匹配A出现在2至4次之间

##grep

功能：grep是文本搜索工具，使用正则表达式搜索文本并打印匹配行

格式：`grep [options] PATTERN [Files]`

注：

输入字符串**作为参数，最好双引号括起**  “mystr”

在**调用变量时，也使用双引号括起**  “$MYSTR”

使用**正则[匹配模式]是，应使用单引号括起**  ‘49[32]’

常用选项：

`-c` 只输出匹配的行数,而不输出匹配的行

`-i` 不区分大小写

`-n` 显示匹配行及行号

`-q`  安静模式，不输出任何东西，如果找到了返回0

`-E` 启用扩展表达式，可使用扩展字符，如：+ ? | () {} , 或者直接使用egrep

`-v` 显示不包含匹配文本的所有行

实例：

1. 在多个文件中查找
`grep “sort” filea fileb`  在filea,fileb中查找

2. 计算匹配行数
`grep -c "sort" file`

3. 使用正则表达式查找
`grep '48[34]' file`

4. 使用扩展元字符
`grep -E 'aaa|bbb' file`

5. 匹配空行
`grep '^$' file`

6. 特殊匹配字符，grep 允许使用国际字符串模式匹配或匹配模式的类名
`grep ‘5[[:upper:]] [[:upper:]]’` data  5开头，两个大写
