
# TablesOfContents.Creator Property (Word)

Returns a 32-bit integer that indicates the application in which the specified object was created. Read-only  **Long**.


## Syntax

 _表达式_. **Creator**

 _表达式_ Required. A variable that represents a **[TablesOfContents](d0d0e5fc-e443-31ae-e1a9-15b945f1e318.md)** collection.


## Remarks

If the object was created in Microsoft Word, the  **Creator** property returns the hexadecimal number 4D535744, which represents the string "MSWD." This property was primarily designed to be used on the Macintosh, where each application has a four-character creator code. For example, Microsoft Word has the creator code MSWD. For additional information about this property, consult the language reference Help included with Microsoft Office Macintosh Edition.


## 另请参阅


#### 概念


[TablesOfContents Collection Object](d0d0e5fc-e443-31ae-e1a9-15b945f1e318.md)
#### 其他资源


[TablesOfContents Object Members](http://msdn.microsoft.com/library/9ed06355-0ac4-ee9c-8673-474d454a1db2%28Office.15%29.aspx)