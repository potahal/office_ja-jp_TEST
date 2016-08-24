---
ms.Toctitle: Add images, videos, and files
title: "OneNote ページに画像、ビデオ、ファイルを追加する" 
description: "OneNote ページに画像、Web ページのスナップショット、埋め込みビデオ、添付ファイルを追加する方法について説明します。"
ms.ContentId: cdee3bca-0da3-4225-9e31-7e0fb519a607
ms.date: July 5, 2016

---
[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]
[!INCLUDE [Add the ONAPI repo styles](../includes/controls/addonapistyles.xml)]

<style>#indent {margin:0px 0px 0px 25px;} #outdent {margin:0px 0px 0px 7px;} #tb-padding {margin:10px 0px 10px 0px;}</style>


# OneNote ページに画像、ビデオ、ファイルを追加する

*__適用対象:__OneDrive のコンシューマー ノートブック | Office 365 のエンタープライズ ノートブック*

**img**、**object**、および **iframe** 要素を使用すると、OneNote ページの[作成](../howto/onenote-create-page.md)時や[更新](../howto/onenote-update-page.md)時に、画像、ビデオ、およびファイルをページに追加できます。 

- **img** を使用して、ページに画像を表示します。
- **iframe** を使用して、ページにビデオを埋め込みます。
- **object** を使用して、ページに添付ファイルを追加します。

<a name="images"></a>
## 画像を追加する

URL の参照または生データの送信によって、画像を追加できます。OneNote API では、OneNote ページに画像、ロゴ、および写真を追加する次の方法がサポートされています。 

<p id="outdent">[Web からパブリック イメージを追加する](#image-img-url-src)</p>
<p id="indent">`img` と `src="http://image-url"` を使用します。パブリックにアクセス可能な画像の URL を指定します。 OneNote ページに画像を表示します。</p>
<p id="outdent">[バイナリ データを使用して画像を追加する](#image-img-binary-src)</p>
<p id="indent">`img` と `src="name:image-block-name"` を使用します。マルチパート要求のデータ部分で画像ファイルを送信します。 OneNote ページに画像を表示します。</p>
<p id="outdent">[Web ページのスナップショットを追加する](#image-img-url-data-render-src)</p>
<p id="indent">`img` と `data-render-src="http://webpage-url"` を使用します。Web ページの URL を指定します。 OneNote ページで Web ページ全体のスナップショットを表示します。</p>
<p id="outdent">[HTML から表示される画像を追加する](#image-img-binary-data-render-src)</p>
<p id="indent">`img` と `data-render-src="name:html-block-name"` を使用します。マルチパート要求のデータ部分で HTML を送信します。 OneNote ページに画像として HTML を表示します。</p>
<p id="outdent">[PDF ファイルのコンテンツの画像を追加する](#file-img-binary-data-render-src)</p>
<p id="indent">`<img data-render-src="name:part-name" />` を使用します。マルチパート要求のデータ部分で PDF ファイルを送信します。 OneNote ページの個別の画像として、PDF の各ページを表示します。</p>
<p id="outdent">[添付ファイルとして画像ファイルを追加する](#image-object)</p>
<p id="indent">`object` と `data="name:file-block-name" data-attachment="file-name.file-ext" type="media-type"` を使用します。マルチパート要求のデータ部分で画像ファイルを送信します。 添付ファイルを OneNote ページに追加して、ファイルのアイコンを表示します。</p>

>OneNote ページで画像を取得するには、最初に[ページのコンテンツの GET 要求](../howto/onenote-get-content.md#get-page-content)を送信します。 これにより、ページで画像リソースへの URL を返します。 次に、[画像リソースに対する GET 要求](../howto/onenote-get-content.md#get-resource)を分離します。

<br />
### 画像の属性

**img** 要素には、**alt**、**height**、**width** 属性をオプションで含めることができ、スタイル属性の **max-width** と **max-height** を含めることができます。

### 画像のメディア タイプ

OneNote API では、TIFF、PNG、GIF、JPEG、BMP の画像タイプがサポートされています。変換を望まない別の形式を使用する画像をキャプチャするには、マルチパート要求の[バイナリ データを送信](#image-img-binary-src)します。Base64 を使用する必要はなく、送信するバイナリ データをエンコードする必要もありません。

>API は元の入力画像の種類を検出し、それを **data-fullres-src-type** として[出力 HTML](../howto/onenote-input-output-html.md#image-output) で返します。 API は、**data-src-type** で最適化された画像の種類も返します。
 
メディアを含むページを作成する場合に適用する[制限](#size-limits)を確認してください。


<a name="image-img-url-src"></a>
### Web からパブリック イメージを追加する

要求の入力 HTML では、`<img src="http://..." />` を含め、**src** 属性の公開されていてアクセス可能な画像の URL を指定します。

```
Content-Type: multipart/form-data; boundary=MyAppPartBoundary
Authorization: Bearer {access-token}

--MyAppPartBoundary
Content-Disposition: form-data; name="Presentation"
Content-Type: text/html

<!DOCTYPE html>
<html>
  <head>
    <title>A page with an image: Public URL</title>
    <meta name="created" value="2015-11-11T12:45:00.000-8:00"/>
  </head>
  <body>
    <p>This page displays an image from the web.</p>
    <img src="http://..." width="300"/>
  </body>
</html>

--MyAppPartBoundary--  
```

<a name="image-img-binary-src"></a>
### バイナリ データを使用して画像を追加する

要求の **Presentation** 部分の入力 HTML に、`<img src="name:part-name" />` を含めます。ここで、*part-name* は、バイナリ画像データを含む[マルチパート要求](../howto/onenote-create-page.md#example) 
のデータ部分の一意識別子です。 バイナリ データのみを送信し、Base64 を使用したりエンコードしたりしないでください。

```
Content-Type: multipart/form-data; boundary=MyAppPartBoundary
Authorization: Bearer {access-token}

--MyAppPartBoundary
Content-Disposition: form-data; name="Presentation"
Content-Type: text/html

<!DOCTYPE html>
<html>
  <head>
    <title>A page with an image: Image binary data</title>
    <meta name="created" value="2015-11-11T12:45:00.000-8:00"/>
  </head>
  <body>
    <p>This page displays the uploaded image.</p>
    <img src="name:image-block-name" alt="a cool image" width="500"/>
  </body>
</html>

--MyAppPartBoundary
Content-Disposition: form-data; name="MyAppPictureId"
Content-Type: image/jpeg

... image binary data ...

--MyAppPartBoundary--  
```

<a name="image-img-url-data-render-src"></a>
### Web ページのスナップショットを追加する

OneNote API を使用して、Web ページ全体のスナップショットを作成し、新しいページに挿入できます。この方法は、Web ページのアーカイブや、(一部の CSS など) OneNote がサポートしない機能を持つ複雑な Web ページをキャプチャする場合に便利です。  

要求の入力 HTML では、`<img src="http://..." />` を含め、**src** 属性に挿入する Web ページの URL を指定します。

```
Content-Type: multipart/form-data; boundary=MyAppPartBoundary
Authorization: Bearer {access-token}

--MyAppPartBoundary
Content-Disposition: form-data; name="Presentation"
Content-Type: text/html

<!DOCTYPE html>
<html>
  <head>
    <title>A page with an image: Webpage capture</title>
    <meta name="created" value="2015-11-11T12:45:00.000-8:00"/>
  </head>
  <body>
    <p>This page displays an image of the webpage.</p>
    <img data-render-src="http://www.onenote.com" width="200"/>
  </body>
</html>

--MyAppPartBoundary--  
```

<a name="image-img-binary-data-render-src"></a>
### HTML から表示される画像を追加する
HTML をデータ ブロックとして渡す場合、ユーザーの資格情報または事前に読み込まれたブラウザー プラグインを必要とするアクティブ コンテンツがないことを確認してください。HTML ページを画像にレンダリングするために OneNote API が使用するエンジンには、ユーザーにログインする機能はありません。また、Adobe Flash、Apple QuickTime などのプラグインも組み込みません。これはまた、データの取得にユーザー ログイン資格情報または Cookie を必要とする場合、動的に読み込まれるコンテンツ (AJAX スクリプトを伴うなど) を表示できないことも意味します。

要求の **Presentation** 部分の入力 HTML に、`<img data-render-src="name:part-name" />` を含めます。ここで、*part-name* は、HTML を含む[マルチパート要求](../howto/onenote-create-page.md#example) 
のデータ部分の一意識別子です。

```
Content-Type: multipart/form-data; boundary=MyAppPartBoundary
Authorization: Bearer {access-token}

--MyAppPartBoundary
Content-Disposition: form-data; name="Presentation"
Content-Type: text/html

<!DOCTYPE html>
<html>
  <head>
    <title>A page with an image: HTML block</title>
    <meta name="created" value="2015-11-11T12:45:00.000-8:00"/>
  </head>
  <body>
    <p>This page displays the block of HTML as an image.</p>
    <img data-render-src="name:html-block-name" alt="a cool image" width="500"/>
  </body>
</html>

--MyAppPartBoundary
Content-Disposition: form-data; name="html-block-name"
Content-Type: text/html

<html>
<body>
<h1>This HTML will render as an image</h1>
<p><b>Don't</b> try to embed another <i>data-render-src</i> type-image inside the HTML part--
it won't work. Instead, use URL-based real images like this:</p>
<img src="http://cdn.onenote.net/1664161560_Images/OneNote.ico" />
</body>
</html>

--MyAppPartBoundary--  
```

<a name="image-object"></a>
### 添付ファイルとして画像ファイルを追加する

要求の **Presentation** 部分の入力 HTML に、`<object data="name:part-name" data-attachment="file-name.file-ext" type="media-type/media-subtype" />` を含めます。ここで、*part-name* は、バイナリ画像データを含む[マルチパート要求](../howto/onenote-create-page.md#example) 
のデータ部分の一意識別子です。 バイナリ データのみを送信し、Base64 を使用したりエンコードしたりしないでください。

```
Content-Type: multipart/form-data; boundary=MyAppPartBoundary
Authorization: Bearer {access-token}

--MyAppPartBoundary
Content-Disposition: form-data; name="Presentation"
Content-Type: text/html

<!DOCTYPE html>
<html>
  <head>
    <title>A page with an image: Binary image data as file attachment</title>
    <meta name="created" value="2015-11-11T12:45:00.000-8:00"/>
  </head>
  <body>
    <p>This page contains the image as a file attachment.</p>
    <object data-attachment="image-file.jpg" data="name:image-block-name" type="image/jpeg" />
  </body>
</html>

--MyAppPartBoundary
Content-Disposition: form-data; name="logo1-file"
Content-Type: image/jpeg

... binary file data ...

--MyAppPartBoundary--
```

[ファイルのメディア タイプ](#file-media-types)について説明します。


<a name="videos"></a>
## ビデオを追加する

入力 HTML の `<iframe data-original-src="http://..." />` を使用して、OneNote ページにビデオを埋め込むことができます。 

**サポートされるビデオ サイト**

- Dailymotion
- Office Mix
- Sway
- Sketchfab
- TED
- YouTube
- Vimeo
- Vine

<br />
### iframe の属性

<p id="outdent">**data-original-src**</p>
<p id="indent">必須。 ビデオの URL。<br />例: `data-original-src="https://www.youtube.com/watch?v=3Ztr44aKmQ8"`</p>
<p id="outdent">**width**</p>
<p id="indent">省略可能。 ビデオを含む iframe の幅。 既定値は 480 です。<br />例: `width="300"`</p>
<p id="outdent">**height**</p>
<p id="indent">省略可能。 ビデオを含む iframe の高さ。 既定値は 360 です。<br />例: `height="300"`</p>

<br />
### 例

要求の入力 HTML では、`<iframe data-original-src="http://..." />` を含め、**data-original-src** 属性のビデオの URL を指定します。

```
Content-Type: multipart/form-data; boundary=MyAppPartBoundary
Authorization: Bearer {access-token}

--MyAppPartBoundary
Content-Disposition: form-data; name="Presentation"
Content-Type: text/html

<!DOCTYPE html>
<html>
    <head>
        <title>A page with an embedded video</title>
    </head>
    <body>
        <iframe data-original-src="https://www.youtube.com/watch?v=3Ztr44aKmQ8" width="340" height="280"/>
    </body>
</html>

--MyAppPartBoundary--
```


<a name="files"></a>
## ファイルを追加する

入力 HTML の **object** 要素を使用して、添付ファイルを OneNote ページに追加できます。PDF ファイルを追加している場合は、**img** 要素を使用して、PDF ページを画像として表示できます。 

<p id="outdent">[添付ファイルを追加する](#file-object)</p>
<p id="indent">`<object .../>` を使用します。マルチパート要求のデータ部分でファイルを送信します。 OneNote ページでファイル アイコンを表示する添付ファイルを追加します。</p>
<p id="outdent">[PDF ファイルのコンテンツの画像を追加する](#file-img-binary-data-render-src)</p>
<p id="indent">`<img data-render-src="name:part-name" />` を使用します。マルチパート要求のデータ部分で PDF ファイルを送信します。 OneNote ページの個別の画像として、PDF の各ページを表示します。</p>

<br />
### ファイルの属性

**object** 要素には、次の属性が必要です。

<p id="outdent">**data-attachment**</p>
<p id="indent">OneNote ページで表示するファイル名と拡張子。<br />例: `data-attachment="filename.docx"`</p>
<p id="outdent">**data**</p>
<p id="indent">バイナリ ファイルのデータを含むマルチパート要求のボディ部の名前。 OneNote API は、ここでの URL 参照の受け渡しはサポートしていません。<br />例: `data="name:part-name"`</p>
<p id="outdent">**type**</p>
<p id="indent">ファイルのメディア タイプ。ページで利用するファイル アイコンの決定に使用されます。また、ユーザーが OneNote からデバイス上のファイルをアクティブ化する際に開始されるアプリケーションを決定します。<br />例: `type="application/pdf"`</p>


<a name="file-media-types"></a>
### ファイルのメディア タイプ

OneNote API は、添付ファイルの定義済みのファイル タイプのアイコンを使用し、API がファイル タイプを認識しない場合は汎用のアイコンを使用します。 API によって認識されている一般的なファイル タイプのいくつかを次の表に示します。

- application/pdf  
- application/vnd.openxmlformats-officedocument.wordprocessingml.document  
- application/vnd.openxmlformats-officedocument.presentationml.presentation
- application/vnd.openxmlformats-officedocument.spreadsheetml.sheet
- image/png
- image/jpeg
- image/gif
- audio/wav
- video/mp4
- application/msword
- application/mspowerpoint
- application/excel

メディアを含むページを作成する場合に適用する[制限](#size-limits)を確認してください。


<a name="file-object"></a>
### 添付ファイルを追加する

要求の **Presentation** 部分の入力 HTML に、`<object data="name:part-name" data-attachment="file-name.file-ext" type="media-type/media-subtype" />` を含めます。ここで、*part-name* は、バイナリ ファイルのデータを含む[マルチパート要求](../howto/onenote-create-page.md#example) 
のデータ部分の一意識別子です。 バイナリ データのみを送信し、Base64 を使用したりエンコードしたりしないでください。

```
Content-Type: multipart/form-data; boundary=MyAppPartBoundary
Authorization: Bearer {access-token}

--MyAppPartBoundary
Content-Disposition: form-data; name="Presentation"
Content-Type: text/html

<!DOCTYPE html>
<html>
  <head>
    <title>A page with an image file attachment</title>
  </head>
  <body>
    <p>This is an image file attachment.</p>
    <object data-attachment="Logo.jpg" data="name:logo1-file" type="image/jpeg" />
  </body>
</html>

--MyAppPartBoundary
Content-Disposition: form-data; name="logo1-file"
Content-Type: image/jpeg

... binary file data ...

--MyAppPartBoundary--
```

<a name="file-binary-data-render-src"></a>
### PDF ファイルのコンテンツの画像を追加する

要求の **Presentation** 部分の入力 HTML に、`<img data-render-src="name:part-name" ... />` を含めます。ここで、*part-name* は、バイナリ ファイルのデータを含む[マルチパート要求](../howto/onenote-create-page.md#example) 
のデータ部分の一意識別子です。 バイナリ データのみを送信し、Base64 を使用したりエンコードしたりしないでください。

```
Content-Type: multipart/form-data; boundary=MyAppPartBoundary
Authorization: Bearer {access-token}

--MyAppPartBoundary
Content-Disposition: form-data; name="Presentation"
Content-Type: text/html

<!DOCTYPE html>
<html>
  <head>
    <title>A page with images of the pages of a PDF file</title>
  </head>
  <body>
    <p>The pages of this PDF file render as images.</p>
    <img data-render-src="name:file-part" alt="PDF file as images" width="500"/>
  </body>
</html>

--MyAppPartBoundary
Content-Disposition: form-data; name="file-part"
Content-Type: application/pdf

... binary file data ...

--MyAppPartBoundary--  
```

<a name="size-limits"></a>
## *POST pages* 要求のサイズの上限

画像やファイルのデータを送信する場合は、これらの制限にご注意ください:

- 合計 POST サイズの上限は 70 MB までであり、画像、ファイル、その他のデータが含まれます。実際の上限はダウンストリームのエンコーディングに影響されるため、固定のバイト数の制限はありません。上限を超える要求は、信頼性の低い結果になる可能性があります。

- 各データ部分の上限は、part ヘッダーを含めて 25 MB です。上限を超えるデータ部分は、OneNote API によって拒否されます。 

- 1 ページあたりの画像の最大数は 150 です。 `src="http://..."` 属性を使用する場合、API は上限を超えた **img** タグを無視します。

- データ部分の最大数は、必須の **Presentation** パートを含めて POST あたり 6 です。

- 各要求は、**data-render-src** を使用する **img** 要素を最大で 5 つ、**data-render-src** を使用する **object** 要素を 1 つ含めることができます。 追加の画像とファイルの参照は無視されます。

- API への送信に使用する方法に関係なく、単一の POST 内の画像の最大数は 30 です。追加の画像は無視されます。多数の画像を含む Web ページをキャプチャする場合は、[スナップショットとしてページ全体をキャプチャする](#image-img-url-data-render-src)ことを検討してください。


## HTML を使用する場合と *data-render-src* を使用する場合 
HTML を OneNote ページに直接配置するか、**data-render-src** 属性を使用するかを検討している場合、以下を考慮してください。

- 複雑な HTMLは、OneNote API が受け入れることができるように HTML を変更するよりも、**data-render-src** を使用してレンダリング エンジンに送信する方が適しています。これは、サポートされていないタグが HTML に含まれている場合にも当てはまります。

- ページのレイアウトや外観を保持する正確なページ レンダリングは、**data-render-src** を使用したレンダリング エンジンを使用する方が適しています。

- 直接編集可能なテキストは、多くの場合 HTML をページに直接挿入する方法が適しています。表示された画像は、光学式文字認識 (OCR) システムによってスキャンされますが、これは同じではありません。

- 履歴またはアーカイブのスナップショット イン タイムには、通常 data-render-src メソッドが最も適しています。

- 改訂するための Web ページ設計のマークアップは、**data-render-src** が真価を発揮する分野です。OneNote のインク機能を使用して、変更箇所を示すためイメージ上に描画したり、重要な場所をコールアウトすることができます。Web ページをイメージにすることにより、これらがより簡単になります。

- 非常に大きい画像、または OneNote が直接そのまま受け入れない形式の画像は、独自のコード内で変換するよりも、**data-render-src** 属性を使用すると容易にサムネイル化されて変換できることがあります。画像がオンラインでも利用できる場合でも、POST にデータを埋め込むことで、OneNote ページの構築に必要なラウンドトリップの合計数を減らし、キャプチャしたページを OneNote ユーザーがすぐに利用できるようになる場合があります。

ユーザーにどの方法が最適かを判断するのに、アプリを開発する際に両方の方法を試してみることが最善の場合があります。


<a name="permissions"></a>
## アクセス許可

OneNote ページを作成または更新するには、適切なアクセス許可を要求する必要があります。アプリの動作に必要な最低限のアクセス許可を選択してください。

**_POST pages_ のアクセス許可**

[!INCLUDE [Create perms](../includes/onenote/create-perms.md)]

**_PATCH pages_ のアクセス許可**

[!INCLUDE [Update perms](../includes/onenote/update-perms.md)]

アクセス許可のスコープと動作のしくみの詳細については、「[OneNote のアクセス許可のスコープ](../howto/onenote-auth.md)」を参照してください。


<a name="see-also"></a>
## その他のリソース

- [OneNote の開発](../howto/onenote-landing.md)
- [OneNote デベロッパー センター](http://dev.onenote.com/)
- [OneNote の開発者ブログ](http://go.microsoft.com/fwlink/?LinkID=390183)
- [スタック オーバーフローに関する OneNote の開発の質問](http://go.microsoft.com/fwlink/?LinkID=390182) 
- [OneNote GitHub のリポジトリ](http://go.microsoft.com/fwlink/?LinkID=390178)  


[!INCLUDE [Enable filtering functionality ](../includes/controls/enablefiltering.xml)]
