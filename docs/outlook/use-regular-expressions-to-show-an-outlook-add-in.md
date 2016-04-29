
# 正規表現アクティブ化ルールを使用して Outlook アドインを表示する
読み取りシナリオで有効となり、Outlook の UI にアドインが表示されるようにするための Outlook アドインの条件を正規表現を使用して指定する方法を説明します。

 _ **適用対象:** apps for Office?| Office Add-ins?| Outlook_


## 正規表現のサポート


読み取りシナリオで Outlook アドインがアクティブ化されるように、正規表現ルールを指定することができます。ユーザーが閲覧ウィンドウまたはインスペクターでメッセージまたは予定を表示したときに、Outlook は正規表現ルールを評価してアドインをアクティブにすべきかどうかを判断します。ユーザーがアイテムを作成しているときは、Outlook はこれらのルールを評価しません。Information Rights Management (IRM) でアイテムが保護されていたり、迷惑メール フォルダーにある場合など、Outlook がアドインをアクティブにしないシナリオは他にもあります。詳細については、「[](../outlook/manifests/activation-rules.md#MailAppDefineRules_Activation)」をご覧ください。

正規表現はアドインの XML マニフェストで [ItemHasRegularExpressionMatch](http://msdn.microsoft.com/ja-jp/library/bfb726cd-81b0-a8d5-644f-2ca90a5273fc%28Office.15%29.aspx) ルールまたは [ItemHasKnownEntity](http://msdn.microsoft.com/ja-jp/library/87e10fd2-eab4-c8aa-bec3-dff92d004d39%28Office.15%29.aspx) ルールの一部として指定できます。Outlook はクライアント コンピューター上のブラウザーで使用されている JavaScript インタープリターのルールに基づいて正規表現を評価します。また、Outlook はすべての XML プロセッサーがサポートしているものと同じ特殊文字のリストをサポートします。これらの特殊文字を下の表に示します。正規表現内でこれらの文字を使用するには、下の表内の対応する文字のエスケープ シーケンスを指定します。



|**文字**|**説明**|**使用するエスケープ シーケンス**|
|:-----|:-----|:-----|
|"|二重引用符|&amp;quot;|
|&amp;|アンパサンド|&amp;amp;|
|'|アポストロフィ|&amp;apos;|
|<|より小さい|&amp;lt;|
|>|より大きい|&amp;gt;|

## ItemHasRegularExpressionMatch ルール


 **ItemHasRegularExpressionMatch** ルールは、サポートされるプロパティの特定の値に基づいてアドインのアクティブ化を制御する際に役立ちます。 **ItemHasRegularExpressionMatch** ルールには次の属性があります。



|**属性名**|**説明**|
|:-----|:-----|
|**RegExName**|アドインのコードで参照できるように、正規表現の名前を指定します。|
|**RegExValue**|アドインを表示するかどうかを判断するために評価する正規表現を指定します。|
|**PropertyName**|正規表現の評価対象となるプロパティの名前を指定します。指定できる値は、 **BodyAsHTML**、 **BodyAsPlaintext**、 **SenderSMTPAddress**、および  **Subject** です。 **BodyAsHTML** を指定すると、Outlook はアイテムの本文が HTML である場合にのみ正規表現を適用し、それ以外の場合はその正規表現に一致するものを返しません。予定は常にリッチ テキスト形式で保存されるため、 **BodyAsHTML** を指定した正規表現が予定アイテムの本文の文字列と一致することはありません。 **BodyAsPlaintext** を指定すると、Outlook はアイテムの本文に対して正規表現を常に適用します。|
|**IgnoreCase**|**RegExName** で指定された正規表現のマッチングで大文字と小文字の違いを無視するかどうかを指定します。|

### ルールで正規表現を使用する際のベスト プラクティス

正規表現を使用する場合は、次の点に特に注意してください。


- アイテムの本文に対して  **ItemHasRegularExpressionMatch** ルールを指定した場合は、正規表現で本文をさらにフィルター処理する必要があります。アイテムの本文全体を返す正規表現は使用しないでください。 `.*` などの正規表現を使用してアイテムの本文全体を取得しても、必ずしも期待どおりの結果が返されるとは限りません。
    
- ブラウザーで返されるプレーン テキストの本文は、ブラウザーによって微妙に異なる場合があります。 **PropertyName** 属性に **BodyAsPlaintext** を指定した [ItemHasRegularExpressionMatch](http://msdn.microsoft.com/ja-jp/library/bfb726cd-81b0-a8d5-644f-2ca90a5273fc%28Office.15%29.aspx) ルールを使用する場合は、アドインがサポートするすべてのブラウザーで正規表現をテストしてください。
    
    選択されたアイテムの本文を取得する方法はブラウザーによって異なるため、本文の一部として返される可能性がある小さな違いに正規表現が対応していることを確認する必要があります。たとえば、Internet Explorer 9 など、DOM の  **innerText** プロパティを使用するブラウザーもあれば、Firefox など、 **.textContent()** メソッドを使用してアイテムの本文を取得するブラウザーもあります。また、改行もブラウザーによって異なります。つまり、改行は、Internet Explorer では "\r\n" として、Firefox と Chrome では "\n" として返されます。詳細については、「 [W3C DOM Compatibility - HTML](http://www.quirksmode.org/dom/w3c_html.mdl#t07)」を参照してください。
    
- アイテムの HTML 本文は、Outlook リッチ クライアントと Outlook Web App または デバイス用 OWA で若干異なります。正規表現を定義するときには注意が必要です。例として、 **PropertyName** 属性値として **BodyAsHTML** を指定した **ItemHasRegularExpressionMatch** ルールで使用される次の正規表現について考えてみましょう。
    
  ```
  http.*\.contoso\.com
  ```


    ルールでこの正規表現を使用した場合、Outlook リッチ クライアントでは、アイテムの HTML 本文に次の  **META** タグの一部として含まれる文字列 "http-equiv="Content-Type" と一致します。
    


  ```HTML
  <META HTTP-EQUIV="Content-Type" CONTENT="text/html; charset=us-ascii">
  ```


    これらのホストの HTML 本文にはこの  **META** タグは含まれていないため、Outlook Web App と デバイス用 OWA で同じルールを使用してもこの一致は返されません。これは、さまざまな Outlook クライアントでアドインが適切にアクティブ化されるかどうかに影響する可能性があります。この例では、代わりに次の正規表現を使用します。
    


  ```
  http://.*\.contoso\.com/
  ```

- ホスト アプリケーション、デバイスの種類、または正規表現を適用するプロパティに応じて、ホストごとに、アクティブ化ルールとして正規表現を設計するときに認識しておく必要がある、ベスト プラクティスと制限事項が他にもあります。詳細については、「 [Outlook アドインのアクティブ化と JavaScript API の制限](../outlook/limits-for-activation-and-javascript-api-for-outlook-add-ins.md)」を参照してください。
    

### 例

次の  **ItemHasRegularExpressionMatch** ルールでは、大文字小文字に関係なく、送信者の SMTP メール アドレスが "@contoso" と一致した場合にアドインをアクティブにします。


```XML
<Rule xsi:type="ItemHasRegularExpressionMatch" 
    RegExName="addressMatches" 
    RegExValue="@[cC][oO][nN][tT][oO][sS][oO]" 
    PropertyName="SenderSMTPAddress"
/>
```

次の例では、 **IgnoreCase** 属性を使用して同じ正規表現を指定しています。




```XML
<Rule xsi:type="ItemHasRegularExpressionMatch" 
    RegExName="addressMatches" 
    RegExValue="@contoso" 
    PropertyName="SenderSMTPAddress"
    IgnoreCase="true"
/>
```

次の  **ItemHasRegularExpressionMatch** ルールでは、現在のアイテムの本文に株式銘柄コードが含まれている場合にアドインをアクティブにします。




```XML
<Rule xsi:type="ItemHasRegularExpressionMatch" 
    PropertyName="BodyAsPlaintext" 
    RegExName="TickerSymbols" 
    RegExValue="\b(NYSE|NASDAQ|AMEX):\s*[A-Za-z]+\b"/>

```


## ItemHasKnownEntity ルール


 **ItemHasKnownEntity** ルールでは、選択されたアイテムの件名または本文に特定のエンティティが存在するかどうかに基づいてアドインをアクティブにします。 [KnownEntityType](http://msdn.microsoft.com/ja-jp/library/432d413b-9fcc-eb50-cfea-0ed10a43bd52%28Office.15%29.aspx) 型は、サポートされるエンティティを定義します。エンティティの値のサブセット (URL の特定のセットや、特定の市外局番の電話番号など) に基づいてアクティブにする場合は、 **ItemHasKnownEntity** ルールで正規表現を適用すると便利です。


 >**メモ**  マニフェストに指定されている既定のロケールに関係なく、Outlook が抽出できるのは英語のエンティティ文字列だけです。  **MeetingSuggestion** エンティティ型をサポートしているのはメッセージだけであり、予定ではサポートされていません。 [送信済みアイテム] フォルダーのアイテムからエンティティを抽出することはできません。また、[ItemHasKnownEntity](http://msdn.microsoft.com/ja-jp/library/87e10fd2-eab4-c8aa-bec3-dff92d004d39%28Office.15%29.aspx) ルールを使用して、 [送信済みアイテム] フォルダーのアイテムに対してアドインをアクティブにすることもできません。

 **ItemHasKnownEntity** ルールは、次の表に示す属性をサポートしています。 **ItemHasKnownEntity** ルールでの正規表現の指定は任意ですが、正規表現をエンティティ フィルターとして使用する場合は、 **RegExFilter** 属性と **FilterName** 属性の両方を指定する必要があります。



|**属性名**|**説明**|
|:-----|:-----|
|**EntityType**|ルールで  **true** に評価するために存在する必要があるエンティティの種類を指定します。複数の種類のエンティティを指定するには、複数のルールを使用します。|
|**RegExFilter**|**EntityType** で指定されているエンティティのインスタンスをさらにフィルター処理する正規表現を指定します。|
|**FilterName**|**RegExFilter** で指定されている正規表現の名前を指定し、それ以降にコードでその正規表現を参照できるようにします。|
|**IgnoreCase**|**RegExFilter** で指定された正規表現のマッチングで大文字と小文字の違いを無視するかどうかを指定します。|

### 例

次の  **ItemHasKnownEntity** ルールでは、現在のアイテムの件名または本文に URL が含まれており、その URL に大文字小文字を問わず "youtube" という文字列が含まれている場合にアドインをアクティブにします。


```XML
<Rule xsi:type="ItemHasKnownEntity" 
    EntityType="Url" 
    RegExFilter="youtube"
    FilterName="youtube"
    IgnoreCase="true"/>
```


## コードでの正規表現の結果の使用


現在のアイテムで次のメソッドを使用して、正規表現に一致するものを取得できます。


- [getRegExMatches](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md) は、アドインの **ItemHasRegularExpressionMatch** ルールと **ItemHasKnownEntity** ルールで指定されているすべての正規表現について、現在のアイテムで一致するものを返します。
    
- [getRegExMatchesByName](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md) は、アドインの **ItemHasRegularExpressionMatch** ルールで指定されている正規表現について、現在のアイテムで一致するものを返します。
    
- [getFilteredEntitiesByName](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md) は、アドインの **ItemHasKnownEntity** ルールで指定されている正規表現について、一致するものを含むエンティティのインスタンス全体を返します。
    
正規表現が評価されると、一致するものが配列オブジェクトでアドインに返されます。 **getRegExMatches** の場合、このオブジェクトには正規表現の名前の識別子が含まれます。


 >**メモ**  Outlook リッチ クライアントは、一致を配列で返すときに特定の順序で返すわけではありません。また、同じメールボックスの同じアイテムに対して、クライアントごとに同じアドインを実行しても、Outlook リッチ クライアントはこの配列で一致するものを Outlook Web App または デバイス用 OWA と同じ順序で返すわけではありません。Outlook リッチ クライアントと Outlook Web App または デバイス用 OWA での正規表現の処理における他の違いについては、「 [Outlook アドインのアクティブ化と JavaScript API の制限](../outlook/limits-for-activation-and-javascript-api-for-outlook-add-ins.md)」をご覧ください。


### 例

 `videoURL` という名前の正規表現を使用する **ItemHasRegularExpressionMatch** ルールを含むルール コレクションの例を次に示します。


```XML
<Rule xsi:type="RuleCollection" Mode="And">
    <Rule xsi:type="ItemIs" ItemType="Message"/>
    <Rule xsi:type="ItemHasRegularExpressionMatch" RegExName="VideoURL" RegExValue="http://www\.youtube\.com/watch\?v=[a-zA-Z0-9_-]{11}" PropertyName="Body"/>
</Rule>
```

次の例では、現在のアイテムの  **getRegExMatches** を使用して、変数 `videos` を前の **ItemHasRegularExpressionMatch** ルールの結果に設定します。




```
var videos = Office.context.mailbox.item.getRegExMatches().videoURL;
```

このオブジェクトには、複数の一致が配列要素として格納されます。次のコード例は、 `reg1` という名前の正規表現に一致するものを反復処理して、HTML として表示する文字列を作成する方法を示しています。




```
function initDialer() 
{
    var myEntities;
    var myString;
    var myCell;
    myEntities = _Item.getRegExMatches();

    myString = "";
    myCell = document.getElementById('dialerholder');
    // Loop over the myEntities collection.
    for (var i in myEntities.reg1) {
        myString += "<p><a href='callto:tel:" + myEntities.reg1[i] + "'>" + myEntities.reg1[i] + "</a></p>";
    }
    myCell.innerHTML = myString;
}

```

次の例では、 **MeetingSuggestion** エンティティと `CampSuggestion` という名前の正規表現を指定した **ItemHasKnownEntity** ルールを示します。Outlook は、現在選択されているアイテムに会議の提案が含まれ、件名または本文に "WonderCamp" という言葉が含まれていることを検出すると、アドインをアクティブにします。




```XML
<Rule xsi:type="ItemHasKnownEntity" 
    EntityType="MeetingSuggestion"
    RegExFilter="WonderCamp"
    FilterName="CampSuggestion"
    IgnoreCase="false"/>
```

次のコード例では、現在のアイテムの  **getFilteredEntitiesByName(name)** を使用して変数 `suggestions` を設定し、前の **ItemHasKnownEntity** ルールで検出された会議の提案の配列を取得します。




```
var suggestions = Office.context.mailbox.item.getFilteredEntitiesByName(CampSuggestion);
```


## その他の技術情報



- [閲覧フォーム用の Outlook アドインを作成する](../outlook/read-scenario.md)
    
- [Outlook アドインのアクティブ化ルール](../outlook/manifests/activation-rules.md)
    
- [Outlook アドインのアクティブ化と JavaScript API の制限](../outlook/limits-for-activation-and-javascript-api-for-outlook-add-ins.md)
    
- [Outlook アイテム内の文字列を既知のエンティティとして照合する](../outlook/match-strings-in-an-item-as-well-known-entities.md)
    
- [.NET Framework での正規表現に関するベスト プラクティス](http://msdn.microsoft.com/ja-jp/library/618e5afb-3a97-440d-831a-70e4c526a51c%28Office.15%29.aspx)
    
