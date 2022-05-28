
# Table of Contents

1.  [20 阅读和打印 Lisp 对象](#orgddffb1e)
    1.  [20.1 阅读与打印简介](#org3b26653)
    2.  [20.2 输入流](#org2d45b49)
    3.  [20.3 输入函数](#orgeab99b9)
    4.  [20.4 输出流](#orge7e7745)
    5.  [20.5 输出函数](#org6a28914)
    6.  [20.6 影响输出的变量](#org88668e8)


<a id="orgddffb1e"></a>

# TODO 20 阅读和打印 Lisp 对象

打印和阅读是将 Lisp 对象转换为文本形式的操作，反之亦然。  他们使用 Lisp 数据类型中描述的打印表示和阅读语法。

本章描述了用于阅读和打印的 Lisp 函数。  它还描述了流，它指定从哪里获取文本（如果阅读）或放置它（如果打印）。


<a id="org3b26653"></a>

## TODO 20.1 阅读与打印简介

读取 Lisp 对象意味着以文本形式解析 Lisp 表达式并生成相应的 Lisp 对象。  这就是 Lisp 程序从 Lisp 代码文件进入 Lisp 的方式。  我们将文本称为对象的读取语法。  例如，文本 '(a . 5)' 是 CAR 为 a 且 CDR 为数字 5 的 cons 单元格的读取语法。

打印 Lisp 对象意味着生成表示该对象的文本——将对象转换为其打印表示（请参阅打印表示和读取语法）。  打印上述 cons 单元格会生成文本“(a . 5)”。

读取和打印或多或少是逆操作：打印读取给定文本的对象通常会生成相同的文本，而读取打印对象的文本通常会生成外观相似的对象。  例如，打印符号 foo 会生成文本“foo”，读取该文本会返回符号 foo。  打印元素为 a 和 b 的列表会生成文本“(ab)”，读取该文本会生成包含元素 a 和 b 的列表（但不是同一个列表）。

然而，这两个操作并不完全相反。  有以下三种例外情况：

打印会产生无法阅读的文本。  例如，缓冲区、窗口、框架、子进程和标记打印为以“#”开头的文本；  如果您尝试阅读此文本，则会收到错误消息。  没有办法读取这些数据类型。
一个对象可以有多个文本表示。  例如，'1' 和 '01' 代表同一个整数，而 '(ab)' 和 '(a . (b))' 代表同一个列表。  阅读将接受任何替代方案，但印刷必须选择其中之一。
注释可以出现在对象读取序列中间的某些点，而不会影响读取结果。


<a id="org2d45b49"></a>

## TODO 20.2 输入流

大多数用于读取文本的 Lisp 函数都将输入流作为参数。  输入流指定从何处或如何获取要读取的文本字符。  以下是可能的输入流类型：

    buffer ¶

输入字符从缓冲区中读取，从点之后的字符开始。  读取字符时点前进。

    marker ¶

输入字符从标记所在的缓冲区中读取，从标记之后的字符开始。  标记位置随着字符的读取而前进。  当流是标记时，缓冲区中的点值无效。

    string ¶

输入字符取自字符串，从字符串中的第一个字符开始，并根据需要使用尽可能多的字符。

    function ¶

输入字符由函数生成，必须支持两种调用：

当不带参数调用它时，它应该返回下一个字符。
当使用一个参数（总是一个字符）调用它时，函数应该保存参数并安排在下一次调用时返回它。  这称为未读字符；  当 Lisp 阅读器读取一个字符过多并想将其放回原处时，就会发生这种情况。  在这种情况下，值函数返回什么没有区别。

    t ¶

t 用作流意味着从 minibuffer 中读取输入。  事实上，minibuffer 被调用一次，用户给出的文本被制成一个字符串，然后用作输入流。  如果 Emacs 以批处理模式运行（请参阅批处理模式），则使用标准输入而不是 minibuffer。  例如，

    (message "%s" (read t))

将以批处理模式从标准输入读取 Lisp 表达式并将结果打印到标准输出。

    nil ¶

nil 作为输入流提供意味着使用标准输入的值来代替；  该值是默认输入流，并且必须是非零输入流。

    symbol

作为输入流的符号等效于符号的函数定义（如果有）。

这是一个从作为缓冲区的流中读取的示例，显示了点在之前和之后的位置：

    ---------- Buffer: foo ----------
    This∗ is the contents of foo.
    ---------- Buffer: foo ----------
    
    
    (read (get-buffer "foo"))
         ⇒ is
    
    (read (get-buffer "foo"))
         ⇒ the
    
    
    ---------- Buffer: foo ----------
    This is the∗ contents of foo.
    ---------- Buffer: foo ----------

请注意，第一次读取会跳过一个空格。  阅读会跳过重要文本之前的任何数量的空白。

这是一个从作为标记的流中读取的示例，最初位于所示缓冲区的开头。  读取的值是符号This。

    ---------- Buffer: foo ----------
    This is the contents of foo.
    ---------- Buffer: foo ----------
    
    
    (setq m (set-marker (make-marker) 1 (get-buffer "foo")))
         ⇒ #<marker at 1 in foo>
    
    (read m)
         ⇒ This
    
    m
         ⇒ #<marker at 5 in foo>   ;; Before the first space.

这里我们从一个字符串的内容中读取：

    (read "(When in) the course")
         ⇒ (When in)

以下示例从 minibuffer 中读取。  提示是：'Lisp 表达式：'。  （这始终是您从流 t 中读取时使用的提示。）用户的输入显示在提示之后。

    (read t)
         ⇒ 23
    ---------- Buffer: Minibuffer ----------
    Lisp expression: 23 RET
    ---------- Buffer: Minibuffer ----------

最后，这是一个名为 useless-stream 的函数流示例。  在我们使用流之前，我们将变量 useless-list 初始化为一个字符列表。  然后对函数 useless-stream 的每次调用都会获取列表中的下一个字符，或者通过将一个字符添加到列表的前面来取消读取该字符。

    
    
    (setq useless-list (append "XY()" nil))
         ⇒ (88 89 40 41)
    
    
    (defun useless-stream (&optional unread)
      (if unread
          (setq useless-list (cons unread useless-list))
        (prog1 (car useless-list)
    	   (setq useless-list (cdr useless-list)))))
         ⇒ useless-stream

现在我们使用这样构造的流来读取：

    (read 'useless-stream)
         ⇒ XY
    
    
    useless-list
         ⇒ (40 41)

请注意，左括号和右括号仍保留在列表中。  Lisp 阅读器遇到了左括号，决定结束输入，然后取消阅读。  此时从流中读取的另一次尝试将读取 '()' 并返回 nil。


<a id="orgeab99b9"></a>

## TODO 20.3 输入函数

本节介绍与阅读有关的 Lisp 函数和变量。

在下面的函数中，stream 代表输入流（参见上一节）。  如果 stream 为 nil 或省略，则默认为标准输入的值。

如果读取遇到未终止的列表、向量或字符串，则会发出文件结束错误信号。

    Function: read &optional stream ¶

此函数从流中读取一个文本 Lisp 表达式，并将其作为 Lisp 对象返回。  这是基本的 Lisp 输入功能。

    Function: read-from-string string &optional start end ¶

此函数从字符串中的文本中读取第一个文本 Lisp 表达式。  它返回一个 cons 单元格，其 CAR 是该表达式，其 CDR 是一个整数，给出字符串中下一个剩余字符的位置（即第一个未读取的字符）。

如果提供了 start，则从字符串中的索引 start 开始读取（第一个字符位于索引 0 处）。  如果您指定 end，则读取将在该索引之前强制停止，就好像字符串的其余部分不存在一样。

例如：

    
    
    (read-from-string "(setq x 55) (setq y 5)")
         ⇒ ((setq x 55) . 11)
    
    (read-from-string "\"A short string\"")
         ⇒ ("A short string" . 16)
    
    
    ;; Read starting at the first character.
    (read-from-string "(list 112)" 0)
         ⇒ ((list 112) . 10)
    
    ;; Read starting at the second character.
    (read-from-string "(list 112)" 1)
         ⇒ (list . 5)
    
    ;; Read starting at the seventh character,
    ;;   and stopping at the ninth.
    (read-from-string "(list 112)" 6 8)
         ⇒ (11 . 8)

    Variable: standard-input ¶

此变量保存默认输入流——当流参数为 nil 时读取使用的流。  默认值为 t，表示使用 minibuffer。

    Variable: read-circle ¶

如果非零，则此变量启用循环和共享结构的读取。  请参阅循环对象的读取语法。  它的默认值为 t。

在批处理模式下从 Emacs 进程的标准输入/输出流读取或写入时，有时需要确保将逐字读取/写入任意二进制数据，和/或不转换换行符到 CR-执行 LF 对。  此问题在 POSIX 主机上不存在，仅在 MS-Windows 和 MS-DOS 上存在。  以下函数允许您控制 Emacs 进程的任何标准流的 I/O 模式。

    Function: set-binary-mode stream mode ¶

将流切换到二进制或文本 I/O 模式。  如果模式为非零，则切换到二进制模式，否则切换到文本模式。  stream 的值可以是标准输入、标准输出或标准错误之一。  此函数会刷新流的任何未决输出数据作为副作用，并返回流的 I/O 模式的先前值。  在 POSIX 主机上，它总是返回一个非零值并且除了刷新挂起的输出之外什么都不做。


<a id="orge7e7745"></a>

## TODO 20.4 输出流

输出流指定如何处理打印产生的字符。  大多数打印函数接受输出流作为可选参数。  以下是可能的输出流类型：

    buffer ¶

输出字符被插入到缓冲区中。  插入字符时点前进。

    marker ¶

输出字符被插入到标记指向的缓冲区中，在标记位置。  标记位置随着字符的插入而前进。  当流为标记时，缓冲区中point的值对打印没有影响，并且这种打印不会移动点（除非标记指向点的位置或之前，点会随着周围的文本前进，照常）。

    function ¶

输出字符被传递给函数，该函数负责将它们存储起来。  它以单个字符作为参数调用，与要输出的字符一样多次，并负责将字符存储在您想要放置它们的任何位置。

    t ¶

输出字符显示在回显区域中。  如果 Emacs 以批处理模式运行（请参阅批处理模式），则输出将改为写入标准输出描述符。

    nil ¶

nil 指定为输出流意味着使用标准输出变量的值；  该值是默认输出流，并且不能为 nil。

    symbol

作为输出流的符号等效于符号的函数定义（如果有）。

许多有效的输出流也可以作为输入流有效。  因此，输入和输出流之间的区别更多地在于您如何使用 Lisp 对象，而不是不同类型的对象。

这是用作输出流的缓冲区的示例。  点最初位于“the”中的“h”之前，如图所示。  最后，点位于同一个“h”之前。

    ---------- Buffer: foo ----------
    This is t∗he contents of foo.
    ---------- Buffer: foo ----------
    
    
    (print "This is the output" (get-buffer "foo"))
         ⇒ "This is the output"
    
    ---------- Buffer: foo ----------
    This is t
    "This is the output"
    ∗he contents of foo.
    ---------- Buffer: foo ----------

现在我们展示了如何使用标记作为输出流。  最初，标记位于缓冲区 foo 中，位于单词 'the' 中的 't' 和 'h' 之间。  最后，标记已经超过了插入的文本，因此它仍然位于相同的“h”之前。  请注意，以通常方式显示的点的位置没有效果。

    
    
    ---------- Buffer: foo ----------
    This is the ∗output
    ---------- Buffer: foo ----------
    
    
    (setq m (copy-marker 10))
         ⇒ #<marker at 10 in foo>
    
    
    (print "More output for foo." m)
         ⇒ "More output for foo."
    
    
    ---------- Buffer: foo ----------
    This is t
    "More output for foo."
    he ∗output
    ---------- Buffer: foo ----------
    
    
    m
         ⇒ #<marker at 34 in foo>

以下示例显示了回显区域的输出：

    (print "Echo Area output" t)
         ⇒ "Echo Area output"
    ---------- Echo Area ----------
    "Echo Area output"
    ---------- Echo Area ----------

最后输出

    (setq last-output nil)
         ⇒ nil
    
    
    (defun eat-output (c)
      (setq last-output (cons c last-output)))
         ⇒ eat-output
    
    
    (print "This is the output" #'eat-output)
         ⇒ "This is the output"
    
    
    last-output
         ⇒ (10 34 116 117 112 116 117 111 32 101 104
        116 32 115 105 32 115 105 104 84 34 10)

现在我们可以通过反转列表来将输出按正确的顺序排列：

    (concat (nreverse last-output))
         ⇒ "
    \"This is the output\"
    "

调用 concat 会将列表转换为字符串，以便您可以更清楚地看到其内容。

    Function: external-debugging-output character ¶

在调试时，此函数可用作输出流。  它将字符写入标准错误流。

例如

    (print "This is the output" #'external-debugging-output)
    -| This is the output
    ⇒ "This is the output"


<a id="org6a28914"></a>

## TODO 20.5 输出函数

本节描述了用于打印 Lisp 对象的 Lisp 函数——将对象转换为它们的打印表示。

一些 Emacs 打印功能在必要时会在输出中添加引号字符，以便可以正确读取。  使用的引用字符是 '"' 和 '\\'；它们将字符串与符号区分开来，并防止在读取时将字符串和符号中的标点符号作为分隔符。有关完整详细信息，请参阅印刷表示和读取语法。您指定引用或没有引用打印功能的选择。

如果要将文本读回 Lisp，则应使用引号字符打印以避免歧义。  同样，如果目的是为 Lisp 程序员清楚地描述 Lisp 对象。  但是，如果输出的目的是为了让人类看起来不错，那么通常最好在不引用的情况下打印。

Lisp 对象可以引用自己。  以正常方式打印自引用对象将需要无限量的文本，并且尝试可能会导致无限递归。  Emacs 检测到这种递归并打印 '#level' 而不是递归打印已经打印的对象。  例如，这里的 '#0' 表示对当前打印操作级别 0 的对象的递归引用：

    (setq foo (list nil))
         ⇒ (nil)
    (setcar foo foo)
         ⇒ (#0)

在下面的函数中，stream 代表输出流。  （有关输出流的描述，请参见上一节。另请参见 external-debugging-output，这是一个对调试有用的流值。）如果 stream 为 nil 或省略，则默认为标准输出的值。

    Function: print object &optional stream ¶

打印功能是一种方便的打印方式。  它将对象的打印表示输出到流中，在对象之前打印一个换行符，在它之后打印另一个换行符。  使用引号字符。  打印返回对象。  例如：

    (progn (print 'The\ cat\ in)
           (print "the hat")
           (print " came back"))
         -|
         -| The\ cat\ in
         -|
         -| "the hat"
         -|
         -| " came back"
         ⇒ " came back"

    Function: prin1 object &optional stream ¶

此函数将对象的打印表示输出到流。  它不像 print 那样打印换行符来分隔输出，但它确实像 print 一样使用引号字符。  它返回对象。

    (progn (prin1 'The\ cat\ in)
           (prin1 "the hat")
           (prin1 " came back"))
         -| The\ cat\ in"the hat"" came back"
         ⇒ " came back"

    Function: princ object &optional stream ¶

此函数将对象的打印表示输出到流。  它返回对象。

此函数旨在生成人们可读的输出，而不是通过阅读，因此它不会插入引号字符，也不会在字符串内容周围放置双引号。  它不会在调用之间添加任何间距。

    (progn
      (princ 'The\ cat)
      (princ " in the \"hat\""))
         -| The cat in the "hat"
         ⇒ " in the \"hat\""

    Function: terpri &optional stream ensure ¶

此函数输出换行符以进行流式传输。  该名称代表“终止打印”。  如果 ensure 不为零，则如果流已经在行首，则不打印换行符。  请注意，在这种情况下，流不能是函数，如果是，则会发出错误信号。  如果打印了换行符，此函数返回 t。

    Function: write-char character &optional stream ¶

此函数将字符输出到流。  它返回字符。

    Function: prin1-to-string object &optional noescape ¶

此函数返回一个字符串，其中包含 prin1 为相同参数打印的文本。

    (prin1-to-string 'foo)
         ⇒ "foo"
    
    (prin1-to-string (mark-marker))
         ⇒ "#<marker at 2773 in strings.texi>"

如果 noescape 不为零，则禁止在输出中使用引号字符。  （此参数在 Emacs 版本 19 及更高版本中受支持。）

    (prin1-to-string "foo")
         ⇒ "\"foo\""
    
    (prin1-to-string "foo" t)
         ⇒ "foo"

有关将 Lisp 对象的打印表示形式获取为字符串的其他方法，请参见格式化字符串中的格式。

    Macro: with-output-to-string body… ¶

此宏执行带有标准输出设置的正文表单，以将输出输入字符串。  然后它返回该字符串。

例如，如果当前缓冲区名称是 'foo'，

    (with-output-to-string
      (princ "The buffer is ")
      (princ (buffer-name)))

返回“缓冲区是 foo”。

    Function: pp object &optional stream ¶

该函数将对象输出到流中，就像 prin1 一样，但以更漂亮的方式执行。  也就是说，它会缩进并填充对象以使其对人类更具可读性。

如果您需要在批处理模式下使用二进制 I/O，例如，使用本节中描述的函数写出任意二进制数据或避免在非 POSIX 主机上转换换行符，请参阅 set-binary-mode。


<a id="org88668e8"></a>

## TODO 20.6 影响输出的变量

    Variable: standard-output ¶

此变量的值是默认输出流——当流参数为 nil 时打印函数使用的流。  默认为 t，表示在回显区域显示。

    Variable: print-quoted ¶

如果这是非零，这意味着使用缩写的阅读器语法打印引用的形式，例如，(quote foo) 打印为 'foo，并且 (function foo) 打印为 #'foo。  默认值为 t。

    Variable: print-escape-newlines ¶

如果此变量不为 nil，则字符串中的换行符将打印为 '\n'，而换页符将打印为 '\f'。  通常这些字符打印为实际的换行符和换页符。

此变量影响打印函数 prin1 和 print 带引号的打印。  它不影响princ。  下面是一个使用prin1的例子：

    (prin1 "a\nb")
         -| "a
         -| b"
         ⇒ "a
    b"
    
    
    (let ((print-escape-newlines t))
      (prin1 "a\nb"))
         -| "a\nb"
         ⇒ "a
    b"

在第二个表达式中，print-escape-newlines 的本地绑定在调用 prin1 期间有效，但在打印结果期间无效。

    Variable: print-escape-control-characters ¶

如果此变量为非零，则字符串中的控制字符将由打印函数 prin1 打印为反斜杠序列，并打印带有引号的打印。  如果此变量和 print-escape-newlines 都不是 nil，则后者优先于换行符和换页符。

    Variable: print-escape-nonascii ¶

如果此变量为非零，则字符串中的单字节非 ASCII 字符将由打印函数 prin1 无条件地打印为反斜杠序列，并打印带引号的打印。

当输出流是多字节缓冲区或指向缓冲区的标记时，这些函数还对单字节非 ASCII 字符使用反斜杠序列，无论此变量的值如何。

    Variable: print-escape-multibyte ¶

如果此变量为非零，则字符串中的多字节非 ASCII 字符将由打印函数 prin1 无条件地打印为反斜杠序列，并打印带引号的打印。

当输出流是单字节缓冲区或指向缓冲区的标记时，这些函数还对多字节非 ASCII 字符使用反斜杠序列，而不管此变量的值。

    Variable: print-charset-text-property ¶

此变量控制打印字符串时“charset”文本属性的打印。  该值应为 nil、t 或默认值。

如果值为 nil，则永远不会打印字符集文本属性。  如果 t，它们总是被打印出来。

如果值为默认值，则仅在存在“意外”字符集属性时才打印字符集文本属性。  对于 ascii 字符，所有字符集都被认为是“预期的”。  否则，字符的预期 charset 属性由 char-charset 给出。

    Variable: print-length ¶

此变量的值是要在任何列表、向量或布尔向量中打印的最大元素数。  如果要打印的对象的元素多于这么多，则用省略号缩写。

如果该值为 nil（默认值），则没有限制。

    (setq print-length 2)
         ⇒ 2
    
    (print '(1 2 3 4 5))
         -| (1 2 ...)
         ⇒ (1 2 ...)

    Variable: print-level ¶

此变量的值是打印时括号和括号的最大嵌套深度。  深度超过此限制的任何列表或向量都用省略号缩写。  nil 值（默认值）表示没有限制。

    User Option: eval-expression-print-length ¶

    User Option: eval-expression-print-level ¶

这些是 eval-expression 使用的 print-length 和 print-level 的值，因此间接地被许多交互式评估命令所使用（请参阅 The GNU Emacs Manual 中的 Evaluating Emacs Lisp Expressions）。

这些变量用于检测和报告循环和共享结构：

    Variable: print-circle ¶

如果非零，则此变量可以检测打印中的循环和共享结构。  请参阅循环对象的读取语法。

    Variable: print-gensym ¶

如果非零，则此变量启用在打印中检测非驻留符号（请参阅创建和驻留符号）。  启用此功能后，非驻留符号会以前缀 '#:' 打印，这会告诉 Lisp 阅读器生成一个非驻留符号。

    Variable: print-continuous-numbering ¶

如果非零，这意味着在打印调用中连续编号。  这会影响为“#n=”标签和“#m#”引用打印的数字。  不要用 setq 设置这个变量；  你应该只用 let 将它临时绑定到 t。  当你这样做时，你还应该将 print-number-table 绑定到 nil。

    Variable: print-number-table ¶

这个变量保存了一个打印内部使用的向量，以实现打印圈功能。  除非在绑定 print-continuous-numbering 时将其绑定到 nil，否则不应使用它。

    Variable: float-output-format ¶

此变量指定如何打印浮点数。  默认值为 nil，这意味着使用代表数字的最短输出而不会丢失信息。

要更精确地控制输出格式，您可以在此变量中放置一个字符串。  该字符串应包含要在 C 函数 sprintf 中使用的“%”规范。  有关您可以使用的更多限制，请参阅变量的文档字符串。

    Variable: print-integers-as-characters ¶

当此变量为非零时，表示图形基本字符的整数将使用 Lisp 字符语法打印（请参阅基本字符语法）。  其他数字以通常的方式打印。  例如，列表 (4 65 -1 10) 将打印为 '(4 ?A -1 ?\n)'。

更准确地说，以字符语法打印的值是那些表示属于 Unicode 通用类别字母、数字、标点符号、符号和私人使用的字符的值（请参阅字符属性），以及具有自己的转义语法的控制字符，例如换行符。

