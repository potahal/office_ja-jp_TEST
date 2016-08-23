SharePoint アドイン モデルの情報管理ポリシー
============================================================

概要
-------

新しい SharePoint アドイン モデルで情報管理ポリシーを適用するする方法は、完全信頼コードの場合とは異なります。一般的な完全信頼コード (FTC) / ファーム ソリューション シナリオでは、情報管理ポリシーは通常タイマー ジョブの一部として SharePoint サーバー側のオブジェクト モデルで管理および適用され、SharePoint ファーム ソリューションで展開されていました。 

SharePoint アドイン モデル シナリオでは、SharePoint クライアント側のオブジェクト モデル (CSOM) およびリモート タイマー ジョブは情報管理ポリシーの管理および適用に使用されます。

基本ガイドライン
---------------------

SharePoint サイトへの情報管理ポリシーの管理と適用については、大まかに次のような基本ガイドラインが提供されています。  

- 情報管理ポリシーをサイト コレクション レベルで定義するときには、サイト所有者によって無効化されている場合がある点に注意してください。
    + 情報管理ポリシーを設定するときにリモート モデルと CSOM を使用すると、サイト コレクションの所有者はそれらを無効にできません。リモート モデルは、エンタープライズわかりやすいモデルが SharePoint 環境全体で情報管理ポリシーを常に有効にする、より企業にやさしいモデルです。
- リモート タイマー ジョブで SharePoint CSOM を使用し、情報管理ポリシーを管理および適用します。

- SharePoint サイトで成果物を検査し、適切に情報管理ポリシーを適用する場合、大量のデータ セットや再帰クロールを操作するときに、大量のデータ セットを操作するときに Office 365 の SharePoint API のスロットル制限に違反していないことを確認してください。
    + [Core.Throttling (O365 Pnp サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.Throttling) は、Office 365 SharePoint の API スロットルを処理するインテリジェントなコードの作成方法を示しています。

**注:** 現在、CSOM には、コンテンツ タイプに基づいて保存期間を設定する (サイト上でのみ) 方法はありません。

**作業の開始**

次の O365 PnP コード サンプルとビデオは、SharePoint サイトに対して情報管理ポリシーを管理および適用する方法を示しています。この例では、コードは SharePoint サイト コレクション内のドキュメント ライブラリに適用するコンテンツ タイプを反復処理し、保持ポリシーを適用します。

- [Governance.ContentTypeEnforceRetention (O365 PnP のコード サンプル)](https://github.com/OfficeDev/PnP/tree/master/Solutions/Governance.ContentTypeEnforceRetention)

次のビデオでは、コードのサンプルについて説明します。

- [情報管理ポリシーとアプリケーション モデル (O365 PnP ビデオ)](http://channel9.msdn.com/blogs/OfficeDevPnP/Information-management-policy-wtih-app-model)

関連リンク
=============
- [情報管理ポリシーとアプリケーション モデル (O365 PnP ビデオ)](http://channel9.msdn.com/blogs/OfficeDevPnP/Information-management-policy-wtih-app-model)
- ガイダンス記事: [http://aka.ms/OfficeDevPnPGuidance](http://aka.ms/OfficeDevPnPGuidance "ガイダンス記事")
- MSDN リファレンス: [http://aka.ms/OfficeDevPnPMSDN](http://aka.ms/OfficeDevPnPMSDN "MSDN リファレンス")
- ビデオ: [http://aka.ms/OfficeDevPnPVideos](http://aka.ms/OfficeDevPnPビデオ"Videos")

関連する PnP サンプル
===================

- [Governance.ContentTypeEnforceRetention (O365 PnP のコード サンプル)](https://github.com/OfficeDev/PnP/tree/master/Solutions/Governance.ContentTypeEnforceRetention)
- https://github.com/OfficeDev/PnP にあるサンプルとコンテンツ

適用対象
==========
- Office 365 マルチテナント (MT)
- Office 365 専用 (D) *一部*
- SharePoint 2013 オンプレミス - *一部*

*専用およびオンプレミスの場合のパターンは SharePoint アドイン モデル手法と同じですが、使用可能なテクノロジは異なる可能性があります。*
