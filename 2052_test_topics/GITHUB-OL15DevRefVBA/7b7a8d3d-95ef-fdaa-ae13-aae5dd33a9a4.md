
# PostItem.ReadComplete 事件 (Outlook)
Outlook 已完成读取的项属性时发生。

## 版本信息

添加的版本： Outlook 2013


## 语法

 _表达式_. **ReadComplete** _(Cancel)_

 _表达式_ 一个代表 PostItem **PostItem** 对象的变量。


### 参数



|**名称**|**必需/可选**|**数据类型**|**说明**|
|:-----|:-----|:-----|:-----|
|||||
| _Cancel_|必需|**Boolean**|(不使用在 VBScript 中)。 **False**在事件发生时。如果该事件过程将此参数设置为 **True**，读取的操作未完成，并在阅读窗格或检查器中不显示项目。|

## 备注

 **ReadComplete**事件发生之后[BeforeRead](26a64e4e-a48e-84e8-4fea-70913a8f170f.md)事件和[Read](404c9b17-c5b6-a802-420a-f8fd279b5f9b.md)事件的项之前。

要确定该项何时从内存中删除，请使用 [Unload](42dea931-c3dd-b8ff-5ace-0744b17650e0.md) 事件。

 **ReadComplete**事件对应于 Exchange 客户端扩展 (ECE) 事件 **IExchExtMessageEvents::OnReadComplete**。


## 另请参阅


#### 概念


[PostItem 对象](de44065d-4e93-315a-279f-7b92f09c0465.md)
#### 其他资源


[PostItem 对象成员](5b150db1-c96d-0721-ec36-d5b5ebc20fd8.md)