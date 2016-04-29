
# テスト用に Outlook アドインを展開してインストールする
Outlook アドイン開発サイクルの一環として、Outlook アドインをテスト用に展開およびインストールする方法について説明します。 

 _ **適用対象:** apps for Office?| Office Add-ins?| Outlook_


## Outlook アドインのテスト用の展開


Outlook アドインを開発するプロセスの一環として、テスト用にアドインの展開およびインストールを繰り返し行うことが多くあります。その場合は、以下の手順が必要です。


1. アドインを記述したマニフェスト ファイルを作成します。
    
2. アドインの UI ファイルを Web サーバーに展開します。
    
3. アドインをメールボックスにインストールします。
    
4. アドインをテストし、UI ファイルまたはマニフェスト ファイルを適切に変更します。さらに、手順 2 および 3 を繰り返して、変更箇所をテストします。
    

### アドイン用のマニフェスト ファイルの作成

各アドインは XML のマニフェストで記述されます。マニフェストは、アドインに関する情報をサーバーに提供し、ユーザーに向けたアドインについての説明的な情報を提供し、アドイン UI の HTML ファイルの場所を識別するドキュメントです。このマニフェストはローカル フォルダーにもサーバーにも保存できますが、その場所は、テストに使用するメールボックスの Exchange サーバーからアクセス可能な場所である必要があります。ここでの説明では、マニフェストがローカル フォルダーに保存されていることを想定しています。マニフェスト ファイルの作成方法については、「 [Outlook アドインのマニフェスト](../outlook/manifests/manifests.md)」を参照してください。 


### Web サーバーへのアドインの展開

HTML と JavaScript を使用して、アドインの UI を作成できます。結果として得られるソース ファイルは、このアドインをホストする Exchange サーバーからアクセス可能な Web サーバー上に保存されます。このソース ファイルは、アドイン マニフェスト ファイル内で指定されている [DesktopSettings](http://msdn.microsoft.com/ja-jp/library/da9fd085-b8cc-2be0-d329-2aa1ef5d3f1c%28Office.15%29.aspx) 要素、 [TabletSettings](http://msdn.microsoft.com/ja-jp/library/5c89cc7c-7ae0-49c9-fdd5-4c52118228f6%28Office.15%29.aspx) 要素、および [PhoneSettings](http://msdn.microsoft.com/ja-jp/library/13e4eae3-8e8c-fd55-a1c2-3297b485f327%28Office.15%29.aspx) 要素の **SourceLocation** 子要素によって識別されます。

アドインの UI ファイルを初期展開した後は、Web サーバー上に保存されている HTML ファイルを新しいバージョンの HTML ファイルに置き換えることで、アドインの UI と動作を更新できます。


## アドインのインストール


アドイン マニフェスト ファイルを準備して、アクセス可能な Web サーバーにアドイン UI を展開した後は、Outlook リッチ クライアント、Outlook Web App、またはデバイス用 OWA を使用するか、または Windows PowerShell コマンドレットをリモートで実行することで、アドインを Exchange サーバーのメールボックスにインストールできます。


### Outlook リッチ クライアントへのアドインのインストール

メールボックスを Exchange Online、Exchange 2013 またはそれ以降のリリース上に配置している場合は、アドインをインストールできます。Outlook for Windows の場合は、Office Fluent Backstage ビューからアドインをインストールできます。この場合は、 **[ファイル]** を選択してから **[アドインの管理]** を選択します。これを選択すると、Exchange 管理センターにサインインできます。サインインした後は、次のセクションの手順 4 に進んでインストール プロセスを続行します。

Outlook for Mac の場合は、アドイン バーの右端にある [ **アドインの管理**] を選択して、Exchange 管理センターにサインインします。次のセクションの手順 4 に進みます。 


### Outlook Web App または Outlook.com を使用したアドインのインストール

Outlook Web App (OWA) を使用して Outlook アドインをインストールするには、以下の手順を実行します。


1. 組織の OWA URL、または Outlook.com に移動して、ログインします。
    
2. 右上隅にある歯車アイコンをクリックし、 **[アドインの管理]** を選択します。
    
3. 4. プラス記号 ( **+**) を選択して、新しいアドインを追加します。
    
5. ドロップダウン リストで [ **ファイルから追加**] を選択します (マニフェストをローカル フォルダーに保存している場合を想定しています)。
    
6. マニフェストのファイル パスを参照し、 [ **インストール**] を選択します。
    
7. ウィンドウの右上隅にあるユーザー名を選択します。次に、 **[個人用メール]** を選択して自分の電子メールに切り替えたら、アドインをテストします。
    

 >**メモ**  アドインの開発に次のどちらも使用しない場合: また、少なくとも Exchange Server の "My Custom Apps" の役割を持っていない場合は、アドインのインストールを Office ストアからのみ行えます。アドイン マニフェストの URL またはファイル名を指定してアドインをテストしたり、一般のアドインをインストールしたりする場合は、Exchange 管理者に連絡して、必要なアクセス許可を得る必要があります。Exchange 管理者は、次のような PowerShell コマンドレットを実行して、必要となるアクセス許可を単一ユーザーに割り当てることができます。この例の場合、wendyri はユーザーの電子メール エイリアスです。New-ManagementRoleAssignment ?Role "My Custom add-ins" ?User "wendyri"必要な場合、管理者は次のようなコマンドレットを実行して、必要となる同じアクセス許可を複数のユーザーに割り当てることができます。$users = Get-Mailbox *$users | ForEach-Object { New-ManagementRoleAssignment -Role "My Custom add-ins" -User $_.Alias}My Custom add-ins の役割の詳細については、「[自分のカスタム アドインの役割](http://technet.microsoft.com/ja-jp/library/aa0321b3-2ec0-4694-875b-7a93d3d99089%28exchg.150%29.aspx)」を参照してください。Office 365、Napa、または Visual Studio を使用してアドインを開発すると、組織の管理者の役割が割り当てられ、EAC でファイルや URL を使用して、あるいは Powershell コマンドレットを使用してアドインをインストールできるようになります。


### リモート PowerShell を使用したアドインのインストール

Exchange サーバー上に Windows PowerShell のリモート セッションを作成した後、次の PowerShell コマンドによって  **New-App** コマンドレットを使用して Outlook アドインをインストールできます。


```
New-App ?URL:"http://<fully-qualified URL">
```

完全修飾 URL は、アドイン用に準備したアドイン マニフェスト ファイルの場所です。

さらに、次の PowerShell コマンドレットを使用すると、メールボックス用のアドインを管理できます。


-  **Get-App** ? メールボックスに対して有効になっているアドインを一覧表示します。
    
-  **Set-App** ? メールボックスに対してアドインを有効または無効にします。
    
-  **Remove-App** ? 現在インストールされているアドインを Exchange サーバーから削除します。
    

## このセクションの内容



- [Outlook アドインで日付値を扱うためのヒント](tips-for-handling-date-values-in-outlook-add-ins.md)
    
- [Outlook アドインのアクティブ化と JavaScript API の制限](../outlook/limits-for-activation-and-javascript-api-for-outlook-add-ins.md)
    
- [Outlook アドインのアクティブ化のトラブルシューティング](../../outlook/troubleshoot-outlook-add-in-activation.md)
    

## その他のリソース



- [Outlook アドイン](../outlook/outlook-add-ins.md)
    
- [Office アドインでのユーザー エラーのトラブルシューティング](../../docs/testing/testing-and-troubleshooting.md)
    
