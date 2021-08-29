---
title: "WPF中的依赖属性"
date: 2021-05-22T10:05:35+08:00
draft: true
tags: ["wpf", "desktop"]
categories: ["wpf",]
---

## 使用

### 定义依赖属性

```c#
public class FrameworkElement : UIElement, ...
{
    public static readonly DependencyProperty MarginProperty;
}
```

定义依赖属性的类需要继承自 DependencyObject

### 注册依赖属性

```c#
static FrameworkElement()
{
    FrameworkPropertyMetadata metadata = new FrameworkPropertyMetadata(new Thickness(), FrameworkPropertyMetadataOptions.AffectsMeasure);

    MarginProperty = DependencyProperty.Register("Margin", typeof(Thickness), typeof(FrameworkElement), metadata, new ValidateValueCallback(FrameworkElement.IsMarginValid));
}
```

### 添加属性包装器

```c#
public Thickness Margin
{
    set {SetValue(MarginProperty, value);}
    get {return (Thickness)GetValue(MarginProperty);}
}
```

注意不要在依赖属性的封装器中设置额外的代码（值校验等）。

### 使用依赖属性的方式

依赖属性的关键行为：
* 更改通知
* 动态值识别

更改通知，WPF 通过数据绑定和触发器传递依赖属性值的变更。

动态值识别，检索属性值时，WPF 通过一系列检查获取最终值，从低到高排列如下：

1. 默认值（由 FrameworkPropertyMetadata 设置）
2. 继承的值（FrameworkPropertyMetadata.Inherits 标志设置了，所在层次中某个元素提供了值）
3. 来自主题样式
4. 来自项目样式
5. 本地值（使用代码或 XAML 设置的值）

确定属性值的步骤：

1. 确定基本值
2. 如果为表达式设置则对表达式求值，表达式包括数据绑定和资源两种
3. 如果属性是动画的目标，就应用动画
4. 运行 CoerceValueCallback 回调来修正属性值

### 共享依赖属性

```c#
TextBlock.FontFamilyProperty = TextElement.FontFamilyProperty.AddOwner(TextBlock);
```

### 附加的依赖属性（附加属性）

* 附加的依赖属性并不被应用到定义该属性的类上
* 附加依赖属性不必定义属性封装器

```c#
FrameworkPropertyMetadata metadata = new FrameworkPropertyMetadata(0, new PropertyChangedCallback(Grid.OnCellAttachedPropertyChanged));

Grid.RowProperty = DependencyProperty.RegisterAttached("Row", typeof(int), typeof(Grid), metadata, new ValidateValueCallback(Grid.IsIntValueNotNegative));
```

## 属性验证

* ValidateValueCallback：接受或拒绝新值
* CoerceValueCallback：将新值改为可接受值

验证过程：
1. CoerceValueCallback 有机会修改提供的值，或者返回  DependencyProperty.UnsetValue 拒绝修改。
2. ValidateValueCallback 校验提供的值（必须是静态方法且无权访问正在被验证的对象）
3. 上面都通过，则触发  PropertyChangedCallback 方法。

