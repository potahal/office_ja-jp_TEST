
# Axis.TickLabels Property (Excel)

Returns a  **[TickLabels](fcb02bc5-fcdc-db32-168b-2d40e5552991.md)** object that represents the tick-mark labels for the specified axis. Read-only.


## Syntax

 _表达式_. **TickLabels**

 _表达式_ A variable that represents an **Axis** object.


## Example

This example sets the color of the tick-mark label font for the value axis in Chart1.


```
Charts("Chart1").Axes(xlValue).TickLabels.Font.ColorIndex = 3
```


## 另请参阅


#### 概念


[Axis Object](7e08c61b-90f4-8d91-0ee2-84283d10b324.md)
#### 其他资源


[Axis Object Members](http://msdn.microsoft.com/library/2b60f79e-339d-a6cf-7ec6-a915b550c634%28Office.15%29.aspx)