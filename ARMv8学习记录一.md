---
title: ARMv8学习记录一
date: 2020-02-28 23:02:16
tags: 汇编
category: ARMv8汇编
---
<!-- TOC -->

- [从一个 HelloWorld 开始](#%E4%BB%8E%E4%B8%80%E4%B8%AA-helloworld-%E5%BC%80%E5%A7%8B)
    - [使用 gcc 编译](#%E4%BD%BF%E7%94%A8-gcc-%E7%BC%96%E8%AF%91)
    - [使用 clang 编译](#%E4%BD%BF%E7%94%A8-clang-%E7%BC%96%E8%AF%91)
    - [ndk-build 编译](#ndk-build-%E7%BC%96%E8%AF%91)
        - [方法1：](#%E6%96%B9%E6%B3%951)
        - [方法2：](#%E6%96%B9%E6%B3%952)
        - [方法3：](#%E6%96%B9%E6%B3%953)
- [调试环境搭建](#%E8%B0%83%E8%AF%95%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA)
    - [安装 gef](#%E5%AE%89%E8%A3%85-gef)
    - [安装模块](#%E5%AE%89%E8%A3%85%E6%A8%A1%E5%9D%97)

<!-- /TOC -->
# 1. 从一个 HelloWorld 开始
## 1.1. 使用 gcc 编译
由于我们主要学习ARMv8的指令，所以直接使用 C 源文件生成一个 ARMv8的汇编源文件。

C 代码如下：
```c
// hello.c
#include<stdio.h>
int main(int argc, char const *argv[]){
    printf("Hello World!\r\n");
    return 0;
}
```

Makefile 内容如下：
```makefile
#文件名称
MODALE_NAME=hello

#ndk根目录
NDK_ROOT = $(HOME)/DevTool/AndroidSdk/ndk-bundle
#编译器根目录
TOOLCHAINS_ROOT_ARM=$(NDK_ROOT)/toolchains/arm-linux-androideabi-4.9/prebuilt/linux-x86_64
TOOLCHAINS_ROOT_AARCH64=$(NDK_ROOT)/toolchains/aarch64-linux-android-4.9/prebuilt/linux-x86_64
#编译器目录
TOOLCHAINS_PREFIX=$(TOOLCHAINS_ROOT_ARM)/bin/arm-linux-androideabi
TOOLCHAINS_PREFIX_AARCH64=$(TOOLCHAINS_ROOT_AARCH64)/bin/aarch64-linux-android

#头文件搜索路径
TOOLCHAINS_INCLUDE=$(TOOLCHAINS_ROOT_ARM)/lib/gcc/arm-linux-androideabi/4.9.x/include-fixed
TOOLCHAINS_INCLUDE_AARCH64=$(TOOLCHAINS_ROOT_AARCH64)/lib/gcc/aarch64-linux-android/4.9.x/include-fixed

#SDK根目录
PLATFROM_ROOT=$(NDK_ROOT)/platforms/android-21/arch-arm
PLATFROM_ROOT_AARCH64=$(NDK_ROOT)/platforms/android-21/arch-arm64

#sdk头文件搜索路径
PLATFROM_INCLUDE=$(PLATFROM_ROOT)/usr/include
PLATFROM_INCLUDE_AARCH64=$(PLATFROM_ROOT_AARCH64)/usr/include

#sdk库文件搜索路径
PLATFROM_LIB=$(PLATFROM_ROOT)/usr/lib
PLATFROM_LIB_AARCH64=$(PLATFROM_ROOT_AARCH64)/usr/lib

#删除
RM=del

#编译选项
FLAGS=-I$(TOOLCHAINS_INCLUDE) \
      -I$(PLATFROM_INCLUDE)   \
      -L$(PLATFROM_LIB) \
      -nostdlib \
      -lgcc \
      -Bdynamic \
      -lc \
      -fPIE

FLAGS_AARCH64=-I$(TOOLCHAINS_INCLUDE_AARCH64) \
      -I$(PLATFROM_INCLUDE_AARCH64)   \
      -L$(PLATFROM_LIB_AARCH64) \
      -nostdlib \
      -lgcc \
      -Bdynamic \
      -lc \
      -fPIE

#所有obj文件
OBJS=$(MODALE_NAME).o \
     $(PLATFROM_LIB)/crtbegin_dynamic.o \
     $(PLATFROM_LIB)/crtend_android.o 

OBJS_AARCH64=$(MODALE_NAME)64.o \
     $(PLATFROM_LIB_AARCH64)/crtbegin_dynamic.o \
     $(PLATFROM_LIB_AARCH64)/crtend_android.o 

#编译器链接
all:
	$(TOOLCHAINS_PREFIX)-gcc $(FLAGS) -E $(MODALE_NAME).c -o $(MODALE_NAME).i
	$(TOOLCHAINS_PREFIX)-gcc $(FLAGS)  -S $(MODALE_NAME).i -o $(MODALE_NAME).s
	$(TOOLCHAINS_PREFIX)-gcc $(FLAGS) -c $(MODALE_NAME).s -o $(MODALE_NAME).o
	$(TOOLCHAINS_PREFIX)-gcc $(FLAGS) $(OBJS) -fPIE -pie -o $(MODALE_NAME)

all64:
	$(TOOLCHAINS_PREFIX_AARCH64)-gcc $(FLAGS_AARCH64) -E $(MODALE_NAME).c -o $(MODALE_NAME)64.i
	$(TOOLCHAINS_PREFIX_AARCH64)-gcc $(FLAGS_AARCH64)  -S $(MODALE_NAME)64.i -o $(MODALE_NAME)64.s
	$(TOOLCHAINS_PREFIX_AARCH64)-gcc $(FLAGS_AARCH64) -c $(MODALE_NAME)64.s -o $(MODALE_NAME)64.o
	$(TOOLCHAINS_PREFIX_AARCH64)-gcc $(FLAGS_AARCH64) $(OBJS_AARCH64) -fPIE -pie -o $(MODALE_NAME)64
#删除所有.o文件
clean:
	$(RM) *.o
#安装程序到手机
install:
	adb push $(MODALE_NAME) /data/local/tmp
	adb shell chmod 755 /data/local/tmp/$(MODALE_NAME)
	adb shell /data/local/tmp/$(MODALE_NAME) 
debug:
	adb forward tcp:23946 tcp:23946
	adb shell /data/local/tmp/android_server

install64:
	adb push $(MODALE_NAME)64 /data/local/tmp
	adb shell chmod 755 /data/local/tmp/$(MODALE_NAME)64
	adb shell /data/local/tmp/$(MODALE_NAME)64 

debug64:
	adb forward tcp:23946 tcp:23946
	adb shell /data/local/tmp/android_server64
#运行程序
run:
	adb shell /data/local/tmp/$(MODALE_NAME)  

run64:
	adb shell /data/local/tmp/$(MODALE_NAME)64 
```
使用 `make all64` 命令编译后的结果：

![](ARMv8学习记录一/2020-02-28-23-37-24.png)

运行结果如下：

![](ARMv8学习记录一/2020-02-28-23-39-35.png)

生成的文件如下：

![](ARMv8学习记录一/2020-02-28-23-41-48.png)

`hello64.s` 对应的内容，后面所有的指令学习将一次文件为基础进行修改。
```
	.cpu generic+fp+simd
	.file	"hello.c"
	.section	.rodata
	.align	3
.LC0:
	.string	"Hello World!\r"
	.text
	.align	2
	.global	main
	.type	main, %function
main:
	stp	x29, x30, [sp, -32]!
	add	x29, sp, 0
	str	w0, [x29,28]
	str	x1, [x29,16]
	adrp	x0, .LC0
	add	x0, x0, :lo12:.LC0
	bl	puts
	mov	w0, 0
	ldp	x29, x30, [sp], 32
	ret
	.size	main, .-main
	.ident	"GCC: (GNU) 4.9.x 20150123 (prerelease)"
	.section	.note.GNU-stack,"",%progbits

```
至此，已经从一个 HelloWorld 开始了 ARMv8 的旅程。

## 使用 clang 编译
首先使用 `which` 命令查看 clang 的位置，由于我没有将 clang 添加到环境变量，最终结果是找不到的，那么需要将 clang 添加到临时环境变量中。

```bash
export PATH="$ANDROID_HOME/ndk/21.0.6113669/toolchains/llvm/prebuilt/linux-x86_64/bin/:$PATH" 
```

然后参考NDK 官方指南 [将 NDK 与其他构建系统配合使用](https://developer.android.com/ndk/guides/other_build_systems) 部分，可以知道调用 Clang 时需要使用 `-target` 参数传递适当的目标，例如以下命令为编译 minSdkVersion 为 21 的 64 位 Android ARM 平台的应用。

```bash
➜  clang -target aarch64-linux-android21 hello.c -o hello
➜  file hello
hello: ELF 64-bit LSB shared object, ARM aarch64, version 1 (SYSV), dynamically linked, interpreter /system/bin/linker64, not stripped
```

以上命令为一步编译成功，实际过程可以分为三步，我们以 ARM 平台为例进行讲解。

1. 预处理

```bash
➜  clang -target arm-linux-android21 -E hello.c -o arm_hello.i
```

2. 编译成汇编文件

```bash
➜  clang -target arm-linux-android21 -S arm_hello.i -o arm_hello.s
```

3. 汇编成 obj 文件

```bash
➜  clang -target arm-linux-android21 -c arm_hello.s -o arm_hello.o
```

4. 链接成可执行程序

```bash
➜  clang -target arm-linux-android21 arm_hello.o -o arm_hello
```

编写 makefile 执行 arm_hello

```makefile
all:
    clang -target arm-linux-android21 arm_hello.s -o arm_hello
	adb push arm_hello /data/local/tmp/arm_hello
	adb shell chmod +x /data/local/tmp/arm_hello
	adb shell /data/local/tmp/arm_hello
```

## ndk-build 编译

### 方法1：

1. 创建一个空文件`AndroidManifest.xml`。

2. 创建一个`Application.mk`，内容如下所示：
```
APP_BUILD_SCRIPT := Android.mk
APP_PLATFORM := android-21
```
3. 创建一个`Android.mk`文件，内容如下所示：
```
LOCAL_PATH := $(call my-dir)
include $(CLEAR_VARS)
LOCAL_SRC_FILES := foo.c
LOCAL_MODULE := foo
include $(BUILD_EXECUTABLE)
```

4. 使用以下命令编译
```
ndk-build NDK_APPLICATION_MK=$(pwd)/Application.mk
```

### 方法2：

直接使用以下命令进行编译：
```
ndk-build NDK_PROJECT_PATH=$(pwd)
或
ndk-build NDK_PROJECT_PATH=$(pwd) APP_BUILD_SCRIPT=$(pwd)/Android.mk
```

### 方法3：
将所有的文件放入  jni 目录，进入jni目录直接使用 ndk-build 命令编译即可。

# 2. 调试环境搭建
IDA的调试环境就不说了，网上一大把，下面主要说明使用 gdb + gef 的调试环境搭建。

## 2.1. 安装 gef
进入 android gdb 的目录 `/ndk-bundle/prebuilt/linux-x86_64/bin`,启动命令行，执行下列命令：
```
wget -q -O .gdbinit-gef.py https://github.com/hugsy/gef/raw/master/gef.py
echo source .gdbinit-gef.py >> .gdbinit
```
然后执行 `./gdb` 命令，出现如下信息
```
GNU gdb (GDB) 7.11
Copyright (C) 2016 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.  Type "show copying"
and "show warranty" for details.
This GDB was configured as "x86_64-linux-gnu".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<http://www.gnu.org/software/gdb/bugs/>.
Find the GDB manual and other documentation resources online at:
<http://www.gnu.org/software/gdb/documentation/>.
For help, type "help".
Type "apropos word" to search for commands related to "word".
GEF for linux ready, type `gef' to start, `gef config' to configure
78 commands loaded for GDB 7.11 using Python engine 2.7
[*] 2 commands could not be loaded, run `gef missing` to know why.
gef> gef missing 
[*] Command `assemble` is missing, reason -> Missing `keystone-engine` package for Python2, install with: `pip2 install keystone-engine`.
[*] Command `set-permission` is missing, reason -> Missing `keystone-engine` package for Python2, install with: `pip2 install keystone-engine`.
```
根据提示可以发现缺少了 `keystone-engine` 模块，当然还有可能缺少其他模块。

**后面的内容可以省略，安装过程中会出现比较多的问题，模块不安装也不影响使用。**

--------

## 2.2. 安装模块
退出 `gdb` , 下载 `[ez_setup.py](https://bootstrap.pypa.io/ez_setup.py)` ，运行列命令，进 行 `easy_install` 工具的安装：
```
./python ez_setup.py
```
安装成功之后就可以使用 `easy_install` 进行安装 `package` 了，然后根据提示安装需要的模块，也可以利用 `easy_install` 先安装 `pip`, 然后在利用 `pip` 进行安装其他模块，这里我使用 `easy_install` 进行安装其他模块。
```
./easy_install keystone-engine
```
安装完 `keystone-engine` 后，发现还是会有同样的提示，此时就需要下载 `[keystone-engine](https://pypi.org/project/keystone-engine/)` 源码进行安装。将源码解压至当前 `gdb` 的目录 `keystone-engine`中，运行下列命令进行安装：
```
cd keystone-engine
../python setup.py install
```
然后再次运行 gdb, 结果如下图：

![](ARMv8学习记录一/2020-02-29-21-33-08.png)

![](ARMv8学习记录一/2020-02-29-21-41-36.png)

至此，我们的环境就配好了，这里可以配合 [Hyperpwn](https://bbs.pediy.com/thread-257344.htm) 这个工具使用。

![](ARMv8学习记录一/2020-02-29-21-42-00.png)

参考： 
```
https://medium.com/bugbountywriteup/pwndbg-gef-peda-one-for-all-and-all-for-one-714d71bf36b8
https://packmad.github.io/gdb-android/
https://o0xmuhe.github.io/2016/06/29/install-gef/
```
