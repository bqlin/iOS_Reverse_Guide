# Logos

[原文](http://iphonedevwiki.net/index.php/Logos)

<!-- Logos is a component of the [Theos](http://iphonedevwiki.net/index.php/Theos) development suite that allows method hooking code to be written easily and clearly, using a set of special preprocessor directives. -->

Logos 是 [Theos](http://iphonedevwiki.net/index.php/Theos) 开发套件的一个组件，他允许使用一组特殊的预处理指令（宏）来轻松、清晰地编写代码。

## 概述

<!-- The syntax provided by Logos greatly simplifies the development of MobileSubstrate extensions ("tweaks") which can hook other methods throughout the OS. In this context, "method hooking" refers to a technique used to replace or modify methods of classes found in other applications on the phone. -->

Logos 提供的语法极大地简化了 MobileSubstrate 扩展（tweaks）的开发，可以在整个操作系统 hook 其他方法。此处，“method hooking”是指用于替换或修改在手机上应用程序中找到的类的方法的技术。

## 获取 Logos

<!-- Logos is distributed with [Theos](http://iphonedevwiki.net/index.php/Theos), and you can use Logos' syntax in any Theos-built project without any extra setup. For more information about Theos, visit [its page](http://iphonedevwiki.net/index.php/Theos). -->

Logos 随 [Theos](http://iphonedevwiki.net/index.php/Theos) 一起发布，你可以在任意 Theos 构建的项目中使用 Logos 语法，而无需任何额外的设置。有关 Theos 的更多信息，请访问[该页面](http://iphonedevwiki.net/index.php/Theos)。

## Logos 指令列表

### 块级别

<!-- The directives in this category open a block of code which must be closed by an `%end` directive (shown below). These should not exist within functions or methods. -->

在该类别中的指令，打开一个代码块必须使用 `%end` 指令来关闭（如下所示）。而这些指令不应写在函数和方法内存中。

#### `%group`

```objective-c
%group Groupname
```

<!-- Begin a hook group (for conditional initialization or code organization) with the name *Groupname*. All ungrouped hooks are in the implicit "_ungrouped" group. -->

使用一个__组名__开始一个 hook 组（用于条件初始化或组织代码）。所有未分组的 hook 代码都在隐式的，名为“_ungrouped”的组内。

<!-- Cannot be inside another `%group` block. -->

`%group` 块不能嵌套使用。

<!-- Grouping can be used to manage backwards compatibility with older code: -->

分组可用于管理与旧代码的向后兼容性。

```objective-c
%group iOS8
%hook IOS8_SPECIFIC_CLASS
	// your code here
%end // end hook
%end // end group ios8

%group iOS9
%hook IOS9_SPECIFIC_CLASS
	// your code here
%end // end hook
%end // end group ios9

%ctor {
	if (kCFCoreFoundationVersionNumber > 1200) {
		%init(iOS9);
	} else {
		%init(iOS8);
	}
}
```

#### `%hook`

```objective-c
%hook Classname
```

<!-- Open a hook block for the class named *Classname*. -->

开启名为 _Classname_ 的类的 hook 块。

<!-- Can be inside a `%group` block. -->

可以在 `%group` 块中使用。

<!-- Here's a trivial example: -->

下面是一个简单的例子：

```objective-c
%hook SBApplicationController
-(void)uninstallApplication:(SBApplication *)application {
	NSLog(@"Hey, we're hooking uninstallApplication:!");
	%orig; // Call the original implementation of this method
	return;
}
%end
```

##### `%new`

```objective-c
%new
%new(signature)
```

<!-- Add a new method to a hooked class or subclass by adding this directive above the method definition. *signature* is the Objective-C type encoding for the new method; if it is omitted, one will be generated. -->

通过给在法定义上方添加此指令，可以将新方法添加到 hook 类或其子类。_signature_ 是 Objective-C 类名，如果省略（omitted），将新生成一个。

<!-- Must be inside a `%hook` block. -->

必须在 `%hook` 块中使用。

#### `%subclass`

```objective-c
%subclass Classname: Superclass <Protocol list>
```

<!-- Subclass block - the class is created at runtime and populated with methods. ivars are not yet supported (use associated objects). The `%new` specifier is needed for a method that doesn't exist in the superclass. To instantiate an object of the new class, you can use the `%c` operator. -->

子类块：在运行时创建类并使用方法填充。尚不支持 ivars（使用关联对象）。在其父类中不存在该方法时，就需要 `%new` 说明符。要实例化一个新类的对象，可以使用 `%c` 运算符。

<!-- Can be inside a `%group` block. -->

可以在 `%group` 块中使用。

<!-- Here's an example: -->

下面给出一个例子：

```objective-c
%subclass MyObject : NSObject

- (id)init {
	self = %orig;
	[self setSomeValue:@"value"];
	return self;
}

//the following two new methods act as `@property (nonatomic, retain) id someValue;`
%new
- (id)someValue {
	return objc_getAssociatedObject(self, @selector(someValue));
}

%new
- (void)setSomeValue:(id)value {
	objc_setAssociatedObject(self, @selector(someValue), value, OBJC_ASSOCIATION_RETAIN_NONATOMIC);
}

%end

%ctor {
	MyObject *myObject = [[%c(MyObject) alloc] init];
	NSLog(@"myObject: %@", [myObject someValue]);
}
```

##### `%property`

```objective-c
%property (nonatomic|assign|retain|copy|weak|strong|getter|setter) Type name;
```

<!-- Add a property to a `%subclass` just like you would with `@property` to a normal Objective-C subclass as well as adding new properties to existing classes within `%hook`. -->

将属性添加到 `%subclass` 就像是使用 `@property` 给普通 Objective-C 子类添加属性一样，同样可以在 `%hook` 现有类中添加新属性。

<!-- Must be inside a `%subclass` or `%hook` block. -->

必须在 `%subclass` 或 `%hook` 块中使用。

#### `%end`

```objective-c
%end
```

<!-- Close a group/hook/subclass block. -->

关闭 group/hook/subclass 块。

### 顶层

<!-- The directives in this category should not exist within a group/hook/subclass block. -->

该类别中的指令不应写在于 group/hook/subclass 块中。

#### `%config`

```objective-c
%config(Key=Value);
```

<!-- Set a logos configuration flag. -->

设置 logos 配置标记。

##### 配置标记

- `generator`
  - `MobileSubstrate`
    - 生成使用 [MobileSubstrate](http://iphonedevwiki.net/index.php/MobileSubstrate) 进行 hook 的代码  
  - `internal`
    - 生成只使用内部 Objective-C runtime 方法进行 hook 的代码
- `warnings`
  - `none`	
    - 无警告
  - `default`
    - 仅保留非致命警告
  - `error`
    - 让所有警告致命
- `dump`
  - `yaml`
    - 适用 YAML 格式 dump 内部解析树  

#### `%hookf`

```objective-c
%hookf(rtype, symbolName, args...) { … }
```

<!-- Generate a function hook for the function named *symbolName*. If the name is passed as a literal string then the function will be dynamically looked up. -->

hook 名为 _symbolName_ 的函数。如果传递的是文字字符串，则动态查找该函数。

```objective-c
// Given the function prototype
FILE *fopen(const char *path, const char *mode);
// The hook is thus made
%hookf(FILE *, fopen, const char *path, const char *mode) {
	NSLog(@"Hey, we're hooking fopen to deny relative paths!");
	if (path[0] != '/') {
		return NULL;
	}
	return %orig; // Call the original implementation of this function
}
```

<!-- Adapting [Ignition](https://github.com/b3ll/Ignition/blob/master/Ignition/Tweak.xmi#L240) hook of `MGGetBoolAnswer`, while previously you had to do: -->

为适配 `MGGetBoolAnswer` 的 [Ignition](https://github.com/b3ll/Ignition/blob/master/Ignition/Tweak.xmi#L240) hook，你以前必须这样做：

```objective-c
CFBooleanRef (*orig_MGGetBoolAnswer)(CFStringRef);
CFBooleanRef fixed_MGGetBoolAnswer(CFStringRef string)
{
	if (CFEqual(string, CFSTR("StarkCapability"))) {
		return kCFBooleanTrue;
	}
	return orig_MGGetBoolAnswer(string);
}

%ctor {
	MSHookFunction(((void *)MSFindSymbol(NULL, "_MGGetBoolAnswer")), (void *)fixed_MGGetBoolAnswer, (void **)&orig_MGGetBoolAnswer);
	...
}
```

<!-- You can now do: -->

现在你可以这样做：

```objective-c
%hookf(CFBooleanRef, "_MGGetBoolAnswer", CFStringRef string)
{
	if (CFEqual(string, CFSTR("StarkCapability"))) {
		return kCFBooleanTrue;
	}
	return %orig;
}
```

#### `%ctor`

```objective-c
%ctor { … }
```

<!-- Generate an anonymous constructor (of default priority). -->

创建匿名构造函数（默认优先级）。

#### `%dtor`

```objective-c
%dtor { … }
```

<!-- Generate an anonymous deconstructor (of default priority). -->

创建匿名析构函数（默认优先级）。

### 函数级别

<!-- The directives in this category should only exist within a function block. -->

该类别中的指令仅能用于函数块中。

#### `%init`

```objective-c
%init;
%init([<class>=<expr>, …]);
%init(Group[, [+|-]<class>=<expr>, …]);
```

<!-- Initialize a group (or the default group). Passing no group name will initialize "_ungrouped", and passing class=expr arguments will substitute the given expressions for those classes at initialization time. The + sigil (as in class methods in Objective-C) can be prepended to the classname to substitute an expression for the metaclass. If not specified, the sigil defaults to -, to substitute the class itself. If not specified, the metaclass is derived from the class. The class name replacement is specially useful for classes that contain characters that can't be used as the class name token for the `%hook` directive, such as spaces and dots. -->

初始化一个组（或默认组）。不传递组名则会初始化“_ungrouped”的组，并且在传递 `class=expr` 参数时，在初始化时会使用给定表达式替换这些类。`+` 号（Objective-C 类方法）可以添加到类名前面，以通过表达式替换元类（metaclass）。如果没有指定，默认是用 `-` 号，来替换该类自身的方法。若果没有指定，该元类（metaclass）会由该类派生。类名替换尤其适用于其类名包含不能用于 `%hook` 指令的字符，例如空格和点。

用法：

```objective-c
%hook SomeClass
-(id)init {
    return %orig;
}
%end

%ctor {
    %init(SomeClass=objc_getClass("class with spaces in the name"));
}
```

#### `%class`

```objective-c
%class Class;
```

> `%class` 指定已被启用，请不要在新代码中使用。

<!-- Forward-declare a class. Outmoded by `%c`, but still exists. Creates a $Class variable, and initializes it with the "_ungrouped" group. -->

前向声明一个类。`%c` 过时，但仍然可用。创建 $Class 变量，然后用“_ungrouped”组初始化。

#### `%c`

```objective-c
%c([+|-]Class)
```

Evaluates to `Class` at runtime. If the + sigil is specified, it evaluates to MetaClass instead of Class. If not specified, the sigil defaults to -, evaluating to Class.

在运行时求 `Class` 的值。如果指定了 `+` 号，则结果将是 MetaClass 而不是 Class。如果没有指定，默认使用 `-` 号，结果为 Class。

#### `%orig`

```objective-c
%orig
%orig(arg1, …)
```

<!-- Call the original hooked method. Doesn't function in a `%new`'d method. Works in subclasses, strangely enough, because MobileSubstrate will generate a supercall closure at hook time. (If the hooked method doesn't exist in the class we're hooking, it creates a stub that just calls the superclass implementation.) args is passed to the original function - don't include `self` and `_cmd`, Logos does this for you. -->

调用原始的 hook  方法。在 `%new` 创建的方法中不起作用。但在子类中是可行的，奇怪得很，因为 MobileSubstrate 会在在 hook 是生成超类调用闭包。（如果我们 hook 的类不存在已经被 hooked 的方法，它会创建一个只调用父类实现的存根（stub）。）参数传递给原始函数，不包括 `self` 和 `_cmd` 参数，Logos 会帮你传递这些。

#### `%log`

```objective-c
%log;
%log([(<type>)<expr>, …]);
```

Dump the method arguments to syslog. Typed arguments included in `%log` will be logged as well.

将方法参数 dump 到系统日志中。还将记录 `%log` 包含的类型的参数。

## Logos 文件扩展

- `.x`
	+ 将由 Logos 处理，然后进行预处理，并编译为 Objective-C。
- `.xm`
	+ 将由 Logos 处理，然后进行预处理，并编译为 Objective-C++。
- `.xi`
	+ 首先作为 Objective-C 进行预处理，然后 Logos 处理结果，然后进行编译。
- `.xmi`
	+ 首先作为 Objective-C++ 进行预处理，然后 Logos 处理结果，然后进行编译。

xi 或 xmi 文件可以在 #define 宏中使用 Logos 指令。

## 拆分 Logo hook 代码为多个文件

<!-- By default, the Logos pre-processor will only process one .xm file at build time. However, it is possible to split the Logos hooking code into multiple files. -->

默认，Logos 预处理器仅在构建式处理一个 .xm 文件。但是，可以将 Logos hook 代码拆分为多个文件。

<!-- First, the main file has to be renamed to an .xmi file. Then, other .xm files can be included in it using the #include directive. The Logos pre-processor will add those files to the main file before processing it. -->

首先，必须将主文件重命名为 .xmi 文件。然后，可以使用 #include 指令将其他 .xm 文件包含其中。Logos 预处理器会在处理之前将这些文件添加到主文件中。

## logify.pl

<!-- You can use logify.pl to create a Logos source file from a header file that will log all of the functions of that header file. Here is an example of a very simple Logos tweak generated by logify.pl -->

你可以使用 logify.pl，来从头文件创建 Logos 源文件，该 Logos 源文件将记录头文件的所有函数。以下是 logify.pl 生成的非常简单的 Logos tweak。

给定一个头文件：

```objective-c
@interface SSDownloadAsset : NSObject
- (NSString *)finalizedPath;
- (NSString *)downloadPath;
- (NSString *)downloadFileName;
+ (id)assetWithURL:(id)url type:(int)type;
- (id)initWithURLRequest:(id)urlrequest type:(int)type;
- (id)initWithURLRequest:(id)urlrequest;
- (id)_initWithDownloadMetadata:(id)downloadMetadata type:(id)type;
@end
```

<!-- You can find logify.pl at $THEOS/bin/logify.pl and you would use it as so: -->

你可以在 $THEOS/bin/logify.pl 找到 logify.pl，使用如下：

```objective-c
$THEOS/bin/logify.pl ./SSDownloadAsset.h
```

输出结果应该是：

```objective-c
%hook SSDownloadAsset
- (NSString *)finalizedPath { %log; NSString * r = %orig; NSLog(@" = %@", r); return r; }
- (NSString *)downloadPath { %log; NSString * r = %orig; NSLog(@" = %@", r); return r; }
- (NSString *)downloadFileName { %log; NSString * r = %orig; NSLog(@" = %@", r); return r; }
+ (id)assetWithURL:(id)url type:(int)type { %log; id r = %orig; NSLog(@" = %@", r); return r; }
- (id)initWithURLRequest:(id)urlrequest type:(int)type { %log; id r = %orig; NSLog(@" = %@", r); return r; }
- (id)initWithURLRequest:(id)urlrequest { %log; id r = %orig; NSLog(@" = %@", r); return r; }
- (id)_initWithDownloadMetadata:(id)downloadMetadata type:(id)type { %log; id r = %orig; NSLog(@" = %@", r); return r; }
%end
```

## 扩展链接

- [main logos file](https://github.com/DHowett/theos/blob/master/bin%2Flogos.pl).
- [logify.pl](https://github.com/DHowett/theos/blob/master/bin%2Flogify.pl)