
# Application.Watches Property (Excel)

Returns a  **[Watches](de403bcc-b927-90f6-75d7-9c936c7f58f7.md)** object representing a range which is tracked when the worksheet is recalculated.


## Syntax

 _表达式_. **Watches**

 _表达式_ A variable that represents an **Application** object.


## Example

This example creates a summation formula in cell A3, and then adds this cell to the Watch Window.


```
Sub AddWatch() 
 With Application 
 .Range("A1").Formula = 1 
 .Range("A2").Formula = 2 
 .Range("A3").Formula = "=Sum(A1:A2)" 
 .Range("A3").Select 
 .Watches.Add Source:=ActiveCell 
 End With 
End Sub
```


## 另请参阅


#### 概念


[Application Object](19b73597-5cf9-4f56-8227-b5211f657f6f.md)
#### 其他资源


[Application Object Members](http://msdn.microsoft.com/library/4cb9ca42-8d07-cc9c-2d80-4eb9a5921e1e%28Office.15%29.aspx)