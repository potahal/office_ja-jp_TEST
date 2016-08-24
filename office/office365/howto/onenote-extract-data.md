---
ms.Toctitle: Extract data
title: "キャプチャからデータを抽出する" 
description: "onnvshort API を使用して、名刺、オンライン レシピ、またはオンラインの製品リストのコンテンツを拡張して変換する方法について説明します。"
ms.ContentId: 901068e3-1d4d-4233-856b-5a2a71cc58c2
ms.date: January 19, 2016
---

[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]
[!INCLUDE [Add the ONAPI repo styles](../includes/controls/addonapistyles.xml)]

<style>##simpletable {margin:-5px 0px 0px 0px; border:none;} #simplecell {border:none; padding:15px 20px; background-color:white;} .rightalign {text-align:right; padding:0px 20px 0px 0px;}</style>

# キャプチャからデータを抽出する

*__Applies to:__ Consumer notebooks on OneDrive | Enterprise notebooks on Office 365*

OneNote API を使用して、画像から名刺データを抽出したり、URL からレシピや製品データを抽出したりします。

<a name="attributes"></a>
## 抽出の属性

単に div を含めるだけで、データの抽出や変換ができます。div は [create-page](../howto/onenote-create-page.md) または [update-page](../howto/onenote-update-page.md) 要求で、ソース コンテンツ、抽出メソッド、およびフォールバックの動作を指定します。API は抽出したデータを、読みやすい形式でページに表示します。 

```
<div
  data-render-src="image-or-url"
  data-render-method="extraction-method"
  data-render-fallback="fallback-action">
</div>
```

**data-render-src**

コンテンツ ソース。名刺の画像、または数多くの人気レシピや製品の Web サイトの絶対 URL にできます。必須。

URL を指定するときに最適な結果を得るために、ソースの Web ページの HTML で定義された正規 URL を使用してください (定義されている場合)。たとえば、正規 URL はソースの Web ページで次のように定義されます。

<p id="indent">`<link rel="canonical" href="www.domainname.com/page/123/size12/type987" />`</p>


**data-render-method**

実行する抽出メソッド: 必須。

| 値 | 説明 |
|:------|:------|
| extract.businesscard | 名刺の抽出。 |
| extract.recipe | レシピの抽出。 |
| extract.product | 製品リストの抽出。 |
| extract | 不明な種類の抽出。 |

For best results, specify the content type (`extract.businesscard`, `extract.recipe`, or `extract.product`) if you know it. 最善の結果を得るために、コンテンツの種類 (extract.businesscard`extract`、extract.recipe、または extract.product) がわかっている場合は種類を指定します。種類が不明な場合は extract メソッドを使用すると OneNote API により自動検出が試行されます。

**data-render-fallback**

抽出できない場合のフォールバック動作です。 省略すると、既定の **render** になります。 

| 値 | 説明 |
|:------|:------|
| render | レシピまたは製品の Web ページのソース画像またはスナップショットを表示します。 |
| none | 何も起こりません。<br />何も実行しません。このオプションは、抽出コンテンツ以外に、ページに名刺または Web ページのスナップショットを必ず含める場合に役立ちます。要求に img 要素を別個に含めてください。 Be sure to send a separate `img` element in the request, as shown in the examples. |

<a name="biz-card"></a>
## 名刺の抽出

OneNote API は、個人または会社の名刺の画像に基づいて、次の連絡先情報の検索および表示を試行します。

<table role="presentation" id="simpletable">
<tr>
<td id="simplecell">名前<br /><br />役職<br /><br />組織<br /><br />電話および FAX 番号<br /><br />郵送先および実際の住所<br /><br />電子メール アドレス<br /><br />Web サイト<br /></td>
<td id="simplecell" class="rightalign">![An example business card extraction.](images\onenote\biz-card-extraction.png)</td>
</tr>
</table>

ページには、抽出された連絡先情報が含まれる vCard (.VCF ファイル) も組み込まれます。この vCard は、ページの HTML コンテンツを取得するときに連絡先情報を入手するのに便利な方法です。

### 名刺抽出の一般的なシナリオ

**名刺情報を抽出し名刺の画像を表示する**

Specify the `extract.businesscard` method and the `none` fallback. Also send an `img` element with the `src` attribute that also references the image. extract.businesscard メソッドを指定し、既定の render フォールバックを使用します。API がコンテンツを抽出できない場合は、代わりに名刺の画像を表示します。

```html 
<div
    data-render-src="name:scanned-card-image"
    data-render-method="extract.businesscard"
    data-render-fallback="none">
</div>
<img src="name:scanned-card-image" />
```

<br />名刺情報を抽出する。抽出に失敗した場合のみ名刺の画像を表示する

Specify the `extract.businesscard` method and use the default `render` fallback. extract.businesscard メソッドを指定し、既定の render フォールバックを使用します。API がコンテンツを抽出できない場合は、代わりに名刺の画像を表示します。

```html
<div
    data-render-src="name:scanned-card-image"
    data-render-method="extract.businesscard">
</div>
```
 
名刺の抽出では、画像はマルチパートの要求の名前付きパートとして送信されます。要求で画像を送信する例については、「[画像とファイルの追加](../howto/onenote-images-files.md)」を参照してください。


<a name="recipe"></a>
## レシピの抽出

OneNote API は、レシピの URL に基づいて、次の情報の検索および表示を試行します。


<table role="presentation" id="simpletable">
<tr>
<td id="simplecell">完成品画像<br /><br />評価<br /><br />材料<br /><br />手順<br /><br />準備、調理、および合計の時間<br /><br />盛付け</td>
<td id="simplecell" class="rightalign">![An example recipe extraction.](images\onenote\recipe-extraction.png)</td>
</tr>
</table>

この API は、*Allrecipes.com*、*FoodNetwork.com*、および *SeriousEats.com* など、数多くの人気サイトのレシピ用に最適化されています。

### レシピ抽出の一般的なシナリオ

**レシピ情報を抽出し、レシピの Web ページのスナップショットも表示する**

Specify the `extract.recipe` method and the `none` fallback. Also send an `img` element with the `data-render-src` attribute set to the recipe URL. extract.recipe メソッドを指定し、既定の表示フォールバックを使用します。API がコンテンツを抽出できない場合は、代わりにレシピの Web ページのスナップショットを表示します。

このシナリオの場合が情報量が最も多くなる可能性があります。Web ページには、お客様のレビューや提案などの追加情報が含まれる場合があるためです。

```html 
<div
    data-render-src="http://allrecipes.com/recipe/guacamole/"
    data-render-method="extract.recipe"
    data-render-fallback="none">
</div>
<img data-render-src="http://allrecipes.com/recipe/guacamole/" />
```
 
<br />レシピ情報を抽出する。抽出に失敗した場合のみ レシピの Web ページのスナップショットも表示する

Specify the `extract.recipe` method and use the default render fallback. extract.recipe メソッドを指定し、既定の表示フォールバックを使用します。API がコンテンツを抽出できない場合は、代わりにレシピの Web ページのスナップショットを表示します。

```html  
<div
    data-render-src="http://www.foodnetwork.com/recipes/alton-brown/creme-brulee-recipe.html"
    data-render-method="extract.recipe">
</div>
```

<br />レシピ情報を抽出し、そのレシピへのリンクも表示する

Specify the `extract.recipe` method and the `none` fallback. extract.recipe`a` メソッドと none`src` フォールバックを指定します。また、レシピの URL に設定された a 属性を src 要素に指定して送信します (または、ページに追加する他の情報を送信できます)。この API がコンテンツを抽出できない場合は、レシピのリンクのみが表示されます。 If the API is unable to extract any content, only the recipe link is rendered.

```html  
<div
    data-render-src="http://www.seriouseats.com/recipes/2014/09/diy-spicy-kimchi-beef-instant-noodles-recipe.html"
    data-render-method="extract.recipe"
    data-render-fallback="none">
</div>
<a href="http://www.seriouseats.com/recipes/2014/09/diy-spicy-kimchi-beef-instant-noodles-recipe.html">Recipe URL</a>
``` 


<a name="product"></a>
## 製品リストの抽出

<table role="presentation" id="simpletable">
<tr>
<td id="simplecell">タイトル<br /><br />評価<br /><br />主要画像<br /><br />説明<br /><br />機能<br /><br />仕様</td>
<td id="simplecell" class="rightalign">![An example product listing extraction.](images\onenote\product-extraction.png)</td>
</tr>
</table>

この API は、Amazon.com や *HomeDepot.com* など、数多くの人気サイトの製品用に最適化されています。

### レシピ抽出の一般的なシナリオ

**製品情報を抽出し、製品の Web のスナップショットも表示する**

Specify the `extract.product` method and the `none` fallback. Also send an `img` element with the `data-render-src` attribute set to the product URL. extract.product メソッドを指定し、既定の表示フォールバックを使用します。API がコンテンツを抽出できない場合は、代わりに製品の Web ページのスナップショットを表示します。

このシナリオの場合が情報量が最も多くなる可能性があります。Web ページには、お客様のレビューや提案などの追加情報が含まれる場合があるためです。

```html 
<div
    data-render-src="http://www.amazon.com/Microsoft-Band-Small/dp/B00P2T2WVO"
    data-render-method="extract.product"
    data-render-fallback="none">
</div>
<img data-render-src="http://www.amazon.com/Microsoft-Band-Small/dp/B00P2T2WVO" />
```

<br />製品情報を抽出する。抽出に失敗した場合のみ製品の Web ページのスナップショットも表示する

Specify the `extract.product` method and use the default render fallback. extract.product メソッドを指定し、既定の表示フォールバックを使用します。API がコンテンツを抽出できない場合は、代わりに製品の Web ページのスナップショットを表示します。

```html 
<div
    data-render-src="http://www.sears.com/craftsman-19hp-42-8221-turn-tight-174-hydrostatic-yard-tractor/p-07120381000P"
    data-render-method="extract.product">
</div>
```
 
<br />製品情報を抽出し、その製品へのリンクも表示する

Specify the `extract.product` method and the `none` fallback. extract.product`a` メソッドと none`src` フォールバックを指定します。また、製品の URL に設定された a 属性を src 要素に指定して送信します (または、ページに追加する他の情報を送信できます)。この API がコンテンツを抽出できない場合は、製品のリンクのみが表示されます。 If the API is unable to extract any content, only the page link is rendered.

```html 
<div
    data-render-src="http://www.homedepot.com/p/Active-Ventilation-5-Watt-Solar-Powered-Exhaust-Attic-Fan-RBSF-8-WT/204203001"
    data-render-method="extract.product"
    data-render-fallback="none">
</div>
<a href="http://www.homedepot.com/p/Active-Ventilation-5-Watt-Solar-Powered-Exhaust-Attic-Fan-RBSF-8-WT/204203001">Product URL</a>
```


<a name="unknown"></a> 
## コンテンツの不明な種類の抽出

送信するコンテンツの種類 (名刺、レシピ、製品) がわからない場合、修飾されていない extract`extract` メソッドを使用して、OneNote API が対象種類を自動検出するようにできます。この方法は、使用しているアプリが送信するキャプチャの種類が異なる場合に使用できます。 You might want to do this if your app sends different capture types.

>>送信するコンテンツの種類がわかっている場合は、extract.businesscard、extract.recipe、または extract.product メソッドを使用します。これは抽出結果の最適化に役立つ場合があります。 In some cases, this can help to optimize the extraction results.
 
### 不明な抽出の一般的なシナリオ

**画像または URL を送信する。抽出に失敗した場合は指定された画像または Web ページのスナップショットを表示する**

extract`extract` メソッドを指定します。これにより、API が自動的にコンテンツの種類を検出し、既定の表示フォールバックを使用します。API がコンテンツを抽出できない場合は、代わりに指定された画像または Web ページのスナップショットを表示します。 extract メソッドを指定します。これにより、API が自動的にコンテンツの種類を検出し、既定の表示フォールバックを使用します。API がコンテンツを抽出できない場合は、代わりに指定された画像または Web ページのスナップショットを表示します。

```html 
<div
    data-render-src="some image or url"
    data-render-method="extract">
</div>
```


<a name="request-response-info"></a>
## 応答情報

| 応答データ | 説明 |  
|------|------|  
| 成功コード | 成功した POST 要求に対しては 201 HTTP ステータス コード、成功した PATCH 要求に対しては 204 HTTP ステータス コードが戻ります。 |  
| エラー | <p>抽出が失敗する場合、この API は可能な限り多くの要求を処理し、応答本文の **api.diagnostics** プロパティで 20136 警告コードを戻します。抽出は以下の場合には失敗します。 The extraction will fail if:<br /> 必須の data-render-src`data-render-src` 属性または data-render-method`data-render-method` 属性がありません。<br /> - The `data-render-src`, `data-render-method`, or `data-fallback-method` values are empty or invalid.</p><p>この API は対象のコンテンツが使用可能な場合であってもその一部しか抽出できないことがあります。その場合、サービスは可能な限り多くの要求を処理しますが、警告は戻しません。</p> |  
| X-CorrelationId ヘッダー | 要求を一意に識別する GUID。Microsoft サポートと問題のトラブルシューティングを行うときに、この値を Date ヘッダーの値とともに使用できます。 |  


<a name="permissions"></a>
## アクセス許可

OneNote ページを作成または更新するには、適切なアクセス許可を要求する必要があります。アプリの動作に必要な最低限のアクセス許可を選択してください。

**_POST pages_ のアクセス許可**

[!INCLUDE [Create perms](../includes/onenote/create-perms.md)] 

**_PATCH pages_ のアクセス許可**

[!INCLUDE [Update perms](../includes/onenote/update-perms.md)]

アクセス許可のスコープと動作のしくみの詳細については、「[OneNote のアクセス許可のスコープ](../howto/onenote-auth.md)」を参照してください。


<a name="see-also"></a>
## その他の技術情報

- [OneNote ページの作成](../howto/onenote-create-page.md)
- [OneNote ページ コンテンツを更新する](../howto/onenote-update-page.md)
- [画像とファイルを追加する](../howto/onenote-images-files.md)
- [OneNote 開発](../howto/onenote-landing.md)
- [OneNote デベロッパー センター](http://dev.onenote.com/)
- [OneNote の開発者ブログ](http://go.microsoft.com/fwlink/?LinkID=390183)
- [スタック オーバーフローに関する OneNote の開発の質問](http://go.microsoft.com/fwlink/?LinkID=390182)
- [OneNote GitHub のリポジトリ](http://go.microsoft.com/fwlink/?LinkID=390178)  


