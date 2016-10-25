
# Shape.ShapeStyle Property (Word)

Returns or sets the shape style for the specified shape. Read/write  **[MsoShapeStyleIndex](http://msdn.microsoft.com/library/61f34054-28e7-6891-5442-3598d64284a0%28Office.15%29.aspx)**.


## Syntax

 _表达式_. **ShapeStyle**

 _表达式_ A variable that represents a **[Shape](604029ce-9b2f-9748-5d4e-b458796fa2f0.md)** object.


## Example

The following code example changes the shape style for the first shape in the active document.


```
Dim myShape As Shape 
 
Set myShape = ActiveDocument.Shapes(1) 
 
myShape.ShapeStyle = msoLineStylePreset12
```


## 另请参阅


#### 概念


[Shape Object](604029ce-9b2f-9748-5d4e-b458796fa2f0.md)
#### 其他资源


[Shape Object Members](http://msdn.microsoft.com/library/4aa8e2f4-5629-3922-11e4-df028bd1e1de%28Office.15%29.aspx)