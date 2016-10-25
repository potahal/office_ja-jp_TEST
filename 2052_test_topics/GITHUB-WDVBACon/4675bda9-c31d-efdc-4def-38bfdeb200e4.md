
# Application.CheckGrammar Method (Word)

Checks a string for grammatical errors. Returns a  **Boolean** to indicate whether the string contains grammatical errors. **True** if the string contains no errors.


## Syntax

 _表达式_. **CheckGrammar**( ** _String_** )

 _表达式_ Required. A variable that represents an **[Application](d1cf6f8f-4e88-bf01-93b4-90a83f79cb44.md)** object.


### Parameters



|**Name**|**Required/Optional**|**Data Type**|**Description**|
|:-----|:-----|:-----|:-----|
| _String_|必需|**String**|The string you want to check for grammatical errors.|

### Return Value

Boolean


## Example

This example displays the result of a grammar check on the selection.


```
strPass = Application.CheckGrammar(String:=Selection.Text) 
MsgBox "Selection is grammatically correct: " &amp; strPass
```


## 另请参阅


#### 概念


[Application Object](d1cf6f8f-4e88-bf01-93b4-90a83f79cb44.md)
#### 其他资源


[Application Object Members](http://msdn.microsoft.com/library/71669f1e-65f1-b0f1-b67d-355dfdbebe50%28Office.15%29.aspx)