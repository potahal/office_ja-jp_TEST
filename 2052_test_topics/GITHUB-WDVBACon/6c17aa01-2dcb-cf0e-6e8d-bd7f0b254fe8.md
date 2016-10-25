
# Options.InsertedTextMark Property (Word)

Returns or sets how Microsoft Word formats inserted text while change tracking is enabled (the  **TrackRevisions** property is **True** ). Read/write **WdInsertedTextMark**.


## Syntax

 _表达式_. **InsertedTextMark**

 _表达式_ Required. A variable that represents an **[Options](873b7b99-3fe1-fd89-9ece-a9355cb827dc.md)** collection.


## Remarks

If change tracking is not enabled, this property is ignored. Use this property with the  **InsertedTextColor** property to control the appearance of inserted text in a document.

The  **ShowRevisions** property must be **True** to see the formatting for inserted text during editing. The **PrintRevisions** property must be **True** in order for Word to use the formatting for inserted text when printing a document.


## Example

This example sets Word to italicize inserted text.


```
Options.InsertedTextMark = wdInsertedTextMarkItalic
```

This example sets Word to format inserted text as bold if it isn't already.




```
If Options.InsertedTextMark <> wdInsertedTextMarkBold Then 
 Options.InsertedTextMark = wdInsertedTextMarkBold 
Else 
 MsgBox Prompt:="Inserted text is already bold!" 
End If
```


## 另请参阅


#### 概念


[Options Object](873b7b99-3fe1-fd89-9ece-a9355cb827dc.md)
#### 其他资源


[Options Object Members](http://msdn.microsoft.com/library/76cd9dfe-6bbb-4c3d-0bfc-79a62bedd15e%28Office.15%29.aspx)