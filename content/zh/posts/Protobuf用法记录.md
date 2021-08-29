---
title: "Protobuf用法记录"
date: 2021-05-04T16:41:42+08:00
draft: false
tags: ["protobuf", ]
categories: ["protobuf", ]
---

## 步骤

### 安装 Protocol 编译器

[下载链接](https://github.com/protocolbuffers/protobuf/releases)

文件名：protoc-$VERSION-$PLATFORM.zip

根据自己所在的开发环境下载。

或者

安装 `Google.Protobuf.Tools` NuGet 包

### Protocal 运行时安装

安装 `Google.Protobuf` NuGet 包

## Proto 文件

```protobuf
syntax = "proto3"; // 使用的语法

import "other.proto"; // 引用其他消息

package my.project; // 打包，命名空间

option csharp_namespace = "My.WebApis"; // 指定 C# 的命名空间

message Person {  // Person 可用于其他消息中作为字段属性 消息可嵌套定义
    int id = 1; // 需要确保标识符和 tag （也就是等号后面的数字唯一）
    repeated string phone_numbers = 2; // repeated 关键字表明此字段是个集合

    Gender gender = 11;
    Address address = 12;

    reserved 3, 4 to 10; // reserved 表示指定的 tag 不能够被使用
    reserved "foo"; // reserved 表示指定的标识符不能够被使用

    enum Gender {  // 枚举也可以定义在消息外 也可以使用 reserved
        option allow_alias = true; // 此句表明 可定义具有重复值的枚举
        NOT_SPECIFIED = 0;
        FEMALE = 1;
        MALE = 2;
        WOMAN = 1;
        MAN = 2;
    }

    message Address {  // 嵌套消息
        string street = 1;
    }
}
```