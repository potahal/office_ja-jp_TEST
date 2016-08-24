---
ms.Toctitle: Integrate Office 365 APIs into Visual Studio
title: ".NET Visual Studio プロジェクトへの Office 365 API の統合"
description: "ADAL を使用して Azure Active Directory で認証してから、Office 365 探索サービスを使用してリソース エンドポイントを特定し、Office 365 ユーザー データにアクセスします。"
ms.ContentId: 8c30ca03-6433-4fcf-a01d-fd142620d12f
ms.date: June 2, 2015

---
[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]



# .NET Visual Studio プロジェクトへの Office 365 API の統合

_**適用対象:**Office 365_


[Office 365 サービスをプロジェクトに追加](..\howto\adding-service-to-your-Visual-Studio-project.md)したら、アプリ ユーザーを認証して、彼らの Office 365 データにアクセスするコードを記述する必要があります。 

MVC などの Web アプリケーションを構築するのか、Windows ストア アプリなどのネイティブ アプリケーションを構築するのかによって、認証プロセスが異なります。次のリンクのいずれかを選択して、Office 365 API を使用したアプリケーションを構築する例を参照してください。 

- [MVC アプリケーションへの Office 365 API の統合](#bk_IntegrateMVC)
- [Windows ストア アプリへの Office 365 API の統合](#bk_IntegrateWindowsStore)

**メモ** これらのチュートリアルでは、[Office Developer Tools for Visual Studio](http://aka.ms/OfficeDevToolsForVS2013) の最新バージョンを使用します。以前のバージョンのツールを使用している場合は、これらの例でコードを正常に動作させるために、最新版へのアップグレードが必要になる可能性があります。


<a name="bk_IntegrateMVC"> </a>
##MVC Visual Studio プロジェクトへの Office 365 API の統合


これから構築する例は、Azure AD シングル サインオンを使用して認証し、Office 365 からユーザーの連絡先情報を 読み取るシングルテナント MVC です。GitHub 上の [Office 365 シングルテナント MVC プロジェクト](https://github.com/OfficeDev/O365-WebApp-SingleTenant)をモデルとして使用します。 

アプリケーションでは、キャッシュにデータベースを使用する永続的な [Active Directory 認証ライブラリ](https://msdn.microsoft.com/en-us/library/azure/jj573266.aspx) (ADAL) トークン キャッシュを採用します。 ADAL により、Active Directory (AD) (この例では Azure AD) に対してユーザーを認証し、API 呼び出しをセキュリティ保護するためのアクセス トークンを取得できます。 

この例の構築を成功させるためには、Office 365 API に対する次のアクセス許可を設定しておく必要があります。 

- **ユーザーおよびグループ** API の場合は、**サインオンを有効にしてユーザーのプロファイルを読み取る**ためのアクセス許可を設定します。

- **連絡先** API の場合は、**ユーザーの連絡先を読み取る**ためのアクセス許可を設定します。

Office 365 ツールを開いて上記のアクセス許可が設定されているかどうかを確認するには、**ソリューション エクスプローラー**で、プロジェクトを右クリックして、**[追加]** > **[接続済みサービス]** の順に選択します。

ここから、例の構築は次の 3 つの主要なステップで構成されます。

 - [Azure AD を使用した認証用にプロジェクトを構成する](#bk_AuthenticateAzure)

 - [Azure Active Directory を使用して認証する](#bk_AuthenticateAzureAD)

 - [Office 365 API にアクセスする](#bk_accessOffice365APIs)


<a name="bk_AuthenticateAzure"> </a>
###Azure Active Directory を使用した認証用にプロジェクトを構成する

構築中の例では Office 365 資格情報認証のみが必要になるため、[OWIN](http://owin.org/) [Katana](http://katanaproject.codeplex.com/documentation) と [Active Directory 認証ライブラリ](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) (ADAL) を使用します。

ただし、まずは、Secure Socket Layer (SSL) を使用するようにプロジェクトを構成します。

####プロジェクトの SSL を有効にする 

Visual Studio では、既定で、プロジェクトが SSL 用に構成されません。プロジェクトで SSL を有効にしていない場合は、ここで、該当するリダイレクト URL を使用して Office 365 サービスを更新します。

SSL を有効にするには:

1. **ソリューション エクスプローラー**で、プロジェクトをクリックします。
2. **[プロパティ]** で、**[SSL 有効]** を **[True]** に設定します。
    
    Visual Studio により、**[SSL URL]** の値が指定されます。

次に、ホーム ページとして HTTPS エンドポイントを使用するようにアプリケーションを更新します。

1.  **ソリューション エクスプローラー**で、プロジェクトを右クリックして **[プロパティ]** を選択します。
2.  **[Web]** タブを選択します。 **[サーバー]** で、**[プロジェクトの URL]** を Visual Studio が **SSL URL** 用に作成した HTTPS エンドポイントに設定します。

最後に、Office 365 サービスを更新します。

3. **ソリューション エクスプローラー**でプロジェクトを右クリックして、**[追加]** > **[接続済みサービス]** の順に選択します。 必要に応じてサインインします。

    Visual Studio から、プロジェクト内の 1 つ以上のリダイレクト URL が Azure AD 内のアプリケーション エントリに存在しないことが通知されます。

4. **[はい]** をクリックして、セキュリティで保護されたリダイレクト URL を Azure AD 内のアプリケーション エントリに追加します。
5. **[OK]** をクリックして、**サービス マネージャー**を終了します。


<a name="bk_AuthenticateDownloadNuGets"> </a>
####認証に必要な NuGet パッケージを管理する

次の Azure AD および OWIN Katana パッケージを認証用にインストールする必要があります。

- [Active Directory 認証ライブラリ](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory)
- [EntityFramework](https://www.nuget.org/packages/EntityFramework)
- [Microsoft.Owin.Host.SystemWeb](https://www.nuget.org/packages/Microsoft.Owin.Host.SystemWeb)
- [Microsoft.Owin.Security.Cookies](https://www.nuget.org/packages/Microsoft.Owin.Security.Cookies)
- [Microsoft.Owin.Security.OpenIdConnect](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect)

これらのパッケージは、依存関係として選択された NuGet パッケージもインストールします。

NuGet パッケージをダウンロードしてインストールするには:

1. **ソリューション エクスプローラー**でプロジェクトを右クリックします。
2. **[NuGet パッケージの管理...]** を選択します。 
3. **[NuGet パッケージの管理]** で、[**オンライン**] をクリックします。
4. 検索ボックスを使用して必要なパッケージを探します。
5. パッケージを選択して、**[インストール]** をクリックします。
6. **[プロジェクトの選択]** で、**[OK]** をクリックします。
7. 画面の指示に従って、すべてのライセンスを受け入れます。

####認証を有効にするように web.config ファイルを更新します。

Office 365 サービスをプロジェクトに追加すると、Visual Studio が次のプロパティを Azure AD 内のアプリケーションのレジストリとソリューション内の **web.config** ファイルに追加します。

    <add key="ida:ClientId" value="app-azure-ad-client-id" />
    <add key="ida:ClientSecret" value="app-azure-ad-password" />
    <add key="ida:TenantId" value="azure-tenant-where-app-is-registered" />
    <add key="ida:AADInstance" value="https://login.microsoftonline.com/" 

<!--
In addition, to successfully authenticate against Azure AD, a single-tenant web application must be able to specify the login authority, including the appropriate tenant ID. The authority follows the pattern

    https://login.windows.net/[tenantId]

where [tenantId] is the ID of the Azure tenant in which the application is registered.

Because of this, you'll need to add the tenant ID to the project's web.config file. You can retrieve the tenant ID for your application from the [Azure Management Portal](https://manage.windowsazure.com).
 

1. Sign into the [Azure Management Portal](https://manage.windowsazure.com/), using your Office 365 subscription credentials.

2. In the left navigation panel, select **Active Directory**. Make sure the **Directory** tab is selected, and then click on the directory name for the Office 365 tenancy is which your app is registered.

    At this point, the URL in the browser window will include a GUID that is the tenant ID of your directory.

3. Copy only the GUID from the browser URL. This is the ID for the tenancy in which your application is registered.

After you have copied the tenant ID, you can add it to the project's web.config file. 

1. In **Solution Explorer**, double-click **Web.config**.
2. In the Web.config file, in the **appSettings** section, add a new key, containing the tenant ID, directly under the ida:AuthorizationUri key:

        <add key="ida:TenantId" value="your-Office-365-tenant-ID"/>

3. Save the file.
-->



####ADAL 認証トークンを管理するためのデータベースを作成します。

次に、ADAL トークン キャッシュとして機能するデータベースをプロジェクトに追加します。

ADAL トークン キャッシュの詳細については、「[ADAL v2 の新しいトークン キャッシュ](http://www.cloudidentity.com/blog/2014/07/09/the-new-token-cache-in-adal-v2/)」を参照してください。


ADAL トークン キャッシュとして機能するデータベースを追加するには、次のようにします。

1. **ソリューション エクスプローラー**で、**App_Data** フォルダーを右クリックして、**[追加]** > **[新しいアイテム...]** の順に選択します。
2. **[データ]** が選択されていることを確認してから、**[SQL Server データベース]** を選択します。 データベースに「**ADALTokenCacheDb**」という名前を付け、**[追加]** をクリックします。

データベース接続がプロジェクト web.config に追加されていることを確認します。

1. **ソリューション エクスプローラー**で、**web.config** をダブルクリックします。
2. 次のセクションが web.config ファイルの **appSettings** セクションの後ろに追加されていることを確認します。 存在しなかった場合は、そのセクションを追加します。

<connectionStrings>
  <add name="DefaultConnection"
    connectionString="Data Source=(LocalDB)\v11.0;AttachDbFilename=|DataDirectory|\ADALTokenCacheDb.mdf;Integrated Security=True" providerName="System.Data.SqlClient" />
</connectionStrings>

    <connectionStrings>
        <add name="DefaultConnection"
         connectionString="Data Source=(LocalDB)\v11.0;AttachDbFilename=|DataDirectory|\ADALTokenCacheDb.mdf;Integrated Security=True" providerName="System.Data.SqlClient" />
    </connectionStrings>



<ol start="3">
<li>ファイルを保存します。</li>
</ol>


####認証に必要なファイルをコピーする

次に、[Office 365 シングルテナント MVC プロジェクト](https://github.com/OfficeDev/O365-WebApp-SingleTenant) GitHub プロジェクトからいくつかのファイルをインポートします。それぞれのファイルの使用目的について説明します。

GitHub プロジェクトからファイルをコピーするには、次のようにします。

1. ブラウザーで、コピーするファイルに移動します  (ファイルが下に一覧表示されます)。
2. **[RAW]** ボタンを右クリックして、**[対象をファイルに保存...]** を選択します。
3. ファイルをコンピューターに保存します。
4. Visual Studio の**ソリューション エクスプローラー**のプロジェクト ノードで、後述するように、指定したフォルダーを右クリックして、**[追加]** > **[既存の項目...]** の順に選択します。
5. コンピューター上の保存場所からファイルを選択します。 

次のファイルをプロジェクト内の **Models** フォルダーにコピーします。これらのクラスは、永続的な ADAL トークン キャッシュをどのように構築してトークンを保存するために、使用できるかを示しています。

- [ADALTokenCache.cs](https://github.com/OfficeDev/O365-WebApp-SingleTenant/blob/master/O365-WebApp-SingleTenant/Models/ADALTokenCache.cs)
- [ApplicationDbContext.cs](https://github.com/OfficeDev/O365-WebApp-SingleTenant/blob/master/O365-WebApp-SingleTenant/Models/ApplicationDbContext.cs)

新しいフォルダー **Utils** を作成します。

1. **ソリューション エクスプローラー**で、プロジェクトを右クリックして、**[追加]** > **[新しいフォルダー]** の順に選択します。
2. フォルダーに「**Utils**」という名前を付けます。 

次のファイルをそのフォルダーにコピーします。このヘルパー クラスには、起動中に ADAL 認証オブジェクトを作成する必要があるプロジェクトの web.config ファイルから値を読み取るメンバー変数が含まれています。 

- [SettingsHelper.cs](https://github.com/OfficeDev/O365-WebApp-SingleTenant/blob/master/O365-WebApp-SingleTenant/Utils/SettingsHelper.cs)


最後に、[_LoginPartial.cshtml](https://github.com/OfficeDev/O365-WebApp-SingleTenant/blob/master/O365-WebApp-SingleTenant/Views/Shared/_LoginPartial.cshtml) ファイルをプロジェクトの **Views** > **Shared** フォルダーにコピーします。


<a name="bk_AuthenticateAzureAD"> </a>
###Azure Active Directory を使用して認証する

これでプロジェクトが認証用に構成されたため、Azure AD を使用して認証を処理する機能を実際に追加できます。

####Azure AD シングル サインオンを構成する

次に、実際に認証を処理するために、OWIN スタートアップ クラスをプロジェクトに追加する必要があります。 OWIN スタートアップ クラスを追加するには、次のようにします。

1. ソリューション エクスプローラーで、プロジェクト名を右クリックして、**[追加]** > **[新しいアイテム...]**の順に選択します。
2. **[新しいアイテムの追加]** の **[Web]** で **[全般]** を選択し、**[OWIN スタートアップ クラス]** を選択します。
3. **[名前]** に「**Startup**」と入力します。
3. **[追加]** をクリックします。

新しいスタートアップ クラスがプロジェクトに追加されます。 

次に、認証を処理するコードを追加します。

1. ファイル内の名前空間参照を次のように置き換えます。


        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Owin.Security;
        using Microsoft.Owin.Security.Cookies;
        using Microsoft.Owin.Security.OpenIdConnect;
        using O365_WebApp_SingleTenant.Models;
        using O365_WebApp_SingleTenant.Utils;
        using Owin;
        using System;
        using System.IdentityModel.Claims;
        using System.Threading.Tasks;
        using System.Web;
        using Microsoft.Owin;



<ol start="2">
<li>次のメソッド ConfigureAuth をスタートアップ クラスに追加します。</li>
</ol>

        public void ConfigureAuth(IAppBuilder app)
        {
            app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

            app.UseCookieAuthentication(new CookieAuthenticationOptions());

            app.UseOpenIdConnectAuthentication(
                new OpenIdConnectAuthenticationOptions
                {
                    ClientId = SettingsHelper.ClientId,
                    Authority = SettingsHelper.Authority,

                    Notifications = new OpenIdConnectAuthenticationNotifications()
                    {                       
                        // If there is a code in the OpenID Connect response, redeem it for an access token and refresh token, and store those away.
                        AuthorizationCodeReceived = (context) =>
                        {
                            var code = context.Code;
                            ClientCredential credential = new ClientCredential(SettingsHelper.ClientId, SettingsHelper.AppKey);
                            String signInUserId = context.AuthenticationTicket.Identity.FindFirst(ClaimTypes.NameIdentifier).Value;

                            AuthenticationContext authContext = new AuthenticationContext(SettingsHelper.Authority, new ADALTokenCache(signInUserId));
                            AuthenticationResult result = authContext.AcquireTokenByAuthorizationCode(code, new Uri(HttpContext.Current.Request.Url.GetLeftPart(UriPartial.Path)), credential, SettingsHelper.AADGraphResourceId);

                            return Task.FromResult(0);
                        },
                        RedirectToIdentityProvider = (context) =>
                        {
                            // This ensures that the address used for sign in and sign out is picked up dynamically from the request
                            // this allows you to deploy your app (to Azure Web Sites, for example)without having to change settings
                            // Remember that the base URL of the address used here must be provisioned in Azure AD beforehand.
                            string appBaseUrl = context.Request.Scheme + "://" + context.Request.Host + context.Request.PathBase;
                            context.ProtocolMessage.RedirectUri = appBaseUrl + "/";
                            context.ProtocolMessage.PostLogoutRedirectUri = appBaseUrl;

                            return Task.FromResult(0);
                        },
                        AuthenticationFailed = (context) =>
                        {
                            // Suppress the exception if you don't want to see the error
                            context.HandleResponse();
                            return Task.FromResult(0);
                        }
                    }

                });
        }

<ol start="3">
<li>ConfigureAuth メソッドを呼び出すコードを Startup.Configuration メソッドに追加します。</li>
</ol>

        public void Configuration(IAppBuilder app)
        {
            ConfigureAuth(app);
        }


####Office 365 のサインインおよびサインアウト用のコントローラーを追加する

サインインとサインアウトを処理するためのアカウント コントローラーを追加します。

1. **ソリューション エクスプローラー**で、**Controllers** フォルダーを右クリックして、**[追加]** > **[コントローラー]** の順に選択します。
2. **[MVC 5 コントローラー - 空]** を選択して、**[追加]** をクリックします。
3. コントローラー名として「**AccountController**」と入力し、[**追加**] をクリックします。
4. コントローラー ファイル内の名前空間参照を次のように置き換えます。


        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Owin.Security;
        using Microsoft.Owin.Security.Cookies;
        using Microsoft.Owin.Security.OpenIdConnect;
        using O365_WebApp_SingleTenant.Utils;
        using System.Security.Claims;
        using System.Web;
        using System.Web.Mvc;


<ol start="5">
<li>Index() メソッドを削除します。</li>
<li>サインインとサインアウトを処理する次のメソッドを AccountController クラスに追加します。</li>
</ol>
 

        public void SignIn()
        {
            if (!Request.IsAuthenticated)
            {
                HttpContext.GetOwinContext().Authentication.Challenge(new AuthenticationProperties { RedirectUri = "/" }, OpenIdConnectAuthenticationDefaults.AuthenticationType);
            }
        }
        public void SignOut()
        {
            string callbackUrl = Url.Action("SignOutCallback", "Account", routeValues: null, protocol: Request.Url.Scheme);

            HttpContext.GetOwinContext().Authentication.SignOut(
                new AuthenticationProperties { RedirectUri = callbackUrl },
                OpenIdConnectAuthenticationDefaults.AuthenticationType, CookieAuthenticationDefaults.AuthenticationType);
        }

        public ActionResult SignOutCallback()
        {
            if (Request.IsAuthenticated)
            {
                // Redirect to home page if the user is authenticated.
                return RedirectToAction("Index", "Home");
            }

            return View();
        }

<a name="bk_accessOffice365APIs"> </a>
###Office 365 API にアクセスする

これで、ユーザーの Office 365 データにアクセスするコードを追加する準備ができました。

####連絡先情報ビューのモデルを作成する 

まず、連絡先ビューの基になるクラスを作成する必要があります。

1. **ソリューション エクスプローラー**で、**Models** フォルダーを右クリックして、**[追加]** > **[クラス...]** の順に選択します。
2. **[クラス]** を選択して、名前として「**MyContact**」と入力し、**[追加]** をクリックします。
3. クラスで、次のコード行を追加します。


        public class MyContact
        {
            public string Name { get; set; }
        }

<ol start="4">
<li>ファイルを保存します。</li>
</ol>



####連絡先情報用のコントローラーを追加する

次に、ユーザーの連絡先情報用のコントローラーを追加します。

1. **ソリューション エクスプローラー**で、**Controllers** フォルダーを右クリックして、**[追加]** > **[コントローラー]** の順に選択します。
2. **[MVC 5 コントローラー - 空]** を選択して、**[追加]** をクリックします。
3. コントローラー名として「**ContactsController**」と入力して、**[追加]** をクリックします。

    Visual Studio が新しい空のコントローラーをプロジェクトに追加します。

4. コントローラー ファイル内の名前空間参照を次のように置き換えます。

        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Office365.Discovery;
        using Microsoft.Office365.OutlookServices;
        using O365_WebApp_SingleTenant.Models;
        using O365_WebApp_SingleTenant.Utils;
        using System.Collections.Generic;
        using System.Security.Claims;
        using System.Threading.Tasks;
        using System.Web.Mvc;


    必ず、プロジェクトの Models 名前空間に対する名前空間参照を追加します。


            using [projectname].Models;


<ol start="5">
<li>[Authorize] 属性を ContactsController クラスに追加します。</li>
</ol>

[Authorize] 属性は、このコントローラーにアクセスする前に、ユーザーが認証されていることを要求します。 


        [Authorize]
        public class ContactsController : Controller

<ol start="6">
<li>Index() メソッドを非同期に変更します。</li>
</ol> 

        public async Task<ActionResult> Index()

<ol start="7">
<li>Index() メソッドに次のコードを追加します。</li>
</ol>   

このコードは、サインインしている Office 365 ユーザー Id と Office 365 テナントに対する権限を使用して、認証コンテキストを作成します。


            List<MyContact> myContacts = new List<MyContact>();

            var signInUserId = ClaimsPrincipal.Current.FindFirst(ClaimTypes.NameIdentifier).Value;
            var userObjectId = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;

            AuthenticationContext authContext = new AuthenticationContext(SettingsHelper.Authority, new ADALTokenCache(signInUserId));

<ol start="8">
<li><b>try-catch</b> ブロックを Index() メソッド内の上記コードのすぐ下に追加します。</li>
</ol>


            try
            {
 
            }
            catch (AdalException exception)
            {
                //handle token acquisition failure
                if (exception.ErrorCode == AdalError.FailedToAcquireTokenSilently)
                {
                    authContext.TokenCache.Clear();

                    //handle token acquisition failure
                }
            }

            return View(myContacts);

<ol start="9">
<li>上記コードの <b>try</b> ブロックで、次のコードを Index() メソッドに追加します。</li>
</ol>

このコードは、アプリケーションのクライアント id とアプリ パスワードのほかにユーザーの資格情報も渡すことによって、探索サービスへのアクセス トークンを取得しようとします。ユーザー認証トークンがキャッシュされているため、コントローラーは、ユーザーに資格情報の入力を要求するダイアログを表示しなくても、必要なアクセス トークンを取得できます。

アクセス トークンを使用すれば、アプリケーションは、探索サービス クライアント オブジェクトを作成し、探索クライアント オブジェクトを使用して Office 365 サービス連絡先 API のリソース エンドポイントを特定できます。

                DiscoveryClient discClient = new DiscoveryClient(SettingsHelper.DiscoveryServiceEndpointUri,
                    async () =>
                    {
                        var authResult = await authContext.AcquireTokenSilentAsync(SettingsHelper.DiscoveryServiceResourceId, new ClientCredential(SettingsHelper.ClientId, SettingsHelper.AppKey), new UserIdentifier(userObjectId, UserIdentifierType.UniqueId));

                        return authResult.AccessToken;
                    });

                var dcr = await discClient.DiscoverCapabilityAsync("Contacts");


<ol start="10">
<li>最後に、<b>try</b> ブロック内の上記コードのすぐ下で、次のコードを Index() メソッドに追加します。</li>
</ol> 


このコードは、ダイアログを表示せずにアプリケーションとユーザーの資格情報を渡す Office 365 サービス連絡先 API のリソース エンドポイントに接続して、Outlook サービスに対するアクセス トークンを取得します。
    
アクセス トークンを受信すると、このコードは Outlook クライアント オブジェクトを初期化できます。その後で、Outlook クライアント オブジェクトの **Me** プロパティを使用して、このユーザーの連絡先情報を取得します。    

最後に、コントローラーがユーザーの連絡先の一覧を読み取って、表示名が一覧表示されたビューを返します。


                OutlookServicesClient exClient = new OutlookServicesClient(dcr.ServiceEndpointUri,
                    async () =>
                    {
                        var authResult = await authContext.AcquireTokenSilentAsync(dcr.ServiceResourceId, new ClientCredential(SettingsHelper.ClientId, SettingsHelper.AppKey), new UserIdentifier(userObjectId, UserIdentifierType.UniqueId));

                        return authResult.AccessToken;
                    });

                var contactsResult = await exClient.Me.Contacts.ExecuteAsync();

                do
                {
                    var contacts = contactsResult.CurrentPage;
                    foreach (var contact in contacts)
                    {
                        myContacts.Add(new MyContact { Name = contact.DisplayName });
                    }

                    contactsResult = await contactsResult.GetNextPageAsync();

                } while (contactsResult != null);

 

完成した Index() メソッドは次のようになるはずです。

        public async Task<ActionResult> Index()
        {
            List<MyContact> myContacts = new List<MyContact>();

            var signInUserId = ClaimsPrincipal.Current.FindFirst(ClaimTypes.NameIdentifier).Value;
            var userObjectId = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;

            AuthenticationContext authContext = new AuthenticationContext(SettingsHelper.Authority, new ADALTokenCache(signInUserId));
            
            try
            {
                DiscoveryClient discClient = new DiscoveryClient(SettingsHelper.DiscoveryServiceEndpointUri,
                    async () =>
                    {
                        var authResult = await authContext.AcquireTokenSilentAsync(SettingsHelper.DiscoveryServiceResourceId, new ClientCredential(SettingsHelper.ClientId, SettingsHelper.AppKey), new UserIdentifier(userObjectId, UserIdentifierType.UniqueId));

                        return authResult.AccessToken;
                    });

                var dcr = await discClient.DiscoverCapabilityAsync("Contacts");                

                OutlookServicesClient exClient = new OutlookServicesClient(dcr.ServiceEndpointUri,
                    async () =>
                    {
                        var authResult = await authContext.AcquireTokenSilentAsync(dcr.ServiceResourceId, new ClientCredential(SettingsHelper.ClientId, SettingsHelper.AppKey), new UserIdentifier(userObjectId, UserIdentifierType.UniqueId));

                        return authResult.AccessToken;
                    });

                var contactsResult = await exClient.Me.Contacts.ExecuteAsync();

                do
                {
                    var contacts = contactsResult.CurrentPage;
                    foreach (var contact in contacts)
                    {
                        myContacts.Add(new MyContact { Name = contact.DisplayName });
                    }

                    contactsResult = await contactsResult.GetNextPageAsync();

                } while (contactsResult != null);
            }
            catch (AdalException exception)
            {
                //handle token acquisition failure
                if (exception.ErrorCode == AdalError.FailedToAcquireTokenSilently)
                {
                    authContext.TokenCache.Clear();

                    //handle token acquisition failure
                }
            }

            return View(myContacts);
        }


####連絡先データ用のビューを作成する

次に、ユーザーの連絡先情報用のビューを作成します。以前作成した **MyContact** クラスに基づいてビューを作成することによって、これを実現します。

1. **ソリューション エクスプローラー**で、**Contacts** フォルダーを右クリックして、**[追加]** > **[ビュー]** の順に選択します。
2. **[ビューの追加]** ダイアログ ボックスで、新しいビューを定義します。
    - **[ビュー名]** に「**Index**」と入力します。 
    - **[テンプレート]** で **[一覧]** を選択します。 
    - **[モデル クラス]** で **[MyContact]** を選択します。
    - **[追加]** をクリックします。



####ナビゲーション バーに [個人用の連絡先] リンクを追加する

1. **ソリューション エクスプローラー**の **Shared** フォルダーで、_Layout.cshtml ファイルをダブルクリックします。

2. [個人用の連絡先] ページ用のアクション リンクを追加して、ユーザー ログイン用の部分クラスを挿入します。ナビゲーション バーの HTML を自動生成されたものから更新します。 

            <div class="navbar-collapse collapse">
                <ul class="nav navbar-nav">
                    <li>@Html.ActionLink("Home", "Index", "Home")</li>
                    <li>@Html.ActionLink("About", "About", "Home")</li>
                    <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
                </ul>
            </div>


        
次のように、[個人用の連絡先] ページ用のアクション リンクを追加して、ユーザー ログイン用の部分クラスを挿入します。

            <div class="navbar-collapse collapse">
                <ul class="nav navbar-nav">
                    <li>@Html.ActionLink("Home", "Index", "Home")</li>
                    <li>@Html.ActionLink("About", "About", "Home")</li>
                    <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
                    <li>@Html.ActionLink("My Contacts", "Index", "Contacts")</li>
                </ul>
                @Html.Partial("_LoginPartial")
            </div>




###アプリケーションをデバッグする


これで、ソリューションを実行する準備ができました。 F5 キーを押して Web アプリケーションをデバッグします。

これが新しい開発環境の場合は、Visual Studio から IIS SSL 証明書を構成するように要求されます。 **[はい]** を 2 回クリックして先に進みます。

1. Visual Studio が、選択されたブラウザーで Web アプリケーションを起動します。

2. ページの右上隅にある [**サインイン**] をクリックして、Web アプリケーションを登録した Office 365 テナントにサインインします。

    組織のユーザーのみがシングルテナント アプリケーションへのサインインを許可されるため、同意する必要はありません。 アプリケーションがマルチ テナントの場合は、Azure AD にアプリが要求されたアクセス許可が一覧表示される同意ページが表示されるため、アプリにそれらのアクセス許可を付与することに同意できます。


5. サインインすると、ホーム ページの最上部のナビゲーション バーに自分の電子メール アドレスと [**サインアウト**] が表示されるはずです。

6. **[個人用の連絡先]** をクリックします。

    **[個人用の連絡先]** ページで、テナントから Exchange 連絡先の名前が取得され、表示されます。


これで、Office 365 API をカスタマイズして統合するための Web アプリケーション プロジェクトが完成しました。

[次のステップ](#bk_nextsteps)


<a name="bk_IntegrateWindowsStore"> </a>
##Windows Visual Studio プロジェクトへの Office 365 API の統合

Windows ストアや Phone アプリなどのネイティブなクライアント アプリケーションに Office 365 API を統合する方法の例については、GitHub 上の [Office 365 Windows アプリ プロジェクト](https://github.com/OfficeDev/O365-Windows)を参照できます。プロジェクトがユーザーのファイルの一覧を取得します。

[AuthenticationHelper.cs](https://github.com/OfficeDev/O365-Windows/blob/master/O365-Windows/O365-Windows/Helpers/AuthenticationHelper.cs) クラスに、Azure AD for Office 365 リソースからのトークンを認証して取得するコードが含まれています。サンプルでは、マイ ファイル リソース用のトークンを取得します。 

この操作は **GetAccessToken** メソッド内で行われます。トークンを取得するために、まず、Office 365 探索サービスに問い合わせてアプリ機能を特定します。マイ ファイル アクセス許可を要求するようにアプリが構成されているため、Office 365 探索サービスは、リソース Id やエンドポイント URI などのマイ ファイル機能に関する情報を返します。こうして、マイ ファイル リソース用のアクセス トークンが取得されます。

###最後のテナント Id の保存

デバイス アプリケーションは既定ではマルチテナント アプリケーションになっているため、ユーザーがサインインに成功してアプリに同意するまで、アプリはどのテナントのどのユーザーがアプリにサインインしようとしているのかを認識できません。

マルチテナント アプリケーションの場合は、外部の Office 365 ユーザーがそのアプリケーションにサインイン して使用できることに注意してください。アプリを Windows ストアに公開すれば、ユーザーがそのアプリをダウンロードして、Office 365 テナントにサインインし、アプリケーションを使用できます。Azure AD が、サインインしているユーザーのテナントごとに、ユーザーの代わりに Windows アプリケーションを登録します。

ユーザーの同意を得たアプリは、アプリ設定に保存されている、サインインしているユーザーのテナント ID を取得できます。その後で、アプリは、ユーザーがアプリを開いたり、要求を発行したりしたときに、このテナント ID を使用して認証コンテキストを初期化することができます。これにより、ユーザーは要求が発行されるたびに同意する必要がなくなります。初めてアプリにアクセスしたときにだけ同意すれば済みます。この状態は、アクセス トークンの有効期限が切れるまで続きます。


<a name="bk_nextsteps"> </a>
## 次のステップ

Office 365 API にアクセスしたら、API 経由で利用可能なユーザー リソースを操作できます。Visual Studio から提供されるクライアント ライブラリを使用して共通タスクを実行する方法とそのコード スニペットについては、以下を参照してください。

- [Outlook メール API リファレンス](..\api\mail-rest-operations.md)
- [Outlook 予定表 API リファレンス](..\api\calendar-rest-operations.md)
- [Outlook 連絡先 API リファレンス](..\api\contacts-rest-operations.md)
- [ファイル API リファレンス](..\api\files-rest-operations.md)

<a name="bk_addresources"> </a>
## その他のリソース

**コード サンプル**

-  [Office 365 API スタート プロジェクトとコード サンプル](..\howto\Starter-projects-and-code-samples.md) 
-  [GitHub 上の Office 開発者](https://github.com/OfficeDev)


**Visual Studio での開発**

-  [Office 365 開発環境のセットアップ](..\howto\setup-development-environment.md)


-  [Office 365 API 入門](http://www.microsoftvirtualacademy.com/training-courses/introduction-to-office-365-development?m=10072&ct=31602) (トレーニングビデオ) 


**リファレンス**

-  [Office 365 API リファレンス](..\api\API-Catalog.md)




  













<!--
After you've [added an Office 365 service to your project](..\howto\adding-service-to-your-Visual-Studio-project.md), you'll need to write code that authenticates the app user and accesses their Office 365 data. This includes: 



1. [Authenticating with Azure Active Directory](#bk_AuthenticateAzure)
2. [Use the Office 365 Discovery Service to determine resource endpoints](#bk_discoveryContext)
3. [Use endpoints to access Office 365 user data and resources](#bk_accessOffice365resources)



<a name="bk_AuthenticateAzure"> </a>
## Authenticate with Azure Active Directory

The Office 365 APIs use the Active Directory Authentication Library (ADAL) with Azure Active Directory for authentication. Use the [Active Directory Authentication Library](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) (ADAL) to authenticate with Azure Active Directory for Office 365. 


The authentication requirements your code needs to address can vary greatly, depending on:

- Whether the application you're building is a web application or native app
- Whether you want to make your application available to a single or multiple tenants
- Any specific business scenarios your application might be addressing

For detailed examples of how to handle various authentication scenarios, see [Microsoft Azure Active Directory Samples and Documentation](https://github.com/AzureADSamples) on [github](https://github.com). The following examples are especially relevant to Office 365 authentication scenarios:

- [WebApp-WebAPI-MultiTenant-OpenIdConnect-DotNet](https://github.com/AzureADSamples/WebApp-WebAPI-MultiTenant-OpenIdConnect-DotNet)
- [WebApp-WebAPI-OpenIDConnect-DotNet](https://github.com/AzureADSamples/WebApp-WebAPI-OpenIDConnect-DotNet)
- [WebApp-WebAPI-OAuth2-UserIdentity-DotNet](https://github.com/AzureADSamples/WebApp-WebAPI-OAuth2-UserIdentity-DotNet)
- [Client Applications Samples](https://github.com/AzureADSamples?query=windows)

For more information on ADAL, see [Azure AD Authentication Library for .NET](http://msdn.microsoft.com/en-us/library/azure/jj573266.aspx)




<a name="bk_discoveryContext"> </a>
##Use the Office 365 Discovery Service to determine resource endpoints


Many of the endpoints that your application may need to call vary by user, such as the endpoints for the user's files and folders. To obtain the URLs of the desired Office 365 endpoints, it's best practice for your application to call the Office 365 Discovery Service.  Typically, this will be the first Office 365 service that your solution calls. 


For more details about the Office 365 Discovery Service, see [Use the Discovery Service API to find endpoints for your Office 365 app](..\howto\discover-service-endpoints.md) and [Discovery Service REST API operations reference](..\API\Discovery-Service-REST-operations.md).

<a name="bk_accessOffice365resources"> </a>
##Use endpoints to access Office 365 user data and resources

You can then use the endpoints returned by the Office 365 Discovery Service to actually access your user's Office 365 data. You do so by creating the appropriate client object in your code. The type of client object you create depends on the Office 365 API service you access.

- For calendar, contacts, and email, create an Outlook Services client of the appropriate type:
    - Mail: [Get the Outlook Service Mail client](..\howto\common-mail-tasks-client-library.md#GetClient)
    - Calendar: [Get the Outlook Service Calendar client](..\howto\common-calendar-tasks-client-library.md#GetClient)
    - Contacts: [Get the Outlook Service Contacts client](..\howto\common-contacts-tasks-client-library.md#GetClient)


- For SharePoint Online and OneDrive for Business files, create a [SharePoint client object](..\howto\common-file-tasks-client-library.md#GetClient).

- For user information, create an Azure AD client object.

<a name="bk_nextsteps"> </a>
## Next steps

After you've accessed the Office 365 APIs, you can then work with the user resources available through the APIs. For more information and code snippets showing how to perform common tasks in Visual Studio, see:

- [Common mail tasks using an Office 365 client library](..\howto\common-mail-tasks-client-library.md)
- [Common calendar tasks using an Office 365 client library](..\howto\common-calendar-tasks-client-library.md)
- [Common contacts tasks using an Office 365 client library](..\howto\common-contacts-tasks-client-library.md)
- [Common file tasks using an Office 365 client library](..\howto\common-file-tasks-client-library.md)

<a name="bk_addresources"> </a>
## Additional resources

**Code Samples**

-  [Office 365 API starter projects, code samples, and videos](..\howto\Starter-projects-and-code-samples.md) 
-  [Office Developer on GitHub](https://github.com/OfficeDev)


**Visual Studio development**

-  [Set up your Office 365 development environment](..\howto\setup-development-environment.md)


-  [Getting started with the Office 365 APIs](http://www.microsoftvirtualacademy.com/training-courses/introduction-to-office-365-development?m=10072&ct=31602): training video 


**Reference**

-  [Office 365 REST APIs reference](..\api\API-Catalog.md)

-->




    

