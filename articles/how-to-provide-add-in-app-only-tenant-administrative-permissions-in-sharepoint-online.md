SharePoint Online でアドイン アプリ専用の管理アクセス許可を付与する方法
================================================

SharePoint アドインの開発時に、そのアドインの登録に ACS モデル (appregnew.aspx および appinv.aspx) を使用する場合は、アドインがテナント管理アクセス許可とアプリ専用モードを要求としているときに特別なプロセスに従う必要があります。 

アプリ専用のテナント管理アクセス許可をアドインに付与する手順:

- アドインを展開するテナント内の通常のサイト コレクションでアドインのアプリ ID を登録する 
  - URL: *https://[テナント].sharepoint.com/_layouts/15/appregnew.aspx*
- アドインの登録に必要な詳細情報を指定して、アドインの ID とシークレットを登録する
- テナント管理サイトの appinv.aspx ページに移動する
  - URL: *https://[テナント]-admin.sharepoint.com/_layouts/15/appinv.aspx*
- 前の手順の appinv.aspx で登録したアプリ ID の検索を実行する
- アドインの登録に必要とされるアクセス許可を付与する
- 更新されたアドインを登録するための信頼を実行する

この操作はテナント管理サイトで完了する必要があり、これらの操作に使用するアカウントにはテナントの管理アクセス許可が必要になる点に注意してください。 それよりも低いアクセス許可をアドインに付与する場合、これらの操作は、より低いアクセス許可を持つ通常のサイト コレクションの URL で完了できます。 


## その他のリソース
<a name="bk_addresources"> </a>

- [SharePoint アドイン 2013 を登録する](https://msdn.microsoft.com/en-us/library/office/jj687469.aspx)
    
- [SharePoint 2013 でのアドインのアクセス許可](https://msdn.microsoft.com/en-us/library/office/fp142383.aspx)

