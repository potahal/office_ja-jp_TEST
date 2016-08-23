# PnP プロビジョニング フレームワーク

Office 365 と SharePoint Online のサイト コレクションで使用できるリモート プロビジョニング機能の概要と、完全に信頼できるサンドボックス ソリューションの作成が推奨されなくなった理由について取り上げます。

サイト コレクションをプロビジョニングするためのコード中心のテンプレート ベースのプラットフォームを提供します。新しいプロビジョニング エンジンを使用すると、Office 365 と SharePoint Online、およびオンプレミス サイト コレクションにプロビジョニング モデルを永続化し、再利用できます。

## 新しい方法が必要な理由

&mdash;「_なぜ新しい方法が必要になるのか?_」という質問が生じます。 また、「_完全に信頼できるサンドボックス ソリューションに代わるこの方法にはどのような利点があるのだろうか?_」という質問もあります。

最初の質問の答えには、SharePoint アドイン と アドイン モデル (旧称「アプリ モデル」) の導入が関係します。この変更に伴い、Microsoft はプロバイダー ホスト型の アドイン とオンプレミス ソリューションの利点を生かすため、完全に信頼できるサンドボックス ソリューションの使用を取りやめました。こうした進展によってプロビジョニング モデルの改善や新しいプロビジョニング エンジンの導入が促進されました。

2 番目の質問 &mdash; 「_新しいプロビジョニング モデルにはどのような利点があるのですか_」の答えとして、 
                &mdash;次のようないくつかの利点があります。

- テンプレートのカスタマイズ。サイト コレクションはすぐに使用できるテンプレートで必ず開始するため、新しいリモート プロビジョニング モデルを使用して導入されるカスタマイズによって、ユーザーによる追加の保守作業を必要としないで、自動更新を行うことができます。さらに、この方法を使用すると、各種サイト コレクションで異なるテンプレートを使用することによって生じる問題を避けられます。
    
- テンプレート ベース モデルの使用。簡単なテンプレート ベースのプロビジョニング モデルを提供します。このモデルを使用すると、既存のサイト デザインをプロビジョニング テンプレートとして保存できます。 
    
- 異なる方法によるテンプレートの定義。 [PnP プロビジョニング スキーマ](pnp-provisioning-schema.md)に対する検証を行う XML 形式のテンプレートを手動で定義することもできますし、管理対象コードを使用してテンプレートを定義し、オブジェクトを階層を構築することもできます。これらの方法を合わせて使用することも可能です。
    
- テンプレートのシリアル化と再利用。プロビジョニング テンプレートをシリアル化してから、再利用できます。
    
- シリアル化形式でのテンプレートの永続化。 プロビジョニング テンプレートを自分に最適なシリアル化形式 &mdash; たとえば、XML、JSON などで永続化できます。
    
- 新しいサイト コレクションのプロビジョニング。プロビジョニング テンプレートを、選択した任意のシリアル化形式で対象サイトに適用することによって、新しいサイト コレクションを簡単にプロビジョニングできます。
    
- クライアント オブジェクト モデルとの統合。クライアント オブジェクト モデル (CSOM) と統合することによって、自動化されたコード ベースのプロビジョニングが行えるため高い柔軟性が得られます。CSOM/REST コードまたは Windows PowerShell スクリプトのいずれかを使用して、プロビジョニング テンプレートによって新しいサイト コレクションをプロビジョニングできます。
    
- 差分プロビジョニングの使用。既存のサイトの最上部にプロビジョニング テンプレートを適用できます。プロビジョニング エンジンは差分プロビジョニングをサポートし、テンプレート定義で指定されているスコープに基づいてサイトの追加/更新を行います。
    
- プロビジョニング エンジンの拡張。カスタム拡張プロバイダーを使用して簡単にプロビジョニング エンジンを拡張できます。このプロバイダーによって、CSOM/REST 管理対象コードで作成されたカスタム ロジックを実行できます。
    
- オンプレミスおよび Office 365 展開にまたがる作業。プロビジョニング エンジンを使用すると、オンプレミスと Office 365 展開の両方にまたがる作業をシームレスに処理できるようになりました。この点は旧来のプロビジョニング技法が改善された部分で、これまでカスタム サイト定義は Office 365 ではサポートされていませんでした。ファームを対象とする展開が必要だったためです。

## Nutshell におけるリモート プロビジョニング

このセクションでは、リモート プロビジョニングの各部の詳細を取り上げます。ただし、最初にリモート プロビジョニングを概観し、最も簡略化した形式で理解すると役立ちます。そうした観点からリモート プロビジョニングを見ると、次の 3 つの要素のみが関係します。

1. サイト カスタマイズのデザインと作成。
    
2. 選択したシリアル化形式でのプロビジョニング テンプレートの作成。およびオプションの操作として永続化も行えます。
    
3. すぐに使用できるサイト テンプレートを使用して作成した既存のサイト コレクション、または新しいサイト コレクションへのプロビジョニング テンプレートの適用。

### 1. サイトとサイト アーティファクトのモデル化

手順 1 では、保存してサイト コレクションに適用するサイト カスタマイズを作成します。いくつかの作成方法があります。最も簡単な方法は、既存のサイト ページに必要な変更を加え、その後、そのページをプロビジョニング テンプレートとして保存することです。詳しくは、「 [Provisioning templates](http://msdn.microsoft.com/library/b3eeb7e7-37cf-4e70-8486-34f67220fe33%28Office.15%29.aspx)」をご覧ください。

また、プロビジョニング テンプレートを XML ファイルとして手動で作成したり、管理対象コード (CSOM/REST) を使用してサイト アーティファクトと構造を表すオブジェクト階層を作成したりできます。スキーマ ファイルを作成する場合、そのファイルをプロビジョニング XSD スキーマに対して検証しなければなりません (「 [PnP プロビジョニング スキーマ](pnp-provisioning-schema.md)」をご覧ください)。

「 [PnP プロビジョニング エンジンとコア ライブラリ](pnp-provisioning-engine-and-the-core-library.md)」という記事に、サイトのモデル化についての詳細が記されています。

### 2. プロビジョニング テンプレートのエクスポートと永続化

カスタマイズしたサイト モデルを希望のシリアル化形式でエクスポートします。プロビジョニング エンジンは永続化形式にとらわれません。カスタマイズのこの保存済みインスタンスはプロビジョニング テンプレートで、最小の労力で新しいサイト コレクションに適用できます。テンプレートのシリアル化と永続化は、テンプレートを永続化する場合にのみ必要となる省略可能な手順です。テンプレートをシリアル化しないでも、新しいサイト コレクションに適用することは可能です。

### 3. プロビジョニング テンプレートの新しいサイト コレクションへの適用

プロビジョニング テンプレートを新しいサイト コレクションまたは既存のサイト コレクションに適用するには、Windows PowerShell スクリプトまたは CSOM/REST コードのどちらかを使用できます。また、サイト コレクション全体をプロビジョニングすることも、その一部のみをプロビジョニングすることもできます。プロビジョニング テンプレートのシリアル化を含め、アクションにおけるリモート テンプレートのサンプルを確認するには、「 [コンソール アプリケーションのプロビジョニング サンプル](provisioning-console-application-sample.md)」をご覧ください。

## その他のリソース
<a name="bk_addresources"> </a>

- [PnP リモート プロビジョニング](pnp-remote-provisioning.md)
    
- [コンソール アプリケーションのプロビジョニング サンプル](provisioning-console-application-sample.md)
    
- [PnP プロビジョニング エンジンとコア ライブラリ](pnp-provisioning-engine-and-the-core-library.md)
    
- [PnP プロビジョニング スキーマ](pnp-provisioning-schema.md)