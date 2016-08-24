---
ms.Toctitle: Extract data
title: "キャプチャからデータを抽出する" 
description: "OneNote API を使用して、名刺やオンライン レシピ、オンラインの製品リストの内容を変換して拡張する方法について説明します。"
ms.ContentId: 901068e3-1d4d-4233-856b-5a2a71cc58c2
ms.date: January 19, 2016
---

[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]
[!INCLUDE [Add the ONAPI repo styles](../includes/controls/addonapistyles.xml)]

<style>#simpletable {margin:-5px 0px 0px 0px; border:none;} #simplecell {border:none; padding:15px 20px; background-color:white;} .rightalign {text-align:right; padding:0px 20px 0px 0px;}</style>

# キャプチャからデータを抽出する

*__適用対象:__OneDrive のコンシューマー ノートブック | Office 365 のエンタープライズ ノートブック*

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

実行する抽出メソッド。 必須。

| 値 | 説明 |
|:------|:------|
| extract.businesscard | 名刺の抽出。 |
| extract.recipe | レシピの抽出。 |
| extract.product | 製品リストの抽出。 |
| extract | 不明な種類の抽出。 |

最適の結果を得るために、コンテンツの種類 (`extract.businesscard`、`extract.recipe`、または `extract.product`) を指定してください (種類がわかっている場合)。 種類が不明の場合は、`extract` メソッドを使用してください。OneNote API は種類の自動検出を試行するようになります。

**data-render-fallback**

抽出できない場合のフォールバック動作。 省略すると、既定の **render** になります。 

| 値 | 説明 |
|:------|:------|
| render | レシピまたは製品の Web ページのソース画像またはスナップショットを表示します。 |
| なし | 何も実行しません。<br />このオプションは、抽出コンテンツに加えて、名刺または Web ページのスナップショットをページに必ず含める場合に役立ちます。 例に示すように、要求には個別の `img` 要素を含めてください。 |

<a name="biz-card"></a>
## 名刺の抽出

OneNote API は、個人または会社の名刺の画像に基づいて、次の連絡先情報の検索および表示を試行します。

<table role="presentation" id="simpletable">
<tr>
<td id="simplecell">名前<br /><br />タイトル<br /><br />組織<br /><br />電話および FAX 番号<br /><br />郵送先および実際の住所<br /><br />電子メール アドレス<br /><br />Web サイト<br /></td>
<td id="simplecell" class="rightalign">![名刺抽出の例。](images\onenote\biz-card-extraction.png)</td>
</tr>
</table>

ページには、抽出された連絡先情報が含まれる vCard (.VCF ファイル) も組み込まれます。この vCard は、ページの HTML コンテンツを取得するときに連絡先情報を入手するのに便利な方法です。

### 名刺抽出の一般的なシナリオ

**名刺情報を抽出して、名刺の画像を表示する**

`extract.businesscard` メソッドと `none` フォールバックを指定します。 また、画像を参照する `src` 属性を含む `img` 要素も送信します。 API ではコンテンツを抽出できない場合は、名刺の画像のみが表示されます。

```html 
<div
    data-render-src="name:scanned-card-image"
    data-render-method="extract.businesscard"
    data-render-fallback="none">
</div>
<img src="name:scanned-card-image" />
```

<br />
**名刺情報を抽出して、抽出に失敗した場合のみ名刺の画像を表示する**

`extract.businesscard` メソッドを指定して、既定の `render` フォールバックを使用します。 API ではコンテンツを抽出できない場合は、その代わりに名刺の画像が表示されます。

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
<td id="simplecell">完成例の画像<br /><br />評価<br /><br />材料<br /><br />手順<br /><br />準備、調理、および合計の時間<br /><br />分量</td>
<td id="simplecell" class="rightalign">![レシピの抽出例。](images\onenote\recipe-extraction.png)</td>
</tr>
</table>

この API は、*Allrecipes.com*、*FoodNetwork.com*、および *SeriousEats.com* など、数多くの人気サイトのレシピ用に最適化されています。

### レシピ抽出の一般的なシナリオ

**レシピ情報を抽出して、レシピの Web ページのスナップショットも表示する**

`extract.recipe` メソッドと `none` フォールバックを指定します。 また、レシピの URL に設定した `data-render-src` 属性を含む `img` 要素も送信します。 API ではコンテンツを抽出できない場合は、レシピの Web ページのスナップショットのみが表示されます。

おそらく、このシナリオが最も多くの情報を提供することになります。Web ページには、お客様のレビューや提案などの追加情報が含まれている場合があるためです。

```html 
<div
    data-render-src="http://allrecipes.com/recipe/guacamole/"
    data-render-method="extract.recipe"
    data-render-fallback="none">
</div>
<img data-render-src="http://allrecipes.com/recipe/guacamole/" />
```
 
<br />
**レシピ情報を抽出して、抽出に失敗した場合にのみレシピの Web ページのスナップショットを表示する**

`extract.recipe` メソッドを指定して、既定の render フォールバックを使用します。 API ではコンテンツを抽出できない場合は、その代わりにレシピの Web ページのスナップショットが表示されます。

```html  
<div
    data-render-src="http://www.foodnetwork.com/recipes/alton-brown/creme-brulee-recipe.html"
    data-render-method="extract.recipe">
</div>
```

<br />
**レシピ情報を抽出して、そのレシピへのリンクも表示する**

`extract.recipe` メソッドと `none` フォールバックを指定します。 レシピの URL に設定された `src` 属性を含む `a` 要素も送信します (または、ページに追加するその他の情報を送信することもできます)。 API ではコンテンツを抽出できない場合は、レシピのリンクのみが表示されます。

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
<td id="simplecell" class="rightalign">![製品リスト抽出の例。](images\onenote\product-extraction.png)</td>
</tr>
</table>

この API は、*Amazon.com* や *HomeDepot.com* など、数多くの人気サイトの製品用に最適化されています。

### レシピ抽出の一般的なシナリオ

**製品情報を抽出して、製品の Web のスナップショットも表示する**

`extract.product` メソッドと `none` フォールバックを指定します。 また、製品の URL に設定した `data-render-src` 属性を含む `img` 要素も送信します。 API ではコンテンツを抽出できない場合は、製品の Web ページのスナップショットのみが表示されます。

おそらく、このシナリオが最も多くの情報を提供することになります。Web ページには、お客様のレビューや提案などの追加情報が含まれている場合があるためです。

```html 
<div
    data-render-src="http://www.amazon.com/Microsoft-Band-Small/dp/B00P2T2WVO"
    data-render-method="extract.product"
    data-render-fallback="none">
</div>
<img data-render-src="http://www.amazon.com/Microsoft-Band-Small/dp/B00P2T2WVO" />
```

<br />
**製品情報を抽出して、抽出に失敗した場合のみ製品の Web ページのスナップショットも表示する**

`extract.product` メソッドを指定して、既定の render フォールバックを使用します。 API ではコンテンツを抽出できない場合は、その代わりに製品の Web ページのスナップショットが表示されます。

```html 
<div
    data-render-src="http://www.sears.com/craftsman-19hp-42-8221-turn-tight-174-hydrostatic-yard-tractor/p-07120381000P"
    data-render-method="extract.product">
</div>
```
 
<br />
**製品情報を抽出して、その製品へのリンクも表示する**

`extract.product` メソッドと `none` フォールバックを指定します。 製品の URL に設定された `src` 属性を含む `a` 要素も送信します (または、ページに追加するその他の情報を送信することもできます)。 API ではコンテンツを抽出できない場合は、ページのリンクのみが表示されます。

```html 
<div
    data-render-src="http://www.homedepot.com/p/Active-Ventilation-5-Watt-Solar-Powered-Exhaust-Attic-Fan-RBSF-8-WT/204203001"
    data-render-method="extract.product"
    data-render-fallback="none">
</div>
<a href="http://www.homedepot.com/p/Active-Ventilation-5-Watt-Solar-Powered-Exhaust-Attic-Fan-RBSF-8-WT/204203001">Product URL</a>
```


<a name="unknown"></a> 
## 不明な種類のコンテンツの抽出

送信するコンテンツの種類 (名刺、レシピ、製品) がわからない場合、修飾されていない `extract` メソッドを使用して、OneNote API が対象種類を自動検出するようにできます。 この方法は、目的のアプリが各種のキャプチャを送信する場合に使用できます。

>送信するコンテンツの種類がわかっている場合は、`extract.businesscard`、`extract.recipe`、または `extract.product` メソッドを使用してください。 これにより、最適な抽出結果が得られるようになることがあります。
 
### 不明な抽出の一般的なシナリオ

**画像または URL を送信して、抽出に失敗した場合は指定された画像または Web ページのスナップショットを表示する**

API が自動的にコンテンツの種類を検出するように `extract` メソッドを指定して、既定の render フォールバックを使用します。 API ではコンテンツを抽出できない場合は、その代わりに指定された画像または Web ページのスナップショットが表示されます。

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
| エラーまたは警告 | <p>抽出に失敗した場合、この API は可能な限り多くの要求を処理し、応答本文の **@api.diagnostics** プロパティに 20136 警告コードを返します。 次の場合に抽出は失敗します。<br /> - 必須の `data-render-src` 属性または `data-render-method` 属性が欠落している。<br /> - `data-render-src`、`data-render-method`、または `data-fallback-method` の値が空であるか無効である。</p><p>この API は対象のコンテンツが使用可能な場合であってもその一部しか抽出できないことがあります。その場合、サービスは可能な限り多くの要求を処理しますが、警告は戻しません。</p> |  
| X-CorrelationId ヘッダー | 要求を一意に識別する GUID。Microsoft サポートと問題のトラブルシューティングを行う際に、この値を Date ヘッダーの値とともに使用できます。 |  


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

- [OneNote ページの作成](../howto/onenote-create-page.md)
- [OneNote ページ コンテンツを更新する](../howto/onenote-update-page.md)
- [画像とファイルを追加する](../howto/onenote-images-files.md)
- [OneNote の開発](../howto/onenote-landing.md)
- [OneNote デベロッパー センター](http://dev.onenote.com/)
- [OneNote の開発者ブログ](http://go.microsoft.com/fwlink/?LinkID=390183)
- [スタック オーバーフローに関する OneNote の開発の質問](http://go.microsoft.com/fwlink/?LinkID=390182)
- [OneNote GitHub のリポジトリ](http://go.microsoft.com/fwlink/?LinkID=390178)  


