SharePoint アドイン モデルにおける機能の関連付け
===============================================

概要
-------

新しい SharePoint アドイン モデルにおいて、SharePoint サイトがプロビジョニングされているときにコードの実行や成果物の展開に使用するアプローチは、完全信頼コードの場合とは異なります。一般的な完全信頼コード (FTC) / ファーム ソリューション シナリオでは、機能の関連付けによって標準のサイト定義を変更していました。SharePoint サイトに関連する成果物、構成、およびブランド化資産は機能によってパッケージ化および展開され、機能はサイト定義に関連付けられました。その後、関連付けられた機能はサイト プロビジョニングによって自動的にインストールされ、アクティブ化されました。

SharePoint アドイン モデル シナリオでは、機能の関連付け、アドインの関連付け、または SharePoint クライアント側オブジェクト モデル (CSOM) を使用して、サイト コレクションとサブサイトを作成および構成した後、成果物、構成、およびブランド化資産をそこに展開することができます。このパターンは、一般的に*リモート プロビジョニング パターン*と呼ばれます。

基本ガイドライン
---------------------

サイト コレクションとサブサイトを作成および構成した後、成果物、構成、およびブランド化資産をそこに展開する方法については、経験に基づく次のような大まかなガイドラインが提供されています。

- 機能の関連付けを引き続き使用する唯一の方法は、機能をサイト コレクションに関連付け、サイト定義および関連付けられた機能をサンドボックス ソリューションによって展開することです。    
- テナント展開型モデルで機能の関連付けに似た機能を実装するには、アドインの関連付けを使用できます。
- リモート プロビジョニング パターンを使用して機能の関連付けに似た機能を実装するには、リモート API によって標準のサイト定義上で追加の機能をアクティブ化します。

サイト コレクションとサブサイトを作成および構成した後、成果物、構成、およびブランド化資産をそこに展開するためのオプション
---------------------------------------------------------------------------------------------------------------------------------

サイト コレクションとサブサイトを作成および構成した後、成果物、構成、およびブランド化資産をそこに展開するためのオプションは複数あります。

- 機能を関連付ける
- アドインを関連付ける
- リモート プロビジョニング パターンを使用する   

機能を関連付ける
---------------
このパターンでは、機能をサイト定義に関連付けます。
    
- このパターンは、サイト コレクション レベルでのみ行うことができます。
- 機能をサブサイトに関連付けることはできません。
- 廃止されたサンド ボックス ソリューションが使用され、アップグレードに対応できないため、これは推奨される最適なアプローチではありません。

**適切な場合**

オンプレミス SharePoint 環境でレガシー コードを移行しており、コードを適切に書き直す時間がない場合。

**はじめに**

次の記事では、サイト定義に機能を関連付ける方法について説明しています。

- [SharePoint 2010 における機能の関連付け (MSDN ブログ記事)](http://blogs.msdn.com/b/kunal_mukherjee/archive/2011/01/11/feature-stapling-in-sharepoint-2010.aspx)

アドインを関連付ける
--------------
このパターンでは、アプリ カタログに保存されているアドインを、特定のサイト コレクション、管理パス、およびサイト テンプレートに展開します。

- アドインの関連付けモデルの詳細については、「["アプリの関連付け" による SharePoint 2013 アプリの展開 (MSDN ブログ記事 - Richard DiZerega)](http://blogs.msdn.com/b/richard_dizeregas_blog/archive/2013/09/18/10399333.aspx)」を参照してください。
- アドインは管理者によってプッシュされるため、サイト所有者は展開の基準を満たすサイトからアドインを削除することはできません。サイト コレクション管理者であっても、アドインを削除することはできません。
- この一元的な展開では、同じ一元的なアドイン リソース (アドイン Web とリモート Web) も共有されます。基本的に、アドインは展開されますが、サイトにはインストールされません。すべてのサイトで、アプリ カタログでインストールされたインスタンスからアドイン Web とリモート Web が利用されます。
- 一元的な展開のため、HandleAppInstalled、HandleAppUninstalled、HandleAppUpgrade などのリモート イベントは、1 回だけ (アドインがアプリ カタログにインストールされる際) 起動されます。
    + アドインの関連付けパターンを使用して変更をアドインの展開先のサイトに自動的に適用することは、サイトへのアドインの展開時にこれらのイベントが起動されないため、難しくなる可能性があります。
- アドインがサイトに関連付けられている場合、アドイン パーツはサポートされません。
- このパターンでは、ユーザーの手動アクションによってアドインを配置する必要があります。

リモート プロビジョニング パターンを使用する
-----------------------------------

このパターンでは、SharePoint クライアント側オブジェクト モデル (CSOM) を使用して、サイト コレクションとサブサイトを作成および構成した後、成果物、構成、およびブランド化資産をそこに展開します。

- このパターンでは、成果物、構成、およびブランド化資産を別個の機能やアドインにパッケージ化する必要がありません。すべてを単一のアドインでパッケージ化することができます。
- このパターンをサイト プロビジョニングに使用する場合、通常は標準のページをオーバーライドして新しいサイトを作成します。
- このパターンの詳細については、「[サイト プロビジョニング (SharePoint アドイン レシピ)](site-provisioning-sharepoint-add-in.md)」を参照してください。
- アドインを SharePoint サイトに配置するには、CSOM を使用することができます。.app マニフェスト ファイルを使用して Office アドインを読み込んで SharePoint サイトにインストールする例を以下に示します。

    ```
    //Create a FileStream object to access the Mail Office Add-in .app file 
    using (FileStream fsSource = new FileStream(System.Web.Hosting.HostingEnvironment.ApplicationPhysicalPath + "Innovation.Management.AFO.app",
    FileMode.Open, FileAccess.Read))
    {
        //Return the subweb where you want to install the Add-in
        var subweb = ctx.Web;
        ctx.Load(subweb);
        ctx.ExecuteQuery();

        //Load and Install the Add-in on the subweb
        AppInstance appInstance = subweb.LoadAndInstallApp(fsSource);
        ctx.Load(appInstance);
        ctx.ExecuteQuery();
    }
    ```

    + サイト プロビジョニングでこのアプローチを使用して Office アドインを SharePoint サイトにインストールする方法については、「[Office、O365、Azure、および WP8 用のアドインによるクラウド ホスト型の基幹業務アプリケーションの作成 (Todd Baginski、Michael Sherman - SharePoint Conference 2014)](https://channel9.msdn.com/Events/SharePoint-Conference/2014/SPC361)」ビデオをご覧ください。
    + 完全な自動化は、完全なテナント アクセス許可を持つ信頼済みのアドインでのみ可能です。
        + 例については、[Core.Sideloading (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.SideLoading) をご覧ください。 

関連リンク
=============
- [SharePoint 2010 における機能の関連付け (MSDN ブログ記事)](http://blogs.msdn.com/b/kunal_mukherjee/archive/2011/01/11/feature-stapling-in-sharepoint-2010.aspx)
- [SharePoint 2013 用のアドインを使用したセルフサービスのサイト プロビジョニング (MSDN ブログ)](http://blogs.msdn.com/b/richard_dizeregas_blog/archive/2013/04/04/self-service-site-provisioning-using-apps-for-sharepoint-2013.aspx)
- ["アプリの関連付け" による SharePoint 2013 アプリの展開 (MSDN ブログ記事 - Richard DiZerega)](http://blogs.msdn.com/b/richard_dizeregas_blog/archive/2013/09/18/10399333.aspx)
- [サイト プロビジョニング (SharePoint アドイン レシピ)](site-provisioning-sharepoint-add-in.md)
- [Office、O365、Azure、および WP8 用のアドインによるクラウド ホスト型の基幹業務アプリケーションの作成 (Todd Baginski、Michael Sherman - SharePoint Conference 2014)](https://channel9.msdn.com/Events/SharePoint-Conference/2014/SPC361)
- [http://aka.ms/OfficeDevPnPGuidance](http://aka.ms/OfficeDevPnPGuidance "ガイダンス記事") にあるガイダンス記事
- MSDN リファレンス: [http://aka.ms/OfficeDevPnPMSDN](http://aka.ms/OfficeDevPnPMSDN "MSDN リファレンス")
- ビデオ: [http://aka.ms/OfficeDevPnPVideos](http://aka.ms/OfficeDevPnPビデオ"Videos")

関連する PnP サンプル
===================

- [Provisioning.Cloud.Sync (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Solutions/Provisioning.Cloud.Sync)
- [Provisioning.SubSiteCreationApp (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Provisioning.SubSiteCreationApp)
- [Provisioning.Services.SiteManager (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Provisioning.Services.SiteManager)
- [Provisioning.SiteCollectionCreation (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Provisioning.SiteCollectionCreation)
- [http://aka.ms/OfficeDevPnP](http://aka.ms/OfficeDevPnP) にあるサンプルとコンテンツ

適用対象
==========
- Office 365 マルチテナント (MT)
- Office 365 専用 (D)
- SharePoint 2013 オンプレミス
