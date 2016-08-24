---
ms.Toctitle: Create absolute positioned elements
title: "絶対配置要素を作成する" 
description: "独立して配置できる div、img、object 要素を含む OneNote ページを作成します。"
ms.ContentId: 34933340-b850-49ac-a25a-8cfa12cd14ff
ms.date: November 30, 2015
---

[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]
[!INCLUDE [Add the ONAPI repo styles](../includes/controls/addonapistyles.xml)]

# 絶対配置要素を作成する

*__適用対象:__OneDrive のコンシューマー ノートブック | Office 365 のエンタープライズ ノートブック*

OneNote ページの本文には、直接の子要素 `div`、`img`、`object` を複数含めることができます。これらの要素はページ上に独立して配置できます。

<a name="attributes"></a>
## 属性と配置動作

次に示すように、`data-absolute-enabled` 属性および [`style`](#style-attributes) 属性を使用して、ページ上に絶対配置要素を作成します。

- body 要素は、`data-absolute-enabled="true"` を指定する必要があります。 省略したり `false` に設定したりすると、API によって作成される `_default` 絶対配置 div の内側にすべての本文のコンテンツが表示され、すべての位置設定が無視されます。

- `div`、`img`、および `object` 要素のみが絶対配置要素になります。 

- 絶対配置要素は、`style="position:absolute"` を指定する必要があります。

- 絶対配置要素は、`body` 要素の直接の子にする必要があります。 body の直接の子要素が絶対配置でない `div`、`img`、または `object` 要素の場合は、絶対配置の `_default` div の内側に静的コンテンツとして表示されます。

- 絶対配置要素は、指定された上と左の座標 (開始位置 0:0 を基準とした、タイトル領域の上部でページの左隅) に配置されます。

- 絶対配置要素が top または left の座標を省略していると、不足している座標には既定値の `top:120px` または `left:48px` が設定されます。 これらの既定の座標は、title エリアの直下の位置を指定します。 座標を省略すると、複数の要素の上部が積み重なることがある点に注意してください。

- 絶対配置要素は、入れ子状態にしたり、定位置要素を含めたりすることはできません。API は、絶対配置の div 内の入れ子型の要素で指定された位置設定をすべて無視し、絶対配置の親 div 内の入れ子型のコンテンツを表示するとともに、応答の **api.diagnostics** プロパティで警告を返します。

<br />
**例:**次の例には、直接の子 `p`、絶対配置 div、および非絶対配置 div が含まれています。

   **入力 HTML** 

   ```html 
   <body data-absolute-enabled="true">
       <p>This content will appear in the _default div.</p>
       <div style="position:absolute;top:175px;left:100px" data-id="div1">
         <p>This content will appear in an absolute positioned div.</p>
       </div>
       <div>
           <p>This content will also appear in the _default div.</p>
       </div>
   </body>
   ```

API は既定の div に非絶対配置 div を表示します。 入れ子になった `<div>` タグは意味情報 (`data-id` など) を定義しないため、破棄されます。

**出力 HTML**

   ```html 
   <body data-absolute-enabled="true" style="font-family:Calibri;font-size:11pt">
       <div data-id="_default" style="position:absolute;left:48px;top:120px;width:624px">
           <p>This content will appear in the _default div.</p>
           <p>This content will also appear in the _default div.</p>
       </div>
       <div data-id="div1" style="position:absolute;left:99px;top:174px;width:624px">
           <p>This content will appear in an absolute positioned div.</p>
       </div>
   </body>
   ```

**例:** 次の例では、絶対配置の div と絶対配置のイメージを 1 つずつ含むページを作成します。

<br />
**入力 HTML** 

```html 
<html>
    <head>
        <title>Page Title</title>
    </head>
    <body data-absolute-enabled="true">
        <div style="position:absolute;width:280px;top:120px;left:68px">
            <p>Some text</p>
            <img style="width:120px" src="http://officeimg.vo.msecnd.net/en-us/files/018/949/ZA103278226.png" />
            <div>
                <p>More text inside a regular, nested div</p>
            </div>
        </div>
        <img style="position:absolute;width:360px;top:350px;left:300px" src="http://officeimg.vo.msecnd.net/en-us/files/018/949/ZA103278226.png" />
    </body>
</html>
```
 
OneNote API は入力 HTML を評価し、OneNote によりサポートされるすべての意味内容と任意の意味情報を保持します。結果のページは、次に示すイメージのように表示されます (div とイメージの境界線は表示されません)。 

![絶対配置の div とイメージのある結果のページ](images\onenote\abs-pos.png)

入力 HTML から変更された、作用していない入れ子になった div に注意してください。 API は div のコンテンツを保持しますが、`<div>` タグは破棄します。これは、div は意味情報 (`data-id` など) を定義しないためです。

OneNote API が入力 HTML と出力 HTML を処理する方法の詳細については、「[OneNote ページの入力 HTML と出力 HTML](../howto/onenote-input-output-html.md)」を参照してください。

<a name="style-attributes"></a>
### サポートされている CSS スタイルの属性

絶対配置要素は、上と左の座標を指定できます。div とイメージでは幅を指定でき、イメージでは高さも指定できます。たとえば、次のようになります。

```html
<img style="position:absolute;top:140px;left:95px;width:480px;height:665px" src="..." />
```

| 属性 | サポートされる要素 | 説明 |  
|:------|:------|:------|  
| top | div、img、object | 要素の上部境界線の Y 軸座標 (ピクセル単位のみ)。 既定は 120 ピクセル。<p>例: `top:140px`</p> |  
| left |  div、img、object  | 要素の左境界線の X 軸座標 (ピクセル単位のみ)。 既定は 48 ピクセル。<p>例: `left:95px`</p> |  
| width |  div、img  | 要素の幅 (ピクセル単位のみ)。<p>例: `width:480px`</p> |  
| height | img | 要素の高さ (ピクセル単位のみ) div の場合、高さは実行時に計算されるため、指定された高さの値は無視されます。<p>例: `height:665px`</p> |  
 
その他の位置属性 (`z-index` など) は無視されます。 絶対配置のイメージは、`data-render-src` または `src` のいずれかを使用できます。


<a name="request-response-info"></a>
## 応答情報
OneNote API は、次の情報を応答で返します。

| 応答データ | 説明 |  
|:------|:------|  
| 成功コード | 成功した POST 要求に対しては 201 HTTP ステータス コード、成功した PATCH 要求に対しては 204 HTTP ステータス コードが戻ります。 |  
| エラーまたは警告 | <p>次に示すいずれかの条件が成立する場合は、応答の **api.diagnostics** プロパティで警告を受け取ることになります。</p><ul><li>要素に `style="position:absolute"` 属性が指定されているが、`body` 要素では `data-absolute-enabled="true"` を指定していない。 すべての位置設定が無視されます。</li><li>body 要素の直接の子でない要素に `style="position:absolute"` 属性が指定されている。 要素が `div`、`img`、または `object` の場合は、その要素を body の直接の子にします。それ以外の場合、位置設定は無視されます。</li><li>`style="position:absolute"` 要素が、`div`、`img`、および ``object` 要素ではない要素に指定されている。</li></ul> |  
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
- [OneNote の開発](../howto/onenote-landing.md)
- [OneNote デベロッパー センター](http://dev.onenote.com/)
- [OneNote の開発者ブログ](http://go.microsoft.com/fwlink/?LinkID=390183)
- [スタック オーバーフローに関する OneNote の開発の質問](http://go.microsoft.com/fwlink/?LinkID=390182)
- [OneNote GitHub のリポジトリ](http://go.microsoft.com/fwlink/?LinkID=390178)  


