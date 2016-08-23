# 実行時に OAuth を使用してプロバイダー ホスト型アドインのユーザーを承認する

実行時にプロバイダー ホスト型アドインで OAuth を使用して、SharePoint リソースへの承認されたアクセスを提供します。

_**適用対象:** SharePoint 2013? | SharePoint Add-ins? | SharePoint Online_

通常、ユーザーが SharePoint アドインにアクセスするには、SharePoint サイトを開き、**[サイト コンテンツ]** を選択してからアドインを選択します。SharePoint は、プロバイダー ホスト型アドインが実行するリモート Web にユーザーをリダイレクトします。ユーザーは SharePoint からアドインにアクセスするので、まず SharePoint によって承認されてからアドインにアクセスできるようになります。

または、ユーザーがプロバイダー ホスト型アドインの URL に直接移動する場合、そのアドインは OAuth を使用して実行時にユーザーを承認する必要があります。このシナリオでは、ユーザーが SharePoint によって最初に承認されていないので、プロバイダー ホスト型アドインが承認を処理する必要があります。[Core.DynamicPermissions](https://github.com/OfficeDev/PnP/tree/dev/Samples/Core.DynamicPermissions) サンプルは、OAuth を使用して Web サイトから動的に許可を要求する方法を示しています。このソリューションを使用して、以下を行うことができます。

- SharePoint からアドインにアクセスするのではなく、プロバイダー ホスト型アドインに直接移動するユーザーを承認します。たとえば、ユーザーに SharePoint UI を使用させないようにする場合です。その代わりに、ユーザーは SharePoint から取得した関連データを表示するプロバイダー ホスト型アドインを使用できます。
    
- OAuth を使用してユーザーを認証でき、かつ Office ストアで販売できる、プロバイダー ホスト型アドインをビルドします。
    
## 始める前に

まず GitHub の [Office 365 Developer Patterns and Practices](https://github.com/OfficeDev/PnP/tree/dev) プロジェクトから、[Core.DynamicPermissions](https://github.com/OfficeDev/PnP/tree/dev/Samples/Core.DynamicPermissions) サンプル アドインをダウンロードします。

コード サンプルを実行する前に、次の操作を実行してください。 

- そのサイトに対する**管理**権限を持っていることを確認します。 詳細については、「[アクセス許可レベルについて](https://support.office.com/article/Understanding-permission-levels-87ECBB0E-6550-491A-8826-C075E4859848)」を参照してください。
    
- AppRegNew.aspx を使用して、アドインを SharePoint サイトに登録します。 
    
    1. SharePoint サイトの appregnew.aspx に移動します。たとえば、contoso.sharepoint.com サイトのアドインを使用する場合は、http://contoso.sharepoint.com/_layouts/15/appregnew.aspx に移動します。
    
    2. **[生成]** を選択して、新しい**クライアント ID**を生成します。
    
    3. **[生成]** を選択して、新しい**クライアント シークレット**を生成します。 
    
    4. **[タイトル]** にアドインのタイトルを入力します。
    
    5. **[アドイン ドメイン]** で、プロバイダー ホスト型アドインの URL を入力します。 たとえば、「localhost」と入力します。 
    
    6. **[リダイレクト URI]** にプロバイダー ホスト型アドインの URL を入力します。SharePoint は承認と同意が付与された後にアドインをこの URL にリダイレクトします。たとえば、「https://localhost:44363/Home/Callback」と入力します。ドメイン名とポート番号は、Visual Studio の Core.DynamicPermissionsWeb プロジェクトの **[SSL URL]** プロパティから取得できます。
    
    7. **[作成]** を選択します。 
    
- クライアント ID とクライアント シークレットを Core.DynamicPermissionsWeb\web.config の **appSettings** 要素にコピーします。

## Core.DynamicPermissions アドインを使用する

コード サンプルを実行するときには、次の操作を実行してください。

1. **[Office 365 に接続]** で、アドインを登録した SharePoint サイトの URL を入力して、**[接続]** を選択します。たとえば、「https://contoso.sharepoint.com」と入力します。
    
2. Office 365 サイトにサインインします。
    
3. アドインが要求しているアクセス許可に同意を付与するように求められた場合、**[信頼する]** を選択します。/_layouts/15/OAuthAuthorize.aspx ページにリダイレクトされることに注意してください。 
    
    
                **メモ:**ユーザーは、アドインが要求しているアクセス許可に同意するために、**管理**アクセス許可を持っている必要があります。 詳細は、「[SharePoint のアドイン用の認証コードの OAuth フロー](http://msdn.microsoft.com/library/e89e91c7-ea39-49b9-af5a-7f047a7e2ab7%28Office.15%29.aspx)」で説明します。

4. **[Contoso に正常に接続されました]** で、作成する新しいリストの名前を入力し、**[リストの作成]** を選択します。
    
5. Contoso サイトに属するすべてのリストを示す **[Contoso 内のリスト]** に、新しいリストが示されていることを確認します。 
    
**[Office 365 に接続]** で **[接続]** を選択すると、Controllers\HomeController.cs の **Connect** が呼び出され、続いて **TokenRepository.Connect** が呼び出されます。 **[Office 365 に接続]** のユーザーによって入力された URL は、**hostUrl** として **TokenRepository.Connect** に渡されます。

**メモ:**  この記事で提供されるコードは、明示または黙示のいかなる種類の保証なしに現状のまま提供されるものであり、特定目的への適合性、商品性、権利侵害の不存在についての暗黙的な保証は一切ありません。

```C#
 public ActionResult Connect(string hostUrl)
        {
            TokenRepository repository = new TokenRepository(Request, Response);
            repository.Connect(hostUrl);
            return View();            
        }
```


                **TokenRepository.Connect** は **TokenHelper.GetAuthorizationUrl** を呼び出します。 
                **TokenHelper.GetAuthorizationUrl** は、**hostUrl** と SharePoint リソースに対して必要なアクセス許可を使用して、OAuthAuthorize.aspx へのリダイレクト URL を返します。 OAuthAuthorize.aspx は、OAuth を使用してユーザーを承認するために使用されます。 OAuthorize.aspx にリダイレクトされたユーザーは、Office 365 にサインインして、アドインが要求するアクセス許可に同意するか、アドインを信頼する必要があります。 SharePoint リソースで必要なアクセス許可は **Web.Manage** です。 ユーザー認証の後に、コード サンプルは SharePoint サイトにリストを作成します。 SharePoint サイトにリストを作成するには、ユーザーが **Web.Manage** アクセス許可を持っている必要があります。


                **メモ:****TokenHelper.GetAuthorizationUrl** は、**https://contoso.sharepoint.com/_layouts/15/OAuthAuthorize.aspx?IsDlg=1&amp;client_id=<Client ID>&amp;scope=Web.Manage&amp;response_type=code** 形式の URL を返します。この **&lt;Client ID&gt;** は、アドインのクライアント ID です。 アドインを販売者ダッシュボードから登録した場合は、アドインは任意の Office 365 サイトでインストールできます。 アドインを販売者ダッシュボードから登録しない場合は、appregnew.aspx を使用してアドインを登録した後で、Core.DynamicPermissionsWeb\web.config を更新する必要があります。 詳細については、「[Register SharePoint アドイン 2013 を登録する](http://msdn.microsoft.com/library/be41a5dc-2af9-4fd9-bf4e-ad6dfa849524%28Office.15%29.aspx)」を参照してください。

```C#
 public void Connect(string hostUrl)
        {
            if (!IsConnectedToO365)
            {
                HttpCookie spHostUrlCookie = new HttpCookie("SPHostUrl");
                spHostUrlCookie.Value = hostUrl;
                spHostUrlCookie.Expires = DateTime.Now.AddYears(5);
                _response.Cookies.Add(spHostUrlCookie);
                _response.Redirect(TokenHelper.GetAuthorizationUrl(hostUrl, "Web.Manage"));
            }
        }
```

承認後、アドインは Controllers\HomeController.cs の **Callback** にリダイレクトされます。これは appregnew.aspx に指定されたリダイレクト URI です。 
                **TokenHelper** は承認コード (**code**) を **Callback** に渡します。 この記事で後述するアクセス トークンを付与するには、承認コード (**code**) が **Callback** に返される必要があります。 その後、**Callback** は **TokenRepository.Callback** を呼び出します。

```C#
 public ActionResult Callback(string code)
        {
            TokenRepository repository = new TokenRepository(Request, Response);
            repository.Callback(code);
            return RedirectToAction("Index");
        }
```


            **TokenRepository.Callback** は **TokenCache.UpdateCacheWithCode** を呼び出します。これは **TokenHelper.GetAccessToken** を使用して、承認コード (**code**) に基づく OAuth アクセス トークンを取得します。

```C#
public void Callback(string code)
        {
            HttpCookie spHostUrlCookie = _request.Cookies["SPHostUrl"];
            if (null != spHostUrlCookie)
            {
                Uri sharePointSiteUrl = new Uri(spHostUrlCookie.Value);
                TokenCache.UpdateCacheWithCode(_request, _response, sharePointSiteUrl);
            }
        }
```

```C#
 public static void UpdateCacheWithCode(HttpRequestBase request, HttpResponseBase response, Uri targetUri)
        {
            string refreshToken = TokenHelper.GetAccessToken(request.QueryString["code"], "00000003-0000-0ff1-ce00-000000000000", targetUri.Authority, TokenHelper.GetRealmFromTargetUrl(targetUri), new Uri(request.Url.GetLeftPart(UriPartial.Path))).RefreshToken;
            SetRefreshTokenCookie(response.Cookies, refreshToken);
            SetRefreshTokenCookie(request.Cookies, refreshToken);
        }
```

これで、アドインはこのユーザー用のアクセス トークンを持つようになり、SharePoint サイトにリストを作成することができます。 

## その他のリソース
<a name="bk_addresources"> </a>

- Office 365 開発パターンとプラクティス (ソリューション ガイダンス)
    
