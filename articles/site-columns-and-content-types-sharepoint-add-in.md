SharePoint アドイン モデル内のサイト列とコンテンツ タイプ
=============================================================

概要
-------

新しい SharePoint アドイン モデルで SharePoint サイトにサイト列とコンテンツ タイプを作成する方法は、完全信頼コードの場合とは異なります。一般的な完全信頼コード (FTC)/ ファーム ソリューション シナリオでは、宣言型コードを使用してサイト列とコンテンツ タイプを作成します。宣言型コードのアプローチでは、サイト内の列とコンテンツ タイプを XML で定義し、次に SharePoint の機能フレームワーク要素を使用してそれらをパッケージ化および展開します。

SharePoint アドイン モデル シナリオでは、SharePoint クライアント側オブジェクト モデル (CSOM) または SharePoint REST API を使用して、サイト列とコンテンツ タイプを作成します。

基本ガイドライン
---------------------

サイト列とコンテンツ タイプの作成については、大まかに次のような基本ガイドラインが提供されています。

- サイト列とコンテンツ タイプを作成するには、SharePoint CSOM または REST API を使用する必要があります。
- サイト列やコンテンツ タイプの作成に機能フレームワーク要素を使用しないでください。
    + このガイドラインの唯一の例外は、Web 内の SharePoint アドインに対する宣言型 XML ベースのプロビジョニングを、SharePoint ホスト型 SharePoint アドインで使用している場合です。これは、SharePoint ホスト型 SharePoint アドインで CSOM を利用できないことが理由です。
- サイト プロビジョニング プロセスの一部としてサイト列とコンテンツ タイプの作成を自動化できます。詳細については、「[サイト プロビジョニングのレシピ](site-provisioning-sharepoint-add-in.md)を参照してください。

SharePoint サイトでのサイト列とコンテンツ タイプの作成に関する課題
----------------------------------------------------------------------

**Web ブラウザーを使用して作成する場合とコードを使用して作成する場合** 

Web ブラウザーを使用する場合とコードを使用する場合でサイト列とコンテンツ タイプの作成方法が異なることを理解しておくことが重要です。このリストでは、さまざまなオプションについて説明しています。

- **Web ブラウザーを使用して作成する**
    + このオプションでは、ユーザーは  Web ブラウザーから SharePoint サイトにアクセスし、管理ページを使用して、サイト列とコンテンツ タイプを作成します。
    + 通常、Web ブラウザーを使用して サイト列とコンテンツ タイプを手動で作成するのは、他のサイト コレクションやサブサイトが含まれるように拡張する計画のない単一の SharePoint サイトを試作または変更するときです。
- **コードを使用して作成する場合**
    + このオプションでは、SharePoint CSOM/REST を実行してサイト列とコンテンツ タイプを作成します。
    + SharePoint CSOM/REST の実行に使用できるいくつかのオプションについては、この記事の後半で説明します。

**Web ブラウザーを使用して作成する**場合は、次の点を考慮してください。

- Web ブラウザーを使用してサイト列とコンテンツ タイプを作成する方法は、通常、複雑で時間のかかるプロセスです。
    + これらの要因により、**エラーが発生する傾向**があります。
- Web ブラウザーを使用して作成するサイト列やコンテンツ タイプの GUID はコントロールされません。
    + これにより、サイト列とコンテンツ タイプを異なる環境に展開し、基幹業務アプリケーションでそれらを一貫して参照するのは**困難**になります。

**コードを使用して作成する**場合は、次の点を考慮してください。

- 一般的に、コードを使用してサイト列とコンテンツ タイプを作成するには、カスタム ユーティリティ ライブラリを使用して SharePoint CSOM/REST コードを実行する必要があります。
    + これらのライブラリは、OfficeDev PnP GitHub リポジトリの多くのプロジェクトで利用できます。それらは記事のあらゆる場所および巻末で参照されています。
    + これらの要因により、コードを使用したサイト列とコンテンツ タイプの作成は、**成功する傾向**があります。
- SharePoint CSOM/REST を使用して作成されたサイト列やコンテンツ タイプの GUID はコントロールできます。
    + これにより、サイト列とコンテンツ タイプを異なる環境に展開し、基幹業務アプリケーションでそれらを一貫して参照するのが**容易**になります。

**すばやく実行する**

通常、サイト列とコンテンツ タイプは、SharePoint サイトのプロビジョニングを行うときに作成します。エンド ユーザーは、新しい SharePoint サイトのプロビジョニングが完了するまで数時間待機する必要があることを受け入れられません。

**一貫して完璧に実行する**

サイト列とコンテンツ タイプは、最も低いレベルで情報アーキテクチャを定義する基礎となります。*それらは完璧でなければなりません*

不適切なサイト列とコンテンツ タイプのプロビジョニングは、それらがプロビジョニングされた SharePoint サイト内の基幹業務アプリケーション全体、また、SharePoint サイトの他の部分や、SharePoint サービスにアクセスする他の基幹業務アプリケーションに影響を与える可能性があります。

例:会社が SharePoint サイトを使用してプロジェクトを管理している場合、それらすべてに共通のリスト スキームを作成する可能性が高くなります。これには、サイト列とコンテンツ タイプを作成する必要があります。SharePoint の検索ページからこれらのサイト内の情報を検索するときは、コンテンツ タイプまたはタグ (サイト列) で結果をフィルターします。サイト列とコンテンツ タイプがすべてのプロジェクト サイト間で完全に一貫していない場合、正確な検索結果は得られません。

この例は、コンテンツ検索 Web パーツ、SharePoint アドイン、モバイル SharePoint アドイン、および SharePoint サイト内の情報にアクセスするその他のシステムにも当てはまります。

SharePoint サイトでサイト列とコンテンツ タイプを作成するオプション
--------------------------------------------------------------------

CSOM/REST コードを呼び出してサイト列とコンテンツ タイプを作成する方法はいくつかあります。これらのパターンはすべて、上記の**コードを使用して作成する**方法に該当します。これらの各パターンの詳細は、「[サイト プロビジョニング レシピ](site-provisioning-sharepoint-add-in.md)」に記載されています。

- サイトの作成リンクを上書きする
- サブ サイトの作成リンクを上書きする
- プロバイダー ホスト型 SharePoint アドインを使用する
- Windows/Java/iOS アプリケーションまたは PowerShell スクリプトを使用する

実装用に選択するオプションがどれであっても、サイト列とコンテンツ タイプの作成には最終的に CSOM/REST を使用します。

CSOM を使用してサイト列やコンテンツ タイプを作成する方法を学習できる記事やサンプルは、多数あります。ここでは、サイト列とコンテンツ タイプの作成のための例 (CSOM コードの呼び出ししに使用されるパターンで分類) をいくつか示します。

プロバイダー ホスト型 SharePoint アドインを使用する
---------------------------------------
このオプションは、カスタム テンプレートを基にした SharePoint サイト コレクションおよびサブ サイトを作成できるセルフサービス機能を、ユーザーに提供する必要がある場合に適しています。

- [Core.ContentTypesAndFields (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.ContentTypesAndFields)
    + ホストの Web で新しいコンテンツ タイプを作成する、ホストの Web で分類フィールドを作成して分類に関連付ける、リストを作成してコンテンツ タイプと関連付ける、および特定の言語でコンテンツ タイプとフィールドを作成する方法を説明します。

Windows/Java/iOS アプリケーションまたは PowerShell スクリプトを使用する
-------------------------------------------------------

このオプションは、Dev-Ops のシナリオに適しています。Dev-Ops プロセスで動作するよう特別に作られたカスタム アプリケーションやスクリプトを作成できます。このオプションは、ユーザーの介入なしに実行する SharePoint のアドインとスクリプトを作成できるため、最高レベルの自動化を提供します。  

- [Core.CreateContentTypes (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.CreateContentTypes)
    + このサンプルは、サイト列とコンテンツ タイプを作成して追加し、次にコンテンツ タイプにサイト列を追加する方法を示します。また、Office 365 CSOM API で導入されたローカライズ機能についても説明します。
- [Core.CreateDocumentContentType (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.CreateDocumentContentType)
    + このサンプルは、ドキュメント コンテンツのタイプを作成し、ドキュメント テンプレートをコンテンツ タイプに関連付ける方法を示します。

関連リンク
=============
- [SharePoint アドイン モデルでのサイト プロビジョニング (O365 PnP レシピ)](site-provisioning-sharepoint-add-in.md)
- ガイダンス記事: [http://aka.ms/OfficeDevPnPGuidance](http://aka.ms/OfficeDevPnPGuidance "ガイダンス記事")
- MSDN リファレンス: [http://aka.ms/OfficeDevPnPMSDN](http://aka.ms/OfficeDevPnPMSDN "MSDN リファレンス")
- ビデオ: [http://aka.ms/OfficeDevPnPVideos](http://aka.ms/OfficeDevPnPビデオ"Videos")

関連する PnP サンプル
===================

- [Core.CreateContentTypes (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.CreateContentTypes)
- [Core.ContentTypesAndFields (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.ContentTypesAndFields)
- [Core.CreateDocumentContentType (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.CreateDocumentContentType)
- [Branding.DisplayTemplates (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Branding.DisplayTemplates)
- [Core.DataStorageModels (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.DataStorageModels)
- [https://github.com/OfficeDev/PnP](https://github.com/OfficeDev/PnP) にあるサンプルとコンテンツ

適用対象
==========
- Office 365 マルチテナント (MT)
- Office 365 専用 (D) *一部*
- SharePoint 2013 オンプレミス - *一部*

*専用およびオンプレミスの場合のパターンは SharePoint アドイン モデル手法と同じですが、使用可能なテクノロジは異なる可能性があります。*
