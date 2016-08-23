# CSOM を使用して SharePoint のコンテンツ タイプを作成する

CSOM を使用して SharePoint コンテンツ タイプを作成し、SharePoint 2013 SP1 で導入された機能を使用してローカライズの変更を行います。

_**適用対象:** Office 365 | SharePoint 2013 | SharePoint Add-ins? | SharePoint Online_

[Core.SPD](https://github.com/OfficeDev/PnP/tree/dev/Samples/Core.SPD) サンプルを使用すると、プログラムによってサイト列のコンテンツ タイプを作成し、それらを相互にリンク付けることができます。また、SharePoint 2013 SP1 CSOM API ([SharePoint Server 2013 クライアント コンポーネント SDK](http://www.microsoft.com/en-us/download/details.aspx?id=35585) で使用できます) を使用すると、特定のコンテンツ タイプ ID を追加したり、コンテンツ タイプ、リスト、サイト タイトルをローカライズしたりすることができます。 

## 始める前に

まず、 [Core.SPD](https://github.com/OfficeDev/PnP/tree/dev/Samples/Core.SPD) サンプル アプリを、GitHub の [Office 365 Developer のパターンおよびプラクティス](https://github.com/OfficeDev/PnP/tree/dev) プロジェクトからダウンロードします。

## コンテンツ タイプとサイト列の作成

次のコード例は、 **ContentTypeCreationInformation** クラスを使用してコンテンツ タイプを作成し、ID を設定する方法について示しています。

**メモ:**  この記事で提供されるコードは、明示または黙示のいかなる種類の保証なしに現状のまま提供されるものであり、特定目的への適合性、商品性、権利侵害の不存在についての暗黙的な保証は一切ありません。

```C#
ContentTypeCollection contentTypes = web.ContentTypes;
cc.Load(contentTypes);
cc.ExecuteQuery();

foreach (var item in contentTypes)
{
  if (item.StringId == "0x0101009189AB5D3D2647B580F011DA2F356FB2")
    return;
}

// Create a Content Type Information object.
ContentTypeCreationInformation newCt = new ContentTypeCreationInformation();
// Set the name for the content type.
newCt.Name = "Contoso Document";
// Inherit from oob document - 0x0101 and assign. 
newCt.Id = "0x0101009189AB5D3D2647B580F011DA2F356FB2";
// Set content type to be available from specific group.
newCt.Group = "Contoso Content Types";
// Create the content type.
ContentType myContentType = contentTypes.Add(newCt);
cc.ExecuteQuery();

Using AddFieldAsXml you can add fields to the FieldCollection of a site collection:
FieldCollection fields = web.Fields;
cc.Load(fields);
cc.ExecuteQuery();

string FieldAsXML = @"<Field ID='{4F34B2ED-9CFF-4900-B091-4C0033F89944}' Name='ContosoString' DisplayName='Contoso String' Type='Text' Hidden='False' Group='Contoso Site Columns' Description='Contoso Text Field' />";
Field fld = fields.AddFieldAsXml(FieldAsXML, true, AddFieldOptions.DefaultValue);
cc.Load(fields);
cc.Load(fld);
cc.ExecuteQuery();

```

**FieldLinkCollection** クラスと **FieldLinkCreationInformation** クラスを使用して、フィールドをコンテンツ タイプにリンクさせます。

```C#
FieldCollection fields = web.Fields;
Field fld = fields.GetByInternalNameOrTitle("ContosoString");
cc.Load(fields);
cc.Load(fld);
cc.ExecuteQuery();

FieldLinkCollection refFields = myContentType.FieldLinks;
cc.Load(refFields);
cc.ExecuteQuery();

foreach (var item in refFields)
{
  if (item.Name == "ContosoString")
    return;
  }

FieldLinkCreationInformation link = new FieldLinkCreationInformation();
link.Field = fld;
myContentType.FieldLinks.Add(link);
myContentType.Update(true);
cc.ExecuteQuery();

```

## コンテンツ タイプ、リスト、サイト タイトルのローカライズ

次のコードを使用して、サイト タイトルとサイト説明をローカライズします。

```C#
web.TitleResource.SetValueForUICulture("fi-FI", "KielikÃ¤Ã¤nnÃ¤ minut");
web.DescriptionResource.SetValueForUICulture("fi-FI", "KielikÃ¤Ã¤nnetty saitti");

```

リストの場合、サイトと同じ方法を使用します。

```C#
list.TitleResource.SetValueForUICulture("fi-FI", "KielikÃ¤Ã¤nnÃ¤ minut");
list.DescriptionResource.SetValueForUICulture("fi-FI", "TÃ¤mÃ¤ esimerkki nÃ¤yttÃ¤Ã¤ miten voit kielikÃ¤Ã¤ntÃ¤Ã¤ listoja.");

```

コンテンツ タイプの場合には、名前と説明をローカライズするオプションがあります。フィールドに関しては、タイトルと説明の値をローカライズできます。

```C#
myContentType.NameResource.SetValueForUICulture("fi-FI", "Contoso Dokumentti");
myContentType.DescriptionResource.SetValueForUICulture("fi-FI", "TÃ¤mÃ¤ on geneerinen Contoso dokumentti.");

fld.TitleResource.SetValueForUICulture("fi-FI", "Contoso Teksti");
fld.DescriptionResource.SetValueForUICulture("fi-FI", "TÃ¤Ã¤ on niiku Contoso metadatalle.");

```

## ドキュメント コンテンツ タイプとサイト列の作成

次の例は、ドキュメント コンテンツ タイプを作成し、ドキュメント テンプレートをコンテンツ タイプに関連付ける方法を示しています。 

この例では、「Contoso Document」という新しいコンテンツ タイプをサイト コレクションに追加します。このコンテンツ タイプには、新しいドキュメントが作成されるときに関連付けられるカスタム テンプレートが含まれています。

```C#
ContentType ct = web.ContentTypes.GetById("0x0101009189AB5D3D2647B580F011DA2F356FB2");
            cc.Load(ct); cc.ExecuteQuery();
            string ctFolderServerRelativeURL = "_cts/" + ct.Name;
            Folder ctFolder = web.GetFolderByServerRelativeUrl(ctFolderServerRelativeURL);
            cc.Load(ctFolder);
            cc.ExecuteQuery();

            string path = @"C:\Data\Test Documents\Doc CT File.docx";
            string fileName = System.IO.Path.GetFileName(path);
            byte[] filecontent = System.IO.File.ReadAllBytes(path);

            using (System.IO.FileStream fs = new System.IO.FileStream(path, System.IO.FileMode.Open))
            {
                FileCreationInformation newFile = new FileCreationInformation();
                newFile.Content = filecontent;
                newFile.Url = ctFolderServerRelativeURL + "/" + fileName;

                Microsoft.SharePoint.Client.File uploadedFile = ctFolder.Files.Add(newFile);
                cc.Load(uploadedFile);
                cc.ExecuteQuery();
                //Microsoft.SharePoint.Client.File.SaveBinaryDirect(clientContext, ctFolderServerRelativeURL + "/" + fileName, fs, true);

                
                cc.Load(ct); cc.ExecuteQuery();
                ct.DocumentTemplate = fileName;
                ct.Update(false);
                cc.Load(ct); cc.ExecuteQuery();
                Console.WriteLine("Content type updates");
            }

```

## その他のリソース
<a name="bk_addresources"> </a>

- [SharePoint サイト プロビジョニング ソリューション](sharepoint-site-provisioning-solutions.md)
    
- [FTC から CAM ? CSO を使用して特定の ID のコンテンツ タイプを作成する](http://blogs.msdn.com/b/vesku/archive/2014/02/28/ftc-to-cam-create-content-types-with-specific-ids-using-csom.aspx)
    
- [SharePoint Server 2013 クライアント コンポーネント SDK](http://www.microsoft.com/en-us/download/details.aspx?id=35585)
