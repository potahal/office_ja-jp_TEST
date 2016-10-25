
# LineFormat.Pattern Property (Word)

Returns or sets a value that represents the pattern applied to the specified line. Read/write  **MsoPatternType**.


## Syntax

 _表达式_. **Pattern**

 _表达式_ Required. A variable that represents a **[LineFormat](28fabccb-d03f-3466-9d07-ea3ebc4cdd11.md)** object.


## Example

This example adds a patterned line to  _myDocument_.


```
Set myDocument = ActiveDocument 
With myDocument.Shapes.AddLine(10, 100, 250, 0).Line 
 .Weight = 6 
 .ForeColor.RGB = RGB(0, 0, 255) 
 .BackColor.RGB = RGB(128, 0, 0) 
 .Pattern = msoPatternDarkDownwardDiagonal 
End With
```


## 另请参阅


#### 概念


[LineFormat Object](28fabccb-d03f-3466-9d07-ea3ebc4cdd11.md)
#### 其他资源


[LineFormat Object Members](http://msdn.microsoft.com/library/775fcd1f-f4be-f607-c63b-4ae952b7c524%28Office.15%29.aspx)