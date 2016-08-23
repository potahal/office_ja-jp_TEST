SharePoint アドイン モデル内のユーザー設定フィールド型
================================================

概要
-------

新しい SharePoint アドイン モデルでカスタマイズされたエンド ユーザー エクスペリエンスを提供する方法は、完全信頼コードの場合とは異なります。一般的な完全信頼コード (FTC) / ファーム ソリューション シナリオにおいて、ユーザー設定フィールド型は、SharePoint サーバー側オブジェクト モデル コードでいずれかの組み込みフィールド型クラスを継承してフィールド型展開ファイル (XML) を作成することにより作成されました。これらのコンポーネントは、SharePoint ソリューションを使用して展開されました。 

SharePoint アドイン モデル シナリオにおいて、カスタマイズされたエンド ユーザー エクスペリエンスは、クライアント側レンダリングを使用して実装されます。このアプローチでは、カスタマイズされたエンド ユーザー エクスペリエンスを実装するために JavaScript ファイルを使用します。リモート プロビジョニング パターンでは、JavaScript ファイルを展開し、JSLink プロパティを使用して SharePoint フィールドに登録します。

エンド ユーザーの視点からは、能力 / 結果は同じように見えます。

Google マップを実装するユーザー設定フィールド型のいくつかの例を、次に示します。これらは Office 365 PnP サンプルの [Branding.JSLink](https://github.com/OfficeDev/PnP/tree/master/Samples/Branding.JSLink) から取られています。

**リスト ビューに表示された Google マップのサムネイル画像:**

![Microsoft Campus のロケーション ポイントとロケーション エリアを表示する 2 つの Google マップ ビュー。](https://github.com/OfficeDev/PnP/blob/master/Samples/Branding.JSLink/readme-images/GoogleMaps.png)


                **Google マップの大きめのサムネイルを表示するインライン編集: **
![2 つの Google マップ。 1 つのビューには Microsoft Campus のロケーション ポイントと、選択した場所へのリンクが表示されます。もう 1 つビューには、Microsoft Campus のロケーション エリアと [Edit Shape] へのリンクが表示されます。](https://github.com/OfficeDev/PnP/blob/master/Samples/Branding.JSLink/readme-images/GoogleMaps_Edit.png)


                **インライン編集が可能なダイアログ:**
![Microsoft Campus の図形を表示するGoogle マップ。 イメージ上のテキストには、「Click on the map to place markers and create your shape (マップをクリックしてマーカーを配置し、図形を作成します)」と書かれています。 最初のマーカーをクリックして終了します。 各マーカーをドラッグしたり、マーカーをクリックして別のオプションを使用できます。 上部の [Clear Map] ボタンを使用すると、すべてのマーカーを削除できます。](https://github.com/OfficeDev/PnP/blob/master/Samples/Branding.JSLink/readme-images/GoogleMaps_Shape_Edit.png)

基本ガイドライン
---------------------

クライアント側レンダリングの実装については、大まかに次のような基本ガイドラインが提供されています。

- ユーザー設定フィールド型を実装するには、JavaScript ファイルとクライアント側レンダリングを使用します。
- JavaScript ファイルを展開して SharePoint フィールドやリスト ビュー Web パーツに登録するには、リモート プロビジョニング パターンを使用します。
- ダウンロード最小化戦略 (MDS) エンジンがカスタムのレンダリング JavaScript ファイルを認識できるようにするには、MDS エンジンに JavaScript ファイルを登録します。
- JSLink プロパティをホスト Web に設定するには少なくとも Web レベルでの完全アクセス許可が必要であるため、このアプローチは SharePoint ストアにあるアドインには適していません。

JSLink プロパティを使用して JavaScript ファイルでクライアント側レンダリングを実装するためのオプション
----------------------------------------------------------------------------------------

JSLink プロパティを使用して JavaScript ファイルでクライアント側レンダリングを実装するためのオプションは、複数あります。

- SharePoint リストのビューをレンダリングするリスト ビュー Web パーツで JSLink プロパティを設定します。 
- SharePoint フィールド用の JSLink プロパティを設定します。 
- SharePoint コンテンツ タイプ用の JSLink プロパティを設定します。 
    

SharePoint リストのビューをレンダリングするリスト ビュー Web パーツで JSLink プロパティを設定する
-----------------------------------------------------------------------------------------
このオプションでは、WebPartDefinition で JSLink プロパティを設定します。
    
- **このアプローチでは、SPField レベルで具体的にユーザー設定フィールド型を作成することはありません。**
    + そのため、*カスタム レンダリングは JSLink プロパティを設定するリスト ビュー Web パーツでのみ適用されます。*
- このアプローチでは、一度に 1 つ以上の SharePoint フィールドのレンダリングを変更できます。
- このアプローチは、宣言型コード、SharePoint サーバー側オブジェクト モデル、SharePoint クライアント側オブジェクト モデル (CSOM)、または PowerShell を使用して実行できます。
    + リモート プロビジョニング パターンを使用して JSLink プロパティを設定するには、SharePoint サーバー側オブジェクト モデル、SharePoint クライアント側オブジェクト モデル、または PowerShell を使用することをお勧めします。

**適切な場合**

SharePoint リスト データ用の特定のビューを定義して複数の SharePoint のフィールドのレンダリングを変更する必要がある場合、これは適切なオプションです。

**作業の開始**

次のサンプルでは、SharePoint リスト ビュー Web パーツで JSLink プロパティを設定します。

- [Branding.ClientSideRendering (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Branding.ClientSideRendering)
    + SharePoint リスト ビュー Web パーツで JSLink プロパティを設定する 9 つのサンプルと、各サンプルの実装方法の説明が含まれています。
    + RegisterJStoWebPart メソッドにより、リスト ビュー Web パーツの JSLink プロパティが設定されます。 

SharePoint フィールド用の JSLink プロパティを設定する
----------------------------------------------

このオプションでは、SPField で JSLink プロパティを設定します。
    
- **このアプローチでは、SPField レベルで具体的に JSLink プロパティを登録します。**
    + そのため、*カスタム レンダリングは SPField がレンダリングされるすべての場所で適用されます*。
- このアプローチでは、1 つの SharePoint フィールドのレンダリングを変更できます。
- このアプローチは、宣言型コード、SharePoint サーバー側オブジェクト モデル、SharePoint クライアント側オブジェクト モデル、または PowerShell を使用して実行できます。
    + リモート プロビジョニング パターンを使用して JSLink プロパティを設定するには、SharePoint サーバー側オブジェクト モデル、SharePoint クライアント側オブジェクト モデル、または PowerShell を使用することをお勧めします。

**適切な場合**

特定の SharePoint フィールド用の特定のビューを定義し、そのフィールドのレンダリング時にそのビューが確実に使用されるようにする必要がある場合、これは適切なオプションです。

**作業の開始**

以下の記事は、SPField で JSLink プロパティを設定する方法の例を示しています。

- [JSLink プロパティの使用による、SharePoint 2013 でのフィールドやビューのレンダリング方法の変更(Tobias Zimmergren)](http://zimmergren.net/technical/sp-2013-using-the-spfield-jslink-property-to-change-the-way-your-field-is-rendered-in-sharepoint-2013)
- [SharePoint 2013 と JSLink を使用する (MSDN マガジン)](https://msdn.microsoft.com/en-us/magazine/dn745867.aspx)

JSLink プロパティを使用して JavaScript ファイルでクライアント側レンダリングを実装する場合の課題
------------------------------------------------------------------------------------------------

カスタムのクライアント側レンダリング コンポーネントを開発する場合は、次のことに注意してください。

- すべての SharePoint フィールドが JSLink プロパティでオーバーライドされるとは限りません。
    + TaxonomyField は良い例です。
- JSLink でいくつかのトークンがサポートされています。
    + _layouts
    + _site
    + _siteCollection
    + _siteCollectionLayouts
    + _siteLayouts
- JSLink JavaScript ファイルを SharePoint スクリプト オンデマンド (SOD) フレームワークに登録して、ファイルを遅延読み込みすることができます。
    - SOD にファイルを登録するには、JSLink URL の末尾で (d) タグを使用します。
 
    ```
    ~sitecollection/Style Library/JSLink-Samples/DependentFields.js(d)
    ```
- JSLink プロパティを使用して複数の JavaScript ファイルを読み込むことができます。
    + クライアント側レンダリングを実装する JavaScript ファイルのライブラリがある場合、これは特に役立ちます。
    + 特定の SharePoint フィールドのクライアント側レンダリングを実装するために必要な JavaScript だけを提供することができるため、モバイル デバイスを対象とするときはこのアプローチの使用を検討してください。
    + 読み込む JavaScript ファイルを区切るには、文字 | を使用します。 
    
    ```
    ~sitecollection/Style Library/JSLink-Samples/MainLibrary.js|~sitecollection/Style Library/JSLink-Samples/SpecificField.js**(d)**
    ```

関連リンク
=============
- [SPField.JSLink プロパティ (MSDN API ドキュメント)](https://msdn.microsoft.com/en-us/library/microsoft.sharepoint.spfield.jslink.aspx)
- [JSLink プロパティの使用による、SharePoint 2013 でのフィールドやビューのレンダリング方法の変更(Tobias Zimmergren)](http://zimmergren.net/technical/sp-2013-using-the-spfield-jslink-property-to-change-the-way-your-field-is-rendered-in-sharepoint-2013)
- [SharePoint 2013 と JSLink を使用する (MSDN マガジン)](https://msdn.microsoft.com/en-us/magazine/dn745867.aspx)
- ガイダンス記事: [http://aka.ms/OfficeDevPnPGuidance](http://aka.ms/OfficeDevPnPGuidance "ガイダンス記事")
- MSDN リファレンス: [http://aka.ms/OfficeDevPnPMSDN](http://aka.ms/OfficeDevPnPMSDN "MSDN リファレンス")
- ビデオ: [http://aka.ms/OfficeDevPnPVideos](http://aka.ms/OfficeDevPnPビデオ"Videos")

関連する PnP サンプル
===================

- [Branding.ClientSideRendering (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Branding.ClientSideRendering)
- [Branding.JSLink (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Branding.JSLink)
- https://github.com/OfficeDev/PnP にあるサンプルとコンテンツ

適用対象
==========
- Office 365 マルチテナント (MT)
- Office 365 専用 (D) *一部*
- SharePoint 2013 オンプレミス - *一部*

*専用およびオンプレミスの場合のパターンは SharePoint アドイン モデル手法と同じですが、使用可能なテクノロジは異なる可能性があります。*
