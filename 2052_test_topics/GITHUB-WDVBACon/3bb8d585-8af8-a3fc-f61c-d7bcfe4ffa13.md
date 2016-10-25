
# Shape.Line Property (Word)

Returns a  **LineFormat** object that contains line formatting properties for the specified shape. Read-only.


## Syntax

 _表达式_. **Line**

 _表达式_ A variable that represents a **[Shape](604029ce-9b2f-9748-5d4e-b458796fa2f0.md)** object.


## Remarks

For a line, the  **LineFormat** object represents the line itself; for a shape with a border, the **LineFormat** object represents the border.


## Example

This example adds a blue dashed line to  _myDocument_.


```
Set myDocument = ActiveDocument 
With myDocument.Shapes.AddLine(10, 10, 250, 250).Line 
 .DashStyle = msoLineDashDotDot 
 .ForeColor.RGB = RGB(50, 0, 128) 
End With
```

This example adds a cross to  _myDocument_ and then sets its border to be 8 points thick and red.




```
Set myDocument = ActiveDocument 
With myDocument.Shapes.AddShape(msoShapeCross, 10, 10, 50, 70).Line 
 .Weight = 8 
 .ForeColor.RGB = RGB(255, 0, 0) 
End With
```


## 另请参阅


#### 概念


[Shape Object](604029ce-9b2f-9748-5d4e-b458796fa2f0.md)
#### 其他资源


[Shape Object Members](http://msdn.microsoft.com/library/4aa8e2f4-5629-3922-11e4-df028bd1e1de%28Office.15%29.aspx)