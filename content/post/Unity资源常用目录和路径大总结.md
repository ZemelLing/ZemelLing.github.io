---
title: "Unity资源常用目录和路径大总结"
date: 2021-11-27T19:42:30+08:00
draft: false
tags: ["unity",]
categories: ["游戏开发",]
---

转载自：[Unity资源常用目录和路径大总结](https://zhuanlan.zhihu.com/p/125109062)  
参考1：[unity不同平台下的路径和读写权限](https://hiramtan.wordpress.com/2017/05/26/unity%E4%B8%8D%E5%90%8C%E5%B9%B3%E5%8F%B0%E4%B8%8B%E7%9A%84%E8%B7%AF%E5%BE%84%E5%92%8C%E8%AF%BB%E5%86%99%E6%9D%83%E9%99%90/)  
参考2：[#你好Unity3D#手机上的路径（来自我的长微博）](http://www.xuanyusong.com/archives/2656)  
参考3：[Unity3D研究院之Android同步方法读取streamingAssets（八十八）](http://www.xuanyusong.com/archives/4033)

## 一、资源路径

|字段|说明|
|-|-|
|Application.dataPath|包含游戏在目标平台上的数据目录，Android平台不可读写，IOS只读，其他可读可写|
|Application.persistentDataPath|持久化数据文件夹目录，通常用于读写数据。在移动平台，升级应用并不会擦除此目录，但用户可以手动修改。除了 Bundle Identifier 被修改的情况，升级应用不会变更此路径。全平台可读可写|
|Application.streamingAssetsPath|unity编辑器Assets文件夹下创建的StreamingAssets文件夹(Assets\StreamingAssets)，并且打包时将被原封不动的合并到安装包中，且在运行时将被存放于此字段指向的路径。其中 WebGL 和安卓平台不支持访问此路径，这两个平台的此路径将返回 URL，需要使用 UnityWebRequest 进行访问。在大多数平台上此路径只读|
|Application.temporaryCachePath|临时数据的缓存目录|

### Unity Editor

|Unity|路径|可读|可写|备注|
|-|-|-|-|-|
|Application.dataPath|{path to project folder}/Assets|是|是|
|Application.persistentDataPath|%userprofile%/AppData/LocalLow/{companyname}/{productname}|是|是|
|Application.streamingAssetsPath|{Application.dataPath}/StreamingAssets|是|是|
|Application.temporaryCachePath|%userprofile%/AppData/Local/Temp/{companyname}/{productname}|是|是|

### Mac平台

|Unity|路径|可读|可写|备注|
|-|-|-|-|-|
|Application.dataPath|{path to player app bundle}/Contents|是|是|
|Application.persistentDataPath|指向用户库目录(user Library folder)，~/Library/Application Support/{companyname}/{productname}|是|是|
|Application.streamingAssetsPath|{Application.dataPath}/Resources/Data/StreamingAssets|是|?|未测试是否可写|
|Application.temporaryCachePath||||

### iOS平台

|Unity|路径|可读|可写|备注|
|-|-|-|-|-|
|Application.dataPath|{path to player app bundle}/{AppName.app}_Data|是|否|
|Application.persistentDataPath|/var/mobile/Containers/Data/Application/{guid}/Documents.|是|是|此处的GUID由Unity根据 Bundle Identifier 生成|
|Application.streamingAssetsPath|{Application.dataPath}/Raw|是|否|参考2|
|Application.temporaryCachePath||||

### Windows平台

|Unity|路径|可读|可写|备注|
|-|-|-|-|-|
|Application.dataPath|{path to executablename_Data folder}|是|是|
|Application.persistentDataPath|%userprofile%/AppData/LocalLow/{companyname}/{productname}|是|是|
|Application.streamingAssetsPath|{Application.dataPath}/StreamingAssets|是|是|
|Application.temporaryCachePath|%userprofile%/AppData/Local/Temp/{companyname}/{productname}|是|是|

### Linux平台

|Unity|路径|可读|可写|备注|
|-|-|-|-|-|
|Application.dataPath|{path to executablename_Data folder}|是|是|
|Application.persistentDataPath|$XDG_CONFIG_HOME/unity3d 或 $HOME/.config/unity3d|是|是|
|Application.streamingAssetsPath|{Application.dataPath}/StreamingAssets|是|是|
|Application.temporaryCachePath||||

### Android平台

|Unity|路径|可读|可写|备注|
|-|-|-|-|-|
|Application.dataPath|通常直接指向apk(/data/app/xxx.xxx.xxx.apk)还有可能指向OBB((Opaque Binary Blob)文件|否|否|详细说明看 参考1|
|Application.persistentDataPath|/storage/emulated/0/Android/data/{packagename}/files|是|是|
|Application.streamingAssetsPath|“jar:file://” + Application.dataPath + “!/assets/”|是|否|
|Application.temporaryCachePath|/storage/emulated/0/Android/data/{packagename}/caches|是|是|

## 二、Unity3D中的资源访问介绍

### Resources

是Unity3D系统指定文件夹，如果你新建的文件夹的名字叫Resources，那么里面的内容在打包时都会被打到发布包中。文件夹特点：

只读，即不能动态修改。所以想要动态更新的资源不要放在这里。
会将文件夹内的资源打包集成到.asset文件里面。
主线程加载。
资源读取使用Resources.Load()。

### StreamingAssets 

也是Unity3D系统指定文件夹 ，和Resources文件的区别就是Resources文件夹中的内容在打包时会被压缩和加密。而StreamingAsset文件夹中的内容则会原封不动的打入包中，因此StreamingAssets主要用来存放存放打包的AB资源，然后用户安装包之后把这些AB资源是放到手机内。文件夹特点：

只读，可以放一些压缩的AB资源。
只能用过WWW类来读取。

### PersistentDataPath

是可读写路径。在iOS上就是应用程序的沙盒，但是在Android可以是程序的沙盒，也可以是sdcard。并且在Android打包的时候，ProjectSetting页面有一个选项Write Access，可以设置它的路径是沙盒还是sdcard。文件夹特点：

内容可读写。
无内容限制。一般是从StreamingAsset中读取二进制文件或者从AssetBundle读取文件来写入PersistentDataPath中。
一般游戏活动通过WWW动态下载的图片也可以缓存到此目录，方便下次快速打开。
