
# Document.CheckIn Method (Word)

Returns a document from a local computer to a server, and sets the local document to read-only so that it cannot be edited locally.


## Syntax

 _表达式_. **CheckIn**( ** _SaveChanges_**, ** _Comments_**, ** _MakePublic_** )

 _表达式_ Required. A variable that represents a **[Document](8d83487a-2345-a036-a916-971c9db5b7fb.md)** object.


### Parameters



|**Name**|**Required/Optional**|**Data Type**|**Description**|
|:-----|:-----|:-----|:-----|
| _SaveChanges_|可选|**Boolean**|**True** saves the document to the server location. The default is **True**.|
| _Comments_|可选|**Variant**|Comments for the revision of the document being checked in (only applies if SaveChanges equals  **True** ).|
|||||
| _MakePublic_|可选|**Boolean**|**True** allows the user to perform a publish on the document after being checked in. This submits the document for the approval process, which can eventually result in a version of the document being published to users with read-only rights to the document (only applies if _SaveChanges_ equals **True** ). The default is **False**.|
|||||
|||||

## Remarks

To take advantage of the collaboration features built into Microsoft Word, documents must be stored on a Microsoft SharePoint Portal Server.


## Example

This example checks the server to see if the specified document can be checked in. If it can be, it saves and closes the document and checks it back into the server.


```
Sub CheckInOut(docCheckIn As String) 
 If Documents(docCheckIn).CanCheckin = True Then 
 Documents(docCheckIn).CheckIn 
 MsgBox docCheckIn &amp; " has been checked in." 
 Else 
 MsgBox "This file cannot be checked in " &amp; 
 "at this time. Please try again later." 
 End If 
End Sub
```

To call the CheckInOut subroutine, use the following subroutine and replace  _"http://servername/workspace/report.doc"_ with the file name of an actual file located on the server mentioned in the Remarks section above.




```
Sub CheckDocInOut() 
 Call CheckInOut (docCheckIn:="http://servername/workspace/report.doc") 
End Sub
```


## 另请参阅


#### 概念


[Document Object](8d83487a-2345-a036-a916-971c9db5b7fb.md)
#### 其他资源


[Document Object Members](http://msdn.microsoft.com/library/fc9ab457-0888-f917-3d52-387168ac23b9%28Office.15%29.aspx)