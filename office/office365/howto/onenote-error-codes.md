---
ms.Toctitle: Error and warning codes
title: "OneNote API のエラー コードと警告コード"
description: "onclong API から返されるエラー コードと警告コードについて説明します。"
ms.ContentId: 27d541f5-b4b0-4ca6-9ad4-be3a62baa32e
ms.date: July 5, 2016

---
[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]
[!INCLUDE [Add the ONAPI repo styles](../includes/controls/addonapistyles.xml)]


# OneNote API のエラー コードと警告コード 

*__Applies to:__ Consumer notebooks on OneDrive | Enterprise notebooks on Office 365*

この記事では、API を通して送信した要求が失敗した場合に OneNote API から返されるエラー コードと警告コードについて説明します。

<a name="responses"></a>
## エラーおよび警告の応答
送信した要求でエラーが発生すると、OneNote API はその要求の実行を停止し、エラー応答を JSON オブジェクトとして返します。エラー応答には、関連付けられているエラー コード、メッセージ、およびこの記事の該当するセクションへのリンクが含まれます。次の例は、エラー応答の様子を示しています。

```json
{
   "error":{
      "code":"10002",
      "message":"The service is currently unavailable. Please try again later.",
      "@api.url":"http://go.microsoft.com/fwlink/?LinkID=400805"
   }
}
```
 
要求が警告を生成した場合、OneNote API はその要求を実行し、応答の中に警告を組み込みます。1 つの応答に 2 つ以上の警告が含まれることがあるため、API は JSON オブジェクトの配列として警告を返します。それぞれの警告には、メッセージと、この記事の該当するセクションへのリンクが含まれます。次の例は、要求が完了したコンテキストでの警告応答の様子を示しています。

```
{
   "@odata.context":"https://www.onenote.com/api/v1.0/$metadata#me/notes/notebooks",
   "api.diagnostics@odata.type":"#Collection(Microsoft.OneNote.Api.Diagnostic)",
   "@api.diagnostics":[
      {
         "message":"Created date/time string 5/5/2014 in 'Presentation' part html did not match any of the allowed formats",
         "url":"http://go.microsoft.com/fwlink/?LinkID=400816"
      }
   ],
   "value":[
      {
         "isDefault":false,
         "createdBy":null,
         "modifiedTime":"0001-01-01T00:00:00Z",
         "userRole":"Owner",
         "id":"55C9F7CBFC6AC1!76345",
         "name":"Notebook1",
         "link":null
      },
      {
         "isDefault":true,
         "createdBy":null,
         "modifiedTime":"0001-01-01T00:00:00Z",
         "userRole":"Owner",
         "id":"55C9F7CBFC6AC1!76428",
         "name":"Notebook2",
         "link":null
      },
      {
         "isDefault":false,
         "createdBy":null,
         "modifiedTime":"0001-01-01T00:00:00Z",
         "userRole":"Owner",
         "id":"55C9F7CBFC6AC1!77509",
         "name":"Notebook3",
         "link":null
      }
   ]
}
```

1 つの要求から、1 つのエラーおよび 1 つ以上の警告が生成されることがあります。そのような場合、OneNote API は要求の実行を停止し、エラーと警告の両方を返します。次の例は、警告を含むエラーの様子を示しています。

```
{
   "error":{
      "code":"10002",
      "message":"The service is currently unavailable. Please try again later.",
      "@api.url":"http://go.microsoft.com/fwlink/?LinkID=400805",
      "api.diagnostics@odata.type":"#Collection(Microsoft.OneNote.Api.Diagnostic)",
      "@api.diagnostics":[
         {
            "message":"Created date/time string 5/5/2014 in 'Presentation' part html did not match any of the allowed formats",
            "url":"http://go.microsoft.com/fwlink/?LinkID=400816"
         }
      ]
   }
}
```

問題を解決するために Microsoft サポートとの作業が必要な場合、必ず [X CorrelationId ヘッダー](#x-correlationid) と API 呼び出しのタイムスタンプも記録するようにしてください。

<a name="C10001-19999"></a>
## 10001 から 19999 のコード
サービスで問題が発生したか、サービスからアプリケーションに情報が送られました。

<a name="C10001"></a>
### 10001
予期しないエラーが発生し、要求が失敗しました。

<a name="C10002"></a>
### 10002
サービスは現在利用できません。

<a name="C10003"></a>
### 10003
現在のユーザーのアカウントで、アクティブな要求の最大数を超えました。アプリは要求を繰り返す必要があります。 Your app will have to repeat the request.

<a name="C10004"></a>
### 10004
サービスは要求されたセクション内にページを作成できません。そのセクションはパスワードで保護されています。

<a name="C10005"></a>
### 10005
要求内で、**data-render-src** 属性に PDF が含まれる イメージ タグが最大数を超えました。詳細については、「[イメージおよびファイルの追加](../howto/onenote-images-files.md)」を参照してください。

<a name="C10006"></a>
### 10006
OneNote API は、指定されたセクション内にページを作成できませんでした。そのセクションは破損しています。

<a name="C10007"></a>
### 10007
現在、サーバーがビジー状態であるため、受信した要求を処理できません。後でもう一度やり直してください。

<a name="C10008"></a>
### 10008
One or more of the document libraries on the user or group's OneDrive contains more than 5000 OneNote items (notebooks, sections, section groups), and cannot be queried using the API. Please make sure that none of the user or group's document libraries contains more than 5000 OneNote items.

<a name="C10012"></a>
### 10012
Unable to create or update the entity because the library that contains the notebook requires items to be checked out before they can be edited. For more information, see https://support.office.com/en-us/article/Configure-a-site-library-to-require-check-out-of-files-f63fcbdc-1db6-4eb7-a3eb-dd815500c9e7.

Either remove the check-out requirement from the library, or move the notebook.

<a name="C19999"></a>
### 19999
未定義のエラーが発生したため、要求は失敗しました。


<a name="C20001-29999"></a>
## 20001 から 29999 のコード
アプリケーション コードが間違った処理を実行しました。

<a name="C20001"></a>
### 20001
必須の "Presentation" パートが要求に含まれていません。このパートは 1 つだけ含まれている必要があります。「OneNote API リファレンス」[ref] を参照してください。 Exactly one is required. See [OneNote API reference][ref].

<a name="C20002"></a>
### 20002
要求に "Presentation" パートが 2 つ以上含まれています。このパートは 1 つだけ含まれている必要があります。「OneNote API リファレンス」[ref] を参照してください。 Exactly one is required. See [OneNote API reference][ref].

<a name="C20003"></a>
### 20003
"Presentation" パートのコンテンツ タイプは、text/html または application/xhtml+xml のいずれかでなければなりません。「OneNote API リファレンス」[ref] を参照してください。 See [OneNote API reference][ref].

<a name="C20004"></a>
### 20004
"Presentation" パートの HTML に含まれるイメージ タグに、**src** プロパティと **data-render-src** プロパティの両方が設定されています。API は src プロパティを無視して data-render-src プロパティを使用します。「OneNote API リファレンス」[ref] を参照してください。 The API will ignore the **src** property and use the **data-render-src** property. See [OneNote API reference][ref].

<a name="C20005"></a>
### 20005
The request URI is too long. 要求 URI が長すぎます。URI の最大サイズ (すべてのパラメーターとデータを含む) は 16 KB (16,384 文字) です。

<a name="C20006"></a>
### 20006
"Presentation" パートの HTML に含まれるイメージ タグに、src プロパティと **data-render-src** プロパティがどちらも設定されていません。API はこの image タグを無視します。「OneNote API リファレンス」[ref]を参照してください。 The API will ignore the **image** tag. See [OneNote API reference][ref].

<a name="C20007"></a>
### 20007
"Presentation" パートの HTML に含まれる作成日時の文字列が、許可される形式のどれとも一致しません。「OneNote API リファレンス」[ref] をご覧ください。 See [OneNote API reference][ref].

<a name="C20008"></a>
### 20008
要求のサイズが大きすぎます。[OneNote API リファレンス][ref] を参照してください。 See [OneNote API reference][ref].

<a name="C20009"></a>
### 20009
The request contains parts with duplicate names. 参照テーブル名は一意である必要があります。 See [OneNote API reference][ref] for more information on how to construct multipart requests.

<a name="C20010"></a>
### 20010
指定されたコンテンツ タイプに対して Content-Disposition ヘッダーが指定されていません。マルチパートの要求を作成する方法の詳細については、「OneNote API リファレンス」[ref] を参照してください。 See [OneNote API reference][ref] for more information on how to construct multipart requests.

<a name="C20011"></a>
### 20011
The request contains a malformed multipart payload. Problems could include missing blank lines, a missing last line, incorrectly formatted part separators, and so on. If you're building the multipart message by hand, carefully check the logic, or consider using a third-party library. See [OneNote API reference][ref] for more information on how to construct a multipart payload.

<a name="C20012"></a>
### 20012
要求の中で、指定したパートのコンテンツ タイプが指定されていません。パートを作成する方法の詳細については、「OneNote API リファレンス」[ref] を参照してください。 See [OneNote API reference][ref] for more information on how to construct a part.

<a name="C20013"></a>
### 20013
要求の中で、指定したパートの Content-Type ヘッダーおよび Content-Disposition ヘッダーが提供されていません。 

<a name="C20014"></a>
### 20014
マルチパート メッセージのパートの長さが最大サイズである 25 MB を超えています。「OneNote API リファレンス」[ref] を参照してください。 See [OneNote API reference][ref].

<a name="C20015"></a>
### 20015
マルチパート メッセージのパートの数が上限の 30 を超えています。「OneNote API リファレンス」[ref] を参照してください。 See [OneNote API reference][ref].

<a name="C20016"></a>
### 20016
マルチパート メッセージのパートの長さが上限の 70 MB を超えています。「OneNote API リファレンス」[ref] を参照してください。 See [OneNote API reference][ref].

<a name="C20100"></a>
### 20100
Something is wrong with the syntax of your request. 要求の構文に間違いがあります。詳細について「 OneNote API リファレンス http://dev.onenote.com/docs 」を参照し、要求が正しく作成されていることを確認してください。

<a name="C20101"></a>
### 20101
要求したプロパティは存在しません。

<a name="C20102"></a>
### 20102
存在しないリソースが要求されました。

<a name="C20103"></a>
### 20103
**expand** クエリはこの要求ではサポートされません。「サポートされる OData のクエリ文字列オプション」を参照してください。 サポートされている OData クエリ文字列オプション

<a name="C20104"></a>
### 20104
**pagelevel** クエリ オプションは、セクションにあるページ コレクションまたは特定のページに対してクエリを行うときだけサポートされます。例: 次に例を示します。  

    GET ../sections/{id}/pages?pagelevel=true

    GET ../pages/{id}?pagelevel=true

<a name="C20106"></a>
### 20106
要求に、サポートされていないクエリ演算子が含まれています。「OneNote API リファレンス」[ref]を参照してください。 See [OneNote API reference][ref].

<a name="C20108"></a>
### 20108
要求に、サポートされていない OData クエリ パラメーターが含まれています。

<a name="C20109"></a>
### 20109
PATCH 要求内のペイロードの構成が正しくありません。

<a name="C20110"></a>
### 20110
データ パートを伴うページ作成要求には、マルチパートにするコンテンツ、すなわち "Presentation" パートが必要です。「OneNote API リファレンス」[ref] を参照してください。 See [OneNote API reference][ref].

<a name="C20111"></a>
### 20111
要求で使用されている OData 機能はサポートされていません。

<a name="C20112"></a>
### 20112
ターゲット ノートブック、セクション グループ、セクション、またはページ エンティティの無効な ID が要求に含まれています。

<a name="C20113"></a>
### 20113
要求で指定されているリソースは削除されました。

<a name="C20115"></a>
### 20115
The name contains invalid characters. 名前の値に無効な文字が含まれています。ノートブック名には、次の文字を含めることができません: ?*\/:<>|'"

<a name="C20117"></a>
### 20117
指定された名前のアイテムは、指定された場所に既に存在します。

<a name="C20119"></a>
### 20119
"Presentation" パートの HTML に含まれる <e>data-attachment</e> 属性の形式が無効であるか、またはファイル名には無効な文字 (<c>\ / : * ? < > <l/> "</c>) が 1 つ以上含まれています。要求では、エラー メッセージに示されている値に置き換えられました。 The request substituted the value indicated in the error message.

<a name="C20120"></a>
### 20120
要求で指定された PATCH ターゲットが見つかりません。

<a name="C20121"></a>
### 20121
要求に含まれている PATCH 引数が無効です。「ページ コンテンツの更新」を参照してください。 See [Update page content](../howto/onenote-update-page.md).

<a name="C20122"></a>
### 20122
要求で指定されている PATCH アクションはサポートされていません。「ページ コンテンツの更新」を参照してください。 See [Update page content](../howto/onenote-update-page.md).

<a name="C20123"></a>
### 20123
PATCH 要求は、指定されたページを変更できません。

<a name="C20124"></a>
### 20124
マルチパートの PATCH 要求に、PATCH アクションの JSON 構造を指定した "commands" パートが含まれていません。「ページ コンテンツの更新」を参照してください。 See [Update page content](../howto/onenote-update-page.md).

<a name="C20125"></a>
### 20125
PATCH 要求にアクションが含まれていません。「ページ コンテンツの更新」を参照してください。 See [Update page content](../howto/onenote-update-page.md).

<a name="C20126"></a>
### 20126
メッセージ本文に、不正な書式設定の JSON またはこの操作に対してはサポートされていないフィールドが含まれています。

<a name="C20127"></a>
### 20127
要求で指定された名前は不明なプロパティです。

<a name="C20128"></a>
### 20128
要求に含まれている OData で、メッセージに示されている位置に構文エラーがあります。

<a name="C20129"></a>
### 20129
要求に、値が大きすぎる**上位**のクエリ文字列オプションが含まれます。ページのクエリの場合、最大値は 100 で、既定値は 20 です。

<a name="C20130"></a>
### 20130
要求に含まれている URI が指している HTTP リソースが見つかりません。

<a name="C20131"></a>
### 20131
要求に含まれている Content-Type の値が無効です。メッセージに示されている値を使用してください。「OneNote API リファレンス」[ref]を参照してください。 Use the value indicated in the message. See [OneNote API reference][ref].

<a name="C20132"></a>
### 20132
要求に含まれている PATCH 引数が無効です。「ページ コンテンツの更新」を参照してください。 要求に含まれているコンテンツが無効です。このエラーのよくある原因は、Content-Type 要求ヘッダーの欠落や、要求の本文にコンテンツが含まれていないことです。「OneNote API リファレンス」[ref] を参照してください。 See [OneNote API reference][ref].

<a name="C20133"></a>
### 20133
要求にはサポートされていない PATCH ターゲットが指定されています。「ページ コンテンツの更新」を参照してください。 See [Update page content](../howto/onenote-update-page.md).

<a name="C20134"></a>
### 20134
要求に、PATCH アクションのターゲットとして無効な要素が指定されています。ターゲットで **data-id** 識別子が使用されている場合は、プレフィックスとして # 記号が付いていることを確認してください。「[ページ コンテンツの更新](../howto/onenote-update-page.md)」を参照してください。

<a name="C20135"></a>
### 20135
要求に指定されたエンティティの種類は、PATCH 操作に対してはサポートされていません。 See [Update page content](../howto/onenote-update-page.md).

<a name="C20136"></a>
### 20136
要求に無効または不足している **data-render-src** または **data-render-method** 属性が含まれます。「キャプチャからのデータの抽出」を参照してください。 キャプチャからデータを抽出する

<a name="C20137"></a>
### 20137
このターゲット ページにおいて、PATCH 要求はサポートされていません。

<a name="C20138"></a>
### 20138
PATCH 要求のターゲット要素の種類では、**append 操作はサポートされていません。「ページ コンテンツの更新**」を参照してください。 See [Update page content](../howto/onenote-update-page.md).

<a name="C20139"></a>
### 20139
要求に、無効な **data-tag** 属性値が含まれています。 「[ノート シールを使用する](../howto/onenote-note-tags.md)」を参照してください。

<a name="C20140"></a>
### 20140
要求に、無効な **data-tag** ステータス値が含まれています。チェックボックスの note タグでは、1 つの completed ステータスが可能です。例: 要求に、無効な **data-tag** ステータス値が含まれています。チェックボックスの note タグでは、1 つの completed ステータスが可能です。例: 例:

    <p data-tag="to-do:completed">To-do note tag in completed state (checked box in the UI)</p>
 
「[ノート シールを使用する](../howto/onenote-note-tags.md)」を参照してください。

<a name="C20141"></a>
### 20141
PATCH 要求のターゲット要素の種類では、指定の操作はサポートされていません。「ページ コンテンツの更新」を参照してください。 See [Update page content](../howto/onenote-update-page.md).

<a name="C20142"></a>
### 20142
要求に、子エンティティの親、または親エンティティの子に対する **expand** 式が含まれていますが、それはサポートされていません。「[サポートされる OData のクエリ文字列オプション](../howto/onenote-get-content.md##query-options)」をご覧ください。

<a name="C20143"></a>
### 20143
OData クエリが無効です。

<a name="C20144"></a>
### 20144
要求に、ナビゲートでないプロパティに対する **expand** 式が含まれています。展開できるのは、ナビゲート プロパティだけです。 Only navigation properties can be expanded.

<a name="C20145"></a>
### 20145
要求の **select** 式または **expand** 式に、無効な項が含まれています。

<a name="C20146"></a>
### 20146
要素で **style="position:absolute"`style="position:absolute"` 属性が指定されていますが、この body** 要素には位置指定をサポートするために必要な data-absolute-enabled="true"`data-absolute-enabled="true"` が指定されていません。すべての位置設定は無視されます。 All position settings will be ignored. 絶対的な位置要素を作成する

<a name="C20147"></a>
### 20147
The `style="position:absolute"` attribute is specified on an element that is not a direct child of the **body** element, which is not supported. 20147**style="position:absolute"** 属性が、**body** 要素の直接の子要素でない要素に指定されています。これはサポートされていません。要素が **div**、img、または object の場合は、本文の直接の子にしてください。そうしないと、位置設定が無視され、絶対位置で配置された div の内側にコンテンツが表示されます。 絶対的な位置要素を作成する

<a name="C20148"></a>
### 20148
20148style="position:absolute"`style="position:absolute"` 属性が、サポートしていない要素の型に指定されています。位置指定をサポートしているのは、div、img、および object 要素のみです。 **style="position:absolute"** 属性が、それをサポートしていない要素の種類に対して指定されています。配置がサポートされているのは、ページ本文の直接の子である **div**、**img**、および object 要素のみです。 絶対的な位置要素を作成する

<a name="C20149"></a>
### 20149
要求が指定しているターゲット要素が見つかりません。

<a name="C20150"></a>
### 20150
この認証の種類には、この要求は無効です。代わりに、../me/notes/ パスを使用してください。 Use the `../me/notes/` path instead.

<a name="C20151"></a>
### 20151
この認証の種類には、この要求は無効です。代わりに、../me/notes/ パスを使用してください。 この認証の種類には、この要求は無効です。../me/notes/section/{id}/pages`../me/notes/section/{id}/pages` エンドポイントを使用して、指定のセクションでページを作成してください。

<a name="C20152"></a>
### 20152
エンティティに name 値が指定されていません。名前を定義する必要があり、その場合、名前が空白だけであってはなりません。 エンティティに name 値が指定されていません。名前を定義する必要があり、その場合、名前には空白を含めることはできません。

<a name="C20153"></a>
### 20153
The entity name contains invalid characters. The name cannot contain the following characters: `? * \ / : < > | & # " % ~`

<a name="C20154"></a>
### 20154
エンティティ名を空白で始めることはできません。

<a name="C20155"></a>
### 20155
名前が長すぎます。 Notebook names have a 128-character limit. Other entity names have a 50-character limit.

<a name="C20156"></a>
### 20156
宛先リソースの指定された ID が存在しません。

<a name="C20157"></a>
### 20157
宛先エンティティの指定された ID が無効です。

<a name="C20158"></a>
### 20158
Unable to get metadata for the site URL specified in the request. Check the format of the supplied URL. Supported formats include `https://domain.sharepoint.com/site-a` and `https://domain.com/sites/site-a`.

<a name="C20160"></a>
### 20160
指定した ID を持つ Office 365 統合グループを見つけることができません。

<a name="C20161"></a>
### 20161
コンテキストで、有効なユーザー ID が指定されていません。一般的なエラーの 1 つとして、PUID/CID が 16 進数ではなく long 型として渡されたことが挙げられます。

<a name="C20166"></a>
### 20166
アプリケーションは、短時間に非常に多くの要求をユーザーの代理で発行しました。OneNote API の安定性と応答性を保つために、アプリケーションがリソースを消費しすぎることが検出されると、API は 429 ステータス コードとこのエラーを返します。詳細については、「[OneNote API の調整とその回避方法](http://blogs.msdn.com/b/onenotedev/archive/2016/01/13/onenote-api-throttling-and-best-practices.aspx)」を参照してください。

<a name="C20168"></a>
### 20168
The video source specified in the request is not supported. See [Supported video sites](../howto/onenote-images-files.md#videos) for the current list.


<a name="C30001-39999"></a>
## 30001 から 39999 のコード
ユーザーのアカウントに問題があります。

<a name="C30101"></a>
### 30101
ユーザー アカウントは、その OneDrive のクォータを超えています。「OneDrive」を参照してください。 See [OneDrive](https://onedrive.live.com/about/en-us/).

<a name="C30102"></a>
### 30102
要求されたセクションは、最大サイズに達したため、これ以上追加できません。

<a name="C30103"></a>
### 30103
要求に対するリソース消費量が多すぎます。対象のユーザー アカウントに大きいデータセットが含まれているか、サービスが同じサイト (ユーザーの個人用サイトやチーム サイトなど) に対する多数の要求を同時に受信しています。

<a name="C30104"></a>
### 30104
ユーザー アカウントが中断されています。

<a name="C30105"></a>
### 30105
ノートブックにアクセスする必要がある、ユーザーの個人用 OneDrive for Business サイトがプロビジョニングされていません。OneNote サービスは今すぐサイトをプロビジョニングします。この処理には数分かかることがあります。

<a name="C30106"></a>
### 30106
OneDrive for Business は、ユーザーのためにプロビジョニング中です。

<a name="C30107"></a>
### 30108
ユーザーの個人用の OneDrive for Business を取得できませんでした。次にいくつかの原因を示します。

| 原因 | 解決策 |
|:------|:------|
| ユーザーの個人用サイトがプロビジョニングされていません。 | ユーザーは、OneDrive for Business を開き、サイトをプロビジョニングするためのすべての指示に従います。これが失敗した場合は、Office 365 テナント管理者に問い合わせてください。 |
| ユーザーの個人用サイトは現在プロビジョニング中です。 | 後で要求を再試行してください。 |
| ユーザーは、有効な OneDrive for Business ライセンスを持っていません。 | Office 365 テナント管理者に問い合わせてください。 |
| ネットワークの問題により、要求が正常に送信されていません。 | 後で要求を再試行してください。 |


<a name="C40001-49999"></a>
## 40001 から 49999 のコード
ユーザーまたはアプリケーションに、適切なアクセス許可がありません。

<a name="C40001"></a>
### 40001
要求に、有効な OAuth トークンが含まれていません。「OneNote の認証およびアクセス許可」を参照してください。 OneNote の認証とアクセス許可

<a name="C40002"></a>
### 40002
ユーザーは、要求された場所に書き込むアクセス許可を持っていません。

<a name="C40003"></a>
### 40003
ユーザーは、要求されたリソースにアクセスする許可を持っていません。

<a name="C40004"></a>
### 40004
OAuth トークンには、要求されたアクションを実行するために必要なスコープが含まれていません。「OneNote の認証およびアクセス許可」を参照してください。 OneNote の認証とアクセス許可

<a name="x-correlationid"></a>
## X-CorrelationId ヘッダー
OneNote API は、標準の HTTP 応答コードのほかに、いくつかのヘッダーを呼び出し元のアプリに返します。すべての応答には、次のような **X-CorrelationId** ヘッダーと **Date** ヘッダーが含まれています。

```
X-CorrelationId: d2d6aaf5-3bde-4ee7-ba18-27727bf3cffe
Date: Fri, 06 Mar 2015 15:10:46 GMT
```

相関 ID は、バックエンド サーバー内のさまざまな部分を結びつける GUID です。相関 ID は連続していないので、この値を使ってページの作成順序を確定することはできません。
 
アプリは、相関 ID と API 呼び出しの日付を記録できます。Microsoft サポートを使用してアプリの問題を解決する場合、または API を使用する場合、これらの値を使用できます。


<a name="see-also"></a>
## その他の技術情報

- [OneNote 開発](../howto/onenote-landing.md)
- [OneNote デベロッパー センター](http://dev.onenote.com/)
- [OneNote の開発者ブログ](http://go.microsoft.com/fwlink/?LinkID=390183)
- [スタック オーバーフローに関する OneNote の開発の質問](http://go.microsoft.com/fwlink/?LinkID=390182) 
- [OneNote GitHub のリポジトリ](http://go.microsoft.com/fwlink/?LinkID=390178)


[#REF!]: http://dev.onenote.com/docs