
# Workbook.NewSheet Event (Excel)

Occurs when a new sheet is created in the workbook.


## Syntax

 _表达式_. **NewSheet**( ** _Sh_** )

 _表达式_ A variable that represents a **Workbook** object.


### Parameters



|**Name**|**Required/Optional**|**Data Type**|**Description**|
|:-----|:-----|:-----|:-----|
| _Sh_|必需|**Object**|The new sheet. Can be a  **[Worksheet](182b705e-854a-81cc-a4b0-59b942de55ae.md)** or **[Chart](179c32ce-49bd-6f36-ea12-89fb5443f3ea.md)** object.|

### Return Value

Nothing


## Example

This example moves new sheets to the end of the workbook.


```
Private Sub Workbook_NewSheet(ByVal Sh as Object) 
 Sh.Move After:= Sheets(Sheets.Count) 
End Sub
```


## 另请参阅


#### 概念


[Workbook Object](8c00aa60-c974-eed3-0812-3c9625eb0d4c.md)
#### 其他资源


[Workbook Object Members](http://msdn.microsoft.com/library/dce102a3-25de-3ff4-2ce5-bc56e08baca7%28Office.15%29.aspx)