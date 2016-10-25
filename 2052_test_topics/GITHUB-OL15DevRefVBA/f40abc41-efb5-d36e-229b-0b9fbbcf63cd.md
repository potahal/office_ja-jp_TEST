
# AppointmentItem.CustomPropertyChange 事件 (Outlook)

当项目（父对象的一个实例）的自定义属性更改时发生。


## 语法

 _表达式_. **CustomPropertyChange**( ** _Name_** )

 _表达式_ 一个代表 **AppointmentItem** 对象的变量。


### 参数



|**名称**|**必需/可选**|**数据类型**|**说明**|
|:-----|:-----|:-----|:-----|
| _Name_|必需|**String**|被更改的自定义属性的名称。|

## 注解

属性名传递给过程，以便于用户确定哪个自定义属性被更改。


## 示例

此 Microsoft Visual Basic 脚本版本 (VBScript) 示例使用 **CustomPropertyChange** 事件的布尔型字段设置为 **True** 时启用的控件。

此示例中，窗体第二页上创建两个自定义字段。第一个 **布尔型** 字段，被命名为"RespondBy"。第二个字段的名称为"DateToRespond"。




```
Sub Item_CustomPropertyChange(ByVal myPropName) 
 
 Select Case myPropName 
 
 Case "RespondBy" 
 
 Set myPages = Item.GetInspector.ModifiedFormPages 
 
 Set myCtrl = myPages("P.2").Controls("DateToRespond") 
 
 If Item.UserProperties("RespondBy").Value Then 
 
 myCtrl.Enabled = True 
 
 myCtrl.Backcolor = 65535 'Yellow 
 
 Else 
 
 myCtrl.Enabled = False 
 
 myCtrl.Backcolor = 0 'Black 
 
 End If 
 
 Case Else 
 
 End Select 
 
End Sub
```


## 另请参阅


#### 概念


[AppointmentItem 对象](204a409d-654e-27aa-643a-8344c631b82d.md)
#### 其他资源


[AppointmentItem 对象成员](c72c459d-6d3c-7a05-aa4a-b1b767ddc0b2.md)