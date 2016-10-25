
# Inspector.SetCurrentFormPage 方法 （Outlook）

在检查器中显示指定的窗体页或窗体区域。


## 语法

 _表达式_. **SetCurrentFormPage**( ** _PageName_** )

 _表达式_ 一个代表 **Inspector** 对象的变量。


### 参数



|**名称**|**必需/可选**|**数据类型**|**说明**|
|:-----|:-----|:-----|:-----|
| _PageName_|必需|**String**|显示名称的窗体页中或窗体区域的内部名称。|

## 说明

可以使用 **SetCurrentFormPage** 来显示窗体区域通过指定窗体区域的 **[InternalName](2478d44e-887c-c245-6cfa-70a6a1e2c828.md)** 属性，如果该窗体区域是一个单独的、 替换或全部替换窗体区域。


## 示例

此 Visual Basic for Applications (VBA) 示例使用 **SetCurrentFormPage** 方法来显示当前打开的项目 **所有字段**页。如果发生错误，Outlook 将向用户显示一个消息框。


```
Sub ShowAllFieldsPage() 
 
 On Error GoTo ErrorHandler 
 
 Dim myInspector As Inspector 
 
 Dim myItem As Object 
 
 
 
 Set myInspector = Application.ActiveInspector 
 
 myInspector.SetCurrentFormPage ("All Fields") 
 
 Set myItem = myInspector.CurrentItem 
 
 myItem.Display 
 
Exit Sub 
 
 
 
ErrorHandler: 
 
 MsgBox Err.Description, vbInformation 
 
End Sub
```


## 另请参阅


#### 概念


[检查对象](d7384756-669c-0549-1032-c3b864187994.md)
#### 其他资源


[检查对象成员](acd3e13f-4727-7966-d2a5-a95e4528425c.md)