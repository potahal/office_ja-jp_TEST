
# UpBars.Creator Property (Word)

Returns a 32-bit integer that indicates the application in which the specified object was created. Read-only  **Long**.


## Syntax

 _表达式_. **Creator**

 _表达式_ A variable that represents an **[UpBars](22dff1d2-8f1b-8c48-354c-570906e0f830.md)** object.


## Remarks

If the object was created in Microsoft Word, the  **Creator** property returns the hexadecimal number 4D535744, which represents the string "MSWD". This property was primarily designed to be used on the Macintosh, where each application has a four-character creator code. For example, Word has the creator code MSWD. For more information about this property, consult the language reference Help included with Microsoft Office for Mac.


## 另请参阅


#### 概念


[UpBars Object](22dff1d2-8f1b-8c48-354c-570906e0f830.md)
#### 其他资源


[UpBars Object Members](http://msdn.microsoft.com/library/7772742e-1230-6987-f8f3-f3663ea4329b%28Office.15%29.aspx)