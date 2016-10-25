
# Borders.InsideLineWidth Property (Word)

Returns or sets the line width of the inside border of an object. .


## Syntax

 _表达式_. **InsideLineWidth**

 _表达式_ Required. A variable that represents a **[Borders](6dd1d4cc-2dcf-22c7-a299-4721a5543ba3.md)** collection.


## Remarks

This property returns  **wdUndefined** if the object has inside borders with more than one line width; otherwise, returns **False** or a **WdLineWidth** constant. Can be set to **True**, **False**, or one of the following **WdLineWidth** constants.


## Example

This example adds borders between rows and between columns in the first table in the active document.


```
Dim tableTemp As Table 
 
If ActiveDocument.Tables.Count >= 1 Then 
 Set tableTemp = ActiveDocument.Tables(1) 
 tableTemp.Borders.InsideLineStyle = wdLineStyleDot 
 tableTemp.Borders.InsideLineWidth = wdLineWidth050pt 
End If
```

This example adds dotted borders between the first four paragraphs of the active document.




```
Dim docActive As Document 
Dim rngTemp As Range 
 
Set docActive = ActiveDocument 
Set rngTemp=docActive.Range( _ 
 Start:=docActive.Paragraphs(1).Range.Start, _ 
 End:=docActive.Paragraphs(4).Range.End) 
 
rngTemp.Borders.InsideLineStyle = wdLineStyleDot 
rngTemp.Borders.InsideLineWidth = wdLineWidth075pt
```


## 另请参阅


#### 概念


[Borders Collection Object](6dd1d4cc-2dcf-22c7-a299-4721a5543ba3.md)
#### 其他资源


[Borders Object Members](http://msdn.microsoft.com/library/7c391c32-ebf4-9ca7-a740-0205852f1bab%28Office.15%29.aspx)