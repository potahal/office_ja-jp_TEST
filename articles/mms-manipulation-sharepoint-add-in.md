SharePoint アドイン モデルにおけるMMS の操作
===============================================

概要
-------

新しい SharePoint アドイン モデルにおいて、管理されたメタデータ サービス (MMS) における作成、読み取り、更新および削除 (CRUD) 操作を実行するための方法は、完全信頼コードの場合とは異なります。一般的な完全信頼コード (FTC) / ファーム ソリューション シナリオでは、MMS CRUD 操作は SharePoint サーバー側オブジェクト モデル コードで実行され、ファーム ソリューション経由で展開されていました。 

SharePoint アドイン モデル シナリオでは、MMS CRUD 操作はクライアント側オブジェクト モデル (CSOM) で実行されます。

**CSOM は、MMS 内のデータのレプリケートや同期化に必要なすべての操作を提供します。**

基本ガイドライン
---------------------

MMS CRUD 操作の実行については、大まかに次のような基本ガイドラインが提供されています。

- MMS CRUD 操作は、クライアント側オブジェクト モデルを使用して実装する必要があります。
- MMS CRUD 操作を実行する適切なアクセス許可を持つアカウントを使用して、CSOM コードを実行します。
- 用語セットを同期するときは、GetAllTerms を使用し、同期するたびに条件を列挙するよりパフォーマンスが優れているため、ChangeInformation クラスを使用します。 


MMS のデータをコピーおよび同期するためのオプション
----------------------------------------

MMS のデータをコピーおよび同期するには、いくつかのオプションがあります。

- 社内
    + データベースのコピー
    + CSOM を使用してデータをコピーする
    + CSOM を使用してデータを同期する
- Office 365
    + CSOM を使用してデータをコピーする
    + CSOM を使用してデータを同期する

オンプレミス - データベースのコピー
---------------------------
オンプレミスの SharePoint 環境の場合は、MMS データベースをあるファームから別のファームにコピーし、用語をすばやくレプリケートできます。

**適切な場合**

オンプレミスの SharePoint 環境で、用語を一方向にコピーするときは、コードを記述することがなく迅速かつ容易に実装できる場合があるこのオプションが適しています。

オンプレミスと O365: CSOM を使用してデータをコピーする
------------------------------------------
オンプレミスまたは Office 365 の SharePoint 環境では、CSOM を使用して、ファーム/テナンシーの MMS データを別のファーム/テナンシーにコピーできます。  ***この方法には、オンプレミスのファームと Office 365 のファームの両方を含めることが可能です。***

**適切な場合**

オンプレミスの SharePoint、Office 365 またはハイブリッド環境を使用し、2 つ以上の SharePoint ファーム / テナンシー間で MMS データをコピーする場合、1 つのファームから別のファームへ MMS データを柔軟にコピーできるこのオプションが適しています。

**作業の開始**

次のサンプルは、MMS CRUD 操作を実行する方法について説明します。

- [Core.MMS (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.MMS)

オンプレミスと O365: CSOM を使用してデータを同期する
------------------------------------------
オンプレミスの SharePoint 環境では、CSOM を使用して、ファーム間で MMS データを同期できます。 ***この方法には、オンプレミスのファーム/テナンシーと Office 365 のファーム/テナンシーの両方を含めることが可能です。***

**適切な場合**

オンプレミスの SharePoint、Office 365 またはハイブリッド環境を使用し、2 つ以上の SharePoint ファーム / テナンシー間で MMS データを同期する場合、真の同期化を柔軟に実行でき、必要なだけソースを含めることができるこのオプションが適しています。

**作業の開始**

次のサンプルは、MMS データの同期ツールを構築する方法を示しています。

- [Core.MMSSync (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.MMSSync)

関連リンク
=============
- [CSOM を使用して用語セットを同期する (MSDN ブログ記事ドキュメント)](http://blogs.msdn.com/b/frank_marasco/archive/2014/06/29/synchronize-term-sets-with-the-term-store-csom.aspx)
- ガイダンス記事: [http://aka.ms/OfficeDevPnPGuidance](http://aka.ms/OfficeDevPnPGuidance "ガイダンス記事")
- MSDN リファレンス: [http://aka.ms/OfficeDevPnPMSDN](http://aka.ms/OfficeDevPnPMSDN "MSDN リファレンス")
- ビデオ: [http://aka.ms/OfficeDevPnPVideos](http://aka.ms/OfficeDevPnPビデオ"Videos")

関連する PnP サンプル
===================

- [Core.MMS (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.MMS)
- [Core.MMSSync (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.MMSSync)
- [https://github.com/OfficeDev/PnP](https://github.com/OfficeDev/PnP) にあるサンプルとコンテンツ

適用対象
==========
- Office 365 マルチテナント (MT)
- Office 365 専用 (D) *一部*
- SharePoint 2013 オンプレミス - *一部*

*専用およびオンプレミスの場合のパターンはアドイン モデル手法と同じですが、使用可能なテクノロジは異なる可能性があります。*
