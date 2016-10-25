
# Workbook.ChangeHistoryDuration Property (Excel)

Returns or sets the number of days shown in the shared workbook's change history. Read/write  **Long**.


## Syntax

 _表达式_. **ChangeHistoryDuration**

 _表达式_ A variable that represents a **Workbook** object.


## Remarks

Any changes in the change history older than the setting for this property are removed when the workbook is closed.


## Example

This example sets the number of days shown in the change history for the active workbook if change tracking is enabled. Any changes in the change history older than the setting for this property are removed when the workbook is closed.


```
With ActiveWorkbook 
 If .KeepChangeHistory Then 
 .ChangeHistoryDuration = 7 
 End If 
End With
```


## 另请参阅


#### 概念


[Workbook Object](8c00aa60-c974-eed3-0812-3c9625eb0d4c.md)
#### 其他资源


[Workbook Object Members](http://msdn.microsoft.com/library/dce102a3-25de-3ff4-2ce5-bc56e08baca7%28Office.15%29.aspx)