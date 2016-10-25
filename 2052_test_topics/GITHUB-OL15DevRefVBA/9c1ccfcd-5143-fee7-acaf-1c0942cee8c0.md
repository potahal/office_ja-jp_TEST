
# TaskRequestAcceptItem.BeforeAttachmentPreview 事件 (Outlook)

预览与父对象的实例相关的附件前发生。


## 语法

 _表达式_. **BeforeAttachmentPreview**( ** _Attachment_**, ** _Cancel_** )

 _表达式_ 一个表示 **TaskRequestAcceptItem** 对象的变量。


### 参数



|**名称**|**必需/可选**|**数据类型**|**说明**|
|:-----|:-----|:-----|:-----|
| _Attachment_|必需|**[Attachment](3e11582b-ac90-0948-bc37-506570bb287b.md)**|**附件** 预览。|
| _Cancel_|必需|**Boolean**|设置为 **True** 以取消操作;否则，设置为 **False** 以允许 **附件** 以进行预览。|

## 说明

此事件在某附件被预览（从活动浏览器的阅读窗格中的附件区中预览，或从活动检查器中预览）之前发生。


## 另请参阅


#### 概念


[TaskRequestAcceptItem 对象](a2905f72-0a67-b07d-7f85-84fe4de17c25.md)
#### 其他资源


[TaskRequestAcceptItem 对象成员](fe91c4cc-f505-11d8-0d0a-84fc4d355651.md)