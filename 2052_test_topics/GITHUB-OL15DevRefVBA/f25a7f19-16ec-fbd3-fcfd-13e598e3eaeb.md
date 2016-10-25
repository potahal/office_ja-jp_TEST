
# TaskRequestAcceptItem.AfterWrite 事件 (Outlook)

在 Microsoft Outlook 保存项目之后发生。


## 语法

 _表达式_. **AfterWrite**

 _表达式_ 一个表示 **TaskRequestAcceptItem** 对象的变量。


## 说明

在 **AfterWrite** 发生事件 **[编写](005b0f33-1848-101b-2119-cb15eb51f411.md)** 事件之后。此事件不是可以取消的。若要确定何时从内存中卸载该项目，请使用 **[Unload](19e89fda-1887-ad50-5db3-a1bb2ad77261.md)** 事件。

 **AfterWrite** 事件对应于 **IExchExtMessageEvents::OnWriteComplete** 的 Exchange 客户端扩展 (ECE) 事件。

在 **AfterWrite** 事件中可以访问只有 item 对象的下列成员:


-  **[class](d829ebf5-ec8a-7c4f-89c2-49c194339672.md)**
    
-  **[MessageClass](817ffe01-109d-5121-96c9-d4738b1dfd91.md)**
    
-  **MAPIOBJECT**
    
 **MAPIOBJECT** 属性是 Outlook 对象模型中的隐藏的属性。此属性提供对基础的 MAPI **[IMessage](http://msdn.microsoft.com/en-us/library/cc842097%28office.14%29.aspx)** 对象，并且可以仅通过 **[IUnknown](http://msdn.microsoft.com/en-us/library/ms680509%28VS.85%29.aspx)** 接口调用。该属性是用支持 **IUnknown** 如 C 或 c + + 语言编写的程序可以访问的。 **MAPIOBJECT** 不能通过 **[IDispatch](http://msdn.microsoft.com/en-us/library/ms221608.aspx)** 接口。如 (VBA)、 视觉 C#，和 Visual Basic 的 Visual Basic for Applications 的开发语言都支持 **IDispatch** 接口并不为 **IUnknown** ，，因此，它们无法访问 **MAPIOBJECT** 。 如果在此事件中访问其他属性或方法的父项，则 Outlook 将引发错误。

获取在此事件中的 **MAPIOBJECT** 属性的对象必须包含所有 outlook 保留所做的更改。 实施者可以调用 **[SaveChanges](http://msdn.microsoft.com/en-us/library/cc842181%28office.14%29.aspx)** **IMessage** 对象以保持对基础 **IMessage** 对象由 **MAPIOBJECT** ，更改，Outlook 将不会还原这些更改。

实现者必须释放来自事件完成前的事件中的 **MAPIOBJECT** 属性的对象。尝试使用该对象的事件上下文之外是不受支持，将导致不可预知的行为。


## 另请参阅


#### 概念


[TaskRequestAcceptItem 对象](a2905f72-0a67-b07d-7f85-84fe4de17c25.md)
#### 其他资源


[TaskRequestAcceptItem 对象成员](fe91c4cc-f505-11d8-0d0a-84fc4d355651.md)