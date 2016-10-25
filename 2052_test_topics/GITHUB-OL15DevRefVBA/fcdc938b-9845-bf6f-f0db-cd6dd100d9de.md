
# FormRegion.DisplayName 属性 (Outlook)

返回一个 **字符串** ，表示该窗体区域的显示名称。只读的。


## 语法

 _表达式_. **DisplayName**

 _表达式_ 一个代表 **FormRegion** 对象的变量。


## 说明

显示名称是可选的窗体区域。如果在相应的窗体区域清单 XML 文件中定义的 < formRegionName > 标记的值，此值会映射到 **显示名称** 属性的值。为窗体区域 XML 架构的详细信息，请参阅[MSDN 库](http://msdn.microsoft.com/library)中的 Microsoft Outlook 2010 XML 架构引用。

 **显示名称** 属性的值将显示在运行时在单独的窗体区域中，功能区的 **显示**选项卡中或相邻窗体区域的标题中。 它用于默认的区域设置，并且可以通过在相应的窗体区域清单 XML 文件中的 < stringOverride > 标记。该字符串不区分大小写，并其最大长度为 256 个字符。


## 另请参阅


#### 概念


[FormRegion 对象](3a0b83eb-4076-9cb3-86a9-68f9e44df89f.md)
#### 其他资源


[FormRegion 对象成员](eb4ff750-2911-8f8d-2ef0-c3f5e7adf4e0.md)