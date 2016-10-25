
# WorksheetFunction.Replace Method (Excel)

Replaces part of a text string, based on the number of characters you specify, with a different text string.


## Syntax

 _表达式_. **Replace**( ** _Arg1_**, ** _Arg2_**, ** _Arg3_**, ** _Arg4_** )

 _表达式_ A variable that represents a **WorksheetFunction** object.


### Parameters



|**Name**|**Required/Optional**|**Data Type**|**Description**|
|:-----|:-----|:-----|:-----|
| _Arg1_|必需|**String**|Text in which you want to replace some characters.|
| _Arg2_|必需|**Double**|The position of the character in  **Arg1** that you want to replace with **Arg4**.|
| _Arg3_|必需|**Double**|The number of characters in  **Arg1** that you want the **Replace** method to replace with **Arg4**.|
| _Arg4_|必需|**String**|Text that will replace characters in  **Arg1**.|

### Return Value

A String value that represents the new string, after replacement.


## Example

This example replaces abcdef with ac-ef and notifies the user during this process.


```
Sub UseReplace() 
 
 Dim strCurrent As String 
 Dim strReplaced As String 
 
 strCurrent = "abcdef" 
 
 ' Notify user and display current string. 
 MsgBox "The current string is: " &amp; strCurrent 
 
 ' Replace "cd" with "-". 
 strReplaced = Application.WorksheetFunction.Replace _ 
 (Arg1:=strCurrent, Arg2:=3, _ 
 Arg3:=2, Arg4:="-") 
 
 ' Notify user and display replaced string. 
 MsgBox "The replaced string is: " &amp; strReplaced 
 
End Sub
```


## 另请参阅


#### 概念


[WorksheetFunction Object](7b1d5639-363d-632c-2cf0-2232562646b6.md)
#### 其他资源


[WorksheetFunction Object Members](http://msdn.microsoft.com/library/6811ca87-4b53-0bff-88c9-30bf7497879a%28Office.15%29.aspx)