---
title: "CSharp中发送HTTP请求的方法"
date: 2021-07-02T12:49:27+08:00
draft: false
tags: ["csharp", "http"]
categories: ["csharp",]
series:
description:
toc: true
authors:
  - zemelling
lastmod: 2021-08-29T13:09:56+08:00
featuredImage:
---

# 发送 Get 请求

```CSharp
// HttpClient is intended to be instantiated once per application, rather than per-use. See Remarks.
static readonly HttpClient client = new HttpClient();

static async Task Main()
{
  // Call asynchronous network methods in a try/catch block to handle exceptions.
  try	
  {
     HttpResponseMessage response = await client.GetAsync("http://www.contoso.com/");
     response.EnsureSuccessStatusCode();
     string responseBody = await response.Content.ReadAsStringAsync();
     // Above three lines can be replaced with new helper method below
     // string responseBody = await client.GetStringAsync(uri);

     Console.WriteLine(responseBody);
  }
  catch(HttpRequestException e)
  {
     Console.WriteLine("\nException Caught!");	
     Console.WriteLine("Message :{0} ",e.Message);
  }
}
```

# 发送 Post 请求
```CSharp
using (var httpClient = new HttpClient { BaseAddress = new Uri(cqcPlatformUrl) })
{
    httpClient.DefaultRequestHeaders.Add("Test", "TestValue");
    httpClient.Timeout = TimeSpan.FromMinutes(30);
    HttpContent content = new StringContent("body", Encoding.UTF8, "application/json");
    var httpResponseMessage = await httpClient.PostAsync($"http://www.contoso.com/", content);
    if (httpResponseMessage.StatusCode == HttpStatusCode.OK)
    {
        var response = await httpResponseMessage.Content.ReadAsStringAsync();
    }
}
```

# HTTP请求超时的处理办法

