SharePoint アドイン モデル内のリモート タイマー ジョブ
================================================

概要
-------

新しい SharePoint アドイン モデルにおいて、リモート タイマー ジョブを実装する方法は、完全信頼コードの場合とは異なります。一般的な完全信頼コード (FTC)/ ファーム ソリューション シナリオでは、SharePoint タイマー ジョブは SharePoint サーバー側オブジェクト モデル コードを使用して作成、ファーム ソリューションで展開、および SharePoint サーバーの全体管理 Web サイトで管理されます。このシナリオでは、SharePoint は、スケジュール設定とタイマー ジョブの実行の両方を処理します。 

SharePoint アドイン モデル シナリオでは、タイマー ジョブは SharePoint の外部で作成され、スケジュール設定されます。このシナリオでは、SharePoint はスケジュールの設定やタイマー ジョブの実行を行いません。

基本ガイドライン
---------------------

タイマー ジョブの作成については、大まかに次のような基本ガイドラインが提供されています。

- タイマー ジョブは、SharePoint の外部で実装する必要があります。
- タイマー ジョブのスケジュール設定は、SharePoint の外部で実装する必要があります。
- タイマー ジョブは、サービス アカウントまたは OAuth を使用して認証する必要があります。

タイマー ジョブの作成に関する課題
------------------------------

Office 365 テナンシーでのタイマー ジョブを作成するときの最大の課題は、ファーム スコープ ソリューションを Office 365 テナントに展開できないということです。ファーム スコープ ソリューションがなければ、SharePoint タイマー ジョブを展開することはできません。

タイマー ジョブを作成する新しい方法は、構築とスケジュール設定を SharePoint の外部で行う方法です。オンプレミスの SharePoint 環境では、SharePoint タイマー ジョブに関連付けられている次の要因を考慮してください。

- SharePoint には数百のタイマー ジョブが標準で付属しています。これらは SharePoint のスケジューラが常に対応する必要のあるものです。  
実装コードを Azure や別の環境に移すことで、SharePoint サーバーで必要なメモリの消費量やプロセッサーの使用電力を削減できます。
- タイマー ジョブと関連付けられたスケジュール設定や実装を別のサーバーに移すことで、SharePoint サーバーの拡張性が向上し、結果として安定します。

タイマー ジョブのスケジュールのオプション
----------------------------

タイマー ジョブのスケジュール設定を実装するには、いくつかのオプションがあります。

- Windows スケジューラー
    + Windows サービス
- Azure WebJob
    + Azure ワーカー プロセス

Windows スケジューラー
-----------------
このパターンでは、Windows スケジューラーは、タイマー ジョブに関連付けられたスケジュールの側面を処理します。実装コードには、コンソール アプリケーション、PowerShell スクリプト、または Windows スケジューラーから呼び出すことができるその他のコードを使用できます。

**Windows サービスのサブ オプション** Windows サービスは、Windows スケジューラーと同じ特性を持ちます。マイクロソフトでは、このパターンを推奨していませんが、一般的に使用されているので取り上げる価値があります。

**Windows スケジューラー環境に適した場合**

Azure WebJobs でタイマー ジョブをスケジュールするために、Azure サブスクリプションにアクセスできない場合は、Windows オペレーティング システムのどのコンピューターでも実装できる Windows スケジューラーの使用が適しています。

- Windows スケジューラーを実行するハードウェアを追加する必要があります。
- タイマー ジョブを実行するハードウェアを追加する必要があります。

**はじめに**

次の記事では、Windows スケジューラー パターンを使用し、開始するためのコード サンプルを提供します。

- [Core.SimpleTimerJob (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.SimpleTimerJob)
    + このパターンに関するエンドツーエンドの記事に、ビデオが付属しています。
- [Core.TimerJobs.Samples (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Solutions/Core.TimerJobs.Samples)
    + 10 個の異なる例を含む優れたコード サンプル 。*注: 10 のコード サンプルすべてが Windows スケジューラー パターンに該当するわけではありません。*

Azure WebJob
------------
このパターンでは、Azure WebJob は、タイマー ジョブに関連付けられているスケジュールの側面を処理し、実装コードを含みます。

- Azure WebJob (スケジュール設定と実装コード) を実行すためハードウェアを追加する必要はありません。
- スケジュール設定や実装コードに Azure WebJob を使用しており、1 つの場所で管理しやすいため、有利です。

**Azure のワーカー ロールのサブ オプション** Azure のワーカー ロールの特性は、Azure WebJob と同じです。マイクロソフトでは、このパターンを推奨していませんが、一般的に使用されているので取り上げる価値があります。

**Azure WebJob 環境に適した場合**

Azure WebJobs でタイマー ジョブをスケジュールするための、Azure サブスクリプションへのアクセス権を持っている場合。

**はじめに**

次の記事では、Azure WebJob パターンについて説明し、開始するためのコード サンプルを提供します。

- [Office 365 サイト (O365 PnP 記事) のために Azure WebJobs (「タイマー ジョブ」) での作業を開始する](https://github.com/OfficeDev/PnP-Guidance/blob/master/articles/Getting-Started-with-building-Azure-WebJobs-for-your-Office365-sites.md)
    + Office 365 またはオンプレミスの SharePoint 環境のためにスケジュールされたジョブとして機能するよう、Azure WebJob を構築する方法。発行と監視に関する情報が含まれています。
- [Core.SimpleTimerJob (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Solutions/Core.TimerJobs.Samples)
    + 10 個の異なる例を含む優れたコード サンプル 。*注: 10 のコード サンプルすべてが Azure WebJob パターンに該当するわけではありません。*

認証オプション
----------------------

タイマー ジョブが SharePoint とやり取りするためには、認証する必要があります。現時点では、タイマー ジョブを認証に使用するパターンは 2 つあります。

- サービス アカウントの使用
- OAuth を使用する

サービス アカウントの使用
---------------------
このパターンでは、タイマー ジョブの認証に使用する 1 つ以上のサービス アカウントを定義します。

- サービス アカウントは、SharePoint で定義されます。
    + Office 365 テナンシーでは、タイマー ジョブに含まれる機能によっては、サービス アカウントに Office 365 ライセンスが割り当てられていなければならない場合があります。
    + タイマー ジョブごとにサービス アカウントを作成するか、すべてのタイマー ジョブ用に単一のアカウントを作成することができます。
    + 実行する操作を簡単に追跡できるように、サービス アカウントの名前をわかりやすいものにします。
    
    次に例を示します。タイマー ジョブでリスト アイテムが変更されると、リスト アイテムの [更新者] 列に、タイマー ジョブに関連付けられているサービス アカウントの名前が表示されます。

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

**はじめに**

次の記事では、サービス アカウント認証パターンの使用方法について説明し、開始するためのコード サンプルを提供します。

- [タイマー ジョブとして SharePoint App を構築する (MSDN ブログ)](http://blogs.msdn.com/b/kaevans/archive/2014/03/02/building-a-sharepoint-app-as-a-timer-job.aspx)
    + このパターンに関するエンドツーエンドの記事。
- [Core.SimpleTimerJob (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.SimpleTimerJob)
    + このパターンに関するエンドツーエンドの記事に、ビデオが付属しています。
- [Core.TimerJobs.Samples (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Solutions/Core.TimerJobs.Samples)
    + 10 個の異なる例を含む優れたコード サンプル 。*注: 10 のコード サンプルすべてがサービス アカウント認証パターンに該当するわけではありません。*

OAuth を使用する
-----------
このパターンでは、SharePoint または Azure Active Directory でアプリケーションを定義し、アプリケーションに関連付けられている認証トークンを使用して認証します。

- SharePoint アプリケーションを使用して認証するときは、アプリケーション プリンシパルを作成し、アクセス許可を割り当てます。
    + このパターンでは、プロバイダー ホスト型 SharePoint アドインまたはコンソール アプリケーションを使用してタイマー ジョブを実装することがあります。
    + プロバイダーホスト型 SharePoint アドインまたはコンソール アプリケーションにアプリケーション プリンシパルを登録するには、SharePoint の AppRegNew ページを使用します。
    
    このページには、次の URL でアクセスします: http://<tenancy>/<site>/_layouts/AppRegNew.aspx

    + アプリケーション プリンシパルにアクセス許可を付与するには、SharePoint の AppInv ページを使用します。
    
    このページには、次の URL でアクセスします: http://<tenancy>/<site>/_layouts/AppInv.aspx

- タイマー ジョブは、対話型ユーザーが関連付けられていない、App Only のアクセス許可を使用します。 
    + 次のコード スニペットは、アクセス トークンを取得し、App Only アクセス許可を使用して SharePoint で認証を行う方法を示しています。

    ```
    string accessToken = TokenHelper.GetAppOnlyAccessToken(TokenHelper.SharePointPrincipal, siteUri.Authority, realm).AccessToken;
    
    using(var clientContext = TokenHelper.GetClientContextWithAccessToken(siteUri.ToString(),accessToken))
    {
        //Implement timer job code
    }
    ```

- Azure Active Directory アプリケーションを使用して認証する場合、Azure 管理ポータルで Azure Active Directory アプリケーションを作成し、アクセス許可を割り当てることができます。
    + このパターンでは、プロバイダー ホスト型 SharePoint アドインまたはコンソール アプリケーションを使用してタイマー ジョブを実装することがあります。
    + このパターンでは、アクセス トークンを取得するため、Active Directory 認証ライブラリまたは Microsoft Graph API とやり取りを行います。
    + アクセス トークンは SharePoint への認証 (および場合によって、Office 365 テナンシーの他の Office 365 サービス) に使用されます。

**はじめに**

次の記事では、OAUth 認証パターンについて説明し、開始するためのコード サンプルを提供します。

- [タイマー ジョブとして SharePoint App を構築する (MSDN ブログ)](http://blogs.msdn.com/b/kaevans/archive/2014/03/02/building-a-sharepoint-app-as-a-timer-job.aspx)
    + このパターンに関するエンドツーエンドの記事。
- [Core.TimerJobs.Samples (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Solutions/Core.TimerJobs.Samples)
    + 10 個の異なる例を含む優れたコード サンプル 。*注: 10 のコード サンプルすべてが OAUth 認証パターンに該当するわけではありません。*

関連リンク
=============
- [Azure WebJob リソース (Azure ドキュメント)](http://azure.microsoft.com/en-us/documentation/articles/websites-webjobs-resources/)
- [Visual Studio を使用して Webjobs を展開する (Azure ドキュメント)](http://azure.microsoft.com/en-us/documentation/articles/websites-dotnet-deploy-webjobs/)
- ガイダンス記事: [http://aka.ms/OfficeDevPnPGuidance](http://aka.ms/OfficeDevPnPGuidance "ガイダンス記事")
- MSDN リファレンス: [http://aka.ms/OfficeDevPnPMSDN](http://aka.ms/OfficeDevPnPMSDN "MSDN リファレンス")
- ビデオ: [http://aka.ms/OfficeDevPnPVideos](http://aka.ms/OfficeDevPnPビデオ"Videos")

関連する PnP サンプル
===================

- [Core.SimpleTimerJob (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.SimpleTimerJob)
- [Core.TimerJobs.Samples (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Solutions/Core.TimerJobs.Samples)
- [Provisioning.Services.SiteManager (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Provisioning.Services.SiteManager)
- [Provisioning.SiteCollectionCreation (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Provisioning.SiteCollectionCreation)
- https://github.com/OfficeDev/PnP にあるサンプルとコンテンツ

適用対象
==========
- Office 365 マルチテナント (MT)
- Office 365 専用 (D) *一部*
- SharePoint 2013 オンプレミス - *一部*

*専用およびオンプレミスの場合のパターンは SharePoint アドイン モデル手法と同じですが、使用可能なテクノロジは異なる可能性があります。*
