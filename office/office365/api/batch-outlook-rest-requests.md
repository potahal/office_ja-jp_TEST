---
ms.Toctitle: Batch Outlook REST requests (preview)
title: "バッチでの Outlook REST 要求の送信 (プレビュー)"
description: "ms.TocTitle:バッチ Outlook REST 要求 (プレビュー)Title:バッチでの Outlook REST 要求の送信 (プレビュー)Description:バッチ処理で複数の REST 要求を 1 つの HTTP 要求にまとめて、複数の HTTP 接続を使用するオーバーヘッドを削減する方法。ms.ContentId: a842854e-46b3-47d9-8899-c27b481f4e78ms.topic: リファレンス (API)ms.date:2016 年 5 月 16 日"
ms.ContentId: a842854e-46b3-47d9-8899-c27b481f4e78
ms.date: May 16, 2016

---
[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]

# バッチでの Outlook REST 要求の送信 (プレビュー)

[!INCLUDE [Add the Outlook REST API filters--beta default v1 v2 disabled](../includes/controls/addOutlookversion_betadefault_v1v2disabled.xml)] 


 _**Applies to:** Exchange Online | Office 365 | Hotmail.com | Live.com | MSN.com | Outlook.com | Passport.com_

<p class="previewnote">This documentation covers sending multiple Outlook REST requests as a single batch request which is currently in preview. Preview features are subject to change prior to finalization, and may break code that uses them. プレビュー機能は、最終処理までに変更される場合があります。それらを使用するコードを破棄する場合もあります。このため、一般に、実稼働コードでは実稼働バージョンの API のみを使用してください。可能な場合には、現在バージョン 2.0 が優先バージョンです。</p> If available, v2.0 is currently the preferred version.</p>

バッチ処理を使用すると、複数の Outlook REST 要求を 1 つの HTTP バッチ要求にまとめられるようになり、HTTP 接続の数とオーバーヘッドを削減できます。バッチに含まれる要求は、サインインしているユーザーのデータにアクセスします。このユーザーは、Office 365 の Azure Active Directory または Microsoft アカウント  The requests in a batch access the data of the signed-in user, secured by Azure Active Directory in Office 365, or in a Microsoft account (Hotmail.com, Live.com, MSN.com, Outlook.com, or Passport.com). 


**メモ** リファレンスをわかりやすくするため、この記事の残りの部分では **"Outlook.com" をこれらの Microsoft アカウントのドメインを含めた語として**使用しています。 

## 概要

REST 要求には、クライアントとサーバーの間の HTTP 接続が必要になります。このため、特定の量のオーバーヘッドが発生します。バッチ処理により、複数の REST API 呼び出しは $batch エンドポイントへの 1 つの HTTP POST 要求に統合され、接続のオーバーヘッドを削減できます。 REST 要求には、クライアントとサーバーの間の HTTP 接続が必要になります。このため、特定の量のオーバーヘッドが発生します。バッチ処理により、複数の REST API 呼び出しは $batch エンドポイントへの 1 つの HTTP POST 要求に統合され、接続のオーバーヘッドを削減できます。  
```
POST https://outlook.office.com/api/{version}/$batch
```
1 つのバッチ要求には、最大 20 の個別の REST API 呼び出しを含めることができます。この呼び出しは、データ クエリ (たとえば、GET) または変更操作 (たとえば、POST) になります。[Prefer](#PreferContinueOnError) ヘッダーを指定することで、1 つまたは複数の REST 呼び出しが失敗したときでも、バッチが継続されるようになります。

バッチ処理は、特に大量のデータを同期する場合に役立ちます。同じバッチに含まれる API 呼び出しは、サインインしているユーザーの同一メールボックスに属している異なるリソース (メッセージやイベントなど) にアクセスできます。 

20 件以上の呼び出しが必要な場合や、呼び出しの完了する順序が問題になる場合は、複数のバッチ要求を使用してください。


## 例

単純な例は、バッチ処理を最もわかりやすく示します。次に示すバッチ要求は、2 つの Outlook REST API 呼び出しを 1 つのバッチにまとめています。
1. サインインしているユーザーのすべてイベントを GET する
2. メッセージを POST する

### バッチ要求

```
POST https://outlook.office.com/api/beta/$batch HTTP/1.1

Authorization: Bearer aGFwcHlnQGRr==
Host: outlook.office.com
Content-Type: multipart/mixed; boundary=batch_myBatchId


--batch_myBatchId
Content-Type: application/http
Content-Transfer-Encoding: binary

GET  /api/beta/me/events?$select=subject,organizer    HTTP/1.1


--batch_myBatchId
Content-Type: application/http
Content-Transfer-Encoding: binary

POST  /api/beta/me/sendmail    HTTP/1.1

{
  "Message": {
    "Subject": "Meet for lunch?",
    "Body": {
      "ContentType": "Text",
      "Content": "The new cafeteria is open."
    },
    "ToRecipients": [
      {
        "EmailAddress": {
          "Address": "rufus@contoso.com"
        }
      }
    ]
  }
}


--batch_myBatchId--
```

### バッチ応答

バッチ応答には、バッチ要求と個別の呼び出しごとに個別の応答コードと本文が含まれています (該当する場合)。 

```
HTTP/1.1 200 OK
Content-Type: multipart/mixed; boundary=batchresponse_8faca3a8-183c-4dea-b396-25ac7dd77e09
Content-Length: 12898

--batchresponse_8faca3a8-183c-4dea-b396-25ac7dd77e09
Content-Type: application/http
Content-Transfer-Encoding: binary

HTTP/1.1 200 OK
OData-Version: 4.0
Content-Type: application/json;odata.metadata=minimal;odata.streaming=true;IEEE754Compatible=false;charset=utf-8

{
    "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/Events(Subject,Organizer)",
    "value": [
        {
            "@odata.id": "https://outlook.office.com/api/beta/Users('ddfc984d-b826-40d7-b48b-57002df800e5@1717f226-49d1-4d0c-9d74-709fad664b77')/Events('AAMkAGEtk-hAAA=')",
            "@odata.etag": "W/\"mODEKWhc/Um6lA3uPm7PPAAAX791Eg==\"",
            "Id": "AAMkAGEtk-hAAA=",
            "Subject": "Standup meeting",
            "Organizer": {
                "EmailAddress": {
                    "Address": "rufus@contoso.com",
                    "Name": "Rufus Tallent"
                }
            }
        },

        {
            "@odata.id": "https://outlook.office.com/api/beta/Users('ddfc984d-b826-40d7-b48b-57002df800e5@1717f226-49d1-4d0c-9d74-709fad664b77')/Events('AAMkAGED3CAAA=')",
            "@odata.etag": "W/\"mODEKWhc/Um6lA3uPm7PPAAAD6Ir3g==\"",
            "Id": "AAMkAGED3CAAA=",
            "Subject": "Discuss the campaign",
            "Organizer": {
                "EmailAddress": {
                    "Address": "basil@contoso.com",
                    "Name": "Basil Pearsall"
                }
            }
        }
    ]
}
--batchresponse_8faca3a8-183c-4dea-b396-25ac7dd77e09
Content-Type: application/http
Content-Transfer-Encoding: binary

HTTP/1.1 202 Accepted

--batchresponse_8faca3a8-183c-4dea-b396-25ac7dd77e09--
```

****

## バッチの詳細

### エンドポイント URL

複数の Outlook REST 要求を含むバッチを送信する場合は、$batch エンドポイントに POST を実行します。
```
POST https://outlook.office.com/api/{version}/$batch HTTP/1.1
```

**Attention**  When using the root URL https://outlook.office.com, for optimal performance, add an **x-AnchorMailbox** header and set it to the user's email address. それをユーザーのメール アドレスに設定してください。さらに、Outlook.com ユーザーのメールボックスは Outlook REST API でまだ有効ではないケースを処理します。エラー コード MailboxNotEnabledForRESTAPI および MailboxNotSupportedForRESTAPI を確認してください。詳細については、ここを参照してください。 See more information [here](..\api\use-outlook-rest-api.md#OutlookRESTCaution). 

**中国の開発者用のメモ** 

- 中国で Office 365 のアプリを開発する場合、[中国向け Office 365 の API エンドポイント](..\api\o365-china-endpoints.md)をそのまま使用する必要があります。

- 中国で _Outlook.com 用_アプリを作成する場合は、[v2 アプリ モデル プレビュー](..\api\use-outlook-rest-api.md#RegAuthConverged)と、次の Outlook REST エンドポイントを使用する必要があります。


###API のバージョン

現在、バッチ処理はプレビュー状態であり、beta 名前空間でのみ使用可能であるため、{version}`{version}` を beta`beta` と指定します。

`https://outlook.office.com/api/beta/$batch`

バッチ要求と、そのバッチに含まれる REST 呼び出しは、同じホストと API のバージョンを使用する必要があるため、現時点では、ベータ バージョンで公開されている Outlook REST 呼び出しのみをバッチ処理できます。 

**Attention** Take note that some API calls that you may include in a batch respond differently in the beta version than in versions for general availability, such as v2.0 and v1.0. 一般公開用のバージョン (v2.0 や v1.0 など) では異なる点に注意してください。一般に、ベータ バージョンに含まれるプレビュー状態の API 呼び出しを使用する場合は、アプリの機能を検証して、重大な変更の可能性に対応する計画を立ててください。
 

###対象ユーザー

サインインしているユーザーの同一メールボックスに対する Outlook REST API 呼び出しは、1 つのバッチにグループ化できます。このメールボックスは、Office 365 と Outlook.com のどちらでもかまいません。

### 認証

Similar to sending [Outlook REST API](..\api\use-outlook-rest-api.md#DefineOutlookRESTAPI) calls as individual requests, you should include a valid access token in an **Authorization** request header. If you specify that header for the batch request, then the access token should provide the necessary permission for all the calls in that batch.
Optionally you can specify an access token for a specific call in the batch if that call requires a different access token.
 
アクセス トークンを取得するには、アプリを登録して識別し、適切な承認を取得する必要があります。効率化された登録と承認のオプションに関する詳細情報を参照してください。要求のバッチ処理についての理解を深める際には、この点に留意してください。 
[Find out more](..\api\use-outlook-rest-api.md#ShortRegAuthWorkflow) about some streamlined registration and authorization options for you. Keep this in mind as you learn more about batching requests.

### バッチ要求のヘッダー

バッチ要求ごとに、次に示すヘッダーを含めます。

```
Host: outlook.office.com
Content-Type: multipart/mixed; boundary=batch_<batchId> 
```

{batchId}`{batchId}` はバッチを識別するものであり、バッチ内の要求を区切るために使用されます。これは、GUID または任意の識別文字列のどちらかになります。 It can be a GUID or any distinguishing string.

You can include other headers where appropriate. Except for content-related headers such as **Content-Type**, headers in the batch request generally apply to every operation in the batch. 適切な場所に、その他のヘッダーを含めることができます。コンテンツ関連のヘッダー (Content-Type など) を除いて、バッチ要求に含まれるヘッダーは、通常、バッチ内のすべての操作に適用されます。バッチ要求と、そのバッチに含まれる特定の操作の両方にヘッダーを指定すると、その操作には後者が優先されて操作に適用されます。   


### バッチ要求の本文

バッチ内の各操作は、次に示す行から開始します。
```
--batch_{batchId} 
Content-Type: application/http 
Content-Transfer-Encoding: binary 
```
バッチ内の最後の操作は、次に示す行で終了します。
```
--batch_{batchId}--
```


### バッチに含まれる操作
1 つのバッチ要求に、最大 20 件の操作を指定できます。

An operation in a batch can be a data query, modification, action, or function invocation request. バッチ内の操作は、データ クエリ、変更、アクション、または関数呼び出しの要求のいずれかになります。バッチに含まれる操作ごとに完全な URL を繰り返す必要も、すべてのヘッダーを宣言する必要もありませんが、ある操作に特定のヘッダーと要求本文が必要な場合は、それらが含まれていることを確認してください。

### バッチ内での相対 URL
操作ごとに完全な URL を指定する代わりに、相対 URL を含めることもできます。たとえば、次に示す例の GET 要求では、バッチ要求で指定された次のホストを基準とした URL /api/beta/me/events を指定しています。 操作ごとに完全な URL を指定する代わりに、相対 URL を含めることもできます。たとえば、次に示す例の GET 要求では、バッチ要求で指定された次のホストを基準とした URL /api/beta/me/events`/api/beta/me/events` を指定しています。

```
POST https://outlook.office.com/api/beta/$batch HTTP/1.1

User-Agent: YouRock
Authorization: Bearer {accessToken}
Host: outlook.office.com
Content-Type: multipart/mixed; boundary=batch_<myBatchID>

--batch_{batchID}
Content-Type: application/http
Content-Transfer-Encoding: binary

GET  /api/beta/me/events?$select=subject,organizer    HTTP/1.1

--batch_{batchID>}--

```

### バッチ要求の処理
A batch request is processed on its own as one request and returns its own response code. A well-formed batch request with correct headers returns HTTP 200 OK. This however doesn't mean all the requests in the batch were successful. Each request in the batch request body is in turn processed separately and returns its own response code, and any response body and error message.

The server may perform operations within a batch in any order. サーバーは、バッチ内の操作を任意の順序で実行することがあります。特定の順序で操作を実行する必要がある場合は、その操作を同一のバッチに含めてはいけません。その代わりに、1 つの操作を単独で送信し、次の送信するまで応答を待機します。 Instead, send one operation by itself, wait for the response before sending the next one.

<a name="PreferContinueOnError"></a>

既定では、処理は最初にエラーを返した操作の時点で停止します。次に示すヘッダーを指定することで、処理はエラーを無視して、バッチ内で後続の操作を継続するようになります。

```
Prefer: odata.continue-on-error
```



<a name="NextSteps"> </a>
## 次のステップ

アプリケーション開発を開始する準備ができている方にも、単に詳しい情報を必要としている方にも、最適なコンテンツをご用意しています。

- [メール、予定表、および連絡先 REST API 入門](http://dev.outlook.com/RestGettingStarted)。

- Office 365 の API を実際に試してみるには、対話形式の [API サンドボックス](https://apisandbox.msdn.microsoft.com/)をお使いください。

- サンプルについては、 [こちらをご覧ください](..\howto\starter-projects-and-code-samples.md)。


Office 365 プラットフォームの使い方の詳細については、次のリンク先をご覧ください。

- [Outlook デベロッパー センターの Outlook REST API](http://dev.outlook.com/RestGettingStarted/Overview)

- [Office 365 プラットフォーム上での開発の概要](..\howto\platform-development-overview.md)

- [Office 365 アプリケーションの認証およびリソース承認](..\howto\common-app-authentication-tasks.md)

- [Office 365 API にアクセスできるようにアプリを手動で Azure AD に登録する](..\howto\add-common-consent-manually.md)

- [メール REST API リファレンス](..\api\mail-rest-operations.md)

- [予定表 REST API リファレンス](..\api\calendar-rest-operations.md)

- [連絡先 REST API リファレンス](..\api\contacts-rest-operations.md)

- [タスク REST API (プレビュー)](..\api\task-rest-operations.md) 

- [メール、予定表、連絡先、タスク REST API のリソース リファレンス](..\api\complex-types-for-mail-contacts-calendar.md)

- [Office 365 データ拡張機能 REST API リファレンス](..\api\extensions-rest-operations.md)

- [People REST API リファレンス](..\api\people-rest-operations.md)

- [通知 REST API リファレンス](..\api\notify-rest-operations.md)

- [ユーザー写真 REST API リファレンス](..\api\photo-rest-operations.md)

[!INCLUDE [Enable filtering functionality ](../includes/controls/enablefiltering.xml)]
