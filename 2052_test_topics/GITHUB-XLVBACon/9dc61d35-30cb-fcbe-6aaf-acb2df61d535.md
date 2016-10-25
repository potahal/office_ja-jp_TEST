
# IRtdServer.Heartbeat Method (Excel)

Determines if the real-time data server is still active. Returns a  **Long** value. Zero or a negative number indicates failure; a positive number indicates that the server is active.


## Syntax

 _表达式_. **Heartbeat**

 _表达式_ A variable that represents an **IRtdServer** object.


### Return Value

Long


## Remarks

The  **Heartbeat** method is called by Microsoft Excel if the **[HeartbeatInterval](45a3df85-59c1-fedb-e94b-8f011601fc72.md)** property has elapsed since the last time Excel was called with the **[UpdateNotify](e3ae5a7e-4d8c-9eba-62ab-a24d1045bc77.md)** method.


## 另请参阅


#### 概念


[IRtdServer Object](6a85aa64-9514-74bb-3c63-141275f1b671.md)
#### 其他资源


[IRtdServer Object Members](http://msdn.microsoft.com/library/90baa971-8dc0-b4b9-77c4-72530f1aaf21%28Office.15%29.aspx)