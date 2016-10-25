
# Worksheet.Comments Property (Excel)

Returns a  **[Comments](f43bf021-1e46-10cf-09bf-070fc6a2c81a.md)** collection that represents all the comments for the specified worksheet. Read-only.


## Syntax

 _表达式_. **Comments**

 _表达式_ A variable that represents a **Worksheet** object.


## Example

This example deletes all comments added by Jean Selva on the active sheet.


```
For Each c in ActiveSheet.Comments 
 If c.Author = "Jean Selva" Then c.Delete 
Next
```


## 另请参阅


#### 概念


[Worksheet Object](182b705e-854a-81cc-a4b0-59b942de55ae.md)
#### 其他资源


[Worksheet Object Members](http://msdn.microsoft.com/library/f8c1afea-1a1c-f5e4-37e3-52c434c8c157%28Office.15%29.aspx)