
# PivotCell.PivotTable Property (Excel)

Returns a  **[PivotTable](a9c1d4a0-78a9-f9a6-6daf-91cb63e45842.md)** object that represents the PivotTable report associated with the PivotCell.


## Syntax

 _表达式_. **PivotTable**

 _表达式_ A variable that represents a **PivotCell** object.


## Example

This example sets the current page for the PivotTable report on Sheet1 to the page named "Canada."


```
Set pvtTable = Worksheets("Sheet1").Range("A3").PivotTable 
pvtTable.PivotFields("Country").CurrentPage = "Canada"
```

This example determines the PivotTable report associated with the Sales chart on the active worksheet, and then it sets the page named "Oregon" as the current page for the PivotTable report.




```
Set objPT = _ 
 ActiveSheet.Charts("Sales").PivotLayout.PivotTable 
objPT.PivotFields("State").CurrentPageName = "Oregon"
```


## 另请参阅


#### 概念


[PivotCell Object](76b8a2dc-90ee-7475-d327-d27cb1e92703.md)
#### 其他资源


[PivotCell Object Members](http://msdn.microsoft.com/library/e486cd5d-3f31-29d4-b811-24fc0aed6803%28Office.15%29.aspx)