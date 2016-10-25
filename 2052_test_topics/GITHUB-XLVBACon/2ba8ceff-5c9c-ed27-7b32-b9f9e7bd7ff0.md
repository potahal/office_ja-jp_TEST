
# PivotItem.RecordCount Property (Excel)

Returns the number of records in the PivotTable cache or the number of cache records that contain the specified item. Read-only  **Long**.


## Syntax

 _表达式_. **RecordCount**

 _表达式_ A variable that represents a **PivotItem** object.


## Remarks

This property reflects the transient state of the cache at the time that it's queried. The cache can change between queries.


## Example

This example displays the number of cache records that contain "Kiwi" in the "Products" field.


```
MsgBox Worksheets(1).PivotTables("Pivot1") _ 
 .PivotFields("Product").PivotItems("Kiwi").RecordCount
```


## 另请参阅


#### 概念


[PivotItem Object](5829a1d9-0924-9ce8-1120-229e4595285a.md)
#### 其他资源


[PivotItem Object Members](http://msdn.microsoft.com/library/dde86683-8c89-2484-cdd0-8c3db0c06f45%28Office.15%29.aspx)