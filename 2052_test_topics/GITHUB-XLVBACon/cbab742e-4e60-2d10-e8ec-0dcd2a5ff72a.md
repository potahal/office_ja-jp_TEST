
# ChartGroup.BubbleScale Property (Excel)

Returns or sets the scale factor for bubbles in the specified chart group. Can be an integer value from 0 (zero) to 300, corresponding to a percentage of the default size. Applies only to bubble charts. Read/write  **Long**.


## Syntax

 _表达式_. **BubbleScale**

 _表达式_ A variable that represents a **ChartGroup** object.


## Example

This example sets the bubble size in chart group one to 200% of the default size.


```
With Worksheets(1).ChartObjects(1).Chart 
 .ChartGroups(1).BubbleScale = 200 
End With
```


## 另请参阅


#### 概念


[ChartGroup Object](7eee66c5-04a7-fd86-6e34-4c22ccaf8de0.md)
#### 其他资源


[ChartGroup Object Members](http://msdn.microsoft.com/library/2d31f7af-d639-c8f4-0714-08fc618ec92d%28Office.15%29.aspx)