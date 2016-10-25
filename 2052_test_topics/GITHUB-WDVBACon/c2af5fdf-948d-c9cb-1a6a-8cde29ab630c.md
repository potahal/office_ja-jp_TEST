
# Tasks.ExitWindows Method (Word)

Closes all open applications, quits Microsoft Windows, and logs the current user off.


## Syntax

 _表达式_. **ExitWindows**

 _表达式_ Required. A variable that represents a **[Tasks](ff521e20-8a25-f9f6-dccf-effea9debeb7.md)** collection.


## Remarks

This method does not save changes to open Microsoft Word documents; however, it does prompt you to save changes to open documents in other Windows-based applications.


## Example

This example saves all open Word documents, closes Word, and then quits Microsoft Windows.


```
Documents.Save NoPrompt:=True, _ 
 OriginalFormat:=wdOriginalDocumentFormat 
Tasks.ExitWindows
```


## 另请参阅


#### 概念


[Tasks Collection Object](ff521e20-8a25-f9f6-dccf-effea9debeb7.md)
#### 其他资源


[Tasks Object Members](http://msdn.microsoft.com/library/e6ca78c6-132d-6e7b-9f83-ea044a395040%28Office.15%29.aspx)