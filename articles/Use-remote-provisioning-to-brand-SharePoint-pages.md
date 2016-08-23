# リモート プロビジョニングを使用して SharePoint ページをブランド化する

リモート プロビジョニングを使用して SharePoint のテーマを操作します。

_**適用対象:** Office 365 | SharePoint 2013 | SharePoint Online_

テーマを適用および操作するために、SharePoint のリモート プロビジョニング機能を使用できます。これらの機能は、次の API によって提供されます。

-  [ApplyTheme メソッド](http://msdn.microsoft.com/library/52c567e8-03e6-7ba3-a9ed-cf4e3c22dbdd%28Office.15%29.aspx)
    
-  [ThemeInfo クラス](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.themeinfo.aspx)
    
**ApplyTheme** メソッドにより、[外観の変更] ウィザードが強化されます。 ウィザードは、指定されたコンポーネントを使用して、SharePoint サイトに構成済みの外観またはカスタマイズされた外観を適用します。 テーマはサイトごとに適用されます。
**ApplyThemeApp** および **ThemeInfo** のサーバー側 API は、CSOM および JSOM の **ApplyTheme** および **ThemeInfo** API 経由で公開されます。
既存のテーマやカスタマイズされたテーマを適用する方法を示すサンプルについては、GitHub の [Branding.Themes](https://github.com/Lauragra/PnP/tree/master/Samples/Branding.Themes) を参照してください。

## ApplyTheme メソッド
<a name="sectionSection0"> </a>

リモート プロビジョニングを使用してテーマを適用するには、この後の例に示すように、**ApplyTheme** クライアント側メソッドを使用します。

```
public void ApplyTheme(
            string colorPaletteUrl,
            string fontSchemeUrl,
            string backgroundImageUrl,
            bool shareGenerated
             )
```

**ApplyTheme** メソッドでは、次のパラメーターを使用します。

- colorPaletteUrl - カラー パレット ファイルのサーバー相対 URL (たとえば、spcolor)。
    
- fontSchemeUrl - フォント パターン ファイルのサーバー相対 URL (たとえば、spfont)。
    
- backgroundImageUrl - 背景画像のサーバー相対 URL。背景画像がない場合、このパラメーターは **null** 参照を返します。
    
- shareGenerated - ブール値。生成したテーマ ファイルをルート Web に適用する必要がある場合は **True**、現行 Web に格納する場合は **false** です。

**メモ**  shareGenerated パラメーターでは、テーマの出力ファイルを Web に固有の場所に格納するか、またはサイト コレクション全体からアクセスできる場所に格納するかを決定します。サイトの種類に応じた既定値を維持することをお勧めします。

## ThemeInfo クラス
<a name="sectionSection1"> </a>

CSOM コードを使用すると、サイトに適用される構成済みの外観に関する情報を取得できます。[ThemeInfo](https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.client.themeinfo.aspx) クラスでは、この後の例に示すように、テーマに関連付けられたコンテキストを取得します。

```
public ThemeInfo ThemeInfo { get; }
```

**ThemeInfo** クラスを使用すると、サイトに適用されるテーマに関する詳細を取得できます。たとえば、構成済みの外観に対して定義されている説明、コンテキスト、オブジェクト データ、色、指定した名前のフォント (および指定した言語コードのフォント)、ならびに背景画像の URI などを取得できます。

## CSOM コードで ApplyTheme および ThemeInfo を使用する
<a name="sectionSection2"> </a>

次のコード サンプルでは、CSOM コードで **ApplyTheme** および **ThemeInfo** を使用する方法を示します。このコードは、リモート プロビジョニング パターンで使用できます。たとえば、構成済みの外観をデザイナーの指定に従ってプログラムで作成し、Web アプリケーションのサイトにプロビジョニングすることができます。

```C#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.Web.UI;
using System.Web.UI.WebControls;
using System.Text;
using System.IO;
using Microsoft.SharePoint.Client;

namespace ApplyThemeAppWeb.Pages
{
    public partial class Default : System.Web.UI.Page
    {
        public string _ContextToken 
        {
            get
            {
                if (ViewState["ContextToken"] == null)
                    return null;
                return ViewState["ContextToken"].ToString();
            }
            set
            {
                ViewState["ContextToken"] = value;
            }
        }

        public string _HostWeb
        {
            get
            {
                if (ViewState["HostWeb"] == null)
                    return null;
                return ViewState["HostWeb"].ToString();
            }
            set
            {
                ViewState["HostWeb"] = value;
            }
        }

        protected void Page_Load(object sender, EventArgs e)
        {
            if (!IsPostBack)
            {
                _ContextToken = TokenHelper.GetContextTokenFromRequest(Page.Request);
                _HostWeb = Page.Request["SPHostUrl"];
            }

            StatusMessage.Text = string.Empty;
        }

        protected void GetThemeInfo_Click(object sender, EventArgs e)
        {
            try
            {
                StatusMessage.Text += GetThemeInfo();
            }
            catch (Exception ex)
            {
                StatusMessage.Text += Environment.NewLine + ex.ToString();
            }
        }

        protected void ApplyTheme_Click(object sender, EventArgs e)
        {
            try
            {
                ApplyTheme();
                StatusMessage.Text += "Theme applied. Click Get Theme Info to see changes." + Environment.NewLine;
            }
            catch (Exception ex)
            {
                StatusMessage.Text += Environment.NewLine + ex.ToString();
            }
        }

        private string GetThemeInfo()
        {
            using (var clientContext = TokenHelper.GetClientContextWithContextToken(_HostWeb, _ContextToken, Request.Url.Authority))
            {

                Web hostWebObj = clientContext.Web;
                ThemeInfo initialThemeInfo = hostWebObj.ThemeInfo;

                // Get the initial theme info for the web. No need to load the entire web object.
                clientContext.Load(hostWebObj, w => w.ThemeInfo, w => w.CustomMasterUrl);

                // Theme component info is available via a method call that must be run.
                var linkShade = initialThemeInfo.GetThemeShadeByName("Hyperlink");
                var titleFont = initialThemeInfo.GetThemeFontByName("title", 1033);

                // Run.
                clientContext.ExecuteQuery();

                // Use ThemeInfo to show some data about the theme currently applied to the web.
                StringBuilder initialInfo = new StringBuilder();
                initialInfo.AppendFormat("Current master page: {0}\r\n", hostWebObj.CustomMasterUrl);
                initialInfo.AppendFormat("Background Image: {0}\r\n", initialThemeInfo.ThemeBackgroundImageUri);
                initialInfo.AppendFormat("The \"Hyperlink\" Color for this theme is: {0}\r\n", linkShade.Value);
                initialInfo.AppendFormat("The \"title\" Font for this theme is: {0}\r\n", titleFont.Value);
                return initialInfo.ToString();
            }
        }

        protected void ApplyTheme()
        {
            using (var clientContext = TokenHelper.GetClientContextWithContextToken(_HostWeb, _ContextToken, Request.Url.Authority))
            {
                // Apply the new theme.

                // First, copy theme files to a temporary location (the web's Site Assets library).
                Web hostWebObj = clientContext.Web;
                Site hostSiteObj = clientContext.Site;
                Web hostRootWebObj = hostSiteObj.RootWeb;
                
                // Get the necessary lists and libraries.
                List themeLibrary = hostRootWebObj.Lists.GetByTitle("Theme Gallery");
                Folder themeFolder = themeLibrary.RootFolder.Folders.GetByUrl("15");
                List looksGallery = hostRootWebObj.Lists.GetByTitle("Composed Looks");
                List masterLibrary = hostRootWebObj.Lists.GetByTitle("Master Page Gallery");
                List assetLibrary = hostRootWebObj.Lists.GetByTitle("Site Assets");

                clientContext.Load(themeFolder, f => f.ServerRelativeUrl);
                clientContext.Load(masterLibrary, l => l.RootFolder);
                clientContext.Load(assetLibrary, l => l.RootFolder);

                // First, upload the theme files to the Theme Gallery.
                DirectoryInfo themeDir = new DirectoryInfo(Server.MapPath("/Theme"));
                foreach (var themeFile in themeDir.EnumerateFiles())
                {
                    FileCreationInformation newFile = new FileCreationInformation();
                    newFile.Content = System.IO.File.ReadAllBytes(themeFile.FullName);
                    newFile.Url = themeFile.Name;
                    newFile.Overwrite = true;
                    
                    // Sort by file extension into the correct library. 
                    switch (themeFile.Extension)
                    {
                        case ".spcolor":
                        case ".spfont":
                            Microsoft.SharePoint.Client.File uploadTheme = themeFolder.Files.Add(newFile);
                            clientContext.Load(uploadTheme);
                            break;
                        case ".master":
                        case ".html":
                            Microsoft.SharePoint.Client.File updloadMaster = masterLibrary.RootFolder.Files.Add(newFile);
                            clientContext.Load(updloadMaster);
                            break;
                        default:
                            Microsoft.SharePoint.Client.File uploadAsset = assetLibrary.RootFolder.Files.Add(newFile);
                            clientContext.Load(uploadAsset);
                            break;
                    }

                }

                // Run the file upload.
                clientContext.ExecuteQuery();

                // Create a new composed look for the theme.
                string themeFolderUrl = themeFolder.ServerRelativeUrl;
                string masterFolderUrl = masterLibrary.RootFolder.ServerRelativeUrl;

                // Optional: Use to make the custom theme available for selection in the UI. For
          // example, for OneDrive for Business sites, you don't need this code because 
                // the ability to set a theme is hidden. 
          ListItemCreationInformation newLook = new ListItemCreationInformation();
                Microsoft.SharePoint.Client.ListItem newLookItem = looksGallery.AddItem(newLook);
                newLookItem["Title"] = "Theme Sample Look";
                newLookItem["Name"] = "Theme Sample Look";

                FieldUrlValue masterFieldValue = new FieldUrlValue();
                masterFieldValue.Url = masterFolderUrl + "/seattle.master";
                newLookItem["MasterPageUrl"] = masterFieldValue;

                FieldUrlValue colorFieldValue = new FieldUrlValue();
                colorFieldValue.Url = themeFolderUrl + "/ThemeSample.spcolor";
                newLookItem["ThemeUrl"] = colorFieldValue;

                FieldUrlValue fontFieldValue = new FieldUrlValue();
                fontFieldValue.Url = themeFolderUrl + "/ThemeSample.spfont";
                newLookItem["FontSchemeUrl"] = fontFieldValue;

                newLookItem.Update();

                // Apply the master page.
                hostWebObj.CustomMasterUrl = masterFieldValue.Url;

                // Update between the last and next steps. ApplyTheme throws errors if the theme
          // and master page are updated in the same query.
                hostWebObj.Update();
                clientContext.ExecuteQuery();

                // Apply the theme.
                hostWebObj.ApplyTheme(
                    colorFieldValue.Url, // URL of the color palette (.spcolor) file,
                    fontFieldValue.Url, // URL to the font scheme (.spfont) file (optional)
                    null, // Background Image URL (optional, null here),
                    false // false stores the composed look files in this web only. True shares the composed look with the site collection (to which you need permission). 

                // Need to call update to apply the change to the host web.
                hostWebObj.Update();

                // Run the Update method.
                clientContext.ExecuteQuery();
            }
        }
    }
}
```

## その他のリソース
<a name="bk_addresources"> </a>

-  [SharePoint サイト ブランド化とページ カスタマイズのソリューション](SharePoint-site-branding-and-page-customization-solutions.md)
    
-  [構成済みの外観を使用して SharePoint サイトをブランド化する](Use-composed-looks-to-brand-SharePoint-sites.md)
    
-  [Branding.Themes サンプル](https://github.com/Lauragra/PnP/tree/master/Samples/Branding.Themes)
