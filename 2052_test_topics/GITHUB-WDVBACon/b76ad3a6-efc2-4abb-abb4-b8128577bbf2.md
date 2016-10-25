
# TextRetrievalMode Object (Word)

Represents options that control how text is retrieved from a  **Range** object.


## Remarks

Use the  **TextRetrievalMode** property to return a **TextRetrievalMode** object. The following example displays the text of the first sentence in the active document, excluding field codes and hidden text.


```
With ActiveDocument.Sentences(1).TextRetrievalMode 
 .IncludeHiddenText = False 
 .IncludeFieldCodes = False 
 MsgBox .Parent.Text 
End With
```

Changing the  **ViewType**, **IncludeHiddentText**, or **IncludeFieldCodes** property of the **TextRetrievalMode** object doesn't change the screen display. Instead, changing one of these properties determines what text is retrieved from a **Range** object when the **Text** property is used.


## 另请参阅


#### 概念


[Word Object Model Reference](be452561-b436-bb9b-6f94-3faa9a74a6fd.md)
#### 其他资源


[TextRetrievalMode Object Members](http://msdn.microsoft.com/library/396684eb-f260-9e82-e8b5-14301c5e55c3%28Office.15%29.aspx)