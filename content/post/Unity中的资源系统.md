---
title: "Unity中的资源系统"
date: 2021-04-15T12:15:15+08:00
draft: false
tags: ["unity", "assetbundle"]
categories: ["游戏开发",]
series:
description:
toc: true
authors:
  - zemelling
lastmod: 2021-08-29T13:09:56+08:00
featuredImage:
---

## 资源

* Unity通过将资源的唯一 ID 写入与资源同名的 .meta 文件来确保对资源的唯一引用。
* 新增的资源 Unity 都会创建与之对应的 .meta 文件
* Unity 读取并处理 Assets 文件夹中的任何文件，并将其转换成可直接用于游戏内容的数据格式，存储在 Libraray 文件夹中，而原文件仍然保留在 Assets 文件夹中
* .meta 文件不止包含资源 ID 同时也包含资源导入设置的数据。
* 备份项目或将项目添加到版本控制库时，应包括 Unity 主项目文件夹，其中包含 Assets 和 ProjectSettings 文件夹。Unity 依靠这些文件夹中的信息来重新导入您的资源并重新构建您的游戏或应用程序。不得备份 Library 和 Temp 文件夹，也不要将其置于版本控制下。

## 源资源和 Artifact

Unity 在 Library 文件夹中保留两个数据库文件，它们统称为资源数据库。这两个数据库可以跟踪有关源资源文件和 Artifact（这是有关导入结果的信息）的信息。

### 源资源数据库

源资源数据库包含有关源资源文件的元数据信息，Unity 将这些信息用来确定文件是否被修改，从而决定是否应该重新导入文件。这些信息中包括诸如上次修改日期、文件内容哈希、GUID 和其他元数据信息之类的信息。

### Artifact 数据库

Artifact 是导入过程的结果。Artifact 数据库包含有关每个源资源的导入结果的信息。每个 Artifact 都包含导入依赖项信息、Artifact 元数据信息和 Artifact 文件列表。

注意：数据库文件位于项目的 Library 文件夹中，因此应从版本控制系统中将这些文件排除。可以在以下位置找到它们：

源资源数据库：Library\SourceAssetDB
Artifact 数据库：Library\ArtifactDB

## AssetBundle

AssetBundle 是一个存档文件，包含可在运行时由 Unity 加载的特定于平台的非代码资源（比如模型、纹理、预制件、音频剪辑甚至整个场景）。AssetBundle 可以表示彼此之间的依赖关系；例如，一个 AssetBundle 中的材质可以引用另一个 AssetBundle 中的纹理。为了提高通过网络传输的效率，可以根据用例要求（LZMA 和 LZ4）选用内置算法选择来压缩 AssetBundle。AssetBundle 可用于可下载内容（DLC），减小初始安装大小，加载针对最终用户平台优化的资源，以及减轻运行时内存压力。

* AssetBundle 可以包含类的实例序列化后的数据，例如 ScriptableObject。AssetBundle 在加载时可查找类然后使用序列化的数据去赋值（反序列化）。

### AssetBundle 工作流程

#### 为 AssetBundle 分配资源

1. 从 Project 视图中选择要为捆绑包分配的资源。 
2. 在 Inspector 中检查对象。 
3. 在 Inspector 底部，有一个用于分配 AssetBundle 和变体的部分。可使用左侧下拉选单分配 AssetBundle，而使用右侧下拉选单分配变量（变体）。
4. 单击左侧下拉选单的 None 以显示当前注册的 AssetBundle 名称。 
5. 单击 New 以创建新的 AssetBundle 
6. 输入所需的 AssetBundle 名称。注意：AssetBundle 名称支持某种类型的文件夹结构，具体取决于您输入的内容。要添加子文件夹，请用 / 分隔文件夹名称。例如，使用 AssetBundle 名称 environment/forest 在 environment 子文件夹下创建名为 forest 的捆绑包 
7. 一旦选择或创建了 AssetBundle 名称，便可以重复此过程在右侧下拉选单中分配或创建变体名称（如果需要）。构建 AssetBundle 不需要变体名称

#### 构建 AssetBundle

在 Assets 文件夹中创建一个名为 Editor 的文件夹，并将包含以下内容的脚本放在该文件夹中：

```c#
using UnityEditor;
using System.IO;

public class CreateAssetBundles
{
    [MenuItem("Assets/Build AssetBundles")]
    static void BuildAllAssetBundles()
    {
        string assetBundleDirectory = "Assets/AssetBundles";
        if(!Directory.Exists(assetBundleDirectory))
        {
            Directory.CreateDirectory(assetBundleDirectory);
        }
        BuildPipeline.BuildAssetBundles(assetBundleDirectory, 
                                        BuildAssetBundleOptions.None, 
                                        BuildTarget.StandaloneWindows);
    }
}
```

#### 加载 AssetBundle 和资源

如果您想从本地存储中加载，请使用 AssetBundle.LoadFromFile API，如下所示：

```c#
public class LoadFromFileExample : MonoBehaviour {
    void Start() {
        var myLoadedAssetBundle 
            = AssetBundle.LoadFromFile(Path.Combine(Application.streamingAssetsPath, "myassetBundle"));
        if (myLoadedAssetBundle == null) {
            Debug.Log("Failed to load AssetBundle!");
            return;
        }
        var prefab = myLoadedAssetBundle.LoadAsset<GameObject>("MyObject");
        Instantiate(prefab);
    }
}
```

从远处服务器加载:

```c#
IEnumerator InstantiateObject()
{
    string url = "file:///" + Application.dataPath + "/AssetBundles/" + assetBundleName;        
    var request 
        = UnityEngine.Networking.UnityWebRequestAssetBundle.GetAssetBundle(url, 0);
    yield return request.Send();
    AssetBundle bundle = UnityEngine.Networking.DownloadHandlerAssetBundle.GetContent(request);
    GameObject cube = bundle.LoadAsset<GameObject>("Cube");
    GameObject sprite = bundle.LoadAsset<GameObject>("Sprite");
    Instantiate(cube);
    Instantiate(sprite);
}
```

### 加载 AssetBundle 的 API

#### AssetBundle.LoadFromMemoryAsync

此函数采用包含 AssetBundle 数据的字节数组。也可以根据需要传递 CRC 值。如果捆绑包采用的是 LZMA 压缩方式，将在加载时解压缩 AssetBundle。LZ4 压缩包则会以压缩状态加载。

以下是如何使用此方法的一个示例：

```c#
using UnityEngine;
using System.Collections;
using System.IO;

public class Example : MonoBehaviour
{
    IEnumerator LoadFromMemoryAsync(string path)
    {
        AssetBundleCreateRequest createRequest = AssetBundle.LoadFromMemoryAsync(File.ReadAllBytes(path));
        yield return createRequest;
        AssetBundle bundle = createRequest.assetBundle;
        var prefab = bundle.LoadAsset<GameObject>("MyObject");
        Instantiate(prefab);
    }
}
```

#### AssetBundle.LoadFromFile

从本地存储中加载未压缩的捆绑包时，此 API 非常高效。如果捆绑包未压缩或采用了数据块 (LZ4) 压缩方式，LoadFromFile 将直接从磁盘加载捆绑包。使用此方法加载完全压缩的 (LZMA) 捆绑包将首先解压缩捆绑包，然后再将其加载到内存中。

如何使用 LoadFromFile 的一个示例：

```c#
using System.IO;
using UnityEngine;

public class LoadFromFileExample : MonoBehaviour
{
    void Start()
    {
        var myLoadedAssetBundle = AssetBundle.LoadFromFile(Path.Combine(Application.streamingAssetsPath, "myassetBundle"));

        if (myLoadedAssetBundle == null)
        {
            Debug.Log("Failed to load AssetBundle!");
            return;
        }
        var prefab = myLoadedAssetBundle.LoadAsset<GameObject>("MyObject");
        Instantiate(prefab);
    }
}
```

#### UnityWebRequestAssetBundle

使用此 API 时，先要使用 UnityWebRequestAssetBundle.GetAssetBundle 创建 Web 请求。然后将请求传递给 DownloadHandlerAssetBundle.GetContent(UnityWebRequestAssetBundle) 以获取 AssetBundle 对象。

下载捆绑包后，还可以在 DownloadHandlerAssetBundle 类上使用 assetBundle 属性，从而以 AssetBundle.LoadFromFile 的效率加载 AssetBundle。

以下示例说明了如何加载包含两个游戏对象的 AssetBundle 并实例化这些游戏对象。要开始这个过程，我们只需要调用 StartCoroutine(InstantiateObject());

```c#
IEnumerator InstantiateObject()
{
    string uri = "file:///" + Application.dataPath + "/AssetBundles/" + assetBundleName; 
    UnityEngine.Networking.UnityWebRequestAssetBundle request 
        = UnityEngine.Networking.UnityWebRequestAssetBundle.GetAssetBundle(uri, 0);
    yield return request.SendWebRequest();
    AssetBundle bundle = DownloadHandlerAssetBundle.GetContent(request);
    GameObject cube = bundle.LoadAsset<GameObject>("Cube");
    GameObject sprite = bundle.LoadAsset<GameObject>("Sprite");
    Instantiate(cube);
    Instantiate(sprite);
}
```

##### 从 AssetBundle 处加载资源

LoadAsset、LoadAllAssets、LoadAssetAsync 和 LoadAllAssetsAsync 用于加载资源。

##### 从 AssetBundle 处加载清单

清单类型为 AssetBundleManifest，可使用加载资源的接口加载清单。清单用于加载 AssetBundle 依赖的其他资源。

首先要加载AssetBundle所在根目录同名的AssetBundle，例如：打包时将所有AB放到 AssetBundles 文件夹下，此时就要先加载名为 AssetBundles 的 AB。

然后，通过以下语句加载清单, LoadAsset 的参数必须是 AssetBundleManifest

```c#
var manifest = ab.LoadAsset<AssetBundleManifest>("AssetBundleManifest");
```

最后就可以通过 manifest 获取指定 AB 包的依赖

##### AssetBundle 的卸载

```c#
AssetBundle.Unload(true);
```

* true 表示现在AB包所有资源，推荐使用true。
* false 表示只卸载未使用的资源，但这容易导致内存泄漏。

### 管理已加载的 AssetBundle

建议使用 Addressable Asset system 管理 AssetBundle

### AssetBundle 的原理

AssetBundle 有 head 和 body ，head 存放 AB 的描述信息，body 存放打包的资源，根据不同的打包格式，其解压行为不同。

### AssetBundle 分组策略

1. 经常更新的资源放一个包，与不经常更新的包分离。
2. 把需要同时更新的资源放同一包。
3. 共享资源放一包
4. 同一资源的不同版本，通过后缀区分