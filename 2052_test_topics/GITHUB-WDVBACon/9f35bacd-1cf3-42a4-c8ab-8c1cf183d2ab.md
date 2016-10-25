
# Range.InsertFile Method (Word)

Inserts all or part of the specified file.


## Syntax

 _表达式_. **InsertFile**( ** _FileName_**, ** _Range_**, ** _ConfirmConversions_**, ** _Link_**, ** _Attachment_** )

 _表达式_ Required. A variable that represents a **[Range](15a7a1c4-5f3f-5b6e-60e9-29688de3f274.md)** object.


### Parameters



|**Name**|**Required/Optional**|**Data Type**|**Description**|
|:-----|:-----|:-----|:-----|
| _FileName_|必需|**String**|The path and file name of the file to be inserted. If you don't specify a path, Word assumes the file is in the current folder.|
| _Range_|可选|**Variant**|If the specified file is a Word document, this parameter refers to a bookmark. If the file is another type (for example, a Microsoft Excel worksheet), this parameter refers to a named range or a cell range (for example, R1C1:R3C4).|
| _ConfirmConversions_|可选|**Variant**|**True** to have Word prompt you to confirm conversion when inserting files in formats other than the Word Document format.|
| _Link_|可选|**Variant**|**True** to insert the file by using an INCLUDETEXT field.|
| _Attachment_|可选|**Variant**|**True** to insert the file as an attachment to an e-mail message.|

## Example

This example uses an INCLUDETEXT field to insert the TEST.DOC file at the end of the current document.


```
ActiveDocument.Range.Collapse Direction:=wdCollapseEnd 
ActiveDocument.Range.InsertFile FileName:="C:\TEST.DOC", Link:=True
```


## 另请参阅


#### 概念


[Range Object](15a7a1c4-5f3f-5b6e-60e9-29688de3f274.md)
#### 其他资源


[Range Object Members](http://msdn.microsoft.com/library/3c4a36d9-2a80-5aaf-827b-275a52bfa193%28Office.15%29.aspx)