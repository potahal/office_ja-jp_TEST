
# ShapeRange.Line Property (Excel)

Returns a  **[LineFormat](13eca34b-adf7-ddd3-8c73-cc8b508c624a.md)** object that contains line formatting properties for the specified shape. (For a line, the **LineFormat** object represents the line itself; for a shape with a border, the **LineFormat** object represents the border). Read-only.


## Syntax

 _表达式_. **Line**

 _表达式_ A variable that represents a **ShapeRange** object.


## Example

This example adds a blue dashed line to  `myDocument`.


```
Set myDocument = Worksheets(1) 
With myDocument.Shapes.AddLine(10, 10, 250, 250).Line 
 .DashStyle = msoLineDashDotDot 
 .ForeColor.RGB = RGB(50, 0, 128) 
End With
```

This example adds a cross to  `myDocument` and then sets its border to be 8 points thick and red.




```
Set myDocument = Worksheets(1) 
With myDocument.Shapes.AddShape(msoShapeCross, 10, 10, 50, 70).Line 
 .Weight = 8 
 .ForeColor.RGB = RGB(255, 0, 0) 
End With
```


## 另请参阅


#### 概念


[ShapeRange Object](e1b8229c-73a0-4a77-5e00-4bcec9032260.md)
#### 其他资源


[ShapeRange Object Members](http://msdn.microsoft.com/library/1d1950c5-32ac-dfc0-8c19-07159a29a2a0%28Office.15%29.aspx)