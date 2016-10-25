
# Cell.SetWidth Method (Word)

Sets the width of columns or cells in a table.


## Syntax

 _表达式_. **SetWidth**( ** _ColumnWidth_**, ** _RulerStyle_** )

 _表达式_ Required. A variable that represents a **[Cell](cbe6ae71-b2da-63a9-1446-0a2f81ab8b14.md)** object.


### Parameters



|**Name**|**Required/Optional**|**Data Type**|**Description**|
|:-----|:-----|:-----|:-----|
| _ColumnWidth_|必需|**Single**|The width of the specified column or columns, in points.|
| _RulerStyle_|必需|**WdRulerStyle**|Controls the way Word adjusts cell widths.|

## Remarks

The  **WdRulerStyle** behavior described above applies to left-aligned tables. The **WdRulerStyle** behavior for center- and right-aligned tables can be unexpected; in these cases, the **SetWidth** method should be used with care.


## Example

This example creates a table in a new document and sets the width of the first cell in the second row to 1.5 inches. The example preserves the widths of the other cells in the table.


```
Set newDoc = Documents.Add 
Set myTable = _ 
 newDoc.Tables.Add(Range:=Selection.Range, NumRows:=3, _ 
 NumColumns:=3) 
myTable.Cell(2,1).SetWidth _ 
 ColumnWidth:=InchesToPoints(1.5), _ 
 RulerStyle:=wdAdjustNone
```

This example sets the width of the cell that contains the insertion point to 36 points. The example also narrows the first column to preserve the position of the right edge of the table.




```
If Selection.Information(wdWithInTable) = True Then 
 Selection.Cells(1).SetWidth ColumnWidth:=36, _ 
 RulerStyle:=wdAdjustFirstColumn 
Else 
 MsgBox "The insertion point is not in a table." 
End If
```


## 另请参阅


#### 概念


[Cell Object](cbe6ae71-b2da-63a9-1446-0a2f81ab8b14.md)
#### 其他资源


[Cell Object Members](http://msdn.microsoft.com/library/f718bcaa-af8a-682b-f403-6db1aeb9bb73%28Office.15%29.aspx)