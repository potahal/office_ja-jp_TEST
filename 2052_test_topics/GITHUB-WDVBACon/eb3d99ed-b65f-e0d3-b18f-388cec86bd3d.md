
# TextFrame.HasText Property (Word)

 **True** if the specified shape has text associated with it. Read-only **Boolean**.


## Syntax

 _表达式_. **HasText**

 _表达式_ A variable that represents a **[TextFrame](46f7e410-80d9-9fe9-2224-488b623f8592.md)** object.


## Example

If the second shape on the active document contains text, this example displays a message if the text overflows its frame.


```
Dim docActive As Document 
 
Set docActive = ActiveDocument 
With docActive.Shapes(2).TextFrame 
 If .HasText = True Then 
 If .Overflowing = True Then 
 Msgbox "Text overflows the frame." 
 End If 
 End If 
End With
```


## 另请参阅


#### 概念


[TextFrame Object](46f7e410-80d9-9fe9-2224-488b623f8592.md)
#### 其他资源


[TextFrame Object Members](http://msdn.microsoft.com/library/bb2efcc6-474f-3de5-6d20-940be7549112%28Office.15%29.aspx)