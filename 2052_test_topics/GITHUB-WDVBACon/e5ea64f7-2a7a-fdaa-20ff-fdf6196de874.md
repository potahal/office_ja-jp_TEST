
# Application.UsableWidth Property (Word)

Returns the maximum width (in points) to which you can set the width of a Microsoft Word document window. Read-only  **Long**.


## Syntax

 _表达式_. **UsableWidth**

 _表达式_ A variable that represents an **[Application](d1cf6f8f-4e88-bf01-93b4-90a83f79cb44.md)** object.


## Example

This example sets the size of the active document window to one quarter of the maximum allowable screen area.


```
With ActiveDocument.ActiveWindow 
 .WindowState = wdWindowStateNormal 
 .Top = 5 
 .Left = 5 
 .Height = (Application.UsableHeight*0.5) 
 .Width = (Application.UsableWidth*0.5) 
End With
```

This example displays the size of the working area in the active document window.




```
With ActiveDocument.ActiveWindow 
 MsgBox "Working area height = " _ 
 &amp; .UsableHeight &amp; vbLf _ 
 &amp; "Working area width = " _ 
 &amp; .UsableWidth 
End With
```


## 另请参阅


#### 概念


[Application Object](d1cf6f8f-4e88-bf01-93b4-90a83f79cb44.md)
#### 其他资源


[Application Object Members](http://msdn.microsoft.com/library/71669f1e-65f1-b0f1-b67d-355dfdbebe50%28Office.15%29.aspx)