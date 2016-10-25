
# ChartGroup.DropLines Property (Excel)

Returns a  **[DropLines](88fdf5f5-2842-2d68-a073-18d05fd2fa38.md)** object that represents the drop lines for a series on a line chart or area chart. Applies only to line charts or area charts. Read-only.


## Syntax

 _表达式_. **DropLines**

 _表达式_ A variable that represents a **ChartGroup** object.


## Example

This example turns on drop lines for chart group one in Chart1 and then sets their line style, weight, and color. The example should be run on a 2-D line chart that has one series.


```
With Charts("Chart1").ChartGroups(1) 
 .HasDropLines = True 
 With .DropLines.Border 
 .LineStyle = xlThin 
 .Weight = xlMedium 
 .ColorIndex = 3 
 End With 
End With
```


## 另请参阅


#### 概念


[ChartGroup Object](7eee66c5-04a7-fd86-6e34-4c22ccaf8de0.md)
#### 其他资源


[ChartGroup Object Members](http://msdn.microsoft.com/library/2d31f7af-d639-c8f4-0714-08fc618ec92d%28Office.15%29.aspx)