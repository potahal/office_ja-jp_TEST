
# Comments.Add Method (Word)

Returns a  **Comment** object that represents a comment added to a range.


## Syntax

 _表达式_. **Add**( ** _Range_**, ** _Text_** )

 _表达式_ Required. A variable that represents a **[Comments](e384b37a-50e3-a214-52a8-6fda2acc4991.md)** collection.


### Parameters



|**Name**|**Required/Optional**|**Data Type**|**Description**|
|:-----|:-----|:-----|:-----|
| _Range_|必需|**Range object**|The range to have a comment added to it.|
| _Text_|可选|**Variant**|The text of the comment.|

### Return Value

Comment


## Example

This example adds a comment at the insertion point.


```
Sub AddComment() 
 Selection.Collapse Direction:=wdCollapseEnd 
 ActiveDocument.Comments.Add _ 
 Range:=Selection.Range, Text:="review this" 
End Sub
```

This example adds a comment to the third paragraph in the active document.




```
Sub Comment3rd() 
 Dim myRange As Range 
 
 Set myRange = ActiveDocument.Paragraphs(3).Range 
 ActiveDocument.Comments.Add Range:=myRange, _ 
 Text:="original third paragraph" 
End Sub
```


## 另请参阅


#### 概念


[Comments Collection Object](e384b37a-50e3-a214-52a8-6fda2acc4991.md)
#### 其他资源


[Comments Object Members](http://msdn.microsoft.com/library/2cd992bf-9e18-7f0e-3e8b-b3507ffd9bc7%28Office.15%29.aspx)