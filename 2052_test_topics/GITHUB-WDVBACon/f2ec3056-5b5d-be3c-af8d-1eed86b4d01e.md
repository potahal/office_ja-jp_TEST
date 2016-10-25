
# Font.Outline Property (Word)

 **True** if the font is formatted as outline. Read/write **Long**.


## Syntax

 _表达式_. **Outline**

 _表达式_ An expression that returns a **[Font](bc97f4df-fc81-d6c8-e99a-d50dc793b7ae.md)** object.


## Remarks

Returns  **True**, **False**, or **wdUndefined** (a mixture of **True** and **False** ). Can be set to **True**, **False**, or **wdToggle**.


## Example

This example applies outline font formatting to the first three words in the active document.


```
Set myRange = ActiveDocument.Range(Start:= _ 
 ActiveDocument.Words(1).Start, _ 
 End:=ActiveDocument.Words(3).End) 
myRange.Font.Outline = True
```

This example toggles outline formatting for the selected text.




```
Selection.Font.Outline = wdToggle
```

This example removes outline font formatting from the selection if outline formatting is partially applied to the selection.




```
Set myFont = Selection.Font 
If myFont.Outline = wdUndefined Then 
 myFont.Outline = False 
End If
```


## 另请参阅


#### 概念


[Font Object](bc97f4df-fc81-d6c8-e99a-d50dc793b7ae.md)
#### 其他资源


[Font Object Members](http://msdn.microsoft.com/library/04a3c706-4062-09bc-70d9-cef3748a7d57%28Office.15%29.aspx)