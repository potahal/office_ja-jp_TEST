---
ms.Toctitle: Discovery Service REST API reference
title: "探索サービス REST API リファレンス"
description: "Office 365 探索サービス API を使用して、Office 365 アプリが接続できるサービスにアクセスするためのエンドポイントを動的に探索する方法についてのリファレンスです。"
ms.ContentId: 799c9512-7020-4033-b7b8-1e9a2174555d
ms.date: June 9, 2015

---
[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]



# 探索サービス REST API リファレンス

<!--You use Discovery Service to find endpoints for services that you access in an Office 365 APIs application. In this article, you'll find reference information for the Discovery Service APIs.-->


 _**適用対象:**Office 365_

<a name="DiscSvc_RESTinterface"> </a>
## Office 365 探索サービスを使用する

[!INCLUDE [Use Microsoft Graph](../includes/use-msgraph-note.txt)]

探索サービス API と対話するには、HTTP および OData 要求を送信します。 探索サービスでは、**予定表**、**連絡先**、**メール**、**MyFiles** (OneDrive および OneDrive for Business のサービス エンドポイントの場合)、**ノート** (OneNote の場合)、および **RootSite** (SharePoint の場合) の探索をサポートしています。

探索サービスのリソース ID は、`ResourceId = https://api.office.com/discovery/` です。

探索サービス API を使用して、Office 365 API によってアクセスするサービスのエンドポイントを見つける方法については、「[Office 365 API:探索サービスの使用方法](https://code.msdn.microsoft.com/Office-365-APIs-How-to-use-609102ea#content)」および「[Office 365 探索サービスのサンプル](https://github.com/OfficeDev/Office365-Discovery-Service-Sample)」のサンプル コードを参照してください。

**注** 探索サービスは、Office 365 オンライン環境向けの機能のみを提供しています。オンプレミスの展開には利用できません。

### バージョン管理

探索サービスのバージョンは、次のとおりです。

|**探索サービス API エンドポイント**|**説明**|
|:-----|:-----|
|https://api.office.com/discovery/v1.0/me| Office 365 API のリリース済みバージョンのサービスごとに 1 つの API エンドポイントをサポートします。 <br>既定では、OData v4 (http://www.odata.org/documentation/odata-version-4-0/) を返します。</br>|
|https://api.office.com/discovery/v2.0/me| Office 365 API のリリース済みバージョンのサービスごとに複数の API エンドポイントをサポートします。  <br>既定では、OData v4 (http://www.odata.org/documentation/odata-version-4-0/) を返します。</br>|




## 探索サービスの操作

<a name="DiscSvc_FirstSignIn"> </a>
### 最初のサインイン

最初にサインインすると、クライアントにはユーザーがアカウント情報を入力する Web ページが表示されます。アカウント情報を入力すると、探索サービスを続行するために必要なエンドポイントが返されます。これは、ユーザーが最初にアプリケーションを試す際にのみ使用されます。このエンドポイントは、アプリケーションに次の情報を提供します。
   1. ユーザーが属しているクラウドについて
   2. ログインするユーザーをアプリがリダイレクトできる場所
   3. トークンを取得する場所

****

|**パラメーター**|**型**|**説明**|
|:-----|:-----|:-----|
| `scope`|string|スペースで区切られた <i>capability.operation</i> トークンの一覧。 このスコープは、Office 365 の表記方法で表されます。 <br>例: MyFiles.Write or Mail.Read</br>|
|`redirect_uri`|string|承認が完了した後のリダイレクト先 URI。 <br>例: https://contoso.com/continue </br>|
|`lcid`|string| オプション。 電子メールの HRD UI をローカライズするための 10 進数の LCID です。 <br>例: 1031 </br><br>**注意**  この API は、意図的にユーザーの電子メールを受け入れないようにしています。ユーザーの電子メールを現在のドメインから送信すると、ユーザーのプライバシーが侵害される可能性があるためです。</br>|


|**応答**|**説明**|
|:-----|:-----|
|`302` Found |応答本文には、アプリとユーザーに関する値が含まれています。|


|**応答本文の項目**|**説明**|
|:-----|:-----|
|Location: redirect_URI| 承認が完了した後のリダイレクト先 URI。|
|?user_email=... | ユーザーが入力した電子メール アドレス。|
|&amp;account_type=... | 1 - Microsoft アカウント (Live) <br>2 - 組織のアカウント (Office 365)</br>|
|&amp;authorization_service=... | ククライアントが承認コードを取得できるエンドポイント URL。|
|&amp;token_service=... |クライアントがアクセス トークンとリフレッシュ トークンの承認コードを交換可能なエンドポイント URL。|
|&amp;scope=... | ターゲット領域に変換された元のスコープ。クライアントに必要な情報は、Office 365 のスコープの表記方法のみです。ターゲット領域が Live の場合、元の Office 365 のスコープは Live の表記方法に変換されます。|
|&amp;unsupported_scope=... |変換できないスコープ項目がある場合、それらの項目は変更されずに unsupported_scope にコンパイルされます。これは、各承認サービスが自身の表記方法でスコープを理解するために必要です。Office 365 承認サービスではスコープ パラメーターを受け入れないため、scope と unsupported_scope はどちらも空の状態で返されます。|
|&amp;discovery_service=... |クライアントがターゲット サービスを探索できるエンドポイント URL。|
|&amp;discovery_resource=... |探索サービスのリソース ID。 探索サービスに対するトークン要求の一部として、トークン サービスに渡される必要があります。|

**Note** この情報すべては、このユーザー アカウントに対して静的です。そのためクライアントはその情報をキャッシュに入れて再使用し、ユーザーが不要な UI で混乱しないようにする必要があります。

**例: **
```
var firstSignInUri = new Uri(string.Format("https://api.office.com/discovery/v1.0/me/FirstSignIn?redirect_uri={0}&scope={1}", TerminalUriText, Scope));
var terminalUri = new Uri(TerminalUriText);

//Starting authorization
var webAuthResult = await WebAuthenticationBroker.AuthenticateAsync(WebAuthenticationOptions.None, firstSignInUri, terminalUri)
   .AsTask().ConfigureAwait(continueOnCapturedContext: true);

//Authorization finished
If (webAuthResult.ResponseStatus == WebAuthenticationStatus.Success)
{
var userEmail = MyExtractParamter("user_email",webAuthResult.ResponseData);
var accountType = MyExtractParamter("account_type",webAuthResult.ResponseData);
var authorizationService = MyExtractParamter("authorization_service",webAuthResult.ResponseData);
var tokenService = MyExtractParamter("token_service", webAuthResult.ResponseData);
var discoveryService = MyExtractParamter("discovery_service", webAuthResult.ResponseData);
var scope = MyExtractParamter("scope",webAuthResult.ResponseData);
var unsupportedScope = MyExtractParamter("unsupported_scope", webAuthResult.ResponseData);

MyCacheUserInfo(...);
}
```

<a name="DiscSvc_Services"> </a>
### 特定のサービスを探索する

**/Services API** を使用して、特定のサービスのエンドポイントを取得します。

****

|**ヘッダー**|**説明**|
|:-----|:-----|
|`Authorization`| 承認フェーズで取得されたアクセス トークン。 <br>例: Authorization: BEARER 2YotnFZFEjr1zCsicMWpAA... </br>|
|`Accept`| 省略可能。 このヘッダーは、応答ペイロードの形式を制御します。 <br>Atom の場合: application/atom+xml</br><br>JSON の場合: application/json;odata=verbose </br><br>このヘッダーを省略した場合の既定値は、Atom です。 </br><br>例: Accept: application/json;odata=verbose </br>|

|**パラメーター **|**型**|**説明**|
|:-----|:-----|:-----|
|`$select`|string| オプション。 コンマで区切られたオブジェクト プロパティの一覧。 サービスは、選択されたプロパティだけを投影します。 このパラメーターは、アプリで使用されないプロパティをダウンロードしないようにすることで、帯域幅を節約するために使用されます。 http://www.odata.org/docs/ をご覧ください。 <br>例: Capability、ServiceUri</br>|
|`$filter`|string| オプション。元の結果セットをフィルター処理するための OData 述部。アプリで使用しないオブジェクト インスタンスをダウンロードしないようにして、帯域幅を節約するために使用します。使用可能な述部関数については、http://www.odata.org の [Documentation] タブを参照してください。 |

|**応答**|**意味**|**説明**|
|:-----|:-----|:-----|
|`200`| OK |応答本文には、OData 要求に従って投影、フィルター処理、およびエンコードされた [ServiceInfo スキーマ](#DiscSvc_SvcInfoSchema) エントリのリストが含まれます。[ServiceInfo スキーマ](#DiscSvc_SvcInfoSchema)の定義を参照してください。|

**例: **
```
var url = string.Format("https://api.office.com/discovery/v1.0/me/services", discoveryService);

var request = HttpWebRequest.CreateHttp(url);
request.Method = "GET";
request.Headers["Authorization"] = "Bearer " + accessToken;

var response = await request.GetResponseAsync().ConfigureAwait(continueOnCapturedContext: true) as HttpWebResponse;
```

<a name="DiscSvc_AllServices"> </a>
### 探索可能なサービスを調べる


**/allservices** API を使用して、探索可能な機能とそれらの機能が実装するサービスを調べます。**/AllServices** は、匿名要求を受け入れるため、アクセス トークンを必要としません。

****


|**ヘッダー**|**説明**|
|:-----|:-----|
|`Accept`| 省略可能。 このヘッダーは、応答ペイロードの形式を制御します。 <br>Atom の場合: application/atom+xml</br><br>JSON の場合: application/json;odata=verbose </br><br>このヘッダーを省略した場合の既定値は、Atom です。 </br><br>例: Accept: application/json;odata=verbose</br>|

|**パラメーター **|**型**|**説明**|
|:-----|:-----|:-----|
|`$select`|string | オプション。オブジェクト プロパティのコンマ区切りリスト。サービスでは、選択したプロパティのみがプロジェクトの対象になります。アプリで使用しないプロパティをダウンロードしないようにして、帯域幅を節約するために使用します。http://www.odata.org/docs/ を参照してください。例: Capability,ServiceUri|
|`$filter`|string | オプション。元の結果セットをフィルター処理するための OData 述部。アプリで使用しないオブジェクト インスタンスをダウンロードしないようにして、帯域幅を節約するために使用します。使用可能な述部関数については、http://www.odata.org の [Documentation] タブを参照してください。 |

|**応答**|**意味**|**説明**|
|:-----|:-----|:-----|
|`200`| OK |応答本文には、OData 要求に従って投影、フィルター処理、およびエンコードされた [ServiceInfo スキーマ](#DiscSvc_SvcInfoSchema) エントリのリストが含まれます。[ServiceInfo スキーマ](#DiscSvc_SvcInfoSchema)の定義を参照してください。|

**例: **
```
var request = HttpWebRequest.CreateHttp("https://api.office.com/discovery/v1.0/me/services");
request.Method = "GET";
request.Headers["Accept"] = "application/json;odata=verbose";

var response = await request.GetResponseAsync().ConfigureAwait(continueOnCapturedContext: true) as HttpWebResponse;
```

## ServiceInfo スキーマ
<a name="DiscSvc_SvcInfoSchema"> </a>

[/services API](#DiscSvc_Services) と [/allservices API](#DiscSvc_AllServices) は、応答本文で ServiceInfo エントリを使用します。

****

|**プロパティ**|**種類**|**例**|
|:-----|:-----|:-----|
|capability|文字列|MyFiles|
|serviceId|文字列||
|serviceName|文字列|O365_SHAREPOINT|
|serviceEndpointUri|文字列|https://contoso-my.sharepoint.com/personal/alexd_contoso_com |
|serviceResourceId|文字列|https://contoso-my.sharepoint.com|

## その他のリソース
<a name="bk_addresources"> </a>


-  [Office 365 プラットフォーム上での開発の概要](..\howto\platform-development-overview.md)

-  [探索サービス API を使用して Office 365 アプリのエンドポイントを検索する](..\howto\discover-service-endpoints.md)

-  [Office 365 API:探索サービスの使用方法](http://code.msdn.microsoft.com/Office-365-APIs-How-to-use-609102ea)
