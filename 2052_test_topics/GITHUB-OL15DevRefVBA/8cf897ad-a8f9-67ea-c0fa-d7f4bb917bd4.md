
# AddressRuleCondition 对象 （Outlook）

代表一个规则条件，该规则条件评估  **[AddressRuleCondition.Address](de4186ec-0741-8ff6-7789-af0a46c470e0.md)** 所指定的地址中是否包含邮件收件人或发件人的地址。


## 说明

 **AddressRuleCondition** 被从 **[RuleCondition](e03f91c2-2c08-b036-104a-d6246f28bc2d.md)** 对象。每个规则都与它有一个 **[RecipientAddress](1b8f361e-0481-75dc-e66e-2bc69228773a.md)** 属性和 **[SenderAddress](6e5eb1cc-385f-b1b2-aea7-12629cc31030.md)** **[RuleConditions](e8e9a05a-b36b-add2-b294-8cdc5a97e119.md)** 对象相关联。这两个属性总是返回一个 **AddressRuleCondition** 对象。 **[AddressRuleCondition.ConditionType](8b531745-1a4d-d903-5c7d-465b9fd8cbf3.md)** 这些规则条件区分开来。该规则有任何启用这些规则条件，如果 **[AddressRuleCondition.Enabled](170cd84c-4733-0801-c411-34736e2e1a06.md)** 为 **True** 。

有关指定规则操作的详细信息，请参阅[指定规则条件](http://msdn.microsoft.com/library/812c131a-fe23-1b8b-5e2d-9459d7102630%28Office.15%29.aspx)。


## 另请参阅


#### 其他资源


[Outlook 对象模型引用](http://msdn.microsoft.com/library/73221b13-d8d8-99b8-3394-b95dbbfd5ddc%28Office.15%29.aspx)
[AddressRuleCondition 对象成员](d15b0554-6b47-b201-fd41-744ea056d3f6.md)