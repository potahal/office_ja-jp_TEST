SharePoint アドイン モデルのバリエーション
=========================================

概要
-------

新しい SharePoint アドイン モデルにおいて、バリエーションの構成方法は、完全信頼コードの場合とは異なります。一般的な完全信頼コード (FTC) / ファーム ソリューション シナリオでは、SharePoint サーバー側オブジェクト モデル (Microsoft.SharePoint.Publishing.Variations) は、バリエーションの構成に使用され、SharePoint ソリューション経由で展開されたコードを実行する機能でした。

SharePoint アドイン モデル シナリオでは、SharePoint クライアント側オブジェクト モデル (CSOM) または REST API を使用してバリエーションを構成します。このパターンは、一般的に *リモート プロビジョニング パターン* と呼ばれます。

基本ガイドライン
---------------------

新しい SharePoint アドイン モデルにおけるバリエーションの構成については、大まかに次のような基本ガイドラインが提供されています。

- 可能な限り、SharePoint クライアント側オブジェクト モデル (CSOM) API を使用してバリエーションを構成します。
    - .Net クライアント側オブジェクト モデル [バリエーション クラス (MSDN API ドキュメント)](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.publishing.variations.aspx)
    - JavaScript クライアント側オブジェクト モデル [バリエーション クラス (MSDN API ドキュメント)](https://msdn.microsoft.com/en-us/library/office/jj994778.aspx)
- 一部のバリエーション構成設定は、上記の SharePoint CSOM API では使用できません。
- 上記 CSOM API のバリエーション クラス以外に、いくつかのバリエーション設定を提供および構成できます。これを行うには、サイト プロパティ バッグに保存されているバリエーション設定の値を設定する、またはバリエーションと関連付られたリストでアイテムを編集します。
    + 「[VariationsExtensions.cs クラス (O365 PnP サンプル)](https://github.com/OfficeDev/PnP-Sites-Core/tree/master/Core/OfficeDevPnP.Core/AppModelExtensions/VariationExtensions.cs)」には、プロパティ バッグとリスト アイテムの値を変更してバリエーション設定を構成するいくつかの例が含まれています。
    + 「[VariationsExtensions.cs クラス (O365 PnP サンプル)](https://github.com/OfficeDev/PnP-Sites-Core/blob/master/Core/OfficeDevPnP.Core/AppModelExtensions/VariationExtensions.cs)」は、バリエーション設定で設定可能な ***すべての設定*** の構成方法を示しています。
    + 「[自分のサイトのバリエーションを作成できるよう,バリエーション設定を有効にする (O365 サポート記事)](https://support.office.com/en-za/article/Turn-on-variations-settings-so-you-can-create-variations-of-your-site-fc021610-bdb5-4b5c-9d59-ce8af6699b4b)は、バリエーション設定 ページで構成可能なアイテムを示し、それらが何をできるかについて説明します。

関連リンク
=============

- ガイダンス記事: [http://aka.ms/OfficeDevPnPGuidance](http://aka.ms/OfficeDevPnPGuidance "ガイダンス記事")
- MSDN リファレンス: [http://aka.ms/OfficeDevPnPMSDN](http://aka.ms/OfficeDevPnPMSDN "MSDN リファレンス")
- ビデオ: [http://aka.ms/OfficeDevPnPVideos](http://aka.ms/OfficeDevPnPビデオ"Videos")

関連する PnP サンプル
===================

- [VariationsExtensions.cs クラス (O365 PnP サンプル)](https://github.com/OfficeDev/PnP-Sites-Core/blob/master/Core/OfficeDevPnP.Core/AppModelExtensions/VariationExtensions.cs)
- [http://aka.ms/OfficeDevPnP](http://aka.ms/OfficeDevPnP) にあるサンプルとコンテンツ

適用対象
==========
- Office 365 マルチテナント (MT)
- Office 365 専用 (D)
- SharePoint 2013 オンプレミス
