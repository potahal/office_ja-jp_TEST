
# CapitalizeNamesOfDays 属性

如果日期名称的第一个字母自动大写，则该属性值为 True。Boolean 类型，可读写。

 _expression_. **CapitalizeNamesOfDays**

 _表达式_ 必需。该表达式返回"应用于"列表中的一个对象。


## 示例

以下示例使 Microsoft Graph 将一周中各天名称的第一个字母变为大写。


```
With myChart.Application.AutoCorrect 
 .CapitalizeNamesOfDays = True 
 .ReplaceText = True 
End With
```

