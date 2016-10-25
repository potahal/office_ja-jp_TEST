
# Cells 属性

当本属性应用于 Range 对象时，返回 Range 对象，该对象代表指定区域内的单元格。当其应用于 DataSheet 对象时，也返回 Range 对象，该对象代表数据表上的所有单元格（不仅仅是当前正在使用的单元格）。Range 对象，只读。

 _expression_. **Cells**

 _表达式_ 所必需。一个表达式，返回应用于列表中的对象。


## 示例

本示例清除数据表上单元格 A1 中的公式。请注意，在数据表上，列 A 为第二列，而行 1 为第二行。


```
myChart.Application.DataSheet.Cells(2,2).ClearContents
```

本示例在数据表上的单元格区域 A1:I3 中循环。如果该区域中的任一单元格的值小于 0.001，本示例就将该值替换为 0（零）。




```
Set mySheet = myChart.Application.DataSheet 
For rwIndex = 2 to 4 
 For colIndex = 2 to 10 
 If mySheet.Cells(rwIndex, colIndex) < .001 Then 
 mySheet.Cells(rwIndex, colIndex).Value = 0 
 End If 
 Next colIndex 
Next rwIndex
```

