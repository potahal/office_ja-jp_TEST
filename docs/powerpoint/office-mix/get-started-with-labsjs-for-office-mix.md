
# LabsJS for Office Mix の概要
Microsoft Office Mix ラボの開発をサポートするために、Labs.js API を使用して Office アドイン 開発環境をセットアップする方法について説明します。

 _ **適用対象:** apps for Office?| Office Add-ins?| Office Mix?| PowerPoint_

LabsJS のコンテンツでは、対話型ラボの開発し、Office Mix に統合して Microsoft PowerPoint で表示するために使用できる API (labs.js)、サンプル、ドキュメント、および関連ファイルを公開しています。これらのラボは実際には、HTML5 および labs.js JavaScript ライブラリを使用して作成される Office アドインです。

## LabsJS コンテンツ

LabsJS は、Office Mix のラボを作成して公開するのに必要なドキュメント、サンプル ラボ、およびファイルを提供します。


**必要なファイル**


|**ファイル**|**説明**|
|:-----|:-----|
|labs-1.0.4.js|Office Mix ラボの開発のための LabsJS JavaScript API です。このファイルは、プロジェクトを Office Mix と統合するために、プロジェクトに含める必要があります。また、このファイルは  `https://az592748.vo.msecnd.net/sdk/LabsJS-1.0.4/labs-1.0.4.js` にあるコンテンツ配信ネットワーク (CDN) でホストされます。アプリを公開する場合は、CDN にあるこのファイルへのリンクを付ける必要があります。|
|labs-1.0.4.d.ts|labs.js の TypeScript 定義ファイルです。このファイルを使用することによって、TypeScript コードを labs.js と簡単に統合することができます。また、この定義ファイルは labs.js に含まれるすべてのコンポーネントの概要を提供します。TypeScript は [http://www.typescriptlang.org/](http://www.typescriptlang.org/) からダウンロードできます。この定義ファイルは、TypeScript バージョン 0.9.1.1 に対して構築されました。|
|履歴|labs.js ライブラリのさまざまなバージョンのリリース履歴です。|
|Labshost.html|PowerPoint のコンテキスト外で、Office Mix に対するラボの表示およびデバッグを可能にする Web ページです。このページを使用するには、メインの入力ボックスに URL を入力してフレーム内に読み込みます。PowerPoint または Office Mix レッスン プレーヤーで実行中に API および Office Mix の間で交換されたデータは、入力ボックスの右側に表示されます。データは事前にシードすることもできます。「サンプル」セクションのサンプル ラボには、ホスト コンテキストで実行中の既存の Office Mix アドインが表示されることに注意してください。|
|SampleManifest.xml|独自のアプリケーション マニフェストを作成するためのテンプレートとして使用するサンプル Office アドイン マニフェスト。|
|Simplelab.html|labs.js で作成されたサンプル ラボです。Web ページの選択と Web ページの挿入が可能で、閲覧しているユーザーを追跡します。|
|Simplelab.ts|Simplelab サンプルを作成するのに使用された TypeScript ファイルです。|
|Simplelab.js|Simplelab サンプルの JavaScript バージョンです。 Simplelab.js と Simplelab.ts から、LabsJS API の使用法を確認できます。|

## 開発環境のセットアップ

labs.js ライブラリは、office.js ライブラリ (Office アドイン の API) の最上部の抽象化層として機能するため、labs.js ライブラリを使用して作成するラボは、実際には Office アドインです。labs.js ライブラリを操作し、これらのラボを Office Mix 内で実行するには、自分を Office アドイン開発者に設定する必要があります。


### Office 365 Developer サイトに登録する

最初のステップは、Office 365 開発者向けサイト にサインアップすることです。これにより、Office ストアに提出する前にラボをホストおよびテストできます。このサイトを使用すると、アドインを Office Mix に発行し、ライブ環境でテストできます。

詳細については、「 [Office 365 で SharePoint アドインの開発環境をセットアップする](http://msdn.microsoft.com/library/b22ce52a-ae9e-4831-9b68-c9210af6dc54%28Office.15%29.aspx)」を参照してください。最初の 2 つの手順のみが必須です。"Napa" 開発者ツールのインストールはオプションです。


### SharePoint Online 上でアプリ カタログをセットアップする

開発者サイトを作成し、プロビジョニングしたら、SharePoint Online 上にアドイン カタログをセットアップします。詳細については、「 [Office 365 でアドイン カタログをセットアップする](../../publish/set-up-an-add-in-catalog-on-office-365.md)」を参照してください。

Office Mix の場合は、アドイン カタログを使用して運用前のアドインをレッスンに挿入し、ラボをストアに提出する前にエンド ツー エンド テストを実行できます。


## ラボを作成する

最初のラボを作成するには、単純な正誤クイズの作成方法を説明した [チュートリアル](walkthrough:-creating-your-first-lab-for-office-mix.md)にある手順に従います。「 [チュートリアル: Office Mix 用の最初のラボの作成](walkthrough:-creating-your-first-lab-for-office-mix.md)」を参照してください。


## ラボを発行する

ラボを作成したら、それを発行し、ストアに提出できます。


### アプリケーション マニフェストを作成してアップロードする

アプリケーション マニフェストは、LabJS ラボを説明する XML ドキュメントです。ラボがホストされている URL への参照を提供し、表示名、説明、アイコン、サイズなどのラボに関する詳細を提供します。

サンプル マニフェスト "SampleManifest.xml" が含まれています。マニフェスト スキーマに関する詳細やスキーマ定義へのリンクについては、「 [Office アドインの XML マニフェスト](../../docs/overview/add-in-manifests.md)」を参照してください。

SharePoint サイトにマニフェストをアップロードするには、まずアプリケーション カタログ (通常は URL  `https://<your site>/sites/AppCatalog` にある) に移動します。次に、 **[新しいアプリ]** ボタンをクリックし、手順に従ってアプリケーション マニフェストをアップロードします。


### PowerPoint 2013 カタログを更新する

次に、PowerPoint 2013 カタログを更新します。その後、開発者アカウントを使用してログオンすることができます。

最初に PowerPoint 2013 カタログを更新します。PowerPoint 2013 を起動し、メニューから  **[ファイル] > [オプション] > [セキュリティ センター] > [セキュリティ センターの設定] > [信頼できるアプリ カタログ]** の順に進みます。そこから、アプリ カタログの参照を追加し、 **[OK]** をクリックします。変更内容が反映されるようにするために、PowerPoint 2013 からサインアウトするように求められます。サインアウトします。

最後に、開発者アカウントを使用して再度ログオンします。PowerPoint 2013 の右上隅にあるログオン名を選択し、開発者アカウントを使用してログオンします。これで、アドインを挿入できるようになります。


### アプリを挿入、発行、表示する

カタログにアドインを挿入するには、 **[挿入]** リボンを選択し、 **[アプリ]** セクションで **[ストア]** を選択します。 **[自分の所属組織]** を選択すると、アドイン カタログにアドインが表示されます。アドインを選択し、 **[挿入]** をクリックすると、アドイン (ラボ) が PowerPoint 2013 ドキュメントに挿入されます。

これで、利用可能なすべての Office Mix 機能を活用して、新しいラボでレッスンを公開できるようになりました。


 >**重要**  アプリケーションを表示するには、レッスンを表示するのと同じブラウザーで SharePoint カタログにログオンする必要があります。 SharePoint カタログには、認証されたユーザーのみがアクセスできるため、アプリケーションを表示するにはまずログオンする必要があります。 


### Office ストアにラボを提出する

Office ストアにラボを提出するには、「 [Office アドインを発行する](../publish/publish.md)」をご覧ください。


## その他の技術情報



- [Office Mix アドイン](../../powerpoint/office-mix/office-mix-add-ins.md)
    
- [Office アドイン](../../docs/overview/office-add-ins.md)
    
- [チュートリアル: Office Mix 用の最初のラボの作成](walkthrough:-creating-your-first-lab-for-office-mix.md)
    
