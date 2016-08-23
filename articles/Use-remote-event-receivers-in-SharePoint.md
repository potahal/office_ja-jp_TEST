# SharePoint でリモート イベント レシーバーを使用する

リモート イベント レシーバーは、SharePoint アドイン モデルでイベントを処理するために使用します。AppInstalled および AppUninstalling イベントを使用すると、アドインに必要な SharePoint オブジェクトとその他のイベント レシーバーをセットアップまたは削除できます。

_**適用対象:** SharePoint 用アドイン | SharePoint 2013 | SharePoint Online_

[Core.EventReceivers](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.EventReceivers) サンプルでは、プロバイダー向けのホスト型アドインを使用してリモート イベント レシーバーで AppInstalled イベントおよび AppUninstalling イベントを処理する方法を紹介します。AppInstalled イベントおよび AppUninstalling イベントでは、アドインが実行時に使用する SharePoint オブジェクトをセットアップおよび削除します。さらに、AppInstalled イベント ハンドラーでは、ItemAdded イベント ハンドラーをリストに追加します。このソリューションは、以下の操作を行うために使用してください。

- アドインの初めての実行時に AppInstalled イベントを使用して構成を行い、さまざまな SharePoint オブジェクトや、アドインで処理する追加のイベント レシーバーをセットアップします。
    
- 完全に信頼されたコード ソリューションを使用して実装したイベント レシーバーと置き換えます。完全に信頼されたコード ソリューションでは、SharePoint サーバー上でイベント レシーバーを実行できます。新しい SharePoint アドイン モデルでは、SharePoint サーバー上でイベント レシーバーを実行できないため、リモート イベント レシーバーは Web サーバー上に実装する必要があります。
    
- SharePoint で発生する変更の通知を受信します。たとえば、新しいアイテムがリストに追加されたときタスクを実行する場合に利用できます。

- 変更ログのソリューションを補完します。リモート イベント レシーバー パターンを変更ログ パターンと一緒に使用すると、SharePoint コンテンツ データベース、サイト コレクション、サイト、またリストに加えられたすべての変更を処理する、さらに信頼性の高いアーキテクチャが提供されます。リモート イベント レシーバーは即座に実行されますが、リモート サーバー上で実行されているため、通信エラーが発生する可能性があります。変更ログ パターンによりすべての変更が処理可能な状態に保持されますが、アプリケーションによる変更の処理は、通常、スケジュールに従って (たとえば、タイマー ジョブとして) 実行されます。これは、変更が即座には処理されないことを意味します。これら 2 つのパターンを一緒に使用すれば、同じ変更を 2 回処理する可能性を確実に回避するメカニズムを使用できます。詳細については、「 [ChangeQuery と ChangeToken で SharePoint の変更ログを照会する](query-sharepoint-change-log-with-changequery-and-changeToken.md)」を参照してください。

**メモ**  SharePoint ホスト型アドインは、リモート イベント レシーバーをサポートしていません。リモート イベント レシーバーを使用するには、プロバイダー向けのホスト型アドインを使用する必要があります。リモート イベント レシーバーは、同期のシナリオや、長時間実行するプロセスのために使用するべきではありません。詳細については、「[SharePoint 2013 でアドイン イベント レシーバーを作成する](https://msdn.microsoft.com/library/office/jj220052.aspx)」を参照してください。

## 始める前に
<a name="sectionSection0"> </a>

まず GitHub の [Office 365 Developer Patterns and Practices](https://github.com/OfficeDev/PnP/tree/dev) プロジェクトから、[Core.EventReceivers](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.EventReceivers) サンプル アドインをダウンロードします。

このアドインを実行する前に、次の手順を実行します。

1. Core.EventReceivers プロジェクトのプロパティで、**[アプリのハンドルがインストールされました]** および **[アプリのハンドルをアンインストール中]** が **True** に設定されていることを確認します。**[アプリのハンドルがインストールされました]** および **[アプリのハンドルをアンインストール中]** を **True** に設定すると、 **AppInstalled** および **AppUninstalling** の各イベントに対するイベント ハンドラーを定義する WCF サービスが作成されます。Core.EventReceivers で、AppManifest.xml のショートカット メニューを開き (右クリック)、**[プロパティ]** を選択します。**InstalledEventEndpoint** および **UninstallingEventEndpoint** は **AppInstalled** および **AppUninstalling** の各イベントを処理するリモート イベント レシーバーをポイントします。
    
2. リモート イベント レシーバーをホスト Web 内のオブジェクトにアタッチするには、通常、そのオブジェクトに対してのみ  **[管理]** アクセス許可が必要です。たとえば、イベント レシーバーを既存のリストにアタッチする場合、アドインにはその **[リスト]** に対してのみ **[管理]** アクセス許可が必要です。このコード サンプルでは、リストを追加し、ホスト Web 上の機能をアクティブ化するため、 **[Web]** に対する **[管理]** アクセス許可が必要です。Web に対して管理アクセス許可を設定するには、次のようにします。
    
    1. Core.EventReceivers\AppManifest.xml をダブルクリックします。
    
    2. **[アクセス許可]** を選択します。
    
    3. **[スコープ]** が **[Web]** に設定され、 **[アクセス許可]** が **[管理]** に設定されていることを確認します。
    
3. このコード例を実行するには、Azure サブスクリプションが必要です。試用をサインアップするには、[1 か月間の無料評価版](http://azure.microsoft.com/en-us/pricing/free-trial/)をご利用ください。
    
4. Azure Service Bus の名前空間を ACS サポート付きで作成します。
    
    1. Azure PowerShell をインストールします。詳細については、「[方法: Azure PowerShell をインストールする](http://azure.microsoft.com/en-us/documentation/articles/powershell-install-configure/#Install)」を参照してください。
    
    2. **Azure PowerShell** を起動します。
    
    3. 「 **Add-AzureAccount** 」と入力します。
    
    4. **[Windows Azure へのサインイン]** ダイアログに電子メール アドレスを入力して、 **[続行]** を選択します。
    
    5. パスワードを入力して、 **[サインイン]** を選択します。
    
    6. 新しい Service Bus 名前空間を、次の PowerShell コマンドレットを使用して作成します。
    
        ```powershell
        New-AzureSBNamespace NamespaceNameRegion -CreateACSNamespace $true -NamespaceType Messaging
        
        ```
    
       Where:
    
       -  _NamespaceName_ は、Azure Service Bus 名前空間の名前です。
    
       -  _Region_ は、最も近いリージョンです。たとえば、 **"West US"** と入力します。リージョン名は二重引用符で囲む必要があります。
    
    7. Azure 管理ポータルに戻ります。 **[Service Bus]** を選択して、入力した名前空間を選択します。
    
    8. **[接続文字列の管理]** を選択し、 **[ACS 接続文字列]** でコピー ボタンを選択します。
    
    9. Visual Studio に戻ります。
    
    10. Core.EventReceivers を右クリックし、 **[プロパティ]** > **[SharePoint]** とクリックします。
    
    11. **[Microsoft Azure Service Bus 経由でのデバッグを有効にする]** を選択します。
    
    12. **[Microsoft Azure Service Bus 接続文字列]** に ACS 接続文字列を貼り付けます。
    
    13. **[保存]** を選択します。
    
5. コード サンプルを実行し、次の追加の手順を実行します。
    
    - **[アプリへのアクセス許可の付与]** ウィンドウの **[信頼する]** をクリックします。
    
    - **[アプリへのアクセス許可の付与]** ウィンドウを閉じます。
    
    - アドインと WCF サービスのインストールが完了すると、ブラウザーが開きます。
    
    - Office 365 サイトにサインインします。Core.EventReceivers アドインのスタート ページが表示されます。

## Core.EventReceivers アドインを使用する
<a name="sectionSection1"> </a>

Core.EventReceivers コード サンプルのデモを見るには、次の手順を実行します。

1. サンプルを実行し、スタート ページで  **[サイトに戻る]** を選択します。
    
2. **[サイト コンテンツ]** を選択します。
    
3. **[Remote Event Receiver Jobs]** (リモート イベント レシーバーのジョブ) を選択します。
    
4. **[new item]** (新しいアイテム) を選択します。
    
5. **[Title]** (タイトル) に「**Contoso**」と入力し、**[Description]** (説明) に「**Contoso test**」と入力します。
    
6. リストに戻り、ページを更新します。
    
7. 新しく追加されたアイテムの説明が「**Contoso test Updated by ReR 192336**」に更新されたことを確認します (**192336** はタイムスタンプ)。
    
Services/AppEventReceiver.cs では、 **AppEventReceiver** が **IRemoteEventService** インターフェイスを実装します。このコード サンプルでは、同期イベントを使用するため、 **ProcessEvent** メソッドの実装を提供しています。非同期イベントを使用する場合は、 **ProcessOneWayEvent** メソッドの実装を提供する必要があります。

**ProcessEvent** は、次の **SPRemoteEventType** リモート イベントを処理します。

-  アドインをインストールするときの  **AppInstalled** イベント。 **AppInstalled** イベントが発生すると、 **ProcessEvent** は **HandleAppInstalled** を呼び出します。
    
-  アドインをアンインストールするときの **AppUninstalling** イベント。**AppUninstalling** イベントが発生すると、**ProcessEvent** は **HandleAppUninstalling** を呼び出します。**AppUninstalling** イベントが実行されるのは、ユーザーがアドインを完全に削除した時点のみです。エンドユーザーの場合はサイトのごみ箱からアドインを削除した時、開発者の場合は **[テスト中アプリ]** の一覧からアドインを削除した時です。
    
-  リストにアイテムが追加された時の **ItemAdded** イベント。 **ItemAdded** イベントが発生すると、 **ProcessEvent** は **HandleItemAdded** を呼び出します。

**メモ**  Core.EventReceiver プロジェクトのプロパティでは、**[アプリのハンドルがインストールされました]** プロパティと **[アプリのハンドルをアンインストール中]** プロパティのみを使用できます。このコード サンプルでは、アドインのインストール中に **AppInstalled** イベントを使用してホスト Web 上のリストに **ItemAdded** イベント ハンドラーを追加する方法を示しています。

**メモ**  この記事で提供されるコードは、明示または黙示のいかなる種類の保証なしに現状のまま提供されるものであり、特定目的への適合性、商品性、権利侵害の不存在についての暗黙的な保証は一切ありません。

```C#
public SPRemoteEventResult ProcessEvent(SPRemoteEventProperties properties)
        {

            SPRemoteEventResult result = new SPRemoteEventResult();

            switch (properties.EventType)
            {
                case SPRemoteEventType.AppInstalled:
                    HandleAppInstalled(properties);
                    break;
                case SPRemoteEventType.AppUninstalling:
                    HandleAppUninstalling(properties);
                    break;
                case SPRemoteEventType.ItemAdded:
                    HandleItemAdded(properties);
                    break;
            }


            return result;
        }
```

**HandleAppInstalled** は、RemoteEventReceiverManager.cs 内の **RemoteEventReceiverManager.AssociateRemoteEventsToHostWeb** を呼び出します。

```C#
 private void HandleAppInstalled(SPRemoteEventProperties properties)
        {
            using (ClientContext clientContext =
                TokenHelper.CreateAppEventClientContext(properties, false))
            {
                if (clientContext != null)
                {
                    new RemoteEventReceiverManager().AssociateRemoteEventsToHostWeb(clientContext);
                }
            }
        }
```

**AssociateRemoteEventsToHostWeb** は、Core.EventReceivers アドインが使用するさまざまな SharePoint オブジェクトを作成または有効にします。要件によっては、異なる動作が必要になります。 **AssociateRemoteEventsToHostWeb** は次の処理を実行します。

- **Web.Features.Add** を使用することにより、プッシュ通知機能を有効にします。
    
- **clientContext** オブジェクトを使用してリストを検索します。リストが存在しない場合は、 **CreateJobsList** でリストを作成します。リストが存在する場合は、 **List.EventReceivers** を使用して、 **ItemAddedEvent** という名前の既存のイベント レシーバーを検索します。
    
- **ItemAddedEvent** イベント レシーバーが存在しない場合は、次の処理を実行します。
    
    - **EventReceiverDefinitionCreationInformation** オブジェクトの新しいインスタンスを作成して、新しいリモート イベント レシーバーを作成します。 **ItemAdded** イベント レシーバー タイプを **EventReceiverDefinitionCreationInformation.EventType** に追加します。
    
    - **EventReceiverDefinitionCreationInformation.ReceiverURL** に **AppInstalled** リモート イベント レシーバーの URL を設定します。
    
    - **List.EventReceivers.Add** を使用して、新しいイベント レシーバーをリストに追加します。

```C#
public void AssociateRemoteEventsToHostWeb(ClientContext clientContext)
        {
            // Add Push Notification feature to host web.
            // Not required but it is included here to show you
            // how to activate features.
            clientContext.Web.Features.Add(
                     new Guid("41e1d4bf-b1a2-47f7-ab80-d5d6cbba3092"),
                     true, FeatureDefinitionScope.None);


            // Get the Title and EventReceivers lists.
            clientContext.Load(clientContext.Web.Lists,
                lists => lists.Include(
                    list => list.Title,
                    list => list.EventReceivers).Where
                        (list => list.Title == LIST_TITLE));

            clientContext.ExecuteQuery();

            List jobsList = clientContext.Web.Lists.FirstOrDefault();

            bool rerExists = false;
            if (null == jobsList)
            {
                // List does not exist, create it. 
                jobsList = CreateJobsList(clientContext);

            }
            else
            {
                foreach (var rer in jobsList.EventReceivers)
                {
                    if (rer.ReceiverName == RECEIVER_NAME)
                    {
                        rerExists = true;
                        System.Diagnostics.Trace.WriteLine("Found existing ItemAdded receiver at "
                            + rer.ReceiverUrl);
                    }
                }
            }

            if (!rerExists)
            {
                EventReceiverDefinitionCreationInformation receiver =
                    new EventReceiverDefinitionCreationInformation();
                receiver.EventType = EventReceiverType.ItemAdded;
                
                // Get WCF URL where this message was handled.
                OperationContext op = OperationContext.Current;
                Message msg = op.RequestContext.RequestMessage;
                receiver.ReceiverUrl = msg.Headers.To.ToString();

                receiver.ReceiverName = RECEIVER_NAME;
                receiver.Synchronization = EventReceiverSynchronization.Synchronous;

                // Add the new event receiver to a list in the host web.
                jobsList.EventReceivers.Add(receiver);
                clientContext.ExecuteQuery();

                System.Diagnostics.Trace.WriteLine("Added ItemAdded receiver at " + receiver.ReceiverUrl);
            }
        }
```

アイテムが **Remote Event Receiver Jobs** リストに追加されると、AppEventReceiver.svc.cs 内の **ProcessEvent** が **ItemAdded** イベントを処理し、**HandleItemAdded** を呼び出します。**HandleItemAdded** は、**RemoteEventReceiverManager.ItemAddedToListEventHandler** を呼び出します。**ItemAddedToListEventHandler** は、追加されたリスト アイテムをフェッチし、リスト アイテムの説明に「**Updated by ReR**」という文字列を追加します。

```C#
 public void ItemAddedToListEventHandler(ClientContext clientContext, Guid listId, int listItemId)
        {
            try
            {
                List photos = clientContext.Web.Lists.GetById(listId);
                ListItem item = photos.GetItemById(listItemId);
                clientContext.Load(item);
                clientContext.ExecuteQuery();

                item["Description"] += "\nUpdated by RER " +
                    System.DateTime.Now.ToLongTimeString();
                item.Update();
                clientContext.ExecuteQuery();
            }
            catch (Exception oops)
            {
                System.Diagnostics.Trace.WriteLine(oops.Message);
            }

        }
```

## その他のリソース
<a name="bk_addresources"> </a>

-  [Office 365 開発パターンとプラクティス (ソリューション ガイダンス)](Office-365-development-patterns-and-practices-solution-guidance.md)
    
-  [Core.AppEvents.HandlerDelegation ](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.AppEvents.HandlerDelegation)
    
