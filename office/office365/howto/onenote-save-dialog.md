---
ms.Toctitle: Use the save dialog
title: "OneNote の保存ダイアログを使用する" 
description: "OneNote の保存ダイアログを使用する方法について説明します。この方法で、すばやく簡単に Web コンテンツを OneNote に送信できます。"
ms.ContentId: 919b5d94-5f13-44a0-8ad0-84595c1beb93
ms.date: November 18, 2015

---
[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]

<style>#indent {margin:2px 0px 0px 25px;}</style>

# Web ページで OneNote 保存ダイアログを使用する

_**適用対象:**OneDrive のコンシューマー ノートブックのみ_

OneNote の保存ダイアログを使用すると、Web 開発者は簡単に Web ページを OneNote に送信できるようになります。 必要なパラメーターを指定した URL を埋め込むだけで、保存ダイアログにより認証が行われ、  
 転送先を選択するプロンプトがユーザーに表示されます。

## 保存ダイアログをプレビューする

OneNote の保存ダイアログが実行される様子を見るには、次の手順に従います。
 
1. 次のアンカー タグを自分の Web ページの HTML にコピーします。

    ```html
<a href="https://www.onenote.com/clipper/save?sourceUrl=http://dev.onenote.com/&
imgUrl=http://antyapps.pl/wp-content/uploads/2013/09/onenote-logo-630x347.jpg&
title=Use the OneNote save dialog on your webpages&
description=It's easy to send web content to OneNote with the OneNote save dialog!&
notes=Sending the OneNote Dev Center webpage to OneNote."
onclick="window.open(this.href, 'targetWindow', 'width=525, height=525'); return false;">
Try the OneNote save dialog</a>
    ```

2. リンクをクリックします。 Microsoft アカウントを使用して認証を行うと、次のダイアログ ボックスが表示されます。

    ![OneNote の保存ダイアログ](images\onenote\OneNoteSaveDialog.png)
    
3. ページを保存するノートブックとセクションを選択して、**[クリップ]** をクリックします。ダイアログにより、OneNote Dev Center ページが保存されます。

保存ダイアログは、サイズが 525 x 525 ピクセルのウィンドウで起動することをお勧めします。OneNote のロゴを使用するボタンを作成する場合は、設計に役立つ「[ブランドのガイドライン](../howto/onenote-branding.md)」を参照してください。

次のセクションでは、リンクで使用される URL を分析して、独自の URL を作成する方法について説明します。

## 保存ダイアログの URL を構築する
 
OneNote の保存ダイアログを起動するためのベース URL は `https://www.onenote.com/clipper/save` です。

これは、各クエリ文字列パラメーターをプレースホルダーにすると、完全な URL がどのようになるのかを示しています。

```
https://www.onenote.com/clipper/save?sourceUrl={url}&imgUrl={url}&title={text}&description={text}&notes={text}`
```

次の表は、各クエリ文字列パラメーターについて説明しています。**sourceUrl** パラメーター以外は必要ありませんが、優れたユーザー エクスペリエンスを提供するために、**imgUrl** パラメーター、**title** パラメーター、および **description** パラメーターも設定します。

| パラメーター | 説明 |  
|------|------|  
| sourceUrl | 必須<br /><br />OneNote に送信する Web ページの URL。 このページのスクリーン ショットは、ユーザーが選択したノートブックとセクションに追加されます。 |  
| imgUrl | 推奨<br /><br />ユーザーの認証後、保存ダイアログの左上隅に表示されるイメージ。 このイメージにより、Web ページのプレビュー、またはユーザーがターゲットのページを確認するための目印を提供します。 このイメージは OneNote には送信されません。 |  
| title | 推奨<br /><br />OneNote ページのタイトル。 |  
| description | 推奨<br /><br />Web ページの説明。 **imgUrl** パラメーターに渡されるイメージと同様に、このテキストもユーザーがページを確認するために役立ちます。 このテキストはダイアログに表示されますが、OneNote には送信されません。 90 文字を超える説明は切り捨てられます。 |  
| notes | 省略可<br /><br />Web ページに関するメモ。 ダイアログのテキスト ボックスに自動入力されます。 ページを送信する前に、ユーザーはダイアログでメモを追加することもできます。 メモは、OneNote に表示されます。 |  
 

<a name="see-also"></a>
## その他のリソース

- [OneNote の開発](../howto/onenote-landing.md)
- [OneNote デベロッパー センター](http://dev.onenote.com/)
- [OneNote の開発者ブログ](http://go.microsoft.com/fwlink/?LinkID=390183)
- [スタック オーバーフローに関する OneNote の開発の質問](http://go.microsoft.com/fwlink/?LinkID=390182) 
- [OneNote GitHub のリポジトリ](http://go.microsoft.com/fwlink/?LinkID=390178)  
