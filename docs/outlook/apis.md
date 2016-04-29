
# Outlook アドインの API
Outlook アドインに必要な API を指定する方法について説明します。

 _ **適用対象:** apps for Office?| Office Add-ins?| Outlook_

Outlook アドインで API を使用するには、Office.js ライブラリの場所、要件セット、スキーマ、アクセス許可を指定する必要があります。

## Office.js

Outlook アドイン API を操作するには、開発者は Office.js 内で JavaScript API を使用する必要があります。ライブラリの CDN は  _https://appsforoffice.microsoft.com/lib/1/hosted/Office.js_ です。ストアに送信されたアドインは、この CDN を使用して Office.js を参照する必要があります。ローカル参照を使用することはできません。アドインの UI を実装する Web ページ (.html, .aspx, .php ファイル) の **[<head>]** タグの、 **[script]** タグの **[src]** 属性で、CDN を宣言します。


```HTML
<script src="https://appsforoffice.microsoft.com/lib/1/hosted/Office.js" type="text/javascript"></script>
```

新しい API が追加されても、Office.js への URL は同じままになります。URL 内のバージョンは、既存の API の動作を分割する場合にのみ変更されます。

ライブラリはアドインが起動してから 5 秒以内に読み込まれる必要があります。そうしないと、Outlook はページが応答しないと判断し、エラー ダイアログ ボックスを表示します。


## 要件セット

すべての Outlook API は、メールボックス要件セットに属しています。メールボックス要件セットには複数のバージョンがあり、リリースされる API の新しいセットはそれぞれのセットの上位バージョンに属します。すべての Outlook クライアントが、リリースされる最新の API のセットをサポートするわけではありませんが、Outlook クライアントが要件セットのサポートを宣言する場合は、その要件セットのすべての API がサポートされます。 

マニフェスト内で要件セットの最小バージョンを指定すると、どの Outlook クライアントでアドインが表示されるかが制御されます。たとえば、要件セットのバージョン 1.3 が指定されている場合、v1.3 以上をサポートしていない Outlook クライアントではアドインが表示されません。ただし、要件セットの指定によって、アドインがそのバージョンの API に制限されるわけではありません。アドインが要件セットの v1.1 を指定し、v1.3 をサポートしている Outlook クライアントで実行されている場合、アドインはこれらの新しい API を引き続き使用できます。要件セットは、どの Outlook クライアントでアドインが表示されるかだけを制御します。

マニフェストで指定した要件セットよりも大きい要件セットの API の可用性を確認するには、JavaScript の標準的な手法を使用できます。




```
if (item.somePropertyOrFunction) {
   item.somePropertyOrFunction…  
}

```

このようなチェックは、マニフェストで指定された要件セットバージョンに存在する API には必要ありません。

開発者は、自分のシナリオで重要な API セットをサポートする最小要件セットを指定する必要があります。指定しないと、アドインの重要な機能が動きません。要件セットを、 **Requirements** 、 **Sets** 、 **Set** 要素のマニフェストに指定する必要があります。詳細については、 [Outlook アドインのマニフェスト](../outlook/manifests/manifests.md)を参照してください。

 **Methods** 要素は メール アドインには適用されないので、特定のメソッドのサポートは宣言できません。


## アクセス許可

アドインには、必要な API を使用するための適切なアクセス許可が必要です。アクセス許可には、以下に要約されている 4 つのレベルがあります。詳細については、 [ユーザーのメールボックスにアクセスする Outlook アドインのためのアクセス許可を指定する](../../docs/outlook/privacy/understanding-outlook-add-in-permissions.md)を参照してください。.


|
|
|**アクセス許可レベル**|**説明**|
|:-----|:-----|
|Restricted|エンティティは使用できますが、正規表現は使用できません。|
|アイテムの読み取り| _Restricted_ で許可されているものに加えて、以下のものが許可されます。
<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>正規表現</p></li><li><p>Outlook アドイン API の読み取りアクセス</p></li><li><p>アイテムのプロパティおよびコールバック トークンの取得</p></li></ul>|
|読み取り/書き込み| _アイテムの読み取り_ で許可されているものに加えて、以下のものが許可されます。
<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><span class="keyword">makeEwsRequestAsync</span> を除くすべての Outlook アドイン API アクセス</p></li><li><p> アイテムのプロパティの設定</p></li></ul>|
|メールボックスの読み取り/書き込み| _読み取り/書き込み_ で許可されているものに加えて、以下のものが許可されます。
<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>アイテムおよびフォルダーの読み取り、書き込み、作成</p></li><li><p>アイテムの送信</p></li><li><p><a href="http://dev.outlook.com/reference/add-ins/Office.context.mailbox.html.aspx#makeEwsRequestAsync" target="_blank">makeEwsRequestAsync</a> の呼び出し</p></li></ul>|
通常は、アドインで必要な最小限のアクセス許可を指定する必要があります。アクセス許可は、マニフェスト内の  **Permissions** 要素で宣言されます。詳細については、 [Outlook アドインのマニフェスト](../outlook/manifests/manifests.md) を参照してください。セキュリティ上の問題については、 [Outlook アドインに関するプライバシー、アクセス許可、セキュリティ](../../docs/develop/privacy-and-security.md) を参照してください。


## その他の技術情報



- [Get Started with Outlook add-ins for Office 365](https://dev.outlook.com/MailAppsGettingStarted/GetStarted.aspx)
    
- [Outlook Add-in API](http://dev.outlook.com/reference/add-ins/index.mdl.aspx)
    
- [Outlook アドインのマニフェスト](../outlook/manifests/manifests.md)
    
- [Outlook アドインに関するプライバシー、アクセス許可、セキュリティ](../../docs/develop/privacy-and-security.md)
    
