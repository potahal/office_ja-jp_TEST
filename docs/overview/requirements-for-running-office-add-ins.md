
# Office アドインを実行するための要件
Office アドインを実行するためのソフトウェアとデバイスの要件について説明します。

 _ **適用対象:** Access apps for SharePoint?| apps for Office?| Excel?| Office Add-ins?| Outlook?| PowerPoint?| Project?| Word_


## サーバーの要件

Office アドインをインストールおよび実行できるようにするには、まずアドインの UI とコードのマニフェストと Web ページ ファイルを、適切なサーバーの場所に展開する必要があります。

すべての種類のアドイン (コンテンツ、Outlook、作業ウィンドウ) で、アドインの Web ページ ファイルを Web サーバー、または [Microsoft Azure](../publish/host-an-office-add-in-on-microsoft-azure.md) などの Web ホスティング サービスに展開する必要があります。


 >**メモ**  Visual Studio でアドインの開発およびデバッグを行う場合、Visual Studio は IIS Express を使用してローカルにアドインの Web ページ ファイルを展開および実行するため、追加の Web サーバーは不要になります。同様に、ブラウザーで Napa を使用して開発およびデバッグを行う場合は、Napa へのサインインに使用するアカウントに関連付けられたストレージからアドインの Web ページのファイルを展開および実行します。

サポートされている Office ホスト アプリケーション (Access Web アプリ、Word、Excel、PowerPoint、または Project) のコンテンツ アドインと作業ウィンドウ アドインでは、アドインの XML マニフェスト ファイルをアップロードするために、 [ネットワーク ファイル共有](../publish/create-a-network-shared-folder-catalog-for-task-pane-and-content-add-ins.md)または SharePoint の [アドイン カタログ](../publish/publish-task-pane-and-content-add-ins-to-an-add-in-catalog.md)も必要になります。

Outlook アドインをテストおよび実行するには、ユーザーの Outlook 電子メール アカウントが、Office 365、Exchange Online、またはオンプレミスのインストールから使用できる Exchange 2013 以降のバージョン上に存在する必要があります。ユーザーまたは管理者は、サーバー上に Outlook アドインのマニフェスト ファイルをインストールします。

Outlook の POP および IMAP 電子メール アカウントは、Office アドイン をサポートしていません。


## クライアントのサポートの概要

次の表に、Office アドインを実行できる Office ホスト アプリケーション (デスクトップ、タブレット、モバイル、および Web クライアントを含む) と、各ホストがサポートするアドインの種類を示します。


**サポートされるアドインの種類**


|**Office アプリケーション**|**コンテンツ アドイン**|**Outlook アドイン**|**作業ウィンドウ アドイン**|
|:-----|:-----|:-----|:-----|
|Access Web アプリ|
![チェック マーク](../../images/mod_off15_checkmark.png)

|||
|Excel 2013 以降|
![チェック マーク](../../images/mod_off15_checkmark.png)

||
![チェック マーク](../../images/mod_off15_checkmark.png)

|
|Excel Online|
![チェック マーク](../../images/mod_off15_checkmark.png)

||
![チェック マーク](../../images/mod_off15_checkmark.png)

|
|Excel for iPad|
![チェック マーク](../../images/mod_off15_checkmark.png)

||
![チェック マーク](../../images/mod_off15_checkmark.png)

|
|Outlook 2013 以降||
![チェック マーク](../../images/mod_off15_checkmark.png)

||
|Outlook for Mac||
![チェック マーク](../../images/mod_off15_checkmark.png)

||
|Outlook Web App||
![チェック マーク](../../images/mod_off15_checkmark.png)

||
|デバイス用 OWA||
![チェック マーク](../../images/mod_off15_checkmark.png)

||
|PowerPoint 2013 以降|
![チェック マーク](../../images/mod_off15_checkmark.png)

||
![チェック マーク](../../images/mod_off15_checkmark.png)

|
|PowerPoint Online|
![チェック マーク](../../images/mod_off15_checkmark.png)

||
![チェック マーク](../../images/mod_off15_checkmark.png)

|
|Project 2013 以降|||
![チェック マーク](../../images/mod_off15_checkmark.png)

|
|Word 2013 以降|||
![チェック マーク](../../images/mod_off15_checkmark.png)

|
|Word Online|||
![チェック マーク](../../images/mod_off15_checkmark.png)

|
|Word for iPad|||
![チェック マーク](../../images/mod_off15_checkmark.png)

|

## クライアントの要件: Windows デスクトップおよびタブレット

Windows ベースのデスクトップ、ノート PC、または タブレット デバイス上で実行されるサポート対象の Office デスクトップ クライアントまたは Web クライアント向けの Office アドインを開発するには、以下のソフトウェアが必要です。


- Windows x86 および x64 デスクトップおよび Surface Pro などのタブレット:
    
      - Windows 7 以降のバージョンで実行している Office 2013 以降のバージョンの、32 ビットまたは 64 ビット バージョン。
    
  - Excel 2013、Outlook 2013、PowerPoint 2013、Project Professional 2013、Project 2013 SP1、Word 2013、またはそれ以降の Office クライアントのバージョン (特にこれらの Office デスクトップ クライアントを対象として Office アドインをテストまたは実行する場合)。Office デスクトップ クライアントはオンプレミスでインストールすることも、クイック実行によってクライアント コンピューターにインストールすることもできます。
    
- Internet Explorer 9 以降。インストールが必要ですが、既定のブラウザーである必要はありません。Office アドインをサポートするために、ホストとして機能する Office クライアントは Internet Explorer 9 以上を構成しているブラウザー コンポーネントを使用します。
    
- 次のいずれかの既定のブラウザー: Internet Explorer 9、Safari 5.0.6、Firefox 5、Chrome 13、またはこれらのブラウザーのそれ以降のバージョン。
    
- メモ帳などの HTML および JavaScript エディター、 [Visual Studio および Microsoft Developer Tools](https://www.visualstudio.com/features/office-tools-vs)、またはサードパーティの Web 開発ツール。
    

## クライアントの要件: OS X デスクトップ

Outlook for Mac は Office 365 に付属していて、Outlook アドインをサポートします。Outlook アドインを Outlook for Mac で実行するための要件は、Outlook for Mac そのものの要件と同じです。オペレーティング システムは、少なくとも OS X v10.10 "Yosemite" である必要があります。Outlook for Mac はレイアウト エンジンとして WebKit を使用して、アドイン ページを表示するので、追加のブラウザーの依存関係はありません。


## クライアントの要件: Office Online Web クライアントと SharePoint のブラウザー サポート

Internet Explorer 9、Chrome 13、Firefox 5、Safari 5.0.6、またはこれらのブラウザーの以降のバージョンなど ECMAScript 5.1、HTML5、および CSS3 をサポートする任意のブラウザー。


## クライアントの要件: Windows 以外のスマートフォンおよび タブレット

特に、スマートフォンや Windows 以外のタブレット デバイス上のブラウザーで動作する デバイス用 OWA と Outlook Web App の場合、Outlook アドインをテストおよび実行するのに以下のソフトウェアが必要です。



|**ホスト アプリケーション**|**デバイス**|**オペレーティング システム**|**Exchange アカウント**|**モバイル ブラウザー**|
|:-----|:-----|:-----|:-----|:-----|
|OWA for Android|Android スマートフォン。技術的には、「 [Android OS](http://developer.android.com/guide/practices/screens_support.mdl)」によって「小型」または「標準」に分類されるデバイス。|Android 4.4 KitKat 以降|Office 365 for Business または Exchange Online の最新の更新プログラムが対象|Android 用のネイティブ アドイン、ブラウザーは適用外|
|OWA for iPad|iPad 2 以降|iOS 6 以降|Office 365 for Business または Exchange Online の最新の更新プログラムが対象|iOS 用のネイティブ アドイン、ブラウザーは適用外|
|OWA for iPhone|iPhone 4S 以降|iOS 6 以降|Office 365 for Business または Exchange Online の最新の更新プログラムが対象|iOS 用のネイティブ アドイン、ブラウザーは適用外|
|Outlook Web App|iPhone 4 以降、iPad 2 以降、iPod Touch 4 以降|iOS 5 以降|Office 365、Exchange Online、または Exchange Server 2013 以降の社内型が対象|Safari|

## Office アドイン ソリューションのコンポーネント


標準的な Office アドイン ソリューションには、以下のコンポーネントが必要です。


- サポート対象の Office クライアントを実行しているクライアント デバイス。デスクトップ、ノート PC、タブレット、またはスマートフォン (デバイス用 OWA 上の Outlook アドインの場合) などです。
    
- Access Web アプリ、Word、Excel、PowerPoint、または Project の場合:
    
      - データベース、ドキュメント、ブック、プレゼンテーション、またはプロジェクト。
    
  - ユーザーがパブリック Office ストアや SharePoint またはファイルベースのアドイン カタログからインストールした作業ウィンドウ アドインまたはコンテンツ アドイン。
    
- Outlook の場合:
    
      - ユーザーの電子メール アカウントとメールボックス (Exchange Server 上にあるもの)。
    
  - ユーザーまたは Exchange Server 管理者が Exchange 管理センター (EAC) を介してインストールした Outlook アドイン。
    

 >**メモ**  ユーザーによる Office アドインのインストールは対応する XML マニフェスト ファイルへのポインターで構成され、このマニフェスト ファイルでは実行時にアドインの Web ページやスクリプトの読み込み元となる URL が指定されます。

すべてのサポート対象 Office アプリケーションでは、Office アドイン自体の実装が以下のサーバーベース コンポーネントで構成されます。


- パブリックまたはプライベート アドイン カタログ、あるいはユーザーの Exchange Server にある XML マニフェスト ファイル。
    
- アドインの HTML、CSS、および JavaScript ファイル (開発者が作成し、Web サーバー上にあるもの)。
    
- JavaScript ライブラリ ファイル。たとえば、マイクロソフトが提供する JavaScript API for Office (Office.js) や、Microsoft AJAX Library (MicrosoftAjax.js)。アドインは、その HTML ファイルでの指定に従って、コンテンツ配信ネットワーク (CDN) の URL から JavaScript ライブラリ ファイルにアクセスします。
    
- CDN から外部の JavaScript ライブラリ、または Web サービスを使用している場合は、必ず Secure Sockets Layer (SSL) を使用してこれらのリソースにアクセスするようにします。そうしない場合、アドインを実行するとブラウザーの警告が表示されます。SSL を使用するには、アドインの <SCRIPT> タグ内のリソースに https URL を追加します。
    
サポートされている Office アプリケーションが起動するときに、ユーザー向けに、またはユーザーによってインストールされているアドインの XML マニフェストを読み取ります。その後、ユーザーが Office アプリケーションで Office アドインを開始すると、次のイベントが発生します。


1. Access Web アプリ、Word、Excel、PowerPoint、または Project の場合: ユーザーが Office アドイン を挿入するか、すでにアドインが含まれている Access Web アプリ、ドキュメント、ブック、プレゼンテーション、またはプロジェクトを開くと、Office アプリケーションがアドインをロードするため、その UI がユーザー インターフェイスに表示されます。
    
    Outlook の場合: 現在の Outlook コンテキストがアドインのアクティブ化条件を満たすと、Outlook がアドインをアクティブにするため、そのアドインが選択用の Outlook UI に表示されます。
    
2. Windows または Web ベースの Office アプリケーションの場合: Office アプリケーションが HTML ページを Web ブラウザー コントロール (デスクトップ クライアントまたは ARM 固有のクライアント) または  **iframe** (Web クライアント) 内で開きます。この Web ブラウザー コントロールは、Internet Explorer 9 以上のコンポーネントを使用し、セキュリティとパフォーマンスの分離を実現します。
    
    OS X ベースの Outlook for Mac の場合: Outlook for Mac はセキュリティで保護された WebKit ランタイム ホスト プロセスを使用して、Outlook アドインの HTML ページを開いたり、同様のレベルのセキュリティとパフォーマンス保護を提供したりします。
    
3. 対応するブラウザー コントロール  **iframe**、または Webkit ランタイム ホスト プロセスが HTML 本文を読み込み、 **onload** イベントに対するイベント ハンドラーを呼び出します。
    
4. Office アドイン フレームワークが [Office](http://msdn.microsoft.com/library/727adf79-a0b5-48d2-99c7-6642c2c334fc%28Office.15%29.aspx) オブジェクトの [initialize](http://msdn.microsoft.com/library/c490b13d-ee52-4291-af5d-f4a5a11d3af0%28Office.15%29.aspx) イベントに対するイベント ハンドラーを呼び出します。
    
5. HTML 本文の読み込みとアドインの初期化が終了すると、アドインのメイン関数は処理を続行できます。
    

## その他の技術情報



- [Office アドイン プラットフォームの概要](../../docs/develop/privacy-and-security.md)
    
