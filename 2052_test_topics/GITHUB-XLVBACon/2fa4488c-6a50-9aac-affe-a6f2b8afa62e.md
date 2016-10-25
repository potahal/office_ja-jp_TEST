
# ChartGroup.AxisGroup Property (Excel)

Returns or sets the group for the specified chart. Read/write


## Syntax

 _表达式_. **AxisGroup**

 _表达式_ A variable that represents a **[ChartGroup](7eee66c5-04a7-fd86-6e34-4c22ccaf8de0.md)** object.


### Return Value

 **[XlAxisGroup](30e0b817-547f-70f8-6e27-4a14031d1d79.md)**


## Remarks

For 3-D charts, only  **xlPrimary** is valid.


## Example

This example deletes the value axis if it is in the secondary group.


```
With myChart.Axes(xlValue) 
 If .AxisGroup = xlSecondary Then .Delete 
End With 

```


## 另请参阅


#### 概念


[ChartGroup Object](7eee66c5-04a7-fd86-6e34-4c22ccaf8de0.md)
#### 其他资源


[ChartGroup Object Members](http://msdn.microsoft.com/library/2d31f7af-d639-c8f4-0714-08fc618ec92d%28Office.15%29.aspx)