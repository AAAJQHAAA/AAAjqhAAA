# process
- 提供有关当前 Node.js 进程的信息并对其进行控制
```text
// 之所以在这里进行命名空间合并，是因为它们专门用作process.stdin、process.stdout和process.stderr.的类型。
// 它们不能存在于tty.d.ts中，因为我们需要消除导入名称的歧义。
interface ReadStream extends tty.ReadStream {}
interface WriteStream extends tty.WriteStream {}
interface MemoryUsageFn {
    /**
     * `process.memoryUsage（）`方法遍历每个页面以收集有关内存使用的信息，根据程序内存分配的不同，这可能会很慢。
     */
    (): MemoryUsage;
    /**
     * 方法返回一个整数，表示以字节为单位的驻留集大小（RSS）。
     */
    rss(): number;
}
interface MemoryUsage {
    rss: number;// 常驻集大小，是进程在主内存设备（即总分配内存的子集）中占用的空间量，包括所有 C++ 和 JavaScript 对象和代码
    heapTotal: number;// V8 的内存量。
    heapUsed: number;// V8 的内存使用量。
    external: number;// 绑定到 V8 管理的 JavaScript 对象的 C++ 对象的内存使用量
    arrayBuffers: number;// 是指为 ArrayBuffer 和 SharedArrayBuffer 分配的内存
}
interface CpuUsage {
    user: number;
    system: number;
}
interface ProcessRelease {
    name: string;
    sourceUrl?: string | undefined;
    headersUrl?: string | undefined;
    libUrl?: string | undefined;
    lts?: string | undefined;
}
interface ProcessVersions extends Dict<string> {
    http_parser: string;
    node: string;
    v8: string;
    ares: string;
    uv: string;
    zlib: string;
    modules: string;
    openssl: string;
}
// 平台
type Platform = | "aix" | "android" | "darwin" | "freebsd" | "haiku" | "linux" | "openbsd" | "sunos" | "win32" | "cygwin" | "netbsd";
// cpu架构
type Architecture = | "arm" | "arm64" | "ia32" | "mips" | "mipsel" | "ppc" | "ppc64" | "riscv64" | "s390" | "s390x" | "x64";
// 信号，在 Worker 线程上不可用
type Signals = | "SIGABRT" | "SIGALRM" | "SIGBUS" | "SIGCHLD" | "SIGCONT" | "SIGFPE" | "SIGHUP" | "SIGILL" | "SIGINT" | "SIGIO" | "SIGIOT" | "SIGKILL" | "SIGPIPE" | "SIGPOLL" | "SIGPROF" | "SIGPWR" | "SIGQUIT" | "SIGSEGV" | "SIGSTKFLT" | "SIGSTOP" | "SIGSYS" | "SIGTERM" | "SIGTRAP" | "SIGTSTP" | "SIGTTIN" | "SIGTTOU" | "SIGUNUSED" | "SIGURG" | "SIGUSR1" | "SIGUSR2" | "SIGVTALRM" | "SIGWINCH" | "SIGXCPU" | "SIGXFSZ" | "SIGBREAK" | "SIGLOST" | "SIGINFO";
type UncaughtExceptionOrigin = "uncaughtException" | "unhandledRejection";
type MultipleResolveType = "resolve" | "reject";
type BeforeExitListener = (code: number) => void;// 退出之前的事件回调
type ExitListener = (code: number) => void;// 退出的事件回调
type DisconnectListener = () => void;// 断开连接事件回调
type MessageListener = (message: unknown, sendHandle: unknown) => void;// 消息接收事件回调
type RejectionHandledListener = (promise: Promise<unknown>) => void;// 处理的异常事件回调
type UncaughtExceptionListener = (error: Error, origin: UncaughtExceptionOrigin) => void;// 未捕获异常事件回调
/**
 * 大多数情况下，unhandledRejection将是一个Error，但这不应该依赖于 * 任何东西 * 都可以被抛出/拒绝，因此假设该值是Error是不安全的。
 */
type UnhandledRejectionListener = (reason: unknown, promise: Promise<unknown>) => void;// 未处理的
type WarningListener = (warning: Error) => void;// 警告回调
type SignalsListener = (signal: Signals) => void;// 信号事件触发回调
type MultipleResolveListener = (type: MultipleResolveType, promise: Promise<unknown>, value: unknown) => void;
type WorkerListener = (worker: Worker) => void;// 新建worker回调
interface Socket extends ReadWriteStream {
    isTTY?: true | undefined;
}
// 兼容性测试
interface ProcessEnv extends Dict<string> {
    /**
     * 可用于在运行时更改默认时区
     */
    TZ?: string;
}
interface HRTime {
    (time?: [number, number]): [number, number];
    bigint(): bigint;
}
interface ProcessReport {
    /**
     * 编写报告的目录
     * Node.js进程的工作目录
     * @default '' 指示报告被写入当前
     */
    directory: string;
    /**
     * 在写报告的地方
     * 默认值为空字符串
     * @default '' 输出文件名将由时间戳、PID和序列号组成。
     */
    filename: string;
    /**
     * 返回正在运行的进程的JSON格式的诊断报告。
     * 报告的JavaScript堆栈跟踪来自err（如果存在）。
     */
    getReport(err?: Error): string;
    /**
     * 如果为true，则会针对致命错误（如内存不足错误或失败的C++断言）生成诊断报告
     * @default false
     */
    reportOnFatalError: boolean;
    /**
     * 如果为true，则在进程收到由process.report.signal指定的信号时生成诊断报告。
     * @default false
     */
    reportOnSignal: boolean;
    /**
     * 如果为真，则会对未捕获的异常生成诊断报告
     * @default false
     */
    reportOnUncaughtException: boolean;
    /**
     * 用于触发诊断报告创建的信号
     * @default 'SIGUSR2'
     */
    signal: Signals;
    /**
     * 将诊断报告写入文件。如果未提供文件名，则默认文件名包括日期、时间、PID和序列号。
     * 报告的JavaScript堆栈跟踪来自err（如果存在）。
     *
     * @param fileName 写入报告的文件的名称。这应该是一个相对路径，它将被附加到`process.report.directory`中指定的目录，或者Node.js进程的当前工作目录（如果未指定）。
     * @param error 用于报告JavaScript堆栈的自定义错误。
     * @return 生成的报告的文件夹。
     */
    writeReport(fileName?: string): string;
    writeReport(error?: Error): string;
    writeReport(fileName?: string, err?: Error): string;
}
interface ResourceUsage {
    fsRead: number;
    fsWrite: number;
    involuntaryContextSwitches: number;
    ipcReceived: number;
    ipcSent: number;
    majorPageFault: number;
    maxRSS: number;
    minorPageFault: number;
    sharedMemorySize: number;
    signalsCount: number;
    swappedOut: number;
    systemCPUTime: number;
    unsharedDataSize: number;
    unsharedStackSize: number;
    userCPUTime: number;
    voluntaryContextSwitches: number;
}
interface EmitWarningOptions {
    /**
     * 当`warning`是一个`string`时，`type`是用于发出的_type_ of警告的名称。
     *
     * @default 'Warning'
     */
    type?: string | undefined;
    /**
     * 正在发出的警告实例的唯一标识符。
     */
    code?: string | undefined;
    /**
     * 当`warning`为`string`时，`ctor`为可选函数，用于限制生成的堆栈跟踪
     *
     * @default process.emitWarning
     */
    ctor?: Function | undefined;
    /**
     * 要包含在错误中的其他文本。
     */
    detail?: string | undefined;
}
interface ProcessConfig {
    readonly target_defaults: {
        readonly cflags: any[];
        readonly default_configuration: string;
        readonly defines: string[];
        readonly include_dirs: string[];
        readonly libraries: string[];
    };
    readonly variables: {
        readonly clang: number;
        readonly host_arch: string;
        readonly node_install_npm: boolean;
        readonly node_install_waf: boolean;
        readonly node_prefix: string;
        readonly node_shared_openssl: boolean;
        readonly node_shared_v8: boolean;
        readonly node_shared_zlib: boolean;
        readonly node_use_dtrace: boolean;
        readonly node_use_etw: boolean;
        readonly node_use_openssl: boolean;
        readonly target_arch: string;
        readonly v8_no_strict_aliasing: number;
        readonly v8_use_snapshot: boolean;
        readonly visibility: string;
    };
}
interface Process extends EventEmitter {
    /**
     * `process.stdout`属性返回一个连接到`stdout`（fd `1`）的流。它是一个`net.Socket`（这是一个`Duplex`流），除非fd `1`引用了一个文件，在这种情况下，它是一个`可复制的`流。
     *
     * 例如，将`process.stdin`复制到`process.stdout`：
     *
     *
     * `process.stdout`与其他Node.js流在重要方面有所不同。有关详细信息，请参阅“进程I/O说明”。
     */
    stdout: WriteStream & {
        fd: 1;
    };
    /**
     * `process.stderr`属性返回一个连接到`stderr`（fd `2`）的流。它是一个`net.Socket`（这是一个`Duplex`流），除非fd `2`引用了一个文件，在这种情况下，它是一个`可复制的`流。
     *
     * `process.stderr`与其他Node.js流在重要方面有所不同。有关详细信息，请参阅“进程I/O说明”。
     */
    stderr: WriteStream & {
        fd: 2;
    };
    /**
     * `process.stdin`属性返回一个连接到`stdin`（fd `0`）的流。它是一个`net.Socket`（这是一个`Duplex`流），除非fd `0`引用一个文件，在这种情况下，它是一个`Readable`流。
     *
     * 有关如何从`stdin`读取的详细信息，请参阅`readable.read（）`。
     *
     * 作为一个`Duplex`流，`process.stdin`也可以在“旧”模式下使用，该模式与为v0.10\之前的Node.js编写的脚本兼容。
     * 有关更多信息，请参阅“流兼容性”。
     *
     * 在“旧”流模式下，默认情况下，“stdin”流是暂停的，因此 必须调用`process.stdin.resume（）`才能从中读取。还请注意，调用`process.stdin.resume（）`本身会将流切换到“旧”模式。
     */
    stdin: ReadStream & {
        fd: 0;
    };
    openStdin(): Socket;
    /**
     * `process.argv`属性返回数组，其中包含启动 Node.js 进程时传入的命令行参数
     * 第一个元素是{@link execPath}。如果需要访问`argv[0]`的原始值，请参见`process.argv0`。 
     * 第二个元素是正在执行的JavaScript文件的路径。其余元素将是任何其他命令行参数。
     * @since v0.1.27
     */
    argv: string[];
    /**
     * `process.argv0`属性存储Node.js启动时传入的`argv[0]`原始值的只读副本。
     * @since v6.4.0
     */
    argv0: string;
    /**
     * `process.execArgv`返回 Node.js 进程启动时传入的一组特定于 Node.js 的命令行选项 
     * 这些选项不会出现在 process.argv 属性返回的数组中，也不包括 Node.js 可执行文件、脚本名称或脚本名称后面的任何选项。
     * 这些选项可用于衍生与父进程具有相同执行环境的子进程。
     *
     * 有关具有此属性的工作线程的详细行为，请参阅`Worker constructor`。
     * @since v0.7.7
     */
    execArgv: string[];
    /**
     * `process.execPath`属性返回启动Node.js进程的可执行文件的绝对路径名。符号链接（如果有）将被解析。
     *
     * @since v0.1.100
     */
    execPath: string;
    /**
     * `process.abort（）`方法会导致Node.js进程立即退出并生成一个核心文件。
     *
     * 此功能在“Worker”线程中不可用
     * @since v0.7.0
     */
    abort(): never;
    /**
     * `process.chdir()`方法更改Node.js进程的当前工作目录，或者在操作失败时抛出异常（例如，如果指定的`directory`不存在）。
     * 此功能在“Worker”线程中不可用。
     * @since v0.1.17
     */
    chdir(directory: string): void;
    /**
     * `process.cwd()`方法返回Node.js进程的当前工作目录。
     * @since v0.1.8
     */
    cwd(): string;
    /**
     * 启用时Node.js调试器使用的端口。
     *
     * @since v0.7.2
     */
    debugPort: number;
    /**
     * `process.emitWarning（）`方法可用于发出自定义或应用程序特定的进程警告。可以通过向"警告“事件添加处理程序来侦听这些事件。
     *
     * 如果`warning`作为`Error`对象传递，则忽略`options`参数。
     * @since v8.0.0
     * @param warning 发出警告。
     */
    emitWarning(warning: string | Error, ctor?: Function): void;
    emitWarning(warning: string | Error, type?: string, ctor?: Function): void;
    emitWarning(warning: string | Error, type?: string, code?: string, ctor?: Function): void;
    emitWarning(warning: string | Error, options?: EmitWarningOptions): void;
    /**
     * `process.env`属性返回一个包含用户环境的对象。See [`environ(7)`](http://man7.org/linux/man-pages/man7/environ.7.html).
     *
     * 在Windows操作系统上，环境变量不区分大小写。
     *
     * 除非在创建“Worker”实例时显式指定， 每个工作线程都有自己的进程.env的副本， 父线程的`process.env`，或指定为`env`选项的任何内容 创建一个'Worker'构造器。
     * 对`process.env`的更改将不可见 在`Worker`线程之间，只有主线程可以进行更改， 对操作系统或本机附加组件可见。
     * 在Windows上，“Worker”实例上的“process.env "副本以区分大小写的方式运行 与主线不同。
     * @since v0.1.27
     */
    env: ProcessEnv;
    /**
     * `process.exit（）`方法指示Node.js同步终止进程，退出状态为`code`。
     * 如果省略了“code”，exit将使用“success”代码“0”或“process.exitCode”的值（如果已设置）。Node.js在所有的exit事件监听器都被调用之前不会终止
     * 
     * 执行Node.js的shell应该看到退出代码为“1”。
     *
     * 调用`process.exit（）`将强制进程尽快退出，即使仍有异步操作挂起尚未完全完成，包括对`process.stdout`和`process.stderr`的I/O操作。
     * 
     * 在大多数情况下，实际上没有必要显式调用`process.exit（）`。
     * Node.js进程将自行退出（如果事件循环中没有额外的工作挂起）。可以设置`process.exitCode`属性来告诉进程在正常退出时使用哪个退出代码
     * 
     * 这是有问题的原因是因为在Node.js中写入`process.stdout`有时是异步的，并且可能发生在Node.js事件循环的多个tick上。但是，调用`process.exit（）`会强制进程在执行对`stdout`的额外写入之前退出。
     * 而不是直接调用`process.exit（）`，代码应该设置 `process.exitCode`，并通过避免为事件循环调度任何额外的工作来允许进程自然退出：
     * 
     * 如果由于错误情况需要终止 Node.js 进程，则抛出未捕获的错误并允许进程相应地终止比调用 process.exit() 更安全
     * 在Worker线程中，此函数停止当前线程而不是当前进程。
     * @since v0.1.13
     * @param [code=0] 退出密码对于字符串类型，只有整数字符串（例如，“1”允许。
     */
    exit(code?: number): never;
    /**
     * 一个数字，当进程正常退出或通过{@link exit}退出而不指定代码时，它将成为进程退出代码。
     * 如果将代码添加到{@link exit}，则会覆盖之前对`process.exitCode`的任何设置。
     * @since v0.11.8
     */
    exitCode?: number | undefined;
    /**
     * `process.getgid（）`方法返回进程的数字组标识。(See [`getgid(2)`](http://man7.org/linux/man-pages/man2/getgid.2.html).)
     *
     * 此功能仅在POSIX平台上可用（即不是Windows或Android）。
     * @since v0.1.31
     */
    getgid?: () => number;
    /**
     * `process. setgid（）`方法设置进程的组标识。(See [`setgid(2)`](http://man7.org/linux/man-pages/man2/setgid.2.html).) 
     * id可以作为数字ID或组名字符串传递。
     * 如果指定了组名，此方法将在解析关联的数字ID时阻塞。
     *
     * 此功能仅在POSIX平台上可用（即不是Windows或Android）。
     * 此功能在“Worker”线程中不可用。
     * @since v0.1.31
     * @param id 组名称或ID
     */
    setgid?: (id: number | string) => void;
    /**
     * `process.getuid（）`方法返回进程的数字用户标识。 (See [`getuid(2)`](http://man7.org/linux/man-pages/man2/getuid.2.html).)
     *
     * 此功能仅在POSIX平台上可用（即不是Windows或Android）。
     * 
     * @since v0.1.28
     */
    getuid?: () => number;
    /**
     * `process.setuid（id）`方法设置进程的用户标识。(See [`setuid(2)`](http://man7.org/linux/man-pages/man2/setuid.2.html).) 
     * “id”可以作为数字ID或用户名字符串传递。
     * 如果指定了用户名，则该方法在解析关联的数字ID时阻塞。
     *
     * 此功能仅在POSIX平台上可用（即不是Windows或Android）。
     * 此功能在“Worker”线程中不可用。
     * @since v0.1.28
     */
    setuid?: (id: number | string) => void;
    /**
     * `process.geteuid（）`方法返回进程的数字有效用户标识。 (See [`geteuid(2)`](http://man7.org/linux/man-pages/man2/geteuid.2.html).)
     *
     * 此功能仅在POSIX平台上可用（即不是Windows或Android）。
     * @since v2.0.0
     */
    geteuid?: () => number;
    /**
     * `process.seteuid（）`方法设置进程的有效用户标识。(See [`seteuid(2)`](http://man7.org/linux/man-pages/man2/seteuid.2.html).) 
     * “id”可以作为数字ID或用户名字符串传递。
     * 如果指定了用户名，则该方法在解析关联的数字ID时阻塞。
     *
     * 此功能仅在POSIX平台上可用（即不是Windows或Android）。
     * 此功能在“Worker”线程中不可用。
     * @since v2.0.0
     * @param id 用户名或ID
     */
    seteuid?: (id: number | string) => void;
    /**
     * `process.getegid（）`方法返回Node.js进程的数字有效组标识。(See [`getegid(2)`](http://man7.org/linux/man-pages/man2/getegid.2.html).)
     *
     * 此功能仅在POSIX平台上可用（即不是Windows或Android）。
     * @since v2.0.0
     */
    getegid?: () => number;
    /**
     * `process.setegid（）`方法设置进程的有效组标识。 (See [`setegid(2)`](http://man7.org/linux/man-pages/man2/setegid.2.html).) 
     * id可以作为数字ID或组名字符串传递。
     * 如果指定了组名，则此方法在解析关联的数字ID时阻塞.
     *
     *
     * 此功能仅在POSIX平台上可用（即不是Windows或Android）
     * 此功能在“Worker”线程中不可用。
     * @since v2.0.0
     * @param id A group name or ID
     */
    setegid?: (id: number | string) => void;
    /**
     * `process.getgroups（）`方法返回一个包含补充组ID的数组。如果包含有效的组ID，POSIX将其保留为未指定，但Node.js确保它始终如此。
     *
     * 此功能仅在POSIX平台上可用（即不是Windows或Android）。
     * @since v0.9.4
     */
    getgroups?: () => number[];
    /**
     * `process.setgroups（）`方法为Node.js进程设置辅助组ID。这是一个特权操作，需要Node.js进程具有root或CAP_SETGID权限。
     * 
     * “groups”数组可以包含数字组ID、组名称或两者。
     * 
     * 此功能仅在POSIX平台上可用（即不是Windows或Android）。
     * 此功能在“Worker”线程中不可用。
     * @since v0.9.4
     */
    setgroups?: (groups: ReadonlyArray<string | number>) => void;
    /**
     * `process.setUncaughtExceptionCaptureCallback（）`函数设置一个函数，当发生未捕获的异常时将调用该函数，该函数将接收异常值本身作为其第一个参数。
     * 如果设置了这样的函数，则不会发出“uncaughtException "事件。如果`--abort-on-uncaught-exception`是从命令行传递的，或者通过`v8.setFlagsFromString（）`设置的，则进程不会中止。
     * 配置为在异常（如报告生成）时发生的操作也将受到影响
     * 如果要取消设置捕获函数，可以使用`process.setUncaughtExceptionCaptureCallback（null）`。当另一个捕获函数被设置时，使用非null参数调用此方法将抛出错误。
     * 使用此函数与使用已弃用的“domain”内置模块是互斥的。
     * @since v9.3.0
     */
    setUncaughtExceptionCaptureCallback(cb: ((err: Error) => void) | null): void;
    /**
     * 是否已使用{@link setUncaughtExceptionCaptureCallback}设置回调。
     * @since v9.3.0
     */
    hasUncaughtExceptionCaptureCallback(): boolean;
    /**
     * `process.sourceMapsEnabled`属性返回是否启用了 [Source Map v3](https://sourcemaps.info/spec.html)对堆栈跟踪的支持。
     * @since v20.7.0
     * @experimental
     */
    readonly sourceMapsEnabled: boolean;
    /**
     * 此函数启用或禁用[Source Map v3](https://sourcemaps.info/spec.html)对堆栈跟踪的支持。
     * 
     * 它提供了与使用命令行选项`--enable-source-maps`启动Node.js进程相同的功能。
     *
     * 只有在启用源映射后加载的JavaScript文件中的源映射才会被解析和加载。
     * @since v16.6.0, v14.18.0
     * @experimental
     */
    setSourceMapsEnabled(value: boolean): void;
    /**
     * `process.version`属性包含Node.js版本字符串。
     *
     * 要获取不带前缀_v_的版本字符串，请使用`process.versions.node`。
     * @since v0.1.3
     */
    readonly version: string;
    /**
     * `process.versions`属性返回一个对象，其中列出了Node.js的版本字符串及其依赖项。
     * “process.versions.modules”表示当前ABI版本，当C++ API更改时，该版本会增加。Node.js将拒绝加载针对不同模块ABI版本编译的模块。
     * 
     * @since v0.2.0
     */
    readonly versions: ProcessVersions;
    /**
     * `process.config`返回一个冻结的 Object，其中包含用于编译当前 Node.js 可执行文件的配置选项的 JavaScript 表示。这与运行 ./configure 脚本时生成的 config.gypi 文件相同
     * 
     * @since v0.7.7
     */
    readonly config: ProcessConfig;
    /**
     * `process.kill（）`方法向`pid`标识的进程发送`信号`。
     *
     * 信号名称是字符串，如“SIGINT "或”SIGHUP"。有关详细信息，请参阅`Signal Events`和[`kill(2)`](http://man7.org/linux/man-pages/man2/kill.2.html)。
     *
     * 如果目标`pid`不存在，此方法将抛出错误。作为一个特殊的情况，信号“0”可以用来测试进程的存在。
     * 如果`pid`被用来杀死一个进程组，Windows平台将抛出一个错误。
     *
     * 尽管这个函数的名字是`process.kill（）`，但它实际上只是一个信号发送器，就像`kill`系统调用一样。发送的信号可能会执行除杀死目标进程以外的操作。
     * 
     * 当Node.js进程接收到SIGUSR1时，Node.js将启动调试器。参见“信号事件”。
     * @since v0.0.6
     * @param pid A process ID
     * @param [signal='SIGTERM'] The signal to send, either as a string or number.
     */
    kill(pid: number, signal?: string | number): true;
    /**
     * `process.pid`属性返回进程的PID。
     *
     * @since v0.1.15
     */
    readonly pid: number;
    /**
     * `process.ppid`属性返回当前进程的父进程的PID。
     *
     * @since v9.2.0, v8.10.0, v6.13.0
     */
    readonly ppid: number;
    /**
     * `process.title`属性返回当前进程标题（即返回`ps`的当前值）。将新值添加到`process.title`修改`ps`的当前值。
     *
     * 当分配新值时，不同的平台将对标题施加不同的最大长度限制。
     * 这种限制通常是有限的。 例如，在Linux和macOS上，“process.title”被限制为二进制名称的大小加上命令行参数的长度，
     * 因为设置“process.title”会覆盖进程的“argv”内存。Node.js v0.8 允许更长的进程标题字符串，同时也会占用'environ'内存，但这在某些情况下（相当模糊）可能不安全和令人困惑。
     *
     * 将值设置为`process.title`可能不会在进程管理器应用程序（如macOS Activity Monitor或Windows Services Manager）中生成准确的标签。
     * @since v0.1.104
     */
    title: string;
    /**
     * 编译Node.js二进制文件的操作系统CPU架构。 可能的值为：`'arm'`, `'arm64'`, `'ia32'`, `'loong64'`, `'mips'`,`'mipsel'`, `'ppc'`, `'ppc64'`, `'riscv64'`, `'s390'`, `'s390x'`, and `'x64'`.
     * @since v0.5.0
     */
    readonly arch: Architecture;
    /**
     * `process.platform`属性返回一个字符串，标识编译Node.js二进制文件的操作系统平台。
     *
     * 目前可能的值是：
     *
     * * `'aix'`
     * * `'darwin'`
     * * `'freebsd'`
     * * `'linux'`
     * * `'openbsd'`
     * * `'sunos'`
     * * `'win32'`
     *
     * 如果Node.js构建在 Android操作系统。然而，Node.js中的Android支持 [is experimental](https://github.com/nodejs/node/blob/HEAD/BUILDING.md#androidandroid-based-devices-eg-firefox-os).
     * @since v0.1.16
     */
    readonly platform: Platform;
    /**
     * `process.mainModule`属性提供了另一种检索`require.main`的方法。
     * 不同的是，如果主模块在运行时发生更改，`require.main`仍然可以引用更改发生之前所需的模块中的原始主模块。
     * 一般来说，可以安全地假设这两个引用同一个模块。
     * 
     * 与`require.main`一样，如果没有入口脚本，`process.mainModule`将是`undefined`。
     * @since v0.1.17
     * @deprecated Since v14.0.0 - Use `main` instead.
     */
    mainModule?: Module | undefined;
    memoryUsage: MemoryUsageFn;
    /**
     * 根据操作系统施加的限制，获取进程可用的内存量（以字节为单位）。如果没有此类约束，或者约束未知，则返回`undefined`。
     *
     * See [`uv_get_constrained_memory`](https://docs.libuv.org/en/v1.x/misc.html#c.uv_get_constrained_memory) for more information.
     * @since v19.6.0, v18.15.0
     * @experimental
     */
    constrainedMemory(): number | undefined;
    /**
     * `process.cpuUsage（）`方法返回当前进程的用户和系统CPU时间使用情况，在具有属性`user`和`system`的对象中，其值为微秒值（百万分之一秒）。
     * 这些值分别测量用户和系统代码所花费的时间，如果多个CPU内核正在为此进程执行工作，则可能会超过实际运行时间。
     * 
     * 前面调用`process.cpuUsage（）`的结果可以作为参数传递给函数，以获得差异阅读。
     *
     * @since v6.1.0
     * @param previousValue 调用`process.cpuUsage（）`的前一个返回值
     */
    cpuUsage(previousValue?: CpuUsage): CpuUsage;
    /**
     * `process.nextTick（）`将 callback 添加到 "下一个滴答队列"
     * 在JavaScript堆栈上的当前操作运行完成之后，并且在允许事件循环继续之前，该队列被完全排空。
     * 如果要递归调用`process.nextTick（）`，则可能会创建一个无限循环。
     * 有关更多背景信息，请参阅[Event Loop]（https：//nodejs.org/zh/docs/guides/event-loop-timers-and-nexttick/#process-nexttick）指南。
     * 让用户有机会在对象构建之后但在任何 I/O 发生之前分配事件处理程序
     * 
     * @since v0.1.26
     * @param args 调用`callback`时要传递的其他参数
     */
    nextTick(callback: Function, ...args: any[]): void;
    /**
     * `process.release`属性返回一个`Object`，其中包含与当前版本相关的元数据，包括源tarball和仅头文件tarball的URL
     *
     * 在从源代码树的非发布版本进行的自定义构建中，可能只存在“name”属性。不应依赖其他属性的存在。
     * @since v3.0.0
     */
    readonly release: ProcessRelease;
    features: {
        inspector: boolean;
        debug: boolean;
        uv: boolean;
        ipv6: boolean;
        tls_alpn: boolean;
        tls_sni: boolean;
        tls_ocsp: boolean;
        tls: boolean;
    };
    /**
     * `process.umask（）`返回Node.js进程的文件模式创建掩码。子进程继承父进程的掩码。
     * @since v0.1.19
     * @deprecated 不带参数调用`process.umask（）`会导致进程范围的umask被写入两次。这在线程之间引入了竞争条件， 安全漏洞。没有安全的跨平台替代API。
     */
    umask(): number;
    /**
     * 只有不在工作线程中时才能设置。
     */
    umask(mask: string | number): number;
    /**
     * process. js（）方法返回当前Node.js进程运行的秒数。
     *
     * 返回值包括秒的分数。使用`Math.floor（）`获取整秒。
     * @since v0.5.0
     */
    uptime(): number;
    // 监控时间差
    hrtime: HRTime;
    /**
     * 如果Node.js进程是通过IPC通道生成的，则process.channel属性是对IPC通道的引用。
     * 如果不存在IPC通道，则此属性未定义。
     * @since v7.1.0
     */
    channel?: {
        /**
         * 如果之前调用了.unref（），此方法使IPC通道保持进程的事件循环运行。
         * 通常，这是通过 process 对象上的 'disconnect' 和 'message' 监听器的数量来管理的。但是，此方法可用于显式请求特定行为。
         * @since v7.1.0
         */
        ref(): void;
        /**
         * 此方法使IPC通道不保持进程的事件循环运行，即使在通道打开时也让它完成。
         * 通常，这是通过 process 对象上的 'disconnect' 和 'message' 监听器的数量来管理的。但是，此方法可用于显式请求特定行为。
         * @since v7.1.0
         */
        unref(): void;
    };
    /**
     * 如果Node.js是通过IPC通道生成的，则可以使用`process.send（）`方法向父进程发送消息。消息将作为父级的`ChildProcess`对象上的“消息”事件接收。
     * 如果Node.js没有使用IPC通道生成，则`process.send`将是`undefined`。
     *
     * 消息经过序列化和解析。生成的消息可能与最初发送的消息不同。
     * @since v0.5.9
     * @param options 用于参数化某些类型句柄的发送。` options`支持以下属性：
     */
    send?(
        message: any,
        sendHandle?: any,
        options?: {
            swallowErrors?: boolean | undefined;
        },
        callback?: (error: Error | null) => void,
    ): boolean;
    /**
     * 如果Node.js进程是通过IPC通道生成的（参见`Child Process`和`Cluster`文档），`process.disconnect（）`方法将关闭父进程的IPC通道，允许子进程在没有其他连接保持其活动时优雅地退出。
     * 
     * 调用`process.disconnect（）`的效果与从父进程调用`ChildProcess.disconnect（）`的效果相同。
     *
     * 如果Node.js进程没有使用IPC通道生成，则`process.disconnect（）`将为`undefined`。
     * @since v0.7.2
     */
    disconnect(): void;
    /**
     * 如果 Node.js 进程是使用 IPC 通道生成的，只要连接了 IPC 通道，process.connected 属性就会返回 true，并在调用 process.disconnect() 后返回 false
     * 一旦 process.connected 为 false，就不能再使用 process.send() 通过 IPC 通道发送消息。
     * @since v0.7.2
     */
    connected: boolean;
    /**
     * `process.allowedNodeEnvironmentFlags`属性是`NODE_OPTIONS`环境变量中允许的特殊只读标志`Set`。
     *
     * `process.allowedNodeEnvironmentFlags`扩展`Set`，但覆盖`Set.prototype.has`以识别几种不同的可能标志表示。在以下情况下，`process.allowedNodeEnvironmentFlags.has（）`将返回`true`：
     * 
     * * 标志可以省略前导单 (-) 或双 (--) 破折号；例如，inspect-brk 代表 --inspect-brk，r 代表 -r
     * * 传递给 V8 的标志（如 --v8-options 中所列）可以将一个或多个非前导破折号替换为下划线，反之亦然；例如，--perf_basic_prof、--perf-basic-prof、--perf_basic-prof 等
     * * 标志可能包含一个或多个等号 (=) 字符；包括第一个等号在内的所有字符都将被忽略；例如，--stack-trace-limit=100。
     * * NODE_OPTIONS 中必须允许标志
     *
     * `process.allowedNodeEnvironmentFlags`的方法`add（）`、`clear（）`和`delete（）`不执行任何操作，并且会以静默方式失败。
     *
     * 如果Node.js在编译时不支持_ `NODE_OPTIONS`（如{@link config}所示），则`process.allowedNodeEnvironmentFlags`将包含允许的内容
     * @since v10.10.0
     */
    allowedNodeEnvironmentFlags: ReadonlySet<string>;
    /**
     * `process.report`是一个对象，其方法用于为当前进程生成诊断报告。其他文件见“报告文件”。
     * @since v11.8.0
     */
    report?: ProcessReport | undefined;
    /**
     * 
     * @since v12.6.0
     * @return 当前进程的资源使用情况。所有这些值都来自`uv_getrusage`调用，该调用返回一个[`uv_rusage_t` struct][uv_rusage_t]。
     */
    resourceUsage(): ResourceUsage;
    /**
     * `process.traceDeprecation`属性表示当前Node.js进程是否设置了`--trace-deprecation`标志。
     * 有关此标志行为的详细信息，请参阅“warning”事件和“emitWarning（）”方法的文档。
     * @since v0.8.0
     */
    traceDeprecation: boolean;
    /* EventEmitter */
    on(event: "beforeExit", listener: BeforeExitListener): this;// 退出之前事件
    on(event: "exit", listener: ExitListener): this;// 退出事件（触发条件：process.exit()、程序结束）
    on(event: "disconnect", listener: DisconnectListener): this;//  进程是使用 IPC 通道生成的，则当 IPC 通道关闭时将触发
    on(event: "message", listener: MessageListener): this;// 进程是使用 IPC 通道生成的，则只要子进程收到父进程使用 childprocess.send() 发送的消息，就会触发
    on(event: "rejectionHandled", listener: RejectionHandledListener): this;// 当 Promise 被拒绝catch完时会触发（可以定期在错误日志中记录此类错误）
    on(event: "uncaughtException", listener: UncaughtExceptionListener): this;// 当 Promise 被拒绝并且未捕获时会触发（可以定期在错误日志中记录此类错误）
    on(event: "uncaughtExceptionMonitor", listener: UncaughtExceptionListener): this;// 在 'uncaughtException' 事件触发或通过 process.setUncaughtExceptionCaptureCallback() 安装的钩子被调用之前触发
    on(event: "unhandledRejection", listener: UnhandledRejectionListener): this;// 当 Promise 被拒绝并且未处理时会触发
    /**
     * 默认情况下，Node.js 会将进程警告打印到 stderr。
     * --no-warnings 命令行选项可用于抑制默认控制台输出，但 'warning' 事件仍将由 process 对象触发
     * --trace-warnings 命令行选项可用于使警告的默认控制台输出包括警告的完整堆栈跟踪
     * --throw-deprecation 命令行标志启动 Node.js 将导致自定义弃用警告作为异常抛出
     * --trace-deprecation 命令行标志将导致自定义弃用与堆栈跟踪一起打印到 stderr
     * --no-deprecation 命令行标志将抑制自定义弃用的所有报告
     * *-deprecation 命令行标志仅影响使用名称 'DeprecationWarning' 的警告
     */
    on(event: "warning", listener: WarningListener): this;// 触发进程警告
    on(event: Signals, listener: SignalsListener): this;// 当 Node.js 进程收到信号时，则将触发信号事件(在 Worker 线程上不可用)
    on(event: "multipleResolves", listener: MultipleResolveListener): this;// 已被弃用
    on(event: "worker", listener: WorkerListener): this;// 创建新的 <Worker> 线程后会触发 'worker' 事件
}
```

# 示例
```text
const process = require('node:process');
// // 进程退出事件
// process.on('beforeExit', (code) => {
//   console.log('进程beforeExit事件，code: ', code);
// });
// process.on('exit', (code) => {
//   console.log('进程exit事件，code: ', code);// 触发条件：process.exit()、程序结束
// });
// 进程异常事件
// const unhandledRejections = new Map();// 未处理异常
// process.on('unhandledRejection', (reason, promise) => {
//   unhandledRejections.set(promise, reason);// 未处理拒绝增加
// });
// process.on('rejectionHandled', (promise) => {
//   unhandledRejections.delete(promise);// 拒绝的异常删除
// });
// // 警告，在检测到可能导致次优应用性能、错误或安全漏洞的不良编码实践时触发警告。
// process.on('warning', (warning) => {
//     console.warn(warning.name);    // Print the warning name
//     console.warn(warning.message); // Print the warning message
//     console.warn(warning.stack);   // Print the stack trace
// });
// 信号事件处理
// process.stdin.resume();// 开始从stdin阅读，这样进程就不会退出。
// process.on('SIGINT', () => {
//   console.log('收到信号情报按Control-C退出。');
// });
// function handle(signal) {
//   console.log(`接收 ${signal}`);// 使用单个函数处理多个信号
// }
// process.on('SIGINT', handle);
// process.on('SIGTERM', handle);

// NODE_OPTIONS 环境变量中允许的标志
// process.abort();// 进程立即退出并生成一个核心文件(在 Worker 线程中不可用)
// console.log(process.allowedNodeEnvironmentFlags)//  NODE_OPTIONS 环境变量中允许的特殊的只读
// process.allowedNodeEnvironmentFlags.forEach((flag) => {
//     console.log(flag)
// });// 遍历
// console.log(process.arch);// 操作系统 CPU 架构
// console.log(process.argv);// 命令行参数
// console.log(process.argv0);// 命令行参数
// console.log(process.cwd());// 启动目录
// try {
//     process.chdir('/tmp');// 修改
//   console.log(process.cwd());
// } catch (err) {
//   console.error(err);
// }
// console.log(process.config)// 编译当前 Node.js 可执行文件的配置选项
// console.log(process.connected)// 是否连接了 IPC 通道
// console.log(process.constrainedMemory())// 进程可用的内存量

// const startUsage = process.cpuUsage();// 当前进程的用户和系统CPU时间使用情况，其值为微秒值（百万分之一秒）。
// console.log(startUsage)// { user: 38579, system: 6986 }
// // 等待500毫秒
// const now = Date.now();
// while (Date.now() - now < 500);
// console.log(process.cpuUsage(startUsage));// { user: 514883, system: 11226 }

// console.log(process.debugPort)// 启用时 Node.js 调试器使用的端口

// process.emitWarning('触发警告!', {
//     code: '我的警告',
//     detail: '详情',
// });// 手动发射警告会触发warning事件

// console.log(process.env)// 环境变量
// process.env.test = 1;// 添加
// console.log(process.env.test)// 环境变量
// delete process.env.test// 删除
// console.log(process.env.test)// 环境变量

// console.log(process.execArgv)// 返回命令行选项（脚本前面），node --harmony script.js --version 返回 ['--harmony']
// console.log(process.execPath)// 可执行文件的绝对路径名
// process.exit(1);// 以 code 的退出状态同步终止进程
// console.log(process.getegid);// 进程的数字有效群组标识

// process.on('SIGHUP', () => {
//     console.log('Got SIGHUP signal.');
// });

// setTimeout(() => {
//     console.log('Exiting.');
//     // process.exit(0);
// }, 100);

// process.kill(process.pid, 'SIGHUP');// 

// console.log(process.mainModule)// 入口脚本
// console.log(process.memoryUsage())// 进程的内存使用量
// console.log(process.memoryUsage.rss());// 内存

// nextTick将 callback 添加到 "下一个滴答队列"
// console.log('start');
// process.nextTick(() => {
//   console.log('nextTick callback');
// });
// console.log('scheduled');

console.log(process.noDeprecation)// 是否在进程上设置了 --no-deprecation 标志
console.log(process.permission)// 其方法用于管理当前进程的权限
```
