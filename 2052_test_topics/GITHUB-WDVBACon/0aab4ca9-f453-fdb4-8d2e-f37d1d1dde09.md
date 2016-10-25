
# Options.PasteAdjustParagraphSpacing Property (Word)

 **True** if Microsoft Word automatically adjusts the spacing of paragraphs when cutting and pasting selections. Read/write **Boolean**.


## Syntax

 _表达式_. **PasteAdjustParagraphSpacing**

 _表达式_ A variable that represents a **[Options](873b7b99-3fe1-fd89-9ece-a9355cb827dc.md)** object.


## Example

This example sets Word to automatically adjust the spacing of paragraphs when cutting and pasting selections if the option has been disabled.


```
Sub AdjustParaSpace() 
 With Options 
 If .PasteAdjustParagraphSpacing = False Then 
 .PasteAdjustParagraphSpacing = True 
 End If 
 End With 
End Sub
```


## 另请参阅


#### 概念


[Options Object](873b7b99-3fe1-fd89-9ece-a9355cb827dc.md)
#### 其他资源


[Options Object Members](http://msdn.microsoft.com/library/76cd9dfe-6bbb-4c3d-0bfc-79a62bedd15e%28Office.15%29.aspx)