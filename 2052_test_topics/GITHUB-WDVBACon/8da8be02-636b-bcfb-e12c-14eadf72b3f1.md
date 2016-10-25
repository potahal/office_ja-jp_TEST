
# Document.PasswordEncryptionFileProperties Property (Word)

 **True** if Microsoft Word encrypts file properties for password-protected documents. Read-only **Boolean**.


## Syntax

 _表达式_. **PasswordEncryptionFileProperties**

 _表达式_ A variable that represents a **[Document](8d83487a-2345-a036-a916-971c9db5b7fb.md)** object.


## Remarks

Use the  **[SetPasswordEncryptionOptions](4e7c2c0a-cac2-6fa3-f237-f02c897757a1.md)** method to specify whether Word encrypts file properties for password-protected documents.


## Example

This example sets the password encryption options if the file properties are not encrypted for password-protected documents.


```
Sub PasswordSettings() 
 With ActiveDocument 
 If .PasswordEncryptionFileProperties = False Then 
 .SetPasswordEncryptionOptions _ 
 PasswordEncryptionProvider:="Microsoft RSA SChannel Cryptographic Provider", _ 
 PasswordEncryptionAlgorithm:="RC4", _ 
 PasswordEncryptionKeyLength:=56, _ 
 PasswordEncryptionFileProperties:=True 
 End If 
 End With 
End Sub
```


## 另请参阅


#### 概念


[Document Object](8d83487a-2345-a036-a916-971c9db5b7fb.md)
#### 其他资源


[Document Object Members](http://msdn.microsoft.com/library/fc9ab457-0888-f917-3d52-387168ac23b9%28Office.15%29.aspx)