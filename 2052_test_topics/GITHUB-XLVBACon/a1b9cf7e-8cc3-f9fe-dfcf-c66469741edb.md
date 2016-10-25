
# Chart.ProtectSelection Property (Excel)

 **True** if chart elements cannot be selected. Read/write **Boolean**.


## Syntax

 _表达式_. **ProtectSelection**

 _表达式_ A variable that represents a **Chart** object.


## Remarks

When this property is  **True**, shapes cannot be added to the chart, and the **Click** and **DoubleClick** events for chart elements don't occur.

This property is not persisted when the file is saved. If you set this property to  **True** and then reopen the file, it will no longer be set to **True**.


## Example

This example prevents chart elements from being selected on embedded chart one on worksheet one.


```
Worksheets(1).ChartObjects(1).Chart.ProtectSelection = True
```


## 另请参阅


#### 概念


[Chart Object](179c32ce-49bd-6f36-ea12-89fb5443f3ea.md)
#### 其他资源


[Chart Object Members](http://msdn.microsoft.com/library/a3f8ac44-02d6-6f3f-b5e0-23f4bd5d6baf%28Office.15%29.aspx)