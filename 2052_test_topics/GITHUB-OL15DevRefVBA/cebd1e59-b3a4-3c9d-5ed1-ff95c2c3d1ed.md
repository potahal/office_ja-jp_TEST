
# ContactItem.BeforeRead 事件 (Outlook)

在 Microsoft Outlook 开始读取项目属性之前发生。


## 语法

 _表达式_. **BeforeRead**

 _表达式_ 一个表示 **ContactItem** 对象的变量。


## 说明

 **BeforeRead** 事件发生之前 **[读取](508b4637-9d74-7645-7719-3c148d0688d8.md)** 事件。与之前前缀与其他事件，此事件不是可以取消的。若要确定何时从内存中卸载该项目，请使用 **[Unload](16a3d7ce-0843-5eb5-bbea-df6557ceda05.md)** 事件。

 **BeforeRead** 事件对应于 **IExchExtMessageEvents::OnRead** 的 Exchange 客户端扩展 (ECE) 事件。

在 **BeforeRead** 事件中可以访问只有 item 对象的下列成员:


-  **[class](7c08cb72-fdbb-aac8-2691-382bfdae22c8.md)**
    
-  **[MessageClass](3d6594b7-8abe-9e49-64e0-be3062807e34.md)**
    
-  **MAPIOBJECT**
    
 **MAPIOBJECT** 属性是 Outlook 对象模型中的隐藏的属性。此属性提供对基础的 MAPI **[IMessage](http://msdn.microsoft.com/en-us/library/cc842097%28office.14%29.aspx)** 对象，并且可以仅通过 **[IUnknown](http://msdn.microsoft.com/en-us/library/ms680509%28VS.85%29.aspx)** 接口调用。该属性是用支持 **IUnknown** 如 C 或 c + + 语言编写的程序可以访问的。 **MAPIOBJECT** 不能通过 **[IDispatch](http://msdn.microsoft.com/en-us/library/ms221608.aspx)** 接口。如 (VBA)、 视觉 C#，和 Visual Basic 的 Visual Basic for Applications 的开发语言都支持 **IDispatch** 接口并不为 **IUnknown** ，，因此，它们无法访问 **MAPIOBJECT** 。 如果在此事件中访问其他属性或方法的父项，则 Outlook 将引发错误。

如果实施者访问该对象的基础 **IMessage** 对象，并更改属性，Outlook 将呈现对 **IMessage** 对象的更改以反映该项目。实施者不需要调用 **IMessage** 对象以使所做的更改会反映在 Outlook 中 **[SaveChanges](http://msdn.microsoft.com/en-us/library/cc842181%28office.14%29.aspx)** 。

实现者必须释放来自事件完成前的事件中的 **MAPIOBJECT** 属性的对象。尝试使用该对象的事件上下文之外是不受支持，将导致不可预知的行为。


## 另请参阅


#### 概念


[创建 ContactItem 对象](8e32093c-a678-f1fd-3f35-c2d8994d166f.md)
#### 其他资源


[创建 ContactItem 对象成员](a8b13369-4c87-02aa-e62a-1f3067e559fa.md)