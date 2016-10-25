
# Hyperlink.Creator Property (Word)

Returns a 32-bit integer that indicates the application in which the specified object was created. Read-only  **Long**.


## Syntax

 _表达式_. **Creator**

 _表达式_ Required. A variable that represents a **[Hyperlink](af785a9e-081a-e359-705f-04f490304e2e.md)** object.


## Remarks

If the object was created in Microsoft Word, the  **Creator** property returns the hexadecimal number 4D535744, which represents the string "MSWD." This property was primarily designed to be used on the Macintosh, where each application has a four-character creator code. For example, Microsoft Word has the creator code MSWD. For additional information about this property, consult the language reference Help included with Microsoft Office Macintosh Edition.


## 另请参阅


#### 概念


[Hyperlink Object](af785a9e-081a-e359-705f-04f490304e2e.md)
#### 其他资源


[Hyperlink Object Members](http://msdn.microsoft.com/library/49699791-6b9c-2061-aff7-c9269747ecea%28Office.15%29.aspx)