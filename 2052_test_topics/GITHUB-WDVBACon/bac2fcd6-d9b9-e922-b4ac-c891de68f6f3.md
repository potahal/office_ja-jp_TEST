
# Window.DisplayVerticalScrollBar Property (Word)

 **True** if a vertical scroll bar is displayed for the specified window. Read/write **Boolean**.


## Syntax

 _表达式_. **DisplayVerticalScrollBar**

 _表达式_ A variable that represents a **[Window](d92f83f9-ae44-56c0-4584-7a9359253c6d.md)** object.


## Example

This example displays the vertical and horizontal scroll bars for each window in the Windows collection.


```
Dim winLoop As Window 
 
For Each winLoop In Windows 
 winLoop.DisplayVerticalScrollBar = True 
 winLoop.DisplayHorizontalScrollBar = True 
Next winLoop
```

This example toggles the vertical scroll bar for the active window.




```
Dim winTemp As Window 
 
Set winTemp = ActiveDocument.ActiveWindow 
winTemp.DisplayVerticalScrollBar = _ 
 Not winTemp.DisplayVerticalScrollBar
```


## 另请参阅


#### 概念


[Window Object](d92f83f9-ae44-56c0-4584-7a9359253c6d.md)
#### 其他资源


[Window Object Members](http://msdn.microsoft.com/library/c0dec747-3695-4f96-ea25-05b6494aad7e%28Office.15%29.aspx)