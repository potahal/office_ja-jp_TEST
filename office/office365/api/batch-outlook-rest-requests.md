---
ms.Toctitle: Batch Outlook REST requests (preview)
title: "バッチでの Outlook REST 要求の送信 (プレビュー)"
description: "複数の REST 要求を一括処理して、単一の HTTP 要求を構成し、複数の HTTP 接続を使用するオーバーヘッドを軽減します。"
ms.ContentId: a842854e-46b3-47d9-8899-c27b481f4e78
ms.date: May 16, 2016

---
[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]

# バッチでの Outlook REST 要求の送信 (プレビュー)

[!INCLUDE [Add the Outlook REST API filters--beta default v1 v2 disabled](../includes/controls/addOutlookversion_betadefault_v1v2disabled.xml)] 


 _**適用対象:**Exchange Online | Office 365 | Hotmail.com | Live.com | MSN.com | Outlook.com | Passport.com_

<p class="previewnote">このドキュメントでは、現在プレビュー機能になっている、複数の Outlook REST 要求を 1 つのバッチ要求として送信する方法について説明します。 プレビュー機能は、最終版までに変更される場合があり、それらの機能を使用するコードが動作しなくなる場合もあります。 このため、一般に、運用コードでは運用バージョンの API のみを使用してください。 入手可能な場合、現時点ではバージョン 2.0 が優先バージョンです。</p>

バッチ処理を使用すると、複数の Outlook REST 要求を 1 つの HTTP バッチ要求にまとめられるようになり、HTTP 接続の数とオーバーヘッドを削減できます。 バッチに含まれる要求によって、サインインしているユーザーのデータにアクセスされます。このユーザーは、Office 365 の Azure Active Directory または Microsoft アカウント (Hotmail.com、Live.com、MSN.com、Outlook.com、または Passport.com) でセキュリティ保護されています。 


**注** リファレンスをわかりやすくするため、この記事の残りの部分では **"Outlook.com" をこれらの Microsoft アカウントのドメインを含めた語として**使用しています。 

## 概要

REST 要求を実行するには、クライアントとサーバー間に HTTP 接続が必要であり、一定のオーバーヘッドが生じます。 バッチ処理を使用し、複数の REST API 呼び出しを $batch に対する 1 つの HTTP POST 要求にまとめることで、接続のオーバーヘッドを軽減できます。  
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

複数の Outlook REST 要求を含むバッチを送信する場合は、$batch エンドポイントで POST を実行します。
```
POST https://outlook.office.com/api/{version}/$batch HTTP/1.1
```

**注意**  ルート URL https://outlook.office.com を使用する場合は、最適なパフォーマンスが得られるように、**x-AnchorMailbox** ヘッダーを追加して、そのヘッダーにユーザーのメール アドレスを設定してください。 また、Outlook.com ユーザーのメールボックスが Outlook REST API でまだ有効になっていない状況にも対応してください。その際には、エラー コードの MailboxNotEnabledForRESTAPI と MailboxNotSupportedForRESTAPI を確認します。 詳細については、[ここ](..\api\use-outlook-rest-api.md#OutlookRESTCaution)を参照してください。 

**中国の開発者用のメモ** 

- 中国で _Office 365 用_アプリを開発する場合は、[中国向け Office 365 の API エンドポイント](..\api\o365-china-endpoints.md)を引き続き使用する必要があります。

- 中国で _Outlook.com_ 用アプリを作成する場合は、[v2 アプリ モデル プレビュー](..\api\use-outlook-rest-api.md#RegAuthConverged)と、次の Outlook REST エンドポイントを使用する必要があります。`https://outlook.office.com/api/{version}`


###API のバージョン

現在、バッチ処理はプレビュー状態であり、beta 名前空間でのみ使用可能であるため、`{version}` を `beta` と指定します。

`https://outlook.office.com/api/beta/$batch`

バッチ要求と、そのバッチに含まれる REST 呼び出しは、同じホストと API のバージョンを使用する必要があるため、現時点では、ベータ バージョンで公開されている Outlook REST 呼び出しのみをバッチ処理できます。 

**注意** バッチに含めることができる同じ API 呼び出しの応答が、ベータ版と一般公開用のバージョン (v2.0 や v1.0 など) では異なる点に注意してください。 一般に、ベータ版に含まれるプレビュー状態の API 呼び出しを使用する場合は、アプリの機能を検証して、重大な変更の可能性に対応する計画を立ててください。
 

###対象ユーザー

サインインしているユーザーの同一メールボックスに対する Outlook REST API 呼び出しは、1 つのバッチにグループ化できます。このメールボックスは、Office 365 と Outlook.com のどちらでもかまいません。

### 認証

複数の [Outlook REST API](..\api\use-outlook-rest-api.md#DefineOutlookRESTAPI) 呼び出しを個別の要求として送信する場合と同様に、**Authorization** ヘッダーには有効なアクセス トークンを含める必要があります。 バッチ要求の場合、このヘッダーを指定すると、アクセス トークンによってバッチに含まれるすべての呼び出しに必要なアクセス許可が提供されます。
バッチに含まれる特定の呼び出しが別のアクセス トークンを必要とする場合は、その呼び出しにアクセス トークンを指定することもできます。
 
アクセス トークンを取得するには、アプリを登録して識別し、適切な承認を取得する必要があります。 
効率化された登録と承認のオプションに関する[詳細情報](..\api\use-outlook-rest-api.md#ShortRegAuthWorkflow)を参照してください。 要求のバッチ処理についての理解を深める際には、この点に注意してください。

### バッチ要求のヘッダー

バッチ要求ごとに、次に示すヘッダーを含めます。

```
Host: outlook.office.com
Content-Type: multipart/mixed; boundary=batch_<batchId> 
```

`{batchId}` はバッチを識別するものであり、バッチ内の要求を区切るために使用されます。 これは、GUID または任意の識別文字列のどちらかになります。

適切な場所に、その他のヘッダーを含めることができます。 コンテンツ関連のヘッダー (**Content-Type** など) を除いて、バッチ要求に含まれるヘッダーは、通常、バッチ内のすべての操作に適用されます。 バッチ要求と、そのバッチに含まれる特定の操作の両方にヘッダーを指定すると、その操作には後者が優先されて操作に適用されます。   


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

バッチ内の操作は、データ クエリ、変更、アクション、または関数呼び出しの要求のいずれかになります。 バッチに含まれる操作ごとに完全な URL を繰り返す必要も、すべてのヘッダーを宣言する必要もありませんが、ある操作に特定のヘッダーと要求本文が必要な場合は、それらが含まれていることを確認してください。

### バッチ内での相対 URL
操作ごとに完全な URL を指定する代わりに、相対 URL を含めることもできます。 たとえば、次に示す例の GET 要求では、バッチ要求 `https://outlook.office.com` で指定された次のホストを基準とした URL `/api/beta/me/events`を指定しています。

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
1 つのバッチ要求は、それ自体が 1 つの要求として処理され、それ自体の応答コードを返します。 正しいヘッダーを指定した適切なバッチ要求は、HTTP 200 OK を返します。 ただし、これはバッチ内のすべての要求が成功したことを示しません。 バッチ要求本文内のバッチ要求は、別々に処理され、独自の応答コードおよび応答の本文とエラー メッセージを返します。

サーバーは、バッチ内の操作を任意の順序で実行することがあります。 特定の順序で操作を実行する必要がある場合は、その操作を同一のバッチに含めてはいけません。 その代わりに、1 つの操作を単独で送信し、次のものを送信するまで応答を待機します。

<a name="PreferContinueOnError"></a>

既定では、処理は最初にエラーを返した操作の時点で停止します。次に示すヘッダーを指定することで、処理はエラーを無視して、バッチ内で後続の操作を継続するようになります。

```
Prefer: odata.continue-on-error
```



<a name="NextSteps"> </a>
## 次の手順

アプリケーション開発を開始する準備ができている方にも、単に詳しい情報を必要としている方にも、最適なコンテンツをご用意しています。

- [メール、予定表、および連絡先 REST API の使用を開始します](http://dev.outlook.com/RestGettingStarted)。

- Office 365 の API を実際に試してみるには、対話形式の [API サンドボックス](https://apisandbox.msdn.microsoft.com/)をお使いください。

- サンプルについては、 [こちらをご覧ください](..\howto\starter-projects-and-code-samples.md)。


Office 365 プラットフォームの使い方の詳細については、次のリンク先をご覧ください。

- [Outlook デベロッパー センターの Outlook REST API](http://dev.outlook.com/RestGettingStarted/Overview)

- [Office 365 プラットフォーム上での開発の概要](..\howto\platform-development-overview.md)

- [Office 365 のアプリ認証とリソース承認](..\howto\common-app-authentication-tasks.md)

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
