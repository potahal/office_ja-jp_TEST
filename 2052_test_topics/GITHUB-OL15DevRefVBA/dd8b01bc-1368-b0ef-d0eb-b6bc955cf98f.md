
# TaskRequestDeclineItem.BeforeCheckNames 事件 (Outlook)

在 Microsoft Outlook 开始解析项目（父对象的一个实例）的收件人集合中的名称前发生。


## 语法

 _表达式_. **BeforeCheckNames**( ** _Cancel_** )

 _表达式_ 一个表示 **TaskRequestDeclineItem** 对象的变量。


### 参数



|**名称**|**必需/可选**|**数据类型**|**说明**|
|:-----|:-----|:-----|:-----|
| _Cancel_|必需|**Boolean**|**假** 的事件发生时。如果事件过程将此参数设置为 **True** ，则不完成名称解析过程。|

## 注解

在 VBScript 中，使用 **BeforeCheckNames** 事件，但当窗体上解析电子邮件名称时不会触发该事件。

下列情况不会触发此事件：


- 自定义"日记条目"窗体，然后解析 **"联系人"**字段中的联系人。
    
- 自定义"联系人"窗体，然后解析 **"联系人"**字段中的联系人。
    
- 自定义任何类型的窗体，然后 Outlook 自动在后台解析名称。
    
- 以编程方式创建和解析收件人。
    



## 另请参阅


#### 概念


[TaskRequestDeclineItem 对象](e842c7c0-7943-9219-329b-30b892ab99b0.md)
#### 其他资源


[TaskRequestDeclineItem 对象成员](3de31d0d-2444-876c-5d4d-1192851301af.md)