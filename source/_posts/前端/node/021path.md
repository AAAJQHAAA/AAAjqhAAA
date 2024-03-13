# path
- `node：path`模块提供了用于处理文件和目录路径的实用程序
# ParsedPath
- 由path.parse()生成或由path.format()使用的已解析路径对象。
```text
interface ParsedPath {
    /**
     * 路径的根，如'/' or 'c:\'
     */
    root: string;
    /**
     * 完整目录路径，如 '/home/user/dir' or 'c:\path\dir'
     */
    dir: string;
    /**
     * 文件名包括扩展名（如果有），如“index.html”
     */
    base: string;
    /**
     * 文件扩展名（如果有），如“.html”
     */
    ext: string;
    /**
     * 不带扩展名的文件名（如果有），如“index”
     */
    name: string;
}
```
# FormatInputPathObject
```text
interface FormatInputPathObject {
    /**
     * 路径的根，如 '/' or 'c:\'
     */
    root?: string | undefined;
    /**
     * 完整目录路径，如 '/home/user/dir' or 'c:\path\dir'
     */
    dir?: string | undefined;
    /**
     * 文件名包括扩展名（如果有），如“index.html”
     */
    base?: string | undefined;
    /**
     * 文件扩展名（如果有），如“.html”
     */
    ext?: string | undefined;
    /**
     * 不带扩展名的文件名（如果有），如“index”
     */
    name?: string | undefined;
}
```
# PlatformPath
```text
interface PlatformPath {
    /**
     * 规范化给定的 path，解析 '..' 和 '.' 片段
     * 当发现多个斜杠时，它们将被替换为单个斜杠;
     * 当路径包含尾随斜杠时，它将被保留。在Windows上使用反斜杠
     *
     * @param path 路径
     * @throws {TypeError} 如果path不是字符串。
     */
    normalize(path: string): string;
    /**
     * 将所有参数连接在一起并规范化结果路径。
     *
     * @param paths 路径
     * @throws {TypeError} 如果任何参数不是字符串。
     */
    join(...paths: string[]): string;
    /**
     * 将路径或路径片段的序列解析为绝对路径
     * 
     * 最右边的参数被认为是{to}。其他参数被认为是{from}的数组。
     *
     * 从最左边的{from}参数开始，将{to}解析为绝对路径。
     *
     * 如果{to}还不是绝对路径，{from}参数将按从右到左的顺序前置，直到找到绝对路径。
     * 如果在使用所有{from}路径后仍然没有找到绝对路径，则也使用当前工作目录。
     * 生成的路径将被规范化，除非路径被解析为根目录，否则尾随的斜杠将被删除。
     *
     * @param paths 路径或路径片段的序列
     * @throws {TypeError} 如果任何参数不是字符串。
     */
    resolve(...paths: string[]): string;
    /**
     * 确定 path 是否为绝对路径。
     *
     * 如果给定的{path}是零长度字符串，则返回`false`。
     *
     * @param path 路径
     * @throws {TypeError} 如果path不是字符串。
     */
    isAbsolute(path: string): boolean;
    /**
     * 根据当前工作目录返回从 from 到 to 的相对路径
     * 有时我们有两条绝对路径，我们需要推导出从一条到另一条的相对路径。这实际上是path.resolve的反向转换。
     *
     * @throws {TypeError} 如果“from”或“to”不是字符串。
     */
    relative(from: string, to: string): string;
    /**
     * 返回 path 的目录名，类似于 Unix dirname 命令
     * 
     * @param path 路径
     * @throws {TypeError} 如果path不是字符串
     */
    dirname(path: string): string;
    /**
     * 返回 path 的最后一部分，类似于 Unix basename 命令
     * 通常用于从完全限定路径中提取文件名。
     *
     * @param path 路径
     * @param suffix 要删除的可选后缀
     * @throws {TypeError} 如果`path`不是一个字符串，或者如果给出了`ext`，但它不是一个字符串。
     */
    basename(path: string, suffix?: string): string;
    /**
     * 返回 path 的扩展名，即 path 的最后一部分中从最后一次出现的 .（句点）字符到字符串的结尾。
     * 如果 path 的最后一部分中没有 .，或者除了 path 的基本名称（参见 path.basename()）的第一个字符之外没有 . 个字符，则返回空字符串
     *
     * @param path 路径
     * @throws {TypeError} 如果path不是字符串。
     */
    extname(path: string): string;
    /**
     * 特定于平台的文件分隔符。 '\\' or '/'.
     */
    readonly sep: "\\" | "/";
    /**
     * 提供特定于平台的路径定界符 ';' or ':'.例如：环境变量的定界符
     */
    readonly delimiter: ";" | ":";
    /**
     * 从路径字符串返回一个对象-与format()相反。
     *
     * @param path 路径
     * @throws {TypeError} 如果path不是字符串。
     */
    parse(path: string): ParsedPath;
    /**
     * 从对象返回路径字符串-与parse()相反。
     *
     * @param pathObject 路径
     */
    format(pathObject: FormatInputPathObject): string;
    /**
     * 仅在 Windows 系统上，返回给定 path 的等效 命名空间前缀路径。如果 path 不是字符串，则 path 将不加修改地返回
     * 此方法仅在Windows系统上有意义。在POSIX系统中，该方法是不可操作的，并且总是返回路径而不进行修改。
     */
    toNamespacedPath(path: string): string;
    /**
     * 提供对 path 方法的 POSIX 特定实现的访问
     */
    readonly posix: PlatformPath;
    /**
     * 提供对 path 方法的 Windows 特定实现的访问
     */
    readonly win32: PlatformPath;
}
```

# 示例
```text
const path = require('node:path');

// 返回最后一部分
// console.log(path.posix.basename('/foo/bar/baz/asdf/quux.html'));// 'quux.html'
// 返回最后一部分去除后缀
// console.log(path.posix.basename('/foo/bar/baz/asdf/quux.html', '.html'));// 'quux'
// console.log(path.win32.basename('C:\\foo.html', '.html'));// 'foo'
// console.log(path.win32.basename('C:\\foo.HTML', '.html'));// 会区分大小写// 'foo.HTML'
// console.log(process.env.PATH);// 环境变量
// console.log(path.delimiter)// 多个路径定界符
// console.log(process.env.PATH.split(path.delimiter))// 环境变量用分隔符切割
// console.log(path.posix.dirname('/foo/bar/baz/asdf/quux'));// 返回父目录 '/foo/bar/baz/asdf'
// 返回扩展名
// console.log(path.extname('index.html'));// '.html'
// console.log(path.extname('index.coffee.md'));// '.md'
// console.log(path.extname('index.'));// '.'
// console.log(path.extname('index'));// ''
// console.log(path.extname('.index'));// ''
// console.log(path.extname('.index.md'));// '.md'
// 对象转路径字符串
// console.log(path.posix.format({
//     root: '/ignored',
//     dir: '/home/user/dir',
//     base: 'file.txt',
// }));// 如果提供了`dir`、`root`和`base`，则返回`${dir}${path.sep}${base}`。“root”被忽略。// '/home/user/dir/file.txt'
// // 如果没有指定`dir`，将使用`root`, 如果只提供了`root`或者`dir`等于`root`，那么将不包括平台分隔符。“ext”将被忽略。
// console.log(path.posix.format({
//     root: '/',
//     base: 'file.txt',
//     ext: 'ignored',
// }));// '/file.txt'
// // 如果没有指定base，将使用name + ext。
// console.log(path.posix.format({
//     root: '/',
//     name: 'file',
//     ext: '.txt',
// }));// '/file.txt'
// // 如果没有在“ext”中指定点，则将添加点
// console.log(path.posix.format({
// root: '/',
// name: 'file',
// ext: 'txt',
// }));// '/file.txt'
// console.log(path.win32.format({
//     dir: 'C:\\path\\dir',
//     base: 'file.txt',
// }));// 'C:\\path\\dir\\file.txt'
// 判断路径是否为绝对路径
// console.log(path.isAbsolute('/foo/bar')); // true
// console.log(path.isAbsolute('/baz/..'));  // true
// console.log(path.isAbsolute('qux/'));     // false
// console.log(path.isAbsolute('.'));        // false
// console.log(path.isAbsolute('//server'));    // true
// console.log(path.isAbsolute('\\\\server'));  // true
// console.log(path.isAbsolute('C:/foo/..'));   // true
// console.log(path.isAbsolute('C:\\foo\\..')); // true
// console.log(path.isAbsolute('bar\\baz'));    // false
// console.log(path.isAbsolute('bar/baz'));     // false
// console.log(path.isAbsolute('.'));           // false
// 规范化生成路径
// console.log(path.join('/foo', 'bar', 'baz/asdf', 'quux', '..'));// '/foo/bar/baz/asdf'
// console.log(path.join('foo', {}, 'bar'));// Throws 'TypeError: Path must be a string. Received {}'
// 规范化给定路径（去除多斜杠）
// console.log(path.normalize('/foo/bar//baz/asdf/quux/..'));// '/foo/bar/baz/asdf'
// console.log(path.normalize('C:\\temp\\\\foo\\bar\\..\\'));// 'C:\\temp\\foo\\'
// console.log(path.win32.normalize('C:////temp\\\\/\\/\\/foo/bar'));// 'C:\\temp\\foo\\bar'
// 路径转对象
// console.log(path.parse('/home/user/dir/file.txt'));
// console.log(path.parse('C:\\path\\dir\\file.txt'));
// 返回从 from 到 to 的相对路径
// console.log(path.relative('/data/orandea/test/aaa', '/data/orandea/impl/bbb'));// '../../impl/bbb'
// console.log(path.relative('C:\\orandea\\test\\aaa', 'C:\\orandea\\impl\\bbb'));// '..\\..\\impl\\bbb'
// 多个路径解析成绝对路径
// console.log(path.resolve('/foo/bar', './baz'));// '/foo/bar/baz'
// console.log(path.resolve('/foo/bar', '/tmp/file/'));// '/tmp/file'
// console.log(path.resolve('wwwroot', 'static_files/png/', '../gif/image.gif'));// 相对当前目录
console.log(path.posix.sep)// 文件分隔符
console.log(path.win32.sep)// 文件分隔符
```
