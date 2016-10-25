
# Templates.Creator Property (Word)

Returns a 32-bit integer that indicates the application in which the specified object was created. Read-only  **Long**.


## Syntax

 _表达式_. **Creator**

 _表达式_ Required. A variable that represents a **[Templates](de62f768-011a-7446-48c3-1c4512da5f7c.md)** collection.


## Remarks

If the object was created in Microsoft Word, the  **Creator** property returns the hexadecimal number 4D535744, which represents the string "MSWD." This property was primarily designed to be used on the Macintosh, where each application has a four-character creator code. For example, Microsoft Word has the creator code MSWD. For additional information about this property, consult the language reference Help included with Microsoft Office Macintosh Edition.


## 另请参阅


#### 概念


[Templates Collection Object](de62f768-011a-7446-48c3-1c4512da5f7c.md)
#### 其他资源


[Templates Object Members](http://msdn.microsoft.com/library/80f2732a-9341-fb5a-1fb8-de3c6555cb92%28Office.15%29.aspx)