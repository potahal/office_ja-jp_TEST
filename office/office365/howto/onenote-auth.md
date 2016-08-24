---
ms.Toctitle: Authentication and permissions
title: "OneNote の認証とアクセス許可" 
description: "Authenticate and register OneNote apps with Microsoft account authentication or Azure Active Directory."
ms.ContentId: 07c9eca4-6c40-4f20-9314-5800d4627b95
ms.date: November 18, 2015

---
[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]
[!INCLUDE [Add the ONAPI repo styles](../includes/controls/addonapistyles.xml)]


# OneNote の認証とアクセス許可

*__Applies to:__ Consumer notebooks on OneDrive | Enterprise notebooks on Office 365*
 
OneNote では、OneNote ノートブックへのセキュリティで保護されたアクセスを提供するために Microsoft アカウント (以前の Live Connect) および Azure Active Directory を使用します。ノートブックにアクセスする前に、まず Microsoft アカウントまたは Azure AD を使用して認証し、アクセス トークンを取得する必要があります。

Choose your platform:

- Microsoft アカウントを使用して認証する (コンシューマー アプリ)

- Azure AD を使用して認証する (エンタープライズ アプリ)

<br />
OneDrive のコンシューマー ノートブックにアクセスするためには Microsoft アカウントが使用され、Office 365 のエンタープライズ ノートブックにアクセスするためには Azure AD が使用されます。

どちらの承認サービスも、OneNote との対話操作のために必要なアクセス トークンを提供するために [OAuth 2.0](http://oauth.net/2/) プロトコルを実装しています。OneNote API に対するすべての要求は、**Authorization** ヘッダーに有効なアクセス トークンを含める必要があります。

この記事では、認証に関連するプロセスのうち、ユーザーのアクションが必要な部分について説明します。クライアント ID を取得するためにアプリを登録し、必要なアクセス許可を指定し、承認サービスを呼び出してユーザーのサインインを行い、アクセス トークンを取得します。 

プラットフォームによっては、[SDK](#sdks) を使用して認証フローを簡略化できる場合があります。

>> OneNote は今後、単一の認証モデルおよび [v2.0 アプリ モデルによるアプリ登録をサポートする予定です。関連する最新ニュースに関しては OneNote 開発者ブログ](https://msdn.microsoft.com/en-us/office/office365/howto/authenticate-Office-365-APIs-using-v2)をご覧ください。 Watch for related updates on the [OneNote Developer Blog](http://go.microsoft.com/fwlink/?LinkID=390183).

**API を試してみる**

API を試してみる場合、[対話型コンソール](../howto/onenote-landing.md#console)の 1 つを使用して、OneDrive のコンシューマー ノートブックを呼び出すことができます。または、この記事の手順に従うことによって、Fiddler など自分のお気に入りのネットワーク ツールを使用してアクセス トークンを取得することもできます。

<a name="msa-auth"></a>
## Microsoft アカウントを使用して認証する (コンシューマー アプリ) 

 1. [アプリケーションを登録し、クライアント ID およびシークレットを取得する](#register-msa)
 2. [OneNote のアクセス許可を選択する](#onenote-perms-msa) 
 3. [ユーザーをサインインさせ、アクセス トークンを取得する](#sign-in-msa) 
 4. [アクセス トークンの期限が切れた後、新しいアクセス トークンを取得する](#get-new-access-token-msa) 

<a name="register-msa"></a>
### アプリケーションを登録し、クライアント ID とシークレットを取得する (コンシューマー アプリ)

開始するには、アプリケーションを Microsoft に登録する必要があります。このプロセスでは、アプリからリンクするサービス プリンシパルを作成し、承認サービスに送信するクライアント ID およびシークレットを生成します。
  
**アプリがコンシューマー ノートブックのみにアクセスする場合、またはコンシューマー ノートブックおよびエンタープライズ ノートブックの両方にアクセスする場合にのみ、これを行ってください。**

1. Microsoft アカウントで、[Microsoft アカウント デベロッパー センター](https://account.live.com/developers/applications)にサインインします。(Windows ストア アプリを開発している場合は、代わりに こちらの方法で進めてください)。 Microsoft アカウントで、Microsoft アカウント デベロッパー センターにサインインします。(Windows ストア アプリを開発している場合は、代わりに [こちらの方法で](#win-store)進めてください)。

  Microsoft アカウントをお持ちでない場合は、[作成](https://signup.live.com/signup)する必要があります。定期的にチェックするメール アドレスを使う必要があります。[おすすめのアプリ](http://dev.onenote.com/apps) ページでアプリをハイライトするため、またはアプリから予期しないネットワーク トラフィックが届いた場合に、ご連絡を差し上げる場合があります。迷惑メールを送ったり、個人情報を販売したりすることはありません。

2. **[アプリケーションの作成]** を選択します。

3. アプリへのアクセス許可を付与するようにユーザーに求めるときに表示する名前を入力し、アプリの優先言語を選択します。  

4. 使用条件とプライバシーおよび Cookie ポリシーに同意する場合は、<e>[同意]</e> を選択して登録を続けます。

5. API 設定ページで、アプリの種類を選択し、アプリに関する情報を提供します。   

  <p id="top-padding">**Web アプリケーション** (サーバー側アプリ)</p>  
  <p id="indent"> a.   a.**モバイルまたはデスクトップ クライアント アプリ**の場合は、**[いいえ] を選択してください。**</p>
  <p id="indent"> b.   b.ターゲット ドメインに関して、サービス URL を入力します。</p>
  <p id="indent"> c.   c.認証が完了し、アプリへのアクセス許可を付与した後にユーザーが移動するリダイレクト URL を入力します。</p>  
  <p id="outdent">**ネイティブ アプリケーション** (デバイスにインストールされたもの)</p>  
  <p id="indent"> a.   a.**モバイルまたはデスクトップ クライアント アプリ**の場合は、**[はい] を選択してください。**</p>
  <p id="indent"> b.   b.(オプション) ターゲット ドメインに関して、モバイル サービス URL を入力します。</p>
  <p id="indent"> c.(オプション) リダイレクト URL に関して、有効な URL を入力します。これはアプリのための識別子として機能します。物理的なエンドポイントである必要はありません。*</p>   

<br />
アプリ設定ページで表示されるクライアント ID およびクライアント シークレット、および入力している場合はリダイレクト URL を保存します。 

Windows ストア アプリ  

Windows アプリを作成している場合、代わりにアプリケーションを [Windows デベロッパー センター](https://dev.windows.com/overview) に登録します。これによりパッケージ ID (パッケージ SID) が提供され、クライアント ID の代わりに使用することになります。 

<!--
>If you have multiple apps, you might need to override the default [JWT sharing behavior](http://msdn.microsoft.com/en-us/library/live/hh826544.aspx#restrict_jwt) on the Microsoft account Developer Center.
-->

1. Microsoft アカウントで [Windows デベロッパー センター](https://dev.windows.com/overview)にサインインします。
2. ダッシュボードで、**[新しいアプリの作成]** を選択し、アプリ名を入力します。
2. Visual Studio で、Windows ストア アプリ プロジェクトを右クリックし、**[ストア] > [アプリケーションをストアと関連付ける]** を選択します。 
2. [アプリケーションをストアと関連付ける] ウインドウで、Microsoft アカウントでサインインし、アプリを選択し、[次へ] > [関連付ける] を選択します。これにより、必要な Windows ストア登録情報がアプリケーション マニフェストに追加されます。 This adds the required Windows Store registration information to the application manifest. 
2. ユニバーサル Windows アプリに関しては、Windows Phone プロジェクトのための前述の 2 つのステップを繰り返します。  


<a name="onenote-perms-msa"></a>
### OneNote のアクセス許可のスコープを選択する (コンシューマー アプリ)

アクセス許可のスコープは、OneNote コンテンツへのアクセスのレベルを表します。アプリに必要なアクセス許可を要求し、ユーザーはアプリにサインインするときにアクセス許可を付与または拒否します。ユーザーは、保有するアクセス許可のみを付与することができます。

<!--All scopes support single sign-on, so users who are already signed into OneNote skip authentication and go straight to authorization. -->

アプリの作業に必要な最低レベルのアクセス許可を選択します。複数のスコープを要求することができます。

| スコープ (コンシューマー) | 説明 |
|-------|------|
| office.onenote_create | ユーザーの OneNote のノートブックのリストを表示すること、そして新しいページを作成することができますが、既存のページの表示または編集はできません。ユーザーのノートブックの階層を列挙して、任意の場所にページを作成することができます。 |
| office.onenote_update_by_app | アプリによって作成されるすべてのページの作成、表示、および変更ができます。 |
| office.onenote_update | ユーザーの OneNote のノートブックとページのすべてのコンテンツの作成、表示、および変更ができます。 |
| office.onenote  | OneNote のノートブックとページの表示ができますが、変更はできません。 |
| wl.signin | A [Microsoft account permission scope](https://msdn.microsoft.com/library/hh243646.aspx).<br />Microsoft アカウントのアクセス許可のスコープ.アプリケーションによるシングル サインオン機能の利用を許可します。 |
| wl.offline_access | A [Microsoft account permission scope](https://msdn.microsoft.com/library/hh243646.aspx).<br />Microsoft アカウントのアクセス許可のスコープ.ユーザーがアクティブでないときでも、アプリケーションがオフラインで作業ができるように、更新トークンの受信を許可します。このスコープはトークン フローでは使用できません。 This scope is not available for the *token* flow. |

Office 365 のノートブックにアクセスするためのアクセス許可については、「[OneNote のアクセス許可を選択する (エンタープライズ アプリ)](#onenote-perms-aad)」を参照してください。

<a name="sign-in-msa"></a>
### ユーザーをサインインさせ、アクセス トークンを取得する (コンシューマー アプリ)

アプリは承認サービスに接続してサインイン プロセスを開始します。ユーザーがまだサインインしていない、またはまだ同意していない場合、承認サービスはユーザーに対して認証情報の入力およびアプリが要求しているアクセス許可への同意を求めます。 認証および承認が成功した場合、OneNote API に対する要求に含めるアクセス トークンを受け取ります。

**重要** 重要!アクセス トークンおよび更新トークンは、ユーザーのパスワードの場合と同様に、安全な方法で扱ってください。

>> プラットフォームによっては、[SDK](#sdks) を使用して認証フローを簡略化できる場合があります。

Choose your authentication flow. 認証フローを選択します。どちらも標準の OAuth 2.0 フローです。

| フロー | 説明 |  
|------|------|  
| [トークン フロー](#token-flow) | <p>Gets an access token in one call. 一度の呼び出でアクセス トークンを取得します。すばやくアクセスするのに有効ですが、長期的なアクセスに必要な更新トークンが提供されません。これは暗黙的フローとも呼ばれます。</p><p>Also called the *Implicit* flow.</p> |  
| [コード フロー](#code-flow) | <p>最初の呼び出しにおいて認証コードを取得し、2 回目の呼び出しでそのコードとアクセス トークンを交換します。wl.offline-access アクセス許可スコープと共に使用する場合、アプリは長期的なアクセスを可能にする更新トークンを受け取ります。これは認証コードフローとも呼ばれます。 When used with the **wl.offline-access** permission scope, your application receives a refresh token that enables long-term access.</p><p>Also called the *Authorization code* flow.</p> |

トークン フローでユーザーをサインインさせる

Web ブラウザーまたは Web ブラウザー コントロールで、以下の URL 要求を読み込みます。

```
GET https://login.live.com/oauth20_authorize.srf
  ?response_type=token
  &client_id={client_id}
  &redirect_uri={redirect_uri}
  &scope={scope}
```

| 必須のクエリ文字列パラメーター | 説明 |  
|------|------|  
| response_type | 使用している認証フローの種類この場合は、*code*。 In this case, *token*. |  
| client_id | アプリケーション用に作成されたクライアント ID。 |  
| redirect_uri | アプリケーション用のリダイレクト URL。 アプリケーション用に登録したリダイレクト URL。これを指定していないモバイルおよびデスクトップ アプリは、次のものを使用できます: https://login.live.com/oauth20_desktop.srf`https://login.live.com/oauth20_desktop.srf` |  
| スコープ | アプリケーションが要求するスコープ例: office.onenote%20wl.sign-in<br />Example: *office.onenote%20wl.sign-in* |  

認証および承認が成功すると、Web ブラウザーはリダイレクト URL へリダイレクトを行い、アクセス パラメーターを URL に追加します。次の例が示すとおり、パラメーターには access_token および token_type が含まれます。アクセス トークンは、expires_in プロパティによって指定された秒数だけ有効となります。 The parameters include  
 the **access_token** and **token_type**, as shown in the following example. 成功した場合、次の例に示すように、応答には**access_token** および refresh_token の入った JSON 文字列が含まれます。アクセス トークンは、expires_in プロパティによって指定された秒数だけ有効となります。 

```
https://your-redirect-url
  #access_token=EwB4Aq...%3d
  &token_type=bearer
  &expires_in=3600
  &scope=office.onenote wl.signin
  &user_id=c519ea026ece84de362cfa77dc0f2348
```

コード フローでユーザーをサインインさせる

コード フローにおいて、アクセス トークンの取得は 2 段階の手順で行われます: 

1. ユーザーをサインインさせ、認証コードを取得します。
2. コードをアクセス トークンと交換します。

<br />ステップ 1 ユーザーをサインインさせ、認証コードを取得します。 手順 1.ユーザーをサインインさせ、認証コードを取得します。サインイン プロセスを開始するために、Web ブラウザーまたは Web ブラウザー コントロールで以下の URL 要求を読み込みます。 

```
https://login.live.com/oauth20_authorize.srf
  ?response_type=code
  &client_id={client-id}
  &redirect_uri={redirect-uri}
  &scope={scope}
```
        
| 必須のクエリ文字列パラメーター |  説明  |  
|------|-------|  
| response_type | 使用している認証フローの種類この場合は、*code*。 In this case, *code*. |  
| client_id | アプリケーション用に作成されたクライアント ID。 |  
| redirect_uri | アプリケーション用のリダイレクト URL。 アプリケーション用に登録したリダイレクト URL。これを指定していないモバイルおよびデスクトップ アプリは、次のものを使用できます: https://login.live.com/oauth20_desktop.srf`https://login.live.com/oauth20_desktop.srf` |
| スコープ | アプリケーションが要求するスコープ例: office.onenote%20wl.sign-in<br />アプリケーションが要求するスコープ例: *office.onenote wl.signin wl.offline_access* |

次の例が示すように、認証および承認が成功すると、Web ブラウザーはリダイレクト URL へリダイレクトを行い、URL に*コード* パラメーターを付加します。手順 2 で使用するために、*コード*の値をコピーします。このコードは、数分間有効です。

```
https://your-redirect-uri
  ?code=M57010781-9e8c-e31e-ca0d-46bc104236c4
```


**手順 2.**認証コードをアクセス トークンおよび更新トークンと交換します。適切にエンコードされた URL 文字列をメッセージ本文に記載した次の HTTP 要求を送信します。

```
POST https://login.live.com/oauth20_token.srf
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code
&client_id={client-id}
&client_secret={client-secret}
&code={code}
&redirect_uri={redirect-uri}
```

| 必要な本文パラメーター | 説明 |  
|------|------|  
| grant_type | 要求の許可の種類。この例では、*refresh_token*。 In this case, *authorization_code*. |  
| client_id | アプリケーション用に作成されたクライアント ID。 |  
| client_secret | アプリケーション用に作成されたクライアント シークレット。 |  
| code | 前の手順で URL パラメーターとして受け取ったコード。 |  
| redirect_uri | アプリケーション用のリダイレクト URL。 アプリケーション用のリダイレクト URL。これは、最初の要求における **redirect_uri** と一致する必要があります。 |  
            
成功した場合、次の例に示すように、応答には **access_token** (および **wl.offline_access** スコープを要求した場合は、**refresh_token**) の入った JSON 文字列が含まれます。アクセス トークンは、**expires_in** プロパティによって指定された秒数だけ有効です。 

```json
{
  "token_type":"bearer",
  "expires_in":3600,
  "scope":"office.onenote wl.sign-in wl.offline-access",
  "access_token":"EwCAAq...wE=",
  "refresh_token":"MCvePE...$$",
  "user_id":"c519ea026ece84de362cfa77dc0f2348"
}
```

**OneNote API に対する要求にアクセス トークンを含める**

OneNote API に対するすべての要求は、**Authorization** ヘッダー内でベアラー トークンとしてアクセス トークンを送信する必要があります。たとえば、次の要求は 5 つのノートブックを取得し、それらはアルファベット順に名前で並べ替えられます。

```
GET https://www.onenote.com/api/v1.0/me/notes/notebooks?top=5
Authorization: Bearer {access-token}
```

アクセス トークンは 1 時間のみ有効であるため、その期限が切れた場合は新しいトークンを取得する必要があります。トークンを使用する前に、その有効期限をチェックし、必要に応じて新しいアクセス トークンを取得する必要があります。ユーザーはサインインした状態を保持することができ、サインアウトする場合やアクセス許可を取り消す場合を除き、アクセス許可に再度同意する必要はありません。

<a name="get-new-access-token-msa"></a>
### アクセス トークンの期限が切れた後、新しいアクセス トークンを取得する (コンシューマー アプリ)

更新トークンを使用するか、または認証手順を最初から再度行うことによって新しいアクセス トークンを要求することができます。

アクセス トークンの期限が切れた場合、API に対する要求は 401 Unauthorized`401 Unauthorized` 応答を返します。アプリはこの応答を処理しなければならないため、要求を送信する前にトークンの有効期限を確認する必要があります。 アクセス トークンの期限が切れた場合、API に対する要求は 401 Unauthorized 応答を返します。アプリはこの応答を処理しなければならないため、要求を送信する前にトークンの有効期限を確認する必要があります。
  
適切にエンコードされた URL 文字列をメッセージ本文に記載した次の HTTP 要求を送信します。

**wl.offline_access** アクセス許可を要求し、コード フローを使用した場合、更新トークンを受け取ります。

```http
POST https://login.live.com/oauth20_token.srf
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token
&client_id={client-id}
&client_secret={client-secret}
&redirect_uri={redirect-uri}
&refresh_token={refresh-token}
```

| 必要な本文パラメーター | 説明 |  
|------|-------|  
| grant_type | 要求の許可の種類。この例では、*refresh_token*。 In this case, *refresh_token*. |  
| client_id | アプリケーション用に作成されたクライアント ID。 |  
| client_secret | アプリケーション用に作成されたクライアント シークレット。 |  
| redirect_uri  | アプリケーション用のリダイレクト URL。 アプリケーション用のリダイレクト URL。これはトークンを取得するために使用した **redirect_uri** と一致する必要があります。 |  
| refresh_token | 以前に受信した更新トークン。 |  

成功した場合、次の例に示すように、POST 要求に対する応答には **access_token** および **refresh_token** の入った JSON 文字列が含まれます。

```json
{
  "token_type":"bearer",
  "expires_in": 3600,
  "scope":"office.onenote wl.sign-in wl.offline-access",
  "access_token":"EwB4Aq...wE=",
  "refresh_token":"MCVw8k...$$",
  "user_id":"c519ea026ece84de362cfa77dc0f2348"
}
```

保存されたトークンを更新し、アプリが最も長い有効期限のトークンを保有するようにします。

<a name="sign-out"></a>
### ユーザーをサインアウトする (コンシューマー アプリ)
ユーザーをサインアウトするには、次の手順を実行します:

1. 受信または保存しているすべてのキャッシュ アクセス トークンまたは更新トークンを削除します。
2. アプリケーションにおいてすべてのサインアウト操作を実施します (たとえば、ローカルの状態のクリーンアップ、すべてのキャッシュ アイテムの削除等)。
3. この URL を使用して承認サービスを呼び出します。

```
https://login.live.com/oauth20_logout.srf
  ?client_id={client_id}
  &redirect_uri={redirect_uri}
```

この呼び出しは、シングル サインオンを有効にするすべての Cookie を削除し、ユーザーがサインインを要求されるようにします。

| 必須のクエリ文字列パラメーター | 説明 |  
|------|------|  
| client_id | アプリケーション用に作成されたクライアント ID 値。 |  
| redirect_uri | アプリケーション用のリダイレクト URL。 アプリケーション用のリダイレクト URL。これはトークンを取得するために使用した **redirect_uri** と一致する必要があります。 |  

Cookie を削除した後、ブラウザーはリダイレクト URL へのリダイレクトを行います。リダイレクト ページは、認証クエリ文字列オプションを指定することなく読み込まれます。このことはユーザーがログアウトしたことを意味します。

アクセス権の取り消し

ユーザーは Microsoft アカウントの[管理](https://account.live.com/consent/Manage)ページから、アプリケーションのアカウントへのアクセスを取り消すことができます。

アプリに関する同意が取り消された場合、以前にアプリに対して提供されたすべての更新トークンは無効となります。トークンを更新しようとすると、次の応答を受け取ります。

```
{
  "error":"invalid_grant",
  "error_description":"The request was denied because one or more scopes requested are unauthorized or expired. The user must first sign in and grant the client application access to the requested scope."
}
```

新しいアクセスおよび更新トークンを要求するために、認証フローを最初から再度行う必要があります。

<a name="aad-auth"></a>
## Azure AD を使用して認証する (エンタープライズ アプリ)

 1. [アプリケーションを登録し、クライアント ID およびシークレットを取得する](#register-aad)
 2. [OneNote のアクセス許可を選択する](#onenote-perms-aad) 
 3. [ユーザーをサインインさせ、アクセス トークンを取得する](#sign-in-aad) 
 4. [アクセス トークンの期限が切れた後、新しいアクセス トークンを取得する](#get-new-access-token-aad) 

<a name="register-aad"></a>
### アプリケーションを登録し、クライアント ID およびシークレットを取得する (エンタープライズ アプリ)

開始するには、アプリケーションを Microsoft に登録する必要があります。このプロセスでは、アプリからリンクするサービス プリンシパルを作成し、承認サービスに送信するクライアント ID およびシークレットを生成します。

**アプリがエンタープライズ ノートブックのみにアクセスする場合、またはコンシューマー ノートブックおよびエンタープライズ ノートブックの両方にアクセスする場合にのみ、これを行ってください。**

Office 365 サブスクリプションに関連付けられた Azure AD テナントにアプリを登録するには、次の作業が必要です。

- **Office 365 テナントへのグローバル管理者のアクセス許可を持つ Office 365 アカウントを取得する**

  You can [try](https://portal.office.com/Signup/Signup.aspx?OfferId=6881A1CB-F4EB-4db3-9F18-388898DAF510&DL=DEVELOPERPACK&ali=1#0) or [buy](https://portal.microsoftonline.com/Signup/MainSignUp.aspx?OfferId=C69E7747-2566-4897-8CBA-B998ED3BAB88&DL=DEVELOPERPACK) an Office 365 Developer subscription, or subscribe to a [qualifying plan](../howto/setup-development-environment.md#bk_Office365Account). 
 
- **テナント用に OneDrive for Business をプロビジョニングする**

  This makes the OneNote application available so you can specify OneNote permissions. これにより、OneNote アプリケーションが利用可能になるため、OneNote のアクセス許可を指定することができます。OneDrive for Business がプロビジョニングされているかどうかをチェックするため、Office 365 の資格情報 (someone@example.com または *someone@example.onmicrosoft.com*のような職場または学校アカウント) で OneNote Online にサインインします。 

  If you see your notebooks, you're all set. If you see "Sorry, we can't seem to get your notebooks...", choose **Go to my account** > **Next**. When your OneDrive for Business page loads, go back and refresh OneNote Online to complete the provisioning.

  **Note:** Your users' personal sites must be provisioned before you can access their notebooks. The OneNote API will attempt to auto-provision their sites if needed. <!--The API returns error [30105](../howto/onenote-error-codes.md#30105) at the start of the process and error [30106](../howto/onenote-error-codes.md#30106) until the provisioning is complete.-->

- **Office 365 サブスクリプションを Azure サブスクリプションと関連付ける**

  これにより、Azure AD にアプリを登録し、管理することができます。(*詳細情報*) 詳細を見る

  Azure サブスクリプションがない場合は、次のセクションのオプション 1 を行います。Azure サブスクリプションがある場合は、オプション 2 を行います。 If you do have one, do Option 2. 

**重要!**有効な Office 365 のライセンスのある Office 365 アカウントが必要です。有効なライセンスのないユーザー アカウントでは、OneNote Online のノートブックを表示することができず、API によるノートブックの呼び出しも失敗します。Office 365 の管理者は、[状態の確認ならびにライセンスの割り当ておよび割り当て解除を行うことができます](https://support.office.com/en-ca/article/Assign-or-unassign-licenses-for-Office-365-for-business-997596b5-4173-4627-b915-36abac6786dc#__toc383721312)。

>> MSDN サブスクライバー:無料の Office 365 Developer サブスクリプションを利用できる場合があり、Azure 特典を利用することによりコスト削減も可能です。MSDN サブスクリプション ページで特典をご確認ください。 Check your benefits on the [MSDN subscriptions page](https://msdn.microsoft.com/subscriptions/manage/default.aspx).


**Azure サブスクリプションを Office 365 サブスクリプションと関連付ける**

**オプション 1:**Office 365 サブスクリプションの管理者の資格情報を使用して、Azure サブスクリプションにサインアップします。Azure サブスクリプションがない場合は、これを実行してください。このプロセスでは、サブスクリプションを関連付けます。

1. Office 365 管理者の資格情報 (someone@example.com または someone@example.onmicrosoft.com のような職場または学校アカウント) で、[Azure 管理ポータル][aad-portal] にサインインします。

2. "サブスクリプションが見つかりません" のページで、**[Microsoft Azure にサインアップ]** を選択します。サインアップ ページが読み込まれ、Office 365 サブスクリプションからの一部の情報が表示されます。このアカウントは、新しい Azure サブスクリプションのサービス管理者になります。 
  
  Office 365 サブスクリプションが試用版か、または有料版かでサインアップの方法が異なります:

   **試用版のサブスクリプションの場合**、お支払い情報を [無料試用版] ページで入力します。有料サブスクリプションに変更しない限り、課金されることはありません。

   サブスクリプション契約、プランの詳細、およびプライバシーに関する声明に同意する場合、チェックボックスをオンにして、**[購入]** を選択します。新しい Azure アカウントのサブスクリプション ページが開きます。試用版サブスクリプションには 200 ドルのクレジットが発行され、30 日間使用できます。いつでもこのページからサブスクリプションを取り消すことができます。

   **有料版サブスクリプションの場合**、連絡先情報を入力して、**[サインアップ]** を選択します。サブスクリプションが作成された後、**[サービスの管理を開始する]** を選択し、Azure 管理ポータルを開きます。

**オプション 2:**既存の Azure サブスクリプションと Office 365 サブスクリプションを関連付けます。Azure サブスクリプションのためのサービス管理者または共同管理者としての Microsoft アカウントがある場合は、これを実行してください。このプロセスによって、Microsoft アカウントが Office 365 テナントのグローバル管理者となります。 

1. Microsoft アカウントの資格情報 (*someone@live.com* など) で、[Azure 管理ポータル][aad-portal] にサインインします。

2. ページの下部にあるドロワーで、**[新規] > [App Services] > [Active Directory] > [ディレクトリ] > [カスタム作成]** を選択します。

3. [ディレクトリの追加] ウィンドウで、ディレクトリについて **[既存のディレクトリの使用]** を選択します。

4. **[サインアウトする準備ができました]** を選択し、チェック マークをクリックします。これにより、ポータルからサインアウトします。 This signs you out of the portal.

5. 関連付けたい Office 365 テナントのグローバル管理者の資格情報を使用して再度サインインします (someone@example.com または *someone@example.onmicrosoft.com*のような職場または学校アカウント)。

6. Azure でディレクトリを使用するかどうかを確認するメッセージが表示されたら、**[続行] > [今すぐサインアウト]** を選択します。

7. ブラウザーを閉じ、そして再度 [Azure 管理ポータル][aad-portal] を開きます。

8. Microsoft アカウントの資格情報で再度サインインし、ナビゲーション ウィンドウ内の **[Active Directory]** を選択します。Office 365 のディレクトリ一覧が [ディレクトリ] タブに表示されます。


**Azure 管理ポータルでアプリを登録する**
  
1. Sign in to the [Azure Management Portal][aad-portal]. [Azure 管理ポータル][aad-portal] にサインインします。Azure サブスクリプションの管理者の資格情報を使用します。 

2. ナビゲーション ウィンドウ内で、**[Active Directory]** を選択します。 

3. アプリケーションを登録したいディレクトリを選択し、[アプリケーション] タブを開きます。

4. ページの下部にあるドロワーで、**[追加] > [組織で開発中のアプリケーションを追加]** を選択します。 

5. アプリケーションのフレンドリ名を入力し、アプリケーションの種類を選択します: 

  <p id="top-padding">**Web アプリケーションまたは Web API** (ブラウザー ベースまたはサーバー アプリ)</p>
  <p id="indent"> a.   a.**Web アプリケーションまたは Web API を選択します。**</p>
  <p id="indent"> b.   b.サインオンの URL に関して、ユーザーがサインインし、アプリを使用する URL を入力します。</p>
  <p id="indent"> c.   c.アプリ ID の URL に関して、アプリの一意識別子を入力します。これは確認済みのカスタム ドメイン内にある必要があります。 This must be in a verified custom domain.</p>
  <p id="indent"> d.応答 URL に関して、OAuth 2.0 要求に応えてリダイレクトする URL を入力します。これは物理的なエンドポイントである必要はありませんが、有効な URI でなければなりません。</p>
  <p id="indent"> e.   e.アプリを外部の Azure テナントに利用可能なものとするため、**[アプリケーションはマルチテナントです]** で **[はい] を選択します。**</p>
  <p id="outdent">**ネイティブ クライアント アプリケーション** (デバイスにインストールされたアプリ)</p>
  <p id="indent"> a.   a.**ネイティブ クライアント アプリケーション を選択します。**</p>
  <p id="indent"> b.OAuth 2.0 要求に応えてリダイレクトする URI を入力します。これは物理的なエンドポイントである必要はありませんが、有効な URI でなければなりません。</p>   

<br />Web アプリの場合、Azure はクライアント ID とアプリ シークレット (またはキー) の両方を生成します。ネイティブ クライアント アプリケーションに関して、Azure はクライアント ID を生成します。これらの値を保存します。

アプリの登録に関する詳細な説明は、[Office 365 ドキュメント](../howto/add-common-consent-manually.md)をご覧ください。


<a name="onenote-perms-aad"></a>
### OneNote のアクセス許可のスコープを選択する (エンタープライズ アプリ)

アクセス許可のスコープは、OneNote コンテンツへのアクセスのレベルを表します。アプリに必要なアクセス許可を要求し、ユーザーはアプリにサインインするときにアクセス許可を付与または拒否します。ユーザーは、保有するアクセス許可のみを付与することができます。

1. [Azure 管理ポータル][aad-portal] で、アプリの構成ページの [他のアプリケーションに対するアクセス許可] セクションで、[アプリケーションの追加] を選択します。

2. OneNote アプリケーションを選択し、右下隅のチェック マークをクリックします。OneNote が一覧にない場合、[テナントに対して OneDrive for Business をプロビジョニングしたかどうか](#register-aad)を確認してください。

3. アプリの動作に必要な最低レベルのアクセス許可を選択し、変更を保存します。複数のスコープを要求することができます。 You can request multiple scopes.

<br />現在のユーザーが保有する個人用ノートブックのためのスコープ

OneDrive for Business の個人用ノートブック**のみ**を使用する場合、次のスコープの中から選択します。

| スコープ (エンタープライズ) | Azure ポータルにおけるアクセス許可 | 説明 |  
|:-------|:------|:------|  
| Notes.Create | OneNote ノートブックにページを作成する | <p>ノートブックおよびセクションのタイトルの表示、任意の場所における新しいページの作成ができます。既存のページの表示または編集はできません。</p><p>Cannot view or edit existing pages.</p> |  
| Notes.ReadWrite.CreatedByApp | アプリケーション専用の OneNote ノートブック アクセス | <p>ノートブックおよびセクションのタイトルの表示、新しいページの作成、セクションの名前の変更、アプリによって作成されたページの表示および変更ができます。他のアプリによって作成されたページ、またはパスワードで保護されたセクションのページの表示や変更はできません。</p><p>Cannot view or modify pages created by other apps or in password protected sections.</p> |  
| Notes.Read | OneNote ノートブックを表示する | <p>ノートブックおよびセクションの内容を表示できます。新しいページの作成、既存のページの変更、パスワードで保護されたセクションへのアクセスはできません。</p><p>ノートブックおよびセクションの内容を表示できます。新しいページの作成、既存のページの変更、パスワードで保護されたセクションへのアクセスはできません。</p> |  
| Notes.ReadWrite | OneNote ノートブックを表示し、変更する | <p>ノートブックおよびセクションのタイトルの表示、すべてのページの表示および変更、新しいページの作成、セクションの名前の変更ができます。パスワードで保護されたセクションへのアクセスはできません。</p><p>Cannot access password protected sections.</p> |  

<br />現在のユーザーがアクセスできるサイトおよびグループ ノートブックのためのスコープ

SharePoint サイトのノートブックまたは統合グループのノートブックを使用している場合は、次のスコープの中から選択します。これらのアクセス許可は現在のユーザーの個人用ノートブックにも適用されますが、他のユーザーと共有している個人用ノートブックには適用されません。共有された個人用コンテンツへのアクセスは、現在サポートされていません。

| スコープ (エンタープライズ) | Azure ポータルにおけるアクセス許可 | 説明 |  
|:-------|:------|:------| 
| Notes.Read.All | 組織内の OneNote ノートブックを表示する | <p>サインインしているユーザーがアクセスできるすべてのノートブックにおいて、ノートブックおよびセクションの内容を表示できます。新しいページの作成、既存のページの変更、パスワードで保護されたセクションへのアクセスはできません。</p><p>ノートブックおよびセクションの内容を表示できます。新しいページの作成、既存のページの変更、パスワードで保護されたセクションへのアクセスはできません。</p>|  
| Notes.ReadWrite.All | 組織内の OneNote ノートブックを表示および変更する | <p>サインインしているユーザーがアクセスできるすべてのノートブックにおいて、ノートブックおよびセクションのタイトルの表示、すべてのページの表示および変更、すべてのセクションの名前の変更、新しいページの作成ができます。パスワードで保護されたセクションへのアクセスはできません。</p><p>Cannot access password protected sections.</p>|  

<br />グループのためのスコープ

グループ ノートブックにアクセスする場合、グループ ID を取得するためにグループ アクセス許可スコープが必要となります。これらのアクセス許可は、管理者権限を必要とします。 These permissions require administrator rights.

| スコープ (エンタープライズ) | Azure ポータルにおけるアクセス許可 | 説明 |  
|:-------|:------|:------|  
| Group.Read.All | すべてのグループの読み取り | すべてのグループのプロパティおよびメンバーシップの閲覧、パブリック グループおよびサインインしているユーザーがメンバーになっているグループの予定表および会話の閲覧ができます。 |  
| Group.ReadWrite.All | すべてのグループの閲覧および書き込み | サインインしているユーザーのためのグループの作成ならびにすべてのグループのプロパティおよびメンバーシップの閲覧、サインインしているユーザーが保有するグループに関するグループ プロパティおよびメンバーシップの変更、パブリック グループおよびサインインしているユーザーがメンバーになっているグループの予定表および会話の閲覧および書き込みができます。 |  

<!--These Group permissions also apply to OneNote operations on group notebooks. So if you're **only** accessing group notebooks, you don't need to request Notes permissions if the Group permissions you request provide the read/write access you need.-->
OneDrive の個人用ノートブックにアクセスするためのアクセス許可については、「[OneNote のアクセス許可を選択する (コンシューマー アプリ)](#onenote-perms-msa)」を参照してください。


<a name="sign-in-aad"></a>
### ユーザーをサインインさせ、アクセス トークンを取得する (エンタープライズ アプリ)

アプリは承認サービスに接続してサインイン プロセスを開始します。ユーザーがまだサインインしていない、またはまだ同意していない場合、サービスはユーザーに対して認証情報の入力およびアプリが要求しているアクセス許可への同意を求めます。認証および承認が成功した場合、OneNote API に対する要求に含めるアクセス トークンを受け取ります。

**重要** 重要!アクセス トークンおよび更新トークンは、ユーザーのパスワードの場合と同様に、安全な方法で扱ってください。

>> プラットフォームによっては、[SDK](#sdks) を使用して認証フローを簡略化できる場合があります。

アクセス トークンの取得は 2 段階の手順で行われます: 

1. ユーザーをサインインさせ、認証コードを取得します。
2. コードをアクセス トークンと交換します。


<br />
This process represents the [Authorization Code Grant Flow](https://msdn.microsoft.com/library/azure/dn645542.aspx). If you want to use the implicit flow, you'll need to edit the manifest file. このプロセスは 認証コード付与フローを表します。暗黙的フローを使用する場合、マニフェスト ファイルを編集する必要があります。「ブラウザー ベースの Web アプリを登録する」の「OAuth 2.0 の暗黙的な付与フローを許可するようアプリケーションを構成する」を参照してください。

<br />ステップ 1 ユーザーをサインインさせ、認証コードを取得します。 手順 1.ユーザーをサインインさせ、認証コードを取得します。サインイン プロセスを開始するために、Web ブラウザーまたは Web ブラウザー コントロールで以下の URL 要求を読み込みます。 

The URL below uses the *common* tenant endpoint, which is valid for any application. <!--To scope your app to your tenant, use the tenant ID instead.-->

```
https://login.microsoftonline.com/common/oauth2/authorize
  ?response_type=code
  &client_id={client-id}
  &redirect_uri={redirect-uri}
  &resource=https://onenote.com/
```

        
| 必須のクエリ文字列パラメーター |  説明  |  
|------|-------|  
| response_type | 使用している認証フローの種類この場合は、*code*。 In this case, *code*. |  
| client_id | アプリケーション用に作成されたクライアント ID。 |   
| redirect_uri | アプリケーション用のリダイレクト URL。 |
| リソース | アクセスが必要なリソースこの例では、https://onenote.com/ アクセスが必要なリソースこの例では、*https://onenote.com/* |
 
次の例が示すように、認証および承認が成功すると、Web ブラウザーはリダイレクト URI へリダイレクトを行い、URL に*コード* パラメーターを付加します。手順 2 で使用するために、*コード*の値をコピーします。このコードは、数分間有効です。

```
https://your-redirect-uri/
  ?code=AAABAA...AA
  &session_state=d56e3523-614e-4fbe-bf89-3ba0f065954b
```


**手順 2.**認証コードをアクセス トークンおよび更新トークンと交換します。適切にエンコードされた URL 文字列をメッセージ本文に記載した次の HTTP 要求を送信します。

```
POST https://login.microsoftonline.com/common/oauth2/token
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code
&client_id={client-id}
&client_secret={client-secret}
&redirect_uri={redirect-uri}
&code={code}
&resource=https://onenote.com/
```

| 必要な本文パラメーター | 説明 |  
|------|------|  
| grant_type | 要求の許可の種類。この例では、*refresh_token*。 In this case, *authorization_code*. |  
| client_id | アプリケーション用に作成されたクライアント ID。 |  
| client_secret | Web アプリケーションおよび Web API のみアプリケーション用に作成されたクライアント シークレット。 |  
| code | 前の手順で URL パラメーターとして受け取ったコード。 |  
| redirect_uri | アプリケーション用のリダイレクト URL。 アプリケーション用のリダイレクト URL。これは、最初の要求における **redirect_uri** と一致する必要があります。 |  
| resource | アクセスが必要なリソースこの例では、https://onenote.com/ アクセスが必要なリソースこの例では、*https://onenote.com/* |  

成功した場合、次の例に示すように、応答には**access_token** および **refresh_token** の入った JSON 文字列が含まれます。アクセス トークンは、**expires_in** プロパティによって指定された秒数だけ有効となります。 

```json
{
  "token_type":"Bearer",
  "expires_in":"3600",
  "scope":"Notes.ReadWrite",
  "expires_on":"1446588136",
  "not_before":"1446584236",
  "resource":"https://onenote.com/",
  "access_token":"eyJ0eX...2-w",
  "refresh_token":"AAABAAA...IAA",
  "id_token":"eyJ0eX...fQ."
}
```

要求において使用可能な追加パラメーターを含むコード付与フローの Azure AD 実装の詳細については、「認証コード付与フロー」を参照してください。 


**OneNote API に対する要求にアクセス トークンを含める**

OneNote API に対するすべての要求は、**Authorization** ヘッダー内でベアラー トークンとしてアクセス トークンを送信する必要があります。たとえば、次の要求は 5 つのノートブックを取得し、それらはアルファベット順に名前で並べ替えられます。

```
GET https://www.onenote.com/api/v1.0/me/notes/notebooks?top=5
Authorization: Bearer {access-token}
```

アクセス トークンは 1 時間のみ有効であるため、その期限が切れた場合は新しいトークンを取得する必要があります。トークンを使用する前に、その有効期限をチェックし、必要に応じて新しいアクセス トークンを取得する必要があります。ユーザーはサインインした状態を保持することができ、サインアウトする場合やアクセス許可を取り消す場合を除き、アクセス許可に再度同意する必要はありません。


<a name="get-new-access-token-aad"></a>
### アクセス トークンの期限が切れた後、新しいアクセス トークンを取得する (エンタープライズ アプリ)

更新トークンを使用するか、または認証手順を最初から再度行うことによって新しいアクセス トークンを要求することができます。

アクセス トークンの期限が切れた場合、API に対する要求は 401 Unauthorized`401 Unauthorized` 応答を返します。アプリはこの応答を処理しなければならないため、要求を送信する前にトークンの有効期限を確認する必要があります。 アクセス トークンの期限が切れた場合、API に対する要求は 401 Unauthorized 応答を返します。アプリはこの応答を処理しなければならないため、要求を送信する前にトークンの有効期限を確認する必要があります。
  
適切にエンコードされた URL 文字列をメッセージ本文に記載した次の HTTP 要求を送信します。

The URL in the following example uses the *common* tenant endpoint, which is valid for any application. <!--To scope your app to a specific tenant, use the tenant ID instead.-->

```http
POST https://login.microsoftonline.com/common/oauth2/token
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token
&client_id={client-id}
&client_secret={client-secret}
&redirect_uri={redirect-uri}
&refresh_token={refresh-token}
&resource={resource-id}
```

| 必要な本文パラメーター | 説明 |  
|------|-------|  
| grant_type | 要求の許可の種類。この例では、*refresh_token*。 In this case, *refresh_token*. |  
| client_id | アプリケーション用に作成されたクライアント ID。 |  
| client_secret | Web アプリケーションおよび Web API のみアプリケーション用に作成されたクライアント シークレット。 |  
| redirect_uri  | アプリケーション用のリダイレクト URL。 |  
| refresh_token | 以前に受信した更新トークン。 |  
| resource | アクセスが必要なリソースこの例では、https://onenote.com/ アクセスが必要なリソースこの例では、*https://onenote.com/* |   

成功した場合、次の例に示すように、POST 要求に対する応答には **access_token** および **refresh_token** の入った JSON 文字列が含まれます。

```json
{
  "token_type":"Bearer",
  "expires_in":"3600",
  "scope":"Group.Read.All Notes.ReadWrite",
  "expires_on":"1447656020",
  "not_before":"1447652120",
  "resource":"https://onenote.com/",
  "access_token":"eyJ0eX...Jww",
  "refresh_token":"AAABAAA...IAA"
}
```

保存されたトークンを更新し、アプリが最も長い有効期限のトークンを保有するようにします。


<a name="sdks"></a>
## OneNote の開発のための SDK

[!INCLUDE [sdks](../includes/onenote/sdks.xml)]

 
<a name="errors"></a>
## エラー
If there are errors with authentication, the web browser is redirected to an error page. 認証でエラーが発生した場合、Web ブラウザーはエラー ページへとリダイレクトされます。エラー ページでは、エンドユーザーにわかりやすいメッセージが表示されますが、そのページの URL には問題を解決するのに役立つ追加パラメーターが含まれます。URL パラメーターはブックマークとして含まれています。例: <c>#error={error_code}&error_description={message}</c> The URL parameters are included as a bookmark, for example: `#error={error_code}&error_description={message}`

ユーザーがアプリケーションに同意しない場合、フローはリダイレクト URL にリダイレクトされ、エラー パラメーターが含まれます。 

エラーの処理に関する詳細については、「[OAuth 2.0 でのエラー処理][oauth2-errors]」を参照してください。


<a name="see-also"></a>
## その他の技術情報

- [OneNote 開発](../howto/onenote-landing.md)
- [OneNote API を使用した開発](../howto/onenote-get-content.md)
- [OneNote デベロッパー センター](http://dev.onenote.com/)
- [OneNote の開発者ブログ](http://go.microsoft.com/fwlink/?LinkID=390183)
- [スタック オーバーフローに関する OneNote の開発の質問](http://go.microsoft.com/fwlink/?LinkID=390182) 
- [OneNote GitHub のリポジトリ](http://go.microsoft.com/fwlink/?LinkID=390178)


[msa-portal]: http://go.microsoft.com/fwlink/p/?LinkId=193157
[aad-portal]: https://manage.windowsazure.com/
[oauth2-errors]: https://msdn.microsoft.com/library/azure/dn645540.aspx
