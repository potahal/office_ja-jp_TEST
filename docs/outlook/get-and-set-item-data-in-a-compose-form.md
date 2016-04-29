
# Outlook で新規作成フォームのアイテム データを取得および設定する
新規作成のシナリオで、受信者、件名、本文、予定の場所と時刻を含む Outlook アドインのアイテムのさまざまなプロパティを取得または設定する方法について説明します。

 _ **適用対象:** apps for Office?| Office Add-ins?| Outlook_


## 新規作成アドインの item プロパティの取得と設定


新規作成フォームでは、閲覧フォームで公開されているのと同じ種類のプロパティのほとんど (出席者、受信者、件名、本文など) を取得でき、さらに、閲覧フォームではなく新規作成フォームのみに関連する少数の追加プロパティ (本文、bcc) を取得できます。 

これらのプロパティのほとんどで、Outlook アドインとユーザーはユーザー インターフェイスの同じプロパティを同時に変更できるため、プロパティの取得と設定のメソッドは非同期になっています。表 1 に、アイテムレベルのプロパティ、および新規作成フォームでそれらのプロパティの取得と設定を行う関連する非同期メソッドを示します。 [item.itemType](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md) プロパティと [item.conversationId](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md) プロパティは、ユーザーが変更できないため例外です。閲覧フォームの場合と同様に、新規作成フォームでも、直接親オブジェクトからプログラムを使用してプロパティを取得できます。

JavaScript API for Office で item プロパティにアクセスする以外に、Exchange Web Services (EWS) を使用してアイテム レベルのプロパティにアクセスすることができます。 **ReadWriteMailbox** アクセス許可があれば、 [mailbox.makeEwsRequestAsync](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.html%28Office.15%29.md) メソッドを使用して EWS 操作の [GetItem](http://msdn.microsoft.com/en-us/library/e3590b8b-c2a7-4dad-a014-6360197b68e4%28Office.15%29.aspx) および [UpdateItem](http://msdn.microsoft.com/en-us/library/5d027523-e0bc-4da2-b60b-0cb9fc1fdfe4%28Office.15%29.aspx) にアクセスし、ユーザーのメールボックスのアイテムのさらに多くのプロパティを取得および設定できます。 **makeEwsRequestAsync** は新規作成フォームと閲覧フォームの両方で利用できます。 **ReadWriteMailbox** アクセス許可、Office アドイン プラットフォームを経由した EWS へのアクセスの詳細については、「 [ユーザーのメールボックスにアクセスする Outlook アドインのためのアクセス許可を指定する](../../docs/outlook/privacy/understanding-outlook-add-in-permissions.md)」および「 [Outlook アドインから Web サービスを呼び出す](../outlook/web-services.md)」を参照してください。


**表 1. 新規作成フォームで item プロパティを取得または設定するための非同期メソッド**


|**プロパティ**|**プロパティの種類**|**取得する非同期メソッド**|**設定する非同期メソッド**|
|:-----|:-----|:-----|:-----|
|[bcc](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md)|[Recipients](http://dev.outlook.com/reference/add-ins/Recipients.html%28Office.15%29.md)|[Recipients.getAsync](http://dev.outlook.com/reference/add-ins/Recipients.html%28Office.15%29.md)|[Recipients.addAsync](http://dev.outlook.com/reference/add-ins/Recipients.html%28Office.15%29.md)[Recipients.setAsync](http://dev.outlook.com/reference/add-ins/Recipients.html%28Office.15%29.md)|
|[body](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md)|[Body](http://dev.outlook.com/reference/add-ins/Body.html%28Office.15%29.md)|[Body.getAsync](http://dev.outlook.com/reference/add-ins/Body.html%28Office.15%29.md)|[Body.prependAsync](http://dev.outlook.com/reference/add-ins/Body.html%28Office.15%29.md)[Body.setAsync](http://dev.outlook.com/reference/add-ins/Body.html%28Office.15%29.md)[Body.setSelectedDataAsync](http://dev.outlook.com/reference/add-ins/Body.html%28Office.15%29.md)|
|[cc](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md)|**Recipients**|**Recipients.getAsync**|**Recipients.addAsync** **Recipients.setAsync**|
|[end](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md)|[Time](http://dev.outlook.com/reference/add-ins/Time.html%28Office.15%29.md)|[Time.getAsync](http://dev.outlook.com/reference/add-ins/Time.html%28Office.15%29.md)|[Time.setAsync](http://dev.outlook.com/reference/add-ins/Time.html%28Office.15%29.md)|
|[location](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md)|[Location](http://dev.outlook.com/reference/add-ins/Location.html%28Office.15%29.md)|[Location.getAsync](http://dev.outlook.com/reference/add-ins/Location.html%28Office.15%29.md)|[Location.setAsync](http://dev.outlook.com/reference/add-ins/Location.html%28Office.15%29.md)|
|[optionalAttendees](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md)|**Recipients**|**Recipients.getAsync**|**Recipients.addAsync** **Recipients.setAsync**|
|[requiredAttendees](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md)|**Recipients**|**Recipients.getAsync**|**Recipients.addAsync** **Recipients.setAsync**|
|[start](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md)|**Time**|**Time.getAsync**|**Time.setAsync**|
|[subject](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md)|[Subject](http://dev.outlook.com/reference/add-ins/Subject.html%28Office.15%29.md)|[Subject.getAsync](http://dev.outlook.com/reference/add-ins/Subject.html%28Office.15%29.md)|[Subject.setAsync](http://dev.outlook.com/reference/add-ins/Subject.html%28Office.15%29.md)|
|[to](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md)|**Recipients**|**Recipients.getAsync**|**Recipients.addAsync** **Recipients.setAsync**|

## このセクションの内容


item プロパティの一部を非同期で取得または設定する方法を示す以下の例を参照してください。


- [Outlook の予定またはメッセージを作成するときに受信者を取得、設定、または追加する](../outlook/get-set-or-add-recipients.md)
    
- [Outlook で予定またはメッセージを作成するときに件名を取得または設定する](../outlook/get-or-set-the-subject.md)
    
- [Outlook で予定またはメッセージを作成するときに本文にデータを挿入する](../outlook/insert-data-in-the-body.md)
    
- [Outlook で予定を作成するときに場所を取得または設定する](../outlook/get-or-set-the-location-of-an-appointment.md)
    
- [Outlook で予定を作成するときに時刻を取得または設定する](../outlook/get-or-set-the-time-of-an-appointment.md)
    

## その他の技術情報



- [新規作成フォーム用の Outlook アドインを作成する](../outlook/compose-scenario.md)
    
- [ユーザーのメールボックスにアクセスする Outlook アドインのためのアクセス許可を指定する](../../docs/outlook/privacy/understanding-outlook-add-in-permissions.md)
    
- [Outlook アドインから Web サービスを呼び出す](../outlook/web-services.md)
    
- [読み取りまたは新規作成フォームの Outlook アイテム データを取得および設定する](../outlook/item-data.md)
    


