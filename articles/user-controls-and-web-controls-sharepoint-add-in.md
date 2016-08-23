SharePoint アドイン モデルにおけるユーザー コントロールと Web コントロール
=============================================================

概要
-------

新しい SharePoint アドイン モデルにおいて、コード内にカスタム コントロールを実装する方法は、完全信頼コードの場合とは異なります。一般的な完全信頼コード (FTC) / ファーム ソリューション シナリオにおいて、カスタム コントロールはユーザー コントロールまたは Web コントロールとして構築され、SharePoint ソリューションを使用して展開されていました。

SharePoint アドイン モデル シナリオでは、カスタム コントロールを実装するため、JavaScript は SharePoint ページに埋め込まれています。

基本ガイドライン
---------------------

新しい SharePoint アドイン モデルにおけるカスタム コントロールの作成については、大まかに次のような基本ガイドラインが提供されています。

- 埋め込み JavaScript を使用して、カスタム コントロールを作成します。
- SharePoint のデータやサービスとやり取りするには、SharePoint ECMA クライアント側オブジェクト モデル (CSOM) や SharePoint/Office 365 REST API を使用します。

SharePoint ページに JavaScript を埋め込むオプション
-----------------------------------------------
SharePoint ページに JavaScript を埋め込むには、いくつかのオプションがあります。

- カスタム ユーザー アクションを使用する
- JavaScript をページ レイアウトに直接埋め込む
- JavaScript をカスタム マスター ページを直接 埋め込む (非推奨)

カスタム ユーザー アクションを使用する
-----------------------
このパターンでは、実行時に JavaScript をページに埋め込むために、カスタム ユーザー アクションが使用されます。

- この方法は、完全にサポートされている有効なアプローチです。

**適切な場合**

すべての SharePoint ページに JavaScript を埋め込む必要がある場合は、このオプションが最適です。

**作業の開始**

以下の記事および付随するビデオでは、カスタム ユーザー アクションを使用して SharePoint ページに JavaScript を埋め込む方法について説明します。

- [Core.EmbedJavaScript (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.EmbedJavaScript)
- [OD4B.NavLinksInjection (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/OD4B.NavLinksInjection)
- [クロスサイト コレクション ナビゲーション (O365 PnP ビデオ)](http://channel9.msdn.com/blogs/OfficeDevPnP/Cross-site-collection-navigation)

JavaScript をページ レイアウトに直接埋め込む
-------------------------------------------

このパターンでは、JavaScript が発行サイトのページ レイアウトに直接埋め込まれます。  

- この方法は、完全にサポートされている有効なアプローチです。
- この方法は、発行サイトで機能します。

**適切な場合**

WCM シナリオで、発行サイト内の特定の SharePoint ページ レイアウトに JavaScript を埋め込む必要がある場合は、このオプションが最適です。

JavaScript をカスタム マスター ページを直接埋め込む
--------------------------------------------------

このパターンでは、JavaScript がカスタム マスター ページに直接埋め込まれます。  

- この方法は非推奨です。
- この方法は有効です。
- カスタム マスター ページに JavaScript を直接埋め込むことができますが、長期にわたる追加コストと将来の更新に関する課題が生じることに留意してください。
    + カスタム マスター ページを使用する場合は、Office 365 に主要な機能更新が適用されたときにカスタム マスター ページに変更を適用する準備をしておいてください。

**適切な場合**

マスター ページごとに JavaScript を埋め込む必要がある場合、JavaScript をどのマスター ページに埋め込むかをコントロールできるため、このオプションが適しています。

関連リンク
=============
- [クロスサイト コレクション ナビゲーション (O365 PnP ビデオ)](http://channel9.msdn.com/blogs/OfficeDevPnP/Cross-site-collection-navigation)
- ガイダンス記事: [http://aka.ms/OfficeDevPnPGuidance](http://aka.ms/OfficeDevPnPGuidance "ガイダンス記事")
- MSDN リファレンス: [http://aka.ms/OfficeDevPnPMSDN](http://aka.ms/OfficeDevPnPMSDN "MSDN リファレンス")
- ビデオ: [http://aka.ms/OfficeDevPnPVideos](http://aka.ms/OfficeDevPnPビデオ"Videos")

関連する PnP サンプル
===================
- [Core.EmbedJavaScript (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.EmbedJavaScript)
- [OD4B.NavLinksInjection (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/OD4B.NavLinksInjection)
- [Core.EmbedJavaScript.WeekNumbers (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.EmbedJavaScript.WeekNumbers)
- [Core.EmbedJavaScriptJSOM (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.EmbedJavaScriptJSOM)
- [Core.JavaScriptCustomization (PnP コア コンポーネントを使用した O365 PnP シナリオ)](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.JavaScriptCustomization)
- [http://aka.ms/OfficeDevPnP](http://aka.ms/OfficeDevPnP) にあるサンプルとコンテンツ

適用対象
==========
- Office 365 マルチテナント (MT)
- Office 365 専用 (D)
- SharePoint 2013 オンプレミス
