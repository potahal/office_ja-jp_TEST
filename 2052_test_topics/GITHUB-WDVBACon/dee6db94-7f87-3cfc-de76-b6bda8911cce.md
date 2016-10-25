
# Style.BuiltIn Property (Word)

 **True** if the specified object is one of the built-in styles or caption labels in Word. Read-only **Boolean**.


## Syntax

 _表达式_. **BuiltIn**

 _表达式_ A variable that represents a **[Style](473f8f41-2cba-769e-c0da-441d9d85b009.md)** object.


## Remarks

You can specify built-in styles across all languages by using the  **WdBuiltinStyle** constants or within a language by using the style name for the language version of Word. For example, if you specify U.S. English in your Microsoft Office language settings, the following statements are equivalent:


```
ActiveDocument.Styles(wdStyleHeading1) 
ActiveDocument.Styles("Heading 1")
```


## Example

This example checks all the styles in the active document. When it finds a style that isn't built in, it displays the name of the style.


```
Dim styleLoop As Style 
 
For Each styleLoop in ActiveDocument.Styles 
 If styleLoop.BuiltIn = False Then 
 Msgbox styleLoop.NameLocal 
 End If 
Next styleLoop
```


## 另请参阅


#### 概念


[Style Object](473f8f41-2cba-769e-c0da-441d9d85b009.md)
#### 其他资源


[Style Object Members](http://msdn.microsoft.com/library/37c68e72-c745-bc9c-1547-0cf177cbdef4%28Office.15%29.aspx)