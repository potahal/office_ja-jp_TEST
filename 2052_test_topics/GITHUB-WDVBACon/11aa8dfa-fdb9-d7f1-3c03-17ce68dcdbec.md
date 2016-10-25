
# LegendEntry.LegendKey Property (Word)

Returns the legend key that is associated with the entry. Read-only  **[LegendKey](07578528-3e73-7898-47dc-296aefb854f0.md)**.


## Syntax

 _表达式_. **LegendKey**

 _表达式_ A variable that represents a **[LegendEntry](9f793578-cb9b-faa1-f0a1-ea0f9e90dc6f.md)** object.


## Example

The following example sets the legend key for legend entry one on the first chart in the active document to be a triangle. You should run the example on a 2-D line chart.


```
With ActiveDocument.InlineShapes(1) 
 If .HasChart Then 
 .Chart.Legend.LegendEntries(1).LegendKey _ 
 .MarkerStyle = xlMarkerStyleTriangle 
 End If 
End With
```


## 另请参阅


#### 概念


[LegendEntry Object](9f793578-cb9b-faa1-f0a1-ea0f9e90dc6f.md)
#### 其他资源


[LegendEntry Object Members](http://msdn.microsoft.com/library/d2167011-bb9a-60bb-dd2c-873ffe52e862%28Office.15%29.aspx)