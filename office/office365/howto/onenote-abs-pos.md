---
ms.Toctitle: Create absolute positioned elements
title: "絶対的な位置要素を作成する" 
description: "ms.TocTitle:絶対配置要素を作成するTitle:絶対配置要素を作成するDescription:独立して配置できる div、img、および object 要素を含む OneNote ページを作成します。ms.ContentId:34933340-b850-49ac-a25a-8cfa12cd14ffms.topic: 記事 (方法) ms.date:2015 年 11 月 30 日"
ms.ContentId: 34933340-b850-49ac-a25a-8cfa12cd14ff
ms.date: November 30, 2015
---

[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]
[!INCLUDE [Add the ONAPI repo styles](../includes/controls/addonapistyles.xml)]

# 絶対的な位置要素を作成する

*__Applies to:__ Consumer notebooks on OneDrive | Enterprise notebooks on Office 365*

OneNote ページの本文には、直接の div、img、および object 子要素を複数含めることができます。これらの要素はページ上に独立して配置できます。

<a name="attributes"></a>
## 属性と配置動作

次に示すように、data-absolute-enabled 属性および  style  属性を使用して、ページ上に絶対配置要素を作成します。

- The body element must specify `data-absolute-enabled="true"`. body 要素は data-absolute-enabled="true"`false` を指定する必要があります。省略したり false`_default` に設定したりすると、API が作成した絶対位置で配置された _default の内側にすべての本文のコンテンツが表示され、すべての位置設定が無視されます。

- div、img、および object 要素のみが絶対配置要素になります。 

- 絶対配置要素は style="position:absolute"`style="position:absolute"` を指定する必要があります。

- Absolute positioned elements must be direct children of the `body` element. 絶対配置要素は必ず body 要素の直接の子要素になります。本文の直接の子要素が絶対配置でない div、img、または object 要素 の場合は、絶対配置の _default div の内側に静的コンテンツとして表示されます。

- 絶対配置要素は、指定された上と左の座標 (開始位置 0:0 を基準とした、タイトル領域の上部でページの左隅) に配置されます。

- If an absolute positioned element omits the top or left coordinate, the missing coordinate is set to its default value: `top:120px` or `left:48px`. These default coordinates specify a position just below the title area. Be aware that omitting coordinates can result in elements that are stacked on top of each other.

- 絶対配置要素は、入れ子状態にしたり、定位置要素を含めたりすることはできません。API は、絶対配置の div 内の入れ子型の要素で指定された位置設定をすべて無視し、絶対配置の親 div 内の入れ子型のコンテンツを表示するとともに、応答の **api.diagnostics** プロパティで警告を返します。

<br />例:次の例には、直接の子 p、絶対配置 div、および非絶対配置 div が含まれています。

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

The API renders the non-absolute positioned div in the default div. API は既定の div に非絶対配置 div を表示します。入れ子になった <c><div></c> タグは意味情報 (<c>data-id</c> など) を定義しないため、破棄されます。

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

<br />入力 HTML 

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

![Resulting page with absolute positioned div and image](images\onenote\abs-pos.png)

Notice the changes to the non-contributing, nested div from the input HTML. 入力 HTML から変更された、作用していない入れ子になった div に注意してください。API は div のコンテンツを保持しますが、<c><div></c> タグは破棄します。これは、div が意味情報 (<c>data-id</c> など) を定義しないためです。

OneNote API が入力 HTML と出力 HTML を処理する方法の詳細については、「[OneNote ページの入力 HTML と出力 HTML](../howto/onenote-input-output-html.md)」を参照してください。

<a name="style-attributes"></a>
### サポートされている CSS スタイルの属性

絶対配置要素は、上と左の座標を指定できます。div とイメージでは幅を指定でき、イメージでは高さも指定できます。たとえば、次のようになります。

```html
<img style="position:absolute;top:140px;left:95px;width:480px;height:665px" src="..." />
```

| 属性 | サポートされる要素 | 説明 |  
|:------|:------|:------|  
| top | div、img、object | 要素の上部境界線の Y 軸座標 (ピクセル単位のみ)。既定では 120 ピクセル。例: top:140px Default is 120 pixels.<p>例`top:140px`</p> |  
| left |  div、img、object  | 要素の上部境界線の X 軸座標 (ピクセル単位のみ)。既定では 48 ピクセル。例: left:95px Default is 48 pixels.<p>例`left:95px`</p> |  
| width |  div、img  | 要素の幅 (ピクセル単位のみ)。例: width:480px<p>例`width:480px`</p> |  
| height | img | The height of the element, in pixels only. 要素の高さ (ピクセル単位のみ)。div の場合、高さは実行時に計算され、指定した高さの値はすべて無視されます。例: height:665px<p>例`height:665px`</p> |  
 
z-index`z-index` など、その他の位置属性は無視されます。 その他の位置属性、たとえば z-index`data-render-src` などは無視されます。絶対配置のイメージは、data-render-src`src` または src のいずれかを使用できます。


<a name="request-response-info"></a>
## 応答情報
OneNote API は、次の情報を応答で返します。

| 応答データ | 説明 |  
|:------|:------|  
| 成功コード | 成功した POST 要求に対しては 201 HTTP ステータス コード、成功した PATCH 要求に対しては 204 HTTP ステータス コードが戻ります。 |  
| エラー | <p>次のいずれかの条件により、応答の **api.diagnostics** プロパティに警告が表示されます。</p><ul><li>要素で style="position:absolute" 属性が指定されていますが、この body 要素には位置指定をサポートするために必要な data-absolute-enabled="true" が指定されていません。すべての位置設定は無視されます。 All position settings are ignored.</li><li>The `style="position:absolute"` attribute is specified on an element that is not a direct child of the body element. If the element is a `div`, `img`, or `object`, make it a direct child of the body; otherwise the position settings will be ignored.</li><li>The `style="position:absolute"` attribute is specified on an element is not a `div`, `img`, and ``object` element.</li></ul> |  
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
- [OneNote 開発](../howto/onenote-landing.md)
- [OneNote デベロッパー センター](http://dev.onenote.com/)
- [OneNote の開発者ブログ](http://go.microsoft.com/fwlink/?LinkID=390183)
- [スタック オーバーフローに関する OneNote の開発の質問](http://go.microsoft.com/fwlink/?LinkID=390182)
- [OneNote GitHub のリポジトリ](http://go.microsoft.com/fwlink/?LinkID=390178)  


