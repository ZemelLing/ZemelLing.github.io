---
title: "Unity中实现存档的方式"
date: 2021-04-04T13:23:06+08:00
draft: false
tags: ["unity", "数据保存"]
categories: ["unity",]
---

## PlayerPrefs

PlayerPrefs 是 Unity 中用于存储用户数据的类，且只能够存储一些基本类型数据。

### 存储路径

存储路径取决于所在操作系统。

* Windows:  存在注册表的 HKCU\Software\ExampleCompanyName\ExampleProductName Key 下
* Windows Store Apps: 存在 %userprofile%\AppData\Local\Packages\[ProductPackageId]\LocalState\playerprefs.dat 文件里
* macOS: ~/Library/Preferences/com.ExampleCompanyName.ExampleProductName.plist
* Linux: ~/.config/unity3d/ExampleCompanyName/ExampleProductName

### 静态方法

| 方法名     | 描述                                                                                                                                  |
|-----------|---------------------------------------------------------------------------------------------------------------------------------------|
| DeleteAll |	Removes all keys and values from the preferences. Use with caution.|
| DeleteKey |	Removes the given key from the PlayerPrefs. If the key does not exist, DeleteKey has no impact.|
| GetFloat  |	Returns the value corresponding to key in the preference file if it exists.|
| GetInt    |	Returns the value corresponding to key in the preference file if it exists.|
| GetString |	Returns the value corresponding to key in the preference file if it exists.|
| HasKey    |	Returns true if the given key exists in PlayerPrefs, otherwise returns false.|
| Save      |	Writes all modified preferences to disk.|
| SetFloat  |	Sets the float value of the preference identified by the given key. You can use PlayerPrefs.GetFloat to retrieve this value.|
| SetInt    |	Sets a single integer value for the preference identified by the given key. You can use PlayerPrefs.GetInt to retrieve this value.|
| SetString |   Sets a single string value for the preference identified by the given key. You can use PlayerPrefs.GetString to retrieve this value.|