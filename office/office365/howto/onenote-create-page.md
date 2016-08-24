---
ms.Toctitle: Create pages
title: "OneNote ページの作成"
description: "ms.TocTitle:ページの作成Title:OneNote ページの作成Description:任意のセクションに OneNote ページを作成して、イメージやファイルなどのコンテンツを含めます。ms.ContentId:0de322cc-570f-4afb-a313-b1d9c3d916f2ms.topic: 記事 (方法) ms.date:2016 年 5 月 17 日"
ms.ContentId: 0de322cc-570f-4afb-a313-b1d9c3d916f2
ms.date: May 17, 2016

---
[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]
[!INCLUDE [Add the ONAPI repo styles](../includes/controls/addonapistyles.xml)]

# OneNote ページの作成

*__Applies to:__ Consumer notebooks on OneDrive | Enterprise notebooks on Office 365*

OneNote ページを作成する場合は、POST 要求を *pages* エンドポイントに送信します。次に例を示します。 次に例を示します。

<p id="indent">`POST ../notes/sections/{id}/pages`</p>

メッセージ本文でページ定義する HTML を送信します。要求が成功すると、OneNote API は 201 HTTP 状態コードを返します。

>>POST 要求についての理解を深めるために、[対話型 REST リファレンス](http://dev.onenote.com/docs)を参照して、セクションの作成、セクション グループの作成、およびノートブックの作成を送信してください。


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

<p id="outdent1"> 既定のノートブックのセクション (名前で指定) にページを作成する (*ルールを参照*)</p>
<p id="indent1">`../me/notes/pages?sectionName=Some%20section%20name`</p>


<br />
完全な要求 URI は、次のいずれかのようになります。
<p id="indent">`https://www.onenote.com/api/v1.0/me/notes/sections/{id}/pages?sectionName=Homework`</p>
<p id="indent">`https://www.onenote.com/api/v1.0/myorganization/sitecollections/{id}/sites/{id}/notes/sections/{id}/pages`</p>
<p id="indent">`https://www.onenote.com/api/v1.0/myorganization/groups/{id}/notes/sections/{id}/pages`</p>

[!INCLUDE [service root url note](../includes/onenote/service-root-note.xml)]

<a name="post-pages-section-name"></a>
### *sectionName* URL パラメーターの使用

次に示すルールは、*sectionName* パラメーターを使用して、既定のノートブックの指名したセクションにページを作成するときに適用されます。

- 最上位のセクションのみを参照できます (セクション グループ内のセクションは参照できません)。

- 指定した名前のセクションが既定のノートブックに存在しない場合、そのセクションは API によって作成されます。セクション名に使用できない文字は、「? * \ / : < >  & # " % ~」です。 These characters are not allowed for section names: ? * \ / : &lt; &gt; | &amp; # " % ~

- セクション名の照合では、大文字と小文字が区別されませんが、セクションが作成されるときには大文字と小文字が維持されます。そのため、"My New Section" はこのとおりに表示されますが、これ以降の POST では "my new section" も一致します。

- Section names must be URL-encoded. 新しいセクション名は、URL としてエンコードする必要があります。たとえば、スペースは *%20* とエンコードします。

- *sectionName* パラメーターは、../notes/pages`../notes/pages` のルートでのみ有効になります (../notes/sections/{id}/pages`../notes/sections/{id}/pages` では無効です)。

セクションは存在しない場合に作成されるため、この呼び出しはアプリで作成するすべてのページに使用しても問題ありません。セクションの名前はユーザーが変更する可能性もありますが、API は指定されたセクション名で新しいセクションを作成します。API から返された、名前を変更したセクションを含むページへのリンクは、引き続き該当する古いページに到達する点に注意してください。 


<a name="message-body"></a>
## メッセージ本文の構築

The HTML that defines page content is called *input HTML*. Input HTML supports a [subset of standard HTML and CSS](#supported-html), with the addition of custom attributes. (Custom attributes, like **data-id** and **data-render-src**, are described in [Input and output HTML](..\howto\onenote-input-output-html.md).) 

Send the input HTML in the message body of the POST request. 入力 HTML は、POST 要求のメッセージ本文で送信します。入力 HTML は、application/xhtml+xml`application/xhtml+xml` または text/html`text/html` コンテンツ タイプを使用して、メッセージ本文に直接含めて送信することも、マルチパート要求の "Presentation" 部分に含めて送信することもできます。 

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

>To simplify programming and consistency in your app, you can use multipart requests to create all pages. It's a good idea to use a library to construct multipart messages. This reduces the risk of creating malformed payloads.


<a name="input-html-rules"></a>
### *POST pages* 要求の入力 HTML に対する要件と制限事項

入力 HTML を送信するときには、次に示す一般的な要件と制限事項に注意してください。  

- Input HTML should be UTF-8 encoded and well-formed XHTML. All container start tags require matching closing tags. すべての属性値は、上記の img タグのように、二重引用符または単一引用符で囲む必要があります。  <!--docs say MUST be encoded-->

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
<p id="indent">ページ タイトルと作成日を設定する <title> および <meta></p>
<p id="indent">`<h1>` through `<h6>` for section headings</p>
<p id="indent">`<p>` for paragraphs</p>
<p id="indent">`<ul>`, `<ol>`, and `<li>` for lists and list items</p>
<p id="indent">`<table>`, `<tr>` and `<td>`, including nested tables</p>
<p id="indent">`<pre><p id="indent"><c><pre></c>: 書式設定済みのテキスト用です (空白と改行が維持されます)</p></p>
<p id="indent">太字と斜体の文字スタイル用の <b> および <i></p>

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

The OneNote API is strict about some formats, such as CRLF newlines in a multipart message body. To reduce the risk of creating malformed payloads, you should use a library to construct multipart messages. If you do receive a 400 status for a malformed payload, check the formatting of newlines and whitespaces, and check for encoding issues. For example, try using `charset=utf-8` (example: `Content-Type: text/html; charset=utf-8`).

[入力 HTML に対する要件と制限事項](#input-html-rules) および [POST 要求のサイズ制限](..\howto\onenote-images-files.md#size-limits)を参照してください。


<a name="request-response-info"></a>
## *POST pages* 要求の要求情報と応答情報

| 要求データ | 説明 |  
|------|------|  
| プロトコル | すべての要求は SSL/TLS HTTPS プロトコルを使用します。 |  
| 承認ヘッダー | <p>`Bearer {token}`。ここで *{token}* は、登録済みのアプリの有効な OAuth 2.0 アクセス トークンです。</p><p>欠落している、または無効な場合は、401 ステータス コードで要求は失敗します。Azure AD を使用した認証 (エンタープライズ アプリ)を参照してください。 認証とアクセス許可</p> |  
| Content-Type ヘッダー | <p>`text/html` or `application/xhtml+xml` for the HTML content, whether it's sent directly in the message body or in the required "Presentation" part of multipart requests.</p><p>
           HTML コンテンツの text/html`multipart/form-data; boundary=part-boundary` または application/xhtml+xml。直接メッセージ本文で送信するか、マルチパート要求の必須の "Presentation" 部分で送信するかです。マルチパート要求は、バイナリ データを送信するときに必要になり、multipart/form-data; boundary=part-boundary コンテンツ タイプを使用します。この *{part-boundary}* は、各データ部分の開始と終了を知らせる文字列です。</p> |  
| 承諾ヘッダー | `application/json` | 

| 応答データ | 説明 |  
|------|------|  
| 成功コード | 201 HTTP ステータス コード。 |  
| 応答本文 | JSON 形式での新しいページの OData 表現。 |  
| エラー | 要求が失敗すると、API は応答本文の **@api.diagnostics** オブジェクトでエラーを返します。 |  
| Location ヘッダー | 新しいページのリソース URL。 |  
| X-CorrelationId ヘッダー | 要求を一意に識別する GUID。Microsoft サポートと問題のトラブルシューティングを行うときに、この値を Date ヘッダーの値とともに使用できます。 |  


<a name="root-url"></a>
### OneNote サービスのルート URL の構築

[!INCLUDE [service root url section](../includes/onenote/service-root-section.xml)]


<a name="permissions"></a>
## アクセス許可

OneNote ページを作成するには、適切なアクセス許可を要求する必要があります。アプリの動作に必要な最低限のアクセス許可を選択してください。

[!INCLUDE [Create perms](../includes/onenote/create-perms.md)] 

アクセス許可のスコープと動作のしくみの詳細については、「[OneNote のアクセス許可のスコープ](../howto/onenote-auth.md)」を参照してください。

<a name="see-also"></a>
## その他の技術情報

- [画像とファイルを追加する](../howto/onenote-images-files.md)
- [絶対的な位置要素を作成する](../howto/onenote-abs-pos.md)  
- [データを抽出する](../howto/onenote-extract-data.md)
- [ノート シールを使用する](../howto/onenote-note-tags.md)
- [OneNote 開発](../howto/onenote-landing.md)
- [OneNote デベロッパー センター](http://dev.onenote.com/)
- [OneNote の開発者ブログ](http://go.microsoft.com/fwlink/?LinkID=390183)
- [スタック オーバーフローに関する OneNote の開発の質問](http://go.microsoft.com/fwlink/?LinkID=390182) 
- [OneNote GitHub のリポジトリ](http://go.microsoft.com/fwlink/?LinkID=390178)


