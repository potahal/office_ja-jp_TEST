
# PageNumbers.IncludeChapterNumber Property (Word)

 **True** if a chapter number is included with page numbers or a caption label. Read/write **Boolean**.


## Syntax

 _表达式_. **IncludeChapterNumber**

 _表达式_ A variable that represents a **[PageNumbers](9090f96e-d898-ace6-35fa-f6e59c527ea2.md)** object.


## Example

This example adds page numbers in the footer for section one in the active document. The page numbers include the chapter number.


```
With ActiveDocument.Sections(1).Footers _ 
 (wdHeaderFooterPrimary).PageNumbers 
 .Add 
 .IncludeChapterNumber = True 
 .HeadingLevelForChapter = 1 
End With
```

This example adds the chapter number from the Heading 2 style to figure captions, sets the caption numbering style, and then inserts a new figure caption. The document should already contain a Heading 2 style with numbering.




```
With CaptionLabels(wdCaptionFigure) 
 .IncludeChapterNumber = True 
 .ChapterStyleLevel = 2 
 .NumberStyle = wdCaptionNumberStyleUppercaseLetter 
End With 
Selection.InsertCaption Label:="Figure", Title:=": History"
```


## 另请参阅


#### 概念


[PageNumbers Collection Object](9090f96e-d898-ace6-35fa-f6e59c527ea2.md)
#### 其他资源


[PageNumbers Object Members](http://msdn.microsoft.com/library/7f6d35df-499d-b3bf-6eaa-70e2ab1a2e8d%28Office.15%29.aspx)