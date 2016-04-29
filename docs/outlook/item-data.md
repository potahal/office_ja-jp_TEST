
# 読み取りまたは新規作成フォームの Outlook アイテム データを取得および設定する
Outlook アドインからの予定やメッセージのデータをユーザーが表示中または新規作成中であるかどうかを考慮に入れて、そのアイテムのデータを取得または設定する方法を説明します。アイテム レベルのプロパティは JavaScript API for Office、Exchange Server のコールバック トークン、または Exchange Web Services で使用できます。

 _ **適用対象:** apps for Office?| Office Add-ins?| Outlook_


## 閲覧フォームと新規作成フォームで使用できるプロパティ


Office アドイン マニフェスト スキーマのバージョン 1.1 以降、Outlook は、アイテムの表示または作成時にアドインをアクティブ化することができます。アドインが閲覧フォームまたは新規作成フォームのどちらでアクティブ化されるかによって、アイテムでアドインが使用できるプロパティも異なります。たとえば、 [dateTimeCreated](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md) プロパティと [dateTimeModified](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md) プロパティは送信済みのアイテム (アイテムは、その後閲覧フォームで表示されます) のみに定義され、(新規作成フォームで) メッセージを作成する場合これらのプロパティは定義されません。もう 1 つの例は [bcc](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md) プロパティです。このプロパティは、(新規作成フォームで) メッセージを作成する場合にのみ使用でき、閲覧フォームでは使用できません。

表 1 は、メール アドインの閲覧モードと新規作成モードのそれぞれで使用可能な JavaScript API for Office のアイテムレベルのプロパティを示しています。通常、閲覧フォームで使用可能なプロパティは読み取り専用で、新規作成フォームで使用可能なプロパティは値の取得および設定を行えます。ただし、例外として、 [itemId](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md) と [conversationId](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md) プロパティがあります。これらのプロパティは、フォームに関係なく読み取り専用です。新規作成フォームで使用可能な残りのアイテムレベルのプロパティは、アドインとユーザーが同時に同じプロパティの読み取りまたは書き込みを行う可能性があるため、新規作成モードでこれらのプロパティの取得や設定を行うメソッドは非同期です。このため、これらのプロパティが返すオブジェクトの種類も、新規作成フォームと閲覧フォームとで異なります。新規作成フォームで非同期のメソッドを使用してアイテムレベルのプロパティを取得または設定することについて詳しくは、「 [Outlook で新規作成フォームのアイテム データを取得および設定する](../outlook/get-and-set-item-data-in-a-compose-form.md)」をご覧ください。


**表 1. 新規作成フォームと閲覧フォームで使用できるアイテムのプロパティ**


|**アイテムの種類**|**プロパティ**|**閲覧フォームにおけるプロパティのタイプ**|**新規作成フォームにおけるプロパティのタイプ**|
|:-----|:-----|:-----|:-----|
|予定とメッセージ|[dateTimeCreated](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md)|JavaScript  **Date** オブジェクト|このプロパティは使用できません|
|予定とメッセージ|[dateTimeModified](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md)|JavaScript  **Date** オブジェクト|このプロパティは使用できません|
|予定とメッセージ|[itemClass](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md)|String|このプロパティは使用できません|
|予定とメッセージ|[itemId](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md)|String|このプロパティは使用できません|
|予定とメッセージ|[itemType](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md)|[ItemType](http://dev.outlook.com/reference/add-ins/Office.MailboxEnums.html%28Office.15%29.md) 列挙型の文字列|このプロパティは使用できません|
|予定とメッセージ|[attachments](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md)|[AttachmentDetails](http://dev.outlook.com/reference/add-ins/simple-types.html%28Office.15%29.md)|このプロパティは使用できません|
|予定とメッセージ|[body](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md)|[Body](http://dev.outlook.com/reference/add-ins/Body.html%28Office.15%29.md)|[Body](http://dev.outlook.com/reference/add-ins/Body.html%28Office.15%29.md)|
|予定|[end](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md)|JavaScript  **Date** オブジェクト|[Time](http://dev.outlook.com/reference/add-ins/Time.html%28Office.15%29.md)|
|予定|[location](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md)|String|[Location](http://dev.outlook.com/reference/add-ins/Location.html%28Office.15%29.md)|
|予定とメッセージ|[normalizedSubject](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md)|String|このプロパティは使用できません|
|予定|[optionalAttendees](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md)|[EmailAddressDetails](http://dev.outlook.com/reference/add-ins/simple-types.html%28Office.15%29.md)|[Recipients](http://dev.outlook.com/reference/add-ins/Recipients.html%28Office.15%29.md)|
|予定|[organizer](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md)|**EmailAddressDetails**|このプロパティは使用できません|
|予定|[requiredAttendees](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md)|**EmailAddressDetails**|**Recipients**|
|予定|[resources](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md)|String|このプロパティは使用できません|
|予定|[start](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md)|JavaScript  **Date** オブジェクト|**Time**|
|予定とメッセージ|[subject](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md)|String|[Subject](http://dev.outlook.com/reference/add-ins/Subject.html%28Office.15%29.md)|
|メッセージ|[bcc](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md)|このプロパティは使用できません|**Recipients**|
|メッセージ|[cc](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md)|**EmailAddressDetails**|**Recipients**|
|メッセージ|[conversationId](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md)|String|文字列 (読み取り専用)|
|メッセージ|[from](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md)|**EmailAddressDetails**|このプロパティは使用できません|
|メッセージ|[internetMessageId](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md)|Integer|このプロパティは使用できません|
|メッセージ|[sender](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md)|**EmailAddressDetails**|このプロパティは使用できません|
|メッセージ|[to](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md)|**EmailAddressDetails**|**Recipients**|

## Exchange Server のコールバックのトークンを閲覧アドインから使用する


Outlook アドインが閲覧フォームでアクティブ化される場合、Exchange コールバック トークンを取得できます。このトークンをサーバー側のコードで使用して、Exchange Web Services (EWS) を介してアイテムのすべてにアクセスできます。アドイン マニフェストで  **ReadItem** のアクセス許可を指定すると、 [mailbox.getCallbackTokenAsync](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.html%28Office.15%29.md) メソッドを使用した Exchange コールバック トークンの取得、 [mailbox.ewsUrl](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.html%28Office.15%29.md) プロパティを使用したユーザーのメールボックスの EWS エンドポイントの URL の取得、 [item.itemId](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md) による選択したアイテムの EWS ID の取得を行えます。その後、コールバック トークン、EWS エンドポイントの URL、EWS アイテム ID をサーバー側のコードに渡して [GetItem](http://msdn.microsoft.com/en-us/library/e3590b8b-c2a7-4dad-a014-6360197b68e4%28Office.15%29.aspx) の操作にアクセスし、アイテムのその他のプロパティを取得することができます。


## 閲覧アドインまたは新規作成アドインから Exchange Web Services にアクセスする


[mailbox.makeEwsRequestAsync](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.html%28Office.15%29.md) メソッドを使用すると、Exchange Web Services (EWS) の操作である [GetItem](http://msdn.microsoft.com/en-us/library/e3590b8b-c2a7-4dad-a014-6360197b68e4%28Office.15%29.aspx) および [UpdateItem](http://msdn.microsoft.com/en-us/library/5d027523-e0bc-4da2-b60b-0cb9fc1fdfe4%28Office.15%29.aspx) にアドインから直接アクセスすることもできます。これらの操作を使用して、指定したアイテムの多数のプロパティを取得および設定できます。このメソッドは、アドイン マニフェストで **ReadWriteMailbox** のアクセス許可が指定されている限り、アドインが閲覧フォームまたは新規作成フォームのどちらでアクティブ化されたかに関係なく、Outlook アドインで使用できます。 **makeEwsRequestAsync** を使用した EWS の操作へのアクセスについて詳しくは、「 [Outlook アドインから Web サービスを呼び出す](../outlook/web-services.md)」をご覧ください。


## その他の技術情報



- [Outlook アドイン](../outlook/outlook-add-ins.md)
    
- [Outlook で新規作成フォームのアイテム データを取得および設定する](../outlook/get-and-set-item-data-in-a-compose-form.md)
    
- [Outlook アドインから Web サービスを呼び出す](../outlook/web-services.md)
    


