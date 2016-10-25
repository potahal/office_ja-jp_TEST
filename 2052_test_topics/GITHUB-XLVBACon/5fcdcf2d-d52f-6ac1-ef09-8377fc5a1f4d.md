
# PivotCache.RecordCount Property (Excel)

Returns the number of records in the PivotTable cache or the number of cache records that contain the specified item. Read-only  **Long**.


## Syntax

 _表达式_. **RecordCount**

 _表达式_ A variable that represents a **PivotCache** object.


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


[PivotCache Object](c3d84ef1-f9e6-b1bc-cbf0-3ba8dfe17439.md)
#### 其他资源


[PivotCache Object Members](http://msdn.microsoft.com/library/113f1109-e1c9-2c6e-0581-9fba82f278dc%28Office.15%29.aspx)