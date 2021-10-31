---
title: "使用Serilog作为日志系统"
date: 2021-05-03T22:21:58+08:00
draft: false
tags: ["serilog", "log"]
categories: ["其他方面",]
series:
description:
toc: true
authors:
  - zemelling
lastmod: 2021-08-29T13:09:56+08:00
featuredImage:
---

## 使用

### 简单使用

```c#
using System;
using Serilog;

namespace LogDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            Log.Logger = new LoggerConfiguration().WriteTo.Console().CreateLogger();
            Log.Information("The global logger has been configured");
        }
    }
}
```

通过设置 Log 类的静态成员 Logger 后，就可以使用 Log 类的静态方法输出日志。


### 完整用法（包括文件输出）

1. 添加 Nuget 包

```
Serilog
Serilog.Sinks.Console
Serilog.Sinks.File
```

2. code

```c#
using System;
using Serilog;

namespace SerilogExample
{
    class Program
    {
        static void Main()
        {
            Log.Logger = new LoggerConfiguration()
                .MinimumLevel.Debug()
                .WriteTo.Console()
                .WriteTo.File("logs/myapp.txt", rollingInterval: RollingInterval.Day)
                .CreateLogger();

            Log.Information("Hello, world!");

            int a = 10, b = 0;
            try
            {
                Log.Debug("Dividing {A} by {B}", a, b);
                Console.WriteLine(a / b);
            }
            catch (Exception ex)
            {
                Log.Error(ex, "Something went wrong");
            }
            finally
            {
                Log.CloseAndFlush();
            }
        }
    }
}
```