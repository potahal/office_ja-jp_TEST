
# ShapeRange.VerticalFlip Property (Word)

 **True** if the specified shape is flipped around the vertical axis. Read-only **MsoTriState**.


## Syntax

 _表达式_. **VerticalFlip**

 _表达式_ Required. A variable that represents a **[ShapeRange](7112acc0-e241-16ef-77bc-101b72d05af0.md)** object.


## Example

This example restores each shape on  _myDocument_ to its original state if it has been flipped horizontally or vertically.


```
For Each s In ActiveDocument.Range.ShapeRange 
 If s.HorizontalFlip Then s.Flip msoFlipHorizontal 
 If s.VerticalFlip Then s.Flip msoFlipVertical 
Next
```


## 另请参阅


#### 概念


[ShapeRange Collection Object](7112acc0-e241-16ef-77bc-101b72d05af0.md)
#### 其他资源


[ShapeRange Object Members](http://msdn.microsoft.com/library/eb882d13-d724-26e9-7e6d-2af55e42bba1%28Office.15%29.aspx)