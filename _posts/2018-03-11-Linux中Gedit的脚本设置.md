---
layout: post
title: Linux下Gedit的脚本设置
---

因为后面可能非常非常经常要用Gedit写题了，所以来总结一下如何用Gedit的脚本功能来搭建一个舒服的调题环境为妙。

###基础配置

首先找到Gedit的偏好设定并且将运行外部脚本的功能开起来，然后打开工具找到“管理外部插件”并且新建一个内容如下的脚本：

{%highlight shell%}

#!/bin/sh
d=$GEDIT_CURRENT_DOCUMENT_DIR
s=$GEDIT_CURRENT_DOCUMENT_NAME
c=`echo $s|cut -d. -f1`
g++ $s -o $c &&\
echo "Success!" &&\
echo "Now let's go..." &&\
gnome-terminal --working-directory=$d -x bash -c \
"time $d/$c; echo; echo 'Press ENTER to continue...';read"

{%endhighlight%}

（这里末尾有\\的是指接下一行，至于最后一段的用&&连接是为了编译失败就不运行。在实际使用的时候可以不换行并且删掉\\字符，这里使用是为了能够保证代码长度不超过框框的宽度）

那这里面的句子是什么意思呢？

其中第一行代表这是一个脚本。

第二行定义了一个值为当前目录的变量。

第三行定义了一个值为当前的cpp文件名的变量。

第四行是把当前的文件名以点分割并且取前半段。注意这里使用的是重音符 '`'，不要打成单引号啦~
<text>
第五行是先编译运行当前的cpp文件（g++ $s -o $c）；
然后在下方的提示框里面输出成功的消息（两个echo）；
接着打开一个新终端（gnome-terminal）：
先把这个终端的运行目录设定成当前的目录（--working-directory=$d）；
然后是我也不知道是干什么的-x bash；
然后是执行一些命令（-c），命令的内容用双引号包起来。命令的内容首先是计时地运行你的程序（time $d/$c），注意这里是绝对路径，接着输出一个换行（echo），然后输出一行提示信息（echo 'Press ENTER to continue'，注意是单引号），最后一个read实现输入等到回车后再关闭窗口。
</text>
接着把它保存为快捷键Ctrl+F11，并保存当前文件即可。

接下来按下Ctrl+F11，如果编译失败就会在下方显示错误信息，成功就会新弹出一个超级良心der终端，是给这个程序专用的，再也不用担心做题的时候程序提前退出然后因为是用粘贴的结果就在终端里面打了一堆乱七八糟的命令x

### 调试模式

新开一个脚本，把刚刚那一段东西里面的g++后面的编译命令里增加一个-DDEBUG，这个脚本快捷键保存为Ctrl+F8，然后在自己的代码开头加入如下一段内容：

{%highlight cpp%}
#ifdef DEBUG
#define debug(a...) fprintf(stderr,a);
#else
#define debug(a...)
#endif
{%endhighlight%}

然后接下来在代码里面要调试的地方直接调用debug函数，例如

{%highlight cpp%}
debug("x=%d\n",x);
{%endhighlight%}

接下来就可以实现如果是Ctrl+F8就会往stderr里面输出debug语句，否则按Ctrl+F11不会输出，评测的时候也不会输出的功能惹。

注意：在define里面的fprintf后面最好接一个分号，防止自己一不小心就打出

{%highlight cpp%}
for(i=1;i<=n;i++,debug("x=%d\n",x))werken(i);
{%endhighlight%}

这样的代码，然后就成功在正式测试的时候CE啦！

当然，自己定义一个新的空函数也是可以的，可以降低CE风险。

但是还是要注意的是如果你为了调试在 $O(n)$ 的算法里面调用了 $O(n^2)$ 次的空函数不删仍然是会T得很惨的吧……

所以也不是万能的。（这句话的深层含义：出锅了不要打我啊喂！）

于是就可以成功地在debug和release模式之间自由切换了x

所以大概就长成这样子了如果读不懂可能就是我的语文能力问题了这我也救不了自己了。