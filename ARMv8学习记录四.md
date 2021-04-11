---
title: ARMv8学习记录四
date: 2021-03-18 09:46:39
tags: 汇编
category: ARMv8汇编
---

> 本文主要讲 ARMv7 逆向，下一篇将 ARMv8 逆向。

# IEEE 浮点数

通常一个浮点数由符号、尾数、基数和指数组成。尾数的位数用于确定精度；指数的位数用于确定能表示的数的范围。

单精度浮点数为 32 位，具有 24 位有效数字，双精度浮点数位 64 位，具有 53 位有效数字。其记录格式如下：

![](2021-03-18-19-48-36.png)

单精度：31位为符号位，22~30 位为指数位，0~22 位为尾数位。其中指数位为指数值加上 127，尾数位为小鼠点后对应的二进制数。

双精度：63位为符号位，51~62  位为指数位，0~51 位为尾数位。其中指数位为指数值加上 1023，尾数位为小鼠点后对应的二进制数。

**例1：** 7.625 浮点数二进制数表示为：

整数部分：111

小数部分：
```
0.625 * 2 = 1.25，	整数位为 1 ==> 0.1
0.25 * 2 = 0.5，	整数位为 0 ==> 0.10
0.5 * 2 = 1，		整数位为 1 ==> 0.101
```
最终结果为 111.101 = 1.11101 * 10^2，对应的单精度浮点格式为
```
0   10000001   11101000000000000000000        ==> 0x40F400
--- ------------ -------------------------------
正数  指数为 127+ 2        尾数（小数部分）
```

对应的双精度浮点格式为
```
0   10000000001    1110100000000000000000000000000000000000000000000000  ==> 0x401E800000000000
--- -------------  ---------------------------------------------------
正数  指数为 1023+2      尾数（小数部分）
```

**例2：** 0.625 的二进制数为 0.101= 1.01*10^-1 ；对应的单精度浮点格式为
```
0         01111110         01000000000000000000000    ==> 0x 3F20 0000
----- ----------------    -------------------------------
正数  指数为 127+ (-1)        尾数（小数部分）
```

**例3：** -7.625 的二进制数为 -1.11101*10^2；对应的单精度浮点格式为
```
1   10000001   11101000000000000000000     ==> 0xC0F4 0000
--- ------------ -------------------------------
正数  指数为 127+ 2        尾数（小数部分）
```

# ARMv7 基本数据类型
源码
```c
# include <stdio.h>

int main(int argc, char const *argv[])
{
    float f1 = 7.625f, f2 = 0.625f, f3 = -7.625f;
    double d1 = 7.625, d2 = 0.625, d3 = -7.625;
    char a = 'a';
    unsigned short u2 = 265;
    unsigned int u4 = 123;
    unsigned long u8 = 456;
    unsigned long long uu8 =789;

    short s2 = -265;
    int i4 = -123;
    long l8 = -456;
    long long ll8 =-789;

    return 0;
}
```
使用下列命令生成 ARMv7 汇编文件。
```
➜  export PATH=$PATH:$ANDROID_HOME/ndk/21.0.6113669/toolchains/llvm/prebuilt/linux-x86_64/bin
➜  clang -target arm-linux-android21 -S basic.c -o basic.s 
➜  clang -target arm-linux-android21 basic.s  -o basic
```

使用 IDA 查看对应的汇编代码：
```armasm
.text:0000039C                 SUB             SP, SP, #0x60
.text:000003A0                 MOV             R2, #0
.text:000003A4                 STR             R2, [SP,#0x60+var_4]
.text:000003A8                 STR             R0, [SP,#0x60+var_8]
.text:000003AC                 STR             R1, [SP,#0x60+var_C]
.text:000003B0                 MOV             R0, #0x40F40000 ; R0 = 0x40F40000  => 7.725f
.text:000003B8                 STR             R0, [SP,#0x60+var_10] ; 使用栈空间保存 7.725 , 即局部变量都保存在栈中，下面的操作类似。
.text:000003BC                 MOV             R0, #0x3F200000
.text:000003C4                 STR             R0, [SP,#0x60+var_14]
.text:000003C8                 MOV             R0, #0xC0F40000
.text:000003D0                 STR             R0, [SP,#0x60+var_18]
.text:000003D4                 MOV             R0, #0x401E8000
.text:000003DC                 STR             R0, [SP,#0x60+var_1C] ; 对应着 double 类型的高32位
.text:000003E0                 STR             R2, [SP,#0x60+var_20] ; 对应着 double 类型的低32位，即最终值为 0x401E 8000 0000 0000
.text:000003E4                 MOV             R0, #0x3FE40000
.text:000003EC                 STR             R0, [SP,#0x60+var_24]
.text:000003F0                 STR             R2, [SP,#0x60+var_28]
.text:000003F4                 MOV             R0, #0xC01E8000
.text:000003FC                 STR             R0, [SP,#0x60+var_2C]
.text:00000400                 STR             R2, [SP,#0x60+var_30]
.text:00000404                 MOV             R0, #0x61 ; 'a'
.text:00000408                 STRB            R0, [SP,#0x60+var_31]
.text:0000040C                 MOV             R0, #0x109
.text:00000414                 STRH            R0, [SP,#0x60+var_34]
.text:00000418                 MOV             R0, #0x7B ; '{'
.text:0000041C                 STR             R0, [SP,#0x60+var_38]
.text:00000420                 MOV             R0, #0x1C8
.text:00000424                 STR             R0, [SP,#0x60+var_3C]
.text:00000428                 STR             R2, [SP,#0x60+var_44]
.text:0000042C                 MOV             R0, #0x315
.text:00000434                 STR             R0, [SP,#0x60+var_48]
.text:00000438                 MOV             R0, #0xFEF7
.text:00000440                 STRH            R0, [SP,#0x60+var_4A]
.text:00000444                 MOV             R0, #0xFFFFFF85
.text:00000448                 STR             R0, [SP,#0x60+var_50]
.text:0000044C                 LDR             R0, =0xFFFFFE38
.text:00000450                 STR             R0, [SP,#0x60+var_54]
.text:00000454                 MOV             R0, #0xFFFFFFFF
.text:00000458                 STR             R0, [SP,#0x60+var_5C] ; 高32位为 -1
.text:0000045C                 MOV             R0, #0xFFFFFCEB
.text:00000460                 STR             R0, [SP,#0x60+var_60] ; 低32位位-789;
.text:00000464                 MOV             R0, R2
.text:00000468                 ADD             SP, SP, #0x60 ; '`'
.text:0000046C                 BX              LR
.text:0000046C ; End of function main
```

# ARMv7 基本运算
## 加、减、乘和位运算
源码
```c
# include <stdio.h>

int main(int argc, char const *argv[])
{
    float f1 = 7.625f;
    double d1 = 7.625;
    char a = 'a';
    unsigned short u2 = 265;
    unsigned int u4 = 123;
    unsigned long u8 = 456;
    unsigned long long uu8 =789;

    short s2 = -265;
    int i4 = -123;
    long l8 = -456;
    long long ll8 =-789;

    d1 = d1 + f1;
    d1 = f1 + argc;
    a = a + 1;
    u4 = u4 + argc;
    u4 = u4 + u8;
    i4 = s2 + argc;
    u4 = f1 + argc;

    d1 = f1 - a;
    d1 = d1 - f1;

    a = a - 5;
    u8 = u8 - u2;
    i4 = s2 - argc;
    u4 = f1 - argc;

    u4 = argc * 4;
    u8 = f1 * 4 * argc;

    u8 = i4 * 5;

    return 0;
}
```


## 除法和模运算


# ARMv7 流程控制

## ARMv7 条件执行

## ARMv7 循环

# ARMv7 结构体和类


# ARMv7 异常

