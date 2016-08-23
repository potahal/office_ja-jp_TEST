# CSS を使用して SharePoint ページをブランド化する
SharePoint でブランド化やサイト デザインをサポートするには、CSS を使用します。このトピックでは、カスタム ブランディングにおけるマスター ページ、corev15.css ファイル、および構成済みの外観での CSS の役割について説明します。

_**適用対象:** Office 365 | SharePoint 2013 | SharePoint Online_

カスケード スタイル シート (CSS: Cascading Style Sheet) は、SharePoint のブランド化で大きな役割を果たします。SharePoint 2013 および SharePoint Online でサイト デザインを適切にカスタマイズするには、SharePoint で CSS が使用される仕組みを知っておくと役立ちます。

## CSS によるブランド化の概要
<a name="sectionSection0"> </a>

SharePoint サイト コレクションを作成すると、SharePoint によってマスター ページ ギャラリー (_catalogs/masterpage) が作成され, .master, .css、画像、HTML、および JavaScript ファイルなど、ブランド化のための大部分の資産がそこに格納されます。

SharePoint 2013 では、チーム サイト、発行サイト、発行機能の付いたチーム サイトとして SharePoint によって既定で seattle.master ページが使用されます。また、OneDrive for Business サイトのために mysite15.master が使用されます。各 .master ファイルは corev15.css ファイルを参照しています。これは、SharePoint に同梱されているさまざまな .css ファイルを基にして作成したものです。

すべての既定のマスター ページは、スタイルを処理する際に corev15.css を使用します。ページの各領域にある特定のコントロールや要素に適用するスタイルを解決するために、その CSS カスケードおよび CSS ファイル共有に依存しています。また、複数の CSS ファイルを結合して作成される oslo.css ファイルは、oslo.master マスター ページで使用されます。

## マスター ページ内の CSS
<a name="sectionSection1"> </a>

`<SharePoint:CssRegistration>` コンテンツ プレースホルダーは、サーバー側のオブジェクト モデルの [WebControls.CssRegistration](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.webcontrols.cssregistration.aspx) クラスに対応し、CSS ファイルを定義します。 マスター ページへの参照と同様に、SharePoint はページを処理する際に、マスター ページ内のトークンを置き換えます。 [WebControls.CssLink](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.webcontrols.csslink.aspx) クラスはマスター ページから登録を読み取り、生成される結果の HTML ファイルに `<link>` タグを挿入します。

次の例を検討してください。

```
<SharePoint:CssRegistration name="<% $SPUrl:~SiteCollection/Style Library/~language/Core Styles/contoso.css%>" runat="server"/>
```

実行時に、このコードは次のようにレンダリングされます。

```
<link rel="stylesheet" type="text/css" href="/sites/nopub/Style%20Library/en-US/Core%20Styles/contoso.css">
```

**CSSLink** クラスは、ページのレンダリングの際にすべてのスタイル シートをレンダリングします。カスタムの .css ファイルで定義しているスタイルが corev15.css でも定義されている場合は、上書きされます。

## Corev15.css ファイル
<a name="sectionSection2"> </a>

多数の CSS ファイルが既定で SharePoint に適用されます。corev15.css ファイルは、SharePoint 内のスタイルの主要なソースです。このファイルは、ルート サイトやホーム ページのマスター ページ ギャラリーではなく、サーバー上の SharePoint 15 フォルダー ( ` \TEMPLATE\LAYOUTS\1033\STYLES\Themable\COREV15.CSS`) に格納されます。

同梱されている CSS が SharePoint によって使用される仕組みを見るには、Web ブラウザーで開発者ツールを使用して corev15.css ファイルをご覧ください。Internet Explorer では、Internet Explorer 開発者ツール バー ( **F12** を押すとアクセスできます) を使用し、 **[CSS]** タブを選択して corev15.css ファイルを表示します。

corev15.css で定義されているスタイルは, .ms-、および .s4- というプレフィックスを使用して、それが Microsoft によって作成されたスタイルであることを示します。また、corev15.css では、多くの子セレクターおよび子孫セレクターも使用されています。 

ファイルをご覧になるとわかるとおり、 `/* [ReplaceFont ( themeFont:"body")] */` という形式のコメントが多数使用されています。構成済みの外観を適用するとき、SharePoint はこれらのコメントを読み取ります。これらのコメントは、コメントの直後にある CSS の属性を変更するように SharePoint に指示します。構成済みの外観を適用すると、適用される多くの既定の色、フォント、および背景画像が変更され、その結果として corev15.css の設定が更新されます。

**メモ**  このようにして corev15.css ファイルを選択すると、保存された CSS ではなく、レンダリングされた CSS が読み込まれます。場合によっては、この 2 つの間で不一致が見られることがあります。また、ブラウザーなどのユーザー エージェントによって、ユーザーの操作に応答して行われるレンダリングが変わることもあります。

**重要**  サーバーにログオンして SharePoint ルートにあるコア SharePoint CSS ファイルを編集またはカスタマイズすることは避けてください。そのようにした場合、サポートやアップグレードに悪影響が及びます。corev15.css は決して直接には編集しないでください。いつでも、コピーを作成し、名前を変更して、その新しいファイルを編集するようにしてください。corev15.css を編集すると、サーバー上のすべての Web アプリケーションにブランド化が適用されます。

### SharePoint 用にカスタム スタイル シートを作成するには

1. corev15.css のコピーを作成し、「contosov15.css」という名前を付けます。
    
2. contosov15.css を開き、CssRegistration の行を次の例のように変更して、corev15.css ではなく contosov15.css を指すようにします。
    
  ```
  <SharePoint:CssRegistration Name="Themable/contoso.css" runat="server" />
  ```

3. CssRegistration 行の下に、次のような行を追加します。
    
  ```
  <SharePoint:CssRegistration name="/_catalogs/masterpage/contoso/contoso.css" 
runat="server">

  ```

## カスタム ブランディングでの構成済みの外観
<a name="sectionSection3"> </a>

マスター ページから CSS を呼び出す場合、カスタム ブランディングの中で構成済みの外観を使用できます。CSS ファイルは、SharePoint のファイル システム内で次のいずれかの場所に格納されています。 

-  `15\TEMPLATE\LAYOUTS\{LCID}\STYLES\Themable`
    
-  `/Style Library/Themable/`
    
-  `/Style Library/{culture}/Themable/`
    
カスタムのブランド化ファイルは  `/Style Library/Themable/` や `/Style Library/{culture}/Themable/` に置くことができますが、 `15\TEMPLATE\LAYOUTS\{LCID}\STYLES\Themable` は編集可能ではないため、その場所にはカスタム ファイルを格納できません。

**メモ**  既定時にこれらの場所が存在しない場合は、手動で作成すれば、SharePoint によってそれらの場所がテーマ設定可能として認識されます。

## SharePoint ページにカスタム CSS を適用する
<a name="sectionSection4"> </a>

カスタム CSS は、リッチ テキスト フィールドおよび Web パーツ領域に追加できます。リッチ テキスト フィールドに CSS を追加するには、ページを編集モードにして、リボンから  **[挿入]** > **[埋め込みコード]** を選択します。Web パーツ領域の場合は、Script Editor Web パーツを使用して HTML、スクリプト、または内部スタイル シートを追加します。既定の SharePoint UI でクイック起動ナビゲーションを非表示にするために、この手法を使用できます。SharePoint Online を使用している場合に、NoScript 機能を使用すると、NoScript によって Script Editor Web パーツが無効にされます。

外部スタイル シートを使用して CSS を SharePoint サイトに適用するには、そのスタイル シートへの参照を SharePoint ページの  `<link>` タグの内側にある `<head>` タグに組み込みます。

次のコード例では、カスタム CSS をアセット ライブラリにアップロードし、カスタム アクションを使用して CSS URL への参照を適用し、新しい CSS ファイルへのリンクを作成するカスタム アクションを作成する方法を示します。このコードは、GitHub にある [Branding.AlternateCSSAndSiteLogo](https://github.com/OfficeDev/PnP/tree/master/Samples/Branding.AlternateCSSAndSiteLogo) サンプルの一部です。

```C#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

using System.IO;
using Microsoft.SharePoint.Client;
using Microsoft.SharePoint.Client.EventReceivers;

namespace AlternateCSSAppAutohostedWeb.Services
{
    public class AppEventReceiver : IRemoteEventService
    {
        public SPRemoteEventResult ProcessEvent(SPRemoteEventProperties properties)
        {
            SPRemoteEventResult result = new SPRemoteEventResult();

            try
            {
                using (ClientContext clientContext = TokenHelper.CreateAppEventClientContext(properties, false))
                {
                    if (clientContext != null)
                    {
                        Web hostWebObj = clientContext.Web;

                        List assetLibrary = hostWebObj.Lists.GetByTitle("Site Assets");
                        clientContext.Load(assetLibrary, l => l.RootFolder);

                        // First, upload the CSS files to the Asset library.
                        DirectoryInfo themeDir = new DirectoryInfo(System.Web.Hosting.HostingEnvironment.ApplicationPhysicalPath + "CSS");
                        foreach (var themeFile in themeDir.EnumerateFiles())
                        {
                            FileCreationInformation newFile = new FileCreationInformation();
                            newFile.Content = System.IO.File.ReadAllBytes(themeFile.FullName);
                            newFile.Url = themeFile.Name;
                            newFile.Overwrite = true;

                            Microsoft.SharePoint.Client.File uploadAsset = assetLibrary.RootFolder.Files.Add(newFile);
                            clientContext.Load(uploadAsset);
                            break;
                        }
                        
                        string actionName = "SampleCSSLink";

                        // Now, apply a reference to the CSS URL via a custom action.
                        
                        // Clean up existing actions that you might have deployed.
                        var existingActions = hostWebObj.UserCustomActions;
                        clientContext.Load(existingActions);

                        // Run uploads and initialize the existing Actions collection.
                        clientContext.ExecuteQuery();

                        // Clean up existing actions.
                        foreach (var existingAction in existingActions)
                        {
                            if(existingAction.Name.Equals(actionName, StringComparison.InvariantCultureIgnoreCase))
                                existingAction.DeleteObject();
                        }
                        clientContext.ExecuteQuery();

                        // Build a custom action to write a link to our new CSS file.
                        UserCustomAction cssAction = hostWebObj.UserCustomActions.Add();
                        cssAction.Location = "ScriptLink";
                        cssAction.Sequence = 100;
                        cssAction.ScriptBlock = @"document.write('<link rel=""stylesheet"" href=""" + assetLibrary.RootFolder.ServerRelativeUrl + @"/cs-style.css"" />');";
                        cssAction.Name = actionName;
                        
                        // Apply.
                        cssAction.Update();
                        clientContext.ExecuteQuery();
                    }
                    result.Status = SPRemoteEventServiceStatus.Continue;
                    return result;
                }
            }
            catch (Exception ex)
            {
                result.Status = SPRemoteEventServiceStatus.CancelWithError;
                result.ErrorMessage = ex.Message;
                return result;
            }
            
        }

        public void ProcessOneWayEvent(SPRemoteEventProperties properties)
        {
            // This method is not used by app events.
        }
    }
}
```

## その他のリソース
<a name="bk_addresources"> </a>

-  [SharePoint サイト ブランド化とページ カスタマイズのソリューション](SharePoint-site-branding-and-page-customization-solutions.md)
    
-  [Branding.AlternateCSSAndSiteLogo サンプル](https://github.com/OfficeDev/PnP/tree/master/Samples/Branding.AlternateCSSAndSiteLogo)
