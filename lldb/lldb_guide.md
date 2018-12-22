# LLDB 常用命令

### LLDB 语法

```
<命令> [<子命令> [<子命令>...]] <命令操作> [-选项 [选项值]] [参数 [参数...]]
```

`[]`：表示命令是可选项，不是必须的

### 断点

设置断点

- `breakpoint set -n 函数名称`
    + `set` 是该命令的操作
    + `-n` 是选项 是`--name` 的缩写!

```shell
# 对函数 test1 设置断点
breakpoint set -n test1

# 对 OC 方法设置断点，一次设置多个断点，这有助于同时启用/关闭这组断点
breakpoint set -n "[ViewController save:]" -n "[ViewController pauseGame:]" -n "[ViewController continueGame:]"
# 输出：Breakpoint 3: 3 locations.

# 对项目中所有同名的方法设置断点，对所有调用 `touchesBegan:withEvent:` 方法设置断点
breakpoint set --selector touchesBegan:withEvent:
# 输出：Breakpoint 8: 76 locations.

# 对指定文件设置断点
breakpoint set --file ViewController.m --selector touchesBegan:withEvent:

# 对工程中所有包含关键字的方法、函数设置断点
breakpoint set -r Game
# 输出：Breakpoint 11: 246 locations.
```

使用 `b` 缩写可以代替 `breakpoint set` 命令。

查看断点列表

- `breakpoint list`

删除断点

- `breakpoint delete 组号`

禁用/启用断点

- `breakpoint disable 组号`
- `breakpoint enable 组号`

为断点添加执行命令：

- `breakpoint command add 组号`


查看 `breakpoint` 更多用法：

```shell
(lldb) help breakpoint
     Commands for operating on breakpoints (see 'help b' for shorthand.)

Syntax: breakpoint <subcommand> [<command-options>]

The following subcommands are supported:

      clear   -- Delete or disable breakpoints matching the specified source
                 file and line.
      command -- Commands for adding, removing and listing LLDB commands
                 executed when a breakpoint is hit.
      delete  -- Delete the specified breakpoint(s).  If no breakpoints are
                 specified, delete them all.
      disable -- Disable the specified breakpoint(s) without deleting them.  If
                 none are specified, disable all breakpoints.
      enable  -- Enable the specified disabled breakpoint(s). If no breakpoints
                 are specified, enable all of them.
      list    -- List some or all breakpoints at configurable levels of detail.
      modify  -- Modify the options on a breakpoint or set of breakpoints in
                 the executable.  If no breakpoint is specified, acts on the
                 last created breakpoint.  With the exception of -e, -d and -i,
                 passing an empty argument clears the modification.
      name    -- Commands to manage name tags for breakpoints
      read    -- Read and set the breakpoints previously saved to a file with
                 "breakpoint write".  
      set     -- Sets a breakpoint or set of breakpoints in the executable.
      write   -- Write the breakpoints listed to a file that can be read in
                 with "breakpoint read".  If given no arguments, writes all
                 breakpoints.

For more help on any particular subcommand, type 'help <command> <subcommand>'.
```

### 查看堆栈信息

显示当前断点在当前线程堆栈信息：

- `bt`

往上跟进调用栈：

- `up`
- Select an older stack frame.  Defaults to moving one frame, a numeric argument can specify an arbitrary number.

往下跟进调用栈：

- `down`
- Select a newer stack frame.  Defaults to moving one frame, a numeric argument can specify an arbitrary number.

根据 `bt` 输出的调用栈，查看特定的 frame 信息：

- `frame select 序号`
- Select the current stack frame by index from within the current thread (see 'thread backtrace'.)

查看当前可用参数：

- `frame variable`
- Show variables for the current stack frame. Defaults to all arguments and local variables in scope. Names of argument, local, file static and file global variables can be specified. Children of aggregate variables can be specified such as 'var->child.x'.

调用栈回滚：

- `thread return`
- Prematurely return from a stack frame, short-circuiting execution of newer frames and optionally yielding a specified value.  Defaults to the exiting the current stack frame.

回滚后 continue，并不会往下执行。直接跳出执行，改变原有的执行流程。

### 流程控制

继续执行：

- `continue` 或 `c` 
    + Continue execution of all threads in the current process.

单步运行，将子函数当做整体一步执行：

- `next` 或 `n`，源码级别的单步执行
    + Source level single step, stepping over calls.  Defaults to current thread unless specified.
- `ni`，汇编层单步执行，跳转到指令内部
    + Instruction level single step, stepping over calls.  Defaults to current thread unless specified.

单步运行，遇到子函数会进去：

- `step` 或 `s`，源码级别的单步进入
    + Source level single step, stepping into calls.  Defaults to current thread unless specified.
- `si`，汇编层单步进入
    + Instruction level single step, stepping into calls.  Defaults to current
     thread unless specified.

结束，跳出：

- `finish`，表示直接走完当前方法，返回到上层 frame
- Finish executing the current stack frame and stop after returning. Defaults to current thread unless specified.

### 执行表达式

- `expression`
- `p` 是 `expression --` 的缩写
- `po` 是 `expression -O  --` 的缩写

```shell
(lldb) help expression
     Evaluate an expression on the current thread.  Displays any returned value
     with LLDB's default formatting.  Expects 'raw' input (see 'help
     raw-input'.)

Syntax: expression <cmd-options> -- <expr>

Command Options Usage:
  expression [-AFLORTgp] [-f <format>] [-G <gdb-format>] [-a <boolean>] [-i <boolean>] [-t <unsigned-integer>] [-u <boolean>] [-l <source-language>] [-X <boolean>] [-v[<description-verbosity>]] [-j <boolean>] [-d <none>] [-S <boolean>] [-D <count>] [-P <count>] [-Y[<count>]] [-V <boolean>] [-Z <count>] -- <expr>
  expression [-AFLORTgp] [-a <boolean>] [-i <boolean>] [-t <unsigned-integer>] [-u <boolean>] [-l <source-language>] [-X <boolean>] [-j <boolean>] [-d <none>] [-S <boolean>] [-D <count>] [-P <count>] [-Y[<count>]] [-V <boolean>] [-Z <count>] -- <expr>
  expression [-r] -- <expr>
  expression <expr>

       -A ( --show-all-children )
            Ignore the upper bound on the number of children to show.

       -D <count> ( --depth <count> )
            Set the max recurse depth when dumping aggregate types (default is
            infinity).

       -F ( --flat )
            Display results in a flat format that uses expression paths for
            each variable or member.

       -G <gdb-format> ( --gdb-format <gdb-format> )
            Specify a format using a GDB format specifier string.

       -L ( --location )
            Show variable location information.

       -O ( --object-description )
            Display using a language-specific description API, if possible.

       -P <count> ( --ptr-depth <count> )
            The number of pointers to be traversed when dumping values (default
            is zero).

       -R ( --raw-output )
            Don't use formatting options.

       -S <boolean> ( --synthetic-type <boolean> )
            Show the object obeying its synthetic provider, if available.

       -T ( --show-types )
            Show variable types when dumping values.

       -V <boolean> ( --validate <boolean> )
            Show results of type validators.

       -X <boolean> ( --apply-fixits <boolean> )
            If true, simple fix-it hints will be automatically applied to the
            expression.

       -Y[<count>] ( --no-summary-depth=[<count>] )
            Set the depth at which omitting summary information stops (default
            is 1).

       -Z <count> ( --element-count <count> )
            Treat the result of the expression as if its type is an array of
            this many values.

       -a <boolean> ( --all-threads <boolean> )
            Should we run all threads if the execution doesn't complete on one
            thread.

       -d <none> ( --dynamic-type <none> )
            Show the object as its full dynamic type, not its static type, if
            available.
            Values: no-dynamic-values | run-target | no-run-target

       -f <format> ( --format <format> )
            Specify a format to be used for display.

       -g ( --debug )
            When specified, debug the JIT code by setting a breakpoint on the
            first instruction and forcing breakpoints to not be ignored (-i0)
            and no unwinding to happen on error (-u0).

       -i <boolean> ( --ignore-breakpoints <boolean> )
            Ignore breakpoint hits while running expressions

       -j <boolean> ( --allow-jit <boolean> )
            Controls whether the expression can fall back to being JITted if
            it's not supported by the interpreter (defaults to true).

       -l <source-language> ( --language <source-language> )
            Specifies the Language to use when parsing the expression.  If not
            set the target.language setting is used.

       -p ( --top-level )
            Interpret the expression as a complete translation unit, without
            injecting it into the local context.  Allows declaration of
            persistent, top-level entities without a $ prefix.

       -r ( --repl )
            Drop into Swift REPL

       -t <unsigned-integer> ( --timeout <unsigned-integer> )
            Timeout value (in microseconds) for running the expression.

       -u <boolean> ( --unwind-on-error <boolean> )
            Clean up program state if the expression causes a crash, or raises
            a signal.  Note, unlike gdb hitting a breakpoint is controlled by
            another option (-i).

       -v[<description-verbosity>] ( --description-verbosity=[<description-verbosity>] )
            How verbose should the output of this expression be, if the object
            description is asked for.
            Values: compact | full

Single and multi-line expressions:

    The expression provided on the command line must be a complete expression
    with no newlines.  To evaluate a multi-line expression, hit a return after
    an empty expression, and lldb will enter the multi-line expression editor.
    Hit return on an empty line to end the multi-line expression.

Timeouts:

    If the expression can be evaluated statically (without running code) then
    it will be.  Otherwise, by default the expression will run on the current
    thread with a short timeout: currently .25 seconds.  If it doesn't return
    in that time, the evaluation will be interrupted and resumed with all
    threads running.  You can use the -a option to disable retrying on all
    threads.  You can use the -t option to set a shorter timeout.

User defined variables:

    You can define your own variables for convenience or to be used in
    subsequent expressions.  You define them the same way you would define
    variables in C.  If the first character of your user defined variable is a
    $, then the variable's value will be available in future expressions,
    otherwise it will just be available in the current expression.

Continuing evaluation after a breakpoint:

    If the "-i false" option is used, and execution is interrupted by a
    breakpoint hit, once you are done with your investigation, you can either
    remove the expression execution frames from the stack with "thread return
    -x" or if you are still interested in the expression result you can issue
    the "continue" command and the expression evaluation will complete and the
    expression result will be available using the "thread.completed-expression"
    key in the thread format.

Examples:

    expr my_struct->a = my_array[3]
    expr -f bin -- (index * 8) + 5
    expr unsigned int $foo = 5
    expr char c[] = \"foo\"; c[0]
     
     Important Note: Because this command takes 'raw' input, if you use any
     command options you must use ' -- ' between the end of the command options
     and the beginning of the raw input.
```

### 内存断点

内存断点 `watchpoint` 可以用来监听某个变量的变更。每当该变量变更时会进入断点，进而可以查看堆栈，执行表达式调试。

通过变量设置内存断点：

- `watchpoint set variable 变量`
- Set a watchpoint on a variable. Use the '-w' option to specify the type of watchpoint and the '-s' option to specify the byte size to watch for. If no '-w' option is specified, it defaults to write. If no '-s' option is specified, it defaults to the variable's byte size. Note that there are limited hardware resources for watchpoints. If watchpoint setting fails, consider disable/delete existing ones to free up resources.

通过内存地址设置内存断点：

- `watchpoint set expression 地址`
- Set a watchpoint on an address by supplying an expression. Use the '-w' option to specify the type of watchpoint and the '-s' option to specify the byte size to watch for. If no '-w' option is specified, it defaults to write. If no '-s' option is specified, it defaults to the target's pointer byte size. Note that there are limited hardware resources for watchpoints. If watchpoint setting fails, consider disable/delete existing ones to free up resources.

watchpoint 也有与 breakpoint 类似的 delete、list 等子命令。

### 断点时执行命令

在断点停止时执行一些命令：

- `target stop-hook add`
- `target stop-hook add -o "单行命令"`

在这里一般是执行一些通用的命令，可以把这些命令集合成配置。

LLDB 在启动时，会执行 `~/.lldbinit` 中的命令，可以把 stop-hook 写在此处，就可以对断点有更多的操作。

### 查看模块信息

查看模块列表：

- `image list`，可查看已加载的库

查看类信息：

- `image lookup -t 类名`

查看地址的内存信息：

- `memory read 地址`