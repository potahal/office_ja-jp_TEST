
# Application.Dialogs Property (Word)

Returns a  **[Dialogs](8dfa5d8a-bb81-1cdd-853b-3acf9db70aa9.md)** collection that represents all the built-in dialog boxes in Word.Read-only.


## Syntax

 _表达式_. **Dialogs**

 _表达式_ A variable that represents an **[Application](d1cf6f8f-4e88-bf01-93b4-90a83f79cb44.md)** object.


## Remarks

For information about returning a single member of a collection, see [Returning an Object from a Collection](28f76384-f495-9640-a7c8-10ada3fac727.md).


## Example

This example displays the built-in  **Find** dialog box, with "Hello" in the **Find What** box.


```
Dim dlgFind As Dialog 
 
Set dlgFind = Dialogs(wdDialogEditFind) 
 
With dlgFind 
 .Find = "Hello" 
 .Show 
End With
```

This example displays the built-in  **Open** dialog box showing all file types.




```
With Dialogs(wdDialogFileOpen) 
 .Name = "*.*" 
 .Show 
End With
```

This example prints the active document, using the settings from the  **Print** dialog box.




```
Dialogs(wdDialogFilePrint).Execute
```


## 另请参阅


#### 概念


[Application Object](d1cf6f8f-4e88-bf01-93b4-90a83f79cb44.md)
#### 其他资源


[Application Object Members](http://msdn.microsoft.com/library/71669f1e-65f1-b0f1-b67d-355dfdbebe50%28Office.15%29.aspx)