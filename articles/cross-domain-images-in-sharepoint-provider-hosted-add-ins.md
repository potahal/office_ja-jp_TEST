# SharePoint のプロバイダー ホスト型アドインでのクロス ドメイン イメージ

プロバイダー ホスト型アドインでドメインを超えてイメージを使用します。

_**適用対象:** SharePoint 2013? | SharePoint Add-ins? | SharePoint Online_

SharePoint サイトのイメージをプロバイダー ホスト型アドインで表示したい場合があります。プロバイダー ホスト型アドインはリモート Web で実行されているため、プロバイダー ホスト型アドインと SharePoint サイトはドメインが異なります。たとえば、アドインが Microsoft Azure で実行されている場合に、イメージを Office 365 から表示しようとすることがあります。プロバイダー ホスト型アドインはイメージにアクセスするためにドメインを超えるので、プロバイダー ホスト型アドインがイメージを表示するには、その前に SharePoint によってユーザーが承認される必要があります。

[Core.CrossDomainImages](https://github.com/OfficeDev/PnP/tree/dev/Samples/Core.CrossDomainImages) コード サンプルは、プロバイダー ホスト型アドインが SharePoint アクセス トークンと REST サービスを使用して SharePoint からイメージを取得する方法を示しています。REST サービスは Base64 エンコード文字列を戻します。この文字列を使用してブラウザーにイメージを表示します。サーバー側またはクライアント側のコードを使用して、SharePoint に格納されているイメージをプロバイダー ホスト型アドインで表示するには、このサンプルのソリューションを使用してください。


            **メモ:**このサンプルでは REST エンドポイントを使用しているため、サーバー側とクライアント側のどちらのコードを使用してもイメージを取得できます。

## はじめに

まず、[Core.CrossDomainImages ](https://github.com/OfficeDev/PnP/tree/dev/Samples/Core.CrossDomainImages) サンプル アドインを GitHub の [Office 365 Developer パターンおよびプラクティス](https://github.com/OfficeDev/PnP/tree/dev) プロジェクトからダウンロードします。

## Core.CrossDomainImages コード サンプルを使用する

このサンプルを実行すると、Default.aspx は次の対象を読み込もうとします。

- イメージ 1 (絶対 URL を使用)。 
    
- イメージ 2 (Base64 エンコード文字列を返す、REST エンドポイントに対するサーバー側呼び出しを使用)。
    
- イメージ 3 (Base64 エンコード文字列を返す、REST エンドポイントに対するクライアント側呼び出しを使用)。
    

              **メモ:**  SharePoint のイメージを取得するためにアドインはドメインを超えるため、イメージ 1 は表示されません。プロバイダー ホスト型アドインの URL は localhost 上で実行されることに注意してください。イメージ 1 のショートカット メニュー (右クリック) を開き、**[プロパティ]** を選択します。**[アドレス (URL)]** は、localhost ではなく、アドイン Web からイメージを取得しようとすることに注意してください。プロバイダー ホスト型アドインはアドイン Web にアクセスするためにドメインを超えるので、イメージにアクセスするには認証が必要です。**[アドレス (URL)]** をコピーして新しいブラウザー ウィンドウに貼り付けることによってイメージ 1 の URL に直接アクセスする (プロバイダー ホスト型アドインによるクロス ドメイン呼び出しではなく) と、エラーなしに解決されることを確認してください。イメージ 1 がエラーなく表示されることを確認します。イメージ 1 のソースとイメージ 2 のソースを比較します。**[アドレス (URL)]** が Base64 エンドポイント文字列を指していることを確認します。

Default.aspx を読み込むと、**Page_Load** が実行され、以下が行われます。

1. Image1.ImageUrl がイメージの絶対パスに設定されます。
    
2. ImgService がインスタンス化されます。 ImgService は、プロバイダー ホスト型アドインと同じドメインで実行される REST エンドポイントです。
    
3. Image2.ImageUrl が、**ImgService.GetImage** から返される Base64 エンコード文字列に設定されます。 **ImgService.GetImage** には、パラメーターとしてアクセス トークン、サイト、フォルダー、ファイル名が渡されます。
    
**メモ:** この記事で提供されるコードは、明示または黙示のいかなる種類の保証なしに現状のまま提供されるものであり、特定目的への適合性、商品性、権利侵害の不存在についての暗黙的な保証は一切ありません。

```C#
 protected void Page_Load(object sender, EventArgs e)
        {
            var spContext = SharePointContextProvider.Current.GetSharePointContext(Context);
            using (var clientContext = spContext.CreateUserClientContextForSPAppWeb())
            {
                // Set the access token in a hidden field for client-side code to use.
                hdnAccessToken.Value = spContext.UserAccessTokenForSPAppWeb;

                // Set the URLs to the images.
                Image1.ImageUrl = spContext.SPAppWebUrl + "AppImages/O365.png";
                Services.ImgService svc = new Services.ImgService();
                Image2.ImageUrl = svc.GetImage(spContext.UserAccessTokenForSPAppWeb, spContext.SPAppWebUrl.ToString(), "AppImages", "O365.png");
            }
        }
```

**GetImage** は以下を実行します。

1. **url** を使用して GetFolderByServerRelativeUrl REST エンドポイント URI を格納します。この URI は、SharePoint からのイメージを取得するときに使用されます。 詳細については、「[ファイルおよびフォルダー REST API リファレンス](http://msdn.microsoft.com/library/2c3d2545-1cd7-497e-b535-12199d8edfbb%28Office.15%29.aspx)」を参照してください。
    
2. **url** を使用して、[HttpWebRequest](https://msdn.microsoft.com/library/system.net.httpwebrequest.aspx) オブジェクトをインスタンス化します。
    
3. アクセス トークンが含まれる HttpWebRequest オブジェクトに Authorization ヘッダーを追加します。 
    
対象のエンドポイント URI に GET 呼び出しを行った後、戻されるストリームは Base64 エンコード文字列です。 戻される文字列はイメージの **src** 属性に設定されます。

```C#
 public string GetImage(string accessToken, string site, string folder, string file)
        {
            // Create the HttpWebRequest to call the REST endpoint.
            string url = String.Format("{0}_api/web/GetFolderByServerRelativeUrl('{1}')/Files('{2}')/$value", site, folder, file);
            HttpWebRequest request = WebRequest.Create(url) as HttpWebRequest;
            request.Headers.Add("Authorization", "Bearer" + " " + accessToken);
            using (HttpWebResponse response = request.GetResponse() as HttpWebResponse)
            {
                using (var sourceStream = response.GetResponseStream())
                {
                    using (var newStream = new MemoryStream())
                    {
                        sourceSteam.CopyTo(newStream);
                        byte[] bytes = newStream.ToArray();
                        return "data:image/png;base64, " + Convert.ToBase64String(bytes);
                    }
                }
            }
        }
```

**Page_Load** の終了後、Default.aspx は Default.aspx のクライアント側コードを実行し、イメージ 3 が読み込まれます。クライアント側コードは jQuery Ajax を使用して **GetImage** を呼び出します。**GetImage** が正常に Base64 エンコード文字列を戻すと、その返された文字列は、イメージ 3 の **src** 属性に設定されます。

```
  ...

                  $.ajax({
                url: '../Services/ImgService.svc/GetImage?accessToken=' + $('#hdnAccessToken').val() + '&amp;site=' + encodeURIComponent(appWebUrl + '/') + '&amp;folder=AppImages&amp;file=O365.png',
                dataType: 'json',
                success: function (data) {
                    $('#Image3').attr('src', data.d);
                },
                error: function (err) {
                    alert('error occurred');
                }
            });

           ...

```

## その他のリソース
<a name="bk_addresources"> </a>

- [Office 365 開発パターンとプラクティス (ソリューション ガイダンス)](Office-365-development-patterns-and-practices-solution-guidance.md)
