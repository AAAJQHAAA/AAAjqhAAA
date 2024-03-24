# c语言入门
```c
/* 
#include预处理命令，用来引入头文件
printf函数在stdio.h(标准输入输出头文件) 中 
<> 表示系统自带的库
" " 表示用户自定义的库（如果自定义的库里面没有这个文件系统会自动查找自带的库）
*/
#include <stdio.h>

/* 
C 语言入口main函数，
 argc参数个数，
 argv是参数值字符串数组（第一个是程序名，后面是参数） 
*/
int main(int argc, char *argv[])
{
    /* 我的第一个 C 程序 */
    printf("Hello, World! \n");
 
    return 0;
}
```
# C 环境设置
- C 编译器
    - UNIX/Linux 上：`gcc -v`
    - Mac OS 上：`gcc -v`
    - Windows 上[安装MinGW](https://www.runoob.com/cprogramming/c-environment-setup.html)
- [工具vscode](https://code.visualstudio.com/docs/languages/cpp)
# 编译 & 执行 C 程序
- 源代码：`hello.c`
- 编译：`gcc hello.c`，会生成一个可执行文件`hello.out`or`hello.exe`（每个平台名字有差异）
- 执行文件：`./hello.out`

- 多文件一起编译：`gcc test1.c test2.c -o main.out`
- 运行：`./main.out`
- `gcc [源文件名] -o [目标文件名]`
# C 数据类型
- 基础数据类型
    - 整型（int）
    - 字符型（char）
    - 浮点型（float）：`#include <float.h>`
    - 双精度浮点型（double）
        - `sizeof(float)`函数：`#include <limits.h>`
- 枚举类型
- void 类型
    - 函数返回为空
    - 函数参数为空
    - 指针指向 void
        - 类型为 `void *` 的指针代表对象的地址，而不是类型
- 派生类型
    - 数组类型
    - 指针类型
    - 结构体类型
- 类型转换：将一个数据类型的值转换为另一种数据类型的值
    - 隐式类型转换：在表达式中自动发生的，将一种较小的类型自动转换为较大的类型
    - 显式类型转换：需要使用强制类型转换运算符（type casting operator），它可以将一个数据类型的值强制转换为另一种数据类型的值
# C 变量
- 变量其实只不过是程序可操作的存储区的名称
    - 定义整型变量age：`int age;`
    - 定义字符型变量grade：`char grade;`
    - 定义整型指针变量ptr：`int *ptr;`
    - 定义字符型变量grade：`char grade;`
    - 定义bool类型`bool flag = false;`（`<stdbool.h>`）
    
- 全局变量保存在内存的全局存储区中，占用静态的存储单元；
- 局部变量保存在栈中，只有在所在函数被调用时才动态地为变量分配存储单元。
