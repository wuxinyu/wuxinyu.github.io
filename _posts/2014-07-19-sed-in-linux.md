---
title: sed 整理
layout: post
category : automatic
tagline: sed
tags : [linux,shell]
---

<h3 id="sed">sed</h3>
<p>sed是一个非交互性文本流编辑器。sed一次处理一行内容。处理时，把当前处理的行存储在临时缓冲区中，称为“模式空间”，接着用sed命令处理缓冲区中的内容，处理完成后，把缓冲区的内容送往屏幕。</p>

<h3 id="section">定位：</h3>
<p>可用行号或模式来定位行。有如下方式：</p>

<pre><code>x                                             x为行号，如1
x，y                                          表范围，如2,5表示2到5行
/pattern/                                     包含此模式的行
/pattern/pattern/                             包含2个模式的行
x，/pattern/                                  通过行号和模式定位
</code></pre>

<h3 id="sed-1">Sed编辑命令：</h3>

<pre><code>a\                                            在当前行后面加入一行文本。
c\                                            用新的文本改变本行的文本。
d                                             从模板块位置删除行。
D                                             删除模板块的第一行。
i\                                            在当前行上面插入文本。
h                                             拷贝模板块的内容到内存中的缓冲区。
H                                             追加模板块的内容到内存中的缓冲区
g                                             获得内存缓冲区的内容，并替代当前模板块中的文本。
G                                             获得内存缓冲区的内容，并追加到当前模板块文本的后面。
l                                             列表不能打印字符的清单。
n                                             读取下一个输入行，用下一个命令处理新的行而不是用第一个命令。
N                                             追加下一个输入行到模板块后面并在二者间嵌入一个新行，改变当前行号码。
p                                             打印模板块的行。
P                                             打印模板块的第一行。
q                                             退出Sed。
r                                             从file中读行。
!                                             表示后面的命令对所有没有被选定的行发生作用。
s                                             替换。
=                                             打印当前行号码。
#                                             把注释扩展到下一个换行符以前。
</code></pre>

<h3 id="section-1">选项：</h3>
<pre><code>-e                                            允许多点编辑。
-h                                            打印帮助，并显示bug列表的地址。
-n                                            取消默认输出。
-f                                            引导sed脚本文件名。
-V                                            打印版本和版权信息。
</code></pre>

<h3 id="section-2">实例：</h3>

<p>d删除</p>

<pre><code>$ sed '2,$d' example                          删除example文件的第二行到末尾所有行。
$ sed '$d' example                            删除example文件的最后一行。
$ sed '/test/'d example-                      删除example文件所有包含test的行。
</code></pre>

<p>s替换</p>

<pre><code>$ sed 's/test/mytest/g' example               在整行范围内把test替换为mytest。如果没有g标记，则只有每行第一个匹配的test被替换
                                              成mytest。
$ sed -n 's/^test/mytest/p' example	          和p标志一起使用表示只打印那些发生替换的行。
$ sed 's/^192.168.0.1/&amp;localhost/' example    &amp;符号表示替换换字符串中被找到的部份。所有以192.168.0.1开头的行都会被替换成它自已
                                              加localhost，变成192.168.0.1localhost。
$ sed -n 's/\(love\)able/\1rs/p' example      love  被标记为1，所有loveable会被替换成lovers，而且替换的行会被打印出来。
$ sed 's#10#100#g' example                    不论什么字符，紧跟着s命令的都被认为是新的分隔符，所以，“#”在这里是分隔符，代替了
                                              默认的“/”分隔符。表示把所有10替换成100。
</code></pre>

<p>，逗号 选定行的范围</p>

<pre><code>$ sed -n '/test/,/check/p' example            所有在模板test和check所确定的范围内的行都被打印。
$ sed '/test/,/check/s/$/sed test/' example   对于模板test和west之间的行，每行的末尾用字符串sed test替换。
</code></pre>

<p>n 下一个</p>

<pre><code>$ sed '/test/{ n; s/aa/bb/; }' example        如果test被匹配，则移动到匹配行的下一行，替换这一行的aa，变为bb，并打印该行，然后继续。
</code></pre>

<p>q 退出</p>

<pre><code>$ sed '10q' example                           打印完第10行后，退出sed。、
</code></pre>

<p>e 多点编辑</p>

<pre><code>$ sed -e '1,5d' -e 's/test/check/' example    e选项允许在同一行里执行多条命令。
$ sed --expression='s/test/check/' --expression='/love/d' example		一个比-e更好的命令是--expression。它能给sed表达式赋值。
</code></pre>

<p>a 追加命令</p>

<pre><code>$ sed '/^test/a\\---&gt;this is a example' example&lt;	'this is a example'被追加到以test开头的行后面，sed要求命令a后面有一个反斜杠。
</code></pre>