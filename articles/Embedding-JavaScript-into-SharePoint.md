# SharePoint への JavaScript の埋め込み

名前空間を使用すると、自身が作成した JavaScript によるカスタマイズと、標準の SharePoint JavaScript または他の開発者が展開した JavaScript によるカスタマイズとの間の競合を避けることができます。 

[OfficeDev/PnP](https://github.com/OfficeDev/PnP/) のサンプルやソリューションには、しばしば JavaScript コードが含まれています。手法をわかりやすくするため、これらのサンプルは通常単純になっており、JavaScript コードを SharePoint に埋め込む際に名前空間が使用されていません。PnP サンプルをソリューションに組み込む際は、この記事で示されている簡単な手順に確実に従うことが重要です。

## 名前空間の使用が重要な理由
<a name="sectionSection0"> </a>

JavaScript は、緩やかに型指定された言語です。 変数や関数を定義するときに、現在のコンテキストに同じ名前の変数や関数がすでに存在していると、既存の値または実装は新しい値または実装によって置き換えられます。
その結果、JavaScript コードを SharePoint に埋め込むと、標準の SharePoint JavaScript コードや他の開発者が展開したカスタマイズが簡単に上書きされます。
これにより発生する可能性のある競合は、識別とデバッグが困難です。

これを避けるため、JavaScript コード用にカスタム名前空間を使用することをお勧めします。

## 名前空間を使用する方法
<a name="sectionSection1"> </a>

次の例は、名前空間とクラスで JavaScript コードを整理するために使用される単純なパターンを示しています。

```JavaScript
var MySolution = MySolution || {};

MySolution.MyClass1 = (function () {
    // private members
    var privateVar1 = 1;
    var privateVar2 = 2;
    
    function privateFunction1(){
      return "";
    }
    
    return {
        // public interface
        myFunction1: function() {
          return privateVar1;
        }
        myFunction2: function(){
          return privateVar2;
        }
    };
})();
```

パブリック インターフェイスで定義された関数は次のものとして呼び出すことができます。

```JavaScript
MySolution.MyClass1.myFunction1();

MySolution.MyClass1.myFunction2();
```

すべてのコードでカスタムの名前空間 *MySolution* を使用しているため、名前付けの競合を避けることができます。

## 名前空間とダウンロード最小化戦略 (MDS)

[ダウンロード最小化戦略機能](https://msdn.microsoft.com/en-us/library/office/dn456544.aspx)を有効にすると、MDS ナビゲーションでグローバル名前空間と変数が解除されます。   
名前空間を保持するには、次のように宣言します。

```JavaScript
    Type.registerNamespace('MySolution');
```

Type 名前空間は SharePoint に固有です。一般的な JavaScript ライブラリの場合は、以下を使用します。

```JavaScript
if (window.hasOwnProperty('Type')) {
    Type.registerNamespace('MySolution');
} else {
    window.MySolution = window.MySolution || {};
}
```

#### 名前空間、MDS および CSR (クライアント側レンダリング)

``RegisterModuleInit`` 関数は適切な ``Type`` 名前空間を宣言します。  
JSLink に添付されているファイルは MDS ナビゲーションでは再実行**されません**。これには、AsyncDeltaManager 関数を使用します。

### リソース

* [Wictor Wilén - SharePoint MDS の有効化されたサイトで JavaScript 関数を実行する正しい方法](http://www.wictorwilen.se/the-correct-way-to-execute-javascript-functions-in-sharepoint-2013-mds-enabled-sites)
* [Hugh Wood - SharePoint JavaScript context development - AsyncDeltaManager](https://www.spcaf.com/blog/sharepoint-javascript-context-development-part-4-the-way-of-the-async-delta-manager/)
* [Marc Anderson - MDS の (再) 読み込みの後に JavaScript を実行する](http://blog.symprogress.com/2013/09/sharepoint-2013-execute-javascript-function-after-mds-load/)
