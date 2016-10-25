
# Task.Creator Property (Word)

Returns a 32-bit integer that indicates the application in which the specified object was created. Read-only  **Long**.


## Syntax

 _表达式_. **Creator**

 _表达式_ Required. A variable that represents a **[Task](8802fcd5-0947-2ea0-308a-376077633e34.md)** object.


## Remarks

If the object was created in Microsoft Word, the  **Creator** property returns the hexadecimal number 4D535744, which represents the string "MSWD." This property was primarily designed to be used on the Macintosh, where each application has a four-character creator code. For example, Microsoft Word has the creator code MSWD. For additional information about this property, consult the language reference Help included with Microsoft Office Macintosh Edition.


## 另请参阅


#### 概念


[Task Object](8802fcd5-0947-2ea0-308a-376077633e34.md)
#### 其他资源


[Task Object Members](http://msdn.microsoft.com/library/0697f813-7087-e031-9ad0-a11a0969c201%28Office.15%29.aspx)