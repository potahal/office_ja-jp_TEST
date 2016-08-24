---
ms.Toctitle: Input and output HTML
title: "OneNote ページの入出力 HTML"  
description: "onnvshort ページの作成に使用する入力 HTML と、ページのコンテンツを取得する際に返される出力 HTML について説明します。"
ms.ContentId: 3f916342-af4a-4386-8fb0-16630f3f944e
ms.date: July 5, 2016

---
[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]
[!INCLUDE [Add the ONAPI repo styles](../includes/controls/addonapistyles.xml)]

<style>##simpletable {余白:-5px 0px 0px 0px; 境界線: なし;} #simplecell {境界線: なし;パディング:15px 20px; バック グラウンド-カラー: ホワイト;}</style>

# OneNote ページの入出力 HTML

*__Applies to:__ Consumer notebooks on OneDrive | Enterprise notebooks on Office 365*

ページを作成または更新する際に、ページ コンテンツと構造を定義する HTML を *入力 HTML * と呼びます。 

ページ コンテンツを取得すると返される HTML を *出力 HTML* と呼びます。出力 HTML は入力 HTML と同じにはなりません。 Output HTML won't be the same as input HTML.

OneNote API は、入力 HTML の意味的コンテンツと基本的構造を保持しますが、その HTML を、[サポート済みの HTML 要素および CSS プロパティ](../howto/onenote-create-page.md#supported-html)のセットに変換します。API はさらに、OneNote 機能をサポートするカスタム属性も追加します。
 
この資料では、入力 HTML と出力 HTML の主な要素と属性について説明します。ページ コンテンツを作成または更新する場合には入力 HTMLについて、返されるページ コンテンツを解析する場合には出力 HTML について理解することは役立ちます。 

<a name="body"></a>
## 本文
ページ本文の HTML コンテンツは、画像やファイルのリソースなどを含むページのコンテンツと構造を表します。入力および出力 HTML では、**body** 要素に次の属性を含めることができます。

 要素の入力属性

| 入力属性 | 説明 |   
|------|------|  
| data-absolute-enabled | [絶対位置で配置](../howto/onenote-abs-pos.md)された要素を入力本文でサポートするかどうかを示します。 |  
| style | <p>要素の CSS [スタイル](#styles)のプロパティ。 本文の CSS のスタイルのプロパティ。出力 HTML では、適切な子要素で入力設定がインラインで返されることがあります。現在、body 要素では背景色はサポートされていません。</p><p>Background color is not currently supported for the **body** element.</p> |
 

 body 要素の出力属性

| 出力属性 | 説明 |   
|------|------|  
| data-absolute-enabled | [絶対位置で配置](../howto/onenote-abs-pos.md)された要素を入力本文でサポートするかどうかを示します。 Always **true** in output HTML. |  
| style | 本文の **font-family** および **font-size** プロパティ。 |


<a name="divs"></a>
## div
Divs contain text, images, and other content. div にはテキスト、画像、およびその他のコンテンツが含まれます。入力および出力 HTML では、**div** 要素に次の属性を含めることができます。

 div 要素の入力属性

| 入力属性 | 説明 |   
|------|------|  
| data-id | 要素の参照。 要素の参照。[ページ コンテンツを更新する](../howto/onenote-update-page.md)ために使用します。 |  
| data-render-fallback | [抽出](../howto/onenote-extract-data.md)が失敗した場合のフォールバック操作。**render** (既定) または **none**。 |  
| data-render-method | [抽出](../howto/onenote-extract-data.md)を実行する方法、たとえば: extract.businesscard`extract.businesscard` または extract.recipe`extract.recipe`。 |  
| data-render-src | [抽出](../howto/onenote-extract-data.md)のコンテンツのソース。 |  
| style | <p>div の位置、サイズ、フォント、および色のプロパティ。</p><p> - **position** (**absolute** only), **left**, **top**, and **width**. (Height is auto-configured for divs.)<br />Used to create an [absolute positioned](../howto/onenote-abs-pos.md) div, only if the div is a direct child of the body when the body sets `data-absolute-enabled="true"`.<br />例`<div style="position:absolute;width:360px;top:350px;left:300px" ... />`</p><p> 要素の CSS [スタイル](#styles)のプロパティ。 要素の CSS のスタイルのプロパティ。出力 HTML では、これらの値は適切な子要素でインラインで返されます。</p> |
 

OneNote API は、すべての本文のコンテンツを 1 つ以上の div でラップします。次の場合に、API は既定の div (data-id="_default" の属性に設定) を作成して本文のコンテンツを含めます。 OneNote API は、すべての本文のコンテンツを 1 つ以上の div でラップします。次の場合に、API は既定の div (data-id="_default"`data-id="_default"` の属性に設定) を作成して本文のコンテンツを含めます。

- 入力要素 body の **data-absolute-enabled** 属性が省略されたか、**false** に設定されている。この場合、すべての本文のコンテンツは既定の div に含められます。

- The input body element's **`data-absolute-enabled** attribute is **true**, but the input HTML contains direct children that aren't [absolute positioned](../howto/onenote-abs-pos.md)&nbsp;**div**, **img**, or **object** elements. In this case, direct children that aren't [absolute positioned](../howto/onenote-abs-pos.md)&nbsp;**div**, **img**, or **object** elements are put in the default div.


 div 要素の出力属性

| 出力属性 | 説明 |   
|------|------|  
| data-id | 要素の参照。 要素の参照。[ページ コンテンツを更新する](../howto/onenote-update-page.md)ために使用します。 | 
| id | 要求の一意の ID。 要素の一意の生成された ID。*includeIDs=true* クエリ オプションを使用した場合に、ページの content`includeIDs=true` エンドポイントに対する GET 要求から返されます。ページの更新のために使用します。 要素の参照。[ページ コンテンツを更新する](../howto/onenote-update-page.md)ために使用します。 | 
| style | div の位置とサイズのプロパティ。 |
 
### 作用していない div
入力 HTML の **div** 要素がページ構造に作用しておらず、OneNote が使用する情報を含んでいない場合、API は div のコンテンツを親または既定の div に移動します。このことは次の例で示しています。

作用していない入れ子型の div を含む**入力 HTML**。

```
<html>
    <head>
        <title>Page Title</title>
    </head>
    <body>
        <div>
            <p>Some text</p>
            <div>
                <p>More text inside a div that doesn't define page structure</p>
            </div>
        </div>
    </body>
</html>
```

**出力 HTML** 出力 HTML。div のコンテンツが親の div に移動し、入れ子状態の <div>`<div>` タグが削除されていることに注目してください。ただし、data-id などのセマンティック情報が div に定義されていたなら、div は保持されていました。 However, the div would have been preserved if it defined any semantic information, such as a **data-id** (example: `<div data-id="keep-me">`).

```
<html xmlns="http://www.w3.org/1999/xhtml" lang="en-US">
    <head>
        <title>Page Title</title>
    </head>
    <body data-absolute-enabled="true" style="font-family:Calibri;font-size:11px">
        <div data-id="_default" style="position:absolute;left:48px;top:120px;width:624px">
            <p>Some text</p>
            <p>More text inside a nested div</p>
        </div>
    </body>
</html>
```


<a name="images"></a>
## Images
Images on OneNote pages are represented by **img** elements. OneNote ページの画像は **img** 要素で表されます。入力 HTML および出力 HTML では、img 要素に次の属性を含めることができます。

 img 要素の入力属性

| 入力属性 | 説明 |   
|------|------|  
| alt | 画像に指定された代替テキスト。 | 
| data-id | 要素の参照。 要素の参照。[ページ コンテンツを更新する](../howto/onenote-update-page.md)ために使用します。 | 
| data-render-src | <p>**data-render-src** または **src** が必要です。</p><p>onnvshort ページ上のビットマップ画像として表示する Web ページ。<br />パブリック URL の場合には data-render-src="http://..." - `data-render-src="http://..."`。<br /> - `data-render-src="name:BlockName"` for an image part in the "Presentation" block of a [multipart request](../howto/onenote-create-page.md#example).</p><p>マルチパート要求の Presentation ブロックにある画像部分の data-render-src="name:BlockName"。このメソッドは、onnvshort ページで忠実に表示できるものよりも複雑な Web ページの場合、またはページにログイン資格情報が必要な場合に役立ちます。</p> |  
| data-tag | 要素上の[ノート シール](../howto/onenote-note-tags.md)。 | 
| style | <p>The position and size properties for the image: **position** (**absolute** only), **left**, **top**, **width**, and **height**.</p><p>Size can be set on any image. Position properties are used to create an [absolute positioned](../howto/onenote-abs-pos.md) image, only if the image is a direct child of the body when the body sets `data-absolute-enabled="true"`.<br />例`<img style="position:absolute;width:360px;top:350px;left:300px" ... />`</p><p>出力 HTML では、**width** 属性および **height** 属性で画像のサイズが個別に返されます。</p> |  
| src | <p>**src** または **data-render-src** が必要です。</p><p>onnvshort ページに表示する画像。<br />インターネットで一般に使用可能な画像の URL の場合には src="http://..." - `src="http://..."`。<br />画像を表すマルチパート要求の名前付き部分の src="name:BlockName" - `src="name:BlockName"`。</p> |
| width、height | 画像の幅または高さ。ピクセル単位ですが px は付けません。例: width="400" 例`width="400"` |
 
>> OneNote API は入力画像の種類を自動的に検出し、それを **data-fullres-src-type** として出力 HTML で返します。またこの API は、data-src-type で最適化された画像の画像種類を返します。 The API also returns the image type of the optimized image in **data-src-type**.
 

 img 要素の出力属性

| 出力属性 | 説明 |   
|------|------|  
| alt | 画像に指定された代替テキスト。 | 
| data-id | 要素の参照。 要素の参照。[ページ コンテンツを更新する](../howto/onenote-update-page.md)ために使用します。 | 
| data-index | 画像の位置のプロパティ。 画像の位置。[分割された画像](#split-images)のサポート向け。 | 
| data-fullres-src | 最初にページに埋め込まれていた画像リソースのバージョンのエンドポイント。 | 
| data-fullres-src-type | **data-fullres-src** リソースの メディア型。たとえば、image/png`image/png` またはimage/jpeg`image/jpeg`。 | 
| data-options | ソースの種類。PDF ファイルの場合は **printout**、その他すべての場合は **splitimage**。data-render-src 属性で作成された ] 分割された画像 にのみ適用されます。 Applies only to []split images](#split-images) created with the **data-render-src** attribute. | 
| data-render-original-src | ソース画像が公開されたインターネットの画像で、**data-render-src** 属性を付けて作成された場合の、画像の最初のソース URL。 | 
| data-src-type | **src** リソースの メディア型。たとえば、image/png`image/png` またはimage/jpeg`image/jpeg`。 | 
| data-tag | 要素上の[ノート シール](../howto/onenote-note-tags.md)。 | 
| id | 要求の一意の ID。 要素の一意の生成された ID。*includeIDs=true* クエリ オプションを使用した場合に、ページの content`includeIDs=true` エンドポイントに対する GET 要求から返されます。ページの更新のために使用します。 要素の参照。[ページ コンテンツを更新する](../howto/onenote-update-page.md)ために使用します。 | 
| src | Web ブラウザー、モバイル、およびタブレットのフォーム ファクター向けに最適化された画像リソースのバージョンのエンドポイント。 | 
| style | 画像の位置のプロパティ。 | 
| width、height | 画像の幅または高さ (ピクセル単位)。 | 
 

<a name="image-output-examples"></a>
### 画像の出力 HTML の例
出力 **img** 要素には、次に示すとおり、画像ファイルのリソースおよび画像の種類のためのエンドポイントが含まれています。バイナリ コンテンツを取得するには、[画像リソースのエンドポイントに対して個別に GET 要求を実行](../howto/onenote-get-content.md#get-resource)します。

```
<img 
    src="https://www.onenote.com/api/v1.0/me/notes/resources/{image-id}/$value"  
    data-src-type="image/png"
    data-fullres-src="https://www.onenote.com/api/beta/resources/{image-id}/$value"  
    data-fullres-src-type="image/png" ... />
```

残りのページのコンテンツと同様、画像はプライベートなもので、取得するには認証を必要とするため、ブラウザーでは画像は直接表示されません。 To get public URLs to the image resources on a page, include **preAuthenticated=true** in the query string when you retrieve the page content (example: `GET ../pages/{page-id}/content?preAuthenticated=true`). The public URLs that are returned are valid for one hour. 

**要求に _preAuthenticated=true_ が含まれる場合の、公開 URL を使用する画像**

```
<img 
    width="170" height="128" 
    src="https://www.onenote.com/api/v1.0/me/notes/resources/{image-id}/content?publicAuth=true&mimeType=image/jpeg" 
    data-src-type="image/{type}" 
    data-fullres-src="https://www.onenote.com/api/v1.0/me/notes/resources/{image-id}/content?publicAuth=true&mimeType=image/jpeg" 
    data-fullres-src-type="image/{type}" />
```

次の例は、**img** 要素が出力 HTML に含む可能性がある情報を示しています。

**Web 対応の高解像度リソースがある画像**

```
<img
    src="{web-ready-image-resource-url}/$value"
    data-src-type="image/{type}"
    data-fullres-src="{high-resolution-image-resource-url}/$value"
    data-fullres-src-type="image/{type}"
    [data-render-original-src="{original-source-url-or-named-part}"]
    [data-id="{image-id}"]
    [alt="supplied alt text"]
    [width="345"] [height="180"]
    [style="..."] />
```

 data-render-src 属性を使用して作成された画像

```
<img
    src="{web-ready-image-resource-url}/$value"
    data-src-type="image/{type}"
    data-fullres-src="{high-resolution-image-resource-url}/$value"
    data-fullres-src-type="image/{type}"
    data-render-original-src="{original-source-url-or-named-part}"
    [data-id="{image-id}"]
    [data-index="{index-of-split-image}"]
    [data-options="{printout-or-splitimage}"]
    [alt="supplied alt text"]
    [width="1024"] [height="1900"]
    [style="..."] />
```

分割された画像

**data-render-src** 属性 (Web ページの URL または名前が付いた部分のもの) を使用して作成された画像は、パフォーマンスと表示上の理由から複数のコンポーネントの画像に分割されることがあります。コンポーネント画像には、すべて同じ **data-id** 値が割り当てられます。各コンポーネントの画像には、元の垂直レイアウトを定義するゼロベースの data-index 属性があります。

**画像を 3 つのコンポーネントの画像に分割する**

```
<div data-id="multi-component-image" style="position:absolute;left:48px;top:120px;width:624px">
    <img
        src="{image-resource0-url}/$value"
        data-src-type="image/{type}"
        data-fullres-src="{image-resource0-url}/$value" 
        data-fullres-src-type="image/{type}" 
        data-index="0" 
        data-render-original-src="{original-source-url-or-named-part}"
        data-id="{same-image-id}" ... />
    <img 
        src="{image-resource1-url}/$value" 
        data-src-type="image/{type}" 
        data-fullres-src="{image-resource1-url}/$value" 
        data-fullres-src-type="image/{type}" 
        data-index="1" 
        data-render-original-src="{original-source-url-or-named-part}"
        data-id="{same-image-id}" ... />
    <img 
        src="{image-resource2-url}/$value" 
        data-src-type="image/{type}" 
        data-fullres-src="{image-resource2-url}/$value" 
        data-fullres-src-type="image/{type}" 
        data-index="2" 
        data-render-original-src=""{original-source-url-or-named-part}"
        data-id="{same-image-id}" ... />
</div>
```

ユーザーはページ上の画像を移動できるため、返されるインデックスの順序が乱れている場合があります。順序は、y 軸方向の競合がある場合、y 軸方向の上から下に 、続いて x 軸方向の左から右に並べ替えられます。


<a name="videos"></a>
## ビデオ
OneNote pages can contain embedded videos represented by **iframe** elements. 

>You can also [attach a video file using an **object** element](../howto/onenote-images-files.md#files).

**object 要素の入力属性**

| 入力属性 | 説明 |   
|------|------|  
| data-original-src | 必須。 ソース ページの URL。 See the [list of supported video sources](../howto/onenote-images-files.md#videos). 例`data-original-src="https://www.youtube.com/watch?v=3Ztr44aKmQ8"` |  
| width、height | 画像の幅または高さ (ピクセル単位)。 例`width=300` | 

**object 要素の出力属性**

| 出力属性 | 説明 |   
|------|------|  
| data-original-src | ソース ページの URL。 |  
| src | A link to the video that is embedded in the OneNote page. |  
| width、height | 画像の幅または高さ (ピクセル単位)。 例`width=300` |  
 
オブジェクトの出力 HTML の例

Output **iframe** elements contain endpoints that link to the source page and video, as shown below. 

```
<iframe 
    width="340" height="280" 
    data-original-src="https://www.youtube.com/watch?v=3Ztr44aKmQ8" 
    src="https://www.youtube.com/embed/3Ztr44aKmQ8?feature=oembed&autoplay=true" />
``` 


<a name="objects"></a>
## オブジェクト
OneNote ページには、**object** 要素で表される添付ファイルを含めることができます。**object** 要素は、入力 HTML および出力 HTML に次の属性を含めることができます。

>ファイルが画像として送信され、かつ data-render-src 属性を使用する場合、onnvshort API はファイルのコンテンツを画像としてページに表示することができます。例: <img data-render-src="name:BlockName" ... /> 例`<img data-render-src="name:part-name" ... />`
 

**object 要素の入力属性**

| 入力属性 | 説明 |   
|------|------|  
| data | 必須。 必須。[マルチパート要求](../howto/onenote-create-page.md#example)のファイルを表す部分の名前。 | 
| data-attachment | 必須。ファイル名。 | 
| data-id | 要素の参照。 要素の参照。[ページ コンテンツを更新する](../howto/onenote-update-page.md)ために使用します。 | 
| style | <p>The position and size properties for the object: **position** (**absolute** only), **left**, **top**, and **width**.</p><p>position (absolute のみ), left、top、および width プロパティ。本文で data-absolute-enabled="true" が設定されているときにオブジェクトが本文の直接の子である場合にのみ、絶対位置で配置されたオブジェクトの作成に使用します。 絶対位置で配置された要素の詳細について確認してください。 例: <object style="position:absolute;top:350px;left:300px" ... /><br />例`<object style="position:absolute;top:350px;left:300px" ... />`</p> | 
| type | 必須。標準のメディア ファイルの型。既知のファイルの型では、OneNote ページにファイルの種類と関連付けられたアイコンが表示されます。未知のファイルの型では、汎用のファイル アイコンが表示されます。 | 
<!--todo: add link to known file types--> 

**object 要素の出力属性**

| 出力属性 | 説明 |   
|------|------|  
| data | ファイルのリソースのエンドポイント。 |  
| data-attachment | ファイル名。 |  
| data-id | 要素の参照。 要素の参照。[ページ コンテンツを更新する](../howto/onenote-update-page.md)ために使用します。 |  
| id | 要求の一意の ID。 要素の一意の生成された ID。*includeIDs=true* クエリ オプションを使用した場合に、ページの content`includeIDs=true` エンドポイントに対する GET 要求から返されます。ページの更新のために使用します。 要素の参照。[ページ コンテンツを更新する](../howto/onenote-update-page.md)ために使用します。 |  
| style | オブジェクトの位置のプロパティ。 |  
| type | 標準のメディア ファイルの種類。 |  
 

オブジェクトの**出力 HTML** の例

出力 **object** 要素には、次に示されているように、ページ内のファイル リソースに関連付けられたエンドポイントが含まれています。バイナリ コンテンツを取得するには、[ファイル リソースのエンドポイントに対して個別の GET 要求を実行](../howto/onenote-get-content.md#get-resource)します。

```
<object
    data="http://www.onenote.com/api/v1.0/me/notes/resources/{file-id}/$value"
    data-attachment="fileName.pdf" 
    type="application/pdf" 
    [style="..."] />
``` 


<a name="text"></a>
## 段落と見出し

段落、見出し、およびその他のテキストのコンテナーには、入力 HTML と出力 HTML の次の属性を含めることができます。

**テキストのコンテナーの入力属性**

| 入力属性 | 説明 |   
|------|------|  
| data-id | 要素の参照。 要素の参照。[ページ コンテンツを更新する](../howto/onenote-update-page.md)ために使用します。 |  
| data-tag | A [note tag](../howto/onenote-note-tags.md) on a **p** or **h1** - **h6** element. |  
| style | 要素の CSS  [スタイル](#styles) のプロパティ。 |  
 

**テキストのコンテナーの出力属性**

| 出力属性 | 説明 |   
|------|------|  
| data-id | 要素の参照。 要素の参照。[ページ コンテンツを更新する](../howto/onenote-update-page.md)ために使用します。 |  
| data-tag | A [note tag](../howto/onenote-note-tags.md) on a **p** or **h1** - **h6** element. |  
| id | 要求の一意の ID。 要素の一意の生成された ID。*includeIDs=true* クエリ オプションを使用した場合に、ページの content`includeIDs=true` エンドポイントに対する GET 要求から返されます。ページの更新のために使用します。 要素の参照。[ページ コンテンツを更新する](../howto/onenote-update-page.md)ために使用します。 |  
| style | 要素の CSS [スタイル](#styles)のプロパティ。出力 HTML では、これらの値は、適切な子要素または **span** 要素でインラインで返すことができます。 |  
 

次の例では、テキストのコンテナーおよび返される出力 HTML にスタイルを定義するさまざまな方法を使用する入力 HTML を示しています。

開始タグ内、および **span** 要素内に、インライン文字スタイルを使用して定義されたスタイル付き **入力 HTML**。

```
<h1>Heading <i>One</i> text</h1>
<p style="font-size:8pt;color:green;font-family:Courier;text-align:center">Some text</p>
<p>Some <span  style="font-size:16px;color:#ff0000;font-family:Segoe UI Black">more</span> text</p>
``` 

**span`<i>` 要素でインライン CSS スタイルとして返される開始タグ <i>`<p>` 内の文字スタイルとフォント設定 <p>** を伴う**出力 HTML**。

```
<h1 style="font-size:16pt;color:#1e4e79;margin-top:11pt;margin-bottom:11pt">Heading <span style="font-style:italic">One</span> text</h1>
<p style="text-align:center"><span style="font-family:Courier;font-size:8pt;color:green">Some text</span></p>
<p>Some <span style="font-family:Segoe UI Black;font-size:12pt;color:red">more</span> text</p>
``` 


<a name="lists"></a>
## リスト
リストは、**li** 要素を含む **ol** 要素または **ul** 要素として表されます。

入力 HTML および出力 HTML には、リストとリスト項目に次の属性を含めることができます。

**リストの入力属性**

| 入力属性 | 説明 |   
|------|------|  
| data-id | 要素の参照。 要素の参照。[ページ コンテンツを更新する](../howto/onenote-update-page.md)ために使用します。 |  
| data-tag | **ul**、**ol**、または **li** の各要素上の[ノート シール](../howto/onenote-note-tags.md)。 |  
| style | リストまたはリスト項目の **list-style-type** および CSS [スタイル](#styles) のプロパティ。 |  
 

**リストの出力属性**

| 出力属性 | 説明 |   
|------|------|  
| data-id | 要素の参照。 要素の参照。[ページ コンテンツを更新する](../howto/onenote-update-page.md)ために使用します。 |  
| data-tag |  **li** 要素内の span 上の[ノート シール](../howto/onenote-note-tags.md)。 |  
| id | 要求の一意の ID。 要素の一意の生成された ID。*includeIDs=true* クエリ オプションを使用した場合に、ページの content`includeIDs=true` エンドポイントに対する GET 要求から返されます。ページの更新のために使用します。 要素の参照。[ページ コンテンツを更新する](../howto/onenote-update-page.md)ために使用します。 |  
| style | 要素の **list-style-type** および CSS [スタイル](#styles)のプロパティ。出力 HTML では、リストレベル設定はリスト項目で返されます。既定のプロパティは返されません。 |  
 

OneNote API は次のリストのスタイルをサポートしています。

| 番号付きリスト | 記号付きリスト |  
|------|------|  
| なし | なし |  
| decimal (default) | disc (default) |  
| lower-alpha | Circle |
| lower-roman | square |  
| upper-alpha | &nbsp; |  
| upper-roman | &nbsp; |
 
入力 HTML の **ol** 要素または **ul** 要素のリストにはグローバル スタイルを適用できますが、スタイルは **li** 要素で返されます。

**同種リスト スタイル**

この例は、**ol** 要素のリスト スタイルの種類と個々のリスト項目の CSS スタイルを設定する入力 HTML を示しています。

```
<ol style="list-style-type:upper-roman;color:blue">
    <li style="font-weight:bold">Jacksonville</li>
    <li style="text-decoration:line-through">Orlando</li>
    <li style="font-family:Courier">Naples</li>
</ol>
``` 

This is the output HTML. これは出力 HTML です。スタイルが個々の **li** 要素または **span** 要素でインラインで返されることに注目してください。

```
<ol>
    <li style="list-style-type:upper-roman"><span style="color:blue;font-weight:bold">Jacksonville</span></li>
    <li style="list-style-type:upper-roman"><span style="color:blue;text-decoration:line-through">Orlando</span></li>
    <li style="list-style-type:upper-roman"><span style="font-family:Courier;color:blue">Naples</span></li>
</ol>
``` 

**可変のリスト スタイル**

この例は、**li** 要素にさまざまなリスト スタイルの種類を設定する入力 HTML を示しています。

```
<ul style="font-style:italic">
    <li style="list-style-type:square">square style</li>
    <li style="list-style-type:circle">circle style</li>
    <li style="list-style-type:disc">disc style (default)</li>
</ul>
``` 

This is the output HTML. これは出力 HTML です。スタイルが個々の **li** 要素または **span** 要素でインラインで返されることに注目してください。

```
<ul>
    <li style="list-style-type:square"><span style="font-style:italic">square style</span></li>
    <li style="list-style-type:circle"><span style="font-style:italic">circle style</span></li>
    <li><span style="font-style:italic">disc style (default)</span></li>
</ul>
``` 


<a name="tables"></a>
## テーブル
テーブルは、**tr** 要素および **td** 要素を含めることができる **table** 要素として表されます。入れ子型のテーブルがサポートされています。 Nested tables are supported.

入力 HTML および出力 HTML には、リストとリスト項目に次の属性を含めることができます。 テーブルには、入力 HTML と出力 HTML に次の属性を含めることができます。OneNote API では、**rowspan** 属性または **colspan** 属性はサポートされていません。

**テーブルの入力属性**

| 入力属性 | 説明 |   
|------|------|  
| data-id | 要素の参照。 要素の参照。[ページ コンテンツを更新する](../howto/onenote-update-page.md)ために使用します。 |  
| style | 要素の CSS [スタイル](#styles)のプロパティ。さらに、<br />Border border。0px または 1px のいずれか。<br />width **width**。**table** および td がピクセル単位またはページ幅の割合 (パーセント) でサポートされています。例: width="100px" または width="60%"<br />Example: `width="100px"` or `width="60%"` |  
 

**テーブルの出力属性**

| 出力属性 | 説明 |   
|------|------|  
| data-id | 要素の参照。 要素の参照。[ページ コンテンツを更新する](../howto/onenote-update-page.md)ために使用します。 |  
| id | 要求の一意の ID。 要素の一意の生成された ID。*includeIDs=true* クエリ オプションを使用した場合に、ページの content`includeIDs=true` エンドポイントに対する GET 要求から返されます。ページの更新のために使用します。 要素の参照。[ページ コンテンツを更新する](../howto/onenote-update-page.md)ために使用します。 |  
| style | 要素の CSS [スタイル](#styles)のプロパティ。 |  
 

次の例では、さまざまな方法を使用してテーブルのスタイルと返される出力 HTML を定義する入力 HTML を示しています。

さまざまなレベルのオプション設定を伴う**入力 HTML**。

```
<table style="border:0px;width:500px;background-color:green">
    <tr> 
        <td>Cell 1</td> 
        <td>Cell 2</td> 
        <td>Cell 3</td> 
    </tr> 
    <tr style="background-color:blue"> 
        <td style="text-align:right;background-color:red">Left</td> 
        <td style="text-align:center">Middle</td> 
        <td>Right</td> 
    </tr> 
</table>
```
 
**td** 要素でインラインで返される CSS スタイルを伴う**出力 HTML**。

```
<table style="border:0px">
    <tr>
        <td style="background-color:green;width:166;border:0px">Cell 1</td>
        <td style="background-color:green;width:166;border:0px">Cell 2</td>
        <td style="background-color:green;width:166;border:0px">Cell 3</td>
    </tr>
    <tr>
        <td style="background-color:red;width:166;border:0px;text-align:right">Left</td>
        <td style="background-color:blue;width:166;border:0px;text-align:center">Middle</td>
        <td style="background-color:blue;width:166;border:0px">Right</td>
    </tr>
</table>
``` 


<a name="styles"></a>
## スタイル
OneNote の API は、ページ本文の要素として次のインライン CSS **style** プロパティを サポートしています。たとえば、**body**、**div**、**p**、**li**、および **span** です。

| プロパティ | 例 |  
|------|------|  
| background-color | style="background-color:#66cc66"`style="background-color:#66cc66"` (既定は白)<br />16 進数形式と色の名前の両方をサポートしています。 |  
| color | style="color:#ffffff"`style="color:#ffffff"` (既定は black です) |  
| font-family | style="font-family:Courier"`style="font-family:Courier"` (既定は Calibri です) |  
| font-size | style="font-size:10pt"`style="font-size:10pt"` (既定は 11pt)<br />onnvshort API では pt または px のフォント サイズを使用できますが、px を pt に変換します。10 進数値は最も近い n.0pt または n.5pt に丸められます。 onnvshort API では pt または px のフォント サイズを使用できますが、px を pt に変換します。10 進数値は最も近い n.0pt または n.5pt に丸められます。 |  
| font-style | style="font-style:italic"`style="font-style:italic"` (標準または斜体のみ) |  
| font-weight | style="font-weight:bold"`style="font-weight:bold"` (標準または太字のみ) |  
| strike-through | `style="text-decoration:line-through"` |  
| text-align | style="text-align:center"`style="text-align:center"` (ブロック要素の場合のみ) |  
| text-decoration | style="text-decoration:underline"`style="text-decoration:underline"` (なし、または下線のみ) |  
 

次のインライン文字スタイルもサポートされています。

<table id="simpletable">
<tr>
<td id="simplecell">b. </td>
<td id="simplecell">&lt;<i>&gt;</td>
<td id="simplecell">&lt;<u>&gt;</td>
</tr>
<tr>
<td id="simplecell">&lt;<em>&gt;</td>
<td id="simplecell">&lt;<strong>&gt;</td>
<td id="simplecell">&lt;<strike>&gt;</td>
</tr>
<tr>
<td id="simplecell">&lt;<sup>&gt;</td>
<td id="simplecell">&lt;<sub>&gt;</td>
<td id="simplecell">&lt;<del>&gt;</td>
</tr>
<tr>
<td id="simplecell">&lt;<cite>&gt;</td>
<td id="simplecell">&nbsp;</td>
<td id="simplecell">&nbsp;</td>
</tr>
</table>

 
<a name="example-input-output"></a>
##次の画像は、OneNote API で作成された簡単なページを示しています。

**入力 HTML から作成されたサンプル ページ**

これは、ページを作成するためにメッセージ本文で送信された入力 HTML です。

```
<html lang="en-US">
    <head>
        <title>Sample Study Notes</title>
        <meta name="created" content="2015-01-01T01:01"/>
    </head>
    <body>
        <h1>Aurora Borealis</h1>
        <p>Dancing lights in the sky. Also called <i>Northern Lights</i>. Caused by solar radiation.</p>
        <br />
        <p><b>Intersting facts</b></p>
        <table>
            <tr>
                <td>Neil Armstrong</td>
                <td>Commander</td>
            </tr>
            <tr>
                <td>Buzz Aldrin</td>
                <td>LM Pilot</td>
            </tr>
            <tr>
                <td>Michael Collins</td>
                <td>Command Module Pilot</td>
            </tr>
        </table>
        <img alt="Apollo 11 commemorative stamp." src="http://upload.wikimedia.org/wikipedia/commons/a/a4/First_Man_on_Moon_1969_Issue-10c.jpg"  width="400"/>
        <p>References:</p>
        <p><a href="http://en.wikipedia.org/wiki/Apollo_11">http://en.wikipedia.org/wiki/Apollo_11</a></p>
        <p><a href="http://www.nasa.gov/mission_pages/apollo/missions/apollo11.html">http://www.nasa.gov/mission_pages/apollo/missions/apollo11.html</a></p>
    </body>
</html>
``` 

これは、[ページ コンテンツを取得](#../howto/onenote-get-content.md#get-page-content)する際に OneNote API が返す出力 HTML です。

>> [ページを作成する](../howto/onenote-create-page.md)際、または[ページのメタデータを取得する](../howto/onenote-get-content.md#get-page)際、API は、**contentUrl** プロパティのページの*コンテンツ* エンドポイント URL を返します。

```
<html xmlns="http://www.w3.org/1999/xhtml" lang="en-US">
    <head>
        <title>Sample Study Notes</title>
        <meta name="created" content="2015-01-01T01:01:00.0000000" />
    </head>
    <body data-absolute-enabled="true" style="font-family:Calibri;font-size:11pt">
        <div data-id="_default" style="position:absolute;left:48px;top:120px;width:624px">
            <h1 style="font-size:16pt;color:#1e4e79;margin-top:11pt;margin-bottom:11pt">American History 101: Moon Landing</h1>
            <p>First moon landing - July 20, 1969 with Apollo 11 (Eagle)</p>
            <br />
            <p><span style="font-weight:bold">Apollo 11 Astronauts</span></p>
            <table style="border:0px">
                <tr>
                    <td style="border:0px">Neil Armstrong</td>
                    <td style="border:0px">Commander</td>
                </tr>
                <tr>
                    <td style="border:0px">Buzz Aldrin</td>
                    <td style="border:0px">LM Pilot</td>
                </tr>
                <tr>
                    <td style="border:0px">Michael Collins</td>
                    <td style="border:0px">Command Module Pilot</td>
                </tr>
            </table>
            <br />
            <img alt="Apollo 11 commemorative stamp." width="400" height="248" src="https://www.onenote.com/api/v1.0/me/notes/resources/0-f717b5fa5eaa454da7ecdf72a8c137fe!1-73DBAF9B7E5C4B4C!10456/$value"
                 data-src-type="image/jpeg" data-fullres-src="https://www.onenote.com/api/v1.0/me/notes/resources/0-f717b5fa5eaa454da7ecdf72a8c137fe!1-73DBAF9B7E5C4B4C!10456/$value" data-fullres-src-type="image/jpeg" />
            <p>References:</p>
            <p><a href="http://en.wikipedia.org/wiki/Apollo_11">http://en.wikipedia.org/wiki/Apollo_11</a></p>
            <p><a href="http://www.nasa.gov/mission_pages/apollo/missions/apollo11.html">http://www.nasa.gov/mission_pages/apollo/missions/apollo11.html</a></p>
        </div>
    </body>
</html>
``` 


<a name="see-also"></a>
## その他の技術情報

- [OneNote コンテンツと構造を取得する](../howto/onenote-get-content.md#get-page-content)
- [OneNote ページの作成](../howto/onenote-create-page.md)
- [OneNote ページ コンテンツを更新する](../howto/onenote-update-page.md)
- [画像とファイルを追加する](../howto/onenote-images-files.md)
- [OneNote 開発](../howto/onenote-landing.md)
- [OneNote デベロッパー センター](http://dev.onenote.com/)
- [OneNote の開発者ブログ](http://go.microsoft.com/fwlink/?LinkID=390183)
- [スタック オーバーフローに関する OneNote の開発の質問](http://go.microsoft.com/fwlink/?LinkID=390182) 
- [OneNote GitHub のリポジトリ](http://go.microsoft.com/fwlink/?LinkID=390178)  
