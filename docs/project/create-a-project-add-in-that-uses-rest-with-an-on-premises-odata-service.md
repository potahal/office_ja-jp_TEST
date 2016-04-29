
# 社内の Project Server OData サービスで REST を使用する Project アドインを作成する
アクティブなプロジェクトのコストと作業のデータを現在の Project Web App インスタンスの全プロジェクトの平均と比較する Project Professional 2013 用の作業ウィンドウ アドインを作成します。このアドインは、REST と jQuery ライブラリを使用して、Project Server 2013 の  **ProjectData** OData レポート サービスにアクセスします。

 _ **適用対象:** apps for Office?| Office Add-ins?| Project_

この記事のコードは、Microsoft Corporation の Saurabh Sanghvi と Arvind Iyer が開発したサンプルに基づいています。

## Project Server のレポート データを読み取る作業ウィンドウ アドインを作成するための前提条件


Project Server 2013の社内インストールにおける Project Web App インスタンスの  **ProjectData** サービスを読み取る Project 作業ウィンドウ アドインを作成するための前提条件を以下に示します。


- 使用するローカルの開発用コンピューターに最新のサービス パックと Windows 更新プログラムをインストールしてあることを確認します。オペレーティング システムは、Windows 7、Windows 8、Windows Server 2008、Windows Server 2012 のいずれでもかまいません。
    
- Project Web App との接続には Project Professional 2013が必要です。Visual Studio での  **F5** デバッグを有効にするには、開発用コンピューターに Project Professional 2013がインストールされている必要があります。
    
     >**メモ**  Project Standard 2013では作業ウィンドウ アドインをホストすることもできますが、Project Web App にログオンすることはできません。
- Office Developer Tools for Visual Studio を備えた Visual Studio 2015 には、Office アドインと SharePoint アドインの作成用のテンプレートが含まれています。最新バージョンの Office Developer Tools がインストールされていることを確認してください。 [Office アドインと SharePoint のダウンロード](http://msdn.microsoft.com/en-us/office/apps/fp123627.aspx)の「 _ツール_ 」セクションを参照してください。
    
     >**メモ**  Project 2013 作業ウィンドウ アドインの作成に Napa を使用することはできません。
- この記事の手順とコード例では、ローカル ドメインの Project Server 2013の  **ProjectData** サービスにアクセスします。この記事の jQuery メソッドは Project Online には対応していません。
    
    開発用コンピューターから  **ProjectData** サービスにアクセスできることを確認してください。
    

### 手順 1. ProjectData サービスにアクセスできることを確認するには


1. ブラウザーで REST クエリからの XML データの直接表示を可能にするには、フィードの読み取りビューをオフにします。Internet Explorer でこれを行う方法については、「 [Project Server 2013 レポート データの OData フィードにクエリを実行する](http://msdn.microsoft.com/library/3eafda3b-f006-48be-baa6-961b2ed9fe01%28Office.15%29.aspx)」の手順 1. のステップ 4. を参照してください。
    
2. ブラウザーで次の URL を使用して、 **ProjectData** サービスに対してクエリを行います。
    
  ```HTML
  http://ServerName /ProjectServerName /_api/ProjectData
  ```


    たとえば、Project Web App インスタンスが  `http://MyServer/pwa` だとすると、ブラウザーに次の結果が表示されます。
    


  ```XML
  <?xml version="1.0" encoding="utf-8"?>
<service xml:base="http://myserver/pwa/_api/ProjectData/" 
        xmlns="http://www.w3.org/2007/app" 
        xmlns:atom="http://www.w3.org/2005/Atom">
    <workspace>
        <atom:title>Default</atom:title>
        <collection href="Projects">
            <atom:title>Projects</atom:title>
        </collection>
        <collection href="ProjectBaselines">
            <atom:title>ProjectBaselines</atom:title>
        </collection>
        <!-- ... and 33 more collection elements -->
    </workspace>
</service>
  ```


    結果を見るためにネットワーク資格情報の入力が必要になることもあります。ブラウザーでアクセスが拒否されことを示すメッセージ (エラー 403) が表示された場合は、その Project Web App インスタンスに対するログオンのアクセス許可を与えられていないか、管理者の支援を必要とするネットワーク上の問題が存在します。
    

## Visual Studio を使用して Project 用の作業ウィンドウ アドインを作成する


Office Developer Tools for Visual Studio には、Project 2013 用の作業ウィンドウ アドインのためのテンプレートが含まれています。 **HelloProjectOData** という名前のソリューションを作成すると、そのソリューションには次の 2 つの Visual Studio プロジェクトが含まれます。


- アドイン プロジェクト。ソリューションの名前が付けられます。このプロジェクトにはアドインの XML マニフェスト ファイルが含まれます。アドイン プロジェクトは .NET Framework 4.5 をターゲットにします。手順 3. に、 **HelloProjectOData** アドインのマニフェストを変更するための手順を示します。
    
- Web プロジェクト。 **HelloProjectODataWeb** という名前が付けられます。このプロジェクトには、作業ウィンドウ内の Web コンテンツのための Web ページ、JavaScript ファイル、CSS ファイル、イメージ、参照、および構成ファイルが含まれます。Web プロジェクトは .NET Framework 4 をターゲットにします。手順 4. および手順 5. に、Web プロジェクト内のファイルを変更して、 **HelloProjectOData** アドインの機能を作成する方法を示します。
    

### 手順 2. Project 用の HelloProjectOData アドインを作成するには


1. 管理者として Visual Studio 2015 を実行し、スタート ページで [ **新しいプロジェクト**] を選択します。
    
2. [ **新しいプロジェクト**] ダイアログ ボックスで、[ **テンプレート**]、[ **Visual C#**]、[ **Office/SharePoint**] の各ノードを展開し、[ ** Office アドイン**] を選択します。中央のウィンドウの上部にある対象のフレームワークのドロップダウン リストから [ **.NET Framework 4.5.2**] を選択し、[ **Office アドイン**] を選択します (次のスクリーンショットを参照)。
    
3. これらの Visual Studio プロジェクトを両方とも同じディレクトリに配置するには、 [ **ソリューションのディレクトリを作成**] を選択し、目的の場所を参照します。
    
4. [ **名前**] フィールドに「HelloProjectOData」と入力し、[ **OK**] をクリックします。
    
    **Office アドインを作成する**

    ![Office 2013 用アプリの作成](../../images/pj15_HelloProjectOData_CreatingApp.png)

5. [ **アドインの種類の選択**] ダイアログ ボックスで、[ **作業ウィンドウ**] を選択して、[ **次へ**] を選択します (次のスクリーンショットを参照)。
    
    **作成するアドインの種類の選択**

    ![作成するアプリの種類の選択](../../images/pj15_HelloProjectOData_ChooseProject.png)

6. [ **ホスト アプリケーションの選択**] ダイアログ ボックスで、[ **Project**] 以外のすべてのチェック ボックスをオフにし (次のスクリーンショットを参照)、 [ **完了**] をクリックします。
    
    **ホスト アプリケーションを選択する**

    ![Project を唯一のホスト アプリケーションとして選択する](../../images/b2144f2c-51f6-4e61-bc0d-972125c57031.png)
    Visual Studio によって、 **HelloProjectOdata** プロジェクトと **HelloProjectODataWeb** プロジェクトが作成されます。
    
[ **AddIn**] フォルダー (次のスクリーンショットを参照) には、カスタム CSS スタイルの App.css ファイルが含まれています。 [ **Home**] サブフォルダーの Home.html ファイルには、CSS ファイルへの参照とアドインが使用する JavaScript ファイル、およびアドインの HTML5 コンテンツが含まれています。また、Home.js ファイルは、カスタムの JavaScript コード用のファイルです。[ **Scripts**] フォルダーには、jQuery ライブラリ ファイルが含まれています。[ **Office**] サブフォルダーには、office.js や project-15.js などの JavaScript ライブラリ、および Office アドインの標準文字列の言語ライブラリが含まれます。[ **Content**] フォルダーの Office.css ファイルには、すべての Office アドインの既定のスタイルが含まれます。

**ソリューション エクスプローラーでの既定の Web プロジェクト ファイルの表示**

![ソリューション エクスプローラーに表示された Web プロジェクト ファイル](../../images/pj15_HelloProjectOData_InitialSolutionExplorer.png)

 **HelloProjectOData** プロジェクトのマニフェストは、HelloProjectOData.xml ファイルです。必要に応じてマニフェストを編集して、アドインの説明、アイコンへの参照、追加の言語の情報、およびその他の設定を追加できます。手順 3. では、アドインの表示名と説明を変更し、アイコンを追加します。

マニフェストの詳細については、「 [Office アドインの XML マニフェスト](../../docs/overview/add-in-manifests.md)」および「[Office アドインのマニフェスト向けのスキーマ リファレンス (v1.1)](http://msdn.microsoft.com/library/7e0cadc3-f613-8eb9-57ef-9032cbb97f92%28Office.15%29.aspx)」を参照してください。


### 手順 3. アドインのマニフェストを変更するには


1. Visual Studio で、HelloProjectOData.xml ファイルを開きます。
    
2. 既定の表示名は、Visual Studio プロジェクトの名前です ("HelloProjectOData")。たとえば、 **DisplayName** 要素の既定値を"ProjectData 概要" に変更します。
    
3. 既定の説明も "HelloProjectOData" です。たとえば、Description 要素の既定値を "Test REST queries of the ProjectData service" に変更します。
    
4. リボンの [ **プロジェクト**] タブの [ **Office アドイン**] ドロップダウン リストに表示するアイコンを追加します。Visual Studio ソリューションにアイコン ファイルを追加することも、アイコンの URL を使用することもできます。以下の手順は、Visual Studio ソリューションにアイコン ファイルを追加するための方法です。
    
1.  **ソリューション エクスプローラー**で、Images という名前のフォルダーに移動します。
    
2. [ **Office アドイン**] ドロップダウン リストに表示するためには、アイコンのサイズを 32 x 32 ピクセルにする必要があります。たとえば、Project 2013 SDK をインストールしてから、 [ **Images**] フォルダーを選択し、SDK から次のファイルを追加します。 `…\Samples\Apps\HelloProjectOData\HelloProjectODataWeb\Images\NewIcon.png`
    
    または、32 x 32 の独自のアイコンを使用するか、次のイメージを NewIcon.png というファイルにコピーし、そのファイルを  `HelloProjectODataWeb\Images` フォルダーに追加します。
![HelloProjectOData アプリのアイコン](../../images/pj15_HelloProjectData_NewIcon.jpg)


    
5. HelloProjectOData.xml マニフェストで、 **Description** 要素の下に **IconUrl** 要素を追加します。ただし、アイコン URL の値は 32 x 32 アイコン ファイルへの相対パスです。たとえば、次の行を追加します。
    
  ```XML
  <IconUrl DefaultValue="~remoteAppUrl/Images/NewIcon.png" />
  ```

これで HelloProjectOData.xml マニフェスト ファイルの内容は次のようになります (実際の  **Id** は、これとは異なるでしょう)。

```XML
<?xml version="1.0" encoding="UTF-8"?>
<OfficeApp xmlns="http://schemas.microsoft.com/office/appforoffice/1.1" 
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="TaskPaneApp">
  <Id>c512df8d-a1c5-4d74-8a34-d30f6bbcbd82 </Id>
  <Version>1.0</Version>
  <ProviderName> [Provider name]</ProviderName>
  <DefaultLocale>en-US</DefaultLocale>
  <DisplayName DefaultValue="Hello ProjectData" />
  <Description DefaultValue="Test REST queries of the ProjectData service"/>
  <IconUrl DefaultValue="~remoteAppUrl/Images/NewIcon.png" />

  <Hosts>
    <Host Name="Project" />
  </Hosts>
  <DefaultSettings>
    <SourceLocation DefaultValue="~remoteAppUrl/AddIn/Home/Home.html" />
  </DefaultSettings>
  <Permissions>ReadWriteDocument</Permissions>
</OfficeApp>
```


## HelloProjectOData アドインの HTML コンテンツを作成する


 **HelloProjectOData** アドインは、デバッグとエラー出力を含むサンプルです。運用環境で使用することを想定したものではありません。HTML コンテンツのコーディングを始める前に、このアドインの UI とユーザー エクスペリエンスを設計してください。また、HTML コードとやり取りする JavaScript 関数の概略も記述してください。詳細については、「[Office アドインの設計ガイドライン](../add-in-design.md)」を参照してください。 

作業ウィンドウには、上部にアドインの表示名が表示されます。これはマニフェストの  **DisplayName** 要素の値です。HelloProjectOData.html ファイルの **body** 要素には、次のような他の UI 要素が含まれています。


- サブタイトルは、 **ODATA REST QUERY** など、操作の一般的な機能または種類を表します。
    
- [ **ProjectData エンドポイントを取得**] ボタンは、 **setOdataUrl** 関数を呼び出して、 **ProjectData** サービスのエンドポイントを取得し、それをテキスト ボックスに表示します。Project が Project Web App と接続されていない場合、アドインはエラー ハンドラーを呼び出して、ポップアップ エラー メッセージを表示します。
    
- [ **すべてのプロジェクトを比較**] ボタンは、アドインが有効な OData エンドポイントを取得するまで無効化されています。このボタンを選択すると、 **retrieveOData** 関数が呼び出されます。この関数は REST クエリを使用して、 **ProjectData** サービスからプロジェクトのコストと作業のデータを取得します。
    
- テーブルには、プロジェクト コスト、実績コスト、作業、および達成率の平均値が表示されます。このテーブルでは、現在アクティブなプロジェクトの値と平均値との比較も行われます。現在の値が全プロジェクトの平均値より大きい場合は、その値が赤で表示されます。現在の値が平均値より小さい場合は、その値が緑で表示されます。現在の値がない場合は、青の  **NA** が表示されます。
    
     **retrieveOData** 関数は **parseODataResult** 関数を呼び出し、それによってテーブルの値を計算して表示します。
    
     >**メモ**  この例では、アクティブなプロジェクトのコストと作業のデータは発行された値から導出されます。Project で値を変更した場合、 **ProjectData** サービスは、そのプロジェクトが発行されるまで変更を反映しません。

### 手順 4. HTML コンテンツを作成するには


1. Home.html ファイルの  **head** 要素内に、アドインで使用する CSS ファイルの **link** 要素を追加します。Visual Studio プロジェクト テンプレートには、カスタム CSS スタイルに使用できる App.css ファイルのリンクが含まれています。
    
2. アドインで使用する JavaScript ライブラリの  **script** 要素を追加します。プロジェクト テンプレートには、[ **Scripts**] フォルダー内の jQuery-[バージョン].js、office.js、MicrosoftAjax.js の各ファイルのリンクが含まれています。
    
     >**メモ**  アドインを展開する前に、office.js の参照と jQuery の参照をコンテンツ配信ネットワーク (CDN) の参照に変更してください。CDN の参照は最新のバージョンと高いパフォーマンスを提供します。

     **HelloProjectOData** アドインでは、SurfaceErrors.js ファイルも使用します。このファイルは、ポップアップ メッセージでエラーを表示するためのものです。「 [テキスト エディターを使用して Project 2013 用の作業ウィンドウ アドインを初めて作成する](../../docs/project/create-your-first-task-pane-add-in-for-project-by-using-a-text-editor.md)」の「 _堅牢なプログラミング_ 」セクションからコードをコピーし、 **HelloProjectODataWeb** プロジェクトの **Scripts\Office** フォルダーに SurfaceErrors.js ファイルを追加してください。
    
    以下に示すのは、 **head** 要素の更新された HTML コードです。SurfaceErrors.js ファイルの行が追加されています。
    


  ```HTML
  <!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8" />
  <meta http-equiv="X-UA-Compatible" content="IE=Edge" />
  <title>Test ProjectData Service</title>

  <link rel="stylesheet" type="text/css" href="../Content/Office.css" />

  <!-- Add your CSS styles to the following file -->
  <link rel="stylesheet" type="text/css" href="../Content/App.css" />

  <!-- Use the CDN reference to the mini-version of jQuery when deploying your add-in. -->
  <!--<script src="http://ajax.aspnetcdn.com/ajax/jquery/jquery-1.9.0.min.js"></script> -->
  <script src="../Scripts/jquery-1.7.1.js"></script>

  <!-- Use the CDN reference to office.js when deploying your add-in. -->
  <!--<script src="https://appsforoffice.microsoft.com/lib/1.0/hosted/office.js"></script>-->

  <!-- Use the local script references for Office.js to enable offline debugging -->
  <script src="../Scripts/Office/1.0/MicrosoftAjax.js"></script>
  <script src="../Scripts/Office/1.0/Office.js"></script>

  <!-- Add your JavaScript to the following files -->
  <script src="../Scripts/HelloProjectOData.js"></script>
  <script src="../Scripts/SurfaceErrors.js"></script>
</head>
<body>
<!-- See the code in Step 3. -->
</body>
</html>
  ```

3.  **body** 要素内で、テンプレートから既存のコードを削除し、ユーザー インターフェイスのコードを追加します。要素にデータを入れるか、要素を jQuery ステートメントで操作する場合、その要素に一意の **id** 属性が含まれている必要があります。次のコードでは、jQuery の関数で使用する **button** 要素、 **span** 要素、および **td** (テーブル セル定義) 要素の **id** 属性を太字で示しています。
    
    次の HTML はグラフィック イメージを追加します。このグラフィック イメージとしては、社名のロゴなどが考えられます。好みのロゴを使用することもできますが、Project 2013 SDK ダウンロードの NewLogo.png ファイルをコピーしてから、 **ソリューション エクスプローラーで**、そのファイルを  `HelloProjectODataWeb\Images` フォルダーに追加することもできます。
    


  ```HTML
  <body>
  <div id="SectionContent">
    <div id="odataQueries">
      ODATA REST QUERY
    </div>
    <div id="odataInfo">
      <button class="button-wide" onclick="setOdataUrl()">Get ProjectData Endpoint</button>
      <br /><br />
      <span class="rest" id="projectDataEndPoint">Endpoint of the 
        <strong>ProjectData</strong> service</span>
      <br />
    </div>
    <div id="compareProjectData">
      <button class="button-wide" disabled="disabled" id="compareProjects"
        onclick="retrieveOData()">Compare All Projects</button>
      <br />
    </div>
  </div>
  <div id="corpInfo">
    <table class="infoTable" aria-readonly="True" style="width: 100%;">
      <tr>
        <td class="heading_leftCol"></td>
        <td class="heading_midCol"><strong>Average</strong></td>
        <td class="heading_rightCol"><strong>Current</strong></td>
      </tr>
      <tr>
        <td class="row_leftCol"><strong>Project Cost</strong></td>
        <td class="row_midCol" id="AverageProjectCost">&amp;nbsp;</td>
        <td class="row_rightCol" id="CurrentProjectCost">&amp;nbsp;</td>
      </tr>
      <tr>
        <td class="row_leftCol"><strong>Project Actual Cost</strong></td>
        <td class="row_midCol" id="AverageProjectActualCost">&amp;nbsp;</td>
        <td class="row_rightCol" id="CurrentProjectActualCost">&amp;nbsp;</td>
      </tr>
      <tr>
        <td class="row_leftCol"><strong>Project Work</strong></td>
        <td class="row_midCol" id="AverageProjectWork">&amp;nbsp;</td>
        <td class="row_rightCol" id="CurrentProjectWork">&amp;nbsp;</td>
      </tr>
      <tr>
        <td class="row_leftCol"><strong>Project % Complete</strong></td>
        <td class="row_midCol" id="AverageProjectPercentComplete">&amp;nbsp;</td>
        <td class="row_rightCol" id="CurrentProjectPercentComplete">&amp;nbsp;</td>
      </tr>
    </table>
  </div>
  <img alt="Corporation" class="logo" src="../Images/NewLogo.png" />
  <br />
  <textarea id="odataText" rows="12" cols="40"></textarea>
</body>
  ```


## このアドインの JavaScript コードを作成する


Project 作業ウィンドウ アドイン用のテンプレートには、既定の初期化コードが含まれています。このコードは、典型的な Office 2013 アドイン用にドキュメント内のデータに対する基本的な取得操作と設定操作を例示するものとして設計されています。Project 2013 はアクティブなプロジェクトへの書き込み操作をサポートしておらず、 **HelloProjectOData** アドインは **getSelectedDataAsync** メソッドを使用しないので、 **Office.initialize** 関数内のスクリプトを削除できます。また、既定の HelloProjectOData.js ファイルに含まれている **setData** 関数と **getData** 関数も削除できます。

JavaScript には、REST クエリ用のグローバル定数と、いくつかの関数で使用されるグローバル変数が含まれています。[ **ProjectData エンドポイントを取得**] ボタンによって呼び出される  **setOdataUrl** 関数は、グローバル変数を初期化し、Project が Project Web App と接続されているかどうかを調べます。

HelloProjectOData.js ファイルには、 **retrieveOData** 関数と **parseODataResult** 関数も含まれています。前者の関数は、ユーザーが [ **すべてのプロジェクトを比較**] を選択したときに呼び出されます。後者の関数は、平均値を計算し、色と単位について書式設定された値を比較テーブルに内に設定します。


### 手順 5. JavaScript コードを作成するには


1. 既定の HelloProjectOData.js ファイル内のコードをすべて削除し、グローバル変数と  **Office.initialize** 関数を追加します。すべて大文字の変数名は、それらが定数であることを示しており、それらは後で **_pwa** 変数と共に使用されて、この例の REST クエリが作成されます。
    
  ```
  var PROJDATA = "/_api/ProjectData";
var PROJQUERY = "/Projects?";
var QUERY_FILTER = "$filter=ProjectName ne 'Timesheet Administrative Work Items'";
var QUERY_SELECT1 = "&amp;$select=ProjectId, ProjectName";
var QUERY_SELECT2 = ", ProjectCost, ProjectWork, ProjectPercentCompleted, ProjectActualCost";
var _pwa;           // URL of Project Web App.
var _projectUid;    // GUID of the active project.
var _docUrl;        // Path of the project document.
var _odataUrl = ""; // URL of the OData service: http[s]://ServerName /ProjectServerName /_api/ProjectData

// The initialize function is required for all add-ins.
Office.initialize = function (reason) {
    // Checks for the DOM to load using the jQuery ready function.
    $(document).ready(function () {
        // After the DOM is loaded, app-specific code can run.
    });
}


  ```

2.  **setOdataUrl** 関数および関連する関数を追加します。 **setOdataUrl** 関数は **getProjectGuid** と **getDocumentUrl** を呼び出して、グローバル変数を初期化します。 [getProjectFieldAsync メソッド](http://msdn.microsoft.com/ja-jp/library/d35b19ff-c403-4db1-8239-7dd67ee29d72%28Office.15%29.aspx)では、 _callback_ パラメーター用の匿名関数が、jQuery ライブラリ内の **removeAttr** メソッドを使用して [ **すべてのプロジェクトを比較**] ボタンを有効にし、 **ProjectData** サービスの URL を表示します。Project が Project Web App と接続されていない場合、この関数はエラーをスローし、それによってポップアップ エラー メッセージが表示されます。SurfaceErrors.js ファイルには、 **throwError** メソッドが含まれています。
    
     >**メモ**  Visual Studio を Project Server コンピューター上で実行する場合、 **F5** デバッグを使用するには、 **_pwa** グローバル変数を初期化する行の後にあるコードのコメントを解除します。Project Server コンピューター上でのデバッグ時に jQuery の **ajax** メソッドを使用できるようにするには、PWA URL の **localhost** 値を設定する必要があります。Visual Studio をリモート コンピューター上で実行する場合、 **localhost** URL は必要ありません。アドインを展開する前に、そのコードをコメント アウトします。

  ```
  function setOdataUrl() {
    Office.context.document.getProjectFieldAsync(
        Office.ProjectProjectFields.ProjectServerUrl,
        function (asyncResult) {
            if (asyncResult.status == Office.AsyncResultStatus.Succeeded) {
                _pwa = String(asyncResult.value.fieldValue);

                // If you debug with Visual Studio on a local Project Server computer, 
                // uncomment the following lines to use the localhost URL.
                //var localhost = location.host.split(":", 1);
                //var pwaStartPosition = _pwa.lastIndexOf("/");
                //var pwaLength = _pwa.length - pwaStartPosition;
                //var pwaName = _pwa.substr(pwaStartPosition, pwaLength);
                //_pwa = location.protocol + "//" + localhost + pwaName;

                if (_pwa.substring(0, 4) == "http") {
                    _odataUrl = _pwa + PROJDATA;
                    $("#compareProjects").removeAttr("disabled");
                    getProjectGuid();
                }
                else {
                    _odataUrl = "No connection!";
                    throwError(_odataUrl, "You are not connected to Project Web App.");
                }
                getDocumentUrl();
                $("#projectDataEndPoint").text(_odataUrl);
            }
            else {
                throwError(asyncResult.error.name, asyncResult.error.message);
            }
        }
    );
}

// Get the GUID of the active project.
function getProjectGuid() {
    Office.context.document.getProjectFieldAsync(
        Office.ProjectProjectFields.GUID,
        function (asyncResult) {
            if (asyncResult.status == Office.AsyncResultStatus.Succeeded) {
                _projectUid = asyncResult.value.fieldValue;
            }
            else {
                throwError(asyncResult.error.name, asyncResult.error.message);
            }
        }
    );
}

// Get the path of the project in Project web app, which is in the form <>\ProjectName .
function getDocumentUrl() {
    _docUrl = "Document path:\r\n" + Office.context.document.url;
}
  ```

3.  **retrieveOData** 関数を追加します。この関数は REST クエリ用に値を連結し、jQuery の **ajax** 関数を呼び出して、要求されたデータを **ProjectData** サービスから取得します。 **support.cors** 変数は、 **ajax** 関数でのクロス オリジン リソース共有 (CORS) を有効にします。 **support.cors** ステートメントがないか、 **false** に設定されていると、 **ajax** 関数は **No transport** エラーを返します。
    
     >**メモ**  次のコードは、Project Server 2013の社内インストールで動作します。Project Online の場合、トークン ベースの認証に OAuth を使用できます。詳細については、「 [Office アドインにおける同一生成元ポリシーの制限への対処](../../docs/develop/addressing-same-origin-policy-limitations.md)」を参照してください。

     **ajax** 呼び出しでは、 _headers_ パラメーターまたは _beforeSend_ パラメーターを使用できます。 _complete_ パラメーターは匿名関数なので、 **retrieveOData** 内の変数と同じスコープ内にあります。 _complete_ パラメーターの関数は、結果を **odataText** コントロール内に表示すると共に、 **parseODataResult** メソッドを呼び出して、JSON 応答を解析し表示します。 _error_ パラメーターは名前付き **getProjectDataErrorHandler** 関数を指定します。この関数は **odataText** コントロールにエラー メッセージを書き込むと共に、 **throwError** メソッドを使用してポップアップ メッセージを表示します。
    


  ```
  /****************************************************************
* Functions to get and parse the Project Server reporting data.
*****************************************************************/

// Get data about all projects on Project Server, 
// by using a REST query with the ajax method in jQuery.
function retrieveOData() {
    var restUrl = _odataUrl + PROJQUERY + QUERY_FILTER + QUERY_SELECT1 + QUERY_SELECT2;
    var accept = "application/json; odata=verbose";
    accept.toLocaleLowerCase();

    // Enable cross-origin scripting (required by jQuery 1.5 and later).
    // This does not work with Project Online.
    $.support.cors = true;

    $.ajax({
        url: restUrl,
        type: "GET",
        contentType: "application/json",
        data: "",      // Empty string for the optional data.
        //headers: { "Accept": accept },
        beforeSend: function (xhr) {
            xhr.setRequestHeader("ACCEPT", accept);
        },
        complete: function (xhr, textStatus) {
            // Create a message to display in the text box.
            var message = "\r\ntextStatus: " + textStatus +
                "\r\nContentType: " + xhr.getResponseHeader("Content-Type") +
                "\r\nStatus: " + xhr.status +
                "\r\nResponseText:\r\n" + xhr.responseText;

            // xhr.responseText is the result from an XmlHttpRequest, which 
            // contains the JSON response from the OData service.
            parseODataResult(xhr.responseText, _projectUid);

            // Write the document name, response header, status, and JSON to the odataText control.
            $("#odataText").text(_docUrl);
            $("#odataText").append("\r\nREST query:\r\n" + restUrl);
            $("#odataText").append(message);

            if (xhr.status != 200 &amp;&amp; xhr.status != 1223 &amp;&amp; xhr.status != 201) {
                $("#odataInfo").append("<div>" + htmlEncode(restUrl) + "</div>");
            }
        },
        error: getProjectDataErrorHandler
    });
}

function getProjectDataErrorHandler(data, errorCode, errorMessage) {
    $("#odataText").text("Error code: " + errorCode + "\r\nError message: \r\n"
      + errorMessage);
    throwError(errorCode, errorMessage);
}
  ```

4.  **parseODataResult** メソッドを追加します。このメソッドは OData サービスからの JSON 応答を逆シリアル化し、処理します。 **parseODataResult** メソッドは、コストと作業のデータの平均値を小数点以下 1 桁または 2 桁の精度まで計算し、適切な色で値を書式化し、単位 ( **$**、 **hrs**、または  **%**) を追加し、指定されたテーブル セルに値を表示します。
    
    アクティブなプロジェクトの GUID が  **ProjectId** 値と一致する場合、 **myProjectIndex** 変数はプロジェクト インデックスに設定されます。アクティブなプロジェクトが Project Server 上に発行されていることを **myProjectIndex** が示している場合は、 **parseODataResult** メソッドがそのプロジェクトのコストと作業のデータを書式化して表示します。アクティブなプロジェクトが発行されていない場合は、アクティブなプロジェクトの値は青の **NA** として表示されます。
    


  ```
  // Calculate the average values of actual cost, cost, work, and percent complete   
// for all projects, and compare with the values for the current project.
function parseODataResult(oDataResult, currentProjectGuid) {
    // Deserialize the JSON string into a JavaScript object.
    var res = Sys.Serialization.JavaScriptSerializer.deserialize(oDataResult);
    var len = res.d.results.length;
    var projActualCost = 0;
    var projCost = 0;
    var projWork = 0;
    var projPercentCompleted = 0;
    var myProjectIndex = -1;

    for (i = 0; i < len; i++) {
        // If the current project GUID matches the GUID from the OData query,  
        // store the project index.
        if (currentProjectGuid.toLocaleLowerCase() == res.d.results[i].ProjectId) {
            myProjectIndex = i;
        }
        projCost += Number(res.d.results[i].ProjectCost);
        projWork += Number(res.d.results[i].ProjectWork);
        projActualCost += Number(res.d.results[i].ProjectActualCost);
        projPercentCompleted += Number(res.d.results[i].ProjectPercentCompleted);

    }
    var avgProjCost = projCost / len;
    var avgProjWork = projWork / len;
    var avgProjActualCost = projActualCost / len;
    var avgProjPercentCompleted = projPercentCompleted / len;

    // Round off cost to two decimal places, and round off other values to one decimal place.
    avgProjCost = avgProjCost.toFixed(2);
    avgProjWork = avgProjWork.toFixed(1);
    avgProjActualCost = avgProjActualCost.toFixed(2);
    avgProjPercentCompleted = avgProjPercentCompleted.toFixed(1);

    // Display averages in the table, with the correct units. 
    document.getElementById("AverageProjectCost").innerHTML = "$"
        + avgProjCost;
    document.getElementById("AverageProjectActualCost").innerHTML
        = "$" + avgProjActualCost;
    document.getElementById("AverageProjectWork").innerHTML
        = avgProjWork + " hrs";
    document.getElementById("AverageProjectPercentComplete").innerHTML
        = avgProjPercentCompleted + "%";

    // Calculate and display values for the current project.
    if (myProjectIndex != -1) {

        var myProjCost = Number(res.d.results[myProjectIndex].ProjectCost);
        var myProjWork = Number(res.d.results[myProjectIndex].ProjectWork);
        var myProjActualCost = Number(res.d.results[myProjectIndex].ProjectActualCost);
        var myProjPercentCompleted =
          Number(res.d.results[myProjectIndex].ProjectPercentCompleted);

        myProjCost = myProjCost.toFixed(2);
        myProjWork = myProjWork.toFixed(1);
        myProjActualCost = myProjActualCost.toFixed(2);
        myProjPercentCompleted = myProjPercentCompleted.toFixed(1);

        document.getElementById("CurrentProjectCost").innerHTML = "$" + myProjCost;

        if (Number(myProjCost) <= Number(avgProjCost)) {
            document.getElementById("CurrentProjectCost").style.color = "green"
        }
        else {
            document.getElementById("CurrentProjectCost").style.color = "red"
        }

        document.getElementById("CurrentProjectActualCost").innerHTML = "$" + myProjActualCost;

        if (Number(myProjActualCost) <= Number(avgProjActualCost)) {
            document.getElementById("CurrentProjectActualCost").style.color = "green"
        }
        else {
            document.getElementById("CurrentProjectActualCost").style.color = "red"
        }

        document.getElementById("CurrentProjectWork").innerHTML = myProjWork + " hrs";

        if (Number(myProjWork) <= Number(avgProjWork)) {
            document.getElementById("CurrentProjectWork").style.color = "red"
        }
        else {
            document.getElementById("CurrentProjectWork").style.color = "green"
        }

        document.getElementById("CurrentProjectPercentComplete").innerHTML = myProjPercentCompleted + "%";

        if (Number(myProjPercentCompleted) <= Number(avgProjPercentCompleted)) {
            document.getElementById("CurrentProjectPercentComplete").style.color = "red"
        }
        else {
            document.getElementById("CurrentProjectPercentComplete").style.color = "green"
        }
    }
    else {
        document.getElementById("CurrentProjectCost").innerHTML = "NA";
        document.getElementById("CurrentProjectCost").style.color = "blue"

        document.getElementById("CurrentProjectActualCost").innerHTML = "NA";
        document.getElementById("CurrentProjectActualCost").style.color = "blue"

        document.getElementById("CurrentProjectWork").innerHTML = "NA";
        document.getElementById("CurrentProjectWork").style.color = "blue"

        document.getElementById("CurrentProjectPercentComplete").innerHTML = "NA";
        document.getElementById("CurrentProjectPercentComplete").style.color = "blue"
    }
}
  ```


## HelloProjectOData アドインのテスト


 **HelloProjectOData** アドインを Visual Studio 2015 でテストしてデバッグするには、開発用コンピューターに Project Professional 2013がインストールされている必要があります。他のテスト シナリオを有効にするには、Project がローカル コンピューター上のファイルに対して開くか、Project Web App と接続するかを選択できることを確認してください。たとえば、次の手順を実行します。


1. リボンの [ **ファイル**] タブで、Backstage ビューの [ **情報**] タブを選択し、[ **アカウントの管理**] を選択します。
    
2. [ **Project Web App アカウント**] ダイアログ ボックスの [ **利用可能なアカウント**] の一覧には、ローカル  **コンピューター** アカウントだけでなく、複数の Project Web App アカウントが表示されることがあります。[ **開始時**] セクションで、[ **アカウントを選択する**] を選択します。
    
3. Project を閉じます。それにより、Visual Studio でアドインのデバッグ用に Project 起動できるようになります。
    
基本的なテストでは、次のことを行う必要があります。


- アドインを Visual Studio から実行し、コストと作業のデータが含まれている Project Web App から発行済みのプロジェクトを開きます。アドインが  **ProjectData** エンドポイントを表示し、コストと作業のデータをテーブルに正しく表示することを確認します。 **odataText** コントロール内の出力により、REST クエリとその他の情報を確認できます。
    
- アドインを再び実行し、Project の起動時に [ **ログイン**] ダイアログ ボックスでローカル コンピューターのプロファイルを選択します。ローカル .mpp ファイルを開き、アドインをテストします。 **ProjectData** エンドポイントを取得しようとしたときに、アドインがエラー メッセージを表示することを確認します。
    
- アドインを再び実行し、コストと作業のデータのタスクを持つプロジェクトを作成します。そのプロジェクトを Project Web App に保存することはできますが、発行はしないでください。アドインが Project Server からのデータを表示するものの、現在のプロジェクトについては  **NA** であることを確認します。
    

### 手順 6. アドインをテストするには


1. Project Professional 2013 を実行し、Project Web App と接続してから、テスト プロジェクトを作成します。ローカル リソースまたはエンタープライズ リソースにタスクを割り当て、いくつかのタスクに対して達成率のさまざまな値を設定してから、そのプロジェクトを発行します。Project を終了します。それにより、Visual Studio がアドインのデバッグ用に Project を起動できるようになります。
    
2. Visual Studio で  **F5** キーを押します。Project Web App にログオンし、前のステップで作成したプロジェクトを開きます。このとき、読み取り専用モードで開くことも、編集モードで開くこともできます。
    
3. リボンの [ **プロジェクト**] タブの [ **Office アドイン**] ドロップダウン リストで、[ **Hello ProjectData**] を選択します (図 4 を参照)。 [ **すべてのプロジェクトを比較**] ボタンが無効化されるはずです。
    
    **図 4. HelloProjectOData アドインの起動**

    ![HelloProjectOData アプリのテスト](../../images/pj15_HelloProjectData_TestTheApp.png)

4. [ **ProjectData 概要**] 作業ウィンドウで、[ **ProjectData エンドポイントを取得**] を選択します。 **projectDataEndPoint** 行に **ProjectData** サービスの URL が表示され、[ **すべてのプロジェクトを比較**] ボタンが有効化されるはずです (図 5 を参照)。
    
5. [ **すべてのプロジェクトを比較**] を選択します。アドインは、 **ProjectData** サービスからデータを取得している間、一時停止する可能性がありますが、その後、書式化された平均値と現在値をテーブルに表示します。
    
    **図 5. REST クエリの結果の表示**

    ![REST クエリの結果の表示](../../images/pj15_HelloProjectData_RESTresults.gif)

6. テキスト ボックス内の出力を調べます。ここには、ドキュメントのパス、REST クエリ、ステータス情報、および  **ajax** と **parseODataResult** の呼び出しからの JSON 結果が表示されるはずです。この出力は、 **parseODataResult** メソッドのコード ( `projCost += Number(res.d.results[i].ProjectCost);` など) の理解と作成とデバッグに役立ちます。
    
    以下に示すのは、Project Web App インスタンス内の 3 つのプロジェクトについての出力の例で、読みやすいようにテキストに改行とスペースを追加してあります。
    


  ```
  Document path: <>\WinProj test1

REST query:
http://sphvm-37189/pwa/_api/ProjectData/Projects?$filter=ProjectName ne 'Timesheet Administrative Work Items'
    &amp;$select=ProjectId, ProjectName, ProjectCost, ProjectWork, ProjectPercentCompleted, ProjectActualCost

textStatus: success
ContentType: application/json;odata=verbose;CHARSET=shift-jis
Status: 200

ResponseText:
{"d":{"results":[
{"__metadata":
    {"id":"http://sphvm-37189/pwa/_api/ProjectData/Projects(guid'ce3d0d65-3904-e211-96cd-00155d157123')",
    "uri":"http://sphvm-37189/pwa/_api/ProjectData/Projects(guid'ce3d0d65-3904-e211-96cd-00155d157123')",
    "type":"ReportingData.Project"},
    "ProjectId":"ce3d0d65-3904-e211-96cd-00155d157123",
    "ProjectActualCost":"0.000000",
    "ProjectCost":"0.000000",
    "ProjectName":"Task list created in PWA",
    "ProjectPercentCompleted":0,
    "ProjectWork":"16.000000"},
{"__metadata":
    {"id":"http://sphvm-37189/pwa/_api/ProjectData/Projects(guid'c31023fc-1404-e211-86b2-3c075433b7bd')",
    "uri":"http://sphvm-37189/pwa/_api/ProjectData/Projects(guid'c31023fc-1404-e211-86b2-3c075433b7bd')",
    "type":"ReportingData.Project"},
    "ProjectId":"c31023fc-1404-e211-86b2-3c075433b7bd",
    "ProjectActualCost":"700.000000",
    "ProjectCost":"2400.000000",
    "ProjectName":"WinProj test 2",
    "ProjectPercentCompleted":29,
    "ProjectWork":"48.000000"},
{"__metadata":
    {"id":"http://sphvm-37189/pwa/_api/ProjectData/Projects(guid'dc81fbb2-b801-e211-9d2a-3c075433b7bd')",
    "uri":"http://sphvm-37189/pwa/_api/ProjectData/Projects(guid'dc81fbb2-b801-e211-9d2a-3c075433b7bd')",
    "type":"ReportingData.Project"},
    "ProjectId":"dc81fbb2-b801-e211-9d2a-3c075433b7bd",
    "ProjectActualCost":"1900.000000",
    "ProjectCost":"5200.000000",
    "ProjectName":"WinProj test1",
    "ProjectPercentCompleted":37,
    "ProjectWork":"104.000000"}
]}}
  ```

7. デバッグを停止し ( **Shift + F5** キーを押す)、 **F5** キーを再び押して、Project の新しいインスタンスを実行します。[ **ログイン**] ダイアログ ボックスで、Project Web App ではなく、ローカル  **コンピューター** のプロファイルを選択します。ローカル プロジェクト .mpp ファイルを作成するか開き、[ **ProjectData 概要**] 作業ウィンドウを開き、[ **ProjectData エンドポイントを取得**] を選択します。アドインが  **接続がありません!** エラーを表示し (図 6 を参照)、 [ **すべてのプロジェクトを比較**] ボタンが無効化されたままになるはずです。
    
    **図 6. Project Web App 接続がない状態でのアドインの使用**

    ![Project Web App 接続がない状態でのアプリの使用](../../images/pj15_HelloProjectData_NoConnection.gif)

8. デバッグを停止してから、 **F5** キーを再び押します。Project Web App にログオンし、コストと作業のデータが含まれるプロジェクトを作成します。このプロジェクトを保存することはできますが、発行しないでください。
    
    [ **ProjectData 概要**] 作業ウィンドウで [ **すべてのプロジェクトを比較**] を選択すると、[ **現在**] 列のフィールドには青の  **NA** が表示されるはずです (図 7 を参照)。
    

    **図 7. 未発行のプロジェクトと他のプロジェクトの比較**

    ![未発行のプロジェクトと他のプロジェクトの比較](../../images/pj15_HelloProjectData_NotPublished.gif)

アドインがこれまでのテストで正常に動作しているとしても、実行する必要のあるテストはまだあります。たとえば、次のことを行います。

- Project Web App からタスクのコストと作業のデータがないプロジェクトを開きます。 [ **現在**] 列のフィールドには 0 の値が表示されるはずです。
    
- タスクがないプロジェクトをテストします。
    
- アドインに変更を加えて発行した場合は、発行したアドインで再び同様のテストを実行する必要があります。その他の考慮事項については、「[次のステップ](#次のステップ)」を参照してください。
    

 >**メモ**   **ProjectData** サービスの 1 回のクエリで返すことのできるデータ量には制限があります。このデータ量はエンティティによって異なります。たとえば、 **Projects** エンティティ セットでの既定の制限は 1 回のクエリで 100 プロジェクトですが、 **Risks** エンティティ セットでの既定の制限は 200 プロジェクトです。運用インストールでは、 **HelloProjectOData** のコード例に変更を加えて、100 プロジェクト以上のクエリを使用できるようにする必要があります。詳細については、「 [次のステップ](#次のステップ)」および「 [Project Server 2013 レポート データの OData フィードにクエリを実行する](http://msdn.microsoft.com/library/3eafda3b-f006-48be-baa6-961b2ed9fe01%28Office.15%29.aspx)」を参照してください。


## HelloProjectOData アドインのコード例


 **HelloProjectOData.html ファイル**???次のコードは、 **HelloProjectODataWeb** プロジェクトの `Pages\HelloProjectOData.html` ファイルに収められています。


```HTML
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8" />
  <meta http-equiv="X-UA-Compatible" content="IE=Edge" />
  <title>Test ProjectData Service</title>

  <link rel="stylesheet" type="text/css" href="../Content/Office.css" />

  <!-- Add your CSS styles to the following file -->
  <link rel="stylesheet" type="text/css" href="../Content/App.css" />

  <!-- Use the CDN reference to the mini-version of jQuery when deploying your add-in. -->
  <!--<script src="http://ajax.aspnetcdn.com/ajax/jquery/jquery-1.9.0.min.js"></script> -->
  <script src="../Scripts/jquery-1.7.1.js"></script>

  <!-- Use the CDN reference to Office.js when deploying your add-in -->
  <!--<script src="https://appsforoffice.microsoft.com/lib/1.0/hosted/office.js"></script>-->

  <!-- Use the local script references for Office.js to enable offline debugging -->
  <script src="../Scripts/Office/1.0/MicrosoftAjax.js"></script>
  <script src="../Scripts/Office/1.0/Office.js"></script>

  <!-- Add your JavaScript to the following files -->
  <script src="../Scripts/HelloProjectOData.js"></script>
  <script src="../Scripts/SurfaceErrors.js"></script>
</head>
<body>
  <div id="SectionContent">
    <div id="odataQueries">
      ODATA REST QUERY
    </div>
    <div id="odataInfo">
      <button class="button-wide" onclick="setOdataUrl()">Get ProjectData Endpoint</button>
      <br />
      <br />
      <span class="rest" id="projectDataEndPoint">Endpoint of the 
        <strong>ProjectData</strong> service</span>
      <br />
    </div>
    <div id="compareProjectData">
      <button class="button-wide" disabled="disabled" id="compareProjects"
        onclick="retrieveOData()">
        Compare All Projects</button>
      <br />
    </div>
  </div>
  <div id="corpInfo">
    <table class="infoTable" aria-readonly="True" style="width: 100%;">
      <tr>
        <td class="heading_leftCol"></td>
        <td class="heading_midCol"><strong>Average</strong></td>
        <td class="heading_rightCol"><strong>Current</strong></td>
      </tr>
      <tr>
        <td class="row_leftCol"><strong>Project Cost</strong></td>
        <td class="row_midCol" id="AverageProjectCost">&amp;nbsp;</td>
        <td class="row_rightCol" id="CurrentProjectCost">&amp;nbsp;</td>
      </tr>
      <tr>
        <td class="row_leftCol"><strong>Project Actual Cost</strong></td>
        <td class="row_midCol" id="AverageProjectActualCost">&amp;nbsp;</td>
        <td class="row_rightCol" id="CurrentProjectActualCost">&amp;nbsp;</td>
      </tr>
      <tr>
        <td class="row_leftCol"><strong>Project Work</strong></td>
        <td class="row_midCol" id="AverageProjectWork">&amp;nbsp;</td>
        <td class="row_rightCol" id="CurrentProjectWork">&amp;nbsp;</td>
      </tr>
      <tr>
        <td class="row_leftCol"><strong>Project % Complete</strong></td>
        <td class="row_midCol" id="AverageProjectPercentComplete">&amp;nbsp;</td>
        <td class="row_rightCol" id="CurrentProjectPercentComplete">&amp;nbsp;</td>
      </tr>
    </table>
  </div>
  <img alt="Corporation" class="logo" src="../Images/NewLogo.png" />
  <br />
  <textarea id="odataText" rows="12" cols="40"></textarea>
</body>
</html>
```

 **HelloProjectOData.js ファイル**???次のコードは、 **HelloProjectODataWeb** プロジェクトの `Scripts\Office\HelloProjectOData.js` ファイルに収められています。




```
/* File: HelloProjectOData.js
* JavaScript functions for the HelloProjectOData example task pane app.
* October 2, 2012
*/

var PROJDATA = "/_api/ProjectData";
var PROJQUERY = "/Projects?";
var QUERY_FILTER = "$filter=ProjectName ne 'Timesheet Administrative Work Items'";
var QUERY_SELECT1 = "&amp;$select=ProjectId, ProjectName";
var QUERY_SELECT2 = ", ProjectCost, ProjectWork, ProjectPercentCompleted, ProjectActualCost";
var _pwa;           // URL of Project Web App.
var _projectUid;    // GUID of the active project.
var _docUrl;        // Path of the project document.
var _odataUrl = ""; // URL of the OData service: http[s]://ServerName /ProjectServerName /_api/ProjectData

// The initialize function is required for all add-ins.
Office.initialize = function (reason) {
    // Checks for the DOM to load using the jQuery ready function.
    $(document).ready(function () {
        // After the DOM is loaded, app-specific code can run.
    });
}

// Set the global variables, enable the Compare All Projects button,
// and display the URL of the ProjectData service.
// Display an error if Project is not connected with Project Web App.
function setOdataUrl() {
    Office.context.document.getProjectFieldAsync(
        Office.ProjectProjectFields.ProjectServerUrl,
        function (asyncResult) {
            if (asyncResult.status == Office.AsyncResultStatus.Succeeded) {
                _pwa = String(asyncResult.value.fieldValue);

                // If you debug with Visual Studio on a local Project Server computer, 
                // uncomment the following lines to use the localhost URL.
                //var localhost = location.host.split(":", 1);
                //var pwaStartPosition = _pwa.lastIndexOf("/");
                //var pwaLength = _pwa.length - pwaStartPosition;
                //var pwaName = _pwa.substr(pwaStartPosition, pwaLength);
                //_pwa = location.protocol + "//" + localhost + pwaName;

                if (_pwa.substring(0, 4) == "http") {
                    _odataUrl = _pwa + PROJDATA;
                    $("#compareProjects").removeAttr("disabled");
                    getProjectGuid();
                }
                else {
                    _odataUrl = "No connection!";
                    throwError(_odataUrl, "You are not connected to Project Web App.");
                }
                getDocumentUrl();
                $("#projectDataEndPoint").text(_odataUrl);
            }
            else {
                throwError(asyncResult.error.name, asyncResult.error.message);
            }
        }
    );
}

// Get the GUID of the active project.
function getProjectGuid() {
    Office.context.document.getProjectFieldAsync(
        Office.ProjectProjectFields.GUID,
        function (asyncResult) {
            if (asyncResult.status == Office.AsyncResultStatus.Succeeded) {
                _projectUid = asyncResult.value.fieldValue;
            }
            else {
                throwError(asyncResult.error.name, asyncResult.error.message);
            }
        }
    );
}

// Get the path of the project in Project web app, which is in the form <>\ProjectName .
function getDocumentUrl() {
    _docUrl = "Document path:\r\n" + Office.context.document.url;
}

/****************************************************************
* Functions to get and parse the Project Server reporting data.
*****************************************************************/

// Get data about all projects on Project Server, 
// by using a REST query with the ajax method in jQuery.
function retrieveOData() {
    var restUrl = _odataUrl + PROJQUERY + QUERY_FILTER + QUERY_SELECT1 + QUERY_SELECT2;
    var accept = "application/json; odata=verbose";
    accept.toLocaleLowerCase();

    // Enable cross-origin scripting (required by jQuery 1.5 and later).
    // This does not work with Project Online.
    $.support.cors = true;

    $.ajax({
        url: restUrl,
        type: "GET",
        contentType: "application/json",
        data: "",      // Empty string for the optional data.
        //headers: { "Accept": accept },
        beforeSend: function (xhr) {
            xhr.setRequestHeader("ACCEPT", accept);
        },
        complete: function (xhr, textStatus) {
            // Create a message to display in the text box.
            var message = "\r\ntextStatus: " + textStatus +
                "\r\nContentType: " + xhr.getResponseHeader("Content-Type") +
                "\r\nStatus: " + xhr.status +
                "\r\nResponseText:\r\n" + xhr.responseText;

            // xhr.responseText is the result from an XmlHttpRequest, which 
            // contains the JSON response from the OData service.
            parseODataResult(xhr.responseText, _projectUid);

            // Write the document name, response header, status, and JSON to the odataText control.
            $("#odataText").text(_docUrl);
            $("#odataText").append("\r\nREST query:\r\n" + restUrl);
            $("#odataText").append(message);

            if (xhr.status != 200 &amp;&amp; xhr.status != 1223 &amp;&amp; xhr.status != 201) {
                $("#odataInfo").append("<div>" + htmlEncode(restUrl) + "</div>");
            }
        },
        error: getProjectDataErrorHandler
    });
}

function getProjectDataErrorHandler(data, errorCode, errorMessage) {
    $("#odataText").text("Error code: " + errorCode + "\r\nError message: \r\n"
      + errorMessage);
    throwError(errorCode, errorMessage);
}

// Calculate the average values of actual cost, cost, work, and percent complete   
// for all projects, and compare with the values for the current project.
function parseODataResult(oDataResult, currentProjectGuid) {
    // Deserialize the JSON string into a JavaScript object.
    var res = Sys.Serialization.JavaScriptSerializer.deserialize(oDataResult);
    var len = res.d.results.length;
    var projActualCost = 0;
    var projCost = 0;
    var projWork = 0;
    var projPercentCompleted = 0;
    var myProjectIndex = -1;

    for (i = 0; i < len; i++) {
        // If the current project GUID matches the GUID from the OData query,  
        // then store the project index.
        if (currentProjectGuid.toLocaleLowerCase() == res.d.results[i].ProjectId) {
            myProjectIndex = i;
        }
        projCost += Number(res.d.results[i].ProjectCost);
        projWork += Number(res.d.results[i].ProjectWork);
        projActualCost += Number(res.d.results[i].ProjectActualCost);
        projPercentCompleted += Number(res.d.results[i].ProjectPercentCompleted);

    }
    var avgProjCost = projCost / len;
    var avgProjWork = projWork / len;
    var avgProjActualCost = projActualCost / len;
    var avgProjPercentCompleted = projPercentCompleted / len;

    // Round off cost to two decimal places, and round off other values to one decimal place.
    avgProjCost = avgProjCost.toFixed(2);
    avgProjWork = avgProjWork.toFixed(1);
    avgProjActualCost = avgProjActualCost.toFixed(2);
    avgProjPercentCompleted = avgProjPercentCompleted.toFixed(1);

    // Display averages in the table, with the correct units. 
    document.getElementById("AverageProjectCost").innerHTML = "$"
        + avgProjCost;
    document.getElementById("AverageProjectActualCost").innerHTML
        = "$" + avgProjActualCost;
    document.getElementById("AverageProjectWork").innerHTML
        = avgProjWork + " hrs";
    document.getElementById("AverageProjectPercentComplete").innerHTML
        = avgProjPercentCompleted + "%";

    // Calculate and display values for the current project.
    if (myProjectIndex != -1) {

        var myProjCost = Number(res.d.results[myProjectIndex].ProjectCost);
        var myProjWork = Number(res.d.results[myProjectIndex].ProjectWork);
        var myProjActualCost = Number(res.d.results[myProjectIndex].ProjectActualCost);
        var myProjPercentCompleted = Number(res.d.results[myProjectIndex].ProjectPercentCompleted);

        myProjCost = myProjCost.toFixed(2);
        myProjWork = myProjWork.toFixed(1);
        myProjActualCost = myProjActualCost.toFixed(2);
        myProjPercentCompleted = myProjPercentCompleted.toFixed(1);

        document.getElementById("CurrentProjectCost").innerHTML = "$" + myProjCost;

        if (Number(myProjCost) <= Number(avgProjCost)) {
            document.getElementById("CurrentProjectCost").style.color = "green"
        }
        else {
            document.getElementById("CurrentProjectCost").style.color = "red"
        }

        document.getElementById("CurrentProjectActualCost").innerHTML = "$" + myProjActualCost;

        if (Number(myProjActualCost) <= Number(avgProjActualCost)) {
            document.getElementById("CurrentProjectActualCost").style.color = "green"
        }
        else {
            document.getElementById("CurrentProjectActualCost").style.color = "red"
        }

        document.getElementById("CurrentProjectWork").innerHTML = myProjWork + " hrs";

        if (Number(myProjWork) <= Number(avgProjWork)) {
            document.getElementById("CurrentProjectWork").style.color = "red"
        }
        else {
            document.getElementById("CurrentProjectWork").style.color = "green"
        }

        document.getElementById("CurrentProjectPercentComplete").innerHTML = myProjPercentCompleted + "%";

        if (Number(myProjPercentCompleted) <= Number(avgProjPercentCompleted)) {
            document.getElementById("CurrentProjectPercentComplete").style.color = "red"
        }
        else {
            document.getElementById("CurrentProjectPercentComplete").style.color = "green"
        }
    }
    else {    // The current project is not published.
        document.getElementById("CurrentProjectCost").innerHTML = "NA";
        document.getElementById("CurrentProjectCost").style.color = "blue"

        document.getElementById("CurrentProjectActualCost").innerHTML = "NA";
        document.getElementById("CurrentProjectActualCost").style.color = "blue"

        document.getElementById("CurrentProjectWork").innerHTML = "NA";
        document.getElementById("CurrentProjectWork").style.color = "blue"

        document.getElementById("CurrentProjectPercentComplete").innerHTML = "NA";
        document.getElementById("CurrentProjectPercentComplete").style.color = "blue"
    }
}
```

 **App.css ファイル**???次のコードは、 **HelloProjectODataWeb** プロジェクトの `Content\App.css` ファイルに収められています。




```
/*
*  File: App.css for the HelloProjectOData app.
*  Updated: 10/2/2012
*/
 
body
{
    font-size: 11pt;
}
h1 
{
    font-size: 22pt;
}
h2 
{
    font-size: 16pt;
}

/******************************************************************
Code label class
******************************************************************/

.rest 
{
    font-family: 'Courier New';
    font-size: 0.9em;
}

/******************************************************************
Button classes
******************************************************************/

.button-wide {
    width: 210px;
    margin-top: 2px;
}
.button-narrow 
{
    width: 80px;
    margin-top: 2px;
}

/******************************************************************
Table styles
******************************************************************/

.infoTable
{
    text-align: center; 
    vertical-align: middle
}
.heading_leftCol
{
    width: 20px;
    height: 20px;
}
.heading_midCol
{
    width: 100px;
    height: 20px;
    font-size: medium; 
    font-weight: bold; 
}
.heading_rightCol
{
    width: 101px;
    height: 20px;
    font-size: medium; 
    font-weight: bold; 
}
.row_leftCol
{
    width: 20px;
    font-size: small; 
    font-weight: bold; 
}
.row_midCol
{
    width: 100px;
}
.row_rightCol
{
    width: 101px;
}
.logo
{
    width: 135px;
    height: 53px;
}
```

 **SurfaceErrors.js ファイル**???SurfaceErrors.js ファイルのコードは、「 [テキスト エディターを使用して Project 2013 用の作業ウィンドウ アドインを初めて作成する](../../docs/project/create-your-first-task-pane-add-in-for-project-by-using-a-text-editor.md)」の「 _堅牢なプログラミング_ 」セクションからコピーできます。


## 次のステップ


 **HelloProjectOData** が Office ストアで販売したり、SharePoint アドイン カタログで配布したりする製品アドインであれば、その設計はまた違ったものになるでしょう。たとえば、テキスト ボックスのデバッグ出力はないでしょうし、 **ProjectData** エンドポイントを取得するためのボタンもおそらくないでしょう。また、100 個以上のプロジェクトを持つ Project Web App インスタンスを処理するために **retireveOData** 関数を書き直す必要があるでしょう。

このアドインには、追加のエラー チェックと、エッジ ケースをキャッチして説明または表示するためのロジックを組み込む必要があります。たとえば、Project Web App インスタンスに、平均期間が 5 日で平均コストが $2400 になる 1000 個のプロジェクトがあって、期間が 20 日より長いのはアクティブ プロジェクトだけだとすると、コストと作業の比較は歪んだものになるでしょう。それは頻度グラフで示すことができます。期間を表示したり、同じような長さのプロジェクトを比較したり、同じ部門または異なる部門のプロジェクトを比較したりするオプションを追加するとよいでしょう。あるいは、表示するフィールドのリストからユーザーが選択できるような方法を追加することもできます。

 **ProjectData** サービスの他のクエリについては、クエリ文字列の長さに制限があり、これは親コレクションから子コレクションのオブジェクトまでにクエリが取ることのできるステップ数に影響します。たとえば、 **Projects** から、 **Tasks**、タスク アイテムへという 2 ステップのクエリはうまく動作しますが、 **Projects** から、 **Tasks**、 **Assignments**、割り当てアイテムへという 3 ステップのクエリになると、URL の既定の最大長を超える可能性があります。詳細については、「 [Project Server 2013 レポート データの OData フィードにクエリを実行する](http://msdn.microsoft.com/library/3eafda3b-f006-48be-baa6-961b2ed9fe01%28Office.15%29.aspx)」を参照してください。

 **HelloProjectOData** アドインを運用環境で使用できるように変更する場合は、次の手順を実行してください。


- HelloProjectOData.html ファイルで、パフォーマンスを向上させるために、office.js の参照をローカル プロジェクトから CDN の参照に変更します。
    
  ```HTML
  <script src="https://appsforoffice.microsoft.com/lib/1.0/hosted/office.js"></script>
  ```

-  **retrieveOData** 関数を書き換えて、100 プロジェクト以上のクエリを使用できるようにします。たとえば、 `~/ProjectData/Projects()/$count` クエリでプロジェクトの数を取得し、プロジェクト データの REST クエリで _$skip_ 操作と _$top_ 操作を使用します。ループの中で複数のクエリを実行し、各クエリからのデータを平均します。プロジェクト データの各クエリは、 `~/ProjectData/Projects()?skip= [numSkipped]&amp;$top=100&amp;$filter=[filter]&amp;$select=[field1,field2, …]` といった形式になります。
    
    詳細については、「[OData System Query Options Using the REST Endpoint](http://msdn.microsoft.com/library/8a938b9b-7fdb-45a3-a04c-4d2d5cf2e353.aspx)」を参照してください。また、Windows PowerShell で [Set-SPProjectOdataConfiguration](http://technet.microsoft.com/library/jj219516%28v=office.15%29.aspx) コマンドを使用して、 **Projects** エンティティ セットの (または 33 個のエンティティ セットのいずれについても) クエリの既定のページ サイズをオーバーライドすることもできます。「 [ProjectData - Project 2013 OData サービス参照](http://msdn.microsoft.com/library/1ed14ee9-1a1a-4960-9b66-c24ef92cdf6b%28Office.15%29.aspx)」を参照してください。
    
- アドインを展開するには、「 [Office アドインを発行する](../publish/publish.md)」を参照してください。
    

## その他の技術情報



- [Project 用の作業ウィンドウ アドイン](../project/project-add-ins.md)
    
- [テキスト エディターを使用して Project 2013 用の作業ウィンドウ アドインを初めて作成する](../../docs/project/create-your-first-task-pane-add-in-for-project-by-using-a-text-editor.md)
    
- [ProjectData - Project 2013 OData サービス参照](http://msdn.microsoft.com/library/1ed14ee9-1a1a-4960-9b66-c24ef92cdf6b%28Office.15%29.aspx)
    
- [Office アドインの XML マニフェスト](../../docs/overview/add-in-manifests.md)
    
- [Office アドインを発行する](../publish/publish.md)
    
