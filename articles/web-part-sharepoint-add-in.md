SharePoint アドイン モデルにおける Web パーツ
=======================================

概要
-------

新しい SharePoint アドイン モデルにおいて、ポータブル ページ コンポーネントの作成方法は、完全信頼コードの場合とは異なります。一般的な完全信頼コード (FTC) ファーム / ソリューションの場合、Web パーツはポータブル ページ コンポーネントを実装するよう作成されていました。

SharePoint アドイン モデル シナリオでは、アドイン パーツ (アプリ パーツ) はポータブル ページ コンポーネントを実装するために作成されます。アドイン パーツは、クライアント側コードを使用します。

基本ガイドライン
---------------------

アドイン パーツについては、大まかに次のような基本ガイドラインが提供されています。

- アドイン パーツでは、サーバー側コードを実行できません。
- アドイン パーツにカスタムのエディター パーツを使用することはできません。
- アドイン スクリプト パーツを使用し、SharePoint やその他のサービスとのやり取りに使用される Javascript にリンクし、ユーザー インターフェースを作成します。
- 既定では、エディター パーツに追加するカスタム プロパティは、エディター パーツ内の最終グループとして常に表示されます。
    + JavaScript を使用してアドイン パーツのエディター パーツの外観を上書きできます。
    + これを実行する方法を示す次のサンプルを参照してください。 
    + [Core.AppPartPropertyUIOverride (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.AppPartPropertyUIOverride)

**はじめに**

アドイン パーツは、標準のアドイン スクリプト パーツを使用して簡単に作成できます。これにより、任意の場所でホストされている JavaScript ファイルへのリンクを提供することができます。JavaScript ファイルは、クライアント側のコードを使用して SharePoint やその他のサービスとやり取りし、ユーザー インターフェイスをレンダリングします。

次の記事は、アドイン スクリプト パーツのパターンとその使用方法について説明します。

- [Office365 アプリ モデル用アプリ スクリプト パーツ パターンについて (MSDN ブログ記事)](http://blogs.msdn.com/b/vesku/archive/2014/07/08/introducing-app-script-part-pattern-for-office365-app-model.aspx)

次のサンプルは、アドイン スクリプト パーツを使用して Yammer、Bing Maps、Google マップと統合する方法について説明します。

- [Core.AppScriptPart (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.AppScriptPart)

次のビデオでは、コードのサンプルについて説明します。

- [Core.AppScriptPart (O365 PnP ビデオ)](https://channel9.msdn.com/Blogs/Office-365-Dev/App-Script-Parts-in-SharePoint-Office-365-Developer-Patterns-and-Practices)

関連リンク
=============

- [Office365 アプリ モデル用アプリ スクリプト パーツ パターンについて (MSDN ブログ記事)](http://blogs.msdn.com/b/vesku/archive/2014/07/08/introducing-app-script-part-pattern-for-office365-app-model.aspx)
- [Core.AppScriptPart (O365 PnP ビデオ)](https://channel9.msdn.com/Blogs/Office-365-Dev/App-Script-Parts-in-SharePoint-Office-365-Developer-Patterns-and-Practices)
- ガイダンス記事: [http://aka.ms/OfficeDevPnPGuidance](http://aka.ms/OfficeDevPnPGuidance "ガイダンス記事")
- MSDN リファレンス: [http://aka.ms/OfficeDevPnPMSDN](http://aka.ms/OfficeDevPnPMSDN "MSDN リファレンス")
- ビデオ: [http://aka.ms/OfficeDevPnPVideos](http://aka.ms/OfficeDevPnPビデオ"Videos")

関連する PnP サンプル
===================

- [Core.AppPartPropertyUIOverride (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.AppPartPropertyUIOverride)
- [Core.AppScriptPart (O365 PnP サンプル)](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.AppScriptPart)
- [http://aka.ms/OfficeDevPnP](http://aka.ms/OfficeDevPnP) にあるサンプルとコンテンツ

適用対象
==========
- Office 365 マルチテナント (MT)
- Office 365 専用 (D)
- SharePoint 2013 オンプレミス
