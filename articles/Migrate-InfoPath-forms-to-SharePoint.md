# InfoPath フォームを SharePoint 2013 に移行する

SharePoint アドインの InfoPath フォームを、Access アプリケーション、サンドボックス ソリューション、アドイン モデルなど、サポートされている他のソリューションに移行します。

_**適用対象:** Office 365 | SharePoint 2013 | SharePoint Online_

アドインのフォーム作成の基礎として InfoPath を使用している場合、今すぐ、フォームを他のソリューションに移行することを是非ご検討ください。現時点で InfoPath はまだサポートされていますが、デスクトップ InfoPath クライアントの最終リリースは InfoPath 2013 であり、InfoPath Forms Services の最終リリースは SharePoint 2013 の InfoPath Forms Services です。SharePoint 2013 のクライアントおよび InfoPath Forms Services のオンプレミス バージョンは、2023 年まで完全にサポートされる予定です。Office 365 において、フォーム サービスは、少なくとも Office の次のメジャー リリースまでサポートされる予定です。

InfoPath フォームを置換するには、以下のうちのいずれか 1 つの方法を選択できます。

- Access アプリケーションを使用する。
    
- InfoPath フォームをサンドボックス ソリューションに変換する。
    
- 複雑な動作をアドイン モデルに移す。
    
最初の 2 つのソリューションをお勧めします。その場合であれば、コード ベースの方法でコーディングして展開する方法を知らない情報処理作業者でも実装が可能だからです。これらの各手段がそれぞれ最も適しているシナリオを以下の表に示します。

**SharePoint 2013 での InfoPath の代替手段**

|**代替手段**|**シナリオ**|
|:-----|:-----|
|Access アプリケーション|このオプションでは、複数の Access テーブル、Excel テーブル、および SharePoint リストに含まれているリレーショナル データを処理する複数のフォームがサポートされます。|
|InfoPath をサンドボックス ソリューションに変換する|これは、フォームが単純であり、サーバー コンポーネントが不要で、外部 Web サービス呼び出しをしない場合に適したオプションです。|
|アドイン モデルに変換する|大規模なコードによって駆動される複雑なフォームは、プロバイダー向けのホスト型アドインに変換できます。このオプションでは、開発者用リソースが必要です。|

## その他のリソース
<a name="bk_addresources"> </a>

-  [Office 365 開発パターンとプラクティス (ソリューション ガイダンス)](Office-365-development-patterns-and-practices-solution-guidance.md)
    
-  [GitHub での Office 365 Development パターンおよびプラクティス](https://github.com/OfficeDev/PnP)
