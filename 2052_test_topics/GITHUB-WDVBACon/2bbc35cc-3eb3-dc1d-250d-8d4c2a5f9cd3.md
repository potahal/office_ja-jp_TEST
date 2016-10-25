
# AutoCorrect.CorrectCapsLock Property (Word)

 **True** if Word automatically corrects instances in which you use the CAPS LOCK key inadvertently as you type. Read/write **Boolean**.


## Syntax

 _表达式_. **CorrectCapsLock**

 _表达式_ A variable that represents an **[AutoCorrect](dea9b72c-4378-05ac-ec4b-51cf3af3f2a3.md)** object.


## Example

This example determines whether Word is set to automatically correct CAPS LOCK key errors.


```
If AutoCorrect.CorrectCapsLock = True Then 
 MsgBox "Correct CAPS LOCK is active." 
Else 
 MsgBox "Correct CAPS LOCK is not active." 
End If
```


## 另请参阅


#### 概念


[AutoCorrect Object](dea9b72c-4378-05ac-ec4b-51cf3af3f2a3.md)
#### 其他资源


[AutoCorrect Object Members](http://msdn.microsoft.com/library/cc5f42d4-6689-221f-5ad2-3b56f3b2c42f%28Office.15%29.aspx)