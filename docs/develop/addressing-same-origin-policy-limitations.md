
# Office アドインにおける同一生成元ポリシーの制限への対処
この記事では、Office アドインにおいて、あるドメインから読み込まれたスクリプトで別のドメインのドキュメントのプロパティを取得または操作できないようにする、同一生成元ポリシーの制限に対処する方法について概要を説明します。

 _ **適用対象:** Access apps for SharePoint?| apps for Office?| Excel?| Office Add-ins?| Outlook?| PowerPoint?| Project?| Word_

ブラウザーによって適用される同一生成元ポリシーでは、あるドメインから読み込まれたスクリプトで別のドメインの Web ページのプロパティを取得または操作できないようにしています。つまり、既定で、要求された URL のドメインは現在の Web ページのドメインと同じである必要があります。たとえば、このポリシーを適用すると、あるドメインの Web ページから、そのページがホストされているドメインとは別のドメインに対して [XmlHttpRequest](http://www.w3.org/TR/XMLHttpRequest/) Web サービスを呼び出せません。

Office アドインはブラウザー コントロールでホストされるので、それらの Web ページで実行されるスクリプトにも同一生成元ポリシーが適用されます。

アドインを開発する際に、同一生成元ポリシーの適用に対処するには、次のようにします。

- JSON/P を使用して匿名アクセスする。 
    
- トークン ベースの認証スキームを使用してサーバーサイド スクリプトを実装する。
    
- クロス オリジン リソース共有 (CORS) を使用する。
    
- IFRAME および POST MESSAGE を使用して独自のプロキシを作成する。
    

## JSON/P を使用した匿名アクセス


この制限に対処する方法の 1 つは、JSON/P を使用して Web サービスのプロキシを提供することです。これを行うためには、任意のドメインでホストされているスクリプトを参照する  **src** 属性を持つ **script** タグを使用します。 **script** タグをプログラムで作成して、 **src** 属性で参照する URL を動的に作成すると、URI クエリ パラメーターを介してパラメーターを URL に渡すことができます。Web サービス プロバイダーは、固有の URL で JavaScript コードを作成およびホストし、URI クエリ パラメーターに応じて異なるスクリプトを返します。それらのスクリプトは挿入された場所で実行され、想定どおりに動作します。

いずれの Office アドインでも機能する手法を使用する JSON/P の例を以下に示します。




```
// Dynamically create an HTML SCRIPT element that obtains the details for the specified video.
function loadVideoDetails(videoIndex) {
    // Dynamically create a new HTML SCRIPT element in the webpage.
    var script = document.createElement("script");
    // Specify the URL to retrieve the indicated video from a feed of a current list of videos,
    // as the value of the src attribute of the SCRIPT element. 
    script.setAttribute("src", "https://gdata.youtube.com/feeds/api/videos/" + 
        videos[videoIndex].Id + "?alt=json-in-script&amp;callback=videoDetailsLoaded");
    // Insert the SCRIPT element at the end of the HEAD section.
    document.getElementsByTagName('head')[0].appendChild(script);
}

```


## トークン ベースの認証スキームを使用するサーバーサイド スクリプトの実装


同一生成元ポリシーの制限に対処する他の方法として、OAuth を使用する ASP ページ、または Cookie で資格情報をキャッシュする ASP ページとして、アドインの Web ページを実装する方法があります。

認証に OAuth を使用する例については、「 [Twitter SharePoint web part with OAuth](http://aidangarnish.net/post/Twitter-SharePoint-Web-Part-With-OAuth.aspx)」を参照してください。

 **System.Net** の **Cookie** オブジェクトを使用して、Cookie の値を取得および設定する方法を示すサーバー側のコード例については、 [Value](http://msdn2.microsoft.com/JA-JP/library/4f772twc) プロパティを参照してください。


## クロス オリジン リソース共有 (CORS) の使用


[XmlHttpRequest2](http://dvcs.w3.org/hg/xhr/raw-file/tip/Overview.mdl) のクロス オリジン リソース共有機能を使用する例については、「 [XMLHttpRequest2 に関する新しいヒント](http://www.mdl5rocks.com/ja/tutorials/file/xhr2/)」の「Cross Origin Resource Sharing (CORS)」のセクションを参照してください。


## IFRAME および POST MESSAGE を使用する独自のプロキシの作成


IFRAME および POST MESSAGE を使用して独自のプロキシを作成する例については、「 [Cross-Window Messaging](http://ejohn.org/blog/cross-window-messaging/)」を参照してください。


## その他の技術情報



- [Office アドインのプライバシーとセキュリティ](87c59a88-10e2-4c88-b6a8-736bd356e5f8.md)
    
