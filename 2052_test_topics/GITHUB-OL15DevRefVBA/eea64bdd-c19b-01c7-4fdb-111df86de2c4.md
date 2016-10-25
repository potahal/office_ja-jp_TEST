
# AppointmentItem.Duration 属性 (Outlook)

返回或设置 **Long** 值，表明 **[AppointmentItem](204a409d-654e-27aa-643a-8344c631b82d.md)** 的持续时间 (以分钟为单位)。读/写。


## 语法

 _表达式_. **Duration**

 _表达式_ 一个代表 **AppointmentItem** 对象的变量。


## 示例

此示例的 Visual Basic for Applications 使用 **[Application.CreateItem](e5fbf367-db16-5042-823e-68e6b805e612.md)** 来创建约会，并使用 **[AppointmentItem.MeetingStatus](cfd970cd-df6c-4537-0a17-b5adab3b667f.md)** 会议将状态设置为"会议"，以将其转换成的必选和可选与会者的会议要求。


```
Sub ScheduleMeeting() 
 
 Dim myItem as AppointmentItem 
 
 Dim myRequiredAttendee As Recipient 
 
 Dim myOptionalAttendee As Recipient 
 
 Dim myResourceAttendee As Recipient 
 
 
 
 Set myItem = Application.CreateItem(olAppointmentItem) 
 
 myItem.MeetingStatus = olMeeting 
 
 myItem.Subject = "Strategy Meeting" 
 
 myItem.Location = "Conference Room B" 
 
 myItem.Start = #9/24/2002 1:30:00 PM# 
 
 myItem.Duration = 90 
 
 Set myRequiredAttendee = myItem.Recipients.Add ("Nate Sun") 
 
 myRequiredAttendee.Type = olRequired 
 
 Set myOptionalAttendee = myItem.Recipients.Add ("Kevin Kennedy") 
 
 myOptionalAttendee.Type = olOptional 
 
 Set myResourceAttendee = myItem.Recipients.Add("Conference Room B") 
 
 myResourceAttendee.Type = olResource 
 
 myItem.Display 
 
End Sub
```


## 另请参阅


#### 概念


[AppointmentItem 对象](204a409d-654e-27aa-643a-8344c631b82d.md)
#### 其他资源


[AppointmentItem 对象成员](c72c459d-6d3c-7a05-aa4a-b1b767ddc0b2.md)