
# Selection.Shrink Method (Word)

Shrinks the selection to the next smaller unit of text.


## Syntax

 _表达式_. **Shrink**

 _表达式_ A variable that represents a **[Selection](7b574a91-c33e-ecfd-6783-6b7528b2ed8f.md)** object.


## Remarks

The unit progression for this method is as follows: entire document, section, paragraph, sentence, word, insertion point.


## Example

This example collapses the selected text to the next smaller unit of text.


```
If Selection.Type = wdSelectionNormal Then 
 Selection.Shrink 
Else 
 MsgBox "You need to select some text." 
End If
```


## 另请参阅


#### 概念


[Selection Object](7b574a91-c33e-ecfd-6783-6b7528b2ed8f.md)
#### 其他资源


[Selection Object Members](http://msdn.microsoft.com/library/71e67a43-d40a-ad9a-8ef2-c5c487733e0d%28Office.15%29.aspx)