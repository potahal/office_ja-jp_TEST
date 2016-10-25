
# Font.ColorIndexBi Property (Word)

Returns or sets the color for the specified  **Font** object in a right-to-left language document. Read/write **WdColorIndex**.


## Syntax

 _表达式_. **ColorIndexBi**

 _表达式_ Required. A variable that represents a **[Font](bc97f4df-fc81-d6c8-e99a-d50dc793b7ae.md)** object.


## Remarks

The  **wdByAuthor** constant is not valid for **Font** objects.


## Example

This example sets the color of the  **Font** object to teal.


```
Selection.Font.ColorIndexBi = wdTeal
```


## 另请参阅


#### 概念


[Font Object](bc97f4df-fc81-d6c8-e99a-d50dc793b7ae.md)
#### 其他资源


[Font Object Members](http://msdn.microsoft.com/library/04a3c706-4062-09bc-70d9-cef3748a7d57%28Office.15%29.aspx)