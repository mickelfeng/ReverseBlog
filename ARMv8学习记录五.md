---
title: ARMv8学习记录五
date: 2021-04-10 13:23:52
tags: 汇编
category: ARMv8汇编
---


# 基本数据类型
使用下列命令，生产 ARMv8 汇编文件。
```bash
➜ clang -target aarch64-linux-android21 -S basic.c -o basic64.s
➜ clang -target arm-linux-android21 basic64.s  -o basic64
```

使用 IDA 查看对应的汇编代码：
```armasm
.text:00000000000005D8                 SUB             SP, SP, #0x70
.text:00000000000005DC                 MOV             W8, #0
.text:00000000000005E0                 MOV             W9, #0x40F40000
.text:00000000000005E4                 FMOV            S0, W9  ; S0 =  7.725f，ARMv8 中直接使用浮点寄存器存储浮点数。
.text:00000000000005E8                 FMOV            S1, #0.625
.text:00000000000005EC                 MOV             W9, #0xC0F40000
.text:00000000000005F0                 FMOV            S2, W9
.text:00000000000005F4                 MOV             X10, #0x401E800000000000
.text:00000000000005FC                 FMOV            D3, X10
.text:0000000000000600                 FMOV            D4, #0.625
.text:0000000000000604                 MOV             X10, #0xC01E800000000000
.text:000000000000060C                 FMOV            D5, X10
.text:0000000000000610                 MOV             W9, #0x61 ; 'a'
.text:0000000000000614                 MOV             W11, #0x109
.text:0000000000000618                 MOV             W12, #0x7B ; '{'
.text:000000000000061C                 MOV             X10, #0x1C8
.text:0000000000000620                 MOV             X13, #0x315
.text:0000000000000624                 MOV             W14, #0xFEF7
.text:0000000000000628                 MOV             W15, #0xFFFFFF85
.text:000000000000062C                 MOV             X16, #0xFFFFFFFFFFFFFE38
.text:0000000000000630                 MOV             X17, #0xFFFFFFFFFFFFFCEB
.text:0000000000000634                 STR             WZR, [SP,#0x70+var_4]
.text:0000000000000638                 STR             W0, [SP,#0x70+var_8]
.text:000000000000063C                 STR             X1, [SP,#0x70+var_10]
.text:0000000000000640                 STR             S0, [SP,#0x70+var_14]
.text:0000000000000644                 STR             S1, [SP,#0x70+var_18]
.text:0000000000000648                 STR             S2, [SP,#0x70+var_1C]
.text:000000000000064C                 STR             D3, [SP,#0x70+var_28]
.text:0000000000000650                 STR             D4, [SP,#0x70+var_30]
.text:0000000000000654                 STR             D5, [SP,#0x70+var_38]
.text:0000000000000658                 STRB            W9, [SP,#0x70+var_39]
.text:000000000000065C                 STRH            W11, [SP,#0x70+var_3C]
.text:0000000000000660                 STR             W12, [SP,#0x70+var_40]
.text:0000000000000664                 STR             X10, [SP,#0x70+var_48]
.text:0000000000000668                 STR             X13, [SP,#0x70+var_50]
.text:000000000000066C                 STRH            W14, [SP,#0x70+var_52]
.text:0000000000000670                 STR             W15, [SP,#0x70+var_58]
.text:0000000000000674                 STR             X16, [SP,#0x70+var_60]
.text:0000000000000678                 STR             X17, [SP,#0x70+var_68]
.text:000000000000067C                 MOV             W0, W8
.text:0000000000000680                 ADD             SP, SP, #0x70 ; 'p'
.text:0000000000000684                 RET
```