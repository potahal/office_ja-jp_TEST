---
ms.Toctitle: Use the save dialog
title: "Web ページで OneNote の保存ダイアログを使用する" 
description: "Web コンテンツを onnvshort のノートブックにすばやく簡単に保存できる onnvshort の保存ダイアログの使用方法について説明します。"
ms.ContentId: 919b5d94-5f13-44a0-8ad0-84595c1beb93
ms.date: November 18, 2015

---
[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]

<style>##indent {margin:2px 0px 0px 25px;}</style>

# Web ページで OneNote の保存ダイアログを使用する

__**適用対象:**OneDrive のコンシューマー ノートブックのみ__

The OneNote save dialog makes it easy for web developers to send webpages to OneNote. OneNote の保存ダイアログは、Web 開発者が容易に Web ページを OneNote に送信できるようにします。必要なパラメーターを指定した URL を埋め込むだけで、保存ダイアログにより認証が行われ、転送先を選択するプロンプトがユーザーに表示されます。  
 authenticating and prompts the user to choose a destination.

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

2. [このリスト] リンクをクリックします。 リンクをクリックします。Microsoft アカウントを使用して認証を行うと、次のダイアログ ボックスが表示されます。

    ![図 1. OneNote の保存ダイアログ](images\onenote\OneNoteSaveDialog.png)
    
3. ページを保存するノートブックとセクションを選択して、**[クリップ]** をクリックします。ダイアログにより、OneNote Dev Center ページが保存されます。

保存ダイアログは、サイズが 525 x 525 ピクセルのウィンドウで起動することをお勧めします。OneNote のロゴを使用するボタンを作成する場合は、設計に役立つ「[ブランドのガイドライン](../howto/onenote-branding.md)」を参照してください。

次のセクションでは、リンクで使用される URL を分割して、独自の URL を作成する方法について説明します。

## 保存ダイアログの URL を構築する
 
OneNote の保存ダイアログを起動するためのベース URL は https://www.onenote.com/clipper/save`https://www.onenote.com/clipper/save` です。

これは、各クエリ文字列パラメーターをプレースホルダーにすると、完全な URL がどのようになるのかを示しています。

```
https://www.onenote.com/clipper/save?sourceUrl={url}&imgUrl={url}&title={text}&description={text}&notes={text}`
```

次の表は、各クエリ文字列パラメーターについて説明しています。**sourceUrl** パラメーター以外は必要ありませんが、優れたユーザー エクスペリエンスを提供するために、**imgUrl** パラメーター、**title** パラメーター、および **description** パラメーターも設定します。

| パラメーター | 説明 |  
|------|------|  
| sourceUrl | 必須<br /><br />The URL of the webpage that you want to send to OneNote. 必須OneNote に送信する Web ページの URL。このページのスクリーン ショットは、ユーザーが選択したノートブックとセクションに追加されます。 |  
| imgUrl | おすすめグラフ<br /><br />An image that appears in the upper-left corner of the save dialog after the user has authenticated. 推奨ユーザーの認証後、保存ダイアログの左上隅に表示されるイメージ。このイメージは、Web ページのプレビュー、またはユーザーがターゲットのページを確認するための目印を提供する必要があります。このイメージは OneNote には送信されません。 This image isn't sent to OneNote. |  
| title | おすすめグラフ<br /><br />ページのタイトル。 |  
| description | おすすめグラフ<br /><br />値についての説明です。 推奨Web ページの説明。**imgUrl** パラメーターに渡されるイメージと同様に、このテキストもユーザーがページを確認するために役立ちます。このテキストはダイアログに表示されますが、OneNote には送信されません。90 文字を超える説明は切り捨てられます。 This text appears in the dialog but isn't sent to OneNote. Descriptions truncate after 90 characters. |  
| notes | 省略可<br /><br />リンクに関するメモを指定します。 This prepopulates the text box in the dialog. 省略可能Web ページに関するメモ。これはダイアログのテキスト ボックスに事前に設定されます。ユーザーはダイアログにメモを追加してからページを送信できます。メモは OneNote に表示されます。 Notes are shown in OneNote. |  
 

<a name="see-also"></a>
## その他の技術情報

- [OneNote 開発](../howto/onenote-landing.md)
- [OneNote デベロッパー センター](http://dev.onenote.com/)
- [OneNote の開発者ブログ](http://go.microsoft.com/fwlink/?LinkID=390183)
- [スタック オーバーフローに関する OneNote の開発の質問](http://go.microsoft.com/fwlink/?LinkID=390182) 
- [OneNote GitHub のリポジトリ](http://go.microsoft.com/fwlink/?LinkID=390178)  
