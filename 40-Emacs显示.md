# 40 Emacs显示

本章描述了许多与Emacs呈现给用户的显示相关的特性。



## 40.1 刷新屏幕

函数 ~redraw-frame ~ 清除并重新显示给定帧的全部内容（请参阅帧）。如果屏幕损坏，这很有用。

    Function: redraw-frame &optional frame ¶

该函数清除并重新显示帧。如果 `frame` 被省略或为零，它会重绘选定的帧。

更强大的是重绘显示：

    Command: redraw-display ¶

此函数清除并重新显示所有可见帧。

在Emacs中，处理用户输入优先于重新显示。如果在输入可用时调用这些函数，它们不会立即重新显示，但请求的重新显示最终会发生——在所有输入都已处理之后。

在文本终端上，挂起和恢复Emacs通常也会刷新屏幕。一些终端仿真器为Emacs等面向显示的程序和普通的顺序显示记录单独的内容。如果您正在使用这样的终端，您可能希望在恢复时禁止重新显示。

    User Option: no-redraw-on-reenter ¶

此变量控制Emacs在暂停和恢复后是否重绘整个屏幕。Non-nil 表示不需要重绘，nil 表示需要重绘。默认值为无。



## 40.2 强制重新显示

Emacs 通常会在等待输入时尝试重新显示屏幕。使用以下函数，您可以在 `Lisp` 代码中间请求立即尝试重新显示，而无需实际等待输入。

    Function: redisplay &optional force ¶

此函数会立即尝试重新显示。可选参数 `force` ，如果非零，则强制执行重新显示，而不是在输入未决时被抢占。

如果函数实际尝试重新显示，则返回 `t` ，否则返回 `nil` 。t 值并不意味着重新显示已完成；它可能已被新到达的输入抢占。

尽管 `redisplay` 会立即尝试重新显示，但它不会改变Emacs决定重新显示其帧的哪些部分的方式。相比之下，以下函数将某些窗口添加到挂起的重新显示工作（好像它们的内容已经完全改变），但不会立即尝试执行重新显示。

    Function: force-window-update &optional object ¶

此函数强制在Emacs下一次重新显示时更新部分或所有窗口。如果对象是一个窗口，则该窗口将被更新。如果 `object` 是缓冲区或缓冲区名称，则显示该缓冲区的所有窗口都将被更新。如果 `object` 为 `nil` （或省略），则所有窗口都将被更新。

此功能不会立即重新显示； `Emacs` 在等待输入或调用函数 `redisplay` 时会这样做。

    Variable: pre-redisplay-function ¶

一个函数在重新显示之前运行。它使用一个参数调用，即要重新显示的一组窗口。该集合可以为 `nil` ，表示仅选定的窗口，或 `t` ，表示所有窗口。

    Variable: pre-redisplay-functions ¶

这个钩子在重新显示之前运行。它在即将重新显示的每个窗口中调用一次，当前缓冲区设置为该窗口中显示的缓冲区。



## 40.3 截断

当一行文本超出窗口的右边缘时，Emacs 可以继续该行（使其换行到下一个屏幕行），或截断该行（将其限制为一个屏幕行）。用于显示长文本行的附加屏幕行称为续行。延续不等于填充；延续只发生在屏幕上，不在缓冲区内容中，它在右边距而不是在单词边界处断行。请参阅填充。

在图形显示上，窗口边缘中的微小箭头图像表示截断和连续的线条（请参阅边缘）。在文本终端上，窗口最右列中的 `$` 表示截断；最右边一列的 `\` 表示换行。（显示表可以指定用于此的替代字符；请参阅显示表）。

由于文本的换行和截断相互矛盾，Emacs 在请求换行时关闭行截断，反之亦然。

    User Option: truncate-lines ¶

如果此缓冲区局部变量为非 `nil` ，则超出窗口右边缘的行将被截断；否则，它们将继续。作为一个特殊的例外，变量 `truncate-partial-width-windows` 在部分宽度窗口（即不占据整个框架宽度的窗口）中具有优先权。

    User Option: truncate-partial-width-windows ¶

此变量控制部分宽度窗口中的行截断。部分宽度窗口是不占据整个框架宽度的窗口（请参阅拆分窗口）。如果值为 `nil` ，则行截断由变量 `truncate-lines` 确定（见上文）。如果该值为整数 `n` ，则如果部分宽度窗口的列少于 `n` 列，则将截断行，而不管 `truncate-lines` 的值如何；如果部分宽度窗口有 `n` 列或更多列，则行截断由 `truncate-lines` 确定。对于任何其他非 `nil` 值，无论 `truncate-lines` 的值如何，每个部分宽度窗口中的行都会被截断。

在窗口中使用水平滚动（请参阅水平滚动）时，会强制截断。

    Variable: wrap-prefix ¶

如果这个缓冲区局部变量不是 `nil` ，它定义了一个 `wrap` 前缀，Emacs 会在每个续行的开头显示该前缀。（如果行被截断，则从不使用 `wrap-prefix` 。）它的值可以是字符串或图像（请参阅其他显示规范），或者是由 `:width` 或 `:align-to` 显示属性指定的一段空白（见指定空间）。该值的解释方式与显示文本属性相同。请参阅显示属性。

也可以使用 `wrap-prefix text` 或 `overlay` 属性为文本区域指定换行前缀。这优先于 `wrap-prefix` 变量。请参阅具有特殊含义的属性。

    Variable: line-prefix ¶

如果这个缓冲区局部变量不是 `nil` ，它定义了一个行前缀，Emacs 会在每个非连续行的开头显示该行前缀。它的值可以是字符串或图像（请参阅其他显示规范），或者是由 `:width` 或 `:align-to` 显示属性指定的一段空白（请参阅指定的空格）。该值的解释方式与显示文本属性相同。请参阅显示属性。

也可以使用 `line-prefix text` 或 `overlay` 属性为文本区域指定行前缀。这优先于行前缀变量。请参阅具有特殊含义的属性。



## 40.4 回显区

回显区域用于显示错误消息（请参阅错误）、使用消息原语生成的消息以及回显击键。它与 `minibuffer` 不同，尽管 `minibuffer` 出现在屏幕上与回显区域相同的位置（当激活时）。请参阅 `GNU Emacs` 手册中的 `Minibuffer` 。

除了本节中记录的函数之外，您还可以通过将 `t` 指定为输出流来将 `Lisp` 对象打印到回显区域。请参阅输出流。



### 40.4.1 在回显区显示消息

本节介绍在回显区域中显示消息的标准功能。

    Function: message format-string &rest arguments ¶

此功能在回显区域显示一条消息。format-string 是一个格式字符串，参数是其格式规范的对象，就像在 `format-message` 函数中一样（请参阅格式化字符串）。生成的格式化字符串显示在回显区域；如果它包含面文本属性，它会与指定的面一起显示（请参阅面）。该字符串也被添加到 `*Messages*` 缓冲区，但没有文本属性（请参阅在 `*Messages*` 中记录消息）。

通常，格式中的重音和撇号会转换为匹配的弯引号，例如， ``Missing `%s'`` 可能会导致 `Missing 'foo'` 。有关如何影响或禁止此翻译的信息，请参阅文本引用样式。

在批处理模式下，消息被打印到标准错误流，后跟换行符。

当 `inhibitor-message` 为非 `nil` 时，回显区域不会显示任何消息，只会记录到 `'*Messages*'` 。

如果 `format-string` 为 `nil` 或空字符串，则 `message` 清除回显区域；如果回显区域已自动扩展，则会将其恢复到正常大小。如果 `minibuffer` 处于活动状态，这会将 `minibuffer` 内容立即带回屏幕。

    (message "Reverting `%s'..." (buffer-name))
     -| Reverting ‘subr.el’...
    ⇒ "Reverting ‘subr.el’..."
    
    
    ---------- Echo Area ----------
    Reverting ‘subr.el’...
    ---------- Echo Area ----------

要根据消息的大小在回显区域或弹出缓冲区中自动显示消息，请使用 `display-message-or-buffer` （见下文）。

警告：如果您想将自己的字符串逐字用作消息，请不要只写（消息字符串）。如果字符串包含 `'%'` 、'\`' 或 `'''` 它可能会被重新格式化，从而产生不希望的结果。而是使用 `(message "%s"` 字符串)。

    Variable: set-message-function ¶

如果此变量非零，它应该是一个参数的函数，即在回显区域中显示的消息文本。该函数将被消息和相关函数调用。如果函数返回 `nil` ，则消息将照常显示在回显区域中。如果此函数返回一个字符串，则该字符串将显示在回显区域而不是原始字符串。如果此函数返回其他非零值，则表示该消息已被处理，因此消息不会在回显区域显示任何内容。另见 `clear-message-function` 可用于清除此函数显示的消息。

默认值是当 `minibuffer` 处于活动状态时在 `minibuffer` 末尾显示消息的函数。但是，如果活动小缓冲区中显示的文本在某些字符上具有 `minibuffer-message` 文本属性（请参阅具有特殊含义的属性），则消息将在具有该属性的第一个字符之前显示。

    Variable: clear-message-function ¶

如果此变量为非 `nil` ，则 `message` 和相关函数在其参数 `message` 为 `nil` 或空字符串时不带参数调用它。

通常在显示回显区域消息后下一个输入事件到达时调用此函数。该函数应清除由 `set-message-function` 指定的对应函数显示的消息。

默认值是清除活动小缓冲区中显示的消息的函数。

    Variable: inhibit-message ¶

当此变量为非零时，消息和相关函数将不会使用回显区域来显示消息。

    Macro: with-temp-message message &rest body ¶

此构造在执行主体期间临时在回显区域中显示一条消息。它显示消息，执行正文，然后返回最后一个正文形式的值，同时恢复先前的回显区域内容。

    Function: message-or-box format-string &rest arguments ¶

此功能显示类似消息的消息，但可能会在对话框而不是回显区域中显示它。如果在使用鼠标调用的命令中调用此函数（更准确地说，如果 `last-nonmenu-event` （请参阅命令循环中的信息）为 `nil` 或列表），则它使用对话框或弹出菜单显示消息。否则，它使用回显区域。（这与 `y-or-np` 用于做出类似决定的标准相同；请参阅是或否查询。）

您可以通过将 `last-nonmenu-event` 绑定到调用周围的合适值来强制使用鼠标或回显区域。

    Function: message-box format-string &rest arguments ¶

此函数显示类似消息的消息，但尽可能使用对话框（或弹出菜单）。如果由于终端不支持而无法使用对话框或弹出菜单，则 `message-box` 使用回显区域，如 `message` 。

    Function: display-message-or-buffer message &optional buffer-name action frame ¶

此函数显示消息消息，它可以是字符串或缓冲区。如果它小于由 `max-mini-window-height` 定义的回波区域的最大高度，则使用消息将其显示在回波区域中。否则，显示缓冲区用于在弹出缓冲区中显示它。

返回显示在回显区域中的字符串，或者在使用弹出缓冲区时返回用于显示它的窗口。

如果 `message` 是字符串，则可选参数 `buffer-name` 是使用弹出缓冲区时用于显示它的缓冲区的名称，默认为 `*Message*` 。在message是字符串并显示在回显区的情况下，不指定是否将内容插入缓冲区。

可选参数 `action` 和 `frame` 与 `display-buffer` 相同，仅在显示缓冲区时使用。

    Function: current-message ¶

此函数返回当前显示在回显区域中的消息，如果没有则返回 `nil` 。



### 40.4.2 上报操作进度

当操作可能需要一段时间才能完成时，您应该通知用户它所取得的进展。这样用户可以估计剩余时间并清楚地看到Emacs正忙于工作，而不是挂起。一种方便的方法是使用进度报告器。

这是一个没有任何用处的工作示例：

    
    
    (let ((progress-reporter
           (make-progress-reporter "Collecting mana for Emacs..."
    			       0  500)))
      (dotimes (k 500)
        (sit-for 0.01)
        (progress-reporter-update progress-reporter k))
      (progress-reporter-done progress-reporter))

    Function: make-progress-reporter message &optional min-value max-value current-value min-change min-time ¶

此函数创建并返回一个进度报告器对象，您将使用它作为下面列出的其他函数的参数。这个想法是预先计算尽可能多的数据，以便非常快速地报告进度。

当后续使用此进度报告器时，它将在回显区域显示消息，然后显示进度百分比。message 被视为一个简单的字符串。例如，如果您需要它依赖于文件名，请在调用此函数之前使用 `format-message` 。

参数 `min-value` 和 `max-value` 应该是代表操作的开始和最终状态的数字。例如，扫描缓冲区的操作应该将这些设置为相应的 `point-min` 和 `point-max` 的结果。最大值应该大于最小值。

或者，您可以将 `min-value` 和 `max-value` 设置为 `nil` 。在这种情况下，进度报告者不会报告进程百分比；相反，它会显示一个 `微调器` ，每次更新进度报告器时都会旋转一个刻度。

如果 `min-value` 和 `max-value` 是数字，您可以给参数 `current-value` 一个数值，指定初始进度；如果省略，则默认为最小值。

其余参数控制回显区域更新的速率。在打印下一条消息之前，进度报告者将等待至少 `min-change more percents` 的操作完成；默认值为百分之一。min-time 指定连续打印之间通过的最短时间（以秒为单位）；默认值为 `0.2` 秒。（在某些操作系统上，进度报告器可能会以不同的精度处理几分之一秒）。

这个函数调用progress-reporter-update，所以第一条消息被立即打印出来。

    Function: progress-reporter-update reporter &optional value suffix ¶

此功能主要负责报告您的操作进度。它显示报告者的消息，后跟由值确定的进度百分比。如果百分比为零，或者根据 `min-change` 和 `min-time` 参数足够接近，则从输出中省略它。

Reporter 必须是调用 `make-progress-reporter` 的结果。value 指定您的操作的当前状态，并且必须在传递给 `make-progress-reporter` 的 `min-value` 和 `max-value` （包括）之间。例如，如果您扫描缓冲区，则 `value` 应该是调用点的结果。

可选参数后缀是要在记者的主要消息和进度文本之后显示的字符串。如果reporter 是一个非数值型的reporter，那么value 应该是nil，或者是一个字符串来代替suffix。

此函数尊重传递给 `make-progress-reporter` 的 `min-change` 和 `min-time` ，因此不会在每次调用时输出新消息。因此它非常快，通常您不应尝试减少对其的调用次数：由此产生的开销很可能会抵消您的努力。

    Function: progress-reporter-force-update reporter &optional value new-message suffix ¶

这个函数类似于progress-reporter-update，只是它在回显区域无条件地打印一条消息。

Reporter、value 和 `suffix` 与 `progress-reporter-update` 的含义相同。可选的新消息允许您更改报告者的消息。由于此功能始终更新回波区域，因此此类更改将立即呈现给用户。

    Function: progress-reporter-done reporter ¶

操作完成时应调用此函数。它在回显区域打印记者的消息，然后是 `完成` 一词。

您应该始终调用此函数，而不是希望 `progress-reporter-update` 打印 `'100%'` 。首先，它可能永远不会打印出来，这有很多很好的理由不会发生。其次， `完成` 更加明确。

    Macro: dotimes-with-progress-reporter (var count [result]) reporter-or-message body… ~¶

~ 这是一个便利宏，其工作方式与 `dotimes` 相同，但也使用上述函数报告循环进度。它可以让你节省一些打字。参数报告器或消息可以是字符串或进度报告器对象。

您可以使用此宏重写本小节开头的示例，如下所示：

    (dotimes-with-progress-reporter
        (k 500)
        "Collecting some mana for Emacs..."
      (sit-for 0.01))

如果要在 `make-progress-reporter` 中指定可选参数，则使用报告器对象作为报告器或消息参数很有用。例如，您可以将前面的示例编写如下：

    (dotimes-with-progress-reporter
        (k 500)
        (make-progress-reporter "Collecting some mana for Emacs..." 0 500 0 1 1.5)
      (sit-for 0.01))

    Macro: dolist-with-progress-reporter (var count [result]) reporter-or-message body… ~¶

~ 这是另一个便利宏，其工作方式与 `dolist` 相同，但也使用上述函数报告循环进度。与 `dotimes-with-progress-reporter` 一样，reporter-or-message 可以是进度报告器或字符串。您可以使用此宏重写前面的示例，如下所示：

    (dolist-with-progress-reporter
        (k (number-sequence 0 500))
        "Collecting some mana for Emacs..."
      (sit-for 0.01))



### 40.4.3 记录消息 `*` 留言\*

几乎所有显示在回显区域的消息也都记录在 `*Messages*` 缓冲区中，以便用户可以参考它们。这包括与 `message` 一起输出的所有消息。默认情况下，这个缓冲区是只读的并且使用主要模式messages-buffer-mode。没有什么可以阻止用户杀死 `*Messages*` 缓冲区，但下一次显示的消息会重新创建它。任何需要直接访问 `*Messages*` 缓冲区并希望确保它存在的 `Lisp` 代码都应该使用函数消息缓冲区。

    Function: messages-buffer ¶

此函数返回 `*Messages*` 缓冲区。如果它不存在，它会创建它，并将其切换到消息缓冲区模式。

    User Option: message-log-max ¶

此变量指定在 `*Messages*` 缓冲区中保留多少行。值 `t` 表示保留多少行没有限制。值 `nil` 完全禁用消息记录。以下是显示消息并防止其被记录的方法：

    (let (message-log-max)
      (message …))

为了使 `*Messages*` 对用户更方便，日志记录工具结合了连续的相同消息。为了两种情况，它还结合了连续的相关消息：问题后接答案，以及一系列进度消息。

一个问题后面跟着一个答案有两条消息，就像 `y-or-np` 产生的那样：第一个是 `问题` ，第二个是 `问题...答案` 。除了第二条消息之外，第一条消息没有传达任何其他信息，因此记录第二条消息会丢弃日志中的第一条消息。

一系列进度消息具有连续的消息，例如由 `make-progress-reporter` 生成的消息。它们具有 `base...how-far` 的形式，其中 `base` 每次都相同，而 `how-far` 则不同。记录系列中的每条消息都会丢弃前一条，前提是它们是连续的。

函数 `make-progress-reporter` 和 `y-or-np` 无需执行任何特殊操作即可激活消息日志组合功能。每当记录两个连续的消息，它们共享一个以 `...` 结尾的公共前缀时，它就会运行。



### 40.4.4 回显区自定义

这些变量控制回显区域如何工作的细节。

    Variable: cursor-in-echo-area ¶

此变量控制在回显区域中显示消息时光标出现的位置。如果它不为零，则光标出现在消息的末尾。否则，光标会出现在点上——根本不在回波区域。

该值通常为零；Lisp 程序在短时间内将它绑定到 `t` 。

    Variable: echo-area-clear-hook ¶

每当回显区域被清除时，这个正常的钩子就会运行——无论是通过（消息 `nil` ）还是出于任何其他原因。

    User Option: echo-keystrokes ¶

此变量确定在命令字符回显之前应该经过多少时间。它的值必须是一个数字，并指定回显前等待的秒数。如果用户键入前缀键（例如 `C-x` ），然后在继续之前延迟了这么多秒，则前缀键会在回显区域中回显。（一旦在键序列中开始回显，同一键序列中的所有后续字符都会立即回显。）

如果该值为零，则不回显命令输入。

    Variable: message-truncate-lines ¶

通常，显示长消息会调整回显区域的大小以显示整个消息，并根据需要换行。但是，如果变量 `message-truncate-lines` 不为零，则会截断长行的 `echo-area` 消息以适应迷你窗口的宽度。

变量 `max-mini-window-height` 指定调整 `minibuffer` 窗口大小的最大高度，也适用于 `echo` 区域（这实际上是 `minibuffer` 窗口的特殊用途；请参阅 `Minibuffer Windows` ）。



## 40.5 报告警告

警告是程序通知用户可能出现的问题但继续运行的一种工具。



### 40.5.1 警告基础

每个警告都有一个文本消息，它为用户解释问题，以及一个严重级别，它是一个符号。以下是可能的严重性级别，按严重性降序排列，以及它们的含义：

    :emergency

如果您不及时处理，很快就会严重影响Emacs操作的问题。

    :error

本质上错误的数据或情况的报告。

    :warning

报告本质上不是错误的数据或情况，但会引起对可能问题的怀疑。

    :debug

如果您正在调试，可能会有用的信息报告。

当你的程序遇到无效的输入数据时，它可以通过调用 `error` 或 `signal` 来表示 `Lisp` 错误，或者报告严重性为 `:error` 的警告。发出 `Lisp` 错误信号是最简单的事情，但这意味着程序无法继续处理。如果您想不厌其烦地实施一种方法来继续处理不良数据，那么报告严重性警告 `:error` 是通知用户问题的正确方法。例如，Emacs Lisp 字节编译器可以通过这种方式报告错误并继续编译其他函数。（如果程序发出 `Lisp` 错误信号，然后用条件情况处理它，用户将看不到错误消息；它可以通过将消息报告为警告来向用户显示该消息。）

每个警告都有一个警告类型来对其进行分类。类型是符号列表。第一个符号应该是您用于程序用户选项的自定义组。例如，字节编译器警告使用警告类型 `(bytecomp)` 。如果您愿意，您还可以通过在列表中使用更多符号对警告进行子分类。

    Function: display-warning type message &optional level buffer-name ¶

此函数上报警告，使用 `message` 作为消息，使用 `type` 作为警告类型。level 应该是严重级别， `:warning` 是默认值。

buffer-name，如果非零，则指定用于记录警告的缓冲区的名称。默认情况下，它是\*警告\*。

    Function: lwarn type level message &rest args ¶

此函数使用 `(format-message message args...)` 的值作为 `*Warnings*` 缓冲区中的消息报告警告。在其他方面，它相当于显示警告。

    Function: warn message &rest args ¶

此函数使用 `(format-message message args...)` 的值作为消息，(emacs) 作为类型，使用 `:warning` 作为严重级别来报告警告。它的存在只是为了兼容；我们建议不要使用它，因为您应该指定特定的警告类型。



### 40.5.2 警告变量

程序可以通过绑定本节中描述的变量来自定义其警告的显示方式。

    Variable: warning-levels ¶

此列表定义警告严重性级别的含义和严重性顺序。每个元素定义一个严重性级别，它们按严重性降序排列。

每个元素都有形式（级别字符串函数），其中级别是它定义的严重级别。字符串指定此级别的文本描述。string 应该使用 `'%s'` 来指定放置警告类型信息的位置，或者它可以省略 `'%s'` 以便不包含该信息。

可选函数，如果非零，是一个不带参数调用的函数，以引起用户的注意。

通常不应更改此变量的值。

    Variable: warning-prefix-function ¶

如果非零，则该值是为警告生成前缀文本的函数。程序可以将变量绑定到合适的函数。display-warning 使用警告缓冲区当前调用此函数，该函数可以在其中插入文本。该文本成为警告消息的开头。

该函数使用两个参数调用，即严重性级别及其在警告级别中的条目。它应该返回一个列表以用作条目（此值不必是警告级别的实际成员）。通过构造此值，函数可以更改警告的严重性，或为给定的严重性级别指定不同的处理。

如果变量的值为 `nil` 则没有函数可以调用。

    Variable: warning-series ¶

程序可以将此变量绑定到 `t` 以表示下一个警告应该开始一个系列。当多个警告形成一个系列时，这意味着在系列的第一个警告上留下点，而不是为每个警告继续移动它，以便它出现在最后一个警告上。当本地绑定解除绑定并且warning-series 再次变为nil 时，该系列结束。

该值也可以是具有函数定义的符号。这等效于 `t` ，除了下一个警告还将调用没有参数且警告缓冲区当前的函数。该函数可以插入文本，作为一系列警告的标题。

一旦系列开始，该值就是一个标记，它指向系列开始的警告缓冲区中的缓冲区位置。

该变量的正常值为 `nil` ，这意味着分别处理每个警告。

    Variable: warning-fill-prefix ¶

当此变量为非零时，它指定用于填充每个警告文本的填充前缀。

    Variable: warning-fill-column ¶

填写警告的列。

    Variable: warning-type-format ¶

此变量指定在警告消息中显示警告类型的格式。以这种方式格式化类型的结果将包含在消息中，由警告级别条目中的字符串控制。默认值为 `(%s)` 。如果将其绑定到 ~~ ，则根本不会出现警告类型。



### 40.5.3 警告选项

用户使用这些变量来控制 `Lisp` 程序报告警告时发生的情况。

    User Option: warning-minimum-level ¶

此用户选项指定应立即向用户显示的最低严重性级别。默认为 `:warning` ，即立即显示除 `:debug` 警告之外的所有警告。

    User Option: warning-minimum-log-level ¶

此用户选项指定应记录在警告缓冲区中的最低严重级别。默认值为 `:warning` ，表示记录除 `:debug` 警告之外的所有警告。

    User Option: warning-suppress-types ¶

此列表指定不应立即向用户显示哪些警告类型。列表的每个元素都应该是一个符号列表。如果其元素与警告类型中的第一个元素匹配，则不会立即显示该警告。

    User Option: warning-suppress-log-types ¶

此列表指定不应将哪些警告类型记录在警告缓冲区中。列表的每个元素都应该是一个符号列表。如果它与警告类型中的前几个元素匹配，则不会记录该警告。



### 40.5.4 延迟警告

有时，您可能希望避免在命令运行时显示警告，仅在命令结束后显示。您可以为此使用功能延迟警告。

    Function: delay-warning type message &optional level buffer-name ¶

此函数是显示警告的延迟对应物（请参阅警告基础知识），并且使用相同的参数调用它。警告消息排队到延迟警告列表中。

    Variable: delayed-warnings-list ¶

此变量的值是当前命令完成后要显示的警告列表。每个元素必须是一个列表

    (type message [level [buffer-name]])

与显示警告的参数列表形式相同，含义相同。运行 `post-command-hook` （请参阅命令循环概述）后，Emacs 命令循环立即显示此变量指定的所有警告，然后将其重置为 `nil` 。

需要进一步自定义延迟警告机制的程序可以更改变量delayed-warnings-hook：

    Variable: delayed-warnings-hook ¶

这是一个普通的钩子，由Emacs命令循环在 `post-command-hook` 之后运行，以处理和显示延迟警告。

它的默认值是两个函数的列表：

    (collapse-delayed-warnings display-delayed-warnings)

函数 `collapse-delayed-warnings` 从延迟警告列表中删除重复的条目。函数 `display-delayed-warnings` 依次对 `delay-warnings-list` 中的每个条目调用 `display-warning` ，然后将 `delay-warnings-list` 设置为 `nil` 。



## 40.6 不可见文本

您可以使用 `invisible` 属性使字符不可见，以便它们不会出现在屏幕上。这可以是文本属性（请参阅文本属性）或覆盖属性（请参阅覆盖）。光标运动也部分忽略了这些字符；如果命令循环在命令后发现该点位于不可见文本范围内，则它将点重新定位到文本的另一侧。

在最简单的情况下，任何非 `nil` 不可见属性都会使字符不可见。这是默认情况——如果你不改变 `buffer-invisibility-spec` 的默认值，这就是 `invisible` 属性的工作方式。如果您不打算自己设置 `buffer-invisibility-spec` ，通常应该使用 `t` 作为 `invisible` 属性的值。

更一般地，您可以使用变量 `buffer-invisibility-spec` 来控制不可见属性的哪些值使文本不可见。这允许您预先将文本分类为不同的子集，通过赋予它们不同的不可见值，然后通过更改 `buffer-invisibility-spec` 的值使各种子集可见或不可见。

使用 `buffer-invisibility-spec` 控制可见性在显示数据库中条目列表的程序中特别有用。它允许执行方便的过滤命令来查看数据库中的部分条目。设置此变量非常快，比扫描缓冲区中的所有文本以查找要更改的属性要快得多。

    Variable: buffer-invisibility-spec ¶

此变量指定哪些类型的不可见属性实际上使字符不可见。设置此变量使其成为缓冲区本地。

    t

如果一个字符的 `invisible` 属性为非 `nil` ，则该字符是不可见的。这是默认设置。

    a list

列表的每个元素都指定了不可见的标准；如果角色的隐形属性符合这些条件中的任何一项，则该角色是隐形的。列表可以有两种元素：

    atom

如果一个字符的不可见属性值是 `atom` 或者它是一个以 `atom` 作为成员的列表，则该字符是不可见的；比较是用eq完成的。

    (atom . t)

如果一个字符的不可见属性值是 `atom` 或者它是一个以 `atom` 作为成员的列表，则该字符是不可见的；比较是用eq完成的。此外，这些字符的序列显示为省略号。

专门提供了两个函数来向 `buffer-invisibility-spec` 添加元素和从中删除元素。

    Function: add-to-invisibility-spec element ¶

此函数将元素元素添加到 `buffer-invisibility-spec` 。如果 `buffer-invisibility-spec` 是 `t` ，它会变成一个列表 `(t)` ，因此不可见属性为 `t` 的文本保持不可见。

    Function: remove-from-invisibility-spec element ¶

这会从 `buffer-invisibility-spec` 中删除元素元素。如果元素不在列表中，则此操作无效。

使用 `buffer-invisibility-spec` 的约定是主要模式应该使用模式自己的名称作为 `buffer-invisibility-spec` 的元素和 `invisible` 属性的值：

    
    
    ;; If you want to display an ellipsis:
    (add-to-invisibility-spec '(my-symbol . t))
    ;; If you don’t want ellipsis:
    (add-to-invisibility-spec 'my-symbol)
    
    (overlay-put (make-overlay beginning end)
    	     'invisible 'my-symbol)
    
    ;; When done with the invisibility:
    (remove-from-invisibility-spec '(my-symbol . t))
    ;; Or respectively:
    (remove-from-invisibility-spec 'my-symbol)

您可以使用以下功能检查隐身性：

    Function: invisible-p pos-or-prop ¶

如果 `pos-or-prop` 是标记或数字，如果该位置的文本当前不可见，则此函数返回非零值。

如果 `pos-or-prop` 是任何其他类型的 `Lisp` 对象，则表示不可见文本或覆盖属性的可能值。在这种情况下，如果该值会导致文本变得不可见，则此函数将根据 `buffer-invisibility-spec` 的当前值返回一个非零值。

如果文本将在显示时完全隐藏，则此函数的返回值为 `t` ，如果文本将被省略号替换，则返回非零、非 `t` 值。

通常，对文本或移动点进行操作的函数并不关心文本是否不可见，它们处理不可见字符和可见字符一样。如果 `line-move-ignore-invisible` 为非 `nil` （默认值），则用户级别的行移动命令，例如 `next-line` 、previous-line，将忽略不可见的换行符，即表现得就像这些不可见的换行符在缓冲区，但仅仅是因为它们被明确编程为这样做。

如果命令以不可见文本内部或边界处的点结束，则主编辑循环将点重新定位到不可见文本的两端之一。Emacs 选择重定位的方向，使其与命令的整体移动方向一致；如果有疑问，它更喜欢插入的字符不会继承不可见属性的位置。此外，如果文本没有被省略号替换并且命令仅在不可见文本内移动，则将点移动一个额外的字符，以便尝试通过光标的可见移动来反映命令的移动。

因此，如果命令将点移回不可见范围（具有通常的粘性），Emacs 会将点移回该范围的开头。如果命令将点向前移动到不可见范围内，Emacs 会将点向前移动到不可见文本后面的第一个可见字符，然后再向前移动一个字符。

可以通过将 `disable-point-adjustment` 设置为非零值来禁用这些在不可见文本中间结束的点的调整。请参阅命令后调整点。

当匹配包含不可见文本时，增量搜索可以使不可见覆盖暂时和/或永久可见。要启用此功能，叠加层应具有非零 `isearch-open-invisible` 属性。属性值应该是一个以叠加层作为参数调用的函数。此功能应使叠加层永久可见；当匹配与退出搜索时的覆盖重叠时使用它。

在搜索过程中，通过临时修改它们的不可见和无形属性，使此类叠加层临时可见。如果您希望对某个叠加层以不同的方式执行此操作，请给它一个 `isearch-open-invisible-temporary` 属性，它是一个函数。该函数使用两个参数调用：第一个是叠加层，第二个是 `nil` 使叠加层可见，或 `t` 使其再次不可见。



## 40.7 选择性显示

选择性显示是指在屏幕上隐藏某些行的一对相关功能。

第一个变体，显式选择性显示，设计用于 `Lisp` 程序：它通过更改文本来控制隐藏哪些行。这种隐藏现在已经过时和弃用了；相反，您应该使用不可见属性（请参阅不可见文本）来获得相同的效果。

在第二个变体中，根据缩进自动选择要隐藏的行。此变体旨在成为用户级功能。

控制显式选择性显示的方法是将换行符 `(control-j)` 替换为回车符 `(control-m)` 。以前是该换行符之后的一行的文本现在被隐藏了。严格来说，它暂时不再是一行，因为只有换行才能分隔行；它现在是前一行的一部分。

选择性显示不直接影响编辑命令。例如，Cf (forward-char) 毫不犹豫地将点移动到隐藏文本中。但是，用回车符替换换行符会影响一些编辑命令。例如，下一行跳过隐藏行，因为它只搜索换行符。使用选择性显示的模式还可以定义考虑换行符的命令，或者控制隐藏文本的哪些部分。

当您将选择性显示的缓冲区写入文件时，所有 `control-m` 都作为换行符输出。这意味着当您下次读取文件时，它看起来还不错，没有任何隐藏。选择性显示效果仅在Emacs中可见。

    Variable: selective-display ¶

此缓冲区局部变量启用选择性显示。这意味着可以隐藏线条或线条的一部分。

如果selective-display的值为t，则字符control-m标记隐藏文本的开始；不显示 `control-m` 及其后的其余行。这是明确的选择性显示。
如果selective-display 的值是一个正整数，则不显示以多于那么多缩进列开始的行。

当缓冲区的某些部分被隐藏时，垂直移动命令就像该部分不存在一样运行，从而允许单个下一行命令跳过任意数量的隐藏行。但是，字符移动命令（例如 `forward-char` ）不会跳过隐藏部分，并且可以（如果棘手）在隐藏部分中插入或删除文本。

在下面的例子中，我们展示了缓冲区 `foo` 的显示外观，它随着选择性显示的值而变化。缓冲区的内容不会改变。

    
    
    (setq selective-display nil)
         ⇒ nil
    
    ---------- Buffer: foo ----------
    1 on this column
     2on this column
      3n this column
      3n this column
     2on this column
    1 on this column
    ---------- Buffer: foo ----------
    
    
    (setq selective-display 2)
         ⇒ 2
    
    ---------- Buffer: foo ----------
    1 on this column
     2on this column
     2on this column
    1 on this column
    ---------- Buffer: foo ----------

    User Option: selective-display-ellipses ¶

如果这个缓冲区局部变量不为 `nil` ，那么Emacs会在行尾显示 `...` ，然后是隐藏文本。这个例子是前一个例子的延续。

    (setq selective-display-ellipses t)
         ⇒ t
    
    ---------- Buffer: foo ----------
    1 on this column
     2on this column ...
     2on this column
    1 on this column
    ---------- Buffer: foo ----------

您可以使用显示表来替换省略号 `('...')` 的其他文本。请参阅显示表格。



## 40.8 临时展示

Lisp 程序使用临时显示将输出放入缓冲区，然后将其呈现给用户阅读而不是编辑。许多帮助命令使用此功能。

    Macro: with-output-to-temp-buffer buffer-name body… ~¶

~ 该函数执行 `body` 中的表单，同时安排将它们打印的任何输出插入名为 `buffer-name` 的缓冲区中，如果需要，首先创建该缓冲区，然后进入帮助模式。（参见下面与-temp-buffer-window 类似的表格。）最后，缓冲区显示在某个窗口中，但该窗口未被选中。

如果 `body` 中的表单没有改变输出缓冲区中的主要模式，因此在它们执行结束时它仍然是帮助模式，那么 `with-output-to-temp-buffer` 使这个缓冲区在最后是只读的，并且还扫描它以查找函数和变量名称，以使它们成为可点击的交叉引用。有关详细信息，请参阅文档字符串提示，特别是文档字符串中的超链接项目。

字符串 `buffer-name` 指定临时缓冲区，它不需要已经存在。参数必须是字符串，而不是缓冲区。缓冲区最初被擦除（不询问任何问题），并在 `with-output-to-temp-buffer` 退出后标记为未修改。

with-output-to-temp-buffer 将标准输出绑定到临时缓冲区，然后评估正文中的表单。默认情况下，使用正文中的 `Lisp` 输出函数输出到该缓冲区（但回显区域中的屏幕显示和消息，虽然它们是一般意义上的 `输出` ，但不受影响）。请参阅输出函数。

有几个钩子可用于自定义此构造的行为；它们在下面列出。

返回正文中最后一个表单的值。

    
    
    ---------- Buffer: foo ----------
     This is the contents of foo.
    ---------- Buffer: foo ----------
    
    
    (with-output-to-temp-buffer "foo"
        (print 20)
        (print standard-output))
    ⇒ #<buffer foo>
    
    ---------- Buffer: foo ----------
    
    20
    
    #<buffer foo>
    
    ---------- Buffer: foo ----------

    User Option: temp-buffer-show-function ¶

如果此变量不为零，with-output-to-temp-buffer 将其作为函数调用以完成显示帮助缓冲区的工作。该函数有一个参数，即它应该显示的缓冲区。

最好让这个函数像 `with-output-to-temp-buffer` 一样运行 `temp-buffer-show-hook` ，在 `save-selected-window` 内并选择选定的窗口和缓冲区。

    Variable: temp-buffer-setup-hook ¶

这个正常的钩子在评估 `body` 之前由 `with-output-to-temp-buffer` 运行。当钩子运行时，临时缓冲区是当前的。这个钩子通常设置了一个函数来将缓冲区置于帮助模式。

    Variable: temp-buffer-show-hook ¶

这个普通的钩子在显示临时缓冲区后由 `with-output-to-temp-buffer` 运行。当钩子运行时，临时缓冲区是当前的，并且显示它的窗口被选中。

    Macro: with-temp-buffer-window buffer-or-name action quit-function body… ~¶

~ 此宏类似于 `with-output-to-temp-buffer` 。与该构造类似，它在安排将其打印的任何输出插入名为 `buffer-or-name` 的缓冲区并在某个窗口中显示该缓冲区的同时执行主体。但是，与 `with-output-to-temp-buffer` 不同，它不会自动将该缓冲区切换到帮助模式。

参数 `buffer-or-name` 指定临时缓冲区。它可以是一个必须已经存在的缓冲区，也可以是一个字符串，在这种情况下，如有必要，将创建一个具有该名称的缓冲区。当 `with-temp-buffer-window` 退出时，缓冲区被标记为未修改和只读。

此宏不调用 `temp-buffer-show-function` 。相反，它将 `action` 参数传递给 `display-buffer` （请参阅选择用于显示缓冲区的窗口）以显示缓冲区。

除非指定了参数 `quit-function` ，否则返回 `body` 中最后一个表单的值。在这种情况下，使用两个参数调用它：显示缓冲区的窗口和正文的结果。最终的返回值就是退出函数返回的值。

这个宏使用普通的钩子 `temp-buffer-window-setup-hook` 和 `temp-buffer-window-show-hook` 来代替 `with-output-to-temp-buffer` 运行的类似钩子。

接下来描述的两个结构与 `with-temp-buffer-window` 基本相同，但与指定的不同：

    Macro: with-current-buffer-window buffer-or-name action quit-function &rest body ¶

这个宏类似于 `with-temp-buffer-window` 但不同的是，它使由 `buffer-or-name` 指定的缓冲区当前用于运行主体。

显示临时缓冲区的窗口可以使用以下模式适合该缓冲区的大小：

    User Option: temp-buffer-resize-mode ¶

启用此次要模式时，显示临时缓冲区的窗口会自动调整大小以适应其缓冲区的内容。

当且仅当它是专门为缓冲区创建的时，才会调整窗口的大小。特别是，以前显示过另一个缓冲区的窗口不会调整大小。默认情况下，此模式使用 `fit-window-to-buffer` （请参阅调整窗口大小）来调整大小。您可以通过自定义以下选项 `temp-buffer-max-height` 和 `temp-buffer-max-width` 来指定不同的函数。

    User Option: temp-buffer-max-height ¶

此选项指定启用 `temp-buffer-resize-mode` 时显示临时缓冲区的窗口的最大高度（以行为单位）。它也可以是一个被调用来选择这样一个缓冲区的高度的函数。它有一个参数，缓冲区，并且应该返回一个正整数。在调用函数时，选择要调整大小的窗口。

    User Option: temp-buffer-max-width ¶

此选项指定启用 `temp-buffer-resize-mode` 时显示临时缓冲区的窗口的最大宽度（以列为单位）。它也可以是一个被调用来选择这样一个缓冲区的宽度的函数。它有一个参数，缓冲区，并且应该返回一个正整数。在调用函数时，选择要调整大小的窗口。

以下函数使用当前缓冲区进行临时显示：

    Function: momentary-string-display string position &optional char message ¶

此函数会在当前缓冲区的位置暂时显示字符串。它对撤消列表或缓冲区的修改状态没有影响。

瞬时显示一直保持到下一个输入事件。如果下一个输入事件是 `char` ，则 `momentary-string-display` 会忽略它并返回。否则，该事件将保持缓冲以供后续用作输入。因此，键入 `char` 将简单地从显示中删除字符串，而键入（例如）Cf 将从显示中删除字符串，然后（可能）向前移动点。默认情况下，参数 `char` 是一个空格。

momentary-string-display 的返回值没有意义。

如果字符串 `string` 不包含控制字符，您可以通过创建（然后删除）具有 `before-string` 属性的覆盖以更通用的方式完成相同的工作。请参见叠加属性。

如果 `message` 不为 `nil` ，则显示在 `echo` 区域中，而 `string` 显示在缓冲区中。如果它是 `nil` ，则默认消息说键入 `char` 以继续。

    在此示例中，点最初位于第二行的开头：
    #+begin<sub>src</sub> emacs-lisp
-----&#x2013;&#x2014; Buffer: foo -----&#x2013;&#x2014;
This is the contents of foo.
∗Second line.
-----&#x2013;&#x2014; Buffer: foo -----&#x2013;&#x2014;

(momentary-string-display
  "**\*\*** Important Message! **\*\***"
  (point) ?\r
  "Type RET when done reading")
⇒ t

-----&#x2013;&#x2014; Buffer: foo -----&#x2013;&#x2014;
This is the contents of foo.

1.  Important Message! \*\*\*\*Second line.

    -----&#x2013;&#x2014; Buffer: foo -----&#x2013;&#x2014;
    
    -----&#x2013;&#x2014; Echo Area -----&#x2013;&#x2014;
    Type RET when done reading
    -----&#x2013;&#x2014; Echo Area -----&#x2013;&#x2014;
        #+end<sub>src</sub>



## 40.9 叠加

为了演示功能，您可以使用覆盖来改变屏幕上缓冲区文本的外观。覆盖是属于特定缓冲区的对象，具有指定的开始和结束。它还具有您可以检查和设置的属性；这些会影响叠加层中文本的显示。

叠加层的视觉效果与相应的文本属性相同（请参阅文本属性）。然而，由于不同的实现，覆盖通常不能很好地扩展（许多操作所花费的时间与缓冲区中的覆盖数量成正比）。如果您需要影响缓冲区中许多部分的视觉外观，我们建议使用文本属性。

覆盖使用标记来记录它的开始和结束；因此，编辑缓冲区的文本会调整每个叠加层的开头和结尾，使其与文本保持一致。创建叠加层时，您可以指定在开头插入的文本应该在叠加层内部还是外部，同样用于叠加层的末尾。



### 40.9.1 管理覆盖

本节介绍创建、删除和移动覆盖以及检查其内容的功能。覆盖更改不会记录在缓冲区的撤消列表中，因为覆盖不是缓冲区内容的一部分。

    Function: overlayp object ¶

如果对象是叠加层，则此函数返回 `t` 。

    Function: make-overlay start end &optional buffer front-advance rear-advance ¶

此函数创建并返回属于缓冲区且范围从开始到结束的覆盖。start 和 `end` 都必须指定缓冲区位置；它们可能是整数或标记。如果省略缓冲区，则在当前缓冲区中创建覆盖。

开始和结束指定相同缓冲区位置的覆盖称为空。如果删除了开头和结尾之间的文本，则非空叠加层可能会变为空。发生这种情况时，默认情况下不会删除覆盖，但您可以通过赋予其 `蒸发` 属性（请参阅蒸发属性）将其删除。

参数front-advance 和rear-advance 分别指定覆盖开始和覆盖结束的标记插入类型。请参阅标记插入类型。如果它们都是 `nil` （默认值），则覆盖将扩展到包括在开头插入的任何文本，但不包括在末尾插入的文本。如果 `front-advance` 不为零，则插入在覆盖开头的文本将从覆盖中排除。如果 `back-advance` 不为零，则插入到覆盖层末尾的文本将包含在覆盖层中。

    Function: overlay-start overlay ¶

此函数以整数形式返回覆盖开始的位置。

    Function: overlay-end overlay ¶

此函数以整数形式返回覆盖结束的位置。

    Function: overlay-buffer overlay ¶

该函数返回叠加层所属的缓冲区。如果覆盖已被删除，则返回 `nil` 。

    Function: delete-overlay overlay ¶

此功能删除覆盖。叠加层继续作为 `Lisp` 对象存在，它的属性列表没有改变，但它不再附加到它所属的缓冲区，并且不再对显示产生任何影响。

已删除的叠加层不会永久断开连接。您可以通过调用 `move-overlay` 再次给它在缓冲区中的位置。

    Function: move-overlay overlay start end &optional buffer ¶

此函数将覆盖移动到缓冲区，并将其边界放置在开始和结束处。参数 `start` 和 `end` 都必须指定缓冲区位置；它们可能是整数或标记。

如果 `buffer` 被省略，overlay 将停留在它已经关联的同一个缓冲区中；如果覆盖被删除，它会进入当前缓冲区。

返回值是覆盖。

这是更改覆盖的端点的唯一有效方法。不要尝试手动修改叠加层中的标记，因为这无法更新其他重要数据结构并可能导致一些叠加层丢失。

    Function: remove-overlays &optional start end name value ¶

此函数删除属性名称具有值 `value` 的 `start` 和 `end` 之间的所有覆盖。它可以移动区域中叠加层的端点，或拆分它们。

如果 `name` 省略或为 `nil` ，则表示删除指定区域内的所有叠加层。如果 `start` 和/或 `end` 被省略或为零，则分别表示缓冲区的开始和结束。因此， `(remove-overlays)` 删除当前缓冲区中的所有覆盖。

    Function: copy-overlay overlay ¶

此函数返回覆盖的副本。副本具有与覆盖相同的端点和属性。但是，覆盖开始和覆盖结束的标记插入类型设置为其默认值（请参阅标记插入类型）。

这里有些例子：

    ;; Create an overlay.
    (setq foo (make-overlay 1 10))
         ⇒ #<overlay from 1 to 10 in display.texi>
    (overlay-start foo)
         ⇒ 1
    (overlay-end foo)
         ⇒ 10
    (overlay-buffer foo)
         ⇒ #<buffer display.texi>
    ;; Give it a property we can check later.
    (overlay-put foo 'happy t)
         ⇒ t
    ;; Verify the property is present.
    (overlay-get foo 'happy)
         ⇒ t
    ;; Move the overlay.
    (move-overlay foo 5 20)
         ⇒ #<overlay from 5 to 20 in display.texi>
    (overlay-start foo)
         ⇒ 5
    (overlay-end foo)
         ⇒ 20
    ;; Delete the overlay.
    (delete-overlay foo)
         ⇒ nil
    ;; Verify it is deleted.
    foo
         ⇒ #<overlay in no buffer>
    ;; A deleted overlay has no position.
    (overlay-start foo)
         ⇒ nil
    (overlay-end foo)
         ⇒ nil
    (overlay-buffer foo)
         ⇒ nil
    ;; Undelete the overlay.
    (move-overlay foo 1 20)
         ⇒ #<overlay from 1 to 20 in display.texi>
    ;; Verify the results.
    (overlay-start foo)
         ⇒ 1
    (overlay-end foo)
         ⇒ 20
    (overlay-buffer foo)
         ⇒ #<buffer display.texi>
    ;; Moving and deleting the overlay does not change its properties.
    (overlay-get foo 'happy)
         ⇒ t

Emacs 将每个缓冲区的覆盖存储在两个列表中，围绕任意中心位置划分。一个列表从该中心位置向后延伸穿过缓冲区，另一个从该中心位置向前延伸。中心位置可以在缓冲区中的任何位置。

    Function: overlay-recenter pos ¶

此函数将当前缓冲区的覆盖集中在位置 `pos` 周围。这使得 `pos` 附近的位置的覆盖查找更快，但远离 `pos` 的位置更慢。

如果您先执行 `(overlay-recenter (point-max))` ，则向前扫描缓冲区并创建覆盖的循环可以运行得更快。



### 40.9.2 覆盖属性

覆盖属性类似于文本属性，因为改变字符显示方式的属性可以来自任一来源。但在大多数方面，它们是不同的。请参阅文本属性进行比较。

文本属性被认为是文本的一部分；叠加层及其属性被特别认为不是文本的一部分。因此，在各种缓冲区和字符串之间复制文本会保留文本属性，但不会尝试保留覆盖。更改缓冲区的文本属性会将缓冲区标记为已修改，而移动覆盖或更改其属性则不会。与文本属性更改不同，覆盖属性更改不会记录在缓冲区的撤消列表中。

由于多个叠加层可以为同一个字符指定一个属性值，因此Emacs允许您为每个叠加层指定一个优先级值。优先级值用于决定哪些重叠覆盖将 `获胜` 。

这些函数读取和设置覆盖的属性：

    Function: overlay-get overlay prop ¶

此函数返回覆盖中记录的属性 `prop` 的值（如果有）。如果 `overlay` 没有记录该属性的任何值，但它确实有一个作为符号的类别属性，则使用该符号的 `prop` 属性。否则，该值为 `nil` 。

    Function: overlay-put overlay prop value ¶

该函数将overlay中记录的property prop的值设置为value。它返回值。

    Function: overlay-properties overlay ¶

这将返回覆盖属性列表的副本。

另请参阅函数 `get-char-property` ，它检查给定字符的叠加属性和文本属性。请参阅检查文本属性。

许多叠加属性具有特殊含义；这是他们的表格：

    priority ¶

该属性的值决定了覆盖的优先级。如果要指定优先级值，请使用 `nil` （或零）或正整数。任何其他值都有未定义的行为。

当两个或多个覆盖覆盖相同的字符并且都指定相同的属性时，优先级很重要；优先级值较大的一个会覆盖另一个。（对于 `face` 属性，优先级较高的叠加层的值不会完全覆盖另一个值；相反，它的面属性会覆盖较低优先级的面属性的面属性。）如果两个叠加层具有相同的优先级值，并且其中一个嵌套在另一种，那么内在的将胜过外在的。如果两者都没有嵌套在另一个中，那么您不应该假设哪个覆盖将占上风。

目前，所有叠加层都优先于文本属性。

请注意，Emacs 有时会对其某些内部覆盖使用非数字优先级值，因此不要尝试对覆盖的优先级进行算术运算（除非它是您创建的）。特别是，用于显示区域的覆盖使用表单（primary .secondary）的优先级值，其中primary 值如上所述使用，而secondary 是在primary 和嵌套考虑无法解决问题时使用的备用值覆盖之间的优先级。但是，建议您不要根据这个实现细节来设计 `Lisp` 程序；如果您需要按优先顺序放置叠加层，请使用叠加层-at 的 `sorted` 参数。请参阅搜索叠加层。

    window ¶

如果 `window` 属性不为 `nil` ，则覆盖仅适用于该窗口。

    category ¶

如果叠加层具有类别属性，我们将其称为叠加层的类别。它应该是一个符号。符号的属性用作叠加层属性的默认值。

    face ¶

此属性控制文本的外观（请参阅 `Faces` ）。该属性的值可以如下：

面名（符号或字符串）。
匿名面：表单的属性列表（关键字值&#x2026;），其中每个关键字是面属性名称，值是该属性的值。
面列表。每个列表元素应该是面名称或匿名面。这指定了一个面，它是每个列出的面的属性的聚合。列表中较早出现的面具有更高的优先级。
形式为 `(foreground-color . color-name)` 或 `(background-color . color-name)` 的 `cons` 单元格。这指定前景色或背景色，类似于 `(:foreground color-name)` 或 `(:background color-name)` 。支持这种形式只是为了向后兼容，应该避免使用。

    mouse-face ¶

当鼠标在覆盖范围内时，使用此属性代替 `face` 。但是，Emacs 会忽略该属性中所有改变文本大小的面属性（例如，:height、:weight 和 `:slant` ）。这些属性始终与未突出显示的文本中的相同。

    display ¶

该属性激活了改变文本显示方式的各种功能。例如，它可以使文本显得更高或更短、更高或更低、更宽或更窄，或者替换为图像。请参阅显示属性。

    help-echo ¶

如果覆盖具有帮助回显属性，那么当您将鼠标移动到覆盖中的文本上时，Emacs 会在回显区域或工具提示窗口中显示帮助字符串。有关详细信息，请参阅文本帮助回显。

    field ¶

具有相同字段属性的连续字符构成一个字段。包括前向字和行首在内的一些运动功能在字段边界处停止移动。请参阅定义和使用字段。

    modification-hooks ¶

这个属性的值是一个函数列表，如果覆盖层中的任何字符被更改或者如果文本被严格地插入到覆盖层中，则该函数将被调用。

每次更改之前和之后都会调用挂钩函数。如果函数保存它们收到的信息，并在调用之间比较注释，它们可以准确地确定缓冲区文本中发生了哪些更改。

在更改之前调用时，每个函数都会接收四个参数：overlay、nil 以及要修改的文本范围的开头和结尾。

在更改后调用时，每个函数都会接收五个参数：叠加层、t、刚刚修改的文本范围的开始和结束，以及被该范围替换的更改前文本的长度。（对于插入，更改前的长度为零；对于删除，该长度是删除的字符数，并且更改后的开头和结尾相等。）

当这些函数被调用时，禁止修改钩子被绑定到非零。如果函数修改了缓冲区，您可能希望将 `inhibitor-modification-hooks` 绑定到 `nil` ，以便为这些修改运行更改挂钩。但是，这样做可能会递归调用您自己的更改挂钩，因此请务必为此做好准备。请参阅更改挂钩。

文本属性也支持 `modify-hooks` 属性，但细节有些不同（请参阅具有特殊含义的属性）。

    insert-in-front-hooks ¶

此属性的值是在叠加层开头插入文本之前和之后要调用的函数列表。调用约定与修改钩子函数相同。

    insert-behind-hooks ¶

此属性的值是在叠加层末尾插入文本之前和之后要调用的函数列表。调用约定与修改钩子函数相同。

    invisible ¶

invisible 属性可以使叠加层中的文本不可见，也就是说它不会出现在屏幕上。有关详细信息，请参阅不可见文本。

    intangible ¶

覆盖上的无形属性就像无形文本属性一样工作。它已经过时了。有关详细信息，请参阅具有特殊含义的属性。

    isearch-open-invisible

此属性告诉增量搜索如何使不可见的覆盖永久可见，如果最终匹配与其重叠。请参阅不可见文本。

    isearch-open-invisible-temporary

此属性告诉增量搜索如何在搜索期间使不可见的覆盖暂时可见。请参阅不可见文本。

    before-string ¶

此属性的值是要添加到叠加层开头的显示的字符串。该字符串在任何意义上都不会出现在缓冲区中——只出现在屏幕上。

    after-string ¶

此属性的值是要添加到叠加层末尾显示的字符串。该字符串在任何意义上都不会出现在缓冲区中——只出现在屏幕上。

    line-prefix

此属性指定在显示时添加到每个非连续行的显示规范。请参阅截断。

    wrap-prefix

此属性指定在显示时添加到每个续行的显示规范。请参阅截断。

    evaporate ¶

如果此属性为非零，则如果覆盖为空（即，如果其长度为零），则会自动删除覆盖。如果你给一个空覆盖（见空覆盖）一个非零的蒸发属性，它会立即删除它。请注意，除非覆盖具有此属性，否则当从缓冲区中删除其开始位置和结束位置之间的文本时，它不会被删除。

    keymap ¶

如果此属性不为 `nil` ，则它为文本的一部分指定一个键映射。此键映射优先于大多数其他键映射（请参阅活动键映射），并且当点位于覆盖范围内时使用它，其中 `front-and-rear-advance` 属性定义边界是否被视为在覆盖范围内。

    local-map ¶

local-map 属性与 `keymap` 类似，但替换了缓冲区的本地映射，而不是扩充现有的 `keymap` 。这也意味着它的优先级低于次要模式键映射。

keymap 和 `local-map` 属性不会影响由 `before-string` 、after-string 或 `display` 属性显示的字符串。这仅与鼠标单击和落在字符串上的其他鼠标事件相关，因为点从不在字符串上。要为字符串绑定特殊的鼠标事件，请为其分配一个键映射或本地映射文本属性。请参阅具有特殊含义的属性。



### 40.9.3 搜索覆盖

    Function: overlays-at pos &optional sorted ¶

此函数返回覆盖当前缓冲区中位置 `pos` 处的字符的所有叠加层的列表。如果 `sorted` 不为零，则列表按优先级降序排列，否则没有特定顺序。覆盖包含位置 `pos` ，如果它开始于 `pos` 或在 `pos` 之前，并在 `pos` 之后结束。

为了说明用法，这里有一个 `Lisp` 函数，它返回一个覆盖层列表，这些覆盖层为点处的字符指定属性 `prop` ：

    (defun find-overlays-specifying (prop)
      (let ((overlays (overlays-at (point)))
    	found)
        (while overlays
          (let ((overlay (car overlays)))
    	(if (overlay-get overlay prop)
    	    (setq found (cons overlay found))))
          (setq overlays (cdr overlays)))
        found))

    Function: overlays-in beg end ¶

这个函数返回一个覆盖区域的覆盖列表。如果覆盖在区域中包含一个或多个字符，则覆盖与区域重叠；空覆盖（参见空覆盖）重叠，如果它们在 `beg` ，严格在 `beg` 和 `end` 之间，或者在 `end` 表示缓冲区可访问部分末尾的位置时。

    Function: next-overlay-change pos ¶

此函数在 `pos` 之后返回覆盖的下一个开始或结束的缓冲区位置。如果没有，则返回 `(point-max)` 。

    Function: previous-overlay-change pos ¶

此函数在 `pos` 之前返回覆盖的前一个开始或结束的缓冲区位置。如果没有，则返回 `(point-min)` 。

例如，这是原始函数 `next-single-char-property-change` 的简化（且效率低下）版本（请参阅文本属性搜索函数）。它从位置 `pos` 向前搜索下一个位置，从覆盖或文本属性获得的给定属性 `prop` 的值发生变化。

    (defun next-single-char-property-change (position prop)
      (save-excursion
        (goto-char position)
        (let ((propval (get-char-property (point) prop)))
          (while (and (not (eobp))
    		  (eq (get-char-property (point) prop) propval))
    	(goto-char (min (next-overlay-change (point))
    			(next-single-property-change (point) prop)))))
        (point)))



## 40.10 显示文本的大小

由于并非所有字符都具有相同的宽度，因此这些函数可让您检查字符的宽度。有关相关功能，请参阅缩进基元和按屏幕线移动。

    Function: char-width char ¶

如果字符 `char` 显示在当前缓冲区中，则此函数返回以列为单位的宽度（即，考虑到缓冲区的显示表，如果有的话；请参阅显示表）。制表符的宽度通常是制表符宽度（请参阅通常的显示约定）。

    Function: string-width string &optional from to ¶

如果字符串显示在当前缓冲区和选定窗口中，则此函数返回以列为单位的宽度。来自和指定要考虑的字符串的子字符串的可选参数，并被解释为在子字符串中（请参阅创建字符串）。

返回值是一个近似值：它只考虑 `char-width` 为组成字符返回的值，总是将制表符作为制表符宽度列，忽略显示属性和字体等。出于这些原因，我们建议使用 `window -text-pixel-size` ，如下所述。

    Function: truncate-string-to-width string width &optional start-column padding ellipsis ellipsis-text-property ¶

此函数返回一个新字符串，它是字符串的截断，适合显示的宽度列。

如果字符串比宽度窄，结果等于字符串；否则结果中会省略多余的字符。如果字符串中的多列字符超过目标宽度，则从结果中省略该字符。因此，结果有时可能会低于宽度，但不能超过它。

可选参数 `start-column` 指定起始列；它默认为零。如果这是非零，则从结果中省略字符串的第一个起始列。如果字符串中的一个多列字符跨越列起始列，则省略该字符。

可选参数填充（如果非零）是在结果字符串的开头和结尾添加的填充字符，以将其扩展到精确宽度的列。如果宽度不足，则填充字符将附加在结果的末尾，达到宽度所需的次数。如果字符串中的多列字符跨越列起始列，它也会在结果的开头添加。

如果省略号是非零，它应该是一个字符串，当它被截断时将替换字符串的结尾。在这种情况下，将从字符串中删除更多字符，以便为省略号释放足够的空间以适应宽度列。但是，如果字符串的显示宽度小于省略号的显示宽度，则省略号不会附加到结果中。如果 `ellipsis` 不是 `nil` 且不是字符串，则它代表函数 `truncate-string-ellipsis` 返回的值，如下所述。

可选参数 `ellipsis-text-property` ，如果非 `nil` ，则表示使用显示省略号的显示文本属性（请参阅显示属性）隐藏字符串的多余部分，而不是实际截断字符串。

    (truncate-string-to-width "\tab\t" 12 4)
         ⇒ "ab"
    (truncate-string-to-width "\tab\t" 12 4 ?\s)
         ⇒ "    ab  "

该函数使用 `string-width` 和 `char-width` 在字符串太宽时找到合适的截断点，因此它遇到与 `string-width` 相同的基本问题。特别是，当字符组合发生在字符串中时，字符串的显示宽度可能小于组成字符的宽度之和，并且此函数可能返回不准确的结果。

    Function: truncate-string-ellipsis ¶

此函数返回要在 `truncate-string-to-width` 和其他类似上下文中用作省略号的字符串。该值是变量truncate-string-ellipsis的值，如果它不为nil，则如果该字符可以显示在所选帧上，则为具有单个字符U + 2026 HORIZONTAL ELLIPSIS的字符串，否则为字符串'&#x2026;' .

以下函数返回文本的大小（以像素为单位），就好像它显示在给定窗口中一样。fit-window-to-buffer 和 `fit-frame-to-buffer` 使用此函数（请参阅调整窗口大小）使窗口与它包含的文本一样大。

    Function: window-text-pixel-size &optional window from to x-limit y-limit mode-lines ¶

此函数返回窗口缓冲区文本的大小（以像素为单位）。window 必须是活动窗口，并且默认为选定的窗口。返回值是任何文本行的最大像素宽度和所有文本行的最大像素高度的组合。此函数的存在是为了允许 `Lisp` 程序将窗口的尺寸调整为它需要显示的缓冲区文本。

可选参数 `from` ，如果非 `nil` ，指定要考虑的第一个文本位置，默认为缓冲区的最小可访问位置。如果 `from` 是 `t` ，它代表不是换行符的最小可访问位置。可选参数，如果非零，指定要考虑的最后一个文本位置，默认为缓冲区的最大可访问位置。如果 `to` 是 `t` ，它代表不是换行符的最大可访问位置。

可选参数 `x-limit` ，如果非 `nil` ，则指定最大 `X` 坐标，超过该坐标应忽略文本；因此，它也是函数可以返回的最大像素宽度值。如果 `x-limit nil` 或省略，则表示使用窗口主体的像素宽度（参见窗口大小）；此默认值意味着比窗口宽的截断行的文本将被忽略。当调用者不打算更改窗口的宽度时，此默认值很有用。否则，调用者应在此处指定窗口主体可能采用的最大宽度；特别是，如果需要截断的行并且需要考虑其文本，则应将 `x-limit` 设置为较大的值。由于计算长线的宽度可能需要一些时间，因此根据需要使这个参数尽可能小总是一个好主意；特别是，如果缓冲区可能包含无论如何都会被截断的长行。

可选参数 `y-limit` ，如果非零，指定最大 `Y` 坐标，超过该坐标文本将被忽略；因此，它也是函数可以返回的最大像素高度。如果 `y-limit` 为 `nil` 或省略，则表示考虑所有文本行，直到 `to` 指定的缓冲区位置。由于计算大缓冲区的像素高度可能需要一些时间，因此指定此参数是有意义的；特别是，如果调用者不知道缓冲区的大小。

可选参数 `mode-lines nil` 或省略表示在返回值中不包括窗口的模式行、制表行或标题行的高度。如果它是符号模式行、制表行或标题行，则在返回值中仅包含该行的高度（如果存在）。如果是 `t` ，则在返回值中包含所有这些行的高度（如果存在）。

window-text-pixel-size 将窗口中显示的文本视为一个整体，而不关心各行的大小。下面的函数可以。

    Function: window-lines-pixel-dimensions &optional window first last body inverse left ¶

此函数计算指定窗口中显示的每一行的像素尺寸。它通过遍历窗口的当前字形矩阵来做到这一点——一个存储当前显示在窗口中的每个缓冲区字符的字形（参见字形）的矩阵。如果成功，它会返回一个 `cons` 对列表，表示每行最后一个字符的右下角的 `x` 和 `y` 坐标。坐标从窗口左上角的原点 `(0, 0)` 以像素为单位测量。window 必须是活动窗口，并且默认为选定的窗口。

如果可选参数 `first` 是一个整数，它表示要返回的窗口字形矩阵的第一行的索引（从 `0` 开始）。请注意，如果窗口有标题行，则索引为 `0` 的行就是该标题行。如果 `first` 为 `nil` ，则要考虑的第一行由可选参数 `body` 的值确定：如果 `body` 为非 `nil` ，这意味着从窗口主体的第一行开始，跳过任何标题行（如果存在）。否则，此函数将从窗口字形矩阵的第一行开始，可能是标题行。

如果可选参数 `last` 是一个整数，它表示应返回的窗口字形矩阵的最后一行的索引。如果 `last` 为 `nil` ，则要考虑的最后一行由 `body` 的值决定： `~ 如果 ~body` 为非 `nil` ，这意味着使用窗口主体的最后一行，省略窗口的模式行（如果存在）。否则，这意味着使用窗口的最后一行，它可能是模式行。

可选参数 `inverse` ，如果为 `nil` ，则表示为任何行返回的 `y` 像素值指定从窗口的左边缘（如果 `body` 为非 `nil` ，则为 `body` 边缘）到该窗口最后一个字形的右边缘的距离（以像素为单位）线。inverse non-nil 表示为任何行返回的 `y` 像素值指定从该行的最后一个字形的右边缘到窗口的右边缘（如果 `body` 为非 `nil` ，则为 `body` 边缘）的距离（以像素为单位）。这对于确定每行末尾的松弛空间量很有用。

可选参数 `left` ，如果非 `nil` ，则表示返回每行最左边字符的左下角的 `x` 和 `y` 坐标。这是应该用于主要从右到左显示文本的窗口的值。

如果 `left` 为非 `nil` 且 `inverse` 为 `nil` ，这意味着为任何行返回的 `y` 像素值指定从该行的最后一个（最左侧）字形的左边缘到右边缘（如果body 是非 `nil)` 的窗口。如果 `left` 和 `inverse` 都非 `nil` ，则为任何行返回的 `y` 像素值指定从窗口的左边缘（如果 `body` 为非 `nil` ，则为 `body` 边缘）到最后一个（最左边）的左边缘的距离（以像素为单位）那条线的字形。

如果当前窗口的字形矩阵不是最新的，则此函数返回 `nil` ，这通常发生在Emacs忙碌时，例如，在处理命令时。该值应该是可检索的，尽管当此函数从一个延迟为零秒的空闲计时器运行时。

    Function: line-pixel-height ¶

此函数返回所选窗口中点的线的高度（以像素为单位）。该值包括行的行距（请参阅行高）。

当缓冲区显示行号时（参见 `GNU Emacs` 手册中的 `Display Custom` ），有时了解显示行号所采用的宽度很有用。以下函数适用于需要此信息进行布局计算的 `Lisp` 程序。

    Function: line-number-display-width &optional pixelwise ¶

此函数返回用于在选定窗口中显示行号的宽度。如果可选参数 `pixelwise` 是符号列，则返回值是帧规范列的浮点数；如果 `pixelwise` 是 `t` 或任何其他非零值，则该值是一个整数，以像素为单位。如果 `pixelwise` 被省略或为零，则该值是为行号面定义的字体的整数列数，并且不包括用于填充显示数字的 `2` 列。如果所选窗口中未显示行号，则无论pixelwise 的值如何，该值都为零。如果您需要有关另一个窗口的信息，请使用 `with-selected-window` （请参阅选择窗口）。



## 40.11 行高

每条显示行的总高度包括行内容的高度，加上显示行上方或下方可选的附加垂直行距。

行内容的高度是该显示行上任何字符或图像的最大高度，包括最后一个换行符（如果有的话）。（继续的显示行不包括最后的换行符。）如果您不指定更大的高度，那是默认的行高。（在最常见的情况下，这等于相应框架的默认字体的高度，请参阅框架字体。）

有几种方法可以显式指定更大的行高，或者通过指定显示行的绝对高度，或者通过指定垂直空间。但是，无论您指定什么，实际行高都不能小于默认值。

换行符可以具有行高文本或覆盖属性，用于控制以该换行符结尾的显示行的总高度。属性值可以是以下几种形式之一：

    t

如果属性值为 `t` ，则换行符对行的显示高度没有影响——可见内容单独决定了高度。在这种情况下，也将忽略下面描述的行间距属性。这对于平铺小图像（或图像切片）而不在图像之间添加空白区域很有用。

    (height total)

如果属性值是显示的表单列表，则会在显示行下方添加额外的空间。首先Emacs使用 `height` 作为高度规范来控制线上方的额外空间；然后它在线条下方添加足够的空间以使总线条高度达到总高度。在这种情况下，换行符的任何 `line-spacing` 属性值都将被忽略。

任何其他类型的属性值都是高度规范，它转换为一个数字——指定的行高。有几种方法可以编写高度规范；以下是它们每个转换为数字的方式：

    integer

如果高度规范是一个正整数，那么高度值就是那个整数。

    float

如果高度规范是浮点数，浮点数，数字高度值是浮点数乘以框架的默认行高。

    (face . ratio)

如果高度规格是所示格式的缺点，则数字高度是比率乘以面高度。ratio 可以是任何类型的数字，或者 `nil` 表示比率为 `1` 。如果 `face` 是 `t` ，它指的是当前面。

    (nil . ratio)

如果高度规范是所示格式的缺点，则数字高度是行内容高度的比率乘以。

因此，任何有效的高度规范都会以一种或另一种方式确定以像素为单位的高度。如果行内容的高度小于该值，Emacs 会在行上方添加额外的垂直空间以达到指定的总高度。

如果不指定 `line-height` 属性，则行高由内容的高度加上行距组成。有几种方法可以为Emacs文本的不同部分指定行距。

在图形终端上，您可以使用 `line-spacing frame` 参数指定框架中所有行的行距（请参阅布局参数）。但是，如果 `line-spacing` 的默认值不是 `nil` ，它会覆盖框架的 `line-spacing` 参数。一个整数指定放置在行下方的像素数。浮点数指定相对于框架默认行高的间距。

您可以通过 `buffer-local line-spacing` 变量指定缓冲区中所有行的行距。一个整数指定放置在行下方的像素数。浮点数指定相对于默认框架行高的间距。这会覆盖为框架指定的行距。

最后，换行符可以有一个行间距文本或覆盖属性，可以扩大默认帧行间距和缓冲区本地行间距变量：如果它的值大于缓冲区或帧默认值，则使用较大的值，对于以该换行符结尾的显示行。

这些机制以一种或另一种方式为每行的间距指定一个 `Lisp` 值。该值是一个高度规范，并且如上所述转换为 `Lisp` 值。但是，在这种情况下，数字高度值指定了行间距，而不是行高。

在文本终端上，行距不能更改。



## 40.12 面

面是用于显示文本的图形属性的集合：字体、前景色、背景色、可选的下划线等。面控制Emacs如何在缓冲区中显示文本，以及框架的其他部分，例如模式行。

表示面的一种方法是作为属性的属性列表，例如 `(:foreground "red" :weight bold)` 。这样的列表称为匿名面。例如，您可以指定一个匿名面作为面文本属性的值，Emacs 将显示具有指定属性的底层文本。请参阅具有特殊含义的属性。

更常见的是，通过面名称来引用面：与一组面属性相关联的 `Lisp` 符号24。命名面是使用 `defface` 宏定义的（请参阅定义面）。Emacs 带有几个标准的命名面（请参阅基本面）。

Emacs 的某些部分需要命名面（例如，面属性函数中记录的函数）。除非另有说明，否则我们将使用术语 `面` 来指代已命名的面。

    Function: facep object ¶

如果对象是一个命名的面，这个函数返回一个非零值：一个 `Lisp` 符号或字符串，用作面名。否则，它返回零。



### 40.12.1 面属性

面属性决定了面的视觉外观。下表列出了所有面属性、它们的可能值及其效果。

除了下面给出的值之外，每个面属性都可以具有未指定的值。这个特殊值意味着面不直接指定该属性。一个未指定的属性告诉Emacs引用父面（参见下面的描述 `:inherit` 属性）；或者，如果失败，则到底层面（请参阅显示面）。默认面必须指定所有属性。

其中一些属性仅在某些类型的显示器上有意义。如果您的显示器无法处理某个属性，则该属性将被忽略。

    :family

字体系列名称（字符串）。有关字体系列的更多信息，请参阅 `GNU Emacs` 手册中的字体。函数 `font-family-list` （见下文）返回可用家族名称的列表。

    :foundry

由 `:family` 属性（字符串）指定的字体系列的字体代工厂的名称。请参阅 `GNU Emacs` 手册中的字体。

    :width

相对字符宽度。这应该是超压缩、超压缩、压缩、半压缩、正常、半扩展、扩展、超扩展或超扩展的符号之一。

    :height

字体的高度。在最简单的情况下，这是一个以 `1/10` 点为单位的整数。

该值也可以是浮点数或函数，它指定相对于底层面的高度（请参阅显示面）。浮点值指定底层面高度的缩放量。使用一个参数调用函数值，即底层面的高度，并返回新面的高度。如果函数被传递一个整数参数，它必须返回一个整数。

必须使用整数指定默认面的高度；不允许使用浮点和函数值。

    :weight

字体粗细——符号之一（从最密集到最微弱）超粗体、超粗体、粗体、半粗体、正常、半轻、轻、超轻或超轻。在支持可变亮度文本的文本终端上，任何大于正常的粗细都显示为超亮，而任何小于正常的粗细都显示为半亮。

    :slant

字体倾斜 `-` 斜体、斜体、正常、反斜体或反斜体符号之一。在支持可变亮度文本的文本终端上，倾斜的文本显示为半亮。

    :foreground

前景色，一个字符串。该值可以是系统定义的颜色名称，也可以是十六进制颜色规范。请参阅颜色名称。在黑白显示器上，某些灰色阴影由点画图案实现。

    :distant-foreground

替代前景色，一个字符串。这就像 `:foreground` 但仅当背景颜色接近本应使用的前景时，颜色才用作前景。例如，这在标记文本（即区域面）时很有用。如果文本具有区域面可见的前景，则使用该前景。如果前景靠近区域面背景，则使用 `:distant-foreground` 代替，以便文本可读。

    :background

背景颜色，一个字符串。该值可以是系统定义的颜色名称，也可以是十六进制颜色规范。请参阅颜色名称。

    :underline

字符是否应该加下划线，以及以什么方式。:underline 属性的可能值是：

    :overline

不要下划线。

    :strike-through

用脸部的前景色划线。

    :box

是否应围绕字符绘制框、其颜色、框线的宽度和 `3D` 外观。以下是 `:box` 属性的可能值及其含义：

    nil

不要画一个盒子。

    t

用前景色绘制一个宽度为 `1` 的框。

    color

用颜色颜色画一个宽度为 `1` 的线框。

    (:line-width (vwidth . hwidth) :color color :style style)

这样，您可以明确指定框的所有方面。vwidth 和 `hwidth` 值分别指定要绘制的垂直线和水平线的宽度；它们默认为 `(1 . 1)` 。负的水平或垂直宽度 `-n` 表示绘制一条宽度为 `n` 的线，占据底层文本的空间，从而避免字符高度或宽度的任何增加。为简化起见，可以仅使用单个数字 `n` 而不是列表来指定宽度，这种情况等效于 `((abs n) . n)` 。

值样式指定是否绘制 `3D` 框。如果它是释放按钮，则该框看起来像未按下的 `3D` 按钮。如果它是按下按钮，则该框看起来像被按下的 `3D` 按钮。如果它是 `nil` 、flat-button 或省略，则使用普通的 `2D` 框。

值 `color` 指定要绘制的颜色。默认为 `3D` 框和平面按钮的面的背景颜色，以及其他框的面的前景色。

    :inverse-video

字符是否应该被覆盖，以及用什么颜色。如果值为 `t` ，则覆盖使用面的前景色。如果该值是字符串，则上划线使用该颜色。值 `nil` 表示不上划线。

    :stipple

字符是否应该被删除，以及用什么颜色。该值的使用与 `:overline` 类似。

    :font

是否应围绕字符绘制框、其颜色、框线的宽度和 `3D` 外观。以下是 `:box` 属性的可能值及其含义：

    nil

不要画一个盒子。

    t

用前景色绘制一个宽度为 `1` 的框。

    color

用颜色颜色画一个宽度为 `1` 的线框。

    (:color color :style style)

这样，您可以明确指定框的所有方面。vwidth 和 `hwidth` 值分别指定要绘制的垂直线和水平线的宽度；它们默认为 `(1 . 1)` 。负的水平或垂直宽度 `-n` 表示绘制一条宽度为 `n` 的线，占据底层文本的空间，从而避免字符高度或宽度的任何增加。为简化起见，可以仅使用单个数字 `n` 而不是列表来指定宽度，这种情况等效于 `((abs n) . n)` 。

值样式指定是否绘制 `3D` 框。如果它是释放按钮，则该框看起来像未按下的 `3D` 按钮。如果它是按下按钮，则该框看起来像被按下的 `3D` 按钮。如果它是 `nil` 、flat-button 或省略，则使用普通的 `2D` 框。

值 `color` 指定要绘制的颜色。默认为 `3D` 框和平面按钮的面的背景颜色，以及其他框的面的前景色。

    :inverse-video

字符是否应以反向视频显示。该值应为 `t` （是）或 `nil` （否）。

    :stipple

背景点画，位图。

值可以是字符串；这应该是包含外部格式 `X` 位图数据的文件的名称。该文件位于变量 `x-bitmap-file-path` 中列出的目录中。

或者，该值可以直接指定位图，带有表格的列表（宽度高度数据）。这里，宽度和高度以像素为单位指定大小，数据是包含位图原始位的字符串，逐行。每行占用字符串中的 `(width + 7) / 8` 个连续字节（为了获得最佳结果，应该是单字节字符串）。这意味着每一行总是占据至少一个完整的字节。

如果值为 `nil` ，则表示不使用点画图案。

通常不需要设置点画属性，因为它会自动用于处理某些灰色阴影。

    :font

用于显示面的字体。它的值应该是一个字体对象或一个字体集。如果它是一个字体对象，它指定了面用来显示 `ASCII` 字符的字体。有关字体对象、字体规范和字体实体的信息，请参阅低级字体表示。有关字体集的信息，请参阅字体集。

当使用 `set-face-attribute` 或 `set-face-font` 指定此属性时（请参阅 `Face Attribute Functions` ），您还可以提供字体规范、字体实体或字符串。Emacs 将这些值转换为适当的字体对象，并将该字体对象存储为实际的属性值。如果你指定一个字符串，字符串的内容应该是一个字体名称（参见 `GNU Emacs` 手册中的字体）；如果字体名称是包含通配符的 `XLFD` ，Emacs 会选择第一个匹配这些通配符的字体。指定此属性还会更改 `:family` 、:foundry、:width、:height、:weight 和 `:slant` 属性的值。

    :inherit

要从中继承属性的面的名称，或面名称的列表。继承面的属性会像底层面一样合并到面中，优先级高于底层面（请参阅显示面）。如果未指定要继承的面，则将其视为 `nil` ，因为Emacs从不合并 `:inherit` 属性。如果使用面列表，则列表中较早面的属性会覆盖后面面的属性。

    :extend

此面是否会超出行尾并影响行尾和窗口边缘之间的空白区域的显示。值应该是 `t` 以使用此面显示行尾和窗口边缘之间的空白空间，或者 `nil` 不使用此面作为行尾和窗口边缘之间的空间。当Emacs合并几个面以显示超出行尾的空白空间时，只有那些具有 `:extend` 非 `nil` 的面会被合并。默认情况下，只有少数面，特别是区域，具有此属性集。此属性与其他属性的不同之处在于，当主题没有为面指定显式值时，将继承由 `defface` 定义的原始面定义的值（请参阅定义面）。

    Function: font-family-list &optional frame ¶

此函数返回可用字体系列名称的列表。可选参数 `frame` 指定显示文本的框架；如果为 `nil` ，则使用选定的帧。

    User Option: underline-minimum-offset ¶

此变量指定显示下划线文本时基线和下划线之间的最小距离（以像素为单位）。

    User Option: x-bitmap-file-path ¶

此变量为 `:stipple` 属性指定用于搜索位图文件的目录列表。

    Function: bitmap-spec-p object ¶

如果 `object` 是有效的位图规范，则返回 `t` ，适合与 `:stipple` 一起使用（见上文）。否则返回 `nil` 。



### 40.12.2 定义面

定义面的常用方法是通过 `defface` 宏。此宏将面名称（符号）与默认面规格相关联。面规范是一种构造，它指定面在任何给定终端上应具有的属性；例如，面规范可能会在高颜色终端上指定一种前景色，在低颜色终端上指定不同的前景色。

人们有时很想创建一个值是面名称的变量。在绝大多数情况下，这不是必需的；通常的程序是用 `defface` 定义一个面，然后直接使用它的名字。

请注意，一旦定义了一个面（通常使用 `defface` ），以后就不能安全地取消定义这个面，除非重新启动 `Emacs` 。

    Macro: defface face spec doc [keyword value]… ~¶

~ 这个宏将 `face` 声明为一个命名的 `face` ，其默认的 `face spec` 由 `spec` 给出。您不应引用符号 `face` ，并且不应以 `-face` 结尾（这将是多余的）。参数 `doc` 是面的文档字符串。附加的关键字参数与 `defgroup` 和 `defcustom` 中的含义相同（请参阅通用项关键字）。

如果 `face` 已经有一个默认的 `face spec` ，这个宏什么也不做。

当没有自定义生效时，默认面规格确定面的外观（请参阅自定义设置）。如果面已经被自定义（通过自定义主题或通过从 `init` 文件中读取的自定义），则其外观由自定义面规范确定，该规范会覆盖默认面规范规范。但是，如果随后删除了自定义，则面外观将再次由其默认面规格确定。

作为例外，如果您在Emacs Lisp 模式下使用 `C-M x` (eval-defun) 或 `C-x C-e` (eval-last-sexp) 评估 `defface` 表单，这些命令的一个特殊功能会覆盖面上的任何自定义面规范，从而导致face 以准确反映 `deface` 所说的内容。

spec 参数是一个面规范，它说明面应该如何出现在不同类型的终端上。它应该是一个 `alist` ，其每个元素都具有以下形式

    (display . plist)

display 指定了一类终端（见下文）。plist 是面属性及其值的属性列表，指定面在此类终端上的显示方式。为了向后兼容，您还可以将元素编写为 `(display plist)` 。

spec 元素的显示部分决定了该元素匹配的终端。如果规范的多个元素与给定终端匹配，则匹配的第一个元素是用于该终端的元素。有三种显示方式：

    default

该规范元素不匹配任何终端；相反，它指定适用于所有终端的默认值。如果使用此元素，则必须是规范的第一个元素。以下每个元素都可以覆盖任何或所有这些默认值。

    t

该规范元素匹配所有终端。因此，从不使用 `spec` 的任何后续元素。通常 `t` 用在 `spec` 的最后一个（或唯一一个）元素中。

    a list

如果 `display` 是一个列表，那么每个元素都应该有形式（特征值…）。这里的特性指定了一种对终端进行分类的方式，这些值是显示应该适用的可能分类。以下是特征的可能值：

    type

终端使用的窗口系统类型——图形（任何支持图形的显示器）、x、pc（用于 `MS-DOS` 控制台）、w32（用于 `MS Windows 9X/NT/2K/XP` ）或 `tty` （非-图形显示）。请参阅窗口系统。

    class

终端支持哪种颜色——彩色、灰度或单色。

    background

背景的种类——浅色或深色。

    min-colors

一个整数，表示终端应支持的最小颜色数。如果终端的 `display-color-cells` 值至少是指定的整数，则它匹配终端。

    supports

终端是否可以显示 `value` 中给出的面属性…（参见面属性）。请参阅显示面属性测试，了解有关此测试具体如何完成的更多信息。

如果显示元素为给定特性指定了多个值，则这些值中的任何一个都是可接受的。如果 `display` 有多个元素，每个元素都应该指定不同的特性；那么终端的每个特征都必须匹配在显示中为其指定的值之一。

例如，下面是标准面高光的定义：

    (defface highlight
      '((((class color) (min-colors 88) (background light))
         :background "darkseagreen2")
        (((class color) (min-colors 88) (background dark))
         :background "darkolivegreen")
        (((class color) (min-colors 16) (background light))
         :background "darkseagreen2")
        (((class color) (min-colors 16) (background dark))
         :background "darkolivegreen")
        (((class color) (min-colors 8))
         :background "green" :foreground "black")
        (t :inverse-video t))
      "Basic face for highlighting."
      :group 'basic-faces)

在内部，Emacs 将每个面的默认规范存储在其 `face-defface-spec` 符号属性中（请参阅符号属性）。saved-face 属性存储用户使用自定义缓冲区保存的任何面规格；custom-face 属性存储了为当前会话定制的面规格，但没有保存；theme-face 属性存储一个列表，将活动的自定义设置和自定义主题与该面的面规格相关联。面的文档字符串存储在 `face-documentation` 属性中。

通常，使用 `defface` 只声明一次面，对其外观的任何进一步更改都使用 `Customize` 框架（例如，通过 `Customize` 用户界面或通过 `custom-set-faces` 功能；请参阅应用自定义），或通过面重映射（请参阅面重映射）。在极少数情况下，您需要直接从 `Lisp` 更改面规范，您可以使用 `face-spec-set` 函数。

    Function: face-spec-set face spec &optional spec-type ¶

此函数将规格用作面的面规格。spec 应该是一个面规范，如上面的 `defface` 文档中所述。

如果 `face` 还不是一个，此函数还将 `face` 定义为有效的 `face name` ，并（重新）计算其在现有帧上的属性。

可选参数 `spec-type` 确定要设置的规范。如果省略或 `nil` 或 `face-override-spec` ，此函数设置覆盖规范，它覆盖下面提到的所有其他类型的 `face` 上的面规范。这在自定义代码之外调用此函数时很有用。如果spec-type是customized-face或者saved-face，这个函数分别设置自定义的spec或者保存的自定义spec。如果是face-defface-spec，这个函数设置默认的face spec（和defface设置的一样）。如果重置，此函数会清除所有自定义规范并覆盖面中的规范（在这种情况下，忽略规范的值）。spec-type 的任何其他值对 `face specs` 的影响保留供内部使用，但该函数仍将定义 `face` 本身并重新计算其属性，如上所述。



### 40.12.3 面属性函数

本节介绍直接访问和修改命名面属性的函数。

    Function: face-attribute face attribute &optional frame inherit ¶

该函数返回帧上面的属性值。

如果 `frame` 被省略或为零，则表示选定的帧（请参阅输入焦点）。如果 `frame` 为 `t` ，此函数返回新创建的帧的指定属性的值，即在面的 `defface` 定义（参见定义面）中应用面规范或由面规范设置的规范之前的属性值放。这个属性的默认值通常是未指定的，除非您使用 `set-face-attribute` 指定了其他值；见下文。

如果inherit 为nil，则只考虑直接由face 定义的属性，因此返回值可能是未指定的，也可能是相对值。如果inherit 为非nil，则face 的属性定义与其:inherit 属性指定的面合并；但是返回值可能仍然是未指定的或相对的。如果inherit 是一个面或面列表，则结果将进一步与该面（或多个面）合并，直到它成为指定的和绝对的。

为确保返回值始终是指定且绝对的，请使用默认值作为继承；这将通过与默认面（始终完全指定）合并来解决任何未指定或相对值。

例如，

    (face-attribute 'bold :weight)
         ⇒ bold

    Function: face-attribute-relative-p attribute value ¶

如果值作为面属性属性的值是相对的，则此函数返回非零。这意味着它将修改，而不是完全覆盖，来自面列表中后续面或从另一个面继承的任何值。

unspecified 是所有属性的相对值。对于 `:height` ，浮点数和函数值也是相对的。

例如：

    (face-attribute-relative-p :height 2.0)
         ⇒ t

    Function: face-all-attributes face &optional frame ¶

此函数返回面属性列表。结果的元素是形式为 `(attr-name . attr-value)` 的名称-值对。可选参数 `frame` 指定要返回其面定义的框架；如果省略或为零，则返回的值描述了新创建的帧的面默认属性，即这些属性在将面规范应用于面的 `defface` 定义或由 `face-spec-set` 设置的规范之前的值。这些属性的默认值通常是未指定的，除非您使用 `set-face-attribute` 指定了其他值；见下文。

    Function: merge-face-attribute attribute value1 value2 ¶

如果 `value1` 是面属性属性的相对值，则返回与基础值 `value2` 合并的值；否则，如果 `value1` 是面属性属性的绝对值，则返回 `value1` 不变。

通常，Emacs 使用每个面的面规格来自动计算每个帧上的属性（请参阅定义面）。函数 `set-face-attribute` 可以通过直接将属性分配给特定帧或所有帧的面来覆盖此计算。此功能主要供内部使用。

    Function: set-face-attribute face frame &rest arguments ¶

该函数为框架设置一个或多个面属性。以这种方式指定的属性会覆盖属于 `face` 的 `face spec(s)` 。

额外的参数参数指定要设置的属性以及它们的值。它们应该由交替的属性名称（例如 `:family` 或 `:underline` ）和值组成。因此，

    (set-face-attribute 'foo nil :weight 'bold :slant 'italic)

将属性 `:weight` 设置为粗体，将属性 `:slant` 设置为斜体。

如果 `frame` 为 `t` ，此函数为新创建的帧设置默认属性；它们将有效地覆盖 `defface` 指定的属性值。如果 `frame` 为 `nil` ，此函数设置所有现有框架的属性，以及新创建的框架。但是，如果您想以同样影响新创建的帧的方式将属性的值重置为未指定，则必须显式调用此函数，并将帧设置为 `t` 并将属性的值设置为未指定（不是 `nil` ！），除了将 `frame` 设置为 `nil` 的调用。这是因为在创建新帧时，新创建的帧的默认属性会与 `defface` 中的面规范合并，因此在新帧的默认属性中未指定将无法覆盖 `defface` ；如上所述对这个函数的特殊调用将安排 `deface` 被覆盖。

以下命令和函数主要提供与旧版本Emacs的兼容性。他们通过调用 `set-face-attribute` 来工作。其框架参数的 `t` 和 `nil` 值（或省略）的处理方式与 `set-face-attribute` 和 `face-attribute` 一样。如果以交互方式调用，这些命令使用 `minibuffer` 读取它们的参数。

    Command: set-face-foreground face color &optional frame ¶

    Command: set-face-background face color &optional frame ¶

这些将 `face` 的 `:foreground` 属性（或 `:background` 属性）设置为颜色。

    Command: set-face-stipple face pattern &optional frame ¶

这会将 `face` 的 `:stipple` 属性设置为图案。

    Command: set-face-font face font &optional frame ¶

将 `face` 的字体相关属性更改为 `font` （字符串或字体对象）的属性。有关字体参数支持的格式，请参见 `face-font-attribute` 。该函数设置面的 `:font` 属性，并间接设置字体定义的 `:family` 、:foundry、:width、:height、:weight 和 `:slant` 属性。如果 `frame` 不为零，则仅更改指定框架上的属性。

    Function: set-face-bold face bold-p &optional frame ¶

这会将 `face` 的 `:weight` 属性设置为 `normal` 如果 `bold-p` 为 `nil` ，否则设置为粗体。

    Function: set-face-italic face italic-p &optional frame ¶

如果 `italic-p` 为 `nil` ，这会将 `face` 的 `:slant` 属性设置为 `normal` ，否则设置为 `italic` 。

    Command: set-face-underline face underline &optional frame ¶

这会将 `face` 的 `:underline` 属性设置为下划线。

    Command: set-face-inverse-video face inverse-video-p &optional frame ¶

这会将 `face` 的 `:inverse-video` 属性设置为 `inverse-video-p` 。

    Command: invert-face face &optional frame ¶

这将交换面的前景色和背景色。

    Command: set-face-extend face extend &optional frame ¶

这会将 `face` 的 `:extend` 属性设置为扩展。

以下函数检查面的属性。它们主要提供与旧版本Emacs的兼容性。如果您不指定框架，则它们指的是选定的框架；t 指的是新帧的默认数据。如果面没有为该属性定义任何值，则它们返回未指定。如果inherit 为nil，则只返回由面直接定义的属性。如果inherit 为非nil，则也考虑由其:inherit 属性指定的任何面，如果inherit 是面或面列表，则它们也会被考虑，直到找到指定的属性。要确保始终指定返回值，请使用默认值作为继承。

    Function: face-font face &optional frame character ¶

该函数返回指定面所使用的字体名称。

如果指定了可选参数框架，则返回该框架的字体名称；如果它为 `nil` 或省略，则 `frame` 默认为选定的框架。如果 `frame` 是 `t` ，该函数会报告要用于新帧的面字体默认值。

默认情况下，返回的字体用于显示 `ASCII` 字符，但如果 `frame` 不是 `t` ，并且提供了可选的第三个参数字符，则该函数返回 `face` 用于该字符的字体名称。

    Function: face-foreground face &optional frame inherit ¶

    Function: face-background face &optional frame inherit ¶

这些函数以字符串的形式返回面的前景色（或背景色）。如果未指定颜色，则返回 `nil` 。

    Function: face-stipple face &optional frame inherit ¶

此函数返回面背景点画图案的名称，如果没有则返回 `nil` 。

    Function: face-bold-p face &optional frame inherit ¶

如果 `face` 的 `:weight` 属性比正常粗体（即半粗体、粗体、超粗体或超粗体之一），则此函数返回非零值。否则，它返回零。

    Function: face-italic-p face &optional frame inherit ¶

如果 `face` 的 `:slant` 属性是斜体或倾斜，则此函数返回非 `nil` 值，否则返回 `nil` 。

    Function: face-underline-p face &optional frame inherit ¶

如果 `face face` 指定了非 `nil :underline` 属性，则此函数返回非 `nil` 。

    Function: face-inverse-video-p face &optional frame inherit ¶

如果 `face face` 指定了非 `nil :inverse-video` 属性，则此函数返回非 `nil` 。

    Function: face-extend-p face &optional frame inherit ¶

如果 `face face` 指定了非 `nil :extend` 属性，则此函数返回非 `nil` 。继承参数被传递给 `face-attribute` 。



### 40.12.4 显示面

当Emacs显示一段给定的文本时，文本的视觉外观可能由来自不同来源的面决定。如果这些不同的来源一起为特定字符指定了多个面，Emacs 会合并各种面的属性。这是Emacs合并面的顺序，从最高优先级到最低优先级：

如果文本由特殊字形组成，则字形可以指定特定的面。请参见字形。
如果文本位于活动区域内，Emacs 会使用区域面突出显示它。请参阅 `GNU Emacs` 手册中的标准面。
如果文本位于具有非零面属性的覆盖层内，Emacs 将应用该属性指定的面。如果覆盖层具有 `mouse-face` 属性并且鼠标离覆盖层足够近，Emacs 将应用由 `mouse-face` 属性指定的 `face` 或 `face` 属性。请参见叠加属性。

当多个覆盖覆盖一个字符时，具有较高优先级的覆盖覆盖具有较低优先级的覆盖。请参见叠加。
如果文本包含 `face` 或 `mouse-face` 属性，Emacs 会应用指定的 `faces` 和 `face` 属性。请参阅具有特殊含义的属性。（这就是字体锁定模式面的应用方式。请参阅字体锁定模式。）
如果文本位于选定窗口的模式行内，Emacs 将应用模式行面。对于未选中窗口的模式线，Emacs 应用模式线非活动面。对于标题行，Emacs 应用标题行面。对于制表符行，Emacs 应用制表符行面。
如果文本来自覆盖字符串通过前字符串或后字符串属性（请参阅覆盖属性），或来自显示字符串（请参阅其他显示规范），并且该字符串不包含 `face` 或 `mouse-face` 属性，或者这些属性没有定义一些面属性，但是受覆盖/显示属性影响的缓冲区文本确实定义了一个面或那些属性，Emacs 应用了 `底层` 缓冲文本的面属性。请注意，即使覆盖或显示字符串显示在显示边距中也是如此（请参阅在边距中显示）。
如果在前面的步骤中没有指定任何给定的属性，Emacs 将应用默认面的属性。

在每个阶段，如果一个面具有有效的 `:inherit` 属性，Emacs 将任何具有未指定值的属性视为具有从父面提取的相应值。请参见面属性。请注意，父面也可能未指定属性；在这种情况下，该属性在下一级面合并中保持未指定。



### 40.12.5 面重映射

变量 `face-remapping-alist` 用于面外观的缓冲区局部或全局变化。例如，它用于实现 `text-scale-adjust` 命令（参见 `GNU Emacs` 手册中的文本缩放）。

    Variable: face-remapping-alist ¶

此变量的值是一个 `alist` ，其元素具有 `(face . remapping)` 形式。这会导致Emacs显示任何带有重新映射的面的文本，而不是普通的面定义。

重新映射可以是任何适用于面文本属性的面规范：面（即面名称或属性/值对的属性列表）或面列表。具体请参见具有特殊含义的属性中对面文本属性的描述。重新映射作为重新映射面的完整规范——它取代了面的正常定义，而不是修改它。

如果 `face-remapping-alist` 是缓冲区本地，则其本地值仅在该缓冲区内生效。如果 `face-remapping-alist` 包含仅适用于某些窗口的面，则通过使用 `(:filtered (:window param val)` 规范），该面仅在匹配过滤条件的窗口中生效（请参阅具有特殊含义的属性）。要暂时关闭面过滤，请将 `face-filters-always-match` 绑定到非零值，然后所有面过滤器将匹配任何窗口。

注意：面重映射是非递归的。如果重新映射直接或通过重新映射中某个其他面的 `:inherit` 属性引用相同的面名称面，则该引用使用面的正常定义。例如，如果使用 `face-remapping-alist` 中的此条目重新映射模式线面：

    (mode-line italic mode-line)

然后模式线面的新定义继承自斜体面，以及模式线面的正常（未重新映射）定义。

以下函数实现了 `face-remapping-alist` 的更高级别接口。大多数 `Lisp` 代码应该使用这些函数而不是直接设置 `face-remapping-alist` ，以避免践踏其他地方应用的重映射。这些函数旨在用于缓冲区局部重映射，因此它们都使 `face-remapping-alist` 缓冲区局部作为副作用。他们管理表单的 `face-remapping-alist` 条目

    (face relative-spec-1 relative-spec-2 ... base-spec)

其中，如上所述，relative-spec-N 和 `base-spec` 中的每一个都是面名或属性/值对的属性列表。每个相对重映射条目，relative-spec-N，由 `face-remap-add-relative` 和 `face-remap-remove-relative` 函数管理；这些旨在用于简单的修改，例如更改文本大小。基础重映射条目 `base-spec` 具有最低优先级，由 `face-remap-set-base` 和 `face-remap-reset-base` 函数管理；它适用于主要模式在它们控制的缓冲区中重新映射面。

    Function: face-remap-add-relative face &rest specs ¶

此函数将规格添加为当前缓冲区中面的相对重映射。specs 应该是一个列表，其中每个元素要么是面名称，要么是属性/值对的属性列表。

返回值是一个用作 `cookie` 的 `Lisp` 对象；如果您稍后需要删除重新映射，则可以将此对象作为参数传递给 `face-remap-remove-relative` 。

    ;; Remap the 'escape-glyph' face into a combination
    ;; of the 'highlight' and 'italic' faces:
    (face-remap-add-relative 'escape-glyph 'highlight 'italic)
    
    ;; Increase the size of the 'default' face by 50%:
    (face-remap-add-relative 'default :height 1.5)

    Function: face-remap-remove-relative cookie ¶

此函数删除之前由 `face-remap-add-relative` 添加的相对重映射。cookie 应该是添加重映射时 `face-remap-add-relative` 返回的 `Lisp` 对象。

    Function: face-remap-set-base face &rest specs ¶

此函数将当前缓冲区中面的基本重新映射设置为规格。如果 `specs` 为空，则恢复默认的基础重映射，类似于调用 `face-remap-reset-base` （见下文）；请注意，这与包含单个值 `nil` 的规范不同，后者具有相反的结果（面的全局定义被忽略）。

这会覆盖默认的 `base-spec` ，它继承了全局面定义，因此如果需要，由调用者添加此类继承。

    Function: face-remap-reset-base face ¶

此函数将 `face` 的基本重新映射设置为其默认值，该值继承自 `face` 的全局定义。



### 40.12.6 处理面的函数

以下是用于创建和使用面的附加功能。

    Function: face-list ¶

此函数返回所有已定义面名称的列表。

    Function: face-id face ¶

该函数返回面的面编号。这是一个在Emacs中唯一标识低级别面的数字。很少需要通过面号来引用面。但是，操纵字形的函数，例如 `make-glyph-code` 和 `glyph-face` （请参阅 `Glyphs` ）在内部访问面数。请注意，面编号存储为面符号的面属性值，因此我们建议不要将该面属性设置为您自己的任何值。

    Function: face-documentation face ¶

此函数返回 `face face` 的文档字符串，如果没有指定，则返回 `nil` 。

    Function: face-equal face1 face2 &optional frame ¶

如果面 `face1` 和 `face2` 具有相同的显示属性，则返回 `t` 。

    Function: face-differs-from-default-p face &optional frame ¶

如果面显示与默认面不同，则返回非零。

面别名为面提供等效名称。您可以通过为别名符号赋予 `face-alias` 属性来定义面别名，其值为目标面名称。以下示例使 `modeline` 成为 `mode-line` 面的别名。

    (put 'modeline 'face-alias 'mode-line)

    Macro: define-obsolete-face-alias obsolete-face current-face when ¶

该宏将obsolete-face定义为current-face的别名，同时也将其标记为obsolete，表示将来可能会被移除。when 应该是一个字符串，指示 `obsolete-face` 何时被废弃（通常是版本号字符串）。



### 40.12.7 自动面分配

该钩子用于自动将面分配给缓冲区中的文本。它是 `Font-Lock` 使用的 `Jit-Lock` 模式实现的一部分。

    Variable: fontification-functions ¶

这个变量保存了一个由Emacsredisplay 根据需要调用的函数列表，就在 `redisplay` 之前。即使未启用字体锁定模式，也会调用它们。启用字体锁定模式时，此变量通常只包含一个函数，即 `jit-lock-function` 。

这些函数按列出的顺序调用，带有一个参数，一个缓冲区位置 `pos` 。总的来说，他们应该尝试从 `pos` 开始将面分配给当前缓冲区中的文本。

这些函数应该通过设置 `face` 属性记录它们分配的面。他们还应该为他们分配面的所有文本添加一个非零字体属性。该属性告诉 `redisplay` 已将面分配给该文本。

如果 `pos` 之后的字符已经具有非 `nil` 字体化属性，则函数不执行任何操作可能是一个好主意，但这不是必需的。如果一个函数覆盖了前一个函数所做的分配，那么最后一个函数完成后的属性才是真正重要的。

为了提高效率，我们建议编写这些函数，以便它们通常在每次调用时将面分配给大约 `4​​00` 到 `600` 个字符。



### 40.12.8 基本面

如果您的Emacs Lisp程序需要为文本分配一些面，则使用某些现有面或从它们继承通常是一个好主意，而不是定义全新的面。这样，如果其他用户自定义了基本面以赋予Emacs某种外观，您的程序无需额外自定义即可适应。

下面列出了Emacs中定义的一些基本面。除此之外，如果字体锁定模式尚未处理突出显示，或者某些字体锁定面未使用，您可能希望使用字体锁定面进行语法突出显示。请参见字体锁定的面。

    default

默认面，其属性全部指定。所有其他面都隐式继承自它：任何未指定的属性默认为该面上的属性（请参阅面属性）。

    bold

    italic

    bold-italic

    underline

    fixed-pitch

    fixed-pitch-serif

    variable-pitch

它们具有由它们的名称指示的属性（例如，粗体具有粗体 `:weight` 属性），所有其他属性未指定（默认情况下如此）。

    shadow

对于变暗的文本。例如，它用于 `minibuffer` 中文件名的被忽略部分（请参阅 `The GNU EmacsManual` 中的 `Minibuffers for File Names` ）。

    link

    link-visited

对于将用户发送到不同缓冲区或位置的可点击文本按钮。

    highlight

对于应该暂时突出的文本段。例如，它通常分配给 `mouse-face` 属性以突出显示光标（请参阅具有特殊含义的属性）。

    match

    isearch

    lazy-highlight

对于文本匹配（分别）永久搜索匹配、交互式搜索匹配和惰性突出显示当前交互式之外的其他匹配。

    error

    warning

    success

有关错误、警告或成功的文本。例如，这些用于 `*Compilation*` 缓冲区中的消息。



### 40.12.9 字体选择

在Emacs可以在图形显示器上绘制字符之前，它必须为该字符选择一种字体25。请参阅 `GNU Emacs` 手册中的字体。通常，Emacs 会根据分配给该字符的面自动选择字体——特别是面属性 `:family` 、:weight、:slant 和 `:width` （请参阅面属性）。字体的选​​择还取决于要显示的字符；某些字体只能显示有限的字符集。如果没有可用的字体完全符合要求，Emacs 会寻找最接近的匹配字体。本节中的变量控制Emacs如何进行此选择。

    User Option: face-font-family-alternatives ¶

如果指定了给定的系列但不存在，则此变量指定要尝试的替代字体系列。每个元素都应具有以下形式：

    (family alternate-families…)

如果指定了 `family` 但不可用，Emacs 将逐个尝试在alternate-family 中给出的其他族，直到找到确实存在的族。

    User Option: face-font-selection-order ¶

如果没有字体与所有所需的面属性（:width、:height、:weight 和 `:slant` ）完全匹配，则此变量指定在选择最接近的匹配字体时应考虑这些属性的顺序。该值应该是包含这四个属性符号的列表，按重要性降序排列。默认值为 `(:width :height :weight :slant)` 。

字体选择首先找到列表中第一个属性的最佳可用匹配；然后，在以这种方式最好的字体中，它在第二个属性中搜索最佳匹配，依此类推。

属性 `:weight` 和 `:width` 在以正常为中心的范围内具有符号值。更极端（远离正常）的匹配比不太极端（更接近正常）的匹配更受欢迎；这是为了尽可能确保非正常面与正常面形成对比。

此变量产生影响的一个示例是默认字体没有等效斜体时。在默认排序下，斜体将使用与默认字体类似的非斜体字体。但是如果你把 `:slant` 放在 `:height` 之前，斜体将使用斜体字体，即使它的高度不是很正确。

    User Option: face-font-registry-alternatives ¶

如果指定了给定的注册表但不存在，此变量允许您指定要尝试的替代字体注册表。每个元素都应具有以下形式：

    (registry alternate-registries…)

如果 `registry` 已指定但不可用，Emacs 将一个接一个地尝试备用注册表中给出的其他注册表，直到找到确实存在的注册表。

Emacs 可以使用可缩放字体，但默认情况下它不使用它们。

    User Option: scalable-fonts-allowed ¶

此变量控制要使用的可缩放字体。默认值 `nil` 表示不使用可缩放字体。t 表示使用任何适合文本的可缩放字体。

否则，该值必须是正则表达式列表。如果其名称与列表中的任何正则表达式匹配，则启用可缩放字体以供使用。例如，

    (setq scalable-fonts-allowed '("iso10646-1$"))

允许使用注册表 `iso10646-1` 的可缩放字体。

    Variable: face-font-rescale-alist ¶

此变量指定某些面的缩放。它的值应该是表单元素的列表

    (fontname-regexp . scale-factor)

如果 `fontname-regexp` 与即将使用的字体名称匹配，则表示根据因子 `scale-factor` 选择更大的相似字体。如果某些字体大于或小于其标称高度和宽度建议的值，您将使用此功能来规范字体大小。

脚注
(25)

在这种情况下，术语字体与字体锁定无关（请参阅字体锁定模式）。



### 40.12.10 查找字体

    Function: x-list-fonts name &optional reference-face frame maximum width ¶

此函数返回与名称匹配的可用字体名称列表。name 应该是包含 `Fontconfig` 、GTK+ 或 `XLFD` 格式的字体名称的字符串（请参阅 `GNU Emacs` 手册中的字体）。在 `XLFD` 字符串中，可以使用通配符：'\*' 字符匹配任何子字符串，'?'  字符匹配任何单个字符。匹配字体名称时忽略大小写。

如果指定了可选参数 `reference-face` 和 `frame` ，则返回的列表仅包括与当前在框架框架上的 `reference-face` （面名称）大小相同的字体。

可选参数最大值设置返回多少字体的限制。如果它是非零，则返回值在第一个最大匹配字体之后被截断。在许多字体与模式匹配的情况下，为最大值指定一个较小的值可以使此函数更快。

可选参数宽度指定所需的字体宽度。如果它不为 `nil` ，则该函数仅返回其字符（平均）宽度为参考面宽度的字体。

    Function: x-family-fonts &optional family frame ¶

此函数返回一个列表，描述框架上家庭系列的可用字体。如果 `family` 被省略或为零，则此列表适用于所有系列，因此它包含所有可用字体。否则，family 必须是字符串；它可能包含通配符 `？`   和 `'*'` 。

该列表描述了该帧所在的显示；如果 `frame` 被省略或为零，则它适用于所选帧的显示（请参阅输入焦点）。

列表中的每个元素都是以下形式的向量：

    [family width point-size weight slant
     fixed-p full registry-and-encoding]

前五个元素对应面属性；如果您为面指定这些属性，它将使用此字体。

最后三个元素提供有关字体的附加信息。如果字体是固定间距的，则 `fixed-p` 不为零。full 是字体的全名，registry-and-encoding 是一个字符串，给出了字体的注册和编码。



### 40.12.11 字体集

字体集是一个字体列表，每个字体都分配给一系列字符代码。单个字体不能显示Emacs支持的整个字符范围，但字体集可以。字体集具有名称，就像字体一样，当您为框架或面指定字体时，您可以使用字体集名称代替字体名称。这是有关在 `Lisp` 程序控制下定义字体集的信息。

    Function: create-fontset-from-fontset-spec fontset-spec &optional style-variant-p noerror ¶

该函数根据规范字符串 `fontset-spec` 定义一个新的字体集。该字符串应具有以下格式：

    fontpattern, [charset:font]…

逗号前后的空白字符将被忽略。

字符串的第一部分 `fontpattern` 应该具有标准 `X` 字体名称的形式，除了最后两个字段应该是 `fontset-alias` 。

新字体集有两个名称，一个长的，一个短的。长名称是 `fontpattern` 的全部。简称为 `fontset-alias` 。您可以通过任一名称引用字体集。如果已经存在同名字体集，则会发出错误信号，除非 `noerror` 为非 `nil` ，在这种情况下，此函数不执行任何操作。

如果可选参数 `style-variant-p` 不为零，则表示还要创建字体集的粗体、斜体和粗斜体变体。这些变体字体集没有短名称，只有长名称，这是通过更改 `fontpattern` 来指示粗体和/或斜体状态。

规范字符串还说明要在字体集中使用哪些字体。详情见下文。

构造 `'charset:font'` 为一个特定的字符集指定使用哪种字体（在此字体集中）。这里，charset 是字符集的名称，font 是用于该字符集的字体。您可以在规范字符串中多次使用此构造。

对于剩余的字符集，那些您没有明确指定的字符集，Emacs 会根据 `fontpattern` 选择一种字体：它将 `'fontset-alias'` 替换为命名一个字符集的值。对于 `ASCII` 字符集， `fontset-alias` 替换为 `ISO8859-1` 。

此外，当几个连续的字段是通配符时，Emacs 会将它们折叠成一个通配符。这是为了防止使用自动缩放字体。通过缩放较大的字体制作的字体不能用于编辑，缩放较小的字体也没有用，因为最好使用较小的字体以自己的大小，Emacs 就是这样做的。

因此，如果 `fontpattern` 是这样的，

    -*-fixed-medium-r-normal-*-24-*-*-*-*-*-fontset-24

ASCII 字符的字体规范是这样的：

    -*-fixed-medium-r-normal-*-24-*-ISO8859-1

中文 `GB2312` 字符的字体规范是这样的：

    -*-fixed-medium-r-normal-*-24-*-gb2312*-*

您可能没有任何符合上述字体规范的中文字体。大多数 `X` 发行版仅包含在 `family` 字段中具有 `song ti` 或 `fangsong ti` 的中文字体。在这种情况下， `Fontset-n` 可以指定如下：

    Emacs.Fontset-0: -*-fixed-medium-r-normal-*-24-*-*-*-*-*-fontset-24,\
    	chinese-gb2312:-*-*-medium-r-normal-*-24-*-gb2312*-*

然后，除中文 `GB2312` 字符之外的所有字体规范在族字段中都有 `固定` ，而中文 `GB2312` 字符的字体规范在族字段中具有通配符 `*` 。

    Function: set-fontset-font name character font-spec &optional frame add ¶

此函数修改现有字体集名称以使用与指定字符的 `font-spec` 匹配的字体。

如果 `name` 为 `nil` ，则此函数修改所选框架的字体集，如果 `frame` 不为 `nil` ，则修改框架的字体集。

如果 `name` 为 `t` ，此函数修改默认字体集，其短名称为 `'fontset-default'` 。

除了指定单个代码点外，字符还可以是一个 `cons` （从 `.to` ），其中 `from` 和 `to` 是字符代码点。在这种情况下，对 `from` 和 `to` （包括）范围内的所有字符使用 `font-spec` 。

字符可能是一个字符集（参见字符集）。在这种情况下，对字符集中的所有字符使用 `font-spec` 。

字符可以是脚本名称（参见 `char-script-table` ）。在这种情况下，对属于脚本的所有字符使用 `font-spec` 。

character 可以为 `nil` ，这意味着对任何未指定 `font-spec` 的字符使用 `font-spec` 。

font-spec 可能是由函数 `font-spec` 创建的 `font-spec` 对象（请参阅 `Low-Level Font Representation` ）。

font-spec 可能是一个缺点；(family .registry)，其中family是字体的家族名称（可能在头部包括铸造厂名称），registry是字体的注册表名称（可能在尾部包括编码名称）。

font-spec 可以是字体名称、字符串。

font-spec 可能为 `nil` ，它明确指定指定字符没有字体。这很有用，例如，可以避免在系统范围内为没有字形的字符进行昂贵的字体搜索，例如来自 `Unicode Private Use Area (PUA)` 的字符。

可选参数 `add` ，如果非 `nil` ，指定如何将 `font-spec` 添加到先前设置的字体规范。如果它是前置的，则 `font-spec` 是前置的。如果是追加，则追加字体规范。默认情况下，font-spec 会覆盖之前的设置。

例如，这会将默认字体集更改为对属于字符集 `Japanese-jisx0208` 的所有字符使用家族名称为 `Kochi Gothic` 的字体。

    (set-fontset-font t 'japanese-jisx0208
    		  (font-spec :family "Kochi Gothic"))

    Function: char-displayable-p char ¶

如果Emacs应该能够显示字符，则此函数返回非 `nil` 。或者更准确地说，如果所选框架的字体集具有显示 `char` 所属字符集的字体。

字体集可以基于每个字符指定字体；当字体集这样做时，此函数的值可能不准确。

即使没有可用的字体，此函数也可能返回非零，因为它还检查文本终端的编码系统是否可以对字符进行编码（请参阅终端 `I/O` 编码）。



### 40.12.12 低级字体表示

通常，没有必要直接操作字体。如果您需要这样做，本节将说明如何进行。

在Emacs Lisp中，字体使用三种不同的 `Lisp` 对象类型来表示：字体对象、字体规范和字体实体。

    Function: fontp object &optional type ¶

如果 `object` 是字体对象、字体规范或字体实体，则返回 `t` 。否则，返回零。

可选参数类型，如果非零，则确定要检查的 `Lisp` 对象的确切类型。在这种情况下，类型应该是 `font-object` 、font-spec 或 `font-entity` 之一。

字体对象是一个 `Lisp` 对象，它表示Emacs已打开的字体。字体对象不能在 `Lisp` 中修改，但可以检查。

    Function: font-at position &optional window string ¶

返回用于在窗口窗口中位置位置显示字符的字体对象。如果 `window` 为 `nil` ，则默认为选定的窗口。如果 `string` 为 `nil` ，则 `position` 指定当前缓冲区中的位置；否则，string 应该是一个字符串，并且 `position` 指定该字符串中的一个位置。

字体规范是一个 `Lisp` 对象，它包含一组可用于查找字体的规范。不止一种字体可能与字体规范中的规范相匹配。

    Function: font-spec &rest arguments ¶

使用参数中的规范返回一个新的字体规范，它应该以属性值对的形式出现。可能的规格如下：

    :name

字体名称（字符串），采用 `XLFD` 、Fontconfig 或 `GTK+` 格式。请参阅 `GNU Emacs` 手册中的字体。

    :family

    :foundry

    :weight

    :slant

    :width

这些与同名的面属性具有相同的含义。请参见面属性。:family 和 `:foundry` 是字符串，而其他三个是符号。作为示例值， `:slant` 可以是斜体， `:weight` 可以是粗体， `:width` 可以是正常的。

    :size

字体大小——可以是指定像素大小的非负整数，也可以是指定点大小的浮点数。

    :adstyle

字体的附加排版样式信息，例如 `sans` 。该值应该是字符串或符号。

    :registry

字符集注册表和字体的编码，例如 `iso8859-1` 。该值应该是字符串或符号。

    :dpi

字体设计的分辨率（以每英寸点数为单位）。该值必须是非负数。

    :spacing

字体间距：proportional、dual、mono 或 `charcell` 。该值应该是整数（0 表示比例，90 表示双，100 表示单声道，110 表示 `charcell` ）或单字母符号（P、D、M 或 `C` 之一）。

    :avgwidth

字体的平均宽度，以 `1/10` 像素为单位。该值应该是一个非负数。

    :script

字体必须支持的脚本（符号）。

    :lang

字体应支持的语言。该值应该是一个符号，其名称是一个由两个字母组成的 `ISO-639` 语言名称。在 `X` 上，该值与字体的 `XLFD` 名称的 `附加样式` 字段匹配（如果它不为空）。在 `MS-Windows` 上，需要符合规范的字体才能支持该语言所需的代码页。目前，此属性仅支持一小部分 `CJK` 语言： `ja` 、 `ko` 和 `zh` 。

    :otf ¶

字体必须是支持这些 `OpenType` 功能的 `OpenType` 字体，前提是Emacs使用库编译，例如 `GNU/Linux` 上的 `libotf` ，该库支持需要的脚本的复杂文本布局。该值必须是表单的列表

    (script-tag langsys-tag gsub gpos)

其中 `script-tag` 是 `OpenType` 脚本标签符号；langsys-tag 是 `OpenType` 语言系统标签符号，或 `nil` 使用默认语言系统；gsub 是 `OpenType GSUB` 功能标记符号的列表，如果不需要，则为 `nil` ；gpos 是 `OpenType GPOS` 功能标签符号的列表，如果不需要，则为 `nil` 。如果 `gsub` 或 `gpos` 是一个列表，则该列表中的 `nil` 元素意味着该字体不得与任何剩余的标记符号匹配。gpos 元素可以省略。

    Function: font-put font-spec property value ¶

将 `font-spec font-spec` 中的 `font` 属性设置为 `value` 。

字体实体是对不需要打开的字体的引用。它的属性介于字体对象和字体规范之间：就像字体对象，与字体规范不同，它指的是单一的、特定的字体。与字体对象不同，创建字体实体不会将该字体的内容加载到计算机内存中。Emacs 可以从一个引用可缩放字体的字体实体打开多个不同大小的字体对象。

    Function: find-font font-spec &optional frame ¶

此函数返回与框架框架上的字体规范 `font-spec` 最匹配的字体实体。如果 `frame` 为 `nil` ，则默认为选定的帧。

    Function: list-fonts font-spec &optional frame num prefer ¶

此函数返回与字体规范 `font-spec` 匹配的所有字体实体的列表。

可选参数 `frame` ，如果非 `nil` ，指定显示字体的框架。可选参数 `num` ，如果非零，应该是一个整数，指定返回列表的最大长度。可选参数prefer，如果非零，应该是另一个字体规范，用于控制返回列表的顺序；返回的字体实体按与该字体规范的接近程度递减的顺序排序。

如果您调用 `set-face-attribute` 并将字体规范、字体实体或字体名称字符串作为 `:font` 属性的值传递，Emacs 会打开可用于显示的最佳匹配字体。然后它将相应的字体对象存储为该面的 `:font` 属性的实际值。

以下函数可用于获取有关字体的信息。对于这些函数，字体参数可以是字体对象、字体实体或字体规范。

    Function: font-get font property ¶

此函数返回字体的字体属性值。

如果 `font` 是字体规范并且字体规范没有指定属性，则返回值为 `nil` 。如果 `font` 是字体对象或字体实体， `:script` 属性的值可能是字体支持的脚本列表。

    Function: font-face-attributes font &optional frame ¶

此函数返回与字体对应的面属性列表。可选参数 `frame` 指定要显示字体的框架。如果为 `nil` ，则使用选定的帧。返回值的形式为

    (:family family :height height :weight weight
       :slant slant :width width)

其中family、height、weight、slant、width的值为面属性值。如果这些键-属性对中的某些未由字体指定，则它们可能会从列表中省略。

    Function: font-xlfd-name font &optional fold-wildcards ¶

此函数返回 `XLFD` （X 逻辑字体描述符），一个字符串，匹配字体。有关 `XLFD` 的信息，请参阅 `GNU Emacs` 手册中的字体。如果名称对于 `XLFD` 来说太长（最多可以包含 `255` 个字符），则函数返回 `nil` 。

如果可选参数 `fold-wildcards` 不为零，则 `XLFD` 中的连续通配符将折叠为一个。

以下两个函数返回有关字体的重要信息。

    Function: font-info name &optional frame ¶

此函数返回有关由其名称指定的字体的信息，一个字符串，因为它在框架上使用。如果 `frame` 被省略或为零，则默认为选定的框架。

函数返回的值是一个形式为[opened-name full-name size height baseline-offset relative-compose default-ascent max-width ascent descent space-width average-width filename capability]的向量。这是该向量的每个组件的描述：

    opened-name

用于打开字体的名称，一个字符串。

    full-name

字体的全名，一个字符串。

    size

字体的像素大小。

    height

字体的高度（以像素为单位）。

    baseline-offset

从 `ASCII` 基线的像素偏移量，正向上。

    relative-compose

    default-ascent

数字控制如何组成字符。

    max-width

字体的最大前进宽度。

    ascent

    descent

这种字体的上升和下降。这两个数字的总和应该等于上面的高度值。

    space-width

字体空格字符的宽度（以像素为单位）。

    average-width

字体字符的平均宽度。如果该值为零，则Emacs在计算显示的文本布局时使用 `space-width` 的值。

    filename

字体的文件名作为字符串。如果字体后端不提供查找字体文件名的方法，则此值可以为零。

    capability

一个列表，其第一个元素是表示字体类型的符号，x、opentype、truetype、type1、pcf 或 `bdf` 之一。对于 `OpenType` 字体，该列表包括 `2` 个附加元素，用于描述该字体支持的 `GSUB` 和 `GPOS` 功能。这些元素中的每一个都是 `((script (langsys feature ...) ...) ...)` 形式的列表，其中 `script` 是表示 `OpenType` 脚本标记的符号，langsys 是表示 `OpenType langsys` 标记的符号（或 `nil` ，代表默认的 `langsys` ），每个特征都是一个符号，代表一个 `OpenType` 特征标签。

    Function: query-font font-object ¶

此函数返回有关字体对象的信息。（这与 `font-info` 形成对比，它以字体名称、字符串作为参数。）

函数返回的值是 `[name filename pixel-size max-width ascent descent space-width average-width capability]` 形式的向量。这是该向量的每个组件的描述：

    name

字体名称，一个字符串。

    filename

字体的文件名作为字符串。如果字体后端不提供查找字体文件名的方法，则此值可以为零。

    pixel-size

用于打开字体的字体像素大小。

    max-width

字体的最大前进宽度。

    ascent

    descent

这种字体的上升和下降。这两个数字的总和给出了字体高度。

    space-width

字体空格字符的宽度（以像素为单位）。

    average-width

字体字符的平均宽度。如果该值为零，则Emacs在计算显示的文本布局时使用 `space-width` 的值。

    capability

一个列表，其第一个元素是表示字体类型的符号，x、opentype、truetype、type1、pcf 或 `bdf` 之一。对于 `OpenType` 字体，该列表包括 `2` 个附加元素，用于描述该字体支持的 `GSUB` 和 `GPOS` 功能。这些元素中的每一个都是 `((script (langsys feature ...) ...) ...)` 形式的列表，其中 `script` 是表示 `OpenType` 脚本标记的符号，langsys 是表示 `OpenType langsys` 标记的符号（或 `nil` ，代表默认的 `langsys` ），每个特征都是一个符号，代表一个 `OpenType` 特征标签。

以下四个函数返回有关各种面使用的字体的大小信息，允许在 `Lisp` 程序中考虑各种布局。这些函数会考虑面重新映射，如果有问题的面被重新映射，则返回有关重新映射的面的信息。请参阅面重映射。

    Function: default-font-width ¶

此函数返回当前缓冲区默认面所使用字体的平均宽度（以像素为单位），因为该面是为选定帧定义的。

    Function: default-font-height ¶

此函数返回当前缓冲区的默认面所使用的字体的高度（以像素为单位），因为该面是为选定帧定义的。

    Function: window-font-width &optional window face ¶

此函数返回窗口中面使用的字体的平均宽度（以像素为单位）。指定的窗口必须是活动窗口。如果 `nil` 或省略，window 默认为选中的窗口，face 默认为 `window` 中的默认面。

    Function: window-font-height &optional window face ¶

此函数返回窗口中面使用的字体的高度（以像素为单位）。指定的窗口必须是活动窗口。如果 `nil` 或省略，window 默认为选中的窗口，face 默认为 `window` 中的默认面。



## 40.13 条纹

在图形显示上，Emacs 在每个窗口旁边画出边缘：细的垂直条在侧面向下，可以显示位图，指示截断、继续、水平滚动等。



### 40.13.1 条纹尺寸和位置

以下缓冲区局部变量控制显示该缓冲区的窗口中边缘的位置和宽度。

    Variable: fringes-outside-margins ¶

边缘通常出现在显示边缘和窗口文本之间。如果该值为非零，它们将出现在显示边距之外。请参阅在边距中显示。

    Variable: left-fringe-width ¶

此变量，如果非零，则指定左边缘的宽度（以像素为单位）。nil 值表示使用窗口框架的左边缘宽度。

    Variable: right-fringe-width ¶

此变量，如果非零，则指定右边缘的宽度（以像素为单位）。nil 值表示使用窗口框架的右边缘宽度。

任何没有为这些变量指定值的缓冲区都使用由左边缘和右边缘帧参数指定的值（请参阅布局参数）。

上述变量实际上是通过函数 `set-window-buffer` （参见 `Buffers` 和 `Windows` ）生效的，该函数将 `set-window-fringes` 作为子例程调用。如果您更改其中一个变量，则在显示缓冲区的现有窗口中不会更新边缘显示，除非您在每个受影响的窗口中再次调用 `set-window-buffer` 。您还可以使用 `set-window-fringes` 来控制单个窗口中的边缘显示。

    Function: set-window-fringes window left &optional right outside-margins persistent ¶

该函数设置窗口窗口的边缘宽度。如果 `window` 为 `nil` ，则使用选定的窗口。

参数 `left` 指定左边缘的宽度（以像素为单位），右边缘也同样如此。任何一个的 `nil` 值都代表默认宽度。如果 `outside-margins` 不为零，则指定边缘应出现在显示边距之外。

如果窗口不够大，无法容纳所需宽度的边缘，则窗口边缘保持不变。

此处指定的值可以稍后通过在 `window` 上调用 `set-window-buffer` （请参阅缓冲区和 `Windows` ）及其 `keep-margins` 参数 `nil` 或省略来覆盖。但是，如果可选的第五个参数 `persistent` 为非 `nil` 并且其他参数已成功处理，则此处指定的值将无条件地在随后的 `set-window-buffer` 调用中存活。这可用于永久关闭 `minibuffer` 窗口中的边缘，请参阅 `set-window-scroll-bars` 的描述以获取示例（请参阅滚动条）。

    Function: window-fringes &optional window ¶

此函数返回有关窗口窗口边缘的信息。如果 `window` 被省略或为零，则使用选定的窗口。该值的形式为（left-width right-width outside-margins persistent）。



### 40.13.2 边缘指标

边缘指示器是显示在窗口边缘的小图标，用于指示截断或连续的行、缓冲区边界等。

    User Option: indicate-empty-lines ¶

当它不是 `nil` 时，Emacs 在图形显示的缓冲区末尾的每个空行的边缘显示一个特殊的字形。见边缘。这个变量在每个缓冲区中都是自动缓冲区局部的。

    User Option: indicate-buffer-boundaries ¶

此缓冲区局部变量控制缓冲区边界和窗口滚动在窗口边缘中的指示方式。

Emacs可以在屏幕上出现角度图标时指示缓冲区边界（即缓冲区中的第一行和最后一行）。此外，Emacs 可以在边缘显示一个向上箭头来表示屏幕上方有文本，以及一个向下箭头来显示屏幕下方有文本。

基本价值分为三种：

    nil

不要显示任何这些边缘图标。

    left

在左边缘显示角度图标和箭头。

    right

在右边缘显示角度图标和箭头。

    any non-alist

在左边缘显示角度图标，不显示箭头。

否则，该值应该是一个列表，指定要显示哪些边缘指标以及在哪里显示。alist 的每个元素都应具有 `(indicator . position)` 形式。这里，indicator 是 `top` 、bottom、up、down 和 `t` 之一（它涵盖了所有尚未指定的图标），而 `position` 是 `left` 、right 和 `nil` 之一。

例如， `((top . left) (t . right))` 将顶角位图放置在左边缘，将底角位图以及两个箭头位图放在右边缘。要在左边缘显示角度位图，而不是箭头位图，请使用 `((top . left) (bottom . left))` 。

    Variable: fringe-indicator-alist ¶

此缓冲区局部变量指定从逻辑边缘指示符到显示在窗口边缘中的实际位图的映射。该值是元素列表（indicator .bitmaps），其中indicator 指定逻辑指标类型，bitmaps 指定用于该指标的边缘位图。

每个指标应为以下符号之一：

    truncation, continuation.

用于截断和续行。

    up, down, top, bottom, top-bottom

当指示缓冲区边界为非零时使用：向上和向下表示位于窗口边缘上方或下方的缓冲区边界；top 和 `bottom` 表示最顶层和最底层的缓冲区文本行；top-bottom 表示缓冲区中只有一行文本的位置。

    empty-line

当指示空行为非零时，用于指示缓冲区结束后的空行。

    overlay-arrow

用于叠加箭头（请参阅叠加箭头）。

每个位图值可能是一个符号列表（left right [left1 right1]）。左右符号为特定指标指定显示在左边缘和/或右边缘的位图。left1 和 `right1` 特定于底部和顶部底部指示符，用于指示最后一个文本行没有最终换行符。或者，位图可以是用于左右边缘的单个符号。

有关标准位图符号的列表以及如何定义自己的符号，请参阅边缘位图。此外，nil 表示空位图（即未显示的指示符）。

当 `fringe-indicator-alist` 具有缓冲区本地值，并且没有为逻辑指标定义位图，或者位图为 `t` 时，使用来自 `fringe-indicator-alist` 的默认值的对应值。



### 40.13.3 边缘光标

当一行与窗口一样宽时，Emacs 将光标显示在右边缘，而不是使用两行。根据当前缓冲区的光标类型，使用不同的位图来表示边缘中的光标。

    User Option: overflow-newline-into-fringe ¶

如果这是非零，则与窗口宽度完全相同的行（不包括最后的换行符）不会继续。相反，当点位于行尾时，光标会出现在右边缘。

    Variable: fringe-cursor-alist ¶

此变量指定从逻辑光标类型到显示在右边缘的实际边缘位图的映射。该值是一个列表，其中每个元素都具有 `(cursor-type . bitmap)` 的形式，这意味着使用边缘位图位图来显示类型为 `cursor-type` 的光标。

每个光标类型应该是 `box` 、hollow、bar、hbar 或hollow-small 之一。前四个与光标类型框架参数中的含义相同（请参阅光标参数）。当普通的空心矩形位图太高而无法放在特定的显示行上时，使用空心小类型代替空心。

每个位图应该是一个符号，指定要为该逻辑光标类型显示的边缘位图。请参阅边缘位图。

当 `fringe-cursor-alist` 具有缓冲区本地值，并且没有为游标类型定义位图时，使用 `fringes-indicator-alist` 的默认值中的对应值。



### 40.13.4 边缘位图

边缘位图是实际位图，代表截断或连续行、缓冲区边界、覆盖箭头等的逻辑边缘指示符。每个位图由一个符号表示。这些符号由变量 `fringe-indicator-alist` 引用，它将边缘指示器映射到位图（请参阅边缘指示器），以及将边缘光标映射到位图的变量 `fringe-cursor-alist` （请参阅边缘光标）。

Lisp 程序也可以直接在左边缘或右边缘显示位图，方法是对出现在行中的字符之一使用显示属性（请参阅其他显示规范）。这样的显示规范具有以下形式

    (fringe bitmap [face])

边缘是符号左边缘或右边缘。位图是标识要显示的位图的符号。可选的面命名一个面，其前景色和背景色将用于显示位图，使用边缘面的属性来表示面未指定的颜色。如果 `face` 被省略，这意味着将默认面的属性用于边缘面未指定的颜色。对于不依赖于默认面和边缘面属性的可预测结果，我们建议您永远不要省略面，而始终提供特定面。特别是，如果您希望位图始终显示在边缘面上，请使用边缘作为面。

例如，要在左边缘显示一个箭头，使用警告面，您可以这样说：

    (overlay-put
     (make-overlay (point) (point))
     'before-string (propertize
    		 "x" 'display
    		 `(left-fringe right-arrow warning)))

这是Emacs中定义的标准边缘位图列表，以及它们当前在Emacs中的使用方式（通过 `fringe-indicator-alist` 和 `fringe-cursor-alist` ）：

    left-arrow, right-arrow

用于表示截断的行。

    left-curly-arrow, right-curly-arrow

用于表示连续行。

    right-triangle, left-triangle

前者由覆盖箭头使用。后者未使用。

    up-arrow, down-arrow

    bottom-left-angle, bottom-right-angle

    top-left-angle, top-right-angle

    left-bracket, right-bracket

    empty-line

用于指示缓冲区边界。

    filled-rectangle, hollow-rectangle

    filled-square, hollow-square

    vertical-bar, horizontal-bar

用于不同类型的边缘光标。

    exclamation-mark, question-mark

不被核心Emacs功能使用。

下一小节描述如何定义您自己的边缘位图。

    Function: fringe-bitmaps-at-pos &optional pos window ¶

该函数返回窗口窗口中包含位置 `pos` 的显示行的边缘位图。返回值的形式为 `(left right ov)` ，其中 `left` 是左边缘中边缘位图的符号（如果没有位图，则为 `nil` ），right 与右边缘相似，如果有则 `ov` 为非 `nil` 左边缘的叠加箭头。

如果 `pos` 在窗口中不可见，则该值为 `nil` 。如果 `window` 为 `nil` ，则表示选定的窗口。如果 `pos` 为 `nil` ，则表示窗口中点的值。



### 40.13.5 自定义边缘位图

    Function: define-fringe-bitmap bitmap bits &optional height width align ¶

此函数将符号位图定义为新的边缘位图，或用该名称替换现有位图。

参数 `bits` 指定要使用的图像。它应该是字符串或整数向量，其中每个元素（整数）对应于位图的一行。整数的每一位对应于位图的一个像素，其中低位对应于位图的最右边的像素。（请注意，此位顺序与 `XBM` 图像中的顺序相反；请参阅 `XBM` 图像。）

高度通常是位的长度。但是，您可以使用非零高度指定不同的高度。宽度通常为 `8` ，但您可以使用非零宽度指定不同的宽度。宽度必须是 `1` 到 `16` 之间的整数。

参数 `align` 指定位图相对于使用它的行范围的定位；默认值是使位图居中。允许的值为顶部、中心或底部。

align 参数也可以是一个列表（定期对齐），其中 `align` 的解释如上所述。如果periodic 不是nil，它指定位中的行应该重复足够的次数以达到指定的高度。

    Function: destroy-fringe-bitmap bitmap ¶

此函数破坏由位图标识的边缘位图。如果位图标识了标准边缘位图，它实际上恢复了该位图的标准定义，而不是完全消除它。

    Function: set-fringe-bitmap-face bitmap &optional face ¶

这将边缘位图位图的面设置为面。如果 `face` 为 `nil` ，则选择边缘面。位图的面控制绘制它的颜色。

face 与边缘面合并，因此通常 `face` 应仅指定前景色。



### 40.13.6 叠加箭头

覆盖箭头对于将用户的注意力引导到缓冲区中的特定行很有用。例如，在用于调试器接口的模式中，覆盖箭头指示将要执行的代码行。此功能与叠加无关（请参阅叠加）。

    Variable: overlay-arrow-string ¶

此变量保存要显示的字符串以引起对特定行的注意，如果未使用箭头功能，则为 `nil` 。在图形显示中，如果显示左边缘，则忽略字符串的内容；而是在显示区域左侧的边缘区域中显示一个字形。

    Variable: overlay-arrow-position ¶

此变量包含一个标记，指示在何处显示叠加箭头。它应该指向一行的开头。在非图形显示器上，或当未显示左边缘时，箭头文本出现在该行的开头，覆盖任何原本会出现的文本。由于箭头通常很短，并且行通常以缩进开头，因此通常不会覆盖任何重要内容。

如果该缓冲区中的覆盖箭头位置的值指向该缓冲区，则覆盖箭头字符串将显示在任何给定缓冲区中。因此，可以通过创建覆盖箭头位置的缓冲区本地绑定来显示多个覆盖箭头字符串。但是，使用overlay-arrow-variable-list 来实现此结果通常更简洁。

您可以通过使用 `before-string` 属性创建覆盖来完成类似的工作。请参见叠加属性。

您可以通过变量 `overlay-arrow-variable-list` 定义多个覆盖箭头。

    Variable: overlay-arrow-variable-list ¶

这个变量的值是一个变量列表，每个变量指定一个覆盖箭头的位置。变量 `overlay-arrow-position` 具有其正常含义，因为它在此列表中。

此列表中的每个变量都可以具有属性 `overlay-arrow-string` 和 `overlay-arrow-bitmap` 指定覆盖箭头字符串（用于文本终端或没有显示左边缘的图形终端）或边缘位图（用于具有左边缘的图形终端）显示在相应的叠加箭头位置。如果任一属性未设置，则使用默认的覆盖箭头字符串或覆盖箭头边缘指示符。



## 40.14 滚动条

通常框架参数vertical-scroll-bars控制框架中的窗口是否有垂直滚动条，以及它们是在左边还是右边。frame 参数 `scroll-bar-width` 指定它们的宽度（nil 表示默认值）。

框架参数 `Horizo​​ntal-scroll-bars` 控制框架中的窗口是否有水平滚动条。框架参数 `scroll-bar-height` 指定它们的高度（nil 表示默认值）。请参阅布局参数。

水平滚动条并非在所有平台上都可用。如果它们在您的系统上可用，则不带参数的函数 `Horizo​​ntal-scroll-bars-available-p` 返回非零。

以下三个函数将默认为所选帧的实时帧作为参数。

    Function: frame-current-scroll-bars &optional frame ¶

此函数报告框架框架的滚动条类型。该值是一个 `cons` 单元格（vertical-type . Horizo​​ntal-type），其中vertical-type 为left、right 或nil（表示没有垂直滚动条）。horizo​​ntal-type 为bottom 或nil（表示没有水平滚动条）滚动条）。

    Function: frame-scroll-bar-width &optional frame ¶

此函数返回帧的垂直滚动条的宽度（以像素为单位）。

    Function: frame-scroll-bar-height &optional frame ¶

此函数返回帧的水平滚动条的高度（以像素为单位）。

您可以使用以下函数覆盖单个窗口的框架特定设置：

    Function: set-window-scroll-bars window &optional width vertical-type height horizontal-type persistent ¶

此函数设置窗口窗口的宽度和/或高度以及滚动条的类型。如果 `window` 为 `nil` ，则使用选定的窗口。

width 以像素为单位指定垂直滚动条的宽度（nil 表示使用为框架指定的宽度）。vertical-type 指定是否有垂直滚动条，如果有，在哪里。可能的值是 `left` 、right、t，这意味着使用框架的默认值，nil 表示没有垂直滚动条。

height 以像素为单位指定水平滚动条的高度（nil 表示使用为框架指定的高度）。Horizo​​ntal-type 指定是否有水平滚动条。可能的值是bottom，t，表示使用框架的默认值，nil 表示没有水平滚动条。请注意，对于迷你窗口，值 `t` 与 `nil` 具有相同的含义，即不显示水平滚动条。您必须明确指定底部才能在迷你窗口中显示水平滚动条。

如果窗口不够大，无法容纳所需尺寸的滚动条，则相应的滚动条将保持不变。

此处指定的值可以稍后通过在 `window` 上调用 `set-window-buffer` （请参阅缓冲区和 `Windows` ）及其 `keep-margins` 参数 `nil` 或省略来覆盖。但是，如果可选的第五个参数 `persistent` 为非 `nil` 并且其他参数已成功处理，则此处指定的值将无条件地在随后的 `set-window-buffer` 调用中存活。

使用 `set-window-scroll-bars` 和 `set-window-fringes` 的持久参数（请参阅边缘大小和位置），您可以通过将以下代码段添加到您早期的init 文件（请参阅初始化文件）。

    (add-hook 'after-make-frame-functions
    	  (lambda (frame)
    	    (set-window-scroll-bars
    	     (minibuffer-window frame) 0 nil 0 nil t)
    	    (set-window-fringes
    	     (minibuffer-window frame) 0 0 nil t)))

以下四个函数将默认为所选窗口的实时窗口作为参数。

    Function: window-scroll-bars &optional window ¶

这个函数返回一个列表的形式（宽度列垂直类型高度线水平类型持久）。

值 `width` 是为垂直滚动条的宽度指定的值（可能为 `nil` ）；columns 是垂直滚动条实际占据的（可能是四舍五入的）列数。

值 `height` 是为水平滚动条的高度指定的值（可能为 `nil` ）；lines 是水平滚动条实际占用的（可能是四舍五入的）行数。

persistent 的值是最后一次成功调用 `set-window-scroll-bars` 时为 `window` 指定的值，如果从未有过，则为 `nil` 。

    Function: window-current-scroll-bars &optional window ¶

此函数报告窗口窗口的滚动条类型。该值是一个 `cons` 单元格（垂直类型。水平类型）。与 `window-scroll-bars` 不同，这会报告实际使用的滚动条类型，一旦考虑到框架默认值和滚动条模式。

    Function: window-scroll-bar-width &optional window ¶

此函数返回窗口垂直滚动条的宽度（以像素为单位）。

    Function: window-scroll-bar-height &optional window ¶

此函数返回窗口水平滚动条的高度（以像素为单位）。

如果不通过 `set-window-scroll-bars` 指定窗口的滚动条设置，缓冲区中的缓冲区局部变量 `vertical-scroll-bar` 、horizo​​ntal-scroll-bar、scroll-bar-width 和 `scroll-bar-height` 正在显示控制窗口的滚动条。函数 `set-window-buffer` 检查这些变量。如果在窗口中已经可见的缓冲区中更改它们，则可以通过调用 `set-window-buffer` 指定已显示的相同缓冲区来使窗口记录新值。

您可以通过设置以下变量来控制特定缓冲区的滚动条的外观，这些变量在设置时会自动变为缓冲区本地。

    Variable: vertical-scroll-bar ¶

此变量指定垂直滚动条的位置。可能的值是 `left` 、right、t，表示使用框架的默认值，nil 表示没有滚动条。

    Variable: horizontal-scroll-bar ¶

此变量指定水平滚动条的位置。可能的值是bottom，t，表示使用框架的默认值，nil 表示没有滚动条。

    Variable: scroll-bar-width ¶

此变量指定缓冲区垂直滚动条的宽度，以像素为单位。nil 值表示使用框架指定的值。

    Variable: scroll-bar-height ¶

此变量指定缓冲区的水平滚动条的高度，以像素为单位。nil 值表示使用框架指定的值。

最后，您可以通过自定义变量滚动条模式和水平滚动条模式来切换所有帧上滚动条的显示。

    User Option: scroll-bar-mode ¶

此变量控制是否以及在所有帧中放置垂直滚动条的位置。没有滚动条的可能值为 `nil` ，left 将滚动条放在左侧，right 将滚动条放在右侧。

    User Option: horizontal-scroll-bar-mode ¶

此变量控制是否在所有帧上显示水平滚动条。



## 40.15 窗口分隔线

窗口分隔线是在框架的窗口之间绘制的条。在窗口和右侧的任何相邻窗口之间绘制右分隔线。它的宽度（厚度）由框架参数 `right-divider-width` 指定。在底部或回波区域的窗口和相邻窗口之间绘制底部分隔线。它的宽度由框架参数bottom-divider-width 指定。在任何一种情况下，将宽度指定为零意味着不绘制此类分隔线。请参阅布局参数。

从技术上讲，右分隔线属于其左侧的窗口，这意味着它的宽度占该窗口的总宽度。底部分隔线属于它上面的窗口，这意味着它的宽度占该窗口的总高度。请参阅窗口大小。当窗口同时具有右分隔线和底部分隔线时，以底部分隔线为准。这意味着底部分隔线绘制在其窗口的整个总宽度上，而右侧分隔线在底部分隔线上方结束。

分隔线可以用鼠标拖动，因此对于用鼠标调整相邻窗口的大小很有用。当不存在滚动条或模式行时，它们还用于在视觉上分隔相邻的窗口。以下三个面允许自定义分隔线的外观：

    window-divider

当分隔线的宽度小于三个像素时，它会与这张脸的前景一起绘制。对于较大的分隔线，此面仅用于内部，不包括第一个和最后一个像素。

    window-divider-first-pixel

这是用于绘制至少三个像素宽的分隔线的第一个像素的面。要获得坚实的外观，请将其设置为与窗口分隔面相同的值。

    window-divider-last-pixel

这是用于绘制至少三个像素宽的分隔线的最后一个像素的面。要获得坚实的外观，请将其设置为与窗口分隔面相同的值。

您可以使用以下两个函数获取特定窗口的分隔线的大小。

    Function: window-right-divider-width &optional window ¶

返回窗口右分隔线的宽度（厚度）（以像素为单位）。window 必须是活动窗口，并且默认为选定的窗口。对于最右边的窗口，返回值始终为零。

    Function: window-bottom-divider-width &optional window ¶

返回窗口底部分隔线的宽度（厚度）（以像素为单位）。window 必须是活动窗口，并且默认为选定的窗口。对于 `minibuffer` 窗口或 `minibuffer-less` 帧上的最底部窗口，返回值为零。



## 40.16 display属性

显示文本属性（或覆盖属性）用于将图像插入到文本中，并控制文本显示方式的其他方面。display 属性的值应该是一个显示规范，或者是一个包含多个显示规范的列表或向量。相同显示属性值中的显示规范通常与它们所涵盖的文本同时应用。

如果多个源（覆盖和/或文本属性）为显示属性指定值，则只有一个值生效，遵循 `get-char-property` 规则。请参阅检查文本属性。

一些显示规范允许包含在显示时评估的 `Lisp` 表单。这在某些情况下可能是不安全的，例如，当显示规范是由某个外部程序/代理生成时。将显示规范包装在以特殊符号 `disable-eval` 开头的列表中，如 `('disable-eval spec)` 中所示，将禁用对规范中任何 `Lisp` 的评估，同时仍支持所有其他显示属性功能。

本节的其余部分描述了几种显示规格及其含义。



### 40.16.1 替换文本的显示规范

某些类型的显示规范指定要显示的内容而不是具有该属性的文本。这些被称为替换显示规范。Emacs 不允许用户以交互方式将点移动到以这种方式替换的缓冲区文本的中间。

如果显示规范列表包含多个替换显示规范，则第一个替换显示规范。更换显示器规格会使大多数其他显示器规格无关紧要，因为这些规格不适用于更换。

对于替换显示规范，具有属性的文本是指具有相同 `Lisp` 对象作为其显示属性的所有连续字符；这些字符被替换为一个单元。如果两个字符有不同的 `Lisp` 对象作为它们的显示属性（即，不是 `eq` 的对象），它们将分别处理。

这是一个说明这一点的例子。字符串用作替换显示规范，它将具有该属性的文本替换为指定的字符串（请参阅其他显示规范）。考虑以下函数：

    (defun foo ()
      (dotimes (i 5)
        (let ((string (concat "A"))
    	  (start (+ i i (point-min))))
          (put-text-property start (1+ start) 'display string)
          (put-text-property start (+ 2 start) 'display string))))

此函数为缓冲区中的前十个字符中的每一个提供了一个显示属性，即字符串 `A` ，但它们并不都获得相同的字符串对象。前两个字符获得相同的字符串对象，因此它们被替换为一个'A'；显示属性是在对 `put-text-property` 的两个单独调用中分配的这一事实无关紧要。类似地，接下来的两个字符得到第二个字符串（concat 创建一个新的字符串对象），因此它们被替换为一个 `'A'` ；等等。因此，十个字符显示为五个 `a` 。



### 40.16.2 指定空间

要显示指定宽度和/或高度的空间，请使用格式为 `(space.props)` 的显示规范，其中 `props` 是属性列表（交替的属性和值的列表）。您可以将此属性放在一个或多个连续字符上；显示指定高度和宽度的空格来代替所有这些字符。这些是您可以在 `props` 中使用的属性来指定空间的权重：

    :width width

如果宽度是一个数字，它指定空间宽度应该是宽度乘以正常字符宽度。width 也可以是像素宽度规范（请参阅空格的像素规范）。

    :relative-width factor

指定应从具有相同显示属性的连续字符组中的第一个字符计算拉伸的宽度。空格宽度是该字符的像素宽度乘以因子。（在文本模式终端上，字符的 `像素宽度` 通常为 `1` ，但对于制表符和双宽 `CJK` 字符可能更大。）

    :align-to hpos

指定空间应该足够宽以达到 `hpos` 。如果 `hpos` 是一个数字，它以正常字符宽度为单位进行测量。hpos 也可以是像素宽度规范（请参阅空间的像素规范）。

您应该使用上述属性中的一个且仅一个。您还可以使用以下属性指定空间的高度：

    You should use one and only one of the above properties. You can also specify the height of the space, with these properties:

指定空间的高度。如果 `height` 是一个数字，它指定空间高度应该是高度乘以正常字符高度。高度也可以是像素高度规范（请参阅空间的像素规范）。

    :height height

指定空间的高度，将具有此显示规范的文本的普通高度乘以因子。

    :relative-height factor

如果 `ascent` 的值是不大于 `100` 的非负数，则指定空间高度的上升百分比应视为空间的上升——即基线以上的部分。上升也可以使用像素上升规范以像素单位指定（请参阅空间的像素规范）。

不要同时使用 `:height` 和 `:relative-height` 。

    :ascent ascent

请注意，空格属性被视为段落分隔符，以便对双向文本进行重新排序以进行显示。有关详细信息，请参阅双向显示。



### 40.16.3 以像素为单位指定间隔

在屏幕的重新绘制过程中， `=:width=` 、 `=:align-to=` 、 `=:height=` 和 `=:ascent=` 属性值具有特殊含义。它们求值后的结果将成为文本的真正像素数。

它们的值中支持以下表达式：

    expr ::= num | (num) | unit | elem | pos | image | xwidget | form
    num  ::= integer | float | symbol
    unit ::= in | mm | cm | width | height
    
    elem ::= left-fringe | right-fringe | left-margin | right-margin
          |  scroll-bar | text
    pos  ::= left | center | right
    form ::= (num . expr) | (op expr ...)
    op   ::= + | -

对于表达式 `=expr=` ，在使用 `=num=` 形式时，代表着该值为字符实际长宽与默认长宽的分数比； `=(num)=` 形式则代表字符的绝对像素数。如果 `num` 是符号 `=symbol=` ，则使用该符号在当前缓冲区（ `=buffer=` ）的变量值，这个值可以是一个数字，也可以是上述形式的 `=cons cell=` 单元（包括那种第一项（ `=car=` ）绑定有变量值的 `=cons cell=` 单元）。

以 `=in=` 、 `=mm=` 和 `=cm=` 为单位意味着指定每英寸、毫米和厘米的像素数；以 `=width=` 和 `=height=` 为单位意味着指定对应于当前 `=face=` 的默认宽度和高度的值； `=(image . props)=` 形式的图像规范（请参阅[图像格式](https://github.com/advanceflow/Elisp/blob/main/40-Emacs显示.org#40171-图像格式)）用于指定图像 `=image=` 的宽度或高度。同理， `=(xwidget . props)=` 形式的 `xwidget` 规范可以指定 `xwidget` 的宽度或高度（请参阅[嵌入式原生组件](https://github.com/advanceflow/Elisp/blob/main/40-Emacs显示.org#4018-嵌入式原生小部件)）。

`left-fringe` （左边框）、 `=right-fringe=` （右边框）、 `=left-margin=` （左边距）、 `=right-margin=` （右边距）、 `=scroll-bar=` （滚动栏）和 `=text=` （文字区域）六种元素可用于指定窗口相应区域的的宽度。当窗口行号显示行号（ `=display-line-numbers=` ）时（请参阅[显示文本的大小](https://github.com/advanceflow/Elisp/blob/main/40-Emacs显示.org#4010-显示文本的大小)），文本区域的宽度将减去显示行号所占用的屏幕空间。

在 `=:align-to=` 中，可以使用 `=left=` 、 `=right=` 和 `=center=` 三种位置属性来指定一个相对于文本区域的左边缘、右边缘或是中心的位置。当窗口显示行号时，考虑到显示行号所占用的屏幕空间， `=left=` 和 `=center=` 位置的值将发生偏移。

除 `=text=` 外，上述所有窗口元素都可以和 `=:align-to=` 一起使用，以表示该位置是相对于该区域的左边缘的。如果已经设置了相对位置的基本偏移量（通过这类符号的第一次出现），再次出现的这类符号的值被当作指定区域的宽度。举个例子，要对齐到左边距的中心，可以使用：

    :align-to (+ left-margin (0.5 . left-margin))

如果没有特别设置基本偏移的话，文本对齐将默认相对于文本区域的左侧边缘。例如，给标题行设置 `' =:align-to 0= '` 将使它与文本区域的文字的第一列对齐。当窗口显示行号时，文本开始的位置将会是行号显示空间的结尾。

`(num . expr)` 形式的值代表拼合 `=num=` 和 `=expr=` 。例如， `=(2 . in)=` 代表 `2` 英寸的宽度， `=(0.5 . image)=` 代表图像 `=image=` 的一半宽或高度。

`(+ expr ...)` 将表达式的值相加； `=(- expr ...)=` 形式否定表达式或将表达式的值相减。



### 40.16.4 其它显示属性值

以下是您可以在 `=display=` 属性中使用的其它属性值。

-   **`string`:** 显示字符串 `=string=` ，而不是被附着该属性的文本。 `/` （即文本显示替换——译者注）/
    
    不支持递归指定——即给字符串 `=string=` 指定 `=display=` 属性（即使指定了，它们也不会被使用）。

-   **`(image . image-props)`:** 这种属性值代表一个图像规范（请参阅[图像格式](https://github.com/advanceflow/Elisp/blob/main/40-Emacs显示.org#40171-图像格式)）。当它被用在 `=display=` 属性上时，拥有该属性的文本将显示为此图像。

-   **`(slice x y width height)`:** 在与 `=image=` 一起使用时，该属性值指定要显示的图片的一个“切片”（即一部分区域）。其中元素 `=x=` 和 `=y=` 用于指定切片的左上角相对于图像的位置； `=width=` 和 `=height=` 指定切片的宽度和高度。指定整数值代表像素数，指定 `0.0` –1.0 范围内的浮点数代表占整个图像的宽度或高度的分数比。

-   **`((margin nil) string)`:** 这种属性值代表在被附着该属性文本的位置显示字符串 `=string=` ，而不是文本本身。它与使用 `=string=` 的效果基本相同，不同之处在于它是被当作边距（ `=margin=` ）显示的一种特殊情况来完成的（请参阅[在边距中显示](https://github.com/advanceflow/Elisp/blob/main/40-Emacs显示.org#40165-在边缘显示)）。

-   **`(left-fringe bitmap [face])` `(right-fringe bitmap [face])`:** 对一行内的任意字符指定此属性值，将在该行的左或右边缘（ `=fringe=` ）显示位图 `=bitmap=` ，并不显示被附着该属性的字符。可选的 `face` 类型参数 `=face=` 的颜色将被用作该位图的颜色。详细信息，请参阅[边缘位图](https://github.com/advanceflow/Elisp/blob/main/40-Emacs显示.org#40134-边缘位图)。 `/` （在 `=truncate-lines=` 下屏幕左右的小箭头就是这种 `=bitmap=` ——译者注）/

-   **`(space-width factor)`:** 该显示属性值将影响被附着文本中的所有空格字符。它将这些空格的显示宽度放大 `=factor=` 倍。 `=factor=` 应为整数或浮点数。空格以外的字符完全不受影响；特别地，该属性对制表符没有影响。

-   **`(height height)`:** 此显示属性值可使文本变得更高或更矮。以下是 `/height/` 的可能值：
    -   **`(+ n)`:** 这代表使用比原先大 `=n=` 级的字体。 `/` 一级/ 的大小由可用的字体集定义——具体来说，是那些在除高度之外的所有属性上，都与该文本所使用的字体相匹配的字体。有这种字体可用的每一个尺寸都被算作是一级。 `=n=` 应该是一个整数。
    
    -   **`(- n)`:** 这代表使用比原先小 `n` 级的字体。
    
    -   **一个数字 ~=factor=:** ~ 数字 `=factor=` 表示使用的字体是默认字体的 `=factor=` 倍高。
    
    -   **一个符号 ~=function=:** ~ 符号 `=symbol=` 将被当作是一个计算高度的函数。它将被传入当前高度作为参数，并应返回要使用的新高度。
    
    -   其它任意表达式 ~:: 
        
        ~ 如果 `/height/` 的值不属于上述情况，则它将被视为是一个表达式。Emacs 将对它求值以获得新的高度。此时符号 `=height=` 绑定的值是当前已指定的字体高度。

-   `(raise factor)`
    
    这种属性值将以本行的基线为标准，纵向抬高或降低被附着文本。它主要是用于支持下标和上标的显示。
    
    `factor` 必须是一个数字，将作为被附着的文本，其高度的倍数。如果它是正数，则抬升字符；如果是负数，则降低字符。
    
    请注意，如果在指定该属性值之前（即在该属性左侧），这段文本已经拥有 `=height=` 属性值，则后者（ `=height=` ）将对抬升或降低的像素量产生影响，因为抬升与降低是基于被抬升的文本的高度的。因此，如果要显示小于正常文本高度的下标或上标，请考虑在指定 `=height=` 之前指定 `=raise=` 。

您可以使任何显示属性值有条件地生效。要做到这一点，请将属性值包在一个格式为 `=(when condition . spec)=` 的列表中，属性值 `=spec=` 将仅在 `=condition=` 的值为非 `=nil=` 时适用。在求值过程中，符号 `=object=` 将被绑定到被指定该显示属性的字符串或缓冲区。符号 `=position=` 和 `=buffer-position=` 分别绑定到 `=object=` 内的位置和 `=display=` 属性在缓冲区中被找到的位置。当 `=object=` 是字符串时，这两个位置可以不同。

请注意，仅当重新显示（ `=redisplay=` ）进行到此显示属性值所在的文本时，才会对 `/condition/` 求值，因此该功能最适合在相对稳定的条件下使用——即，在特定的缓冲区位置，每次求值都会产生相同的结果。如果在文本的位置相同的情况下，求值的结果发生了变化——例如，求值结果会被光标的位置影响——那么，这种条件指定可能不会像你期望的那样工作：因为重新显示（ `=redisplay=` ）只会检查那些（自上次显示周期以来）被认为发生了变化的缓冲区文本。



### 40.16.5 在边缘显示

缓冲区可以在左侧和右侧具有称为显示边距的空白区域。普通文本永远不会出现在这些区域，但您可以使用 `display` 属性将内容放入显示边距。目前没有办法使边缘中的文本或图像对鼠标敏感。

在边距中显示某些内容的方法是在某些文本的 `display` 属性中的边距显示规范中指定它。这是一个替换显示规范，这意味着您放置的文本不会显示；边距显示出现，但该文本没有出现。

边距显示规范看起来像 `((margin right-margin) spec)` 或 `((margin left-margin) spec)` 。在这里，spec 是另一个显示规范，它说明要在边距中显示什么。通常它是要显示的文本字符串或图像描述符。

要在与某些缓冲区文本相关联的边距中显示某些内容，而不更改或阻止该文本的显示，请在文本上放置一个前字符串属性并将边距显示规范放置在前字符串的内容上。

请注意，如果要在边距中显示的字符串未指定面，则使用与在文本区域中显示的字符串相同的规则和优先级来确定其面（请参阅显示面）。如果这导致不希望的面 `泄漏` 到边距中，请确保字符串具有为其指定的明确面。

在显示边距可以显示任何内容之前，您必须给它们一个非零宽度。通常的方法是设置这些变量：

    Variable: left-margin-width ¶

此变量指定左边距的宽度，以字符单元（也称为 `列` ）为单位。它在所有缓冲区中都是缓冲区本地的。值为 `nil` 表示没有左边缘区域。

    Variable: right-margin-width ¶

此变量指定右边距的宽度，以字符单元为单位。它在所有缓冲区中都是缓冲区本地的。值为 `nil` 表示没有右边缘区域。

设置这些变量不会立即影响窗口。当窗口中显示新缓冲区时，会检查这些变量。因此，您可以通过调用 `set-window-buffer` 使更改生效。不要使用这些变量来尝试确定左边距或右边距的当前宽度。相反，使用函数窗口边距。

您也可以立即设置边距宽度。

    Function: set-window-margins window left &optional right ¶

此函数指定窗口窗口的边距宽度，以字符单元为单位。参数 `left` 控制左边距，而 `right` 控制右边距（默认为 `0` ）。

如果窗口不够大，无法容纳所需宽度的边距，则窗口的边距保持不变。

此处指定的值可以稍后通过在 `window` 上调用 `set-window-buffer` （请参阅缓冲区和 `Windows` ）及其 `keep-margins` 参数 `nil` 或省略来覆盖。

    Function: window-margins &optional window ¶

此函数将窗口左右边距的宽度作为表单的一个 `cons` 单元格返回 `(left . right)` 。如果两个边缘区域之一不存在，则其宽度返回为零；如果两个边距都不存在，则函数返回 `(nil)` 。如果 `window` 为 `nil` ，则使用选定的窗口。



## 40.17 图像

要在Emacs缓冲区中显示图像，您必须首先创建一个图像描述符，然后将其用作所显示文本的显示属性中的显示说明符（请参阅显示属性）。

Emacs 在图形终端上运行时通常能够显示图像。图像不能显示在文本终端、某些不支持此功能的图形终端上，或者如果Emacs编译时没有图像支持。您可以使用函数 `display-images-p` 来确定原则上是否可以显示图像（请参阅显示功能测试）。



### 40.17.1 图像格式

Emacs 可以显示许多不同的图像格式。仅当安装了特定的支持库时才支持其中一些图像格式。在某些平台上，Emacs 可以按需加载支持库；如果是这样，变量 `dynamic-library-alist` 可用于修改这些动态库的已知名称集。请参阅动态加载的库。

支持的图像格式（以及所需的支持库）包括 `PBM` 和 `XBM` （它们不依赖于支持库并且始终可用）、XPM (libXpm)、GIF (libgif 或 `libungif)` 、JPEG (libjpeg)、TIFF (libtiff)、 `PNG (libpng)` 和 `SVG (librsvg)` 。

这些图像格式中的每一种都与图像类型符号相关联。上述格式的符号分别为 `pbm` 、xbm、xpm、gif、jpeg、tiff、png 和 `svg` 。

此外，如果您使用 `ImageMagick (libMagickWand)` 支持构建 `Emacs` ，Emacs 可以显示 `ImageMagick` 可以显示的任何图像格式。请参阅 `ImageMagick` 图像。通过 `ImageMagick` 显示的所有图像都有类型符号 `imagemagick` 。

    Variable: image-types ¶

此变量包含当前配置中可能支持的图像格式的类型符号列表。

`可能` 意味着Emacs知道图像类型，不一定知道它们可以使用（例如，它们可能依赖于不可用的动态库）。要知道哪些图像类型真正可用，请使用 `image-type-available-p` 。

    Function: image-type-available-p type ¶

如果可以加载和显示类型类型的图像，则此函数返回非 `nil` 。type 必须是图像类型符号。

对于支持库静态链接的图像类型，此函数始终返回 `t` 。对于动态加载支持库的图像类型，如果可以加载库，则返回 `t` ，否则返回 `nil` 。



### 40.17.2 图像描述符

图像描述符是一个列表，它指定图像的基础数据以及如何显示它。它通常用作显示覆盖或文本属性的值（请参阅其他显示规范）；但请参阅显示图像，以获取将图像插入缓冲区的便捷辅助函数。

每个图像描述符都具有 `(image.props)` 形式，其中 `props` 是交替的关键字符号和值的属性列表，至少包括指定图像类型的一对 `:type type` 。

定义图像尺寸的图像描述符，:width, :height, :max-width 和 `:max-height` ，可以采用整数，表示以像素为单位的尺寸，或一对 `(value.em)` ，其中 `value` 是尺寸的ems26 中的长度。一个 `em` 相当于字体的高度，值可以是整数或浮点数。

以下是对所有图像类型有意义的属性列表（还有一些属性仅对某些图像类型有意义，如以下小节所述）：

    :type type

图像类型。请参阅图像格式。每个图像描述符都必须包含此属性。

    :file file

这表示从文件文件加载图像。如果 `file` 不是绝对文件名，它会相对于 `data-directory` 的 `images` 子目录进行扩展，如果失败，则相对于 `x-bitmap-file-path` 列出的目录（请参阅面属性）。

    :data data

这指定了原始图像数据。每个图像描述符必须具有 `:data` 或 `:file` ，但不能同时具有。

对于大多数图像类型， `:data` 属性的值应该是包含图像数据的字符串。某些图像类型不支持 `:data; ~ 对于其他一些人，仅 ~:data` 是不够的，因此您需要将其他图像属性与 `:data` 一起使用。有关详细信息，请参阅以下小节。

    :margin margin

这指定了在图像周围添加多少像素作为额外边距。值 `margin` 必须是一个非负数，或一对 `(x . y)` 这样的数字。如果是一对，x 指定水平添加多少像素，y 指定垂直添加多少像素。如果 `:margin` 未指定，则默认为零。

    :ascent ascent

这指定了用于上升的图像高度量，即基线以上的部分。值 `ascent` 必须是 `0` 到 `100` 范围内的数字，或符号中心。

如果 `ascent` 是一个数字，则图像高度的该百分比用于其上升。

如果 `ascent` 为中心，则图像以中心线为中心垂直居中，该中心线将是在图像位置绘制的文本的垂直中心线，其方式由应用于图像的文本属性和叠加层指定。

如果省略此属性，则默认为 `50` 。

    :relief relief

这会在图像周围添加一个阴影矩形。浮雕值指定阴影线的宽度，以像素为单位。如果浮雕为负，则绘制阴影，使图像显示为按下的按钮；否则，它会显示为未按下的按钮。

    :width width, :height height

:width 和 `:height` 关键字用于缩放图像。如果仅指定其中一个，则将计算另一个以保持纵横比。如果两者都指定，则可能不会保留纵横比。

    :max-width max-width, :max-height max-height

如果图像的大小超过这些值，则使用 `:max-width` 和 `:max-height` 关键字进行缩放。如果设置了:width，它将优先于max-width，如果设置了:height，它将优先于max-height，但是您可以根据需要混合这些关键字。

如果 `:max-width` 和 `:height` 都指定了，但 `:width` 没有指定，则保留纵横比可能需要宽度超过 `:max-width` 。如果发生这种情况，缩放将使用较小的高度值，以便在不超过 `:max-width` 的情况下保持纵横比。同样，当同时指定 `:max-height` 和 `:width` 时，但没有指定 `:height` 。例如，如果你有一个 `200x100` 的图像并指定 `:width` 应该是 `400` 并且 `:max-height` 应该是 `150` ，你最终会得到一个 `300x150` 的图像： `~ 保留纵横比并且不超过 ~max` 设置.  这种参数组合是表示 `尽可能大地显示此图像，但不大于可用显示区域` 的有用方式。

    :scale scale

这应该是一个数字，其中大于 `1` 的值意味着增加尺寸，低于 `1` 意味着通过乘以宽度和高度来减小尺寸。例如，值 `0.25` 将使图像大小变为原来的四分之一。如果缩放使图像大于 `:max-width` 或 `:max-height` 指定的大小，则生成的大小不会超过这两个值。如果 `:scale` 和 `:height/:width` 都指定了，则高度/宽度将根据指定的缩放因子进行调整。

    :rotation angle

以度为单位指定旋转角度。仅支持 `90` 度的倍数，除非图像类型是 `imagemagick` 。正值顺时针旋转，负值逆时针旋转。在缩放和裁剪之后执行旋转。

    :transform-smoothing smooth

如果这是 `t` ，则任何图像变换都将应用平滑；如果为零，则不会应用平滑。使用的确切算法取决于平台，但应该等效于双线性过滤。禁用平滑将使用最近邻算法。

如果未指定此属性，create-image 将使用 `image-transform-smoothing` 用户选项来说明是否应该进行缩放。此选项可以是 `nil` （不平滑）、t（使用平滑）或使用图像对象作为唯一参数调用的谓词函数，并且应该返回 `nil` 或 `t` 。默认值是缩小以应用平滑，而大规模放大则不应用平滑。

    :index frame

请参阅多帧图像。

    :conversion algorithm

这指定了在图像显示之前应该应用于图像的转换算法；值，algorithm，指定哪种算法。

    laplace

    emboss

指定拉普拉斯边缘检测算法，该算法会模糊颜色的微小差异，同时突出较大的差异。人们有时认为这对于显示禁用按钮的图像很有用。

    (edge-detection :matrix matrix :color-adjust adjust) ¶

指定通用边缘检测算法。矩阵必须是九元素列表或九元素数字向量。变换图像中位置 `x/y` 处的像素是根据该位置周围的原始像素计算的。矩阵为 `x/y` 邻域中的每个像素指定一个因子，该像素将影响变换后的像素；元素 `0` 指定 `x-1/y-1` 处像素的因子，元素 `1` 指定 `x/y-1` 处像素的因子等，如下所示：

    (x-1/y-1  x/y-1  x+1/y-1
     x-1/y    x/y    x+1/y
     x-1/y+1  x/y+1  x+1/y+1)

生成的像素是根据将周围像素的 `RGB` 值相加得到的颜色的颜色强度计算得出的，乘以指定的因子，然后将该总和除以因子绝对值的总和。

拉普拉斯边缘检测目前使用的矩阵

    (1  0  0
     0  0  0
     0  0 -1)

浮雕边缘检测使用矩阵

    ( 2 -1  0
     -1  0  1
      0  1 -2)

    disabled

指定转换图像以使其看起来被禁用。

    :mask mask

如果 `mask` 是启发式或（启发式 `bg` ），则为图像构建一个剪贴蒙版，以便在图像后面可以看到帧的背景。如果未指定 `bg` ，或者如果 `bg` 为 `t` ，则通过查看图像的四个角来确定图像的背景颜色，假设角中最常出现的颜色是图像的背景颜色。否则， `bg` 必须是一个列表（红绿蓝），指定图像背景的颜色。

如果掩码为 `nil` ，则从图像中删除掩码（如果有掩码）。某些格式的图像包含一个掩码，可以通过指定 `:mask nil` 来删除该掩码。

    :pointer shape

这指定鼠标指针悬停在该图像上时的指针形状。有关可用的指针形状，请参阅指针形状。

    :map map ¶

这将热点的图像映射与该图像相关联。

图像映射是一个列表，其中每个元素都具有格式（区域 `id plist` ）。区域被指定为矩形、圆形或多边形。

矩形是一个 `cons (rect . ((x0 . y0) . (x1 . y1)))` ，它指定矩形区域左上角和右下角的像素坐标。

圆是一个 `cons (circle . ((x0 . y0) . r))` ，它指定圆的中心和半径；r 可以是浮点数或整数。

多边形是一个 `cons (poly . [x0 y0 x1 y1 ...])` ，其中向量中的每一对都描述了多边形中的一个角。

当鼠标指针位于图像的热点区域时，查询该热点的plist；如果它包含 `help-echo` 属性，则定义热点的工具提示，如果它包含指针属性，则定义鼠标光标在热点上时的形状。有关可用的指针形状，请参阅指针形状。

当鼠标指针在热点上点击鼠标时，将热点的id和鼠标事件组合成一个事件；例如，如果热点的 `id` 是 `area4` ，则为 `[area4 mouse-1]` 。

    Function: image-mask-p spec &optional frame ¶

如果图像规范具有掩码位图，则此函数返回 `t` 。frame 是显示图像的框架。frame nil 或省略表示使用选定的帧（请参阅输入焦点）。

    Function: image-transforms-p &optional frame ¶

如果 `frame` 支持图像缩放和旋转，此函数返回非 `nil` 。frame nil 或省略表示使用选定的帧（请参阅输入焦点）。返回的列表包括指示支持哪些图像转换操作的符号：

规模

     帧通过 `:scale` 、:width、:height、:max-width 和 `:max-height` 属性支持图像缩放。
旋转90

如果旋转角度是 `90` 度的整数倍，则框架支持图像旋转。

如果不支持图像变换，:rotation, :crop, :width, :height, :scale, :max-width 和 `:max-height` 将只能通过 `ImageMagick` 使用，如果可用的话（参见 `ImageMagick` 图像）。

脚注
(26)

在排版中，em 是与字体高度相等的距离。例如，当使用 `12` 点类型时，1 em 等于 `12` 点。它的使用确保距离和类型保持成比例。



### 40.17.3 XBM 图像

要使用 `XBM` 格式，请将 `xbm` 指定为图像类型。这种图像格式不需要外部库，因此始终支持这种类型的图像。

xbm 图像类型支持的其他图像属性包括：

    :foreground foreground

值 `foreground` 应该是指定图像前景色的字符串，或者默认颜色为 `nil` 。此颜色用于 `XBM` 中为 `1` 的每个像素。默认为框架的前景色。

    :background background

值 `background` 应该是指定图像背景颜色的字符串，或者默认颜色为 `nil` 。此颜色用于 `XBM` 中为 `0` 的每个像素。默认为框架的背景颜色。

如果您使用Emacs中的数据而不是外部文件指定 `XBM` 映像，请使用以下三个属性：

    :data data

值 `data` 指定图像的内容。您可以为数据使用三种格式：

字符串向量或布尔向量，每个指定图像的一行。请指定 `:height` 和 `:width` 。
包含与 `XBM` 文件相同的字节序列的字符串。在这种情况下，您不能指定 `:height` 和 `:width` ，因为省略它们表示数据具有 `XBM` 文件的格式。文件内容指定图像的高度和宽度。
包含图像位的字符串或布尔向量（可能在末尾加上一些不会使用的额外位）。它应该至少包含 `stride * height` 位，其中 `stride` 是大于或等于图像宽度的 `8` 的最小倍数。在这种情况下，您应该指定 `:height` 、:width 和 `:stride` 来指示字符串仅包含位而不是整个 `XBM` 文件，并指定图像的大小。

    :width width

值 `width` 指定图像的宽度，以像素为单位。

    :height height

值 `height` 指定图像的高度，以像素为单位。

请注意，只有在传入未指定宽度和高度的数据（例如，包含图像位的字符串或向量）时才能使用 `:width` 和 `:height` 。XBM 文件通常会自行指定，在这些文件上使用这两个属性是错误的。另请注意，大多数其他图像格式使用 `:width` 和 `:height` 来指定显示的图像应该是什么，这通常意味着执行某种缩放。XBM 图像不支持此功能。

    :stride stride

每行存储的布尔向量条目数；大于或等于宽度的 `8` 的最小倍数。



### 40.17.4 XPM 图像

要使用 `XPM` 格式，请将 `xpm` 指定为图像类型。附加的图像属性 `:color-symbols` 对于 `xpm` 图像类型也有意义：

    :color-symbols symbols

值，symbols，应该是一个 `alist` ，其元素的格式为 `(name.color)` 。在每个元素中，name 是出现在图像文件中的颜色名称，而 `color` 指定用于显示该名称的实际颜色。



### 40.17.5 ImageMagick 图像

如果您的Emacs构建支持 `ImageMagick` ，您可以使用 `ImageMagick` 库来加载许多图像格式（请参阅 `GNU Emacs` 手册中的文件便利）。通过 `ImageMagick` 加载的图像的图像类型符号是 `imagemagick` ，与实际的底层图像格式无关。

要检查 `ImageMagick` 支持，请使用以下命令：

    (image-type-available-p 'imagemagick)

    Function: imagemagick-types ¶

此函数返回当前 `ImageMagick` 安装支持的图像文件扩展名列表。每个列表元素都是一个符号，表示图像类型的内部 `ImageMagick` 名称，例如 `.bmp` 图像的 `BMP` 。

    User Option: imagemagick-enabled-types ¶

这个变量的值是一个 `ImageMagick` 图像类型的列表，Emacs 可能会尝试使用 `ImageMagick` 渲染这些图像类型。每个列表元素应该是 `imagemagick-types` 返回的列表中的符号之一，或等效的字符串。或者，值 `t` 为所有可能的图像类型启用 `ImageMagick` 。不管这个变量的值是什么，imagemagick-types-inhibit（见下文）优先。

    User Option: imagemagick-types-inhibit ¶

此变量的值列出了永远不应使用 `ImageMagick` 渲染的 `ImageMagick` 图像类型，无论 `imagemagick-enabled-types` 的值如何。t 值完全禁用 `ImageMagick` 。

    Variable: image-format-suffixes ¶

此变量是一个将图像类型映射到文件扩展名的列表。Emacs 将它与 `:format image` 属性（见下文）结合使用，为 `ImageMagick` 库提供有关图像类型的提示。每个元素都有形式（类型扩展名），其中类型是指定图像内容类型的符号，扩展名是指定相关文件扩展名的字符串。

使用 `ImageMagick` 加载的图像支持以下附加图像描述符属性：

    :background background

背景，如果非零，应该是一个指定颜色的字符串，如果图像支持透明度，则用作图像的背景颜色。如果值为 `nil` ，则默认为框架的背景颜色。

    :format type

值 `type` 应该是指定图像数据类型的符号，如 `image-format-suffixes` 中所示。这在图像没有关联文件名时使用，以向 `ImageMagick` 提供提示以帮助其检测图像类型。

    :crop geometry

geometry 的值应该是一个形式的列表（宽高 `xy` ）。width 和 `height` 指定裁剪图像的宽度和高度。如果 `x` 是正数，则它指定裁剪区域与原始图像左侧的偏移量，如果为负数，则指定从右侧的偏移量。如果 `y` 是正数，它指定从原始图像顶部的偏移量，如果是负数，则从底部开始。如果 `x` 或 `y` 为 `nil` 或未指定，则裁剪区域将以原始图像为中心。

如果裁剪区域位于图像边缘或与图像边缘重叠，则会缩小以排除图像之外的任何区域。这意味着不能使用 `:crop` 通过输入较大的宽度或高度值来增加图像的大小。

裁剪在缩放之后但在旋转之前执行。



### 40.17.6 SVG 图像

SVG（可缩放矢量图形）是一种用于指定图像的 `XML` 格式。SVG 图像支持以下附加图像描述符属性：

    :foreground foreground

foreground，如果非零，应该是一个指定颜色的字符串，用作图像的前景色。如果值为 `nil` ，则默认为当前面的前景色。

    :background background

背景，如果非零，应该是一个指定颜色的字符串，如果图像支持透明度，则用作图像的背景颜色。如果值为 `nil` ，则默认为当前面的背景颜色。

    :css css

css，如果非零，应该是一个字符串，指定 `CSS` 以覆盖生成图像时使用的默认 `CSS` 。

    SVG library

如果您的Emacs构建支持 `SVG` ，您可以使用 `svg.el` 库中的以下函数创建和操作这些图像。

    Function: svg-create width height &rest args ¶

创建具有指定尺寸的新的空 `SVG` 图像。args 是一个参数 `plist` ，您可以指定以下内容：

    :stroke-width

创建的任何线条的默认宽度（以像素为单位）。

    :stroke

创建的任何线条上的默认笔触颜色。

此函数返回一个 `SVG` 对象，一个指定 `SVG` 图像的 `Lisp` 数据结构，并且以下所有函数都在该结构上工作。以下函数中的参数 `svg` 指定了这样一个 `SVG` 对象。

    Function: svg-gradient svg id type stops ¶

在 `svg` 中创建一个带有标识符 `id` 的渐变。type 指定渐变类型，可以是线性的或径向的。stop 是百分比/颜色对的列表。

下面将创建一个线性渐变，从开始时的红色到 `25%` 的绿色，最后到蓝色：

    (svg-gradient svg "gradient1" 'linear
    	      '((0 . "red") (25 . "green") (100 . "blue")))

创建的渐变（并插入到 `SVG` 对象中）稍后可以被所有创建形状的函数使用。

以下所有函数都采用关键字参数的可选列表，这些参数会改变各种属性的默认值。有效属性包括：

    :stroke-width

绘制的线条的宽度（以像素为单位），以及实体形状周围的轮廓。

    :stroke-color

绘制线条的颜色，以及实体形状周围的轮廓。

    :fill-color

用于实体形状的颜色。

    :id

形状的标识。

    :gradient

如果给定，这应该是先前定义的渐变对象的标识符。

    :clip-path

剪辑路径的标识符。

    Function: svg-rectangle svg x y width height &rest args ¶

向 `svg` 添加一个矩形，其左上角位于 `x/y` 位置，其大小为宽度/高度。

    (svg-rectangle svg 100 100 500 500 :gradient "gradient1")

    Function: svg-circle svg x y radius &rest args ¶

向 `svg` 添加一个圆心位于 `x/y` 且半径为半径的圆。

    Function: svg-ellipse svg x y x-radius y-radius &rest args ¶

向 `svg` 添加一个椭圆，其中心位于 `x/y` ，水平半径为 `x` 半径，垂直半径为 `y` 半径。

    Function: svg-line svg x1 y1 x2 y2 &rest args ¶

向 `svg` 添加一条从 `x1/y1` 开始并延伸到 `x2/y2` 的线。

    Function: svg-polyline svg points &rest args ¶

向 `svg` 添加一条穿过点的多段线（又名 `折线` ），它是 `X/Y` 位置对的列表。

    (svg-polyline svg '((200 . 100) (500 . 450) (80 . 100))
    	      :stroke-color "green")

    Function: svg-polygon svg points &rest args ¶

向 `svg` 添加一个多边形，其中 `points` 是描述多边形外周的 `X/Y` 对列表。

    (svg-polygon svg '((100 . 100) (200 . 150) (150 . 90))
    	     :stroke-color "blue" :fill-color "red")

    Function: svg-path svg commands &rest args ¶

根据命令将形状的轮廓添加到 `svg` ，请参阅 `SVG` 路径命令。

默认情况下，坐标是绝对的。要使用相对于最后一个位置的坐标，或者 `-` 最初 `-` 相对于原点，请将属性 `:relative` 设置为 `t` 。可以为函数或单个命令指定此属性。如果为函数指定，则默认情况下所有命令都使用相对坐标。要使单个命令使用绝对坐标，请将 `:relative` 设置为 `nil` 。

    (svg-path svg
    	  '((moveto ((100 . 100)))
    	    (lineto ((200 . 0) (0 . 200) (-200 . 0)))
    	    (lineto ((100 . 100)) :relative nil))
    	  :stroke-color "blue"
    	  :fill-color "lightblue"
    	  :relative t)

    Function: svg-text svg text &rest args ¶

将指定的文本添加到 `svg` 。

    (svg-text
     svg "This is a text"
     :font-size "40"
     :font-weight "bold"
     :stroke "black"
     :fill "white"
     :font-family "impact"
     :letter-spacing "4pt"
     :x 300
     :y 400
     :stroke-width 1)

    Function: svg-embed svg image image-type datap &rest args ¶

将嵌入（光栅）图像添加到 `svg` 。如果 `datap` 为 `nil` ，则 `image` 应该是文件名；否则它应该是一个包含图像数据作为原始字节的字符串。image-type 应该是 `MIME` 图像类型，例如 `image/jpeg` 。

    (svg-embed svg "~/rms.jpg" "image/jpeg" nil
    	   :width "100px" :height "100px"
    	   :x "50px" :y "75px")

    Function: svg-embed-base-uri-image svg relative-filename &rest args ¶

要 `svg` 添加放置在相对文件名的嵌入（光栅）图像。在 `:base-uri svg` 图像属性的文件名目录中搜索相对文件名。:base-uri 指定要创建的 `svg` 图像的（可能不存在的）文件名，因此所有嵌入的文件都相对于 `:base-uri` 文件名的目录进行搜索。如果省略 `:base-uri` ，则使用加载 `svg` 图像的文件名。与 `svg-embed` 相比，使用 `:base-uri` 提高了嵌入大图像的性能，因为所有工作都直接由 `librsvg` 完成。

    ;; Embeding /tmp/subdir/rms.jpg and /tmp/another/rms.jpg
    (svg-embed-base-uri-image svg "subdir/rms.jpg"
    	   :width "100px" :height "100px"
    	   :x "50px" :y "75px")
    (svg-embed-base-uri-image svg "another/rms.jpg"
    	   :width "100px" :height "100px"
    	   :x "75px" :y "50px")
    (svg-image svg :scale 1.0
    	   :base-uri "/tmp/dummy"
    	   :width 175 :height 175)

    Function: svg-clip-path svg &rest args ¶

为 `svg` 添加剪切路径。如果通过 `:clip-path` 属性应用于形状，则不会绘制该形状中位于剪切路径之外的部分。

    (let ((clip-path (svg-clip-path svg :id "foo")))
      (svg-circle clip-path 200 200 175))
    (svg-rectangle svg 50 50 300 300
    	       :fill-color "red"
    	       :clip-path "url(#foo)")

    Function: svg-node svg tag &rest args ¶

将自定义节点标签添加到 `svg` 。

    (svg-node svg
    	  'rect
    	  :width 300 :height 200 :x 50 :y 100 :fill-color "green")

    Function: svg-remove svg id ¶

从 `svg` 中删除具有标识符 `id` 的元素。

    Function: svg-image svg ¶

最后，svg-image 将一个 `SVG` 对象作为其参数，并返回一个适合在 `insert-image` 等函数中使用的图像对象。

这是一个完整的示例，它创建并插入带有圆圈的图像：

    (let ((svg (svg-create 400 400 :stroke-width 10)))
      (svg-gradient svg "gradient1" 'linear '((0 . "red") (100 . "blue")))
      (svg-circle svg 200 200 100 :gradient "gradient1"
    		  :stroke-color "green")
      (insert-image (svg-image svg)))

SVG 路径命令
SVG 路径允许通过组合直线、曲线、圆弧和其他基本形状来创建复杂的图像。下面描述的函数允许从 `Lisp` 程序调用 `SVG` 路径命令。

    Command: moveto points ¶

将笔移动到点的第一个点。附加点用线连接。points 是 `X/Y` 坐标对的列表。随后的 `moveto` 命令表示新子路径的开始。

    (svg-path svg '((moveto ((200 . 100) (100 . 200) (0 . 100))))
    	  :fill "white" :stroke "black")

    Command: closepath ¶

通过将当前子路径连接回其初始点来结束当前子路径。沿着连接绘制一条线。

    (svg-path svg '((moveto ((200 . 100) (100 . 200) (0 . 100)))
    		(closepath)
    		(moveto ((75 . 125) (100 . 150) (125 . 125)))
    		(closepath))
    	  :fill "red" :stroke "black")

    Command: lineto points ¶

从当前点到点的第一个元素画一条线，一个 `X/Y` 位置对的列表。如果指定了多个点，则绘制一条折线。

    (svg-path svg '((moveto ((200 . 100)))
    		(lineto ((100 . 200) (0 . 100))))
    	  :fill "yellow" :stroke "red")

    Command: horizontal-lineto x-coordinates ¶

从当前点到 `x` 坐标中的第一个元素绘制一条水平线。指定多个坐标是可能的，尽管这通常没有意义。

    (svg-path svg '((moveto ((100 . 200)))
    		(horizontal-lineto (300)))
    	  :stroke "green")

    Command: vertical-lineto y-coordinates ¶

画垂直线。

    (svg-path svg '((moveto ((200 . 100)))
    		(vertical-lineto (300)))
    	  :stroke "green")

    Command: curveto coordinate-sets ¶

使用坐标集中的第一个元素，从当前点绘制三次贝塞尔曲线。如果有多个坐标集，绘制一个多边形。每个坐标集都是 `(x1 y1 x2 y2 xy)` 形式的列表，其中 `(x, y)` 是曲线的终点。(x1, y1) 和 `(x2, y2)` 分别是开始和结束的控制点。

    (svg-path svg '((moveto ((100 . 100)))
    		(curveto ((200 100 100 200 200 200)
    			  (300 200 0 100 100 100))))
    	  :fill "transparent" :stroke "red")

    Command: smooth-curveto coordinate-sets ¶

使用坐标集中的第一个元素，从当前点绘制三次贝塞尔曲线。如果有多个坐标集，绘制一个多边形。每个坐标集是一个 `(x2 y2 xy)` 形式的列表，其中 `(x, y)` 是曲线的终点， `(x2, y2)` 是对应的控制点。第一个控制点是上一个命令的第二个控制点相对于当前点的反映，如果该命令是curveto 或smooth-curveto。否则第一个控制点与当前点重合。

    (svg-path svg '((moveto ((100 . 100)))
    		(curveto ((200 100 100 200 200 200)))
    		(smooth-curveto ((0 100 100 100))))
    	  :fill "transparent" :stroke "blue")

    Command: quadratic-bezier-curveto coordinate-sets ¶

使用坐标集中的第一个元素，从当前点绘制二次贝塞尔曲线。如果有多个坐标集，绘制一个多边形。每个坐标集是一个 `(x1 y1 xy)` 形式的列表，其中 `(x, y)` 是曲线的终点， `(x1, y1)` 是控制点。

    (svg-path svg '((moveto ((200 . 100)))
    		(quadratic-bezier-curveto ((300 100 300 200)))
    		(quadratic-bezier-curveto ((300 300 200 300)))
    		(quadratic-bezier-curveto ((100 300 100 200)))
    		(quadratic-bezier-curveto ((100 100 200 100))))
    	  :fill "transparent" :stroke "pink")

    Command: smooth-quadratic-bezier-curveto coordinate-sets ¶

使用坐标集中的第一个元素，从当前点绘制二次贝塞尔曲线。如果有多个坐标集，绘制一个多边形。每个坐标集是一个 `(xy)` 形式的列表，其中 `(x, y)` 是曲线的终点。控制点是前一个命令的控制点相对于当前点的反映，如果该命令是 `quadratic-bezier-curveto` 或 `smooth-quadratic-bezier-curveto` 。否则控制点与当前点重合。

    (svg-path svg '((moveto ((200 . 100)))
    		(quadratic-bezier-curveto ((300 100 300 200)))
    		(smooth-quadratic-bezier-curveto ((200 300)))
    		(smooth-quadratic-bezier-curveto ((100 200)))
    		(smooth-quadratic-bezier-curveto ((200 100))))
    	  :fill "transparent" :stroke "lightblue")

    Command: elliptical-arc coordinate-sets ¶

使用坐标集中的第一个元素，从当前点绘制一条椭圆弧。如果有多个坐标集，则绘制一系列椭圆弧。每个坐标集是一个形式为 `(rx ry xy)` 的列表，其中 `(x, y)` 是椭圆的终点， `(rx, ry)` 是它的半径。属性可以附加到列表中：

    :x-axis-rotation

椭圆的 `x` 轴相对于当前坐标系的 `x` 轴旋转的角度（以度为单位）。

    :large-arc

如果设置为 `t` ，则绘制大于或等于 `180` 度的弧形扫描。否则，绘制小于或等于 `180` 度的圆弧扫描。

    :sweep

如果设置为 `t` ，则在正角度方向绘制圆弧。否则，以负角方向绘制。

    
    
    (svg-path svg '((moveto ((200 . 250)))
    		(elliptical-arc ((75 75 200 350))))
    	  :fill "transparent" :stroke "red")
    (svg-path svg '((moveto ((200 . 250)))
    		(elliptical-arc ((75 75 200 350 :large-arc t))))
    	  :fill "transparent" :stroke "green")
    (svg-path svg '((moveto ((200 . 250)))
    		(elliptical-arc ((75 75 200 350 :sweep t))))
    	  :fill "transparent" :stroke "blue")
    (svg-path svg '((moveto ((200 . 250)))
    		(elliptical-arc ((75 75 200 350 :large-arc t
    				     :sweep t))))
    	  :fill "transparent" :stroke "gray")
    (svg-path svg '((moveto ((160 . 100)))
    		(elliptical-arc ((40 100 80 0)))
    		(elliptical-arc ((40 100 -40 -70
    				     :x-axis-rotation -120)))
    		(elliptical-arc ((40 100 -40 70
    				     :x-axis-rotation -240))))
    	  :stroke "pink" :fill "lightblue"
    	  :relative t)



### 40.17.7 其他图像类型

对于 `PBM` 图像，请指定图像类型 `pbm` 。支持彩色、灰度和单色图像。对于单声道 `PBM` 图像，支持两个额外的图像属性。

    :foreground foreground

值 `foreground` 应该是指定图像前景色的字符串，或者默认颜色为 `nil` 。此颜色用于 `PBM` 中为 `1` 的每个像素。默认为帧的前景色。

    :background background

值 `background` 应该是指定图像背景颜色的字符串，或者默认颜色为 `nil` 。此颜色用于 `PBM` 中为 `0` 的每个像素。默认为框架的背景颜色。

Emacs 可以支持的其余图像类型有：

    GIF

图像类型 `gif` 。支持 `:index` 属性。请参阅多帧图像。

    JPEG

图像类型 `jpeg` 。

    PNG

图片类型.png

    TIFF

图像类型 `tiff` 。支持 `:index` 属性。请参阅多帧图像。



### 40.17.8 定义图像

函数 `create-image` 、defimage 和 `find-image` 提供了创建图像描述符的便捷方法。

    Function: create-image file-or-data &optional type data-p &rest props ¶

此函数创建并返回一个使用文件或数据中数据的图像描述符。file-or-data 可以是文件名或包含图像数据的字符串；对于前一种情况，data-p 应为 `nil` ，对于后一种情况，应为非 `nil` 。

可选参数类型是指定图像类型的符号。如果 `type` 被省略或为零，create-image 会尝试根据文件的前几个字节或文件名来确定图像类型。

其余参数 `props` 指定附加图像属性，例如，

    (create-image "foo.xpm" 'xpm nil :heuristic-mask t)

如果不支持这种类型的图像，该函数将返回 `nil` 。否则它返回一个图像描述符。

    Macro: defimage symbol specs &optional doc ¶

该宏将符号定义为图像名称。参数 `specs` 是一个列表，它指定如何显示图像。第三个参数 `doc` 是一个可选的文档字符串。

specs 中的每个参数都具有属性列表的形式，并且每个参数都应至少指定 `:type` 属性和 `:file` 或 `:data` 属性。:type 的值应该是指定图像类型的符号，:file 的值是要从中加载图像的文件，而 `:data` 的值是包含实际图像数据的字符串。这是一个例子：

    (defimage test-image
      ((:type xpm :file "~/test1.xpm")
       (:type xbm :file "~/test1.xbm")))

defimage 一个一个地测试每个参数，看它是否可用——也就是说，是否支持该类型并且文件是否存在。第一个可用参数用于生成存储在符号中的图像描述符。

如果没有一个替代方案有效，则符号被定义为 `nil` 。

    Function: image-property image property ¶

返回图像中属性的值。可以使用 `setf` 设置属性。将属性设置为 `nil` 将从图像中删除该属性。

    Function: find-image specs ¶

此功能提供了一种方便的方法来查找满足图像规范列表中的一个的图像。

specs 中的每个规范都是一个属性列表，其内容取决于图像类型。所有规范必须至少包含属性 `:type type` 和 `:file file` 或 `:data data` ，其中 `type` 是指定图像类型的符号，例如 `xbm` ，file 是从中加载图像的文件，data 是字符串包含实际的图像数据。列表中支持类型且文件存在的第一个规范用于构造要返回的图像规范。如果不满足任何规范，则返回 `nil` 。

图像在图像加载路径中查找。

    User Option: image-load-path ¶

此变量的值是搜索图像文件的位置列表。如果元素是字符串或值为字符串的变量符号，则该字符串被视为要搜索的目录的名称。如果元素是一个变量符号，其值为一个列表，则将其视为要搜索的目录列表。

默认是在data-directory指定的目录的images子目录中搜索，然后是data-directory指定的目录，最后是load-path中的目录。子目录不会自动包含在搜索中，因此如果将图像文件放在子目录中，则必须明确提供子目录。例如，要在数据目录中查找图像 `images/foo/bar.xpm` ，您应该按如下方式指定图像：

    (defimage foo-image '((:type xpm :file "foo/bar.xpm")))

    Function: image-load-path-for-library library image &optional path no-error ¶

此函数为 `Lisp` 包库使用的图像返回合适的搜索路径。

该函数首先使用 `image-load-path` 搜索图像，不包括 `data-directory/images` ，然后在 `load-path` 中搜索适合库的路径，包括 `../../etc/images` 和 `../ etc/images` 相对于库文件本身，最后在 `data-directory/images` 中。

然后此函数返回一个目录列表，其中首先包含找到图像的目录，然后是 `load-path` 的值。如果给出了路径，则使用它来代替加载路径。

如果 `no-error` 不是 `nil` 并且找不到合适的路径，则不要发出错误信号。相反，像以前一样返回目录列表，除了 `nil` 出现在图像目录的位置。

以下是使用 `image-load-path-for-library` 的示例：

    (defvar image-load-path) ; shush compiler
    (let* ((load-path (image-load-path-for-library
    		    "mh-e" "mh-logo.xpm"))
           (image-load-path (cons (car load-path)
    			      image-load-path)))
      (mh-tool-bar-folder-buttons-init))

基于图像缩放因子变量创建图像时，图像会自动缩放。该值可以是浮点数（其中大于 `1` 的数字表示增加大小，而小于 `1` 表示缩小大小）或符号 `auto` ，它将根据字体像素大小计算缩放因子。



### 40.17.9 显示图像

您可以通过自己设置显示属性来使用图像描述符，但使用本节中的函数更容易。

    Function: insert-image image &optional string area slice ¶

此函数将图像插入到当前缓冲区中的点。值图像应该是图像描述符；它可以是 `create-image` 返回的值，也可以是用 `defimage` 定义的符号的值。参数字符串指定要放入缓冲区以保存图像的文本。如果省略或为零，则 `insert-image` 默认使用 ~ ~ 。

参数 `area` 指定是否将图像放在边距中。如果是left-margin，图像出现在左边距；right-margin 指定右边距。如果 `area` 为 `nil` 或省略，则图像显示在缓冲区文本内的某个点。

参数 `slice` 指定要插入的图像切片。如果 `slice` 为 `nil` 或省略，则插入整个图像。（但是，请注意，图像在窗口的右边缘显示时会被截断，因为不支持环绕图像。）否则， `slice` 是一个列表（xy 宽度高度），它指定图像区域的 `x` 和 `y` 位置以及宽度和高度插入。整数值以像素为单位。0.0–1.0 范围内的浮点数代表整个图像的宽度或高度的一部分。

在内部，这个函数在缓冲区中插入字符串，并给它一个指定图像的显示属性。请参阅显示属性。

    Function: insert-sliced-image image &optional string area rows cols ¶

该函数将图像插入当前缓冲区中的点，如 `insert-image` ，但将图像拆分为 `rowsxcols` 大小相等的切片。

Emacs将每个切片显示为单独的图像，并允许更直观的向上/向下滚动，而不是在通过显示（大）图像的缓冲区分页时向上/向下跳转整个图像。

    Function: put-image image pos &optional string area ¶

此函数将图像 `image` 放在当前缓冲区中 `pos` 的前面。参数 `pos` 应该是一个整数或一个标记。它指定图像应该出现的缓冲区位置。参数字符串指定应保存图像的文本作为默认值的替代。

参数 `image` 必须是一个图像描述符，可能由 `create-image` 返回或由 `defimage` 存储。

参数 `area` 指定是否将图像放在边距中。如果是left-margin，图像出现在左边距；right-margin 指定右边距。如果 `area` 为 `nil` 或省略，则图像显示在缓冲区文本内的某个点。

在内部，此函数创建一个叠加层，并为其提供一个包含文本的前字符串属性，该文本具有一个显示属性，其值为图像。（哇！）

    Function: remove-images start end &optional buffer ¶

此函数删除位置 `start` 和 `end` 之间的缓冲区中的图像。如果缓冲区被省略或为零，图像将从当前缓冲区中删除。

这只会删除像 `put-image` 那样放入缓冲区的图像，而不是使用 `insert-image` 或其他方式插入的图像。

    Function: image-size spec &optional pixels frame ¶

此函数将图像的大小作为一对（宽度.高度）返回。spec 是图像规范。非零像素表示返回以像素为单位测量的大小，否则返回以帧的默认字符大小测量的大小（请参阅帧字体）。frame 是显示图像的框架。frame nil 或省略表示使用选定的帧（请参阅输入焦点）。

    Variable: max-image-size ¶

此变量用于定义Emacs将加载的图像的最大大小。Emacs 将拒绝加载（和显示）任何大于此限制的图像。

如果该值为整数，则直接指定图像的最大高度和宽度，以像素为单位。如果是浮点数，它指定最大图像高度和宽度作为帧高度和宽度的比率。如果该值为非数字，则对图像的大小没有明确限制。

这个变量的目的是防止不合理的大图像被意外加载到Emacs中。它仅在第一次加载图像时生效。一旦图像被放置在图像缓存中，它就可以一直显示，即使 `max-image-size` 的值随后发生了变化（参见图像缓存）。

使用上述插入功能插入的图像还会在跨越显示图像的文本属性（或覆盖）中安装本地键映射。此键盘映射定义了以下命令：

    +

增加图像尺寸（image-increase-size）。前缀值 `4` 表示将大小增加 `40%` 。默认值为 `20%` 。

    -

减小图像尺寸 `(image-increase-size)` 。前缀值 `4` 表示将大小减小 `40%` 。默认值为 `20%` 。

    r

将图像顺时针旋转 `90` 度（图像旋转）。前缀表示逆时针旋转 `90` 度。

    o

将图像保存到文件（图像保存）。



### 40.17.10 多帧图像

一些图像文件可以包含多个图像。我们说图像中有多个 `帧` 。目前，Emacs 支持 `GIF` 、TIFF 和某些 `ImageMagick` 格式（如 `DJVM` ）的多帧。

帧可用于表示多个页面（例如，多帧 `TIFF` 文件通常是这种情况），或创建动画（通常是多帧 `GIF` 文件的情况）。

多帧图像有一个属性 `:index` ，它的值是一个整数（从 `0` 开始计数），它指定正在显示的帧。

    Function: image-multi-frame-p image ¶

如果图像包含多于一帧，此函数返回非零。实际返回值是一个 `cons (nimages . delay)` ，其中 `nimages` 是帧数，delay 是它们之间的延迟（以秒为单位），如果图像没有指定延迟，则返回 `nil` 。旨在进行动画处理的图像通常指定帧延迟，而旨在被视为多页的图像则没有。

    Function: image-current-frame image ¶

该函数返回图像当前帧号的索引，从 `0` 开始计数。

    Function: image-show-frame image n &optional nocheck ¶

此函数将图像切换到帧号 `n` 。它将有效范围之外的帧号替换为范围末尾的帧号，除非 `nocheck` 为非零。如果图像不包含具有指定编号的帧，则图像显示为空心框。

    Function: image-animate image &optional index limit ¶

此功能为图像设置动画。可选的整数索引指定开始的帧（默认为 `0` ）。可选参数限制控制动画的长度。如果省略或为零，则图像仅动画一次；如果 `t` 它永远循环；如果数字动画在这么多秒后停止。

动画通过计时器进行操作。请注意，Emacs 施加了 `0.01 (image-minimum-frame-delay)` 秒的最小帧延迟。如果图像本身没有指定延迟，Emacs 使用 `image-default-frame-delay` 。

    Function: image-animate-timer image ¶

此函数返回负责动画图像的计时器（如果有）。



### 40.17.11 图像缓存

Emacs 缓存图像，以便它可以更有效地再次显示它们。当Emacs显示图像时，它会在图像缓存中搜索与所需规格相等的现有图像规格。如果找到匹配项，则显示缓存中的图像。否则，Emacs 会正常加载图像。

    Function: image-flush spec &optional frame ¶

此函数从帧帧的图像缓存中删除具有规范规范的图像。使用 `equal` 比较图像规格。如果 `frame` 为 `nil` ，则默认为选定的帧。如果 `frame` 为 `t` ，则在所有现有帧上刷新图像。

在Emacs当前的实现中，每个图形终端都拥有一个图像缓存，该缓存由该终端上的所有帧共享（请参阅多个终端）。因此，刷新一帧中的图像也会刷新同一终端上的所有其他帧中的图像。

image-flush 的一个用途是告诉Emacs一个图像文件的变化。如果图像规范包含 `:file` 属性，则图像在首次显示时根据文件的内容进行缓存。即使文件随后发生更改，Emacs 也会继续显示旧版本的图像。调用 `image-flush` 从缓存中刷新图像，迫使Emacs在下次需要显示该图像时重新读取该文件。

图像刷新的另一个用途是节省内存。如果您的 `Lisp` 程序在比 `image-cache-eviction-delay` 更短的时间内创建了大量临时图像（见下文），您可以选择自己刷新未使用的图像，而不是等待Emacs自动执行。

    Function: clear-image-cache &optional filter ¶

此函数清除图像缓存，删除存储在其中的所有图像。如果过滤器被省略或为零，它将清除所选帧的缓存。如果过滤器是一个帧，它会清除该帧的缓存。如果 `filter` 为 `t` ，则清除所有图像缓存。否则，过滤器将被视为文件名，并且与该文件名关联的所有图像都将从所有图像缓存中删除。

如果图像缓存中的图像在指定的时间段内没有显示，Emacs 会将其从缓存中删除并释放相关的内存。

    Variable: image-cache-eviction-delay ¶

此变量指定图像可以保留在缓存中而不显示的秒数。当图像在这段时间内未显示时，Emacs 会将其从图像缓存中删除。

在某些情况下，如果缓存中的图像数量增长过大，实际的驱逐延迟可能会比这更短。

如果值为 `nil` ，Emacs 不会从缓存中删除图像，除非您明确清除它。此模式可用于调试。

    Function: image-cache-size ¶

此函数返回当前图像缓存的总大小，以字节为单位。例如，大小为 `200x100` 、每种颜色 `24` 位的图像将具有 `60000` 字节的缓存大小。



## 40.18 嵌入式原生小部件

当Emacs使用必要的支持库构建并在图形终端上运行时，Emacs 能够在Emacs缓冲区中显示本机小部件，例如 `GTK+ WebKit` 小部件。要测试Emacs是否支持显示嵌入式小部件，请检查 `xwidget-internal` 功能是否可用（请参阅功能）。

要在缓冲区中显示嵌入的小部件，您必须首先创建一个 `xwidget` 对象，然后将该对象用作显示文本或覆盖属性中的显示说明符（请参阅显示属性）。

    Function: make-xwidget type title width height arguments &optional buffer ¶

这将创建并返回一个 `xwidget` 对象。如果缓冲区被省略或为零，则默认为当前缓冲区。如果缓冲区命名一个不存在的缓冲区，它将被创建。type 标识 `xwidget` 组件的类型，它可以是以下之一：

网络套件

WebKit 组件。

width 和 `height` 参数以像素为单位指定小部件的大小，并且 `title` 是一个字符串，指定其标题。

    Function: xwidgetp object ¶

如果 `object` 是 `xwidget` ，此函数返回 `t` ，否则返回 `nil` 。

    Function: xwidget-plist xwidget ¶

该函数返回 `xwidget` 的属性列表。

    Function: set-xwidget-plist xwidget plist ¶

此函数将 `xwidget` 的属性列表替换为 `plist` 给出的新属性列表。

    Function: xwidget-buffer xwidget ¶

该函数返回 `xwidget` 的缓冲区。

    Function: get-buffer-xwidgets buffer ¶

此函数返回与缓冲区关联的 `xwidget` 对象列表，可以将其指定为缓冲区对象或现有缓冲区的名称、字符串。如果缓冲区不包含 `xwidget` ，则该值为 `nil` 。

    Function: xwidget-webkit-goto-uri xwidget uri ¶

此函数浏览给定 `xwidget` 中的指定 `uri` 。uri 是一个字符串，用于指定文件或 `URL` 的名称。

    Function: xwidget-webkit-execute-script xwidget script ¶

此函数使 `xwidget` 指定的浏览器小部件执行指定的 `JavaScript` 脚本。

    Function: xwidget-webkit-execute-script-rv xwidget script &optional default ¶

这个函数像 `xwidget-webkit-execute-script` 一样执行指定的脚本，但它也将脚本的返回值作为字符串返回。如果脚本没有返回值，则此函数返回默认值，如果省略默认值，则返回 `nil` 。

    Function: xwidget-webkit-get-title xwidget ¶

此函数将 `xwidget` 的标题作为字符串返回。

    Function: xwidget-resize xwidget width height ¶

此函数将指定 `xwidget` 的大小调整为宽度 `x` 高度像素。

    Function: xwidget-size-request xwidget ¶

此函数将所需大小的 `xwidget` 作为表单列表返回（宽度高度）。尺寸以像素为单位。

    Function: xwidget-info xwidget ¶

此函数将 `xwidget` 的属性作为 `[type title width height]` 形式的向量返回。属性通常在创建 `xwidget` 时由 `make-xwidget` 确定。

    Function: set-xwidget-query-on-exit-flag xwidget flag ¶

此功能允许您安排Emacs在退出之前或在终止与 `xwidget` 关联的缓冲区之前要求用户确认。如果 `flag` 不为 `nil` ，Emacs 会查询用户，否则不会。

    Function: xwidget-query-on-exit-flag xwidget ¶

此函数返回 `xwidgets query-on-exit` 标志的当前设置，t 或 `nil` 。



## 40.19 按钮

Button 包定义了用于插入和操作可以用鼠标或键盘命令激活的按钮的函数。这些按钮通常用于各种超链接。

按钮本质上是一组文本或覆盖属性，附加到缓冲区中的一段文本。这些属性称为按钮属性。这些属性之一，action 属性，指定当用户使用键盘或鼠标调用按钮时调用的函数。动作函数可以检查按钮并根据需要使用它的其他属性。

在某些方面，Button 包复制了 `Widget` 包中的功能。请参阅Emacs小部件库中的介绍。Button 包的优点是它更快、更小、更易于编程。从用户的角度来看，这两个包产生的界面非常相似。



### 40.19.1 按钮属性

每个按钮都有一个相关的属性列表，定义其外观和行为，其他任意属性可用于特定应用目的。以下属性对 `Button` 包具有特殊意义：

    action ¶

当用户调用按钮时调用的函数，该函数被传递给单个参数按钮。默认情况下，这是忽略，它什么都不做。

    mouse-action ¶

这类似于动作，并且当存在时，将用于代替由鼠标单击引起的按钮调用的动作（而不是用户点击 `RET` ）。如果不存在，则鼠标单击将使用操作。

    face ¶

这是一个Emacs界面，用于控制此类按钮的显示方式；默认情况下，这是按钮面。

    mouse-face ¶

这是在鼠标悬停期间控制外观的附加面（与通常的按钮面合并）；默认情况下，这是通常的Emacs高亮面。

    keymap ¶

按钮的键盘映射，定义在按钮区域内活动的绑定。默认情况下，这是通常的按钮区域键盘映射，存储在变量 `button-map` 中，它定义了 `RET` 和 `mouse-2` 来调用按钮。

    type ¶

按钮类型。请参阅按钮类型。

    help-echo ¶

Emacs工具提示帮助系统显示的字符串；默认情况下， `鼠标 ~2` ，RET：按下此按钮~ 。或者，返回要显示的字符串或 `nil` 的函数或计算结果的表单。有关详细信息，请参阅文本帮助回显。

该函数使用三个参数调用，window、object 和 `pos` 。第二个参数 `object` 是具有属性的覆盖（对于覆盖按钮），或者是包含按钮的缓冲区（对于文本属性按钮）。其他参数与特殊文本属性 `help-echo` 的含义相同。

    follow-link ¶

follow-link 属性，定义鼠标 `1` 单击在此按钮上的行为方式，请参阅定义可单击文本。

    button ¶

所有按钮都有一个非零按钮属性，这可能有助于查找包含按钮的文本区域（这是标准按钮功能所做的）。

还为按钮中的文本区域定义了其他属性，但这些属性通常对典型用途并不感兴趣。



### 40.19.2 按钮类型

每个按钮都有一个按钮类型，它定义了按钮属性的默认值。按钮类型按层次结构排列，特殊类型继承自更通用的类型，因此很容易为特定任务定义特殊用途的按钮类型。

    Function: define-button-type name &rest properties ¶

定义一个名为 `name` （一个符号）的按钮类型。剩下的参数形成一个属性值对的序列，为这种类型的按钮指定默认属性值（一个按钮的类型可以通过在创建按钮时给它一个 `type` 属性来设置，使用 `:type` 关键字参数）。

此外，关键字参数 `:supertype` 可用于指定 `name` 继承其默认属性值的按钮类型。请注意，这种继承仅在定义名称时发生；对超类型的后续更改不会反映在其子类型中。

没有必要使用 `define-button-type` 来定义按钮的默认属性——没有任何指定类型的按钮使用内置的 `button-type` 按钮——但我们鼓励这样做，因为这样做通常会使生成的代码更清晰、更高效。



### 40.19.3 制作按钮

按钮与文本区域相关联，使用覆盖或文本属性来保存特定于按钮的信息，所有这些信息都是从按钮的类型（默认为内置按钮类型按钮）初始化的。像所有Emacs文本一样，按钮的外观由 `face` 属性控制；默认情况下（通过从按钮 `button-type` 继承的 `face` 属性）这是一个简单的下划线，就像典型的网页链接一样。

为方便起见，有两种按钮创建函数，一种是向缓冲区的现有区域添加按钮属性，称为 `make-...button` ，另一种是插入按钮文本，称为 ~insert-&#x2026;button .

~ 按钮创建函数都采用 `&rest` 参数属性，它应该是一系列属性值对，指定要添加到按钮的属性；请参阅按钮属性。此外，关键字参数 `:type` 可用于指定继承其他属性的按钮类型；请参阅按钮类型。在创建期间未明确指定的任何属性都将从按钮的类型继承（如果类型定义了此类属性）。

以下函数使用覆盖添加按钮（请参阅覆盖）来保存按钮属性：

    Function: make-button beg end &rest properties ¶

这会在当前缓冲区中创建一个从请求到结束的按钮，然后返回它。

    Function: insert-button label &rest properties ¶

这将插入一个带有标签标签的按钮，并将其返回。

以下功能类似，但使用文本属性（请参阅文本属性）来保存按钮属性。此类按钮不会向缓冲区添加标记，因此如果按钮数量非常多，缓冲区中的编辑不会减慢速度。但是，如果文本上存在现有的面文本属性（例如，由字体锁定模式指定的面），则按钮面可能不可见。这两个函数都返回新按钮的起始位置。

    Function: make-text-button beg end &rest properties ¶

这使得按钮在当前缓冲区中从请求到结束，使用文本属性。

    Function: insert-text-button label &rest properties ¶

这将使用文本属性插入一个带有标签标签的按钮。

    Function: button-buttonize string callback &optional data ¶

有时将字符串制作成按钮而不立即将其插入缓冲区会更方便，例如，在创建稍后可能插入缓冲区的数据结构时。该函数将字符串变成这样的字符串，当用户点击按钮时会调用回调。调用回调时，可选的数据参数将用作参数。如果为 `nil` ，则将按钮用作参数。



### 40.19.4 操作按钮

这些是用于获取和设置按钮属性的函数。通常这些被按钮的调用函数用来确定要做什么。

如果指定了按钮参数，则表示引用特定按钮的对象，可以是覆盖（用于覆盖按钮），也可以是缓冲区位置或标记（用于文本属性按钮）。这样的对象在被调用时作为第一个参数传递给按钮的调用函数。

    Function: button-start button ¶

返回按钮开始的位置。

    Function: button-end button ¶

返回按钮结束的位置。

    Function: button-get button prop ¶

获取名为 `prop` 的按钮按钮的属性。

    Function: button-put button prop val ¶

将按钮的 `prop` 属性设置为 `val` 。

    Function: button-activate button &optional use-mouse-action ¶

调用按钮的动作属性（即调用作为该属性值的函数，将单个参数按钮传递给它）。如果 `use-mouse-action` 不是 `nil` ，尝试调用按钮的 `mouse-action` 属性而不是 `action` ；如果按钮没有鼠标动作属性，则正常使用动作。如果 `button-data` 属性存在于 `button` 中，则将其用作 `action` 函数的参数，而不是 `button` 。

    Function: button-label button ¶

返回按钮的文本标签。

    Function: button-type button ¶

返回按钮的按钮类型。

    Function: button-has-type-p button type ¶

如果按钮具有按钮类型类型或类型的子类型之一，则返回 `t` 。

    Function: button-at pos ¶

返回当前缓冲区中位置 `pos` 处的按钮，或 `nil` 。如果 `pos` 处的按钮是文本属性按钮，则返回值是指向 `pos` 的标记。

    Function: button-type-put type prop val ¶

将 `button-type` 类型的 `prop` 属性设置为 `val` 。

    Function: button-type-get type prop ¶

获取名为 `prop` 的按钮类型的属性。

    Function: button-type-subtype-p type supertype ¶

如果按钮类型类型是超类型的子类型，则返回 `t` 。



### 40.19.5 按钮缓冲区命令

这些是用于在Emacs缓冲区中定位和操作按钮的命令和函数。

push-button 是用户用于实际按下按钮的命令，默认情况下，在按钮本身中使用按钮覆盖或文本属性中的本地键映射将按钮绑定到 `RET` 和 `mouse-2` 。在按钮本身之外有用的命令，例如前进按钮和后退按钮，在存储在按钮缓冲区映射中的键盘映射中也可用；使用按钮的模式可能希望使用按钮缓冲区映射作为其键映射的父键映射。或者，可以打开按钮模式以获得几乎相同的效果：这是一种次要模式，除了将按钮缓冲区映射安装为次要模式键盘映射之外什么都不做。

如果按钮具有非零跟随链接属性，并且设置了 `mouse-1-click-follows-link` ，则快速单击鼠标 `1` 也会激活按钮命令。请参阅定义可点击文本。

    Command: push-button &optional pos use-mouse-action ¶

执行位置 `pos` 处的按钮指定的操作。pos 可以是缓冲区位置或鼠标事件。如果 `use-mouse-action` 不是 `nil` ，或者 `pos` 是鼠标事件（请参阅鼠标事件），请尝试调用按钮的 `mouse-action` 属性而不是 `action` ；如果按钮没有鼠标动作属性，则正常使用动作。pos 默认为 `point` ，除非作为鼠标事件的结果以交互方式调用按钮时，在这种情况下，使用鼠标事件的位置。如果 `pos` 处没有按钮，则不执行任何操作并返回 `nil` ，否则返回 `t` 。

    Command: forward-button n &optional wrap display-message no-error ¶

如果 `n` 为负数，则移至第 `n` 个下一个按钮，或第 `n` 个上一个按钮。如果 `n` 为零，则移动到任意按钮的开头点。如果 `wrap` 不为零，则从缓冲区的任一端移动过去从另一端继续。如果 `display-message` 不为零，则显示按钮的帮助回显字符串。任何具有非 `nil` 跳过属性的按钮都会被跳过。返回找到的按钮，如果找不到按钮，则发出错误信号。如果 `no-error` 不是 `nil` ，则返回 `nil` 而不是发出错误信号。

    Command: backward-button n &optional wrap display-message no-error ¶

如果 `n` 为负数，则移至第 `n` 个上一个按钮，或第 `n` 个下一个按钮。如果 `n` 为零，则移动到任意按钮的开头点。如果 `wrap` 不为零，则从缓冲区的任一端移动过去从另一端继续。如果 `display-message` 不为零，则显示按钮的帮助回显字符串。任何具有非 `nil` 跳过属性的按钮都会被跳过。返回找到的按钮，如果找不到按钮，则发出错误信号。如果 `no-error` 不是 `nil` ，则返回 `nil` 而不是发出错误信号。

    Function: next-button pos &optional count-current ¶

    Function: previous-button pos &optional count-current ¶

在当前缓冲区中的位置 `pos` 之后（对于下一个按钮）或之前（对于上一个按钮）返回下一个按钮。如果 `count-current` 不为零，则在搜索中计算 `pos` 处的任何按钮，而不是从下一个按钮开始。



## 40.20 抽象显示

Ewoc 包构造表示 `Lisp` 对象结构的缓冲区文本，并更新文本以跟随该结构的变化。这就像 `模型-视图-控制器` 设计范式中的 `视图` 组件。Ewoc 的意思是 `Emacs 的对象集合小部件` 。

ewoc 是一种结构，用于组织构建表示某些 `Lisp` 数据的缓冲区文本所需的信息。ewoc的缓冲文本分为三部分，依次为：一是固定的头部文本；接下来，一系列数据元素的文本描述（您指定的 `Lisp` 对象）；最后，固定页脚文本。具体来说，一个 `ewoc` 包含以下信息：

生成其文本的缓冲区。
文本在缓冲区中的起始位置。
页眉和页脚字符串。
一个双向链接的节点链，每个节点包含：
     一个数据元素，一个单一的 `Lisp` 对象。
     链接到链中的前后节点。
一个漂亮的打印机函数，负责将数据元素值的文本表示插入当前缓冲区。

通常，您使用 `ewoc-create` 定义一个 `ewoc` ，然后将生成的 `ewoc` 结构传递给 `Ewoc` 包中的其他函数以在其中构建节点，并将其显示在缓冲区中。一旦它显示在缓冲区中，其他函数确定缓冲区位置和节点之间的对应关系，将点从一个节点的文本表示移动到另一个节点，等等。请参阅抽象显示函数。

节点封装数据元素的方式与变量保存值的方式非常相似。通常，封装作为向 `ewoc` 添加节点的一部分发生。您可以检索数据元素值并在其位置放置一个新值，如下所示：

    (ewoc-data node)
    ⇒ value
    
    (ewoc-set-data node new-value)
    ⇒ new-value

作为数据元素值，您还可以使用 `Lisp` 对象（列表或向量），它是实际值的容器，或者是其他结构的索引。该示例（请参阅抽象显示示例）使用后一种方法。

当数据发生变化时，您将需要更新缓冲区中的文本。您可以通过调用 `ewoc-refresh` 更新所有节点，或者使用 `ewoc-invalidate` 仅更新特定节点，或者使用 `ewoc-map` 更新所有满足谓词的节点。或者，您可以使用 `ewoc-delete` 或 `ewoc-filter` 删除无效节点，并在其位置添加新节点。从 `ewoc` 中删除节点也会从缓冲区中删除其关联的文本描述。



### 40.20.1 抽象显示函数

在本小节中，ewoc 和 `node` 代表上述结构（参见 `Abstract Display` ），而 `data` 代表用作数据元素的任意 `Lisp` 对象。

    Function: ewoc-create pretty-printer &optional header footer nosep ¶

这构造并返回一个新的 `ewoc` ，没有节点（因此没有数据元素）。pretty-printer 应该是一个函数，它接受一个参数，一个您计划在此 `ewoc` 中使用的数据元素，并使用 `insert` 将其文本描述插入点（并且永远不要在标记之前插入，因为这会干扰Ewoc 包的内部机制）。

通常，在页眉、页脚和每个节点的文本描述之后会自动插入换行符。如果nosep 为非零，则不插入换行符。例如，这对于在单行上显示整个 `ewoc` 或通过安排漂亮打印机对这些节点不执行任何操作来使节点不可见可能很有用。

一个 `ewoc` 在创建它时将其文本保存在当前的缓冲区中，因此在调用 `ewoc-create` 之前切换到预期的缓冲区。

    Function: ewoc-buffer ewoc ¶

这将返回 `ewoc` 维护其文本的缓冲区。

    Function: ewoc-get-hf ewoc ¶

这将返回一个由 `ewoc` 的页眉和页脚组成的 `cons` 单元格（页眉。页脚）。

    Function: ewoc-set-hf ewoc header footer ¶

这会将 `ewoc` 的页眉和页脚分别设置为字符串页眉和页脚。

    Function: ewoc-enter-first ewoc data ¶

    Function: ewoc-enter-last ewoc data ¶

这些添加了一个封装数据的新节点，将其分别放在 `ewoc` 节点链的开头或结尾。

    Function: ewoc-enter-before ewoc node data ¶

    Function: ewoc-enter-after ewoc node data ¶

这些添加了一个封装数据的新节点，分别在节点之前或之后将其添加到 `ewoc` 。

    Function: ewoc-prev ewoc node ¶

    Function: ewoc-next ewoc node ¶

这些分别返回 `ewoc` 中节点的前一个节点和下一个节点。

    Function: ewoc-nth ewoc n ¶

这将返回 `ewoc` 中在从零开始的索引 `n` 处找到的节点。负数表示从末尾开始计数。如果 `n` 超出范围，ewoc-nth 返回 `nil` 。

    Function: ewoc-data node ¶

这会提取节点封装的数据并返回。

    Function: ewoc-set-data node data ¶

这会将节点封装的数据设置为数据。

    Function: ewoc-locate ewoc &optional pos guess ¶

这将确定 `ewoc` 中包含点（或 `pos` ，如果指定）的节点，并返回该节点。如果 `ewoc` 没有节点，则返回 `nil` 。如果 `pos` 在第一个节点之前，则返回第一个节点；如果 `pos` 在最后一个节点之后，则返回最后一个节点。可选的第三个参数猜测应该是可能靠近 `pos` 的节点；这不会改变结果，但会使函数运行得更快。

    Function: ewoc-location node ¶

这将返回节点的起始位置。

    Function: ewoc-goto-prev ewoc arg ¶

    Function: ewoc-goto-next ewoc arg ¶

这些移动分别指向 `ewoc` 中的上一个或下一个 `argth` 节点。如果 `ewoc-goto-prev` 已经在第一个节点或 `ewoc` 为空，则不会移动，而 `ewoc-goto-next` 会经过最后一个节点，返回 `nil` 。除了这种特殊情况，这些函数返回移动到的节点。

    Function: ewoc-goto-node ewoc node ¶

这将指向 `ewoc` 中节点的开头。

    Function: ewoc-refresh ewoc ¶

此函数重新生成 `ewoc` 的文本。它的工作原理是删除页眉和页脚之间的文本，即所有数据元素的表示，然后按顺序为每个节点一个接一个地调用漂亮打印机函数。

    Function: ewoc-invalidate ewoc &rest nodes ¶

这类似于 `ewoc-refresh` ，只是只更新 `ewoc` 中的节点而不是整个集合。

    Function: ewoc-delete ewoc &rest nodes ¶

这将从 `ewoc` 中删除节点中的每个节点。

    Function: ewoc-filter ewoc predicate &rest args ¶

这会为 `ewoc` 中的每个数据元素调用谓词，并删除谓词返回 `nil` 的那些节点。任何参数都传递给谓词。

    Function: ewoc-collect ewoc predicate &rest args ¶

这将为 `ewoc` 中的每个数据元素调用谓词，并返回谓词返回非零的那些元素的列表。列表中的元素按缓冲区中的顺序排列。任何参数都传递给谓词。

    Function: ewoc-map map-function ewoc &rest args ¶

这会为 `ewoc` 中的每个数据元素调用 `map-function` ，并更新那些 `map-function` 返回非 `nil` 的节点。任何 `args` 都会传递给 `map-function` 。



### 40.20.2 抽象显示示例

这是一个简单的例子，它使用 `ewoc` 包的函数来实现颜色分量显示，缓冲区中的一个区域以各种方式表示三个整数的向量（本身表示 `24` 位 `RGB` 值）。

    (setq colorcomp-ewoc nil
          colorcomp-data nil
          colorcomp-mode-map nil
          colorcomp-labels ["Red" "Green" "Blue"])
    
    (defun colorcomp-pp (data)
      (if data
          (let ((comp (aref colorcomp-data data)))
    	(insert (aref colorcomp-labels data) "\t: #x"
    		(format "%02X" comp) " "
    		(make-string (ash comp -2) ?#) "\n"))
        (let ((cstr (format "#%02X%02X%02X"
    			(aref colorcomp-data 0)
    			(aref colorcomp-data 1)
    			(aref colorcomp-data 2)))
    	  (samp " (sample text) "))
          (insert "Color\t: "
    	      (propertize samp 'face
    			  `(foreground-color . ,cstr))
    	      (propertize samp 'face
    			  `(background-color . ,cstr))
    	      "\n"))))
    
    (defun colorcomp (color)
      "Allow fiddling with COLOR in a new buffer.
    The buffer is in Color Components mode."
      (interactive "sColor (name or #RGB or #RRGGBB): ")
      (when (string= "" color)
        (setq color "green"))
      (unless (color-values color)
        (error "No such color: %S" color))
      (switch-to-buffer
       (generate-new-buffer (format "originally: %s" color)))
      (kill-all-local-variables)
      (setq major-mode 'colorcomp-mode
    	mode-name "Color Components")
      (use-local-map colorcomp-mode-map)
      (erase-buffer)
      (buffer-disable-undo)
      (let ((data (apply 'vector (mapcar (lambda (n) (ash n -8))
    				     (color-values color))))
    	(ewoc (ewoc-create 'colorcomp-pp
    			   "\nColor Components\n\n"
    			   (substitute-command-keys
    			    "\n\\{colorcomp-mode-map}"))))
        (set (make-local-variable 'colorcomp-data) data)
        (set (make-local-variable 'colorcomp-ewoc) ewoc)
        (ewoc-enter-last ewoc 0)
        (ewoc-enter-last ewoc 1)
        (ewoc-enter-last ewoc 2)
        (ewoc-enter-last ewoc nil)))

这个例子可以扩展为一个颜色选择小部件（换句话说， `模型-视图-控制器` 设计范例的 `控制器` 部分），通过定义命令来修改 `colorcomp-data` 并完成选择过程，以及键盘映射以方便地将它们捆绑在一起。

    (defun colorcomp-mod (index limit delta)
      (let ((cur (aref colorcomp-data index)))
        (unless (= limit cur)
          (aset colorcomp-data index (+ cur delta)))
        (ewoc-invalidate
         colorcomp-ewoc
         (ewoc-nth colorcomp-ewoc index)
         (ewoc-nth colorcomp-ewoc -1))))
    
    (defun colorcomp-R-more () (interactive) (colorcomp-mod 0 255 1))
    (defun colorcomp-G-more () (interactive) (colorcomp-mod 1 255 1))
    (defun colorcomp-B-more () (interactive) (colorcomp-mod 2 255 1))
    (defun colorcomp-R-less () (interactive) (colorcomp-mod 0 0 -1))
    (defun colorcomp-G-less () (interactive) (colorcomp-mod 1 0 -1))
    (defun colorcomp-B-less () (interactive) (colorcomp-mod 2 0 -1))
    
    (defun colorcomp-copy-as-kill-and-exit ()
      "Copy the color components into the kill ring and kill the buffer.
    The string is formatted #RRGGBB (hash followed by six hex digits)."
      (interactive)
      (kill-new (format "#%02X%02X%02X"
    		    (aref colorcomp-data 0)
    		    (aref colorcomp-data 1)
    		    (aref colorcomp-data 2)))
      (kill-buffer nil))
    
    (setq colorcomp-mode-map
          (let ((m (make-sparse-keymap)))
    	(suppress-keymap m)
    	(define-key m "i" 'colorcomp-R-less)
    	(define-key m "o" 'colorcomp-R-more)
    	(define-key m "k" 'colorcomp-G-less)
    	(define-key m "l" 'colorcomp-G-more)
    	(define-key m "," 'colorcomp-B-less)
    	(define-key m "." 'colorcomp-B-more)
    	(define-key m " " 'colorcomp-copy-as-kill-and-exit)
    	m))

请注意，我们从不修改每个节点中的数据，当 `ewoc` 创建为 `nil` 或向量 `colorcomp-data` （实际颜色分量）的索引时，该数据是固定的。



## 40.21 闪烁的括号

本节描述当用户插入右括号时Emacs显示匹配的左括号的机制。

    Variable: blink-paren-function ¶

这个变量的值应该是一个函数（没有参数），每当插入带有右括号语法的字符时就会被调用。blink-paren-function 的值可能为 `nil` ，在这种情况下什么都不做。

    User Option: blink-matching-paren ¶

如果此变量为 `nil` ，则 `blink-matching-open` 什么也不做。

    User Option: blink-matching-paren-distance ¶

此变量指定在放弃之前扫描匹配括号的最大距离。

    User Option: blink-matching-delay ¶

此变量指定保持指示匹配括号的秒数。几分之一秒通常会产生良好的结果，但默认值为 `1` ，适用于所有系统。

    Command: blink-matching-open ¶

该函数是 `blink-paren-function` 的默认值。它假定 `point` 跟随一个带有右括号语法的字符，并立即将适当的效果应用于匹配的开始字符。如果该字符尚未出现在屏幕上，则会在回显区域中显示该字符的上下文。为避免长时间延迟，此函数不会搜索比眨眼匹配父距离字符更远的距离。

这是显式调用此函数的示例。

    
    
    (defun interactive-blink-matching-open ()
      "Indicate momentarily the start of parenthesized sexp before point."
      (interactive)
    
      (let ((blink-matching-paren-distance
    	 (buffer-size))
    	(blink-matching-paren t))
        (blink-matching-open)))



## 40.22 字符显示

本节介绍Emacs实际显示字符的方式。通常，字符显示为字形（在屏幕上占据一个字符位置的图形符号），其外观对应于字符本身。例如，字符 `a` （字符代码 `97` ）显示为 `a` 。但是，某些字符会特别显示。例如，换页符（字符代码 `12` ）通常显示为两个字形的序列， `^L` ，而换行符（字符代码 `10` ）开始一个新的屏幕行。

您可以通过定义一个显示表来修改每个字符的显示方式，该表将每个字符代码映射到一系列字形中。请参阅显示表格。



### 40.22.1 通常的显示约定

以下是显示每个字符代码的约定（在没有显示表的情况下，可以覆盖这些约定；请参阅显示表）。

-   可打印的 `ASCII` 字符、字符代码 `32` 到 `126` （由数字、英文字母和 `#` 等符号组成）按字面显示。
-   制表符（字符代码 `9` ）显示为空格，一直延伸到下一个制表位列。请参阅 `GNU Emacs` 手册中的文本显示。变量 `tab-width` 控制每个制表位的空格数（见下文）。
-   换行符（字符代码 `10` ）有一个特殊的效果：它结束前一行并开始一个新行。
-   不可打印的 `ASCII` 控制字符——字符代码 `0` 到 `31` ，以及 `DEL` 字符（字符代码 `127` ）——根据变量 `ctl-arrow` 以两种方式之一显示。如果此变量不为 `nil` （默认值），则这些字符显示为两个字形的序列，其中第一个字形是 `'^'` （显示表可以指定要使用的字形而不是 `'^'` ）；例如，DEL 字符显示为 `^?` 。

-   如果 `ctl-arrow` 为 `nil` ，这些字符将显示为八进制转义符（见下文）。

-   如果该字符出现在缓冲区中，此规则也适用于回车（字符代码 `13` ）。但是回车通常不会出现在缓冲区文本中；它们作为行尾转换的一部分被消除（参见编码系统的基本概念）。
-   原始字节是代码 `128` 到 `255` 的非 `ASCII` 字符（请参阅文本表示）。这些字符显示为八进制转义：四个字形的序列，其中第一个字形是 `'\'` 的 `ASCII` 代码，其他字符是表示八进制字符代码的数字字符。（显示表可以指定要使用的字形而不是 `\` 。）
-   如果终端支持，每个代码高于 `255` 的非 `ASCII` 字符都会按字面显示。如果终端不支持，则称该字符为无字形，通常使用占位符字形显示。例如，如果图形终端没有字符的字体，Emacs 通常会显示一个包含十六进制字符代码的框。请参阅无字形字符显示。

即使存在显示表，上述显示约定也适用于在活动显示表中的条目为 `nil` 的任何字符。因此，当您设置显示表时，您只需指定您想要特殊显示行为的字符。

以下变量会影响某些字符在屏幕上的显示方式。由于它们改变了字符占据的列数，它们也会影响缩进功能。它们还会影响模式行的显示方式；如果您想使用新值强制重新显示模式行，请调用函数 `force-mode-line-update` （请参阅模式行格式）。

    User Option: ctl-arrow ¶

此缓冲区局部变量控制控制字符的显示方式。如果它不是 `nil` ，则它们显示为插入符号，后跟字符：'<sup>A</sup>'。如果它是 `nil` ，它们将显示为八进制转义：反斜杠后跟三个八进制数字，如 `'\001'` 。

    User Option: tab-width ¶

这个缓冲区局部变量的值是用于在Emacs缓冲区中显示制表符的制表位之间的间距。该值以列为单位，默认值为 `8` 。请注意，此功能完全独立于命令 `tab-to-tab-stop` 使用的用户可设置的制表位。请参阅可调整的制表位。



### 40.22.2 显示表格

显示表是一种特殊用途的字符表（参见 `Char-Tables` ），以 `display-table` 作为其子类型，用于覆盖通常的字符显示约定。本节介绍如何制作、检查和分配元素到显示表对象。下一部分（参见活动显示表）描述了各种标准显示表及其优先级。

    Function: make-display-table ¶

这将创建并返回一个显示表。该表最初在所有元素中都为零。

显示表的普通元素以字符代码为索引；索引 `c` 处的元素说明了如何显示字符代码 `c` 。该值应为 `nil` （这意味着根据通常的显示约定显示字符 `c` ；请参阅通常的显示约定），或字形代码的向量（这意味着将字符 `c` 显示为那些字形；请参阅 `Glyphs` ）。

警告：如果您使用显示表更改换行符的显示，整个缓冲区将显示为一长行。

展示台还有六个额外的插槽，用于特殊用途。这是它们的含义表；任何插槽中的 `nil` 表示使用该插槽的默认值，如下所述。

    0

截断屏幕行末尾的字形（默认为 `$` ）。请参见字形。在图形终端上，Emacs 默认使用边缘中的箭头来表示截断，因此显示表没有效果，除非您禁用边缘（请参阅 `GNU Emacs` 手册中的窗口边缘）。

    1

续行结尾的字形（默认为 `\` ）。在图形终端上，Emacs 默认使用边缘中的弯曲箭头来指示继续，因此显示表没有效果，除非您禁用边缘。

    2

用于指示显示为八进制字符代码的字符的字形（默认为 `\` ）。

    3

用于指示控制字符的字形（默认为 `'^'` ）。

    4

用于指示存在不可见线条的字形向量（默认为 `...` ）。请参阅选择性显示。

    5

用于在并排窗口之间绘制边框的字形（默认为 `|` ）。请参阅拆分窗口。这目前仅对文本终端有效；在图形终端上，如果支持并使用垂直滚动条，则滚动条将两个窗口分开，如果没有垂直滚动条和分隔符（请参阅窗口分隔符），Emacs 使用细线表示边框。

例如，这里是如何构建一个显示表来模拟将 `ctl-arrow` 设置为非 `nil` 值的效果（请参阅 `Glyphs` ，了解函数 `make-glyph-code` ）：

    (setq disptab (make-display-table))
    (dotimes (i 32)
      (or (= i ?\t)
          (= i ?\n)
          (aset disptab i
    	    (vector (make-glyph-code ?^ 'escape-glyph)
    		    (make-glyph-code (+ i 64) 'escape-glyph)))))
    (aset disptab 127
          (vector (make-glyph-code ?^ 'escape-glyph)
    	      (make-glyph-code ?? 'escape-glyph)))))

    Function: display-table-slot display-table slot ¶

该函数返回 `display-table` 的额外 `slot slot` 的值。参数 `slot` 可以是从 `0` 到 `5` 的数字，也可以是插槽名称（符号）。有效符号为截断、换行、转义、控制、选择性显示和垂直边框。

    Function: set-display-table-slot display-table slot value ¶

此函数将值存储在 `display-table` 的额外插槽槽中。参数 `slot` 可以是从 `0` 到 `5` 的数字，也可以是插槽名称（符号）。有效符号为截断、换行、转义、控制、选择性显示和垂直边框。

    Function: describe-display-table display-table ¶

该函数在帮助缓冲区中显示显示表 `display-table` 的描述。

    Command: describe-current-display-table ¶

此命令在帮助缓冲区中显示当前显示表的描述。



### 40.22.3 活动显示表

每个窗口都可以指定一个显示表，每个缓冲区也可以。窗口的显示表（如果有）优先于缓冲区的显示表。如果两者都不存在，Emacs 会尝试使用标准显示表；如果那是 `nil` ，Emacs 使用通常的字符显示约定（请参阅通常的显示约定）。

请注意，显示表会影响模式行的显示方式，因此如果您想使用新的显示表强制重新显示模式行，请调用 `force-mode-line-update` （请参阅模式行格式）。

    Function: window-display-table &optional window ¶

此函数返回窗口的显示表，如果没有则返回 `nil` 。窗口的默认值是选定的窗口。

    Function: set-window-display-table window table ¶

该函数将窗口的显示表格设置为表格。参数表应该是显示表或 `nil` 。

    Variable: buffer-display-table ¶

此变量在所有缓冲区中自动为缓冲区局部；它的值指定缓冲区的显示表。如果为nil，则没有缓冲区显示表。

    Variable: standard-display-table ¶

此变量的值是标准显示表，当Emacs在没有定义窗口显示表或缓冲区显示表的窗口中显示缓冲区时，或者当Emacs将文本输出到标准输出或错误流时，使用该表。尽管它的默认值通常为 `nil` ，但在交互式会话中，如果终端无法显示弯引号，它的默认值会将弯引号映射到 `ASCII` 近似值。请参阅文本引用样式。

disp-table 库定义了几个用于更改标准显示表的函数。



### 40.22.4 字形

字形是在屏幕上占据单个字符位置的图形符号。每个字形在 `Lisp` 中都表示为一个字形代码，它指定一个字符和一个可选的面来显示它（参见面）。字形代码的主要用途是作为显示表的条目（参见显示表）。以下函数用于操作字形代码：

    Function: make-glyph-code char &optional face ¶

此函数返回一个字形代码，表示 `char char` 和 `face face` 。如果 `face` 被省略或 `nil` ，则字形使用默认面；在这种情况下，字形代码是一个整数。如果 `face` 不为零，则字形代码不一定是整数对象。

    Function: glyph-char glyph ¶

该函数返回字形代码字形的字符。

    Function: glyph-face glyph ¶

此函数返回字形代码字形的面，如果字形使用默认面，则返回 `nil` 。

您可以设置字形表来更改字形代码在文本终端上的实际显示方式。此功能已过时；改用 `glyphless-char-display` （请参阅 `Glyphless Character Display` ）。

    Variable: glyph-table ¶

此变量的值，如果非零，则为当前字形表。只对字符终端生效；在图形显示上，所有字形都按字面显示。字形表应该是一个向量，其第 `g` 个元素指定如何显示字形代码 `g` ，其中 `g` 是未指定面的字形的字形代码。每个元素应该是以下之一：

    nil

逐字显示此字形。

    a string

通过将指定的字符串发送到终端来显示此字形。

    a glyph code

改为显示指定的字形代码。

任何大于或等于字形表长度的整数字形代码都会按字面显示。



### 40.22.5 无字形字符显示

无字形字符是以特殊方式显示的字符，例如，作为包含十六进制代码的框，而不是按字面显示。这些包括明确定义为无字形的字符，以及没有可用字体的字符（在图形显示上），以及终端编码系统无法编码的字符（在文本终端上）。

    Variable: glyphless-char-display ¶

这个变量的值是一个字符表，它定义了无字形字符以及它们的显示方式。每个条目必须是以下显示方法之一：

    nil

以通常的方式显示字符。

    zero-width

不显示字符。

    thin-space

在图形显示上显示 `1` 个像素宽，或在文本终端上显示 `1` 个字符宽。

    empty-box

显示一个空框。

    hex-code

以十六进制表示法显示一个包含字符的 `Unicode` 代码点的框。

    an ASCII string

显示一个包含该字符串的框。该字符串最多应包含 `6` 个 `ASCII` 字符。

    a cons cell (graphical . text)

在图形显示器上显示图形，在文本终端上显示文本。图形和文本都必须是上述显示方法之一。

细空间、空框、十六进制代码和 `ASCII` 字符串显示方法使用 `glyphless-char` 面绘制。在文本终端上，方框由方括号 `[]` 模拟。

char-table 有一个额外的槽，它决定了如何显示不能用任何可用字体显示的字符，或者不能被终端的编码系统编码的字符。它的值应该是上述显示方法之一，除了零宽度或 `cons` 单元格。

如果一个字符在活动显示表中有非零条目，则显示表生效；在这种情况下，Emacs 根本不咨询 `glyphless-char-display` 。

    User Option: glyphless-char-display-control ¶

此用户选项提供了一种为相似字符组设置 `glyphless-char-display` 的便捷方法。不要直接从 `Lisp` 代码中设置它的值；该值仅通过自定义 `:set` 函数生效（请参阅定义自定义变量），该函数会更新 `glyphless-char-display` 。

它的值应该是一个元素列表（group.method），其中 `group` 是指定一组字符的符号，method 是指定如何显示它们的符号。

组应该是以下之一：

    c0-control

ASCII 控制字符 `U+0000` 到 `U+001F` ，不包括换行符和制表符（通常显示为转义序列，如 `'^A'` ；请参阅 `GNU Emacs` 手册中的文本显示方式）。

    c1-control

非 `ASCII` 、非打印字符 `U+0080` 到 `U+009F` （通常显示为八进制转义序列，如 `'\230'` ）。

    format-control

Unicode 通用类别 `[Cf]` 的字符，例如 `U+200E LEFT-TO-RIGHT MARK` ，但不包括具有图形图像的字符，例如 `U+00AD SOFT HYPHEN` 。

    variation-selectors

Unicode VS-1 到 `VS-16` （U+FE00 到 `U+FE0F` ），用于在相同代码点（通常是表情符号）的不同字形之间进行选择。

    no-font

没有合适的字体或终端编码系统无法编码的字符。

方法符号应该是零宽度、细空间、空框或十六进制代码之一。这些与上面的 `glyphless-char-display` 具有相同的含义。



## 40.23 哔哔声

本节介绍如何使Emacs响铃（或闪烁屏幕）以吸引用户的注意力。对您执行此操作的频率保持保守；频繁的铃声会变得烦人。还要注意不要在发出错误信号时只使用哔哔声（请参阅错误）。

    Function: ding &optional do-not-terminate ¶

此功能会发出哔哔声或使屏幕闪烁（请参见下面的可视铃）。它还会终止当前正在执行的任何键盘宏，除非 `do-not-terminate` 为非 `nil` 。

    Function: beep &optional do-not-terminate ¶

这是丁的同义词。

    User Option: visible-bell ¶

这个变量决定了Emacs是否应该让屏幕闪烁来代表一个铃声。非零表示是，零表示否。这在图形显示和文本终端上有效，前提是终端的 `Termcap` 条目定义了可视铃铛功能 `('vb')` 。

    User Option: ring-bell-function ¶

如果这是非零，它指定Emacs应该如何响铃。它的值应该是一个没有参数的函数。如果这是非零，它优先于可见钟变量。



## 40.24 窗户系统

Emacs 可与多个窗口系统一起使用，其中最著名的是 `X` 窗口系统。Emacs 和 `X` 都使用术语 `窗口` ，但使用方式不同。就 `X` 而言，Emacs 框架是单个窗口；X 根本不知道各个Emacs窗口。

    Variable: window-system ¶

这个终端本地变量告诉 `Lisp` 程序Emacs使用什么窗口系统来显示框架。可能的值为

    x ¶

Emacs使用 `X` 显示框架。

    w32

Emacs使用本机 `MS-Windows GUI` 显示框架。

    ns

Emacs使用 `Nextstep` 界面（在 `GNUstep` 和 `macOS` 上使用）显示框架。

    pc

Emacs使用 `MS-DOS` 直接屏幕写入来显示框架。

    nil

Emacs在基于字符的终端上显示框架。

    Variable: initial-window-system ¶

此变量保存用于Emacs在启动期间创建的第一帧的 `window-system` 的值。（当Emacs作为守护进程调用时，它不会创建任何初始帧，因此 `initial-window-system` 为 `nil` ，除了在 `MS-Windows` 上，它仍然是 `w32` 。请参阅 `GNU Emacs` 手册中的守护进程。）

    Function: window-system &optional frame ¶

这个函数返回一个符号，它的名字告诉什么窗口系统用于显示框架（默认为当前选择的框架）。它返回的可能符号列表与上面为变量 `window-system` 记录的符号列表相同。

如果您想编写在文本终端和图形显示上以不同方式工作的代码，请不要使用 `window-system` 和 `initial-window-system` 作为谓词或布尔标志变量。这是因为 `window-system` 在给定的显示类型上并不是Emacs功能的良好指标。相反，请使用 `display-graphic-p` 或显示功能测试中描述的任何其他 `display-*-p` 谓词。



## 40.25 工具提示

工具提示是特殊的框架（请参阅框架），用于显示与鼠标指针当前位置相关的有用提示（也称为 `提示` ）。Emacs 使用工具提示来显示有关文本活动部分的帮助字符串（请参阅具有特殊含义的属性）和各种 `UI` 元素，例如菜单项（请参阅扩展菜单项）和工具栏按钮（请参阅工具栏）。

    Function: tooltip-mode ¶

工具提示模式是一种允许显示工具提示的次要模式。关闭此模式会导致工具提示显示在回显区域中。在文本模式（又名 `TTY` ）框架上，工具提示始终显示在回显区域中。

当Emacs使用 `GTK+` 支持构建时，它默认使用 `GTK+` 函数显示工具提示，然后工具提示的外观由 `GTK+` 设置控制。可以通过将变量 `x-gtk-use-system-tooltips` 的值更改为 `nil` 来禁用 `GTK+` 工具提示。本小节的其余部分描述了如何控制Emacs本身提供的非 `GTK+` 工具提示。

工具提示显示在称为工具提示框架的特殊框架中，这些框架具有自己的框架参数（请参阅框架参数）。与其他框架不同，工具提示框架的默认参数存储在一个特殊变量中。

    User Option: tooltip-frame-parameters ¶

此可自定义选项包含用于显示工具提示的默认框架参数。任何字体和颜色参数都将被忽略，而使用工具提示面的相应属性。如果包含 `left` 或 `top` 参数，它们将用作应显示工具提示的绝对帧相对坐标。（工具提示的鼠标相对位置可以使用 `GNU Emacs` 手册中工具提示中描述的变量进行自定义。）请注意，如果存在 `left` 和 `top` 参数，则会覆盖鼠标相对偏移的值。

工具提示面决定了工具提示中显示的文本的外观。它通常应使用大小最好小于默认框架字体的可变间距字体。

    Variable: tooltip-functions ¶

这个异常钩子是Emacs需要显示工具提示时调用的函数列表。每个函数都使用单个参数事件调用，该事件是最后一个鼠标移动事件的副本。如果此列表中的函数实际显示工具提示，它应该返回非 `nil` ，然后将不会调用其余函数。该变量的默认值是单个函数 `tooltip-help-tips` 。

如果您编写自己的函数以放入工具提示功能列表中，您可能需要知道触发工具提示显示的鼠标事件的缓冲区。以下函数提供了该信息。

    Function: tooltip-event-buffer event ¶

此函数返回发生事件的缓冲区。使用来自 `tooltip-functions` 的函数参数调用它，以获取其文本触发工具提示的缓冲区。请注意，事件可能不会发生在缓冲区上（例如，在工具栏上），在这种情况下，此函数将返回 `nil` 。

工具提示显示的其他方面由几个可自定义的设置控制；请参阅GNU Emacs手册中的工具提示。



## 40.26 双向显示

Emacs 可以显示用脚本编写的文本，例如阿拉伯语、波斯语和希伯来语，其水平文本显示的自然顺序是从右到左。此外，嵌入在从右到左的文本中的拉丁字母和数字段从左到右显示，而从左到右的文本中嵌入的从右到左的脚本段（例如，评论中的阿拉伯语或希伯来语文本）或程序源文件中的字符串）适当地从右到左显示。我们将这种从左到右和从右到左文本的混合称为双向文本。本节介绍用于编辑和显示双向文本的工具和选项。

文本以逻辑（或阅读）顺序存储在Emacs缓冲区和字符串中，即人类阅读每个字符的顺序。在从右到左和双向文本中，字符在屏幕上的显示顺序（称为视觉顺序）与逻辑顺序不同；字符的屏幕位置不会随字符串或缓冲区位置单调增加。在执行这种双向重新排序时，Emacs 遵循 `Unicode` 双向算法（又名 `UBa` ），该算法在 `Unicode` 标准（<https://www.unicode.org/reports/tr9/）的附件> `#9` 中进行了描述。Emacs 提供了 `UBA` 的 `完全双向` 类实现，符合 `Unicode` 标准 `v9.0` 的要求。但是请注意，当文本方向与基本段落方向相反时，Emacs 显示续行的方式与 `UBA` 有所不同，UBA 需要在重新排序文本以进行显示之前执行换行。

    Variable: bidi-display-reordering ¶

如果这个缓冲区局部变量的值是非零（默认值），Emacs 执行双向重新排序以进行显示。重新排序会影响缓冲区文本，以及缓冲区中文本和覆盖属性的显示字符串和覆盖字符串（请参阅覆盖属性和显示属性）。如果值为 `nil` ，则Emacs不会在缓冲区中执行双向重新排序。

bidi-display-reordering 的默认值控制不直接由缓冲区提供的字符串的重新排序，包括显示在模式行（请参阅模式行格式）和标题行（请参阅窗口标题行）中的文本。

Emacs 从不重新排序单字节缓冲区的文本，即使缓冲区中的双向显示重新排序不为零。这是因为单字节缓冲区包含原始字节，而不是字符，因此缺乏重新排序所需的方向性属性。因此，要测试缓冲区中的文本是否会被重新排序以进行显示，仅测试bidi-display-reordering 的值是不够的。正确的测试是这样的：

    (if (and enable-multibyte-characters
    	 bidi-display-reordering)
        ;; Buffer is being reordered for display
      )

但是，如果它们的父缓冲区被重新排序，则单字节显示和覆盖字符串会被重新排序。这是因为Emacs将纯 `ASCII` 字符串存储为单字节字符串。如果单字节显示或覆盖字符串包含非 `ASCII` 字符，则假定这些字符具有从左到右的方向。

由显示文本属性覆盖的文本、由值为字符串的显示属性覆盖的文本以及由替换缓冲区文本的任何其他属性覆盖的文本，在重新排序以进行显示时将被视为一个单元。也就是说，这些属性覆盖的整个文本块被重新排序在一起。此外，这样一个文本块中字符的双向属性被忽略了，Emacs 将它们重新排序，就好像它们被替换为单个字符 `U+FFFC` ，称为对象替换字符。这意味着将显示属性放置在部分文本上可能会改变周围文本重新排序以进行显示的方式。为防止出现这种意外效果，请始终将此类属性放置在方向性与其周围文本相同的文本上。

双向文本的每个段落都有一个基本方向，从右到左或从左到右。从左到右的段落从窗口的左边距开始显示，并在文本到达右边距时被截断或继续。从右到左的段落从右边距开始显示，并在左边距继续或截断。

就EmacsUBA 实现而言，段落的确切开始和结束位置由以下两个缓冲区局部变量确定（请注意，段落开始和段落分隔对此没有影响）。默认情况下，这两个变量都为 `nil` ，段落以空行为界，即完全由零个或多个空格字符组成的行，后跟换行符。

    Variable: bidi-paragraph-start-re ¶

如果非零，则该变量的值应该是一个正则表达式，匹配开始或分隔两个段落的行。正则表达式总是在换行符之后匹配，因此最好将其锚定，即以 `^` 开头。

    Variable: bidi-paragraph-separate-re ¶

如果非零，这个变量的值应该是一个正则表达式匹配一行分隔两个段落。正则表达式总是在换行符之后匹配，因此最好将其锚定，即以 `^` 开头。

如果您修改这两个变量中的任何一个，您通常应该同时修改这两个变量，以确保它们一致地描述段落。例如，要让每个新行开始一个新段落以进行双向重新排序，请将两个变量都设置为 `^` 。

默认情况下，Emacs 通过查看开头的文本来确定每个段落的基本方向。确定基准方向的精确方法由 `UBA` 规定；简而言之，段落中具有明确方向性的第一个字符决定了段落的基本方向。但是，有时缓冲区可能需要为其段落强制使用某个基本方向。例如，包含程序源代码的缓冲区应该强制所有段落从左到右显示。您可以使用以下变量来执行此操作：

    User Option: bidi-paragraph-direction ¶

如果此缓冲区局部变量的值是符号从右到左或从左到右，则假定缓冲区中的所有段落都具有该指定方向。任何其他值都等效于 `nil` （默认值），这意味着从每个段落的内容确定每个段落的基本方向。

程序源代码的模式应将其设置为从左到右。Prog 模式默认执行此操作，因此从 `Prog` 模式派生的模式不需要显式设置（请参阅基本主要模式）。

    Function: current-bidi-paragraph-direction &optional buffer ¶

此函数返回命名缓冲区中某个点的段落方向。返回值是一个符号，从左到右或从右到左。如果缓冲区被省略或为零，则默认为当前缓冲区。如果变量 `bidi-paragraph-direction` 的缓冲区局部值非零，则返回值将与该值相同；否则，返回值反映Emacs动态确定的段落方向。对于 `bidi-display-reordering` 值为 `nil` 的缓冲区以及单字节缓冲区，此函数始终从左到右返回。

有时需要以严格的视觉顺序将点移动到当前屏幕位置的左侧或右侧。Emacs 提供了一个原语来做到这一点。

    Function: move-point-visually direction ¶

此功能将当前选定窗口的点移动到屏幕上立即出现在右侧或左侧的缓冲区位置。方向为正时，point向右移动一屏位置，否则向左移动一屏位置。请注意，根据周围的双向上下文，这可能会将点移开许多缓冲区位置。如果在屏幕行的末尾调用，该函数将指针移动到下一个或上一个屏幕行的最右边或最左边的屏幕位置，以适应方向的值。

该函数将新的缓冲区位置作为其值返回。

当具有双向内容的两个字符串并列在缓冲区中或以其他方式以编程方式连接成文本字符串时，双向重新排序可能会产生令人惊讶和不愉快的效果。一个典型的有问题的情况是缓冲区由由空格或标点字符分隔的文本字段序列组成，例如缓冲区菜单模式或 `Rmail` 摘要模式。由于用作分隔符的标点符号方向性较弱，因此它们承担了周围文本的方向性。因此，在具有双向内容的字段之后的数字字段可能会显示在前一个字段的左侧，从而打乱了预期的布局。有几种方法可以避免这个问题：

-   将特殊字符 `U+200E LEFT-TO-RIGHT MARK` 或 `LRM` 添加到可能具有双向内容的每个字段的末尾，或将其添加到后续字段的开头。如下所述的函数 `bidi-string-mark-left-to-right` 可用于此目的。（在从右到左的段落中，请改用 `U+200F RIGHT-TO-LEFT MARK` 或 `RLM` 。）这是 `UBA` 推荐的解决方案之一。
-   在字段分隔符中包含制表符。制表符在双向重新排序中起到段分隔符的作用，导致两边的文本分别重新排序。
-   使用显示属性分隔字段或使用表单的属性值覆盖（空格 `.PROPS` ）（请参阅指定空格）。Emacs 将此显示规范视为段落分隔符，并分别重新排序两侧的文本。

    Function: bidi-string-mark-left-to-right string ¶

此函数返回其参数字符串，可能已修改，以便结果可以安全地与另一个字符串连接，或与缓冲区中的另一个字符串并列，而不会破坏此字符串和显示的下一个字符串的相对布局。如果此函数返回的字符串显示为从左到右段落的一部分，则它将始终显示在其后文本的左侧。该函数通过检查其参数的字符来工作，如果这些字符中的任何一个可能导致显示重新排序，该函数会将 `LRM` 字符附加到字符串中。附加的 `LRM` 字符通过赋予其不可见文本属性 `t` 使其不可见（请参阅不可见文本）。

重新排序算法使用存储的字符的双向属性作为它们的双向类属性（请参阅字符属性）。Lisp 程序可以通过调用 `put-char-code-property` 函数来更改这些属性。但是，这样做需要对 `UBA` 有透彻的了解，因此不建议这样做。对角色双向属性的任何更改都会产生全局影响：它们会影响所有Emacs框架和窗口。

类似地，mirroring 属性用于在重新排序的文本中显示适当的镜像字符。Lisp 程序可以通过更改此属性来影响镜像显示。同样，任何此类更改都会影响所有Emacs显示。

字符的双向属性可以通过在文本中插入特殊的方向控制字符 `LEFT-TO-RIGHT OVERRIDE (LRO)` 和 `RIGHT-TO-LEFT OVERRIDE (RLO)` 来覆盖。RLO 和后面的换行符或 `POP DIRECTIONAL FORMATTING (PDF)` 控制字符之间的任何字符（以先到者为准）将被显示为从右到左的强字符，即它们将在显示时反转。同样，LRO 和 `PDF` 或换行符之间的任何字符都将显示为从左到右的强字符，即使它们是从右到左的强字符也不会反转。

当您希望使某些文本不受重新排序算法的影响，而是直接控制显示顺序时，这些覆盖非常有用。但它们也可用于恶意目的，即网络钓鱼。具体来说，可以操纵网页上的 `URL` 或电子邮件消息中的链接，使其视觉外观无法识别，或类似于某些流行的良性位置，而由浏览器按逻辑顺序解释的真实位置则大不相同.

Emacs 提供了一个原语，应用程序可以使用该原语来检测其双向属性被覆盖的文本实例，从而使从左到右的字符显示为从右到左的字符，反之亦然。

    Function: bidi-find-overridden-directionality from to &optional object ¶

此函数查看从（包括）和到（不包括）位置之间的指定对象的文本，并返回它找到一个强从左到右字符的第一个位置，该字符的方向属性被强制显示为右- to-left，或强制显示为从左到右的强从右到左字符。如果在指定的文本区域中没有找到这样的字符，则返回 `nil` 。

可选参数对象指定要搜索的文本，默认为当前缓冲区。如果 `object` 不是 `nil` ，它可以是其他缓冲区，也可以是字符串或窗口。如果它是一个字符串，该函数将搜索该字符串。如果它是一个窗口，则该函数搜索显示在该窗口中的缓冲区。如果您要检查其文本的缓冲区显示在某个窗口中，我们建议通过该窗口指定它，而不是将缓冲区传递给函数。这是因为告诉函数关于窗口允许它正确考虑特定于窗口的覆盖，如果缓冲区中的某些文本被覆盖覆盖，这可能会改变函数的结果。

当包含混合的从右到左和从左到右字符和双向控件的文本复制到不同的位置时，它可以改变其视觉外观，也可以影响目标周围文本的视觉外观。这是因为由 `UBA` 指定的双向文本的重新排序对复制的文本和将围绕它的复制目标处的文本都具有重要的上下文相关影响。

有时，Lisp 程序可能需要保留复制文本在目的地的确切视觉外观，以及副本周围的文本。Lisp 程序可以使用以下函数来实现该效果。

    Function: buffer-substring-with-bidi-context start end &optional no-properties ¶

此函数的工作方式类似于缓冲区子字符串（请参阅检查缓冲区内容），但它预先添加并附加到复制的文本双向控制字符，以在将文本插入另一个位置时保持文本的视觉外观。可选参数 `no-properties` ，如果非 `nil` ，则表示从文本副本中删除文本属性。

