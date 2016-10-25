
# Range.EmphasisMark Property (Word)

Returns or sets the emphasis mark for a character or designated character string. Read/write  **WdEmphasisMark**.


## Syntax

 _表达式_. **EmphasisMark**

 _表达式_ Required. A variable that represents a **[Range](15a7a1c4-5f3f-5b6e-60e9-29688de3f274.md)** object.


## Example

This example sets the emphasis mark over the fourth word in the active document to a comma.


```
ActiveDocument.Words(4).EmphasisMark = wdEmphasisMarkOverComma
```


## 另请参阅


#### 概念


[Range Object](15a7a1c4-5f3f-5b6e-60e9-29688de3f274.md)
#### 其他资源


[Range Object Members](http://msdn.microsoft.com/library/3c4a36d9-2a80-5aaf-827b-275a52bfa193%28Office.15%29.aspx)