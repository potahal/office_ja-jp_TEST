SharePoint アドイン モデル内のカスタム アクション
=============================================

概要
-------

新しい SharePoint アドイン モデルで SharePoint 内のリスト アイテム メニューとリボンを変更する方法は、完全信頼コードの場合とは異なります。一般的な完全信頼コード (FTC) / ファーム ソリューション シナリオにおいて、リスト アイテム メニューとリボンの変更内容は XML (カスタム アクション) で定義され、機能内にパッケージ化され、SharePoint ソリューションを使用して展開されます。

SharePoint アドイン モデル シナリオでは、SharePoint クライアント側オブジェクト モデル (CSOM) または REST API を使用して、リスト アイテム メニューとリボンを変更するカスタム アクションを作成します。このパターンは、一般的に *リモート プロビジョニング パターン* と呼ばれます。

基本ガイドライン
---------------------

新しい SharePoint アドイン モデルにおけるカスタム アクションの作成と展開については、大まかに次のような基本ガイドラインが提供されています。

- カスタム アクションを使用して、リスト アイテム メニューとリボンを変更することができます。
- カスタム アクションを実装するアドインから直接カスタム アクションを使用してメニュー項目を非表示にすることはできません。
    + これは、[HideCustomAction 要素 (MSDN API ドキュメント)](https://msdn.microsoft.com/en-us/library/office/ms414790.aspx) が、SharePoint ECMA クライアント側オブジェクト モデル (CSOM) - [UserCustomAction プロパティ (MSDN API ドキュメント)](https://msdn.microsoft.com/en-us/library/microsoft.sharepoint.client.usercustomaction_properties.aspx)、または SharePoint/Office 365 REST API - [SP.UserCustomActionCollection オブジェクト (sp.js) (MSDN API ドキュメント)](https://msdn.microsoft.com/en-us/library/office/jj247124.aspx) で使用できないためです。
    + メニュー項目を非表示にする必要がある場合は、カスタム アクションを使用して JavaScript またはカスタマイズ済み CSS を SharePoint ページに埋め込む必要があります。SharePoint ページに埋め込まれた JavaScript または CSS により、メニュー項目は非表示になります。
- カスタム アクションを実装するには、SharePoint クライアント側オブジェクト モデル (CSOM) や SharePoint/Office 365 REST API を使用します。

**作業の開始**

次のサンプルでは、ホスト Web のサイト設定メニューにカスタム アクションを追加する方法、カスタム アクションでダイアログ ボックスを表示する方法、リモート アドイン Web からページをホストするダイアログを非表示にする方法、およびカスタム アクションを使用してリストの作成と Web テーマの設定を行う方法の例を示しています。

- [Provisioning.SiteModifier (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Provisioning.SiteModifier)

    ここでは、[サイトの設定] メニューにサンプルのカスタム アクションのリンクが追加されたことを確認できます。
    
    ![[Office 365 の設定] メニューが表示され、[サイトの変更] メニュー オプションが強調表示されます。](media/Recipes/CustomActions/Custom-Action-In-Site-Settings.png)
    
    ここでは、[サイトの変更] のリンクからポップアップ ウィンドウが開くことを確認できます。
    
    ![[サイトの変更] ポップアップ ウィンドウが表示されます。このウィンドウには、[リスト] という名前のチェック ボックスのグループと、その中に 2 つのチェック ボックス ([プロジェクト] と [連絡先]) があります。 [リスト] の下には [各種] という名前のチェック ボックスのグループがあります。このグループには [テーマの適用] という名前のチェック ボックスがあります。 この下には 2 つのボタン ([確認] と [キャンセル]) があります。](media/Recipes/CustomActions/Custom-Action-Popup-Menu.png)

関連リンク
=============

- [ユーザー コントロールと Web コントロール(SharePoint アドイン レシピ)](user-controls-and-web-controls-sharepoint-add-in.md)
- ガイダンス記事: [http://aka.ms/OfficeDevPnPGuidance](http://aka.ms/OfficeDevPnPGuidance "ガイダンス記事")
- MSDN リファレンス: [http://aka.ms/OfficeDevPnPMSDN](http://aka.ms/OfficeDevPnPMSDN "MSDN リファレンス")
- ビデオ: [http://aka.ms/OfficeDevPnPVideos](http://aka.ms/OfficeDevPnPビデオ"Videos")

関連する PnP サンプル
===================

- [Provisioning.SiteModifier (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Provisioning.SiteModifier)
- [http://aka.ms/OfficeDevPnP](http://aka.ms/OfficeDevPnP) にあるサンプルとコンテンツ

適用対象
==========
- Office 365 マルチテナント (MT)
- Office 365 専用 (D)
- SharePoint 2013 オンプレミス
