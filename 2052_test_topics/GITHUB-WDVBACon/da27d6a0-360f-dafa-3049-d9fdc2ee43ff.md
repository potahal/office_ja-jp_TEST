
# Series.BarShape Property (Word)

Returns or sets the shape used for a single series in a 3-D bar or column chart. Read/write  **[XlBarShape](a5f77af8-d244-9118-97d5-bb12abc88bef.md)**.


## Syntax

 _表达式_. **BarShape**

 _表达式_ A variable that represents a **[Series](212c323f-8acb-2ba7-1359-ab0f43268e77.md)** object.


## Example

The following example sets the shape used for the first series of the first chart in the active document.


```
With ActiveDocument.InlineShapes(1) 
 If .HasChart Then 
 .Chart.SeriesCollection(1).BarShape = xlConeToPoint 
 End If 
End With
```


## 另请参阅


#### 概念


[Series Object](212c323f-8acb-2ba7-1359-ab0f43268e77.md)
#### 其他资源


[Series Object Members](http://msdn.microsoft.com/library/0bc84851-3f0a-15e0-ae2b-c36215709220%28Office.15%29.aspx)