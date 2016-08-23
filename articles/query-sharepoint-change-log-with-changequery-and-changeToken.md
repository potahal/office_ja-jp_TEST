# ChangeQuery と ChangeToken で SharePoint の変更ログを照会する

**ChangeQuery** と **ChangeToken** を使用すると、SharePoint 変更ログを照会し、SharePoint コンテンツ データベース、サイト コレクション、サイト、またはリストに加えられた変更内容を取得できます。

_**適用対象:** SharePoint 2013? | SharePoint Add-ins? | SharePoint Online_

[ChangeQuery](https://msdn.microsoft.com/library/office/microsoft.sharepoint.client.changequery.aspx) と [ChangeToken](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.changetoken.aspx) を使用して SharePoint 変更ログを照会すると、SharePoint コンテンツ データベース、サイト コレクション、サイト、またはリストに加えられた変更内容を検索して、処理できます。

[Core.ListItemChangeMonitor](https://github.com/OfficeDev/PnP/tree/dev/Samples/Core.ListItemChangeMonitor) コード サンプルでは、SharePoint リストに加えられた変更を SharePoint の変更ログを使用して検索および処理する方法を紹介します。このコード サンプルは、次の操作を実行するために使用してください。

- SharePoint をモニターして、リスト、サイト、サイト コレクションまたはコンテンツ データベースに対する変更を検出します。
    
- リスト内のアイテムに変更が加えられた後、ビジネス プロセスを開始します。
    
- リモート イベント レシーバーを補完します。変更ログ パターンをリモート イベント レシーバー パターンと一緒に使用すると、SharePoint コンテンツ データベース、サイト コレクション、サイト、またリストに加えられたすべての変更を処理する、さらに信頼性の高いアーキテクチャが提供されます。リモート イベント レシーバーは即座に実行されますが、リモート サーバー上で実行されているため、通信エラーが発生する可能性があります。変更ログ パターンによりすべての変更が処理可能な状態に保持されますが、アプリケーションによる変更の処理は、通常、スケジュールに従って (たとえば、タイマー ジョブとして) 実行されます。これは、変更が即座には処理されないことを意味します。これら 2 つのパターンを一緒に使用すれば、同じ変更を 2 回処理する可能性を確実に回避するメカニズムを使用できます。詳細については、「 [SharePoint でリモート イベント レシーバーを使用する](Use-remote-event-receivers-in-SharePoint.md)」を参照してください。
    
## 始める前に

まず、 [Core.ListItemChangeMonitor](https://github.com/OfficeDev/PnP/tree/dev/Samples/Core.ListItemChangeMonitor) サンプル アドインを、GitHub の [Office 365 Developer Patterns and Practices](https://github.com/OfficeDev/PnP/tree/dev) プロジェクトからダウンロードします。

このコード サンプルを実行する前に、次の手順を実行します。

1. リストを作成する場所となる Office 365 サイトにサインインします。
    
2. **[サイト コンテンツ]** を選択します。
    
3. **[アドインの追加]** を選択します。
    
4. **[カスタム リスト]** を選択します。
    
5. **[名前]** に、「 **TestList** 」と入力します。
    
6. **[作成]** を選択します。
    
このコード サンプルのデモを表示するには、次の手順を実行します。

1. Visual Studio で **[開始]** を選択します。
    
2. Office 365 サイトの URL、リストの名前 (**TestList**)、および Office 365 の資格情報を入力します。コンソール アプリケーションは、**TestList** に対して変更が実行されるのを待機するようになります。既定では、コンソール アプリケーションは変更ログを 30 秒ごとに確認し、表示を更新します。
    
3. **[TestList]** に新しいアイテムを追加します。
    
    1. Office 365 サイトを開き、 **[サイト コンテンツ]** > **[TestList]** に移動します。
    
    2. **[新しいアイテム]** を選択します。
    
    3. **[タイトル]** に「 **MyTitle** 」と入力してから、 **[保存]** を選択します。
    
4. 加えた変更がコンソール アプリケーションに表示されることを確認します。SharePoint の変更ログを強制的にコンソール アプリケーションに読み取らせるには、コンソール アプリケーションに「 **r** 」と入力します。

## Core.ListItemChangeMonitor アドインを使用する

Program.cs では  **Main** が **DoWork** を呼び出し、SharePoint の変更ログを読み取って処理します。

1. SharePoint の変更ログにアクセスするための  **ChangeQuery** オブジェクトを作成します。
    
2. 変更ログを使用し、 **cq.Item = true** を使用することにより変更をアイテムに返します。変更には次のものがあります。
    
    - **cq.Add = true** を使用して追加された新しいアイテム。
    
    - **cq.DeleteObject = true** を使用して削除されたアイテム。
    
    - **cq.Update = true** を使用して変更されたアイテム。
    
3. **ChangeToken** オブジェクトを作成し、特定の時点から変更ログの変更内容を読み取ります。
    
4. [ChangeToken.StringValue](https://msdn.microsoft.com/library/office/microsoft.sharepoint.client.changetoken.stringvalue.aspx) に、バージョン番号、変更スコープ、**TestList** の GUID、変更が発生した日時、および ChangeToken の変更アイテム値 (値 -1 で初期化する) を設定します。 このコード サンプルでは、変更ログから読み取る変更内容の量を制限するために、**DateTime.Now.AddDays(-2).ToUniversalTime().Ticks.ToString()** を使用して、変更の発生した日時を直前の 2 日間に初期化しています。
    
5.  変更ログを、30 秒 (定数 **WaitSeconds** で設定する既定の待機期間) ごとに、またはユーザーが **r** を入力した時点で変更ログを読み取ります。変更ログを読み取るとき、コンソール アプリケーションは次の手順を実行します。
    
    1.  [List.GetChanges](https://msdn.microsoft.com/library/office/microsoft.sharepoint.client.list.getchanges.aspx) を使用して [ChangeCollection](https://msdn.microsoft.com/library/office/microsoft.sharepoint.client.changecollection.aspx) を返します。これは、最後に変更が処理されてから後にリストに加えられた変更のコレクションです。
    
    2. **DisplayChanges** を呼び出して、**ChangeCollection** オブジェクトに含まれる変更内容を表示します。
    
    3. 変更ログから変更を読み取る新しい時点を設定します。リストに変更があった場合 (**coll** に入れて返される) は、**ChangeTokenStart** を最後の変更の日時に設定します。

**メモ:**  この記事で提供されるコードは、明示または黙示のいかなる種類の保証なしに現状のまま提供されるものであり、特定目的への適合性、商品性、権利侵害の不存在についての暗黙的な保証は一切ありません。

```C#
private static void DoWork()
        {
            Console.WriteLine();
            Console.WriteLine("Url: " + url);
            Console.WriteLine("User name: " + userName);
            Console.WriteLine("List name: " + listName);
            Console.WriteLine();
            try
            {

                Console.WriteLine(string.Format("Connecting to {0}", url));
                Console.WriteLine();
                ClientContext cc = new ClientContext(url);
                cc.AuthenticationMode = ClientAuthenticationMode.Default;
                cc.Credentials = new SharePointOnlineCredentials(userName, password);

                ListCollection lists = cc.Web.Lists;
                IEnumerable<List> results = cc.LoadQuery<List>(lists.Where(lst => lst.Title == listName));
                cc.ExecuteQuery();
                List list = results.FirstOrDefault();
                if (list == null)
                {

                    Console.WriteLine("A list named \"{0}\" does not exist. Press any key to exit...", listName);
                    Console.ReadKey();
                    return;
                }

                nextRunTime = DateTime.Now;

                ChangeQuery cq = new ChangeQuery(false, false);
                cq.Item = true;
                cq.DeleteObject = true;
                cq.Add = true;
                cq.Update = true;

                // Set the ChangeTokenStart to two days ago to reduce how much data is returned from the change log. Depending on your requirements, you might want to change this value. 
                // The value of the string assigned to ChangeTokenStart.StringValue is semicolon delimited, and takes the following parameters in the order listed:
                // Version number. 
                // The change scope (0 - Content Database, 1 - site collection, 2 - site, 3 - list).
                // GUID of the item the scope applies to (for example, GUID of the list). 
                // Time (in UTC) from when changes occurred.
                // Initialize the change item on the ChangeToken using a default value of -1.

                cq.ChangeTokenStart = new ChangeToken();
                cq.ChangeTokenStart.StringValue = string.Format("1;3;{0};{1};-1", list.Id.ToString(), DateTime.Now.AddDays(-2).ToUniversalTime().Ticks.ToString());

                Console.ForegroundColor = ConsoleColor.Red;
                Console.WriteLine(string.Format("Ctrl+c to exit. Press \"r\" key to force the console application to read the change log without waiting {0} seconds.", WaitSeconds));
                Console.WriteLine();
                Console.ResetColor();
                do
                {
                    do
                    {
                        if (Console.KeyAvailable &amp;&amp; Console.ReadKey(true).KeyChar == 'r') { break; }
                    }
                    while (nextRunTime > DateTime.Now);

                    Console.WriteLine(string.Format("Looking for items modified after {0} UTC", GetDateStringFromChangeToken(cq.ChangeTokenStart)));

                    
                    ChangeCollection coll = list.GetChanges(cq);
                    cc.Load(coll);
                    cc.ExecuteQuery();


                    DisplayChanges(coll, cq.ChangeTokenStart);
                    // If there are changes to the list (which was returned in coll), set ChangeTokenStart to the last change's date and time. This will be used as the starting point for the next read from the change log.                      
                    cq.ChangeTokenStart = coll.Count > 0 ? coll.Last().ChangeToken : cq.ChangeTokenStart;

                    nextRunTime = DateTime.Now.AddSeconds(WaitSeconds);

                } while (true);
            }
            catch (Exception e)
            {
                Console.WriteLine(e.Message);
                Console.WriteLine("Press any key to exit...");
                Console.ReadKey();

            }
        }
```

## 予定表、連絡先
<a name="bk_addresources"> </a>

- [Office 365 開発パターンとプラクティス (ソリューション ガイダンス)](Office-365-development-patterns-and-practices-solution-guidance.md)
    
- [SharePoint でリモート イベント レシーバーを使用する](Use-remote-event-receivers-in-SharePoint.md)
    
- [ChangeQuery のメンバー](https://msdn.microsoft.com/library/office/microsoft.sharepoint.client.changequery_members.aspx)
    
- [パターンとプラクティス デベロッパー センター](http://dev.office.com/patterns-and-practices)
