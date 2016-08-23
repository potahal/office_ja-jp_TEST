SharePoint アドイン モデルにおけるワークフロー、アクション (アクティビティ)、イベント、およびフォーム
=================================================================================

概要
-------

新しい SharePoint アドイン モデルにおいて、ワークフローおよびその関連コンポーネントの実装作成方法は、完全信頼コードの場合とは異なります一般的な完全信頼コード (FTC) / ファーム ソリューション シナリオでは、ワークフローやその関連コンポーネントはサーバー側コードを使用して構築され、SharePoint ソリューションで展開されていました。ワークフローおよびその関連コンポーネントは、SharePoint サーバー上で実行されました。

SharePoint アドイン モデル シナリオでは、ワークフローおよびその関連コンポーネントは、リモート サーバー上で実行されるコードを使用して開発されます。ワークフローおよびその関連コンポーネントはワークフロー サービスに登録されていますが、すべてのコードはリモート サーバー上で実行します。

基本ガイドライン
---------------------

新しい SharePoint アドイン モデルにおけるワークフローおよびその関連コンポーネントの作成については、大まかに次のような基本ガイドラインが提供されています。

- コード ビハインド ワークフローは一般的に、Office 365 テナンシーや SharePoint アドイン モデルでは使用できません。
- ワークフローおよびその関連コンポーネントと関連付られたコード ビハインドはすべて、リモート サーバーで実行する外部の Web サービスに置く必要があります。

カスタム ワークフローおよびその関連コンポーネントを作成するオプション
------------------------------------------------------------------
カスタム ワークフローおよびその関連コンポーネントを作成するには、いくつかのオプションがあります。

- カスタム ワークフローを作成する
- カスタム ワークフロー アクティビティを作成する
- カスタム ワークフロー イベントを作成する
- カスタム ワークフロー フォームを作成する

カスタム ワークフローを作成する
-----------------------
このオプションでは、カスタム ワークフローは SharePoint のホスト Web で作成および展開され、関連付けられます。

- カスタム ワークフローは Visual Studio または SharePoint Designer で作成できます。
    + 両方のオプションをまとめ、それぞれの長所と短所について説明した「[Visual Studio を使用して SharePoint 2013 ワークフローを開発する (MSDN 記事)](https://msdn.microsoft.com/en-us/library/office/jj163199.aspx)」を参照してください。
- カスタム ワークフローは、いくつかの方法で SharePoint の Web ホストに展開し、関連付けることができます。
    - Visual Studio で作成されたワークフローは、展開用の機能の中に自動的にパッケージ化されます。
    - SharePoint デザイナーで作成されたワークフローは、開発サーバーから運用サーバーにエクスポートおよびインポートし、展開する必要があります。
    - カスタム ワークフローは、リモート プロビジョニング パターンを使用して SharePoint の Web ホストに展開し、関連付けることができます。リモート プロビジョニング パターンの詳細については、「[サイト プロビジョニング (SharePoint アドイン レシピ)](site-provisioning-sharepoint-add-in.md)」を参照してください。

次のコード サンプルでは、CSOM を使用してワークフローのプロビジョニングを行う方法について説明し、ワークフローをサポートするリストを示します。

    public void ProvisionIncidentWorkflowAndRelatedLists(string incidentWorkflowFile, string suiteLevelWebAppUrl, string dispatcherName)
    {
        //Return the list the workflow will be associated with
        var incidentsList = CSOMUtil.GetListByTitle(clientContext, "Incidents");

        //Create a new WorkflowProvisionService class instance which uses the 
        //Microsoft.SharePoint.Client.WorkflowServices.WorkflowServicesManager to
        //provision and configure workflows
        var service = new WorkflowProvisionService(clientContext);

        //Read the workflow .XAML file
        var incidentWF = System.IO.File.ReadAllText(incidentWorkflowFile);

        //Create the WorkflowDefinition and use the 
        //Microsoft.SharePoint.Client.WorkflowServices.WorkflowServicesManager
        //to save and publish it.  
        //This method is shown below for reference.
        var incidentWFDefinitionId = service.SaveDefinitionAndPublish("Incident",
            WorkflowUtil.TranslateWorkflow(incidentWF));

        //Create the workflow tasks list
        var taskListId = service.CreateTaskList("Incident Workflow Tasks");

        //Create the workflow history list
        var historyListId = service.CreateHistoryList("Incident Workflow History");

        //Use the Microsoft.SharePoint.Client.WorkflowServices.WorkflowSubscriptionService to 
        //subscibe the workflow to the list the workflow is associated with, register the
        //events it is associated with, and register the tasks and history list. 
        //This method is shown below for reference.
        service.Subscribe("Incident Workflow", incidentWFDefinitionId, incidentsList.Id,
            WorkflowSubscritpionEventType.ItemAdded, taskListId, historyListId);
    }
    
    public Guid SaveDefinitionAndPublish(string name, string translatedWorkflows)
    {
        var definition = new WorkflowDefinition(ClientContext)
        {
            DisplayName = name,
            Xaml = translatedWorkflows
        };

        var deploymentService = workflowServicesManager.GetWorkflowDeploymentService();
        var result = deploymentService.SaveDefinition(definition);
        ClientContext.ExecuteQuery();

        deploymentService.PublishDefinition(result.Value);
        ClientContext.ExecuteQuery();

        return result.Value;
    }

    public Guid Subscribe(string name, Guid definitionId, Guid targetListId, WorkflowSubscritpionEventType eventTypes, Guid taskListId, Guid historyListId)
    {
        var eventTypesList = new List<string>();
        foreach (WorkflowSubscritpionEventType type in Enum.GetValues(typeof(WorkflowSubscritpionEventType)))
        {
            if ((type & eventTypes) > 0)
                eventTypesList.Add(type.ToString());
        }

        var subscription = new WorkflowSubscription(ClientContext)
        {
            Name = name,
            Enabled = true,
            DefinitionId = definitionId,
            EventSourceId = targetListId,
            EventTypes = eventTypesList.ToArray()
        };

        subscription.SetProperty("TaskListId", taskListId.ToString());
        subscription.SetProperty("HistoryListId", historyListId.ToString());

        var subscriptionService = workflowServicesManager.GetWorkflowSubscriptionService();
        var result = subscriptionService.PublishSubscriptionForList(subscription, targetListId);

        ClientContext.ExecuteQuery();
        return result.Value;
    }

**適切な場合**

コード ビハインドを使用するカスタム ワークフローを作成する必要がある場合は、このオプションが適しています。

**はじめに**

次の記事は、カスタム ワークフローの作成方法について説明します。

- [Visual Studio 2012 を使用して SharePoint ワークフロー アプリを作成する (MSDN 記事)](https://msdn.microsoft.com/EN-US/library/office/dn456545.aspx)
- [方法:Visual Studio を使用して SharePoint 2013 ワークフローを作成する (MSDN 記事)](https://msdn.microsoft.com/EN-US/library/office/dn584771.aspx)

カスタム ワークフロー アクティビティを作成する
---------------------------------
このオプションでは、カスタム ワークフロー アクティビティが作成され、SharePoint に展開されます。

- カスタム ワークフロー アクティビティは、Visual Studio で作成できます。
- SharePoint デザイナーを使用して、カスタム ワークフロー アクティビティを作成することはできません。
- カスタム ワークフロー アクティビティは、SharePoint 環境に展開された後、SharePoint Designer で使用できます。

**適切な場合**

ビジネス プロセスでワークフローを実装する必要があり、SharePoint Designer のワークフロー アクションの標準ライブラリがその要件を満たしていない場合

**はじめに**

次の記事では、Visual Studio でカスタム ワークフロー アクティビティを作成する方法について説明します。

- [方法:ワークフロー カスタム アクションの構築と展開 (MSDN 記事)](https://msdn.microsoft.com/en-us/library/office/jj163911.aspx)

「[Workflow.Activities (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Workflow.Activities)には、Visual Studio で作成されたいくつかのカスタム ワークフロー アクティビティが含まれています。また、カスタム ワークフロー アクティビティをワークフローで使用する方法についても説明します。

カスタム ワークフロー イベントを作成する
-----------------------------
このオプションでは、カスタム ワークフロー イベントが作成され、SharePoint に展開されます。

- カスタム ワークフロー イベントは、Visual Studio で作成できます。
- SharePoint デザイナーを使用して、カスタム ワークフロー イベントを作成することはできません。
- カスタム ワークフロー イベントは、SharePoint 環境に展開された後、SharePoint Designer で使用できます。

**適切な場合**

ビジネス プロセスでワークフローを実装する必要があり、その要件でワークフローをカスタム イベントを待機してから続行する必要がある場合。

**はじめに**

「[Workflow.CustomEvents (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Workflow.CustomEvents)は、カスタム イベントの発生を待機してから続行するワークフローの作成方法について説明します。また、ワークフロー サービス マネージャーに JavaScript クライアント側オブジェクト モデル (JSOM) を使用してカスタム イベントを発生させる方法についても説明します。

カスタム ワークフロー フォームを作成する
------------------------------------------------
このオプションでは、カスタム ワークフロー フォームが作成され、SharePoint に展開されます。

- カスタム ワークフロー フォームは、Visual Studio で作成できます。
- SharePoint デザイナーを使用して、カスタム ワークフロー フォームを作成することはできません。
- カスタム ワークフロー フォームは、SharePoint 環境に展開された後、SharePoint Designer で使用できます。

**適切な場合**

ビジネス プロセスでワークフローを実装する必要があり、その要件でカスタム フォームが必要な場合。

**はじめに**

次の記事では、カスタム ワークフローの関連付けと初期フォームを使用し、それらをワークフローで使用する方法について説明します。

- [方法:Visual Studio 2012 を使用して SharePoint Server 2013 カスタム ワークフロー フォームを作成する方法 (MSDN 記事)](https://msdn.microsoft.com/EN-US/library/office/dn518136.aspx)

「[Workflow.CustomTasks (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Workflow.CustomTasks)」は、カスタム タスクと初期フォームを作成し、それらをワークフローで使用する方法を説明します。

カスタム ワークフローから SharePoint データを更新するオプション
--------------------------------------------------------
カスタム ワークフローから SharePoint データを更新するには、いくつかのオプションがあります。

- コンテキスト トークンとアドイン Web URL を使用して SharePoint への認証を行います。
- アドイン Web URL と ShrePoint Web プロキシを使用して SharePoint への認証を行います。

コンテキスト トークンとアドイン Web URL を使用して SharePoint への認証を行う
----------------------------------------------------------------------

このオプションでは、ワークフローから、http ヘッダー経由でワークフローが呼び出すサービスへコンテキスト トークンとアドイン Web URL を渡します。サービスはコンテキスト トークンとアドイン Web URL を使用して SharePoint への認証を行い、SharePoint データの更新を行う前にアクセス トークンを返します。 

- この方法では、ワークフローから Web サービスにコンテンツ トークンを渡す必要があります。

**適切な場合**

クライアント レベルで SharePoint への通信を行う場合。

**はじめに**

次の例は、コンテキスト トークンとアドイン Web URL を使用して SharePoint への認証と SharePoint データの更新を行う方法を説明します。

- [Workflow.CallCustomService (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Workflow.CallCustomService)

コンテキスト トークンとアドイン Web URL をワークフローからサービスに渡す方法の詳細については、[Workflow.CallCustomService (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Workflow.CallCustomService) README の「[Web API サービスの呼び出し](https://github.com/OfficeDev/PnP/tree/master/Samples/Workflow.CallCustomService#call-web-api-service)」セクションを参照してください。

[Workflow.CallCustomService (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Workflow.CallCustomService) README の他のセクションは、ワークフロー全体やリモート サービスの詳細な情報も提供し、Microsoft Azure 内のすべてのコンポーネントの設定についても説明します。

アドイン Web URL と ShrePoint Web プロキシを使用して SharePoint への認証を行う
---------------------------------------------------------------------------------

このオプションでは、ワークフローが開始すると、ワークフローからワークフローが呼び出すサービスにアドイン Web URL を渡します。サービスはアドイン Web URL を SharePoint Web プロキシに渡します。ShrePoint Web プロキシはアドイン Web URL を使用して SharePoint への認証を行い、アクセス トークンを返します。次に、SharePoint Web プロキシは、SharePoint データの更新を更新するための呼び出しを行う前に、http ヘッダーにアクセス トークンを追加します。

- この方法では、ワークフローから Web サービスにコンテンツ トークンを渡す必要はありません。

**適切な場合**

SharePoint への通信をサーバー レベルで行う場合。

**はじめに**

次の例は、アドイン Web URL と ShrePoint Web プロキシを使用して SharePoint への認証と SharePoint データの更新を行う方法を説明します。

- [Workflow.CallServiceUpdateSPViaProxy (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Workflow.CallServiceUpdateSPViaProxy)

SharePoint Web プロキシの呼び出しの詳細については、[Workflow.CallServiceUpdateSPViaProxy (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Workflow.CallServiceUpdateSPViaProxy) README の「[Web プロキシを使用して Web API サービスを呼び出す](https://github.com/OfficeDev/PnP/tree/master/Samples/Workflow.CallServiceUpdateSPViaProxy#call-web-api-service-via-web-proxy)」セクションを参照してください。

[Workflow.CallServiceUpdateSPViaProxy (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Workflow.CallServiceUpdateSPViaProxy) README の他のセクションは、ワークフロー全体やリモート サービスの詳細な情報も提供し、Microsoft Azure 内のすべてのコンポーネントの設定についても説明します。

SharePoint Web プロキシの詳細については、「[SharePoint 2013 で Web プロキシを使用してリモート サービスのクエリを実行する (MSDN 記事)](https://msdn.microsoft.com/en-us/library/office/fp179895.aspx)」を参照してください。

関連リンク
=============
- [SharePoint 2013 ワークフロー オブジェクト モデル (MSDN 記事)](https://msdn.microsoft.com/EN-US/library/office/jj163969.aspx)
- [SharePoint ワークフロー開発における通信エラー メッセージ (MSDN 記事)](https://msdn.microsoft.com/EN-US/library/office/dn449112.aspx)
- [SharePoint 2013 でワークフローの相互運用を使用する (MSDN 記事)](https://msdn.microsoft.com/EN-US/library/office/jj670125.aspx)
- [Visual Studio を使用して SharePoint 2013 ワークフローを開発する (MSDN 記事)](https://msdn.microsoft.com/en-us/library/office/jj163199.aspx)
- [サイト プロビジョニング (SharePoint アドイン レシピ)](site-provisioning-sharepoint-add-in.md)
- [Visual Studio 2012 を使用して SharePoint ワークフロー アプリを作成する (MSDN 記事)](https://msdn.microsoft.com/EN-US/library/office/dn456545.aspx)
- [方法:Visual Studio を使用して SharePoint 2013 ワークフローを作成する (MSDN 記事)](https://msdn.microsoft.com/EN-US/library/office/dn584771.aspx)
- [方法:ワークフロー カスタム アクションの構築と展開 (MSDN 記事)](https://msdn.microsoft.com/en-us/library/office/jj163911.aspx)
- [方法:Visual Studio 2012 を使用して SharePoint Server 2013 カスタム ワークフロー フォームを作成する方法 (MSDN 記事)](https://msdn.microsoft.com/EN-US/library/office/dn518136.aspx)
- [SharePoint 2013 で Web プロキシを使用してリモート サービスのクエリを実行する (MSDN 記事)](https://msdn.microsoft.com/en-us/library/office/fp179895.aspx)
- [モジュール (SharePoint アドインのレシピ)](modules-sharepoint-add-in.md)
- [サイト プロビジョニング (SharePoint アドイン レシピ)](site-provisioning-sharepoint-add-in.md)
- ガイダンス記事: [http://aka.ms/OfficeDevPnPGuidance](http://aka.ms/OfficeDevPnPGuidance "ガイダンス記事")
- MSDN リファレンス: [http://aka.ms/OfficeDevPnPMSDN](http://aka.ms/OfficeDevPnPMSDN "MSDN リファレンス")
- ビデオ: [http://aka.ms/OfficeDevPnPVideos](http://aka.ms/OfficeDevPnPビデオ"Videos")

関連する PnP サンプル
===================
- [Workflow.Activities (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Workflow.Activities)
- [Workflow.CustomEvents (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Workflow.CustomEvents)
- [Workflow.CustomTasks (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Workflow.CustomTasks)
- [Workflow.CallCustomService (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Workflow.CallCustomService)
- [Workflow.CallServiceUpdateSPViaProxy (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Workflow.CallServiceUpdateSPViaProxy)
- [http://aka.ms/OfficeDevPnP](http://aka.ms/OfficeDevPnP) にあるサンプルとコンテンツ

適用対象
==========
- Office 365 マルチテナント (MT)
- Office 365 専用 (D)
- SharePoint 2013 オンプレミス
