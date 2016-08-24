---
ms.Toctitle: Use the Outlook REST API
title: "Outlook REST API の使用"
description: "ms.TocTitle:Outlook REST API の使用Title:Outlook REST API の使用Description:登録や承認、サポートされるエンドポイントのバージョン、ターゲット ユーザー、およびクライアント ライブラリを使用してすべての Outlook REST API ユーザーのメールボックス (メール、予定表、連絡先、ユーザー、データ拡張機能、拡張プロパティ、通知、およびユーザー写真などを含む) にアクセスする方法に関する共通情報。ms.ContentId: ffb867ed-eede-405f-91ff-28276c39e874ms.topic: reference (API) ms.date:2016 年 4 月 22 日"
ms.ContentId: ffb867ed-eede-405f-91ff-28276c39e874
ms.date: July 26, 2016
---
[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]

# Outlook REST API の使用

[!INCLUDE [Add the Outlook REST API filters--v2 default](../includes/controls/addOutlookVersion_v2default.xml)]

 _**Applies to:** Exchange Online | Office 365 | Hotmail.com | Live.com | MSN.com | Outlook.com | Passport.com_

<a name="DefineOutlookRESTAPI"></a>

The Outlook REST API includes the following subsets to allow access to users' mailbox data in Office 365, Hotmail.com, Live.com, MSN.com, Outlook.com, and Passport.com. The table below lists the first version that the core functionality of each subset was made available. Note that within a subset, new features and API can be subsequently added to a later version. For example, the Calendar API was introduced in v1.0, and the feature that suggests meeting times is currently available in only beta but not v1.0 or v2.0. 

|**API サブセット**|**使用可能なバージョン**|
|:-------------|:---------------------|
|[予定表 API](..\api\calendar-rest-operations.md)|v1.0 以降|
|[連絡先 API](..\api\contacts-rest-operations.md)|v1.0 以降|
|[データ拡張機能 API](..\api\extensions-rest-operations.md)|v2.0 以降|
|[拡張プロパティ API](..\api\extended-properties-rest-operations.md)|v2.0 以降|
|[メール API](..\api\mail-rest-operations.md)|v1.0 以降|
|[通知 API](..\api\notify-rest-operations.md)|v2.0 以降|
|[People API](..\api\people-rest-operations.md) (プレビュー)|ベータ版|
|[タスク API](..\api\task-rest-operations.md) (プレビュー)|ベータ版|
|[ユーザー写真 API](..\api\photo-rest-operations.md)|v2.0 以降| 
|[複数の API 呼び出しのバッチ処理](#..\api\batch-outlook-rest-requests.md) (プレビュー)|ベータ版| 

**注意**  

この資料の残りの部分では、Outlook REST API のすべてのサブセットに適用される一般的な情報について説明します。参照しやすくするため、この資料の残りの部分では、 For simplicity of reference, the rest of this article uses **"Outlook.com" to include accounts in the domains Hotmail.com, Live.com, MSN.com, Outlook.com, and Passport.com.**

<a name="ShortRegAuthWorkflow"></a>

## アプリの登録および認証

Outlook の REST API を使用して、ユーザーのメールボックス データにアクセスするには、アプリの登録とユーザー認証を次のように実行する必要があります。
1. 最初に、Outlook REST API にアクセスするアプリを登録します。これで、アプリに API 呼び出しを実装することができます。 

2. 実行時に、ユーザーからの承認を取得し、ユーザーのメールボックスにアクセスするための REST API 要求を実行します。 

現在、アプリの登録とユーザー認証を処理する方法には次の 2 つがあります。
- [Azure AD v2 認証エンドポイントの使用](#RegAuthConverged)
- [Azure Active Directory および OAuth の使用](#RegAuthAzure) 

<a name="RegAuthConverged"> </a>

### Azure AD v2 認証エンドポイント 

Azure AD v2 認証エンドポイントにより、Microsoft の組織アカウントと個人アカウントの両方で動作するアプリの構築が簡単になります。これを使用すると、職場、教育機関、および個人のアカウント ユーザーは、ボタン 1 つでサインインできるようになります。

この集中型プログラミング モデルは、次の内容で構成されている最新の方法です。
- [Microsoft アプリケーション登録ポータル](https://apps.dev.microsoft.com) 
- [v2 認証スコープ](..\howto\authenticate-Office-365-APIs-using-v2.md#bk_Outlookscopes)
- v2 認証エンドポイント
  - https://login.microsoftonline.com/common/oauth2/v2.0/authorize
  - https://login.microsoftonline.com/common/oauth2/v2.0/token
- v2 ADAL ライブラリ。 

この方法では、シームレスなアプリ登録とユーザー認証エクスペリエンスにより、Office 365 および Outlook.com でユーザーのメールボックスのデータにアクセスするための適切なトークンを取得します。Outlook.com のアプリを開発する場合は、この方法を使用する必要があります。 If you are developing an app for Outlook.com, you must use this approach. 

v2 認証エンドポイントでは、アプリの登録時にアクセス許可を要求する代わりに、実行時に対応するユーザーの操作に基づいて、動的にダイアログを表示し、必要なアクセス許可を要求することができます。 

この集中型のプログラミング モデルは、複数のコンポーネントで構成されているため、1 つのコンポーネントを使用する場合、他のコンポーネントも使用する必要があります。詳細については、作業開始の例および「v2 認証エンドポイントを使用した Office 365 および Outlook.com API の認証」を参照してください。 この集中型のプログラミング モデルは、複数のコンポーネントで構成されているため、1 つのコンポーネントを使用する場合、他のコンポーネントも使用する必要があります。詳細については、[作業開始の例](https://dev.outlook.com/RestGettingStarted)および「[v2 認証エンドポイントを使用した Office 365 および Outlook.com API の認証](..\howto\authenticate-Office-365-APIs-using-v2.md)」を参照してください。

**注意** 

- v2 認証エンドポイントを使用するには、アプリは直接 Outlook REST API を呼び出す必要があります。既存の Office 365 クライアント ライブラリは使用しないでください。ライブラリは後で更新され、v2 認証エンドポイントと新規の Outlook REST エンドポイントで動作するようになります。

- [アプリを作成または更新する](#UpgradePath)場合、有効になっている Outlook.com のメールボックスをそのアプリが処理できることと、有効になるのを待機しているそれらのメールボックスに Outlook REST API を使用してアクセスできることを確認します。詳しくは、「このような Outlook.com シナリオのテストおよび処理方法」をご覧ください。 Find out more about [testing and handling such Outlook.com scenarios](#OutlookRESTCaution).
  

<a name="RegAuthAzure"> </a>

### Azure AD および OAuth を使用した登録と認証

これは、Office 365 のメールボックス データにのみアクセスする場合の以前の方法です。Outlook.com でのデータへのアクセスや、Office 365 向けの新しいアプリの作成を計画している場合、代わりに [v2 認証エンドポイント](#RegAuthConverged)を使用してください。 

しばらくの間は、Office 365 のメールボックス データにアクセスする際に、以前のように Azure AD でアプリを登録し、アプリが必要とする[適切な範囲のアクセス許可](..\howto\Application-Manifest.md#AppManifest_ExchangeScopes)を指定することができます。実行時に、アプリは Azure AD および OAuth を使用して、アプリケーションの要求を引き続き認証することができます。詳細については、「Office 365 アプリ認証の概念」を参照してください。アップグレード パスの計画を立てる必要があります。 At runtime, your app can continue to use Azure AD and OAuth to authenticate application requests. You can find details at [Office 365 app authentication concepts](..\howto\common-app-authentication-tasks.md).
You should plan for an [upgrade path](#UpgradePath). 

この手法を使用すると、Office 365 アプリで直接呼び出すことによって、または Office 365 クライアント ライブラリを使用して、Outlook REST API にアクセスすることができます。 

<a name="UpgradePath"></a>

### アップグレード パス

The v2 authentication endpoint has been [promoted from preview to Generally Available (GA) status for Outlook and Outlook.com developers](https://blogs.technet.microsoft.com/ad/2016/02/23/for-developers-the-first-use-cases-of-the-converged-microsoft-account-and-azure-active-directory-programming-model-are-now-ga/). v2 認証エンドポイントは、プレビューから Outlook および Outlook.com 開発者の一般公開 (GA) 状態に昇格されています。Office 365 メールボックスのデータにアクセスする運用環境アプリがある場合、または Office 365 または Outlook.com 向けの_新しい_アプリを作成する場合は、v2 認証エンドポイントを最新の Outlook エンドポイント (https://outlook.office.com/`https://outlook.office.com/`、詳細については以下を参照) と一緒に使用して、組織の Office 365 ユーザーと個人の Outlook.com ユーザーの両方に対して 1 つのコード セットだけを利用することを検討してください。

If you have an in-production app that uses Windows Live API to access Outlook.com mailbox data, you must rewrite the app to use the the v2 authentication endpoint and the Outlook REST API. As Windows Live API is being deprecated for Outlook.com and Outlook.com users get their mailboxes enabled for the Outlook REST API, these users will get HTTP 404 errors when attempting to run such Windows Live API apps. On the other hand, the Outlook REST API opens up new opportunities for you -- you can choose from a list of common languages such as Node, Python, Swift, Java, and so on, write the app once, and capture both Outlook.com _and_ Office 365 users on the web, iOS, Android, or Windows devices. 

**注意** 

運用環境アプリで引き続き Outlook.com メールボックスのデータに_のみ_アクセスする場合は、Windows Live のスコープを使用して、Outlook REST API のほとんどにアクセスすることができます。詳細については、「Windows Live のスコープおよび Outlook REST API を使用して Outlook.com メールボックスのデータにアクセスする」を参照してください。 {0} を参照してください。  
[Windows Live のスコープおよび Outlook REST API を使用して Outlook.com メールボックスのデータにアクセスする](#WindowsLiveScopeMapping)

アプリの登録とユーザー認証で使用する方法や、アプリが Office 365 または Outlook.com メールボックス データのどちらをターゲットにしているかにかかわらず、最新の Outlook REST エンドポイントは運用環境にあるため、REST API 呼び出しでいつでも使用を開始できます。

`https://outlook.office.com/api/{version}/{user_context}`

次の Outlook REST エンドポイントは、しばらくの間は Office 365 メールボックス データでのみ動作します。

`https://outlook.office365.com/api/{version}/{user_context}`

[サポートされる REST 操作、エンドポイント](#SupportedEndpoints)および[バージョン](#SupportedVersions)について詳しくは、以下を参照してください。 

<a name="OutlookRESTCaution"></a>

**注意**

- Basic authorization is no longer supported by the Outlook REST API endpoint `https://outlook.office.com`. Outlook REST API エンドポイント https://outlook.office.com では、基本認証はサポートされなくなりました。新しい Outlook REST API エンドポイントを使用するアプリの認証および承認を行う場合は、v2 認証エンドポイントまたは Azure AD を使用してください。  
- 新しい Outlook REST エンドポイントを使用する場合に最適なパフォーマンスを得るには、要求ごとに x-AnchorMailbox`x-AnchorMailbox` ヘッダーを追加し、それをユーザーの電子メール アドレスに設定します。例: 例: `x-AnchorMailbox:john@contoso.com`
- Outlook REST API での Outlook.com のメールボックスの有効化は一定期間行われるため、既存の Outlook.com アカウントを有効にするには時間がかかる場合があります。既に有効になっている Outlook.com メールボックスのデータにアクセスするアプリをテストする場合、outlookdev@microsoft.com に電子メールを送信することによって、新しい有効な Outlook.com 開発者プレビュー アカウントを要求できます。 Outlook REST API での Outlook.com のメールボックスの有効化は一定期間行われるため、既存の Outlook.com アカウントを有効にするには時間がかかる場合があります。既に有効になっている Outlook.com メールボックスのデータにアクセスするアプリをテストする場合、outlookdev@microsoft.com に電子メールを送信することによって、新しい有効な Outlook.com 開発者プレビュー アカウントを要求できます。 
- アプリが Outlook.com メールボックス データにアクセスする場合は、ユーザーのメールボックスが Outlook REST API でまだ有効になっていないシナリオを処理する必要があります。このような状況で REST 要求を行うと、次のようなエラーが発生します。
  - HTTP エラー:404
  - エラー コード:MailboxNotEnabledForRESTAPI または MailboxNotSupportedForRESTAPI 
  - エラー メッセージ:"REST API はこのメールボックスでまだサポートされていません。
- v2 認証エンドポイントを使用するコード サンプルについては、[dev.outlook.com](https://dev.outlook.com/RestGettingStarted) を参照してください。  

<a name="WindowsLiveScopeMapping"></a>

**Windows Live のスコープおよび Outlook REST API を使用して Outlook.com メールボックスのデータにアクセスする**

Outlook.com の運用環境アプリで Windows Live のスコープを使用し、Office 365 メールボックスのデータを対象にしない場合は、ほとんどの Outlook REST API でこれらの Windows Live のスコープをそのまま使用することができます。次の表は、Windows Live のスコープが Outlook REST API スコープをマップする方法を示しています。Mail.Read 用の Windows Live のスコープの直接マッピングは存在しないことに注意してください。アプリは wl.imap を使用して、Mail.Read を必要とするこれらの Outlook REST API 操作にアクセスすることができます。 The table below shows how Windows Live scopes map to the Outlook REST API scopes. Note that there isn't a Windows Live scope direct mapping for Mail.Read; your app can use wl.imap to access those Outlook REST API operations that require Mail.Read.

|**Windows Live のスコープ**|**Outlook REST API のスコープ**|
|:-----|:-----|
|wl.basic | User.Read, Contacts.Read |
|wl.calendars | Calendars.Read |
|wl.calendars_update | Calendars.ReadWrite |
|wl.contacts_create | Contacts.ReadWrite |
|wl.contacts_calendars | Calendars.Read |
|wl.emails | User.Read |
|wl.events_create | Calendars.ReadWrite |
|wl.imap | Mail.ReadWrite, Mail.Send |


****

<a name="SupportedEndpoints"></a>

## サポートされている REST 処理およびエンドポイント

Outlook REST API を操作するには、サポートされているメソッド (GET、POST、PATCH、または DELETE) を使用する HTTP 要求を送信します。POST と PATCH 要求の本文とサーバー応答は、JSON ペイロードで送信されます。

パス URL リソース名とクエリ パラメーターは、大文字と小文字が区別されません。ただし、割り当てた値、エンティティ ID、その他の base64 でエンコードされた値では大文字と小文字を区別します。

すべての Outlook REST API 要求で次の新しいルート URL 形式を使用します。

 `https://outlook.office.com/api/{version}/{user_context}`

適切なユーザー認証を使用した場合、このルート URL の REST API は、Office 365 および Outlook.com のメールボックス データへのアクセスを提供します。この記事の残りの部分では、このエンドポイントの REST API について説明します。 

既存のルート URL を使用するコードがある場合:

 `https://outlook.office365.com/api/{version}/{user_context}`
 
コードはしばらくの間はOffice 365 でそのまま機能しますが、新しいルート URL への切り替えを計画する必要があります。

**Attention**  For optimal performance, when making a REST request using the new root URL, add an `x-AnchorMailbox` header and set it to the user's email address. それをユーザーのメール アドレスに設定してください。さらに、Outlook.com ユーザーのメールボックスは Outlook REST API でまだ有効ではないケースを処理します。エラー コード MailboxNotEnabledForRESTAPI および MailboxNotSupportedForRESTAPI を確認してください。詳細については、ここを参照してください。 See more information [here](..\api\use-outlook-rest-api.md#OutlookRESTCaution).  

### 中国の開発者用のメモ 

- 中国で Office 365 のアプリを開発する場合、[中国向け Office 365 の API エンドポイント](..\api\o365-china-endpoints.md)をそのまま使用する必要があります。

- 中国で Outlook.com のアプリを作成する場合、 [v2 認証エンドポイント](..\api\use-outlook-rest-api.md#RegAuthConverged)を試し、次の新しい Outlook REST エンドポイントを使用してください。

<a name="SupportedVersions"> </a>

##サポートされる API のバージョン:

{version}`{version}` は、指定したルート URL の Outlook REST API のバージョンを表します。次のいずれかの値を指定できます。 次の場所の種類のいずれかを指定できます。 

- `v1.0`. v1.0これは、一般提供 (GA) の状態で最も古いバージョンであり、運用コードで使用できます。URL の例は http://outlook.office.com/api/v1.0/me/events です。 An example URL is `http://outlook.office.com/api/v1.0/me/events`. 

- `v2.0`. v2.0このバージョンは、GA の状態でも、運用コードでも使用することができます。URL の例は An example URL is `http://outlook.office.com/api/v2.0/me/events`. This version includes changes that may break v1.0, and additional API sets that have matured and been promoted from preview to GA status. 

- `beta`. beta:このバージョンはプレビューであるため、運用コードでは使用しないでください。URL の例は An example URL is `http://outlook.office.com/api/beta/me/events`. betaこのバージョンはプレビュー状態であるため、運用コードでは使用しないでください。exampleURL は http://outlook.office.com/api/beta/me/events です。このバージョンには、GA 段階の最新の API と、プレビュー段階の追加 API セットが含まれており、最終処理までに変更される場合があります。

ほとんどの Outlook REST API の REST 操作のはサポートされており、上に一覧表示されているバージョンと同様の方法で動作します。 

##ターゲット ユーザー

Outlook REST API 要求は常に現在のユーザーに代わって実行されますが、ユーザー写真 REST API を除いて、{user_context}`{user_context}` は現在サインインしているユーザーになります。

ユーザー写真 REST API では、{user_context}`{user_context}` はサインインしているユーザー、またはユーザー ID で指定されているユーザーになります。 

Outlook REST API のセットにかかわらず、REST _要求_で {user_context}`{user_context}` を次のいずれかの方法で指定できます。 

- With the `me` shortcut: `/api/{version}/me`. The root URL becomes `https://outlook.office.com/api/{version}/me`.


<!-- ==================================== Start beta content ==================================================== -->

[!INCLUDE [BEGIN Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

- サインイン ユーザーの UPN またはプロキシ アドレスを渡す users/{upn}`users/{upn}` 形式を使用する (例: /api/beta/users/sadie@contoso.com`https://outlook.office.com/api/beta/users/sadie@contoso.com`)。この例では、ルート URL は https://outlook.office.com/api/beta/users/sadie@contoso.com になります。

- users/{AAD_userId@AAD_tenandId}`users/{AAD_userId@AAD_tenandId}` 形式では、ユーザー ID および Azure Active Directory のテナント ID を使用します。たとえば、users/ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77 です。この例では、ルート URL は次になります。 例: `users/ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77`
The root URL would become `https://outlook.office.com/api/beta/users/ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77`.

どちらを使用するかは好みの問題です。 

サーバー _responses_ では、ユーザー コンテキストは users/{AAD_userId@AAD_tenandId} 形式で識別されます。


[!INCLUDE [END Outlook beta section](../includes/controls/outlookrestapibetasection.xml)]

<!-- ==================================== End BETA content ====================================================== -->


<!-- ==================================== Start v2 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

- サインイン ユーザーの UPN またはプロキシ アドレスを渡す users/{upn}`users/{upn}` 形式を使用する (例: /api/beta/users/sadie@contoso.com`https://outlook.office.com/api/beta/users/sadie@contoso.com`)。この例では、ルート URL は https://outlook.office.com/api/beta/users/sadie@contoso.com になります。

- users/{AAD_userId@AAD_tenandId}`users/{AAD_userId@AAD_tenandId}` 形式では、ユーザー ID および Azure Active Directory のテナント ID を使用します。たとえば、users/ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77 です。この例では、ルート URL は次になります。 例: `users/ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77`
The root URL would become `https://outlook.office.com/api/beta/users/ddfcd489-628b-40d7-b48b-57002df800e5@1717622f-1d94-4d0c-9d74-709fad664b77`.

どちらを使用するかは好みの問題です。 

サーバー _responses_ では、ユーザー コンテキストは users/{AAD_userId@AAD_tenandId} 形式で識別されます。


[!INCLUDE [END Outlook v2 section](../includes/controls/outlookrestapiv2section.xml)]

<!-- ==================================== End v2 content ======================================================== -->



<!-- ==================================== Start v1 content ====================================================== -->

[!INCLUDE [BEGIN Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

- サインイン ユーザーの UPN またはプロキシ アドレスを渡す users/{upn}`users/{upn}` 形式を使用する (例: /api/beta/users/sadie@contoso.com`https://outlook.office.com/api/v1.0/users/sadie@contoso.com`)。この例では、ルート URL は https://outlook.office.com/api/beta/users/sadie@contoso.com になります。

どちらを使用するかは好みの問題です。 サーバー _responses_ では、ユーザー コンテキストは users/{AAD_userId@AAD_tenandId}`/api/v1.0/users('sadie@contoso.com')` 形式で識別されます。

[!INCLUDE [END Outlook v1 section](../includes/controls/outlookrestapiv1section.xml)]

<!-- ==================================== End v1 content ======================================================== -->

## コレクション内のアイテムへのアクセス

Outlook REST API は、エンティティ コレクションにアイテムを指定する 2 つの方法をサポートします。いずれも同様に評価されるため、使用は好みの問題です。

- コレクション後の URL セグメント (例: /api/{version}/me/events/AAMkAGI3...`/api/{version}/me/events/AAMkAGI3...`)
    
- かっこ内で文字列を引用符で囲む (例: /api/{version}/me/events('AAMkAGI3...')`/api/{version}/me/events('AAMkAGI3...')`)


****

## クライアント ライブラリを使用した Outlook REST API へのアクセス

アプリが、アプリの登録および認証に Azure AD および OAuth を使用する場合、Office 365 API クライアント ライブラリは、REST API と対話しやすくなります (.NET、JavaScript、Android、および iOS)。認証トークンを管理し、Office 365 のデータの照会と使用に必要なコードを簡略化するのに役立ちます。 They help manage authentication tokens and simplify the code needed to query and consume data on Office 365.


**注意** クライアント ライブラリは後で更新され、[v2 認証エンドポイント](..\api\use-outlook-rest-api.md#RegAuthConverged)で 登録、承認、および Outlook.com を実行できるようになります。Outlook.com メールボックスの データにアクセスする場合は、REST API を直接呼び出します。


.NET または JavaScript ライブラリは、[Office 365 API Tools for Visual Studio の最新バージョンに含まれています。または、Windows ストア アプリ用スタート プロジェクト](http://aka.ms/clientlibrary)をすぐに試していただくこともできます。 .NET または JavaScript ライブラリは、Office 365 API Tools for Visual Studio の最新バージョンに含まれています。または、[Windows ストア アプリ用スタート プロジェクト](http://aka.ms/o365-apis-start-windows)をすぐに試していただくこともできます。

.NET または JavaScript クライアント ライブラリを使用して Outlook REST API にアクセスするには、アクセス トークンを取得し、Outlook サービス クライアントを取得する必要があります。その後、非同期クエリを送信して予定表データを操作できます。

アクセス トークンを取得する  Outlook サービス クライアントを取得する

<a name="GetAuthToken"> </a>
### アクセス トークンを取得する

認証に使用するアクセス トークンを取得します。クライアント ID と承認 URI は、Microsoft Azure Active Directory にアプリを登録するときに割り当てられます。 

**Note** This pattern applies only to code that runs on Windows devices. 注: このパターンは、Windows デバイス上で実行されるコードにのみ適用されます。Windows ストア アプリでは、アプリケーションを登録するときに ClientID および AuthorizationUri の値がプロジェクトの App.xaml ファイルに追加されます。
AuthorizationUri は、CommonAuthority 変数のホスト名として使用されます。

<!-- BEGINSECTION class="tabbedCodeSnippets" -->

```cs
// Properties of the native client app. Get the ClientId from the resources section of the App.xaml file.
public const string ClientID = Application.Current.Resources["ida:ClientID"] as String; 
// Get the _returnUri from app settings.
public static Uri _returnUri = Windows.Security.Authentication.Web.WebAuthenticationBroker.GetCurrentApplicationCallbackUri();

// Properties used to communicate with a Windows Azure AD tenant.  
public const string CommonAuthority = "https://login.windows.net/Common"; 
public const string DiscoveryResourceId = "https://api.office.com/discovery/"; 

//Store authority in application data so that it isn't tied to the lifetime of the access token.
private static ApplicationDataContainer _settings = ApplicationData.Current.LocalSettings;
private static string LastAuthority
{
    get
    {
        if (_settings.Values.ContainsKey("LastAuthority") && _settings.Values["LastAuthority"] != null)
        {
            return _settings.Values["LastAuthority"].ToString();
        }
        else
        {
            return string.Empty;
        }

    }

    set
    {
        _settings.Values["LastAuthority"] = value;
    }
}

public static AuthenticationContext _authenticationContext { get; set; } 
private static async Task<string> GetTokenHelperAsync(AuthenticationContext context, string resourceId)
{
    string accessToken = null;
    AuthenticationResult result = null;

    result = await context.AcquireTokenAsync(resourceId, ClientID, _returnUri);

    accessToken = result.AccessToken;
    //Store authority in application data.
    _settings.Values["LastAuthority"] = context.Authority;

    return accessToken;
}
```
```javascript 
var authContext;
var authToken; // for use with creating an outlookClient later.
authContext = new O365Auth.Context();
authContext.getIdToken("https://outlook.office365.com/")
   .then((function (token) {
       authToken = token;
       // The auth token also carries additional information. For example:  
       userName = token.givenName + " " + token.familyName;
   }).bind(this), function (reason) {
       console.log('Failed to login. Error = ' + reason.message);
   });

```
<!-- ENDSECTION -->

<a name="GetClient"> </a>
###Outlook サービス クライアントを取得する


Get the **OutlookServicesClient** object. OutlookServicesClient オブジェクトを取得します。このコードは、Outlook サービス クライアントを使用する他のメソッドから呼び出すことができます。

<!-- BEGINSECTION class="tabbedCodeSnippets" -->

```cs
public static async Task<OutlookServicesClient> CreateOutlookClientAsync(string capability)
{
    try
    {
        //First, look for the authority used during the last authentication.
        //If that value is not populated, use CommonAuthority.
        string authority = null;
        if (String.IsNullOrEmpty(LastAuthority))
        {
            authority = CommonAuthority;
        }
        else
        {
            authority = LastAuthority;
        }
        // Create an AuthenticationContext using this authority.
        _authenticationContext = new AuthenticationContext(authority);
        
        //See the Discovery Service Sample (https://github.com/OfficeDev/Office365-Discovery-Service-Sample)
        //for an approach that improves performance by storing the discovery service information in a cache.
        DiscoveryClient discoveryClient = new DiscoveryClient(
            async () => await GetTokenHelperAsync(_authenticationContext, DiscoveryResourceId));

        // Get the specified capability ("Calendar").
        CapabilityDiscoveryResult result = 
            await discoveryClient.DiscoverCapabilityAsync(capability);
        var client = new OutlookServicesClient(
            result.ServiceEndpointUri,
            async () => 
                await GetTokenHelperAsync(_authenticationContext, result.ServiceResourceId));
        return client;
    }
    catch (Exception)
    {
        if (_authenticationContext != null && _authenticationContext.TokenCache != null)
        _authenticationContext.TokenCache.Clear();
        return null;
    }
}
```
```javascript 
// Once the authToken has been acquired, create an outlookClient. One place to do this is inside of the
//    ".then" function callback of authContext.getIdToken(...) above.
var outlookClient = new Microsoft.OutlookServices.Client('https://outlook.office365.com/api/v1.0', authToken.getAccessTokenFn('https://outlook.office365.com'));
```
<!-- ENDSECTION -->

****


<a name="ShutdownEarlierPreviewEndpoint"></a>
## 以前のプレビュー エンドポイントの終了

If you are already using `https://outlook.office.com/api` or `https://outlook.office365.com/api` as the root URL  
to access Outlook REST API, this deprecation doesn't affect you. Read on if you are still using the earlier preview endpoint `https://outlook.office365.com/ews/odata`.

Outlook REST API は、2014 年 10 月に、初回プレビューから v1.0 の一般提供に移行されました。この移行を完了するため、従来のプレビュー エンドポイント https://outlook.office365.com/ews/odata を  To complete this transition, we are finally shutting down the old preview endpoint `https://outlook.office365.com/ews/odata` on **October 15, 2015**.

従来のエンドポイント

`https://outlook.office365.com/ews/odata` 

を使用したすべてのアプリおよびサービスを

`https://outlook.office.com/api/v1.0` 

2015 年 10 月 15 日をもって終了することになりました。 2015 年 10 月 15 日までに移行する必要があります。v1.0 は、その日付までに移行する必要がある最小限の一般提供 (GA) エンドポイントです。 

Alternatively, you can use the preferred GA endpoint v2.0 (`https://outlook.office.com/api/v2.0`), or the preview endpoint (`https://outlook.office.com/api/beta`) to try the latest APIs in preview status. Keep in mind that in general, API in preview status may change before finalization. If you use them, be prepared to verify features in your app and make appropriate changes. When in-preview APIs get finalized, you will be able to access these enhancements in a GA endpoint as well.



## 次の手順

Outlook REST API の使用に進みます。

- [メール API リファレンス](..\api\mail-rest-operations.md)

- [予定表 API リファレンス](..\api\calendar-rest-operations.md)
  
- [連絡先 API リファレンス](..\api\contacts-rest-operations.md)

- [タスク REST API](..\api\task-rest-operations.md) (プレビュー)

- [メール、予定表、連絡先、タスク REST API のリソース リファレンス](..\api\complex-types-for-mail-contacts-calendar.md)

- [People API リファレンス](..\api\people-rest-operations.md) (プレビュー)

- [データ拡張機能 API リファレンス](..\api\extensions-rest-operations.md)

- [拡張プロパティ API リファレンス (プレビュー)](..\api\extended-properties-rest-operations.md) 

- [通知 REST API リファレンス](..\api\notify-rest-operations.md)

- [ユーザー写真 REST API リファレンス](..\api\photo-rest-operations.md) 

- バッチ Outlook REST 要求 (プレビュー)

[!INCLUDE [Enable filtering functionality ](../includes/controls/enablefiltering.xml)]



