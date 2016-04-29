
# テキスト エディターを使用して Project 2013 用の作業ウィンドウ アドインを初めて作成する
共有で HTML ファイルを参照する XML マニフェストを使用してアドインの Project オブジェクト モデルをテストする作業ウィンドウを作成する Office アドインを開発します。

 _ **適用対象:** apps for Office?| Office Add-ins?| Project_

Visual Studio 2015 を使用して複雑な Web アプリケーションを作成したり、テキスト エディターを使用してローカル アドイン用のファイルを作成したりすることで、Project Standard 2013または Project Professional 2013 用の作業ウィンドウ アドインを作成できます。この記事では、ファイル共有上の HTML ファイルを参照する XML マニフェストを使用するシンプルなアドインの作成方法について説明します。Project OM Test サンプル アドインは、アドイン用のオブジェクト モデルを使用するいくつかの JavaScript 関数をテストします。Project 2013の **セキュリティ センター**を使用して、マニフェスト ファイルを含むファイル共有を登録した後は、作業ウィンドウ アドインをリボンの [ **プロジェクト**] タブから開くことができます (この記事のサンプル コードは、Microsoft Corporation の Arvind Iyer によるテスト アプリケーションに基づくものです)。

Project 2013では、他の Microsoft Office 2013 クライアントと同じアドイン マニフェスト スキーマを使用し、JavaScript API も同じものを多数使用します。この記事で説明するアドインの完全なコードは、Project 2013 SDK ダウンロードの  `Samples\Apps` サブディレクトリ内にあります。

Project OM Test サンプル アドインは、タスクの GUID や、アプリケーションとアクティブなプロジェクトのプロパティを取得できます。Project Professional 2013で SharePoint ライブラリ内にあるプロジェクトを開いた場合、このアドインはそのプロジェクトの URL を表示できます。 [Project 2013 SDK のダウンロード](https://www.microsoft.com/en-us/download/details.aspx?id=30435)には完全なソース コードが含まれています。Project2013SDK.msi に含まれる SDK を展開してインストールしたら、 `\Samples\Apps\Copy_to_AppManifests_FileShare` サブディレクトリにマニフェスト ファイルが、 `\Samples\Apps\Copy_to_AppSource_FileShare` サブディレクトリにソース コードがあることを確認します。サンプルの JSOMCall.html では、インクルードされる office.js ファイルと project-15.js ファイル内の JavaScript 関数を使用しています。対応するデバッグ ファイル (office.debug.js および project-15.debug.js) を使用すると、これらの関数を検証できます。
Office アドインでの JavaScript の使用の概要については、「 [JavaScript API for Office について](../develop/understanding-the-javascript-api-for-office.md)」を参照してください。

## 手順 1. アドイン マニフェスト ファイルを作成するには



- ローカル ディレクトリに XML ファイルを作成します。この XML ファイルには、 **OfficeApp** 要素と子要素が含まれます。これらの要素については、「 [Office アドインの XML マニフェスト](../../docs/overview/add-in-manifests.md)」を参照してください。たとえば、次の XML を含む JSOM_SimpleOMCalls.xml というファイルを作成します ( **Id** 要素の GUID 値を変更します)。
    
  ```XML
  <?xml version="1.0" encoding="utf-8"?>
<OfficeApp xmlns="http://schemas.microsoft.com/office/appforoffice/1.1" 
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
           xsi:type="TaskPaneApp">
  <Id>93A26520-9414-492F-994B-4983A1C7A607</Id>
  <Version>15.0</Version>
  <ProviderName>Microsoft</ProviderName>
  <DefaultLocale>en-us</DefaultLocale>
  <DisplayName DefaultValue="Project OM Test">
    <Override Locale="fr-fr" Value="Le Project OM Test"/>
  </DisplayName>
  <Description DefaultValue="Test the task pane add-in object model for Project - English (US)">
    <Override Locale="fr-fr" Value="Test the task pane add-in object model for Project - French (France)"/>
  </Description>
  <Hosts>
    <Host Name="Project"/>
    <Host Name="Workbook"/>
    <Host Name="Document"/>
  </Hosts>
  <DefaultSettings>
    <SourceLocation DefaultValue="\\ServerName\AppSource\JSOMCall.html">
      <Override Locale="fr-fr" Value="\\ServerName\AppSource\JSOMCall.html"/>
    </SourceLocation>
  </DefaultSettings>
  <Permissions>ReadWriteDocument</Permissions>
  <IconUrl DefaultValue="http://officeimg.vo.msecnd.net/_layouts/images/general/office_logo.jpg">
    <Override Locale="fr-fr" Value="http://officeimg.vo.msecnd.net/_layouts/images/general/office_logo.jpg"/>
  </IconUrl>
  <AllowSnapshot>true</AllowSnapshot>
</OfficeApp>
  ```


    Project の場合、 **OfficeApp** 要素に `xsi:type="TaskPaneApp"` 属性値が含まれている必要があります。 **Id** 要素は GUID です。 **SourceLocation** 値はファイル共有パスであるか、あるいはアドインの HTML ソース ファイルまたは作業ウィンドウで実行される Web アプリケーションの SharePoint URL である必要があります。マニフェスト ファイル内のその他の要素については、「 [Project 用の作業ウィンドウ アドイン](../project/project-add-ins.md)」を参照してください。
    
手順 2. では、JSOM_SimpleOMCalls.xml マニフェストが Project テスト アドインのために指定する HTML ファイルの作成方法を示します。この HTML 内で指定されているボタンは、関連する JavaScript 関数を呼び出します。JavaScript 関数は、この HTML ファイル内に追加したり、別の .js ファイル内に配置したりできます。

## 手順 2. Project OM Test アドインのソース ファイルを作成するには



1. JSOM_SimpleOMCalls.xml マニフェスト内の  **SourceLocation** 要素によって指定されている名前の HTML ファイルを作成します。たとえば、JSOMCall.html というファイルを `C:\Project\AppSource` ディレクトリ内に作成します。簡単なテキスト エディターを使用してソース ファイルを作成することもできますが、Visual Studio 2015 などのツールを使用した方が簡単です。このツールは、特定のドキュメント タイプ (HTML、JavaScript など) に対応し、その他の編集支援機能も備えています。「 [Project 用の作業ウィンドウ アドイン](../project/project-add-ins.md)」の説明にある Bing Search サンプルをまだ実行していない場合は、手順 3. に記されている、マニフェストで指定する  `\\ServerName\AppSource` ファイル共有の作成方法を参照してください。
    
    JSOMCall.html ファイルでは、AJAX の機能については共通の MicrosoftAjax.js ファイルを、Microsoft Office 2013 アプリケーション内のアドイン機能については Office.js ファイルを使用しています。
    


  ```HTML
  <!DOCTYPE html>
<html>
<head>
    <title>Project OM Sample Code</title>
    <meta http-equiv="X-UA-Compatible" content="IE=Edge" />
    <script type="text/javascript" src="MicrosoftAjax.js"></script>

    <!-- Use the CDN reference to office.js when deploying your add-in. -->
    <!-- <script src="https://appsforoffice.microsoft.com/lib/1/hosted/Office.js"></script> -->
    <script type="text/javascript" src="Office.js"></script>
    <script type="text/javascript" src="JSOM_Sample.js"></script>
</head>
<body>
    <div id="Common_JSOM_API">
        OBJECT MODEL TESTS
    </div>

    <textarea id="text" rows="6" cols="25">This is the text result.</textarea>
</body>
</html>
  ```


     **textarea** 要素は、JavaScript 関数の結果を表示するテキスト ボックスを指定しています。
    
     >**メモ**  Project OM Test サンプルを実行するには、Project 2013 SDK ダウンロードに含まれるファイル (Office.js、Project-15.js、および MicrosoftAjax.js) を JSOMCall.html ファイルと同じディレクトリにコピーします。

    手順 2. では、Project OM Test サンプル アドインが使用する特定の関数のために JSOM Sample.js というファイルを追加します。以降の手順では、JavaScript 関数を呼び出すボタン用にその他の HTML 要素を追加します。
    
2. JSOM_Sample.js という名前の JavaScript ファイルを、JSOMCall.html ファイルと同じディレクトリ内に作成します。次のコードは、Office.js ファイル内の関数を使用して、アプリケーションのコンテキストとドキュメント情報を取得します。 **text** オブジェクトは、HTML ファイル内にある **textarea** コントロールの ID です。
    
     **_projDoc** 変数は、 **ProjectDocument** オブジェクトによって初期化されています。このコードには、いくつかの簡単なエラー処理関数と、アプリケーション コンテキストやプロジェクト ドキュメント コンテキストのプロパティを取得する **getContextValues** 関数が含まれています。Project の JavaScript オブジェクト モデルの詳細については、「 [JavaScript API for Office](http://msdn.microsoft.com/library/b27e70c3-d87d-4d27-85e0-103996273298%28Office.15%29.aspx)」を参照してください。
    


  ```
  /*
* JavaScript functions for the Project OM Test example app
* in the Project 2013 SDK.
*/

var _projDoc;
var _app;
var taskGuid;
var resourceGuid;

// The initialize function is required for all add-ins.
Office.initialize = function (reason) {
    // Checks for the DOM to load using the jQuery ready function.
    $(document).ready(function () {
        // After the DOM is loaded, app-specific code can run.
        _projDoc = Office.context.document;
        _app = Office.context;
    });
}

function logError(errorText) {
    text.value = "Error in " + errorText;
}

function logEventError(erroneousEvent) {
    logError("event " + erroneousEvent);
}

function logMethodError(methodName, errorName, errorMessage) {
    logError(methodName + " method.\nError name: " + errorName + "\nMessage: " + errorMessage);
}

// . . . Add other JavaScript functions here.

function getContextValues() {
    getDocumentUrl();
    getDocumentMode();
    getApplicationContentLanguage();
    getApplicationDisplayLanguage();
}

function getDocumentUrl() {
    text.value ="Document URL:\n" +_projDoc.url;
}

function getDocumentMode() {
    var docMode = _projDoc.mode;
    text.value = text.value + "\n\nDocument mode: " + docMode;
}

function getApplicationContentLanguage() {
    text.value = text.value + "\nApp language: " + _app.contentLanguage;
}

function getApplicationDisplayLanguage() {
    text.value = text.value + "\nDisplay language: " + _app.displayLanguage;
}
  ```


    Office.debug.js ファイル内の関数については、「 [JavaScript API for Office](http://msdn.microsoft.com/library/b27e70c3-d87d-4d27-85e0-103996273298%28Office.15%29.aspx)」を参照してください。たとえば、 **getDocumentUrl** 関数は、開かれているプロジェクトの URL またはファイル パスを取得します。
    
3. Office.js および Project-15.js 内の非同期関数を呼び出して選択されているデータを取得する JavaScript 関数を追加します。
    
      - たとえば、 **getSelectedDataAsync** は、選択されているデータの書式設定されていないテキストを取得する、Office.js 内の汎用関数です。詳細については、「 [AsyncResult オブジェクト](http://msdn.microsoft.com/ja-jp/library/540c114f-0398-425c-baf3-7363f2f6bc47%28Office.15%29.aspx)」を参照してください。
    
  - Project-15.js 内の  **getSelectedTaskAsync** 関数は、選択されているタスクの GUID を取得します。同様に、 **getSelectedResourceAsync** 関数は、選択されているリソースの GUID を取得します。タスクまたはリソースが選択されていない状態でこれらの関数を呼び出すと、未定義のエラーが発生します。
    
  -  **getTaskAsync** 関数は、タスク名と、割り当てられているリソースの名前を取得します。タスクが同期された SharePoint タスク リストである場合、 **getTaskAsync** は SharePoint リスト内のタスク ID を取得します。それ以外の場合は、SharePoint タスク ID が 0 になります。
    
     >**メモ**  サンプル コードには、デモ用にバグが含まれています。 **taskGuid** は未定義なので、 **getTaskAsync** 関数はエラーによって終了します。有効なタスク GUID を取得した後に異なるタスクを選択した場合、 **getTaskAsync** 関数は、操作された直近のタスクのデータを **getSelectedTaskAsync** 関数によって取得します。
  -  **getTaskFields**、 **getResourceFields**、および  **getProjectFields** は、それぞれ **getTaskFieldAsync**、 **getResourceFieldAsync**、または  **getProjectFieldAsync** を複数回呼び出して、タスクまたはリソースの指定フィールドを取得するローカル関数です。project-15.debug.js ファイルには、 **ProjectTaskFields** 列挙型と **ProjectResourceFields** 列挙型がサポートするフィールドが示されています。
    
  -  **getSelectedViewAsync** 関数は、ビューの種類 (project-15.debug.js 内の **ProjectViewTypes** 列挙型で定義されています) とビューの名前を取得します。
    
  - プロジェクトが SharePoint タスク リストと同期されている場合、 **getWSSUrlAsync** 関数はそのタスク リストの URL と名前を取得します。プロジェクトが SharePoint タスク リストと同期されていない場合、 **getWSSUrlAsync** 関数はエラーによって終了します。
    
     >**メモ**  タスク リストの SharePoint URL と名前を取得するには、 **getProjectFieldAsync** 関数で [ProjectProjectFields](http://msdn.microsoft.com/ja-jp/library/d0808b99-9940-478d-88b7-f68061cc77c5%28Office.15%29.aspx) 列挙型の定数 **WSSUrl** と **WSSList** を使用することをお勧めします。

    次のコード内の各関数には、 `function (asyncResult)` によって指定された匿名関数が含まれています。これは、非同期の結果を取得するコールバックです。匿名関数の代わりに名前付き関数を使用することもでき、その結果、複雑なアドインの保守容易性が向上する場合があります。
    


  ```
  // Get the data in the selected cells of the grid in the active view.
function getSelectedDataAsync() {
    _projDoc.getSelectedDataAsync(
        Office.CoercionType.Text,
        { ValueFormat: "Formatted" },
        function (asyncResult) {
            if (asyncResult.status == Office.AsyncResultStatus.Succeeded)
                text.value = asyncResult.value;
            else
                logMethodError("getSelectedDataAsync", asyncResult.error.name,
                               asyncResult.error.message);
        }
    );
}

// Get the GUID of the selected task.
function getSelectedTaskAsync() {
    _projDoc.getSelectedTaskAsync(function (asyncResult) {
        if (asyncResult.status == Office.AsyncResultStatus.Succeeded) {
            text.value = asyncResult.value;
            taskGuid = asyncResult.value;
        }
        else {
            logMethodError("getSelectedTaskAsync", asyncResult.error.name,
                               asyncResult.error.message);
        }
    });
}

// Get the GUID of the selected resource.
function getSelectedResourceAsync() {
    _projDoc.getSelectedResourceAsync(function (asyncResult) {
        if (asyncResult.status == Office.AsyncResultStatus.Succeeded) {
            text.value = asyncResult.value;
            resourceGuid = asyncResult.value;
        }
        else {
            logMethodError("getSelectedResourceAsync", asyncResult.error.name,
                               asyncResult.error.message);
        }
    });
}

// Get data for the specified task.
function getTaskAsync() {
    if (taskGuid != undefined) {
        _projDoc.getTaskAsync(
            taskGuid,
            function (asyncResult) {
                if (asyncResult.status === Office.AsyncResultStatus.Failed) {
                    logMethodError("getTaskAsync", asyncResult.error.name,
                               asyncResult.error.message);
                } else {
                    var taskInfo = asyncResult.value;
                    var taskOutput = "Task name: " + taskInfo.taskName +
                                     "\nGUID: " + taskGuid +
                                     "\nWSS Id: " + taskInfo.wssTaskId +
                                     "\nResourceNames: " + taskInfo.resourceNames;
                    text.value = taskOutput;
                }
            }
        );
    } else {
        text.value = 'Task GUID not valid:\n' + taskGuid;
    } 
}

// Get additional data for task fields.
function getTaskFields() {
    text.value = "";

    _projDoc. getTaskFieldAsync(taskGuid, Office.ProjectTaskFields.Name,
        function (asyncResult) {
            if (asyncResult.status == Office.AsyncResultStatus.Succeeded) {
                text.value = text.value + "Name: "
                    + asyncResult.value.fieldValue + "\n";
            }
            else {
                logMethodError("getTaskFieldAsync", asyncResult.error.name,
                               asyncResult.error.message);
            }
        }
    );

    _projDoc.getTaskFieldAsync(taskGuid, Office.ProjectTaskFields.ID,
        function (asyncResult) {
            if (asyncResult.status == Office.AsyncResultStatus.Succeeded) {
                text.value = text.value + "ID: "
                    + asyncResult.value.fieldValue + "\n";
            }
            else {
                logMethodError("getTaskFieldAsync", asyncResult.error.name,
                               asyncResult.error.message);
            }
        }
    );

    _projDoc.getTaskFieldAsync(taskGuid, Office.ProjectTaskFields.Start,
        function (asyncResult) {
            if (asyncResult.status == Office.AsyncResultStatus.Succeeded) {
                text.value = text.value + "Start: "
                    + asyncResult.value.fieldValue + "\n";
            }
            else {
                logMethodError("getTaskFieldAsync", asyncResult.error.name,
                               asyncResult.error.message);
            }
        }
    );

    _projDoc.getTaskFieldAsync(taskGuid, Office.ProjectTaskFields.Duration,
        function (asyncResult) {
            if (asyncResult.status == Office.AsyncResultStatus.Succeeded) {
                text.value = text.value + "Duration: "
                    + asyncResult.value.fieldValue + "\n";
            }
            else {
                logMethodError("getTaskFieldAsync", asyncResult.error.name,
                               asyncResult.error.message);
            }
        }
    );

    _projDoc.getTaskFieldAsync(taskGuid, Office.ProjectTaskFields.Priority,
        function (asyncResult) {
            if (asyncResult.status == Office.AsyncResultStatus.Succeeded) {
                text.value = text.value + "Priority: "
                    + asyncResult.value.fieldValue + "\n";
            }
            else {
                logMethodError("getTaskFieldAsync", asyncResult.error.name,
                               asyncResult.error.message);
            }
        }
    );

    _projDoc.getTaskFieldAsync(taskGuid, Office.ProjectTaskFields.Notes,
        function (asyncResult) {
            if (asyncResult.status == Office.AsyncResultStatus.Succeeded) {
                text.value = text.value + "Notes: "
                    + asyncResult.value.fieldValue + "\n";
            }
            else {
                logMethodError("getTaskFieldAsync", asyncResult.error.name,
                               asyncResult.error.message);
            }
        }
    ); 
}

// Get data for the specified resource fields.
function getResourceFields() {
    text.value = "";

    _projDoc.getResourceFieldAsync(resourceGuid, Office.ProjectResourceFields.Name,
        function (asyncResult) {
            if (asyncResult.status == Office.AsyncResultStatus.Succeeded) {
                text.value = text.value + "Resource name: " + asyncResult.value.fieldValue + "\n";
            }
            else {
                logMethodError("getResourceFieldAsync", asyncResult.error.name,
                               asyncResult.error.message);
            }
        }
    );

    _projDoc.getResourceFieldAsync(resourceGuid, Office.ProjectResourceFields.Cost,
        function (asyncResult) {
            if (asyncResult.status == Office.AsyncResultStatus.Succeeded) {
                text.value = text.value + "Cost: " + asyncResult.value.fieldValue + "\n";
            }
            else {
                logMethodError("getResourceFieldAsync", asyncResult.error.name,
                               asyncResult.error.message);
            }
        }
    );

    _projDoc.getResourceFieldAsync(resourceGuid, Office.ProjectResourceFields.StandardRate,
        function (asyncResult) {
            if (asyncResult.status == Office.AsyncResultStatus.Succeeded) {
                text.value = text.value + "Standard Rate: " + asyncResult.value.fieldValue + "\n";
            }
            else {
                logMethodError("getResourceFieldAsync", asyncResult.error.name, asyncResult.error.message);
            }
        }
    );

    _projDoc.getResourceFieldAsync(resourceGuid, Office.ProjectResourceFields.ActualCost,
        function (asyncResult) {
            if (asyncResult.status == Office.AsyncResultStatus.Succeeded) {
                text.value = text.value + "Actual Cost: " + asyncResult.value.fieldValue + "\n";
            }
            else {
                logMethodError("getResourceFieldAsync", asyncResult.error.name, asyncResult.error.message);
            }
        }
    );

    _projDoc.getResourceFieldAsync(resourceGuid, Office.ProjectResourceFields.ActualWork,
        function (asyncResult) {
            if (asyncResult.status == Office.AsyncResultStatus.Succeeded) {
                text.value = text.value + "Actual Work: " + asyncResult.value.fieldValue + "\n";
            }
            else {
                logMethodError("getResourceFieldAsync", asyncResult.error.name,
                               asyncResult.error.message);
            }
        }
    );

    _projDoc.getResourceFieldAsync(resourceGuid, Office.ProjectResourceFields.Units,
        function (asyncResult) {
            if (asyncResult.status == Office.AsyncResultStatus.Succeeded) {
                text.value = text.value + "Units: " + asyncResult.value.fieldValue + "\n";
            }
            else {
                logMethodError("getResourceFieldAsync", asyncResult.error.name,
                               asyncResult.error.message);
            }
        }
    );
}

// Get the URL and list name of the synchronized SharePoint task list.
// Recommended: use getProjectField instead.
function getWSSUrlAsync() {
    _projDoc.getWSSUrlAsync(function (asyncResult) {
        if (asyncResult.status == Office.AsyncResultStatus.Succeeded) {
            text.value = "SharePoint URL:\n" + asyncResult.value.serverUrl
                + "\nList name: " + asyncResult.value.listName;
        }
        else {
            logMethodError("getWSSUrlAsync", asyncResult.error.name, asyncResult.error.message);
        }
    });
}

// Get the type and name of the selected view.
function getSelectedViewAsync() {
    _projDoc.getSelectedViewAsync(function (asyncResult) {
        text.value = "View type: " + asyncResult.value.viewType
            + "\nName: " + asyncResult.value.viewName;
    });
}

// Get information about the active project.
function getProjectFields() {
    text.value = "";

    _projDoc.getProjectFieldAsync(Office.ProjectProjectFields.GUID,
        function (asyncResult) {
            if (asyncResult.status == Office.AsyncResultStatus.Succeeded) {
                text.value = text.value + "Project GUID: " + asyncResult.value.fieldValue + "\n";
            }
            else {
                logMethodError("getProjectFieldAsync", asyncResult.error.name, asyncResult.error.message);
            }
        }
    );

    _projDoc.getProjectFieldAsync(Office.ProjectProjectFields.Start,
        function (asyncResult) {
            if (asyncResult.status == Office.AsyncResultStatus.Succeeded) {
                text.value = text.value + "\nStart: " + asyncResult.value.fieldValue + "\n";
            }
            else {
                logMethodError("getProjectFieldAsync", asyncResult.error.name, asyncResult.error.message);
            }
        }
    );

    _projDoc.getProjectFieldAsync(Office.ProjectProjectFields.Finish,
        function (asyncResult) {
            if (asyncResult.status == Office.AsyncResultStatus.Succeeded) {
                text.value = text.value + "\nFinish: " + asyncResult.value.fieldValue + "\n";
            }
            else {
                logMethodError("getProject " + errorText);
            }
        }
    );

    _projDoc.getProjectFieldAsync(Office.ProjectProjectFields.CurrencyDigits,
        function (asyncResult) {
            if (asyncResult.status == Office.AsyncResultStatus.Succeeded) {
                text.value = text.value + "\nCurrency digits: " + asyncResult.value.fieldValue + "\n";
            }
            else {
                logMethodError("getProjectFieldAsync", asyncResult.error.name, asyncResult.error.message);
            }
        }
    );


    _projDoc.getProjectFieldAsync(Office.ProjectProjectFields.CurrencySymbol,
        function (asyncResult) {
            if (asyncResult.status == Office.AsyncResultStatus.Succeeded) {
                text.value = text.value + "Currency symbol: " + asyncResult.value.fieldValue + "\n";
            }
            else {
                logMethodError("getProjectFieldAsync", asyncResult.error.name, asyncResult.error.message);
            }
        }
    );

    _projDoc.getProjectFieldAsync(Office.ProjectProjectFields.CurrencySymbolPosition,
        function (asyncResult) {
            if (asyncResult.status == Office.AsyncResultStatus.Succeeded) {
                text.value = text.value + "\nSymbol position: " + asyncResult.value.fieldValue + "\n";
            }
            else {
                logMethodError("getProjectFieldAsync", asyncResult.error.name, asyncResult.error.message);
            }
        }
    );

    _projDoc.getProjectFieldAsync(Office.ProjectProjectFields.ProjectServerUrl,
        function (asyncResult) {
            if (asyncResult.status == Office.AsyncResultStatus.Succeeded) {
                text.value = text.value + "\nProject web app URL:\n  " + asyncResult.value.fieldValue + "\n";
            }
            else {
                logMethodError("getProjectFieldAsync", asyncResult.error.name, asyncResult.error.message);
            }
        }
    );

    _projDoc.getProjectFieldAsync(Office.ProjectProjectFields.WSSUrl,
        function (asyncResult) {
            if (asyncResult.status == Office.AsyncResultStatus.Succeeded) {
                text.value = text.value + "\nSharePoint URL:\n  " + asyncResult.value.fieldValue + "\n";
            }
            else {
                logMethodError("getProjectFieldAsync", asyncResult.error.name, asyncResult.error.message);
            }
        }
    );

    _projDoc.getProjectFieldAsync(Office.ProjectProjectFields.WSSList,
        function (asyncResult) {
            if (asyncResult.status == Office.AsyncResultStatus.Succeeded) {
                text.value = text.value + "\nSharePoint list: " + asyncResult.value.fieldValue + "\n";
            }
            else {
                logMethodError("getProjectFieldAsync", asyncResult.error.name, asyncResult.error.message);
            }
        }
    );
}
  ```

4. JavaScript イベント ハンドラー コールバックおよび関数を追加して、タスク選択、リソース選択、およびビュー選択の変更に関するイベント ハンドラーの登録と登録解除を行います。 **manageEventHandlerAsync** 関数は、 _operation_ パラメーターに応じて、指定されたイベント ハンドラーを追加または削除します。この操作は **addHandlerAsync** または **removeHandlerAsync** のどちらかになります。
    
     **manageTaskEventHandler**、 **manageResourceEventHandler**、および  **manageViewEventHandler** の各関数では、 _docMethod_ パラメーターの指定によって、イベント ハンドラーを追加または削除できます。
    


  ```
  // Task selection changed event handler.
function onTaskSelectionChanged(eventArgs) {
    text.value = "In task selection change event handler";
}

// Resource selection changed event handler.
function onResourceSelectionChanged(eventArgs) {
    text.value = "In Resource selection changed event handler";
}

// View selection changed event handler.
function onViewSelectionChanged(eventArgs) {
    text.value = "In View selection changed event handler";
}

// Add or remove the specified event handler.
function manageEventHandlerAsync(eventType, handler, operation, onComplete) {
    _projDoc[operation]   //The operation is addHandlerAsync or removeHandlerAsync.
    (
        eventType,
        handler,
        function (asyncResult) {
            if (onComplete) {
                onComplete(asyncResult, operation);
            } else {
                var message = "Operation: " + operation;
                message = message + "\nStatus: " + asyncResult.status + "\n";
                text.value = message;
            }
        }
    );
}

// Write the asyncResult status from the manageEventHandlerAsync function (optional). 
function onComplete(asyncResult, operation) {
    var message = "In onComplete function for " + operation;
    message = message + "\nStatus: " + asyncResult.status;
    text.value = message;
}

// Add or remove a task selection changed event handler.
function manageTaskEventHandler(docMethod) {
    manageEventHandlerAsync(
        Office.EventType.TaskSelectionChanged,      // The task selection changed event.
        onTaskSelectionChanged,                     // The event handler.
        docMethod,                // The Office.Document method to add or remove an event handler.
        onComplete                // Manages the successful asyncResult data (optional).
    );
}

// Add or remove a resource selection changed event handler.
function manageResourceEventHandler(docMethod) {
    manageEventHandlerAsync(
        Office.EventType.ResourceSelectionChanged,  // The resource selection changed event.
        onResourceSelectionChanged,                 // The event handler.
        docMethod,                // The Office.Document method to add or remove an event handler.
        onComplete                // Manages the successful asyncResult data (optional).
    );
}

// Add or remove a view selection changed event handler.
function manageViewEventHandler(docMethod) {
    manageEventHandlerAsync(
        Office.EventType.ViewSelectionChanged,      // The view selection changed event.
        onViewSelectionChanged,                     // The event handler.
        docMethod,                // The Office.Document method to add or remove an event handler.
        onComplete                // Manages the successful asyncResult data (optional).
    );
}
  ```

5. この HTML ドキュメントの本文に、テストのために JavaScript 関数を呼び出すボタンを追加します。たとえば、共通の JSOM API の  **div** 要素には、汎用の **getSelectedDataAsync** 関数を呼び出す入力ボタンを追加します。
    
  ```HTML
  <body>
    <div id="Common_JSOM_API">
    OBJECT MODEL TESTS
    <br /><br />       
    <strong>General function:</strong>
    <br />
    <input id="Button5" class="button-wide" type="button" onclick="getSelectedDataAsync()" 
        value="getSelectedDataAsync" />
    </div>
   <!--  more code . . .  -->
  ```

6. プロジェクト特有のタスク関数用のボタンと  **TaskSelectionChanged** イベント用のボタンを備えた **div** セクションを追加します。
    
  ```HTML
  <div id="ProjectSpecificTask">
  <br />
  <strong>Project-specific task methods:</strong><br />
  <button class="button-wide" onclick="getSelectedTaskAsync()">getSelectedTaskAsync</button><br />
  <button class="button-wide" onclick="getTaskAsync()">getTaskAsync</button><br />
  <button class="button-wide" onclick="getTaskFields()">Get Task Fields</button><br />
  <button class="button-wide" onclick="getWSSUrlAsync()">getWSSUrlAsync</button>
  <strong>Task selection changed:</strong>
  <button class="button-narrow" onclick="manageTaskEventHandler('addHandlerAsync')">Add</button>
  <button class="button-narrow" onclick="manageTaskEventHandler('removeHandlerAsync')">Remove</button>         
</div>
  ```

7. リソースに関するメソッドやイベント、ビューに関するメソッドやイベント、プロジェクトのプロパティ、およびコンテキストのプロパティのための各ボタンを備えた  **div** セクションを追加します。
    
  ```HTML
  <div id="ResourceMethods">
  <br />
  <strong>Resource methods:</strong>
  <button class="button-wide" onclick="getSelectedResourceAsync()">getSelectedResourceAsync</button><br />
  <button class="button-wide" onclick="getResourceFields()">Get Resource Fields</button><br />
  <strong>Resource selection changed:</strong>
  <button class="button-narrow" onclick="manageResourceEventHandler('addHandlerAsync')">Add</button>
  <button class="button-narrow" onclick="manageResourceEventHandler('removeHandlerAsync')">Remove</button>
</div>
<div id="ViewMethods">
  <br />
  <strong>View method:</strong>
  <button class="button-wide" onclick="getSelectedViewAsync()">getSelectedViewAsync</button><br />
  <strong>View selection changed:</strong>
  <button class="button-narrow" onclick="manageViewEventHandler('addHandlerAsync')">Add</button>
  <button class="button-narrow" onclick="manageViewEventHandler('removeHandlerAsync')">Remove</button>         
</div>
<div id="ProjectMethods">
  <br />
  <strong>Project properties:</strong>
  <button class="button-wide" onclick="getProjectFields()">Get Project Fields</button><br />
</div>
<div id="ContextVariables">
  <br />
  <strong>Context properties:</strong>
  <button class="button-wide" onclick="getContextValues()">Get Context Values</button>
</div>
  ```

8. ボタンの要素の書式設定を行うために、CSS の  **style** 要素を追加します。たとえば、 **head** 要素の子要素として次の要素を追加します。
    
  ```HTML
  <style type="text/css">
    .button-wide
    {
        width: 210px;
        margin-top: 2px;
    }
    .button-narrow
    {
        width: 80px;
        margin-top: 2px;
    }
</style>
  ```


     >**メモ**  Visual Studio 2015 の [ **作業ウィンドウ アドイン (プロジェクト)**] テンプレートでは、アドインの外観を共通化するために既定の .css ファイルをインクルードしています。
手順 3. では、Project OM Test アドインの機能をインストールして使用する方法を示します。

## 手順 3. Project OM Test アドインをインストールして使用するには



1. JSOM SimpleOMCalls.xml マニフェストが含まれているディレクトリに対するファイル共有を作成します。ファイル共有は、ローカル コンピューター上、またはネットワーク上のアクセス可能なリモート コンピューター上に作成できます。たとえば、このマニフェストがローカル コンピューター上の  `C:\Project\AppManifests` ディレクトリ内にある場合は、次のコマンドを実行します。
    
  ```
  Net share AppManifests=C:\Project\AppManifests
  ```


    詳細については、「 [作業ウィンドウ アドインおよびコンテンツ アドイン用のネットワーク共有フォルダー カタログを作成する](../publish/create-a-network-shared-folder-catalog-for-task-pane-and-content-add-ins.md)」を参照してください。
    
2. Project OM Test アドインの HTML および JavaScript ファイルが含まれるディレクトリに対するファイル共有を作成します。このファイル共有パスは、JSOM SimpleOMCalls.xml マニフェストで指定されているパスに一致するようにしてください。たとえば、このファイルがローカル コンピューター上の  `C:\Project\AppSource` ディレクトリにある場合は、次のコマンドを実行します。
    
  ```
  net share AppSource=C:\Project\AppSource
  ```

3. Project で、 [ **Project のオプション**] ダイアログ ボックスを開き、[ **セキュリティ センター**]、[ **セキュリティ センターの設定**] の順に選択します。
    
    アドインを登録するための詳しい手順については、「[Project 用の作業ウィンドウ アドイン](../project/project-add-ins.md)」を参照してください。
    
4. [ **セキュリティ センター**] ダイアログ ボックスの左側のウィンドウで、[ **信頼されているアドイン カタログ**] を選択します。
    
5. 既に  `\\ServerName\AppManifests` というパスを Bing 検索アドイン用に追加している場合は、この手順をスキップしてください。それ以外の場合は、[ **信頼されているアドイン カタログ**] ウィンドウの [ **カタログの URL**] テキスト ボックスに  `\\ServerName\AppManifests` パスを追加し、[ **カタログの追加**] を選択して、このネットワーク共有を既定のソースとして有効にしてから (図 1 を参照)、 [ **OK**] を選択します。
    
    **図 1. アドイン マニフェスト用のネットワーク ファイル共有の追加**

    ![アプリ マニフェスト用のネットワーク ファイル共有の追加](../../images/pj15_CreateSimpleAgave_ManageCatalogs.png)

6. 新しいアドインを追加するか、ソース コードを変更したら、Project を再起動します。[ **プロジェクト**] リボンで、[ **Office アドイン**] ドロップダウン メニューの [ **すべて表示**] を選択します。[ **アドインの挿入**] ダイアログ ボックスで、[ **共有フォルダー**] を選択し (図 2 を参照)、 [ **Project OM Test**]、[ **挿入**] の順に選択します。Project OM Test アドインが作業ウィンドウ内で起動します。
    
    **図 2. ファイル共有上にある Project OM Test アドインの起動**

    ![アプリの挿入](../../images/pj15_CreateSimpleAgave_StartAgaveApp.png)

7. Project で、少なくとも 2 つのタスクを備えた単純なプロジェクトを作成して保存します。たとえば、T1 とT2 というタスク、およびM1 というマイルストーンを作成し、タスクの期間と先行タスクを図 3 のように設定します。リボンの [ **プロジェクト**] タブを選択し、タスク T2 の行全体を選択して、作業ウィンドウの [ **getSelectedDataAsync**] ボタンを選択します。図 3 に、 **Project OM Test** アドインのテキスト ボックス内で選択されているデータを示します。
    
    **図 3. Project OM Test アドインの使用**

    ![Project OM Test アプリの使用](../../images/pj15_CreateSimpleAgave_ProjectOMTest.gif)

8. 最初のタスクの [ **期間**] 列内にあるセルを選択し、 **Project OM Test** アドイン内の [ **getSelectedDataAsync**] ボタンを選択します。 **getSelectedDataAsync** 関数により、テキスト ボックスの値が `2 days` を示すように設定されます。
    
9. 3 つのタスクすべての [ **期間**] セル (3 つ) を選択します。 **getSelectedDataAsync** 関数により、各行で選択されたセルのセミコロン区切りテキスト値が返されます (例: `2 days;4 days;0 days`)。
    
     **getSelectedDataAsync** 関数は、ある行内で選択されたセルのコンマ区切りテキスト値を返します。たとえば、図 3 ではタスク T2 の行全体が選択されています。ここで [ **getSelectedDataAsync**] を選択すると、テキスト ボックスには " `,Auto Scheduled,T2,4 days,Thu 6/14/12,Tue 6/19/12,1,,<NA>`" と表示されます。
    
    [ **インジケーター**] 列と [ **リソース名**] 列はどちらも空なので、テキスト配列にはこれらの列に対応する空の値が表示されます。 `<NA>` 値は、[ **新しい列の追加**] セルに対応しています。
    
10. タスク T2 の行の任意のセル、またはタスク T2 の行全体を選択し、[ **getSelectedTaskAsync**] を選択します。テキスト ボックスにタスクの GUID 値が表示されます (例:  `{25D3E03B-9A7D-E111-92FC-00155D3BA208}`)。この値は、プロジェクトによって  **Project OM Test** アドインのグローバル変数 **taskGuid** に格納されます。
    
11.  **getTaskAsync** を選択します。 **taskGuid** 変数にタスク T2 の GUID が格納されている場合、テキスト ボックスにはタスク情報が表示されます。 **ResourceNames** 値は空です。
    
    2 つのローカル リソース R1 とR2 を作成し、これらをタスク T2 に 50% ずつ割り当て、再び **getTaskAsync** を選択します。テキスト ボックス内の結果には、このリソース情報が含まれます。同期された SharePoint タスク リスト内にタスクが存在する場合、結果には SharePoint タスク ID も含まれます。
    


  ```
  Task name: T2
GUID: {25D3E03B-9A7D-E111-92FC-00155D3BA208}
WSS Id: 0
ResourceNames: R1[50%],R2[50%]
  ```

12. [ **タスク フィールドの取得**] ボタンを選択します。 **getTaskFields** 関数により、 **getTaskfieldAsync** 関数の呼び出しが複数回行われ、タスク名、インデックス、開始日、期間、優先度、およびタスク ノートが取得されます。
    
  ```
  Name: T2
ID: 2
Start: Thu 6/14/12
Duration: 4d
Priority: 500
Notes: This is a note for task T2. It is only a test note. If it had been a real note, there would be some real information.
  ```

13. [ **getWSSUrlAsync**] ボタンを選択します。プロジェクトが次の種類のどちらかであれば、タスク リストの URL と名前が結果に表示されます。 
    
      - Project Server にインポートされた SharePoint タスク リスト
    
  - Project Professional にインポートされ、SharePoint に (Project Server を使用せずに) 保存された SharePoint タスク リスト
    
     >**メモ**  Project Professional が Windows Server コンピューターにインストールされており、プロジェクトを SharePoint に保存できる場合は、 **サーバー マネージャー**を使用して **デスクトップ エクスペリエンス**機能を追加できます。

    プロジェクトがローカル プロジェクトであるか、Project Server で管理されるプロジェクトを開くために Project Professional を使用した場合は、 **getWSSUrlAsync** メソッドから未定義のエラーが生成されます。
    


  ```
  SharePoint URL: http://ServerName
List name: Test task list
  ```

14. [ **TaskSelectionChanged イベント**] セクションの [ **追加**] ボタンを選択します。このボタンを選択すると、 **manageTaskEventHandler** 関数の呼び出しにより、タスク選択の変更イベントが登録され、" `In onComplete function for addHandlerAsync Status: succeeded`" が返されてテキスト ボックス内に表示されます。別のタスクを選択すると、テキスト ボックスには " `In task selection changed event handler`" と表示されます。これは、タスク選択の変更イベントに対するコールバック関数の出力です。[ **削除**] ボタンを選択して、イベント ハンドラーの登録を解除します。
    
15. リソースに関するメソッドを使用するには、最初に [ **リソース シート**]、[ **リソース配分状況**]、[ **リソース フォーム**] などのビューを選択し、次にそのビュー内でリソースを選択します。[ **getSelectedResourceAsync**] を選択して  **resourceGuid** 変数を初期化し、[ **リソース フィールドの取得**] を選択して、 **getResourceFieldAsync** の複数回の呼び出しによってリソースのプロパティを取得します。また、リソース選択変更のイベント ハンドラーを追加または削除することもできます。
    
  ```
  Resource name: R1
Cost: $800.00
Standard Rate: $50.00/h
Actual Cost: $0.00
Actual Work: 0h
Units: 100%
  ```

16. [ **getSelectedViewAsync**] を選択して、アクティブなビューの種類と名前を表示します。また、ビュー選択変更のイベント ハンドラーを追加または削除することもできます。たとえば、[ **リソース フォーム**] がアクティブなビューである場合、 **getSelectedViewAsync** 関数は、テキスト ボックスに次のように表示します。
    
  ```
  View type: 6
Name: Resource Form
  ```

17. [ **プロジェクト フィールドの取得**] を選択して、 **getProjectFieldAsync** 関数の複数回の呼び出しによってアクティブなプロジェクトの各種プロパティを取得します。プロジェクトが Project Web App から開かれる場合、 **getProjectFieldAsync** 関数は Project Web App のインスタンスの URL を取得できます。
    
  ```
  Project GUID: 9845922E-DAB4-E111-8AF3-00155D3BA208

Start: Tue 6/12/12
Finish: Tue 6/19/12

Currency digits: 2
Currency symbol: $
Symbol position: 0

Project web app URL:
  http://servername/pwa
  ```

18. [ **コンテキスト値の取得**] ボタンを選択して、アドインが実行されているドキュメントやアプリケーションのプロパティを取得します。そのために、 **Office.Context.document** オブジェクトや **Office.context.application** オブジェクトのプロパティが取得されます。たとえば、Project1.mpp ファイルがローカル コンピューターのデスクトップ上にある場合、ドキュメントの URL は `C:\Users\UserAlias\Desktop\Project1.mpp` となります。この .mpp ファイルが SharePoint ライブラリ内にある場合、値はドキュメントの URL になります。Project Professional 2013を使用して Project1 という名前のプロジェクトを Project Web App から開いている場合、ドキュメントの URL は `<>\Project1` となります。
    
  ```
  Document URL:
<>\Project1
Document mode: readWrite
App language: en-US
Display language: en-US
  ```

19. ソース コードを編集した後は、Project をいったん閉じて再起動することで、アドインを最新の情報に更新できます。[ **プロジェクト**] リボンの [ **Office アドイン**] ドロップダウン リストに、最近使用したアドインの一覧が保持されています。
    

## 例


Project 2013 SDK のダウンロードには、JSOMCall.html ファイル、JSOM_Sample.js ファイル、関連する Office.js、Office.debug.js、Project-15.js、および Project-15.debug.js の各ファイルの完全なコードが含まれています。次に、JSOMCall.html ファイルのコードを示します。


```HTML
<!DOCTYPE html>
<html>
    <head>
        <title>Project OM Sample Code</title>
        <meta http-equiv="X-UA-Compatible" content="IE=Edge"/>

        <script type="text/javascript" src="MicrosoftAjax.js"></script>

        <!-- Use the CDN reference to office.js when deploying your add-in. -->
        <!-- <script src="https://appsforoffice.microsoft.com/lib/1/hosted/Office.js"></script> -->
        <script type="text/javascript" src="Office.js"></script>
        <script type="text/javascript" src="JSOM_Sample.js"></script>

        <style type="text/css">           
            .button-wide {
                width: 210px;
                margin-top: 2px;
            }
            .button-narrow 
            {
                width: 80px;
                margin-top: 2px;
            }
        </style>
    </head>

    <body>
      <div id="Common_JSOM_API">
        OBJECT MODEL TESTS
        <br /><br />       
        <strong>General method:</strong>
        <br />
        <input id="Button5" class="button-wide" type="button" onclick="getSelectedDataAsync()" 
            value="getSelectedDataAsync" />
      </div>

      <div id="ProjectSpecificTask">
        <br />
        <strong>Project-specific task methods:</strong><br />
        <button class="button-wide" onclick="getSelectedTaskAsync()">getSelectedTaskAsync</button><br />
        <button class="button-wide" onclick="getTaskAsync()">getTaskAsync</button><br />
        <button class="button-wide" onclick="getTaskFields()">Get Task Fields</button><br />
        <button class="button-wide" onclick="getWSSUrlAsync()">getWSSUrlAsync</button>
        <strong>Task selection changed:</strong>
        <button class="button-narrow" onclick="manageTaskEventHandler('addHandlerAsync')">Add</button>
        <button class="button-narrow" onclick="manageTaskEventHandler('removeHandlerAsync')">Remove</button>         
      </div>
<div id="ResourceMethods">
  <br />
  <strong>Resource methods:</strong>
  <button class="button-wide" onclick="getSelectedResourceAsync()">getSelectedResourceAsync</button><br />
  <button class="button-wide" onclick="getResourceFields()">Get Resource Fields</button><br />
  <strong>Resource selection changed:</strong>
  <button class="button-narrow" onclick="manageResourceEventHandler('addHandlerAsync')">Add</button>
  <button class="button-narrow" onclick="manageResourceEventHandler('removeHandlerAsync')">Remove</button>
</div>
<div id="ViewMethods">
  <br />
  <strong>View method:</strong>
  <button class="button-wide" onclick="getSelectedViewAsync()">getSelectedViewAsync</button><br />
  <strong>View selection changed:</strong>
  <button class="button-narrow" onclick="manageViewEventHandler('addHandlerAsync')">Add</button>
  <button class="button-narrow" onclick="manageViewEventHandler('removeHandlerAsync')">Remove</button>         
</div>
<div id="ProjectMethods">
  <br />
  <strong>Project properties:</strong>
  <button class="button-wide" onclick="getProjectFields()">Get Project Fields</button><br />
</div>
<div id="ContextVariables">
  <br />
  <strong>Context properties:</strong>
  <button class="button-wide" onclick="getContextValues()">Get Context Values</button>
</div>

      <br />
      <textarea id="text" rows="10" cols="25">This is the text result.</textarea>
    </body>
</html
```


## 堅牢なプログラミング


 **Project OM Test** アドインは、Project-15.js と Office.js の各ファイル内にある Project 2013用のいくつかの JavaScript 関数の使用法を示すための例です。この例は単なるテスト用で、堅牢なエラー チェックは含まれていません。たとえば、リソースを選択せずに **getSelectedResourceAsync** 関数を実行すると、 **resourceGuid** 変数は初期化されず、 **getResourceFieldAsync** の呼び出しでエラーが返されます。実際に運用するアドインでは、特定のエラーをチェックして結果を無視したり、特定の状況に該当しない機能を隠したり、機能を使用する前にビューや有効な項目を選択するようにユーザーに通知したりする必要があります。

次のコード例のエラー出力には  **actionMessage** 変数が含まれています。この変数には、 **getSelectedResourceAsync** 関数のエラーを回避するための操作が指定されています。




```
function logError(errorText) {
    text.value = "Error in " + errorText;
}

function logMethodError(methodName, errorName, errorMessage, actionMessage) {
    logError(methodName + " method.\nError name: " + errorName
        + "\nMessage: " + errorMessage
        + "\n\nAction: " + actionMessage);
}
// Get the GUID of the selected resource.
function getSelectedResourceAsync() {
    _projDoc.getSelectedResourceAsync(function (asyncResult) {
        if (asyncResult.status == Office.AsyncResultStatus.Succeeded) {
            text.value = asyncResult.value;
            resourceGuid = asyncResult.value;
        }
        else {
            var actionMessage = "Select a resource before running the getSelectedResourceAsync method.";
            logMethodError("getSelectedResourceAsync", asyncResult.error.name,
                               asyncResult.error.message, actionMessage);
        }
    });
}
```

Visual Studio 2015 を使用すると、ブレークポイントを設定して JavaScript コードを容易にデバッグしたり、エラー処理用の一般的なルーチンをすばやく組み込んだりできるため、アドインの開発がいっそう簡単になります。たとえば、Project 2013 SDK ダウンロードのサンプル  **HelloProject_OData** には、JQuery ライブラリを使用してポップアップ エラー メッセージを表示する SurfaceErrors.js ファイルが含まれています。図 4 に、"toast" 通知のエラー メッセージを示します。また、このサンプルには、Office.js ファイルと Project-15.js ファイルの JavaScript 関数に Intellisense を提供する Office-vsdoc.js ファイルも含まれています。

SurfaceErrors.js ファイル内の次のコードには、 **Toast** オブジェクトを作成する **throwError** 関数が含まれています。




```
/*
 * Show error messages in a "toast" notification.
 */

// Throws a custom defined error.
function throwError(errTitle, errMessage) {
    try {
        // Define and throw a custom error.
        var customError = { name: errTitle, message: errMessage }
        throw customError;
    }
    catch (err) {
        // Catch the error and display it to the user.
        Toast.showToast(err.name, err.message);
    }
}

// Add a dynamically-created div "toast" for displaying errors to the user.
var Toast = {

    Toast: "divToast",
    Close: "btnClose",
    Notice: "lblNotice",
    Output: "lblOutput",

    // Show the toast with the specified information.
    showToast: function (title, message) {

        if (document.getElementById(this.Toast) == null) {
            this.createToast();
        }

        document.getElementById(this.Notice).innerText = title;
        document.getElementById(this.Output).innerText = message;

        $("#" + this.Toast).hide();
        $("#" + this.Toast).show("slow");
    },

    // Create the display for the toast.
    createToast: function () {
        var divToast;
        var lblClose;
        var btnClose;
        var divOutput;
        var lblOutput;
        var lblNotice;

        // Create the container div.
        divToast = document.createElement("div");
        var toastStyle = "background-color:rgba(220, 220, 128, 0.80);" +
            "position:absolute;" +
            "bottom:0px;" +
            "width:90%;" +
            "text-align:center;" +
            "font-size:11pt;";
        divToast.setAttribute("style", toastStyle);
        divToast.setAttribute("id", this.Toast);

        // Create the close button.
        lblClose = document.createElement("div");
        lblClose.setAttribute("id", this.Close);
        var btnStyle = "text-align:right;" +
            "padding-right:10px;" +
            "font-size:10pt;" +
            "cursor:default";
        lblClose.setAttribute("style", btnStyle);
        lblClose.appendChild(document.createTextNode("CLOSE "));

        btnClose = document.createElement("span");
        btnClose.setAttribute("style", "cursor:pointer;");
        btnClose.setAttribute("onclick", "Toast.close()");
        btnClose.innerText = "X";
        lblClose.appendChild(btnClose);

        // Create the div to contain the toast title and message.
        divOutput = document.createElement("div");
        divOutput.setAttribute("id", "divOutput");
        var outputStyle = "margin-top:0px;";
        divOutput.setAttribute("style", outputStyle);

        lblNotice = document.createElement("span");
        lblNotice.setAttribute("id", this.Notice);
        var labelStyle = "font-weight:bold;margin-top:0px;";
        lblNotice.setAttribute("style", labelStyle);

        lblOutput = document.createElement("span");
        lblOutput.setAttribute("id", this.Output);

        // Add the child nodes to the toast div.
        divOutput.appendChild(lblNotice);
        divOutput.appendChild(document.createElement("br"));
        divOutput.appendChild(lblOutput);
        divToast.appendChild(lblClose);
        divToast.appendChild(divOutput);

        // Add the toast div to the document body.
        document.body.appendChild(divToast);
    },

    // Close the toast.
    close: function () {
        $("#" + this.Toast).hide("slow");
    }
}
```

 **throwError** 関数を使用するには、JSOMCall.html ファイルに JQuery ライブラリと SurfaceErrors.js スクリプトを含め、さらに、 **logMethodError** などの別の JavaScript 関数に **throwError** の呼び出しを追加します。


 >**メモ**  アドインを展開する前に、office.js の参照と jQuery の参照をコンテンツ配信ネットワーク (CDN) の参照に変更してください。CDN の参照は最新のバージョンと高いパフォーマンスを提供します。




```HTML
<!DOCTYPE html>
<html>
<head>
    <title>Project OM Sample Code</title>
    <meta http-equiv="X-UA-Compatible" content="IE=Edge" />

    <script type="text/javascript" src="MicrosoftAjax.js"></script>

    <!-- Use the CDN reference to Office.js and jQuery when deploying your add-in. -->
    <!-- <script src="https://appsforoffice.microsoft.com/lib/1/hosted/Office.js"></script> -->
    <script type="text/javascript" src="Office.js"></script>
    <script type="text/javascript" src="http://ajax.microsoft.com/ajax/jQuery/jquery-1.9.0.min.js"></script>

    <script type="text/javascript" src="JSOM_Sample.js"></script>
    <script type="text/javascript" src="SurfaceErrors.js"></script>

    <!-- . . . INVALID USE OF SYMBOLS
</head>

```




```
function logMethodError(methodName, errorName, errorMessage, actionMessage) {
    logError(methodName + " method.\nError name: " + errorName
        + "\nMessage: " + errorMessage
        + "\n\nAction: " + actionMessage);

    throwError(methodName + " error", actionMessage);
}
```


**図 4. SurfaceErrors.js ファイル内の関数は "toast" 通知を表示できます。**

![SurfaceError ルーチンを使用してエラーの表示](../../images/pj15_CreateSimpleAgave_SurfaceError.gif)


## その他のリソース



- [Project 用の作業ウィンドウ アドイン](../project/project-add-ins.md)
    
- [アドイン用の JavaScript API について](http://msdn.microsoft.com/ja-jp/library/01180dae-ca45-40c8-b3dd-fd2a85651c0c%28Office.15%29.aspx)
    
- [JavaScript API for Office アドイン](http://msdn.microsoft.com/ja-jp/library/b27e70c3-d87d-4d27-85e0-103996273298%28Office.15%29.aspx)
    
- [Office アドインのマニフェスト向けのスキーマ リファレンス (v1.1)](http://msdn.microsoft.com/library/7e0cadc3-f613-8eb9-57ef-9032cbb97f92%28Office.15%29.aspx)
    
- [Project 2013 SDK download](https://www.microsoft.com/en-us/download/details.aspx?id=30435)
    
