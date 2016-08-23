# SharePoint 2013 および SharePoint Online 用複合ビジネス アプリ

複合ビジネス アドインを使用すると、SharePoint ソリューションをビジネス プロセスとテクノロジに統合できます。SharePoint ホスト型アドインとプロバイダー ホスト型アドインのどちらがソリューションにとって正しい選択肢であるかを判断してください。

_**適用対象:** Office 365 | SharePoint 2013 | SharePoint Online_

複合ビジネス アプリは、各種のビジネス プロセスと基幹業務 (LOB) テクノロジ (データベースや Web サービスなど) に緊密に統合されるアプリです。通常、これらのアプリには、ユーザーやその他のテクノロジとの複雑な相互作用が、数多く含まれます。このセクションで説明するサンプルの複合ビジネス アプリは、お使いのテクノロジやプロセスを SharePoint 2013 のアドイン モデルに統合するために使用できる、構成要素を提供します。

## SharePoint ホスト型アドインとプロバイダー ホスト型アドイン
<a name="sectionSection0"> </a>

複合ビジネス アプリを作成する前に、まずアプリをホストする場所を決定する必要があります。JavaScript で操作できる単一サイトの実装で要件が満たせる場合、SharePoint ホスト型アドインが最適です。より複雑なビジネス要件には、プロバイダー ホスト型アドインの方が適しています。

次の表は、アプリをホストする場所を決定する際に考慮する要因をまとめたものです。

|**SharePoint ホスト型アドイン**|**プロバイダー ホスト型アドイン**|
|:-----|:-----|
|必要なことは JavaScript ですべて行える。|JavaScript 以外の言語を使用する必要がある。|
|複数のサイトをまたいで作業する必要がないアドイン (チーム予定表アプリやおすすめのニュース ローテーターなど)。|複数のサイトをまたいで情報にアクセスし、作業する必要があるアドイン (サイト コレクションのプロビジョニング アプリなど)。|
|コンテンツは機密性が高く、セキュリティの保たれた状態で、SharePoint だけに置かれている必要がある。|他の基幹業務技術と統合する必要があるアドイン。|
||アプリ専用のポリシーによって可能になる、管理者特権のアクセス許可が必要なアドイン。|
||高度にカスタマイズされた UI が必要なアドイン。|

## このセクションの内容
<a name="sectionSection1"> </a>

|**記事**|**サンプル**|**説明される方法...**|
|:-----|:-----|:-----|
|[InfoPath フォームを SharePoint 2013 に移行する](Migrate-InfoPath-forms-to-SharePoint.md) ||InfoPath 2013 フォームを、サポートされている他のテクノロジに移行する。|
|[SharePoint Online でのデータ ストレージ オプション](Data-storage-options-in-SharePoint-Online.md) |[Core.DataStorageModels](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.DataStorageModels) |各種のストレージ モデルを使用して SharePoint Online のデータを格納する。|
|[会社イベント アドインと SharePoint との統合](Corporate-app-event-registration-with-SharePoint.md)|[BusinessApps.CorporateEventsApp](https://github.com/OfficeDev/PnP/tree/master/Solutions/BusinessApps.CorporateEventsApp)|プロバイダー ホスト型アドインを使用して複雑なビジネス タスクを実装する。|
|[SharePoint ワークフローからの Web サービス呼び出し](Call-web-services-from-SharePoint-workflows.md)|<p>[Workflow.CallCustomService](https://github.com/OfficeDev/PnP/tree/master/Samples/Workflow.CallCustomService)</p><p>[Workflow.CallServiceUpdateSPViaProxy](https://github.com/OfficeDev/PnP/tree/master/Samples/Workflow.CallServiceUpdateSPViaProxy)</p><p>[Workflow.AssociateToHostWeb](https://github.com/OfficeDev/PnP/tree/master/Samples/Workflow.AssociateToHostWeb)</p>|プロバイダー ホスト型アプリを使用して、ビジネス データが含まれているリモート Web サービスを呼び出す。|

## その他のリソース
<a name="bk_addresources"> </a>

-  [Office 365 開発パターンとプラクティス (ソリューション ガイダンス)](Office-365-development-patterns-and-practices-solution-guidance.md)
    
-  [GitHub での Office 365 Development パターンおよびプラクティス](https://github.com/OfficeDev/PnP)
