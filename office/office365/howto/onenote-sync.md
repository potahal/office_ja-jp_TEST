---
ms.Toctitle: Subscribe for webhooks
title: "Webhook を購読して変更の通知を取得する"
description: "Webhook を購読することで、ユーザーが OneNote で行う変更を見逃さないようにします。"
ms.ContentId: 0793a8f9-4805-4666-9d45-3b79c278d765
ms.date: February 2, 2016
---

[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]
[!INCLUDE [Add the ONAPI repo styles](../includes/controls/addonapistyles.xml)]


# Webhook を購読して変更の通知を取得する

*__適用対象:__OneDrive のコンシューマー ノートブック*

Webhook サービスを購読することで、ユーザーが OneNote で行う変更を見逃さないようにします。

パブリック エンドポイントを公開する Web アプリケーションまたは Web サービスを持っている場合に、自分の OneDrive ユーザーの、個人の OneNote ノートブックに変更が行われると、ほとんどリアルタイムで通知を受信できます。  

自分のユーザーの 1 人が所有するノートブックに変更が行われると、通知が送信されます。これには、ノートブック階層の任意のレベルでの変更 (ページのコンテンツに対する変更を含む) が含まれます。 

通知には、変更やユーザーに関する詳細情報は含まれません。変更が行われると通知がアラートを送信します。これにより、イベント ハンドラーは特定の詳細に対してクエリを実行できます。Webhook サービスは変更やリソースの特定の種類に対して、詳細に設定されたサブスクリプションをサポートしていません。

次の例に示すように、通知は JSON オブジェクトを含む POST 要求として送信されます。

```json
{
  "value":[
    {
      "subscriptionId":"client-id-of-your-registered-application",
      "userId":"id-of-the-user-who-owns-the-changed-resource" 
    }
  ]
}
```

サービスは次に示す HTTP 状態コードの 1 つを使用して、ただちに通知に応答する必要があります。

- 200 OK
- 202 Accepted
- 204 No Content

応答を受信しない場合は指数関数的にバックオフを 2 から 3 回再実行して、最終的にメッセージを破棄します。

<br />
通知の詳細を次に示します。

- 通知は自分のユーザーが所有するコンテンツに限定されます (変更を行ったユーザーは関係ありません)。自分のユーザーと共有するコンテンツに行われた変更の場合、通知は送信されません。つまり、自分のユーザーではない Bob が自分のユーザーである Alice の所有するノートブックに変更を行った場合、Alice のユーザー ID がついた通知を受信することになります。Alice が Bob のノートブックを変更した場合、通知は受信しません。

- 通知の **userId** は、OneNote API の応答の **X-AuthenticatedUserId** ヘッダーで返される ID と一致します。

- 短期間の間に行われた変更は、1 つの通知に結合される場合があります。  

- 通知は通常、変更後数分以内に送信されます。クライアントでは即座に変更が使用可能になる場合もありますが、API ではこの待機時間により、変更が使用可能になります。


## 通知ワークフロー 

一般的な通知ワークフローの概要は、次のようになります。

1. 自分のユーザーの 1 人が所有するノートブックに変更が行われます。
2. OneNote は登録されたコールバック URL に 1 つ以上の POST 要求を送信します。各 POST 要求は、1 つ以上の変更を表します。
3. サービスは 200、202、または 204 の HTTP 状態コードを使用して、各 POST 要求に応答します。 
3. 通知はコールバック API でイベント ハンドラーをトリガーします。 
3. 詳細な変更のために、OneNote サービスにクエリを実行します。

最後の変更のタイムスタンプに基づいてクエリを実行することは、すべての変更をキャプチャする最良の方法になります。結果から最新の **lastModifiedTime** のタイムスタンプを保存して、次の変更クエリで使用します。

この要求は **lastModifiedTime** プロパティを使用して、保存したタイムスタンプ以降に変更されたページをすべて返します。

```
GET ../me/notes/pages?filter=lastModifiedTime%20ge%20{stored-iso-8601-timestamp}
```

変更についてのクエリを実行した後、最新の **lastModifiedTime** で保存したタイムスタンプを更新します。

>Webhook サービスは、同期メカニズムとして使用することを目的としていません。 通知の送信や受信ができない場合には、変更についてのクエリを定期的に実行する必要があります。  


<a name="subscribe"></a>
## Webhook サービスに登録する方法
OneNote の Webhook サービスに登録するには、次の操作を実行する必要があります。

- OneNote の Webhook サービスから HTTP POST 要求を受信するパブリック エンドポイントのコールバック URL (HTTP または HTTPS)<!-- hosted on the same domain as your app--> を登録します。 Webhook サービスは HTTP リダイレクトに従いません。

- 次に示すアプリケーションのアクセス許可を要求します。 
   - `wl.offline_access` (Microsoft アカウントのアクセス許可)
   - `office.onenote`、`office.onenote_create`、`office.onenote_update_by_app`、または `office.onenote_update` (OneNote API の場合。アプリの実行内容によって異なります)
 
<br />
登録の用意ができたら、[@onenotedev](http://twitter.com/onenotedev) 宛てにお問い合わせください。 チームのメンバーがセットアップに協力します。 

自分のアプリケーションを使用してユーザーが登録を行うときには、ユーザーの代わりに OneNote API の呼び出しを行う必要があります (例: *GET ../me/notes/notebooks*)。 この呼び出しの結果は次のようになります。

- OneNote はコールバック通知のためにユーザーを登録します。
- **X-AuthenticatedUserId** 応答ヘッダーで返されるユーザーの ID を取得して保存できるようになります。


### モデルの有効期限
Webhook 通知では、ユーザーの ID を登録します。特定のユーザーにより行われた各操作は、ユーザーの登録を 6 か月更新します。

6 か月間ユーザーの代わりに OneNote API を呼び出さなかった場合、登録は 6 か月後に非アクティブになります。非アクティブのユーザー (アプリをアンインストールしたユーザーや、アクセス許可を取り消したユーザーを含む) に対する通知は受信しません。変更通知にはユーザーや変更についての詳細は含まれていません。そのため、ユーザーのプライバシーに対する潜在的なリスクは低減されます。非アクティブのユーザーの代わりに OneNote API を呼び出すたびに、非アクティブのユーザーが再登録されます。


<a name="see-also"></a>
## その他のリソース

- [OneNote の開発](../howto/onenote-landing.md)
- [OneNote コンテンツと構造を取得する](../howto/onenote-get-content.md)
- [OneNote デベロッパー センター](http://dev.onenote.com/)
- [OneNote の開発者ブログ](http://go.microsoft.com/fwlink/?LinkID=390183)
- [スタック オーバーフローに関する OneNote の開発の質問](http://stackoverflow.com/questions/tagged/onenote-api+onenote) 
- [OneNote GitHub のリポジトリ](http://go.microsoft.com/fwlink/?LinkID=390178)
