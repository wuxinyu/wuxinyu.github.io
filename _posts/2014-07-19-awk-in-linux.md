---
title: awk 整理
layout: post
category : automatic
tagline: awk
tags : [linux,shell]
---
<h3 id="awk">awk</h3>

<p>awk 是一种编程语言，用于在linux/unix下对文本和数据进行处理。数据可以来自标准输入、一个或多个文件，或其它命令的输出。它支持用户自定义函数和动态正则表达式等先进功能，是linux/unix下的一个强大编程工具。</p>

<h3 id="section">选项</h3>
<pre><code>-F  指定输入文件折分隔符。
-f  从脚本文件中读取awk命令。
</code></pre>

<h3 id="section-1">模式和操作：</h3>
<p>awk脚本是由模式和操作组成的,pattern {action} 如:</p>

<pre><code>$ awk '/root/' test
$ awk '$3 &lt; 100' test
</code></pre>

<p>模式是用来匹配行的，操作是对匹配结果执行的。两者是可选的，如果没有模式，则action应用到全部记录，如果没有action，则输出匹配全部记录。默认情况下，每一个输入行都是一条记录，但用户可通过RS变量指定不同的分隔符进行分隔。</p>

<h3 id="section-2">记录：</h3>
<p>awk把每一个以换行符结束的行称为一个记录。
记录分隔符：默认的输入和输出的分隔符都是回车，保存在内建变量ORS和RS中。
$0变量：它指的是整条记录。如</p>

<pre><code>$ awk '{print $0}' test                将输出test文件中的所有记录。
</code></pre>

<p>变量NR：一个计数器，每处理完一条记录，NR的值就增加1。如</p>

<pre><code>$ awk '{print NR,$0}' test             将输出test文件中所有记录，并在记录前显示记录号。
</code></pre>

<h3 id="section-3">域：</h3>
<p>记录中每个单词称做“域”，默认情况下以空格或tab分隔。awk可跟踪域的个数，并在内建变量NF中保存该值。如</p>

<pre><code>$ awk '{print $1,$3}' test             将打印test文件中第一和第三个以空格分开的列(域)。
</code></pre>

<p>内建变量FS保存输入域分隔符的值，默认是空格或tab。我们可以通过-F命令行选项修改FS的值。如</p>

<pre><code>$awk -F'[:\t]' '{print $1,$3}' test    表示以空格、冒号和tab作为分隔符。
</code></pre>

<p>输出域的分隔符默认是一个空格，保存在OFS中。如</p>

<pre><code>$ awk -F: '{print $1,$5}' test          $1和$5间的逗号就是OFS的值。
</code></pre>

<p>实例：</p>

<pre><code>$ awk '/^(no|so)/' test                  打印所有以模式no或so开头的行。
$ awk '/^[ns]/{print $1}' test           如果记录以n或s开头，就打印这个记录。
$ awk '$1 ~/[0-9][0-9]$/(print $1}' test 如果第一个域以两个数字结束就打印这个记录。
$ awk '$1 != 10' test                    如果第一个域不等于10就打印该行。
$ awk '/^root/,/^mysql/' test            打印以正则表达式root开头的记录到以正则表达式mysql开头的记录范围内的所有记录。如果找到一个新的正则表达式root开头的记录，则继续打印直到下一个以正则表达式mysql开头的记录为止，或到文件末尾。
</code></pre>
