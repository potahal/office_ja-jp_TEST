# リモート プロビジョニングと CSS を使用して SharePoint ページをカスタマイズする

CSS を使用して SharePoint のリッチ テキスト フィールドおよび Web パーツ領域をカスタマイズします。

_**適用対象:** Office 365? | SharePoint 2013? | SharePoint Online_

カスケード スタイル シート (CSS: Cascading Style Sheet) を使用すると、SharePoint のリッチ テキスト フィールドや Web パーツ領域をカスタマイズできます。リッチ テキスト フィールドのカスタマイズは、編集中のページで直接行うことができます。Web パーツの領域の場合は、Script Editor Web パーツを使用して HTML またはスクリプトを追加するか、CSS スタイル シートを関連付けることができます。この記事に関連するコード サンプルは、GitHub の [Office 365 Developer のパターンおよびプラクティス](https://github.com/OfficeDev/PnP)で [Branding.AlternateCSSAndSiteLogo](https://github.com/OfficeDev/PnP/tree/master/Samples/Branding.AlternateCSSAndSiteLogo) を参照してください。

## リッチ テキスト フィールドをカスタマイズする
<a name="sectionSection0"> </a>

リッチ テキスト フィールドをカスタマイズするには、ページ エディターで CSS を使用して直接作業します。

1. SharePoint ページで、**[編集]** を選択してページ エディターを開きます。
    
2. リボンで **[挿入]** > **[埋め込みコード]** を選択します。
    
これで、リッチ テキスト フィールドの CSS 要素を追加または変更できるようになります。

## Web パーツ領域をカスタマイズする
<a name="sectionSection1"> </a>

CSS を使用して Web パーツ領域をカスタマイズするには、スクリプト エディターの Web パーツを使用します。詳細については、「[SharePoint 2013 で、Script Editor Web パーツを使用する方法](http://community.bamboosolutions.com/blogs/sharepoint-2013/archive/2013/05/20/how-to-use-script-editor-web-part-in-sharepoint-2013.aspx)」を参照してください。


            **メモ** SharePoint Online と NoScript 機能を使用している場合、Script Editor Web パーツは無効になっています。 

次のコード例では、カスタム CSS をアセット ライブラリにアップロードし、カスタム アクションを使用して CSS URL への参照を適用し、新しい CSS ファイルへのリンクを作成するカスタム アクションを作成します。

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

                        // First, upload the CSS files to the Asset Library.
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
                        
                        // Clean up existing actions that we may have deployed.
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

                        // Build a custom action to write a link to your new CSS file.
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
    
-  [SharePoint 2013 で、Script Editor Web パーツを使用する方法](http://community.bamboosolutions.com/blogs/sharepoint-2013/archive/2013/05/20/how-to-use-script-editor-web-part-in-sharepoint-2013.aspx)
    
-  [ScriptEditorWebPart クラス](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.webpartpages.scripteditorwebpart.aspx)
