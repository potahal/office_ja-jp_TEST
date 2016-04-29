
# ドキュメント テンプレートと作業ウィンドウ アドインを含む SharePoint アドインを作成する
Visual Studio を使用して、SharePoint アドインから開いたドキュメント内に表示される Office アドインを開発します。

 _ **適用対象:** apps for Office?| Excel?| Office Add-ins?| PowerPoint?| Project?| Word_

ドキュメント テンプレート (経費明細書など) を含む SharePoint アドインを作成できます。ドキュメントには、SharePoint データを操作する作業ウィンドウ アドインを含めることができます。たとえば、ユーザーは、Business Connectivity Services (BCS) のデータを使用して請求書のフィールドにデータを設定したり、SharePoint リストから経費カテゴリを選択して経費明細書を作成したりできます。

このチュートリアルでは、Excel ブックを含む SharePoint アドインの作成方法を示します。Excel ブックには、作業ウィンドウ アドインに SharePoint の日付が付いたドロップダウン リスト ボックスを作成するために SharePoint 2013 で提供される REST インターフェイスを使用する作業ウィンドウ アドインが含まれています。


## 前提条件


始める前に、次のコンポーネントをインストールします。




- SharePoint 開発環境
    
      - Office 365 の SharePoint を対象とする SharePoint アドインを開発する場合は、「 [方法: Office 365 で SharePoint アドインを開発する環境のセット アップ](http://msdn.microsoft.com/ja-jp/library/office/apps/fp161179%28v=office.15%29)」を参照してください。
    
  - オンプレミス インストールの SharePoint を対象とする SharePoint アドインを開発する場合は、「 [SharePoint アドインのオンプレミスの開発環境をセットアップする](http://msdn.microsoft.com/ja-jp/library/office/apps/fp179923%28v=office.15%29)」を参照してください。
    
- [Visual Studio 2015 と Microsoft Office Developer Tools](https://www.visualstudio.com/features/office-tools-vs)
    
- Excel 2013 または Office 365 アカウント
    

## Visual Studio で SharePoint アドイン プロジェクトを作成する



1. Visual Studio を起動します。
    
2. メニュー バーで、 [ **ファイル**]、[ **新規作成**]、[ **プロジェクト**] の順に選択します。
    
    [ **新しいプロジェクト**] ダイアログ ボックスが表示されます。
    
3. テンプレート ウィンドウで、使用する言語のノードの下の [ **Office/SharePoint**] を展開して、[ **Office アドイン**] を選択します。
    
4. プロジェクトの種類の一覧で、 **[SharePoint アドイン]** を選択し、プロジェクトに「OfficeEnabledAddin」という名前を付けて、 **[OK]** をクリックします。
    
    [ **新しい SharePoint アドイン**] ダイアログ ボックスが表示されます。
    
5. [ **アドインのデバッグに使用する SharePoint サイト**] ドロップダウン リストで、SharePoint サイトの URL を選択または入力します。
    
6. [ **SharePoint アドイン アドインをホストする方法**] ボックスの一覧で、[ **SharePoint ホスト型**] をクリックして、[ **次へ**] をクリックします。
    
     >**メモ**  このシナリオは、[ **SharePoint アドインをホストする方法**] ドロップダウン リストに表示された SharePoint ホスト型およびプロバイダー向けのホスト型の各オプションでのみ機能します。
7. 次のページで、 **[SharePoint 2013]** を選択し、 **[完了]** ボタンを選択してダイアログ ボックスを閉じます。
    

## 作業ウィンドウ アドイン アイテムを追加する


次に、プロジェクトに Office アドインを追加します。任意の種類のアドインを追加できます。ここでは作業ウィンドウ アドインを追加します。


1. [ **ソリューション エクスプローラー**] で、[ **OfficeEnabledAddin**] プロジェクト ノードを選択します。
    
2. [ **プロジェクト**] メニューの [ **新しいアイテムの追加**] をクリックします。
    
3. [ **新しいアイテムの追加**] ダイアログ ボックスで [ **Office/SharePoint**] を選択して、[ **Office アドイン**] を選択します。
    
4. 作業ウィンドウ アドインに「MyTaskPaneAddin」という名前を付けて、[ **追加**] ボタンをクリックします。
    
    [ **Office 用アドインの作成**] ダイアログ ボックスが表示されます。
    
5. [ **オフィス用アドインの作成**] ダイアログ ボックスで、[ **作業ウィンドウ**] を選択して [ **次へ**] を選択します。次のページで [ **Word**] と [ **PowerPoint** ] のチェック ボックスをオフにし、[ **次へ**] を選択します。
    
6. [ **Office アドインを新規ドキュメントまたは既存のドキュメントのどちらに表示しますか**] ページで、[ **新規ドキュメントを作成してアドインを挿入する**] を選択して、[ **完了**] を選択します。
    
    Visual Studio では、ライブラリのドキュメント ライブラリとブックのテンプレートが追加されました。ブックには作業ウィンドウ アドインが含まれています。
    

## ドキュメント ライブラリを追加する


この手順では、ドキュメント ライブラリを追加して、ブックをドキュメント ライブラリの既定のテンプレートにします。


1. [ **ソリューション エクスプローラー**] で、[ **OfficeEnabledAddin**] プロジェクト ノードを選択します。
    
2. [ **プロジェクト**] メニューの [ **新しいアイテムの追加**] をクリックします。
    
3. [ **新しいアイテムの追加**] ダイアログ ボックスで、[ **Office/SharePoint**] を選択してから [ **リスト**] を選択し、リストに「MyDocumentLibrary」という名前を付けて、[ **追加**] ボタンを選択します。
    
4. [ **SharePoint カスタマイズ ウィザード**] で、[ **次のカスタマイズ可能なリスト テンプレートとリスト インスタンスを作成する:**] を選択します。
    
5. このオプションの下にあるドロップダウン リストで、[ **ドキュメント ライブラリ**] を選択し、[ **次へ**] をクリックします。
    
6. [ **このドキュメント ライブラリのテンプレートを選択します。このライブラリでユーザーが作成するドキュメントは、そのテンプレートに基づいて作成されます**] ページで、[ **次のドキュメントをこのライブラリのテンプレートとして使用する**] を選択して、[ **参照**] をクリックします。
    
7. [ **開く**] ダイアログ ボックスで、[ **OfficeDocuments**] フォルダーを開き、[ **MyTaskPaneApp.xlsx**] ファイルを選択して、[ **開く**] をクリックし、[ **完了**] をクリックして、リスト デザイナーを閉じます。
    
8. [ **ソリューション エクスプローラー**] で、[ **OfficeEnabledAddin**] プロジェクト ノードを選択します。
    
9. [ **表示**] メニューの [ **プロパティ ウィンドウ**] をクリックします。
    
10. [ **ソリューション エクスプローラー**] で [ **AppManifest.xml**] ファイルを選択します。
    
11. [ **表示**]、[ **デザイナー**] の順に選択します。
    
12. マニフェスト デザイナーで、[ **スタート ページ**] の値を ~appWebUrl/Lists/MyDocumentLibrary に設定します。これにより、OfficeEnabledAddin/Lists/MyDocumentLibrary の値が変換されます。
    
     >**メモ**  この URL は、ドキュメント ライブラリを参照しています。アドイン Web 内のアイテムを参照している Office アドイン マニフェスト内の URL の先頭で ~appWebUrl トークンを使用する必要があります。SharePoint アドイン プロジェクトでの URL トークンの詳細については、「[SharePoint アドインの URL 文字列とトークン](http://msdn.microsoft.com/library/800ec8cd-a448-46bc-b41e-d4030eeb4048%28Office.15%29.aspx)」を参照してください。
13. マニフェスト デザイナーを閉じ、変更内容を保存します。
    

## 作業ウィンドウで SharePoint データを使用する


この手順では、SharePoint 2013 で提供される REST インターフェイスを使用して、サイト ユーザーの一覧を表示します。

この例では、SharePoint のリストのデータが表示されるだけですが、このようなデータをドキュメント承認アドインの一部として使用することができます。ユーザーがリストから名前を選択した際に、コードがドキュメント追跡リストの校閲者列の値を設定します。そのリストに関連するワークフローは、そのユーザーに校閲通知を送信することができます。また、選択した名前でドキュメント設定に保存できます。その後、ユーザーがドキュメントを開く際に、現在のユーザーとドキュメント設定に保存したユーザーが同じである場合にのみ、作業ウィンドウ アドインにコントロールを表示することができます。詳細については、次のトピックを参照してください。


- [SharePoint 2013 REST エンドポイントを使用して基本的な操作を完了する](http://msdn.microsoft.com/library/e3000415-50a0-426e-b304-b7de18f2f7d9%28Office.15%29.aspx)
    
- [SharePoint 2013 の JavaScript ライブラリ コードを使用して基本的な操作を完了する](http://msdn.microsoft.com/library/29089af8-dbc0-49b7-a1a0-9e311f49c826%28Office.15%29.aspx)
    
- [アドインの状態および設定を保持する](../../docs/develop/persisting-add-in-state-and-settings.md)
    

1. [ **ソリューション エクスプローラー**] で [ **MyTaskPaneAddin**] フォルダーを展開し、[ **Home**] フォルダーを展開して [ **Home.html**] ファイルを選択します。
    
    コード エディターに Home.html ファイルが開きます。
    
2.  `get-data-from-selection` ボタンの下に次の HTML を追加します。
    
  ```HTML
  <p>Select Reviewer:</p> <select class="select" id="select-reviewer" name="D1"> </select>
  ```

3. [ **Home.js**] ファイルを選択し、コード エディターで Home.js ファイルを開きます。
    
4. Home.js ファイルの先頭に次の宣言を追加します。
    
  ```
  var appWebURL; var web;
  ```

5.  `Initialize` 関数を、以下のコードで置換します。
    
    このコードでは、次のタスクを実行します。
    
      - jQuery で  `getScript` 関数を使用して SP.Runtime.js ファイルと SP.js ファイルを読み込みます。ファイルの読み込みが完了すると、プログラムから SharePoint の JavaScript オブジェクト モデルにアクセスできるようになります。
    
  - 現在の Web サイト オブジェクトを読み込みます。
    
  - サイトのユーザーをすべて取得する関数を呼び出します。次の手順でその関数のコードを追加します。
    



  ```
   // The initialize function must be run each time a new page is loaded Office.initialize = function (reason) { $(document).ready(function () { app.initialize(); var scriptbase = "/_layouts/15/"; $.getScript(scriptbase + "SP.Runtime.js", function () { $.getScript(scriptbase + "SP.js", function () { getAppWeb(function () { getSPUsers(populateUsersDropDown); }); }); }); function getAppWeb(functionToExecuteOnReady) { var context = SP.ClientContext.get_current(); web = context.get_web(); context.load(web); context.executeQueryAsync(onSuccess, onFailure); function onSuccess() { appWebURL = web.get_url(); functionToExecuteOnReady(); } function onFailure(sender, args) { app.initialize(); app.showNotification("Failed to connect to SharePoint. Error: " + args.get_message()); } } $('#get-data-from-selection').click(getDataFromSelection); }); };
  ```

6. Home.js ファイルの下部に次のコードを追加します。
    
    このコードは SharePoint 2013 で提供される REST インターフェイスを使用して、Web サイト ユーザーの一覧を取得します。次に、このコードは各ユーザーの名前および ID でドロップ ダウン リストを作成します。
    


  ```
  function getSPUsers(functionToExecuteOnReady) { var url = appWebURL + "/../_api/web/siteUsers"; jQuery.ajax({ url: url, type: "GET", headers: { "ACCEPT": "application/json;odata=verbose" }, success: onSuccess, error: onFailure }); function onSuccess(data) { var results = data.d.results; functionToExecuteOnReady(results); } function onFailure(jaXHR, textStatus, errorThrown) { var error = textStatus + " " + errorThrown; app.showNotification(error); } } function populateUsersDropDown(results) { for (var i = 0; i < results.length; i++) { var IDTemp = results[i].Id; $('#select-reviewer').append("<option value='" + IDTemp + "'>" + results[i].Title + "</option>"); } }
  ```

7.  **ソリューション エクスプローラー**で、 **AppManifest.xml** ファイルのショートカット メニューを開き、 [ **ビュー デザイナー**] を選択します。
    
8. デザイナーで、[ **権限**] ページを選択します。
    
9. [ **範囲**] 列のドロップダウン リストから [ **Web**] アイテムを選択します。
    
10. [ **アクセス許可**] 列のドロップダウン リストからアイテムの [ **読み取り**] を選択します。
    

## 作業ウィンドウ アドインをデバッグする


ドキュメントを開始または SharePoint アドインを開始し、ドキュメント ライブラリからドキュメントを開くことで、作業ウィンドウ アドインをデバッグできます。


### ドキュメントを開始して作業ウィンドウ アドインをデバッグする




 >**メモ**  このプロシージャは Excel を開くため、Office がシステムにインストールされている場合にのみ動作します。そうでない場合は、「このプロジェクトの種類に関連付けられているアプリケーションがこのコンピューターにインストールされていません」というエラーが表示されます。


1. コードエディターで Home.js ファイルを開いて、 `getDataFromSelection` メソッドの横にブレークポイントを設定します。
    
2.  **ソリューション エクスプ ローラー**で、[ **OfficeEnabledApp**] プロジェクト ノードを選択します。
    
3. [ **表示**] メニューの [ **プロパティ ウィンドウ**] をクリックします。
    
4. プロパティ ウィンドウで、[ **開始動作**] ドロップダウン リストから  **Office デスクトップ クライアント** アイテムを選択します。 そのときに、[ **ドキュメントの開始**] という新しいプロパティが表示されます。
    
5. [ **ドキュメントの開始**] ドロップダウン リストから  **OfficeDocuments\TaskPaneApp.xlsx** アイテムを選択します。
    
6. [ **デバッグ**] メニューの [ **デバッグ開始**] を選択します。
    
    この設定により、アドインを実行すると作業ウィンドウ アドイン内のブックが表示されます。ブックが開き、作業ウィンドウ アドインが表示されます。
    
7. 作業ウィンドウ アドインで、[ **校閲者の選択**] ドロップ ダウン リストを選択して SharePoint ユーザーの一覧を表示します。
    
8. Excel ブックで、任意のセルを選択します。
    
9. 作業ウィンドウ アドインで [ **選択範囲からデータを取得します**] ボタンをクリックします。
    
     `getDataFromSelection` メソッドの横に設定したブレークポイントの位置で実行が停止します。
    

### SharePoint を開始して作業ウィンドウ アドインをデバッグする




 >**メモ**  この手順では、Excel Online を開きます。この操作は、Office 365 アカウントを持っている場合にのみ機能します。「[[方法] Office 365 で SharePoint アドインを開発する環境をセットアップする](http://msdn.microsoft.com/ja-jp/library/office/apps/fp161179%28v=office.15%29)」を参照してください。


1. コード エディターで Home.js ファイルを開いて、 `getDataFromSelection` メソッドの横にブレークポイントを設定します。
    
2.  **ソリューション エクスプ ローラー**で、[ **OfficeEnabledApp**] プロジェクト ノードを選択します。
    
3. [ **表示**] メニューの [ **プロパティ ウィンドウ**] を選択します。
    
4. [プロパティ ウィンドウ] の [ **開始動作**] ドロップダウン リストから、  **Internet Explorer** アイテムを選択します。
    
5. [ **デバッグ**] メニューで、[ **デバッグ開始**] を選択します。
    
    Visual Studio に SharePoint が開かれて、[ **MyDocumentLibrary**] ライブラリが表示されます。
    
6. SharePoint の [ **ファイル**] タブで、[ **新しいドキュメント**] を選択します。
    
7. プロジェクトの MyTaskPaneApp.xlsx ブックに移動します。
    
    ブックが開き、作業ウィンドウ アドインが表示されます。
    
8. ブラウザーでスクリプトのデバッグを有効にしてください。Internet Explorer でスクリプトのデバッグを有効にするには、[ **インターネット オプション**] ダイアログ ボックスを開き、[ **詳細設定**] タブをクリックし、[ **スクリプトのデバッグを使用しない (Internet Explorer)**] チェック ボックスおよび [ **スクリプトのデバッグを使用しない (その他)**] チェック ボックスをオフにします。
    
9. Visual Studio の [ **デバッグ**] メニューで、 [ **プロセスにアタッチ**] を選択します。
    
10. [ **プロセスにアタッチ**] ダイアログ ボックスで、利用可能なすべての [ **iexplore.exe**] プロセスを選択して、[ **アタッチ**] をクリックします。
    
11. 作業ウィンドウ アドインで、[ **校閲者の選択**] ドロップ ダウン リストを選択して SharePoint ユーザーの一覧を表示します。
    
    リスト内のデータは、REST 呼び出しを使用して SharePoint から取得されます。
    
12. Excel ブックで、任意のセルを選択します。
    
13. 作業ウィンドウ アドインで、[ **選択範囲からデータを取得します**] ボタンをクリックします。
    
     `getDataFromSelection` メソッドの横に設定したブレークポイントの位置で実行が停止します。
    
     >**メモ**  ブックにデータが含まれていない場合は、ブックのツールバーで [ **ブックの編集**]、[ **Excel Online で編集**] の順に選択することによって、データを追加できます。

## アドインをパッケージ化して発行する


アドインの発行用にパッケージ化する準備ができたら、 **Office と SharePoint アドインの発行**ウィザードを開きます。


-  **ソリューション エクスプローラー**で、SharePoint アドイン プロジェクトのショートカット メニューを開き、[ **発行**] を選択します。
    
     **Office と SharePoint アドインの発行**ウィザードが表示されます。詳細については、「[Visual Studio を使用して SharePoint アドインを公開する](http://msdn.microsoft.com/library/8137d0fa-52e2-4771-8639-60af80f693bb%28Office.15%29.aspx)」を参照してください。
    

## その他の技術情報



- [作業ウィンドウ アドインとコンテンツ アドインの概要](task-pane-and-content-add-ins.md)
    
- [Office アドインの設計ガイドライン](../add-in-design.md)
    
- [Office アドインの開発ライフ サイクル](../../docs/design/add-in-development-lifecycle.md)
    
- [Office アドインを発行する](../publish/publish.md)
    
- [JavaScript API for Office について](../develop/understanding-the-javascript-api-for-office.md)
    
- [Office アドインの XML マニフェスト](../../docs/overview/add-in-manifests.md)
    
- [Office アドインの API とスキーマ参照](../../reference/reference.md)
    
