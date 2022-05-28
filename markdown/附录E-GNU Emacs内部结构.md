
# Table of Contents

1.  [附录 E GNU Emacs内部结构](#org29dae9c)
    1.  [E.1 构建 Emacs](#org5101df2)
    2.  [E.2 纯存储](#org5569757)
    3.  [E.3 垃圾收集](#org04b826b)
    4.  [E.4 堆栈分配的对象](#orgcd9fa4c)
    5.  [E.5 内存使用](#org9263842)
    6.  [E.6 C方言](#org2ed411d)
    7.  [E.7 编写Emacs原语](#orgbc3a03e)
    8.  [E.8 编写动态加载的模块](#orgbab4a07)
        1.  [E.8.1 模块初始化代码](#org090cf94)
        2.  [E.8.2 编写模块函数](#org7697c89)
        3.  [E.8.3 Lisp和模块值之间的转换](#orge7c34d7)
        4.  [E.8.4 模块的其他便利功能](#org81670e5)
        5.  [E.8.5 模块中的非本地出口](#orgb492617)
2.  [E.9 对象内部](#orgd69f55d)
        1.  [E.9.1 缓冲器内部](#orgb1116f9)
        2.  [E.9.2 窗口内部](#org7a88311)
        3.  [E.9.3 过程内部](#org315672b)
    1.  [E.10 C 整数类型](#orge40762f)



<a id="org29dae9c"></a>

# TODO 附录 E GNU Emacs内部结构

本章描述了如何将可运行的 Emacs 可执行文件与其中预加载的 Lisp 库一起转储，如何分配存储空间，以及 C 程序员可能感兴趣的 GNU Emacs 的一些内部方面。


<a id="org5101df2"></a>

## TODO E.1 构建 Emacs

本节介绍构建 Emacs 可执行文件所涉及的步骤。  您不必知道这些材料来构建和安装 Emacs，因为 makefile 会自动完成所有这些事情。  此信息与 Emacs 开发人员相关。

构建 Emacs 需要 GNU Make 版本 3.81 或更高版本。

编译 src 目录中的 C 源文件会生成一个名为 temacs 的可执行文件，也称为裸非纯 Emacs。  它包含 Emacs Lisp 解释器和 I/O 例程，但不包含编辑命令。

命令 temacs -l loadup 将运行 temacs 并引导它加载 loadup.el。  loadup 库加载了额外的 Lisp 库，这些库设置了正常的 Emacs 编辑环境。  在这一步之后，Emacs 可执行文件就不再是裸露的了。

因为加载标准 Lisp 文件需要一些时间，所以 temacs 可执行文件通常不是由用户直接运行的。  相反，构建 Emacs 的最后一个步骤是运行命令“temacs -batch -l loadup &#x2013;temacs=dump-method”。  特殊选项 &#x2013;temacs 告诉 temacs 如何记录所有标准预加载的 Lisp 函数和变量，这样当您随后运行 Emacs 时，它会启动得更快。  &#x2013;temacs 选项需要一个参数 dump-method，它可以是以下之一：

    ‘pdump’ ¶

在转储文件中记录预加载的 Lisp 数据。  此方法生成一个额外的数据文件，Emacs 将在启动时加载该文件。  生成的转储文件通常称为 emacs.pdmp，并安装在 Emacs 执行目录中（请参阅帮助函数）。  这种方法是最受欢迎的方法，因为它不需要 Emacs 使用任何特殊的内存分配技术，这可能会妨碍现代系统用于增强安全性和隐私性的各种内存布局技术。

    ‘pbootstrap’ ¶

与 'pdump' 类似，但在引导 Emacs 时使用，此时没有以前的 Emacs 二进制文件和 \*.elc 字节编译的 Lisp 文件可用。  在这种情况下，生成的转储文件通常命名为 bootstrap-emacs.pdmp。

    ‘dump’ ¶

这种方法会导致 temacs 转储出一个名为 emacs 的可执行程序，该程序已经预加载了所有标准 Lisp 文件。  （'-batch' 参数阻止 temacs 尝试在终端上初始化它的任何数据，从而使转储的 Emacs 中的终端信息表为空。）此方法也称为 unexec，因为它会生成一个程序文件来自正在运行的进程，因此在某种意义上与执行程序以启动进程相反。  尽管这种方法是 Emacs 传统上保存其状态的方式，但现在已弃用。

    ‘bootstrap’

类似于 'dump'，但在使用 unexec 方法引导 Emacs 时使用。

转储的 emacs 可执行文件（也称为纯 Emacs）是已安装的。  如果使用便携式转储程序构建 Emacs，则 emacs 可执行文件实际上是 temacs 的精确副本，并且还安装了相应的 emacs.pdmp 文件。  变量 preloaded-file-list 存储了转储文件或转储的 Emacs 可执行文件中记录的预加载 Lisp 文件的列表。  如果您将 Emacs 移植到新的操作系统，并且无法实现任何类型的转储，那么 Emacs 必须在每次启动时加载 loadup.el。

默认情况下，转储的 emacs 可执行文件会记录构建时间和主机名等详细信息。  使用 configure 的 &#x2013;disable-build-details 选项来抑制这些细节，以便从相同的源构建和安装 Emacs 两次更有可能导致 Emacs 的相同副本。

您可以通过编写一个名为 site-load.el 的库来指定要预加载的其他文件。  您可能需要使用添加的定义来重建 Emacs

    #define SITELOAD_PURESIZE_EXTRA n

增加 n 个字节的纯空间来保存额外的文件；  参见 src/puresize.h。  （尝试增加 20000 的增量，直到它足够大。）但是，随着机器变得更快，预加载附加文件的优势会降低。  在现代机器上，通常不建议这样做。

在 loadup.el 读取 site-load.el 后，它通过调用 Snarf-documentation（请参阅访问文档）在文件 etc/DOC 中找到原始和预加载函数（和变量）的文档字符串。

您可以通过将其他 Lisp 表达式放在名为 site-init.el 的库中来指定在转储之前执行的其他 Lisp 表达式。  此文件在找到文档字符串后执行。

如果您想预加载函数或变量定义，您可以通过三种方式执行此操作，并在随后运行 Emacs 时使它们的文档字符串可访问：

-   安排在生成 etc/DOC 文件时扫描这些文件，并使用 site-load.el 加载它们。
-   使用 site-init.el 加载文件，然后在安装 Emacs 时将文件复制到 Lisp 文件的安装目录。
-   在每个文件中为 byte-compile-dynamic-docstrings 指定一个 nil 值作为局部变量，并使用 site-load.el 或 site-init.el 加载它们。  （此方法的缺点是文档字符串一直占用 Emacs 中的空间。）

不建议在 site-load.el 或 site-init.el 中放置任何会改变用户在普通未修改 Emacs 中期望的任何功能的内容。  如果您觉得必须覆盖站点的正常功能，请使用 default.el 执行此操作，以便用户可以根据需要覆盖您的更改。  请参阅摘要：启动时的操作顺序。  请注意，如果 site-load.el 或 site-init.el 更改了 load-path，则转储后更改将丢失。  请参阅图书馆搜索。  要永久更改加载路径，请使用配置的 &#x2013;enable-locallisppath 选项。

在可以预加载的包中，有时有必要（或有用）延迟某些评估，直到 Emacs 随后启动。  绝大多数此类情况与可定制变量的值有关。  例如，tutorial-directory 是在 startup.el 中定义的变量，它是预加载的。  默认值是根据数据目录设置的。  该变量需要在 Emacs 启动时访问 data-directory 的值，而不是在转储时，因为 Emacs 可执行文件可能在转储后安装在不同的位置。

    Function: custom-initialize-delay symbol value ¶

该函数将符号的初始化延迟到下一次 Emacs 启动。  您通常通过将其指定为可自定义变量的 :initialize 属性来使用此函数。  （参数值未使用，仅用于与 Custom 期望的形式兼容。）

万一您需要比 custom-initialize-delay 提供的更通用的功能，您可以使用 before-init-hook（请参阅摘要：启动时的操作序列）。

    Function: dump-emacs-portable to-file &optional track-referrers ¶

此函数使用 pdump 方法将 Emacs 的当前状态转储到转储文件到文件中。  通常，转储文件称为 emacs-name.dmp，其中 emacs-name 是 Emacs 可执行文件的名称。  可选参数 track-referrers，如果非 nil，会导致可移植转储程序保留附加信息，以帮助追踪 pdump 方法尚不支持的对象类型的出处。

尽管可移植的转储程序代码可以在许多平台上运行，但它生成的转储文件是不可移植的——它们只能由转储它们的 Emacs 可执行文件加载。

如果您想在已转储的 Emacs 中使用此功能，则必须使用“-batch”选项运行 Emacs。

    Function: dump-emacs to-file from-file ¶

此函数使用 unexec 方法将 Emacs 的当前状态转储到可执行文件到文件中。  它从源文件中获取符号（这通常是可执行文件 temacs）。

此函数不能在已转储的 Emacs 中使用。  此函数已弃用，默认情况下 Emacs 构建时不支持 unexec，因此此函数不可用。

    Function: pdumper-stats ¶

如果当前 Emacs 会话从转储文件恢复其状态，则此函数返回有关转储文件的信息以及恢复 Emacs 状态所用的时间。  该值是一个alist ((dumped-with-pdumper .t) (load-time .time) (dump-file-name .file))，其中file是转储文件的名称，time是以秒为单位的时间它需要从转储文件中恢复状态。  如果当前会话不是从转储文件中恢复的，则该值为 nil。


<a id="org5569757"></a>

## TODO E.2 纯存储

Emacs Lisp 对用户创建的 Lisp 对象使用两种存储方式：普通存储和纯存储。  普通存储是保存在 Emacs 会话期间创建的所有新数据的地方（请参阅垃圾收集）。  纯存储用于预加载的标准 Lisp 文件中的某些数据——这些数据在 Emacs 的实际使用过程中永远不会改变。

只有在 temacs 加载标准的预加载 Lisp 库时才会分配纯存储。  在文件 emacs 中，它被标记为只读（在允许这样做的操作系统上），以便内存空间可以由机器上运行的所有 Emacs 作业一次共享。  纯存储不可扩展；  编译 Emacs 时会分配固定数量，如果这对于预加载的库来说还不够，则 temacs 会为不适合的部分分配动态内存。  如果将使用 pdump 方法转储 Emacs（请参阅构建 Emacs），则纯空间溢出并不特别重要（它只是意味着某些预加载的内容无法与其他 Emacs 作业共享）。  但是，如果 Emacs 将使用现已过时的 unexec 方法转储，则生成的映像将起作用，但在这种情况下会禁用垃圾收集（请参阅垃圾收集），从而导致内存泄漏。  除非您尝试预加载其他库或向标准库添加功能，否则这种溢出通常不会发生。  如果 Emacs 使用 unexec 转储，Emacs 将在启动时显示有关溢出的警告。  如果发生这种情况，您应该在文件 src/puresize.h 中增加编译参数 SYSTEM\_PURESIZE\_EXTRA 并重新构建 Emacs。

    Function: purecopy object ¶

这个函数在对象的纯存储中创建一个副本，并返回它。  它通过简单地在纯存储中创建一个具有相同字符但没有文本属性的新字符串来复制字符串。  它递归地复制向量和 cons 单元格的内容。  它不会复制其他对象（例如符号），而只是将它们原封不动地返回。  如果要求复制标记，它会发出错误信号。

这个函数是无操作的，除非 Emacs 正在构建和转储；  它通常只在预加载的 Lisp 文件中调用。

    Variable: pure-bytes-used ¶

这个变量的值是到目前为止分配的纯存储的字节数。  通常，在转储的 Emacs 中，这个数字非常接近可用的纯存储总量——如果不是，我们会预分配更少。

    Variable: purify-flag ¶

这个变量决定了 defun 是否应该在纯存储中复制函数定义。  如果它是非零，那么函数定义被复制到纯存储中。

在最初加载构建 Emacs 的所有基本函数时，此标志为 t（允许这些函数可共享和不可收集）。  将 Emacs 作为可执行文件转储始终会在此变量中写入 nil，无论转储前后它实际具有的值如何。

您不应该在正在运行的 Emacs 中更改此标志。


<a id="org04b826b"></a>

## TODO E.3 垃圾收集

当一个程序创建一个列表或用户定义一个新函数（例如通过加载一个库）时，该数据被放置在正常存储中。  如果正常存储空间不足，那么 Emacs 会要求操作系统分配更多内存。  不同类型的 Lisp 对象，例如符号、cons 单元、小向量、标记等，在内存中被隔离在不同的块中。  （大向量、长字符串、缓冲区和某些其他相当大的编辑类型被分配在单独的块中，每个对象一个；小字符串被打包成 8k 字节的块，小向量被打包成 4k 字节的块） .

除了基本向量之外，许多对象（如标记、叠加层和缓冲区）都像向量一样进行管理。  对应的 C 数据结构包括 union vectorlike\_header 字段，其 size 成员包含 enum pvec\_type 枚举的子类型，以及有关此结构包含多少 Lisp\_Object 字段以及其余数据大小的信息。  计算对象的内存占用需要此信息，并在迭代向量块时由向量分配代码使用。

使用一些存储一段时间，然后通过（例如）终止缓冲区或删除指向对象的最后一个指针来释放它是很常见的。  Emacs 提供了一个垃圾收集器来回收这个废弃的存储。  垃圾收集器本质上是通过查找和标记 Lisp 程序仍可访问的所有 Lisp 对象来操作的。  首先，它假定所有符号、它们的值和相关的函数定义以及当前在堆栈上的任何数据都是可访问的。  任何可以通过其他可访问对象间接访问的对象也是可访问的，但是这种计算是“保守地”完成的，因此它可能会稍微高估有多少对象是可访问的。

标记完成后，所有仍未标记的对象都是垃圾。  无论 Lisp 程序或用户做什么，都无法引用它们，因为不再有办法接触它们。  他们的空间也可以重复使用，因为没有人会想念他们。  垃圾收集器的第二（清扫）阶段安排重用它们。  （但由于标记是“保守地”完成的，因此并非所有未使用的对象都保证被任何一次扫描进行垃圾收集。）

扫描阶段将未使用的 cons 单元放入空闲列表以供将来分配；  同样适用于符号和标记。  它压缩了可访问的字符串，因此它们占用更少的 8k 块；  然后它释放其他 8k 块。  来自向量块的不可达向量被合并以创建最大可能的空闲区域；  如果一个空闲区域跨越一个完整的 4k 块，则该块被释放。  否则，空闲区域被记录在一个空闲列表数组中，其中每个条目对应一个相同大小区域的空闲列表。  大型向量、缓冲区和其他大型对象是单独分配和释放的。

Common Lisp 注意：与其他 Lisp 不同，GNU Emacs Lisp 在空闲列表为空时不会调用垃圾收集器。  相反，它只是请求操作系统分配更多存储空间，然后继续处理直到 gc-cons-threshold 字节被使用。

这意味着您可以确保垃圾收集器不会在 Lisp 程序的某个部分运行，方法是在它之前显式调用垃圾收集器（前提是该部分程序不使用太多空间来强制执行第二个垃圾收藏）。

    Command: garbage-collect ¶

此命令运行垃圾收集，并返回有关正在使用的空间量的信息。  （如果自上次垃圾收集以来使用的 Lisp 数据的 gc-cons-threshold 字节以上，垃圾收集也会自发发生。）

垃圾收集返回一个列表，其中包含有关正在使用的空间量的信息，其中每个条目的形式为“（使用的名称大小）”或“（使用的名称大小免费）”。  在条目中，name 是描述该条目所代表的对象类型的符号，size 是每个对象使用的字节数，used 是在堆中找到的那些对象的数量，可选的 free 是那些不存在但 Emacs 保留以供将来分配的对象。  所以总体结果是：

    ((conses cons-size used-conses free-conses)
     (symbols symbol-size used-symbols free-symbols)
     (strings string-size used-strings free-strings)
     (string-bytes byte-size used-bytes)
     (vectors vector-size used-vectors)
     (vector-slots slot-size used-slots free-slots)
     (floats float-size used-floats free-floats)
     (intervals interval-size used-intervals free-intervals)
     (buffers buffer-size used-buffers)
     (heap unit-size total-size free-size))

这是一个例子：

    (garbage-collect)
          ⇒ ((conses 16 49126 8058) (symbols 48 14607 0)
    		 (strings 32 2942 2607)
    		 (string-bytes 1 78607) (vectors 16 7247)
    		 (vector-slots 8 341609 29474) (floats 8 71 102)
    		 (intervals 56 27 26) (buffers 944 8)
    		 (heap 1024 11715 2678))

下面是解释每个元素的表格。  请注意，最后一个堆条目是可选的，并且仅在底层 malloc 实现提供 mallinfo 功能时才存在。

    cons-size

cons 单元的内部大小，即 sizeof (struct Lisp\_Cons)。

    used-conses

正在使用的 cons 单元数。

    free-conses

已从操作系统获得空间但当前未使用的 cons 单元数。

    symbol-size

符号的内部大小，即 sizeof (struct Lisp\_Symbol)。

    used-symbols

正在使用的符号数。

    free-symbols

已从操作系统获得空间但当前未使用的符号数。

    string-size

字符串头的内部大小，即 sizeof (struct Lisp\_String)。

    used-strings

正在使用的字符串标头数。

    free-strings

已从操作系统获得空间但当前未使用的字符串标头数。

    byte-size

这是为了方便而使用的，等于 sizeof (char)。

    used-bytes

所有字符串数据的总大小（以字节为单位）。

    vector-size

长度为 1 的向量的大小（以字节为单位），包括其标头。

    used-vectors

从向量块分配的向量头的数量。

    slot-size

向量槽的内部大小，总是等于 sizeof (Lisp\_Object)。

    used-slots

所有使用的向量中的槽数。  插槽计数可能包括来自矢量头的部分或全部开销，具体取决于平台。

    free-slots

所有向量块中的空闲槽数。

    float-size

浮点对象的内部大小，即 sizeof (struct Lisp\_Float)。  （不要将其与本机平台浮动或双精度混淆。）

    used-floats

正在使用的浮点数。

    free-floats

已从操作系统获得空间但当前未使用的浮点数。

    interval-size

区间对象的内部大小，即sizeof(struct interval)。

    used-intervals

正在使用的间隔数。

    free-intervals

已从操作系统获得空间但当前未使用的间隔数。

    buffer-size

缓冲区的内部大小，即 sizeof (struct buffer)。  （不要与 buffer-size 函数返回的值混淆。）

    used-buffers

正在使用的缓冲区对象的数量。  这包括对用户不可见的已终止缓冲区，即 all\_buffers 列表中的所有缓冲区。

    unit-size

堆空间测量的单位，总是等于 1024 字节。

    total-size

总堆大小，以单位大小为单位。

    free-size

当前未使用的堆空间，以单位大小为单位。

如果纯空间发生溢出（请参阅 Pure Storage），并且 Emacs 使用（现已过时的）unexec 方法（请参阅构建 Emacs）转储，则垃圾收集返回 nil，因为在这种情况下无法完成真正的垃圾收集。

    User Option: garbage-collection-messages ¶

如果这个变量不为 nil，Emacs 会在垃圾回收的开始和结束时显示一条消息。  默认值为无。

    Variable: post-gc-hook ¶

这是一个在垃圾回收结束时运行的普通钩子。  在钩子函数运行时垃圾收集被禁止，所以要小心编写它们。

    User Option: gc-cons-threshold ¶

此变量的值是在一次垃圾回收之后必须为 Lisp 对象分配的存储字节数，以便触发另一次垃圾回收。  您可以使用垃圾收集返回的结果来获取有关特定对象类型大小的信息；  分配给缓冲区内容的空间不计算在内。

初始阈值为 GC\_DEFAULT\_THRESHOLD，在 alloc.c 中定义。  由于它是以 word\_size 为单位定义的，因此默认 32 位配置的值为 400,000，而 64 位配置的值为 800,000。  如果您指定一个较大的值，垃圾回收的发生频率就会降低。  这减少了垃圾收集所花费的时间，但增加了总内存使用量。  在运行创建大量 Lisp 数据的程序时，您可能希望这样做。

您可以通过指定较小的值（低至 GC\_DEFAULT\_THRESHOLD 的 1/10）来提高收集频率。  小于此最小值的值将仅在后续垃圾收集之前有效，此时垃圾收集会将阈值设置回最小值。

    User Option: gc-cons-percentage ¶

此变量的值指定垃圾回收发生之前的 consing 数量，作为当前堆大小的一部分。  此标准和 gc-cons-threshold 并行应用，垃圾收集仅在满足这两个标准时才会发生。

随着堆大小的增加，执行垃圾回收的时间也会增加。  因此，可能希望按比例减少它们的频率。

通过 gc-cons-threshold 和 gc-cons-percentage 对垃圾收集器的控制只是近似值。  尽管 Emacs 会定期检查阈值耗尽，但出于效率原因，它不会在每次更改堆或 gc-cons-threshold 或 gc-cons-percentage 后立即执行此操作，因此耗尽阈值不会立即触发垃圾收集。  此外，为了提高阈值计算的效率，Emacs 近似于堆大小，它计算堆中当前可访问对象使用的字节数。

垃圾收集返回的值描述了 Lisp 数据使用的内存量，按数据类型细分。  相比之下，函数 memory-limit 提供有关 Emacs 当前使用的内存总量的信息。

    Function: memory-limit ¶

此函数返回 Emacs 当前使用的虚拟内存的总字节数除以 1024 的估计值。您可以使用它来大致了解您的操作如何影响内存使用。

    Variable: memory-full ¶

如果 Emacs 的 Lisp 对象几乎没有内存，则此变量为 t，否则为 nil。

    Function: memory-use-counts ¶

这将返回一个数字列表，该列表计算在此 Emacs 会话中创建的对象的数量。  这些计数器中的每一个都会针对某种对象递增。  有关详细信息，请参阅文档字符串。

    Function: memory-info ¶

此函数返回系统总内存量以及其中有多少是空闲的。  在不受支持的系统上，该值可能为零。

    Variable: gcs-done ¶

这个变量包含到目前为止在这个 Emacs 会话中完成的垃圾回收的总数。

    Variable: gc-elapsed ¶

此变量包含到目前为止在此 Emacs 会话中垃圾收集期间经过的总秒数，作为浮点数。

    Function: memory-report ¶

有时查看 Emacs 在哪里使用内存（在各种变量、缓冲区和缓存中）很有用。  此命令将打开一个新缓冲区（称为“\*内存报告\*”），除了列出“最大”缓冲区和变量之外，该缓冲区还将提供概述。

这里的所有数据都是近似的，因为实际上没有一致的方法来计算变量的大小。  例如，两个变量可能共享数据结构的一部分，这将被计算两次，但是这个命令仍然可以提供一个有用的高级概述，了解 Emacs 的哪些部分正在使用内存。


<a id="orgcd9fa4c"></a>

## TODO E.4 堆栈分配的对象

上述垃圾收集器用于管理从 Lisp 程序可见的数据，以及 Lisp 解释器内部使用的大部分数据。  有时使用解释器的 C 堆栈分配临时内部对象可能很有用。  这有助于提高性能，因为堆栈分配通常比使用堆内存分配和垃圾收集器释放更快。  缺点是在这些对象被释放后使用它们会导致未定义的行为，因此使用应该经过深思熟虑并通过使用 GC\_CHECK\_MARKED\_OBJECTS 功能仔细调试（参见 src/alloc.c）。  特别是，堆栈分配的对象不应该对用户 Lisp 代码可见。

目前，可以通过这种方式分配 cons 单元格和字符串。  这是由 AUTO\_CONS 和 AUTO\_STRING 等 C 宏实现的，它们定义了具有块生命周期的命名 Lisp\_Object。  这些对象不会被垃圾收集器释放；  相反，它们具有自动存储持续时间，即，它们像局部变量一样被分配，并在定义对象的 C 块执行结束时自动释放。

出于性能原因，堆栈分配的字符串仅限于 ASCII 字符，其中许多字符串是不可变的，即，对它们调用 ASET 会产生未定义的行为。


<a id="org9263842"></a>

## TODO E.5 内存使用

这些函数和变量提供有关 Emacs 已完成的内存分配总量的信息，按数据类型细分。  注意这些和垃圾收集返回的值之间的区别；  这些计算当前存在的对象，但这些计算所有分配的数量或大小，包括那些已经被释放的对象。

    Variable: cons-cells-consed ¶

到目前为止，此 Emacs 会话中已分配的 cons 单元的总数。

    Variable: floats-consed ¶

到目前为止，在此 Emacs 会话中已分配的浮点总数。

    Variable: vector-cells-consed ¶

到目前为止，在此 Emacs 会话中已分配的向量单元的总数。  这包括类似矢量的对象，例如标记和覆盖，以及用户不可见的某些对象。

    Variable: symbols-consed ¶

到目前为止，此 Emacs 会话中已分配的符号总数。

    Variable: string-chars-consed ¶

到目前为止在此会话中分配的字符串字符总数。

    Variable: intervals-consed ¶

到目前为止，此 Emacs 会话中已分配的时间间隔总数。

    Variable: strings-consed ¶

到目前为止，此 Emacs 会话中已分配的字符串总数。


<a id="org2ed411d"></a>

## TODO E.6 C方言

Emacs 的 C 部分可移植到 C99 或更高版本：C11 特定的特性，如“<stdalign.h>”和“\_Noreturn”，通常在配置时不检查使用，并且 Emacs 构建过程提供替代实现如有必要。  一些 C11 特性，例如匿名结构和联合，太难以模拟，因此完全避免使用它们。

在未来的某个时候，基本的 C 方言无疑会变成 C11。


<a id="orgbc3a03e"></a>

## TODO E.7 编写Emacs原语

Lisp 原语是用 C 实现的 Lisp 函数。连接 C 函数以便 Lisp 可以调用它的细节由几个 C 宏处理。  真正理解如何编写新的 C 代码的唯一方法是阅读源代码，但我们可以在这里解释一些事情。

一个特殊形式的例子是 or 的定义，来自 eval.c。  （普通函数具有相同的一般外观。）

    
    
    DEFUN ("or", For, Sor, 0, UNEVALLED, 0,
           doc: /* Eval args until one of them yields non-nil,
    then return that value.
    The remaining args are not evalled at all.
    If all args return nil, return nil.
    
    usage: (or CONDITIONS...)  */)
      (Lisp_Object args)
    {
      Lisp_Object val = Qnil;
    
    
      while (CONSP (args))
        {
          val = eval_sub (XCAR (args));
          if (!NILP (val))
    	break;
          args = XCDR (args);
          maybe_quit ();
        }
    
    
      return val;
    }

让我们从对 DEFUN 宏参数的精确解释开始。  这是他们的模板：

    DEFUN (lname, fname, sname, min, max, interactive, doc)

    lname

这是要定义为函数名的 Lisp 符号的名称；  在上面的例子中，它是或。

    fname

这是此函数的 C 函数名称。  这是在 C 代码中用于调用函数的名称。  按照约定，该名称是在 Lisp 名称前面加上“F”，而 Lisp 名称中的所有破折号 (“-”) 都更改为下划线。  因此，要从 C 代码调用此函数，请调用 For。

    sname

这是一个 C 变量名称，用于保存在 Lisp 中表示函数的 subr 对象的数据的结构。  此结构将 Lisp 符号名称传递给初始化例程，该例程将创建符号并将 subr 对象作为其定义存储。  按照惯例，此名称始终为 fname，其中 'F' 替换为 'S'。

    min

这是函数需要的最小参数数量。  该函数或允许最少零个参数。

    max

这是函数接受的最大参数数量（如果有固定最大值）。  或者，它可以是 UNEVALLED，表示接收未评估参数的特殊形式，或 MANY，表示无限数量的评估参数（相当于 &rest）。  UNEVALLED 和 MANY 都是宏。  如果 max 是一个数字，它必须大于 min 但小于 8。

    interactive

这是一个交互式规范，一个字符串，例如可以用作 Lisp 函数中 interactive 的参数（请参阅使用交互式）。  or的情况下为0（空指针），表示or不能交互调用。  "" 值表示在交互调用时不应接收任何参数的函数。  如果值以 '"(' 开头，则字符串被评估为 Lisp 形式。例如：

    DEFUN ("foo", Ffoo, Sfoo, 0, 3,
           "(list (read-char-by-name \"Insert character: \")\
    	      (prefix-numeric-value current-prefix-arg)\
    	      t)",
           doc: /* … */)

    doc

这是文档字符串。  它使用 C 注释语法而不是 C 字符串语法，因为注释语法不需要什么特别的东西来包含多行。  'doc:' 将后面的注释标识为文档字符串。  开始和结束注释的 '***' 和 '***' 分隔符不是文档字符串的一部分。

如果文档字符串的最后一行以关键字“用法：”开头，则该行的其余部分被视为用于文档目的的参数列表。  这样，您可以在文档字符串中使用与 C 代码中使用的参数名称不同的参数名称。  如果函数有无限数量的参数，则需要“用法：”。

一些原语有多个定义，每个平台一个（例如，x-create-frame）。  在这种情况下，不是在每个定义中编写相同的文档字符串，而是只有一个定义具有实际文档。  其他的有以“SKIP”开头的占位符，解析 DOC 文件的函数会忽略这些占位符。

Lisp 代码中文档字符串的所有常用规则（请参阅文档字符串提示）也适用于 C 代码文档字符串。

文档字符串后面可以跟着实现原语的 C 函数的 C 函数属性列表，如下所示：

    DEFUN ("bar", Fbar, Sbar, 0, UNEVALLED, 0
           doc: /* … */
           attributes: attr1 attr2 …)

您可以一个接一个地指定多个属性。  目前，仅识别以下属性：

    noreturn

将 C 函数声明为永远不会返回的函数。  这对应于 GCC 的 C11 关键字 <span class="underline">Noreturn 和 <span class="underline">\_attribute</span></span> ((<span class="underline"><span class="underline">noreturn</span></span>)) 属性（请参阅使用 GNU 编译器集合中的函数属性）。

    const

声明该函数不检查除其参数之外的任何值，并且除了返回值之外没有任何影响。  这对应于 GCC 的 <span class="underline"><span class="underline">attribute</span></span> ((<span class="underline"><span class="underline">const</span></span>)) 属性。

    noinline

这对应于 GCC 的 <span class="underline"><span class="underline">attribute</span></span> ((<span class="underline"><span class="underline">noinline</span></span>)) 属性，它可以防止函数被考虑内联。  这可能是需要的，例如，为了抵消链接时间优化对基于堆栈的变量的影响。

在调用 DEFUN 宏之后，您必须为 C 函数编写参数列表，包括参数的类型。  如果原语接受固定的最大数量的 Lisp 参数，则每个 Lisp 参数必须有一个 C 参数，并且每个参数必须是 Lisp\_Object 类型。  （用于创建 Lisp\_Object 类型值的各种宏和函数在文件 lisp.h 中声明。）如果原语是特殊形式，它必须接受一个 Lisp 列表，其中包含其未计算的 Lisp 参数作为 Lisp\_Object 类型的单个参数。  如果原语对评估的 Lisp 参数的数量没有上限，它必须正好有两个 C 参数：第一个是 Lisp 参数的数量，第二个是包含它们的值的块的地址。  它们分别具有 ptrdiff\_t 和 Lisp\_Object \* 类型。  由于 Lisp\_Object 可以保存任何数据类型的任何 Lisp 对象，因此您只能在运行时确定实际数据类型；  因此，如果您希望原语​​仅接受某种类型的参数，则必须使用合适的谓词显式检查类型（请参阅类型谓词）。

在函数 For 自身中，局部变量 args 引用由 Emacs 的堆栈标记垃圾收集器控制的对象。  尽管垃圾收集器不会回收可从 C Lisp\_Object 堆栈变量中访问的对象，但它可能会移动对象的某些组件，例如字符串的内容或缓冲区的文本。  因此，访问这些组件的函数必须注意在执行 Lisp 评估后重新获取它们的地址。  这意味着代码应该保留缓冲区或字符串位置，并在执行 Lisp 评估后从该位置重新计算 C 指针，而不是保留指向字符串内容或缓冲区文本的 C 指针。  Lisp 评估可以通过直接或间接调用 eval\_sub 或 Feval 来进行。

注意循环内部对maybe\_quit 的调用：该函数检查用户是否按下了Cg，如果是，则中止处理。  您应该在可能需要大量迭代的任何循环中执行此操作；  在这种情况下，参数列表可能会很长。  这增加了 Emacs 的响应能力并改善了用户体验。

除非在转储 Emacs 后永远不会写入变量，否则不得将 C 初始化程序用于静态或全局变量。  由于转储 Emacs，这些带有初始化程序的变量被分配在变为只读的内存区域中（在某些操作系统上）。  请参阅纯存储。

定义 C 函数不足以使 Lisp 原语可用；  您还必须为原语创建 Lisp 符号，并将合适的 subr 对象存储在其函数单元中。  代码如下所示：

    defsubr (&sname);

这里 sname 是您用作 DEFUN 的第三个参数的名称。

如果您向已经定义了 Lisp 原语的文件添加新原语，请找到名为 syms\_of\_something 的函数（靠近文件末尾），然后在此处添加对 defsubr 的调用。  如果该文件没有此功能，或者如果您创建了一个新文件，请在其中添加一个 syms\_of\_filename（例如，syms\_of\_myfile）。  然后在 emacs.c 中找到调用所有这些函数的位置，并在那里添加对 syms\_of\_filename 的调用。

函数 syms\_of\_filename 也是定义任何作为 Lisp 变量可见的 C 变量的地方。  DEFVAR\_LISP 使 Lisp\_Object 类型的 C 变量在 Lisp 中可见。  DEFVAR\_INT 使 int 类型的 C 变量在 Lisp 中可见，其值始终为整数。  DEFVAR\_BOOL 使 int 类型的 C 变量在 Lisp 中可见，其值为 t 或 nil。  请注意，使用 DEFVAR\_BOOL 定义的变量会自动添加到字节编译器使用的列表 byte-boolean-vars 中。

这些宏都需要三个参数：

    lname

Lisp 程序要使用的变量的名称。

    vname

C 源代码中变量的名称。

    doc

变量的文档，作为 C 注释。  有关更多详细信息，请参阅文档基础。

按照惯例，在定义“本机”类型（int 和 bool）的变量时，C 变量的名称是 Lisp 变量的名称，其中 - 替换为 \_。  当变量具有 Lisp\_Object 类型时，约定也是在 C 变量名称前加上 V。即

    DEFVAR_INT ("my-int-variable", my_int_variable,
    	   doc: /* An integer variable.  */);
    
    DEFVAR_LISP ("my-lisp-variable", Vmy_lisp_variable,
    	   doc: /* A Lisp variable.  */);

在 Lisp 中，您需要引用符号本身而不是符号的值。  一种这样的情况是临时覆盖变量的值，在 Lisp 中是用 let 完成的。  在 C 源代码中，这是通过定义相应的常量符号并使用 specbind 来完成的。  按照约定，Qmy\_lisp\_variable 对应 Vmy\_lisp\_variable；  要定义它，请使用 DEFSYM 宏。  IE

    DEFSYM (Qmy_lisp_variable, "my-lisp-variable");

要执行实际绑定：

    specbind (Qmy_lisp_variable, Qt);

在 Lisp 中，符号有时需要被引用，为了在 C 中达到相同的效果，您再次使用相应的常量符号 Qmy\_lisp\_variable。  例如，在 Lisp 中创建缓冲区局部变量（请参阅缓冲区局部变量）时，您可以编写：

    (make-variable-buffer-local 'my-lisp-variable)

在C中对应的代码使用Fmake\_variable\_buffer\_local结合DEFSYM，即

    DEFSYM (Qmy_lisp_variable, "my-lisp-variable");
    Fmake_variable_buffer_local (Qmy_lisp_variable);

如果你想让一个在 C 中定义的 Lisp 变量表现得像一个用 defcustom 声明的，添加一个适当的条目到 cus-start.el。  有关要使用的格式的说明，请参阅定义自定义变量。

如果直接定义 Lisp\_Object 类型的文件范围 C 变量，则必须通过在 syms\_of\_filename 中调用 staticpro 来保护它免受垃圾收集，如下所示：

    staticpro (&variable);

这是另一个示例函数，具有更复杂的参数。  这来自 window.c 中的代码，它演示了如何使用宏和函数来操作 Lisp 对象。

    
    
    DEFUN ("coordinates-in-window-p", Fcoordinates_in_window_p,
           Scoordinates_in_window_p, 2, 2, 0,
           doc: /* Return non-nil if COORDINATES are in WINDOW.
      …
    
      or `right-margin' is returned.  */)
      (register Lisp_Object coordinates, Lisp_Object window)
    {
      struct window *w;
      struct frame *f;
      int x, y;
      Lisp_Object lx, ly;
    
    
      w = decode_live_window (window);
      f = XFRAME (w->frame);
      CHECK_CONS (coordinates);
      lx = Fcar (coordinates);
      ly = Fcdr (coordinates);
      CHECK_NUMBER (lx);
      CHECK_NUMBER (ly);
      x = FRAME_PIXEL_X_FROM_CANON_X (f, lx) + FRAME_INTERNAL_BORDER_WIDTH (f);
      y = FRAME_PIXEL_Y_FROM_CANON_Y (f, ly) + FRAME_INTERNAL_BORDER_WIDTH (f);
    
    
      switch (coordinates_in_window (w, x, y))
        {
        case ON_NOTHING:            /* NOT in window at all.  */
          return Qnil;
    
    
        …
    
        case ON_MODE_LINE:          /* In mode line of window.  */
          return Qmode_line;
    
    
        …
    
        case ON_SCROLL_BAR:         /* On scroll-bar of window.  */
          /* Historically we are supposed to return nil in this case.  */
          return Qnil;
    
    
        default:
          emacs_abort ();
        }
    }

注意，C 代码不能按名称调用函数，除非它们是用 C 定义的。调用用 Lisp 编写的函数的方法是使用 Ffuncall，它体现了 Lisp 函数 funcall。  由于 Lisp 函数 funcall 接受无限数量的参数，因此在 C 中它需要两个：Lisp 级别参数的数量，以及包含它们的值的一维数组。  第一个 Lisp 级别的参数是要调用的 Lisp 函数，其余的是要传递给它的参数。

C 函数 call0、call1、call2 等提供了方便的方法来方便地使用固定数量的参数调用 Lisp 函数。  他们通过调用 Ffuncall 来工作。

eval.c 是一个非常好的文件，可以查看示例；  lisp.h 包含一些重要的宏和函数的定义。

如果您定义一个无副作用或纯函数，请分别给它一个非零无副作用或纯属性（请参阅标准符号属性）。


<a id="orgbab4a07"></a>

## TODO E.8 编写动态加载的模块

本节介绍 Emacs 模块 API 以及如何将其用作为 Emacs 编写扩展模块的一部分。  模块 API 是用 C 编程语言定义的，因此本节中的描述和示例假定模块是用 C 编写的。对于其他编程语言，您将需要使用适当的绑定、接口和工具来调用 C 代码。  Emacs C 代码需要 C99 或更高版本的编译器（请参阅 C 方言），因此本节中的代码示例也遵循该标准。

编写一个模块并将其集成到 Emacs 中包括以下任务：

-   为模块编写初始化代码。
-   编写一个或多个模块函数。
-   在 Emacs 和您的模块函数之间传递值和对象。
-   处理错误条件和非本地退出。

以下小节更详细地描述了这些任务和 API 本身。

编写模块后，根据底层平台的约定对其进行编译以生成共享库。  然后将共享库放在 load-path 中提到的目录中（请参阅库搜索），Emacs 会在其中找到它。

如果您希望验证模块与 Emacs 动态模块 API 的一致性，请使用 &#x2013;module-assertions 选项调用 Emacs。  请参阅 GNU Emacs 手册中的初始选项。


<a id="org090cf94"></a>

### TODO E.8.1 模块初始化代码

通过包含头文件 emacs-module.h 并定义 GPL 兼容性符号来开始您的模块：

    #include <emacs-module.h>
    
    int plugin_is_GPL_compatible;

emacs-module.h 文件作为 Emacs 安装的一部分安装到系统的包含树中。  或者，您可以在 Emacs 源代码树中找到它。

接下来，为模块编写一个初始化函数。

    Function: int emacs_module_init (struct emacs_runtime *runtime) ¶

Emacs 在加载模块时调用此函数。  如果模块没有导出名为 emacs\_module\_init 的函数，则尝试加载模块将发出错误信号。  如果初始化成功，初始化函数应该返回零，否则返回非零。  在后一种情况下，Emacs 将发出错误信号，并且模块的加载将失败。  如果用户在初始化过程中按下 Cg，Emacs 会忽略初始化函数的返回值并退出（参见 Quitting）。  （如果需要，您可以在初始化函数中捕获用户退出，请参阅 should\_quit。）

参数 runtime 是指向包含 2 个公共字段的 C 结构的指针： size，提供结构的大小（以字节为单位）；  和 get\_environment，它提供了一个指向函数的指针，该函数允许模块初始化函数访问 Emacs 环境对象及其接口。

初始化函数应该执行模块所需的任何初始化。  此外，它还可以执行以下任务：

-   兼容性验证
    
    模块可以通过将运行时结构的 size 成员与编译到模块中的值进行比较来验证加载模块的 Emacs 可执行文件是否与模块兼容：
    
        int
        emacs_module_init (struct emacs_runtime *runtime)
        {
          if (runtime->size < sizeof (*runtime))
            return 1;
        }
    
    如果传递给模块的运行时对象的大小小于它的预期大小，这意味着该模块是为比尝试加载它的版本更新（晚）的 Emacs 版本编译的，即该模块可能与 Emacs 不兼容二进制。
    
    此外，模块可以验证模块 API 与模块期望的兼容性。  以下示例代码假定它是上面显示的 emacs\_module\_init 函数的一部分：
    
        emacs_env *env = runtime->get_environment (runtime);
         if (env->size < sizeof (*env))
           return 2;
    
    这使用运行时结构中提供的指针调用 get\_environment 函数来检索指向 API 环境的指针，这是一个 C 结构，它还有一个 size 字段，以字节为单位保存结构的大小。
    
    最后，您可以通过将 Emacs 传递的环境大小与已知大小进行比较，编写一个适用于旧版本 Emacs 的模块，如下所示：
    
        emacs_env *env = runtime->get_environment (runtime);
        if (env->size >= sizeof (struct emacs_env_26))
          emacs_version = 26;  /* Emacs 26 or later.  */
        else if (env->size >= sizeof (struct emacs_env_25))
          emacs_version = 25;
        else
          return 2; /* Unknown or unsupported version.  */
    
    这是可行的，因为后来的 Emacs 版本总是向环境中添加成员，从不删除任何成员，因此大小只能随着​​ Emacs 新版本的增加而增长。  给定 Emacs 的版本，该模块只能使用该版本中存在的模块 API 的部分，因为这些部分在以后的版本中是相同的。
    
    emacs-module.h 定义了一个预处理器宏 EMACS\_MAJOR\_VERSION。  它扩展为一个整数文字，这是标题支持的 Emacs 的最新主要版本。  请参阅版本信息。  请注意，EMACS\_MAJOR\_VERSION 的值是编译时常量，并不代表当前正在运行并已加载您的模块的 Emacs 版本。  如果你希望你的模块兼容各种版本的 emacs-module.h 以及各种版本的 Emacs，你可以使用基于 EMACS\_MAJOR\_VERSION 的条件编译。
    
    我们建议模块始终执行兼容性验证，除非它们完全在初始化函数中完成它们的工作，并且不要访问任何 Lisp 对象或使用任何可通过环境结构访问的 Emacs 函数。

-   将模块函数绑定到 Lisp 符号
    
    这给了模块函数名称，以便 Lisp 代码可以使用该名称调用它。  我们在下面的编写模块函数中描述了如何做到这一点。


<a id="org7697c89"></a>

### TODO E.8.2 编写模块函数

编写 Emacs 模块的主要原因是为加载该模块的 Lisp 程序提供附加功能。  本小节介绍如何编写此类模块函数。

模块函数具有以下一般形式和签名：

    Function: emacs_value emacs_function (emacs_env *env, ptrdiff_t nargs, emacs_value *args, void *data) ¶

env 参数提供了一个指向 API 环境的指针，需要访问 Emacs 对象和函数。  nargs 参数是所需的参数数量，可以为零（参见下面的 make\_function 以获得更灵活的参数数量规范），而 args 是指向函数参数数组的指针。  参数 data 指向函数所需的附加数据，这些数据是在调用 make\_function（见下文）从 emacs\_function 创建 Emacs 函数时安排的。

模块函数使用 emacs\_value 类型在 Emacs 和模块之间通信 Lisp 对象（请参阅 Lisp 和模块值之间的转换）。  API，在下面和以下小节中描述，为基本 C 数据类型和相应的 emacs\_value 对象之间的转换提供了便利。

模块函数总是返回一个值。  如果函数正常返回，调用它的 Lisp 代码会看到函数返回的 emacs\_value 值对应的 Lisp 对象。  但是，如果用户键入 Cg，或者如果模块函数或其被调用者发出错误信号或非本地退出（请参阅模块中的非本地退出），Emacs 将忽略返回值并退出或抛出，就像 Lisp 代码遇到相同情况时一样.

头文件 emacs-module.h 提供类型 emacs\_function 作为指向模块函数的函数指针的别名类型。

在为模块函数编写 C 代码之后，您应该使用 make\_function 函数从中创建一个 Lisp 函数对象，该函数的指针在环境中提供（回想一下，指向环境的指针由 get\_environment 返回）。  这通常在模块初始化函数中完成（参见模块初始化函数），在验证 API 兼容性之后。

    Function: emacs_value make_function (emacs_env *env, ptrdiff_t min_arity, ptrdiff_t max_arity, emacs_function func, const char *docstring, void *data) ¶

这将返回一个从 C 函数 func 创建的 Emacs 函数，其签名与上面对 emacs\_function 的描述相同。  参数 min\_arity 和 max\_arity 指定 func 可以接受的参数的最小和最大数量。  max\_arity 参数可以具有特殊值 emacs\_variadic\_function，这使得函数可以接受无限数量的参数，就像 Lisp 中的 &rest 关键字（参见参数列表的特性）。

参数 data 是一种安排任意附加数据在调用时传递给 func 的方法。  无论传递给 make\_function 的指针都会原封不动地传递给 func。

参数 docstring 指定函数的文档字符串。  它应该是 ASCII 字符串，或 UTF-8 编码的非 ASCII 字符串，或 NULL 指针；  在后一种情况下，该函数将没有文档。  文档字符串可以以指定广告调用约定的行结尾，请参阅函数的文档字符串。

由于每个模块函数都必须接受指向环境的指针作为其第一个参数，因此可以从任何模块函数调用 make\_function，但您通常希望从模块初始化函数中执行此操作，以便知道所有模块函数加载模块后到 Emacs。

最后，您应该将 Lisp 函数绑定到一个符号，以便 Lisp 代码可以通过名称调用您的函数。  为此，请使用模块 API 函数实习生（请参阅实习生），其指针也在模块函数可以访问的环境中提供。

结合上述步骤，安排 C 函数 module\_func 可作为 Lisp 中的 module-func 调用的代码将如下所示，作为模块初始化函数的一部分：

    emacs_env *env = runtime->get_environment (runtime);
    emacs_value func = env->make_function (env, min_arity, max_arity,
    				       module_func, docstring, data);
    emacs_value symbol = env->intern (env, "module-func");
    emacs_value args[] = {symbol, func};
    env->funcall (env, env->intern (env, "defalias"), 2, args);

这通过调用 env->intern 使 Emacs 知道符号 module-func，然后从 Emacs 调用 defalias 以将函数绑定到该符号。  请注意，可以使用 fset 代替 defalias；  差异在 defalias 中描述。

包括 emacs\_module\_init 函数的模块函数（请参阅模块初始化函数）只能通过从一些实时 emacs\_env 指针调用环境函数来与 Emacs 交互，同时从 Emacs 直接或间接调用。  换句话说，如果模块函数想要调用 Lisp 函数或 Emacs 原语，将 emacs\_value 对象与 C 数据类型转换（参见 Lisp 和模块值之间的转换），或者以任何其他方式与 Emacs 交互，则从 Emacs 调用 emacs\_module\_init或者一个模块函数必须在调用堆栈中。  垃圾收集运行时，模块函数可能无法与 Emacs 交互；  请参阅垃圾收集。  它们只能通过 Emacs 创建的 Lisp 解释器线程（包括主线程）与 Emacs 交互；  请参阅线程。  &#x2013;module-assertions 命令行选项可以检测到一些违反上述要求的情况。  请参阅 GNU Emacs 手册中的初始选项。

使用模块 API，可以定义更复杂的函数和数据类型：内联函数、宏等。但是，生成的 C 代码会很麻烦且难以阅读。  因此，我们建议您将创建函数和数据结构的模块代码限制在绝对最小值，并将其余部分留给模块随附的 Lisp 包，因为在 Lisp 中执行这些额外任务要容易得多，并且会产生更具可读性的代码。  例如，给定一个如上所述定义的模块函数 module-func，基于它制作宏 module-macro 的一种方法是使用以下简单的 Lisp 包装器：

    (defmacro module-macro (&rest args)
      "Documentation string for the macro."
      (module-func args))

当包被加载到 Emacs 中时，与你的模块一起的 Lisp 包可以使用加载原语（参见 Emacs 动态模块）加载模块。

默认情况下，make\_function 创建的模块函数不是交互式的。  要使它们具有交互性，您可以使用以下功能。

    Function: void make_interactive (emacs_env *env, emacs_value function, emacs_value spec) ¶

这个函数从 Emacs 28 开始可用，使用交互规范规范使函数函数交互。  Emacs 将规范解释为交互式表单的参数。  使用交互，请参阅 Code Characters 进行交互。  function 必须是 make\_function 返回的 Emacs 模块函数。

请注意，没有本地模块支持检索模块功能的交互式规范。  为此使用功能交互形式。  使用交互式。  一旦使用 make\_interactive 使其具有交互性，就不可能使模块功能成为非交互性的。

如果你想在模块函数对象（即 make\_function 返回的对象）被垃圾回收时运行一些代码，你可以安装一个函数终结器。  函数终结器从 Emacs 28 开始可用。例如，如果您已将一些堆分配的结构传递给 make\_function 的数据参数，则可以使用终结器来释放结构。  请参阅 (libc)Basic Allocation，并参阅 (libc)Freeing after Malloc。  终结器函数具有以下签名：

    void finalizer (void *data)

这里，data 接收调用 make\_function 时传递给 data 的值。  请注意，终结器不能以任何方式与 Emacs 交互。

直接在调用 make\_function 之后，新创建的函数没有终结器。  如果需要，使用 set\_function\_finalizer 添加一个。

    Function: void emacs_finalizer (void *ptr) ¶

头文件 emacs-module.h 提供类型 emacs\_finalizer 作为 Emacs 终结器函数的类型别名。

    Function: emacs_finalizer get_function_finalizer (emacs_env *env, emacs_value arg) ¶

该函数从 Emacs 28 开始可用，它返回与 arg 表示的模块函数关联的函数终结器。  arg 必须引用模块函数，即 make\_function 返回的对象。  如果没有终结器与函数关联，则返回 NULL。

    Function: void set_function_finalizer (emacs_env *env, emacs_value arg, emacs_finalizer fin) ¶

该函数从 Emacs 28 开始可用，它将与 arg 表示的模块函数关联的函数终结器设置为 fin。  arg 必须引用模块函数，即 make\_function 返回的对象。  fin 可以是 NULL 以清除 arg 的函数终结器，也可以是指向要在 arg 表示的对象被垃圾回收时调用的函数的指针。  每个函数最多可以设置一个函数终结器；  如果 arg 已经有一个终结器，则将其替换为 fin。


<a id="orge7c34d7"></a>

### TODO E.8.3 Lisp和模块值之间的转换

除了极少数例外，大多数模块都需要与调用它们的 Lisp 程序交换数据：接受模块函数的参数并从模块函数返回值。  为此，模块 API 提供了 emacs\_value 类型，它表示通过 API 通信的 Emacs Lisp 对象；  它是 Emacs C 原语中使用的 Lisp\_Object 类型的功能等价物（请参阅编写 Emacs 原语）。  本节介绍模块 API 中允许创建与基本 Lisp 数据类型对应的 emacs\_value 对象的部分，以及如何从与 Lisp 对象对应的 emacs\_value 对象中的 C 数据访问。

下面描述的所有函数实际上都是通过指向每个模块函数接受的环境的指针提供的函数指针。  因此，模块代码应该通过环境指针调用这些函数，如下所示：

    emacs_env *env;  /* the environment pointer */
    env->some_function (arguments…);

emacs\_env 指针通常来自模块函数的第一个参数，或者如果您需要模块初始化函数中的环境，则来自对 get\_environment 的调用。

下面描述的大部分功能在 Emacs 25 中可用，这是第一个支持动态模块的 Emacs 版本。  对于在后来的 Emacs 版本中可用的少数功能，我们提到了第一个支持它们的 Emacs 版本。

以下 API 函数从 emacs\_value 对象中提取各种 C 数据类型的值。  如果参数 emacs\_value 对象不是函数所期望的类型，它们都会引发错误类型参数错误条件（请参阅类型谓词）。  请参阅模块中的非本地退出，了解有关 Emacs 模块中信号错误如何工作的详细信息，以及如何在模块内部的错误条件报告给 Emacs 之前捕获它们。  API 函数 type\_of（参见 type\_of）可用于获取 emacs\_value 对象的类型。

    Function: intmax_t extract_integer (emacs_env *env, emacs_value arg) ¶

此函数返回由 arg 指定的 Lisp 整数的值。  返回值的 C 数据类型 intmax\_t 是 C 编译器支持的最宽整数数据类型，通常为 long long。  如果 arg 的值不适合 intmax\_t，则该函数使用错误符号 overflow-error 发出错误信号。

    Function: bool extract_big_integer (emacs_env *env, emacs_value arg, int *sign, ptrdiff_t *count, emacs_limb_t *magnitude) ¶

这个函数从 Emacs 27 开始可用，它提取 arg 的整数值。  arg 的值必须是整数（fixnum 或 bignum）。  如果 sign 不为 NULL，它将 arg 的符号（-1、0 或 +1）存储到 \*sign 中。  幅度存储到幅度如下。  如果count 和magnitude 都不是NULL，那么magnitude 必须指向一个至少包含\*count unsigned long 元素的数组。  如果幅度大到足以容纳 arg 的幅度，则此函数将幅度以 little-endian 形式写入幅度数组，将写入的数组元素的数量存储到 \*count 中，并返回 true。  如果幅度不够大，它将所需的数组大小存储到 \*count 中，发出错误信号并返回 false。  如果 count 不为 NULL 且幅度为 NULL，则该函数将所需的数组大小存储到 \*count 中并返回 true。

Emacs保证\*count的最大要求值永远不会超过min(PTRDIFF\_MAX, SIZE\_MAX)/sizeof(emacs\_limb\_t)，所以可以使用malloc(\*count \* sizeof \*magnitude)来分配幅度数组，不用担心size中的整数溢出计算。

    Type alias: emacs_limb_t ¶

这是一个无符号整数类型，用作大整数转换函数的幅度数组的元素类型。  该类型保证具有唯一的对象表示，即没有填充位。

    Macro: EMACS_LIMB_MAX ¶

此宏扩展为一个常量表达式，指定 emacs\_limb\_t 对象的最大可能值。  该表达式适用于#if。

    Function: double extract_float (emacs_env *env, emacs_value arg) ¶

此函数返回由 arg 指定的 Lisp 浮点值，作为 C 双精度值。

    Function: struct timespec extract_time (emacs_env *env, emacs_value arg) ¶

此函数从 Emacs 27 开始可用，它将 arg 解释为 Emacs Lisp 时间值并返回相应的 struct timespec。  请参阅一天中的时间。  struct timespec 表示具有纳秒精度的时间戳。  它有以下成员：

    time_t tv_sec

整数秒数。

    long tv_nsec

以纳秒数表示的小数秒。  对于 extract\_time 返回的时间戳，它始终是非负数且小于 10 亿。  （虽然 POSIX 要求 tv\_nsec 的类型为 long，但在某些非标准平台上该类型为 long long。）

请参阅 (libc) 已用时间。

如果时间的精度高于纳秒，则此函数会将其截断为纳秒精度，直至负无穷大。  如果时间（截断为纳秒）不能由 struct timespec 表示，则此函数会发出错误信号。  例如，如果 time\_t 是 32 位整数类型，则 100 亿秒的时间值将发出错误信号，但 600 皮秒的时间值将被截断为零。

如果您需要处理 struct timespec 无法表示的时间值，或者如果您想要更高的精度，请调用 Lisp 函数 encode-time 并使用它的返回值。  请参阅时间转换。

    Function: bool copy_string_contents (emacs_env *env, emacs_value arg, char *buf, ptrdiff_t *len) ¶

此函数将 arg 指定的 Lisp 字符串的 UTF-8 编码文本存储在 buf 指向的 char 数组中，该数组应该有足够的空间来保存至少 \*len 个字节，包括终止的空字节。  参数 len 不能是 NULL 指针，并且在调用函数时，它应该指向一个指定 buf 大小（以字节为单位）的值。

如果 \*len 指定的缓冲区大小足以容纳字符串的文本，则函数将复制到 buf 的实际字节数存储在 \*len 中，包括终止的空字节，并返回 true。  如果缓冲区太小，该函数会引发 args-out-of-range 错误条件，将所需的字节数存储在 \*len 中，并返回 false。  有关如何处理未决错误条件的信息，请参阅模块中的非本地出口。

参数 buf 可以是 NULL 指针，在这种情况下，函数将存储 arg 内容所需的字节数存储在 \*len 中，并返回 true。  这是确定存储特定字符串所需的 buf 大小的方法：首先调用 NULL 作为 buf 的 copy\_string\_contents，然后分配足够的内存来保存函数在 \*len 中存储的字节数，然后再次调用该函数-NULL buf 实际执行文本复制。

    Function: emacs_value vec_get (emacs_env *env, emacs_value vector, ptrdiff_t index) ¶

此函数返回索引处的向量元素。  第一个向量元素的索引为零。  如果 index 的值无效，该函数将引发 args-out-of-range 错误条件。  要从函数返回的值中提取 C 数据，请使用此处描述的其他提取函数，适用于存储在该向量元素中的 Lisp 数据类型。

    Function: ptrdiff_t vec_size (emacs_env *env, emacs_value vector) ¶

此函数返回向量中的元素数。

    Function: void vec_set (emacs_env *env, emacs_value vector, ptrdiff_t index, emacs_value value) ¶

此函数将值存储在索引为索引的向量元素中。  如果 index 的值无效，它会引发 args-out-of-range 错误条件。

以下 API 函数从基本 C 数据类型创建 emacs\_value 对象。  它们都返回创建的 emacs\_value 对象。

    Function: emacs_value make_integer (emacs_env *env, intmax_t n) ¶

此函数接受一个整数参数 n 并返回相应的 emacs\_value 对象。  它根据 n 的值是否在 most-negative-fixnum 和 most-positive-fixnum 设置的限制内返回一个 fixnum 或一个 bignum（请参阅整数基础）。

    Function: emacs_value make_big_integer (emacs_env *env, int sign, ptrdiff_t count, const emacs_limb_t *magnitude) ¶

这个函数从 Emacs 27 开始可用，它接受一个任意大小的整数参数并返回一个对应的 emacs\_value 对象。  sign 参数给出返回值的符号。  如果 sign 不为零，则幅度必须指向一个至少包含 count 个元素的数组，该数组指定返回值的 little-endian 幅度。

以下示例使用 GNU 多精度库 (GMP) 来计算给定整数之后的下一个可能的素数。  有关 GMP 的一般概述，请参阅 (gmp)Top，有关如何将幅度数组与 GMP mpz\_t 值相互转换，请参阅 (gmp)Integer Import and Export。

    #include <emacs-module.h>
    int plugin_is_GPL_compatible;
    
    #include <assert.h>
    #include <limits.h>
    #include <stdint.h>
    #include <stdlib.h>
    #include <string.h>
    
    #include <gmp.h>
    
    static void
    memory_full (emacs_env *env)
    {
      static const char message[] = "Memory exhausted";
      emacs_value data = env->make_string (env, message,
    				       strlen (message));
      env->non_local_exit_signal
        (env, env->intern (env, "error"),
         env->funcall (env, env->intern (env, "list"), 1, &data));
    }
    
    enum
    {
      order = -1, endian = 0, nails = 0,
      limb_size = sizeof (emacs_limb_t),
      max_nlimbs = ((SIZE_MAX < PTRDIFF_MAX ? SIZE_MAX : PTRDIFF_MAX)
    		/ limb_size)
    };
    
    static bool
    extract_big_integer (emacs_env *env, emacs_value arg, mpz_t result)
    {
      ptrdiff_t nlimbs;
      bool ok = env->extract_big_integer (env, arg, NULL, &nlimbs, NULL);
      if (!ok)
        return false;
      assert (0 < nlimbs && nlimbs <= max_nlimbs);
      emacs_limb_t *magnitude = malloc (nlimbs * limb_size);
      if (magnitude == NULL)
        {
          memory_full (env);
          return false;
        }
      int sign;
      ok = env->extract_big_integer (env, arg, &sign, &nlimbs, magnitude);
      assert (ok);
      mpz_import (result, nlimbs, order, limb_size, endian, nails, magnitude);
      free (magnitude);
      if (sign < 0)
        mpz_neg (result, result);
      return true;
    }
    
    static emacs_value
    make_big_integer (emacs_env *env, const mpz_t value)
    {
      size_t nbits = mpz_sizeinbase (value, 2);
      int bitsperlimb = CHAR_BIT * limb_size - nails;
      size_t nlimbs = nbits / bitsperlimb + (nbits % bitsperlimb != 0);
      emacs_limb_t *magnitude
        = nlimbs <= max_nlimbs ? malloc (nlimbs * limb_size) : NULL;
      if (magnitude == NULL)
        {
          memory_full (env);
          return NULL;
        }
      size_t written;
      mpz_export (magnitude, &written, order, limb_size, endian, nails, value);
      assert (written == nlimbs);
      assert (nlimbs <= PTRDIFF_MAX);
      emacs_value result = env->make_big_integer (env, mpz_sgn (value),
    					      nlimbs, magnitude);
      free (magnitude);
      return result;
    }
    
    static emacs_value
    next_prime (emacs_env *env, ptrdiff_t nargs, emacs_value *args,
    	    void *data)
    {
      assert (nargs == 1);
      mpz_t p;
      mpz_init (p);
      extract_big_integer (env, args[0], p);
      mpz_nextprime (p, p);
      emacs_value result = make_big_integer (env, p);
      mpz_clear (p);
      return result;
    }
    
    int
    emacs_module_init (struct emacs_runtime *runtime)
    {
      emacs_env *env = runtime->get_environment (runtime);
      emacs_value symbol = env->intern (env, "next-prime");
      emacs_value func
        = env->make_function (env, 1, 1, next_prime, NULL, NULL);
      emacs_value args[] = {symbol, func};
      env->funcall (env, env->intern (env, "defalias"), 2, args);
      return 0;
    }

    Function: emacs_value make_float (emacs_env *env, double d) ¶

这个函数接受一个双参数 d 并返回相应的 Emacs 浮点值。

    Function: emacs_value make_time (emacs_env *env, struct timespec time) ¶

该函数从 Emacs 27 开始可用，它采用 struct timespec 参数 time 并将相应的 Emacs 时间戳作为一对（ticks .hz）返回。  请参阅一天中的时间。  返回值表示与时间完全相同的时间戳：所有输入值都是可表示的，并且永远不会损失精度。  time.tv\_sec 和 time.tv\_nsec 可以是任意值。  特别是，没有要求将时间标准化。  这意味着 time.tv\_nsec 可以为负数或大于 999,999,999。

    Function: emacs_value make_string (emacs_env *env, const char *str, ptrdiff_t len) ¶

此函数从 str 指向的 C 文本字符串创建一个 Emacs 字符串，该字符串的字节长度（不包括终止的空字节）为 len。  str 中的原始字符串可以是 ASCII 字符串，也可以是 UTF-8 编码的非 ASCII 字符串；  它可以包含嵌入的空字节，并且不必以 str[len] 处的终止空字节结尾。  如果 len 为负数或超过 Emacs 字符串的最大长度，该函数将引发溢出错误错误条件。  如果 len 为零，则 str 可以为 NULL，否则它必须指向有效内存。  对于非零 len，make\_string 返回唯一的可变字符串对象。

    Function: emacs_value make_unibyte_string (emacs_env *env, const char *str, ptrdiff_t len) ¶

该函数与make\_string类似，但对C字符串中字节的值没有限制，可用于将二进制数据以单字节字符串的形式传递给Emacs。

API 不提供操作 Lisp 数据结构的函数，例如，使用 cons 和 list 创建列表（请参阅构建 Cons 单元格和列表），使用 car 和 cdr 提取列表成员（请参阅访问列表元素），使用 vector (请参阅向量函数）等。对于这些，使用下一小节中描述的 intern 和 funcall 来调用相应的 Lisp 函数。

通常，emacs\_value 对象的生命周期相当短：当用于创建它们的 emacs\_env 指针超出范围时，它就会结束。  有时，您可能需要创建全局引用：emacs\_value 对象可以随心所欲地存在。  使用以下两个函数来管理此类对象。

    Function: emacs_value make_global_ref (emacs_env *env, emacs_value value) ¶

此函数返回值的全局引用。

    Function: void free_global_ref (emacs_env *env, emacs_value global_value) ¶

此函数释放之前由 make\_global\_ref 创建的 global\_value。  调用后 global\_value 不再有效。  您的模块代码应将每次调用 make\_global\_ref 与相应的 free\_global\_ref 配对。

保留需要稍后传递给模块函数的 C 数据结构的另一种方法是创建用户指针对象。  用户指针或 user-ptr 对象是封装了 C 指针的 Lisp 对象，并且可以具有关联的终结器函数，该函数在对象被垃圾回收时调用（请参阅垃圾回收）。  模块 API 提供了创建和访问 user-ptr 对象的函数。  如果在不代表 user-ptr 对象的 emacs\_value 上调用这些函数，则会引发错误类型参数错误条件。

    Function: emacs_value make_user_ptr (emacs_env *env, emacs_finalizer fin, void *ptr) ¶

此函数创建并返回一个包装 C 指针 ptr 的用户 ptr 对象。  终结器函数 fin 可以是 NULL 指针（意味着没有终结器），也可以是具有以下签名的函数：

    typedef void (*emacs_finalizer) (void *ptr);

如果 fin 不是一个 NULL 指针，当 user-ptr 对象被垃圾回收时，它将以 ptr 作为参数被调用。  不要在终结器中运行任何昂贵的代码，因为 GC 必须快速完成以保持 Emacs 响应。

    Function: void * get_user_ptr (emacs_env *env, emacs_value arg) ¶

此函数从 arg 表示的 Lisp 对象中提取 C 指针。

    Function: void set_user_ptr (emacs_env *env, emacs_value arg, void *ptr) ¶

此函数将嵌入在由 arg 表示的 user-ptr 对象中的 C 指针设置为 ptr。

    Function: emacs_finalizer get_user_finalizer (emacs_env *env, emacs_value arg) ¶

此函数返回由 arg 表示的 user-ptr 对象的终结器，如果没有终结器，则返回 NULL。

    Function: void set_user_finalizer (emacs_env *env, emacs_value arg, emacs_finalizer fin) ¶

此函数将 arg 表示的 user-ptr 对象的终结器更改为 fin。  如果 fin 是 NULL 指针，则 user-ptr 对象将没有终结器。

请注意，emacs\_finalizer 类型适用于用户指针和模块函数终结器。  请参阅模块函数终结器。


<a id="org81670e5"></a>

### TODO E.8.4 模块的其他便利功能

本小节描述了模块 API 提供的一些便利功能。  和前面小节中描述的函数一样，它们实际上都是函数指针，需要通过 emacs\_env 指针调用。  在 Emacs 25 调用它们可用的第一个版本之后引入的函数的描述。

    Function: bool eq (emacs_env *env, emacs_value a, emacs_value b) ¶

如果 a 和 b 表示的 Lisp 对象相同，则此函数返回 true，否则返回 false。  这与 Lisp 函数 eq 相同（参见 Equality Predicates），但避免了对参数表示的对象进行实习的需要。

没有其他相等谓词的 API 函数，因此您需要使用下面描述的 intern 和 funcall 来执行更复杂的相等测试。

    Function: bool is_not_nil (emacs_env *env, emacs_value arg) ¶

该函数测试 arg 表示的 Lisp 对象是否为非 nil；  它相应地返回真或假。

请注意，您可以通过使用 intern 来获得一个表示 nil 的 emacs\_value 来实现等效测试，然后使用上述 eq 来测试相等性。  但是使用这个功能更方便。

    Function: emacs_value type_of (emacs_env *env, emacs_value arg) ¶

此函数将 arg 的类型作为表示符号的值返回：字符串表示字符串，整数表示整数，进程表示进程等。请参阅类型谓词。  如果您的代码需要依赖于对象类型，您可以使用 intern 和 eq 与已知类型符号进行比较。

    Function: emacs_value intern (emacs_env *env, const char *name) ¶

此函数返回一个名为 name 的内部 Emacs 符号，它应该是一个以 ASCII 空字符结尾的字符串。  如果一个符号尚不存在，它会创建一个新符号。

与下面描述的 funcall 一起，该函数提供了一种调用任何 Lisp 可调用 Emacs 函数的方法，前提是它的名称是纯 ASCII 字符串。  例如，下面是如何通过调用更强大的 Emacs 实习函数来实习名称 name\_str 是非 ASCII 的符号（请参阅创建和实习符号）：

    emacs_value fintern = env->intern (env, "intern");
    emacs_value sym_name =
      env->make_string (env, name_str, strlen (name_str));
    emacs_value symbol = env->funcall (env, fintern, 1, &sym_name);

emacs\_value fintern = env->intern (env, "intern");
emacs\_value sym\_name =
  env->make\_string (env, name\_str, strlen (name\_str));
emacs\_value 符号 = env->funcall (env, fintern, 1, &sym\_name);

    Function: emacs_value funcall (emacs_env *env, emacs_value func, ptrdiff_t nargs, emacs_value *args) ¶

此函数调用指定的函数，将 args 参数从 args 指向的数组传递给它。  参数 func 可以是函数符号（例如，由上述实习生返回）、make\_function 返回的模块函数（参见编写模块函数）、用 C 编写的子例程等。如果 nargs 为零，则 args 可以是 NULL 指针.

该函数返回 func 返回的值。

如果您的模块包含可能长时间运行的代码，最好不时检查该代码中的用户是否想要退出，例如，通过键入 Cg（请参阅退出）。  自 Emacs 26.1 起可用的以下函数就是为此目的而提供的。

    Function: bool should_quit (emacs_env *env) ¶

如果用户想退出，此函数返回 true。  在这种情况下，我们建议您的模块函数中止任何正在进行的处理并尽快返回。  在大多数情况下，请改用 process\_input。

除了检查用户是否想要退出之外，要处理输入事件，请使用以下函数，该函数从 Emacs 27.1 开始可用。

    Function: enum emacs_process_input_result process_input (emacs_env *env) ¶

此函数处理待处理的输入事件。  如果用户想要退出或在处理信号时发生错误，它会返回 emacs\_process\_input\_quit。  在这种情况下，我们建议您的模块函数中止任何正在进行的处理并尽快返回。  如果模块代码可以继续运行，process\_input 返回 emacs\_process\_input\_continue。  当且仅当 env 中没有挂起的非本地退出时，返回值是 emacs\_process\_input\_continue。  如果模块在调用 process\_input 后​​继续，则变量值和缓冲区内容等全局状态可能已以任意方式修改。

    Function: int open_channel (emacs_env *env, emacs_value pipe_process) ¶

此功能从 Emacs 28 开始可用，它为现有管道进程打开了一个通道。  pipe\_process 必须引用由 make-pipe-process 创建的现有管道进程。  管道流程。  如果成功，返回值将是一个新的文件描述符，您可以使用它来写入管道。  与所有其他模块函数不同，您可以使用从任意线程返回的文件描述符，即使没有模块环境处于活动状态。  您可以使用 write 函数写入文件描述符。  完成后，使用 close 关闭文件描述符。  (libc) 低级 I/O。


<a id="orgb492617"></a>

### TODO E.8.5 模块中的非本地出口

Emacs Lisp 支持非本地退出，由此程序控制从程序中的一个点转移到另一个远程点。  请参阅非本地出口。  因此，您的模块调用的 Lisp 函数可能会通过调用 signal 或 throw 非本地退出，并且您的模块函数必须正确处理此类非本地退出。  需要这样的处理是因为 C 程序在这些情况下不会自动释放资源并执行其他清理；  您的模块代码必须自己完成。  模块 API 为此提供了便利，如本小节所述。  它们从 Emacs 25 开始普遍可用；  它们中的那些在以后的版本中可用明确地调用了第一个 Emacs 版本，它们成为 API 的一部分。

当模块函数调用的某些 Lisp 代码发出错误信号或抛出异常时，非本地出口被捕获，待处理的出口及其相关数据被存储在环境中。  每当一个非本地出口在环境中挂起时，使用指向该环境的指针调用的任何模块 API 函数将立即返回而不进行任何处理（函数 non\_local\_exit\_check、non\_local\_exit\_get 和 non\_local\_exit\_clear 是此规则的例外）。  如果你的模块函数然后什么都不做并返回 Emacs，一个挂起的非本地退出将导致 Emacs 对其采取行动：发出错误信号或抛出相应的 catch。

因此，对模块函数中的非本地退出最简单的“处理”就是不做任何特别的事情，让其余的代码像什么都没发生一样运行。  但是，这可能会导致两类问题：

-   您的模块函数可能使用未初始化或未定义的值，因为 API 函数会立即返回而不会产生预期结果。
-   您的模块可能会泄漏资源，因为它可能没有机会释放它们。

因此，我们建议您的模块函数使用下面描述的函数检查非本地退出条件并从中恢复。

    Function: enum emacs_funcall_exit non_local_exit_check (emacs_env *env) ¶

此函数返回存储在 env 中的非本地退出条件。  可能的值是：

    emacs_funcall_exit_return ¶

最后一个 API 函数正常退出。

    emacs_funcall_exit_signal ¶

最后一个 API 函数发出错误信号。

    emacs_funcall_exit_throw ¶

最后一个 API 函数通过 throw 退出。

    Function: enum emacs_funcall_exit non_local_exit_get (emacs_env *env, emacs_value *symbol, emacs_value *data) ¶

此函数返回存储在 env 中的非本地退出条件类型，就像 non\_local\_exit\_check 一样，但它也返回有关非本地退出的完整信息（如果有）。  如果返回值为 emacs\_funcall\_exit\_signal，则该函数将错误符号存储在 \*symbol 中，并将错误数据存储在 \*data 中（请参阅如何发出错误信号）。  如果返回值为 emacs\_funcall\_exit\_throw，则函数将 catch 标记符号存储在 \*symbol 中，将 throw 值存储在 \*data 中。  当返回值为 emacs\_funcall\_exit\_return 时，该函数不会在这些参数指向的内存中存储任何内容。

您应该检查重要的非本地退出条件：在分配某些资源之前或在分配可能需要释放的资源之后，或者失败意味着进一步处理是不可能或不可行的。

一旦你的模块函数检测到一个非本地出口处于挂起状态，它可以返回到 Emacs（在执行必要的本地清理之后），或者它可以尝试从非本地出口恢复。  以下 API 函数将帮助完成这些任务。

    Function: void non_local_exit_clear (emacs_env *env) ¶

此函数从 env 中清除挂起的非本地退出条件和数据。  调用后，模块API函数将正常工作。  如果您的模块函数可以从它调用的 Lisp 函数的非本地退出中恢复并继续，并且在调用以下任何两个函数（或任何其他 API 函数，如果您希望它们在非本地时执行其预期处理）之前，请使用此函数退出待定）。

    Function: void non_local_exit_throw (emacs_env *env, emacs_value tag, emacs_value value) ¶

这个函数抛出到由 tag 表示的 Lisp catch 符号，将它的值作为要返回的值传递。  您的模块函数通常应该在调用此函数后很快返回。  此函数的一种用途是当您想要从调用的 API 或 Lisp 函数之一重新抛出非本地退出时。

    Function: void non_local_exit_signal (emacs_env *env, emacs_value symbol, emacs_value data) ¶

这个函数用指定的错误数据数据来表示错误符号符号所代表的错误。  调用此函数后，模块函数应立即返回。  这个函数可能很有用，例如，用于从模块函数向 Emacs 发送错误信号。


<a id="orgd69f55d"></a>

# TODO E.9 对象内部

Emacs Lisp 提供了一组丰富的数据类型。  其中一些，如 cons 单元格、整数和字符串，几乎是所有 Lisp 方言所共有的。  其他一些，如标记和缓冲区，非常特殊，需要为在 Lisp 中编写编辑器命令提供基本支持。  为了实现如此多种对象类型并提供一种在解释器的子系统之间传递对象的有效方式，有一组 C 数据结构和一种特殊类型来表示指向所有这些对象的指针，称为标记指针.

在 C 中，标记指针是 Lisp\_Object 类型的对象。  这种类型的任何已初始化变量始终保存以下基本数据类型之一的值：整数、符号、字符串、cons 单元格、浮点数或矢量对象。  这些数据类型中的每一种都有相应的标签值。  所有标签都由 enum Lisp\_Type 枚举并放入 Lisp\_Object 的 3 位位域中。  其余位是值本身。  整数是直接的，即直接由那些值位表示，而所有其他对象都由指向从堆中分配的相应对象的 C 指针表示。  Lisp\_Object 的宽度取决于平台和配置：通常它等于底层平台指针的宽度（即，在 32 位机器上为 32 位，在 64 位机器上为 64 位），但也存在是一种特殊的配置，其中 Lisp\_Object 是 64 位的，但所有指针都是 32 位的。  后一个技巧旨在通过对 Lisp\_Object 使用 64 位 long long 类型来克服 32 位系统上 Lisp 整数值的有限范围。

lisp.h 中定义了以下 C 数据结构，以表示整数以外的基本数据类型：

    struct Lisp_Cons

Cons cell，用于构造列表的对象。

    struct Lisp_String

String，表示字符序列的基本对象。

    struct Lisp_Vector

数组，一组固定大小的 Lisp 对象，可以通过索引访问。

    struct Lisp_Symbol

符号，通常用作标识符的唯一命名实体。

    struct Lisp_Float

浮点值。

这些类型是内部类型系统的一等公民。  由于标签空间有限，所有其他类型都是 Lisp\_Vectorlike 的子类型。  向量子类型由 enum pvec\_type 枚举，几乎所有复杂的对象，如窗口、缓冲区、帧和进程都属于这一类。

下面是 Lisp\_Vectorlike 的几个子类型的描述。  Buffer 对象表示要显示和编辑的文本。  窗口是显示结构的一部分，它显示缓冲区或用作容器以递归地将其他窗口放置在同一帧上。  （不要将 Emacs Lisp 窗口对象与作为 X 一样由用户界面系统管理的实体的窗口混淆；在 Emacs 术语中，后者称为框架。）最后，进程对象用于管理子进程。


<a id="orgb1116f9"></a>

### TODO E.9.1 缓冲器内部

两个结构（见 buffer.h）用于表示 C 中的缓冲区。 buffer\_text 结构包含描述缓冲区文本的字段；  缓冲区结构包含其他字段。  在间接缓冲区的情况下，两个或多个缓冲区结构引用相同的 buffer\_text 结构。

以下是 struct buffer\_text 中的一些字段：

    beg

缓冲区内容的地址。  缓冲区内容是一个线性 C 字符数组，中间有间隙。

    gpt

    gpt_byte

缓冲区间隙的字符和字节位置。  请参阅缓冲间隙。

    z

    z_byte

缓冲区文本结尾的字符和字节位置。

    gap_size

缓冲区间隙的大小。  请参阅缓冲间隙。

    modiff

    save_modiff

    chars_modiff

    overlay_modiff

这些字段计算在此缓冲区中执行的缓冲区修改事件的数量。  modiff 在每个缓冲区修改事件之后递增，并且永远不会更改；  save\_modiff 包含上次访问或保存缓冲区时的 modiff 值；  chars\_modiff 只计算对缓冲区中字符的修改，忽略所有其他类型的更改（例如文本属性）；  并且 overlay\_modiff 只计算对缓冲区覆盖的修改。

    beg_unchanged

    end_unchanged

自上次完全重新显示以来已知未更改的文本开头和结尾的字符数。

    unchanged_modified

    overlay_unchanged_modified

分别在最后一次完全重新显示之后的 modiff 和 overlay\_modiff 的值。  如果它们的当前值匹配 modiff 或 overlay\_modiff，则意味着 beg\_unchanged 和 end\_unchanged 不包含有用信息。

    markers

引用此缓冲区的标记。  这实际上是一个标记，其标记链（链表）中的连续元素是引用此缓冲区文本的其他标记。

    intervals

记录此缓冲区的文本属性的区间树。

struct buffer 的一些字段是：

    header

union vectorlike\_header 类型的标头对所有 vectorlike 对象都是通用的。

    own_text

通常保存缓冲区内容的 struct buffer\_text 结构。  在间接缓冲区中，不使用该字段。

    text

指向此缓冲区的 buffer\_text 结构的指针。  在普通缓冲区中，这是上面的 own\_text 字段。  在间接缓冲区中，这是基本缓冲区的 own\_text 字段。

    next

指向所有缓冲区链中下一个缓冲区的指针，包括终止缓冲区。  该链仅用于分配和垃圾收集，以便正确收集已终止的缓冲区。

    pt

    pt_byte

缓冲区中点的字符和字节位置。

    begv

    begv_byte

缓冲区中可访问文本范围开头的字符和字节位置。

    zv

    zv_byte

缓冲区中可访问文本范围末尾的字符和字节位置。

    base_buffer

在间接缓冲区中， this 指向基本缓冲区。  在普通缓冲区中，它为空。

    local_flags

此字段包含指示某些变量在此缓冲区中是本地的标志。  此类变量在 C 代码中使用 DEFVAR\_PER\_BUFFER 声明，并且它们的缓冲区本地绑定存储在缓冲区结构本身的字段中。  （此表中描述了其中一些字段。）

    modtime

被访问文件的修改时间。  它在文件被写入或读取时设置。  在将缓冲区写入文件之前，将该字段与文件的修改时间进行比较，以查看文件在磁盘上是否发生了变化。  请参阅缓冲区修改。

    auto_save_modified

上次自动保存缓冲区的时间。

    last_window_start

上次在窗口中显示缓冲区时缓冲区中的窗口开始位置。

    clip_changed

此标志指示缓冲区中的缩小已更改。  请参阅收窄。

    prevent_redisplay_optimizations_p

此标志指示不应使用重新显示优化来显示此缓冲区。

    inhibit_buffer_hooks

    此标志指示缓冲区不应运行钩子 kill-buffer-hook、kill-buffer-query-functions（请参阅 Killing Buffers）和 buffer-list-update-hook（请参阅缓冲区列表）。  它在缓冲区创建时设置（请参阅创建缓冲区），并避免减慢内部或临时缓冲区，例如由 with-temp-buffer 创建的缓冲区（请参阅当前缓冲区）。
覆盖中心

该字段保存当前的覆盖中心位置。  请参阅管理叠加。

    overlay_center

    overlays_before

这些字段分别保存在当前覆盖中心处或之前结束的覆盖列表，以及在当前覆盖中心之后结束的覆盖列表。  请参阅管理叠加。  overlays\_before 按结束位置递减顺序排序，overlays\_after 按起始位置递增顺序排序。

    overlays_after

一个命名缓冲区的 Lisp 字符串。  它保证是唯一的。  请参阅缓冲区名称。  此字段和以下字段在 C 结构定义中的名称以 \_ 结尾，表示不应直接访问它们，而应通过 BVAR 宏访问它们，如下所示：

Lisp\_Object buf\_name = BVAR（缓冲区，名称）；

    name

上次读取或保存时，此缓冲区正在访问的文件的长度。  它可以有 2 个特殊值：-1 表示在此缓冲区中关闭了自动保存，-2 表示如果缓冲区文本缩小很多，则不关闭自动保存。  这个和其他与保存有关的字段不会保存在 buffer\_text 结构中，因为从不保存间接缓冲区。

    save_length

扩展相对文件名的目录。  这是缓冲区局部变量 default-directory 的值（请参阅扩展文件名的函数）。

    directory

在此缓冲区中访问的文件的名称，或 nil。  这是缓冲区局部变量缓冲区文件名的值（请参阅缓冲区文件名）。

    filename

    undo_list

    backed_up

    auto_save_file_name

    auto_save_file_format

    read_only

    file_format

    file_truename

    invisibility_spec

    display_count

    display_time

这些字段存储自动本地缓冲区的 Lisp 变量的值（请参阅缓冲区本地变量），其对应的变量名称具有附加前缀 buffer- 并且下划线替换为破折号。  例如，undo\_list 存储 buffer-undo-list 的值。

    mark

缓冲区的标记。  该标记是一个标记，因此它也包含在列表标记中。  见标记。

    local_var_alist

描述此缓冲区的缓冲区局部变量绑定的关联列表，不包括缓冲区对象中具有特殊插槽的内置缓冲区局部绑定。  （此表中省略了这些插槽。）请参阅缓冲区局部变量。

    major_mode

命名此缓冲区的主要模式的符号，例如 lisp-mode。

    mode_name

主要模式的漂亮名称，例如“Lisp”。

    keymap

    abbrev_table

    syntax_table

    category_table

    display_table

这些字段存储缓冲区的本地键映射（请参阅键映射）、缩写表（请参阅缩写表）、语法表（请参阅语法表）、类别表（请参阅类别）和显示表（请参阅显示表）。

    downcase_table

    upcase_table

    case_canon_table

这些字段存储用于将文本转换为小写、大写以及规范化文本以进行大小写搜索的转换表。  请参阅案例表。

    minor_modes

此缓冲区的次要模式的列表。

    pt_marker

    begv_marker

    zv_marker

这些字段仅在间接缓冲区或作为间接缓冲区基础的缓冲区中使用。  每个缓冲区都有一个标记，当缓冲区不是当前缓冲区时，该标记分别记录该缓冲区的 pt、begv 和 zv。

    mode_line_format

    header_line_format

    case_fold_search

    tab_width

    fill_column

    left_margin

    auto_fill_function

    truncate_lines

    word_wrap

    ctl_arrow

    bidi_display_reordering

    bidi_paragraph_direction

    selective_display

    selective_display_ellipses

    overwrite_mode

    abbrev_mode

    mark_active

    enable_multibyte_characters

    buffer_file_coding_system

    cache_long_line_scans

    point_before_scroll

    left_fringe_width

    right_fringe_width

    fringes_outside_margins

    scroll_bar_width

    indicate_empty_lines

    indicate_buffer_boundaries

    fringe_indicator_alist

    fringe_cursor_alist

    scroll_up_aggressively

    scroll_down_aggressively

    cursor_type

    cursor_in_non_selected_windows

这些字段存储自动为缓冲区局部的 Lisp 变量的值（请参阅缓冲区局部变量），其对应的变量名称的下划线替换为破折号。  例如，mode\_line\_format 存储 mode-line-format 的值。

    last_selected_window

这是最后一个选择的带有此缓冲区的窗口，如果该窗口不再显示此缓冲区，则为 nil。


<a id="org7a88311"></a>

### TODO E.9.2 窗口内部

一个窗口的字段（完整列表见window.h中struct window的定义）包括：

    frame

此窗口所在的框架，作为 Lisp 对象。

    mini

如果此窗口是 minibuffer 窗口、显示 minibuffer 或回显区域的窗口，则非零。

    pseudo_window_p ¶

如果此窗口是伪窗口，则非零。  伪窗口是用于显示菜单栏或工具栏的窗口（当 Emacs 使用不显示自己的菜单栏和工具栏的工具包时）或选项卡栏或在工具提示框架上显示工具提示的窗口。  伪窗口通常不能从 Lisp 代码访问。

    parent

在内部，Emacs 将窗口排列成一棵树；  每组兄弟姐妹都有一个父窗口，其区域包括所有兄弟姐妹。  该字段指向该树中窗口的父级，作为 Lisp 对象。  对于树的根窗口和一个 minibuffer 窗口，它总是 nil。

父窗口不显示缓冲区，并且在显示中几乎没有作用，除了塑造它们的子窗口。  Emacs Lisp 程序不能直接操作父窗口；  它们在树的叶子上的窗口上操作，实际上显示缓冲区。

    contents

对于叶窗口和显示工具提示的窗口，这是作为 Lisp 对象的窗口正在显示的缓冲区。  对于内部（“父”）窗口，这是它的第一个子窗口。  对于显示菜单或工具栏的伪窗口，此值为 nil。  对于已删除的窗口，它也为零。

    next

    prev

此窗口的下一个和上一个兄弟姐妹作为 Lisp 对象。  如果窗口在其组中的最右边或最底部，则 next 为 nil；  如果 prev 在其组中位于最左侧或最顶部，则 prev 为 nil。  兄弟姐妹是左/右还是上/下由兄弟姐妹的父级的水平字段确定：如果它非零，则兄弟姐妹水平排列。

作为一种特殊情况，帧的根窗口的下一个指向该帧的 minibuffer 窗口，前提是这不是 minibuffer-only 或 minibuffer-less 帧。  在此类帧上，微型缓冲区窗口的 prev 指向该帧的根窗口。  在任何其他情况下，根窗口的 next 和 minibuffer 窗口（如果存在）的 prev 字段为零。

    left_col

窗口的左侧边缘，以列为单位，相对于窗口原生框架的最左侧列（第 0 列）。

    top_line

窗口的上边缘，以线为单位，相对于窗口的本机框架的最顶行（第 0 行）。

    pixel_left

    pixel_top

此窗口的左侧和顶部边缘，以像素为单位，相对于窗口原生框架的左上角 (0, 0)。

    total_cols

    total_lines

窗口的总宽度和高度，分别以列和行测量。  这些值包括滚动条和边缘、分隔线和/或窗口右侧的分隔线（如果有）。

    pixel_width;

    pixel_height;

以像素为单位测量的窗口的总宽度和高度。

    start

指向缓冲区中位置的标记，该位置是窗口中显示的第一个字符（按逻辑顺序，请参阅双向显示）。

    pointm ¶

这是选择此窗口时当前缓冲区中的点的值；  当它未被选中时，它保留其先前的值。

    old_pointm

上次重新显示时 pointm 的值。

    force_start

如果这个标志不为 nil，它表示窗口已经被 Lisp 程序显式滚动，并且窗口的 start 值被设置为重新显示以兑现。  如果点不在屏幕上，这会影响下一次重新显示的操作：不是滚动窗口以显示点周围的文本，而是将点移动到屏幕上的某个位置。

    optional_new_start

这与 force\_start 类似，但仅当点保持可见时，下一次重新显示才会服从它。

    start_at_line_beg

非 nil 表示 start 的当前值在选择时是行的开头。

    use_time

这是最后一次选择窗口。  函数 get-lru-window 使用该字段。

    sequence_number

创建此窗口时为其分配的唯一编号。

    last_modified

窗口缓冲区的 modiff 字段，截至上次在此窗口中完成的重新显示。

    last_overlay_modified

窗口缓冲区的 overlay\_modiff 字段，截至上次在此窗口中完成的重新显示。

    last_point

缓冲区的点值，截至上次在此窗口中完成的重新显示。

    last_had_star

非零值意味着窗口的缓冲区在窗口最后一次更新时被修改。

    vertical_scroll_bar_type

    horizontal_scroll_bar_type

此窗口的垂直和水平滚动条的类型。

    scroll_bar_width

    scroll_bar_height

此窗口的垂直滚动条的宽度和此窗口的水平滚动条的高度，以像素为单位。

    left_margin_cols

    right_margin_cols

此窗口中左右边距的宽度。  零值意味着没有保证金。

    left_fringe_width

    right_fringe_width

此窗口中左右条纹的像素宽度。  值 -1 表示使用框架的值。

    fringes_outside_margins

非零值表示显示边缘之外的边缘；  othersize 它们位于页边距和文本之间。

    window_end_pos

这计算为 z 减去窗口当前矩阵中最后一个字形的缓冲区位置。  该值仅在 window\_end\_valid 非零时有效。

    window_end_bytepos

window\_end\_pos 对应的字节位置。

    window_end_vpos

包含 window\_end\_pos 的行的相对于窗口的垂直位置。

    window_end_valid

如果 window\_end\_pos 和 window\_end\_vpos 确实有效，则该字段设置为非零值。  如果非平凡的重新显示被抢占，则该值为零，因为在这种情况下，计算 window\_end\_pos 的显示不会出现在屏幕上。

    cursor

描述光标在此窗口中位置的结构。

    last_cursor_vpos

自上次重新显示完成时显示光标的行的窗口相对垂直位置。

    phys_cursor

描述此窗口的光标物理位置的结构。

    phys_cursor_type

    phys_cursor_height

    phys_cursor_width

上次显示在此窗口上的光标的类型、高度和宽度。

    phys_cursor_on_p

如果光标在物理上，则此字段非零。

    cursor_off_p

非零表示此窗口中的光标在逻辑上是关闭的。  这用于闪烁光标。

    last_cursor_off_p

该字段包含上次重新显示时 cursor\_off\_p 的值。

    must_be_updated_p

当必须更新此窗口时，在重新显示期间将其设置为 1。

    hscroll

这是窗口中显示水平向左滚动的列数。  通常，这是 0。当只有当前行被 hscroll 时，这描述了当前行被滚动了多少。

    min_hscroll

hscroll 的最小值，由用户通过 set-window-hscroll 设置（参见水平滚动）。  当只有当前行被 hscrolled 时，这描述了当前行以外的行的水平滚动。

    vscroll

垂直滚动量，以像素为单位。  通常，这是 0。

    dedicated

如果此窗口专用于其缓冲区，则非零。

    combination_limit

此窗口的组合限制，仅对父窗口有意义。  如果这是 t，则不允许删除该窗口并将其子窗口与该窗口的其他兄弟窗口重新组合。

    window_parameters

此窗口的参数列表。

    display_table

窗口的显示表，如果没有指定，则为 nil。

    update_mode_line

非零表示此窗口的模式行需要更新。

    mode_line_height

    header_line_height

模式行和标题行的高度（以像素为单位），如果未知，则为 -1。

    base_line_number

缓冲区中某个位置的行号，或零。  这用于显示模式行中点的行号。

    base_line_pos

行号已知的缓冲区中的位置，或零表示不知道行号。  如果是 -1，只要窗口显示该缓冲区，就不要显示行号。

    column_number_displayed

当前显示在此窗口模式行中的列号，如果未显示列号，则为 -1。

    current_matrix

    desired_matrix

描述此窗口当前和所需显示的字形矩阵。


<a id="org315672b"></a>

### TODO E.9.3 过程内部

进程的字段（完整列表见 process.h 中 struct Lisp\_Process 的定义）包括：

    name

Lisp 字符串，进程的名称。

    command

包含用于启动此过程的命令参数的列表。  对于网络或串行进程，如果进程正在运行，则为 nil，如果进程停止，则为 t。

    filter

一个 Lisp 函数，用于接受进程的输出。

    sentinel

每当进程状态发生变化时调用的 Lisp 函数。

    buffer

进程的关联缓冲区。

    pid

一个整数，操作系统的进程 ID。  网络或串行连接等伪进程使用值 0。

    childp

一个标志，如果这真的是一个子进程，则为 t。  对于网络或串行连接，它是基于 make-network-process 或 make-serial-process 参数的 plist。

    mark

一个标记，指示插入缓冲区的此进程的最后一个输出的结束位置。  这通常但不总是缓冲区的结尾。

    kill_without_query

如果它不为零，则在该进程仍在运行时终止 Emacs 不会要求确认终止该进程。

    raw_status

原始进程状态，由等待系统调用返回。

    status

进程状态，因为 process-status 应该返回它。  这是一个 Lisp 符号、一个 cons 单元格或一个列表。

    tick

    update_tick

如果这两个字段不相等，则需要报告进程状态的变化，方法是运行哨兵或在进程缓冲区中插入消息。

    pty_flag

如果与子进程的通信使用 pty，则非零；  如果它使用管道，则为零。

    infd

来自进程的输入的文件描述符。

    outfd

输出到进程的文件描述符。

    tty_name

子进程正在使用的终端的名称，如果使用管道，则为 nil。

    decode_coding_system

用于解码来自该过程的输入的编码系统。

    decoding_buf

用于解码的工作缓冲区。

    decoding_carryover

解码中残留的大小。

    encode_coding_system

用于将输出编码到此过程的编码系统。

    encoding_buf

用于编码的工作缓冲区。

    inherit_coding_system_flag

从用于解码进程输出的编码系统中设置进程缓冲区的编码系统的标志。

    type

表示进程类型的符号：真实、网络、串行。


<a id="orge40762f"></a>

## TODO E.10 C 整数类型

以下是在 Emacs C 源代码中使用整数类型的一些指南。  这些指南有时会给出相互矛盾的建议；  建议使用常识。

-   避免任意限制。  例如，避免 int len = strlen (s);  除非出于其他原因需要 s 的长度以适应 int 范围。
-   不要假设有符号整数运算会在溢出时回绕。  这不再适用于 Emacs 移植目标：有符号整数溢出在实践中具有未定义的行为，并且可能会转储内核，甚至导致更早或更晚的代码行为不合逻辑。  无符号溢出确实可以可靠地环绕，以 2 的幂为模。
-   更喜欢有符号类型而不是无符号类型，因为当有符号和无符号类型组合时，代码会变得混乱。  许多其他指南假定类型已签名。  在需要无符号类型的极少数情况下，类似的建议可能适用于无符号对应项（例如，用 size\_t 代替 ptrdiff\_t，或用 uintptr\_t 代替 intptr\_t）。
-   对于 Emacs 字符代码，首选 int，范围为 0 .. 0x3FFFFF。  更一般地说，对于已知在 int 范围内的整数，例如屏幕列计数，更喜欢使用 int。
-   首选 ptrdiff\_t 作为大小，即，对于由任何单个 C 对象的最大大小或任何 C 数组中的最大元素数限制的整数。  这是 Emacs 对有符号类型的普遍偏好的一部分。  使用 ptrdiff\_t 将对象限制为 PTRDIFF\_MAX 字节，但较大的对象无论如何都会造成麻烦，因为它们会破坏指针减法，因此这不会施加任意限制。
-   避免使用 ssize\_t，除非与具有 ssize\_t 相关限制的低级 API 通信。  虽然它在典型平台上相当于 ptrdiff\_t，但 ssize\_t 偶尔会更窄，因此将其用于与大小相关的计算可能会溢出。  此外，ptrdiff\_t 更普遍且更标准化，具有标准的 printf 格式，并且是 Emacs 内部大小溢出检查的基础。  使用 ssize\_t 时，请注意 POSIX 仅需要支持 -1 .. SSIZE\_MAX 范围内的值。
-   通常，更喜欢 intptr\_t 用于指针的内部表示，或者用于仅由在任何给定时间可以存在的对象数或可以分配的字节总数限制的整数。  但是，更喜欢 uintptr\_t 来表示可以跨页边界的指针算法。  例如，在具有 32 位地址空间的机器上，数组可能会跨越 0x7fffffff/0x80000000 边界，当 (intptr\_t) 0x7fffffff 加 1 时会导致整数溢出。
-   首选 Emacs 定义的类型 EMACS\_INT 来表示转换为 Emacs Lisp fixnums 或从 Emacs Lisp fixnums 转换的值，因为 fixnum 算法基于 EMACS\_INT。
-   当表示系统值（例如文件大小或自 Epoch 以来的秒数）时，首选相应的系统类型（例如，off\_t、time\_t）。  不要假设系统类型已签名，除非已知此假设是安全的。  例如，虽然 off\_t 总是有符号的，但 time\_t 不需要。
-   首选 intmax\_t 来表示可能是任何有符号整数值的值。  printf 系列函数可以通过“%”PRIdMAX 之类的格式打印这样的值。
-   对于布尔值，首选 bool、false 和 true。  使用 bool 可以使程序更易于阅读，并且比使用 int 快一点。  虽然用int、0和1也可以，但是这种老风格正在逐渐被淘汰。  使用 bool 时，请遵守 bool 替换实现的限制，如源文件 lib/stdbool.in.h 中所述。  特别是，布尔位域应该是 bool\_bf 类型，而不是 bool 类型，这样即使在使用标准 GCC 编译 Objective C 时它们也能正常工作。
-   在位域中，比起 int，更喜欢 unsigned int 或 signed int，因为 int 的可移植性较差：它可能是有符号的，也可能不是。  单比特位字段应该是 unsigned int 或 bool\_bf 以便它们的值为 0 或 1。

