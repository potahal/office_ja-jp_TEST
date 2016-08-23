SharePoint アドイン モデル内の委任コントロール
================================================

概要
-------

新しい SharePoint アドイン モデルにおいて、コード内で委任コントロールを実装する方法は、完全信頼コードの場合とは異なります。一般的な完全信頼コード (FTC) / ファーム ソリューション シナリオにおいて、委任コントロールはユーザー コントロールまたは Web コントロールとして構築され、機能に登録され、SharePoint ソリューションを使用して展開されました。

SharePoint アドイン モデル シナリオでは、JavaScript が SharePoint ページに埋め込まれることにより、委任コントロールと同じ機能が実装されます。

大まかなガイドライン
---------------------

新しい SharePoint アドイン モデルにおける委任コントロールの作成については、経験に基づく次のような大まかなガイドラインが提供されています。

- SharePoint アドイン モデルには、委任コントロールを直接置き換えるものはありません。
- エンドユーザーの視点から委任コントロールと同じ機能を実装するには、埋め込み JavaScript を使用します。
- SharePoint のデータやサービスとやり取りするには、SharePoint JavaScript クライアント側オブジェクト モデル (JSOM) や SharePoint/Office365 REST API を使用します。

カスタム ユーザー アクションを使用してすべての SharePoint ページに JavaScript を埋め込む方法と、ページ レイアウトやマスター ページに JavaScript を直接埋め込む方法については、「[ユーザー コントロールと Web コントロール (SharePoint アドイン モデル レシピ)](user-controls-and-web-controls-sharepoint-add-in.md)」を参照してください。

関連リンク
=============
- [クロスサイト コレクション ナビゲーション (O365 PnP ビデオ)](http://channel9.msdn.com/blogs/OfficeDevPnP/Cross-site-collection-navigation)
- ガイダンス記事: [http://aka.ms/OfficeDevPnPGuidance](http://aka.ms/OfficeDevPnPGuidance "ガイダンス記事")
- MSDN リファレンス: [http://aka.ms/OfficeDevPnPMSDN](http://aka.ms/OfficeDevPnPMSDN "MSDN リファレンス")
- ビデオ: [http://aka.ms/OfficeDevPnPVideos](http://aka.ms/OfficeDevPnPビデオ"Videos")

関連する PnP サンプル
===================
- [OD4B.NavLinksInjection (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/OD4B.NavLinksInjection)
- [http://aka.ms/OfficeDevPnP](http://aka.ms/OfficeDevPnP) にあるサンプルとコンテンツ

適用対象
==========
- Office 365 マルチテナント (MT)
- Office 365 専用 (D)
- SharePoint 2013 オンプレミス
