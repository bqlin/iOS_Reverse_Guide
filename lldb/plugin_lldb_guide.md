# 插件 LLDB 命令速查

项目链接：<https://github.com/DerekSelander/LLDB>

## LLDB 脚本

对于以下所有命令，你可以通过 `help {command}` 方式查看文档。如果要查看命令有哪些参数，可以通过 `{command} -h` 查询。

<!-- TLDR: `search`, `lookup`, and `dclass` are good GOTOs irregardless if you're a dev or exploring without source.  -->

TLDR：如果你手头上没有源码，你可以使用 `search`、`lookup` 和 `dclass` 直接开干。

如果你喜欢 ObjC swizzling，可以查看 `sclass` 命令。如果你喜欢 DTrace，可以查看 `pmodule` 和 `snoopie` 命令。

### search

<!-- Searchs the heap for all alive instances of a certain class. This class must by dynamic (aka inherit from a `NSObject`/`SwiftObject` class). Currently doesn't work with `NSString` or `NSNumber` (tagged pointer objects). -->

在堆中搜索给定类所有存活的实例对象。这个类必须通过 dynamic 的（也就是从 `NSObject`/`SwiftObject` 类继承的）。目前无法搜索 `NSString` 或 `NSNumber` 类（标记指针对象（tagged pointer objects））。

示例：

		# 找到所有 UIView 和其子类的实例对象
		(lldb)  search UIView
	
		# Find all instances of UIView that are UIViews. Ignore subclasses.
		(lldb) search UIView -e
	
		# 找到 tag 等于 5 的所有 UIView 对象。只支持 Objective-C 语法。可以通过 “obj” 引用对象。
		(lldb) search UIView -c "(int)[obj tag]==5"
	
		# 找到其类在 SpringBoardUI 模块中实现的 UIView 子类的所有实例对象。
		(lldb) search UIView -m SpringBoardUI
	
		# 找到在“Woot”模块中创建的，所有 UIView 子类对象，并隐藏它们
		(lldb) search UIView -m Woot -p "[obj setHidden:YES]"
	
		# 搜索 UIView，但只打印类，不打印对象描述。（非常适合 Swift 隐藏指针）
		(lldb) search UIView -b
	
		# 记住，Swift 在类名中包含了模块，所以如果你在 WOOT 模块中有一个叫 TestView 的 Swift UIView，你应该这么使用：
		(lldb) search WOOT.TestView -b
	
		# 搜索包含指针 0xfeedfacf 的引用的所有类
		(lldb) search -r 0xfeedfacf

### dclass

<!-- Dumps all the `NSObject`/`SwiftObject` inherited classes in the process. If you give it a module, it will dump only the classes within that module. You can also filter out classes to only a certain type and can also generate a header file for a specific class. -->

Dump 进程中所有继承 `NSObject`/`SwiftObject` 的类。如果给定一个模块，则只 dump 该模块中的类。你还可以将类筛选仅为某种类型，还可以为特定类生成头文件。

示例：

		# Dump 进程中发现的所有类（Swift 和 Objective-C）
		(lldb) dclass
	
		# Dump 关于“Hello.SomeClass”类的 ObjC/Swift 信息
		(lldb) dclass Hello.SomeClass
	
		# Dump 进程中所有 UIViewController 的类
		(lldb) dclass -f UIViewController
	
		# 在类名中使用正则表达式，并且不区分大小写，搜索“viewcontroller”所有类，并进行 dump
		(lldb) dclass -r (?i)viewCoNtrolLer
	
		# Dump 在 UIKit 模块中的所有类
		(lldb) dclass -m UIKit
	
		# Dump 在 CKConfettiEffect NSBundle 中，且为 UIView 子类的所有类
		(lldb) dclass /System/Library/Messages/iMessageEffects/CKConfettiEffect.bundle/CKConfettiEffect -f UIView
	
		# 为给定的类生成头文件
		(lldb) dclass -g UIView
	
		# 生成可以将对象强制转换的协议，适合在开发中使用私有类
		(lldb) dclass -P UIView
	
		# Dump 特定模块的所有类和方法，非常适合用于查看库中的更改
		(lldb) dclass -o UIKit
	
		# 仅 dump 其父类为 NSObject 且在 UIKit 模块中的类。非常适合用于追踪特定的类，例如可能继承自 NSObject 的数据源。
		(lldb) dclass -s NSObject -m UIKit
	
		# 仅 dump Swift 类
		(lldb) dclass -t swift
	
		# 仅 dump Objective-C 类
		(lldb) dclass -t objc
	
		# 获取 UIView 类的简化“class-dump”
		(lldb) dclass -i UIView
	
		# 获取有关 UIView 的更多信息
		(lldb) dclass -I UIView

### section

<!-- Displays data in the Mach-O segments/sections of the executable or frameworks loaded into the proc -->

显示加载到进程（proc）的可执行文件或 Mach-O segments/sections 的数据。

		# Dump the Mach-O segments to the main executable
		(lldb) section
	
		# Dump the Mach-O segments to UIKit
		(lldb) section UIKit
	
		# Dump the Mach-O sections of the __TEXT segment of UIKit
		(lldb) section UIKit __TEXT
	
		# Get the load address of all the hard-coded uint8_t * strings in the UIKit binary
		(lldb) section UIKit __TEXT.__cstring -l
	
		# Get the entitlements for the executable (simulator only, entitlements for actual app in __LINKEDIT)
		(lldb) section  __TEXT.__entitlements
	
		# Get all the load address to the lazy symbol stubs in the main executable
		(lldb) section  __DATA.__la_symbol_ptr -l

### dd

<!-- Alternative to LLDB's `disassemble` command. Uses colors. Terminal only and designed for x86)64. ARM64 support will come one day... -->

替代 LLDB 的 `disassemble` 命令。使用颜色。仅限 x86/64 的终端可用。ARM64 也将会支持……

![yoink example](https://github.com/DerekSelander/LLDB/raw/master/Media/dd.png)

### sbt

<!-- Symbolicate backtrace. Will symbolicate a stripped backtrace from an executable if the backtrace is using Objective-C code. Currently doesn't work on aarch64 stripped executables but works great on x64 :] -->

符号表还原（Symbolicate backtrace）。可以还原 Objective-C 代码编写的程序的符号表。目前不适用于 aarch64 剥离的可执行文件，但在 x64 上运行良好。:]

<!-- You learn how to make this command in the book :] -->

![sbt example](https://github.com/DerekSelander/LLDB/raw/master/Media/sbt_gif.gif)

### msl

		msl 0xadd7e55

<!-- msl or malloc stack logging will take an address and try and obtain the stack trace to when it was created.  -->

msl 或 malloc 栈日志将获取一个地址，并获尝试取创建时的堆栈信息。

<!-- You will need to set the env var to MallocStackLogging, or `execute turn_on_stack_logging(1)` while the process is active -->

你需要将 env var 设置为 MallocStackLogging，或者在进程出于活动状态时执行 `execute turn_on_stack_logging(1)`。

<!-- You learn how to make this command in the book :] -->

![msl example](https://github.com/DerekSelander/LLDB/raw/master/Media/msl_gif.gif)

### lookup

<!-- Perform a regular expression search for stuff in an executable -->

对可执行文件中的内容执行正则表达式搜索。

示例：

		# 查找包含 viewDidLoad 的所有方法
		(lldb) lookup viewDidLoad
	
		# 查找具有包含 viewDidLoad 的（已知）函数的所有模块摘要
		(lldb) lookup viewDidLoad -s
	
		# 在剥离模块中搜索 Objective-C 代码（例如在 SpringBoard 中）
		(lldb) loo -x StocksFramework .
	
		# 在剥离的 main bundle 中搜索包含不区分大小写的 init 的 Objective-C 代码
		(lldb) lookup -X (?i)init
	
		# 在 UIKit 中搜索包含 http 的所有硬编码嵌入的 `char *` 字符串
		(lldb) lookup -S http -m UIKit
	
		# Dump all the md5'd base64 keys in libMobileGestalt along w/ the address in memory
		(lldb) loo -S ^[a-zA-Z0-9\+]{22,22}$ -m libMobileGestalt.dylib -l
	
		# Dump 所有 DWARF 引用的所有全局 bass 代码。这适用于不在作用域中访问 `static` 变量
		(lldb) lookup . -g HonoluluArt -l

### biof

<!-- Break if on func. Syntax: biof [ModuleName] regex1 ||| [ModuleName2] regex2 -->

在函数上设置断点。语法：`biof [ModuleName] regex1 ||| [ModuleName2] regex2`

<!-- Regex breakpoint that takes two regex inputs. The first regex creates a breakpoint on all matched functions.The second regex will make a breakpoint condition to stop only if the second regex breakpoint is in the stack trace -->

正则表达式断点。需要两个正则表达式作为输入。第一个正则表达式在所有匹配的函数上设置断点。第二个正则表达式让断点仅在栈追踪时停止。

<!-- For example, to only stop if code in the "TestApp" module resulted in executing the setTintColor: method being called -->

例如，仅在“TestApp”模块中的代码导致执行 `setTintColor:` 时才停止：

		biof setTintColor: ||| . Test

<!-- As a tip, it would be wise to have a limited regex1 that matches a small amount of functions, while keeping regex2 at any size -->

提示，这里的 regex1 应匹配少量函数，而 regex2 保持任意大小，这样做效果最佳。

### yoink

<!-- Takes a path on a iOS/tvOS/watchOS and writes to the **/tmp/** dir on your computer.If it can be read by `-[NSData dataWithContentsOfFile:]`, it can be written to disk. -->

输出 iOS/tvOS/watchOS 文件到电脑的 __tmp__ 目录。如果可以通过 `-[NSData dataWithContentsOfFile:]` 读取，就可以将其写到磁盘上。

示例（在 iOS 10 设备上）：

		(lldb) yoink /System/Library/Messages/iMessageEffects/CKConfettiEffect.bundle/CKConfettiEffect

![yoink example](https://github.com/DerekSelander/LLDB/raw/master/Media/yoink_gif.gif)

### pmodule

<!-- Creates a custom dtrace script that profiles modules in an executable based upon its memory layout and ASLR. Provide no arguments w/ '-a' if you want a count of all the modules firing. Provide a module if you want to dump all the methods as they occur. The location of the script is copied to your computer so you can paste the soon to be executed dtrace script in the Terminal. -->

创建自定义 dtrace 脚本，该脚本根据其内存布局和 ASLR 对可执行文件中的模块进行概要分析。如果要计算加载的所有模块个数，请不要提供 w/ '-a' 参数。如果要 dump 所有出现的方法，请提供模块参数。复制脚本的路径，以便你可以在终端中粘贴即将执行的 dtrace 脚本。

<!-- WARNING: YOU MUST DISABLE ROOTLESS TO USE DTRACE -->

警告：使用 DTRACE 你必须禁用 ROOTLESS

		# 跟踪 UIKit 中的所有 Objective-C 代码
		(lldb) pmodule UIKit
	
		# 跟踪 libsystem_kernel.dylib 中所有非 Objective-C 代码（例如：pid$target:libsystem_kernel.dylib::entry）
		(lldb) pmodule -n libsystem_kernel.dylib
	
		# Dump 所有代码，在脚本结束后，只显示模块中的函数调用次数。警告，这将是个漫长的过程。
		(lldb) pmodule -a

![pmodule example](https://github.com/DerekSelander/LLDB/raw/master/Media/pmodule_gif.gif)

### snoopie

<!-- Generates a DTrace sciprt that will only profile classes implemented in the main executable irregardless if binary is stripped or not. This is done via profiling objc_msgSend. The creation of this command is discussed in the book. -->

生成一个 DTrace 脚本，它只会分析主可执行文件实现的类，而不管二进制是否被剥离。这是通过分析 objc_msgSend 完成的。

<!-- WARNING: YOU MUST DISABLE ROOTLESS TO USE DTRACE -->

警告：使用 DTRACE 你必须禁用 ROOTLESS

## LLDB 命令

### ls

<!-- List a directory from the process's perspective. Useful when working on an actual device. -->

在进程角度列出目录。在真机上很实用。

		# List the root dir's contents on an actual iOS device
		(lldb) ls /

		# List contents for /System/Library on an actual iOS device
		(lldb) ls /System/Library

### reload_lldbinit

<!-- Reloads all the contents in your ~/.lldbinit file. Useful for seeing if your python script(s) broke or want to do incremental updates to a python script. -->

重新加载 ~/.lldbinit 文件中的所有内容。用于查看你的 Python 脚本是否损坏或需要对 Python 脚本进行增量更新。

		# Reload/Refresh your LLDB scripts
		(lldb) reload_lldbinit

### tv

<!-- Toggle view. Hides/Shows a view depending on it's current state. You don't need to resume LLDB to see changes. ObjC only -->

切换视图。隐藏/显示视图，具体取决于视图当前的显示状态。你无需恢复 LLDB 即可看到变更结果。仅限 Objective-C。

		# Toggle a view on or off
		(lldb) tv [UIView new]

### pprotocol

<!-- Dumps all the required and optional methods for specific protocol (Objective-C only) -->

Dump 特定协议的所有必要的和可选的方法（仅限 Objective-C）。

		# Dump the protocol for UITableViewDataSource
		(lldb) pprotocol UITableViewDataSource

### pexecutable

<!-- Prints the location (on disk) of the filepath to the executable -->

打印可执行文件（在磁盘上）的位置。

		(lldb) pexecutable

### pframework

<!-- Prints the location (on disk) of a framework -->

打印库（在磁盘上）的位置。

		(lldb) pframework UIKit

### sys

<!-- Drops into the shell to execute commands. Note you can execute LLDB commands via the $() syntax -->

放到 shell 中执行命令。注意，你可以通过 $() 语法执行 LLDB 命令。

		# ls the directory LLDB is running in
		(lldb) sys ls
	
		# Use otool -l on the UIKit framework
		(lldb) sys otool -l $(pframework UIKit)
	
		# Open the main executable in another program
		(lldb) sys open -a "Hopper" $(pexecutable)

### methods

Dumps all methods inplemented by the NSObject subclass (iOS, NSObject subclass only)

		# Get all the methods of UIView
		(lldb) methods UIView

### ivars

<!-- Dumps all ivars for an instance of a particular class which inherits from NSObject (iOS, NSObject subclass only) -->

Dump NSObject 子类对象所有 ivars（仅限 iOS，NSObject 子类）。

		# Get all the ivars on a newly created instance of UIView
		(lldb) ivars [UIView new]

### overlaydbg

<!-- Displays the UIDebuggingInformationOverlay on iOS in 11. Check out http://ryanipete.com/blog/ios/swift/objective-c/uidebugginginformationoverlay/ for instructions -->

在 iOS 11 上显示 UIDebuggingInformationOverlay。请参阅 <http://ryanipete.com/blog/ios/swift/objective-c/uidebugginginformationoverlay/>。

		# Display UIDebuggingInformationOverlay
		(lldb) overlaydbg

<!-- You read all the way to here!? [Here's a video highlighting some of these scripts](https://vimeo.com/231806976) -->

[这里](https://vimeo.com/231806976)是 LLDB 作者对其中一些脚本的演示视频。