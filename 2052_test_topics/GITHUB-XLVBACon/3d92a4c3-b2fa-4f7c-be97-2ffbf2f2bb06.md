
# 列中的空白单元格到向下填充值

下面的示例查找列，并如果没有空白单元格，则设置为等于其上方的单元格的值为空单元格的值。这继续沿整列向下填充值，每个空单元格中。

 **示例提供的代码:** Tom Urtis，[Atlas 编程管理](http://www.atlaspm.com/)



```
Sub FillCellsFromAbove()
    ' Turn off screen updating to improve performance
    Application.ScreenUpdating = False
    On Error Resume Next
    ' Look in column A
    With Columns(1)
        ' For blank cells, set them to equal the cell above
        .SpecialCells(xlCellTypeBlanks).Formula = "=R[-1]C"
        'Convert the formula to a value
        .Value = .Value
    End With
    Err.Clear
    Application.ScreenUpdating = True
End Sub
```


## 有关参与者
<a name="AboutContributor"> </a>

MVP Tom Urtis 是 Atlas Programming Management（提供 Microsoft Office 和 Excel 业务解决方案全面服务的公司，位于硅谷）的创始人。Tom 在业务管理和开发 Microsoft Office 应用程序方面拥有超过 25 年的经验，并协助编写了"Holy Macro! It's 2,500 Excel VBA Examples" 。

