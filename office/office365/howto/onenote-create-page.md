---
ms.Toctitle: Create pages
title: "OneNote ページの作成"
description: "任意のセクションに OneNote ページを作成して、イメージやファイルなどのコンテンツを含めます。"
ms.ContentId: 0de322cc-570f-4afb-a313-b1d9c3d916f2
ms.date: May 17, 2016

---
[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]
[!INCLUDE [Add the ONAPI repo styles](../includes/controls/addonapistyles.xml)]

# OneNote ページの作成

*__適用対象:__OneDrive のコンシューマー ノートブック | Office 365 のエンタープライズ ノートブック*

OneNote ページを作成する場合は、POST 要求を *pages* エンドポイントに送信します。 たとえば次のようにします。

<p id="indent">`POST ../notes/sections/{id}/pages`</p>

メッセージ本文でページ定義する HTML を送信します。要求が成功すると、OneNote API は 201 HTTP 状態コードを返します。

>セクション、セクション グループ、ノートブックを作成するために送信できる POST 要求について調べるために、[対話型 REST リファレンス](http://dev.onenote.com/docs)をご覧ください。


<a name="request-uri"></a>
## 要求 URI の構築

要求 URI を構築するには、サービス ルート URL から開始します。

[!INCLUDE [service root url](../includes/onenote/service-root-url.xml)]

次に、*pages* エンドポイントを追加します。

<p id="outdent1">**任意のセクション (ID で指定) にページを作成する**</p>
<p id="indent1">`../sections/{id}/pages`</p>

OneDrive または OneDrive for Business のユーザーの個人用ノートブックにページを作成する場合は、OneNote API が提供する、既定のノートブックにページを作成するためのエンドポイントを使用することもできます。

<p id="outdent1">**既定のノートブックの既定のセクションにページを作成する**</p>
<p id="indent1">`../me/notes/pages`</p>

<p id="outdent1">**既定のノートブックのセクション (名前で指定) にページを作成する** (*[ルールを参照](#post-pages-section-name)*)</p>
<p id="indent1">`../me/notes/pages?sectionName=Some%20section%20name`</p>


<br />
完全な要求 URI は、次に示す例のいずれかのようになります。
<p id="indent">`https://www.onenote.com/api/v1.0/me/notes/sections/{id}/pages?sectionName=Homework`</p>
<p id="indent">`https://www.onenote.com/api/v1.0/myorganization/sitecollections/{id}/sites/{id}/notes/sections/{id}/pages`</p>
<p id="indent">`https://www.onenote.com/api/v1.0/myorganization/groups/{id}/notes/sections/{id}/pages`</p>

[!INCLUDE [service root url note](../includes/onenote/service-root-note.xml)]

<a name="post-pages-section-name"></a>
### *sectionName* URL パラメーターの使用

次に示すルールは、*sectionName* パラメーターを使用して、既定のノートブックの指名したセクションにページを作成するときに適用されます。

- 最上位のセクションのみを参照できます (セクション グループ内のセクションは参照できません)。

- 指定した名前のセクションが既定のノートブックに存在しない場合、そのセクションは API によって作成されます。 セクション名に使用できない文字は、「 * \ / : &lt; &gt; | &amp; # " % ~」です。

- セクション名の照合では、大文字と小文字が区別されませんが、セクションが作成されるときには大文字と小文字が維持されます。そのため、"My New Section" はこのとおりに表示されますが、これ以降の POST では "my new section" も一致します。

- セクション名は URL エンコードを適用する必要があります。 たとえば、スペースは *%20* としてエンコードする必要があります。

- *sectionName* パラメーターは、`../notes/pages` のルートでのみ有効になります (`../notes/sections/{id}/pages` では無効です)。

セクションは存在しない場合に作成されるため、この呼び出しはアプリで作成するすべてのページに使用しても問題ありません。セクションの名前はユーザーが変更する可能性もありますが、API は指定されたセクション名で新しいセクションを作成します。API から返された、名前を変更したセクションを含むページへのリンクは、引き続き該当する古いページに到達する点に注意してください。 


<a name="message-body"></a>
## メッセージ本文の構築

ページのコンテンツを定義する HTML は、*入力 HTML* と呼ばれます。 入力 HTML は、[標準の HTML および CSS のサブセット](#supported-html)と、追加のカスタム 属性をサポートしています  (**data-id** や **data-render-src** のようなカスタム属性は[入力および出力 HTML](..\howto\onenote-input-output-html.md) で記述されます)。 

入力 HTML は、POST 要求のメッセージ本文で送信します。 入力 HTML は、`application/xhtml+xml` または `text/html` コンテンツ タイプを使用して、メッセージ本文に直接含めて送信することも、マルチパート要求の "Presentation" 部分に含めて送信することもできます。 

次に示す例では、入力 HTML をメッセージ本文に直接含めて送信しています。

```
POST https://www.onenote.com/api/v1.0/me/notes/pages
Authorization: Bearer {token}
Content-Type: application/xhtml+xml

<!DOCTYPE html>
<html>
  <head>
    <title>A page with a block of HTML</title>
    <meta name="created" content="2015-07-22T09:00:00-08:00" />
  </head>
  <body>
    <p>This page contains some <i>formatted</i> <b>text</b> and an image.</p>
    <img src="http://..." alt="an image on the page" width="500" />
  </body>
</html>
```

バイナリ データを送信する場合は、[マルチパート要求](#example)を使用する必要があります。 

>プログラミングを簡単にして、アプリでの一貫性を保つために、すべてのページの作成にマルチパート要求を使用できます。 マルチパート メッセージを作成するライブラリの使用をお勧めします。 これにより、無効な形式のペイロードを作成してしまう危険性が少なくなります。


<a name="input-html-rules"></a>
### *POST pages* 要求の入力 HTML に対する要件と制限事項

入力 HTML を送信するときには、次に示す一般的な要件と制限事項に注意してください。  

- 入力 HTML は、UTF-8 でエンコードされた整形式の XHTML である必要があります。 すべてのコンテナーの開始タグは、終了タグと一致する必要があります。 すべての属性値は、二重引用符または一重引用符で囲む必要があります。  <!--docs say MUST be encoded-->

- JavaScript コード (ファイルを含む) および CSS は削除されます。 

- HTML フォームは全体が削除されます。  

- OneNote API は [HTML 要素のサブセット](#supported-html)をサポートします。 

- OneNote API は一般的な HTML 属性のサブセットと、ページの更新に使用するカスタム属性 (**data-id** 属性など) のセットをサポートします。サポートされる属性については、「[入力 HTML と出力 HTML](..\howto\onenote-input-output-html.md)」を参照してください。


<a name="supported-html"></a>
### OneNote ページでサポートされる HTML と CSS

すべての要素、属性、およびプロパティがサポートされるわけではありません (HTML4、XHTML、CSS、HTML5 などについて)。OneNote API は、ユーザーの OneNote の使用方法に適した、限定的な HTML のセットを受け入れます。詳細については、「[ページでサポートされる HTML タグ](http://dev.onenote.com/docs#/introduction/html-tag-support-for-pages)」を参照してください。この参照先に示されていないタグは、無視されることがあります。

<!--The OneNote API only accepts UTF-8 data. Be sure that all requests are encoded that way, and your content-type headers indicate that as well. xx our examples don't show this-->

次に、OneNote API でサポートされる基本的な要素の一覧を示します。

<p id="indent">`<head>` and `<body>`</p>
<p id="indent">`<title>` and `<meta>` (ページのタイトルと作成日を設定する)</p>
<p id="indent">`<h1>` through `<h6>` (セクションの見出し用)</p>
<p id="indent">`<p>` (段落用)</p>
<p id="indent">`<ul>`, `<ol>`, and `<li>` (リストおよびリスト アイテム用)</p>
<p id="indent">`<table>`, `<tr>` and `<td>` (入れ子の表を含む)</p>
<p id="indent">`<pre>` (書式設定済みのテキスト用であり、空白と改行が維持される)</p>
<p id="indent">`<b>` and `<i>` (太字および斜体用)</p>

OneNote API は、ページの作成時に 入力 HTML の意味的な内容と基本的構造を維持しますが、サポートされている HTML および CSS のセットを使用するように入力 HTML を変換します。OneNote に存在しない機能は変換されないため、ソース HTML では認識されない可能性があります。 


<a name="example"></a>
## 要求の例

ここに示すマルチパート要求の例では、イメージと埋め込みファイルが含まれたページを作成します。必須の **Presentation** 部分には、ページを定義する HTML が含まれています。**imageBlock1** 部分には、バイナリ イメージ データが含まれ、**fileBlock1** にはバイナリ ファイル データが含まれています。データ部分には、OneNote API が OneNote ページに[イメージとして HTML をレンダリングする](../howto/onenote-images-files.md#image-img-binary-data-render-src)場合の HTML を含めることもできます。 

```
POST https://www.onenote.com/api/v1.0/me/notes/pages
Authorization: Bearer {token}
Content-Type: multipart/form-data; boundary=MyPartBoundary198374

--MyPartBoundary198374
Content-Disposition:form-data; name="Presentation"
Content-Type:text/html

<!DOCTYPE html>
<html>
  <head>
    <title>A page with rendered images and an attached file</title>
    <meta name="created" content="2015-07-22T09:00:00-08:00" />
  </head>
  <body>
    <p>Here's an image from an <i>online source</i>:</p>
    <img src="http://..." alt="an image on the page" width="500" />
    <p>Here's an image uploaded as <b>binary data</b>:</p>
    <img src="name:imageBlock1" alt="an image on the page" width="300" />
    <p>Here's a file attachment:</p>
    <object data-attachment="FileName.pdf" data="name:fileBlock1" type="application/pdf" />
  </body>
</html>

--MyPartBoundary198374
Content-Disposition:form-data; name="imageBlock1"
Content-Type:image/jpeg

... binary image data ...

--MyPartBoundary198374
Content-Disposition:form-data; name="fileBlock1"
Content-Type:application/pdf

... binary file data ...

--MyPartBoundary198374--
```

複数イメージやその他のファイルを含むページを作成する方法示す例については、「[画像とファイルを追加する](..\howto\onenote-images-files.md)」、および[チュートリアル](../howto/onenote-tutorial.md)と[サンプル](https://github.com/onenotedev)を参照してください。また、[絶対配置要素の作成](../howto/onenote-abs-pos.md)方法、[ノート シールの使用](../howto/onenote-note-tags.md)方法と、名刺キャプチャ、オンラインのレシピ一覧およびオンラインの製品一覧の[データ抽出](../howto/onenote-extract-data.md)方法について調べてください。

OneNote API は、マルチパート メッセージ本文の CRLF 改行など、一部の形式については厳密です。 無効な形式のペイロードを作成してしまう危険性を少なくするために、マルチパート メッセージはライブラリを使用して作成してください。 無効な形式のペイロードに関する 400 状態を受信した場合は、改行と空白の書式を確認し、エンコーディングの問題について調べます。 たとえば、`charset=utf-8` を使用してみてください (例: `Content-Type: text/html; charset=utf-8`)。

[入力 HTML に対する要件と制限事項](#input-html-rules) および [POST 要求のサイズ制限](..\howto\onenote-images-files.md#size-limits)を参照してください。


<a name="request-response-info"></a>
## *POST pages* 要求の要求情報と応答情報

| 要求データ | 説明 |  
|------|------|  
| プロトコル | すべての要求は SSL/TLS HTTPS プロトコルを使用します。 |  
| 承認ヘッダー | <p>`Bearer {token}`。*{token}* は、登録済みアプリの有効な OAuth 2.0 アクセス トークンになります。</p><p>これがないか、無効の場合、要求は失敗し、401 ステータス コードが表示されます。 「[認証とアクセス許可](..\howto\onenote-auth.md)」を参照してください。</p> |  
| Content-Type ヘッダー | <p>HTML コンテンツの `text/html` または `application/xhtml+xml`。メッセージ本文またはマルチパート要求の必須の "Presentation" パートで直接送信します。</p><p>マルチパート要求はバイナリ データを送信するときに必須であり、コンテンツ タイプとして `multipart/form-data; boundary=part-boundary` を使用します。*{part-boundary}* は、各データ パートの開始と終了を伝える文字列です。</p> |  
| Accept ヘッダー | `application/json` | 

| 応答データ | 説明 |  
|------|------|  
| 成功コード | 201 HTTP ステータス コード。 |  
| 応答本文 | JSON 形式での新しいページの OData 表現。 |  
| エラー | 要求が失敗すると、API は応答本文の **@api.diagnostics** オブジェクトでエラーを返します。 |  
| Location ヘッダー | 新しいページのリソース URL。 |  
| X-CorrelationId ヘッダー | 要求を一意に識別する GUID。Microsoft サポートと問題のトラブルシューティングを行う際に、この値を Date ヘッダーの値とともに使用できます。 |  


<a name="root-url"></a>
### OneNote サービスのルート URL の構築

[!INCLUDE [service root url section](../includes/onenote/service-root-section.xml)]


<a name="permissions"></a>
## アクセス許可

OneNote ページを作成するには、適切なアクセス許可を要求する必要があります。アプリの動作に必要な最低限のアクセス許可を選択してください。

[!INCLUDE [Create perms](../includes/onenote/create-perms.md)] 

アクセス許可のスコープと動作のしくみの詳細については、「[OneNote のアクセス許可のスコープ](../howto/onenote-auth.md)」を参照してください。

<a name="see-also"></a>
## その他のリソース

- [画像とファイルを追加する](../howto/onenote-images-files.md)
- [絶対配置要素を作成する](../howto/onenote-abs-pos.md)  
- [データを抽出する](../howto/onenote-extract-data.md)
- [ノート シールを使用する](../howto/onenote-note-tags.md)
- [OneNote の開発](../howto/onenote-landing.md)
- [OneNote デベロッパー センター](http://dev.onenote.com/)
- [OneNote の開発者ブログ](http://go.microsoft.com/fwlink/?LinkID=390183)
- [スタック オーバーフローに関する OneNote の開発の質問](http://go.microsoft.com/fwlink/?LinkID=390182) 
- [OneNote GitHub のリポジトリ](http://go.microsoft.com/fwlink/?LinkID=390178)


