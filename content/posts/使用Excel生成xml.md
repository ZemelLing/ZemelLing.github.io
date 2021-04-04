---
title: "使用Excel生成xml"
date: 2021-04-04T22:45:07+08:00
draft: false
---

1. 制作 XML 模板

```xml
<?xml version="1.0" encoding="UTF-8"?>
<root>
	<item ID="">		
		<surname></surname>
		<man></man>
		<woman></woman>
	</item>
	<item ID="">		
		<surname></surname>
		<man></man>
		<woman></woman>
	</item>
</root>
```

2. 在 Excel 选项中启用开发工具

![excel启用开发工具](/images/excel启用开发工具-20210404.png)

3. Excel 映射 XML 模板

![excel映射xml模板](/images/excel映射xml模板-20210404.png)

然后映射元素，填充数据

4. 使用开发工具的 XML 导出，即可生成 XML 文件

![excel导出xml文件](/images/excel导出xml文件-20210404.png)