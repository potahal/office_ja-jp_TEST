
# PageNumbers.ChapterPageSeparator Property (Word)

Returns or sets the separator character used between the chapter number and the page number. Read/write  **[WdSeparatorType](94cb01b0-b850-ddc6-46ce-ea0261d38247.md)**.


## Syntax

 _表达式_. **ChapterPageSeparator**

 _表达式_ An expression that represents a **[PageNumbers](9090f96e-d898-ace6-35fa-f6e59c527ea2.md)** object.


## Remarks

Before you can create page numbers that include chapter numbers, the document headings must have a numbered outline format applied that uses styles from the  **Bullets and Numbering** dialog box. To do this in Visual Basic, use the **ApplyListTemplate** method.


## Example

The first part of this example creates a new document, adds chapter titles and page breaks, and then formats the document by using the last numbered outline format listed in the  **Bullets and Numbering** dialog box. The second part of the example adds centered page numbers  including the chapter number  to the header; an en dash separates the chapter number and the page number.


```
Dim intLoop As Integer 
Dim hfTemp As HeaderFooter 
 
Documents.Add 
For intLoop = 1 To 5 
 With Selection 
 .TypeParagraph 
 .InsertBreak 
 End With 
Next intLoop 
ActiveDocument.Content.Style = wdStyleHeading1 
ActiveDocument.Content.ListFormat.ApplyListTemplate _ 
 ListTemplate:=ListGalleries(wdOutlineNumberGallery) _ 
 .ListTemplates(7) 
 
Set hfTemp = ActiveDocument.Sections(1) _ 
 .Headers(wdHeaderFooterPrimary) 
With hfTemp.PageNumbers 
 .Add PageNumberAlignment:=wdAlignPageNumberCenter 
 .NumberStyle = wdPageNumberStyleArabic 
 .IncludeChapterNumber = True 
 .HeadingLevelForChapter = 0 
 .ChapterPageSeparator = wdSeparatorEnDash 
End With
```


## 另请参阅


#### 概念


[PageNumbers Collection Object](9090f96e-d898-ace6-35fa-f6e59c527ea2.md)
#### 其他资源


[PageNumbers Object Members](http://msdn.microsoft.com/library/7f6d35df-499d-b3bf-6eaa-70e2ab1a2e8d%28Office.15%29.aspx)