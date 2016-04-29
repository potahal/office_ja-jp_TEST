
# JavaScript API for Office の変更点
JavaScript API for Office は、Office アドインの機能を拡張するため、オブジェクト、メソッド、プロパティ、イベント、列挙体の新規追加や更新によって定期的に更新が加えられています。新規および更新された API のメンバーを確認するには、次のリンクを参照してください。

 _ **適用対象:** Access apps for SharePoint?| apps for Office?| Excel?| Office Add-ins?| Outlook?| PowerPoint?| Project?| Word_

新しい API メンバーを使用してアドインを開発するには、 [プロジェクトで JavaScript API for Office ファイルを更新する](../docs/develop/update-your-javascript-api-for-office-and-manifest-schema-version.md)必要があります。

前回の更新から変更されていない API メンバーを含むすべての API メンバーを表示するには、「 [JavaScript API for Office](http://msdn.microsoft.com/library/b27e70c3-d87d-4d27-85e0-103996273298%28Office.15%29.aspx)」を参照してください。


## 新規および更新された API

 **新規オブジェクトと更新されたオブジェクト**



|**オブジェクト**|**説明**|**追加または更新されたバージョン **|
|:-----|:-----|:-----|
|[Item](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md)|
<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>メッセージまたは予定の件名と本文でユーザーの選択内容の取得と上書きをサポートする <a href="http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html(Office.15).aspx#getSelectedDataAsync" target="_blank">getSelectedDataAsync</a> メソッドと <a href="http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html(Office.15).aspx#setSelectedDataAsync" target="_blank">setSelectedDataAsync</a> メソッドが追加されました。</p></li><li><p><a href="http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html(Office.15).aspx#displayReplyAllForm" target="_blank">displayReplyAllForm</a> メソッドと <a href="http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html(Office.15).aspx#displayReplyForm" target="_blank">displayReplyForm</a> メソッドが、予定の回答フォームへの添付ファイルの追加をサポートするよう更新されました。</p></li></ul>|Mailbox 1.2|
|[Item](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md)|新規作成モードの Outlook アドインを作成するためのメソッドとフィールドを含めるよう更新されました。 |1.1|
|[Binding](http://msdn.microsoft.com/library/42882642-d22b-47d2-a8d3-3aa8c6a4435e%28Office.15%29.aspx)|Access 用コンテンツ アドインにおけるテーブル バインドをサポートするよう更新されました。|1.1|
|[Bindings](http://msdn.microsoft.com/library/09979e31-3bfb-45be-adda-0f7cc2db1fe1%28Office.15%29.aspx)|Access 用コンテンツ アドインにおけるテーブル バインドをサポートするよう更新されました。|1.1|
|[Body](http://dev.outlook.com/reference/add-ins/Body.html%28Office.15%29.md)|新規作成モードの Outlook アドインでメッセージや予定の本文を作成および編集できるよう追加されました。|1.1|
|[Document](http://msdn.microsoft.com/library/f8859516-cc1f-4b20-a8f3-cee37a983e70%28Office.15%29.aspx)|次に対して更新および追加が行われました。
<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>Access 用のコンテンツ アドインで <a href="http://msdn.microsoft.com/library/551369c3-315b-428f-8b7e-08987f6b0e00(Office.15).aspx" target="_blank">mode</a>、<a href="http://msdn.microsoft.com/library/77ba7daf-419f-44b6-8747-7fd5618b7053(Office.15).aspx" target="_blank">settings</a>、および <a href="http://msdn.microsoft.com/library/480ac3c6-370e-4505-aba3-1d0dce9fb3dc(Office.15).aspx" target="_blank">url</a> の各プロパティをサポートします。</p></li><li><p>PowerPoint および Word 用アドインで、<a href="http://msdn.microsoft.com/library/35dda81c-235e-4eab-8a77-9acb3b73a380(Office.15).aspx" target="_blank">getFileAsync</a> メソッドを使用してドキュメントを PDF として取得します。</p></li><li><p>Excel、PowerPoint、および Word 用アドインで、<a href="http://msdn.microsoft.com/library/2533a563-95ae-4d52-b2d5-a6783e4ef5b4(Office.15).aspx" target="_blank">getFileProperties</a> メソッドを使用してファイルのプロパティを取得します。</p></li><li><p>Excel および PowerPoint 用アドインで、<a href="http://msdn.microsoft.com/library/35dda81c-235e-4eab-8a77-9acb3b73a380(Office.15).aspx" target="_blank">goToByIdAsync</a> メソッドを使用して、ドキュメント内の場所とオブジェクトに移動します。</p></li><li><p>PowerPoint 用アドインで、<a href="http://msdn.microsoft.com/library/f85ad02c-64f0-4b73-87f6-7f521b3afd69(Office.15).aspx" target="_blank">getSelectedDataAsync</a> メソッドを使用して (新しい <span class="keyword">Office.CoercionType.SlideRange</span><a href="http://msdn.microsoft.com/library/735eaab6-5e31-4bc2-add5-9d378900a31b(Office.15).aspx" target="_blank">coercionType</a> 列挙体を指定した場合)、選択したスライドの ID、タイトル、およびインデックスを取得します。</p></li></ul>|1.1|
|[Location](http://dev.outlook.com/reference/add-ins/Location.html%28Office.15%29.md)|新規作成モードの Outlook アドインで予定の場所を設定できるよう追加されました。|1.1|
|[Office](http://msdn.microsoft.com/library/c490b13d-ee52-4291-af5d-f4a5a11d3af0%28Office.15%29.aspx)|Access 用コンテンツ アドインにおけるバインドの取得をサポートするよう select メソッドが更新されました。|1.1|
|[Recipients](http://dev.outlook.com/reference/add-ins/Recipients.html%28Office.15%29.md)|新規作成モードでメッセージや予定の受信者を取得および設定できるよう追加されました。|1.1|
|[Settings](http://msdn.microsoft.com/library/ad733387-a58c-4514-8fc2-53e64fad468d%28Office.15%29.aspx)|Access 用コンテンツ アドインにおけるカスタム設定の作成をサポートするよう更新されました。|1.1|
|[Subject](http://dev.outlook.com/reference/add-ins/Subject.html%28Office.15%29.md)|新規作成モードの Outlook アドインでメッセージや予定の件名を取得および設定できるよう追加されました。|1.1|
|[Time](http://dev.outlook.com/reference/add-ins/Time.html%28Office.15%29.md)|新規作成モードの Outlook アドインで予定の開始時刻および終了時刻を取得および設定できるよう追加されました。|1.1|

**新規列挙体および更新された列挙体**


|**オブジェクト**|**説明**|**バージョン**|
|:-----|:-----|:-----|
|[ActiveView](http://msdn.microsoft.com/library/1f1d963e-04e1-4cf2-b161-5329d7ad0a3e%28Office.15%29.aspx)|ユーザーがドキュメントを編集できるかどうかなど、ドキュメントのアクティブなビューの状態を示します。PowerPoint 用アドインで、ユーザーがプレゼンテーション ( **スライド ショー**) を閲覧しているのか、スライドを編集しているのかを判断できるように追加されました。 |1.1|
|[CoercionType](http://msdn.microsoft.com/library/735eaab6-5e31-4bc2-add5-9d378900a31b%28Office.15%29.aspx)|PowerPoint 用アドインで、 **Office.CoercionType.SlideRange** メソッドを使用して選択されたスライドの範囲の取得をサポートするよう **getSelectedDataAsync** が更新されました。|1.1|
|[EventType](http://msdn.microsoft.com/library/82c79659-52da-48b0-92a9-831226eb9a7f%28Office.15%29.aspx)|新しい ActiveViewChanged イベントを含めるよう更新されました。|1.1|
|[FileType](http://msdn.microsoft.com/library/fadbb4cf-a0e4-47b2-93dd-123f0b06d4ae%28Office.15%29.aspx)|PDF 形式での出力を指定するよう更新されました。|1.1|
|[GoToType](http://msdn.microsoft.com/library/8de45be3-de35-4765-a67a-e128a46786bd%28Office.15%29.aspx)|ドキュメントの移動先の場所またはオブジェクトを指定するよう追加されました。|1.1|

## その他の技術情報


- [Office アドインの API とスキーマ参照](../../reference/reference.md)
    
- [Office アドイン](../../docs/overview/office-add-ins.md)
    
