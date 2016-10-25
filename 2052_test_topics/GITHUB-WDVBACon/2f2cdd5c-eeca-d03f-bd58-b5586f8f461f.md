
# Document.SendForReview Method (Word)

Sends a document in an e-mail message for review by the specified recipients.


## Syntax

 _表达式_. **SendForReview**( ** _Recipients_**, ** _Subject_**, ** _ShowMessage_**, ** _IncludeAttachment_** )

 _表达式_ Required. A variable that represents a **[Document](8d83487a-2345-a036-a916-971c9db5b7fb.md)** object.


### Parameters



|**Name**|**Required/Optional**|**Data Type**|**Description**|
|:-----|:-----|:-----|:-----|
| _Recipients_|可选|**Variant**|A string that lists the people to whom to send the message. These can be unresolved names and aliases in an e-mail phone book or full e-mail addresses. Separate multiple recipients with a semicolon (;). If left blank and ShowMessage is  **False**, you will receive an error message and the message will not be sent.|
| _Subject_|可选|**Variant**|A string for the subject of the message. If left blank, the subject will be: Please review "file name".|
| _ShowMessage_|可选|**Variant**|A  **Boolean** value that indicates whether the message should be displayed when the method is executed. The default value is **True**. If set to **False**, the message is automatically sent to the recipients without first showing the message to the sender.|
| _IncludeAttachment_|可选|**Variant**|A  **Boolean** value that indicates whether the message should include an attachment or a link to a server location. The default value is **True**. If set to **False**, the document must be stored at a shared location.|

## Remarks

The  **SendForReview** method starts a collaborative review cycle. Use the **EndReview** method to end a review cycle.


## Example

This example automatically sends the current document as an attachment in an e-mail message to the specified recipients.


```
Sub WebReview() 
 ActiveDocument.SendForReview _ 
 Recipients:="someone@example.com; amy jones", _ 
 Subject:="Please review this document.", _ 
 ShowMessage:=False, _ 
 IncludeAttachment:=True 
End Sub
```


## 另请参阅


#### 概念


[Document Object](8d83487a-2345-a036-a916-971c9db5b7fb.md)
#### 其他资源


[Document Object Members](http://msdn.microsoft.com/library/fc9ab457-0888-f917-3d52-387168ac23b9%28Office.15%29.aspx)