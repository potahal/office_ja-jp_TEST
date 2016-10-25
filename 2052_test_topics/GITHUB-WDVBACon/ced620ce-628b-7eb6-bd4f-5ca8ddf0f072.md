
# Column.AutoFit Method (Word)

Changes the width of a table column to accommodate the width of the text without changing the way text wraps in the cells.


## Syntax

 _表达式_. **AutoFit**

 _表达式_ Required. A variable that represents a **[Column](49d68571-2a57-6795-34b9-eb09aeb43043.md)** object.


## Remarks

If the table is already as wide as the distance between the left and right margins, this method has no affect.


## Example

This example creates a 3x3 table in a new document and then changes the width of the first column to accommodate the width of the text.


```
Dim docNew as Document 
Dim tableNew as Table 
 
Set docNew = Documents.Add 
Set tableNew = docNew.Tables.Add(Range:=Selection.Range, _ 
 NumRows:=3, NumColumns:=3) 
With tableNew 
 .Cell(1,1).Range.InsertAfter "First cell" 
 .Columns(1).AutoFit 
End With
```

This example creates a 3x3 table in a new document and then changes the width of all the columns to accommodate the width of the text.




```
Dim docNew as Document 
Dim tableNew as Table 
 
Set docNew = Documents.Add 
Set tableNew = docNew.Tables.Add(Selection.Range, 3, 3) 
With tableNew 
 .Cell(1,1).Range.InsertAfter "First cell" 
 .Cell(1,2).Range.InsertAfter "This is cell (1,2)" 
 .Cell(1,3).Range.InsertAfter "(1,3)" 
 .Columns.AutoFit 
End With
```


## 另请参阅


#### 概念


[Column Object](49d68571-2a57-6795-34b9-eb09aeb43043.md)
#### 其他资源


[Column Object Members](http://msdn.microsoft.com/library/e8b86d58-eb4b-6d02-7171-f70436a31f4c%28Office.15%29.aspx)