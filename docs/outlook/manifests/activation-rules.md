
# Outlook アドインのアクティブ化ルール
Outlook アドイン マニフェスト内でアクティブ化ルールを定義し、どのような場合に Outlook でアドインを表示するかを指定します。

 _ **適用対象:** apps for Office?| Office Add-ins?| Outlook_


## 指定したルールによるアクティブ化


Outlook は、ユーザーが閲覧または作成しているメッセージや予定が、アドインのアクティブ化ルールの条件を満たしている場合に、ある種のアドインをアクティブ化します。この仕組みは、1.1 マニフェスト スキーマを使用するすべてのアドインと、カスタム ウィンドウ アドインに適用されます。その後、ユーザーは Outlook UI からそのアドインを選択して、現在のアイテムに対して起動できます。

次の図は、閲覧ウィンドウにあるメッセージ用にアドイン バーの中でアクティブ化された Outlook アドインを示しています。 


**閲覧ウィンドウの閲覧フォームの中で、メッセージ用にアクティブ化された 2 つの Outlook アドインを表示するアドイン バー**

![メール読み取りアプリがアクティブ化されたことを示すアプリ バー](../../images/mod_off15_MailAppAppBar.png)


## マニフェストでのアクティブ化ルールの指定


Outlook に特定の条件でアドインをアクティブ化させるには、アドインのマニフェストにアクティブ化ルールを指定します。次の 2 つの  **Rule** 要素を使用できます。


- 個別のルールを指定する [Rule 要素 (MailApp complexType) (Office アドイン スキーマ v1.1)](http://msdn.microsoft.com/ja-jp/library/56dfc32e-2b8c-1724-05be-5595baf38aa3%28Office.15%29.aspx)
    
- 論理演算を使用して複数のルールを結合する [Rule 要素 (RuleCollection complexType) (Office アドイン スキーマ v1.1)](http://msdn.microsoft.com/ja-jp/library/c6ce9d52-4b53-c6a6-de7e-c64106135c81%28Office.15%29.aspx)
    

 >**メモ**  個別のルールの指定に使用する [Rule](http://msdn.microsoft.com/ja-jp/library/56dfc32e-2b8c-1724-05be-5595baf38aa3%28Office.15%29.aspx) 要素は抽象 [Rule](http://msdn.microsoft.com/ja-jp/library/bcd7a3a7-9cd4-a270-89e0-5386d1c6df01%28Office.15%29.aspx) 複合型です。以下のルールの種類は、それぞれ、この抽象 **Rule** 複合型を拡張します。したがって、マニフェストで個別のルールを指定するときは、 [xsi:type](http://www.w3.org/TR/xmlschema-1/) 属性を使用して次のいずれかのルールの種類をさらに定義する必要があります。たとえば、以下のルールでは、 [ItemIs](http://msdn.microsoft.com/ja-jp/library/f7dac4a3-1574-9671-1eda-47f092390669%28Office.15%29.aspx) ルールが定義されます。 `<Rule xsi:type="ItemIs" ItemType="Message" />`注:  **FormType** 属性はマニフェスト v1.1 のアクティブ化ルールに適用されますが、この属性は **VersionOverrides** v1.0 では定義されていません。そのため、 [ItemIs](http://msdn.microsoft.com/ja-jp/library/f7dac4a3-1574-9671-1eda-47f092390669%28Office.15%29.aspx) を **VersionOverrides** ノードで使用する場合には、この属性は使用できません。

次の表は、使用できるルールの種類を示しています。詳細は、この表の後、および「 [閲覧フォーム用の Outlook アドインを作成する](../outlook/read-scenario.md)」に示された記事で確認できます。


**閲覧フォームで使用できるルールの種類**


|**ルール名**|**該当するフォーム**|**説明**|
|:-----|:-----|:-----|
|[ItemIs](http://msdn.microsoft.com/ja-jp/library/f7dac4a3-1574-9671-1eda-47f092390669%28Office.15%29.aspx)|閲覧、新規作成、カスタムのウィンドウ|現在選択されているアイテムは指定された種類のアイテム (メッセージまたは予定) かどうかを調べます。また、アイテム クラス、フォームの種類、さらにはオプションでアイテム メッセージ クラスも調べることができます。|
|[ItemHasAttachment](http://msdn.microsoft.com/ja-jp/library/031db7be-8a25-5185-a9c3-93987e10c6c2%28Office.15%29.aspx)|閲覧、カスタム ウィンドウ|選択されているアイテムに添付ファイルが含まれるかどうかを調べます。|
|[ItemHasKnownEntity](http://msdn.microsoft.com/ja-jp/library/87e10fd2-eab4-c8aa-bec3-dff92d004d39%28Office.15%29.aspx)|閲覧、カスタム ウィンドウ|選択されているアイテムに 1 つ以上の一般的なエンティティが含まれるかどうかを調べます。詳細: [Outlook アイテム内の文字列を既知のエンティティとして照合する](../outlook/match-strings-in-an-item-as-well-known-entities.md).|
|[ItemHasRegularExpressionMatch](http://msdn.microsoft.com/ja-jp/library/bfb726cd-81b0-a8d5-644f-2ca90a5273fc%28Office.15%29.aspx)|閲覧、カスタム ウィンドウ|選択されているアイテムの送信者の電子メール アドレス、件名、本文に正規表現と一致するものが含まれるかどうかを調べます。詳細: [正規表現アクティブ化ルールを使用して Outlook アドインを表示する](../outlook/use-regular-expressions-to-show-an-outlook-add-in.md)|
|[RuleCollection](http://msdn.microsoft.com/ja-jp/library/926249ab-2d2f-39f5-1d73-fab1c989966f%28Office.15%29.aspx)|閲覧、新規作成、カスタム ウィンドウ|複数のルールを組み合わせて、より複雑なルールを作成できます。|

## ItemIs ルール


 **ItemIs** の複合型は、現在のアイテムがアイテムの種類と一致しており、なおかつ、オプションとしてルールに記載がある場合はアイテムのメッセージ クラスと一致している場合に **true** と評価されるルールを定義します。

次のいずれかの種類のアイテムを  **ItemIs** ルールの **ItemType** 属性に指定します。1 つのマニフェスト内に複数の **ItemIs** ルールを指定できます。 [ItemType](http://msdn.microsoft.com/ja-jp/library/5a890b98-3d83-77ef-ef03-9b513d35b79f%28Office.15%29.aspx) simpleType は、Outlook アドインをサポートする Outlook アイテムの種類を定義します。



|**ItemType の値**|**説明**|
|:-----|:-----|
|**Appointment**|Outlook の予定表内のアイテムを指定します。このアイテムには、開催者と出席者を持つ応答済みの会議アイテムと、開催者と出席者を持たない、単なる予定表上のアイテムである予定が含まれます。これは Outlook の IPM.Appointment メッセージ クラスに対応します。|
|**Message**|通常は受信トレイで受信された次のアイテムのいずれかを指定します。
<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>電子メール メッセージ。これは Outlook の IPM.Note メッセージ クラスに対応します。</p></li><li><p>会議出席依頼、返信、または取り消し。Outlook の次のメッセージ クラスに対応します。</p><p>IPM.Schedule.Meeting.Request</p><p>IPM.Schedule.Meeting.Neg</p><p>IPM.Schedule.Meeting.Pos</p><p>IPM.Schedule.Meeting.Tent</p><p>IPM.Schedule.Meeting.Canceled</p></li></ul>|
 **FormType** 属性は、アドインをアクティブ化するときのモード (閲覧または新規作成) を指定するために使用します。


 >**メモ**  [ItemIs](http://msdn.microsoft.com/ja-jp/library/f7dac4a3-1574-9671-1eda-47f092390669%28Office.15%29.aspx) **FormType** 属性はスキーマ v1.1 以降では定義されていますが、 **VersionOverrides** v1.0 では定義されていません。カスタム ウィンドウのルールを定義するときには、 **FormType** 属性は含めないでください。

アドインがアクティブ化された後は、 [mailbox.item](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md) プロパティを使用して Outlook で現在選択されているアイテムを取得し、 [item.itemType](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md) プロパティを使用して現在のアイテムの種類を取得できます。

必要に応じて、 **ItemClass** 属性を使用してアイテムのメッセージ クラスを指定し、 **IncludeSubClasses** 属性を使用して、アイテムが指定したクラスのサブクラスであるときにルールを **true** にするかどうかを指定できます。

メッセージ クラスの詳細については、「 [アイテムの種類とメッセージ クラス](http://msdn.microsoft.com/library/15b709cc-7486-b6c7-88a3-4a4d8e0ab292%28Office.15%29.aspx)」をご覧ください。

次の例は、ユーザーがメッセージを読むときに Outlook のアドイン バーにアドインを表示する  **ItemIs** ルールを示しています。




```
<Rule xsi:type="ItemIs" ItemType="Message" FormType="Read" />
```

次の例は、ユーザーがメッセージを読むときに Outlook のアドイン バーにアドインを表示する  **ItemIs** ルールを示しています。




```XML
<Rule xsi:type="RuleCollection" Mode="Or">
  <Rule xsi:type="ItemIs" ItemType="Message" FormType="Read" />
  <Rule xsi:type="ItemIs" ItemType="Appointment" FormType="Read" />
</Rule>
```


## ItemHasAttachment ルール


 **ItemHasAttachment** 複合型は、選択されているアイテムに添付ファイルが含まれるかどうかを調べるルールを定義します。


```XML
<Rule xsi:type="ItemHasAttachment" />
```


## ItemHasKnownEntity ルール


アドインがアイテムを処理できるようになる前に、サーバーによってそのアイテムが調べられ、件名または本文に既知のエンティティのいずれかである可能性があるテキストが含まれるかどうかが判別されます。それらのいずれかのエンティティが発見されると、そのエンティティは既知のエンティティのコレクションに格納されます。これらのエンティティには、そのアイテムの  **getEntities** メソッドまたは **getEntitiesByType** メソッドを使用してアクセスします。

[ItemHasKnownEntity](http://msdn.microsoft.com/ja-jp/library/87e10fd2-eab4-c8aa-bec3-dff92d004d39%28Office.15%29.aspx) 複合型を使用すると、指定した種類のエンティティがアイテム内に存在する場合にアドインを表示するルールを指定できます。 **ItemHasKnownEntity** ルールの **EntityType** 属性には、次の既知のエンティティを指定できます。


-  **Address**
    
-  **Contact**
    
-  **EmailAddress**
    
-  **MeetingSuggestion**
    
-  **PhoneNumber**
    
-  **TaskSuggestion**
    
-  **URL**
    
これらのエンティティは、 [KnownEntityType](http://msdn.microsoft.com/ja-jp/library/432d413b-9fcc-eb50-cfea-0ed10a43bd52%28Office.15%29.aspx) 単純型の列挙値として定義されています。

必要に応じて、 **RegularExpression** 属性に正規表現を含め、正規表現に一致するエンティティが存在するときにのみアドインを表示できます。 **ItemHasKnownEntity** ルールで指定されている正規表現と一致したものを取得するには、現在選択されている Outlook アイテムに対して **getRegExMatches** メソッドまたは **getFilteredEntitiesByName** メソッドを使用できます。

次の例では、指定した既知のエンティティのいずれかがメッセージに存在するときにアドインを表示する  **Rule** 要素のコレクションを示します。




```XML
<Rule xsi:type="RuleCollection" Mode="Or">
    <Rule xsi:type="ItemHasKnownEntity" EntityType="Address" />
    <Rule xsi:type="ItemHasKnownEntity" EntityType="MeetingSuggestion" />
    <Rule xsi:type="ItemHasKnownEntity" EntityType="TaskSuggestion" />
</Rule>
```

次の例は、"contoso" という単語を含む URL がメッセージ内に存在するときにアドインをアクティブ化する、 **RegularExpression** 属性を持った **ItemHasKnownEntity** ルールを示しています。




```XML
<Rule xsi:type="ItemHasKnownEntity" EntityType="Url" RegularExpression="contoso" />
```

アクティブ化ルールのエンティティの詳細については、「 [Outlook アイテム内の文字列を既知のエンティティとして照合する](../outlook/match-strings-in-an-item-as-well-known-entities.md)」を参照してください。


## ItemHasRegularExpressionMatch ルール


 **ItemHasRegularExpressionMatch** 複合型は、アイテムの指定プロパティの内容を照合する正規表現を使用するルールを定義します。正規表現に一致するテキストがアイテムの指定プロパティ内に見つかった場合に、Outlook はアドイン バーをアクティブ化してそのアドインを表示します。指定された正規表現と一致したものを取得するには、現在選択されているアイテムを表すオブジェクトの **getRegExMatches** メソッドまたは **getRegExMatchesByName** メソッドを使用します。

次の例は、選択したアイテムの本文に「apple」、「banana」、「coconut」(大文字/小文字は問わない) が含まれる場合に、アドインをアクティブ化する  **ItemHasRegularExpressionMatch** を示しています。




```XML
<Rule xsi:type="ItemHasRegularExpressionMatch" RegExName="fruits" RegExValue="apple|banana|coconut" pPropertyName="BodyAsPlaintext" IgnoreCase="true" />
```

 **ItemHasRegularExpressionMatch** ルールの使用方法の詳細については、「 [正規表現アクティブ化ルールを使用して Outlook アドインを表示する](../outlook/use-regular-expressions-to-show-an-outlook-add-in.md)」を参照してください。


## RuleCollection ルール


 **RuleCollection** 複合型は、複数のルールを 1 つのルールに結合します。 **Mode** 属性を使用すると、コレクション内のルールを論理 OR または論理 AND のどちらで結合するかを指定できます。

論理 AND を指定する場合、アドインは、コレクション内で指定されているすべてのルールにアイテムが一致する場合にのみ表示されます。論理 OR を指定する場合は、コレクションで指定されているルールのいずれか 1 つにでもアイテムが一致すれば、アドインは表示されます。


 >**メモ**  ルールのコレクションの指定に使用する [Rule](http://msdn.microsoft.com/ja-jp/library/c6ce9d52-4b53-c6a6-de7e-c64106135c81%28Office.15%29.aspx) 要素は、抽象 [Rule](http://msdn.microsoft.com/ja-jp/library/bcd7a3a7-9cd4-a270-89e0-5386d1c6df01%28Office.15%29.aspx) 複合型です。 **RuleCollection** 複合型はこの抽象 **Rule** 複合型を拡張しています。したがって、マニフェストでルール コレクションを指定するときは、 **xsi:type** 属性を使用して **RuleCollection** 複合型を指定する必要があります。

 **RuleCollection** ルールを組み合わせて、複雑なルールを作成できます。次の例では、件名か本文に住所が含まれるメッセージあるいは予定表のアイテムをユーザーが表示する場合に、アドインがアクティブ化されます。




```
<Rule xsi:type="RuleCollection" Mode="And">
  <Rule xsi:type="RuleCollection" Mode="Or">
    <Rule xsi:type="ItemIs" ItemType="Message" FormType="Read" />
    <Rule xsi:type="ItemIs" ItemType="Appointment" FormType="Read"/>
  </Rule>
  <Rule xsi:type="ItemHasKnownEntity" EntityType="Address" />
</Rule>
```

次の例では、ユーザーがメッセージを新規作成するときか、件名か本文に住所が含まれる予定を表示するときに、アドインがアクティブ化されます。




```XML
<Rule xsi:type="RuleCollection" Mode="Or"> 
  <Rule xsi:type="ItemIs" ItemType="Message" FormType="Edit" /> 
  <Rule xsi:type="RuleCollection" Mode="And">
    <Rule xsi:type="ItemIs" ItemType="Appointment" FormType="Read" />
    <Rule xsi:type="ItemHasKnownEntity" EntityType="Address" />
  </Rule> 
</Rule>
```


## ルールと正規表現の制約事項


Outlook アドインで満足のゆくエクスペリエンスを提供するには、アクティベーションと API の使用に関するガイドラインに従う必要があります。以下の表に、正規表現とルールに関する一般的な制約事項を示します。ただし、ホストごとの特有のルールも存在します。詳しくは、「 [Outlook アドインのアクティブ化と JavaScript API の制限](../outlook/limits-for-activation-and-javascript-api-for-outlook-add-ins.md)」および「 [Outlook アドインのアクティブ化のトラブルシューティング](../../outlook/troubleshoot-outlook-add-in-activation.md)」をご覧ください。


|
|
|**アドインの要素**|**ガイドライン**|
|:-----|:-----|
|マニフェストのサイズ|256 KB 未満。|
|ルール|15 ルール未満。|
|[ItemHasKnownEntity](http://msdn.microsoft.com/ja-jp/library/87e10fd2-eab4-c8aa-bec3-dff92d004d39%28Office.15%29.aspx)|Outlook リッチ クライアントでは、本文の最初の 1 MB にルールを適用し、残りの部分には適用しません。|
|正規表現|すべての Outlook ホストの [ItemHasKnownEntity](http://msdn.microsoft.com/ja-jp/library/87e10fd2-eab4-c8aa-bec3-dff92d004d39%28Office.15%29.aspx) ルールまたは [ItemHasRegularExpressionMatch](http://msdn.microsoft.com/ja-jp/library/bfb726cd-81b0-a8d5-644f-2ca90a5273fc%28Office.15%29.aspx) ルールの場合:
<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>Outlook アドインのアクティベーション ルールで指定する正規表現は 5 個までにしてください。その制約数を超えるアドインをインストールすることはできません。</p></li><li><p><span class="keyword">getRegExMatches</span> メソッド呼び出しが戻す最初の 50 件以内で期待する結果が得られるような正規表現を指定します。 </p></li><li><p>正規表現で前読みアサーションは指定できますが、後読み (?<=text) および否定後読み (?<!text) は指定できません。</p></li><li><p>一致項目が次の制約を超えない正規表現を指定します。</p><div class="tableSection"><table border="1" width="50%" cellspacing="2" cellpadding="5" frame="lhs"><tbody><tr><th><p>正規表現の長さ制限</p></th><th><p>Outlook リッチ クライアント</p></th><th><p>デバイス用 Outlook Web App</p></th></tr><tr><td><p>アイテムの本文がテキスト形式の場合</p></td><td><p>1.5 KB</p></td><td><p>3 KB</p></td></tr><tr><td><p>アイテムの本文が HTML の場合</p></td><td><p>3 KB</p></td><td><p>3 KB</p></td></tr></tbody></table></div></li></ul>|

## その他のリソース



- [Outlook アドイン](../outlook/outlook-add-ins.md)
    
- [新規作成フォーム用の Outlook アドインを作成する](../outlook/compose-scenario.md)
    
- [Outlook アドインのアクティブ化と JavaScript API の制限](../outlook/limits-for-activation-and-javascript-api-for-outlook-add-ins.md)
    
- [アイテムの種類とメッセージ クラス](http://msdn.microsoft.com/library/15b709cc-7486-b6c7-88a3-4a4d8e0ab292%28Office.15%29.aspx)
    
- [正規表現アクティブ化ルールを使用して Outlook アドインを表示する](../outlook/use-regular-expressions-to-show-an-outlook-add-in.md)
    
- [Outlook アイテム内の文字列を既知のエンティティとして照合する](../outlook/match-strings-in-an-item-as-well-known-entities.md)
    
