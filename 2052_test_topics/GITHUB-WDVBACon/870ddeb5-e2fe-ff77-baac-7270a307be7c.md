
# Paragraph.Shading Property (Word)

Returns a  **[Shading](e136509a-1be1-29e4-7b37-1faf659e37ba.md)** object that refers to the shading formatting for the specified paragraph.


## Syntax

 _表达式_. **Shading**

 _表达式_ Required. A variable that represents a **[Paragraph](0a704079-a082-4ab1-841b-fc0d49dd26d4.md)** object.


## Example

This example applies yellow shading to the first paragraph in the selection.


```
With Selection.Paragraphs(1).Shading 
 .Texture = wdTexture12Pt5Percent 
 .BackgroundPatternColorIndex = wdYellow 
 .ForegroundPatternColorIndex = wdBlack 
End With
```


## 另请参阅


#### 概念


[Paragraph Object](0a704079-a082-4ab1-841b-fc0d49dd26d4.md)
#### 其他资源


[Paragraph Object Members](http://msdn.microsoft.com/library/e1fc5b91-e908-580e-ab72-898648a5c0c3%28Office.15%29.aspx)