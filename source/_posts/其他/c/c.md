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
        
