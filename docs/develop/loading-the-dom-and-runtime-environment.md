
# DOM とランタイム環境を読み込む
アドインでは、DOM と Office アドイン両方のランタイム環境が、独自のカスタム ロジックを実行する前に読み込まれていることを確認する必要があります。 

 _ **適用対象:** Access apps for SharePoint?| apps for Office?| Excel?| Office Add-ins?| Outlook?| PowerPoint?| Project?| Word_


## コンテンツまたは作業ウィンドウ アドインの起動



**図 1. Excel、PowerPoint、Project、Word、または Access でコンテンツ アドインまたは作業ウィンドウ アドインの起動に関連するイベントのフロー**

![コンテンツ/タスク ウィンドウ アプリ開始時のイベントのフロー](../../images/off15appsdk_LoadingDOMAgaveRuntime.png)???

次に、コンテンツまたは作業ウィンドウ アドインの起動および初期化に関連するイベントをまとめます。 



1. ユーザーは、既にアドインが含まれているドキュメントを開くか、ドキュメントにアドインを挿入します。
    
2. Office ホスト アプリケーションが、アドインの XML マニフェストを Office ストア、SharePoint のアドイン カタログ、またはアドインの発生元である共有フォルダー カタログから読み取ります。
    
3. Office ホスト アプリケーションが、ブラウザー コントロールにアドインの HTML ページを開きます。
    
    次の手順 4. と 5. は、同時に実行されることも、同時に実行されないこともあります。したがって、次の処理に進む前に、DOM とアドイン ランタイム環境の両方の読み込みが完了したことをアドインのコードで確認する必要があります。
    
4. ブラウザー コントロールが、DOM と HTML 本文を読み込み、 **window.onload** イベントに対するイベント ハンドラーを呼び出します。
    
5. Office ホスト アプリケーションがランタイム環境を読み込みます (このランタイム環境は、コンテンツ配布ネットワーク (CDN) サーバーから JavaScript API for JavaScript ライブラリ ファイルをダウンロードしてキャッシュします)。その後、 [Office](http://msdn.microsoft.com/library/727adf79-a0b5-48d2-99c7-6642c2c334fc%28Office.15%29.aspx) オブジェクトの [initialize](http://msdn.microsoft.com/library/c490b13d-ee52-4291-af5d-f4a5a11d3af0%28Office.15%29.aspx) イベントに対するアドインのイベント ハンドラーを呼び出します。
    
6. DOM と HTML 本文の読み込み、およびアドインの初期化が完了すると、アドインのメイン関数は処理を続行できます。
    

## Outlook アドインの起動



**図 2. デスクトップ、タブレット、スマートフォンで実行される Outlook アドインの起動に関係するイベントのフロー**

![Outlook メール アプリ開始時のイベントのフロー](../../images/olowawecon15_LoadingDOMAgaveRuntime.png)???

次に、Outlook アドインを起動する際に実行される一連のイベントを示します。 



1. Outlook は起動時に、ユーザーの電子メール アカウント用にインストールされている Outlook アドインの XML マニフェストを読み取ります。
    
2. ユーザーが Outlook でアイテムを選択します。
    
3. 選択されたアイテムが Outlook アドインのアクティブ化条件を満たしている場合は、Outlook がアドインをアクティブにし、ボタンをユーザー インターフェイスに表示します。
    
4. ユーザーがボタンをクリックして Outlook アドインを起動すると、Outlook がアプリの HTML ページをブラウザー コントロール内に表示します。次の手順 5 と 6 は同時に行われます。
    
5. ブラウザー コントロールが DOM と HTML 本文を読み込んで、 **onload** イベントに対するイベント ハンドラーを呼び出します。
    
6. Outlook がアドインの [Office](http://msdn.microsoft.com/library/727adf79-a0b5-48d2-99c7-6642c2c334fc%28Office.15%29.aspx) オブジェクトの [initialize](http://msdn.microsoft.com/library/c490b13d-ee52-4291-af5d-f4a5a11d3af0%28Office.15%29.aspx) イベントに対するイベント ハンドラーを呼び出します。
    
7. DOM および HTML 本文の読み込みが終わると、アドインは初期化を完了し、アドインのメイン関数は処理を続行できます。
    

## 読み込み状態のチェック


DOM と ランタイム環境の両方の読み込みが完了したことを確認する方法の 1 つに、jQuery [.ready()](http://api.jquery.com/ready/)関数の  `$(document).ready()` を使用する方法があります。たとえば、次の **initialize** イベント ハンドラー関数は、アプリを初期化する固有のコードを実行する前に、DOM が読み込まれていることを確認します。その後、 **initialize** イベント ハンドラーは [mailbox.item](../reference/outlook/Office.context.mailbox.item.md%28Office.15%29.md) プロパティを使用して、Outlook で現在選択されているアイテムを取得し、アプリのメイン関数 `initDialer` を呼び出します。


```
Office.initialize = function () {
    // Checks for the DOM to load.
    $(document).ready(function () {
        // After the DOM is loaded, add-in-specific code can run.
        var mailbox = Office.context.mailbox;
        _Item = mailbox.item;
        initDialer();
    });
}
```

この方法は、任意の Office アドインの  **initialize** ハンドラーで使用できます。

ダイヤラー サンプル Outlook アドインでは、JavaScript のみを使用してこれらと同じ条件を確認するという少し異なる方法を使用しています。 

 **重要:** アドインに実行する初期化タスクがない場合でも、次の例のような最小のイベント ハンドラー関数 **Office.initialize** が少なくとも 1 つ含まれている必要があります。




```
Office.initialize = function () {
};
```

イベント ハンドラー  **Office.initialize** を含めていないと、アドインの起動時にエラーが発生するおそれがあります。また、ユーザーが Excel Online、PowerPoint Online、または Outlook Web App などの Office Online Web クライアントでアドインを使用しようとする場合、アプリの実行が失敗します。

アドインに複数のページが含まれる場合、新しいページが読み込まれるときに、そのページに  **Office.initialize** イベント ハンドラーが含まれるか、そのページからこのイベント ハンドラーを呼び出されなければなりません。


## その他のリソース



- [JavaScript API for Office について](../develop/understanding-the-javascript-api-for-office.md)
    
