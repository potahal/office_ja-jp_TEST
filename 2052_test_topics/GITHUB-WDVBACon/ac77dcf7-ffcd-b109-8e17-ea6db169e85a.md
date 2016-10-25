
# Range.InsertBefore Method (Word)

Inserts the specified text before the specified range.


## Syntax

 _表达式_. **InsertBefore**( ** _Text_** )

 _表达式_ Required. A variable that represents a **[Range](15a7a1c4-5f3f-5b6e-60e9-29688de3f274.md)** object.


### Parameters



|**Name**|**Required/Optional**|**Data Type**|**Description**|
|:-----|:-----|:-----|:-----|
| _Text_|必需|**String**|The text to be inserted.|

## Remarks

After the text is inserted, the range is expanded to include the new text. If the range is a bookmark, the bookmark is also expanded to include the next text.

You can insert characters such as quotation marks, tab characters, and nonbreaking hyphens by using the Visual Basic  **Chr** function with the **InsertBefore** method. You can also use the following Visual Basic constants: **vbCr**, **vbLf**, **vbCrLf** and **vbTab**.


## Example

This example inserts the text "Introduction" as a separate paragraph at the beginning of the active document.


```
With ActiveDocument.Content 
 .InsertParagraphBefore 
 .InsertBefore "Introduction" 
End With
```


## 另请参阅


#### 概念


[Range Object](15a7a1c4-5f3f-5b6e-60e9-29688de3f274.md)
#### 其他资源


[Range Object Members](http://msdn.microsoft.com/library/3c4a36d9-2a80-5aaf-827b-275a52bfa193%28Office.15%29.aspx)