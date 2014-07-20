---
title: grep 整理
layout: post
category : automatic
tagline: awk sed grep
tags : [linux,shell]
---

<h3 id="grep">grep</h3>

<h4 id="grep--grep---">grep使用正则表达式搜索文本，并把匹配的行打印出来。 一般格式 grep [选项] 基本正则式 [文件]</h4>

<h4 id="section">选项：</h4>

<p>-c 不输出内容只统计匹配行数 </p>

<pre><code>grep -c "123" filename      相当于 grep "123" filename | wc -l -i 不区分大小写

grep -i "ok" filename       匹配包含ok,Ok,oK,OK的所有行
</code></pre>

<p>-h 查询多文件时不显示文件名</p>

<pre><code>grep -h "123" *.txt         不加-h时会在每一行前显示该行所在文件名
</code></pre>

<p>-l 查询多文件是只显示包含匹配字符的文件名</p>

<p>-n 查询匹配行及行号</p>

<p>-v 显示不包含匹配文本的所有行</p>

<p>-? 同时显示匹配行上下的？行</p>

<pre><code>grep -2 "123" filename       同时显示匹配行的上下2行。
</code></pre>

<h4 id="section-1">正则：</h4>

<p>^ 锚定行的开始如:</p>

<pre><code>'^grep'                       匹配所有以grep开头的行。
</code></pre>

<p>$ 锚定行的结束如:</p>

<pre><code>'grep$'                       匹配所有以grep结尾的行。
</code></pre>

<p>. 匹配一个非换行符的字符如:</p>

<pre><code>'gr.p'                        匹配gr后接一个任意字符，然后是p。
</code></pre>

<ul>
  <li>
    <p>匹配零个或多个先前字符如:</p>

    <p>‘*grep’                       匹配所有一个或多个空格后紧跟grep的行。</p>
  </li>
</ul>

<p>.*一起用代表任意字符。</p>

<p>[] 匹配一个指定范围内的字符如:</p>

<pre><code>'[Gg]rep'                     匹配Grep和grep。
</code></pre>

<p>[^] 匹配一个不在指定范围内的字符如:</p>

<pre><code>'[^A-FH-Z]rep'                 匹配不包含A-R和T-Z的一个字母开头，紧跟rep的行。
</code></pre>

<p>(..)标记匹配字符如:</p>

<pre><code>'\(love\)'                     love被标记为1。
</code></pre>

<p>&lt; 锚定单词的开始 如:</p>

<pre><code>'\&lt;grep'                       匹配包含以grep开头的单词的行。
</code></pre>

<p>&gt; 锚定单词的结束如:</p>

<pre><code>'grep\&gt;'                       匹配包含以grep结尾的单词的行。
</code></pre>

<p>\b 单词锁定符 如: </p>

<pre><code>'\bgrep\b'                     只匹配grep。
</code></pre>

<h4 id="section-2">特殊事项：</h4>

<p>1.引号的使用</p>

<p>首先说明引号的作用，在shell中使用grep一般要打引号，例如：”grep”这样做，一是防止被误解为shell命令，二是可以查找多个单词的字符串。如，”aaa  bbb”。如果没引号，将会把bbb误认为文件。</p>

<p>一般在grep中输入字符串参数是打双引号，如：</p>

<pre><code>$mystr="aaa";grep "$mystr" file，
</code></pre>

<p>这样$mystr会先被替换成aaa，执行操作是grep “aaa” file。而在单引号中，$mystr不被识别，因此，单引号一般用在正则表达式的匹配上，这样可防止于grep中使用的模式与shell命令中的特殊方式混淆。</p>

<p>2.egrep和 grep -E的元字符扩展
egrep是扩展的grep，支持基本正则和扩展正则，等同于grep -E。扩展集如下：</p>

<p>+ 匹配一个或多个先前的字符。如：</p>

<pre><code>'[a-z]+able'                 匹配一个或多个小写字母后跟able的串。
</code></pre>

<table>
  <tbody>
    <tr>
      <td>a</td>
      <td>b</td>
      <td>c 匹配a或b或c。如：</td>
    </tr>
  </tbody>
</table>

<pre><code>grep|sed                     匹配grep或sed
</code></pre>

<p>( )   分组符号如：</p>

<pre><code>love(able|rs)                匹配loveable或lovers。
</code></pre>

<p>egrep还支持将一个文件作为保存的字符串，然后将之传给egrep作为参数，需使用-f开关。</p>

<p>3.POSIX字符类</p>

<p>为了在不同国家的字符编码中保持一至，POSIX(The Portable Operating System Interface)增加了特殊的字符类。</p>

<pre><code>[:alnum:]文字数字字符
[:alpha:]文字字符
[:digit:]数字字符
[:graph:]非空字符（非空格、控制字符）
[:lower:]小写字符
[:cntrl:]控制字符
[:print::]非空字符（包括空格）
[:punct:]标点符号
[:space:]所有空白字符
[:upper:]大写字符
[:xdigit:]十六进制数字（0-9，a-f，A-F）
</code></pre>

<p>grep支持这种模式，例如：grep ‘[[:upper:]][[:lower:]]’ file 。使用时要打双中括号。</p>
