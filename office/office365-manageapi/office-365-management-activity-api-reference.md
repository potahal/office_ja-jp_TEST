
# Office 365 管理アクティビティ API リファレンス
Office 365 管理アクティビティ API は、Office 365 と Azure AD のアクティビティ ログから、ユーザー、管理者、システム、およびポリシー アクションとポリシー イベントについての情報を取得するために使用します。 

 
 _**適用対象:** Office 365_


Office 365 と Microsoft Azure Active Directory の監査ログとアクティビティ ログに記録されたアクションとイベントを使用して、監視、分析、およびデータ可視化を提供するソリューションを作成できます。このソリューションにより、企業はそのコンテンツに対して実行されたアクションの可視性を向上させることができます。これらのアクションとイベントは、Office 365 アクティビティ レポートからも入手できます。詳細については、「Office 365 アクティビティ レポートを実行する」を参照してください。 These solutions give organizations greater visibility into actions taken on their content. These actions and events are also available in the Office 365 Activity Reports. For more information, see [Run the Office 365 activity report](https://technet.microsoft.com/en-US/library/ms.o365.cc.auditreporthelp.aspx).

Office 365 管理アクティビティ API は、REAT Web サービスの 1 つであり、任意の言語と、HTTPS および X.509 証明書をサポートするホスト環境を用いて、ソリューションの開発に使用できます。この API は、認証と承認に Azure AD と OAuth2 プロトコルを使用します。アプリケーションからこの API にアクセスする場合は、まず Azure AD で登録し、適切なアクセス許可で設定する必要があります。これにより、アプリケーションで、API を呼び出すために必要な OAuth2 アクセス トークンを要求できます。詳細については、「[Office 365 管理 API の使用を開始する](get-started-with-office-365-management-apis.md)」を参照してください。。

 **注** Office 365 管理アクティビティ API で返されるデータのスキーマと、実装に影響を与える可能性がある既知の問題および今後の変更については、「[Office 365 管理アクティビティ API のスキーマ](office-365-management-activity-api-schema.md)」を参照してください。


## Office 365 管理アクティビティ API の操作

Office 365 管理アクティビティ API は、アクションとイベントを、タイプ別およびコンテンツのソース別に分類されたテナント固有コンテンツ blob に集約します。現時点では、次のコンテンツ タイプがサポートされています。


- Audit.AzureActiveDirectory
    
- Audit.Exchange
    
- Audit.SharePoint
    
- Audit.Sway
    
イベントおよびこれらのコンテンツ タイプに関連付けられているプロパティの詳細については、「[Office 365 管理アクティビティ API のスキーマ](office-365-management-activity-api-schema.md)」を参照してください。

テナントのコンテンツ blob の取得を開始するには、まず目的のコンテンツ タイプのサブスクリプションを作成します。複数のテナントのコンテンツ blob を取得する場合は、テナントごとに 1 つずつ、目的の各コンテンツ タイプの複数のサブスクリプションを作成します。

サブスクリプションを作成したら、ダウンロードできる新しいコンテンツ blob を発見するために定期的にポーリングできます。または Webhook エンドポイントをサブスクリプションに登録して、新しいコンテンツ blob が入手可能になったときにそのエンドポイントで通知を受け取ることができます。


 **注** サブスクリプションを作成した場合、最初のコンテンツ blob がそのサブスクリプションで利用可能になるには、最大で 12 時間かかることがあります。blob コンテンツは、複数のサーバーとデータセンターにまたがってアクションとイベントを収集および集約することで作成します。この分散プロセスの結果として、コンテンツ blob に入れられたイベントとアクションは、必ずしも発生した順序で表示されるとは限りません。コンテンツ blob によっては、それよりも前のコンテンツ blob に入っているものよりも前に発生したアクションとイベントが含まれる場合があります。アクションとイベントの発生時からそれらをコンテンツ blob 内で参照できるようになるまで遅延時間は短縮が図られていますが、発生順に表示されることは保証できません。


## アクティビティ API の操作

すべての API 操作のスコープは単一のテナントであり、API のルート URL にはテナント コンテキストを示すテナント ID が含まれています。テナント ID は、GUID です。GUID を取得する方法の詳細については、「[Office 365 管理 API の使用を開始する](get-started-with-office-365-management-apis.md)」を参照してください。


```
https://manage.office.com/api/v1.0/{tenant_id}/activity/feed/{operation}
```

Webhook に送信される通知には**テナント ID** が含まれるので、同じ Webhook を使用してすべてのテナントの通知を受け取ることができます。

すべての API 操作には、Azure AD から取得したアクセス トークンを使用する認証 HTTP ヘッダーが必要です。アクセス トークンのテナント ID は、API のルート URL 内のテナント ID と一致する必要があり、アクセス トークンには、ActivityFeed.Read 要求 (Azure AD でアプリケーションに対して設定したアクセス許可 [組織のアクティビティ データの読み取り] に相当) が含まれている必要があります。



```
Authorization: Bearer eyJ0e...Qa6wg
```

アクティビティ API は、次の操作をサポートします。


-  **サブスクリプションの開始**。テナントの通知の受け取りとアクティビティ データの取得を開始します。
    
-  **サブスクリプションの停止**。テナントのデータの取得を中止します。
    
-  **現在のサブスクリプションのリストの作成**
    
-  **利用可能なコンテンツのリストの作成**。これには対応するコンテンツの URL も含まれます。
    
-  **通知の受け取り**。この通知は新しいコンテンツが利用可能になったときに Webhook により送信されます。
    
-  **コンテンツの取得**。コンテンツ URL を使用して実行します。
    
-  **通知のリストを作成**。この通知は Webhook によって送信されます。

-  **Retrieve resource friendly names** for  objects in the datafeed identified by guids.
    

### サブスクリプションの開始

この操作は、指定したコンテンツ タイプのサブスクリプションを開始します。指定したコンテンツ タイプのサブスクリプションが既に存在する場合、この操作は次の目的で実行します。


- アクティブな Webhook のプロパティを更新します。
    
- 失敗通知数の超過により無効になった Webhook を有効にします。
    
- 新しい (または null の) 有効期限日を指定することで、有効期限切れの Webhook を再度有効にします。
    
- Webhook を削除します。
    
||**サブスクリプション**|**説明**|
|:-----|:-----|:-----|
|**パス**| `/subscriptions/start?contentType={ContentType}`||
|**パラメーター**|contentType|有効なコンテンツ タイプである必要があります。|
|**本文**|webhook|Optional JSON object with three properties:<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><b>address</b>: Required HTTPS endpoint that can receive notifications.  A test message will be sent to the webhook to validate the webhook before creating the subscription.</p></li><li><p><b>authId</b>: Optional string that will be included as the WebHook-AuthID header in notifications sent to the webhook as a means of identifying and authorizing the source of the request to the webhook.</p></li><li><p><b>expiration</b>: Optional datetime that indicates a datetime after which notifications should no longer be sent to the webhook.</p></li></ul>|
|**応答**|contentType|呼び出しで指定するコンテンツ タイプ。|
||status|サブスクリプションの状態。サブスクリプションが無効の場合、コンテンツを取得したりそのリストを作成したりすることはできません。|
||webhook|Webhook の状態と一緒に、呼び出しに指定される Webhook のプロパティ。Webhook が無効である場合、通知は受け取りませんが、サブスクリプションが有効であれば、コンテンツを取得したりそのリストを作成したりできます。|
要求の例を次に示します。




```

POST {root}/subscriptions/start?contentType=Audit.SharePoint
Content-Type: application/json; utf-8
Authorization: Bearer eyJ0e...Qa6wg

{
    "webhook" : {
        "address": "https://webhook.myapp.com/o365/",
        "authId": "o365activityapinotification",
        "expiration": ""
    }
}

```

応答の例を次に示します。




```

HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{
    "contentType": "Audit.SharePoint",
    "status": "enabled",
    "webhook": {
        "status": "enabled",
        "address":  "https://webhook.myapp.com/o365/",
        "authId": "o365activityapinotification",
        "expiration": null
    }
}

```


### Webhook の検証

/Start 操作が呼び出され、Webhook が指定されると、アクティブ リスナーが通知を受け入れて処理できることを検証するために、指定された Webhook アドレスに検証通知が送信されます。HTTP 200 OK 応答が返信されない場合は、サブスクリプションは作成されません。または、Webhook を既存のサブスクリプションに追加するために /start が呼び出されているものの、HTTP 200 OK の応答が返信されない場合は、Webhook は追加されず、サブスクリプションは変更されません。

要求の例を次に示します。




```
POST {webhook address}
Content-Type: application/json; charset=utf-8
Webhook-AuthID: (webhook authId) if provided)
Webhook-ValidationCode: (random opaque string)

{
    "validationCode": (random opaque string, same as header)
}

```

応答の例を次に示します。




```

HTTP/1.1 200 OK

```


### サブスクリプションの停止

この操作は、指定したコンテンツ タイプのサブスクリプションを停止します。 

サブスクリプションを停止すると、通知を受け取ることはなくなり、利用可能なコンテンツを取得することはできません。サブスクリプションを後で再開すると、その時点から新しいコンテンツにアクセスできるようになります。サブスクリプションが停止して再開するまでの間に使用可能であったコンテンツは取得できません。


||**サブスクリプション**|**説明**|
|:-----|:-----|:-----|
|**パス**| `/subscriptions/stop?contentType={ContentType}`||
|**パラメーター**|contentType|有効なコンテンツ タイプである必要があります。|
|**本文**|(空)||
|**応答**|(空)|||

要求の例を次に示します。




```
POST {root}/subscriptions/stop?contentType=Audit.SharePoint
Authorization: Bearer eyJ0e...Qa6wg

```

応答の例を次に示します。




```
HTTP/1.1 200 OK
```


### 現在のサブスクリプションのリストの作成

この操作は、現在のサブスクリプションのコレクションと、関連する Webhook を返します。


||**サブスクリプション**|**説明**|
|:-----|:-----|:-----|
|**パス**| `/subscriptions/list`||
|**パラメーター**|(なし)||
|**本文**|(空)||
|**応答**|JSON 配列|Each subscription will be represented by a JSON object with three properties:<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><b>contentType</b>:コンテンツ タイプを示します。</p></li><li><p><b>status</b>: Indicates the status of the subscription.</p></li><li><p><b>webhook</b>: Indicates the configured webhook, together with the status (enabled, disabled, expired) of the webhook.  If a subscription does not have a webhook, the webhook property will be present but with null value.</p></li></ul>|
要求の例を次に示します。




```
GET {root}/subscriptions/list
Authorization: Bearer eyJ0e...Qa6wg

```

応答の例を次に示します。




```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

[
    {
        "contentType" : "Audit.SharePoint",
        "status": "enabled",
        "webhook": {
            "status": "enabled",
            "address": "https://webhook.myapp.com/o365/",
            "authId": "o365activityapinotification",
            "expiration": null
        }
    },

    ...

    {
        "contentType": "Audit.Exchange",
        "webhook": null
    }
]

```


### 利用可能なコンテンツのリストの作成

この操作は、指定したコンテンツ タイプの、現在取得可能なコンテンツのリストを作成します。コンテンツは、複数のデータセンターにある複数のサーバーから収集されたアクションとイベントの集約です。コンテンツは、集約が利用可能になった順にリストされされますが、集約内のイベントとアクションは必ずしも連続的にはなりません。サブスクリプションの状態が無効である場合は、エラーが返されます。


||**サブスクリプション**|**説明**|
|:-----|:-----|:-----|
|**パス**| `/subscriptions/content?contentType={ContentType}&amp;startTime={0}&amp;endTime={1}`||
|**パラメーター**|contentType|有効なコンテンツ タイプである必要があります。|
||startTimeendTime|Optional datetimes (UTC) indicating the time range of content to return, based on when the content became available. The time range is inclusive with respect to startTime (startTime <= contentCreated) and exclusive with respect to endTime (contentCreated < endTime), so that non-overlapping, incrementing time intervals can used to page through available content.<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>YYYY-MM-DD</p></li><li><p>YYYY-MM-DDTHH:MM</p></li><li><p>YYYY-MM-DDThh:mm:ss</p></li></ul>開始時刻と終了時刻の両方を指定する (または両方を省略する) 必要があります。その時間差は 24 時間を超えてはならず、開始時刻は過去 7 日以内でなければなりません。 By default, if startTime and endTime are omitted, then the content available in the last 24 hours is returned.|
|**応答**|JSON 配列|The available content will be represented by JSON objects with the following properties:<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><b>contentType</b>:コンテンツ タイプを示します。</p></li><li><p><b>contentId</b>:コンテンツを一意に識別する、符号化文字列です。</p></li><li><p><b>contentUri</b>:コンテンツを取得するときに使用する URL です。</p></li><li><p><b>contentCreated</b>:コンテンツが利用可能になった日付/時刻です。</p></li><li><p><b>contentExpiration</b>:コンテンツ取得の締切となる日付/時刻です。</p></li></ul>|
要求の例を次に示します。




```
GET {root}/subscriptions/content?contentType=Audit.SharePoint
Authorization: Bearer eyJ0e...Qa6wg

```

応答の例を次に示します。




```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

[
    {
        "contentType": "Audit.SharePoint",
        "contentId": "492638008028$492638008028$f28ab78ad40140608012736e373933ebspo2015043022$4a81a7c326fc4aed89c62e6039ab833b$04",
        "contentUri": "https://manage.office.com/api/v1.0/f28ab78a-d401-4060-8012-736e373933eb/activity/feed/audit/492638008028$492638008028$f28ab78ad40140608012736e373933ebspo2015043022$4a81a7c326fc4aed89c62e6039ab833b$04",
        "contentCreated": "2015-05-23T17:35:00Z",
        "contentExpiration": "2015-05-30T17:35:00Z"
    },
    ...
]

```


#### 改ページ

ある時間範囲の利用可能なコンテンツのリストを作成する場合、返される結果の数は、応答のタイムアウトを防ぐために制限されます。指定の時間範囲内に 1 つの応答で返せるよりもより多くの結果がある場合、結果は切り捨てられ、次ページの結果を取得するために使用する URL を示すヘッダーが応答に追加されます。この URL には、元の要求に指定されているのと同じ _startTime_ および _endTime_ パラメーターと、次ページの内部 ID を示すパラメーターが含まれます。元の要求で _startTime_ と _endTime_ が指定されていなかった場合、それらは元の要求が出された時点に先行する 24 時間間隔を反映するように設定されます。


```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
NextPageUrl: https://manage.office.com/api/v1/{tenant_id}/activity/feed/subscriptions/content?contentType=Audit.SharePoint&amp;startTime=2015-10-01&amp;endTime=2015-10-02&amp;nextPage=2015101900R022885001761

```

指定の時間範囲にある利用可能なすべてのコンテンツのリストを作成するために、**NextPageUrl** ヘッダーがない応答を受信するまで、複数のページを取得することが必要な場合があります。


### 通知の受け取り

新しいコンテンツが利用可能になると、サブスクリプションの設定済み Webhook に通知が送信されます。通知にはテナント ID が含まれるので、同じ Webhook を使用して、サブスクリプションがあるすべてのテナントの通知を受け取ることができます。

通知は、SSL 経由の HTTP POST として、サブスクリプションの開始時に指定された Webhook アドレスに送信されます。Webhook 構成に認証 ID が含まれている場合、HTTP ヘッダーWebhook-AuthID として送信されます。HTTP 200 OK 以外の応答はエラーと見なされ、通知が再試行されます。クライアント証明書ベースの認証を要求するように Webhook を設定することもでき、manage.office.com 証明書を使用して認証されます。

要求の本文には、利用可能なコンテンツ blob を表す、1 つ以上の JSON オブジェクトの配列が含まれます。通知を比較的小さいサイズにしておくために、各通知内のコンテンツ blob の数は制限されています。この制限は変更される場合があるので、実装では、固定サイズを前提とするのではなく、配列の長さを照会する必要があります。各オブジェクトには、/content 操作によって返されたのと同じプロパティ、データが属するテナントの GUID、およびサブスクリプションを作成したアプリケーションの GUID が含まれます。これにより、Webhook は、複数のテナントとアプリケーションでの使用中に、コンテキストを確立することができます。


-  **tenantId**:コンテンツが属するテナントの GUID です。
    
-  **clientId**:サブスクリプションを作成したアプリケーションの GUID です。
    
-  **contentType**:コンテンツ タイプを示します。
    
-  **contentId**:コンテンツを一意に識別する、符号化文字列です。
    
-  **contentUri**:コンテンツを取得するときに使用する URL です。
    
-  **contentCreated**:コンテンツが利用可能になった日付/時刻です。
    
-  **contentExpiration**:コンテンツ取得の締切となる日付/時刻です。
    
通知の例を次に示します。




```
POST https://webhook.myapp.com/o365/ 
Content-Type: application/json; utf-8
Webhook-AuthID: o365activityapinotification

[
    {
        "tenantId": "{GUID}"
        "clientId": "{GUID}"
        "contentType": "Audit.SharePoint",
        "contentId": "492638008028$492638008028$f28ab78ad40140608012736e373933ebspo2015043022$4a81a7c326fc4aed89c62e6039ab833b$04",
        "contentUri": "https://manage.office.com/api/v1.0/f28ab78a-d401-4060-8012-736e373933eb/activity/feed/audit/492638008028$492638008028$f28ab78ad40140608012736e373933ebspo2015043022$4a81a7c326fc4aed89c62e6039ab833b$04",
        "contentCreated": "2015-05-23T17:35:00Z",
        "contentExpiration": "2015-05-30T17:35:00Z"
    },
    ...
]

```


### 通知の失敗および再試行

通知システムは、新しいコンテンツが利用可能になったときに通知を送信します。通知の送信時に何度も障害が発生した場合は、再試行メカニズムによって再試行の間隔が指数関数的に長くなります。障害が継続的に発生する場合、Webhook は最終的に無効にされ、通知の送信は完全に停止します。/start 操作を実行すると、無効にされた Webhook を再度有効にできます。


### コンテンツの取得

コンテンツ blob を取得するには、利用可能なコンテンツのリストと Webhook に送信された通知に含まれている、対応するコンテンツ URI に対して GET 要求を行います。返されるコンテンツは、JSON 形式の、1 つ以上のアクションまたはイベントのコレクションになります。

要求の例を次に示します。




```
GET https://manage.office.com/api/v1.0/41463f53-8812-40f4-890f-865bf6e35190/activity/feed/audit/301299007231$301299007231$41463f53881240f4890f865bf6e35190aad2015062920$e1c2ab19858a469fb1f1fd097effffc9$04 HTTP/1.1
Authorization: Bearer eyJ0e...Qa6wg
```

応答の例を次に示します。




```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

[
    {
        "CreationTime": "2015-06-29T20:03:19",
        "Id": "80c76bd2-9d81-4c57-a97a-accfc3443dca",
        "Operation": "PasswordLogonInitialAuthUsingPassword",
        "OrganizationId": "41463f53-8812-40f4-890f-865bf6e35190",
        "RecordType": 9,
        "ResultStatus": "failed",
        "UserKey": "1153977025279851686@contoso.onmicrosoft.com",
        "UserType": 0,
        "Workload": "AzureActiveDirectory",
        "ClientIP": "134.170.188.221",
        "ObjectId": "admin@contoso.onmicrosoft.com",
        "UserId": "admin@contoso.onmicrosoft.com",
        "AzureActiveDirectoryEventType": 0,
        "ExtendedProperties": [
            {
                "Name": "LoginError",
                "Value": "-2147217390;PP_E_BAD_PASSWORD;The entered and stored passwords do not match."
            }
        ],
        "Client": "Exchange",
        "LoginStatus": -2147217390,
        "UserDomain": "contoso.onmicrosoft.com"
    },
    {
        "CreationTime": "2015-06-29T20:03:34",
        "Id": "4e655d3f-35fa-42e0-b050-264b2d255c7a",
        "Operation": "PasswordLogonInitialAuthUsingPassword",
        "OrganizationId": "41463f53-8812-40f4-890f-865bf6e35190",
        "RecordType": 9,
        "ResultStatus": "success",
        "UserKey": "1153977025279851686@contoso.onmicrosoft.com",
        "UserType": 0,
        "Workload": "AzureActiveDirectory",
        "ClientIP": "134.170.188.221",
        "ObjectId": "admin@contoso.onmicrosoft.com",
        "UserId": "admin@contoso.onmicrosoft.com",
        "AzureActiveDirectoryEventType": 0,
        "Client": "Exchange",
        "LoginStatus": 0,
        "UserDomain": "contoso.onmicrosoft.com"
    },
    {
        "CreationTime": "2015-06-29T20:04:55",
        "Id": "b567caf0-088e-4c1c-a4ea-633a1e3d66c8",
        "Operation": "Add User.",
        "OrganizationId": "41463f53-8812-40f4-890f-865bf6e35190",
        "RecordType": 8,
        "ResultStatus": "success",
        "UserKey": "1003BFFD8EC47CA6@contoso.onmicrosoft.com",
        "UserType": 0,
        "Workload": "AzureActiveDirectory",
        "ObjectId": "user001@contoso.onmicrosoft.com",
        "UserId": "admin@contoso.onmicrosoft.com",
        "AzureActiveDirectoryEventType": 0,
        "Actor": [
            {
                "ID": "1cef1fdb-ff52-48c4-8e4e-dfb5ea83d357",
                "Type": 2
            },
            {
                "ID": "admin@contoso.onmicrosoft.com",
                "Type": 5
            },
            {
                "ID": "1003BFFD8EC47CA6",
                "Type": 3
            }
        ],
        "ActorContextId": "41463f53-8812-40f4-890f-865bf6e35190",
        "InterSystemsId": "c2ced078-ad57-4079-a743-5c37f5284790",
        "IntraSystemId": "d1497f7e-15b4-49aa-83ad-11a17ca4a2f4",
        "Target": [
            {
                "ID": "user001@contoso.onmicrosoft.com",
                "Type": 5
            },
            {
                "ID": "10037FFE91510806",
                "Type": 3
            }
        ],
        "TargetContextId": "41463f53-8812-40f4-890f-865bf6e35190"
    }
]

```


### 通知のリスト作成

この操作では、指定したコンテンツ タイプのすべての通知の試行についてのリストが作成されます。コンテンツ タイプへのサブスクリプションを開始するときに Webhook を含めなかった場合は、取得される通知はありません。通知の再試行が障害発生時に行われるため、この操作では同じコンテンツ対する複数の通知が返される場合があり、通知が送信される順序は、コンテンツが利用可能になった順序と必ずしも一致しません (特に障害と再試行が複数回繰り返される場合はそうなります)。この操作は、Webhook と通知に関連する問題の調査に使用できますが、どのコンテンツが現在取得可能であるかを特定するためには使用しないでください。その場合は代わりに、/content 操作を実行します。サブスクリプションの状態が無効である場合は、エラーが返されます。


||**サブスクリプション**|**説明**|
|:-----|:-----|:-----|
|**パス**| `/subscriptions/notifications?contentType={ContentType}&amp;startTime={0}&amp;endTime={1}`||
|**パラメーター**|contentType|有効なコンテンツ タイプである必要があります。|
||startTimeendTime|Optional datetimes (UTC) that indicate the time range of content to return, based on when the content became available. The time range is inclusive with respect to  _startTime_ ( _startTime_ <= contentCreated) and exclusive with respect to _endTime_ ( _contentCreated_ < endTime), so that non-overlapping, incrementing time intervals can used to page through available content.<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>YYYY-MM-DD</p></li><li><p>YYYY-MM-DDTHH:MM</p></li><li><p>YYYY-MM-DDThh:mm:ss</p></li></ul>開始時刻と終了時刻の両方を指定する (または両方を省略する) 必要があります。その時間差は 24 時間を超えてはならず、開始時刻は過去 7 日以内でなければなりません。 By default, if  _startTime_ and _endTime_ are omitted, the content available in the last 24 hours is returned.|
|**応答**|JSON 配列|The notifications will be represented by JSON objects with the following properties: **contentType**: indicates the content type. **contentId**:コンテンツを一意に識別する、符号化文字列です。 **contentUri**:コンテンツを取得するときに使用する URL です。 **contentCreated**:コンテンツが利用可能になった日付/時刻です。 **contentExpiration**:コンテンツ取得の締切となる日付/時刻です。 **notificationSent**: the datetime when the notification was sent. **notificationStatus**: indicates the success or failure of the notification attempt.|
要求の例を次に示します。




```
GET {root}/subscriptions/notifications?contentType=Audit.SharePoint
Authorization: Bearer eyJ0e...Qa6wg

```

応答の例を次に示します。




```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

[
    {
        "contentType": "Audit.SharePoint",
        "contentId": "492638008028$492638008028$f28ab78ad40140608012736e373933ebspo2015043022$4a81a7c326fc4aed89c62e6039ab833b$04",
        "contentUri": "https://manage.office.com/api/v1.0/f28ab78a-d401-4060-8012-736e373933eb/activity/feed/audit/492638008028$492638008028$f28ab78ad40140608012736e373933ebspo2015043022$4a81a7c326fc4aed89c62e6039ab833b$04",
        "contentCreated": "2015-05-23T17:35:00Z",
        "contentExpiration": "2015-05-30T17:35:00Z",
        "notificationSent": "2015-05-23T17:36:00Z",
        "notificationStatus": "success"

    },
    ...
]

```


#### 改ページ

ある時間範囲の通知履歴のリストを作成する場合、返される結果の数は、応答のタイムアウトを防ぐために制限されます。指定の時間範囲内に 1 つの応答で返せるよりもより多くの結果がある場合、結果は切り捨てられ、次ページの結果を取得するために使用する URL を示すヘッダーが応答に追加されます。この URL には、元の要求に指定されているのと同じ _startTime_ および _endTime_ パラメーターと、次ページの内部 ID を示すパラメーターが含まれます。元の要求で _startTime_ と _endTime_ が指定されていなかった場合、それらは元の要求が出された時点に先行する 24 時間間隔を反映するように設定されます。


```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
NextPageUrl: https://manage.office.com/api/v1/{tenant_id}/activity/feed/subscriptions/content?contentType=Audit.SharePoint&amp;startTime=2015-10-01&amp;endTime=2015-10-02&amp;nextPage=2015101900R022885001761

```

指定の時間範囲にある利用可能なすべてのコンテンツのリストを作成するために、**NextPageUrl** ヘッダーがない応答を受信するまで、複数のページを取得することが必要な場合があります。

### Retrieve resource friendly names
This operation retrieves friendly names for objects in the datafeed identified by guids. Currently, "DlpSensitiveType" is the only supported object. 


||**サブスクリプション**|**説明**|
|:-----|:-----|:-----|
|**パス**| `/resources/dlpSensitiveTypes`||
|**パラメーター**|なし||
|**Headers**|Accept-Language|Header to specify the desired language for localized names. For example, use "en-US" for English or "es" for Spanish. The default language (en-US) will be returned if this header is not present.|
|**本文**|(空)||
|**応答**|JSON 配列|The available content will be represented by JSON objects with the following properties:<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><b>id</b>: Indicates the guid of the sensitive information type.</p></li><li><p><b>name</b>: The friendly name of the sensitive information type.</p></li></ul>|

要求の例を次に示します。




```
GET {root}/resources/dlpSensitiveTypes
Authorization: Bearer eyJ0e...Qa6wg
Accept-Language: {language code} 

```

応答の例を次に示します。




```
HTTP/1.1 200 OK

[
    {
        "id": "50842eb7-edc8-4019-85dd-5a5c1f2bb085",
        "name": "CreditCardNumber"
    }, 
    {
        "id": "0e9b3178-9678-47dd-a509-37222ca96b42",
        "name": "EUDebitCardNumber"
    }, 
    ...
    {
    }
]

```



## エラー

サービスでエラーが検出されると、標準の HTTP エラー コードの構文を使用して、エラー応答コードが呼び出し元に報告されます。失敗した呼び出しの本体には、追加情報が単一の JSON オブジェクトとして含められます。完全な JSON エラー本体の例を以下に示します。 


```

{ 
    "error":{ 
        "code":"AF50000",
        "message": "An internal server error occurred. Retry the request."
    } 
}

```


|||
|:-----|:-----|
|**コード**|**メッセージ**|
|AF10001|要求で送信されたアクセス許可セット ({0}) に、必要なアクセス許可 **ActivityFeed.Read が含まれていませんでした。{0} = アクセス トークン内のアクセス許可セット。**<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>{0} = the permission set in the access token.</p></li></ul>|
|AF20001|Missing parameter: {0}. <ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>パラメーターがありません: {0}。{0} = 存在していないパラメーターの名前。</p></li></ul>|
|AF20002|省略可能なパラメーターの型が無効です Expected type: {1}<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>{0} = the name of the invalid parameter.</p></li><li><p>{1} = the expected type (int, datetime, guid).</p></li></ul>|
|AF20003|指定した有効期限 {0} は、過去の日付と時刻に設定されています。{0} = API 呼び出しに渡される有効期限。<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>指定した有効期限 {0} は、過去の日付と時刻に設定されています。{0} = API 呼び出しに渡される有効期限。</p></li></ul>|
|AF20010|URL ({0}) で渡されたテナント ID が、アクセス トークン ({1}) で渡されたテナント ID と一致しません。{0} = URL で渡されたテナント ID。{1} = アクセス トークンで渡されたテナント ID。<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>{0} = tenant ID passed in the URL</p></li><li><p>{1} = tenant ID passed in the access token</p></li></ul>|
|AF20011|指定したテナント ID ({0}) は、システムに存在しないか、削除されています。	{0} = URL で渡されたテナント ID。 <ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>   {0} = tenant ID passed in the URL</p></li></ul>|
|AF20012|指定したテナント ID ({0}) が、システムで正しく構成されていません。	{0} = URL で渡されたテナント ID。 <ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>    {0} = tenant ID passed in the URL</p></li></ul>|
|AF20013|URL ({0}) で渡されたテナント ID は、有効な GUID ではありません。	{0} = URL で渡されたテナント ID。<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p> {0} = tenant ID passed in the URL</p></li></ul>|
|AF20020|指定したコンテンツ タイプが無効です。|
|AF20021|The webhook endpoint {{0}) could not be validated. {1}<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>{0} = webhook address.</p></li><li><p>{1} = "The endpoint did not return HTTP 200." or "The address must begin with HTTPS."</p></li></ul>|
|AF20022|指定したコンテンツ タイプのサブスクリプションがありません。|
|AF20023|The subscription was disabled by {0}.<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>サブスクリプションが {0} によって無効にされました。{0} = "テナント管理" または "サービス管理"。</p></li></ul>|
|AF20030|開始時刻と終了時刻の両方を指定する (または両方を省略する) 必要があります。その時間差は 24 時間を超えてはならず、開始時刻は過去 7 日以内でなければなりません。|
|AF20031|Invalid nextPage Input: {0}.<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>nextPage の入力が無効です: {0}。{0} = URL に渡された次ページ インジケーター。</p></li></ul>|
|AF20050|指定された ID が存在しません。<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>指定したコンテンツ ({0}) は存在しません。{0} = リソース ID またはリソース URL。</p></li></ul>|
|AF20051|Content requested with the key {0} has already expired. Content older than 7 days cannot be retrieved.<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>•    {0} = resource id or resource URL</p></li></ul>|
|AF20052|URL 内のコンテンツの ID {0} は無効です。{0} = リソース ID またはリソース URL。<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>指定したコンテンツ ({0}) は存在しません。{0} = リソース ID またはリソース URL。</p></li></ul>|
|AF20053|Only one language may be present in the Accept-Language header.|
|AF20054|Invalid syntax in Accept-Language header.|
|AF50000|内部エラーが発生しました。要求を再試行してください。|
