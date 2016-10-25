
# Range.Creator Property (Word)

Returns a 32-bit integer that indicates the application in which the specified object was created. Read-only  **Long**.


## Syntax

 _表达式_. **Creator**

 _表达式_ Required. A variable that represents a **[Range](15a7a1c4-5f3f-5b6e-60e9-29688de3f274.md)** object.


## Remarks

If the object was created in Microsoft Word, the  **Creator** property returns the hexadecimal number 4D535744, which represents the string "MSWD." This property was primarily designed to be used on the Macintosh, where each application has a four-character creator code. For example, Microsoft Word has the creator code MSWD. For additional information about this property, consult the language reference Help included with Microsoft Office Macintosh Edition.


## 另请参阅


#### 概念


[Range Object](15a7a1c4-5f3f-5b6e-60e9-29688de3f274.md)
#### 其他资源


[Range Object Members](http://msdn.microsoft.com/library/3c4a36d9-2a80-5aaf-827b-275a52bfa193%28Office.15%29.aspx)