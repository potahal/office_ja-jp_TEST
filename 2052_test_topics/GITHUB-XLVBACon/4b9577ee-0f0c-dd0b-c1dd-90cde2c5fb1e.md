
# Application.Ready Property (Excel)

Returns  **True** when the Microsoft Excel application is ready; **False** when the Excel application is not ready. Read-only **Boolean**.


## Syntax

 _表达式_. **Ready**

 _表达式_ A variable that represents an **Application** object.


## Example

In this example, Microsoft Excel checks to see if the  **Ready** property is set to **True**, and if so, a message displays "Application is ready." Otherwise, Excel displays the message "Application is not ready."


```
Sub UseReady() 
 
 If Application.Ready = True Then 
 MsgBox "Application is ready." 
 Else 
 MsgBox "Application is not ready." 
 End If 
 
End Sub
```


## 另请参阅


#### 概念


[Application Object](19b73597-5cf9-4f56-8227-b5211f657f6f.md)
#### 其他资源


[Application Object Members](http://msdn.microsoft.com/library/4cb9ca42-8d07-cc9c-2d80-4eb9a5921e1e%28Office.15%29.aspx)