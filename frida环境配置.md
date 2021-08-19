---
title: frida环境配置
date: 2020-01-19 16:10:09
tags: frida
category: frida
---

开发环境：ubuntu18.04

> 源码地址：https://github.com/frida/frida
> 官网：https://frida.re/
<!-- TOC -->

- [安装 Frida](#%E5%AE%89%E8%A3%85-frida)
    - [在安装frida之前最好创建一个 python 虚拟环境，这样可以避免与其他环境产生干扰](#%E5%9C%A8%E5%AE%89%E8%A3%85frida%E4%B9%8B%E5%89%8D%E6%9C%80%E5%A5%BD%E5%88%9B%E5%BB%BA%E4%B8%80%E4%B8%AA-python-%E8%99%9A%E6%8B%9F%E7%8E%AF%E5%A2%83%E8%BF%99%E6%A0%B7%E5%8F%AF%E4%BB%A5%E9%81%BF%E5%85%8D%E4%B8%8E%E5%85%B6%E4%BB%96%E7%8E%AF%E5%A2%83%E4%BA%A7%E7%94%9F%E5%B9%B2%E6%89%B0)
    - [进入虚拟环境，直接运行下来命令即可安装完成](#%E8%BF%9B%E5%85%A5%E8%99%9A%E6%8B%9F%E7%8E%AF%E5%A2%83%E7%9B%B4%E6%8E%A5%E8%BF%90%E8%A1%8C%E4%B8%8B%E6%9D%A5%E5%91%BD%E4%BB%A4%E5%8D%B3%E5%8F%AF%E5%AE%89%E8%A3%85%E5%AE%8C%E6%88%90)
    - [根据f rida 的版本去下载对应的 frida-server-12.8.7-android-arm64.xz](#%E6%A0%B9%E6%8D%AEf-rida-%E7%9A%84%E7%89%88%E6%9C%AC%E5%8E%BB%E4%B8%8B%E8%BD%BD%E5%AF%B9%E5%BA%94%E7%9A%84-frida-server-1287-android-arm64xz)
    - [将 frida-server push 进 data/local/tmp 目录，并给予其运行权限，使用 root 用户启动。](#%E5%B0%86-frida-server-push-%E8%BF%9B-datalocaltmp-%E7%9B%AE%E5%BD%95%E5%B9%B6%E7%BB%99%E4%BA%88%E5%85%B6%E8%BF%90%E8%A1%8C%E6%9D%83%E9%99%90%E4%BD%BF%E7%94%A8-root-%E7%94%A8%E6%88%B7%E5%90%AF%E5%8A%A8)
- [配置开发环境](#%E9%85%8D%E7%BD%AE%E5%BC%80%E5%8F%91%E7%8E%AF%E5%A2%83)
    - [从官方下载API类型定义](#%E4%BB%8E%E5%AE%98%E6%96%B9%E4%B8%8B%E8%BD%BDapi%E7%B1%BB%E5%9E%8B%E5%AE%9A%E4%B9%89)
    - [另一种方式：](#%E5%8F%A6%E4%B8%80%E7%A7%8D%E6%96%B9%E5%BC%8F)
    - [简单使用](#%E7%AE%80%E5%8D%95%E4%BD%BF%E7%94%A8)
- [Frida 代码片段](#frida-%E4%BB%A3%E7%A0%81%E7%89%87%E6%AE%B5)

<!-- /TOC -->

# 安装 Frida
## 1. 在安装frida之前最好创建一个 python 虚拟环境，这样可以避免与其他环境产生干扰
```
mkvirtualenv -p python3 frida_12.8.7
```

## 2. 进入虚拟环境，直接运行下来命令即可安装完成
```
pip install frida-tools # CLI tools
pip install frida       # Python bindings

```
安装完成后，运行 `frida --version` 查看 frida 的版本

![](frida环境配置/2020-01-19-16-20-36.png)

## 3. 根据f rida 的版本去[下载](https://github.com/frida/frida/releases)对应的 `frida-server-12.8.7-android-arm64.xz` 
## 4. 将 `frida-server` push 进 `data/local/tmp` 目录，并给予其运行权限，使用 root 用户启动。
```
# chmod +x frida-server
# ./frida-server
```
执行 `frida-ps -U` ,出现以下信息则表明安装成功。

![](frida环境配置/2020-01-19-16-31-30.png)

# 配置开发环境
## 从官方下载API类型定义
> `https://github.com/frida/frida-gum/blob/6e36eebe1aad51c37329242cf07ac169fc4a62c4/bindings/gumjs/types/frida-gum/frida-gum.d.ts`

其他方式下载
> https://www.npmjs.com/package/@types/frida-gum

在 IDE 中需要使用 FridaAPI 的文件开头加上
`///<reference path='./frida-gum.d.ts' />`
即可。

![](frida环境配置/2020-01-19-16-50-57.png)

## 另一种方式：
frida-gum.d.ts已经被大胡子删掉了，他现在不赞成使用这种方式。
目前最新正确的Frida环境搭建方法：
1. `git clone https://github.com/oleavr/frida-agent-example`
2. `npm install`
3. 使用 PyCharm 等 IDE 打开此工程，在 agent 下编写 typescript ，会有智能提示。
4. `npm run watch` 会监控代码修改自动编译生成 js 文件
5. python 脚本或 cli 加载 `_agent.js` 。


参考：https://bbs.pediy.com/thread-254086.htm

## 简单使用
参考官网文档简单使用一下 Frida .

例子[下载地址](https://github.com/ctfs/write-ups-2015/tree/master/seccon-quals-ctf-2015/binary/reverse-engineering-android-apk-1)

# Frida 代码片段

Hook StringBuilder and print data only from a specific class
```javascript
var sbActivate = false;

Java.perform(function() {
  const StringBuilder = Java.use('java.lang.StringBuilder');
  StringBuilder.toString.implementation = function() {

    var res = this.toString();
    if (sbActivate) {
        var tmp = "";
        if (res !== null) {
          tmp = res.toString().replace("/n", "");
          console.log(tmp);
        }
    }
    return res;
  };

});

Java.perform(function() {
  const someclass = Java.use('<the specific class you are interested in>');
  someclass.someMethod.implementation = function() {

    sbActivate = true;
    var res = this.someMethod();
    sbActivate = false;

    return res;
  };

});
```

byte[]这种 hook输出的时候该怎么写呢？
```javascript
var ByteString = Java.use("com.android.okhttp.okio.ByteString");
var j = Java.use("c.business.comm.j");
j.x.implementation = function() {
    var result = this.x();
    console.log("j.x:", ByteString.of(result).hex());
    return result;
};

j.a.overload('[B').implementation = function(bArr) {
    this.a(bArr);
    console.log("j.a:", ByteString.of(bArr).hex());
};
```

hook Androd 7以上的 dlopen 。
```javascript
var android_dlopen_ext = Module.findExportByName(null, "android_dlopen_ext");
console.log(android_dlopen_ext);
if (android_dlopen_ext != null) {
    Interceptor.attach(android_dlopen_ext, {
        onEnter: function (args) {
            var soName = args[0].readCString();
            console.log(soName);
            if (soName.indexOf("**.so") !== -1) {
                console.log("------load **----------")
                this.hook = true;
            }
        },
        onLeave: function (retval) {
            if (this.hook) {
                //TODO hookso
            }
        }
    });
}
```

frida如何注入dex？
```javascript
Java.openClassFile(dexPath).load();
```

在系统里装上这个这个npm包，可以在任意工程获得frida的代码提示、补全和API查看。
```
npm i -g @types/frida-gum
```