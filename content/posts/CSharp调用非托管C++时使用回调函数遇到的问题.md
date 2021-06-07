---
title: "CSharp调用非托管C++时使用回调函数遇到的问题"
date: 2021-06-07T16:36:57+08:00
draft: false
tags: ["c++", "CSharp"]
categories: ["CSharp", "c++"]
---

1. 如果该回调函数会被定时调用时，C#代码需要将回调函数对应的委托实例化成一个静态变量后使用，以避免 GC 将委托释放导致的异常问题。

参考：https://stackoverflow.com/questions/61985465/passing-c-sharp-callback-method-with-parameter-to-c-dll-leads-to-system-execut

```

I found the answer to my question, thanks to this post:

In order to keep the unmanaged function pointer alive (guarding against GC), you need to hold an instance of the delegate in a variable

So the modified code is ONLY in C#

// -------------- PREVIOUS CODE
async Task loadDllMethods() {
    // load the delegates => great, it works!
    SetCallbackFunction_Dll = GetDelegate<_SetCallbackFunction_>("SetCallbackFunction");
    // set callback in DLL, calls the delegate once successfully...
    SetCallbackFunction(cSharpCallback);
    await doTask();
}

// -------------- WORKING CODE!!!!
// add static reference....
static CallbackFunction _callbackInstance = new CallbackFunction(cSharpCallback); // <==== Added reference to prevent GC!!! 
async Task loadDllMethods() {
    // load the delegates => great, it works!
    SetCallbackFunction_Dll = GetDelegate<_SetCallbackFunction_>("SetCallbackFunction");
    // create callback!!!
    SetCallbackFunction(_callbackInstance); // <====== pass the instance here, not the method itself!!!
    await doTask();
}
NOTE: I also changed StringBuilder to string !
```