
# Application.NewDocument Property (Word)

Returns a  **NewFile** object that represents a document listed on the **New** tab.


## Syntax

 _表达式_. **NewDocument**

 _表达式_ A variable that represents an **[Application](d1cf6f8f-4e88-bf01-93b4-90a83f79cb44.md)** object.


## Example

This example creates a document list item on the New Document task pane in the New From Existing File section.


```
Sub CreateNewDocument() 
 Application.NewDocument.Add FileName:="C:\NewFile.doc", _ 
 Section:=msoNewfromExistingFile, DisplayName:="New File", _ 
 Action:=msoCreateNewFile 
End Sub
```


## 另请参阅


#### 概念


[Application Object](d1cf6f8f-4e88-bf01-93b4-90a83f79cb44.md)
#### 其他资源


[Application Object Members](http://msdn.microsoft.com/library/71669f1e-65f1-b0f1-b67d-355dfdbebe50%28Office.15%29.aspx)