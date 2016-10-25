
# Broadcast.End Method (Word)

Ends the specified broadcast session.


## Syntax

 _表达式_. **End**

 _表达式_ A variable that represents a **Broadcast** object.


### Return value

 **VOID**


## Remarks

Calling the  **End** method terminates the broadcast session without displaying a confirmation prompt to the user. It also sets the value of the[Broadcast.AttendeeURL](3abe1a3c-14eb-8405-c16d-0bdf6b30e34f.md) property to an empty string.

If the document is not being broadcast, the method returns runtime error 4702.


## 另请参阅


#### 其他资源


[Broadcast Object](47a77749-ef18-d38a-af24-03f32c9e1151.md)
[Broadcast Members](http://msdn.microsoft.com/library/936c0328-6b7d-b886-c9c8-e942455c5081%28Office.15%29.aspx)