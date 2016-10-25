
# Pane.VerticalPercentScrolled Property (Word)

Returns or sets the vertical scroll position as a percentage of the document length. Read/write  **Long**.


## Syntax

 _表达式_. **VerticalPercentScrolled**

 _表达式_ Required. A variable that represents a **[Pane](4a0c2690-d9d2-4e34-fef4-cc41365f5251.md)** object.


## Example

This example vertically scrolls the active pane of the window for Document1 to the end.


```
With Windows("Document1") 
 .Activate 
 .ActivePane.VerticalPercentScrolled = 100 
End With
```


## 另请参阅


#### 概念


[Pane Object](4a0c2690-d9d2-4e34-fef4-cc41365f5251.md)
#### 其他资源


[Pane Object Members](http://msdn.microsoft.com/library/e0739460-3209-f981-71ea-80a5ea7f8935%28Office.15%29.aspx)