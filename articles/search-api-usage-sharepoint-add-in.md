SharePoint アドイン モデル内で API 使用状況を検索する
===============================================

概要
-------

新しい SharePoint アドイン モデルでの SharePoint 検索サービスを使用した検索方法は、完全信頼コードの場合とは異なります。一般的な完全信頼コード (FTC)/ ファーム ソリューション シナリオでは、SharePoint サーバー側オブジェクト モデル(クエリ結果 Web パーツが上書きする) または検索 Web サービスは、SharePoint 検索サービスを使用した検索の実行に使用されていました。

SharePoint アドイン モデル シナリオでは、CSOM または REST API から SharePoint の検索サービスを使用して検索を実行します。

基本ガイドライン
---------------------

サイト コレクションとサブサイトを作成および構成した後、成果物、構成、およびブランド化資産をそこに展開する方法については、大まかに次のような基本ガイドラインが提供されています。

- AppOnly 認証の使用は、どの検索サービスでもサポートされていません。
    + これは、検索サービスはユーザーのプロファイル情報を検索するためにユーザー プロファイル サービス (UPS) にアクセスし、UPS は AppOnly 認証をサポートしていないためです。
    + したがって、検索関連性やその他の検索ファセットは指定されたユーザーやそのユーザーのプロファイル 属性によって異なるため、AppOnly 認証パターンは機能しません。

SharePoint 検索サービスでの検索実行オプション
--------------------------------------------------------------

SharePoint 検索サービスで検索を実行するには、いくつかのオプションがあります。

- .Net CSOM API
- JavaScript CSOM (JSOM) API
- REST API  

.Net CSOM API
-------------
このオプションでは、SharePoint 検索サービスで検索を実行するのに .Net CSOM API を使用します。
    
- この API は、.Net マネージ コードでのみ使用できます。


**適切な場合**

- この API は、プロバイダー ホスト型アドイン、長い時間がかかる操作、または .Net プラットフォーム上で実行されるその他のサーバー側のシナリオに最適です。
- これらのシナリオの一例として、ASP.NET MVC Web サイト、ASP.NET Web API サービス、.Net コンソールや Windows アプリケーション、Azure Web Jobs などがあります。

**はじめに**

次のサンプルでは、.Net CSOM API を使用して SharePoint の検索サービスで検索を実行する方法を示します。この例では、ユーザーのプロファイルにアクセスし、検索結果をカスタマイズする方法も示します。

- [Search.PersonalizedResults (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Search.PersonalizedResults)

![検索 API と個人用設定のページ。 画像内のテキスト: 検索 API 検索の実行。 テナント全体の検索クエリに検索フィルターを指定:「Test」という単語が入力されたテキスト ボックス。 ボタンのテキスト:簡易検索を実行する。 すべてのサイト テンプレートに対して、プロファイル データを使用して個人用に設定した検索を実行する。 「プロファイル」に「AppTest」というテキストが含まれていない場合は、チーム サイト (WebTemplate = STS) のみを検索します。 「AppTest」が存在する場合は、すべてのサイトを検索します。 シナリオ: ユーザーのプロファイルに基づいて、特定の場所から得られるサイトまたは集計データを表示する。 例としては、現在のユーザーの場所または都市と一致する識別子を含むタグが付けられたニュース ページのみを集計することが挙げられます。 ボタンのテキスト:個人用に設定された検索を実行する。](media/Recipes/SearchAPI/Search.PersonalizedResults.png)

[CSOM を使用してカスタムの検索クエリを実行する方法 (O365 PnP ビデオ)](https://channel9.msdn.com/blogs/OfficeDevPnP/How-to-perform-personalized-search-queries-with-CSOM) は、[Search.PersonalizedResults (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Search.PersonalizedResults) について説明します。

**Default.aspx.cs クラス**の [btnPerformSearch_Click](https://github.com/OfficeDev/PnP/blob/master/Samples/Search.PersonalizedResults/PersonalizedSearchResultsWeb/Pages/Default.aspx.cs) メソッドは、ユーザーが検索ボックスに入力したテキスト値の検索を実行し、サイト コレクションに保存されているすべてのコンテンツに対して検索内容を調べます。contentclass:"STS_Site" パラメーターは、検索範囲をサイト コレクションに制限します。

    protected void btnPerformSearch_Click(object sender, EventArgs e)
    {
        var spContext = SharePointContextProvider.Current.GetSharePointContext(Context);

        using (var clientContext = spContext.CreateUserClientContextForSPHost())
        {
            // Since in this case we want only site collections, let's filter based on result type
            string query = searchtext.Text + " contentclass:\"STS_Site\"";
            ClientResult<ResultTableCollection> results = ProcessQuery(clientContext, query);
            lblStatus1.Text = FormatResults(results);
        }
    }

**Default.aspx.cs クラス**の [btnPersonalizedSearch_Click](https://github.com/OfficeDev/PnP/blob/master/Samples/Search.PersonalizedResults/PersonalizedSearchResultsWeb/Pages/Default.aspx.cs) メソッドは、btnPerformSearch_Click メソッドと同じ検索を実行し、現在のユーザー プロファイルに基づいてパラメーターを追加します。PeopleManager クラスは、現在のユーザーのプロファイル プロパティへのアクセスに使用されます。

    protected void btnPersonalizedSearch_Click(object sender, EventArgs e)
    {
        var spContext = SharePointContextProvider.Current.GetSharePointContext(Context);

        using (var clientContext = spContext.CreateUserClientContextForSPHost())
        {
            // Load user profile properties
            PeopleManager peopleManager = new PeopleManager(clientContext);
            PersonProperties personProperties = peopleManager.GetMyProperties();
            clientContext.Load(personProperties);
            clientContext.ExecuteQuery();
            // Check teh value for About Me to investigate current values
            string aboutMeValue = personProperties.UserProfileProperties["AboutMe"];
            string templateFilter = ResolveAdditionalFilter(aboutMeValue);
            // Let's build the query
            string query = "contentclass:\"STS_Site\" " + templateFilter;
            ClientResult<ResultTableCollection> results = ProcessQuery(clientContext, query);
            lblStatus2.Text = FormatResults(results);
        }
    }

**Default.aspx.cs クラス**の [ResolveAdditionalFilter](https://github.com/OfficeDev/PnP/blob/master/Samples/Search.PersonalizedResults/PersonalizedSearchResultsWeb/Pages/Default.aspx.cs) メソッドは、現在のユーザーのプロファイル プロパティを評価し、適切な検索パラメーターを返します。この例では、aboutMeValue ユーザー プロファイル プロパティに AppTest が含まれている場合、検索パラメーター WebTemplate=STS が返されます。このパラメーターは、STS (チーム サイト) テンプレートで構築されたサイトの検索範囲を制限します。
 
    private string ResolveAdditionalFilter(string aboutMeValue)
    {
        if (!aboutMeValue.Contains("AppTest"))
        {
            return "WebTemplate=STS";
        }
        
        return "";
    }

いずれの場合も、**Default.aspx.cs クラス**の [ProcessQuery](https://github.com/OfficeDev/PnP/blob/master/Samples/Search.PersonalizedResults/PersonalizedSearchResultsWeb/Pages/Default.aspx.cs) メソッドは SearchExecutor クラスを使用して検索クエリを実行し、結果を返します。 

    private ClientResult<ResultTableCollection> ProcessQuery(ClientContext ctx, string keywordQueryValue)
    {
        KeywordQuery keywordQuery = new KeywordQuery(ctx);
        keywordQuery.QueryText = keywordQueryValue;
        keywordQuery.RowLimit = 500;
        keywordQuery.StartRow = 0;
        keywordQuery.SelectProperties.Add("Title");
        keywordQuery.SelectProperties.Add("SPSiteUrl");
        keywordQuery.SelectProperties.Add("Description");
        keywordQuery.SelectProperties.Add("WebTemplate");
        keywordQuery.SortList.Add("SPSiteUrl", Microsoft.SharePoint.Client.Search.Query.SortDirection.Ascending);
        SearchExecutor searchExec = new SearchExecutor(ctx);
        ClientResult<ResultTableCollection> results = searchExec.ExecuteQuery(keywordQuery);
        ctx.ExecuteQuery();
        return results;
    }

JavaScript CSOM (JSOM) API
--------------------------
このオプションでは、SharePoint 検索サービスで検索を実行するのに JavaScript CSOM (JSOM) API を使用します。
    
- この API は、クライアント側 JavaScript コードでのみ使用できます。

**適切な場合**

- この API は、あらゆる Web プラットフォームで実行される SharePoint ホスト型アドインおよびプロバイダー ホスト型アドインに最適です。
- これらのシナリオの一例として、ASP.NET MVC Web サイト、PHP Web サイト、Python Web サイトなどがあります。

**はじめに**

次のコード例は、JavaScript CSOM (JSOM) API を使用して SharePoint 検索サービスを実行する方法を示しています。この例では、「Blizzard」 という単語を含むすべてのアイテムの検索を実行します。

    var context = SP.ClientContext.get_current();
    
    var keywordQuery = 
    new Microsoft.SharePoint.Client.Search.Query.KeywordQuery(context);
    
    keywordQuery.set_queryText("Blizzard");
    
    var searchExecutor = 
    new Microsoft.SharePoint.Client.Search.Query.SearchExecutor(context);
    
    results = searchExecutor.executeQuery(keywordQuery);
    
    context.executeQueryAsync(onGetEventsSuccess, onGetEventsFail);

REST API
--------
このオプションでは、SharePoint 検索サービスで検索を実行するのに REST API を使用します。
    
- この API は、サーバー側コードとクライアント側コードの両方で使用できるため、最も柔軟性が高い方法です。
- SharePoint 検索サービスの REST API のルート エンドポイント:
    + https://<tenant>/site/_api/search/query
- 次にシンプルな例を示します。
    + キーワード検索
    
        ```
        https://tenant/site/_api/search/query?querytext='{Apples}'
        ```
    + 特定のプロパティの選択
    
        ```
        https://tenant/site/_api/search/query?querytext='test'&selectproperties='Rank, Title'
        ```
    + 並べ替え
    
        ```
        https://tenant/site/_api/search/query?querytext='Oranges'&sortlist='LastModifiedTime:ascending'
        ```

**適切な場合**

この API は、あらゆる Web プラットフォームで実行される SharePoint ホスト型アドインおよびプロバイダー ホスト型アドインに最適です。
- これらのシナリオの一例として、ASP.NET MVC Web サイト、PHP Web サイト、Python Web サイト、ASP.NET Web API サービス、.Net コンソールや Windows アプリケーション、Azure Web Jobs などがあります。

**はじめに**

**サーバー側オプション**

次の例では、.Net マネージ コードから REST API を使用して SharePoint 検索サービスで検索を実行する方法を示します。

- [EmployeeDirectory (OfficeDev トレーニング コンテンツ)](https://github.com/OfficeDev/TrainingContent/tree/master/O3656/O3656-6%20Deep%20Dive%20into%20Search%20Scenarios%20in%20Office%20365/Demos/EmployeeDirectory)

**HomeController.cs クラス**の [Index](https://github.com/OfficeDev/TrainingContent/blob/master/O3656/O3656-6%20Deep%20Dive%20into%20Search%20Scenarios%20in%20Office%20365/Demos/EmployeeDirectory/EmployeeDirectoryWeb/Controllers/HomeController.cs) メソッドは、ユーザーがクリックするテキスト値が姓の先頭に来るすべてのユーザーに対する検索を実行します。

    var spContext = SharePointContextProvider.Current.GetSharePointContext(HttpContext);

    string accessToken = spContext.UserAccessTokenForSPHost;

    //Build the REST API request
    StringBuilder requestUri = new StringBuilder()
    .Append(spContext.SPHostUrl)
    .Append("/_api/search/query?querytext='LastName:")
    .Append(startLetter)
    .Append("*'&selectproperties='LastName,FirstName,WorkEmail,WorkPhone'&sourceid='B09A7990-05EA-4AF9-81EF-EDFAB16C4E31'&sortlist='FirstName:ascending'");

    //Create HTTP Client
    HttpClient client = new HttpClient();
    //Add the REST API request
    HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Get, requestUri.ToString());
    //Set accept header
    request.Headers.Accept.Add(new MediaTypeWithQualityHeaderValue("application/xml"));
    //Set Bearer header equal to access token
    request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);

    //Send the REST API request
    HttpResponseMessage response = await client.SendAsync(request);
    //Set the response
    string responseString = await response.Content.ReadAsStringAsync();

[検索を活用する SharePoint アドインの構築方法 (O365 PnP ビデオ)](https://channel9.msdn.com/blogs/OfficeDevPnP/How-to-build-SharePoint-add-ins-that-leverage-search)  は、[EmployeeDirectory (OfficeDev トレーニング ディレクトリ)](https://github.com/OfficeDev/TrainingContent/tree/master/O3656/O3656-6%20Deep%20Dive%20into%20Search%20Scenarios%20in%20Office%20365/Demos/EmployeeDirectory) について説明します。

**クライアント側のオプション**

次のコード例は、JavaScript から REST API を使用して SharePoint 検索サービスを実行する方法を示しています。この例では、「Lacrosse」 という単語を含むすべてのアイテムの検索を実行します。
    
    $.ajax(
            {
                url: "http://site/_api/search/" +
                     "query?querytext='{Lacrosse}‘",
                method: "GET",
                headers: {
                    "accept": "application/json;odata=verbose",
                },
                success: onSuccess,
                error: onError
            }
        );

関連リンク
=============
- [CSOM を使用してカスタムの検索クエリを実行する方法 CSOM (O365 PnP ビデオ)](https://channel9.msdn.com/blogs/OfficeDevPnP/How-to-perform-personalized-search-queries-with-CSOM)
- [EmployeeDirectory (OfficeDev トレーニング コンテンツ)](https://github.com/OfficeDev/TrainingContent/tree/master/O3656/O3656-6%20Deep%20Dive%20into%20Search%20Scenarios%20in%20Office%20365/Demos/EmployeeDirectory)
- [検索を活用する SharePoint アドインの構築方法 (O365 PnP ビデオ)](https://channel9.msdn.com/blogs/OfficeDevPnP/How-to-build-SharePoint-add-ins-that-leverage-search)
- ガイダンス記事: [http://aka.ms/OfficeDevPnPGuidance](http://aka.ms/OfficeDevPnPGuidance "ガイダンス記事")
- MSDN リファレンス: [http://aka.ms/OfficeDevPnPMSDN](http://aka.ms/OfficeDevPnPMSDN "MSDN リファレンス")
- ビデオ: [http://aka.ms/OfficeDevPnPVideos](http://aka.ms/OfficeDevPnPビデオ"Videos")

関連する PnP サンプル
===================
- [Search.PersonalizedResults (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Search.PersonalizedResults)
- [http://aka.ms/OfficeDevPnP](http://aka.ms/OfficeDevPnP) にあるサンプルとコンテンツ

適用対象
==========
- Office 365 マルチテナント (MT)
- Office 365 専用 (D)
- SharePoint 2013 オンプレミス
