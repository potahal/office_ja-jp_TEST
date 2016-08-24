---
ms.Toctitle: File handlers overview
title: "Office 365 ファイル ハンドラーの概要"
description: "ファイル ハンドラーを使用すると、マイクロソフト以外のファイルの種類を Office 365 に統合し、アイコンを表示したり、新しいファイルを作成したり、プレビューしたり、マイクロソフト以外のエディターで開いたりできます。"
ms.ContentId: 78826f1a-6aa2-46ca-b2b4-a6caf5ee9040
ms.date: December 10, 2015

---
[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]



# Office 365 ファイル ハンドラーの概要


<!-- Get an overview of Office 365 file handler add-in for Office 365. -->

_**適用対象:**Office 365_

  
ファイル ハンドラーは、マイクロソフト以外のファイルの種類を Office のファイルの種類と同じ方法で Office 365 に統合できる新しい種類の Office のアドインです。

ファイル ハンドラーを使用すると、マイクロソフト以外のファイルの種類に対して次のユーザー エクスペリエンスを実現できます。
-  カスタマイズされたファイル アイコン
-  ブラウザーでの新しいファイルの作成
-  ファイルのプレビュー
-  多機能な表示/編集


<a name="sectionSection0"> </a>
## Office 365 ファイル ハンドラーの機能

ファイル ハンドラーは、次の内容で構成されています。 
-  **ファイル ハンドラー エンドポイント** クラウドでホストされるアプリであり、ファイル ハンドラーでサポートされる新しいファイルの種類の作成機能、プレビュー機能および編集機能をオプションで提供します。
-  **ファイル アイコン** Office 365 でファイルの種類を表すイメージです。

<a name="sectionSection1"> </a>
### ファイル ハンドラー アプリケーション

ファイル ハンドラー アプリケーションはクラウドでホストされるアプリケーションであり、そのアプリケーションが処理する種類のファイルを作成したり、プレビューしたり、開いたり、保存したりするための機能ロジックが含まれます。 Microsoft 以外のスタックなど、任意のスタックでホストできます。 ファイル ハンドラーは Azure AD を使用して Office 365 リソースへのアクセスの承認を得るので、アプリケーションを Azure AD に登録する必要があります。 Azure AD へのアプリケーションの登録の詳細については、以下を参照してください。「[Visual Studio を使用してアプリを登録し、Office 365 API を追加する](https://msdn.microsoft.com/office/office365/HowTo/adding-service-to-your-Visual-Studio-project)」および「[Office 365 API にアクセスできるようにアプリを手動で Azure AD に登録する](https://msdn.microsoft.com/en-us/office/office365/howto/add-common-consent-manually)」。

Visual Studio を使用して基本的なファイル ハンドラーを作成、展開、および登録する手順については、「[Office 365 でのファイル ハンドラーの作成](..\howto\create-file-handler-extensions.md)」を参照してください。

さらに複雑なファイル ハンドラーの例については、GitHub の「[GPX-FileHandler](https://github.com/OfficeDev/GPX-FileHandler)」を参照してください、


<a name="sectionSection2"> </a>
#### 実行時のファイル ハンドラー

 ファイル ハンドラーは、Azure AD アプリ マニフェストの addIns プロパティで指定されている **newFileUrl**、**openUrl** または **previewUrl** URL で呼び出されます。 動作を理解するために、ユーザーが省略記号 (**...**) をクリックしてファイル コールアウトを開く場合のシナリオについて調べてみましょう。 そのファイルの種類に対応する登録済みのファイル ハンドラーがある場合、Office 365 はアプリ マニフェストの **previewUrl** で指定されている URL に **POST** 要求を行うことでファイル ハンドラー アプリを呼び出します。この要求では、ファイルの場所とその他の詳細を渡します。 **previewUrl** は、ファイルのファイル ストリームを取得するファイル ハンドラー アプリケーション内のメソッドを指し示しています。その後、メソッドは、このストリームを使用してブラウザーでのファイルのプレビューを可能にします。
 
 これは開く機能と同じアプローチです。ユーザーが **[ブラウザーで編集]** をクリックした場合、またはライブラリでドキュメントのタイトルを直接クリックした場合、Office 365 は **openUrl** の URL に対して **POST** 要求を行います。このエンドポイントは、適切なエディターにファイル ストリームを読み込み、ユーザーがファイルを編集できるようにします。また、アプリケーションでは、更新されたファイル ストリームをユーザーがファイルの場所に戻してファイルを保存できるようにする必要もあります。ファイルの場所は、**filePut** アクティブ化パラメーターで指定されています。

ユーザーが **[新しいファイル]** をクリックすると、Office 365 は **newFileUrl** に **POST** 要求を送信します。 このエンドポイントにより、適切なエディターが開かれるため、ユーザーは新しいファイルにコンテンツを追加できるようになります。 アプリケーションでは、**filePut** アクティブ化パラメーターで指定されたファイルの場所に、ユーザーがファイルを保存できるようにする必要があります。

<a name="sectionSection3"> </a>
#### アクティブ化パラメーター

新しいファイルの作成、ファイルを開くおよびファイルのプレビューのシナリオでは、ファイルを操作するために、アクティブ化パラメーターと呼ばれる詳細 (ファイル、テナント、Office 365 クライアントなどに関する詳細) を必要とします。Office 365 は、ファイルを開くメソッドまたはファイルをプレビューするメソッドへの最初の **POST** 要求で送信するフォーム データとして、これらの詳細を含めます。

**表 1. ファイル ハンドラーが起動されたときに Office 365 が送信するアクティブ化パラメーターについての説明。**

|**パラメーター**|**説明**|
|:-----|:-----|:-----|
| **クライアント**|ファイルを開いた Office 365 クライアントまたはファイルをプレビューした Office 365 クライアント (例: "SharePoint")。|
| **CultureName**|現在のスレッドのカルチャ名。ローカリゼーションに使用されます。|
| **FileGet**|Office 365 からファイルを取得するためにアプリが呼び出す REST エンドポイントの完全な URL。アプリでは HTTP GET メソッドでこれを呼び出す必要があります。|
| **FilePut**|Office 365 にファイルを戻して保存するためにアプリが呼び出す REST エンドポイントの完全な URL。アプリでは HTTP POST メソッドでこれを呼び出す必要があります。|
| **ResourceId**|Azure AD からアクセス トークンを取得するために使用される Office 365 テナントの URL。|
| **FileId**|特定のドキュメントのドキュメント ID。アプリケーションは同時に複数のドキュメントを開くことができます。| 


Request.Form コレクションを使用して、要求本文からこれらの値にアクセスします。 たとえば次のようにします。

```cs
Request.Form["FileId"];
```

アプリケーションでは、これらの値をファイル ハンドラーが呼び出された直後にキャッシュする必要があります。アプリケーションがユーザーに対して初めて呼び出された時点では、アプリケーションはアクセス トークンを持っていないため、Azure AD にリダイレクトされて、OAuth 承認のコード フローがトリガーされることになります。すぐにキャッシュに格納すると、Azure AD の承認が完了した後でそれを使用でき、ブラウザーはファイル ハンドラーに戻るようにリダイレクトされます。データ モデル オブジェクトとハンドラー メソッドを使用して cookie に含まれるアクティブ化パラメーターをキャッシュする例については、[GPX FileHandler のサンプル](https://github.com/OfficeDev/GPX-FileHandler) (具体的には、[ActivationParameters.cs](https://github.com/OfficeDev/GPX-FileHandler/blob/7182d942a738564a53aa06362669dc074be40df6/MVCO365DemoMT/Models/ActivationParameters.cs) と [FileHandlerController.cs](https://github.com/OfficeDev/GPX-FileHandler/blob/7182d942a738564a53aa06362669dc074be40df6/MVCO365DemoMT/Controllers/FileHandlerController.cs)) を参照してください。

<a name="sectionSection4"> </a>
#### アプリ マニフェストと addIns プロパティ


ファイル ハンドラーの詳細は、アプリ マニフェストの addIns プロパティで指定します。addIns プロパティは、アプリに含まれたファイル ハンドラーと関連プロパティを詳細化します。Office 365 では、この詳細情報を使用して拡張子に対応するファイル ハンドラーを構成します。この構成により、その拡張子に対応する新しいファイルの動作、ファイルを開く動作およびプレビューの動作を指定します。 

マニフェストの構文は次のとおりです。

```json
{
    "addIns": [
        {
            "id": "unique guid",
            "type": "FileHandler",
            "properties": [
                {
                    "key": "extension",
                    "value": "List of file extensions separated by semicolons"
                },
                {
                    "key": "fileIcon",
                    "value": "URL of icon for the file type"
                },
                {
                    "key": "newFileUrl",
                    "value": "URL for the new file function"
                },
                {
                    "key": "openUrl",
                    "value": "URL for the file open function"
                },
                {
                    "key": "previewUrl",
                    "value": "URL for the file preview function"
                }
            ]
        }
    ]
}
```

ファイル ハンドラーを構成する場合は、Azure AD でアプリ マニフェストを更新する必要があります。現時点では、これを実行するための UI が Azure 管理ポータルに存在しません。そのため、Azure AD Graph API のクエリを使用する必要があります。詳細については、「[Office 365 でのファイル ハンドラーの構成と更新](..\howto\programmatically-install-update-or-delete-apps.md)」を参照してください。


<a name="sectionSection101"> </a>
## ファイル ハンドラーの使用可能性

次の表は、ファイル ハンドラーをサポートする Office 365 サービスの一覧です。

|**サービス名**|**Availability**|
|:-----|:-----|:-----|
|SharePoint Online|一般公開 (GA)|
|OneDrive for Business|GA|
|Outlook Web App|GA|

<a name="bk_addresources"> </a>
## このセクションの内容


    

-  [Office 365 でのファイル ハンドラーの作成](..\howto\create-file-handler-extensions.md)
    
-  [Office 365 のファイル ハンドラーのインストール、更新、および削除](..\howto\programmatically-install-update-or-delete-apps.md)
    

