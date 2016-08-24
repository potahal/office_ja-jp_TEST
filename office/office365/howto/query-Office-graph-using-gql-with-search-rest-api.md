---
ms.Toctitle: Query the Office Graph
title: "GQL および SharePoint Online 検索 REST API を使用した Office Graph のクエリ"
description: "グラフ クエリ言語 (GQL) を使用すると、SharePoint Online 検索 REST API を使用して、フィルターを満たすアイテム (エッジの関係) を Office Graph からクエリできます。"
ms.ContentId: 1be99137-c27a-4724-a01a-722e39df329e
ms.date: November 19, 2015

---
[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]



# GQL および SharePoint Online 検索 REST API を使用した Office Graph のクエリ

**プレリリース コンテンツ**

_**適用対象:**Office 365 |Office 365 初回リリース プログラム | SharePoint Online_

<p class="previewnote">Microsoft の複数のクラウド テクノロジに 1 つのエンドポイントでアクセスできる Microsoft Graph の詳細については、 「[Microsoft Graph](https://graph.microsoft.io)」を参照してください。 ここからの内容は、現在プレビュー段階の Office Graph に適用されます。</p>

グラフ クエリ言語 (GQL) は、SharePoint Online 検索 REST API を使用して Office Graph に対してクエリを行うために設計された暫定的なクエリ言語です。GQL を使用すると、Office Graph にクエリを行って、特定のフィルターに一致するアクターのアイテムを取得できます。



**注:** この記事に記載されている機能および API はプレビュー中であり、変更される可能性があります。 検索 REST API への現在の追加は、Office Graph へのクエリを可能にする暫定的なソリューションです。主に、Office Delve を対象としたエクスペリエンスです。 Office Graph へは自由にクエリを試してください。ただし、これらの機能やこの記事に記載されているその他の機能と API は、運用環境では使用しないでください。 これらの機能および API に関するフィードバックは貴重です。 ご意見を[お聞かせください](http://officespdev.uservoice.com/)。 [スタック オーバーフロー](http://stackoverflow.com/questions/tagged/office365)でご連絡いただけます。 ご質問には、タグ [office365] を付けてください。


## エンタープライズ オブジェクト間の関係をエッジで表す Office Graph
<a name="sectionSection0"> </a>

Office Graph には、人、ドキュメントなどのエンタープライズ オブジェクト、およびこれらのオブジェクト間の関係およびやり取りに関する情報が含まれています。 関係とやり取りは、_エッジ_で表現されます。

一部のエッジは単一の相互作用を表します。
-  **Modified** — Carl がドキュメントを変更しました。
-  **Viewed** — Jarvis がプレゼンテーションを表示しました。
    
一部のエッジは複数の相互作用に基づいて計算されます。
-  **WorkingWith** — 頻繁にやり取りするユーザー。   
-  **TrendingAround** — 同僚の間で人気のあるアイテム。
    
一部のエッジはエンタープライズ オブジェクト間の関係性です。
-  **OrgManager**、**OrgColleague** など — 組織構造のエッジ。
    
現在の Office Graph のエッジとその説明については、「[使用可能なアクションの種類](..\howto\query-Office-graph-using-gql-with-search-rest-api.md#bk_actiontypes)」を参照してください。

図 1 に Office Graph の検索の側面を示します。情報は、Office 365 のサービス全体のアクティビティから収集され、エッジを作成するため処理されます。現在、Office Graph の情報は、SharePoint Online、OneDrive for Business、Exchange Online、Microsoft Azure Active Directory、および Delve から収集されています。




**図 1. Office Graph と Delve の検索面の概略図、作用する主要エクスペリエンス**

![Office Graph および Delve の検索に関する側面は、強化された主なエクスペリエンスです。](images\Office_graph_search_aspecpt.png)


## Office Graph データ モデルおよびエッジ プロパティ
<a name="sectionSection1"> </a>


あらゆるグラフと同様に、Office Graph の各エッジには、ソース ノードとターゲット ノードがあります。ソース ノードを**アクター**と呼び、ターゲット ノードを**オブジェクト**と呼びます。

**図 2. アクター、エッジ、およびオブジェクト間の関係**

![それぞれの端には、ソース ノード (アクター) およびターゲット ノード (オブジェクト) があります](images\Office_graph_relationship_actor_object.png)


**エッジ**のプロパティを表 1 に示します。


**表 1. エッジ プロパティの説明とその種類**


|**プロパティ**|**型**|**説明**|
|:-----|:-----|:-----|
| **ActorId**|整数|アクターの ID です。|
| **ObjectId**|整数|オブジェクトの ID です。|
| **アクションの種類**|整数|エッジが表すアクションまたは関係の種類を識別する ID です。 重要なアクションの種類については、「[使用可能なアクションの種類](..\howto\query-Office-graph-using-gql-with-search-rest-api.md#bk_actiontypes)」を参照してください。|
| **時刻**|String|ISO 8601 標準に基づくエッジのタイムスタンプです。タイムスタンプのセマンティクスは、エッジの種類によって異なります。「[使用可能なアクションの種類](..\howto\query-Office-graph-using-gql-with-search-rest-api.md#bk_actiontypes)」を参照してください。|
| **Weight**|整数|エッジの重要度を示す数値です。重みのセマンティクスは、エッジの種類によって異なります。「[使用可能なアクションの種類](..\howto\query-Office-graph-using-gql-with-search-rest-api.md#bk_actiontypes)」を参照してください。 |
| **BLOB**|BLOB|内部使用のために用意されています。|
| **BlobContent**|String|内部使用のために用意されています。|
| **ObjectSource**|整数|内部使用のために用意されています。|
Office Graph のノードには、SharePoint Online の[検索スキーマ](http://technet.microsoft.com/en-us/library/jj219630%28v=office.15%29)で定義されている同じ管理プロパティがあります。 **SelectProperties** クエリ プロパティを使用すると、**取得可能な**プロパティを取得できます。


## SharePoint Online 検索 REST API のグラフ クエリ拡張機能
<a name="sectionSection2"> </a>

SharePoint Online の [SharePoint 検索 REST API](http://msdn.microsoft.com/library/8a4f7863-e4c1-4099-9189-a1894db36930%28Office.15%29.aspx) で Office Graph に照会するには、クエリ プロパティ バッグに 2 つの新しいプロパティ (**GraphQuery** と **GraphRankingModel**) を含めます。 **GraphQuery** は GQL で記述されます。

絞り込みとクエリ テンプレートを除き、その他の SharePoint Online 検索の使い慣れたクエリ パラメーターを **GraphQuery** および **GraphRankingModel** と組み合せることができます。

通常、Office Graph にクエリを実行する場合、他のアイテムと関連するアイテムを検索して、それらのアイテムとその関係についての情報を取得します。たとえば、"Carl Steadman と関係するすべてのこと" または "Jarvis Ferro が変更したすべてのアイテム" について、グラフにクエリを実行します。




**図 2. グラフに対する REST を介した典型的なクエリ呼び出し**

![グラフ クエリでは、コンテンツ パーツ (Querytext) およびグラフ パーツ (GraphQuery) を使用できます。GraphQuery プロパティは [プロパティ] の一部として指定されます。](images\Office_graph_graph_query_components.png)

グラフ クエリには、コンテンツの部分 (**Querytext**) とグラフの部分 (**GraphQuery**) の両方を含めることができます。 これらを使用すると、アイテム全体のコンテンツとこの特定のアイテムとユーザーのやり取りを同時に検索できます。 **Querytext** プロパティは必須です。 コンテンツをどの部分もフィルターせずにすべてのアイテムを一致させるには、アスタリスク (*) を使用します。

GQL には **ACTOR** という主要な演算子が 1 つあります。**ACTOR** 演算子は、フィルターに一致する特定のアクターのすべてのアクションを検索し、それらのアクションのオブジェクトをすべて返します。たとえば、Carl Steadman が変更したドキュメントを返す場合、この情報を次のように分割ができます (Carl Steadman の **ActorId** が 1234 だと仮定します)。



- Carl Steadman は **ACTOR** です。
    
- 変更は ID = 1003 の **action** です。
    
- ドキュメントは結果で返される **object** です。
    
その後、次のクエリを記述します。

 `ACTOR(1234, action:1003)`

**ACTOR** 演算子の構文を次に示します。

 `ACTOR(<ActorId> [, filter])`

**ActorId** は、アクションを検索するノードの ID です。 **filter** は、アクターのすべての送信エッジに適用されている述語です。 **filter** は、表 1 の **Action**、**Time** および **Weight** と、ブール演算子 **AND**、**NOT**、および **OR** を組み合わせて構築します。 クエリの結果は、**filter** に一致するすべてのエッジのオブジェクトです。

**ACTOR** 演算子は、**AND** 演算子と **OR** 演算子を使用して結合できます。たとえば、Jarvis Ferro (**ActorId** = 1234) と Austin Ingalls (**ActorId** = 5678) の両方が変更したすべてのアイテムを返すには、次のように記述します。

 `AND(ACTOR(1234, action:1003), ACTOR(5678, action:1003))`

Jarvis Ferro または Austin Ingalls のいずれかが変更したすべてのアイテムを返すには、次のように記述します。

 `OR(ACTOR(1234, action:1003), ACTOR(5678, action:1003))`

認証されたユーザーの **ActorId** を使用する必要があるグラフ クエリを作成する場合、**ME** マクロを同等の代替として使用できます。たとえば、認証されたユーザーが変更したドキュメントを返すには、次のように記述します。

 `ACTOR(ME, action:1003)`


## 使用可能なアクションの種類
<a name="bk_actiontypes"> </a>


**表 2 アクションの種類とその説明**

|**アクションの種類**|**説明**|**表示/非表示**|**ID**|**Weight**|**Timestamp**|
|:-----|:-----|:-----|:-----|:-----|:-----|
| **PersonalFeed**|Delve の **[ホーム]** ビューに表示されるアクターの個人フィード。|プライベート|1021|シーケンス番号|Delve の **[ホーム]** ビューでフィードにアイテムが追加された日時。|
| **更新日時**|アクターが過去 3 か月に変更したアイテム。|パブリック|1003|変更の回数。|最終変更日時|
| **OrgColleague**|アクターとして同じマネージャーに直属するすべてのユーザー。|パブリック|1015|常に 1。|-|
| **OrgDirect**|アクターの直属の部下。|パブリック|1014|常に 1。|-|
| **OrgManager**|アクターの直上の上司。|パブリック|1013|常に 1。|-|
| **OrgSkipLevelManager**|アクターのスキップレベル (直上よりさらに上の) マネージャー。|パブリック|1016|常に 1。|-|
| **WorkingWith**|アクターが高い頻度で連絡をとる、または一緒に仕事をするユーザー。|プライベート|1019|関連性のスコア。|-|
| **TrendingAround**|アクターが高い頻度で一緒に仕事をする、または連絡をとるユーザーに人気のアイテム。|パブリック|1020|関連性のスコア。|-|
| **Viewed**|アクターが過去 3 か月に参照したアイテム。|プライベート|1001|参照回数|最後に参照した日時。|
| **WorkingWithPublic**|**WorkingWith** エッジの公開バージョン。|パブリック|1033|シーケンス番号。|-|

## SharePoint 2013 に対するグラフ クエリの実行
<a name="sectionSection4"> </a>

Office Graph に対してクエリを実行する場合、**GraphQuery** を **KeywordQuery** クラスのプロパティ バッグに追加する必要があります。このプロパティの値は、GQL 形式のグラフ クエリ文字列である必要があります。Carl Steadman (username:carls) などのユーザー ID (**ActorId**) についてクエリを実行する場合は、次の REST クエリを記述します。

```no-highlight
https://<tenant_address>/_api/search/query?Querytext='Username:carls'&amp;SourceId='b09a7990-05ea-4af9-81ef-edfab16c4e31'&amp;SelectProperties='UserName,DocId'
```

**注:** 特定のクエリを実行するために使用する結果のソースは **SourceId** で、b09a7990-05ea-4af9-81ef-edfab16c4e31 は、ユーザー検索の結果ソースの ID です。

出力結果には、次のようにユーザーの ID が含まれます (DocId = 21865248)。




```XML
<d:element m:type="SP.KeyValue">
    <d:Key>DocId</d:Key>
    <d:Value>21865248</d:Value>
    <d:ValueType>Edm.Int64</d:ValueType>
</d:element>

```


### 例

次の例では、Office Graph にクエリを実行する場合に、1 つのアクターと複数のアクターのクエリを記述する方法を示します。これらの例では、アクターとして Carl を使用しています (**ActorId**: 2962)。

これらのクエリが返す結果には最大 10 個のアイテムが含まれています (既定値が 10 であるため)。[この例](..\howto\query-Office-graph-using-gql-with-search-rest-api.md#rowlimit_example)のように、**RowLimit()** プロパティを使用すると、結果数を増やすことができます。


### 単一のアクターのクエリ


- 自分に関連する最初の 10 個のアイテム。
    
     **構文**:  `ACTOR(ME)`

```no-highlight
https://<tenant_address>/_api/search/query?Querytext='*'&amp;Properties='GraphQuery:ACTOR(ME)'
```

- Carl に関連する最初の 10 個のアイテム。
    
     **構文**:  `ACTOR(2962)`

```no-highlight
https://<tenant_address>/_api/search/query?Querytext='*'&amp;Properties='GraphQuery:ACTOR(2962)'
```

- 自分のマネージャー。
    
     **構文**:  `ACTOR(ME, action:1013)`

```no-highlight
https://<tenant_address>/_api/search/query?Querytext='*'&amp;Properties='GraphQuery:ACTOR(ME\,action\:1013)'
```

- 最近変更または表示した最初の 10 個のアイテム。
    
     **構文**:  `ACTOR(ME, OR(action:1001,action:1003))`

```no-highlight
https://<tenant_address>/_api/search/query?Querytext='*'&amp;Properties='GraphQuery:ACTOR(ME\,OR(action\:1001\,action\:1003))'
```

- 2014 年 8 月 15 日に変更した最初の 10 個のアイテム。
    
     **構文**:  `ACTOR(ME, AND(action:1003, time:datetime(2014-08-15)))`

```no-highlight
https://<tenant_address>/_api/search/query?Querytext='*'&amp;Properties='GraphQuery:ACTOR(ME\,AND(action\:1003\,time\:datetime(2014-08-15)))'
```

- 2014 年 6 月 26 日以降に変更した最初の 10 個のアイテム。
    
     **構文**:  `ACTOR(ME, AND(action:1003, time:range(datetime(2014-06-26),max)))`

```no-highlight
https://<tenant_address>/_api/search/query?Querytext='*'&amp;Properties='GraphQuery:ACTOR(ME\,AND(action\:1003\,time\:range(datetime(2014-06-26)\,max)))'
```

### 複数のアクターのクエリ


- ユーザーと Carl に関連する最初の 10 個のアイテム。
    
     **構文**:  `AND(ACTOR(ME), ACTOR(2962))`

```no-highlight
https://<tenant_address>/_api/search/query?Querytext='*'&amp;Properties='GraphQuery:AND(ACTOR(ME)\,ACTOR(2962))'
```

- ユーザーまたは Carl に関連する最初の 10 個のアイテム。
    
     **構文**:  `OR(ACTOR(ME), ACTOR(2962))`

```no-highlight
https://<tenant_address>/_api/search/query?Querytext='*'&amp;Properties='GraphQuery:OR(ACTOR(ME)\,ACTOR(2962))'
```

- ユーザーが最近表示し最近 Carl が変更した最初の 10 個のアイテム。
    
     **構文**:  `AND(ACTOR(ME, action:1001), ACTOR(2962, action:1003))`

```no-highlight
https://<tenant_address>/_api/search/query?Querytext='*'&amp;Properties='GraphQuery:AND(ACTOR(ME\,action\:1001)\,ACTOR(2962\,action\:1003))'
```

### グラフ クエリの結果の形式について

グラフ クエリの結果の形式は、[SharePoint 2013 検索クエリ API](http://msdn.microsoft.com/library/ae9d73ed-1140-430b-9287-01dbbe8ae7d1%28Office.15%29.aspx) によって **Edges** が **RelevantResult** **ResultTable** の一部として 1 列追加されて返されるのを除き、検索クエリの結果に似ています。 **Edges** の形式は、JSON にシリアル化されたエッジの配列です。

たとえば、次のクエリは特定のアクター (**ActorId**:21894957) が共同作業 (**ActionId**:1033) をするユーザー一覧を要求します。

```no-highlight 
https://<tenant_address>/_api/search/query?Querytext='*'&amp;Properties='Graphquery:ACTOR(21894957\,action\:1033)'&amp;SelectProperties='Docid,Title'
```

**Edges** とその他の要素を含む次の XML 構造が出力されます。 この例では、**Edges** 要素が 1 つのみ返されます。

```XML
<d:element m:type="SP.KeyValue">
    <d:Key>Edges</d:Key>
    <d:Value>[{"ActorId":21894957,"ObjectId":21900499,
        "Properties":{"Action":1033,"Blob":[],
        "ObjectSource":1,"Time":"2013-12-02T13:56:25.5979646Z",
        "Weight":61}}]
    </d:Value>
    <d:ValueType>Edm.String</d:ValueType>
</d:element>
```

### グラフ クエリのアクセス制御

グラフ クエリには、検索クエリと同じアクセス制御メカニズムが使用されます。 グラフ クエリでは、ユーザーがアクセスするアイテムのみが返されます。

Office Graph では、そのすべてのエッジに、アクセス制御メカニズムも提供しています。 グラフ内のエッジの各アクション タイプは、プライベートまたはパブリックです。 パブリック エッジは組織内のすべてのユーザーが参照できます。 プライベート エッジはアクターのみが参照できます。 これは、別のユーザーがグラフ クエリを実行する際には無視されます。 たとえば、Carl Steadman は、自身が Office Graph で参照したアイテムをクエリできます。 Carl Steadman の **ActorId** を使用して別のユーザーがこのクエリを実行した場合、空の結果が返されます。 どのアクションの種類がパブリックでどれがプライベートであるかは、表 2 の [表示/非表示] 列に一覧表示しています。


### 高度なクエリの例

さまざまなタスクを遂行するために、各種の方法でグラフ クエリを作成できます。次の各例では、このような高度なクエリを作成する方法に関する簡単なガイドを示します。


#### GraphQuery プロパティを他のクエリ プロパティと組み合わせる


 **例 1**:ユーザーまたは Carl に関連する最初の 10 のアイテムで、結果に **DocId** プロパティと **Edges** プロパティを含めます。

```no-highlight 
https://<tenant_address>/_api/search/query?Querytext='*'&amp;Properties='GraphQuery:OR(ACTOR(ME)\,ACTOR(2962))'&amp;SelectProperties='DocId,Edges'
```

**注**  出力には **DocId** と **Edges** だけでなく、その他のプロパティ (**RankId** や **PartitionId** など) も含まれます。これらは検索サービスによって返される既定のプロパティであるためです。


 **例 2**:自分または Carl に関連する最初の 100 個のアイテム。
<a name="rowlimit_example"> </a>

```no-highlight 
https://<tenant_address>/_api/search/query?Querytext='*'&amp;Properties='GraphQuery:OR(ACTOR(ME)\,ACTOR(2962))'&amp;RowLimit=100
```

グラフ クエリの結果として得られるエッジは、クエリで具体的に要求したものです。しかし、場合によっては、クエリの結果にどのドキュメントが返されるのかに影響を与えずに、追加のエッジの種類を取得したい場合があります。


 **例 3**:ユーザーにトレンド分析されるすべてのドキュメントを取得し (**ActionId**:1020)、ユーザーがこれらのドキュメントを参照および変更したかどうかの情報も取得します。 これには、以下に示す Boolean 構文を使用します。


 **構文**: `AND(
           ACTOR(ME, action:1020), 
           ACTOR(ME, OR(action:1020,action:1001,action:1003)))`

```no-highlight 
https://<tenant_address>/_api/search/query?Querytext='*'&Properties='GraphQuery:AND(ACTOR(ME\,action\:1020)\,ACTOR(ME\,OR(action\:1020\,action\:1001\,action\:1003)))'&SelectProperties='Docid,Title'
```


#### GraphRankingModel プロパティを使用して結果を並べ替える

グラフ クエリによって返される結果は 2 つの方法 (エッジの **Timestamp** またはエッジの **Weight**) で並べ替えることができます。


- エッジの Timestamp に基づいて並べ替えるには、**GraphRankingModel** プロパティを `{"features"\:[{"function"\:"EdgeTime"}]}` と等しくします。
    
- エッジの Weight に基づいて並べ替えるには、**GraphRankingModel** プロパティを `{"features"\:[{"function"\:"EdgeWeight"}]}` と等しくします。
    
いずれの場合も、**RankingModelId** プロパティを `'0c77ded8-c3ef-466d-929d-905670ea1d72'` に設定する必要があります。 結果内のアイテムのオブジェクトにグラフ クエリと一致する複数のエッジがある場合、最も上位の **Timestamp** または **Weight** が使用されます。



 **例 1**:最後に更新した日時を使用して、最近変更したアイテムを並べ替えます。

 **構文**:  `GraphQuery:ACTOR(ME, action:1003) GraphRankingModel:{"features"\:[{"function"\:"EdgeTime"}]} RankingModelId='0c77ded8-c3ef-466d-929d-905670ea1d72'`

```no-highlight
https://<tenant_address>/_api/search/query?Querytext='*'&amp;Properties='GraphQuery:ACTOR(ME\,action\:1003),GraphRankingModel:{"features"\:[{"function"\:"EdgeTime"}]}'
&amp;RankingModelId='0c77ded8-c3ef-466d-929d-905670ea1d72'
```

 **例 2**:親密さに基づいて、一緒に働いているユーザー (**WorkingWith**) を並べ替えます。

 **構文**:  `GraphQuery:ACTOR(ME, action:1019) GraphRankingModel:{"features"\:[{"function"\:"EdgeWeight"}]} RankingModelId='0c77ded8-c3ef-466d-929d-905670ea1d72'`

```no-highlight
https://<tenant_address>/_api/search/query?Querytext='*'&amp;Properties='GraphQuery:ACTOR(ME\,action\:1019),GraphRankingModel:{"features"\:"function"\:"EdgeWeight"}]}'
&amp;RankingModelId='0c77ded8-c3ef-466d-929d-905670ea1d72'
```



複数のアクターのクエリの場合、**actorCombination** というパラメーターを **GraphRankingModel** で使用し、さまざまなアクターのスコア順位を結合する方法を選択できます。


 **例 3**:自分と Carl の周囲で人気のあるドキュメントを検索して、人気の重みの合計で並べ替えます。

 **構文**:  `AND(ACTOR(ME, action:1020), ACTOR(2962, action:1020))
             GraphRankingModel:{"actorCombination"\:"sum"\,"features"\:[{"function"\:"EdgeWeight"}]}
             RankingModelId='0c77ded8-c3ef-466d-929d-905670ea1d72'`

```no-highlight
https://<tenant_address>/_api/search/query?Querytext='*'&Properties='GraphQuery:AND(ACTOR(ME\, action\:1020)\,ACTOR(2962\,action\:1020)),
GraphRankingModel:{ "actorCombination"\:"sum"\,"features"\:[{"function"\:"EdgeWeight"}]}'
&RankingModelId='0c77ded8-c3ef-466d-929d-905670ea1d72'
```



**actorCombination** パラメーターは、"min"、"max" および "sum" の値をサポートしています。既定値は "max" です。 

#### GraphQuery プロパティをクエリ結果またはフルテキスト クエリと組み合わせる



 **例**:**GraphQuery** と **Querytext**='Title:design' を結合して、タイトルに "design" がある最近参照したすべてのアイテムをクエリします。

 **構文**:  `GraphQuery:ACTOR(ME, action:1001) Querytext='Title:design'`

```no-highlight
https://<tenant_address>/_api/search/query?Querytext='Title:design'&amp;Properties='GraphQuery:ACTOR(ME)\,action\:1001))
```

クエリを変更する場合は、**Querytext** に任意のクエリを使用できます。



#### 結果を変更するための GraphRestrictionMode クエリ プロパティの使用
グラフ クエリを実行した場合、既定の戻り値はグラフの交差部分とコンテンツの結果です。ただし、Office Graph でコンテンツの結果のみを返す場合は、**GraphRestrictionMode** プロパティを false に設定します。グラフ クエリに一致するすべてのエッジもこの結果の一部として返されます。



 **例**:**GraphQuery** と **Querytext**='Title:design' を結合し、タイトルに "design" がある最近参照したすべてのアイテムをクエリします。

 **構文**:  `GraphQuery:ACTOR(ME, action:1001) Querytext='design'
            GraphRestrictionMode:false`

```no-highlight
https://<tenant_address>/_api/search/query?Querytext='design'&amp;Properties='GraphQuery:ACTOR(ME)\,action\:1001),GraphRestrictionMode:false'&SelectProperties='Docid,Title'
```



## その他のリソース
<a name="bk_addresources"> </a>


-  [Office 365 API プラットフォームの概要](..\howto\platform-development-overview.md)
    
-  [Delve とは?](http://support.office.com/article/1315665a-c6af-4409-a28d-49f8916878ca)
    
-  [Office 365 管理者向け Delve](http://support.office.com/article/54f87a42-15a4-44b4-9df0-d36287d9531b)
    

