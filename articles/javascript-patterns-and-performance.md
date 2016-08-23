# JavaScript のパターンとパフォーマンス

数年前、ASP.NET はサーバー側 UI コントロールのレンダリングを提供しており、それで問題はありませんでした。ただし、サーバー側のレンダリングでは、完全信頼コードが必要です。SharePoint と Office 365 に移行された現在では、完全信頼コードは使用されなくなりました。つまり、サーバー側の UI コントロールのレンダリングはもう機能しなくなっています。

しかし、ビジネスではまだ、それらの Web サイトやアプリのカスタム UI の機能が必要とされています。つまり、カスタム UI 機能をサーバー側からクライアント側へ移す必要があります。 

クライアント側の JavaScript は、現在の UI コントロールのレンダリング方法です。

## 
            <a name="JavaScriptPatterns"></a> JavaScript のパターン

クライアント側の JavaScript はパスですが、クライアント側の JavaScript を実装する最善の方法は何でしょうか。 どのように開始すればよいでしょうか。

いくつかのオプションがあります。

|オプション|説明|
|:---|:---|
|JavaScript の埋め込み | 
                [Site.UserCustomActions](https://msdn.microsoft.com/EN-US/library/office/microsoft.sharepoint.client.site.usercustomactions.aspx) または [Web.UserCustomActions](https://msdn.microsoft.com/EN-US/library/office/microsoft.sharepoint.client.web.usercustomactions.aspx) を使用することで、ページのマークアップに直接スクリプトを含めることができます。 この方法は、以下で説明する[ローダー パターン](#LoaderPattern)で使用されています。|
|表示テンプレート | ビューと検索に適用されます。アプリケーションやプロバイダー ホスト型のコードを展開する必要はありません。カスタム ビューへのスタイル ライブラリ (例) のビューにアップロードできる JavaScript ファイルです。JavaScript を使用して必要なビューを作成できます。|
|SharePoint ホスト型アドイン | ホスト Web またはアドイン Web への返信には、JSOM を使用します。クロス ドメイン呼び出しのため、Web プロキシへのアクセスを提供します。|
|プロバイダー ホスト型アドイン | セキュリティで保護された SharePoint との統合を維持しながら、さまざまなテクノロジの積み重ねで複雑なアプリケーションを作成できます。|
|JSLink | 1 つ以上の JavaScript ファイルを多くの OOTB Web パーツに読み込むことができます。|
|ScriptEditor Webpart | スクリプトを直接含むか、スクリプト タグとマークアップを使用して読み込み、SharePoint サイト内で完全にホストされた 1 ページのアプリケーションを作成します。|

状況に応じたように別のオプションを使用した方が良いと思われる場合は、これらの選択肢が固定されていると思い込まないでください。 

## 
            <a name="JavaScriptPerformance"></a> JavaScript のパフォーマンス

開発プロセスの各ステップで、パフォーマンスに留意することが重要です。JavaScript のパフォーマンスに大きな違いを生むいくつかのポイントを以下に示します。

|オプション|説明|
|:---|:---|
|[要求の数を減らす](#ReduceTheNumberOfRequests) | 要求の数が少なければ、サーバーへのラウンド トリップも少なくなり、遅延時間が減少します。|
|[必要なデータのみを取得する](#RetrieveOnlyTheDataYouNeed) | ネットワーク経由で送信されるデータ量を削減します。 また、サーバーの負荷を軽減します。|
|[適切なページ読み込みエクスペリエンスを提供する](#ProvideAGoodUserExperience) | ユーザーへの UI の応答性を保持します。 たとえば、100 個以上のレコードのダウンロードを開始する*前に*ページのメニューを更新します。|
|[可能な限りの非同期呼び出しとパターンを使用する](#EverythingIsAsynchronous) | ポーリングでは、非同期の呼び出しやコールバックを使用するよりパフォーマンスに重い負担がかかります。| 
|[キャッシュが重要](#ClientSideCaching) | キャッシュはパフォーマンスをただちに改善し、サーバーの負荷をさらに軽減します。|
|[想像より多くのページ ビューに備える](#PriceOfPopularity) | ヒットが数個しかない場合は、データ負荷の高いランディング ページでも問題ありません。数千ものヒットがある場合、これはパフォーマンスに大きな影響を与えます。|

## 
            <a name="WhatIsMyCodeDoing"></a> コードの実行内容

パフォーマンスについては、自分のコードがいつ何を実行するかを知っておくことが重要です。これにより、効率を向上させる方法を特定できます。これを実行するための優れた方法をいくつか次に示します。

### 
            <a name="ReduceTheNumberOfRequests"></a> 要求の数を減らす

常に要求数はできるだけ少なく、サイズは小さく保ってください。要求を削除するたびに、サーバーやクライアントにかかるパフォーマンスの負荷が軽減されます。要求のサイズを小さくすることで、パフォーマンスの負荷をさらに削減します。

要求数を削減し、要求のサイズを小さくする方法はいくつかあります。

- 本番環境での JavaScript ファイルの数を制限します。JavaScript ファイルの分離は開発には適していますが、本番環境にはあまり適していません。JavaScript ファイルを組み合わせて 1 つの JavaScript ファイルにするか、JavaScript ファイルの数をできるだけ少なくします。
- ファイルのサイズを縮小します。改行、空白、およびコメントを削除することによって、本番環境の JavaScript ファイルを最小限に抑えます。JavaScript ファイルのサイズを大幅に削減できる JavaScript プログラムや Web サイトがいくつかあります。
- ブラウザー ファイルのキャッシュを使用して要求を減らします。 以下の更新された[ローダー パターン](#LoaderPattern)を使用すると、この概念について詳しく説明することができます。

### <a name"RetrieveOnlyTheDataYouNeed"></a> 必要なデータのみを取得する

データを要求するときは、実際に必要なものに要求の焦点を当てるようにしてください。たとえば、タイトルを取得するために記事全体をダウンロードすると、パフォーマンスが大幅に低下します。

- サーバーのフィルタリング、選択、および制限を使用してネットワーク経由のトラフィックを最小限に抑えます。
- もう 1 つの例として、最初の 5 つの記事のみが必要な場合にすべての記事を要求しないでください。
- 1 つのプロパティのみが必要なときにプロパティ バッグ全体を要求しないでください。たとえば、スクリプトでプロパティの 1 つだけが必要なところでプロパティ バッグを全体を要求すると、容量は 800 KB にもなります。また、オブジェクトのサイズは時間の経過と共に変化する場合があることにも注意してください。今数キロバイトしかなくても、製品ライフ サイクルの後半になると、メガバイト単位になる可能性があります。 

### 
            <a name="DontRequestDataYouWillDiscard"></a> 使用せずに破棄するようなデータを要求しない

実際に使用するより多くのデータを取得する場合は、最初のクエリにフィルターを適用するチャンスであると考えてください。 

- レコード全体ではなく、名前や住所など、必要なフィールドのみを要求します。
- 具体的で計画的なフィルター要求を行います。たとえば、利用可能な記事のリストを作成する場合は、タイトル、PublishingDate、および作成者を取得します。それ以外のフィールドは要求には含めず残します。

### 
            <a name="ProvideAGoodUserExperience"></a> 優れたユーザー エクスペリエンスを提供する

不自然で一貫性のないユーザー インターフェイスは、パフォーマンスだけでなく、体感的なパフォーマンスにも影響を与えます。スムーズなエクスペリエンスを提供するようにコードを記述します。

- スピン コントロールを使用して、処理を読み込み中または時間がかかっていることを示します。
- コードの実行順序を理解し、最適なユーザー エクスペリエンスのために形成します。たとえば、サーバーから大量のデータを取得し、メニューを非表示にしてユーザー インターフェイスを変更しようとしている場合は、最初にメニューを非表示にします。これにより、ユーザーの UI エクスペリエンスのふらつきを防ぎます。

## 
            <a name="EverythingIsAsynchronous"></a> すべては非同期

ブラウザー内のすべてのコード活動は非同期であると見なしてください。ファイルはある順序で読み込まれます。DOM の読み込むを待機する必要があり、SharePoint への要求は異なる速度で完了します。 

- 時間内のコードの動作を理解します。
- ポーリングではなく、イベントとコールバックを使用します。
- Promise を使用します。 jQuery では、**Deferred** オブジェクトと呼ばれます。 Q、WinJS、ES6 には同様の概念があります。
- 同期呼び出しを支持する非同期呼び出しを使用します。
- 遅延が発生する可能性がある場合は常に非同期を使用します。
    - AJAX 要求の間。
    - 重要な DOM 操作の間。

非同期パターンは、パフォーマンスと応答性を向上させ、依存アクションの効果的な組み合わせを可能にします。

## 
            <a name="ClientSideCaching"></a> クライアント側キャッシュ

クライアント側キャッシュは、使われないことが最も多いパフォーマンス拡張機能の 1 つであり、コードに追加できます。

データをキャッシュできる場所は 3 つあります。

|オプション|説明|
|:---|:---|
|セッション ストレージ|クライアント上にキーと値のペアとしてデータを格納します。 これは、常に文字列として格納されるセッションごとのストレージです。 <br /> JSON.stringify() は、JavaScript オブジェクトを、オブジェクトを格納する文字列に変換します。
|ローカル ストレージ|<p>クライアント上にキーと値のペアとしてデータを格納します。 これは、常に文字列として格納されるセッション間で永続的です。</p><p>JSON.stringify() は、JavaScript オブジェクトを、オブジェクトを格納する文字列に変換します。</p>|
|ローカル データベース|クライアント上にリレーショナル データを保存します。 SQL-Lite をデータベース エンジンとして頻繁に使用します。<br>ローカル データベースのストレージをすべてのブラウザーで使用できない場合があります。&mdash;ターゲット ブラウザーのサポートを確認してください

キャッシュを作成するときは、利用可能な容量の制限と、データの鮮度に注意してください。 
- 容量の制限の終わりに到達すると、古いキャッシュ データまたは重要度の低いキャッシュ データを削除する方が良い場合があります。 
- データの種類が異なると、他のデータより古くなるのが早い場合があります。ニュース記事の一覧は 5 ～ 10 分で古くなる可能性がありますが、ユーザーのプロファイル名は 24 時間以上の安全にキャッシュできることがよくあります。 

ローカルおよびセッションのストレージには、組み込みの有効期限がありませんが、cookie にはあります。保存済みデータを cookie とリンク付、ローカル ストレージとセッション ストレージに有効期限を追加することができます。また、有効期限の日付を含むストレージ ラッパーを作成し、これをコード内で確認することもできます。

## 
            <a name="PriceOfPopularity"></a>人気の価格

ページはどのくらいの頻度で閲覧されていますか。従来のシナリオでは、企業のホームページは組織全体で全ブラウザーの起動ページに設定されます。すると突然、トラフィックがそれまでの想像を超えて多くなります。コンテンツのすべてのバイトが突然、ホーム ページが使用する、サーバーのパフォーマンスと帯域幅に拡大されます。

ソリューション:ホーム ページを軽量化し、他のコンテンツにリンクさせます。

また、データ量の多いダッシュボードも、単独で拡張できるプロバイダー ホスト型アプリケーションの候補です。

## 
            <a name="LoaderPattern"></a> ローダー パターン

ローダー パターンの目的は、サイトを更新せずに、不明な数のリモート スクリプトをサイトに埋め込む方法を提供することです。更新プログラムは、CDN をリモートで行うことができ、すべてのサイトが更新されます。

ローダー パターンは、ファイルがキャッシュされないようにするために、URL の末尾に日付とタイム スタンプを付けて構築します。ローダー ファイル上で依存関係として jQuery を設定し、その後ローダーで関数を実行します。これにより、jQuery が読み込みを完了した後、カスタムの JavaScript を読み込むようにします。

PnP-dev\Samples\Core.JavaScript\Core.JavaScript.Embedder\Program.cs:

```csharp
static void Main(string[] args)
{
    ContextManager.WithContext((context) =>
        // this is the script block that will be embedded into the page
        // in practice this can be done during provisioning of the site/web
        // make sure to include ';' at end to play nice with page embedding
        // using the script on demand feature built into SharePoint we load jQuery, then our remote loader(pnp-loader.js or pnp-loader-cached.js) file using a dependency
        var script = @"(function (loaderFile, nocache) {
                                var url = loaderFile + ((nocache) ? '?' + encodeURIComponent((new Date()).getTime()) : '');
                                SP.SOD.registerSod('pnp-jquery.js', 'https://localhost:44324/js/jquery.js');
                                SP.SOD.registerSod('pnp-loader.js', url);
                                SP.SOD.registerSodDep('pnp-loader.js', 'pnp-jquery.js');
                                SP.SOD.executeFunc('pnp-loader.js', null, function() {});
                        })('https://localhost:44324/pnp-loader.js', true);";


        // this version of the script along with pnp-loaderMDS.js (or pnp-loaderMDS-cached.js) handles pages where the minimum download strategy is active
        var script2 = @"ExecuteOrDelayUntilBodyLoaded(function () {
                            var url = 'https://localhost:44324/js/pnp-loaderMDS.js?' + encodeURIComponent((new Date()).getTime());
                            SP.SOD.registerSod('pnp-jquery.js', 'https://localhost:44324/js/jquery.js');
                            SP.SOD.registerSod('pnp-loader.js', url);
                            SP.SOD.registerSodDep('pnp-loader.js', 'pnp-jquery.js');
                            SP.SOD.executeFunc('pnp-loader.js', null, function () {
                                if (typeof pnpLoadFiles === 'undefined') {
                                    RegisterModuleInit('https://localhost:44324/js/pnp-loaderMDS.js', pnpLoadFiles);
                                } else {
                                    pnpLoadFiles();
                                }
                            });    
                        });";

        // load the collection of existing links
        var links = context.Site.RootWeb.UserCustomActions;
        context.Load(links, ls => ls.Include(l => l.Title));
        context.ExecuteQueryRetry();

        // this block handles deleting previous test custom actions
        var doDelete = false;

        foreach (var link in links.ToArray().Where(l => l.Title.Equals("MyTestCustomAction", StringComparison.OrdinalIgnoreCase)))
        {
            link.DeleteObject();
            doDelete = true;
        }

        if (doDelete)
        {
            context.ExecuteQueryRetry();
        }

        // now we embed our script into the user custom action
        var newLink = context.Site.RootWeb.UserCustomActions.Add();
        newLink.Title = "MyTestCustomAction";
        newLink.Description = "Doing some testing.";
        newLink.ScriptBlock = script2;
        newLink.Location = "ScriptLink";
        newLink.Update();
        context.ExecuteQueryRetry();
    });
}
```

`SP.SOD.registerSodDep('pnp-loader.js', 'pnp-jquery.js');` は依存関係を設定し、`SP.SOD.executeFunc('pnp-loader.js', null, function() {});` はカスタムの Javascript を読み込む前に jQuery を完全に読み込むよう強制します。

`newLink.ScriptBlock = script2;` と `newLink.Location = "ScriptLink";` は、これをユーザーの顧客操作に追加する主要部分です。

次に、pnp-loader.js ファイルは JavaScript ファイルのリストと、各ファイルの読み込み時に実行可能な Promise を読み込みます

PnP-dev\Samples\Core.JavaScript\Core.JavaScript.CDN\js\pnp-loader.js:

```javascript
(function () {

    var urlbase = 'https://localhost:44324';
    var files = [
        '/js/pnp-settings.js',
        '/js/pnp-core.js',
        '/js/pnp-clientcache.js',
        '/js/pnp-config.js',
        '/js/pnp-logging.js',
        '/js/pnp-devdashboard.js',
        '/js/pnp-uimods.js'
    ];

    // create a promise
    var promise = $.Deferred();

    // this function will be used to recursively load all the files
    var engine = function () {

        // maintain context
        var self = this;

        // get the next file to load
        var file = self.files.shift();

        var fullPath = urlbase + file;

        // load the remote script file
        $.getScript(fullPath).done(function () {
            if (self.files.length > 0) {
                engine.call(self);
            }
            else {
                self.promise.resolve();
            }
        }).fail(self.promise.reject);
    };

    // create our "this" we will apply to the engine function
    var ctx = {
        files: files,
        promise: promise
    };

    // call the engine with our context
    engine.call(ctx);

    // give back the promise
    return promise.promise();

})().done(function () {
    /* all scripts are loaded and I could take actions here */
}).fail(function () {
    /* something failed, take some action here if needed */
});
```

pnp loader.js ファイルはキャッシュを行わないため、開発環境に適しています。 pnp-loader-cached.js ファイルは `$.getScript` 関数を `$.ajax` 関数に置き換えます。この関数はブラウザーによるファイルのキャッシュを許可し、本番環境により適しています。

PnP-dev\Samples\Core.JavaScript\Core.JavaScript.CDN\js\pnp-loader.js から

```javascript
    // load the remote script file
    $.ajax({
        type: 'GET',
        url: fullPath,
        cache: true,
        dataType: 'script'
    }).done(function () {
        if (self.files.length > 0) {
            engine.call(self);
        }
        else {
            self.promise.resolve();
        }
    }).fail(self.promise.reject);
```

このパターンでは、サイトへの展開と更新を容易にします。 何千ものサイト コレクションで展開や更新を行うときに特に便利です。

## 
            <a name="CachingCurrentUserInfo"></a> 現在のユーザーをキャッシュする

ユーザー情報が既にキャッシュされている場合、この関数は、セッションのキャッシュからデータを取得します。キャッシュにユーザー情報が格納されていない場合は、必要な特定のユーザー情報を取得し、キャッシュに格納します。

また、Deferred (Promise の jQuery バージョン) も使用します。キャッシュまたはサーバーからデータを取得する場合、Deferred が解決されます。エラーがある場合、Dererred は拒否されます。

PnP-dev\Samples\Core.JavaScript\Core.JavaScript.CDN\js\pnp-core.js から:

```javascript
    getCurrentUserInfo: function (ctx) {

        var self = this;

        if (self._currentUserInfoPromise == null) {

            self._currentUserInfoPromise = $.Deferred(function (def) {

                var cachingTest = $pnp.session !== 'undefined' && $pnp.session.enabled;

                // if we have the caching module loaded
                if (cachingTest) {
                    var userInfo = $pnp.session.get(self._currentUserInfoCacheKey);
                    if (userInfo !== null) {
                        self._currentUserInfo = userInfo;
                        def.resolveWith(ctx || self._currentUserInfo, [self._currentUserInfo]);
                        return;
                    }
                }

                // send the request and allow caching
                $.ajax({
                    method: 'GET',
                    url: '/_api/SP.UserProfiles.PeopleManager/GetMyProperties?$select=AccountName,DisplayName,Title',
                    headers: { "Accept": "application/json; odata=verbose" },
                    cache: true
                }).done(function (response) {

                    // we also parse and add some custom properties as an example
                    self._currentUserInfo = $.extend(response.d,
                        {
                            ParsedLoginName: $pnp.core.getUserIdFromLogin(response.d.AccountName)
                        });

                    if (cachingTest) {
                        $pnp.session.add(self._currentUserInfoCacheKey, self._currentUserInfo);
                    }

                    def.resolveWith(ctx || self._currentUserInfo, [self._currentUserInfo]);

                }).fail(function (jqXHR, textStatus, errorThrown) {

                    console.error('[PNP]=>[Fatal Error] Could not load current user data data from /_api/SP.UserProfiles.PeopleManager/GetMyProperties. status: ' + textStatus + ', error: ' + errorThrown);
                    def.rejectWith(ctx || null);
                });
            });
        }

        return this._currentUserInfoPromise.promise();
    }
}
```

## 
            <a name="CachingPatternUsingAsynchronousAndDeferred"></a> 非同期と Deferred を使用したキャッシュのパターン

キャッシュのもう 1 つのパターンは、modernizer storageTest から取得される pnp-clientcache.js storageTest にあります。キャッシュ内にある場合はキャッシュされたデータを返し、キャッシュ内にない場合はサーバーからデータを取得してキャッシュに追加し、呼び出し関数に反復的書き込みコードを保存する追加、取得、削除、および getOrAdd のための関数が含まれています。有効期限はローカル ストレージの機能ではないため、取得では JSON.parse を使用して有効期限をテストします。_createPersistable は、ローカル  ストレージ キャッシュ内に JavaScript オブジェクトを格納します。

PnP-dev\Samples\Core.JavaScript\Core.JavaScript.CDN\js\pnp-clientcache.js から:

```javascript
// adds the client cache capability
caching: {

    // determine if we have local storage once
    enabled: storageTest(),

    add: function (/*string*/ key, /*object*/ value, /*datetime*/ expiration) {

        if (this.enabled) {
            localStorage.setItem(key, this._createPersistable(value, expiration));
        }
    },

    // gets an item from the cache, checking the expiration and removing the object if it is expired
    get: function (/*string*/ key) {

        if (!this.enabled) {
            return null;
        }

        var o = localStorage.getItem(key);

        if (o == null) {
            return o;
        }

        var persistable = JSON.parse(o);

        if (new Date(persistable.expiration) <= new Date()) {

            this.remove(key);
            o = null;

        } else {

            o = persistable.value;
        }

        return o;
    },

    // removes an item from local storage by key
    remove: function (/*string*/ key) {

        if (this.enabled) {
            localStorage.removeItem(key);
        }
    },

    // gets an item from the cache or adds it using the supplied getter function
    getOrAdd: function (/*string*/ key, /*function*/ getter) {

        if (!this.enabled) {
            return getter();
        }

        if (!$.isFunction(getter)) {
            throw 'Function expected for parameter "getter".';
        }

        var o = this.get(key);

        if (o == null) {
            o = getter();
            this.add(key, o);
        }

        return o;
    },

    // creates the persisted object wrapper using the value and the expiration, setting the default expiration if none is applied
    _createPersistable: function (/*object*/ o, /*datetime*/ expiration) {

        if (typeof expiration === 'undefined') {
            expiration = $pnp.core.dateAdd(new Date(), 'minute', $pnp.settings.localStorageDefaultTimeoutMinutes);
        }

        return JSON.stringify({
            value: o,
            expiration: expiration
        });
    }
},
```

より複雑な非同期と Deferred の使用方法のために、pnp clientcache.js の開発者ダッシュボードを参照できます。

## 
            <a name="Resources"></a> リソース

### 関連項目
- [JavaScript: パターンと使用法](http://dev.office.com/blogs/javascript-development-patterns-with-sharepoint)
- [JavaScript: パフォーマンスに関する考慮事項](http://dev.office.com/blogs/javascript-performance-considerations-with-sharepoint)
- [SharePoint 2013 の JavaScript ライブラリ コードを使用して基本的な操作を完了する](https://msdn.microsoft.com/EN-US/library/office/jj163201.aspx)
- [クライアント側のレンダリング (JS リンク) のコード サンプル](https://code.msdn.microsoft.com/office/Client-side-rendering-JS-2ed3538a)
- [JavaScript の埋め込み (JavaScript を使用して SharePoint サイト UI をカスタマイズする)](https://msdn.microsoft.com/EN-US/library/dn913116.aspx)
- [SharePoint の検索のカスタマイズ](https://msdn.microsoft.com/EN-US/library/mt210901.aspx)
- [SharePoint アドインのレシピ: カスタム フィールドの種類 (クライアント側レンダリングを使用)](custom-field-type-sharepoint-add-in.md)

### サンプル

- [Performance.Caching](https://github.com/OfficeDev/PnP/tree/master/Samples/Performance.Caching)
- [Core.JavaScript](https://github.com/OfficeDev/Pnp/tree/master/Samples/Core.JavaScript)
