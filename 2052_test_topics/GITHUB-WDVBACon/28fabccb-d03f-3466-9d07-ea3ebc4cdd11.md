
# LineFormat Object (Word)

Represents line and arrowhead formatting. For a line, the  **LineFormat** object contains formatting information for the line itself; for a shape with a border, this object contains formatting information for the shape's border.


## Remarks

Use the  **Line** property to return a **LineFormat** object. The following example adds a blue, dashed line to the active document. There is a short, narrow oval at the line's starting point and a long, wide triangle at its endpoint.


```
With ActiveDocument.Shapes.AddLine(100, 100, 200, 300).Line 
 .DashStyle = msoLineDashDotDot 
 .ForeColor.RGB = RGB(50, 0, 128) 
 .BeginArrowheadLength = msoArrowheadShort 
 .BeginArrowheadStyle = msoArrowheadOval 
 .BeginArrowheadWidth = msoArrowheadNarrow 
 .EndArrowheadLength = msoArrowheadLong 
 .EndArrowheadStyle = msoArrowheadTriangle 
 .EndArrowheadWidth = msoArrowheadWide 
End With
```


## 另请参阅


#### 概念


[Word Object Model Reference](be452561-b436-bb9b-6f94-3faa9a74a6fd.md)
#### 其他资源


[LineFormat Object Members](http://msdn.microsoft.com/library/775fcd1f-f4be-f607-c63b-4ae952b7c524%28Office.15%29.aspx)