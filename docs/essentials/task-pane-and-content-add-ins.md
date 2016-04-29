
# 作業ウィンドウ アドインとコンテンツ アドインの概要
JavaScript API を使用して、さまざまなホスト アプリケーションに必要な機能を持つ作業ウィンドウ アドインまたはコンテンツ アドインを作成します。 

 _ **適用対象:** Access apps for SharePoint?| apps for Office?| Excel?| Office Add-ins?| PowerPoint?| Project?| Word_


## コンテンツ アドインと作業ウィンドウ アドインの JavaScript API のサポート


このセクションでは、コンテンツ アドインと作業ウィンドウ アドインから呼び出すことのできる [JavaScript API for Office](http://msdn.microsoft.com/library/b27e70c3-d87d-4d27-85e0-103996273298%28Office.15%29.aspx) のサブセットについて簡単に説明します。API の機能の概要については、「 [JavaScript API for Office について](../develop/understanding-the-javascript-api-for-office.md)」を参照してください。サンプルについては、「 [Office 用アドインのコード サンプル](code-samples.md)」を参照してください。

API をアドインの種類別およびホスト アプリケーション別にまとめた表を次に示します。



|**アドインの種類別に検索する:**|**ホスト アプリケーション別に検索する:**|
|:-----|:-----|
|
|||
|:-----|:-----|
|[![コンテンツ アプリ用 Office オブジェクト モデルへのズーム](../../images/appIcons_content.png)](http://go.microsoft.com/fwlink/?LinkId=391752)|コンテンツ アドイン [ZoomIt](http://go.microsoft.com/fwlink/?LinkId=391752)|
|[![作業ウィンドウ アプリ用オブジェクト モデルへのズーム](../../images/appIcons_taskpane.png)](http://go.microsoft.com/fwlink/?LinkId=391757)|作業ウィンドウ アドイン [ZoomIt](http://go.microsoft.com/fwlink/?LinkId=391757)|
||[マップ セットのダウンロード](http://www.microsoft.com/en-us/download/details.aspx?id=42032)アドインの種類およびホスト アプリケーション別。|
|
|||
|:-----|:-----|
|[![Access](../../images/appIcons_Access.png)](http://go.microsoft.com/fwlink/?LinkId=391750)|Access [ZoomIt](http://go.microsoft.com/fwlink/?LinkId=391750)|
|[![Excel 用アプリ オブジェクト モデルへのズーム](../../images/appIcons_Excel.png)](http://go.microsoft.com/fwlink/?LinkId=391753)|Excel [ZoomIt](http://go.microsoft.com/fwlink/?LinkId=391753)|
|[![PowerPoint 用アプリ オブジェクト モデルへのズーム](../../images/appIcons_PowerPoint.png)](http://go.microsoft.com/fwlink/?LinkId=391755)|PowerPoint [ZoomIt](http://go.microsoft.com/fwlink/?LinkId=391755)|
|[![Project 用アプリ オブジェクト モデルへのズーム](../../images/appIcons_Project.png)](http://go.microsoft.com/fwlink/?LinkId=391756)|Project [ZoomIt](http://go.microsoft.com/fwlink/?LinkId=391756)|
|[![Word 用アプリ オブジェクト モデルへのズーム](../../images/appIcons_Word.png)](http://go.microsoft.com/fwlink/?LinkId=391758)|Word [ZoomIt](http://go.microsoft.com/fwlink/?LinkId=391758)|
|
コンテンツ アドインと作業ウィンドウ アドインでサポートされているプライマリ オブジェクトとメソッドは次のように分類することができます。


1.  **他の Office アドイン**と共有される共通のオブジェクト
    
    このようなオブジェクトとしては、 [Office](http://msdn.microsoft.com/ja-jp/library/c490b13d-ee52-4291-af5d-f4a5a11d3af0%28Office.15%29.aspx)、 [Context](http://msdn.microsoft.com/library/662883d5-b86f-4bdc-99f0-9ee9129ed16c%28Office.15%29.aspx)、および [AsyncResult](http://msdn.microsoft.com/library/540c114f-0398-425c-baf3-7363f2f6bc47%28Office.15%29.aspx) があります。 **Office** オブジェクトは JavaScript API for Office のルート オブジェクトです。 **Context** オブジェクトはアドインのランタイム環境を表します。 **Office** と **Context** はいずれも Office アドイン の基礎となるオブジェクトです。 **AsyncResult** オブジェクトは、ユーザーが文書内で選択したものを読み取る **getSelectedDataAsync** メソッドに返されたデータなどの非同期操作の結果を表します。
    
2.  **Document オブジェクト**
    
    コンテンツ アドインと作業ウィンドウ アドインで使用可能な API の大部分は、 [Document](http://msdn.microsoft.com/ja-jp/library/f8859516-cc1f-4b20-a8f3-cee37a983e70%28Office.15%29.aspx) オブジェクトのメソッド、プロパティ、およびイベントを通して公開されます。この API のサブセットを使用すれば、コンテンツ アドインまたは作業ウィンドウ アドインは、このトピックで後述するタスクを実行できます。
    
    コンテンツ アドインと作業ウィンドウ アドインは、 [Office.context.document](http://msdn.microsoft.com/library/92351713-9ea0-43e1-b549-dad93b3208b2%28Office.15%29.aspx) プロパティを使用して **Document** オブジェクトにアクセスし、それを通して、 [Bindings](http://msdn.microsoft.com/library/09979e31-3bfb-45be-adda-0f7cc2db1fe1%28Office.15%29.aspx) オブジェクト、 [CustomXmlParts](http://msdn.microsoft.com/library/ba40cd4c-29bb-4f31-875d-6f1382fd1ee8%28Office.15%29.aspx) オブジェクト、 [getSelectedDataAsync](http://msdn.microsoft.com/library/f85ad02c-64f0-4b73-87f6-7f521b3afd69%28Office.15%29.aspx) メソッド、 [setSelectedDataAsync](http://msdn.microsoft.com/library/998f38dc-83bd-4659-a759-4758c632a6ef%28Office.15%29.aspx) メソッド、 [getFileAsync](http://msdn.microsoft.com/library/78047418-89c4-4c7d-9427-4735b8559518%28Office.15%29.aspx) メソッドなどの文書内のデータを操作するための API の主要メンバーにアクセスできます。 **Document** オブジェクトは、文書が読み取り専用モードと編集モードのどちらになっているかを判断するための [mode](http://msdn.microsoft.com/library/551369c3-315b-428f-8b7e-08987f6b0e00%28Office.15%29.aspx) プロパティと、現在の文書の URL を取得して、 [Settings](http://msdn.microsoft.com/library/480ac3c6-370e-4505-aba3-1d0dce9fb3dc%28Office.15%29.aspx) オブジェクトにアクセスするための [url](http://msdn.microsoft.com/library/ad733387-a58c-4514-8fc2-53e64fad468d%28Office.15%29.aspx) プロパティも提供します。 **Document** オブジェクトは、 [SelectionChanged](http://msdn.microsoft.com/library/4cbc527c-a1d5-4fb0-b6db-28cc40c5d5e2%28Office.15%29.aspx) イベントのイベント ハンドラの追加もサポートしているため、ユーザーが文書内の選択を変更した時点を検出できます。
    
    コンテンツ アドインや作業ウィンドウ アドインが  **Document** オブジェクトにアクセスできるのは、DOM とランタイム環境が [Office.initialize](http://msdn.microsoft.com/library/727adf79-a0b5-48d2-99c7-6642c2c334fc%28Office.15%29.aspx) イベント用のイベント ハンドラーなどで読み込まれた後だけです。アドインが初期化されるときのイベント フローと、DOM とラインタイムが正常に読み込まれたかどうかの確認方法については、「 [DOM とランタイム環境を読み込む](../../docs/develop/loading-the-dom-and-runtime-environment.md)」を参照してください。
    
3.  **特定の機能を操作するためのオブジェクト**
    
    API の特定の機能を操作するために、コンテンツ アドインまたは作業ウィンドウ アドインで以下のオブジェクトとメソッドを操作できます。
    
      - [Bindings](http://msdn.microsoft.com/library/09979e31-3bfb-45be-adda-0f7cc2db1fe1%28Office.15%29.aspx) オブジェクトのメソッドを使用してバインドを作成または取得してから、 [Binding](http://msdn.microsoft.com/library/42882642-d22b-47d2-a8d3-3aa8c6a4435e%28Office.15%29.aspx) オブジェクトのメソッドとプロパティを使用してそれらのデータを操作します。
    
  - [CustomXmlParts](http://msdn.microsoft.com/library/ba40cd4c-29bb-4f31-875d-6f1382fd1ee8%28Office.15%29.aspx)、 [CustomXmlPart](http://msdn.microsoft.com/library/83f0e668-8236-4f2f-a20f-b173a9e3f65f%28Office.15%29.aspx)、および関連するオブジェクトを使用して、Word 文書内のカスタム XML パーツを作成して操作します。
    
  - [File](http://msdn.microsoft.com/library/04923ddf-8efa-459f-aed5-d8c06385ca50%28Office.15%29.aspx) オブジェクトと [Slice](http://msdn.microsoft.com/library/011b5647-639b-4b06-8625-ba9de01bed4b%28Office.15%29.aspx) オブジェクトを使用して、文書全体のコピーを作成し、それをチャンクまたは「スライス」に分割してから、それらのスライスに含まれるデータを読み取ったり、転送したりします。
    
  - [Settings](http://msdn.microsoft.com/library/ad733387-a58c-4514-8fc2-53e64fad468d%28Office.15%29.aspx) オブジェクトを使用して、ユーザー設定やアドインの状態などのカスタム データを保存します。
    

 >**重要**  API メンバーの一部は、コンテンツ アドインと作業ウィンドウ アドインをホスト可能なすべての Office アプリケーションでサポートされているわけではありません。サポートされているメンバーを特定するには、次のいずれかを参照してください。

Office ホスト アプリケーション全体で使用可能な JavaScript API for Office のサポートの概要については、「 [JavaScript API for Office について](../develop/understanding-the-javascript-api-for-office.md)」トピックの「 [API サポート マトリックス](understanding-the-javascript-api-for-office.md#APIOverview_APISupportMatrix)」を参照してください。


## アクティブな選択範囲の読み取りと書き込み


文書、スプレッドシート、またはプレゼンテーション内のユーザーの現在の選択範囲に対して読み書きをすることができます。アドイン用のホスト アプリケーションに応じて、 [Document](http://msdn.microsoft.com/library/f85ad02c-64f0-4b73-87f6-7f521b3afd69%28Office.15%29.aspx) オブジェクトの [getSelectedDataAsync](http://msdn.microsoft.com/library/998f38dc-83bd-4659-a759-4758c632a6ef%28Office.15%29.aspx) メソッドと [setSelectedDataAsync](http://msdn.microsoft.com/library/f8859516-cc1f-4b20-a8f3-cee37a983e70%28Office.15%29.aspx) メソッド内のパラメーターとして読み書きするデータ構造のタイプを指定できます。たとえば、Word には任意のデータ タイプ (テキスト、HTML、表形式データ、または Office Open XML)、Excel にはテキストと表形式データ、および PowerPoint と Project にはテキストを指定できます。ユーザーの選択範囲に対する変更を検出するためのイベント ハンドラーを作成することもできます。以下の例では、 **getSelectedDataAsync** メソッドを使用して、選択範囲からデータをテキストとして取得します。


```
Office.context.document.getSelectedDataAsync(
    Office.CoercionType.Text, function (asyncResult) {
        if (asyncResult.status == Office.AsyncResultStatus.Failed) {
            write('Action failed. Error: ' + asyncResult.error.message);
        }
        else {
            write('Selected data: ' + asyncResult.value);
        }
    });

// Function that writes to a div with id='message' on the page.
function write(message){
    document.getElementById('message').innerText += message; 
}

```

さらなる詳細と例については、「 [ドキュメントまたはスプレッドシート内のアクティブな選択範囲へのデータの読み取りおよび書き込み](../../docs/develop/read-and-write-data-to-the-active-selection-in-a-document-or-spreadsheet.md)」を参照してください。


## 文書またはスプレッドシート内の領域へのバインド


前のセクションで説明したように、 **getSelectedDataAsync** メソッドと **setSelectedDataAsync** メソッドを使用すれば、文書、スプレッドシート、またはプレゼンテーション内のユーザーの _現在_ の選択範囲に対して読み書きできます。ただし、ユーザーに選択を要求せずにアドインの複数の実行セッションに渡って文書内の同じ領域にアクセスする場合は、最初にその領域をバインドする必要があります。そのバインドした領域に対するデータおよび選択範囲変更イベントにサブスクライブすることもできます。

バインドは、 [Bindings](http://msdn.microsoft.com/library/afbadac7-60c7-47cb-9477-6e9466ded44c%28Office.15%29.aspx) オブジェクトの [addFromNamedItemAsync](http://msdn.microsoft.com/library/9dc03608-b08b-4700-8be1-3c86ae236799%28Office.15%29.aspx) メソッド、 [addFromPromptAsync](http://msdn.microsoft.com/library/edc99214-e63e-43f2-9392-97ead42fc155%28Office.15%29.aspx) メソッド、または [addFromSelectionAsync](http://msdn.microsoft.com/library/09979e31-3bfb-45be-adda-0f7cc2db1fe1%28Office.15%29.aspx) メソッドを使用して追加できます。これらのメソッドは、バインド内のデータにアクセスするため、あるいは、データ変更または選択範囲変更イベントにサブスクライブするために使用可能な識別子を返します。

 **Bindings.addFromSelectionAsync** メソッドを使用して、文書内で現在選択されているテキストにバインドを追加する例を以下に示します。




```
Office.context.document.bindings.addFromSelectionAsync(
    Office.BindingType.Text, { id: 'myBinding' }, function (asyncResult) {
    if (asyncResult.status == Office.AsyncResultStatus.Failed) {
        write('Action failed. Error: ' + asyncResult.error.message);
    } else {
        write('Added new binding with type: ' +
            asyncResult.value.type + ' and id: ' + asyncResult.value.id);
    }
});

// Function that writes to a div with id='message' on the page.
function write(message){
    document.getElementById('message').innerText += message; 
}
```

さらなる詳細と例については、「 [ドキュメントまたはスプレッドシート内の領域へのバインド](../../docs/develop/bind-to-regions-in-a-document-or-spreadsheet.md)」を参照してください。


## 文書全体の取得


作業ウィンドウ アドインが PowerPoint または Word で実行される場合は、 [Document.getFileAsync](http://msdn.microsoft.com/ja-jp/library/78047418-89c4-4c7d-9427-4735b8559518%28Office.15%29.aspx) メソッド、 [File.getSliceAsync](http://msdn.microsoft.com/ja-jp/library/5a8a5cc2-e883-42cd-92ab-d63e10c4c707%28Office.15%29.aspx) メソッド、および [File.closeAsync](http://msdn.microsoft.com/ja-jp/library/1ad5cebf-6feb-43ff-8b19-97d91132ab2b%28Office.15%29.aspx) メソッドを使用して、プレゼンテーションまたは文書全体を取得できます。

 **Document.getFileAsync** を呼び出すと、 [File](http://msdn.microsoft.com/ja-jp/library/04923ddf-8efa-459f-aed5-d8c06385ca50%28Office.15%29.aspx) オブジェクトに文書のコピーが返されます。 **File** オブジェクトは、 [Slice](http://msdn.microsoft.com/ja-jp/library/011b5647-639b-4b06-8625-ba9de01bed4b%28Office.15%29.aspx) オブジェクトとして表現される「チャンク」内の文書へのアクセスを提供します。 **getFileAsync** を呼び出すときに、ファイル タイプ (テキストまたは圧縮 Open Office XML 形式) とスライスのサイズ (4 MB 以下) を指定できます。 **File** オブジェクトの内容にアクセスするには、 [Slice.data](http://msdn.microsoft.com/ja-jp/library/95a68949-6009-49ae-a531-2df77687b85d%28Office.15%29.aspx) プロパティに生データを返す **File.getSliceAsync** を呼び出します。圧縮形式を指定した場合は、ファイル データがバイト配列で返されます。ファイルを Web サービスに転送する場合は、圧縮生データを base64 エンコード文字列に変換してから送信できます。最後に、ファイルのスライスの取得が完了したら、 **File.closeAsync** メソッドを使用して文書を閉じます。

詳細については、「 [PowerPoint または Word 用アドインからドキュメント全体を取得する](http://msdn.microsoft.com/ja-jp/library/47a4ab14-0f1e-4cc8-8814-fa7e97362360%28Office.15%29.aspx)方法」を参照してください。 


## Word 文書のカスタム XML パーツの読み取りと書き込み


Open Office XML ファイル形式とコンテンツ コントロールを使用すれば、Word 文書にカスタム XML パーツを追加して、その文書内のコンテンツ コントロールに XML パーツ内の要素をバインドすることができます。文書を開くと、Word が、自動的に、バインドされたコンテンツ コントロールを読み取って、カスタム XML パーツからのデータを設定します。ユーザーは、コンテンツ コントロールにデータを書き込むこともできます。ユーザーが文書を保存すると、コントロール内のデータがバインドされた XML パーツに保存されます。Word 用の作業ウィンドウ アドインは、 [Document.customXmlParts](http://msdn.microsoft.com/ja-jp/library/b72c08bc-b49c-497c-9521-26ccce148bda%28Office.15%29.aspx) プロパティ、 [CustomXmlParts](http://msdn.microsoft.com/ja-jp/library/ba40cd4c-29bb-4f31-875d-6f1382fd1ee8%28Office.15%29.aspx) オブジェクト、 [CustomXmlPart](http://msdn.microsoft.com/ja-jp/library/83f0e668-8236-4f2f-a20f-b173a9e3f65f%28Office.15%29.aspx) オブジェクト、および [CustomXmlNode](http://msdn.microsoft.com/ja-jp/library/dc1518de-47fa-4108-aab7-04a022724b04%28Office.15%29.aspx) オブジェクトを使用して、文書に対して動的にデータを読み書きすることができます。

カスタム XML パーツは名前空間に関連付けることができます。名前空間内のカスタム XML パーツからデータを取得するには、 [CustomXmlParts.getByNamespaceAsync](http://msdn.microsoft.com/ja-jp/library/9902f555-5c20-45d6-9a8c-ae6bf013dfaf%28Office.15%29.aspx) メソッドを使用します。

[CustomXmlParts.getByIdAsync](http://msdn.microsoft.com/ja-jp/library/31a21b58-426e-4bbe-acdf-885b32ce50ab%28Office.15%29.aspx) メソッドを使用して、GUID でカスタム XML パーツにアクセスすることもできます。カスタム XML パーツを取得したら、 [CustomXmlPart.getXmlAsync](http://msdn.microsoft.com/ja-jp/library/6606365a-9244-49b5-9393-fe2186091af7%28Office.15%29.aspx) メソッドを使用して XML データを取得します。

文書に新しいカスタム XML パーツを追加するには、 **Document.customXmlParts** プロパティを使用して文書内に存在するカスタム XML パーツを取得し、 [CustomXmlParts.addAsync](http://msdn.microsoft.com/ja-jp/library/2816397c-b86a-4f52-8b13-036f527f4bb7%28Office.15%29.aspx) メソッドを呼び出します。

作業ウィンドウ アドインでのカスタム XML パーツの操作方法の詳細については、「 [Office Open XML を使用してより良い Word 用アドインを作成する](http://msdn.microsoft.com/ja-jp/library/c5bad651-a42f-4e57-bc60-c9b27eb2383b%28Office.15%29.aspx)」を参照してください。


## アドイン設定を保存する


ユーザー設定やアドインの状態などのアドインに関するカスタム データを保存して、次にアドインを開いたときにそのデータにアクセスすることが必要な場合がよくあります。そのデータは、ブラウザー Cookie や HTML 5 Web ストレージなどの一般的な Web プログラミング手法を使用して保存できます。または、アドインが Excel、PowerPoint、または Word で動作する場合は、 [Document.Settings](http://msdn.microsoft.com/ja-jp/library/ad733387-a58c-4514-8fc2-53e64fad468d%28Office.15%29.aspx) オブジェクトのメソッドを使用できます。 **Settings** オブジェクトで作成したデータは、アドインを挿入して保存したスプレッドシート、プレゼンテーション、または文書内に保管されます。このデータには、それを作成したアドイン以外からはアクセスできません。

文書が保存されているサーバーとのやり取りを避けるために、 **Settings** オブジェクトを使用して作成されたデータはランタイムでメモリ上で管理されます。過去に保存した設定データがアドインの初期化時にメモリに読み込まれ、そのデータに対する変更は [Settings.saveAsync](http://msdn.microsoft.com/ja-jp/library/7147c221-937c-477c-98a6-f59d6200c27b%28Office.15%29.aspx) メソッドを呼び出したときにのみ文書に保存されます。内部的に、データはシリアル化された JSON オブジェクト内に名前と値のペアとして保存されます。データのメモリ内コピーに対してアイテムの読み取り、書き込み、および削除を実行するには、 **Settings** オブジェクトの [get](http://msdn.microsoft.com/ja-jp/library/aeac06dd-994e-4235-b208-1bd117395296%28Office.15%29.aspx) メソッド、 [set](http://msdn.microsoft.com/ja-jp/library/4e2c9758-953e-41e8-aca6-d8daf764a584%28Office.15%29.aspx) メソッド、および [remove](http://msdn.microsoft.com/ja-jp/library/a92446bf-de65-45bd-8412-36ea8e77c5a2%28Office.15%29.aspx) メソッドを使用します。以下のコード行は、 `themeColor` という名前の設定を作成して、その値を 'green' に設定する方法を示しています。




```
Office.context.document.settings.set('themeColor', 'green');
```

 **set** メソッドと **remove** メソッドを使用した設定データの作成または削除はそのデータのメモリ内コピーに対して機能するため、 **saveAsync** を呼び出して、設定データに対する変更をアドインが操作する文書内に保持する必要があります。

 **Settings** オブジェクトのメソッドを使用したカスタム データの操作方法について詳しくは、「 [アドインの状態および設定を保持する](../../docs/develop/persisting-add-in-state-and-settings.md)」を参照してください。


## プロジェクト文書のプロパティの読み取り


作業ウィンドウ アドインが Project で動作する場合は、そのアドインでアクティブ プロジェクト内のプロジェクト フィールド、リソース、およびタスク フィールドの一部からデータを読み取ることができます。これを実現するには、追加の Project 固有の機能を提供するように  **Document** オブジェクトを拡張する [ProjectDocument](http://msdn.microsoft.com/ja-jp/library/1908af4f-93b9-4859-87e3-06942014fae1%28Office.15%29.aspx) オブジェクトのメソッドとイベントを使用します。

Project のデータを読み取る例については、「 [テキスト エディターを使用して Project 2013 用の作業ウィンドウ アドインを初めて作成する](../../docs/project/create-your-first-task-pane-add-in-for-project-by-using-a-text-editor.md)」を参照してください。


## アクセス許可モデルとガバナンス


アドインは、そのマニフェスト内の  **Permissions** 要素を使用して、必要な機能のレベルにアクセスするためのアクセス許可を JavaScript API for Office に要求します。たとえば、アドインで文書に対する読み取りと書き込みアクセス権が必要な場合は、そのマニフェストで `ReadWriteDocument` をその **Permissions** 要素内のテキスト値として指定する必要があります。アクセス許可はユーザーのプライバシーとセキュリティを保護するために存在しているので、ベスト プラクティスとしては、その機能に必要な最低限のアクセス許可を要求することをお勧めします。以下の例は、作業ウィンドウのマニフェストで **ReadDocument** アクセス許可を要求する方法を示しています。


```XML
<?xml version="1.0" encoding="utf-8"?>
<OfficeApp xmlns="http://schemas.microsoft.com/office/appforoffice/1.0"
 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
 xsi:type="TaskPaneApp">
…<!-- Other manifest elements omitted. -->
  <Permissions>ReadDocument</Permissions>
…
</OfficeApp>

```

詳細については、「 [コンテンツ アドインおよび作業ウィンドウ アドインでの API 使用のアクセス許可を要求する](../../docs/develop/requesting-permissions-for-api-use-in-content-and-task-pane-add-ins.md)」を参照してください。


## その他のリソース



- [JavaScript API for Office](http://msdn.microsoft.com/library/b27e70c3-d87d-4d27-85e0-103996273298%28Office.15%29.aspx)
    
- [Office アドインのマニフェスト向けのスキーマ リファレンス (v1.1)](http://msdn.microsoft.com/ja-jp/library/7e0cadc3-f613-8eb9-57ef-9032cbb97f92.aspx)
    
- [Office アドインでのユーザー エラーのトラブルシューティング](../../docs/testing/testing-and-troubleshooting.md)
    
