---
title: "CSharp中的XML序列化"
date: 2021-04-24T22:04:09+08:00
draft: false
tags: ["csharp", "xml"]
categories: ["编程语言",]
series:
description:
toc: true
authors:
  - zemelling
lastmod: 2021-08-29T13:09:56+08:00
featuredImage:
---

## 可 XML 序列化的内容

* 公有类的公有可读写属性和字段
* 接口 ICollection 或 IEnumerable 的实现类
* XmlElement 对象
* XmlNode 对象
* DataSet 对象

## XML 使用的 Attribute

* 通常 Xml 元素的名称由类名或成员名称，但是也可以通过 Attribute 去控制。

### XmlArrayAttribute 和 XmlArrayItemAttribute

XmlArrayAttribute 和 XmlArrayItemAttribute 用于控制数组的序列化。

XmlArrayAttribute 可以指定数组序列化时的 Xml元素名称。

```c#
public class Group {
    [XmlArray("TeamMembers")]
    public Employee[] Employees;
}
```

```xml
<Group>
<TeamMembers> <!-- 如果没有指定 XmlArrayAttribute 此处的元素应该名叫 Employees -->
    <Employee>
        <Name>Haley</Name> 
    </Employee>
</TeamMembers> 
</Group>
```

XmlArrayItemAttribute 影响数组内 item 的序列化。

```c#
public class Group {
    [XmlArrayItem("MemberName")]
    public Employee[] Employees;
}
```

```xml
<Group>
<Employees>
    <MemberName>Haley</MemberName> <!-- 如果没有指定 XmlArrayItemAttribute 此处的元素应该名叫 Employee -->
</Employees>
</Group>
```

当数组中包含数组元素的类型的派生类实列时，若未使用 XmlArrayItemAttribute 指定数组所能包含的类型时，将导致报错。

```c#
public class Group {
    [XmlArrayItem(Type = typeof(Employee)),
    XmlArrayItem(Type = typeof(Manager))] // 若未指定子类的特性，一旦 Employees 数组中包含 Manager 类型的实例时，将报错。
    public Employee[] Employees;
}
public class Employee {
    public string Name;
}
public class Manager:Employee {
    public int Level;
}
```

```xml
<Group>
<Employees>
    <Employee>
        <Name>Haley</Name>
    </Employee>
    <Employee xsi:type = "Manager">
        <Name>Ann</Name>
        <Level>3</Level>
    </Employee>
</Employees>
</Group>
```

### XmlElementAttribute

将 XmlElementAttribute 应用于数组时将导致，序列化的数组扁平化，而不是嵌套结构

```c#
public class Group {
    [XmlElement]
    public Employee[] Employees;
}
```
```xml
<Group>
<Employees>
    <Name>Haley</Name>
</Employees>
<Employees>
    <Name>Noriko</Name>
</Employees>
<Employees>
    <Name>Marco</Name>
</Employees>
</Group>
```

XmlElementAttribute 还可用于指定类成员序列化的后的 Xml 元素的名称

### XmlRootAttribute 和 XmlTypeAttribute

XmlRootAttribute 可用于指定类序列化后的 Xml 根元素的名称和命名空间。
XmlTypeAttribute 用于控制生成 Xml 的 Schema。

## 序列化对象

```c#
MySerializableClass myObject = new MySerializableClass();  
// Insert code to set properties and fields of the object.  
XmlSerializer mySerializer = new XmlSerializer(typeof(MySerializableClass));  
// To write to a file, create a StreamWriter object.  
StreamWriter myWriter = new StreamWriter("myFileName.xml");  
mySerializer.Serialize(myWriter, myObject);  
myWriter.Close();
```

## 反序列化对象

```c#
// Construct an instance of the XmlSerializer with the type
// of object that is being deserialized.
var mySerializer = new XmlSerializer(typeof(MySerializableClass));
// To read the file, create a FileStream.
var myFileStream = new FileStream("myFileName.xml", FileMode.Open);
// Call the Deserialize method and cast to the object type.
var myObject = (MySerializableClass) mySerializer.Deserialize(myFileStream)
```

## XmlDocument

### 加载 Xml 文件

```c#
XmlDocument doc = new XmlDocument();
doc.PreserveWhitespace = true;
try { doc.Load("booksData.xml"); }
catch (System.IO.FileNotFoundException)
{
    doc.LoadXml("<?xml version=\"1.0\"?> \n" +
    "<books xmlns=\"http://www.contoso.com/books\"> \n" +
    "  <book genre=\"novel\" ISBN=\"1-861001-57-8\" publicationdate=\"1823-01-28\"> \n" +
    "    <title>Pride And Prejudice</title> \n" +
    "    <price>24.95</price> \n" +
    "  </book> \n" +
    "  <book genre=\"novel\" ISBN=\"1-861002-30-1\" publicationdate=\"1985-01-01\"> \n" +
    "    <title>The Handmaid's Tale</title> \n" +
    "    <price>29.95</price> \n" +
    "  </book> \n" +
    "</books>");
}
```

### 获取子节点

```c#
using System;
using System.IO;
using System.Xml;

public class Sample {

  public static void Main() {

    XmlDocument doc = new XmlDocument();
    doc.LoadXml("<book ISBN='1-861001-57-5'>" +
                "<title>Pride And Prejudice</title>" +
                "<price>19.95</price>" +
                "</book>");

    XmlNode root = doc.FirstChild;

    //Display the contents of the child nodes.
    if (root.HasChildNodes)
    {
      for (int i=0; i<root.ChildNodes.Count; i++)
      {
        Console.WriteLine(root.ChildNodes[i].InnerText);
      }
    }
  }
}
```

## 例子

### 序列化 DataSet

```c#
private void SerializeDataSet(string filename){
    XmlSerializer ser = new XmlSerializer(typeof(DataSet));

    // Creates a DataSet; adds a table, column, and ten rows.
    DataSet ds = new DataSet("myDataSet");
    DataTable t = new DataTable("table1");
    DataColumn c = new DataColumn("thing");
    t.Columns.Add(c);
    ds.Tables.Add(t);
    DataRow r;
    for(int i = 0; i<10;i++){
        r = t.NewRow();
        r[0] = "Thing " + i;
        t.Rows.Add(r);
    }
    TextWriter writer = new StreamWriter(filename);
    ser.Serialize(writer, ds);
    writer.Close();
}
```

### 序列化 XmlElement 和 XmlNode

```c#
private void SerializeElement(string filename){
    XmlSerializer ser = new XmlSerializer(typeof(XmlElement));
    XmlElement myElement=
    new XmlDocument().CreateElement("MyElement", "ns");
    myElement.InnerText = "Hello World";
    TextWriter writer = new StreamWriter(filename);
    ser.Serialize(writer, myElement);
    writer.Close();
}

private void SerializeNode(string filename){
    XmlSerializer ser = new XmlSerializer(typeof(XmlNode));
    XmlNode myNode= new XmlDocument().
    CreateNode(XmlNodeType.Element, "MyNode", "ns");
    myNode.InnerText = "Hello Node";
    TextWriter writer = new StreamWriter(filename);
    ser.Serialize(writer, myNode);
    writer.Close();
}
```

### 序列化包含复杂类型的属性或字段 

序列化复杂属性或字段时，会将复杂属性或字段的类型嵌套序列化到主类型中，例如：

```c#
public class PurchaseOrder
{
    public Address MyAddress;
}
public class Address
{
    public string FirstName;
}
```

```xml
<PurchaseOrder>
    <MyAddress>
        <FirstName>George</FirstName>
    </MyAddress>
</PurchaseOrder>
```

### 序列化数组对象

```c#
public class PurchaseOrder
{
    public Item [] ItemsOrders;
}

public class Item
{
    public string ItemID;
    public decimal ItemPrice;
}
```

```xml
<PurchaseOrder xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <ItemsOrders>
        <Item>
            <ItemID>aaa111</ItemID>
            <ItemPrice>34.22</ItemPrice>
        </Item>
        <Item>
            <ItemID>bbb222</ItemID>
            <ItemPrice>2.89</ItemPrice>
        </Item>
    </ItemsOrders>
</PurchaseOrder>
```

## 备注

* XML 序列化不能转换方法、索引器、私有字段或只读属性（只读集合除外）。 
* XmlSerializer 类在序列化时会在系统的 TEMP 目录创建 .cs 文件并将其编译成 .dll 文件，这就容易导致安全问题，恶意程序或用户通过替换文件以达到不法目的。
* 类必须要有无参构造函数