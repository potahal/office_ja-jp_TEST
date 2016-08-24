---
ms.Toctitle: Platform overview
title: "Office 365 API のプラットフォームの概要"
description: "ms.TocTitle: プラットフォームの概要Title: Office 365 API プラットフォームの概要Description: Office 365 API を使用すると、お客様の Office 365 データにアクセスするカスタム ソリューションを作成したり、それらのアプリをモバイル、Web、およびデスクトップ プラットフォームにわたって構築したりできます。ms.ContentId: 16fbf0c0-5470-466b-aab8-a0c9074c94e2 ms.topic: 記事 (方法)"
ms.ContentId: 16fbf0c0-5470-466b-aab8-a0c9074c94e2
ms.date: July 20, 2015
---

<script src="https://cdn.optimizely.com/js/3043860527.js"></script>

[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]



# O365API リポジトリ スタイルの追加

_**適用対象:** Office 365_

[!INCLUDE [Use Microsoft Graph](../includes/use-msgraph-note.txt)]

Office 365 のリッチ データをアプリに組み込む、Office 365 そのものの中でカスタム エクスペリエンスを作成する、カスタム レポートを使用して Office 365 Enterprise 環境を円滑に実行できるようにするといった目標を実現するために、次に示す開発者向けの機能を使用できます。 

Office 365 のリッチ データをアプリに組み込む、Office 365 そのものの中でカスタム エクスペリエンスを作成する、カスタム レポートを使用して Office 365 Enterprise 環境を円滑に実行できるようにするといった目標を実現するために、次に示す開発者向けの機能を使用できます。<table id="mytable">
    <tr>
        <td>
            ![Office 365 API](images\O365_APIsAndSuiteApps_1.png)

        </td>
        <td>

                <p>**Office 365 APIs.**</p>
                
                <p>Integrate Office 365 data into your own apps</p> 
                
                <p>All right from within your app itself.</p>
                
                <p>See the [Office 365 API](..\api\api-catalog.md).</p>
        </td>
    </tr>
    <tr>
        <td>
            ![Office 365 API](images\O365_APIsAndSuiteApps_2.png)
        </td>
        <td>
            <p>**Office 365 APIs.**</p>
            
            <p>Create custom experiences within Office 365</p>  
                    
            <ul>
                <li>[Create a FileHandler add-in](..\howto\using-cross-suite-apps.md)。ユーザー作成の種類のファイルを SharePoint Online に表示する方法と操作性を制御できます。たとえば、ユーザー作成の種類のファイルのアイコン、Office 365 UI に表示されるファイル プレビュー、その種類のファイルをカスタム エディターで開く方法などを調整できます。 And since FileHandler add-ins host their data and logic remotely, you can develop your add-in using the language, tools, and web development stack of your choice.
                </li>
                <li>[Add your app to the app launcher](..\howto\connect-your-app-to-o365-app-launcher.md)。アプリを Office 365 のホーム ページに表示し、直接アクセスできるようにします。 Take advantage of Azure AD single sign-on to provide seamless access to your app for authorized users.
                </li>
            </ul>

        </td>
    </tr>
    <tr>
        <td>
            ![Take advantage of Azure AD single sign-on to provide seamless access to your app for authorized users.](images\O365_APIsAndSuiteApps_3.png)
        </td>
        <td>
            <p>**Office 365 APIs.**</p>
            
            <p>Analyze and manage the health of your Office 365 Enterprise environment</p>
            
            <ul>
                <li>[Access the Reporting web service](https://msdn.microsoft.com/en-us/library/office/jj984325.aspx) to build reporting dashboards, charts, and graphs to help their organization manage their subscription usage.
                </li>
                <li>[Access the Reporting web service](https://msdn.microsoft.com/library/office/dn707386.aspx) to build reporting dashboards, charts, and graphs to help their organization manage their subscription usage.</li>
                <li>Office 365 管理アクティビティ API は、Office 365 と Azure AD のアクティビティ ログから、ユーザー、管理者、システム、およびポリシー アクションとポリシー イベントについての情報を取得するために使用します。 Use this information to build solutions that provide monitoring, analysis, and data visualizations. 
                </li>
            </ul>

        </td>
    </tr>
    
</table>

Word、Excel、PowerPoint などの Office クライアントや SharePoint 2013 および SharePoint Online 内で、カスタム エクスペリエンスを作成することもできます。詳細については、「Office アドイン」および「SharePoint アドイン」を参照してください。 To learn more, see [Office add-ins](https://msdn.microsoft.com/library/jj220060.aspx) and [SharePoint add-ins](https://msdn.microsoft.com/library/fp179930.aspx).
  

## 予定表、連絡先
<a name="bk_addresources"> </a>


### 全般
-  [Code samples on dev.office.com](http://dev.office.com/code-samples#?filters=office%20365%20app)

-  [Office 365 開発環境のセットアップ](..\howto\setup-development-environment.md)
    
-  [Office 365 のアプリ認証とリソース承認](..\howto\common-app-authentication-tasks.md)

-  [ブログ: Office 365 API 用の .NET および JavaScript ライブラリ](http://blogs.office.com/2014/05/12/net-and-javascript-libraries-for-office-365-apis/)

-  [Visual Studio および Office 365 クライアント ライブラリ用の Office 365 API ツール](http://aka.ms/clientlibrary)

-  [Office 365 API クライアント ライブラリの理解](http://chakkaradeep.com/index.php/getting-familiar-with-the-office-365-api-client-libraries/)

-  [Office 365 API クライアント ライブラリ - Office 365 に対するクライアントの認証](http://chakkaradeep.com/index.php/office-365-api-client-libraries-authenticating-your-client-to-office-365/)

-  [夏の更新プログラムでの Office 365 API 認証ライブラリへの変更](http://chakkaradeep.com/index.php/changes-to-office-365-api-authentication-library-in-the-summer-update/)

-  [Training videos on the Office Dev Center](http://dev.office.com/training)

-  [Office 365 API 入門 (トレーニングビデオ)](http://www.microsoftvirtualacademy.com/training-courses/introduction-to-office-365-development?m=10072&ct=31602) 

### Android アプリ

-  [Android 用 Office 365 REST API アプリの開発](..\howto\getting-started-Office-365-APIs.md)

### Web アプリケーション
-  [Office 365 ASP.NET MVC アプリの構築](..\howto\getting-started-Office-365-APIs.md)

-  [Office 365 API と Python 第 1 部: OAuth2](http://blogs.msdn.com/b/exchangedev/archive/2015/01/05/office-365-apis-and-python-part-1-oauth2.aspx)
    
-  [Office 365 API と Python 第 2 部: 連絡先 API](http://blogs.msdn.com/b/exchangedev/archive/2015/01/09/office-365-apis-and-python-part-2-contacts-api.aspx)
    
-  [Office 365 API と Python 第 3 部: メールおよび予定表 API](http://blogs.msdn.com/b/exchangedev/archive/2015/01/15/office-365-apis-and-python-part-3-mail-and-calendar-api.aspx)

    

