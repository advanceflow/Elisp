# 格式说明
由于github对markdown的预览支持更好，所以仅保留markdown格式。
# 文档浏览

![img](./Demo.gif)


<a id="org1ec6c34"></a>

# 术语对照

针对不合适的术语表述，欢迎提交修正。

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<tbody>
<tr>
<td class="org-left">Face</td>
<td class="org-left">面</td>
</tr>


<tr>
<td class="org-left">Eval</td>
<td class="org-left">评估</td>
</tr>


<tr>
<td class="org-left">Kill Ring</td>
<td class="org-left">环</td>
</tr>


<tr>
<td class="org-left">Numbers</td>
<td class="org-left">数字</td>
</tr>


<tr>
<td class="org-left">Lists</td>
<td class="org-left">列表</td>
</tr>


<tr>
<td class="org-left">Sequences, Arrays, and Vectors</td>
<td class="org-left">序列、数组和向量</td>
</tr>


<tr>
<td class="org-left">Records</td>
<td class="org-left">记录</td>
</tr>


<tr>
<td class="org-left">Hash Tables</td>
<td class="org-left">哈希表</td>
</tr>


<tr>
<td class="org-left">Symbols</td>
<td class="org-left">符号</td>
</tr>


<tr>
<td class="org-left">Control Structures</td>
<td class="org-left">控制结构</td>
</tr>


<tr>
<td class="org-left">Variables</td>
<td class="org-left">变量</td>
</tr>


<tr>
<td class="org-left">Functions</td>
<td class="org-left">函数</td>
</tr>


<tr>
<td class="org-left">Macros</td>
<td class="org-left">宏</td>
</tr>


<tr>
<td class="org-left">Loading</td>
<td class="org-left">加载</td>
</tr>


<tr>
<td class="org-left">Byte Compilation</td>
<td class="org-left">字节编译</td>
</tr>


<tr>
<td class="org-left">Minibuffers</td>
<td class="org-left">小缓冲区</td>
</tr>


<tr>
<td class="org-left">Command Loop</td>
<td class="org-left">命令循环</td>
</tr>


<tr>
<td class="org-left">Keymaps</td>
<td class="org-left">键映射</td>
</tr>


<tr>
<td class="org-left">Major and Minor Modes</td>
<td class="org-left">主要和次要模式</td>
</tr>


<tr>
<td class="org-left">Documentation</td>
<td class="org-left">文档</td>
</tr>


<tr>
<td class="org-left">Files</td>
<td class="org-left">文件</td>
</tr>


<tr>
<td class="org-left">Buffers</td>
<td class="org-left">缓冲区</td>
</tr>


<tr>
<td class="org-left">Windows</td>
<td class="org-left">窗口</td>
</tr>


<tr>
<td class="org-left">Frames</td>
<td class="org-left">帧</td>
</tr>


<tr>
<td class="org-left">Positions</td>
<td class="org-left">位置</td>
</tr>


<tr>
<td class="org-left">Markers</td>
<td class="org-left">标记</td>
</tr>


<tr>
<td class="org-left">Text</td>
<td class="org-left">文本</td>
</tr>


<tr>
<td class="org-left">Non-ASCII Characters</td>
<td class="org-left">非ASCII字符</td>
</tr>


<tr>
<td class="org-left">Syntax Tables</td>
<td class="org-left">语法表</td>
</tr>


<tr>
<td class="org-left">Threads</td>
<td class="org-left">线程</td>
</tr>
</tbody>
</table>


<a id="org14b200d"></a>

# 翻译进度

1.  第一阶段S1： 机器翻译全部内容。
2.  第二阶段S2： 把翻译成中文的代码块和函数名给替换回来。
3.  第三阶段S3： 局部微调。
    
    S1，S2已完成，目前正处于S3，欢迎大家提交修正内容。


<a id="orga500321"></a>

## 1 简介

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<tbody>
<tr>
<td class="org-left">Content</td>
<td class="org-left">S1</td>
<td class="org-left">S2</td>
<td class="org-left">S3/Author</td>
</tr>


<tr>
<td class="org-left">1.1 注意事项</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">1.2 Lisp 历史</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">1.3 约定</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">1.3.1 一些条款</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">1.3.2 nil和 t</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">1.3.3 评估符号</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">1.3.4 打印符号</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">1.3.5 错误信息</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">1.3.6 缓冲区文本符号</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">1.3.7 说明格式</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">1.3.7.1 示例函数描述</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">1.3.7.2 示例变量描述</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">1.4 版本信息</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">1.5 致谢</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>
</tbody>
</table>


<a id="orga52417b"></a>

## 2 Lisp 数据类型

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<tbody>
<tr>
<td class="org-left">Content</td>
<td class="org-left">S1</td>
<td class="org-left">S2</td>
<td class="org-left">S3/Author</td>
</tr>


<tr>
<td class="org-left">2.1 打印表示和读取语法</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">2.2 特殊读语法</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">2.3 评估</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">2.4 编程类型</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">2.4.1 整数类型</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">2.4.2 浮点型</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">2.4.3 字符类型</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">2.4.3.1 基本字符语法</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">2.4.3.2 通用转义语法</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">2.4.3.3 控制字符语法</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">2.4.3.4 元字符语法</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">2.4.3.5 其他字符修饰符位</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">2.4.4 符号类型</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">2.4.5 序列类型</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">2.4.6 缺点单元格和列表类型</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">2.4.6.1 以框图形式绘制列表</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">2.4.6.2 点对符号</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">2.4.6.3 关联列表类型</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">2.4.7 数组类型</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">2.4.8 字符串类型</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">2.4.8.1 字符串的语法</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">2.4.8.2 字符串中的非 ASCII 字符</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">2.4.8.3 字符串中的非打印字符</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">2.4.8.4 字符串中的文本属性</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">2.4.9 向量类型</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">2.4.10 字符表类型</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">2.4.11 Bool-Vector 类型</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">2.4.12 哈希表类型</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">2.4.13 功能类型</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">2.4.14 宏类型</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">2.4.15 原始函数类型</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">2.4.16 字节码函数类型</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">2.4.17 记录类型</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">2.4.18 类型描述符</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">2.4.19 自动加载类型</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">2.4.20 终结器类型</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">2.5 编辑类型</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">2.5.1 缓冲区类型</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">2.5.2 标记类型</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">2.5.3 窗口类型</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">2.5.4 帧类型</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">2.5.5 终端类型</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">2.5.6 窗口配置类型</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">2.5.7 帧配置类型</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">2.5.8 流程类型</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">2.5.9 线程类型</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">2.5.10 互斥体类型</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">2.5.11 条件变量类型</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">2.5.12 流类型</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">2.5.13 键映射类型</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">2.5.14 覆盖类型</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">2.5.15 字体类型</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">2.6 循环对象的读语法</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">2.7 类型谓词</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">2.8 等式谓词</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">2.9 可变性</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>
</tbody>
</table>


<a id="orgfa011ac"></a>

## 3 数字

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<tbody>
<tr>
<td class="org-left">Content</td>
<td class="org-left">S1</td>
<td class="org-left">S2</td>
<td class="org-left">S3/Author</td>
</tr>


<tr>
<td class="org-left">3.1 整数基础</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">3.2 浮点基础</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">3.3 数字的类型谓词</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">3.4 数字比较</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">3.5 数值转换</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">3.6 算术运算</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">3.7 舍入操作</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">3.8 整数的按位运算</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">3.9 标准数学函数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">3.10 随机数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>
</tbody>
</table>


<a id="orga54069f"></a>

## 4 字符串和字符

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<tbody>
<tr>
<td class="org-left">Content</td>
<td class="org-left">S1</td>
<td class="org-left">S2</td>
<td class="org-left">S3/Author</td>
</tr>


<tr>
<td class="org-left">4.1 字符串和字符基础</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">4.2 字符串谓词</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">4.3 创建字符串</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">4.4 修改字符串</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">4.5 字符与字符串的比较</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">4.6 字符和字符串的转换</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">4.7 格式化字符串</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">4.8 自定义格式字符串</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">4.9 Lisp 中的大小写转换</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">4.10 案例表</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>
</tbody>
</table>


<a id="org44d86ae"></a>

## 5 列表

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<tbody>
<tr>
<td class="org-left">Content</td>
<td class="org-left">S1</td>
<td class="org-left">S2</td>
<td class="org-left">S3/Author</td>
</tr>


<tr>
<td class="org-left">5.1 列表和缺点单元格</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">5.2 列表上的谓词</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">5.3 访问列表元素</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">5.4 构建 Cons 单元格和列表</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">5.5 修改列表变量</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">5.6 修改现有列表结构</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">5.6.1 改变列表元素 setcar</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">5.6.2 更改列表的 CDR</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">5.6.3 重新排列列表的函数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">5.7 使用列表作为集合</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">5.8 关联列表</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">5.9 属性列表</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">5.9.1 属性列表和关联列表</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">5.9.2 符号外的属性列表</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>
</tbody>
</table>


<a id="org473d2d3"></a>

## 6 序列、数组和向量

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<tbody>
<tr>
<td class="org-left">Content</td>
<td class="org-left">S1</td>
<td class="org-left">S2</td>
<td class="org-left">S3/Author</td>
</tr>


<tr>
<td class="org-left">6.1 序列</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">6.2 数组</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">6.3 操作数组的函数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">6.4 向量</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">6.5 向量函数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">6.6 字符表</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">6.7 布尔向量</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">6.8 管理固定大小的对象环</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>
</tbody>
</table>


<a id="orgbf3aaf3"></a>

## 7 记录

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<tbody>
<tr>
<td class="org-left">Content</td>
<td class="org-left">S1</td>
<td class="org-left">S2</td>
<td class="org-left">S3/Author</td>
</tr>


<tr>
<td class="org-left">7.1 记录功能</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">7.2 向后兼容性</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>
</tbody>
</table>


<a id="orgde8476b"></a>

## 8 哈希表

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<tbody>
<tr>
<td class="org-left">Content</td>
<td class="org-left">S1</td>
<td class="org-left">S2</td>
<td class="org-left">S3/Author</td>
</tr>


<tr>
<td class="org-left">8.1 创建哈希表</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left"><a href="https://github.com/Kinneyzhang">Kinneyzhang</a></td>
</tr>


<tr>
<td class="org-left">8.2 哈希表访问</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left"><a href="https://github.com/Kinneyzhang">Kinneyzhang</a></td>
</tr>


<tr>
<td class="org-left">8.3 定义哈希比较</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left"><a href="https://github.com/Kinneyzhang">Kinneyzhang</a></td>
</tr>


<tr>
<td class="org-left">8.4 其他哈希表函数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left"><a href="https://github.com/Kinneyzhang">Kinneyzhang</a></td>
</tr>
</tbody>
</table>


<a id="orgc8605b9"></a>

## 9 符号

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<tbody>
<tr>
<td class="org-left">Content</td>
<td class="org-left">S1</td>
<td class="org-left">S2</td>
<td class="org-left">S3/Author</td>
</tr>


<tr>
<td class="org-left">9.1 符号组件</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">9.2 定义符号</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">9.3 创建和嵌入符号</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">9.4 符号属性</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">9.4.1 访问符号属性</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">9.4.2 标准符号属性</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">9.5 速记</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">9.5.1 例外</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>
</tbody>
</table>


<a id="orgc2381aa"></a>

## 10 评估

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<tbody>
<tr>
<td class="org-left">Content</td>
<td class="org-left">S1</td>
<td class="org-left">S2</td>
<td class="org-left">S3/Author</td>
</tr>


<tr>
<td class="org-left">10.1 评估简介</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">10.2 表格种类</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">10.2.1 自我评估表</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">10.2.2 符号形式</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">10.2.3 列表形式的分类</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">10.2.4 符号函数间接</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">10.2.5 函数形式的评估</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">10.2.6 Lisp 宏求值</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">10.2.7 特殊表格</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">10.2.8 自动加载</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">10.3 报价</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">10.4 反引号</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">10.5 评估</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">10.6 延迟和惰性评估</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>
</tbody>
</table>


<a id="orge76a2ce"></a>

## 11 控制结构

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<tbody>
<tr>
<td class="org-left">Content</td>
<td class="org-left">S1</td>
<td class="org-left">S2</td>
<td class="org-left">S3/Author</td>
</tr>


<tr>
<td class="org-left">11.1 测序</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">11.2 条件</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">11.3 组合条件的构造</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">11.4 模式匹配条件</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">11.4.1 该 pcase宏</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">11.4.2 扩展 pcase</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">11.4.3 反引号样式模式</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">11.4.4 解构 pcase模式</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">11.5 迭代</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">11.6 生成器</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">11.7 非本地出口</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">11.7.1 显式非本地出口： catch和 throw</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">11.7.2 示例 catch和 throw</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">11.7.3 错误</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">11.7.3.1 如何发出错误信号</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">11.7.3.2 Emacs 如何处理错误</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">11.7.3.3 编写代码来处理错误</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">11.7.3.4 错误符号和条件名称</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">11.7.4 清理非本地出口</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>
</tbody>
</table>


<a id="org763aaec"></a>

## 12 变量

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<tbody>
<tr>
<td class="org-left">Content</td>
<td class="org-left">S1</td>
<td class="org-left">S2</td>
<td class="org-left">S3/Author</td>
</tr>


<tr>
<td class="org-left">12.1 全局变量</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">12.2 永不改变的变量</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">12.3 局部变量</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">12.4 当变量为空时</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">12.5 定义全局变量</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">12.6 稳健定义变量的技巧</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">12.7 访问变量值</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">12.8 设置变量值</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">12.9 当变量改变时运行函数。</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">12.9.1 限制</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">12.10 变量绑定的作用域规则</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">12.10.1 动态绑定</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">12.10.2 正确使用动态绑定</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">12.10.3 词法绑定</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">12.10.4 使用词法绑定</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">12.10.5 转换为词法绑定</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">12.11 缓冲区局部变量</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">12.11.1 缓冲区局部变量简介</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">12.11.2 创建和删除缓冲区本地绑定</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">12.11.3 缓冲区局部变量的默认值</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">12.12 文件局部变量</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">12.13 目录局部变量</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">12.14 连接局部变量</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">12.15 变量别名</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">12.16 有限制值的变量</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">12.17 广义变量</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">12.17.1 setf宏</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">12.17.2 定义新的 setf形式</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>
</tbody>
</table>


<a id="orge89d785"></a>

## 13 函数

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<tbody>
<tr>
<td class="org-left">Content</td>
<td class="org-left">S1</td>
<td class="org-left">S2</td>
<td class="org-left">S3/Author</td>
</tr>


<tr>
<td class="org-left">13.1 什么是函数？</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">13.2 Lambda 表达式</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">13.2.1 Lambda 表达式的组成部分</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">13.2.2 一个简单的 Lambda 表达式示例</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">13.2.3 参数列表的特点</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">13.2.4 函数的文档字符串</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">13.3 命名函数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">13.4 定义函数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">13.5 调用函数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">13.6 映射函数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">13.7 匿名函数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">13.8 泛型函数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">13.9 访问函数单元格内容</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">13.10 闭包</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">13.11 建议 Emacs Lisp 函数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">13.11.1 操纵建议的原语</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">13.11.2 建议命名函数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">13.11.3 编写建议的方法</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">13.11.4 使用旧的 defadvice 适配代码</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">13.12 声明过时的函数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">13.13 内联函数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">13.14 declare形式</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">13.15 告诉编译器定义了一个函数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">13.16 判断一个函数是否可以安全调用</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">13.17 其他与函数相关的话题</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>
</tbody>
</table>


<a id="orga0c1c90"></a>

## 14 宏

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<tbody>
<tr>
<td class="org-left">Content</td>
<td class="org-left">S1</td>
<td class="org-left">S2</td>
<td class="org-left">S3/Author</td>
</tr>


<tr>
<td class="org-left">14.1 一个简单的宏例子</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">14.2 宏调用的扩展</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">14.3 宏和字节编译</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">14.4 定义宏</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">14.5 使用宏的常见问题</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">14.5.1 错误时间</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">14.5.2 反复评估宏参数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">14.5.3 宏展开中的局部变量</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">14.5.4 评估扩展中的宏观参数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">14.5.5 宏扩展了多少次？</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">14.6 缩进宏</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>
</tbody>
</table>


<a id="orgc2f6761"></a>

## 15 自定义设置

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<tbody>
<tr>
<td class="org-left">Content</td>
<td class="org-left">S1</td>
<td class="org-left">S2</td>
<td class="org-left">S3/Author</td>
</tr>


<tr>
<td class="org-left">15.1 常用项关键字</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">15.2 定义自定义组</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">15.3 定义自定义变量</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">15.4 自定义类型</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">15.4.1 简单类型</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">15.4.2 复合类型</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">15.4.3 拼接成列表</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">15.4.4 键入关键字</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">15.4.5 定义新类型</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">15.5 应用自定义</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">15.6 自定义主题</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>
</tbody>
</table>


<a id="orgaa4b306"></a>

## 16 加载

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<tbody>
<tr>
<td class="org-left">Content</td>
<td class="org-left">S1</td>
<td class="org-left">S2</td>
<td class="org-left">S3/Author</td>
</tr>


<tr>
<td class="org-left">16.1 程序如何加载</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">16.2 加载后缀</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">16.3 图书馆搜索</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">16.4 加载非 ASCII 字符</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">16.5 自动加载</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">16.5.1 按前缀自动加载</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">16.5.2 何时使用自动加载</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">16.6 重复加载</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">16.7 特点</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">16.8 哪个文件定义了某个符号</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">16.9 卸载</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">16.10 装载挂钩</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">16.11 Emacs 动态模块</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>
</tbody>
</table>


<a id="org38c7f9f"></a>

## 17 字节编译

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<tbody>
<tr>
<td class="org-left">Content</td>
<td class="org-left">S1</td>
<td class="org-left">S2</td>
<td class="org-left">S3/Author</td>
</tr>


<tr>
<td class="org-left">17.1 字节编译代码的性能</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">17.2 字节编译函数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">17.3 文档字符串和编译</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">17.4 单个函数的动态加载</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">17.5 编译期间的评估</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">17.6 编译器错误</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">17.7 字节码函数对象</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">17.8 反汇编字节码</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>
</tbody>
</table>


<a id="org76bcdb2"></a>

## 18 Lisp编译成Native代码

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<tbody>
<tr>
<td class="org-left">Content</td>
<td class="org-left">S1</td>
<td class="org-left">S2</td>
<td class="org-left">S3/Author</td>
</tr>


<tr>
<td class="org-left">18.1 本机编译函数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">18.2 本机编译变量</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>
</tbody>
</table>


<a id="orgf806660"></a>

## 19 调试 Lisp 程序

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<tbody>
<tr>
<td class="org-left">Content</td>
<td class="org-left">S1</td>
<td class="org-left">S2</td>
<td class="org-left">S3/Author</td>
</tr>


<tr>
<td class="org-left">19.1 Lisp 调试器</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">19.1.1 出错时进入调试器</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">19.1.2 调试无限循环</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">19.1.3 在函数调用中进入调试器</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">19.1.4 修改变量时进入调试器</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">19.1.5 显式进入调试器</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">19.1.6 使用调试器</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">19.1.7 回溯</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">19.1.8 调试器命令</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">19.1.9 调用调试器</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">19.1.10 调试器的内部结构</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">19.2 调试</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">19.2.1 使用 Edebug</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">19.2.2 为 Edebug 检测</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">19.2.3 Edebug 执行模式</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">19.2.4 跳跃</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">19.2.5 其他 Edebug 命令</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">19.2.6 断点</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">19.2.6.1 调试断点</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">19.2.6.2 全局中断条件</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">19.2.6.3 源断点</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">19.2.7 捕获错误</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">19.2.8 调试视图</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">19.2.9 评估</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">19.2.10 评估列表缓冲区</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">19.2.11 在 Edebug 中打印</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">19.2.12 跟踪缓冲区</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">19.2.13 覆盖测试</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">19.2.14 外部环境</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">19.2.14.1 检查是否停止</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">19.2.14.2 调试显示更新</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">19.2.14.3 Edebug 递归编辑</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">19.2.15 调试和宏</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">19.2.15.1 检测宏调用</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">19.2.15.2 规格表</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">19.2.15.3 规范中的回溯</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">19.2.15.4 规范示例</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">19.2.16 调试选项</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">19.3 调试无效的 Lisp 语法</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">19.3.1 多余的开括号</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">19.3.2 多余的右括号</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">19.4 测试覆盖率</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">19.5 剖析</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>
</tbody>
</table>


<a id="org94b93ea"></a>

## 20 读入和打印 Lisp 对象

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<tbody>
<tr>
<td class="org-left">Content</td>
<td class="org-left">S1</td>
<td class="org-left">S2</td>
<td class="org-left">S3/Author</td>
</tr>


<tr>
<td class="org-left">20.1 读入与打印简介</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">20.2 输入流</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">20.3 输入函数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">20.4 输出流</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">20.5 输出函数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">20.6 影响输出的变量</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>
</tbody>
</table>


<a id="orgf1a8148"></a>

## 21 小缓冲区

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<tbody>
<tr>
<td class="org-left">Content</td>
<td class="org-left">S1</td>
<td class="org-left">S2</td>
<td class="org-left">S3/Author</td>
</tr>


<tr>
<td class="org-left">21.1 Minibuffers 简介</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">21.2 用 Minibuffer 读取文本字符串</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">21.3 用 Minibuffer 读取 Lisp 对象</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">21.4 小缓冲区历史</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">21.5 初始输入</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">21.6 完成</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">21.6.1 基本完成函数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">21.6.2 完成和小缓冲区</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">21.6.3 完成完成的 Minibuffer 命令</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">21.6.4 高级完成函数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">21.6.5 读取文件名</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">21.6.6 完成变量</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">21.6.7 编程完成</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">21.6.8 在普通缓冲区中完成</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">21.7 是或否查询</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">21.8 提出多项选择题</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">21.9 读取密码</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">21.10 小缓冲区命令</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">21.11 小缓冲窗口</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">21.12 小缓冲区内容</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">21.13 递归小缓冲区</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">21.14 抑制交互</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">21.15 小缓冲区杂记</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>
</tbody>
</table>


<a id="org57568ff"></a>

## 22 命令循环

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<tbody>
<tr>
<td class="org-left">Content</td>
<td class="org-left">S1</td>
<td class="org-left">S2</td>
<td class="org-left">S3/Author</td>
</tr>


<tr>
<td class="org-left">22.1 命令循环概述</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">22.2 定义命令</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">22.2.1 使用 interactive</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">22.2.2 代码字符 interactive</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">22.2.3 使用示例 interactive</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">22.2.4 指定命令模式</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">22.2.5 在命令选项中进行选择</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">22.3 交互调用</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">22.4 区分交互调用</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">22.5 来自命令循环的信息</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">22.6 指令后点调整</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">22.7 输入事件</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">22.7.1 键盘事件</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">22.7.2 功能键</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">22.7.3 鼠标事件</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">22.7.4 点击事件</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">22.7.5 拖动事件</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">22.7.6 按钮按下事件</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">22.7.7 重复事件</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">22.7.8 运动事件</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">22.7.9 焦点事件</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">22.7.10 其他系统事件</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">22.7.11 事件示例</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">22.7.12 分类事件</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">22.7.13 访问鼠标事件</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">22.7.14 访问滚动条事件</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">22.7.15 将键盘事件放入字符串中</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">22.8 读数输入</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">22.8.1 按键序列输入</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">22.8.2 读取一个事件</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">22.8.3 修改和翻译输入事件</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">22.8.4 调用输入法</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">22.8.5 引用字符输入</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">22.8.6 杂项事件输入功能</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">22.9 特别活动</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">22.10 等待经过时间或输入</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">22.11 退出</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">22.12 前缀命令参数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">22.13 递归编辑</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">22.14 禁用命令</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">22.15 命令历史</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">22.16 键盘宏</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>
</tbody>
</table>


<a id="orgd6a5d2d"></a>

## 23 键映射

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<tbody>
<tr>
<td class="org-left">Content</td>
<td class="org-left">S1</td>
<td class="org-left">S2</td>
<td class="org-left">S3/Author</td>
</tr>


<tr>
<td class="org-left">23.1 按键序列</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">23.2 键映射基础</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">23.3 键映射格式</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">23.4 创建键映射</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">23.5 继承和键映射</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">23.6 前缀键</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">23.7 活动键映射</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">23.8 搜索活动键映射</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">23.9 控制激活的键映射</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">23.10 密钥查找</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">23.11 键查找函数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">23.12 更改键绑定</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">23.13 重映射命令</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">23.14 用于翻译事件序列的键映射</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">23.14.1 与普通键映射的交互</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">23.15 绑定键的命令</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">23.16 扫描键映射</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">23.17 菜单键映射</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">23.17.1 定义菜单</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">23.17.1.1 简单菜单项</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">23.17.1.2 扩展菜单项</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">23.17.1.3 菜单分隔符</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">23.17.1.4 别名菜单项</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">23.17.2 菜单和鼠标</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">23.17.3 菜单和键盘</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">23.17.4 菜单示例</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">23.17.5 菜单栏</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">23.17.6 工具栏</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">23.17.7 修改菜单</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">23.17.8 简易菜单</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>
</tbody>
</table>


<a id="org876fea8"></a>

## 24 主和次模式

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<tbody>
<tr>
<td class="org-left">Content</td>
<td class="org-left">S1</td>
<td class="org-left">S2</td>
<td class="org-left">S3/Author</td>
</tr>


<tr>
<td class="org-left">24.1 钩子</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">24.1.1 运行钩子</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">24.1.2 设置挂钩</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">24.2 主模式</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">24.2.1 主模式约定</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">24.2.2 Emacs 如何选择主模式</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">24.2.3 获取有关主模式的帮助</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">24.2.4 定义派生模式</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">24.2.5 基本主模式</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">24.2.6 模式挂钩</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">24.2.7 列表模式</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">24.2.8 通用模式</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">24.2.9 主模式示例</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">24.3 次模式</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">24.3.1 编写次模式的约定</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">24.3.2 键映射和次模式</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">24.3.3 定义次模式</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">24.4 模式线格式</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">24.4.1 模式线基础</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">24.4.2 模式行的数据结构</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">24.4.3 顶层模式线控制</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">24.4.4 模式行中使用的变量</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">24.4.5 %- 模式线中的构造</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">24.4.6 模式行中的属性</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">24.4.7 窗口标题行</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">24.4.8 模拟模式行格式</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">24.5 名称</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">24.6 字体锁定模式</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">24.6.1 字体锁定基础</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">24.6.2 基于搜索的字体</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">24.6.3 自定义基于搜索的字体</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">24.6.4 其他字体锁定变量</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">24.6.5 字体锁定级别</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">24.6.6 预计算字体</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">24.6.7 字体锁定面</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">24.6.8 语法字体锁定</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">24.6.9 多行字体锁定结构</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">24.6.9.1 字体锁定多行</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">24.6.9.2 缓冲区更改后要字体化的区域</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">24.7 代码自动缩进</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">24.7.1 简单的缩进引擎</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">24.7.1.1 SMIE 设置和功能</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">24.7.1.2 运算符优先级文法</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">24.7.1.3 定义语言的语法</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">24.7.1.4 定义令牌</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">24.7.1.5 使用弱解析器</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">24.7.1.6 指定缩进规则</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">24.7.1.7 缩进规则的辅助函数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">24.7.1.8 缩进规则示例</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">24.7.1.9 自定义缩进</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">24.8 桌面保存模式</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>
</tbody>
</table>


<a id="orgfaf118b"></a>

## 25 文档

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<tbody>
<tr>
<td class="org-left">Content</td>
<td class="org-left">S1</td>
<td class="org-left">S2</td>
<td class="org-left">S3/Author</td>
</tr>


<tr>
<td class="org-left">25.1 文档基础</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">25.2 访问文档字符串</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">25.3 替换文档中的键绑定</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">25.4 文本引用样式</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">25.5 描述帮助信息的字符</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">25.6 帮助功能</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">25.7 文档组</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>
</tbody>
</table>


<a id="org587bac9"></a>

## 26 文件

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<tbody>
<tr>
<td class="org-left">Content</td>
<td class="org-left">S1</td>
<td class="org-left">S2</td>
<td class="org-left">S3/Author</td>
</tr>


<tr>
<td class="org-left">26.1 访问文件</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">26.1.1 文件访问函数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">26.1.2 访问子程序</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">26.2 保存缓冲区</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">26.3 从文件中读取</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">26.4 写入文件</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">26.5 文件锁</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">26.6 文件信息</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">26.6.1 测试可访问性</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">26.6.2 区分文件种类</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">26.6.3 真名</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">26.6.4 文件属性</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">26.6.5 扩展文件属性</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">26.6.6 在标准位置定位文件</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">26.7 更改文件名和属性</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">26.8 文件和二级存储</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">26.9 文件名</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">26.9.1 文件名组件</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">26.9.2 绝对和相对文件名</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">26.9.3 目录名称</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">26.9.4 扩展文件名的函数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">26.9.5 生成唯一文件名</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">26.9.6 文件名补全</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">26.9.7 标准文件名</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">26.10 目录的内容</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">26.11 创建、复制和删除目录</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">26.12 使某些文件名“神奇”</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">26.13 文件格式转换</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">26.13.1 概述</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">26.13.2 往返规范</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">26.13.3 零碎规格</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>
</tbody>
</table>


<a id="org67cb572"></a>

## 27 备份和自动保存

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<tbody>
<tr>
<td class="org-left">Content</td>
<td class="org-left">S1</td>
<td class="org-left">S2</td>
<td class="org-left">S3/Author</td>
</tr>


<tr>
<td class="org-left">27.1 备份文件</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">27.1.1 制作备份文件</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">27.1.2 重命名备份还是复制备份？</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">27.1.3 制作和删除编号备份文件</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">27.1.4 命名备份文件</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">27.2 自动保存</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">27.3 还原</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>
</tbody>
</table>


<a id="org6f4d248"></a>

## 28 缓冲区

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<tbody>
<tr>
<td class="org-left">Content</td>
<td class="org-left">S1</td>
<td class="org-left">S2</td>
<td class="org-left">S3/Author</td>
</tr>


<tr>
<td class="org-left">28.1 缓冲区基础</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">28.2 当前缓冲区</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">28.3 缓冲区名称</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">28.4 缓冲区文件名</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">28.5 缓冲区修改</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">28.6 缓冲区修改时间</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">28.7 只读缓冲区</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">28.8 缓冲区列表</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">28.9 创建缓冲区</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">28.10 终止缓冲区</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">28.11 间接缓冲区</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">28.12 在两个缓冲区之间交换文本</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">28.13 缓冲间隙</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>
</tbody>
</table>


<a id="org4acad99"></a>

## 29 窗口

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<tbody>
<tr>
<td class="org-left">Content</td>
<td class="org-left">S1</td>
<td class="org-left">S2</td>
<td class="org-left">S3/Author</td>
</tr>


<tr>
<td class="org-left">29.1 Emacs Windows的基本概念</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">29.2 窗户和框架</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">29.3 选择窗口</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">29.4 窗口大小</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">29.5 调整窗口大小</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">29.6 保留窗口大小</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">29.7 分割窗口</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">29.8 删除窗口</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">29.9 重新组合窗口</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">29.10 Windows的循环排序</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">29.11 缓冲区和窗口</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">29.12 切换到窗口中的缓冲区</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">29.13 在合适的窗口中显示缓冲区</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">29.13.1 选择显示缓冲区的窗口</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">29.13.2 缓冲区显示的动作函数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">29.13.3 缓冲区显示的动作列表</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">29.13.4 显示缓冲区的附加选项</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">29.13.5 动作函数的优先级</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">29.13.6 缓冲区显示之禅</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">29.14 窗口历史</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">29.15 专用窗口</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">29.16 退出窗口</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">29.17 侧窗</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">29.17.1 在侧窗中显示缓冲区</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">29.17.2 侧窗选项和功能</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">29.17.3 带有侧窗的框架布局</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">29.18 原子窗口</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">29.19 窗口和点</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">29.20 窗口开始和结束位置</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">29.21 文本滚动</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">29.22 垂直小数滚动</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">29.23 水平滚动</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">29.24 坐标和窗口</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">29.25 鼠标窗口自动选择</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">29.26 窗口配置</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">29.27 窗口参数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">29.28 窗口滚动和改变的钩子</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>
</tbody>
</table>


<a id="org20f037d"></a>

## 30 帧

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<tbody>
<tr>
<td class="org-left">Content</td>
<td class="org-left">S1</td>
<td class="org-left">S2</td>
<td class="org-left">S3/Author</td>
</tr>


<tr>
<td class="org-left">30.1 创建帧</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">30.2 多终端</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">30.3 帧几何</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">30.3.1 帧布局</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">30.3.2 帧字体</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">30.3.3 帧位置</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">30.3.4 帧大小</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">30.3.5 隐含的帧大小调整</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">30.4 帧参数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">30.4.1 访问帧参数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">30.4.2 初始帧参数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">30.4.3 窗框参数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">30.4.3.1 基本参数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">30.4.3.2 位置参数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">30.4.3.3 尺寸参数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">30.4.3.4 布局参数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">30.4.3.5 缓冲区参数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">30.4.3.6 帧交互参数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">30.4.3.7 鼠标拖动参数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">30.4.3.8 窗口管理参数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">30.4.3.9 光标参数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">30.4.3.10 字体和颜色参数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">30.4.4 几何</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">30.5 终端参数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">30.6 帧标题</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">30.7 删除帧</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">3 查找所有帧</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">30.9 小缓冲区和帧</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">30.10 输入焦点</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">30.11 框架的可见性</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">30.12 提升、降低和重新堆叠框架</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">30.13 帧配置</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">30.14 子框架</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">30.15 鼠标跟踪</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">30.16 鼠标位置</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">30.17 弹出菜单</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">30.18 对话框</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">30.19 指针形状</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">30.20 窗口系统选择</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">30.21 拖放</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">30.22 颜色名称</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">30.23 文本终端颜色</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">30.24 X 资源</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">30.25 显示功能测试</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>
</tbody>
</table>


<a id="org0883b74"></a>

## 31 位置

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<tbody>
<tr>
<td class="org-left">Content</td>
<td class="org-left">S1</td>
<td class="org-left">S2</td>
<td class="org-left">S3/Author</td>
</tr>


<tr>
<td class="org-left">31.1 点</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">31.2 运动</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">31.2.1 角色动作</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">31.2.2 词动</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">31.2.3 移动到缓冲区末端</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">31.2.4 文本行的运动</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">31.2.5 屏幕线运动</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">31.2.6 移动平衡表达式</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">31.2.7 跳过字符</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">31.3 远足</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">31.4 收窄</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>
</tbody>
</table>


<a id="org13cd52d"></a>

## 32 标记

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<tbody>
<tr>
<td class="org-left">Content</td>
<td class="org-left">S1</td>
<td class="org-left">S2</td>
<td class="org-left">S3/Author</td>
</tr>


<tr>
<td class="org-left">32.1 标记概述</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">32.2 关于标记的谓词</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">32.3 创建标记的函数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">32.4 来自标记的信息</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">32.5 标记插入类型</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">32.6 移动标记位置</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">32.7 标记</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">32.8 区域</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>
</tbody>
</table>


<a id="org6a99efd"></a>

## 33 文本

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<tbody>
<tr>
<td class="org-left">Content</td>
<td class="org-left">S1</td>
<td class="org-left">S2</td>
<td class="org-left">S3/Author</td>
</tr>


<tr>
<td class="org-left">33.1 检查文本近点</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.2 检查缓冲区内容</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.3 比较文本</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.4 插入文本</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.5 用户级插入命令</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.6 删除文本</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.7 用户级删除命令</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.8 环</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.8.1 环概念</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.8.2 杀死函数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.8.3 扬克</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.8.4 Yanking 函数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.8.5 低级环</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.8.6 环的内部</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.9 撤消</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.10 维护撤销列表</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.11 填充</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.12 填充边距</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.13 自适应填充模式</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.14 自动填充</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.15 文本排序</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.16 计数列</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.17 缩进</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.17.1 缩进原语</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.17.2 主模式控制的缩进</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.17.3 缩进整个区域</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.17.4 相对于前几行的缩进</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.17.5 可调制表位</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.17.6 基于缩进的运动命令</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.18 案例变更</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.19 文本属性</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.19.1 检查文本属性</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.19.2 更改文本属性</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.19.3 文本属性搜索功能</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.19.4 具有特殊含义的属性</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.19.5 格式化文本属性</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.19.6 文本属性的粘性</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.19.7 文本属性的惰性计算</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.19.8 定义可点击文本</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.19.9 定义和使用字段</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.19.10 为什么文本属性不是区间</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.20 替换字符代码</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.21 寄存器</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.22 文本转置</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.23 替换缓冲区文本</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.24 处理压缩数据</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.25 Base 64 编码</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.26 校验和/哈希</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.27 GnuTLS 密码学</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.27.1 GnuTLS 加密输入的格式</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.27.2 GnuTLS 加密函数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.28 解析 HTML 和 XML</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.28.1 文档对象模型</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.29 解析和生成 JSON 值</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.30 JSONRPC 通信</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.30.1 概述</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.30.2 基于进程的 JSONRPC 连接</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.30.3 JSONRPC JSON对象格式</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.30.4 延迟的 JSONRPC 请求</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.31 原子变更组</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">33.32 更改挂钩</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>
</tbody>
</table>


<a id="orga6eefab"></a>

## 34 非 ASCII 字符

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<tbody>
<tr>
<td class="org-left">Content</td>
<td class="org-left">S1</td>
<td class="org-left">S2</td>
<td class="org-left">S3/Author</td>
</tr>


<tr>
<td class="org-left">34.1 文本表示</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">34.2 禁用多字节字符</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">34.3 转换文本表示</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">34.4 选择表示</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">34.5 字符代码</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">34.6 字符属性</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">34.7 字符集</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">34.8 扫描字符集</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">34.9 字符翻译</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">34.10 编码系统</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">34.10.1 编码系统的基本概念</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">34.10.2 编码和 I/O</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">34.10.3 Lisp 中的编码系统</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">34.10.4 用户选择的编码系统</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">34.10.5 默认编码系统</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">34.10.6 为一个操作指定编码系统</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">34.10.7 显式编码和解码</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">34.1 终端 I/O 编码</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">34.11 输入法</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">34.12 语言环境</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>
</tbody>
</table>


<a id="org8c5c566"></a>

## 35 搜索和匹配

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<tbody>
<tr>
<td class="org-left">Content</td>
<td class="org-left">S1</td>
<td class="org-left">S2</td>
<td class="org-left">S3/Author</td>
</tr>


<tr>
<td class="org-left">35.1 搜索字符串</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">35.2 搜索和案例</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">35.3 正则表达式</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">35.3.1 正则表达式的语法</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">35.3.1.1 正则表达式中的特殊字符</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">35.3.1.2 字符类</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">35.3.1.3 正则表达式中的反斜杠结构</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">35.3.2 复杂正则表达式示例</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">35.3.3 该 rx结构化正则表达式表示法</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">35.3.3.1 构造 rx正则表达式</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">35.3.3.2 函数和宏使用 rx正则表达式</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">35.3.3.3 定义新的 rx形式</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">35.3.4 正则表达式函数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">35.3.5 正则表达式的问题</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">35.4 正则表达式搜索</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">35.5 POSIX正则表达式搜索</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">35.6 匹配数据</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">35.6.1 替换匹配的文本</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">35.6.2 简单匹配数据访问</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">35.6.3 访问整个比赛数据</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">35.6.4 保存和恢复比赛数据</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">35.7 搜索和替换</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">35.8 编辑中使用的标准正则表达式</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>
</tbody>
</table>


<a id="orga1287e9"></a>

## 36 语法表

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<tbody>
<tr>
<td class="org-left">Content</td>
<td class="org-left">S1</td>
<td class="org-left">S2</td>
<td class="org-left">S3/Author</td>
</tr>


<tr>
<td class="org-left">36.1 语法表概念</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">36.2 语法描述符</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">36.2.1 语法类表</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">36.2.2 语法标志</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">36.3 语法表函数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">36.4 语法属性</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">36.5 运动和句法</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">36.6 解析表达式</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">36.6.1 基于解析的运动命令</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">36.6.2 查找位置的解析状态</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">36.6.3 解析器状态</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">36.6.4 低级解析</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">36.6.5 控制解析的参数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">36.7 语法表内部</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">36.8 类别</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>
</tbody>
</table>


<a id="org199a481"></a>

## 37 缩写和缩写扩展

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<tbody>
<tr>
<td class="org-left">Content</td>
<td class="org-left">S1</td>
<td class="org-left">S2</td>
<td class="org-left">S3/Author</td>
</tr>


<tr>
<td class="org-left">37.1 缩略表</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">37.2 定义缩写</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">37.3 在文件中保存缩写</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">37.4 查找和扩展缩略语</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">37.5 标准缩写表</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">37.6 缩写属性</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">37.7 缩写表属性</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>
</tbody>
</table>


<a id="org61b07d6"></a>

## 38 线程

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<tbody>
<tr>
<td class="org-left">Content</td>
<td class="org-left">S1</td>
<td class="org-left">S2</td>
<td class="org-left">S3/Author</td>
</tr>


<tr>
<td class="org-left">38.1 基本线程函数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">38.2 互斥体</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">38.3 条件变量</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">38.4 线程列表</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>
</tbody>
</table>


<a id="orgdbef65a"></a>

## 39 进程

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<tbody>
<tr>
<td class="org-left">Content</td>
<td class="org-left">S1</td>
<td class="org-left">S2</td>
<td class="org-left">S3/Author</td>
</tr>


<tr>
<td class="org-left">39.1 创建子进程的函数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">39.2 Shell 参数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">39.3 创建同步进程</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">39.4 创建一个异步进程</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">39.5 删除进程</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">39.6 过程信息</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">39.7 向进程发送输入</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">39.8 向进程发送信号</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">39.9 接收进程的输出</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">39.9.1 进程缓冲区</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">39.9.2 过程过滤器函数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">39.9.3 解码过程输出</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">39.9.4 接受进程的输出</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">39.9.5 进程和线程</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">39.10 Sentinels：检测进程状态变化</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">39.11 退出前查询</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">39.12 访问其他进程</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">39.13 事务队列</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">39.14 网络连接</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">39.15 网络服务器</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">39.16 数据报</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">39.17 低级网络访问</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">39.17.1 make-network-process</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">39.17.2 网络选项</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">39.17.3 测试网络功能的可用性</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">39.18 其他网络设施</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">39.19 与串口通信</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">39.20 打包和解包字节数组</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">39.20.1 描述数据布局</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">39.20.2 解包和打包字节的函数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">39.20.3 高级数据布局规范</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>
</tbody>
</table>


<a id="orgcab19b3"></a>

## 40 Emacs 显示

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<tbody>
<tr>
<td class="org-left">Content</td>
<td class="org-left">S1</td>
<td class="org-left">S2</td>
<td class="org-left">S3</td>
</tr>


<tr>
<td class="org-left">40.1 刷新屏幕</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.2 强制重新显示</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.3 截断</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.4 回声区</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.4.1 在回显区显示消息</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.4.2 上报操作进度</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.4.3 记录消息 <b>留言</b></td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.4.4 回声区自定义</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.5 报告警告</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.5.1 警告基础</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.5.2 警告变量</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.5.3 警告选项</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.5.4 延迟警告</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.6 不可见文本</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.7 选择性显示</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">4 临时展示</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.9 叠加</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.9.1 管理覆盖</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.9.2 覆盖属性</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.9.3 搜索覆盖</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.10 显示文本的大小</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.11 行高</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.12 面</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.12.1 面属性</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.12.2 定义面</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.12.3 面属性函数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.12.4 显示面</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.12.5 面重映射</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.12.6 处理面的函数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.12.7 自动面分配</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.12.8 基本面</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.12.9 字体选择</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.12.10 查找字体</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.12.11 字体集</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.12.12 低级字体表示</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.13 条纹</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.13.1 条纹尺寸和位置</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.13.2 边缘指标</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.13.3 边缘光标</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.13.4 边缘位图</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.13.5 自定义边缘位图</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.13.6 叠加箭头</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.14 滚动条</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.15 窗口分隔线</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.16 display属性</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.16.1 替换文本的显示规范</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.16.2 指定空间</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.16.3 以像素为单位指定间隔</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">Rosario S.E. 3vau</td>
</tr>


<tr>
<td class="org-left">40.16.4 其它显示属性值</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">Rosario S.E. 3vau</td>
</tr>


<tr>
<td class="org-left">40.16.5 在边缘显示</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.17 图像</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.17.1 图像格式</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.17.2 图像描述符</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.17.3 XBM 图像</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.17.4 XPM 图像</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.17.5 ImageMagick 图像</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.17.6 SVG 图像</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.17.7 其他图像类型</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.17.8 定义图像</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.17.9 显示图像</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.17.10 多帧图像</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.17.11 图像缓存</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.18 嵌入式原生小部件</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.19 按钮</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.19.1 按钮属性</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.19.2 按钮类型</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.19.3 制作按钮</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.19.4 操作按钮</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.19.5 按钮缓冲区命令</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.20 抽象显示</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.20.1 抽象显示函数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.20.2 抽象显示示例</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.21 闪烁的括号</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.22 字符显示</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.22.1 通常的显示约定</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.22.2 显示表格</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.22.3 活动显示表</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.22.4 字形</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.22.5 无字形字符显示</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.23 哔哔声</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.24 窗户系统</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.25 工具提示</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">40.26 双向显示</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>
</tbody>
</table>


<a id="orgafe7776"></a>

## 41 操作系统接口

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<tbody>
<tr>
<td class="org-left">Content</td>
<td class="org-left">S1</td>
<td class="org-left">S2</td>
<td class="org-left">S3/Author</td>
</tr>


<tr>
<td class="org-left">41.1 启动 Emacs</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">Advanceflow</td>
</tr>


<tr>
<td class="org-left">41.1.1 小结：启动时的动作顺序</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">Advanceflow</td>
</tr>


<tr>
<td class="org-left">41.1.2 初始化文件</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">Advanceflow</td>
</tr>


<tr>
<td class="org-left">41.1.3 终端特定初始化</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">Advanceflow</td>
</tr>


<tr>
<td class="org-left">41.1.4 命令行参数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">Advanceflow</td>
</tr>


<tr>
<td class="org-left">41.2 退出 Emacs</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">Advanceflow</td>
</tr>


<tr>
<td class="org-left">41.2.1 杀死 Emacs</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">Advanceflow</td>
</tr>


<tr>
<td class="org-left">41.2.2 挂起 Emacs</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">Advanceflow</td>
</tr>


<tr>
<td class="org-left">41.3 操作系统环境</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">Advanceflow</td>
</tr>


<tr>
<td class="org-left">41.4 用户识别</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">Advanceflow</td>
</tr>


<tr>
<td class="org-left">41.5 时间</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">Advanceflow</td>
</tr>


<tr>
<td class="org-left">41.6 时区规则</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">Advanceflow</td>
</tr>


<tr>
<td class="org-left">41.7 时间转换</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">Advanceflow</td>
</tr>


<tr>
<td class="org-left">41.8 解析和格式化时间</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">Advanceflow</td>
</tr>


<tr>
<td class="org-left">41.9 处理器运行时间</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">Advanceflow</td>
</tr>


<tr>
<td class="org-left">41.10 时间计算</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">Advanceflow</td>
</tr>


<tr>
<td class="org-left">41.11 延迟执行的定时器</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">Advanceflow</td>
</tr>


<tr>
<td class="org-left">41.12 空闲定时器</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">Advanceflow</td>
</tr>


<tr>
<td class="org-left">41.13 终端输入</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">Advanceflow</td>
</tr>


<tr>
<td class="org-left">41.13.1 输入模式</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">Advanceflow</td>
</tr>


<tr>
<td class="org-left">41.13.2 录音输入</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">Advanceflow</td>
</tr>


<tr>
<td class="org-left">41.14 终端输出</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">Advanceflow</td>
</tr>


<tr>
<td class="org-left">41.15 声音输出</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">Advanceflow</td>
</tr>


<tr>
<td class="org-left">41.16 X11 Keysyms 上的操作</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">Advanceflow</td>
</tr>


<tr>
<td class="org-left">41.17 批处理模式</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">Advanceflow</td>
</tr>


<tr>
<td class="org-left">41.18 会话管理</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">Advanceflow</td>
</tr>


<tr>
<td class="org-left">41.19 桌面通知</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">Advanceflow</td>
</tr>


<tr>
<td class="org-left">41.20 文件更改通知</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">Advanceflow</td>
</tr>


<tr>
<td class="org-left">41.21 动态加载的库</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">Advanceflow</td>
</tr>


<tr>
<td class="org-left">41.22 安全考虑</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">Advanceflow</td>
</tr>
</tbody>
</table>


<a id="orgb15e823"></a>

## 42 准备分发的 Lisp 代码

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<tbody>
<tr>
<td class="org-left">Content</td>
<td class="org-left">S1</td>
<td class="org-left">S2</td>
<td class="org-left">S3/Author</td>
</tr>


<tr>
<td class="org-left">42.1 包装基础</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">42.2 简单包</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">42.3 多文件包</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">42.4 创建和维护包档案</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">42.5 与存档 Web 服务器的接口</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>
</tbody>
</table>


<a id="org0c7f0aa"></a>

## 附录

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<tbody>
<tr>
<td class="org-left">Content</td>
<td class="org-left">S1</td>
<td class="org-left">S2</td>
<td class="org-left">S3/Author</td>
</tr>


<tr>
<td class="org-left">附录 A Emacs 27 反新闻</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">附录 B GNU 自由文档许可证</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">附录 C GNU 通用公共许可证</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">附录 D 提示和约定</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">D.1 Emacs Lisp 编码约定</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">D.2 键绑定约定</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">D.3 Emacs 编程技巧</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">D.4 快速编译代码的技巧</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">D.5 避免编译器警告的技巧</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">D.6 文档字符串提示</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">D.7 撰写评论的技巧</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">D.8 Emacs 库的常规头文件</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">附录 E GNU Emacs 内部结构</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">E.1 构建 Emacs</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">E.2 纯存储</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">E.3 垃圾收集</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">E.4 堆栈分配的对象</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">E.5 内存使用</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">E.6 C 方言</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">E.7 编写 Emacs 原语</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">E.8 编写动态加载的模块</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">E.8.1 模块初始化代码</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">E.8.2 编写模块函数</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">E.8.3 Lisp 和模块值之间的转换</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">E.8.4 模块的其他便利功能</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">E.8.5 模块中的非本地出口</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">E.9 对象内部</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">E.9.1 缓冲器内部</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">E.9.2 窗口内部</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">E.9.3 过程内部</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">E.10 C 整数类型</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">附录 F 标准错误</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">附录 G 标准键盘映射</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">附录 H 标准钩子</td>
<td class="org-left">DONE</td>
<td class="org-left">DONE</td>
<td class="org-left">&#xa0;</td>
</tr>
</tbody>
</table>
