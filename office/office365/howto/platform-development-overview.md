---
ms.Toctitle: Platform overview
title: "Office 365 API プラットフォームの概要"
description: "Office 365 API を使用すると、お客様の Office 365 データにアクセスするカスタム ソリューションを作成したり、それらのアプリをモバイル、Web、およびデスクトップ プラットフォームにわたって構築したりできます。"
ms.ContentId: 16fbf0c0-5470-466b-aab8-a0c9074c94e2
ms.date: July 20, 2015
---

<script src="https://cdn.optimizely.com/js/3043860527.js"></script>

[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]



# Office 365 API プラットフォームの概要

_**適用対象:**Office 365_

[!INCLUDE [Use Microsoft Graph](../includes/use-msgraph-note.txt)]

Office 365 のリッチ データをアプリに組み込む、Office 365 そのものの中でカスタム エクスペリエンスを作成する、カスタム レポートを使用して Office 365 Enterprise 環境を円滑に実行できるようにするといった目標を実現するために、次に示す開発者向けの機能を使用できます。 

<style>#mytable {border:none;} #mytable td {border:none;}</style>
<table id="mytable">
    <tr>
        <td>
            ![Office 365 API。](images\O365_APIsAndSuiteApps_1.png)

        </td>
        <td>

                <p>**Office 365 のデータを独自のアプリに統合する**</p>
                
                <p>ユーザーの豊富な Office 365 データにアクセスし、やり取りできるカスタム ソリューションを作成でき、これらのソリューションをすべてのモバイル、Web、デスクトップ プラットフォーム間で構築できます。新しいOffice 365 API は、メール、予定表、連絡先、ファイル、フォルダーを含む Office 365 データへのアクセスを可能にします。すべてアプリから直接できます。</p> 
                
                <p>.NET、PHP、Java、Python、Ruby on Rails を使用して Web アプリケーションを構築しても、Windows 8、ユニバーサル アプリ、iOS、Android や他のデバイス プラットフォーム用のアプリを作成してもかまいません。</p>
                
                <p>「[Office 365 API](..\api\api-catalog.md)」を参照してください。</p>
        </td>
    </tr>
    <tr>
        <td>
            ![Office 365 API。](images\O365_APIsAndSuiteApps_2.png)
        </td>
        <td>
            <p>**Office 365 内でカスタムのエクスペリエンスを作成する**</p>
            
            <p>Create custom experiences within Office 365</p>  
                    
            <ul>
                <li>[FileHandler アドインを作成する](..\howto\using-cross-suite-apps.md)ことで、Office 365 でカスタムのファイルの種類を表示する方法と操作する方法を制御できます。たとえば、カスタムのファイルの種類のアイコン、Office 365 UI でのファイル プレビュー、そのファイルの種類をカスタム エディターで作成する方法や開く方法などを制御できます。 また、FileHandler アドインはデータとロジックをリモートでホストするため、任意の言語、ツール、および Web 開発スタックを使用してアドインを開発できます。
                </li>
                <li>[アプリ起動ツールにアプリを追加する](..\howto\connect-your-app-to-o365-app-launcher.md)ことで、Office 365 のホーム ページにアプリを表示して、すぐにアクセスできるようになります。 Azure AD シングル サインオンを利用すると、承認されたユーザーはアプリにシームレスにアクセスできます。
                </li>
            </ul>

        </td>
    </tr>
    <tr>
        <td>
            ![Office 365 API。](images\O365_APIsAndSuiteApps_3.png)
        </td>
        <td>
            <p>**Office 365 Enterprise 環境の正常性を分析および管理する**</p>
            
            <p>Office 365 Enterprise ではさまざまな開発者向け機能が用意されており、管理者はドメインとサブスクリプションを効果的かつ適切に調整された状態で保持できます。</p>
            
            <ul>
                <li>
  [レポート Web サービスにアクセスする](https://msdn.microsoft.com/en-us/library/office/jj984325.aspx)ことで、レポート ダッシュボード、チャート、およびグラフを作成して、組織がサブスクリプションの使用状況を管理できます。
                </li>
                <li>
  [Access the Reporting web service](https://msdn.microsoft.com/library/office/dn707386.aspx) to build reporting dashboards, charts, and graphs to help their organization manage their subscription usage.</li>
                <li>
  [Office 365 管理アクティビティ API を使用する](https://msdn.microsoft.com/library/office/mt227394.aspx)ことで、Office 365 と Azure AD のアクティビティ ログから、ユーザー、管理者、システム、およびポリシーのアクションとイベントについての情報を取得できます。 この情報を使用して、監視、分析、およびデータのビジュアル化を提供するソリューションを構築します。 
                </li>
            </ul>

        </td>
    </tr>
    
</table>

Word、Excel、PowerPoint などの Office クライアントや SharePoint 2013 および SharePoint Online 内で、カスタム エクスペリエンスを作成することもできます。 詳細については、「[Office アドイン](https://msdn.microsoft.com/library/jj220060.aspx)」および「[SharePoint アドイン](https://msdn.microsoft.com/library/fp179930.aspx)」を参照してください。
  

## その他のリソース
<a name="bk_addresources"> </a>


### 全般
-  [dev.office.com のコード サンプル](http://dev.office.com/code-samples#?filters=office%20365%20app)

-  [Office 365 開発環境のセットアップ](..\howto\setup-development-environment.md)
    
-  [Office 365 のアプリ認証とリソース承認](..\howto\common-app-authentication-tasks.md)

-  [ブログ: Office 365 API 用の .NET および JavaScript ライブラリ](http://blogs.office.com/2014/05/12/net-and-javascript-libraries-for-office-365-apis/)

-  [Visual Studio および Office 365 クライアント ライブラリ用の Office 365 API ツール](http://aka.ms/clientlibrary)

-  [Office 365 API クライアント ライブラリの理解](http://chakkaradeep.com/index.php/getting-familiar-with-the-office-365-api-client-libraries/)

-  [Office 365 API クライアント ライブラリ - Office 365 に対するクライアントの認証](http://chakkaradeep.com/index.php/office-365-api-client-libraries-authenticating-your-client-to-office-365/)

-  [夏の更新プログラムでの Office 365 API 認証ライブラリへの変更](http://chakkaradeep.com/index.php/changes-to-office-365-api-authentication-library-in-the-summer-update/)

-  [Office デベロッパー センターでのトレーニング ビデオ](http://dev.office.com/training)

-  [Office 365 API 入門 (トレーニングビデオ)](http://www.microsoftvirtualacademy.com/training-courses/introduction-to-office-365-development?m=10072&ct=31602) 

### Android アプリ

-  [Android 用 Office 365 REST API アプリの開発](..\howto\getting-started-Office-365-APIs.md)

### Web アプリケーション
-  [Office 365 ASP.NET MVC アプリの構築](..\howto\getting-started-Office-365-APIs.md)

-  [Office 365 API と Python 第 1 部: OAuth2](http://blogs.msdn.com/b/exchangedev/archive/2015/01/05/office-365-apis-and-python-part-1-oauth2.aspx)
    
-  [Office 365 API と Python 第 2 部: 連絡先 API](http://blogs.msdn.com/b/exchangedev/archive/2015/01/09/office-365-apis-and-python-part-2-contacts-api.aspx)
    
-  [Office 365 API と Python 第 3 部:メールおよび予定表 API](http://blogs.msdn.com/b/exchangedev/archive/2015/01/15/office-365-apis-and-python-part-3-mail-and-calendar-api.aspx)

    

