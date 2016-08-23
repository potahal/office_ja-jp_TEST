SharePoint アドイン モデルにおけるローカライズ
===========================================

概要
-------

新しい SharePoint アドイン モデルにおいて、アドインのローカライズを実装する方法は、完全信頼コードの場合とは異なります。一般的な完全信頼コード (FTC)/ ファーム ソリューション シナリオでは、Web パーツ、ユーザー コントロール、および Web コントロールなどのカスタム コンポーネントのローカライズは、リソース ファイル、.Net で管理されたコード、プロパティ、および宣言型のコードの組み合わせを使用して実装されていました。すべての成果物は、SharePoint ソリューションを使用して展開されている機能にパッケージ化されています。

SharePoint アドイン モデル シナリオでは、JavaScript またはアドインを構築する Web テクノロジと関連付られたローカライズ機能を使用して、ローカライズを実装します。ローカライズされたリソースによっては、従来のリソース ファイルの使用が必要となる場合もあります (アドイン定義の機能フレームワーク要素を使用してアドイン Web に展開された要素をローカライズする場合など)。

基本ガイドライン
---------------------

ローカライズの実装については、大まかに次のような基本ガイドラインが提供されています。

- ユーザーが特定の言語とカルチャで Web サイトを作成できるようにするには、オンプレミスの SharePoint および Office 365 環境に適切な言語パックをインストールする必要があります。
- Script Editor アドイン パーツのコンテンツをローカライズする場合は、JavaScript を使用して SharePoint アドインでローカライズを実装することもできます。 

ローカライズのシナリオ
----------------------

アドインへローカライズを実装する必要が出るケースには、2 つの異なるシナリオがあります。

- SharePoint ホスト型アドイン
- プロバイダー ホスト型アドイン

アドイン Web コンポーネントまたは資産
-------------------------
このシナリオでは、ローカライズは JavaScript を使用してアドインに適用されます。

- SharePoint ホスト型アドインには、SharePoint サーバーのサーバー ベースのリソース ファイルに対するアクセス権はありませんが、ユーザーは機能要素の resx ファイルへのアクセス権を持っています。 
- SharePoint ホスト型アドインおよび Office アドインのローカライズ方法は、両方とも JavaScript を使用しているため、非常に似ています。

**適切な場合**

SharePoint ホスト型アドインを作成するときは、JavaScript の使用が最適です。これは、JavaScript を使用してローカライズを実装し、ローカライズのサポートに必要なすべての JavaScript ファイルを SharePoint ホスト型アドインで展開できるためです。また、プロバイダー ホスト型のアドインにも特定のアドイン Web が含まれている場合、この方法のメリットを活用することもできます。

**はじめに**

[Core.JavaScriptCustomization (O365 PnP サンプル))](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.JavaScriptCustomization) のシナリオ 2 は、JavaScript を使用してアドイン内のテキストや、アドイン内の HTML 要素に関連付けられた属性をローカライズする方法を示しています。

また、[SharePoint アドインをローカライズする ](https://msdn.microsoft.com/en-us/library/fp179919(v=office.15).aspx) では、JavaScript を使用してアドイン Web 内の資産をローカライズする方法も示しています。

リモート コンポーネント
-------------------------
このシナリオでは、アドインをホストしている Web テクノロジに関連付けられたローカライズ テクノロジを使用して、ローカライズがアドインに適用されます。

- ASP.NET を使用してアドインを実装する場合、ローカライズするリソース ファイルや JavaScript ファイルが使用されます。
- PHP、Python、Ruby などの別のテクノロジを使用してアドインを実装する場合は、これらのプラットフォームに関連付けられているローカライズ機能が使用されます。

**適切な場合**

プロバイダー ホスト型アドインを作成する場合は、Web ホスティング プラットフォームに付属しているローカライズ テクノロジを使用すると、カスタム コードの導入や複雑性が発生しない方法でアドインを構築するため最適です。

**はじめに**

次の記事では、リソース ファイルと JavaScript を使用してプロバイダー ホスト型アドインをローカライズする方法について説明します。

- [SharePoint アドインをローカライズする (MSDN 記事)](https://msdn.microsoft.com/en-us/library/fp179919(v=office.15).aspx)
- [アドイン Web、ホスト Web、およびアドインのリモート コンポーネントをローカライズする (MSDN コード サンプル)](https://code.msdn.microsoft.com/office/SharePoint-2013-Bookstore-328060fc)

関連リンク
=============

- [SharePoint アドインをローカライズする (MSDN 記事)](https://msdn.microsoft.com/en-us/library/fp179919(v=office.15).aspx)
- [アドイン Web、ホスト Web、およびアドインのリモート コンポーネントをローカライズする (Office Dev GitHub サンプル)](https://github.com/OfficeDev/SharePoint-Add-in-Localization)
- ガイダンス記事: [http://aka.ms/OfficeDevPnPGuidance](http://aka.ms/OfficeDevPnPGuidance "ガイダンス記事")
- MSDN リファレンス: [http://aka.ms/OfficeDevPnPMSDN](http://aka.ms/OfficeDevPnPMSDN "MSDN リファレンス")
- ビデオ: [http://aka.ms/OfficeDevPnPVideos](http://aka.ms/OfficeDevPnPビデオ"Videos")

関連する PnP サンプル
===================

- [VariationsExtensions.cs クラス (O365 PnP サンプル)](https://github.com/OfficeDev/PnP-Sites-Core/tree/master/Core/OfficeDevPnP.Core/AppModelExtensions/VariationExtensions.cs)
- [http://aka.ms/OfficeDevPnP](http://aka.ms/OfficeDevPnP) にあるサンプルとコンテンツ

適用対象
==========
- Office 365 マルチテナント (MT)
- Office 365 専用 (D)
- SharePoint 2013 オンプレミス
