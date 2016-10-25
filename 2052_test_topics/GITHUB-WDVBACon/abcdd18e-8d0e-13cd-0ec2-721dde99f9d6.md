
# 分配 Range

有几种方法将现有的 **[Range](15a7a1c4-5f3f-5b6e-60e9-29688de3f274.md)** 对象分配给变量。本主题介绍两种不同方法的结果。在以下示例中， `Range1`和 `Range2`变量引用的 **范围** 对象。例如，以下说明对 `Range1`和 `Range2`变量指定活动文档中的第一和第二个单词。


```
Set Range1 = ActiveDocument.Words(1) 
Set Range2 = ActiveDocument.Words(2)
```


## 设置范围对象变量等于另一个的范围对象变量

下面的指令将指定范围变量名为 `Range2`来表示与 `Range1`相同的位置。


```
Set Range2 = Range1
```

现在有两个变量代表同一范围。执行操作的开始或终结点或文本的 `Range2`，它也会影响 `Range1` ，反之亦然。

请注意，下面的指令是与 `Range2.Text = Range1.Text`相同。此指令将 `Range1`，即 **[文本](495fe06e-ba87-0d96-9f6e-3e62fd71d4a5.md)** 属性的默认属性分配给 `Range2`的默认属性。它不会更改实际引用的对象。




```
Range2 = Range1
```

范围 ( `Range2` ，和 `Range1`) 具有相同的内容，但它们可能指向文档或甚至不同的文档中的不同位置。


## 使用重复属性

下面的指令创建一个新复制的 **Range** 对象， `Range2`，具有相同的开始和终结点以及与 `Range1`的文本。


```
Set Range2 = Range1.Duplicate
```

如果您更改的起点或终点的 `Range1`，它不会影响 `Range2,` ，反之亦然。因为这两个区域指向文档中的同一位置，更改一个区域中的文本会影响其他区域中的文本。

