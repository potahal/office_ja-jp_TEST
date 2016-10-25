
# Selection.TypeBackspace Method (Word)

Deletes the character preceding a collapsed selection (an insertion point).


## Syntax

 _表达式_. **TypeBackspace**

 _表达式_ Required. A variable that represents a **[Selection](7b574a91-c33e-ecfd-6783-6b7528b2ed8f.md)** object.


## Remarks

This method corresponds to functionality of the BACKSPACE key. If the selection isn't collapsed to an insertion point, the selection is deleted.


## Example

This example deletes the character preceding the insertion point (the collapsed selection).


```
With Selection 
 .Collapse Direction:=wdCollapseEnd 
 .TypeBackspace 
End With
```

This example extends the selection to the end of the current paragraph (including the paragraph mark) and then deletes the selection.




```
With Selection 
 .EndOf Unit:=wdParagraph, Extend:=wdExtend 
 .TypeBackspace 
End With
```


## 另请参阅


#### 概念


[Selection Object](7b574a91-c33e-ecfd-6783-6b7528b2ed8f.md)
#### 其他资源


[Selection Object Members](http://msdn.microsoft.com/library/71e67a43-d40a-ad9a-8ef2-c5c487733e0d%28Office.15%29.aspx)