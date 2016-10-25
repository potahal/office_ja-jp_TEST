
# Editors.Creator Property (Word)

Returns a 32-bit integer that indicates the application in which the specified object was created. Read-only  **Long**.


## Syntax

 _表达式_. **Creator**

 _表达式_ Required. A variable that represents an **[Editors](acce718a-e3c1-deac-8b7f-fd8a5a9e47c6.md)** collection.


## Remarks

If the object was created in Microsoft Word, the  **Creator** property returns the hexadecimal number 4D535744, which represents the string "MSWD." This property was primarily designed to be used on the Macintosh, where each application has a four-character creator code. For example, Microsoft Word has the creator code MSWD. For additional information about this property, consult the language reference Help included with Microsoft Office Macintosh Edition.


## 另请参阅


#### 概念


[Editors Collection](acce718a-e3c1-deac-8b7f-fd8a5a9e47c6.md)
#### 其他资源


[Editors Object Members](http://msdn.microsoft.com/library/dcb26f83-bbff-8d3a-2493-f7d87ce40d21%28Office.15%29.aspx)