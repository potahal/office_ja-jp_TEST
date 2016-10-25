
# Paragraphs.Last Property (Word)

Returns a  **Paragraph** object that represents the last item in the collection of paragraphs.


## Syntax

 _表达式_. **Last**

 _表达式_ Required. A variable that represents a **[Paragraphs](bdc7a183-2a98-7d47-c86a-5cecd6c91449.md)** collection.


## Example

This example formats the last paragraph in the active document to be right-aligned.


```
ActiveDocument.Paragraphs.Last.Alignment = wdAlignParagraphRight
```


## 另请参阅


#### 概念


[Paragraphs Collection Object](bdc7a183-2a98-7d47-c86a-5cecd6c91449.md)
#### 其他资源


[Paragraphs Object Members](http://msdn.microsoft.com/library/490e2695-3cdd-4906-f730-583d18486aa2%28Office.15%29.aspx)