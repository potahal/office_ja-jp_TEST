
# RuleConditions.From 属性 (Outlook)

返回一个 **[ToOrFromRuleCondition](ec5cae2a-cde8-5681-6a49-74e2f0226a4f.md)** **olConditionFrom** **[ToOrFromRuleCondition.ConditionType](a5c6e08c-643e-965d-cd3e-b434f20579a0.md)** 。只读的。


## 语法

 _表达式_. **From**

 _表达式_ 一个代表 **RuleConditions** 对象的变量。


## 说明

枚举的规则条件或例外条件的现有规则，或者在创建一个新规则来指定的条件或例外条件时，消息的发件人位于指定的 **[收件人](774f56b7-4de8-9584-60cd-4fbf361f4c85.md)** 列表，请使用返回的 **ToOrFromRuleCondition** 对象。

始终 **[RuleConditions](e8e9a05a-b36b-add2-b294-8cdc5a97e119.md)** 集合的该属性返回一个 **ToOrFromRuleCondition** 对象，而不论是否与此 **RuleConditions** 集合关联的规则已定义规则条件。如果该规则已定义并启用此类规则条件，则 **[ToOrFromRuleCondition.Enabled](31e43906-b47a-95e3-d51b-3fa6af553fad.md)** 将 **真** 。


## 另请参阅


#### 概念


[RuleConditions 对象](e8e9a05a-b36b-add2-b294-8cdc5a97e119.md)
#### 其他资源


[RuleConditions 对象成员](b2af6ebf-f9f8-8106-20a3-1725c3b78174.md)