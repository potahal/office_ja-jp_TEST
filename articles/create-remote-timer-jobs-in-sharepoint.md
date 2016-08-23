# SharePoint でリモート タイマー ジョブを作成する

リモートのタイマー ジョブを SharePoint に作成して、環境を管理または制御するバックグラウンド タスクを実行します。

_**適用対象:** SharePoint 2013 | SharePoint Add-ins? | SharePoint Online_

例

- ガバナンス タスクの実行。特定の条件が満たされない場合にサイトにメッセージを表示したり、保持ポリシーを適用したりします。
    
- プロセッサ集約的なスケジュール プロセスの実行。

## リモート タイマー ジョブの作成を開始する前に

まず、 [Core.TimerJobs.Samples ](https://github.com/OfficeDev/PnP/tree/dev/Solutions/Core.TimerJobs.Samples) サンプル アドインを、GitHub の [Office 365 Developer patterns and practices](https://github.com/OfficeDev/PnP/tree/dev) プロジェクトからダウンロードします。

Core.TimerJobs.Samples ソリューションの使用を開始するには、 **Core.TimerJobs.Samples.SimpleJob** のショートカット メニュー (右クリック) を開いて [ **スタートアップ プロジェクトに設定**] を選択することにより、スタートアップ プロジェクト (SimpleJob プロジェクトなど) を選択する必要があります。

**メモ:**  Visual Studio に新しいプロジェクトを作成するとき、新しいリモート タイマー ジョブのビルドを開始するには、**OfficeDevPnP.Core** NuGet パッケージをプロジェクトに追加します。Visual Studio で、**[ツール]**  >  **[NuGet パッケージ マネージャー]**  >  **[ソリューションの Nuget パッケージの管理]** を選択します。

## リモート タイマー ジョブをスケジュールする

タイマー ジョブは、1 回実行するようにスケジュールすることも、定期的なジョブとすることもできます。運用環境でリモート タイマー ジョブをスケジュールするには、コードを exe ファイルにコンパイルして、Windows Task Scheduler または Microsoft Azure WebJob を使用してファイルを実行します。詳しくは、「[タイマー ジョブの展開オプション](https://github.com/OfficeDev/PnP/blob/master/OfficeDevPnP.Core/TimerJob%20Framework.md#timer-job-deployment-options)」を参照してください。

## Core.TimerJobs.Samples.SimpleJob アドインを使用する

Core.TimerJobs.Samples.SimpleJob で、Program.cs の  **Main** は以下の手順を実行します。

1. [OfficeDevPnP.Core.Framework.TimerJobs.TimerJob](https://github.com/OfficeDev/PnP/blob/dev/OfficeDevPnP.Core/OfficeDevPnP.Core/Framework/TimerJobs/TimerJob.cs) 基本クラスから継承する SimpleJob オブジェクトを作成します。
    
2. **TimerJob.UseOffice365Authentication** を使用してタイマー ジョブを実行する際に使用する Office 365 ユーザー資格情報を設定します。ユーザー資格情報には、サイト コレクションに対する適切なアクセス権限がなければなりません。詳しくは、「 [認証](https://github.com/OfficeDev/PnP/blob/master/OfficeDevPnP.Core/TimerJob%20Framework.md#authentication)」を参照してください。
    
3. **TimerJob.AddSite** を使用してタイマー ジョブがタスクを実行する必要のあるサイトを追加します。必要に応じて、**TimerJob.AddSite** ステートメントを繰り返すことにより複数のサイトを追加したり、ワイルドカード文字 * を使用して特定の URL の下にあるすべてのサイトを追加したりすることもできます。たとえば、http://contoso.sharepoint.com/sites/* は、**sites** 管理パスの下にあるすべてのサイトに対してタイマー ジョブを実行します。
    
4. **PrintJobSettingsAndRunJob** を使用してタイマー ジョブの情報を印刷し、タイマー ジョブを実行します。

**メモ:**  この記事で提供されるコードは、明示または黙示のいかなる種類の保証なしに現状のまま提供されるものであり、特定目的への適合性、商品性、権利侵害の不存在についての暗黙的な保証は一切ありません。

```C#
 static void Main(string[] args)
        {
            SimpleJob simpleJob = new SimpleJob();
            
            // The user credentials must have access to the site collections you supply.
            simpleJob.UseOffice365Authentication(User, Password);

            // Use the following code if you are using SharePoint Server 2013 on-premises. 
            //simpleJob.UseNetworkCredentialsAuthentication(User, Password, Domain);
            
            // Add one or more sites that the timer job should work with.
            simpleJob.AddSite("https://contoso.sharepoint.com/sites/dev");
            
            // Prints timer job information and then calls Run().
            PrintJobSettingsAndRunJob(simpleJob);
        }
```

**simpleJob** オブジェクトがインスタンス化されると、**SimpleJob** コンストラクターは以下を行います。

1. TimerJob 基本クラスのコンス トラクターを呼び出します。 
    
2. **SimpleJob_TimerJobRun** イベント ハンドラーが **TimerJobRun** イベントを扱うように割り当てます。 **SimpleJob_TimerJobRun** は、 **TimerJob.Run** が呼び出されると実行します。これについては、この記事の後の部分で詳しく説明します。

```C#
public SimpleJob() : base("SimpleJob")
        {
            TimerJobRun += SimpleJob_TimerJobRun;
        }
```

**PrintJobSettingsAndRunJob** が実行すると、TimerJob からの出力がコンソール ウィンドウに書き込まれて、**TimerJob.Run** が呼び出されます。

```C#
 private static void PrintJobSettingsAndRunJob(TimerJob job)
        {
            Console.ForegroundColor = ConsoleColor.Yellow;
            Console.WriteLine("************************************************");
            Console.WriteLine("Job name: {0}", job.Name);
            Console.WriteLine("Job version: {0}", job.Version);
            Console.WriteLine("Use threading: {0}", job.UseThreading);
            Console.WriteLine("Maximum threads: {0}", job.MaximumThreads);
            Console.WriteLine("Expand sub sites: {0}", job.ExpandSubSites);
            Console.WriteLine("Authentication type: {0}", job.AuthenticationType);
            Console.WriteLine("Manage state: {0}", job.ManageState);
            Console.WriteLine("SharePoint version: {0}", job.SharePointVersion);
            Console.WriteLine("************************************************");
            Console.ForegroundColor = ConsoleColor.Gray;

            // Run job.
            job.Run();
        }
```

**TimerJob.Run** は、**TimerJobRun** イベントを発生させます。**TimerJob.Run** は、**TimerJobRun** イベントを処理するイベント ハンドラーとして **SimpleJob** のコンストラクターに設定した SimpleJob.cs の **SimpleJob_TimerJobRun** を呼び出します。**SimpleJob_TimerJobRun** で、タイマー ジョブの実行時にタイマー ジョブが実行するカスタム コードを追加できます。**SimpleJob_TimerJobRun** は、Program.cs の **TimerJob.AddSite** を使用して追加したサイトで、カスタム コードを実行します。このコード サンプルでは、**SimpleJob_TimerJobRun** が **ClientContext** を **TimerJob** から使用して、サイトのタイトルをコンソール ウィンドウに書き込みます。**TimerJob.AddSite** を使用して複数のサイトが追加された場合、各サイトに対して **SimpleJob_TimerJobRun** が呼び出されます。

```C#
 void SimpleJob_TimerJobRun(object sender, TimerJobRunEventArgs e)
        {
            e.WebClientContext.Load(e.WebClientContext.Web, p => p.Title);
            e.WebClientContext.ExecuteQueryRetry();
            Console.WriteLine("Site {0} has title {1}", e.Url, e.WebClientContext.Web.Title);
        }
```

## 例: コンテンツ タイプの保持を適用するタイマー ジョブ

Core.TimerJobs.Samples.ContentTypeRetentionEnforcementJob プロジェクトは、SharePoint タイマー ジョブを使用してコンテンツ タイプに保持ポリシーを適用する方法を示しています。app.config で  **ContentTypeRetentionPolicyPeriod** 要素を使用して、次のように指定します。

-  **key** 、これは保存期間が適用されるコンテンツ タイプ用のコンテンツ タイプ ID です。
    
-  **value** 、これは保存期間の日数です。保存期間は、 **key** で指定されたコンテンツ タイプを使用して作成されたすべてのリスト項目に適用されます。

```XML
<ContentTypeRetentionPolicyPeriod>
    <!-- Key is the content type ID, and value is the retention period in days -->
    <!-- Specifies an audio content type should be kept for 183 days -->
    <add key="0x0101009148F5A04DDD49cbA7127AADA5FB792B006973ACD696DC4858A76371B2FB2F439A" value="183" />
    <!-- Specifies a document content type should be kept for 365 days -->   
    <add key="0x0101" value="365" />
</ContentTypeRetentionPolicyPeriod>
```

**ContentTypeRetentionEnforcementJob_TimerJobRun** は、 **TimerJobRun** イベントのイベント ハンドラーとして設定されています。Program.cs で **TimerJob.Run** が呼び出されると、 **ContentTypeRetentionEnforcementJob_TimerJobRun** は、Program.cs の **TimerJob.AddSite** を使用して追加された各サイトで以下の手順を実行します。


1. サイト上のすべてのドキュメント ライブラリを取得します。
    
2. サイト上の各ドキュメント ライブラリで、app.config の  **ContentTypeRetentionPolicyPeriod** で指定されたすべての構成情報を読み取ります。app.config から読み取られるコンテンツ タイプ ID と保持期間のペアごとに、 **ApplyRetentionPolicy** が呼び出されます。

```C#
 void ContentTypeRetentionEnforcementJob_TimerJobRun(object sender, TimerJobRunEventArgs e)
        {
            try
            {
                Log.Info("ContentTypeRetentionEnforcementJob", "Scanning web {0}", e.Url);

                // Get all document libraries. Lists are excluded.
                var documentLibraries = GetAllDocumentLibrariesInWeb(e.WebClientContext, e.WebClientContext.Web);

                // Iterate through all document libraries.
                foreach (var documentLibrary in documentLibraries)
                {
                    Log.Info("ContentTypeRetentionEnforcementJob", "Scanning library {0}", documentLibrary.Title);

                    // Iterate through configured content type retention policies specified in app.config.
                    foreach (var contentTypeName in configContentTypeRetentionPolicyPeriods.Keys)
                    {
                        var retentionPeriods = configContentTypeRetentionPolicyPeriods.GetValues(contentTypeName as string);
                        if (retentionPeriods != null)
                        {
                            var retentionPeriod = int.Parse(retentionPeriods[0]);
                            ApplyRetentionPolicy(e.WebClientContext, documentLibrary, contentTypeName, retentionPeriod);
                        }
                    }
                }
            }
            catch(Exception ex)
            {
                Log.Error("ContentTypeRetentionEnforcementJob", "Exception processing site {0}. Exception is {1}", e.Url, ex.Message);
            }
        }
```

**ApplyRetentionPolicy** は、以下の方法でカスタムの保持ポリシー アクションを適用します。

1. **validationDate** を計算します。 **ApplyRetentionPolicy** メソッドは、 **validationDate** より前に変更されたドキュメントに保持ポリシー アクションを適用します。 **validationDate** は CAML 日付として形式設定されて、 **camlDate** に割り当てられます。
    
2. CAML クエリを実行して、app.config で指定されたコンテンツ タイプ ID に基づいて、ドキュメント ライブラリ内のドキュメントをフィルター処理します。[ **変更日時**] の日付が  **camlDate** より小さくなるようにします。
    
3. リスト アイテムごとに、カスタム コードを使用してドキュメントに実行するカスタムの保持アクションを適用します。

```C#
private void ApplyRetentionPolicy(ClientContext clientContext, List documentLibrary, object contentTypeId, int retentionPeriodDays)
        {
            // Calculate validation date. You need to enforce the retention policy on any document modified before validation date.
            var validationDate = DateTime.Now.AddDays(-retentionPeriodDays);
            var camlDate = validationDate.ToString("yyyy-MM-ddTHH:mm:ssZ");

            // Get old documents in the library that match the content type.
            if (documentLibrary.ItemCount > 0)
            {
                var camlQuery = new CamlQuery();
                
                camlQuery.ViewXml = String.Format(
                    @"<View>
                        <Query>
                            <Where><And>
                                <BeginsWith><FieldRef Name='ContentTypeId'/><Value Type='ContentTypeId'>{0}</Value></BeginsWith>
                                <Lt><FieldRef Name='Modified' /><Value Type='DateTime'>{1}</Value></Lt>
                            </And></Where>
                        </Query>
                    </View>", contentTypeId, camlDate);

                var listItems = documentLibrary.GetItems(camlQuery);
                clientContext.Load(listItems,
                    items => items.Include(
                        item => item.Id,
                        item => item.DisplayName,
                        item => item.ContentType));

                clientContext.ExecuteQueryRetry(); 

                foreach (var listItem in listItems)
                {
                    Log.Info("ContentTypeRetentionEnforcementJob", "Document '{0}' has been modified earlier than {1}. Retention policy will be applied.", listItem.DisplayName, validationDate);
                    Console.WriteLine("Document '{0}' has been modified earlier than {1}. Retention policy will be applied.", listItem.DisplayName, validationDate);
                    
                    // Apply your custom retention actions here. For example, archiving documents, or starting a disposition workflow.
                }
            }
        }
```

## 例: ガバナンス タイマー ジョブ

Core.TimerJobs.Samples.GovernanceJob プロジェクトは、タイマー ジョブを使用して 2 人の管理者がサイト コレクションに割り当てられていることを確認し、割り当てられていない場合にはサイトに通知メッセージを表示します。

**SiteGovernanceJob_TimerJobRun** は、 **TimerJobRun** イベントのイベント ハンドラーとして設定されています。Program.cs で **TimerJob.Run** が呼び出されると、 **SiteGovernanceJob_TimerJobRun** は、Program.cs の **TimerJob.AddSite** を使用して追加された各サイト コレクションで以下の手順を実行します。

1. [OfficeDevPnP.Core](https://github.com/OfficeDev/PnP/blob/master/OfficeDevPnP.Core/OfficeDevPnP.Core/AppModelExtensions/SecurityExtensions.cs) の一部である拡張メソッド [GetAdministrators](https://github.com/OfficeDev/PnP/tree/master/OfficeDevPnP.Core) を使用して、サイト コレクションに割り当てられている管理者の数を取得します。
    
2. [OfficeDevPnP.Core](https://github.com/OfficeDev/PnP/blob/master/OfficeDevPnP.Core/OfficeDevPnP.Core/AppModelExtensions/FileFolderExtensions.cs) の一部である [UploadFile](https://github.com/OfficeDev/PnP/tree/master/OfficeDevPnP.Core) を使用して、JavaScript ファイルを SiteAssets または スタイル ライブラリ リストにアップロードします。 
    
3. サイトに設定された管理者が 2 人より少ない場合、 [AddJSLink](https://github.com/OfficeDev/PnP/blob/master/OfficeDevPnP.Core/OfficeDevPnP.Core/AppModelExtensions/JavaScriptExtensions.cs) は JavaScript を使用して通知メッセージをサイトに追加します。詳しくは、「 [JavaScript を使用して SharePoint サイト UI をカスタマイズする](Customize-your-SharePoint-site-UI-by-using-JavaScript.md)」を参照してください。
    
4. サイトに 2 人以上の管理者が設定されている場合、通知メッセージは [DeleteJsLink](https://github.com/OfficeDev/PnP/blob/master/OfficeDevPnP.Core/OfficeDevPnP.Core/AppModelExtensions/JavaScriptExtensions.cs) を使用して削除されます。

```C#
void SiteGovernanceJob_TimerJobRun(object o, TimerJobRunEventArgs e)
        {
            try
            {
                string library = "";

                // Get the number of administrators.
                var admins = e.WebClientContext.Web.GetAdministrators();

                Log.Info("SiteGovernanceJob", "ThreadID = {2} | Site {0} has {1} administrators.", e.Url, admins.Count, Thread.CurrentThread.ManagedThreadId);

                // Get a reference to the list.
                library = "SiteAssets";
                List list = e.WebClientContext.Web.GetListByUrl(library);

                if (!e.GetProperty("ScriptFileVersion").Equals("1.0", StringComparison.InvariantCultureIgnoreCase))
                {
                    if (list == null)
                    {
                        // Get a reference to the list.
                        library = "Style%20Library";
                        list = e.WebClientContext.Web.GetListByUrl(library);
                    }

                    if (list != null)
                    {
                        // Upload js file to list.
                        list.RootFolder.UploadFile("sitegovernance.js", "sitegovernance.js", true);

                        e.SetProperty("ScriptFileVersion", "1.0");
                    }
                }

                if (admins.Count < 2)
                {
                    // Show notification message because you need at least two site collection administrators.
                    e.WebClientContext.Site.AddJsLink(SiteGovernanceJobKey, BuildJavaScriptUrl(e.Url, library));
                    Console.WriteLine("Site {0} marked as incompliant!", e.Url);
                    e.SetProperty("SiteCompliant", "false");
                }
                else
                {
                    // Remove the notification message because two administrators are assigned.
                    e.WebClientContext.Site.DeleteJsLink(SiteGovernanceJobKey);
                    Console.WriteLine("Site {0} is compliant", e.Url);
                    e.SetProperty("SiteCompliant", "true");
                }

                e.CurrentRunSuccessful = true;
                e.DeleteProperty("LastError");
            }
            catch(Exception ex)
            {
                Log.Error("SiteGovernanceJob", "Error while processing site {0}. Error = {1}", e.Url, ex.Message);
                e.CurrentRunSuccessful = false;
                e.SetProperty("LastError", ex.Message);
            }
        }
```

## その他のリソース
<a name="bk_addresources"> </a>

- [Office 365 開発パターンとプラクティス (ソリューション ガイダンス)](Office-365-development-patterns-and-practices-solution-guidance.md)
    
- [タイマー ジョブ フレームワークを使用するためのガイド](https://github.com/OfficeDev/PnP/blob/master/OfficeDevPnP.Core/TimerJob%20Framework.md)
    
- [タイマー ジョブ フレームワーク](https://github.com/OfficeDev/PnP/tree/dev/Solutions/Core.TimerJobs.Samples)
