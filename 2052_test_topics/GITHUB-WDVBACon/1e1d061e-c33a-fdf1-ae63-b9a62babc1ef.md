
# Document.SendFaxOverInternet Method (Word)

Sends a document to a fax service provider, who faxes the document to one or more specfied recipients.


## Syntax

 _表达式_. **SendFaxOverInternet**( ** _Recipients_**, ** _Subject_**, ** _ShowMessage_** )

 _表达式_ Required. A variable that represents a **[Document](8d83487a-2345-a036-a916-971c9db5b7fb.md)** object.


### Parameters



|**Name**|**Required/Optional**|**Data Type**|**Description**|
|:-----|:-----|:-----|:-----|
| _Recipients_|可选|**Variant**|A  **String** that represents the fax numbers and e-mail addresses of the people to whom to send the fax. Separate multiple recipients with a semicolon.|
| _Subject_|可选|**Variant**|A  **String** that represents the subject line for the faxed document.|
| _ShowMessage_|可选|**Variant**|**True** displays the fax message before sending it. **False** sends the fax without displaying the fax message.|

## Remarks

Using the  **SendFaxOverInternet** method requires that a fax service is enabled on a user's computer. If a fax service is not enabled, the **SendFaxOverInternet** method will cause a runtime error.

The format used for specifying fax numbers in the Recipients parameter is either recipientsfaxnumber@usersfaxprovider or recipientsname@recipientsfaxnumber. You can access the user's fax provider information using the following registry path:




```
HKEY_CURRENT_USER\Software\Microsoft\Office\11.0\Common\Services\Fax
```

Use the FaxAddress key value at this registry location to determine the format to use for a user. If this registry entry does not exist, no fax service is available.


## Example

The following example sends a fax to the fax service provider, who will fax the message to the recipient.


```
ActiveDocument.SendFaxOverInternet _ 
 "14255550101@consolidatedmessenger.com", _ 
 "For your review", True
```


## 另请参阅


#### 概念


[Document Object](8d83487a-2345-a036-a916-971c9db5b7fb.md)
#### 其他资源


[Document Object Members](http://msdn.microsoft.com/library/fc9ab457-0888-f917-3d52-387168ac23b9%28Office.15%29.aspx)