
# ChartColorFormat.RGB Property (Word)

Returns the red-green-blue value of the specified color. Read-only  **Long**.


## Syntax

 _表达式_. **RGB**

 _表达式_ A variable that represents a **[ChartColorFormat](8bc25b6c-3691-fc85-fcc6-d21ed3f903b9.md)** object.


## Example

The following example enables up and down bars, then adds a criss-cross pattern to the down bars and sets the pattern color to the chart area foreground fill color, for the first chart group of the first chart in the active document.


```
With ActiveDocument.InlineShapes(1) 
 If .HasChart Then 
 With .Chart 
 .ChartGroups(1).HasUpDownBars = True 
 .ChartGroups(1).DownBars.Interior.Pattern = xlPatternCrissCross 
 .ChartGroups(1).DownBars.Interior.PatternColor = _ 
 .ChartArea.Fill.ForeColor.RGB 
 End With 
 End If 
End With
```


## 另请参阅


#### 概念


[ChartColorFormat Object](8bc25b6c-3691-fc85-fcc6-d21ed3f903b9.md)
#### 其他资源


[ChartColorFormat Object Members](http://msdn.microsoft.com/library/f3bbb759-bbc1-366c-a6ce-151c47580fa7%28Office.15%29.aspx)