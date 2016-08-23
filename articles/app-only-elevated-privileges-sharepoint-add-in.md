SharePoint アドイン モデルでのアプリ専用の権限と昇格した権限
===============================================================

概要
-------

新しい SharePoint アドイン モデルにおいて、コード内で権限の昇格を実行するために使用するアプローチは、完全信頼コードの場合とは異なります。一般的な完全信頼コード (FTC) / ファーム ソリューション シナリオでは、RunWithElevatedPrivileges API を SharePoint サーバー側オブジェクト モデル コードと共に使用し、ファーム ソリューションによって展開します。

SharePoint アドイン モデル シナリオでは、実行が承認されていない操作の実行を現在のユーザーに許可するため、AllowAppOnlyPolicy アクセス許可またはサービス アカウントを使用します。

基本ガイドライン
---------------------

コードでの権限の昇格については、大まかに次のような基本ガイドラインが提供されています。

- AllowAppOnlyPolicy は次のものでは機能しません 
    + 検索 - ターゲットが SharePoint オンプレミスの場合。 これについて、SharePoint Online のサポートが追加されています ([ブログの投稿](https://blogs.msdn.microsoft.com/vesku/2016/03/07/using-add-in-only-app-only-permissions-with-search-queries-in-sharepoint-online/))
    + ユーザー プロファイル CSOM 操作
    + 分類サービス エントリ (書き込み) の更新 - 読み取りは機能します **Note:** In these scenarios you need to use a specific service account.
- AllowAppOnlyPolicy は RunWithElevatedPrivileges と似ていますが、まったく同じではありません。
    + AllowAppOnlyPolicy では、操作を実行するための適切なアクセス許可を持っている別のユーザーの代わりにではなく、SharePoint アドインに付与されたアクセス許可に基づいてコードが実行されます。

App Only Policy トークンを返してコンテキスト オブジェクトの作成に使用する例を以下に示します。

    Uri siteUrl = new Uri(ConfigurationManager.AppSettings["MySiteUrl"]);
    try
    {
        //Connect to the give site using App Only token
        string realm = TokenHelper.GetRealmFromTargetUrl(siteUrl);
        var token = TokenHelper.GetAppOnlyAccessToken(TokenHelper.SharePointPrincipal, siteUrl.Authority, realm).AccessToken;

        using (var ctx = TokenHelper.GetClientContextWithAccessToken(siteUrl.ToString(), token))
        {
            // Perform operations using the app only token access. 
        }
    }
    catch (Exception ex)
    {
        Console.WriteLine("Error in execution: " + ex.Message);
    }

- AllowAppOnlyPolicy を使用するときは、これがプロバイダー ホスト型 SharePoint アドインでのみ機能することに留意してください。
- AllowAppOnlyPolicy は、ユーザーの代わりにコードが実行されることがないため、すべてのシナリオで適切とは限りません。
- サービス アカウントは、SharePoint で定義されます。
    + Office 365 テナンシーでは、コードの要件に含まれる機能によっては、サービス アカウントに Office 365 ライセンスが割り当てられていなければならない可能性があります。
    + SharePoint アドインごとにサービス アカウントを作成するか、すべての SharePoint アドイン用に単一のアカウントを作成することができます。
    + 実行する操作を簡単に追跡できるように、サービス アカウントの名前をわかりやすいものにします。
    
    次に例を示します。SharePoint アドインでリスト アイテムが変更されると、リスト アイテムの [更新者] 列に、SharePoint アドインに関連付けられているサービス アカウントの名前が表示されます。

- サービス アカウントによって認証するときは、サービス アカウントのユーザー名とサービス アカウントを取得する必要があります。
    + 下のコード スニペットは、ユーザー名とパスワードを使用して認証する例を示しています。
    + ユーザー名とパスワードの保存と取得は安全な方法で行うよう注意してください。

    ```
    using (ClientContext context = new ClientContext("https://tenancy.sharepoint.com"))
    {
    
        // Use default authentication mode
        context.AuthenticationMode = ClientAuthenticationMode.Default;  
        // Specify the credentials for the account that will execute the request
        context.Credentials = new SharePointOnlineCredentials("User Name", "Password");
    }
    ```

アクセス許可を昇格するためのオプション
------------------------------

アクセス許可を昇格するためのオプションは複数あります。

- OAuth (AllowAppOnlyPolicy)
    + S2S (サブオプション)
    + ACS (サブオプション)
- サービス アカウント
    + リモートでホストされているコード (例: Azure Web ジョブ)

OAuth (AllowAppOnlyPolicy)
--------------------------
このオプションでは、AppPermissionRequests 要素で AllowAppOnlyPolicy が true に設定され、SharePoint アドイン マニフェストでアクセス許可が設定されます。OAuth は、実行用のアクセス許可を得ている操作の実行を許可するアクセス トークンを SharePoint アドインに返すために使用されます。

**S2S サブオプション**

S2S サブオプションは、オンプレミス SharePoint 環境でのみ機能します。

S2S のシナリオで OAuth によって認証を行う場合、SharePoint アドイン用のアクセス トークンを返すために **TokenHelper::GetS2SAccessTokenWithWindowsIdentity** メソッドが使用されます。このアクセス トークンにより、SharePoint アドインは SharePoint アドイン マニフェストで付与された任意の操作を実行できます。

このオプションは、ユーザーの代わりにコードが実行されることがないため、すべてのシナリオで適切とは限りません。

**適切な場合**

このオプションは SharePoint S2S で機能し、非常に簡単に実装できるため、S2S シナリオで権限を昇格する必要がある場合に適したオプションです。

**はじめに**

次の記事は、S2S で AllowAppOnlyPolicy を使用する例を示しています。

- [SharePoint 2013 でアプリ専用ポリシーが簡単に (Kirk Evans - MSDN ブログ投稿)](http://blogs.msdn.com/b/kaevans/archive/2013/02/23/sharepoint-2013-app-only-policy-made-easy.aspx)

**ACS サブオプション**

ACS サブオプションは、オンプレミスと Office 365 の SharePoint 環境で機能します。

ACS のシナリオで OAuth によって認証を行う場合、SharePoint アドイン用のアクセス トークンを返すために **TokenHelper::GetS2SAccessTokenWithWindowsIdentity** が使用されます。その後、 **TokenHelper::GetClientContextWithAccessToken** メソッドが呼び出され、SharePoint アドイン マニフェストで付与された権限に基づいて SharePoint アドインが実行アクセス権限を持つ任意の操作を実行するために必要なクライアント コンテキストが返されます。

このオプションは、ユーザーの代わりにコードが実行されることがないため、すべてのシナリオで適切とは限りません。

**適切な場合**

このオプションは SharePoint ACS で動作し、非常に簡単に実装できるため、ACS シナリオで権限を昇格する必要がある場合に適したオプションです。このオプションは、ACS との信頼関係を確立したオンプレミス SharePoint 環境がある場合に適しています。これは、Office 365 SharePoint 環境がある場合の唯一の OAuth オプションです。

**はじめに**

次の記事は、ACS で AllowAppOnlyPolicy を使用する例を示しています。

- [SharePoint 2013 でアプリ専用ポリシーが簡単に (Kirk Evans - MSDN ブログ投稿)](http://blogs.msdn.com/b/kaevans/archive/2013/02/23/sharepoint-2013-app-only-policy-made-easy.aspx)

サービス アカウント
---------------
このパターンでは、コードを実行するユーザーのコンテキストを確立するために SharePointOnlineCredentials クラスが使用されます。

**適切な場合**

これは、ユーザー (サービス アカウント) と SharePoint アドインのアクセス許可でアクションが実行されるため、特定のユーザー (サービス アカウント) の代わりにコードを実行する必要がある場合に適したオプションです。

**はじめに**

次の記事は、コードを実行するユーザーのコンテキストを確立するために SharePointOnlineCredentials クラスを使用する例を示しています。

- [Office 365 サイト用の Azure Web ジョブ ("タイマー ジョブ") の構築を開始する (「認証に関する考慮事項」のセクション) - Tobias Zimmergren のブログ投稿](http://zimmergren.net/technical/getting-started-with-building-azure-webjobs-timer-jobs-for-your-office-365-sites)

関連リンク
=============
- [SharePoint 2013 でアプリ専用ポリシーが簡単に (Kirk Evans - MSDN ブログ投稿)](http://blogs.msdn.com/b/kaevans/archive/2013/02/23/sharepoint-2013-app-only-policy-made-easy.aspx)
- [Office 365 サイト用の Azure Web ジョブ ("タイマー ジョブ") の構築を開始する (「認証に関する考慮事項」のセクション) - Tobias Zimmergren のブログ投稿](http://zimmergren.net/technical/getting-started-with-building-azure-webjobs-timer-jobs-for-your-office-365-sites)
- [SharePointOnlineCredentials クラス (MSDN API ドキュメント)](https://msdn.microsoft.com/en-us/library/microsoft.sharepoint.client.sharepointonlinecredentials.aspx)
- [SharePoint Online での検索クエリにアドインのみ/アプリのみのアクセス許可を使用する - Vesa Juvonen ブログ記事](https://blogs.msdn.microsoft.com/vesku/2016/03/07/using-add-in-only-app-only-permissions-with-search-queries-in-sharepoint-online/)
- ガイダンス記事: [http://aka.ms/OfficeDevPnPGuidance](http://aka.ms/OfficeDevPnPGuidance "ガイダンス記事")
- MSDN リファレンス: [http://aka.ms/OfficeDevPnPMSDN](http://aka.ms/OfficeDevPnPMSDN "MSDN リファレンス")
- ビデオ: [http://aka.ms/OfficeDevPnPVideos](http://aka.ms/OfficeDevPnPビデオ"Videos")

関連する PnP サンプル
===================

- [Core.SimpleTimerJob (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.SimpleTimerJob)
- https://github.com/OfficeDev/PnP にあるサンプルとコンテンツ

適用対象
==========
- Office 365 マルチテナント (MT)
- Office 365 専用 (D) *一部*
- SharePoint 2013 オンプレミス - *一部*

*専用およびオンプレミスの場合のパターンは SharePoint アドイン モデル手法と同じですが、使用可能なテクノロジは異なる可能性があります。*
