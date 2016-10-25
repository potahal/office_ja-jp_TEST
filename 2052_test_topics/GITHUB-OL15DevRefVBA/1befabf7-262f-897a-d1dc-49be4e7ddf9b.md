
# TaskItem.Respond 方法 （Outlook）

响应任务要求。


## 语法

 _表达式_. **Respond**( ** _Response_**, ** _fNoUI_**, ** _fAdditionalTextDialog_** )

 _表达式_ 一个代表 **TaskItem** 对象的变量。


### 参数



|**名称**|**必需/可选**|**数据类型**|**说明**|
|:-----|:-----|:-----|:-----|
| _Response_|必需|**[OlTaskResponse](7616cbdc-fc9c-abbe-fd07-ebdadc13ede2.md)**|响应要求。|
| _fNoUI_|必需|**Variant**|**真** 不显示对话框。自动发送响应。 **假** 以显示对话框的响应。|
| _fAdditionalTextDialog_|必需|**Variant**|**假** 不提示用户进行输入;响应显示在检查器中进行编辑。 **真** 要提示用户或者直接发送或带批注发送。此参数是 _fNoUI_为 **假** 的情况下才有效。|

### 返回值

一个代表对任务要求的响应的  **[TaskItem](5df8cfa5-5460-a5a1-a130-ba5bca1a0091.md)** 。


## 注解

 **OlTaskAccept** 参数的 **响应** 方法调用时，Outlook 会创建重复的任务请求项新 **TaskItem** 。新的项目都有一个不同的条目 id。Outlook 然后删除原始项目。

下表描述了根据父对象，并使用 _fNoUI_和 _fAdditionalTextDialog_参数的 **响应** 方法的行为。



|** _fNoUI、fAdditionalTextDialog_**|** _结果_**|
|:-----|:-----|
|**True、True**|不带用户界面返回响应项目。若要发送响应，必须调用 **[Send](54f751fc-cff1-5d17-f635-f688cd8ad6f8.md)** 方法。|
|**True、False**|与  **True、True** 结果相同。|
|**False、True**|如果已调用 **[显示](fea0619d-06dc-df44-fe93-5756eefb1be0.md)** 方法，用户提示出现。否则为在不提示的情况下发送项目，而且结果项目为空。|
|**False、False**|无反应。|

## 另请参阅


#### 概念


[TaskItem 对象](5df8cfa5-5460-a5a1-a130-ba5bca1a0091.md)
#### 其他资源


[TaskItem 对象成员](97234a76-2fc5-bbe4-2e14-25ae18694fc9.md)