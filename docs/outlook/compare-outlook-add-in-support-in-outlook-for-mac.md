
# Outlook for Mac と他の Outlook ホストの Outlook アドイン サポートの比較
Outlook for Mac でサポートされる Outlook アドインと、Outlook ホストでサポートされるその他のアドインには違いがあります。

 _ **適用対象:** apps for Office?| Office Add-ins?| Outlook_

Outlook for Mac でも、Outlook for Windows、デバイス用 OWA、および Outlook Web App などの他のホストで行うのと同様に Outlook アドインを作成して実行することができ、ホストごとに JavaScript をカスタマイズする必要はありません。通常、アドインから JavaScript API for Office に対する同じ呼び出しは、以下の表に示す領域を除き同様の動作をします。

 >**メモ**  Outlook for Mac は、Outlook の閲覧モードでのみ JavaScript API for Office をサポートします。



|**分野**|**Outlook for Windows、デバイス用 OWA、Outlook Web App**|**Outlook for Mac**|
|:-----|:-----|:-----|
|サポート対象バージョンの office.js および Office アドインのマニフェスト スキーマ|Office.js および スキーマ v1.1 のすべての API。|
<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>閲覧モードで適用可能な API のみ。<a href="72915b13-720f-4dc5-b5d1-4676e2a536ba.htm#mod_off15_WhatsNewMailApps_NewJSAPI">office.js v1.1 の新しい API や拡張された API</a> を使用するアドインはアクティブ化できますが、新規作成モード用の API は Outlook for Mac では正しく実行されません。 </p></li><li><p>スキーマ v1.1。</p></li></ul>|
|定期的な予定系列のインスタンス|
<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>定期的な系列のマスター予定または予定インスタンスのアイテム ID および他のプロパティを取得できます。 </p></li><li><p><a href="../reference/outlook/Office.context.mailbox.md(Office.15).aspx#displayAppointmentForm" target="_blank">mailbox.displayAppointmentForm</a> を使用して、定期的な系列のインスタンスまたはマスターを表示できます。</p></li></ul>|
<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>マスター予定のアイテム ID と他のプロパティを取得できますが、定期的な系列のインスタンスのアイテム ID とプロパティは取得できません。</p></li><li><p>定期的な系列のマスター予定を表示できます。アイテム ID がない場合、定期的な系列のインスタンスは表示できません。</p></li></ul>|
|予定出席者の受信者の種類|[EmailAddressDetails.recipientType](../reference/outlook/simple-types.md%28Office.15%29.md) を使用して、出席者の受信者の種類を特定できます。|**EmailAddressDetails.recipientType** は、予定出席者に対して **undefined** を返します。|
|ホストのバージョン文字列 |[diagnostics.hostVersion](../reference/outlook/Office.context.mailbox.diagnostics.md%28Office.15%29.md) から返されるバージョン文字列の形式は、ホストの実際の種類によって異なります。以下に例を示します。
<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>Outlook for Windows: 15.0.4454.1002</p></li><li><p>Outlook Web App: 15.0.918.2</p></li></ul>|Outlook for Mac 上の  **Diagnostics.hostVersion** によって戻されるバージョン文字列の例: 15.0 (140325)|
|アイテムのカスタム プロパティ|ネットワークが使用できなくなっても、アドインはキャッシュに入っているカスタム プロパティに引き続きアクセスできます。|Outlook for Mac はカスタム プロパティをキャッシュに入れないので、ネットワークが使用できなくなると、アドインはそれらのプロパティにはアクセスできなくなります。|
|添付ファイルの詳細|[AttachmentDetails](https://dev.outlook.com/reference/add-ins/simple-types.mdl.aspx#AttachmentDetails) オブジェクトのコンテンツ タイプと添付ファイルの名前は、ホストの種類によって異なります。
<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><span class="keyword">AttachmentDetails.contentType</span> の JSON の例: <span class="keyword">"contentType": "image/x-png"</span>。 </p></li><li><p><span class="keyword">AttachmentDetails.name</span> にはファイル名拡張子は含まれません。たとえば、添付ファイルが「RE: Summer activity」という件名のメッセージの場合、添付ファイル名を表す JSON オブジェクトは <span class="keyword">"name": "RE: Summer activity"</span> となります。</p></li></ul>|
<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><span class="keyword">AttachmentDetails.contentType</span> の JSON の例: <span class="keyword">"contentType": "image/png"</span></p></li><li><p><span class="keyword">AttachmentDetails.name</span> には、ファイル名拡張子が必ず含まれます。メール アイテムの添付ファイルの拡張子は .eml で、予定の拡張子は .ics です。添付ファイルが「RE: Summer activity」という件名の電子メールである場合、その添付ファイル名を表す JSON オブジェクトは <span class="keyword">"name": "RE: Summer activity.eml"</span> となります。</p></li></ul>|
|**dateTimeCreated** プロパティおよび **dateTimeModified** プロパティでのタイム ゾーンを表す文字列|例: Thu Mar 13 2014 14:09:11 GMT+0800 (中国標準時)|例: Thu Mar 13 2014 14:09:11 GMT+0800 (CST)|
|**dateTimeCreated** および **dateTimeModified** の時間の精度|アドインが以下のコードを使用する場合、精度は最大でミリ秒単位になります。
```
JSON.stringify(Office.context.mailbox.item, null, 4);

```

|精度は最大で秒単位となります。|

## その他の技術情報



- [テスト用に Outlook アドインを展開してインストールする](../outlook/testing-and-tips.md)
    
