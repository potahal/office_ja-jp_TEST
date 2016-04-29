
# コンテンツ アドインおよび作業ウィンドウ アドインでの API 使用のアクセス許可を要求する
この記事では、アドインの機能のために必要となる JavaScript API アクセスのレベルを指定するために、コンテンツ アドインまたは作業ウィンドウ アドインのマニフェストで宣言できるさまざまなアクセス許可レベルについて説明します。 

 _ **適用対象:** Access apps for SharePoint?| apps for Office?| Excel?| Office Add-ins?| PowerPoint?| Project?| Word_


## アクセス許可モデル


5 レベルの JavaScript API アクセス許可モデルは、コンテンツ アドインと作業ウィンドウ アドインでのユーザーのプライバシーとセキュリティの基礎となります。図 1 に、アドイン マニフェストで宣言できる 5 レベルの API アクセス許可を示します。


**図 1. コンテンツ アドインと作業ウィンドウ アドインの 5 レベル アクセス許可モデル**

![作業ウィンドウ アプリの権限レベル](../../images/off15appsdk_TaskPaneAppPermission.gif)



これらのアクセス許可は、ユーザーがアドインを挿入してアクティブ化 (信頼) したときに、アドイン ランタイムがコンテンツ アドインまたはタスク ウィンドウ アドインに使用を許可する API のサブセットを指定します。コンテンツ アドインまたは作業ウィンドウ アドインに必要なアクセス許可レベルを宣言するには、アドインのマニフェストの [Permissions](http://msdn.microsoft.com/ja-jp/library/d4cfe645-353d-8240-8495-f76fb36602fe%28Office.15%29.aspx) 要素に、いずれかのアクセス許可テキスト値を指定します。以下の例では、ドキュメントに書き込みができる (しかし読み取りはできない) メソッドだけを許可する、 **WriteDocument** アクセス許可を要求します。




```XML
<Permissions>WriteDocument</Permissions>
```

ベスト プラクティスとして、最小特権の原則に基づいてアクセス許可を要求します。つまり、アドインが正常に機能するために必要な API の最小のサブセットにアクセスする許可だけを要求する必要があります。たとえば、アドインの機能のためにはユーザーのドキュメント内のデータを読み取ることだけが必要な場合は、 **ReadDocument** 許可だけを要求するようにします。

各レベルのアクセス許可で使用可能になる JavaScript API のサブセットを次の表に示します。



|**アクセス許可**|**使用可能な API のサブセット**|
|:-----|:-----|
|**Restricted**|[Settings](http://msdn.microsoft.com/ja-jp/library/ad733387-a58c-4514-8fc2-53e64fad468d%28Office.15%29.aspx) オブジェクトのメソッドと [Document.getActiveViewAsync](http://msdn.microsoft.com/ja-jp/library/6b53c90a-df57-4851-98d1-fae2b54f6ad6%28Office.15%29.aspx) メソッド。これは、コンテンツ アドインまたは作業ウィンドウ アドインで要求することができる、最小のアクセス許可レベルです。|
|**ReadDocument**|**Restricted** アクセス許可によって使用可能となる API に加えて、ドキュメントの読み取りとバインディングの管理に必要な API メンバーへのアクセス権を追加します。これには以下の使用が含まれます。
<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>選択されたテキスト、HTML (Word のみ)、または表形式のデータは取得するが、ドキュメント内のすべてのデータを含んでいる基礎となる Open Office XML (OOXML) コードは取得しない、<a href="http://msdn.microsoft.com/ja-jp/library/f85ad02c-64f0-4b73-87f6-7f521b3afd69(Office.15).aspx" target="_blank">Document.getSelectedDataAsync</a> メソッド。</p></li><li><p>ドキュメント内のすべてのテキストを取得するが、基礎となるドキュメントの OOXML バイナリ コピーは取得しない、<a href="http://msdn.microsoft.com/ja-jp/library/78047418-89c4-4c7d-9427-4735b8559518(Office.15).aspx" target="_blank">Document.getFileAsync</a> メソッド。</p></li><li><p>ドキュメント内のバインドされたデータを読み取るための <a href="http://msdn.microsoft.com/ja-jp/library/5372ffd8-579d-4fcb-9e5b-e9a2128f3201(Office.15).aspx" target="_blank">Binding.getDataAsync</a> メソッド。</p></li><li><p>ドキュメントでバインディングを作成するための <span class="keyword">Bindings</span> オブジェクトの <a href="http://msdn.microsoft.com/ja-jp/library/afbadac7-60c7-47cb-9477-6e9466ded44c(Office.15).aspx" target="_blank">addFromNamedItemAsync</a>、<a href="http://msdn.microsoft.com/ja-jp/library/9dc03608-b08b-4700-8be1-3c86ae236799(Office.15).aspx" target="_blank">addFromPromptAsync</a>、<a href="http://msdn.microsoft.com/ja-jp/library/edc99214-e63e-43f2-9392-97ead42fc155(Office.15).aspx" target="_blank">addFromSelectionAsync</a> の各メソッド。</p></li><li><p>ドキュメントでバインディングにアクセスしてそれを削除するための <span class="keyword">Bindings</span> オブジェクトの <a href="http://msdn.microsoft.com/ja-jp/library/ef902b73-cc4c-4551-95de-d8a51eeba82f(Office.15).aspx" target="_blank">getAllAsync</a>、<a href="http://msdn.microsoft.com/ja-jp/library/2727c891-bc05-465c-9324-113fbfeb3fbb(Office.15).aspx" target="_blank">getByIdAsync</a>、および <a href="http://msdn.microsoft.com/ja-jp/library/ad285984-8b44-435d-9b84-f0ade570c896(Office.15).aspx" target="_blank">releaseByIdAsync</a> の各メソッド。</p></li><li><p>ドキュメントの URL など、ドキュメント ファイルのプロパティにアクセスするための <a href="http://msdn.microsoft.com/ja-jp/library/2533a563-95ae-4d52-b2d5-a6783e4ef5b4(Office.15).aspx" target="_blank">Document.getFilePropertiesAsync</a> メソッド。</p></li><li><p>ドキュメント内で名前付きオブジェクトや場所に移動するための <a href="http://msdn.microsoft.com/ja-jp/library/35dda81c-235e-4eab-8a77-9acb3b73a380(Office.15).aspx" target="_blank">Document.goToByIdAsync</a> メソッド。</p></li><li><p>Project 用の作業ウィンドウ アドインについては、<a href="http://msdn.microsoft.com/ja-jp/library/1908af4f-93b9-4859-87e3-06942014fae1(Office.15).aspx" target="_blank">ProjectDocument</a> オブジェクトのすべての "get" メソッド。 </p></li></ul>|
|**ReadAllDocument**|**Restricted** および **ReadDocument** アクセス許可によって使用可能になる API に加えて、ドキュメント データに対する以下の追加のアクセスも許可されます。
<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><span class="keyword">Document.getSelectedDataAsync</span> メソッドおよび <span class="keyword">Document.getFileAsync</span> メソッドは、ドキュメント (テキストだけでなく、書式設定、リンク、埋め込まれたグラフィックス、コメント、リビジョンなど) の基礎となる OOXML コードにアクセスできます。</p></li></ul>|
|**WriteDocument**|**Restricted** アクセス許可によって使用可能になる API に加えて、以下の API メンバーに対するアクセス権も追加されます。
<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>ドキュメントでのユーザーの選択内容に書き込むための <a href="http://msdn.microsoft.com/ja-jp/library/998f38dc-83bd-4659-a759-4758c632a6ef(Office.15).aspx" target="_blank">Document.setSelectedDataAsync</a> メソッド。</p></li></ul>|
|**ReadWriteDocument**|**Restricted**、 **ReadDocument**、 **ReadAllDocument**、および  **WriteDocument** アクセス許可によって使用可能になる API に加えて、イベントを購読するメソッドなど、コンテンツ アドインと作業ウィンドウ アドインによってサポートされる他のすべての API へのアクセスを含みます。これらの追加の API メンバーにアクセスするには  **ReadWriteDocument** アクセス許可を宣言する必要があります。
<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>ドキュメントのバインドされている領域に書き込むための <a href="http://msdn.microsoft.com/ja-jp/library/6a59bb6d-40b6-4a95-9b98-d70d4616de09(Office.15).aspx" target="_blank">Binding.setDataAsync</a> メソッド。</p></li><li><p>バインド テーブルに行を追加するための <a href="http://msdn.microsoft.com/ja-jp/library/1cd23454-8435-4e13-98b3-d0d29ed278a8(Office.15).aspx" target="_blank">TableBinding.addRowsAsync</a> メソッド。</p></li><li><p>バインド テーブルに列を追加するための <a href="http://msdn.microsoft.com/ja-jp/library/8f1bfa81-3850-4ea1-ba2e-c9bcf5847a44(Office.15).aspx" target="_blank">TableBinding.addColumnsAsync</a> メソッド。</p></li><li><p>バインド テーブルからすべてのデータを削除するための <a href="http://msdn.microsoft.com/ja-jp/library/8f5cc783-384d-4520-a218-190dfed74dd2(Office.15).aspx" target="_blank">TableBinding.deleteAllDataValuesAsync</a> メソッド。</p></li><li><p>バインド テーブルに書式設定とオプションを設定するための <span class="keyword">TableBinding</span> オブジェクトの <a href="http://msdn.microsoft.com/ja-jp/library/49712906-f582-4055-9ef8-6edde6e97679(Office.15).aspx" target="_blank">setFormatsAsync</a>、<a href="http://msdn.microsoft.com/ja-jp/library/cc56e9c0-b33c-4d9b-b676-a7e50f757c10(Office.15).aspx" target="_blank">clearFormatsAsync</a>、および <a href="http://msdn.microsoft.com/ja-jp/library/2885fc57-4527-4ca4-a43d-9ee447ec27d3(Office.15).aspx" target="_blank">setTableOptionsAsync</a> の各メソッド。</p></li><li><p><a href="http://msdn.microsoft.com/ja-jp/library/dc1518de-47fa-4108-aab7-04a022724b04(Office.15).aspx" target="_blank">CustomXmlNode</a>、<a href="http://msdn.microsoft.com/ja-jp/library/83f0e668-8236-4f2f-a20f-b173a9e3f65f(Office.15).aspx" target="_blank">CustomXmlPart</a>、<a href="http://msdn.microsoft.com/ja-jp/library/ba40cd4c-29bb-4f31-875d-6f1382fd1ee8(Office.15).aspx" target="_blank">CustomXmlParts</a>、および <a href="http://msdn.microsoft.com/ja-jp/library/18b9aa8c-83e7-4c2f-8530-6a0ac8ce5535(Office.15).aspx" target="_blank">CustomXmlPrefixMappings</a> の各オブジェクトのすべてのメンバー。</p></li><li><p>コンテンツ アドインと作業ウィンドウ アドインによってサポートされるイベントにサブスクライブするためのすべてのメソッド、特に <a href="http://msdn.microsoft.com/ja-jp/library/1908af4f-93b9-4859-87e3-06942014fae1(Office.15).aspx" target="_blank">Binding</a>、<a href="http://msdn.microsoft.com/ja-jp/library/ad733387-a58c-4514-8fc2-53e64fad468d(Office.15).aspx" target="_blank">CustomXmlPart</a>、<a href="http://msdn.microsoft.com/ja-jp/library/42882642-d22b-47d2-a8d3-3aa8c6a4435e(Office.15).aspx" target="_blank">Document</a>、<a href="http://msdn.microsoft.com/ja-jp/library/83f0e668-8236-4f2f-a20f-b173a9e3f65f(Office.15).aspx" target="_blank">ProjectDocument</a>、および <a href="http://msdn.microsoft.com/ja-jp/library/f8859516-cc1f-4b20-a8f3-cee37a983e70(Office.15).aspx" target="_blank">Settings</a> の各オブジェクトの <span class="keyword">addHandlerAsync</span> メソッドおよび <span class="keyword">removeHandlerAsync</span> メソッド。</p></li></ul>|

## その他の技術情報



- [作業ウィンドウ アドインとコンテンツ アドインの概要](task-pane-and-content-add-ins.md)
    
- [Office アドインのプライバシーとセキュリティ](87c59a88-10e2-4c88-b6a8-736bd356e5f8.md)
    


