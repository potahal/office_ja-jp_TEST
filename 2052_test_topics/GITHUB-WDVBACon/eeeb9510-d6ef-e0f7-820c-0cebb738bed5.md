
# ChartCharacters.Application Property (Word)

When used without an object qualifier, returns an  **[Application](d1cf6f8f-4e88-bf01-93b4-90a83f79cb44.md)** object that represents the Microsoft Word application. When used with an object qualifier, returns an **Application** object that represents the creator of the specified object (you can use this property with an Automation object to return the application of that object). Read-only.


## Syntax

 _表达式_. **Application**

 _表达式_ A variable that represents a **[ChartCharacters](cffe50a7-3fdc-75ad-2e32-081ba2310c1d.md)** object.


## Example

The following example displays a message about the application that created  `myObject`.


```
Set myObject = ActiveDocument 
If myObject.Application.Value = "Microsoft Word" Then 
 MsgBox "This is a Word Application object." 
Else 
 MsgBox "This is not a Word Application object." 
End If
```


## 另请参阅


#### 概念


[ChartCharacters Object](cffe50a7-3fdc-75ad-2e32-081ba2310c1d.md)
#### 其他资源


[ChartCharacters Object Members](http://msdn.microsoft.com/library/eb07f51c-64e4-274f-81f4-cc5a7b9694e6%28Office.15%29.aspx)