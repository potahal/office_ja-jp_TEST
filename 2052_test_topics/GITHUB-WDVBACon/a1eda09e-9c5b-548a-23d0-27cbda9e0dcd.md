
# Window.Document Property (Word)

Returns a  **[Document](8d83487a-2345-a036-a916-971c9db5b7fb.md)** object associated with the specified pane, window, or selection. Read-only.


## Syntax

 _表达式_. **Document**

 _表达式_ A variable that represents a **[Window](d92f83f9-ae44-56c0-4584-7a9359253c6d.md)** object.


## Example

This example sets myDoc to the document associated with the active window. The focus is changed to the next window, and the window is split. The  **Activate** method is used to switch back to the original document.


```
Set myDoc = Application.ActiveWindow.Document 
If Windows.Count >= 2 Then 
 Application.ActiveWindow.Next.Activate 
 Application.ActiveWindow.Split = True 
 myDoc.Activate 
End If
```


## 另请参阅


#### 概念


[Window Object](d92f83f9-ae44-56c0-4584-7a9359253c6d.md)
#### 其他资源


[Window Object Members](http://msdn.microsoft.com/library/c0dec747-3695-4f96-ea25-05b6494aad7e%28Office.15%29.aspx)