
# HeadersFooters.Creator Property (Word)

Returns a 32-bit integer that indicates the application in which the specified object was created. Read-only  **Long**.


## Syntax

 _表达式_. **Creator**

 _表达式_ Required. A variable that represents a **[HeadersFooters](41dbbaa7-f139-3d3c-54d4-03a57ab8417a.md)** collection.


## Remarks

If the object was created in Microsoft Word, the  **Creator** property returns the hexadecimal number 4D535744, which represents the string "MSWD." This property was primarily designed to be used on the Macintosh, where each application has a four-character creator code. For example, Microsoft Word has the creator code MSWD. For additional information about this property, consult the language reference Help included with Microsoft Office Macintosh Edition.


## 另请参阅


#### 概念


[HeadersFooters Collection Object](41dbbaa7-f139-3d3c-54d4-03a57ab8417a.md)
#### 其他资源


[HeadersFooters Object Members](http://msdn.microsoft.com/library/6cf7f768-d356-7ff4-089c-7f3f810d00a8%28Office.15%29.aspx)