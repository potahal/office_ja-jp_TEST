
# Outlook アドインのアーキテクチャと機能の概要
Outlook アドインのアーキテクチャと機能について説明します。

 _ **適用対象:** apps for Office?| Office Add-ins?| Outlook_


## アーキテクチャ

Outlook アドインは、XML マニフェストとコード (JavaScript と HTML) で構成されます。マニフェストは、アドインの名前と説明に加えて、アドインを Outlook に統合する方法を指定します。開発者は、マニフェストを使用して、コマンド サーフェスへのボタンを配置したり、正規表現に一致するもののリンクをオフにしたりすることなどができます。また、マニフェストは、アドインの JavaScript と HTML のコードをホストする URL を定義します。

ユーザーまたは管理者がアドインを取得すると、アドインのマニフェストがユーザーのメールボックスまたは組織内に保存されます。Outlook は、起動時に、ユーザーがインストールしたマニフェストをすべて読み込んでから、そのマニフェストを処理してアドインのすべての拡張点を設定します (たとえば、コマンド サーフェスにボタンを表示したり、現在選択されているメッセージに対して正規表現を実行したりします)。その結果、ユーザーがそのアドインを使用できるようになります。

ユーザーがアドインを操作すると、マニフェストで指定されたホストの場所から JavaScript と HTML のファイルが読み込まれます。

アドインは、Office.js API を使用して Outlook アドイン API にアクセスして、Outlook とやり取りします。


**ユーザーが Outlook を起動したときの一般的なコンポーネントの動作**

![Outlook メール アプリ開始時のイベントのフロー](../../images/olowawecon15_LoadingDOMAgaveRuntime.png)
### バージョン管理

Outlook クライアントとアドイン プラットフォームを発展させ、アドインを統合する新しい方法を追加していくときに、すべてのクライアント (Mac、Windows、Web、モバイル) に同時に機能を実装することができないことがあります。そういった状況に対処するため、Microsoft ではマニフェストと API の両方のバージョン管理を行っているため、プラットフォームは常時下位互換性をサポートします。つまり、開発者は、古いクライアントではダウン レベルの方法での動作をし、新しいクライアントでは新機能を利用することができるアドインを作成することができます。バージョン管理のしくみの詳細については、「 [Outlook アドインのマニフェスト](../outlook/manifests/manifests.md)」をお読みください。


## Outlook アドインの機能

Outlook アドインは、さまざまなシナリオをサポートするために使用できる多数の豊富な機能を提供します。


|
|
|**機能**|**説明**|
|:-----|:-----|
|コンテキストに応じたアクティブ化|Outlook アドインはコンテキストに基づいています。Outlook アドインは、次の条件に基づいてアクティブにできます。 
<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>(既定) メールボックスまたは予定表の任意のアイテム</p></li><li><p>特定のアイテムの種類 (メール メッセージ、会議出席依頼メッセージ、または予定) で。</p></li><li><p>アイテムのメッセージ クラスで。</p></li><li><p>メッセージまたは予定の特定のエンティティで。「<span sdata="link"><a href="2cd5d8f1-69b3-4a2a-b31e-81a07a7cdd9f.htm">コンテキスト Outlook アドイン</a></span>」を参照してください。 </p></li><li><p>特定のルールまたは正規表現。「<span sdata="link"><a href="b3fd6d69-b968-461d-a40e-6063f4febfe6.htm">Outlook アドインのアクティブ化ルール</a></span>」および「<span sdata="link"><a href="93504f92-896f-4c80-9205-ba0b125f4290.htm">正規表現アクティブ化ルールを使用して Outlook アドインを表示する</a></span>」を参照してください。 </p></li><li><p>プロパティの一致する文字列。「<span sdata="link"><a href="a6b0904b-afe9-4882-9136-3d8cfd57fcf8.htm">Outlook アイテム内の文字列を既知のエンティティとして照合する</a></span>」を参照してください。</p></li></ul>|
|アドイン コマンド|Outlook アドイン コマンドは、リボンから特定のアドイン操作を開始する方法を提供します。これらのコマンドは、すべてのメールやイベントに適用されるアドインでのみ使用できます。詳細は、「 [Outlook のアドイン コマンド](../outlook/add-in-commands-for-outlook.md)」を参照してください。 |
|ローミングの設定|Outlook アドインでは、ユーザーのメールボックスに固有のデータを、ユーザーが後の Outlook セッションでアクセスできるように保存しておくことができます。詳細は、「 [Outlook アドインのアドイン メタデータの取得と設定](../outlook/metadata-for-an-outlook-add-in.md)」を参照してください。 |
|カスタム プロパティ|Outlook アドインでは、ユーザーのメールボックス内のアイテムに固有なデータを、ユーザーが後の Outlook セッションでアクセスできるように保存しておくことができます。詳細は、「 [Outlook アドインのアドイン メタデータの取得と設定](../outlook/metadata-for-an-outlook-add-in.md)」を参照してください。|
|添付ファイルまたは選択されたアイテム全体を取得する|Outlook アドインは、サーバー側から添付ファイルおよび選択されたアイテム全体にアクセスできます。以下を参照してください。
<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>添付ファイル - 「<span sdata="link"><a href="0f872924-ea1a-4aa2-bb7b-e12d31014612.htm">サーバーから Outlook アイテムの添付ファイルを取得する</a></span>」および「<span sdata="link"><a href="62669c4d-6829-4476-bac2-cac95fc0961e.htm">Outlook で新規作成フォームのアイテムに添付ファイルを追加および削除する</a></span>」を参照してください。</p></li><li><p>選択されたアイテム全体 - コールバック トークンを使用した添付ファイルの取得に似ています。以下を参照してください。</p><ul><li><p><a href="../reference/outlook/Office.context.mailbox.html(Office.15).aspx#getCallbackTokenAsync" target="_blank">mailbox.getCallbackTokenAsync</a>  メソッド。Exchange サーバーのアドインのサーバー側コードを識別するコールバック トークンを指定します。</p></li><li><p><a href="https://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html(Office.15).aspx#itemId" target="_blank">item.itemId</a> プロパティ。ユーザーが読み取り、サーバー側コードが取得するアイテムを指定します。</p></li><li><p><a href="https://dev.outlook.com/reference/add-ins/Office.context.mailbox.md(Office.15).aspx#ewsUrl" target="_blank">mailbox.ewsUrl</a> プロパティ。EWS エンドポイント URL を指定します。サーバー側コードはこれをコールバック トークンやアイテム ID と一緒に使用して、<a href="http://msdn.microsoft.com/en-us/library/e3590b8b-c2a7-4dad-a014-6360197b68e4(Office.15).aspx" target="_blank">GetItem</a> EWS 操作にアクセスし、アイテム全体を取得することができます。</p></li></ul></li></ul>|
|ユーザー プロファイル|メール アドインでは、ユーザーのプロファイル内の表示名、メール アドレス、タイム ゾーンにアクセスできます。詳しくは、「 [UserProfile](../reference/outlook/Office.context.mailbox.userProfile.md%28Office.15%29.md) オブジェクト」をご覧ください。|

## Outlook アドインの作成を開始する

Outlook アドインの作成を開始するには、「 [Get Started with Outlook add-ins for Office 365](https://dev.outlook.com/MailAppsGettingStarted/GetStarted.aspx)」をご覧ください。


## その他のリソース


一般的に、Office アドイン の開発に適用される概念については、以下の資料を参照してください。


- [Office アドインの設計ガイドライン](../add-in-design.md)
    
- [Office アドイン開発のベスト プラクティス](../../docs/design/add-in-development-best-practices.md)
    
- [Office アドインおよび SharePoint アドインのライセンス](http://msdn.microsoft.com/library/3e0e8ff6-66d6-44ff-b0c2-59108ebd9181%28Office.15%29.aspx)
    
- [Office ストアに Office アドインと SharePoint アドインおよび Office 365 Web アプリを提出する](http://msdn.microsoft.com/library/ff075782-1303-4517-91cc-b3d730e9b9ae%28Office.15%29.aspx)
    
- [JavaScript API for Office](http://msdn.microsoft.com/ja-jp/library/fp142185%28v=office.15%29.aspx(Office.15).aspx)
    
- [メール アドイン マニフェスト](../outlook/manifests/manifests.md)
    
