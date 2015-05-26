title: linux命令sed和awk
date: 2015-05-26 07:39:28
category: [linux]
tags: [linux命令]
---

##sed
	
### 主要功能

sed，stream editor。是一个”非交互式“字符流编辑器。输入流通过程序并输出到标准输出端。
sed主要用来自动编辑一个或者多个文件（替换，插入，删除，追加，更改）

###常见应用

1. 抽区域
2. 匹配正则表达式
3. 比较域
4. 增加，附加，替换

###执行过程

sed一次处理一行或多行内容。处理时，把当前处理的行存储在临时缓冲区中，称为“模式空间”（pattern space），接着用sed命令处理缓冲区中的内容，处理完成后，把缓冲区的内容送往屏幕。接着处理下一行或多行，这样不断重复，直到文件末尾。文件内容并没有改变，除非你使用重定向或写入命令存储输出。


###调用方式

* 命令行输入

sed [选项] ‘sed命令’ 输入文件
* 使用sed脚本
sed [选项] –f sed脚本文件 输入文件

### 常用选项

-n：不打印，不写编辑行到标准输出，缺省情况下打印所有行[编辑/未编辑]p命令可以打印编辑行

-c：下一命令是编辑命令，使用多项编辑时加入此选项

-f： 调用sed脚本 sed –f sedScriptFile targetFile

-i：将修改附加到源文件上

###使用技巧

重定向sed结果输出
$sed 'sed-command' inputfile > outputfile

定位内容的方式

`x` 行x

`x,y` 行x到行y

`/pattern/` 模式

`/pattern/pattern/` 两个模式

`/pattern/,x`     模式+行【在给定行号上查询模式】

`X,y` /pattern/ 通过行号和模式查询匹配行

`X,y!` 不包含指定行号

###基本的sed命令

P 打印匹配行 print

= 打印匹配行行号

a\ 定位行号后附加新文本信息 append

i\ 定位行号后插入   insert

d 删除定位行   delete

c\ 用新文本替换定位文本   change

s 使用替换模式替换相应模式 
 
r 从另一个文件中读文本  read

w 写文本到一个文件   write

q 第一个模式匹配完成后退出或立即退出

{} 定位执行命令组

n 从另一个文件中读文本下一行，并附加在下一行

g 将模式2黏贴到/pattern n/

y 传送字符

###实例：

*	显示文本

`$sed -n '1,4p' file`

显示1-4行

`$sed -n '4,/Str/p'`

显示第4行到匹配到Str的一行，/str/代表匹配到的一行


* 插入修改文本

`$sed '/str/a\ "inserted line"' file`

在满足条件的行后，插入内容

`sed '/str/i\ "appended line"' file`

在满足条件的行前插入内容

`$sed '3 c\ "changed line"' file`

满足条件的行，整行替换掉

* 删除文本

`$sed '1,3d' file`

删除1-3行

`$sed '/str/d' file`

删除匹配行

`$sed -n '/Begin/,/End/p' file | more`

删除两个匹配行之间的数据

* 替换文本

格式：`[address[,address]] s/pattern-find/replacement-pattern/[g,p,w,n]`

n    1到512之间的一个数字，表示对本模式中指定模式第n次出现的情况进行替换。

g    对模式空间所有出现的情况进行全局更改【缺省只替换首次出现的模式 】

p    打印模式空间的内容

w    file

`$sed 's/str/tostr/' file` 

替换每一行首次出现的str为tostr

`$sed 's/str/tostr/g' file`

替换所有的行内，出现的所有str为tostr

`$sed 's/str/tostr/w output' file`

替换后重定向到output

转换字符

`sed 'y/cp/wd/' test.txt`

c转换成w，p转换成d

* Shell向sed传值

`echo $input | sed 's/bb/'$str'/'`

`echo $input | sed "s/bb/$str/"`

##awk


###主要功能

awk是一种用于处理文本的工具，主要用于格式化报文，或从一个大文本中抽取数据。

###执行过程

awk每次读入一行，执行' '中的内容，按模式匹配来采取动作

###格式

`awk 'pattern+{action}' file`

pattern用于筛选查询匹配行，决定了动作何时触发，可以使用条件语句，正则表达式

action用于对筛选后的内容进行处理

BEGIN可以设置计数和打印头（可选）

END打印输出文本总数和结尾状态标识(可选)

###常用参数

-F 指定读取一行数据的分隔符，默认为空格
-f 指定处理程序的脚本文件，这个文件必须符合awk语法

###调用方式：

`awk –f awk-script-file input-files`

###常用内置参数

`$0,$1,....$n`   `$0`代表当前行的内容，`$i`代表当前行被分割后的第i个字段的内容

ARGC 命令行参数个数

ARGV 命令行参数排列

ENVIRON 支持队列中系统环境变量的使用

FILENAME 实际操作的文件名

FNR 浏览文件记录数，<=NR

FS **设置输入域分隔符，等价于命令行-F选项** ，可在BEGIN中进行设置,然后执行的时候均以设置的符号为分隔符

NF **浏览记录 域的个数**，在记录被读取时设置【number of fields】一共有多少个域

NR **已读取记录数**【number of rows】

RS 控制记录分隔符，缺省：新行\n，Row Separator记录分隔符，可以根据具体数据需求，设置读取一条记录的区间

OFS **输出域的分隔符**，缺省空格，输出结果 print $1,$2默认加的是空格，可以在BEGIN中设置，改为其他分隔符

ORS 输出记录的分隔符，缺省：新行\n，整体记录的


###实例

* 打印

打印所有行

`awk '{print $0}' file`

打印包含头尾

`awk 'BEGIN{print "Name Age"}{print $1,$2}END{print "END_OF_REPORT"}'`

* 使用判断语句

`<  <=  >  >=  ==  !=`  

~匹配正则   !~不匹配正则

`|| && !` 或且非

`awk '{if($2!~/Rudy/) print $0}' content`

`awk '{if($1=="001" && $2~/^Ru/) print $0}' content`


* 使用内置的变量 

`awk 'BEGIN{OFS='\t'}{print NF,NR,$0}END{print FILENAME}' content > output`

设置输出的分隔符为'\t'，输出一些内置变量的信息

* AWK变量中的字符串和数字的转换

字符串->整数

`$ awk 'BEGIN{a="100";b="10test10";print (a+b+0);}' `

`110`

只需要将变量通过”+”连接运算。自动强制将字符串转为整型。非数字变成0，发现第一个非数字字符，后面自动忽略。

整数->字符串

`awk 'BEGIN{a=100;b=100;c=(a""b);print c}'`      

`100100`

只需要将变量与””符号连接起来运算即可。

* 使用内置的字符串函数

`gsub(r,s)` 在整个$0中**用s代替r**

`gsub(r,s,t)` 在整个t中用s替代r

`index(s,t)` 返回s中字符串t的第一位置

`length(s)` **返回s长度**

`match(s,r)` 测试s是否包含匹配r的字符串

`split(s,a,fs)` 在fs上将s分成序列a.fs为分隔符

`sprint(fmt,exp)` 返回经fmt格式化后的exp

`sub(r,s)` 用$0中最左边最长的子串代替s

`substr(s,p)` 返回字符串s中从p开始的后缀部分

`substr(s,p,n)` 返回字符串s中从p开始长度为n的后缀部分

替换字符串

`awk 'BEGIN{FS='\t'}{gsub(/Rudy/,"RUDY");{print $0}}' content`

* 使用printf进行格式化输出

`%c` ASCII字符

`%d` 整数

`%e` 浮点数，可科学计数法

`%f` 浮点数，小数形式

`%g` 由awk决定使用哪种浮点数转换e或f

`%o` 八进制

`%s` 字符串

`%x` 十六进制

格式化输出一个字符串

`awk -F'\t' '{printf("%s\t%s\n",$2,$1)}' content`

* 向awk中传递参数

`awk '{if($3<=AGE){print $0}}' AGE=20 content` 

* 写一个awk脚本

		#!bin/awk -f
		BEGIN{
		        FS="['\t']"
		        printf("%s\t%s\t%s\n","NUMBER","NAME","AGE")
		}
		{
		        printf("%s\t%s\t%s\n",$1,$2,$3)
		}
		END{
		        print "END OF FILE"
		}


