
# Workbook.SetPasswordEncryptionOptions Method (Excel)

Sets the options for encrypting workbooks using passwords.


## Syntax

 _表达式_. **SetPasswordEncryptionOptions**( ** _PasswordEncryptionProvider_**, ** _PasswordEncryptionAlgorithm_**, ** _PasswordEncryptionKeyLength_**, ** _PasswordEncryptionFileProperties_** )

 _表达式_ A variable that represents a **Workbook** object.


### Parameters



|**Name**|**Required/Optional**|**Data Type**|**Description**|
|:-----|:-----|:-----|:-----|
| _PasswordEncryptionProvider_|可选|**Variant**|A case sensitive string of the encryption provider.|
| _PasswordEncryptionAlgorithm_|可选|**Variant**|A case sensitive string of the algorithmic short name (i.e. "RC4").|
| _PasswordEncryptionKeyLength_|可选|**Variant**|The encryption key length which is a multiple of 8 (40 or greater).|
| _PasswordEncryptionFileProperties_|可选|**Variant**|**True** (default) to encrypt file properties.|

## Remarks

The  _PasswordEncryptionProvider_,  _PasswordEncryptionAlgorithm_, and  _PasswordEncryptionKeyLength_ arguments are not independent of each other. A selected encryption provider limits the set of algorithms and key length that can be chosen.

For the  _PasswordEncryptionKeyLength_ argument there is no inherent limit on the range of the key length. The range is determined by the Cryptographic Service Provider which also determines the cryptographic algorithm.


## Example

This example sets the password encryption options for the active workbook.


```
Sub SetPasswordOptions() 
 
 ActiveWorkbook.SetPasswordEncryptionOptions _ 
 PasswordEncryptionProvider:="Microsoft RSA SChannel Cryptographic Provider", _ 
 PasswordEncryptionAlgorithm:="RC4", _ 
 PasswordEncryptionKeyLength:=56, _ 
 PasswordEncryptionFileProperties:=True 
 
End Sub
```


 **注释**  The code and this method do not do anything for the new Excel file formats (xlsx, xlsb, xlsm, etc...) as the workbook will always use AES 128 bit encryption. If a property is set using this method, it will appear set. When the file is reloaded, the properties will be reset to the AES setting.


## 另请参阅


#### 概念


[Workbook Object](8c00aa60-c974-eed3-0812-3c9625eb0d4c.md)
#### 其他资源


[Workbook Object Members](http://msdn.microsoft.com/library/dce102a3-25de-3ff4-2ce5-bc56e08baca7%28Office.15%29.aspx)