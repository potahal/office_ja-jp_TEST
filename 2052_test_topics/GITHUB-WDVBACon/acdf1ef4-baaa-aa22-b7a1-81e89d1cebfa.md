
# PageSetup.LineNumbering Property (Word)

Returns or sets a  **[LineNumbering](a2dd1278-c7dd-af4c-be32-1daded5556d6.md)** object that represents the line numbers for the specified **PageSetup** object.


## Syntax

 _表达式_. **LineNumbering**

 _表达式_ An expression that returns a **[PageSetup](1879d601-80ad-4fc0-1a87-92e999b59f88.md)** object.


## Remarks

You must be in print layout view to see line numbering.


## Example

This example enables line numbering for the active document.


```
ActiveDocument.PageSetup.LineNumbering.Active = True
```

This example enables line numbering for a document named "MyDocument.doc" The starting number is set to one, every fifth line number is shown, and the numbering is continuous throughout all sections in the document.




```
set myDoc = Documents("MyDocument.doc") 
With myDoc.PageSetup.LineNumbering 
 .Active = True 
 .StartingNumber = 1 
 .CountBy = 5 
 .RestartMode = wdRestartContinuous 
End With
```

This example sets the line numbering in the active document equal to the line numbering in MyDocument.doc.




```
ActiveDocument.PageSetup.LineNumbering = Documents("MyDocument.doc") _ 
 .PageSetup.LineNumbering
```


## 另请参阅


#### 概念


[PageSetup Object](1879d601-80ad-4fc0-1a87-92e999b59f88.md)
#### 其他资源


[PageSetup Object Members](http://msdn.microsoft.com/library/9ff8b896-933b-1a19-19d5-5e5d87aab1b5%28Office.15%29.aspx)