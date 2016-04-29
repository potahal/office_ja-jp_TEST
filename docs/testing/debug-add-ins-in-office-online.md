
# Office Online でアドインをデバッグする
たとえば Mac で開発している場合など、Windows プラットフォームで Office を実行せずに、Office Online を使用してアドインをデバッグします。

 _ **適用対象:** apps for Office?| Excel?| Office Add-ins?| Word_

Windows、Office 2013、または Office 2016 デスクトップ クライアント以外を実行しているコンピューターでアドインの作成とデバッグを行えます。この記事では、Office Online を使用してアドインのテストとデバッグを行う方法について説明します。 

開始するには、


- Office 365 の開発者アカウントをまだお持ちでない場合はこれを取得します。または SharePoint サイトにアクセスできるようにします。
    
     >**メモ**  無料の Office 365 の開発者アカウントにサイン アップするには、 [Office 365 開発者向けプログラム](https://dev.office.com/devprogram)に参加します。
- Office 365 (SharePoint Online) にアドイン カタログをセット アップします。アドイン カタログは、Office アドインのドキュメント ライブラリをホストする SharePoint Online の専用のサイト コレクションです。独自の SharePoint サイトをお持ちの場合は、アドイン カタログ ドキュメント ライブラリを設定できます。詳細については、以下を参照してください。
    
      - [SharePoint Online 上でアドイン カタログをセットアップする](https://msdn.microsoft.com/JA-JP/library/office/dn574752.aspx)
    
  - [SharePoint 上でアドイン カタログをセットアップする](https://msdn.microsoft.com/JA-JP/library/office/fp123530.aspx)
    

## Excel Online または Word Online からアドインをデバッグする

Office Online を使用してアドインをデバッグするには、


1. SSL をサポートするサーバーにアドインを展開します。
    
     >**メモ**  アドインを作成およびホストするには、 [Yeoman ジェネレーター](https://github.com/OfficeDev/generator-office)の使用をお勧めします。 
2. [アドイン マニフェスト ファイル](../../docs/overview/add-in-manifests.md) で、相対 URI ではなく絶対 URI を含めるように **SourceLocation** 要素の値を更新します。たとえば、次の操作を実行します。
    
     ` <SourceLocation DefaultValue="https://localhost:44300/App/Home/Home.html" />`
    
3. SharePoint のアドイン カタログにある Office アドイン ライブラリにマニフェストをアップロードします。
    
4. Office 365 のアプリ起動ツールから Excel Online または Word Online を起動し、新しいドキュメントを開きます。
    
5. [挿入] タブで、 **[個人用アドイン]** または **[Office アドイン]** をクリックし、アプリにアドインを挿入してテストします。
    
6. お気に入りのブラウザーのツール デバッガーを使用してアドインをデバッグします。
    
    デバッグ時に発生する可能性がある問題のいくつかは次のとおりです。
    
      - 表示される JavaScript エラーのいくつかは Office Online に起因している可能性があります。
    
  - ブラウザーが、バイパスが必要になる、無効な証明書エラーを表示することがあります。
    
  - コードにブレークポイントを設定する場合、Office Online から、保存できないというエラーがスローされることがあります。
    

## その他の技術情報



- [Office アドインの設計のベスト プラクティス](d455b76b-4d76-493d-a681-6b02ba1f38a8.md)
    
- [Office ストアに提出されたアプリとアドインの検証ポリシー (バージョン 1.9)](http://msdn.microsoft.com/library/cd90836a-523e-42f5-ab02-5123cdf9fefe%28Office.15%29.aspx)
    
- [効果的な Office ストア アプリおよびアドインを作成する](http://msdn.microsoft.com/library/c66a6e6b-2e96-458f-8f8c-2a499fe942c9%28Office.15%29.aspx)
    
- [Office アドインでのユーザー エラーのトラブルシューティング](../../docs/testing/testing-and-troubleshooting.md)
    
