
# RecurrencePattern.RecurrenceType 属性 (Outlook)

返回或设置一个  **[OlRecurrenceType](63bc267e-6b9d-2cb5-3a96-4beb41afff72.md)** 常量，该常量指定定期模式的发生频率。可读/写。


## 语法

 _表达式_. **RecurrenceType**

 _表达式_ 一个代表 **RecurrencePattern** 对象的变量。


## 注解

设置 **[RecurrencePattern](36c098f7-59fb-879a-5173-ed0260d13fa4.md)** 对象的其他属性之前，必须设置 **介于 1** 。您可以在以后设置 **RecurrencePattern** 属性取决于的值 **介于 1** ，如下表所示:


|||
|:-----|:-----|
|**OlRecurrenceType**|**有效的 RecurrencePattern 属性**|
|**olRecursDaily**|**[Duration](91cceed3-fd56-bae3-ee00-16f4b02eb2e3.md)** 、 **[EndTime](7babda13-9e57-4c80-1ab3-56025753ed9d.md)** 、 **[Interval](e3220174-38dc-d1e3-8d26-b3f208b554a4.md)** 、 **[NoEndDate](47c5841a-c0d2-2b06-ec73-7093779ceafa.md)** 、 **[Occurrences](a99a8a1c-dcd3-e96d-6091-0a005ca3b05f.md)** 、 **[PatternStartDate](20c82dbd-a622-91b6-618c-7cbe8bff2ca7.md)** 、 **[PatternEndDate](0f78ea71-3d92-2d38-be10-e05ab7bcf44a.md)** 、 **[StartTime](557e0f8d-c95d-e1f9-91a2-0734248d8628.md)**|
|**olRecursWeekly**|**[年](79268798-90ab-4161-5a6e-97669daa475a.md)** 、 **持续时间** 、 **结束时间** 、 **间隔** 、 **NoEndDate** 、 **事件** 、 **PatternStartDate** 、 **PatternEndDate** 、 **开始时间**|
|**olRecursMonthly**|**[DayOfMonth](d89a9a55-060c-d25d-4bf6-21e345da36d1.md)** 、 **持续时间** 、 **结束时间** 、 **间隔** 、 **NoEndDate** 、 **事件** 、 **PatternStartDate** 、 **PatternEndDate** 、 **开始时间**|
|**olRecursMonthNth**|**年** 、 **持续时间** 、 **结束时间** 、 **间隔** 、 **[实例](3458aeff-97b7-02f8-e352-203ecc92dedd.md)** 、 **NoEndDate** 、 **事件** 、 **PatternStartDate** 、 **PatternEndDate** 、 **开始时间**|
|**olRecursYearly**|**DayOfMonth** 、 **持续时间** 、 **结束时间** 、 **间隔** 、 **MonthOfYear** 、 **NoEndDate** 、 **事件** 、 **PatternStartDate** 、 **PatternEndDate** 、 **开始时间**|
|**olRecursYearNth**|**年** 、 **持续时间** 、 **结束时间** 、 **间隔** 、 **实例** 、 **NoEndDate** 、 **事件** 、 **PatternStartDate** 、 **PatternEndDate** 、 **开始时间**|

## 示例

此示例的 Visual Basic for Applications 使用 **[GetRecurrencePattern](a9f67c5b-a77f-4e34-e654-d12560a6dba0.md)** 的 **[RecurrencePattern](36c098f7-59fb-879a-5173-ed0260d13fa4.md)** 对象获取新创建的 **[AppointmentItem](204a409d-654e-27aa-643a-8344c631b82d.md)** 。设置属性， **介于 1** 、 **年** 、 **[MonthOfYear](14112950-1e2a-a99a-7c48-3e76358de645.md)** 、 **实例** 、 **实例** 、 **开始时间** 、 **结束时间** 和 **[主题](57f0f242-6d04-175f-4ea2-25145787f5bd.md)** ，约会是保存，则显示与该模式:"发生 6 月的第一个星期一生效 6/1/2007 6/6/2016 从下午 2:00 到下午 5:00 之前。"


```
Sub RecurringYearNth() 
 
 Dim oAppt As AppointmentItem 
 
 Dim oPattern As RecurrencePattern 
 
 Set oAppt = Application.CreateItem(olAppointmentItem) 
 
 Set oPattern = oAppt.GetRecurrencePattern 
 
 With oPattern 
 
 .RecurrenceType = olRecursYearNth 
 
 .DayOfWeekMask = olMonday 
 
 .MonthOfYear = 6 
 
 .Instance = 1 
 
 .Occurrences = 10 
 
 .Duration = 180 
 
 .PatternStartDate = #6/1/2007# 
 
 .StartTime = #2:00:00 PM# 
 
 .EndTime = #5:00:00 PM# 
 
 End With 
 
 oAppt.Subject = "Recurring YearNth Appointment" 
 
 oAppt.Save 
 
 oAppt.Display 
 
End Sub 
 

```


## 另请参阅


#### 概念


[RecurrencePattern 对象](36c098f7-59fb-879a-5173-ed0260d13fa4.md)
#### 其他资源


[RecurrencePattern 对象成员](d282fdb2-2b6d-983d-fe5f-698113d35f89.md)