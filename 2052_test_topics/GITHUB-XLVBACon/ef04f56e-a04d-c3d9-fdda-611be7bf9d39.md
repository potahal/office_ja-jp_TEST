
# Workbook.Permission Property (Excel)

Returns a  **Permission** object that represents the permission settings in the specified workbook.


## Syntax

 _表达式_. **Permission**

 _表达式_ A variable that represents a **Workbook** object.


## Example

The following example returns the permission settings for the active workbook.


```
Dim objPermission As Permission 
 
Set objPermission = ActiveWorkbook.Permission
```


## 另请参阅


#### 概念


[Workbook Object](8c00aa60-c974-eed3-0812-3c9625eb0d4c.md)
#### 其他资源


[Workbook Object Members](http://msdn.microsoft.com/library/dce102a3-25de-3ff4-2ce5-bc56e08baca7%28Office.15%29.aspx)