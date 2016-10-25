
# Range.Style Property (Word)

Returns or sets the style for the specified object. Read/write  **Variant**.


## Syntax

 _表达式_. **Style**

 _表达式_ Required. A variable that represents a **[Range](15a7a1c4-5f3f-5b6e-60e9-29688de3f274.md)** object.


## Remarks

To set this property, specify the local name of the style, an integer, a  **[WdBuiltinStyle](9ef433e9-6770-0e20-e1b6-2d9929ffd616.md)** constant, or an object that represents the style. When you return the style for a range that includes more than one style, only the first character or paragraph style is returned.


## Example

This example displays the style for each character in the selection. 


 **注释**  Each element of the  **Characters** collection is a **Range** object.


```
For each c in Selection.Characters 
 MsgBox c.Style 
Next c
```


## 另请参阅


#### 概念


[Range Object](15a7a1c4-5f3f-5b6e-60e9-29688de3f274.md)
#### 其他资源


[Range Object Members](http://msdn.microsoft.com/library/3c4a36d9-2a80-5aaf-827b-275a52bfa193%28Office.15%29.aspx)