
# ChartTitle.Creator Property (Word)

Returns a 32-bit integer that indicates the application in which the specified object was created. Read-only  **Long**.


## Syntax

 _表达式_. **Creator**

 _表达式_ A variable that represents a **[ChartTitle](fc8ca540-0a29-123b-2fdf-b16aaa1f940c.md)** object.


## Remarks

If the object was created in Microsoft Word, the  **Creator** property returns the hexadecimal number 4D535744, which represents the string "MSWD". This property was primarily designed to be used on the Macintosh, where each application has a four-character creator code. For example, Word has the creator code MSWD. For more information about this property, consult the language reference Help included with Microsoft Office for Mac.


## 另请参阅


#### 概念


[ChartTitle Object](fc8ca540-0a29-123b-2fdf-b16aaa1f940c.md)
#### 其他资源


[ChartTitle Object Members](http://msdn.microsoft.com/library/e85a7f56-06f4-0561-a37b-7444115965fa%28Office.15%29.aspx)