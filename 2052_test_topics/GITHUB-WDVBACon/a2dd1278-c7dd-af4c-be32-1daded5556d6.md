
# LineNumbering Object (Word)

Represents line numbers in the left margin or to the left of each newspaper-style column.


## Remarks

Use the  **LineNumbering** property to return the **LineNumbering** object. The following example applies line numbering to the text in the first section of the active document.


```
With ActiveDocument.Sections(1).PageSetup.LineNumbering 
 .Active = True 
 .CountBy = 5 
 .RestartMode = wdRestartPage 
End With
```

The following example applies line numbering to the pages in the current section.




```
Selection.PageSetup.LineNumbering.Active = True
```


## 另请参阅


#### 概念


[Word Object Model Reference](be452561-b436-bb9b-6f94-3faa9a74a6fd.md)
#### 其他资源


[LineNumbering Object Members](http://msdn.microsoft.com/library/f1301749-6e7d-f547-abe8-073661966fc2%28Office.15%29.aspx)