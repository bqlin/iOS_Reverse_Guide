# Chisel 命令速查

项目链接：<https://github.com/facebook/chisel>

## alamborder

<!-- Put a border around views with an ambiguous layout. Expects 'raw' input (see 'help raw-input'.) -->

框选布局含糊的视图。

语法：`alamborder [--color=color] [--width=width]`

选项：

  - `--color/-c <color>`
    + 类型：string。诸如“red”、“green”、“magenta” 等这样的颜色名称。
  - `--width/-w <width>`
    + 类型：CGFloat。定义边框宽度。

该命令由 `FBAutolayoutBorderAmbiguous` 实现，该文件位于：
/usr/local/Cellar/chisel/1.5.0/libexec/commands/FBAutoLayoutCommands.py


## alamunborder

<!-- Removes the border around views with an ambiguous layout. Expects 'raw' input (see 'help raw-input'.) -->

取消框选布局含糊的视图。

语法：`alamunborder`

该命令由 `FBAutolayoutUnborderAmbiguous` 实现，该文件位于：
/usr/local/Cellar/chisel/1.5.0/libexec/commands/FBAutoLayoutCommands.py


## binside

<!-- Set a breakpoint for a relative address within the framework/library that's currently running. This does the work of finding the offset for the framework/library and sliding your address accordingly. Expects 'raw' input (see 'help raw-input'.) -->

在正在运行的 framework/library 的相对地址设置断点。这样做可以找到 framework/library 的偏移量并相应地滑动地址。

语法：`binside <address>`

参数：

  - `<address>`
    + 类型：string。在当前运行的库内设置断点的地址。

该命令由 `FBFrameworkAddressBreakpointCommand` 实现，该文件位于：
/usr/local/Cellar/chisel/1.5.0/libexec/commands/FBDebugCommands.py


## bmessage

<!-- Set a breakpoint for a selector on a class, even if the class itself doesn't override that selector. It walks the hierarchy until it finds a class that does implement the selector and sets a conditional breakpoint there. Expects 'raw' input (see 'help raw-input'.) -->

给类的 selector 设置断点，即使该类本身没有该 selector。该命令会遍历类的继承关系，直至找到实现 selector 的类，并在其设置断点。

语法：`bmessage <expression>`

参数：

  - `<expression>`
    + 类型：string。用于设置断点的表达式，如：“-[MyView setFrame:]”、“+[MyView awesomeClassMethod]”或“-[0xabcd1234 setFrame:]”。

该命令由 `FBMethodBreakpointCommand` 实现，该文件位于：
/usr/local/Cellar/chisel/1.5.0/libexec/commands/FBDebugCommands.py


## border

<!-- Draws a border around <viewOrLayer>. Color and width can be optionally provided. Additionally depth can be provided in order to recursively border subviews. Expects 'raw' input (see 'help raw-input'.) -->

在视图或图层绘制边框。可选择提供颜色和宽度。另外还可以设置深度，以递归其子视图绘制边框。

语法：`border [--color=color] [--width=width] [--depth=depth] <viewOrLayer>`

参数：

  - `<viewOrLayer>`
    + 类型：UIView/NSView/CALayer \*。绘制边框的 view/layer。NSViews 必须是支持图层的。

选项：

  - `--color/-c <color>`
    + 类型：string。诸如“red”、“green”、“magenta” 等这样的颜色名称。
  - `--width/-w <width>`
    + 类型：CGFloat。定义边框宽度。
  - `--depth/-d <depth>`
    + 类型：int。添加边框的子视图等级。每个等级从默认的颜色或设置的颜色开始，每级使用不同的颜色。

该命令由 `FBDrawBorderCommand` 实现，该文件位于：
/usr/local/Cellar/chisel/1.5.0/libexec/commands/FBDisplayCommands.py


## caflush

<!-- Force Core Animation to flush. This will 'repaint' the UI but also may mess with ongoing animations. Expects 'raw' input (see 'help raw-input'.) -->

强制刷新 Core Animation。这将导致重新绘制 UI，但也可能会让当前进行的动画产生混乱。

语法：`caflush`

该命令由 `FBCoreAnimationFlushCommand` 实现，该文件位于：
/usr/local/Cellar/chisel/1.5.0/libexec/commands/FBDisplayCommands.py


## dcomponents

<!-- Set debugging options for components. Expects 'raw' input (see 'help raw-input'.) -->

设置组件（components）调试选项。

语法：`dcomponents [--set] [--unset]`

选项：

  - `--set/-s`
    + 给组件设置调试模式
  - `--unset/-u`
    + 给组件取消设置调试模式

该命令由 `FBComponentsDebugCommand` 实现，该文件位于：
/usr/local/Cellar/chisel/1.5.0/libexec/commands/FBComponentCommands.py


## dismiss

<!-- Dismiss a presented view controller. Expects 'raw' input (see 'help raw-input'.) -->

关闭（dismiss）模态推出的（presented）视图控制。

语法：`dismiss <viewController>`

参数：

  - `<viewController>`
    + 类型：UIViewController \*。要关闭的视图控制器。

该命令由 `FBDismissViewControllerCommand` 实现，该文件位于：
/usr/local/Cellar/chisel/1.5.0/libexec/commands/FBDisplayCommands.py


## fa11y

<!-- Find the views whose accessibility labels match labelRegex and puts the address of the first result on the clipboard. Expects 'raw' input (see 'help raw-input'.) -->

查找符合正则表达式的辅助标签（accessibility labels）的视图，并将找到的第一个结果放在剪贴板上。

语法：`fa11y <labelRegex>`

参数：

  - `<labelRegex>`
    + 类型：string。用于在视图层级搜索辅助标签的正则表达式。

该命令由 `FBFindViewByAccessibilityLabelCommand` 实现，该文件位于：
/usr/local/Cellar/chisel/1.5.0/libexec/commands/FBAccessibilityCommands.py


## flicker

<!-- Quickly show and hide a view to quickly help visualize where it is. Expects 'raw' input (see 'help raw-input'.) -->

快速显示和隐藏视图以快速可视化它的位置。

语法：`flicker <viewOrLayer>`

参数：

  - `<viewOrLayer>`
    + 类型：UIView/NSView \*。闪烁的视图。

该命令由 `FBFlickerViewCommand` 实现，该文件位于：
/usr/local/Cellar/chisel/1.5.0/libexec/commands/FBFlickerCommands.py


## fv

<!-- Find the views whose class names match classNameRegex and puts the address of first on the clipboard. Expects 'raw' input (see 'help raw-input'.) -->

找到符合正则表达式的类名的视图对象，并将找到的第一个地址放在剪贴板上。

语法：`fv <classNameRegex>`

参数：

  - `<classNameRegex>`
    + 类型：string。用于在视图层级中搜索视图类的正则表达式。

该命令由 `FBFindViewCommand` 实现，该文件位于：
/usr/local/Cellar/chisel/1.5.0/libexec/commands/FBFindCommands.py


## fvc

<!-- Find the view controllers whose class names match classNameRegex and puts the address of first on the clipboard. Expects 'raw' input (see 'help raw-input'.) -->

找到符合正则表达式的类名的视图控制器对象，并将找到的第一个地址放在剪贴板上。

语法：`fvc [--name=classNameRegex] [--view=view]`

选项：

  - `--name/-n <classNameRegex>`
    + 类型：string。用于在视图控制器层级中搜索视图控制器类的正则表达式。
  - `--view/-v <view>`
    + 类型：UIView。此功能用于打印拥有给定视图的视图控制器。

该命令由 `FBFindViewControllerCommand` 实现，该文件位于：
/usr/local/Cellar/chisel/1.5.0/libexec/commands/FBFindCommands.py


## hide

<!-- Hide a view or layer. Expects 'raw' input (see 'help raw-input'.) -->

隐藏视图或图层。

语法：`hide <viewOrLayer>`

参数：
  - `<viewOrLayer>`
    + 类型：UIView/NSView/CALayer \*。

该命令由 `FBHideViewCommand` 实现，该文件位于：
/usr/local/Cellar/chisel/1.5.0/libexec/commands/FBDisplayCommands.py


## mask

<!-- Add a transparent rectangle to the window to reveal a possibly obscured or hidden view or layer's bounds  Expects 'raw' input (see 'help raw-input'.) -->

在窗口上添加一个透明矩形遮罩，以显示可能遮挡（obscured）或隐藏的视图或图层边界。其反向命令是 `unmask`。

语法：`mask [--color=color] [--alpha=alpha] <viewOrLayer>`

参数：
  - `<viewOrLayer>`
    + 类型：UIView/NSView/CALayer \*。添加遮罩的 view/layer。

选项：

  - `--color/-c <color>`
    + 类型：string。诸如“red”、“green”、“magenta” 等这样的颜色名称。
  - `--alpha/-a <alpha>`
    + 类型：CGFloat。遮罩的透明度。

该命令由 `FBMaskViewCommand` 实现，该文件位于：
/usr/local/Cellar/chisel/1.5.0/libexec/commands/FBDisplayCommands.py


## mwarning

<!-- simulate a memory warning  Expects 'raw' input (see 'help raw-input'.) -->

模拟内存警告。

语法：`mwarning`

该命令由 `FBMemoryWarningCommand` 实现，该文件位于：
/usr/local/Cellar/chisel/1.5.0/libexec/commands/FBDebugCommands.py


## pa11y

<!-- Print accessibility labels of all views in hierarchy of <aView>. Expects 'raw' input (see 'help raw-input'.) -->

打印 `<aView>` 层级结构中所有视图的辅助标签（accessibility labels）。

语法：`pa11y <aView>`

参数：

  - `<aView>`
    + 类型：UIView \*。用于打印层级的视图。

该命令由 `FBPrintAccessibilityLabels` 实现，该文件位于：
/usr/local/Cellar/chisel/1.5.0/libexec/commands/FBAccessibilityCommands.py


## pa11yi

<!-- Print accessibility identifiers of all views in hierarchy of <aView>. Expects 'raw' input (see 'help raw-input'.) -->

打印 `<aView>` 层级结构中所有视图的辅助标识符（accessibility identifiers）。

语法：`pa11yi <aView>`

参数：

  - `<aView>`
    + 类型：UIView \*。用于打印层级的视图。

该命令由 `FBPrintAccessibilityIdentifiers` 实现，该文件位于：
/usr/local/Cellar/chisel/1.5.0/libexec/commands/FBAccessibilityCommands.py


## pactions

<!-- Print the actions and targets of a control. Expects 'raw' input (see 'help raw-input'.) -->

打印控件的 actions 和 targets。

语法：`pactions <control>`

参数：

  - `<control>`
    + 类型：UIControl \*。检索 acttions 的控件。

该命令由 `FBPrintTargetActions` 实现，该文件位于：
/usr/local/Cellar/chisel/1.5.0/libexec/commands/FBPrintCommands.py


## paltrace

<!-- Print the Auto Layout trace for the given view. Defaults to the key window. Expects 'raw' input (see 'help raw-input'.) -->

打印给定视图的 Auto Layout 层级（trance）。默认打印 key window。

语法：`paltrace <view>`

参数：

  - `<view>`
    + 类型：UIView \*。打印 Auto Layout 层级（trance）的视图。

该命令由 `FBPrintAutolayoutTrace` 实现，该文件位于：
/usr/local/Cellar/chisel/1.5.0/libexec/commands/FBAutoLayoutCommands.py


## panim

<!-- Prints if the code is currently execution with a UIView animation block. Expects 'raw' input (see 'help raw-input'.) -->

在代码正在使用 UIView 动画 block 执行时打印。

语法：`panim`

该命令由 `FBPrintIsExecutingInAnimationBlockCommand` 实现，该文件位于：
/usr/local/Cellar/chisel/1.5.0/libexec/commands/FBPrintCommands.py


## pbcopy

<!-- Print object and copy output to clipboard Expects 'raw' input (see 'help raw-input'.) -->

打印对象并拷贝输出结果到剪贴板中。

语法：`pbcopy <object>`

参数：

  - `<object>`
    + 类型：id。要打印的对象。

该命令由 `FBPrintToClipboard` 实现，该文件位于：
/usr/local/Cellar/chisel/1.5.0/libexec/commands/FBPrintCommands.py


## pblock

<!-- Print the block`s implementation address and signature. Expects 'raw' input (see 'help raw-input'.) -->

打印 block 的实现地址和签名。

语法：`pblock <block>`

参数：

  - `<block>`
    + 要打印的 block

该命令由 `FBPrintBlock` 实现，该文件位于：
/usr/local/Cellar/chisel/1.5.0/libexec/commands/FBClassDump.py


## pbundlepath

<!-- Print application's bundle directory path. Expects 'raw' input (see 'help raw-input'.) -->

打印应用的包（bundle）路径。

语法：`pbundlepath [--open]`

选项：

  - `--open/-o`
    + 在 Finder 中打开

该命令由 `FBPrintApplicationBundlePath` 实现，该文件位于：
/usr/local/Cellar/chisel/1.5.0/libexec/commands/FBPrintCommands.py


## pca

<!-- Print layer tree from the perspective of the render server. Expects 'raw' input (see 'help raw-input'.) -->

从渲染服务角度打印图层树。

语法：`pca`

该命令由 `FBPrintCoreAnimationTree` 实现，该文件位于：
/usr/local/Cellar/chisel/1.5.0/libexec/commands/FBPrintCommands.py


## pcells

<!-- Print the visible cells of the highest table view in the hierarchy. Expects 'raw' input (see 'help raw-input'.) -->

打印层级结构中最高的表视图可见单元格。

语法：`pcells`

该命令由 `FBPrintOnscreenTableViewCells` 实现，该文件位于：
/usr/local/Cellar/chisel/1.5.0/libexec/commands/FBPrintCommands.py


## pclass

<!-- Print the inheritance starting from an instance of any class. Expects 'raw' input (see 'help raw-input'.) -->

打印给定类的继承关系。

语法：`pclass <object>`

参数：

  - `<object>`
    + 类型：id。需要检索的实例对象。

该命令由 `FBPrintInheritanceHierarchy` 实现，该文件位于：
/usr/local/Cellar/chisel/1.5.0/libexec/commands/FBPrintCommands.py


## pcomponents

<!-- Print a recursive description of components found starting from <aView>. Expects 'raw' input (see 'help raw-input'.) -->

递归打印从给定的视图开始的组件（components）描述。

语法：`pcomponents [--up] [--show-views=showViews] <aView>`

参数：

  - `<aView>`
    + 类型：UIView \*。开始搜索组件的视图。

选项：

  - `--up/-u`
    + 打印在第一个拥有它们的父视图找到的组件层级结构，将搜索带到其窗口。（Print only the component hierarchy found on the first superview that has them, carrying the search up to its window.）
  - `--show-views/-v <showViews>`
    + 类型：BOOL。YES/NO。NO，则打印组件层级结构是不打印视图。默认为 YES。

该命令由 `FBComponentsPrintCommand` 实现，该文件位于：
/usr/local/Cellar/chisel/1.5.0/libexec/commands/FBComponentCommands.py


## pcurl

<!-- Print the NSURLRequest (HTTP) as curl command. Expects 'raw' input (see 'help raw-input'.) -->

将 NSURLRequest(HTTP) 打印为 curl 命令。

语法：`pcurl [--embed-data] <request>`

参数：

  - `<request>`
    + 类型：NSURLRequest \*/NSMutableURLRequest \*。转换为 curl 命令的请求对象。

选项：

  - `--embed-data/-e` ; Embed request data as base64.
    + 将请求数据嵌入为 base64。

该命令由 `FBPrintAsCurl` 实现，该文件位于：
/usr/local/Cellar/chisel/1.5.0/libexec/commands/FBPrintCommands.py


## pdata

<!-- Print the contents of NSData object as string. Expects 'raw' input (see 'help raw-input'.) -->

以字符串形式打印 NSData 对象的内容。

语法：`pdata [--encoding=encoding] <data>`

支持的编码：

- ascii,
- utf8,
- utf16, unicode,
- utf16l (Little endian),
- utf16b (Big endian),
- utf32,
- utf32l (Little endian),
- utf32b (Big endian),
- latin1, iso88591 (88591),
- latin2, iso88592 (88592),
- cp1251 (1251),
- cp1252 (1252),
- cp1253 (1253),
- cp1254 (1254),
- cp1250 (1250),

参数：

  - `<data>`
    + 类型：NSData \*。NSData 对象。

选项：

  - `--encoding/-e <encoding>`
    + 类型：string。使用的编码，默认为 utf-8。

该命令由 `FBPrintData` 实现，该文件位于：
/usr/local/Cellar/chisel/1.5.0/libexec/commands/FBPrintCommands.py


## pdocspath

<!-- Print application's 'Documents' directory path. Expects 'raw' input (see 'help raw-input'.) -->

打印应用的文档（Documents）目录路径。

语法：`pdocspath [--open]`

选项：

  - `--open/-o`
    + 在 Finder 中打开。

该命令由 `FBPrintApplicationDocumentsPath` 实现，该文件位于：
/usr/local/Cellar/chisel/1.5.0/libexec/commands/FBPrintCommands.py


## pinternals

<!-- Show the internals of an object by dereferencing it as a pointer. Expects 'raw' input (see 'help raw-input'.) -->

通过将对象解引用（dereferencing）作为指针来显示对象的内部。

语法：`pinternals <object>`

参数：

  - `<object>`
    + 类型：id。要求值的对象表达式。

该命令由 `FBPrintInternals` 实现，该文件位于：
/usr/local/Cellar/chisel/1.5.0/libexec/commands/FBPrintCommands.py


## pinvocation

<!-- Print the stack frame, receiver, and arguments of the current invocation. It will fail to print all arguments if any arguments are variadic (varargs). Expects 'raw' input (see 'help raw-input'.) -->

打印当前调用的堆栈帧（stack frame）、receiver 和参数。若果存在参数是可变参数（varargs），将无法打印所有参数。

语法：`pinvocation [--all]`

注意：遗憾的是，目前仅在 x86 上实现了该命令。

选项：

  - `--all/-a`
    + 指定打印整个堆栈，而不是打印当前栈帧。

该命令由 `FBPrintInvocation` 实现，该文件位于：
/usr/local/Cellar/chisel/1.5.0/libexec/commands/FBInvocationCommands.py


## pivar

<!-- Print the value of an object's named instance variable. Expects 'raw' input (see 'help raw-input'.) -->

打印对象命名的成员变量。

语法：`pivar <object> <ivarName>`

参数：

  - `<object>`
    + 类型：id。要求值的对象表达式。
  - `<ivarName>`
    + 要打印的成员变量的名称。

该命令由 `FBPrintInstanceVariable` 实现，该文件位于：
/usr/local/Cellar/chisel/1.5.0/libexec/commands/FBPrintCommands.py


## pjson

<!-- Print JSON representation of NSDictionary or NSArray object  Expects 'raw' input (see 'help raw-input'.) -->

以 JSON 形式打印 NSDictionary 或 NSArray 对象。

语法：`pjson [--plain] <object>`

参数：

  - `<object>`
    + 类型：id。要打印的 NSDictionary 或 NSArray 对象。

选项：

  - `--plain/-p`
    + 未格式化的 JSON。

该命令由 `FBPrintJSON` 实现，该文件位于：
/usr/local/Cellar/chisel/1.5.0/libexec/commands/FBPrintCommands.py


## pkp

<!-- Print out the value of the key path expression using -valueForKeyPath:. Expects 'raw' input (see 'help raw-input'.) -->

使用 `-valueForKeyPath:` 输出键值。

语法：`pkp <keypath>`

参数：

  - `<keypath>`
    + 类型：NSString \*。打印的 keypath。

该命令由 `FBPrintKeyPath` 实现，该文件位于：
/usr/local/Cellar/chisel/1.5.0/libexec/commands/FBPrintCommands.py


## pmethods

<!-- Print the class and instance methods of a class. Expects 'raw' input (see 'help raw-input'.) -->

打印类及其实例方法。

语法：`pmethods [--address] [--instance] [--class] [--name] <instance or class>`

参数：

  - `<instance or class>`
    + 类型：实例对象或 Class。Objective-C 类。

选项：

  - `--address/-a`
    + 打印方法的实现地址
  - `--instance/-i`
    + 打印实例方法
  - `--class/-c`
    + 打印类方法
  - `--name/-n`
    + 将参数作为类名

该命令由 `FBPrintMethods` 实现，该文件位于：
/usr/local/Cellar/chisel/1.5.0/libexec/commands/FBClassDump.py


## poobjc

<!-- Print the expression result, with the expression run in an ObjC++ context. (Shortcut for "expression -O -l ObjC++ -- " )  Expects 'raw' input (see 'help raw-input'.) -->

打印表达式结果，表达式在 ObjC++ 上下文中运行。这是 `expression -O -l ObjC++ -- ` 的捷径。

语法：`poobjc <expression>`

参数：

  - `<expression>`
    + 用来求值和打印的表达式

该命令由 `FBPrintObjectInObjc` 实现，该文件位于：
/usr/local/Cellar/chisel/1.5.0/libexec/commands/FBPrintCommands.py


## pproperties

<!-- Print the properties of an instance or Class  Expects 'raw' input (see 'help raw-input'.) -->

打印实例对象或类的属性。

语法：`pproperties [--name] <instance or class>`

参数：

  - `<instance or class>`
    + 类型：实例对象或 Class。Objective-C 类。

选项：

  - `--name/-n`
    + 把参数作为类名

该命令由 `FBPrintProperties` 实现，该文件位于：
/usr/local/Cellar/chisel/1.5.0/libexec/commands/FBClassDump.py


## present

<!-- Present a view controller. Expects 'raw' input (see 'help raw-input'.) -->

模态推出一个视图控制器。

语法：`present <viewController>`

参数：

  - `<viewController>`
    + 类型：UIViewController \*。模态推出的视图控制器。

该命令由 `FBPresentViewControllerCommand` 实现，该文件位于：
/usr/local/Cellar/chisel/1.5.0/libexec/commands/FBDisplayCommands.py


## presponder

<!-- Print the responder chain starting from a specific responder. Expects 'raw' input (see 'help raw-input'.) -->

从给定的响应者开始打印响应链。

语法：`presponder <startResponder>`

参数：

  - `<startResponder>`
    + 类型：UIResponder \*。响应链起始响应者。

该命令由 `FBPrintUpwardResponderChain` 实现，该文件位于：
/usr/local/Cellar/chisel/1.5.0/libexec/commands/FBPrintCommands.py


## ptv

<!-- Print the highest table view in the hierarchy. Expects 'raw' input (see 'help raw-input'.) -->

打印层级结构中最高的表视图。

语法：`ptv`

该命令由 `FBPrintOnscreenTableView` 实现，该文件位于：
/usr/local/Cellar/chisel/1.5.0/libexec/commands/FBPrintCommands.py


## pvc

<!-- Print the recursion description of <aViewController>. Expects 'raw' input (see 'help raw-input'.) -->

打印给定视图控制器的递归描述。

语法：`pvc <aViewController>`

参数：

  - `<aViewController>`
    + 类型：UIViewController \*。要打印描述的视图控制器。

该命令由 `FBPrintViewControllerHierarchyCommand` 实现，该文件位于：
/usr/local/Cellar/chisel/1.5.0/libexec/commands/FBPrintCommands.py


## pviews

<!-- Print the recursion description of <aView>. Expects 'raw' input (see 'help raw-input'.) -->

打印给定的视图的递归描述。

语法：`pviews [--up] [--depth=depth] <aView>`

参数：

  - `<aView>`
    + 类型：UIView \*/NSView \*。打印递归描述的视图。

选项：

  - `--up/-u`
    + 仅打印视图正上方的层级结构，直至其窗口。
  - `--depth/-d <depth>`
    + 类型：int。仅打印给定的深度。0 表示无限深度。

该命令由 `FBPrintViewHierarchyCommand` 实现，该文件位于：
/usr/local/Cellar/chisel/1.5.0/libexec/commands/FBPrintCommands.py


## rcomponents

<!-- Synchronously reflow and update root components found starting from <aView>. Expects 'raw' input (see 'help raw-input'.) -->

从给定的视图，同步重排（reflow）和更新找到的根组件。

语法：`rcomponents [--up] <aView>`

参数：

  - `<aView>`
    + 类型：UIView \*。开始搜索根组件的视图。

选项：

  - `--up/-u`
    + 仅重排具有它们的第一个父视图上找到的根组件，将搜索带到其窗口。

该命令由 `FBComponentsReflowCommand` 实现，该文件位于：
/usr/local/Cellar/chisel/1.5.0/libexec/commands/FBComponentCommands.py


## setinput

<!-- Input text into text field or text view that is first responder. Expects 'raw' input (see 'help raw-input'.) -->

将文本输入作为第一响应者的 text field 或 text view 中。

语法：`setinput <inputText>`

参数：

  - `<inputText>`
    + 类型：string。输入的文字。

该命令由 `FBInputTexToFirstResponderCommand` 实现，该文件位于：
/usr/local/Cellar/chisel/1.5.0/libexec/commands/FBTextInputCommands.py


## settext

<!-- Set text on text on a view by accessibility id. Expects 'raw' input (see 'help raw-input'.) -->

通过辅助（accessibility）id 设置视图的文本。

语法：`settext <accessibilityId> <replacementText>`

参数：

  - `<accessibilityId>`
    + 类型：string。输入视图的辅助 ID。
  - `<replacementText>`
    + 类型：string。设置的文本。

该命令由 `FBInputTexByAccessibilityIdCommand` 实现，该文件位于：
/usr/local/Cellar/chisel/1.5.0/libexec/commands/FBTextInputCommands.py


## show

<!-- Show a view or layer. Expects 'raw' input (see 'help raw-input'.) -->

显示视图或图层。

语法：`show <viewOrLayer>`

参数：

  - `<viewOrLayer>`
    + 类型：UIView/NSView/CALayer \*。要显示的 view/layer。

该命令由 `FBShowViewCommand` 实现，该文件位于：
/usr/local/Cellar/chisel/1.5.0/libexec/commands/FBDisplayCommands.py


## slowanim

<!-- Slows down animations. Works on the iOS Simulator and a device. Expects 'raw' input (see 'help raw-input'.) -->

减慢动画速度。

语法：`slowanim <speed>`

参数：

  - `<speed>`
    + 类型：float。动画速度（默认是 0.1）。

该命令由 `FBSlowAnimationCommand` 实现，该文件位于：
/usr/local/Cellar/chisel/1.5.0/libexec/commands/FBDisplayCommands.py


## taplog

<!-- Log tapped view to the console. Expects 'raw' input (see 'help raw-input'.) -->

打印点击的视图到控制台。

语法：`taplog`

该命令由 `FBTapLoggerCommand` 实现，该文件位于：
/usr/local/Cellar/chisel/1.5.0/libexec/commands/FBFindCommands.py


## unborder

<!-- Removes border around <viewOrLayer>. Expects 'raw' input (see 'help raw-input'.) -->

去除给定视图或图层的边框。

语法：`unborder [--depth=depth] <viewOrLayer>`

参数：

  - `<viewOrLayer>`
    + 类型：UIView/NSView/CALayer \*。去除边框的 view/layer。

选项：

  - `--depth/-d <depth>`
    + 类型：int。要去除边框的子视图层级。

该命令由 `FBRemoveBorderCommand` 实现，该文件位于：
/usr/local/Cellar/chisel/1.5.0/libexec/commands/FBDisplayCommands.py


## unmask

<!-- Remove mask from a view or layer  Expects 'raw' input (see 'help raw-input'.) -->

去除视图或图层的添加的遮罩。其反向命令是 `mask`。

语法：`unmask <viewOrLayer>`

参数：

  - `<viewOrLayer>`
    + 类型：UIView/CALayer \*。要去除遮罩的 view/layer。

该命令由 `FBUnmaskViewCommand` 实现，该文件位于：
/usr/local/Cellar/chisel/1.5.0/libexec/commands/FBDisplayCommands.py


## unslowanim

<!-- Turn off slow animations. Expects 'raw' input (see 'help raw-input'.) -->

关闭动画减速。

语法：`unslowanim`

该命令由 `FBUnslowAnimationCommand` 实现，该文件位于：
/usr/local/Cellar/chisel/1.5.0/libexec/commands/FBDisplayCommands.py


## visualize

<!-- Open a UIImage, CGImageRef, UIView, or CALayer in Preview.app on your Mac. Expects 'raw' input (see 'help raw-input'.) -->

在预览应用中打开 UIImage、CGImageRef、UIView 或 CALayer。

语法：`visualize <target>`

参数：

  - `<target>`
    + 类型：id。要可视化的对象。

该命令由 `FBVisualizeCommand` 实现，该文件位于：
/usr/local/Cellar/chisel/1.5.0/libexec/commands/FBVisualizationCommands.py


## vs

<!-- Interactively search for a view by walking the hierarchy. Expects 'raw' input (see 'help raw-input'.) -->

通过遍历层级结构，以交互方式搜索视图。

语法：`vs <view>`

参数：

  - `<view>`
    + 类型：UIView \*。要搜索的视图。

该命令由 `FBViewSearchCommand` 实现，该文件位于：
/usr/local/Cellar/chisel/1.5.0/libexec/commands/FBFlickerCommands.py


## wivar

<!-- Set a watchpoint for an object's instance variable. Expects 'raw' input (see 'help raw-input'.) -->

给对象的成员变量设置 watchpoint。

语法：`wivar <object> <ivarName>`

参数：

  - `<object>`
    + 类型：id。要求值的对象表达式。
  - `<ivarName>`
    + 要监视的成员变量名称。

该命令由 `FBWatchInstanceVariableCommand` 实现，该文件位于：
/usr/local/Cellar/chisel/1.5.0/libexec/commands/FBDebugCommands.py
