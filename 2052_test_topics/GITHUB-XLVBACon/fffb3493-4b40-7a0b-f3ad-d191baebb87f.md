
# ColumnWidth 属性

返回或设置指定区域中所有列的列宽。Variant 类型，可读写。

 _expression_. **ColumnWidth**

 _表达式_ 必需。该表达式返回"应用于"列表中的一个对象。


## 说明

一个列宽单位等于"常规"样式中一个字符的宽度。对于比例字体，则使用字符 0（零）的宽度。

如果区域中所有列的列宽都相等， **ColumnWidth** 属性返回该宽度值。如果区域中的列宽不等，本属性返回 **Null** 。


## 示例

以下示例使数据表上 A 列的列宽加倍。


```
With myChart.Application.DataSheet.Columns("A") 
 .ColumnWidth = .ColumnWidth * 2 
End With
```

