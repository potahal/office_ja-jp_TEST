SharePoint アドイン モデルでのサイト プロビジョニング
================================================

概要
-------

新しい SharePoint アドイン モデルで サイト コレクションとサブ サイトを作成する方法は、完全信頼コードの場合とは異なります。一般的な完全信頼コード (FTC)/ ファーム ソリューション シナリオでは、サイト定義と Web テンプレートを使用してサイト コレクションとサブサイトを作成し、その後宣言型コードを使用してサイトを構成し、カスタマイズを適用します。このモデルで一般的に宣言型コードを使用してサイト列、コンテンツ タイプは、XML で定義されたリストを作成し、その後 SharePoint の機能フレームワーク要素を使用してそれらをパッケージ化および展開します。

SharePoint アドイン モデル シナリオでは、SharePoint クライアント側オブジェクト モデル (CSOM) を使用してサイト コレクションとサブサイトを作成および構成できます。このパターンは、一般的に *リモート プロビジョニング パターン* と呼ばれます。

大まかなリモート プロビジョニング パターンは、次のようになります。

![1) リモート タイマー ジョブは、2) すぐに使用できるサイトに基づいた初期プロビジョニングに向けられています。 通常は、チーム サイトまたは発行サイトです。 アセットは、CSOM/REST を使用してプロビジョニング エンジンからアップロードされます。 3) プロジェクト サイト、組織サイト、またはワークグループ サイトのいずれを作成するかについてのユーザーの選択に基づいた、すぐに使用できるサイトのトップに必要な変更 (構成など) を適用します。 これは特殊な部分ですが、OOB サイト起点になるため、このサイトはベース ラインとして常に最新の改良が加えられています。](media/Recipes/SiteProvisioning/overview.png)

基本ガイドライン
---------------------

サイト コレクションとサブサイトの作成については、大まかに次のような基本ガイドラインが提供されています。

- SharePoint に付属している標準のサイト テンプレートに基づいてサイト コレクションとサブ サイトのプロビジョニングを行います。
    + SharePoint CSOM を使用してサイト コレクションとサブ サイトを作成します。
- 標準のサイト コレクションとサブ サイトにカスタマイズと設定を適用し、要件を満たします。
    + SharePoint CSOM を使用してカスタマイズと設定を適用します。
- サイト コレクションとサブ サイトを作成するとき、機能フレームワーク要素を使用しないでください。
    + このガイドラインの唯一の例外は、アドイン Web に対する宣言型 XML ベースのプロビジョニングを、SharePoint ホスト型 SharePoint アドインで使用している場合です。これは、SharePoint ホスト型 SharePoint アドインで CSOM を利用できないためです。

サイト コレクションとサブ サイトの作成に関する課題
--------------------------------------------------

**Web ブラウザーを使用して作成する場合とコードを使用して作成する場合** 

Web ブラウザーを使用した場合とコードを使用した場合ではサイト コレクションとサブ サイトの作成方法が異なることを、理解しておくことが重要です。このリストでは、さまざまなオプションについて説明しています。

- **Web ブラウザーを使用して作成する**
    + このオプションでは、ユーザーは Web ブラウザーを使用して SharePoint サイトにアクセスし、管理ページを使用して、サイト コレクションとサブ サイトを作成します。
    + 通常、Web ブラウザーを使用してサイト コレクションとサブ サイトを手動で作成するのは、他のサイト コレクションやサブサイトが含まれるように拡張する計画のない単一の SharePoint サイトを試作または変更するときです。  
- **コードを使用して作成する場合**
    + このオプションでは、SharePoint CSOM コードを実行してサイト コレクションとサブ サイトを作成します。 
    + この記事の後半では、SharePoint CSOM のコードの実行に使用できるいくつかのアプローチについて説明します。

**Web ブラウザーを使用して作成する**場合は、次の点を考慮してください。

- Web ブラウザーを使用したサイト コレクションと サブ サイトの作成は通常、複雑で時間のかかるプロセスです。
    + これらの要因により、Web ブラウザーを使用したサイト コレクションとサブ サイトの作成では**エラーが発生する傾向**があります。   
- Web ブラウザーを使用して作成されたサイト コレクションとサブ サイト (およびそれらに含まれるコンポーネント) を反復可能な方法で再作成することはできません。 
    + これにより、開発からテスト、本番へと移行しながら、サイト コレクションとサブ サイトを異なる環境に迅速かつ一貫して展開するのは**困難**になります。

**コードを使用して作成する**場合は、次の点を考慮してください。

- コードを使用してサイト コレクションとサブ サイトを作成する場合、通常、カスタム ユーティリティ ライブラリを使用して SharePoint CSOM コードを実行します。
    + これらのライブラリは、OfficeDev PnP GitHub リポジトリの多くのプロジェクトで利用できます。それらは記事のあらゆる場所および巻末で参照されています。
    + これらの要因により、コードを使用したサイト コレクションとサブ サイトの作成は**成功する傾向**があります。
- コードを使用して作成されたサイト コレクションとサブ サイトは、粒度の細かい方法で**容易かつ一貫してレプリケート**できます。
    + 開発からテスト、本番へと移行しながら、サイト コレクションとサブ サイトを異なる環境に**容易に**展開して参照できます。

**すばやく実行する**

エンド ユーザーは、自分たちの新しい SharePoint サイトのプロビジョニングが完了するまで数時間待機する必要があることを受け入れられません。

**一貫して完璧に実行する**

サイト コレクションとサブ サイト、およびサイト列、コンテンツ タイプ、リスト、マスター ページ、JavaScript ファイル、画像などを含む様々なコンポーネントは、最も低いレベルで情報アーキテクチャを定義する基礎となります。*それらは完璧でなければなりません*

不適切なサイト コレクションとサブ サイトのプロビジョニングは、それらがプロビジョニングされた SharePoint サイト内の基幹業務アプリケーション全体、また、SharePoint サイトの他の部分や、SharePoint サービスにアクセスする他の基幹業務アプリケーションに影響を与える可能性があります。

例:社内でプロジェクトの管理に使用される SharePoint サイトがある場合、それらすべてに共通のリスト スキームを作成する可能性が高くなります。これには、サイト列とコンテンツ タイプを作成する必要があります。SharePoint の検索ページからこれらのサイト内の情報を検索するときは、コンテンツ タイプまたはタグ (サイト列) で結果をフィルターします。サイト列とコンテンツ タイプがすべてのプロジェクト サイト間で完全に一貫していない場合、正確な検索結果は得られません。

この例は、コンテンツ検索 Web パーツ、SharePoint アドイン、モバイル アプリ、および SharePoint サイト内の情報にアクセスするその他のシステムにも当てはめることができます。

サイト コレクションとサブ サイトを作成するオプション
------------------------------------------------

新しい SharePoint アドイン モデルで サイト コレクションとサブ サイトを作成するには、いくつかのオプションがあります。これらのオプションはすべて、上記の**コードを使用して作成する**方法に該当します。

- サイトの作成リンクを上書きする
- サブ サイトの作成リンクを上書きする
- プロバイダー ホスト型 SharePoint アドインを使用する
- .NET/Java/Objective-C アプリケーションまたは PowerShell スクリプトを使用する

サイトの作成リンクを上書きする
-----------------------------

このパターンでは、サイト コレクションを作成するリンクは、プロバイダー ホスト型 SharePoint アドインを示すリンクで上書きされます。プロバイダー ホスト型 SharePoint アドインで実行される CSOM コードは、サイト作成プロセスの一部としてリモート プロビジョニング パターンを使用して実行されます。

- このパターンは、サイト コレクションの作成を対象とした場合にのみ使用され、サブ サイトの作成には使用できません。
- 上書き URL は、SharePoint 管理センターで構成されます。この URL はプロバイダー ホスト型 SharePoint アドインを示します。
- プロバイダー ホスト型 SharePoint アドインは、CSOM API を使用してサイト コレクションを作成します。
    + また、プロビジョニング プロセス中に、サイトの他の側面を構成するため、CSOM/REST API を使用することもできます。
- この方法は、Office 365 のテナントとオンプレミスの SharePoint で使用できます。
- SharePoint サイト コレクションの作成と構成において、非常に高い柔軟性が得られます。
- 短期および長期にわたって、容易かつ安価に実装および維持できます。

**構成**

サイトの作成リンクを上書きするには、SharePoint 管理センターの設定ページを開きます (以下を参照)。

![[設定] メニュー オプションが強調表示されている [SharePoint 管理センター] メニュー。](media/Recipes/SiteProvisioning/sp-admin-center.png)

次に、この URL チェック ボックスで [フォームの使用] をオンにし、サイト作成機能を実装するプロバイダー ホスト型 SharePoint アドインへの URL を入力します(以下を参照)。

![「Web ページからのメッセージ」というタイトルの付いた [OK]/[キャンセル] ダイアログ ボックス。メッセージは、次のとおりです。ユーザーがサイトを作成する場所を変更すると、それらのユーザーに、他のサイトにアクセスできるユーザー設定のスクリプトの実行を許可してしまう可能性があります (詳しくは、http://go.microsoft.com/fwlink/?Linkld=164264 を参照してください)。 これを防止するには、次の SharePoint Online 管理シェル コマンドを実行します。Set-SPOsite <SiteURL> -DenyAddAndCustomizePages 1 ユーザーがサイトを作成する場所を変更しますか。](media/Recipes/SiteProvisioning/override-warning.png)

SharePoint によってこの方法と関連付られたセキュリティ上の影響についての警告が表示され (下のダイアログ) 、このタイプの機能を無効にするオプションが提供されます。

![このページでは、「次の URL のフォームを使用する」というタイトルが付けられたチェック ボックスを矢印で指しています。チェック ボックスはオフになっています。 このページにある、その他のテキストとコントロールは次のとおりです。定義された場所に新しいチーム サイトを作成するためのショートカットを提供します。 [リンクを非表示にする] ラジオ ボタンはオフになっていて、[リンクを表示する] ラジオボタンはオンになっています。 [サイトの分類] フィールドは、[ユーザーに表示しない] になっています。 [代理の連絡先] フィールドは、[不要] になっています。](media/Recipes/SiteProvisioning/override-form.png)

**適切な場合**

このオプションは、カスタム テンプレートを基にして SharePoint サイト コレクションを作成するセルフサービス機能を、ユーザーに提供する必要がある場合に適しています。

**はじめに**

次の記事では、サイトの作成リンク パターンの上書きについて説明し、開始するためのコード サンプルを提供します。

- [SharePoint 2013 用アドインを使用したセルフサービスのサイト プロビジョニング (MSDN ブログ)](http://blogs.msdn.com/b/richard_dizeregas_blog/archive/2013/04/04/self-service-site-provisioning-using-apps-for-sharepoint-2013.aspx)
    + このパターンに関するエンドツーエンドの記事に、ビデオが付属しています。
- [Provisioning.Cloud.Sync (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Solutions/Provisioning.Cloud.Sync)
    + このソリューションは、実際のサンド ボックス ソリューションや stp ファイルを使用せずにサイト テンプレートのモデルを導入するための、同期サイト コレクションまたはサブ サイト作成エクスペリエンスを提供するモデルを示します。 **{Todd, please re-read the previous sentence and consider rewriting.You might just be missing "a" before the word model, or more text is missing.}**

サブ サイトの作成リンクを上書きする
---------------------------------

このパターンでは、サブ サイト作成リンクは、プロバイダー ホスト型 SharePoint アドインを示すリンクに上書きされます。プロバイダー ホスト型 SharePoint アドインで実行される CSOM コードは、サイト作成プロセスの一部としてリモート プロビジョニング パターンを使用して実行されます。
 
- このパターンは、サブ サイトの作成を対象とした場合にのみ使用され、サイト コレクションの作成には使用できません。
- 上書き URL は、JavaScript を使用してリンクを変更するカスタム アクションで構成されます。この URL はプロバイダー ホスト型 SharePoint アドインを示します。
- プロバイダー ホスト型 SharePoint アドインには、CSOM API を使用してサブ サイトを作成します。
    + また、プロビジョニング プロセス中に、サイトの他の側面を構成するため、CSOM/REST API を使用することもできます。
- この方法は、Office 365 のテナントとオンプレミスの SharePoint で使用できます。
- SharePoint サイトの作成と構成において、非常に高い柔軟性が得られます。
- 短期および長期にわたって、容易かつ安価に実装および維持できます。

**適切な場合**

このオプションは、カスタム テンプレートを基にして SharePoint サブ サイトを作成するセルフサービス機能を、ユーザーに提供する必要がある場合に適しています。

**はじめに**

次の記事では、サブ サイトの作成リンク パターンの上書きについて説明し、開始するためのコード サンプルを提供します。

- [Provisioning.Cloud.Sync (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Solutions/Provisioning.Cloud.Sync)
    + このソリューションは、実際のサンド ボックス ソリューションや stp ファイルを使用せずにサイト テンプレートのモデルを導入するための、同期サイト コレクションまたはサブ サイト作成エクスペリエンスを提供するモデルを示します。 
- [Provisioning.SubSiteCreationApp (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Provisioning.SubSiteCreationApp)
    + このソリューションは、リモート プロビジョニング パターンを使用し、可能な限り柔軟なサブ サイト テンプレート システムを提供します。また、付属のビデオも含まれています。


プロバイダー ホスト型 SharePoint アドインを使用する
---------------------------------------

このパターンでは、プロバイダー ホスト型 SharePoint アドインで実行される CSOM コードは、サイト作成プロセスの一部としてリモート プロビジョニング パターンを使用して実行されます。
 
- このパターンは、サイト コレクションとサブ サイトの作成を対象とした場合に使用できます
- SharePoint ホスト型アドインに、SharePoint 環境へのフル コントロール アクセス許可を与える必要があります。
    + このパターンではフル コントロール アクセス許可を必要とするため、Microsoft Marketplace で使用することはできません。
- プロバイダー ホスト型 SharePoint アドインは、CSOM API を使用してサイト コレクションとサブ サイトを作成します。
    + また、プロビジョニング プロセス中に、サイトの他の側面を構成するため、CSOM/REST API を使用することもできます。
- この方法は、Office 365 のテナントとオンプレミスの SharePoint で使用できます。
- SharePoint サイトの作成と構成において、非常に高い柔軟性が得られます。
- 短期および長期にわたって、容易かつ安価に実装および維持できます。

**適切な場合**

このオプションは、カスタム テンプレートを基にした SharePoint サイト コレクションおよびサブ サイトを作成できるセルフサービス機能を、ユーザーに提供する必要がある場合に適しています。*ユーザーがアクセスできるよう、プロバイダー ホスト型アプリケーションへのリンクをユーザーに提供する必要があります。*

- [ハイブリッド シナリオ での非同期のプロビジョニング (MSDN ブログ記事)](http://blogs.msdn.com/b/vesku/archive/2015/03/05/hybrid-site-collection-provisioning-from-azure-to-on-premises-sharepoint.aspx)
- [Provisioning.Hybrid.Simple (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Provisioning.Hybrid.Simple)
    + このサンプルでは、Azure ストレージのキュー、WebJobs およびサービス バス リレーを使用した最も簡単なハイブリッド セットアップについて説明します。これは、新しいカスタム ブランドのサイト コレクションを、オンプレミスの SharePoint アドイン インフラストラクチャ要件を備えていないオンプレミスのファームにプロビジョニングするときに使用できる、Azure Web サイトでのプロバイダー SharePoint アドインのホストを示しています。
- [Provisioning.Services.SiteManager (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Provisioning.Services.SiteManager)
    + このサンプルは、プロバイダー ホスト型 SharePoint アドインからのサイト コレクションの作成をサポートするため、オンプレミスのファームを拡張する方法を示します。
- [Provisioning.SiteCollectionCreation (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Provisioning.SiteCollectionCreation)
    + プロバイダー ホスト型 SharePoint アドインから CSOM for Office 365 を使用してサイト コレクションを作成する方法を説明します。

.NET/Java/Objective-C アプリケーションまたは PowerShell スクリプトを使用する
----------------------------------------------------

このパターンでは、CSOM コードは .NET/Objective-C/iOS アプリケーションまたは PowerShell スクリプトを使用して実行されます。また、このパターンには、Azure Web ジョブなどのリモート タイマー ジョブを使用した場合も含まれます。
 
- このパターンは、サイト コレクションとサブ サイトの作成を対象とした場合に使用できます。
- SharePoint アドインに、SharePoint 環境へのフル コントロール アクセス許可を与える必要があります。
- 作成する SharePoint アドインのタイプや、SharePoint のセキュリティ設定によっては認証が困難となる場合があります。
- プロバイダー ホスト型 SharePoint アドインは、CSOM API を使用してサイト コレクションとサブ サイトを作成します。
    + また、プロビジョニング プロセス中に、サイトの他の側面を構成するため、CSOM/REST API を使用することもできます。
- この方法は、Office 365 のテナントとオンプレミスの SharePoint で使用できます。
- SharePoint サイトの作成と構成において、非常に高い柔軟性が得られます。
- 短期および長期にわたって、容易かつ安価に実装および維持できます。

**適切な場合**

このオプションは、Dev-Ops のシナリオに適しています。Dev-Ops プロセスで動作するよう特別に作られたカスタム アプリケーションやスクリプトを作成できます。このオプションは、ユーザーの介入なしに実行する SharePoint のアドインとスクリプトを作成できるため、最高レベルの自動化を提供します。 

-   [WebJobs を使用した Office 365 の非同期プロビジョニング (MSDN のブログ記事)](http://blogs.msdn.com/b/vesku/archive/2015/03/04/asynchronous-on-demand-site-collection-provisioning-to-office-365-with-azure-webjobs.aspx) 
- [Provisioning.Cloud.Async.WebJob (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Provisioning.Cloud.Async.WebJob)
    + Azure ストレージ キューおよび Azure WebJobs を使用して非同期のセルフサービスのサイト コレクション プロビジョニング ソリューションを作成する方法を示すソリューション。
-   [Provisioning.Framework.Console (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/samples/Provisioning.Framework.Console) – 新しいエンジンのパワーを示す、サイト プロビジョニング フレームワークのサンプル。
-   
                [Provisioning.Cloud.Async (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Provisioning.Cloud.Async) - Office 365/SharePoint でサイト コレクションを非同期的に作成する方法を説明します。 要求は、SharePoint ホスト Web 内のリストに保存されます。 このサンプルに含まれるコンソール アプリケーションは、Azure に対し、またはオンプレミス環境やスケジュールで展開されます。 
 

関連リンク
=============
- [SharePoint 2013 用アドインを使用したセルフサービスのサイト プロビジョニング (MSDN ブログ)](http://blogs.msdn.com/b/richard_dizeregas_blog/archive/2013/04/04/self-service-site-provisioning-using-apps-for-sharepoint-2013.aspx)
- ガイダンス記事: [http://aka.ms/OfficeDevPnPGuidance](http://aka.ms/OfficeDevPnPGuidance "ガイダンス記事")
- MSDN リファレンス: [http://aka.ms/OfficeDevPnPMSDN](http://aka.ms/OfficeDevPnPMSDN "MSDN リファレンス")
- ビデオ: [http://aka.ms/OfficeDevPnPVideos](http://aka.ms/OfficeDevPnPビデオ"Videos")

関連する PnP サンプル
===================

- [Provisioning.Cloud.Sync (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Solutions/Provisioning.Cloud.Sync)
- [Provisioning.SubSiteCreationApp (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Provisioning.SubSiteCreationApp)
- [Provisioning.Services.SiteManager (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Provisioning.Services.SiteManager)
- [Provisioning.SiteCollectionCreation (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Provisioning.SiteCollectionCreation)
- https://github.com/OfficeDev/PnP にあるサンプルとコンテンツ

適用対象
==========
- Office 365 マルチテナント (MT)
- Office 365 専用 (D) *一部*
- SharePoint 2013 オンプレミス - *一部*

*専用およびオンプレミスの場合のパターンは SharePoint アドイン モデル手法と同じですが、使用可能なテクノロジは異なる可能性があります。*
