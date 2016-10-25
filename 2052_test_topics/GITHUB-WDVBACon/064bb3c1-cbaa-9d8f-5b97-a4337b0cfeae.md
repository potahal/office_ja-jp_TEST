
# Selection.FootnoteOptions Property (Word)

Returns  **[FootnoteOptions](5fdeb6d6-ce33-44f5-62c1-743fc3770457.md)** object that represents the footnotes in a selection.


## Syntax

 _表达式_. **FootnoteOptions**

 _表达式_ A variable that represents a **[Selection](7b574a91-c33e-ecfd-6783-6b7528b2ed8f.md)** object.


## Example

This example sets the numbering rule in the selection to restart at the beginning of the new section.


```
Sub SetFootnoteOptionsRange() 
 Selection.FootnoteOptions.NumberingRule = wdRestartSection 
End Sub
```


## 另请参阅


#### 概念


[Selection Object](7b574a91-c33e-ecfd-6783-6b7528b2ed8f.md)
#### 其他资源


[Selection Object Members](http://msdn.microsoft.com/library/71e67a43-d40a-ad9a-8ef2-c5c487733e0d%28Office.15%29.aspx)