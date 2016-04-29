
# Office ライブラリの JavaScript API をそのコンテンツ配信ネットワーク (CDN) から参照する
コンテンツ配信ネットワーク (CDN) の場所から JavaScript API for Office ライブラリ (Office.js および関連のアプリケーション固有の .js ファイル) を参照します。

 _ **適用対象:** Access apps for SharePoint?| apps for Office?| Excel?| Office Add-ins?| Outlook?| PowerPoint?| Project?| Word_

[JavaScript API for Office](http://msdn.microsoft.com/library/b27e70c3-d87d-4d27-85e0-103996273298%28Office.15%29.aspx) ライブラリは Office.js ファイルと Excel-15.js や Outlook-15.js などの関連のホスト アプリケーション固有の .js ファイルから構成されています。任意の Office ホスト アプリケーション用に Office アドインを開発する場合、アプリの UI を実装する Web ページ (.html, .aspx、または .php ファイルなど) の `<head>` タグ内で JavaScript API for Office ライブラリを参照する必要があります。そうするには、 `src` 属性を次の CDN URL に設定した `script` タグを追加します。



```HTML
<script src="https://appsforoffice.microsoft.com/lib/1/hosted/Office.js" type="text/javascript"></script>
```

CDN URL で  `office.js` の前にある `/1/` は、Office.js のバージョン 1 内で最新の増分リリースを使用するよう指定します。 JavaScript API for Office は下位互換性を保持しているので、最新のリリースは以前にバージョン 1 で導入された API メンバーを引き続きサポートします。既存のプロジェクトをアップグレードする必要がある場合は、「 [JavaScript API for Office およびマニフェスト スキーマ ファイルのバージョンを更新する](../docs/develop/update-your-javascript-api-for-office-and-manifest-schema-version.md)」を参照してください。
アドインが初めて読み込まれると、Office.js およびアプリケーション固有の .js ファイルの最新の実装が アドインで使われるように、JavaScript API for Office ライブラリ ファイルがダウンロードされ、キャッシュされます。
[最新の Microsoft Office Developer Tools の更新プログラム](https://www.visualstudio.com/features/office-tools-vs)を使用して Visual Studio に指定される  **Office 用アドイン** プロジェクト テンプレート ファイルを使用して アドインを開発する場合は、プロジェクト内の既定の Home.html ファイルに、適切な `script` タグが含まれます。

## その他の技術情報



- [JavaScript API for Office について](../develop/understanding-the-javascript-api-for-office.md)
    
- [Office アドイン プラットフォームの概要](../../docs/develop/privacy-and-security.md)
    
- [Office アドインの開発ライフ サイクル](../../docs/design/add-in-development-lifecycle.md)
    
- [JavaScript API for Office](http://msdn.microsoft.com/library/b27e70c3-d87d-4d27-85e0-103996273298%28Office.15%29.aspx)
    
