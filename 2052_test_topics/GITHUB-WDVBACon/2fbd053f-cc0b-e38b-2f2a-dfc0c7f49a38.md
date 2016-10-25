
# EmailOptions.AutoFormatAsYouTypeReplacePlainTextEmphasis Property (Word)

 **True** if manual emphasis characters are automatically replaced with character formatting as you type; for example, "*bold*" is changed to " **bold** ". Read/write **Boolean**.


## Syntax

 _表达式_. **AutoFormatAsYouTypeReplacePlainTextEmphasis**

 _表达式_ A variable that represents an **[EmailOptions](41fefa03-c993-e218-0f92-0cf30c0bfbd4.md)** collection.


## Example

This example turns on the replacement of manual emphasis characters with character formatting.


```
Options.AutoFormatAsYouTypeReplacePlainTextEmphasis = True
```

This example returns the status of the  ***Bold* and _underline_ with real formatting** option on the **AutoFormat As You Type** tab in the **AutoCorrect** dialog box ( **Tools** menu).




```
Dim blnAutoFormat as Boolean 
 
blnAutoFormat = _ 
 Options.AutoFormatAsYouTypeReplacePlainTextEmphasis
```


## 另请参阅


#### 概念


[EmailOptions Object](41fefa03-c993-e218-0f92-0cf30c0bfbd4.md)
#### 其他资源


[EmailOptions Object Members](http://msdn.microsoft.com/library/0f8a549b-283c-dc9d-dc1e-1179a9d6fb0b%28Office.15%29.aspx)