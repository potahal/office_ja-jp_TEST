
# Dialog.Execute Method (Word)

Applies the current settings of a Microsoft Word dialog box.


## Syntax

 _表达式_. **Execute**

 _表达式_ Required. A variable that represents a **[Dialog](f90f6e6d-aaa0-c127-ab37-ca074144eff1.md)** object.


## Example

The following example enables the  **Keep with next** check box on the **Line and Page Breaks** tab in the **Paragraph** dialog box.


```
With Dialogs(wdDialogFormatParagraph) 
 .KeepWithNext = 1 
 .Execute 
End With
```


## 另请参阅


#### 概念


[Dialog Object](f90f6e6d-aaa0-c127-ab37-ca074144eff1.md)
#### 其他资源


[Dialog Object Members](http://msdn.microsoft.com/library/f5c755d5-9fdf-bfb4-2c4b-8999ae176635%28Office.15%29.aspx)