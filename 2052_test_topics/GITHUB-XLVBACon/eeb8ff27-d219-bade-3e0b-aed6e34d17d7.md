
# Application.Width Property (Excel)

Returns or sets a  **Double** value that represents the distance, in points, from the left edge of the application window to its right edge.


## Syntax

 _表达式_. **Width**

 _表达式_ A variable that represents an **Application** object.


## Remarks

 If the window is minimized, **Width** is read-only and returns the width of the window icon.


## Example

This example expands the active window to the maximum size available (assuming that the window isn't maximized).


```
With ActiveWindow 
 .WindowState = xlNormal 
 .Top = 1 
 .Left = 1 
 .Height = Application.UsableHeight 
 .Width = Application.UsableWidth 
End With
```


## 另请参阅


#### 概念


[Application Object](19b73597-5cf9-4f56-8227-b5211f657f6f.md)
#### 其他资源


[Application Object Members](http://msdn.microsoft.com/library/4cb9ca42-8d07-cc9c-2d80-4eb9a5921e1e%28Office.15%29.aspx)