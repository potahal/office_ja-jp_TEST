SharePoint アドイン モデルにおけるリスト インスタンス
============================================

概要
-------

新しい SharePoint アドイン モデルにおいて、リスト インスタンスを作成する方法は、完全信頼コードの場合とは異なります。一般的な完全信頼コード (FTC)/ ファーム  ソリューション シナリオでは、リスト インスタンスは宣言型コードを使用して作成され、SharePoint ソリューションを使用して展開されていました。 

SharePoint アドイン モデル シナリオでは、リモート プロビジョニング パターンを使用してリスト インスタンスが作成されます。

基本ガイドライン
---------------------

リスト インスタンスの作成については、大まかに次のような基本ガイドラインが提供されています。

- リスト インスタンスの作成には、リモート プロビジョニング パターンを使用します。
- リスト インスタンスの作成に宣言型コード (elements.xml) を使用しないでください。

**作業の開始**

次の PnP O365 コードのサンプルとビデオは、ユーザー インターフェイスを提供する SharePoint アドインを作成し、そこでエンド ユーザーに新しいドキュメント ライブラリの作成を許可する方法を示しています。また、まとめてテンプレートとなる特定の構成を使用してドキュメント ライブラリを作成する方法についても示します。このサンプルでは、リスト インスタンスを作成するコードを示します。

- [ECM.DocumentLibraries (O365 PnP のコード サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/ECM.DocumentLibraries)

次のビデオでは、コードのサンプルについて説明します。

- [アプリケーション モデルを使用したドキュメントとリストのテンプレート (O365 PnP ビデオ)](http://channel9.msdn.com/blogs/OfficeDevPnP/Document-and-list-templates-with-app-model)

リモート プロビジョニング パターンからリスト インスタンスを作成するには、SharePoint CSOM で AddList メソッドを使用します。次のコードはこの方法を示した [ECM.DocumentLibraries (O365 PnP コード サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/ECM.DocumentLibraries) の抜粋です。

    private void CreateLibrary(ClientContext ctx, Library library, string associateContentTypeID) 
    {
        if (!ctx.Web.ListExists(library.Title))
        {
           // Create List Instance
           ctx.Web.AddList(ListTemplateType.DocumentLibrary, library.Title, false);
           List _list = ctx.Web.GetListByTitle(library.Title);
           
           //Set Description
           if(!string.IsNullOrEmpty(library.Description)) 
           {
            _list.Description = library.Description;
           }

           //Turn on versioning 
           if(library.VerisioningEnabled) {
              _list.EnableVersioning = true;
           }
           
            //Turn on Content Types
           _list.ContentTypesEnabled = true;
           _list.Update();

           //Add Content Type to List
           ctx.Web.AddContentTypeToListById(library.Title, associateContentTypeID, true);
    
           //Remove the default Document Content Type
           _list.RemoveContentTypeByName(ContentTypeManager.DEFAULT_DOCUMENT_CT_NAME);

           ctx.Web.Context.ExecuteQuery();
        }
    }

次のコード サンプルは、SharePoint REST API でリスト インスタンスを作成する方法を示しています。このサンプルは、[リストおよびリスト アイテム REST API リファレンス (MSDN 記事)](https://msdn.microsoft.com/en-us/library/office/dn531433.aspx) の抜粋です

    executor.executeAsync({
      url: "<app web url>/_api/SP.AppContextSite(@target)/web/lists
        ?@target='<host web url>'",
      method: "POST",
      body: "{ '__metadata': { 'type': 'SP.List' }, 'AllowContentTypes': true, 'BaseTemplate': 100,
        'ContentTypesEnabled': true, 'Description': 'My list description', 'Title': 'Test title' }",
      headers: { "content-type": "application/json;odata=verbose" },
      success: successHandler,
      error: errorHandler
    });

関連リンク
=============
- [リストおよびリスト アイテム REST API リファレンス (MSDN 記事)](https://msdn.microsoft.com/en-us/library/office/dn531433.aspx)
- [リスト定義とリスト テンプレート (SharePoint アドイン モデル レシピ)](list-definition-template-sharepoint-add-in.md)
- [アプリケーション モデルを使用したドキュメントとリストのテンプレート (O365 PnP ビデオ)](http://channel9.msdn.com/blogs/OfficeDevPnP/Document-and-list-templates-with-app-model)
- ガイダンス記事: [http://aka.ms/OfficeDevPnPGuidance](http://aka.ms/OfficeDevPnPGuidance "ガイダンス記事")
- MSDN リファレンス: [http://aka.ms/OfficeDevPnPMSDN](http://aka.ms/OfficeDevPnPMSDN "MSDN リファレンス")
- ビデオ: [http://aka.ms/OfficeDevPnPVideos](http://aka.ms/OfficeDevPnPビデオ"Videos")

関連する PnP サンプル
===================

- [ECM.DocumentLibraries (O365 PnP のコード サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/ECM.DocumentLibraries)
- [https://github.com/OfficeDev/PnP](https://github.com/OfficeDev/PnP) にあるサンプルとコンテンツ

適用対象
==========
- Office 365 マルチテナント (MT)
- Office 365 専用 (D) *一部*
- SharePoint 2013 オンプレミス - *一部*

*専用およびオンプレミスの場合のパターンは SharePoint アドイン モデル手法と同じですが、使用可能なテクノロジは異なる可能性があります。*
