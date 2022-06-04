# 18 Lisp编译成Native代码

除了上一章中描述的字节编译之外，Emacs还可以选择性地将Lisp函数定义编译成真正的编译代码，称为 ~ 本地代码~ 。此功能使用 `libgccjit`  库，它是GCC发行版的一部分，并且要求构建Emacs时支持使用该库。它还需要在您的系统上安装GCC和Binutils（汇编器和链接器），以便您能够本地编译Lisp代码。

要确定当前Emacs进程是否可以生成和加载本机编译的Lisp代码，请调用 `native-comp-available-p`  （请参阅 ~Native-Compilation Functions ）。

与字节编译的代码不同，本机编译的Lisp代码直接由机器硬件执行，因此以主机CPU可以提供的全速运行。最终的加速通常取决于Lisp代码的作用，但通常比相应的字节编译代码快2.5到5倍。

由于本机代码通常在不同系统之间不兼容，本机编译的代码不能从一台机器传输到另一台机器，它只能在产生它的同一台机器上或非常相似的机器上使用（具有相同的CPU和运行时库）。本机编译代码的可传输性与共享库（.so 或 `.dll` 文件）的可传输性相同。

本机编译代码的库包括对Emacs Lisp原语的关键依赖项（请参阅什么是函数？）及其调用约定，因此Emacs通常不会加载由早期或更高版本的Emacs生成的本机编译代码；不同Emacs版本对相同Lisp代码的本地编译通常会以唯一文件名生成一个本地编译库，只有该版本的Emacs才能加载该库。然而，使用唯一文件名允许在同一个目录中拥有由几个不同版本的Emacs本地编译的同一个Lisp库的多个版本。

`no-byte-compile` 的非 `nil`  文件局部变量绑定（请参阅字节编译）也会禁用该文件的本机编译。此外，类似的变量 `no-native-compile`  仅禁用文件的本机编译。如果同时指定了 `no-byte-compile`  和 `no-native-compile`  ，则前者优先。


<a id="orgb77a97b"></a>

## 18.1 本机编译函数

Native-Compilation是作为字节编译的副作用实现的（请参阅字节编译）。因此，以本机方式编译Lisp代码也总是生成其字节码，因此为字节编译准备Lisp代码的所有规则和注意事项（请参阅字节编译函数）也适用于本机编译。

您可以使用 `native-compile`  函数本地编译单个函数或宏定义，或整个Lisp代码文件。本机编译文件将生成相应的带有字节码的.elc文件和带有本机代码的.eln文件。

本机编译可能会产生警告或错误消息； `这` 些通常记录在名为 `*Native-compile-Log*`  的缓冲区中。在交互式会话中，它使用特殊的LIMPLE模式（ `native-comp-limple-mode`  ），该模式会根据该日志设置font-lock，否则与 `Fundamental` 模式相同。本地编译产生的消息的日志记录可以由 `native-comp-verbose`  变量控制（请参阅 `Native-Compilation`  变量）。

当Emacs以非交互方式运行时，本机编译产生的消息通过调用message报告（请参阅在Echo区域显示消息），并且通常显示在调用Emacs的终端的标准错误流上。

    Function: native-compile function-or-file &optional output ¶

此函数将函数或文件编译为本机代码。参数 `function-or-file`  可以是函数符号、Lisp 形式或包含要编译的Emacs Lisp源代码的文件的名称（字符串）。如果提供了可选参数输出，它必须是一个字符串，指定要写入编译代码的文件的名称。否则，如果 `function-or-file`  是函数或 `Lisp` 形式，则此函数返回编译后的对象，如果 `function-or-file`  是文件名，则函数返回它为编译后创建的文件的完整绝对名称代码。默认情况下，输出文件的扩展名为.eln。

该函数在一个单独的子进程中运行本机编译的最后阶段，通过 `libgccjit`  调用 `GCC` ，该子进程调用与调用该函数的进程相同的Emacs可执行文件。

    Function: batch-native-compile &optional for-tarball ¶

此函数以批处理模式对Emacs命令行上指定的文件运行本机编译。它只能在Emacs的批处理执行中使用，因为它会在编译完成时杀死Emacs。如果一个或多个文件编译失败，Emacs进程将尝试编译所有其他文件，并以非零状态码终止。tarball的可选参数，如果不是 `nil`  ，则告诉函数将生成的.eln文件放在 `native-comp-eln-load-path`  中提到的最后一个目录中（请参阅库搜索）； `这` 是第一次用作构建Emacs源tarball的一部分，当源tarball中不存在的本机编译文件应该在构建树而不是用户的缓存目录中生成时。

本机编译可以完全异步运行，在主Emacs进程的子进程中。这使得主Emacs进程可以在编译在后台运行时自由使用。这是Emacs在没有可用的本机编译文件时对加载到Emacs的任何Lisp文件或字节编译的Lisp文件进行本机编译的方法。请注意，由于使用了子进程，本机编译可能会产生字节编译不会产生的警告和错误，因此可能需要修改Lisp代码才能正常工作。有关更多详细信息，请参阅本地编译变量中的 `native-comp-async-report-warnings-errors`  。

    Function: native-compile-async files &optional recursively load selector ¶

此函数异步编译命名文件。参数文件应该是单个文件名（字符串）或一个或多个文件和/或目录名称的列表。如果列表中存在目录，则可选参数递归地应为非零，以使编译递归到这些目录中。如果 `load` 不为零，Emacs将加载它成功编译的每个文件。可选参数选择器允许控制将编译哪些文件； ~~ 它可以具有以下值之一：

    nil or omitted

选择文件中的所有文件和目录。

    a regular expression string

选择名称与正则表达式匹配的文件和目录。

    a function

一个谓词函数，它将与files中的每个文件和目录一起调用，如果应该选择文件或目录进行编译，则应该返回非 `nil`  。

在具有多个 `CPU` 执行单元的系统上，当文件命名多个文件时，此函数通常会在 `native-comp-async-jobs-number`  的控制下并行启动多个编译子进程（参见 `Native-Compilation Variables`  ）。

下面的函数允许Lisp程序在运行时测试本地编译是否可用。

    Function: native-comp-available-p ¶

如果正在运行的Emacs进程已编译了本机编译支持，则此函数返回非 `nil`  。在动态加载 `libgccjit`  的系统上，它还确保库可用并且可以加载。需要预先知道本机编译是否可用的Lisp程序应该使用这个谓词。


<a id="org9ffcba4"></a>

## 18.2 本机编译变量

本节记录了控制本机编译的变量。

    User Option: native-comp-speed ¶

此变量指定本机编译的优化级别。它的值应该是介于-1和3之间的一个数字。介于0和3之间的值指定与编译器的相应编译器 `-O0` 、-O1 等命令行选项等效的优化级别。值-1表示禁用本机编译； `函` 数和文件将仅进行字节编译。默认值为 `2` 。

    User Option: native-comp-debug ¶

此变量指定本机编译产生的调试信息级别。它的值应该是一个介于0和3之间的数字，含义如下：

    0

没有调试输出。这是默认设置。

    1

使用本机代码发出调试符号。这允许使用gdb等调试器更轻松地调试本机代码。

    2

像1，另外转储伪C代码。

    3

像2，另外转储GCC中间通道和 `libgccjit`  日志文件。

    User Option: native-comp-verbose ¶

此变量通过抑制其发出的部分或全部日志消息来控制本机编译的详细程度。如果它的值为零，默认情况下，所有日志消息都被抑制。将其设置为1到3之间的值将允许记录其级别高于该值的消息。这些值具有以下解释：

    0

没有记录。这是默认设置。

    1

记录代码的最终LIMPLE表示。

    2

记录 `LAP` 、最后的LIMPLE和一些额外的通行证信息。

    3

最大冗长：记录所有内容。

    User Option: native-comp-async-jobs-number ¶

此变量确定将同时启动的本机编译子进程的最大数量。它应该是一个非负数。默认值为0，表示使用CPU执行单元数的一半，如果CPU只有一个执行单元，则为1。

    User Option: native-comp-async-report-warnings-errors ¶

如果此变量的值为非零，则来自异步本机编译子进程的警告和错误将在名为 `*Warnings*`  的缓冲区中的主Emacs会话中报告。默认值 `t`  表示显示结果缓冲区。要在不弹出 `*Warnings*`  缓冲区的情况下记录警告，请将此变量设置为静默。

异步本机编译产生警告的一个常见原因是编译缺少某些必要功能要求的文件。该功能可能会加载到主Emacs中，但由于本机编译总是从具有原始环境的子进程开始，因此子进程可能并非如此。

    User Option: native-comp-async-query-on-exit ¶

如果该变量的值为非 `nil`  ，Emacs 将在退出时询问是否退出并杀死任何仍在运行的异步原生编译子进程，从而阻止写入相应的.eln文件。如果值为 `nil` ，默认值，Emacs将杀死这些子进程而不进行查询。

