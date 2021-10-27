---
title: "AspNetCore学习记录(1)"
date: 2021-03-26T22:19:59+08:00
draft: false
tags: ["aspnetcore",]
categories: ["aspnetcore",]
series:
description:
toc: true
authors:
  - zemelling
lastmod: 2021-08-29T13:09:56+08:00
featuredImage:
---

## 配置MVC

修改Startup文件成如下所示：
```c#
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Hosting;
using Microsoft.AspNetCore.Http;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;

namespace MockSchool
{
    public class Startup
    {
        public void ConfigureServices(IServiceCollection services)
        {
            // 设置 EnableEndpointRouting 为 false 是为了使用 MVC 默认路由
            //services.AddMvc(a => a.EnableEndpointRouting = false);
            // 使用 AddControllersWithViews 而不使用 AddMvc 是为了不引入 RazorPages 
            // 等未用到的服务
            services.AddControllersWithViews(a => a.EnableEndpointRouting = false);
        }

        public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
        {
            if (env.IsDevelopment())
            {
                app.UseDeveloperExceptionPage();
            }

            // 先处理静态文件
            app.UseStaticFiles();

            app.UseMvcWithDefaultRoute();

            app.Run(async (context) =>
            {
                await context.Response.WriteAsync("Hello World!");
            });
        }
    }
}

```