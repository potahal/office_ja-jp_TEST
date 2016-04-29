
# Office アドイン
Office アドイン の作業を開始するにあたり、ブラウザーから Napa Office 365 開発ツール を使用して、Excel、Outlook、PowerPoint、Project、Word のいずれかの基本的なコンテンツ、作業ウィンドウ、Outlook のアドインを作成します。

 _ **適用対象:** Access apps for SharePoint?| apps for Office?| Excel?| Office Add-ins?| Outlook?| PowerPoint?| Project?| Word_

[Office アドイン](../../docs/develop/privacy-and-security.md)は、ブラウザー コントロールまたは iframe でホストされる Web アプリケーションで、Office アプリケーションのコンテキストで実行されます。アドインは、現在のドキュメントまたはメール アイテム内のデータにアクセスし、Web サービスや他の Web ベースのリソースに接続できます。アドインを開発するには、HTML5、JavaScript、CSS3、XML、REST API などの Web 標準のテクノロジを使用します。アドインが、Office ホスト アプリケーションを実行するコンピューター上に実際にインストールされることはありません。実装は Web サーバー上でホストされるので、サーバーから簡単に保守と更新を行えます。

Napa を使用して簡単な Office アドインを作成できます。

そのためには、以下のものが必要になります。

- [Microsoft アカウント](http://www.microsoft.com/ja-jp/msaccount/default.aspx)
    
- [Napa Office 365 開発ツール](https://www.napacloudapp.com/) Web アプリの URL
    

## 基本的なアドインの作成



1. ブラウザーで [Napa Office 365 開発ツール](https://www.napacloudapp.com/) Web アプリを開きます。
    
2. [ **新しいプロジェクトの追加**] タイルをクリックします。
    
     **注意:** [ **新しいプロジェクトの追加**] タイルは、他のプロジェクトを既に作成している場合にのみ表示されます。これが最初のプロジェクトの場合は、次の手順に進んでください。
    
    ![プロジェクト ページ](../../images/08fc36cf-7cc1-442f-a9a5-b6bb30d786a4.png)

3. 作成するアドインの種類、プロジェクトの名前を選択し、 [ **作成**] ボタンをクリックします。 
    
    ![Excel アプリ タイル](../../images/Apps_NAPA_Excel_Tile.png)
    コード エディターが開き、既定の Web ページが表示されます。このページには、追加操作を行わないでも実行可能なコード サンプルが最初から含まれています。
    
4. ページの横に配置されている [実行] (
![[実行] ボタン](../../images/Apps_NAPA_Run_Button.png)) ボタンをクリックします。
    
    選択した種類のアドインと関連付けられている Office アプリケーションが開き、サンプル アドイン が表示されます。 これで、アドインの機能を試すことができます。
    

## 次の手順


これで最初のアドインを作成しました。さらに詳しい次のいくつかの例をご覧ください。


- [Napa Office 365 開発ツールを使用して Excel 用コンテンツ アドインを作成する](create-a-content-add-in-with-napa.md)
    
- [Napa Office 365 開発ツールを使用して作業ウィンドウ アドインを作成する](create-a-task-pane-add-in-with-napa.md)
    
Visual Studio またはテキスト エディターを使用して Office アドインを作成することもできます。詳しくは、以下をご覧ください。


- [Visual Studio を使用して作業ウィンドウ アドインまたはコンテンツ アドインを作成する](create-a-task-pane-or-content-add-in-with-visual-studio.md)
    
- [テキスト エディターを使用して Word 用や Excel 用の作業ウィンドウ アドインやコンテンツ アドインを作成する](create-a-task-pane-or-content-add-in-for-word-or-excel-by-using-a-text-editor.md)
    

## このセクションの内容



- [Office アドイン プラットフォームの概要](../../docs/develop/privacy-and-security.md)
    
- [Office アドインを実行するための要件](../../docs/overview/requirements-for-running-office-add-ins.md)
    
- [Office アドインの設計ガイドライン](../add-in-design.md)
    
- [Office アドインの開発ライフ サイクル](../../docs/design/add-in-development-lifecycle.md)
    
- [Office アドインを発行する](../publish/publish.md)
    
- [Office 用アドインのコード サンプル](code-samples.md)
    
- [Office アドインの API とスキーマ参照](../../reference/reference.md)
    

## その他の技術情報



- [Office ストアに Office アドインと SharePoint アドインおよび Office 365 Web アプリを提出する](http://msdn.microsoft.com/library/ff075782-1303-4517-91cc-b3d730e9b9ae%28Office.15%29.aspx)
    
- [Office デベロッパー プラットフォームに関するフィードバックの送信](http://officespdev.uservoice.com/)
    
- [Office アドインのフォーラムに質問を投稿する](http://social.msdn.microsoft.com/Forums/officeapps/ja-JP/home?forum=appsforoffice%2Cofficestore&amp;filter=alltypes&amp;sort=lastpostdesc)
    
