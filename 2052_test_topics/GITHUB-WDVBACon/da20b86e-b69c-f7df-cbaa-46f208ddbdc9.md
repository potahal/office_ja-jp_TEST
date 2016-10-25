
# ParagraphFormat.SpaceBefore Property (Word)

Returns or sets the spacing (in points) before the specified paragraphs. Read/write  **Single**.


## Syntax

 _表达式_. **SpaceBefore**

 _表达式_ Required. A variable that represents a **[ParagraphFormat](712d754a-dc92-f1a3-531d-dfae74a42c23.md)** object.


## Example

This example sets the spacing before all paragraphs in the active document to 12 points.


```
ActiveDocument.Range.ParagraphFormat.SpaceBefore = 12
```


## 另请参阅


#### 概念


[ParagraphFormat Object](712d754a-dc92-f1a3-531d-dfae74a42c23.md)
#### 其他资源


[ParagraphFormat Object Members](http://msdn.microsoft.com/library/d34122e7-adfb-dd34-eb1d-cd62b20a83ff%28Office.15%29.aspx)