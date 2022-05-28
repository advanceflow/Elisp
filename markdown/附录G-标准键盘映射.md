
# Table of Contents

1.  [附录 G 标准键盘映射](#orga9ae230)



<a id="orga9ae230"></a>

# TODO 附录 G 标准键盘映射

在本节中，我们列出了一些更通用的键盘映射。  其中许多在 Emacs 首次启动时存在，但有些仅在访问相应功能时才加载。

除了这些，还有许多其他更专业的地图。  特别是那些与主要和次要模式相关的。  minibuffer 使用多个键映射（请参阅完成完成的 Minibuffer 命令）。  有关键映射的更多详细信息，请参阅键映射。

    2C-mode-map

前缀 Cx 6 的子命令的稀疏键映射。
请参阅 GNU Emacs 手册中的两列编辑。

    abbrev-map ¶

前缀 Cx a 的子命令的稀疏键映射。
请参阅 GNU Emacs 手册中的定义缩写。

    button-buffer-map

对包含缓冲区的缓冲区有用的稀疏键映射。
您可能希望将其用作父键映射。  请参阅按钮。

    button-map

按钮使用的稀疏键盘映射。

    ctl-x-4-map

前缀 Cx 4 的子命令的稀疏键映射。

    ctl-x-5-map

前缀 Cx 5 的子命令的稀疏键映射。

    ctl-x-map

Cx 命令的完整键盘映射。

    ctl-x-r-map ¶

前缀 Cx r 的子命令的稀疏键映射。
请参阅 GNU Emacs 手册中的寄存器。

    esc-map

ESC（或 Meta）命令的完整键盘映射。

    function-key-map

所有本地功能键映射 (qv) 实例的父键映射。

    global-map

包含默认全局键绑定的完整键映射。
模式不应修改全局地图。

    goto-map

用于 Mg 前缀键的稀疏键映射。

    help-map

帮助字符 Ch 之后的键的稀疏键映射。
请参阅帮助功能。

    Helper-help-map

帮助实用程序包使用的完整键盘映射。
它在其值单元格和功能单元格中具有相同的键映射。

    input-decode-map

用于翻译键盘和功能键的键盘映射。
如果没有，则它包含一个空的稀疏键映射。  请参阅用于翻译事件序列的键映射。

    key-translation-map

用于翻译键的键映射。  与 local-function-key-map 不同，这个覆盖了普通的键绑定。  请参阅用于翻译事件序列的键映射。

    kmacro-keymap ¶

遵循 Cx Ck 前缀搜索的键的稀疏键映射。
请参阅 GNU Emacs 手册中的键盘宏。

    local-function-key-map

用于将键序列转换为首选替代项的键映射。
如果没有，则它包含一个空的稀疏键映射。  请参阅用于翻译事件序列的键映射。

    menu-bar-file-menu ¶

    menu-bar-edit-menu

    menu-bar-options-menu

    global-buffers-menu-map

    menu-bar-tools-menu

    menu-bar-help-menu

这些键映射在菜单栏中显示主要的顶级菜单。
其中一些包含子菜单。  例如，编辑菜单包含菜单栏搜索菜单等。请参阅菜单栏。

    minibuffer-inactive-mode-map

小缓冲区不活动时使用的完整键盘映射。
请参阅 GNU Emacs 手册中的 Minibuffer 中的编辑。

    mode-line-coding-system-map ¶

    mode-line-input-method-map

    mode-line-column-line-number-mode-map

这些键映射控制模式行的各个区域。
请参阅模式行格式。

    mode-specific-map

抄送后字符的键盘映射。  请注意，这是在全球地图中。  此映射实际上不是特定于模式的：它的名称被选择为在 Ch b（显示绑定）中提供信息，它描述了 Cc 前缀键的主要用途。

    mouse-appearance-menu-map ¶

用于 S-mouse-1 键的稀疏键映射。

    mule-keymap

用于 Cx RET 前缀键的全局键映射。

    narrow-map ¶

前缀 Cx n 的子命令的稀疏键映射。

    prog-mode-map

Prog 模式使用的键盘映射。
请参阅基本主要模式。

    query-replace-map

    multi-query-replace-map

用于查询替换和相关命令中的响应的稀疏键映射；  也适用于 y-or-np 和 map-y-or-np。  使用此映射的函数不支持前缀键；  他们一次查找一个事件。  multi-query-replace-map 扩展 query-replace-map 以进行多缓冲区替换。  请参阅查询替换映射。

    search-map

为搜索相关命令提供全局绑定的稀疏键映射。

    special-mode-map

特殊模式使用的键盘映射。
请参阅基本主要模式。

    tab-prefix-map

用于选项卡栏相关命令的 Cx t 前缀键的全局键映射。
请参阅 GNU Emacs 手册中的选项卡栏。

    tab-bar-map

定义选项卡栏内容的键映射。
请参阅 GNU Emacs 手册中的选项卡栏。

    tool-bar-map

定义工具栏内容的键盘映射。
请参阅工具栏。

    universal-argument-map ¶

处理 Cu 时使用的稀疏键映射。
请参阅前缀命令参数。

    vc-prefix-map

用于 Cx v 前缀键的全局键映射。

    x-alternatives-map ¶

用于在图形框架下映射某些键的稀疏键映射。
函数 x-setup-function-keys 使用它。

