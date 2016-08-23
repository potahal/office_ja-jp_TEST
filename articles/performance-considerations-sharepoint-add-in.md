SharePoint アドイン モデルのパフォーマンスに関する考慮事項
=========================================================

概要
-------

新しい SharePoint アドイン モデルで SharePoint の最適なパフォーマンスを確保する方法は、完全信頼コードの場合とは異なります。一般的な完全信頼コード (FTC)/ ファーム ソリューション シナリオでは、ほとんどのコード操作は SharePoint サーバー側オブジェクト モデル コードで行われます。

SharePoint アドイン モデル シナリオでは、SharePoint クライアント側オブジェクト モデル (CSOM) や SharePoint REST API を使用してコードを実行します。

2 つのモデル間の主な違いは、サーバー側のコードとクライアント側のコードです。SharePoint アドイン モデルでは、クライアント経由でコードが実行されるため、追加のネットワーク トラフィックが発生します。SharePoint サーバーへの API 呼び出しラウンド トリップの数を最小限に抑えると、SharePoint アドインのパフォーマンスが向上し、SharePoint サイトで消費されるリソースの量が減少します。

さらに、SharePoint アドイン モデルでは、クライアント経由でコードが実行されるため、応答を受信する前に長い遅延が発生する可能性があります。長時間実行される操作 (ユーザー プロファイル API など) のデータのキャッシュでは、情報を返したり、処理の完了確認を受信するのにかかる時間を短縮できます。

基本ガイドライン
---------------------

新しい SharePoint アドイン モデルにおける SharePoint の最適なパフォーマンスの確保については、大まかに次のような基本ガイドラインが提供されています。

- SharePoint サーバーへのクライアント側 API 呼び出しを最小限に抑えます。
- サーバー側およびクライアント側でキャッシュ技術を使用し、情報を保存します。
- キャッシュに clientIDs、clientSecrets、ユーザー名、パスワード、トークン、またはその他の機密セキュリティ情報を保存することはお勧めしません。

SharePoint の最適なパフォーマンスを確保するためのオプション
-----------------------------------------------------
SharePoint の最適なパフォーマンスを確保するには、いくつかのオプションがあります。

- クライアント側キャッシュを使用する 
- サーバー側キャッシュを使用する 

クライアント側キャッシュを使用する 
-----------------------
このパターンでは、データのキャッシュにクライアント側キャッシュ技術 (HTML5 LocalStorage や cookie など) が使用されています。これらの場所に保存されている情報は、SharePoint サーバーへの API 呼び出しが必要かどうかの判断に使用されます。

- このパターンでは、クライアント側キャッシュ内の SharePoint API 呼び出しから返されたデータを保存します。
- このパターンでは、クライアント側キャッシュに保存されているデータを評価するために、クライアント側コード (JavaScript) を使用します。
- Cookie は、4095 バイトのデータの保存に制限されます。
    + Cookie には、組み込みのデータの有効期限のメカニズムがあります。
- HTML5 LocalStorage は、5 MB のデータに制限されます。
    - HTML5 LocalStorage には、組み込みのデータの有効期限のメカニズムはありません。ただし、このような有効期限のメカニズムは JavaScript に迅速かつ簡単に実装できます。
    
        例については、[Performance.LocalStorage e (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Performance.Caching/Performance.LocalStorage) の [App.js JavaScript ファイル](https://github.com/OfficeDev/PnP/blob/master/Samples/Performance.Caching/Performance.LocalStorage/Scripts/App.js)の **setLocalStorageKeyExpiry** 関数と **isLocalStorageExpired** 関数を参照してください。

        **LocalStorage で有効期限のキーを設定する**

        ```
        function setLocalStorageKeyExpiry(key) 
        {       
            // Check for expiration config values
            var expiryConfig = localStorage.getItem(expiryConfigKey);
            
            // Check for existing expiration stamp
            var existingStamp = localStorage.getItem(key + expiryKeySuffix);    
        
            // Override cached setting if a user has entered a value that is different than what is stored
            if (expiryConfig != null) 
            {                       
                var currentTime = Math.floor((new Date().getTime()) / 1000);
                expiryConfig = parseInt(expiryConfig);
                
                var newStamp = Math.floor((currentTime + expiryConfig));
                localStorage.setItem(key + expiryKeySuffix, newStamp);
                
                // Log status to window        
                cachingStatus += "\n" + "Setting expiration for the " + key + " key...";
                $('#status').val(cachingStatus);
            }    
            else 
            {
               
            }
        }
        ```

        **有効期限のキーが LocalStorage で期限切れになっているかどうかを確認する:**
        ```
        function isLocalStorageExpired(key, keyTimeStampName) 
        {
            // Retrieve the example setting for expiration in seconds
            var expiryConfig = localStorage.getItem(expiryConfigKey);
            
            // Retrieve the example setting for expiration in seconds
            var expiryStamp = localStorage.getItem(keyTimeStampName);
        
            if (expiryStamp != null && expiryConfig != null) {
        
                // Retrieve the expiration stamp and compare against specified settings to see if it is expired
                var currentTime = Math.floor((new Date().getTime()) / 1000);
        
                if (currentTime - parseInt(expiryStamp) > parseInt(expiryConfig)) {
                    cachingStatus += "\n" + "The " + key + " key time stamp has expired...";
                    $('#status').val(cachingStatus);
                    return true;
                }
                else 
                {
                    var estimatedSeconds = parseInt(expiryStamp) - currentTime;
                    cachingStatus += "\n" + "The " + key + " time stamp expires in " + estimatedSeconds + " seconds...";
                    $('#status').val(cachingStatus);
                    return false;
                }
            }
            else 
            {
                //default
                return true;
            }
        }
        ```

**適切な場合**

SharePoint ECMA クライアント側オブジェクト モデル API (sp.js) を使用し、クライアント側データを評価して API の呼び出しの必要性を判断する必要がある場合。

**作業の開始**

[Performance.Caching (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Performance.Caching) は、LocalStorage と cookie ベースのクライアント側キャッシュの両方をアドイン モデルに実装する方法について説明し、いくつかのサンプルとの記事へのリンクを提供します。

サーバー側キャッシュを使用する 
-----------------------
このパターンでは、サーバー側キャッシュ技術 (セッションやサーバー側 cookie 評価など) を使用してキャッシュされたデータにアクセスします。これらの場所に保存されている情報は、SharePoint サーバーへの API 呼び出しが必要かどうかの判断に使用されます。

- このパターンでは、サーバー側キャッシュで SharePoint API 呼び出しから返されたデータを保存します。    
- このパターンでは、サーバー側の場所に保存されたデータを評価するためにサーバー側コードを使用します。
    + サーバー側の場所には、セッション ベースのデータ、RAM に保存されているサーバー側のキャッシュ、またはサードパーティ サーバー ベースのその他のキャッシュ テクノロジが含まれる場合があります。
- このパターンでは、cookie に保存されたデータを評価するためにサーバー側コードを使用します。
- Cookie は、4095 バイトのデータの保存に制限されます。
    + Cookie には、組み込みのデータの有効期限のメカニズムがあります。
    
    cookie を評価するためにサーバー側コードを使う方法については、[OD4B.Configuration.Async (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Solutions/OD4B.Configuration.Async) の [Customizer.aspx.cs クラス](https://github.com/OfficeDev/PnP/blob/master/Solutions/OD4B.Configuration.Async/OD4B.Configuration.AsyncWeb/Pages/Customizer.aspx.cs)の **CookieCheckSkip** メソッドを参照してください。

**独自の「man-in-the-middle」キャッシュ レイヤーの実装** 場合によっては、独自のカスタム キャッシュ レイヤーを作成するのが最適です。良い例として、ユーザーのプロファイルから情報を返す必要がある場合があります。ユーザー プロファイル API の実行には時間がかかる場合があります。エンド ユーザーに高速なユーザー インターフェイス エクスペリエンスを提供するため、リモート タイマー ジョブを作成して、ユーザー プロファイル サービスのクエリを実行、さまざまなデータ ストアの情報を保存します。次に、SharePoint アドインから JavaScript の呼び出し経由でデータ ストアをクエリできるサービスを作成できます。  

Azure には、情報の保存に使用できるさまざまなストレージ メカニズムがあり、それらの多くは Table ストレージや Blob ストレージのように、非常に高速で実行されます。また、Azure では、サービスをホストする Web アプリを作成したり、Office 365 SharePoint テナンシーや、ディレクトリ同期が有効になっているオンプレミスの SharePoint 環境と同じ Azure Active Directory を使用して、すべてのデータやサービスを保護できます。

**適切な場合**

- SharePoint マネージ コード クライアント側オブジェクト モデル API (Microsoft.SharePoint.Client.dll) を使用し、サーバー側のデータや cookie を評価して API の呼び出しが必要かどうかを判断する必要がある場合。
- 時間がかかり、ユーザーが操作の開始を試行した回数に関係なく、指定された時間枠に 1 回のみ開始する必要がある操作 (Azure Web ジョブなど) がある場合。  
    + カスタムの OneDrive for Business 構成の適用は、このようなシナリオの良い例です。

**作業の開始**

[Performance.Caching (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Performance.Caching) は、アドイン モデルに LocalStorage と cookie ベースのクライアント側キャッシュの両方を実装する方法について説明し、いくつかのサンプルと記事へのリンクを提供します。

関連リンク
=============
- ガイダンス記事: [http://aka.ms/OfficeDevPnPGuidance](http://aka.ms/OfficeDevPnPGuidance "ガイダンス記事")
- MSDN リファレンス: [http://aka.ms/OfficeDevPnPMSDN](http://aka.ms/OfficeDevPnPMSDN "MSDN リファレンス")
- ビデオ: [http://aka.ms/OfficeDevPnPVideos](http://aka.ms/OfficeDevPnPビデオ"Videos")

関連する PnP サンプル
===================

- [http://aka.ms/OfficeDevPnP](http://aka.ms/OfficeDevPnP) にあるサンプルとコンテンツ

適用対象
==========
- Office 365 マルチテナント (MT)
- Office 365 専用 (D)
- SharePoint 2013 オンプレミス
