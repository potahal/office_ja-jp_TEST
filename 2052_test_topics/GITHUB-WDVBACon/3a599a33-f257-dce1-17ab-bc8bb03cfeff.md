
# OMathBreaks.Creator Property (Word)

Returns a 32-bit integer that indicates the application in which the add-in was created. Read-only  **Long**.


## Syntax

 _表达式_. **Creator**

 _表达式_ An expression that returns an **[OMathBreaks](fa01cd62-b8ad-52bf-f36a-f5d1548d3d1e.md)** object.


## Remarks

If the object was created in Microsoft Word, the  **Creator** property returns the hexadecimal number 4D535744, which represents the string "MSWD." This property was primarily designed to be used on the Macintosh, where each application has a four-character creator code. For example, Microsoft Word has the creator code MSWD. For additional information about this property, consult the language reference Help included with Microsoft Office Macintosh Edition.


 **注释**  This value can also be represented by the constant  **wdCreatorCode**.


## 另请参阅


#### 概念


[OMathBreaks Collection](fa01cd62-b8ad-52bf-f36a-f5d1548d3d1e.md)
#### 其他资源


[OMathBreaks Object Members](http://msdn.microsoft.com/library/8a16ddcf-9fdc-0cb6-b033-99fe89846a04%28Office.15%29.aspx)