
# RemoteItem.Write 事件 (Outlook)

保存父对象的实例时发生；可以是显式保存（例如，使用  **[Save](0f4e57ab-388c-7ba1-c6b8-f14bfc0ac73c.md)** 或 **[SaveAs](1c2c7b68-5239-05f8-4291-d2584fe95194.md)** 方法），也可以是隐式保存（例如，关闭项目的检查器时为了响应提示而进行的保存）。


## 语法

 _表达式_. **Write**( ** _Cancel_** )

 _表达式_ 一个代表 **RemoteItem** 对象的变量。


### 参数



|**名称**|**必需/可选**|**数据类型**|**说明**|
|:-----|:-----|:-----|:-----|
| _Cancel_|必需|**Boolean**|(不使用在 VBScript 中)。 **假** 的事件发生时。如果事件过程将此参数设置为 **True** ，则保存操作没有完成。|

## 注解

在 Microsoft Visual Basic 脚本版本 (VBScript)，如果您将此函数的返回值设置为 **False** ，保存操作未完成。


## 另请参阅


#### 概念


[RemoteItem 对象](6302aaff-cdcf-4d86-60f1-4bed15540d9f.md)
#### 其他资源


[RemoteItem 对象成员](15c0872e-88cc-9b9b-c31e-c15d6971e6e0.md)