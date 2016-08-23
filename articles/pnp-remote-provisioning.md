# PnP リモート プロビジョニング

アドイン モデルの機能を使用して、Office 365 および SharePoint Online のサイト コレクションのリモート プロビジョニングを行う方法について説明します。リモート プロビジョニングは SharePoint 2013 と SharePoint 2016 でもサポートされています。

Microsoft SharePoint 2013 と新しい アドイン モデルを使用すると、リモート プロビジョニングを Office 365 と SharePoint Online サイト コレクション、および SharePoint 2013 と SharePoint 2016 に対して使用することができます。プロビジョニング テンプレートは簡単に作成でき、単純な Windows PowerShell スクリプトまたは C# コードでそのテンプレートを適用および再利用することが可能です。

**重要** PnP リモート プロビジョニングの使用に関する資料、サンプル、サポートを利用するための 2 つの方法があります。1 つはこのセクションのこの MSDN のドキュメントです。もう 1 つは、GitHub にある一連の資料とサンプルです。主な GitHub リソースについては、「GitHub の PnP リモート プロビジョニング リソース」という下記の表にまとめています。

## GitHub の PnP リモート プロビジョニング リソース

|**記事**|**以下の方法を説明します**|
|:-----|:-----|
|[Office365 デベロッパー パターンとプラクティス (PnP)](https://github.com/OfficeDev/PnP)|GitHub における SharePoint と Office 365 PnP リソースのルート サイトです。コア ライブラリが含まれています。|
|[PnP プロビジョニング エンジンの入門](https://channel9.msdn.com/blogs/OfficeDevPnP/Getting-Started-with-PnP-Provisioning-Engine)|Paolo Pialorsi によって配信された、Microsoft Channel 9 音声/ビデオによる PnP プロビジョニング エンジンの紹介。|
|[PnP プロビジョニング エンジンの紹介](https://github.com/OfficeDev/PnP-Guidance/blob/551b9f6a66cf94058ba5497e310d519647afb20c/articles/Introducing-the-PnP-Provisioning-Engine.md)|PnP プロビジョニング エンジンの詳細を紹介する GitHub 記事。|
|[OfficeDevPnP.Core ](https://github.com/OfficeDev/PnP/tree/master/OfficeDevPnP.Core)|Office 365 Developer PnP コア コンポーネントは、CSOM プロビジョニング オブジェクトをサポートするための再利用可能な拡張メソッドとして一般的に使用されるリモート CSOM/REST 操作をカプセル化した拡張機能です。|
|[PnP プロビジョニング スキーマの内側](https://channel9.msdn.com/blogs/OfficeDevPnP/Deep-dive-to-PnP-provisioning-engine-schema)|Paolo Pialorsi によって配信された、Microsoft Channel 9 音声/ビデオによる PnP プロビジョニング スキーマの紹介。|

## その他のリソース
<a name="bk_addresources"> </a>

- [Office 365 開発パターンとプラクティス (ソリューション ガイダンス)](Office-365-development-patterns-and-practices-solution-guidance.md)
