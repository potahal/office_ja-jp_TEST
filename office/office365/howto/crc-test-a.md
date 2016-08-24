---
ms.Toctitle: CRC Model Page
title: "CRC Model Page"
description: "This page illustrates the markdown for on-page filtering by platform."
ms.ContentId: 72d424bc-ad83-4f91-90ba-66f2cb775f33

---
[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]

You are currently viewing the sandbox branch of the O365 API repo.

[!INCLUDE [Add the platforms filter bar](../includes/controls/addplatformsfilter.xml)]

# オンページ フィルター処理 (CRC) のモデルのページ  

このファイルのマークダウンは、**O365API リポジトリ**でプラットフォーム別のオンページ フィルター処理をセットアップする方法について説明します。プラットフォームのコンテンツ セクションの後に、[フィルター処理を有効にする方法](#filtering_howto)についての説明とコード行が続きます。

[!INCLUDE [BEGIN iOS section](../includes/controls/iossection.xml)]

## iOS セクション

Lorem way ipsum dolor sit amet, consectetur adipiscing elit.Integer nec odio.Praesent libero.Sed cursus ante dapibus diam.Sed nisi.Nulla quis sem at nibh elementum imperdiet.Duis sagittis ipsum.Praesent mauris.Fusce nec tellus sed augue semper porta.Mauris massa.Vestibulum lacinia arcu eget nulla.Class aptent taciti sociosqu ad litora torquent per conubia nostra, per inceptos himenaeos. 

Curabitur sodales ligula in libero.Sed dignissim lacinia nunc.Curabitur tortor.Pellentesque nibh.Aenean quam.In scelerisque sem at dolor.Maecenas mattis.Sed convallis tristique sem.Proin ut ligula vel nunc egestas porttitor.Morbi lectus risus, iaculis vel, suscipit quis, luctus non, massa.Fusce ac turpis quis ligula lacinia aliquet.Mauris ipsum. 

[!INCLUDE [END iOS section](../includes/controls/iossection.xml)]

[!INCLUDE [BEGIN Android section](../includes/controls/androidsection.xml)]

## Android セクション

Nulla metus metus, ullamcorper vel, tincidunt sed, euismod in, nibh.Quisque volutpat condimentum velit.Class aptent taciti sociosqu ad litora torquent per conubia nostra, per inceptos himenaeos.Nam nec ante.Sed lacinia, urna non tincidunt mattis, tortor neque adipiscing diam, a cursus ipsum ante quis turpis.Nulla facilisi.Ut fringilla.Suspendisse potenti.Nunc feugiat mi a tellus consequat imperdiet.Vestibulum sapien.Proin quam.Etiam ultrices.Suspendisse in justo eu magna luctus suscipit.Sed lectus. 

Integer euismod lacus luctus magna.Quisque cursus, metus vitae pharetra auctor, sem massa mattis sem, at interdum magna augue eget diam.Vestibulum ante ipsum primis in faucibus orci luctus et ultrices posuere cubilia Curae; Morbi lacinia molestie dui.Praesent blandit dolor.Sed non quam.In vel mi sit amet augue congue elementum.Morbi in ipsum sit amet pede facilisis laoreet.Donec lacus nunc, viverra nec, blandit vel, egestas et, augue.Vestibulum tincidunt malesuada tellus.Ut ultrices ultrices enim.Curabitur sit amet mauris.Morbi in dui quis est pulvinar ullamcorper.Nulla facilisi.Integer lacinia sollicitudin massa. 

[!INCLUDE [END Android section](../includes/controls/androidsection.xml)]

[!INCLUDE [BEGIN JavaScript section](../includes/controls/javascriptsection.xml)]

## JavaScript セクション

Cras metus.Sed aliquet risus a tortor.Integer id quam.Morbi mi.Quisque nisl felis, venenatis tristique, dignissim in, ultrices sit amet, augue.Proin sodales libero eget ante.Nulla quam.Aenean laoreet.Vestibulum nisi lectus, commodo ac, facilisis ac, ultricies eu, pede.Ut orci risus, accumsan porttitor, cursus quis, aliquet eget, justo.Sed pretium blandit orci. 

Ut eu diam at pede suscipit sodales.Aenean lectus elit, fermentum non, convallis id, sagittis at, neque.Nullam mauris orci, aliquet et, iaculis et, viverra vitae, ligula.Nulla ut felis in purus aliquam imperdiet.Maecenas aliquet mollis lectus.Vivamus consectetuer risus et tortor.Lorem ipsum dolor sit amet, consectetur adipiscing elit.Integer nec odio.Praesent libero.Sed cursus ante dapibus diam.Sed nisi.Nulla quis sem at nibh elementum imperdiet.Duis sagittis ipsum.Praesent mauris. 

[!INCLUDE [END JavaScript section](../includes/controls/javascriptsection.xml)]

[!INCLUDE [BEGIN ASP.NET MVC section](../includes/controls/aspnetmvcsection.xml)]

## ASP.NET MVC セクション

Fusce nec tellus sed augue semper porta.Mauris massa.Vestibulum lacinia arcu eget nulla.Class aptent taciti sociosqu ad litora torquent per conubia nostra, per inceptos himenaeos.Curabitur sodales ligula in libero.Sed dignissim lacinia nunc.Curabitur tortor.Pellentesque nibh.Aenean quam.In scelerisque sem at dolor.Maecenas mattis. 

Sed convallis tristique sem.Proin ut ligula vel nunc egestas porttitor.Morbi lectus risus, iaculis vel, suscipit quis, luctus non, massa.Fusce ac turpis quis ligula lacinia aliquet.Mauris ipsum.Nulla metus metus, ullamcorper vel, tincidunt sed, euismod in, nibh.Quisque volutpat condimentum velit.Class aptent taciti sociosqu ad litora torquent per conubia nostra, per inceptos himenaeos.Nam nec ante.Sed lacinia, urna non tincidunt mattis, tortor neque adipiscing diam, a cursus ipsum ante quis turpis.Nulla facilisi.Ut fringilla. 

[!INCLUDE [END ASP.NET MVC section](../includes/controls/aspnetmvcsection.xml)]


## オンページ フィルター処理を有効にする方法
オンページ フィルター処理は、次に示す 3 種類の Markdown 形式のコンテンツ インクルードを追加することで有効にします。

-  単一の**フィルター バー インクルード**。H1 行 (つまり、ボタンが表示される位置) の上。プラットフォームのフィルター処理バーを表示するには、プラットフォームのフィルター バーのインクルードを使用します。
-  プラットフォームの **セクション境界のインクルード ペア**のペア。プラットフォーム固有のコンテンツをセクションごとに囲みます。
-  単一の**フィルター機能のイネーブラー** インクルード。ファイルの最後。

<p style="font-size:85%;margin:0 0 2em 2em;border:1px solid deeppink;border-left-width:8px;background-color:mistyrose;padding:.5em 1em;"><b>重要:</b>すべてのコンテンツ インクルード マークダウン行は、直前または直後に少なくとも 1 行の空行が必要です。ただし、直後が EOF になるフィルター機能のイネーブラーは除きます。</p>

コンテンツ インクルードのコード行を次に示します。

**プラットフォームのフィルター バーのインクルード**

**プラットフォームのセクション境界のインクルード ペア**

<p style="font-size:85%;margin:0 0 2em 2em;border:1px solid rgb(104,189,69);border-left-width:8px;background-color:rgb(236,247,232);padding:.5em 1em;">プラットフォームのセクション境界マーカーの XML _ファイル名_ (かっこで囲む) は、BEGIN セクションと END セクションで同じになります。ただし、ラベル (角かっこで囲む) のみが異なります。わかりやすくするためにラベルを追加します。「BEGIN iOS section 1」、「END iOS section 1」、「BEGIN iOS section 2」など、必要に応じたラベルを使用できます。</p>

**フィルター機能のイネーブラー**


## リンクのテスト セクション

* [bk #pagetitle](#pagetitle)
* [cross-dir ..\api\mail-rest-operations.md](..\api\mail-rest-operations.md)
* [cross-dir-with-bk ../API/files-rest-operations#DeleteFilesClient](../API/files-rest-operations.md#FileoperationsDeleteafile)
* [external-target http://msdn.microsoft.com/ja-jp/library/azure/dn499820.aspx](http://msdn.microsoft.com/en-us/library/azure/dn499820.aspx)
* [external-target-with-bk http://msdn.microsoft.com/ja-jp/library/azure/dn499820.aspx#BKMK_Web (http://msdn.microsoft.com/ja-jp/library/azure/dn499820.aspx#BKMK_Web)
* [out-n-back ..\howto\Starter-projects-and-code-samples.md](..\howto\Starter-projects-and-code-samples.md)
* [out-n-back-with-bk ..\howto\setup-development-environment.md](..\howto\setup-development-environment.md)
* [relative starter-projects-and-code-samples.md](starter-projects-and-code-samples.md)
* [root-relative .\starter-projects-and-code-samples.md](.\starter-projects-and-code-samples.md)

### Filter-selecting のリンク

* <a href="setup-development-environment.md">JavaScript のフィルターが適用されたビューを表示するセットアップ トピックを開く</a>

### Filter+bookmark-selecting のリンク

[!INCLUDE [Enable filtering functionality ](../includes/controls/enablefiltering.xml)]