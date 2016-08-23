SharePoint アドイン モデルでの検索の設定
===================================================

概要
-------

新しい SharePoint アドイン モデルにおいて、検索を構成する方法は、完全信頼コードの場合とは異なります。一般的な完全信頼コード (FTC) / ファーム ソリューション シナリオでは、SharePoint サーバー側オブジェクト モデル コードは検索に使用するよう設定され、SharePoint ソリューション経由で展開されていました。

SharePoint アドイン モデル シナリオでは、SharePoint クライアント側オブジェクト モデル (CSOM) または REST API を使用して検索を構成します。このパターンは、一般的に *リモート プロビジョニング パターン* と呼ばれます。

基本ガイドライン
---------------------

新しい SharePoint アドイン モデルにおける検索の設定については、大まかに次のような基本ガイドラインが提供されています。

- 可能な限り、検索構成の設定をインポートおよびエクスポートすることにより、SharePoint クライアント側オブジェクト モデル (CSOM) API を使用して検索を構成します。
- 検索構成設定の一部は、SharePoint CSOM API 経由では現在利用できません。
    + エクスポートおよびインポート可能な検索構成の設定のリストについては、「[SharePoint Server 2013 でのカスタム検索構成設定のエクスポートとインポート (TechNet 記事)](https://technet.microsoft.com/en-us/library/jj871675.aspx#BKMK_2)」を参照してください。
    + CSOM を使用して検索構成を設定できない場合、構成値を設定するには管理ユーザー  インターフェイスが必要です。
- SharePoint REST API は (現時点で) 検索構成設定のインポートおよびエクスポートには対応していません。

**作業の開始**

次の例は、SharePoint テナント、サイト コレクションおよびサイトの間で検索設定をインポートおよびエクスポートする方法を示します。

- [Core.SearchSettingsPortability (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.SearchSettingsPortability)

関連リンク
=============

- ガイダンス記事: [http://aka.ms/OfficeDevPnPGuidance](http://aka.ms/OfficeDevPnPGuidance "ガイダンス記事")
- MSDN リファレンス: [http://aka.ms/OfficeDevPnPMSDN](http://aka.ms/OfficeDevPnPMSDN "MSDN リファレンス")
- ビデオ: [http://aka.ms/OfficeDevPnPVideos](http://aka.ms/OfficeDevPnPビデオ"Videos")

関連する PnP サンプル
===================

- [Core.SearchSettingsPortability (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.SearchSettingsPortability)
- [http://aka.ms/OfficeDevPnP](http://aka.ms/OfficeDevPnP) にあるサンプルとコンテンツ

適用対象
==========
- Office 365 マルチテナント (MT)
- Office 365 専用 (D)
- SharePoint 2013 オンプレミス
