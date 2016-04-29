
# ユーザーのメールボックスにアクセスする Outlook アドインのためのアクセス許可を指定する
Outlook アドインのアクセス許可モデルを使用して、アドインに適切なメールボックスのアクセス許可 ( **Restricted**、 **ReadItem**、 **ReadWriteItem**、または  **ReadWriteMailbox**) を要求します。

 _ **適用対象:** apps for Office?| Office Add-ins?| Outlook_


## Outlook アドインのアクセス許可モデル


Outlook アドインでは、マニフェストに必要なアクセス許可のレベルを指定します。使用可能なレベルは  **Restricted**、 **ReadItem**、 **ReadWriteItem**、 **ReadWriteMailbox** です。これらのアクセス許可のレベルは累積的です。 **Restricted** が一番下のレベルで、上の各レベルには下のレベルのアクセス許可がすべて含まれます。 **ReadWriteMailbox** には、サポートするアクセス許可がすべて含まれます。

メール アドインが要求するアクセス許可を、Office ストア からメール アドインをインストールする前に表示できます。Exchange 管理センターで、インストールしたアドインに必要なアクセス許可を表示することもできます。


## 制限付きアクセス許可


 **Restricted** アクセス許可は、最も基本的なレベルのアクセス許可です。マニフェストの [Permissions](http://msdn.microsoft.com/ja-jp/library/c20cdf29-74b0-564c-e178-b75d148b36d1%28Office.15%29.aspx) 要素に **Restricted** を指定することによってこのアクセス許可を要求できます。メール アドインのマニフェストに特定のアクセス許可の要求がない場合、Outlook はそのアドインにこのアクセス許可を既定で割り当てます。


### できること


- アイテムの件名または本文から [特定のエンティティのみを取得](../outlook/match-strings-in-an-item-as-well-known-entities.md#MailAppEntities_Retrieving) (電話番号、アドレス、URL)。
    
- 閲覧フォームまたは新規作成フォームの現在のアイテムが特定のアイテムの種類であることを要求する [ItemIs アクティブ化ルール](../outlook/manifests/activation-rules.md#MailAppDefineRules_ItemIs)を指定、または、選択したアイテムでサポートされる既知のエンティティ (電話番号、アドレス、URL) の小さなサブセットに一致する [ItemHasKnownEntity ルール](../outlook/match-strings-in-an-item-as-well-known-entities.md#MailAppEntities_Activating)を指定。
    
- ユーザーまたはアイテムに関する特定の情報に関連 **しない** プロパティーとメソッドにアクセス。(関連するメンバーのリストは、次のセクションを参照。)
    

### できないこと


- 連絡先、電子メール アドレス、会議の提案、タスクの提案のエンティティで [ItemHasKnownEntity](http://msdn.microsoft.com/ja-jp/library/87e10fd2-eab4-c8aa-bec3-dff92d004d39%28Office.15%29.aspx) ルールを使用。
    
- [ItemHasAttachment](http://msdn.microsoft.com/ja-jp/library/031db7be-8a25-5185-a9c3-93987e10c6c2%28Office.15%29.aspx) ルールまたは [ItemHasRegularExpressionMatch](http://msdn.microsoft.com/ja-jp/library/bfb726cd-81b0-a8d5-644f-2ca90a5273fc%28Office.15%29.aspx) ルールを使用。
    
- ユーザーまたはアイテムの情報に関する次のリストに示すメンバーへのアクセス。このリストのメンバーにアクセスしようとすると、 **null** が返され、Outlook がメール アドインにアクセス許可の引き上げを要求していることを伝えるエラー メッセージが表示されます。
    
      - [item.addFileAttachmentAsync](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md)
    
  - [item.addItemAttachmentAsync](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md)
    
  - [item.attachments](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md)
    
  - [item.bcc](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md)
    
  - [item.body](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md)
    
  - [item.cc](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md)
    
  - [item.from](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md)
    
  - [item.getRegExMatches](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md)
    
  - [item.getRegExMatchesByName](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md)
    
  - [item.optionalAttendees](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md)
    
  - [item.organizer](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md)
    
  - [item.removeAttachmentAsync](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md)
    
  - [item.requiredAttendees](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md)
    
  - [item.resources](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md)
    
  - [item.sender](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md)
    
  - [item.to](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md)
    
  - [mailbox.getCallbackTokenAsync](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.html%28Office.15%29.md)
    
  - [mailbox.getUserIdentityTokenAsync](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.html%28Office.15%29.md)
    
  - [mailbox.makeEwsRequestAsync](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.html%28Office.15%29.md)
    
  - [mailbox.userProfile](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.userProfile.html%28Office.15%29.md)
    
  - [Body](http://dev.outlook.com/reference/add-ins/Body.html%28Office.15%29.md) およびその子メンバーすべて
    
  - [Location](http://dev.outlook.com/reference/add-ins/Location.html%28Office.15%29.md) およびその子メンバーすべて
    
  - [Recipients](http://dev.outlook.com/reference/add-ins/Recipients.html%28Office.15%29.md) およびその子メンバーすべて
    
  - [Subject](http://dev.outlook.com/reference/add-ins/Subject.html%28Office.15%29.md) およびその子メンバーすべて
    
  - [Time](http://dev.outlook.com/reference/add-ins/Time.html%28Office.15%29.md) およびその子メンバーすべて
    

## ReadItem アクセス許可


 **ReadItem** アクセス許可は、アクセス許可モデルの次のレベルのアクセス許可です。マニフェストの **Permissions** 要素に **ReadItem** を指定すると、このアクセス許可を要求できます。


### できること


- 閲覧フォームまたは [新規作成フォーム](../outlook/get-and-set-item-data-in-a-compose-form.md)の現在のアイテムの [すべてのプロパティの読み取り](../outlook/item-data.md)。たとえば、閲覧フォームの [item.to](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md) および新規作成フォームの [item.to.getAsync](http://dev.outlook.com/reference/add-ins/Recipients.html%28Office.15%29.md)。
    
- [アイテムの添付ファイルまたは完全なアイテムを取得するためにコールバック トークンを取得する](../outlook/get-attachments-of-an-outlook-item.md)。
    
- そのアイテムのアドインが設定する [カスタム プロパティの書き込み](30217d63-7615-4f3f-8618-c91e4e60cd43.md)。
    
- アイテムの件名または本文から、サブセットだけでなく [存在する既知のエンティティをすべて取得する](../outlook/match-strings-in-an-item-as-well-known-entities.md#MailAppEntities_Retrieving)。
    
- [ItemHasKnownEntity](http://msdn.microsoft.com/ja-jp/library/87e10fd2-eab4-c8aa-bec3-dff92d004d39%28Office.15%29.aspx) ルールの [既知のエンティティ](../outlook/manifests/activation-rules.md#MailAppDefineRules_ItemHasKnownEntity)、または [ItemHasRegularExpressionMatch](http://msdn.microsoft.com/ja-jp/library/bfb726cd-81b0-a8d5-644f-2ca90a5273fc%28Office.15%29.aspx) ルールの [正規表現](../outlook/manifests/activation-rules.md#MailAppDefineRules_ItemHasRegularExpressionMatch)をすべて使用します。次の例は、スキーマ v1.1 に従っています。選択されたメッセージの件名または本文に既知のエンティティが 1 つ以上ある場合にアクティブ化されるルールを示しています。
    

```XML
<Permissions>ReadItem</Permissions>
    <Rule xsi:type="RuleCollection" Mode="And">
    <Rule xsi:type="ItemIs" FormType = "Read" ItemType="Message" />
    <Rule xsi:type="RuleCollection" Mode="Or">
        <Rule xsi:type="ItemHasKnownEntity" 
            EntityType="PhoneNumber" />
        <Rule xsi:type="ItemHasKnownEntity" EntityType="Address" />
        <Rule xsi:type="ItemHasKnownEntity" EntityType="Url" />
        <Rule xsi:type="ItemHasKnownEntity" 
            EntityType="MeetingSuggestion" />
        <Rule xsi:type="ItemHasKnownEntity" 
            EntityType="TaskSuggestion" />
        <Rule xsi:type="ItemHasKnownEntity" 
            EntityType="EmailAddress" />
        <Rule xsi:type="ItemHasKnownEntity" EntityType="Contact" />
</Rule>
```


### できないこと

 **mailbox.makeEWSRequestAsync**または次の書き込みメソッドにアクセスする。


- [item.addFileAttachmentAsync](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md)
    
- [item.addItemAttachmentAsync](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md)
    
- [item.bcc.addAsync](../reference/outlook/Recipients.md%28Office.15%29.md)
    
- [item.bcc.setAsync](../reference/outlook/Recipients.md%28Office.15%29.md)
    
- [item.body.prependAsync](http://dev.outlook.com/reference/add-ins/Body.html%28Office.15%29.md)
    
- [item.body.setAsync](http://dev.outlook.com/reference/add-ins/Body.html%28Office.15%29.md)
    
- [item.body.setSelectedDataAsync](http://dev.outlook.com/reference/add-ins/Body.html%28Office.15%29.md)
    
- [item.cc.addAsync](../reference/outlook/Recipients.md%28Office.15%29.md)
    
- [item.cc.setAsync](../reference/outlook/Recipients.md%28Office.15%29.md)
    
- [item.end.setAsync](http://dev.outlook.com/reference/add-ins/Time.html%28Office.15%29.md)
    
- [item.location.setAsync](http://dev.outlook.com/reference/add-ins/Location.html%28Office.15%29.md)
    
- [item.optionalAttendees.addAsync](http://dev.outlook.com/reference/add-ins/Recipients.html%28Office.15%29.md)
    
- [item.optionalAttendees.setAsync](http://dev.outlook.com/reference/add-ins/Recipients.html%28Office.15%29.md)
    
- [item.removeAttachmentAsync](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md)
    
- [item.requiredAttendees.addAsync](http://dev.outlook.com/reference/add-ins/Recipients.html%28Office.15%29.md)
    
- [item.requiredAttendees.setAsync](http://dev.outlook.com/reference/add-ins/Recipients.html%28Office.15%29.md)
    
- [item.start.setAsync](http://dev.outlook.com/reference/add-ins/Time.html%28Office.15%29.md)
    
- [item.subject.setAsync](http://dev.outlook.com/reference/add-ins/Subject.html%28Office.15%29.md)
    
- [item.to.addAsync](http://dev.outlook.com/reference/add-ins/Recipients.html%28Office.15%29.md)
    
- [item.to.setAsync](http://dev.outlook.com/reference/add-ins/Recipients.html%28Office.15%29.md)
    

## ReadWriteItem アクセス許可


マニフェストの  **Permissions** 要素に **ReadWriteItem** を指定すると、このアクセス許可を要求できます。作成フォームでアクティブになり、書き込みメソッド ( **Message.to.addAsync** または **Message.to.setAsync**) を使用するメール アドインは、このレベル以上のアクセス許可を使用する必要があります。


### できること


- Outlook で閲覧または新規作成されているアイテムの [すべてのアイテム レベルのプロパティを読み書き](../outlook/item-data.md)。
    
- そのアイテムで [添付ファイルを追加または削除](../outlook/add-and-remove-attachments-to-an-item-in-a-compose-form.md)。
    
- JavaScript API for Office の中でメール アドインに適用される、 **Mailbox.makeEWSRequestAsync** を除くほかのすべてのメンバーの使用。
    

### できないこと

 **Mailbox.makeEWSRequestAsync** を使用。


## ReadWriteMailbox アクセス許可


 **ReadWriteMailbox** アクセス許可は、最上位レベルのアクセス許可です。マニフェストの **Permissions** 要素に **ReadWriteMailbox** を指定すると、このアクセス許可を要求できます。

 **ReadWriteItem** アクセス許可のサポートに加え、 **Mailbox.makeEWSRequestAsync** を使用することで、サポートされる Exchange Web Services (EWS) の操作にアクセスして、次の作業を行うことができます。


- ユーザーのメール ボックスのアイテムのすべてのプロパティーの読み取りと書き込み。
    
- そのメール ボックスのフォルダーまたはアイテムの作成、読み取り、書き込み。
    
- そのメール ボックスからのアイテムの送信。
    
 **mailbox.makeEWSRequestAsync** を使用して、次の EWS の操作にアクセスできます。


- [CopyItem](http://msdn.microsoft.com/ja-jp/library/bcc68f9e-d511-4c29-bba6-ed535524624a%28Office.15%29.aspx)
    
- [CreateFolder](http://msdn.microsoft.com/ja-jp/library/6f6c334c-b190-4e55-8f0a-38f2a018d1b3%28Office.15%29.aspx)
    
- [CreateItem](http://msdn.microsoft.com/ja-jp/library/78a52120-f1d0-4ed7-8748-436e554f75b6%28Office.15%29.aspx)
    
- [FindConversation](http://msdn.microsoft.com/ja-jp/library/2384908a-c203-45b6-98aa-efd6a4c23aac%28Office.15%29.aspx)
    
- [FindFolder](http://msdn.microsoft.com/ja-jp/library/7a9855aa-06cc-45ba-ad2a-645c15b7d031%28Office.15%29.aspx)
    
- [FindItem](http://msdn.microsoft.com/ja-jp/library/ebad6aae-16e7-44de-ae63-a95b24539729%28Office.15%29.aspx)
    
- [GetConversationItems](http://msdn.microsoft.com/ja-jp/library/8ae00a99-b37b-4194-829c-fe300db6ab99%28Office.15%29.aspx)
    
- [GetFolder](http://msdn.microsoft.com/ja-jp/library/355bcf93-dc71-4493-b177-622afac5fdb9%28Office.15%29.aspx)
    
- [GetItem](http://msdn.microsoft.com/ja-jp/library/e3590b8b-c2a7-4dad-a014-6360197b68e4%28Office.15%29.aspx)
    
- [MarkAsJunk](http://msdn.microsoft.com/ja-jp/library/1f71f04d-56a9-4fee-a4e7-d1034438329e%28Office.15%29.aspx)
    
- [MoveItem](http://msdn.microsoft.com/ja-jp/library/dcf40fa7-7796-4a5c-bf5b-7a509a18d208%28Office.15%29.aspx)
    
- [SendItem](http://msdn.microsoft.com/ja-jp/library/337b89ef-e1b7-45ed-92f3-8abe4200e4c7%28Office.15%29.aspx)
    
- [UpdateFolder](http://msdn.microsoft.com/ja-jp/library/3494c996-b834-4813-b1ca-d99642d8b4e7%28Office.15%29.aspx)
    
- [UpdateItem](http://msdn.microsoft.com/ja-jp/library/5d027523-e0bc-4da2-b60b-0cb9fc1fdfe4%28Office.15%29.aspx)
    
サポートされていない操作を使用すると、エラーが返されます。


## その他の技術情報



- [Outlook アドインに関するプライバシー、アクセス許可、セキュリティ](../../docs/develop/privacy-and-security.md)
    
- [Outlook アイテム内の文字列を既知のエンティティとして照合する](../outlook/match-strings-in-an-item-as-well-known-entities.md)
    
