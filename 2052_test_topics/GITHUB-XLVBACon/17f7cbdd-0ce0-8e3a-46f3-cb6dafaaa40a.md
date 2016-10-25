
# Workbook.CanCheckIn Method (Excel)

 **True** if Microsoft Excel can check in a specified workbook to a server. Read/write **Boolean**.


## Syntax

 _表达式_. **CanCheckIn**

 _表达式_ A variable that represents a **Workbook** object.


### Return Value

Boolean


## Example

This example checks the server to see if the specified workbook can be checked in. If it can be, it saves and closes the workbook and checks it back into the server.


```
Sub CheckInOut(strWkbCheckIn As String) 
 
 ' Determine if workbook can be checked in. 
 If Workbooks(strWkbCheckIn).CanCheckIn = True Then 
 Workbooks(strWkbCheckIn).CheckIn 
 MsgBox strWkbCheckIn &amp; " has been checked in." 
 Else 
 MsgBox "This file cannot be checked in " &amp; _ 
 "at this time. Please try again later." 
 End If 
 
End Sub
```


## 另请参阅


#### 概念


[Workbook Object](8c00aa60-c974-eed3-0812-3c9625eb0d4c.md)
#### 其他资源


[Workbook Object Members](http://msdn.microsoft.com/library/dce102a3-25de-3ff4-2ce5-bc56e08baca7%28Office.15%29.aspx)