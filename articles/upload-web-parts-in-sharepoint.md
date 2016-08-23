# SharePoint の Web パーツをアップロードする

ユーザー用に事前に構成されている標準の SharePoint Web パーツを展開します。

_**適用対象:** SharePoint 2013 | SharePoint Add-ins? | SharePoint Online_

事前に構成された標準的な SharePoint Web パーツをアップロードして、ユーザーが自分の SharePoint サイトに追加できるようにすることができます。たとえば、事前に構成された以下のパーツをアップロードできます。

- リモート Web 上で JavaScript ファイルを使用する Script Editor Web パーツ。
    
- コンテンツ検索 Web パーツ。
    
この記事では、リモート Web 上の JavaScript ファイルを使用するように Script Editor Web パーツを事前に構成して、UI のカスタマイズを実行する方法を説明します。このソリューションを使用して、以下を行うことができます。

- ホスト Web 上の **[サイトのリソース ファイル]** リストにあるスクリプトを参照する代わりに、Web パーツのリモート Web にあるスクリプト ファイルを使用します。
    
- カスタム サイト プロビジョニング プロセスで構成済みの Web パーツを展開します。たとえば、カスタム サイト プロビジョニング プロセスの一環として、新しいサイトが作成される際にユーザーにサイト使用状況のポリシーの情報を表示できます。 
    
- ユーザーのために、フィルターされたコンテンツを Web パーツに自動的に読み込みます。たとえば、外部システムから読み取ったローカル ニュースの情報をスクリプト ファイルで表示できます。
    
- ユーザーが **[Web パーツ ギャラリー]** にある Web パーツを使用してサイトに他の機能を追加できるようにします。

## 始める前に

まず、[Core.AppScriptPart](https://github.com/OfficeDev/PnP/tree/dev/Samples/Core.AppScriptPart) サンプル アドインを、GitHub の [Office 365 Developer Patterns and Practices](https://github.com/OfficeDev/PnP/tree/dev) プロジェクトからダウンロードします。

## Core.AppScriptPart アドインを使用する

コード サンプルを実行して、**[シナリオの実行]** を選択するとき:

1. **[サイトに戻る]** を選択します。
    
2. **[ページ]**  >  **[編集]**  >  **[挿入]**  >  **[Web パーツ]** を選択します。
    
3. **[カテゴリ]** で、**[アドイン スクリプト パート]** を選択してから **[ユーザー プロファイル情報]** を選択します。
    
4. **[追加]** を選択します。
    
5. **[ユーザー プロファイル情報]** Web パーツの右上隅にあるドロップダウンで、**[Web パーツの編集]** を選択します。
    
6. **[スニペットを編集]** を選択します。
    
7. **
            &lt;SCRIPT&gt;** 要素を確認します。
    
    **src** 属性は、リモート Web の JavaScript ファイルにリンクすることに注意してください。 次の例のように、**&lt;SCRIPT&gt;** 要素は Core.AppScriptPartWeb\userprofileinformation.webpart の **Content** プロパティで設定されています。 **src** 属性にリンクする JavaScript ファイルは、Core.AppScriptPartWeb\Scripts\userprofileinformation.js です。 Userprofileinformation.js は、ユーザー プロファイル サービスから現在のユーザーのプロファイル情報を読み取り、Web パーツにこの情報を表示します。
    
     **メモ:** この記事で提供されるコードは、明示または黙示のいかなる種類の保証なしに現状のまま提供されるものであり、特定目的への適合性、商品性、権利侵害の不存在についての暗黙的な保証は一切ありません。

  ```XML
  <property name="Content" type="string">&amp;lt;script type="text/javascript" src="https://localhost:44361/scripts/userprofileinformation.js"&amp;gt;&amp;lt;/script&amp;gt;
&amp;lt;div id="UserProfileAboutMe"&amp;gt;&amp;lt;div&amp;gt;
  </property>
  ```

8. **[キャンセル]** を選択します。
    
9. **[保存]** を選択します。

**メモ:** ユーザー プロファイル画像が表示されない場合は、OneDrive for Business サイトを開いて、ホスト Web に戻ります。

Core.AppScriptPartWeb\Pages\Default.aspx で、**[シナリオの実行]** は **btnScenario_Click** を実行し、その結果として以下が行われます。

1. **[Web パーツ ギャラリー]** フォルダーへの参照を取得します。
    
2. [FileCreationInformation](https://msdn.microsoft.com/library/office/microsoft.sharepoint.client.filecreationinformation.aspx) を使用して、プロバイダー ホスト型アドインから **[Web パーツ ギャラリー]** にアップロードする userprofileinformation.webpart ファイルを作成します。**folder.Files.Add** メソッドは、そのファイルを **[Web パーツ ギャラリー]** に追加します。
    
3. **[Web パーツ ギャラリー]** 内のすべてのリスト項目を取得して、userprofileinformation.webpart を検索します。
    
4. userprofileinformation.webpart が見つかると、Web パーツを **[アドイン スクリプト パート]** という名前のカスタム グループに割り当てます。

```C#
protected void btnScenario_Click(object sender, EventArgs e)
        {
            var spContext = SharePointContextProvider.Current.GetSharePointContext(Context);
            using (var clientContext = spContext.CreateUserClientContextForSPHost())
            {
                var folder = clientContext.Web.Lists.GetByTitle("Web Part Gallery").RootFolder;
                clientContext.Load(folder);
                clientContext.ExecuteQuery();

                // Upload the "userprofileinformation.webpart" file.
                using (var stream = System.IO.File.OpenRead(
                                Server.MapPath("~/userprofileinformation.webpart")))
                {
                    FileCreationInformation fileInfo = new FileCreationInformation();
                    fileInfo.ContentStream = stream;
                    fileInfo.Overwrite = true;
                    fileInfo.Url = "userprofileinformation.webpart";
                    File file = folder.Files.Add(fileInfo);
                    clientContext.ExecuteQuery();
                }

                // Update the group that the Web Part belongs to. Start by getting all list items in the Web Part Gallery, and then find the Web Part that was just uploaded.
                var list = clientContext.Web.Lists.GetByTitle("Web Part Gallery");
                CamlQuery camlQuery = CamlQuery.CreateAllItemsQuery(100);
                Microsoft.SharePoint.Client.ListItemCollection items = list.GetItems(camlQuery);
                clientContext.Load(items);
                clientContext.ExecuteQuery();
                foreach (var item in items)
                {
                    // Create new group.
                    if (item["FileLeafRef"].ToString().ToLowerInvariant() == "userprofileinformation.webpart")
                    {
                        item["Group"] = "add-in Script Part";
                        item.Update();
                        clientContext.ExecuteQuery();
                    }
                }

                lblStatus.Text = string.Format("add-in script part has been added to Web Part Gallery. You can find 'User Profile Information' script part under 'App Script Part' group in the <a href='{0}'>host web</a>.", spContext.SPHostUrl.ToString());
            }
        }
```

## その他のリソース
<a name="bk_addresources"> </a>

- [Office 365 開発パターンとプラクティス (ソリューション ガイダンス)](Office-365-development-patterns-and-practices-solution-guidance.md)
    
- [JavaScript を使用して SharePoint サイト UI をカスタマイズする](Customize-your-SharePoint-site-UI-by-using-JavaScript.md)
