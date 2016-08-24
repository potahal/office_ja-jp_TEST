---
ms.Toctitle: Get content and structure
title: "OneNote コンテンツと構造を取得する" 
description: "OneNote のエンティティ、階層、ページとファイルのコンテンツを取得します。 OData クエリ文字列オプションを使用し、クエリをフィルター処理します。"
ms.ContentId: f3e2d1e6-0c62-4c22-bd8b-4dd378356502
ms.date: May 24, 2016

---
[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]
[!INCLUDE [Add the ONAPI repo styles](../includes/controls/addonapistyles.xml)]

# OneNote API によって OneNote コンテンツと構造を取得する

*__適用対象:__OneDrive のコンシューマー ノートブック | Office 365 のエンタープライズ ノートブック*

OneNote のコンテンツと構造を取得するには、ターゲット エンドポイントに GET 要求を送信します。 たとえば次のようにします。

<p id="indent">`GET ../notes/pages/{id}`</p>

要求が成功すると、OneNote API は 200 HTTP 状態コードと、要求したエンティティまたはコンテンツを返します。OneNote のエンティティは、OData バージョン 4.0 仕様に準拠した JSON オブジェクトとして返されます。

クエリ文字列オプションを使用すると、クエリをフィルターしてパフォーマンスを向上させることができます。

<a name="request-uri"></a>
## 要求 URI の構築

要求 URI を構築するには、サービス ルート URL から開始します。

[!INCLUDE [service root url](../includes/onenote/service-root-url.xml)]

次に、取得するリソースのエンドポイントを追加します  ([リソース パス](#resource-paths)が次のセクションに表示されます)。

<br />
完全な要求 URI は、次に示す例のいずれかのようになります。
<p id="indent">`https://www.onenote.com/api/v1.0/me/notes/notebooks/{id}/sections`</p>
<p id="indent">`https://www.onenote.com/api/v1.0/myOrganization/siteCollections/{id}/sites/{id}/notes/pages`</p>
<p id="indent">`https://www.onenote.com/api/v1.0/myOrganization/groups/{id}/notes/pages?select=title,self`</p>

[!INCLUDE [service root url note](../includes/onenote/service-root-note.xml)]


<a name="resource-paths"></a>
## GET 要求のリソース パス

次のリソース パスを使用して、ページ、セクション、セクション グループ、ノートブック、画像、またはファイル リソースを取得します。

[ページ コレクション](#get-pages)&nbsp;&nbsp;|&nbsp;&nbsp;[ページ エンティティ](#get-page)&nbsp;&nbsp;|&nbsp;&nbsp;[ページ プレビュー](#get-page-preview)&nbsp;&nbsp;|&nbsp;&nbsp;[ページ HTML コンテンツ](#get-page-content)&nbsp;&nbsp;|&nbsp;&nbsp;
[セクション コレクション](#get-sections)&nbsp;&nbsp;|&nbsp;&nbsp;[セクション エンティティ](#get-section)&nbsp;&nbsp;|&nbsp;&nbsp;[SectionGroup コレクション](#get-section-groups)&nbsp;&nbsp;|&nbsp;&nbsp;
[SectionGroup エンティティ](#get-section-group)&nbsp;&nbsp;|&nbsp;&nbsp;[ノートブック コレクション](#get-notebooks)&nbsp;&nbsp;|&nbsp;&nbsp;[ノートブック エンティティ](#get-notebook)&nbsp;&nbsp;|&nbsp;&nbsp;
[画像またはその他のファイル リソース](#get-resource)

<a name="get-pages"></a>
### ページ コレクション

<p id="outdent1">すべてのノートブックのページ (メタデータ) を取得する</p>
<p id="indent1">`../pages[?filter,orderby,select,expand,top,skip,search,count]`</p>

<p id="outdent1">特定のセクションからページ (メタデータ) を取得する</p>
<p id="indent1">`../sections/{section-id}/pages[?filter,orderby,select,expand,top,skip,search,count,pagelevel]`</p>

<br />
`search`クエリ文字列オプションは、コンシューマー ノートパソコンでのみ使用できます。

ページの既定の並べ替え順序は `lastModifiedTime desc` です。

既定のクエリは、親セクションを展開し、セクションの `id`、`name`、および `self` プロパティを選択します。

既定では、*GET ページ*要求に対して上位 20 のエントリのみが返されます。**上位**クエリ文字列オプションを指定しない要求を行った場合、次の 20 エントリを取得するために使用できる **@odata.nextLink** リンクが応答で返されます。

セクションのページ コレクションの場合、**pagelevel** を使用して、ページのインデント レベルとセクション内でのその順序を返します。 例: `GET ../sections/{section-id}/pages?pagelevel=true`

- - -

<a name="get-page"></a> 
### ページのエンティティ

<p id="outdent1">特定のページのメタデータを取得する</p>
<p id="indent1">`../pages/{page-id}[?select,expand,pagelevel]`</p>

<br />
ページは、**parentNotebook** プロパティと **parentSection** プロパティを展開できます。

既定のクエリは、親セクションを展開し、セクションの `id`、`name`、および `self` プロパティを選択します。

**pagelevel** を使用して、ページのインデント レベルと親セクション内でのその順序を返します。 例: `GET ../pages/{page-id}?pagelevel=true`

- - -

<a name="get-page-preview"></a> 
### ページ プレビュー

<p id="outdent1">ページのテキストと画像のプレビュー コンテンツを取得する</p>
<p id="indent1">`../pages/{page-id}/preview`</p>

<br />
JSON 応答には、ページに含まれているものを特定するために使用できる、プレビュー コンテンツが含まれています。

```json
{
  "@odata.context":"https://www.onenote.com/api/v1.0/$metadata#Microsoft.OneNote.Api.PagePreview",
  "previewText":"text-snippet",
  "links":{
    "previewImageUrl":{
      "href":"https://www.onenote.com/api/v1.0/resources/{id}/content?publicAuth=true&mimeType=image/png"
    }
  }
}
```

**previewText** プロパティには、ページからのテキスト スニペットが含まれています。 OneNote API は、最大 300 文字の完全な文字列を返します。 

ページにプレビュー UI のビルドに使用できる画像が含まれている場合、**previewImageUrl** オブジェクトの **href** プロパティには、事前認証されたパブリックの [image resource](#get-resource) のリンクが含まれます。 このリンクは HTML で使用できます。たとえば、`<img src="https://www.onenote.com/api/v1.0/resources/{id}/content?publicAuth=true&mimeType=image/png" />` です。 それ以外の場合、**href** は、null 値を返します。

- - -

<a name="get-page-content"></a> 
### ページ HTML コンテンツ

<p id="outdent1">ページの HTML コンテンツを取得する</p>
<p id="indent1">`../pages/{page-id}/content[?includeIDs,preAuthenticated]`<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(*[返される HTML コンテンツ](../howto/onenote-input-output-html.md)*の詳細)</p>

<br />
**includeIDs=true** クエリ文字列オプションを使用して、[ページを更新](../howto/onenote-update-page.md)するために使用する生成 ID を取得します。

**preAuthenticated=true** クエリ文字列オプションを使用して、ページ上の[画像リソース](#get-resource)のパブリック URL を取得します。パブリック URL は 1 時間有効です。 

- - -

<a name="get-sections"></a>
### セクション コレクション

<p id="outdent1">ユーザーが所有するすべてのノートブックから、ネストされたセクション グループのセクションを含む、すべてのセクションを取得する</p>
<p id="indent1">`../sections[?filter,orderby,select,top,skip,expand,count]`</p>

<p id="outdent1">特定のセクション グループの直下にあるすべてのセクションを取得する</p>
<p id="indent1">`../sectionGroups/{sectiongroup-id}/sections[?filter,orderby,select,top,skip,expand,count]`</p>

<p id="outdent1">特定のノートブックの直下にあるすべてのセクションを取得する</p>
<p id="indent1">`../notebooks/{notebook-id}/sections[?filter,orderby,select,top,skip,expand,count]`</p>

<br />
セクションは、**parentNotebook** プロパティと **parentSectionGroup** プロパティを展開できます。

セクションの既定の並べ替え順序は `name asc` です。

既定のクエリは、親ノートブックおよび親セクション グループを展開し、`id`、`name`、および `self` プロパティを選択します。

- - -

<a name="get-section"></a>
### セクション エンティティ

<p id="outdent1">特定のセクションを取得する</p>
<p id="indent1">`../sections/{section-id}[?select,expand]`</p>

<br />
セクションは、**parentNotebook** プロパティと **parentSectionGroup** プロパティを展開できます。

既定のクエリは、親ノートブックおよび親セクション グループを展開し、`id`、`name`、および `self` プロパティを選択します。

- - -

<a name="get-section-groups"></a>
### SectionGroup コレクション

<p id="outdent1">ユーザーが所有するすべてのノートブックから、ネストされたセクション グループを含む、すべてのセクション グループを取得する</p>
<p id="indent1">`../sectionGroups[?filter,orderby,select,top,skip,expand,count]`</p>

<p id="outdent1">特定のノートブックの直下にあるすべてのセクション グループを取得する</p>
<p id="indent1">`../notebooks/{notebook-id}/sectionGroups[?filter,orderby,select,top,skip,expand,count]`</p>

<br />
セクション グループは、**セクション**、**sectionGroups**、**parentNotebook**、および **parentSectionGroup** プロパティを展開できます。

セクション グループの既定の並べ替え順序は `name asc` です。

既定のクエリは、親ノートブックおよび親セクション グループを展開し、`id`、`name`、および `self` プロパティを選択します。

- - -

<a name="get-section-group"></a>
### SectionGroup エンティティ

<p id="outdent1">特定のセクション グループを取得する</p>
<p id="indent1">`../sectionGroups/{sectiongroup-id}[?select,expand]`</p>

<br />
セクション グループは、**セクション**、**sectionGroups**、**parentNotebook**、および **parentSectionGroup** プロパティを展開できます。

既定のクエリは、親ノートブックおよび親セクション グループを展開し、`id`、`name`、および `self` プロパティを選択します。

- - -

<a name="get-notebooks"></a>
### ノートブック コレクション

<p id="outdent1">ユーザーによって所有されるすべてのノートブックを取得する</p>
<p id="indent1">`../notebooks[?filter,orderby,select,top,skip,expand,count]`</p>

<br />
ノートブックは、**セクション**および **sectionGroups** プロパティを展開できます。

ノートブックの既定の並べ替え順序は `name asc` です。 

>`classNotebooks` エンドポイントを使用して、[クラス ノートブックを取得](../howto/onenote-classnotebook.md)できます。

- - -

<a name="get-notebook"></a>
### ノートブック エンティティ

<p id="outdent1">特定のノートブックを取得する</p>
<p id="indent1">`../notebooks/{notebook-id}[?select,expand]`</p>

<br />
ノートブックは、**セクション**および **sectionGroups** プロパティを展開できます。

>`classNotebooks` エンドポイントを使用して、[クラス ノートブックを取得](../howto/onenote-classnotebook.md)できます。

- - -

<a name="get-resource"></a>
### 画像またはその他のファイル リソース

<p id="outdent1">特定のリソースのバイナリ データを取得する</p>
<p id="indent1">`../resources/{resource-id}/$value`</p>

<br />
ファイルのリソース URI は、ページの[出力 HTML](..\howto\onenote-input-output-html.md) にあります。

たとえば、 **img** タグには、**data-fullres-src** 属性の元のイメージのエンドポイントと、**src** 属性の最適化されたイメージのエンドポイントが含まれます。例:

```
<img 
    src="https://www.onenote.com/api/v1.0/me/notes/resources/{image-id}/$value"  
    data-src-type="image/png"
    data-fullres-src="https://www.onenote.com/api/v1.0/resources/{image-id}/$value"  
    data-fullres-src-type="image/png" ... />
```

**object** タグには、**data** 属性のファイル リソースのエンドポイントが含まれています。 例:

```
<object
    data="http://www.onenote.com/api/v1.0/me/notes/resources/{file-id}/$value"
    data-attachment="fileName.pdf" 
    type="application/pdf" ... />
```

ページにある画像リソースへの事前認証された公開 URL を取得するには、[ページのコンテンツを取得する](#get-page-content)際にクエリ文字列に **preAuthenticated=true** を含めます (例: `GET ../pages/{page-id}/content?preAuthenticated=true`)。 [出力 HTML](../howto/onenote-input-output-html.md#image-output-examples) に返されるパブリック URL は 1 時間有効です。 このフラグを設定していない場合、取得された画像は非公開で、またページのコンテンツの残りの部分と同様、それらを取得するために承認を必要とするため、ブラウザーで直接レンダリングすることはありません。 

リソースのコレクションの取得はサポートされていません。 

ファイル リソースを取得する場合、**Accept** コンテンツ タイプを要求に含める必要はありません。

GET 要求の詳細については、OneNote API 対話型 REST リファレンスの [GET Pages](http://dev.onenote.com/docs#/reference/get-pages)、[GET Sections](http://dev.onenote.com/docs#/reference/get-sections)、[GET SectionGroups](http://dev.onenote.com/docs#/reference/get-sectiongroups)、および [GET Notebooks](http://dev.onenote.com/docs#/reference/get-notebooks) を参照してください。


<a name="example"></a>
## GET 要求の例
OneNote のエンティティおよび検索ページ コンテンツを照会して、必要な情報だけを取得することができます。次の例は、OneNote API への GET 要求で、[サポートされているクエリ文字列オプション](#query-options)を使用するいくつかの方法を示しています。 

次の点に注意してください。

- GET 要求はすべて、[サービス ルート URL](#request-uri) で始まります。<br />&nbsp;&nbsp;例: `https://www.onenote.com/api/v1.0/me/notes`<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`https://www.onenote.com/api/v1.0/myOrganization/siteCollections/{id}/sites/{id}/notes/`

- URL クエリ文字列のスペースには %20 エンコードを使用する必要があります。<br />&nbsp;&nbsp;例: `filter=title%20eq%20'biology'`

- プロパティ名と OData 文字列比較では大文字と小文字が区別されます。 文字列比較には OData **tolower** 関数を使用することをお勧めします。<br />&nbsp;&nbsp;例: `filter=tolower(name) eq 'spring'`
 

**search & filter**  

特定のアプリによって作成された、*recipe* という用語が含まれるページすべてを取得します   (`search` は、コンシューマー ノートパソコンでのみ使用可能)。

```
[GET] ../pages?search=recipe&filter=createdByAppId eq 'WLID-000000004C12821A'
```
 
**search & select**  

*golgi app* という用語が含まれるすべてのページの、タイトル、OneNote クライアントのリンク、および **contentUrl** リンクを取得します   (`search` は、コンシューマー ノートパソコンでのみ使用可能)。

```
[GET] ../pages?search=golgi app&select=title,links,contentUrl
```
 
**expand**  

すべてのノートブックを取得し、そのセクションとセクション グループを展開します。  

```
[GET] ../notebooks?expand=sections,sectionGroups
```
 
特定のセクション グループを取得し、そのセクションとセクション グループを展開します。  

```
[GET] ../sectionGroups/{sectiongroup-id}?expand=sections,sectionGroups
```

ページを取得し、その親セクションと親ノートブックを展開します。

```
[GET] ../pages/{page-id}?expand=parentSection,parentNotebook
```

**expand** (複数レベル)  

すべてのノートブックを取得してセクションとセクション グループを展開し、各セクション グループのすべてのセクションを展開します。  

```
[GET] ../notebooks?expand=sections,sectionGroups(expand=sections)
```
 
>子エンティティの親を展開、または親エンティティの子を展開すると、循環参照が作成されますが、これはサポートされていません。

 
**expand & select** (複数レベル)  

特定のセクション グループの名前と **self** リンクを取得し、そのすべてのセクションの名前と **self** リンクを取得します。  

```
[GET] ../sectionGroups/{sectiongroup-id}?expand=sections(select=name,self)&select=name,self
```
 
すべてのセクションの名前と **self** リンクを取得し、各セクションの親ノートブックの名前と作成時刻を取得します。  

```
[GET] ../sections?expand=parentNotebook(select=name,createdTime)&select=name,self
```
 
すべてのページのタイトルと ID を取得し、親セクションと親ノートブックの名前を取得します。

```
[GET] ../pages?select=id,title&expand=parentSection(select=name),parentNotebook(select=name)
```

**expand & levels** (複数レベル)  

すべてのノートブック、セクション、セクション グループを取得します。  

```
[GET] ../notebooks?expand=sections,sectionGroups(expand=sections,sectionGroups(levels=max;expand=sections))
```
 
**filter**  

2014 年 10 月に作成されたセクションすべてを取得します。

```
[GET] ../sections?filter=createdTime ge 2014-10-01 and createdTime le 2014-10-31
```

2015 年 1 月 1 日以降に特定のアプリによって作成されたページを取得します。

```
[GET] ../pages?filter=createdByAppId eq 'WLID-0000000048118631' and createdTime ge 2015-01-01
```

**filter & expand**  

特定のノートブックに含まれるすべてのページを取得します。 既定では、この API は20 エントリを戻します。

```
[GET] ../pages?filter=parentNotebook/id eq '{notebook-id}'&expand=parentNotebook
```

*School* ノートブックのすべてのセクションの名前と **pagesUrl** リンクを取得します。OData 文字列比較では大文字小文字が区別されるため、**tolower** 関数を使用するのがベスト プラクティスです。

```
[GET] ../notebooks?filter=tolower(name) eq 'school'&expand=sections(select=name,pagesUrl)
```

**filter & select & orderby**   

セクション名に *spring* という語が含まれるすべてのセクションの名前と **pagesUrl** リンクを取得します。セクションを最終変更日で並べ替えます。

```
[GET] ../sections?filter=contains(tolower(name),'spring')&select=name,pagesUrl&orderby=lastModifiedTime desc
```
 
**orderby**  

**createdByAppId** プロパティ、次に作成時刻が新しい順序に並び替えて最初の 20 ページを取得します。 既定では、この API は20 エントリを戻します。

```
[GET] ../pages?orderby=createdByAppId,createdTime desc
```

**search & filter & top**  

*cell division* という語句を含む、2015 年 1 月 1 日以降に作成されたページで新しいものを 5 つ取得します。 既定ではこの API は 20 エントリを返します。最大では 100 エントリを返します。 ページの既定の並べ替え順序は `lastModifiedTime desc` です  (`search` は、コンシューマー ノートパソコンでのみ使用可能)。

```
[GET] ../pages?search="cell division"&filter=createdTime ge 2015-01-01&top=5
```

**search & filter & top & skip**  

結果セットの次の 5 つのページを取得します  (`search` は、コンシューマー ノートパソコンでのみ使用可能)。

```
[GET] ../pages?search=biology&filter=createdTime ge 2015-01-01&top=5&skip=5
```

さらに次の 5 つを取得します  (`search` は、コンシューマー ノートパソコンでのみ使用可能)。

```
[GET] ../pages?search=biology&filter=createdTime ge 2015-01-01&top=5&skip=10
```

>**search** と **filter** がどちらも同じ要求に適用される場合、結果には両方の条件に一致するエンティティのみが含まれます。
 
**select**  

ユーザーのノートブック内にあるすべてのセクションの名前、作成時刻、**self** リンクを取得します。

```
[GET] ../sections?select=name,createdTime,self
```

特定のページのタイトル、作成時刻、OneNote クライアント リンクを取得します。

```
[GET] ../pages/{page-id}?select=title,createdTime,links
```

**select & expand & filter** (複数レベル)  

ユーザーの既定ノートブックにあるすべてのセクションの名前と **pagesUrl** リンクを取得します。

```
[GET] ../notebooks?select=name&expand=sections(select=name,pagesUrl)&filter=isDefault eq true
```

**top & select & orderby**  

最初の 50 ページのタイトルと **self** リンクを取得します。タイトルのアルファベット順に並べられます。 既定ではこの API は 20 エントリを返します。最大では 100 エントリを返します。 ページの既定の並べ替え順序は `lastModifiedTime desc` です 

```
[GET] ../pages?top=50&select=title,self&orderby=title
```

**skip & top & select & orderby**  

51 ～ 100 ページを取得します。 既定ではこの API は 20 エントリを返します。最大では 100 エントリを返します。

```
[GET] ../pages?skip=50&top=50&select=title,self&orderby=title
```

>既定のエントリ数を取得する pages に対する GET 要求 (つまり、**top** 式を指定しない要求) は、応答で次の 20 エントリを取得するために使用できる **@odata.nextLink** リンクを戻します。
 

<a name="query-options"></a>
## サポートされている OData クエリ文字列オプション

GET 要求を OneNote API に送信する場合、OData クエリ文字列オプションを使用してクエリをカスタマイズし、必要な情報だけを取得できます。また、サービスに対する呼び出し数と応答ペイロードのサイズを減らすことによって、パフォーマンスを向上できます。

>この記事の例では読みやすくするために、URL クエリ文字列内のスペースで必要な %20 パーセント エンコードを使用していません: `filter=isDefault%20eq%20true`
 
| クエリ オプション | 例と説明 |  
|------|------|  
| count | <p>`count=true`</p><p>コレクション内のエンティティのカウントです。 この値は、応答の **@odata.count** プロパティで戻ります。</p> |  
| expand | <p>`expand=sections,sectionGroups`</p><p>応答でインラインを返すナビゲーション プロパティ。 **expand** 式には次のプロパティがサポートされています。<br /> - ページ: **parentNotebook**、**parentSection**<br /> - セクション: **parentNotebook**、**parentSectionGroup**<br /> - セクション グループ: **sections**、**sectionGroups**、**parentNotebook**、**parentSectionGroup**<br /> - ノートブック: **sections**、**sectionGroups**</p><p>既定では、ページの GET 要求は **parentSection** を展開し、セクションの **id**、**name**、**self** の各プロパティを選択します。セクションとセクションのグループの既定の GET 要求は **parentNotebook** と **parentSectionGroup** の両方を展開し、親の **id**、**name**、**self** の各プロパティを選択します。</p><p>1 つのエンティティまたはコレクションに使用できます。 コンマを使用して複数のプロパティを区切ります。 プロパティ名では、大文字と小文字を区別します。</p> |   
| filter | <p>`filter=isDefault eq true`</p><p>結果セットの入力に含めるかどうかを決めるブール式。 サポートされている OData 演算子と関数:<br /> - 比較演算子: **eq**、**ne**、**gt**、**ge**、**lt**、**le**<br /> - 論理演算子: **and**、**or**、**not**<br /> - 文字列関数: **contains**、**endswith**、**startswith**、**length**、**indexof**、**substring**、**tolower**、**toupper**、**trim**、**concat**</p><p>[プロパティ](#properties)名と OData 文字列比較では大文字と小文字が区別されます。 文字列比較には OData **tolower** 関数を使用することをお勧めします。 例: `filter=tolower(name) eq 'spring'`</p> |  
| orderby | <p>`orderby=title,createdTime desc`</p><p>並べ替え基準を示す[プロパティ](#properties)です。オプションで、**asc** (既定) または **desc** 並べ替え順序を指定します。要求対象コレクション内のエンティティの任意のプロパティで並べ替えが可能です。</p><p>ノートブック、セクション グループ、セクションの既定の並べ替え順序は `name asc` で、ページの場合には `lastModifiedTime desc` (最新の変更ページが先頭になります)。</p><p>複数のプロパティはコンマで区切り、任意の順序で一覧表示します。 プロパティ名では、大文字と小文字を区別します。</p> |  
| search | <p>`search=cell div`</p><p>コンシューマー ノートパソコンでのみ使用可能。</p><p>
ページ タイトル、ページ本文、画像の代替テキスト、画像の OCR テキストで検索する用語または語句です。既定では、検索クエリは関連度で並べ替えて結果を返します。</p><p>OneNote は、Bing のフルテキスト検索を使用して、語句の検索、語形変化、スペルの曖昧さ、関連性とランキング、単語分割、複数の言語、他のフルテキスト検索機能をサポートします。 検索文字列では、大文字と小文字が区別されます。</p><p>ユーザーが所有しているノートブック内のページにのみ適用されます (ユーザーとは共有されません)。 インデックス付けされたコンテンツは非公開であり、所有者のみがアクセスできます。 パスワードで保護されたページにはインデックスは付けられません。 `pages` エンドポイントにのみ適用されます。</p> |  
| select | <p>`select=id,title`</p><p>返す[プロパティ](#properties)。 1 つのエンティティまたはコレクションに使用できます。 コンマを使用して複数のプロパティを区切ります。 プロパティ名では、大文字と小文字を区別します。</p> |  
| skip | <p>`skip=10`</p><p>結果セットでスキップするエントリの数。 通常、結果のページングに使用されます。</p> |  
| top | <p>`top=50`</p><p>結果セット内で返すエントリ数で、最大数は 100 です。 既定値は 20 です。</p> |  

OneNote にも、親セクション内でページのレベルと順序を取得する `pagelevel` クエリ文字列オプションが用意されています。 特定のセクションのページのクエリ、または特定のページのクエリのにのみ適用されます。 たとえば次のようにします。 

<p id="indent">`GET ../sections/{section-id}/pages?pagelevel=true`</p>
<p id="indent">`GET ../pages/{page-id}?pagelevel=true`</p>

### サポートされている OData 演算子と関数

OneNote API は、**フィルター**式で次の OData 演算子および関数をサポートしています。 OData 式を使用する場合には、次の点に注意してください。  
- URL クエリ文字列のスペースは `%20` エンコードで置換する必要があります。<br />&nbsp;&nbsp;例: `filter=isDefault%20eq%20true`
- プロパティ名と OData 文字列比較では大文字と小文字が区別されます。 文字列比較には OData **tolower** 関数を使用することをお勧めします。<br />&nbsp;&nbsp;例: `filter=tolower(name) eq 'spring'`

| 比較演算子 | 例 |  
|------|------|  
| eq<br />(等しい) | `createdByAppId eq '{app-id}'` |  
| ne<br />(等しくない) | `userRole ne 'Owner'` |  
| gt<br />(より大きい) | `createdTime gt 2014-02-23` |  
| ge<br />(以上) | `lastModifiedTime ge 2014-05-05T07:00:00Z` |  
| lt<br />(より小さい) | `createdTime lt 2014-02-23` |  
| le<br />(以下) | `lastModifiedTime le 2014-02-23` |  
 
| 論理演算子 | 例 |  
|------|------|  
| および | `createdTime le 2014-01-30 and createdTime gt 2014-01-23` |  
| または | `createdByAppId eq '{app-id}' or createdByAppId eq '{app-id}'` |  
| not | `not contains(tolower(title),'school')` |  
 
| String 関数 | 例 |  
|------|------|   
| contains | `contains(tolower(title),'spring')` |  
| endswith | `endswith(tolower(title),'spring')` |  
| startswith | `startswith(tolower(title),'spring')` |  
| length | `length(title) eq 19` |  
| indexof | `indexof(tolower(title),'spring') eq 1` |  
| substring | `substring(tolower(title),1) eq 'spring'` |  
| tolower | `tolower(title) eq 'spring'` |  
| toupper | `toupper(title) eq 'SPRING'` |  
| trim | `trim(tolower(title)) eq 'spring'` |  
| concat | `concat(title,'- by MyRecipesApp') eq 'Carrot Cake Recipe - by MyRecipesApp'` |  
 

<a name="properties"></a>
## ページ、セクション、セクション グループ、ノートブックのプロパティ

**filter**、**select**、**expand**、および **orderby** クエリ式には、 OneNote エンティティのプロパティを含めることができます。 たとえば次のようにします。

<p id="indent">`../sections?filter=createdTime ge 2015-01-01&select=name,pagesUrl&orderby=lastModifiedTime desc`</p>

クエリ式のプロパティ名は大文字小文字を区別します。

プロパティおよびプロパティ タイプのリストについては、次を参照してください。

- [ページ](http://dev.onenote.com/docs#/reference/get-pages)
- [セクション](http://dev.onenote.com/docs#/reference/get-sections)
- [SectionGroups](http://dev.onenote.com/docs#/reference/get-sectiongroups)
- [ノートブック](http://dev.onenote.com/docs#/reference/get-notebooks)

**expand** クエリ文字列オプションは、以下のナビゲーション プロパティとともに使用できます。

- ページ: **parentNotebook**、**parentSection**
- セクション: **parentNotebook**、**parentSectionGroup**
- セクション グループ: **sections**、**sectionGroups**、**parentNotebook**、**parentSectionGroup**
- ノートブック: **sections**、**sectionGroups**


<a name="request-response-info"></a>
## 要求および *GET* 要求の応答に関する情報

| 要求データ | 説明 |  
|------|------|  
| プロトコル | すべての要求は SSL/TLS HTTPS プロトコルを使用します。 |  
| 承認ヘッダー | <p>`Bearer {token}`。*{token}* は、登録済みアプリの有効な OAuth 2.0 アクセス トークンになります。</p><p>これがないか、無効の場合、要求は失敗し、401 ステータス コードが表示されます。 「[認証とアクセス許可](..\howto\onenote-auth.md)」を参照してください。</p> |  
| Accept ヘッダー | <p>OneNote エンティティとエンティティ セットの - `application/json`</p><p>ページ コンテンツの - `text/html`</p> | 

| 応答データ | 説明 |  
|------|------|  
| 成功コード | 200 HTTP ステータス コード。 |  
| 応答本文 | JSON 形式のエンティティまたはエンティティ セット、ページ HTML、またはファイル リソースのバイナリ データの OData 表現。  |  
| エラー | 要求が失敗すると、API は応答本文の [@api.diagnostics](../howto/onenote-error-codes.md) オブジェクトに**エラー**を返します。 |  
| X-CorrelationId ヘッダー | 要求を一意に識別する GUID。Microsoft サポートと問題のトラブルシューティングを行う際に、この値を Date ヘッダーの値とともに使用できます。 |  


<a name="root-url"></a>
### OneNote サービスのルート URL の構築

[!INCLUDE [service root url section](../includes/onenote/service-root-section.xml)]


<a name="permissions"></a>
## GET 要求のアクセス許可

OneNote のコンテンツまたは構造を取得するには、適切なアクセス許可を要求する必要があります。 

次のスコープは、OneNote API に対する GET 要求を許可します。アプリの動作に必要な最低限のアクセス許可を選択してください。

| プラットフォーム | アクセス許可の適用範囲 |  
|------|------|  
| コンシューマー | office.onenote, office.onenote_update_by_app, office.onenote_update |  
| エンタープライズ | Notes.Read, Notes.ReadWrite.CreatedByApp, Notes.ReadWrite, Notes.ReadWrite.All |  

アクセス許可のスコープと動作のしくみの詳細については、「[OneNote のアクセス許可のスコープ](../howto/onenote-auth.md#onenote-perms)」を参照してください。

<a name="see-also"></a>
## その他のリソース

- [OneNote ページの入出力 HTML](../howto/onenote-input-output-html.md)
- [OneNote の開発](../howto/onenote-landing.md)
- [OneNote デベロッパー センター](http://dev.onenote.com/)
- [OneNote の開発者ブログ](http://go.microsoft.com/fwlink/?LinkID=390183)
- [スタック オーバーフローに関する OneNote の開発の質問](http://go.microsoft.com/fwlink/?LinkID=390182)
- [OneNote GitHub のリポジトリ](http://go.microsoft.com/fwlink/?LinkID=390178)  


[!INCLUDE [Enable filtering functionality ](../includes/controls/enablefiltering.xml)]