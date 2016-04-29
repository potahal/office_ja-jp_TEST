
# 作業ウィンドウ アドインとコンテンツ アドインを SharePoint のアドイン カタログに発行する
アドイン マニフェストをカタログにアップロードすると、ユーザーは、アドインが [Office アドイン] ダイアログ ボックスから使用できるように Office クライアントを構成することができます。

 _ **適用対象:** Access apps for SharePoint?| apps for Office?| Excel?| Office Add-ins?| PowerPoint?| Project?| Word_

次の手順に従って、作業ウィンドウ アドインまたはコンテンツ アドインのマニフェストを SharePoint の Office アドイン カタログにアップロードします。

 >**メモ**  「Office 用アプリ」という名称は、「 Office アドイン」に変更されました。移行中、一部の Office アプリケーションと Visual Studio ツールのドキュメントと UI で、まだ「アプリ」という名称を使用している可能性があります。詳しくは、「 [Office と SharePoint のアプリの新しい名前: Office アドインと SharePoint アドイン](https://msdn.microsoft.com/ja-jp/library/fp161507.aspx#Anchor_2) 」をご覧ください。


## アドイン カタログを作成または検索する

アドイン カタログをセットアップするには、「 [SharePoint 上でアドイン カタログをセットアップする](../publish/set-up-an-add-in-catalog-on-sharepoint.md)」または「 [Office 365 でアドイン カタログをセットアップする](../../publish/set-up-an-add-in-catalog-on-office-365.md)」をご参照ください。

SharePoint の Web アプリケーション用にアドイン カタログが既にセットアップされている場合は、次の手順で検索します。


1. SharePoint サーバーの全体管理メイン ページを開きます。
    
2.  **[アドイン]** を選択します。
    
3. [ **アドイン カタログの管理**] を選択します。
    
4. 表示されたリンクを選択し、左側のナビゲーション バーで [ **Office アドイン**] を選択します。
    

## アドイン カタログへの発行


1. アドイン カタログを参照します。
    
2. [ **新しいアイテムの追加**] リンクを選択します。
    
3.  **[参照]** を選択し、アップロードする [[マニフェスト]](../../docs/overview/add-in-manifests.md) を指定します。
    
    これで、このカタログ内のコンテンツ アドインと作業ウィンドウ アドインを [ **Office アドイン**] ダイアログ ボックスから利用できます。それらにアクセスするには、 [ **挿入**] タブで [ **自分のアドイン**] を選択してから、[ **自分の所属組織**] を選択します。
    
アドイン マニフェストを Office アドイン カタログにアップロードすると、ユーザーは次の操作を行ってアドインにアクセスできます。


1. Office アプリケーションで、 **[ファイル]** > **[オプション]** > **[セキュリティ センター]** > **[セキュリティ センターの設定]** > **[信頼できるアドイン カタログ]** の順に移動します。
    
2. アドイン カタログの  _親 SharePoint サイト コレクション_ の URL を指定します。たとえば、Office アドイン カタログの URL が次のような場合:
    
    https:// _domain_ /sites/ _AddinCatalogSiteCollection_ /AgaveCatalog
    
    単に親サイト コレクションの URL を指定します:
    
    https:// _domain_ /sites/ _AddinCatalogSiteCollection_
    
3. Office アプリケーションを閉じて、再度開きます。アドイン カタログが  **[Office アドイン]** ダイアログ ボックスに表示されます。
    
または、管理者はグループ ポリシーを使用することにより SharePoint 上の Office アドイン カタログを指定できます。詳細については、TechNet の「[Office アドインの概要](https://technet.microsoft.com/ja-jp/library/jj219429.aspx)」にある「グループ ポリシーを使用して、ユーザーが Office アドインをインストールおよび使用する方法を管理する」のセクションをご参照ください。


## その他の技術情報


- [Office アドインを発行する](../publish/publish.md)
    
- [SharePoint 上でアドイン カタログをセットアップする](../publish/set-up-an-add-in-catalog-on-sharepoint.md)
    
- [Office 365 でアドイン カタログをセットアップする](../../publish/set-up-an-add-in-catalog-on-office-365.md)
    
