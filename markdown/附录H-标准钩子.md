





# TODO 附录 H 标准挂钩

以下是一些钩子变量的列表，这些变量让您可以在适当的情况下提供从 Emacs 中调用的函数。

大多数这些变量的名称都以“-hook”结尾。  它们是普通的钩子，通过 run-hooks 运行。  这种钩子的值是一个函数列表；  这些函数在没有参数的情况下被调用，并且它们的值被完全忽略。  在这种钩子上放置新函数的推荐方法是调用 add-hook。  有关使用钩子的更多信息，请参阅钩子。

名称以“-functions”结尾的变量通常是异常钩子（一些旧代码也可能使用已弃用的“-hooks”后缀）。  它们的值是函数列表，但这些函数以特殊方式调用：它们要么是传递的参数，要么以某种方式使用它们的返回值。  名称以“-function”结尾的变量具有单个函数作为它们的值。

这不是一个详尽的列表，它只涵盖了更一般的钩子。  例如，每个主要模式都定义了一个名为“modename-mode-hook”的钩子。  主模式命令使用 run-mode-hooks 作为它执行的最后一件事来运行这个普通的钩子。  请参阅模式挂钩。  大多数次要模式也有模式挂钩。

一项特殊功能允许您指定表达式以评估是否以及何时加载文件（请参阅加载挂钩）。  该功能不完全是一个钩子，但做了类似的工作。

    activate-mark-hook

    deactivate-mark-hook

见标记。

    after-change-functions

    before-change-functions

    first-change-hook

请参阅更改挂钩。

    after-change-major-mode-hook

    change-major-mode-after-body-hook

请参阅模式挂钩。

    after-init-hook

    before-init-hook

    emacs-startup-hook

    window-setup-hook

请参阅初始化文件。

    after-insert-file-functions

    write-region-annotate-functions

    write-region-post-annotation-function

请参阅文件格式转换。

    after-make-frame-functions

    before-make-frame-hook

    server-after-make-frame-hook

请参阅创建框架。

    after-save-hook

    before-save-hook

    write-contents-functions

    write-file-functions

请参阅保存缓冲区。

    after-setting-font-hook ¶

框架字体更改后的挂钩运行。

    auto-save-hook

请参阅自动保存。

    before-hack-local-variables-hook

    hack-local-variables-hook

请参阅文件局部变量。

    buffer-access-fontify-functions

请参阅文本属性的延迟计算。

    buffer-list-update-hook ¶

当缓冲区列表更改时挂钩运行（请参阅缓冲区列表）。

    buffer-quit-function ¶

调用以退出当前缓冲区的函数。

    change-major-mode-hook

请参阅创建和删除缓冲区本地绑定。

    comint-password-function

这个异常钩子允许派生模式在不提示用户的情况下为底层命令解释器提供密码。

    command-line-functions

请参阅命令行参数。

    delayed-warnings-hook ¶

命令循环在 post-command-hook (qv) 后不久运行。

    focus-in-hook ¶

    focus-out-hook

请参阅输入焦点。

    delete-frame-functions

    after-delete-frame-functions

请参阅删除框架。

    delete-terminal-functions

请参阅多个终端。

    pop-up-frame-function

    split-window-preferred-function

请参阅显示缓冲区的其他选项。

    echo-area-clear-hook

请参阅回声区域自定义。

    find-file-hook

    find-file-not-found-functions

请参阅访问文件的函数。

    font-lock-extend-after-change-region-function

请参阅缓冲区更改后要字体化的区域。

    font-lock-extend-region-functions

请参阅多行字体锁定结构。

    font-lock-fontify-buffer-function

    font-lock-fontify-region-function

    font-lock-mark-block-function

    font-lock-unfontify-buffer-function

    font-lock-unfontify-region-function

请参阅其他字体锁定变量。

    fontification-functions

请参阅自动面分配。

    frame-auto-hide-function

请参阅退出 Windows。

    quit-window-hook

请参阅退出 Windows。

    kill-buffer-hook

    kill-buffer-query-functions

请参阅杀死缓冲区。

    kill-emacs-hook

    kill-emacs-query-functions

请参阅杀死 Emacs。

    menu-bar-update-hook

请参阅菜单栏。

    minibuffer-setup-hook

    minibuffer-exit-hook

请参阅 Minibuffer Miscellany。

    mouse-leave-buffer-hook ¶

当用户在窗口中单击鼠标时挂钩运行。

    mouse-position-function

请参阅鼠标位置。

    prefix-command-echo-keystrokes-functions ¶

由前缀命令（例如 Cu）运行的异常钩子，它应该返回描述当前前缀状态的字符串。  例如，Cu 产生“Cu-”和“Cu 1 2 3-”。  每个钩子函数都在没有参数的情况下被调用，并且应该返回一个描述当前前缀状态的字符串，如果没有前缀状态，则返回 nil。  请参阅前缀命令参数。

    prefix-command-preserve-state-hook ¶

当前缀命令需要通过将当前前缀命令状态传递给下一个命令来保留前缀时，挂钩运行。  例如，当用户键入 Cu 时，Cu 需要将状态传递给下一个命令 - 或者在 Cu 后面跟一个数字。

    pre-redisplay-functions

在重新显示之前在每个窗口中运行钩子。  请参阅强制重新显示。

    post-command-hook

    pre-command-hook

请参阅命令循环概述。

    post-gc-hook

请参阅垃圾收集。

    post-self-insert-hook

请参阅键盘映射和次要模式。

    suspend-hook

    suspend-resume-hook

    suspend-tty-functions

    resume-tty-functions

请参阅暂停 Emacs。

    syntax-begin-function

    syntax-propertize-extend-region-functions

    syntax-propertize-function

    font-lock-syntactic-face-function

请参阅语法字体锁定。  请参阅语法属性。

    temp-buffer-setup-hook

    temp-buffer-show-function

    temp-buffer-show-hook

请参阅临时展示。

    tty-setup-hook

请参阅特定于终端的初始化。

    window-configuration-change-hook

    window-scroll-functions

    window-size-change-functions

请参阅用于窗口滚动和更改的挂钩。

