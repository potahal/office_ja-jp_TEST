
# 选择和激活单元格

在 Microsoft Excel 中您通常要选择一个单元格或者单元格，然后执行操作，如设置单元格格式或在其中输入值。在 Visual Basic 中则通常没有必要对其进行修改之前选择的单元格。

例如，要使用 Visual Basic 的单元格 D6 中输入公式，您不必选择 D6 的范围。仅返回该单元格的 **Range** 对象，然后将 **Formula** 属性设置为所需的公式，如下面的示例中所示。



```VB.net
Sub EnterFormula() 
    Worksheets("Sheet1").Range("D6").Formula = "=SUM(D2:D5)" 
End Sub
```

有关详细信息和使用其他方法来控制，而选择这些单元格的示例，请参见[如何: 引用单元格和区域](a16caa8d-21c9-ff33-347b-ce671248a92d.md)。

## 使用 Select 方法和 Selection 属性

 **Select** 方法激活工作表和工作表上的对象；而 **Selection** 属性返回代表活动工作簿中活动工作表上的当前选定区域的对象。在成功使用 **Selection** 属性之前，必须先激活工作簿，并激活或选定工作表，然后用 **Select** 方法选定单元格区域（或其他对象）。

宏录制器通常会创建一个宏使用 **Select** 方法，并 **选择** 属性。下面的 **Sub** 过程使用宏录制器，创建和显示 **选择** ， **选择** 如何协同工作。




```VB.net
Sub Macro1() 
    Sheets("Sheet1").Select 
    Range("A1").Select 
    ActiveCell.FormulaR1C1 = "Name" 
    Range("B1").Select 
    ActiveCell.FormulaR1C1 = "Address" 
    Range("A1:B1").Select 
    Selection.Font.Bold = True 
End Sub
```

下面的示例执行相同的任务，但不激活或选择工作表或单元格。




```VB.net
Sub Labels() 
    With Worksheets("Sheet1") 
        .Range("A1") = "Name" 
        .Range("B1") = "Address" 
        .Range("A1:B1").Font.Bold = True 
    End With 
End Sub
```


## 选择活动工作表上的单元格

如果用  **Select** 方法选定单元格，应注意 **Select** 方法仅用于活动工作表。如果从模块中运行 **Sub** 过程，必须先在该过程中激活工作表，然后才能用 **Select** 方法选定单元格区域，否则 **Select** 方法将失败。例如，下述过程在活动工作簿中将 Sheet1 中的一行复制到 Sheet2 上。


```VB.net
Sub CopyRow() 
    Worksheets("Sheet1").Rows(1).Copy 
    Worksheets("Sheet2").Select 
    Worksheets("Sheet2").Rows(1).Select 
    Worksheets("Sheet2").Paste 
End Sub
```


## 激活选择区域内的单元格

可用  **Activate** 方法激活选定区域内的单元格。即使选定了单元格区域，也只能有一个活动单元格。下述过程选定了一个单元格区域，然后激活该区域内的一个单元格，但并不改变选定区域。


```VB.net
Sub MakeActive() 
    Worksheets("Sheet1").Activate 
    Range("A1:D4").Select 
    Range("B2").Activate 
End Sub
```

