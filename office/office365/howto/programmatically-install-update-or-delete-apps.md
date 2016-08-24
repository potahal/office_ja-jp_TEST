---
ms.Toctitle: Configure and update file handlers
title: "Office 365 でのファイル ハンドラーの構成と更新"
description: "新しいファイル ハンドラーの構成や、既存のファイル ハンドラー構成の更新を実行します。"
ms.ContentId: ae338417-ced2-49f1-93e5-8afa05c03a5e
ms.date: December 10, 2015

---
[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]



# Office 365 でのファイル ハンドラーの構成と更新

<!-- Learn how to register new file handlers, and update configuration for existing file handlers. -->


_**適用対象:**Office 365_



## アプリ マニフェストと addIns プロパティ
<a name="sectionSection0"> </a>

Azure Active Directory (AD) の [アプリケーション エンティティ](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#ApplicationEntity)には、ファイル ハンドラーをサポートするための新しいプロパティ **addIns** が含まれています。**addIns** プロパティは、アプリに含まれたファイル ハンドラーと関連プロパティを詳細化します。Office 365 では、この詳細情報を使用して拡張子に対応するファイル ハンドラーを構成します。この構成により、その拡張子に対応する新しいファイルの動作、ファイルを開く動作およびプレビューの動作を指定します。 

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

**注** プロパティのキーは、大文字と小文字が区別されます。

スキーマについては、次の表で説明します。

|**メンバー	**|**説明**|**データ型**|**親**|**子メンバー**|
|:-----|:-----|:-----|:-----|:-----|
| **addIns**|ルート要素。アプリケーションのファイル ハンドラーを定義するオブジェクトの配列が含まれます。|配列|なし|**type**<br/> **properties**|
| **id**|アドイン コレクション内で個別のアドインを識別します。この値は、GUID 生成ツールを使用して生成できます。|GUID|**addIns**|なし|
| **type**|アドインの型。 ファイル ハンドラーの場合、値は "FileHandler" になります。|string| **addIns**|なし|
| **properties**|ファイル ハンドラーのプロパティを定義します。|object| **addIns**|**extension**<br/> **fileIcon**<br/> **newFileUrl**<br/> **previewUrl**<br/> **openUrl**|
| **extension**|先頭の "." 文字を含まないファイル ハンドラーの拡張子 (例: "psd")。 セミコロンで区切ることで、複数の拡張子を指定できます。|string| **properties**|なし|
| **fileIcon**|ファイルの種類を表すアイコンの URL (例: "https://fabrikam.com/SampleFileHandler/psdicon.png")。プロトコルは https でなければならず、HTML ページで表示される任意のイメージ形式を使用できます。イメージは自動的にサイズ変更されます。このイメージに推奨される寸法は、32x32 ピクセルです。|string| **properties**|なし|
| **newFileUrl** |新規ファイル機能の URL (例: "https://fabrikam.com/SampleFileHandler/newFile")。 プロトコルは https でなければなりません。|string| **properties**|なし|
| **previewUrl**|プレビュー機能の URL (例: "https://fabrikam.com/SampleFileHandler/preview")。 プロトコルは https でなければなりません。|string| **properties**|なし|
| **openUrl**|編集用にファイルを開く機能の URL (例: "https://fabrikam.com/SampleFileHandler/open")。 プロトコルは https でなければなりません。|string| **properties**|なし|

ファイル ハンドラーを構成する場合は、**addIns** プロパティを使用して、Azure AD のアプリ マニフェストにファイル ハンドラーの詳細を追加する必要があります。現時点では、これを実行するための UI が Azure の管理ポータルに存在しません。そのため、[Azure AD Graph API](https://azure.microsoft.com/en-us/documentation/articles/active-directory-graph-api-quickstart/) のクエリを使用する必要があります。 

最初のクエリでは、アプリケーションの **objectId** を取得する必要があります。 これには、クエリのフィルター パラメーターとして **appId** を指定した **GET** 要求を使用する必要があります。
**注** **objectId** と **appId** は、どちらもアプリケーションを識別する GUID ですが、それぞれの値は異なります。

**appId** (クライアント ID とも呼ばれる) は、Azure 管理ポータルのアプリの構成ページにある **[クライアント ID]** プロパティでわかります。また、アプリケーションの web.config に含まれる **appSettings** の **ClientID** キーでもわかります。

**objectId** を取得するには、次に示す **GET** 要求を Azure AD Graph に送信します。

```
GET https://graph.windows.net/contoso.com/applications?api-version=1.6&$filter=appId%20eq%20'appId'
```

contoso.com は自分のテナント ドメインに置き換えてください。また、*appId* はクライアント ID の値に置き換えてください。このクエリは、**objectId** プロパティを格納しているアプリケーション オブジェクトを返します。

この時点で、ファイル ハンドラーの構成を更新するためのクエリを作成できます。次に例を示します。

```
PATCH https://graph.windows.net/contoso.com/applications/objectId/?api-version=1.6
```

contoso.com は自分のテナント ドメインに置き換えてください。また、*objectId* は最初のクエリで取得した **objectId** の値に置き換えてください。コンテンツ タイプを application/json に設定し、ファイル ハンドラーの構成の詳細を指定する json を要求の本文で渡します。


**注****addIns** プロパティは [Azure AD Graph 1.6 スキーマ](https://graph.windows.net/microsoft.com/$metadata?api-version=1.6) の一部であるため、前述の例に示したように、api-version パラメーターには 1.6 を指定する必要があります。

詳細については、「[Azure AD Graph API のバージョン管理](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-versioning)」を参照してください。

**注** アプリケーション構成の更新後、マニフェスト ファイルをダウンロードすると、変更内容は見えなくなります。

<a name="sectionSection1"> </a>
## AddIns Manager サンプル ツール

[AddIns Manager サンプル ツール](https://addinsmanager.azurewebsites.net/)を使用して、ファイル ハンドラーを構成および更新するための Azure AD Graph のクエリを実行できます (ファイル ハンドラー アプリケーションが Azure AD に登録してある場合)。AddIns Manager ツールを使用すると、Azure AD でアプリの構成が更新されます。

**注** AddIns Manager サンプル ツールは、デモおよびテストのみを目的としたものです。運用環境では使用しないでください。 

####AddIns Manager ツールを使用してファイル ハンドラーを構成するには
1. [AddIns Manager サンプル ツール](https://addinsmanager.azurewebsites.net/)に移動します。
2. ブラウザーにページが読み込まれたら、ページの右上にある **[Sign in]** をクリックします。
3. テナント管理者の資格情報を入力して、**[sign in]** をクリックします。
4. 左側のナビゲーション バーの **[My Applications]** で、ファイル ハンドラー アプリの名前を選択します。
5. **[Register Add-In]** をクリックします。
6. **[Register Add-In]** ダイアログで、**[File Handler]** を選択します。
7. **[File Handler Add-In]** のドロップダウンをクリックします。
8. ファイル ハンドラーの詳細を入力します。 **注** プロトコルは HTTPS にする必要があります。
9. **[Update Add-In]** をクリックします。 


ファイル ハンドラーが正常に構成されると、正常な更新を確認するダイアログ ボックスが表示されます。

