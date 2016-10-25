
# Range.TextVisibleOnScreen Property (Word)

Returns a  **Long** that indicates whether the text in the specified range is visible on the screen. Read-only.


## Syntax

 _表达式_. **TextVisibleOnScreen**

 _表达式_ A variable that represents a **Range** object.


## Remarks

The  **TextVisibleOnScreen** property returns 1 if all text in the range is visible; it returns 0 if no text in the range is visible; and it returns -1 if some text in the range is visible and some is not. Text that is not visible could be, for example, text that is in a collapsed heading.


## Property value

 **INT32**


## 另请参阅


#### 概念


[Range Object](15a7a1c4-5f3f-5b6e-60e9-29688de3f274.md)
#### 其他资源


[Range Members](http://msdn.microsoft.com/library/3c4a36d9-2a80-5aaf-827b-275a52bfa193%28Office.15%29.aspx)