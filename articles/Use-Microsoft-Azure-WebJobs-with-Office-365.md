# Office 365 での Microsoft Azure WebJobs の使用

Azure WebJobs を使用して、SharePoint Online にアクセスできるタイマー ジョブを実装します。

_**適用対象:** SharePoint 用アドイン | SharePoint 2013 | SharePoint Online_

**提供元:** Tobias Zimmergren

SharePoint Online でタスクを実行するには、[Microsoft Azure WebJobs](http://azure.microsoft.com/documentation/articles/websites-webjobs-resources/) または Windows タスク スケジューラを使用してタイマー ジョブ機能を実装します。タイマー ジョブは、特定のタスクを実行するために SharePoint で稼働する、スケジュールされた反復バックグラウンド プロセスです。たとえば、タイマー ジョブにより、SharePoint リストに入力されたデータをデータベースにコピーできます。SharePoint Online では、過去にはタイマー ジョブの展開方法であったファーム ソリューションは展開できません。SharePoint Online で類似のタイマー ジョブ機能を実装するには、コンソール アプリケーションを Azure WebJobs として実行する必要があります。コンソール アプリケーションは、クライアント側オブジェクト モデル (CSOM) を使用して SharePoint Online にアクセスします。この記事では、コンソール アプリケーションを Azure WebJobs として展開し、実行して SharePoint Online のサイトとコンテンツにアクセスすることに関連する、基本的な概念について説明します。

## コンソール アプリケーションの Azure WebJobs としての作成および実行
<a name="sectionSection0"> </a>

コンソール アプリケーションを Azure WebJobs として実行するようにセットアップするには、次の手順を実行する必要があります。

1. SharePoint のサイトとコンテンツにアクセスするために使用する Azure WebJobs 用の組織アカウントを作成します。
    
2. コンソール アプリケーションを作成してセットアップします。
    
3. コードをコンソール アプリケーションに追加します。
    
4. コンソール アプリケーションを Azure WebJobs として発行します。
    
5. Azure WebJobs を実行して確認します。

## 組織アカウントの作成
<a name="sectionSection1"> </a>

SharePoint のサイトとコンテンツにアクセスするときに使用する、Azure WebJobs 用のアカウントを作成する必要があります。詳細については、「[Office 365 にユーザーを個別に追加する - 管理者向けヘルプ](https://support.office.microsoft.com/article/Add-users-individually-to-Office-365---Admin-Help-1970f7d6-03b5-442f-b385-5880b9c256ec)」を参照してください。Azure WebJobs を実行すると、[**更新者**] フィールドに、組織アカウントの [**表示名**] で入力した値が取り込まれて表示されます。表示名は、SharePoint のアクセスに Azure WebJobs で使用するアカウントであることがユーザーに容易に分かる名前にしてください。

## コンソール アプリケーションの作成およびセットアップ
<a name="sectionSection2"> </a>

Azure WebJobs として実行するコンソール アプリケーションを作成するには、次の手順を実行します。

1. 新しいコンソール アプリケーション プロジェクトを作成します。
    
    1. Visual Studio で、[**新しいプロジェクト**]  >  [**Visual C#**]  >  [**コンソール アプリケーション**]  >  [**OK**] の順に選択します。
    
    2. コンソール アプリケーションを作成してから、[**ツール**]  >  [**NuGet パッケージ マネージャー**]  >  [**ソリューションの NuGet パッケージの管理**]  >  [**オンライン**]  >  [**すべて**] の順に選択します。
    
    3. 「**SharePoint 用アプリ Web ツールキット**」を検索します。
    
    4. [**インストール**] を選択し、[**OK**] を選択します。
    
    5. Choose  **Close**.
    
    6. SharePointContext.cs と TokenHelper.cs が、コンソール アプリケーション プロジェクトに追加されたことを確認します。
    
2. 表示された **appSettings** 要素を追加することで、アカウント情報を app.config 内に保存します。**SPOAccount** と **SPOPassword** を、事前に作成した組織アカウントのパスワードとユーザー名に変更します。
    
    ```XML
    <?xml version="1.0" encoding="utf-8" ?>
     <configuration>
      <startup> 
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
      </startup>
      <appSettings>
        <add key="SPOAccount" value="admin@contoso.onmicrosoft.com"/>
        <add key="SPOPassword" value="Contoso"/>
      </appSettings>
     </configuration>
    ```

    
                **注意**  App.config には、組織のアカウントのユーザー名とパスワードがクリア テキストで格納されます。 このメソッドはデモンストレーション目的でのみ使用されます。Azure WebJobs の運用環境のデプロイでは使用できません。 パスワードは暗号化するか、OAuth でアクセス トークンを使用して認証することをお勧めします。 詳細については、Kirk Evans のブログ記事「[SharePoint アドインをタイマー ジョブとして構築する](http://blogs.msdn.com/b/kaevans/archive/2014/03/02/building-a-sharepoint-app-as-a-timer-job.aspx)」を参照してください。

## コンソール アプリケーションへのコードの追加
<a name="sectionSection3"> </a>

Program.cs で、コンソール アプリケーションに次のコードを追加します。

**メモ**  この記事で提供されるコードは、明示または黙示のいかなる種類の保証なしに現状のまま提供されるものであり、特定目的への適合性、商品性、権利侵害の不存在についての暗黙的な保証は一切ありません。


1. **using** ステートメントを追加します。
    
    ```C#
    using Microsoft.SharePoint.Client;
    using System.Security;
    using System.Configuration; 
    ```

2. 次のメソッドをクラスに追加します。
    
    -  **Main** は、SharePoint のサイトにサインインし、CSOM を使用してサイトやコンテンツに対するタスクを実行します。このコード サンプルでは、リストを検索したり、リストにあるアイテムの合計数をコンソール ウィンドウに出力したりするために、CSOM を使用しています。Azure WebJobs を使用している場合、コンソール ウィンドウの出力は WebJobs の実行の詳細に表示されます。これについては、「[Azure WebJobs の実行および確認](Use-Microsoft-Azure-WebJobs-with-Office-365.md#runandverify)」で説明しています。
    
  -  **GetSPOSecureStringPassword** は、パスワードを app.config から読み取ります。
    
  -  **GetSPOAccountName** は、ユーザー名を app.config ファイルから読み取ります。

    ```C#
    static void Main(string[] args)
        {
            using (ClientContext context = new ClientContext("https://contoso.sharepoint.com"))
            {
                // Use default authentication mode.
                context.AuthenticationMode = ClientAuthenticationMode.Default;                 
                context.Credentials = new SharePointOnlineCredentials(GetSPOAccountName(), GetSPOSecureStringPassword());
    
                // Add your CSOM code to perform tasks on your sites and content.
    
                try
                {
                    List objList = context.Web.Lists.GetByTitle("Docs");
                    context.Load(objList);
                    context.ExecuteQuery();
    
                    if (objList != null &amp;&amp; objList.ItemCount > 0)
                    {
                        Console.WriteLine(objList.Title.ToString() + " has " + objList.ItemCount + " items.");
                    }
    
                }
                catch (Exception ex)
                {
                    Console.WriteLine("ERROR: " + ex.Message);
                    Console.WriteLine("ERROR: " + ex.Source);
                    Console.WriteLine("ERROR: " + ex.StackTrace);
                    Console.WriteLine("ERROR: " + ex.InnerException);
    
                }
            }
                
        }
    
    private static SecureString GetSPOSecureStringPassword()
    {
      try
      {
          Console.WriteLine("Entered GetSPOSecureStringPassword.");
          var secureString = new SecureString();
          foreach (char c in ConfigurationManager.AppSettings["SPOPassword"])
          {
              secureString.AppendChar(c);
          }
          Console.WriteLine("Constructed the secure password.");
    
          return secureString;
      }
      catch
      {
          throw;
      }
    }
    
    private static string GetSPOAccountName()
    {
      try
      {
          Console.WriteLine("Entered GetSPOAccountName.");
          return ConfigurationManager.AppSettings["SPOAccount"];
      }
      catch
      {
          throw;
      }
    }
    
    ```

## コンソール アプリケーションの Azure WebJobs としての発行
<a name="sectionSection4"> </a>

コンソール アプリケーションの開発が完了したら、コンソール アプリケーションを Azure WebJobs として展開する必要があります。コンソール アプリケーションを Azure WebJobs として展開するには、次の手順を実行できます。

- バイナリを Azure Web App にアップロードし、[Microsoft Azure ポータル](https://portal.azure.com/)を使用して Azure WebJobs を作成します。Visual Studio プロジェクトのバイナリは、Visual Studio プロジェクトの bin/Debug または bin/Release フォルダーにあります。Azure Web App を作成してバイナリをアップロードするには、「[Visual Studio を使用した Azure App Service への ASP.NET Web アプリのデプロイ](http://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)」に記載されている手順に従います。オンデマンド、継続的、またはスケジュールされた Azure WebJobs を作成するには、「[WebJobs でバックグラウンド タスクを実行する](http://azure.microsoft.com/documentation/articles/web-sites-create-web-jobs/)」を参照してください。
    
- Visual Studio から Azure WebJobs を発行します。詳細については、「[Web プロジェクトなしで WebJobs デプロイメントを有効にする](http://azure.microsoft.com/documentation/articles/websites-dotnet-deploy-webjobs/#convertnolink)」を参照してください。
    
## Azure WebJobs の実行および確認
<a name="runandverify"> </a>

上記のすべての手順を完了したら、Azure WebJobs は稼働し、Office 365 サブスクリプションのタスクを実行することになります。場合によっては、Azure WebJobs の保守やトラブルシューティングの実行が必要になることがあります。Azure WebJobs が実行していることを確認するには、次のようにします。

- Azure WebJob がリスト アイテムなどの SharePoint アイテムを更新した場合、[**変更者**] フィールドには、Azure WebJobs が SharePoint へのアクセスに使用した組織アカウントが表示されます。
    
- WebJobs 詳細ログから Azure WebJobs を確認します。WebJobs 詳細ログは、ジョブが実行された時刻、ジョブ実行の成功または失敗、Webjobs からの出力 (Console.WriteLine が呼び出されたなどの場合)、およびジョブ実行の他の詳細を確認できます。詳細については、「[Web ジョブでバックグラウンド タスクを実行する](http://azure.microsoft.com/documentation/articles/web-sites-create-web-jobs/)」の「ジョブ履歴の表示」のセクションを参照してください。

## その他のリソース
<a name="bk_addresources"> </a>

-  Office 365 開発パターンとプラクティス (ソリューション ガイダンス)
    
-  
            [Azure App サービスに .NET Web ジョブを作成する](http://azure.microsoft.com/documentation/articles/websites-dotnet-webjobs-sdk-get-started/)
    
-  
            [Azure WebJobs のドキュメント リソース](http://azure.microsoft.com/documentation/articles/websites-webjobs-resources/)
