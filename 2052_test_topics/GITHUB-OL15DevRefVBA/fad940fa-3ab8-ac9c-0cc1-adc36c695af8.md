
# MailItem.BeforeAttachmentWriteToTempFile 事件 (Outlook)

将与父对象的实例相关的附件写到临时文件前发生。


## 语法

 _表达式_. **BeforeAttachmentWriteToTempFile**( ** _Attachment_**, ** _Cancel_** )

 _表达式_ 一个表示 **MailItem** 对象的变量。


### 参数



|**名称**|**必需/可选**|**数据类型**|**说明**|
|:-----|:-----|:-----|:-----|
| _Attachment_|必需|**[Attachment](3e11582b-ac90-0948-bc37-506570bb287b.md)**|**附件** 以进行写入。|
| _Cancel_|必需|**Boolean**|设置为 **True** 以取消操作;否则，设置为 **False** 以允许写入 **附件** 。|

## 另请参阅


#### 概念


[MailItem 对象](14197346-05d2-0250-fa4c-4a6b07daf25f.md)
#### 其他资源


[MailItem 对象成员](1094d7df-ee80-a4b0-5a21-db2979506e6b.md)