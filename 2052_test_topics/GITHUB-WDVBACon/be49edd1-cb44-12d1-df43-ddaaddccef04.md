
# Options.AutoFormatAsYouTypeAutoLetterWizard Property (Word)

 **True** for Microsoft Word to automatically start the Letter Wizard when the user enters a letter salutation or closing. Read/write.


## Syntax

 _表达式_. **AutoFormatAsYouTypeAutoLetterWizard**

 _表达式_ Required. A variable that represents an **[Options](873b7b99-3fe1-fd89-9ece-a9355cb827dc.md)** collection.


## Example

This example sets Microsoft Word to automatically start the Letter Wizard when the user enters a letter salutation or closing.


```
Sub AutoLeterWizard() 
 Options.AutoFormatAsYouTypeAutoLetterWizard = True 
End Sub
```


## 另请参阅


#### 概念


[Options Object](873b7b99-3fe1-fd89-9ece-a9355cb827dc.md)
#### 其他资源


[Options Object Members](http://msdn.microsoft.com/library/76cd9dfe-6bbb-4c3d-0bfc-79a62bedd15e%28Office.15%29.aspx)