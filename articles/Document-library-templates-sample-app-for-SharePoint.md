# ドキュメント ライブラリ テンプレートのサンプル SharePoint アドイン

Enterprise Content Management (ECM) ストラテジの一部として、カスタム ドキュメント ライブラリ テンプレートを実装し、サイト列、サイト コンテンツ タイプ、分類フィールド、バージョン設定、および既定のドキュメント コンテンツ タイプをカスタマイズします。
    
_**適用対象:** Office 365? | SharePoint 2013? | SharePoint Online_

[ECM.DocumentLibraries](https://github.com/OfficeDev/PnP/tree/master/Samples/ECM.DocumentLibraries) サンプルには、プロバイダー向けのホスト型 アドインを使用することによって、リストまたはドキュメント ライブラリを作成したり、それにコンテンツ タイプを割り当てたり、既定のコンテンツ タイプを削除したりする方法が示されています。このソリューションは、次の場合に使用します。    

- リストまたはドキュメント ライブラリを作成し、既定のコンテンツ タイプを適用する場合。
    
- カスタム フィールドのローカライズ バージョンの追加、保守、または実装についてより大きな制御が必要な場合。
    
- リストまたはライブラリについての既定のコンテンツ タイプを削除する場合。
    
- リストまたはライブラリを作成する際にライブラリの設定値を適用する場合。

## はじめに
<a name="sectionSection0"> </a>

まず、[ECM.DocumentLibraries](https://github.com/OfficeDev/PnP/tree/master/Samples/ECM.DocumentLibraries) サンプル アドインを、GitHub の [Office 365 Developer Patterns and Practices](https://github.com/OfficeDev/PnP/tree/dev) プロジェクトからダウンロードします。

ECM.DocumentLibraries アドインにアクセスするユーザーには、リストを管理するための許可が必要です。ユーザーにリスト管理の許可があるかどうかをチェックするには、Default.aspx.cs の  **DoesUserHavePermission** メソッドを使用します。リスト管理の許可がユーザーに与えられていない場合、アドインからそのユーザーにエラー メッセージが出されます。

```C#
private bool DoesUserHavePermission()
        {
            var spContext = SharePointContextProvider.Current.GetSharePointContext(Context);
            using (var ctx = spContext.CreateUserClientContextForSPHost())
            {
                BasePermissions perms = new BasePermissions();
                perms.Set(PermissionKind.ManageLists);
                ClientResult<bool> _permResult = ctx.Web.DoesUserHavePermissions(perms);
                ctx.ExecuteQuery();
                return _permResult.Value;
            }
        }

```

## ECM.DocumentLibraries サンプル アドインの使用 
<a name="sectionSection1"> </a>

このアドインを起動すると、図 1 に示すような開始ページが表示されます。 ECM.DocumentLibraries の開始ページは、**[サイト コンテンツ]** > **[アプリの追加]** > **[ドキュメント ライブラリ]** > **[詳細オプション]** を選択したときに表示される、新しいドキュメント ライブラリを追加するためのページに似ていますが、1 つの違いがあります。 このアドインを起動すると、[ドキュメント テンプレート] ドロップダウン リストに、カスタム ドキュメント ライブラリ テンプレート、IT ドキュメントおよび Contoso ドキュメントが表示されます。 ユーザーが **[作成]** を選択すると、新しいドキュメント ライブラリに選択済みのカスタム コンテンツ タイプが割り当てられます。 

**図 1. ECM.DocumentLibraries アドインの起動ページ **

![選択肢として IT ドキュメントが一覧に示される [ドキュメント テンプレート] ドロップダウン ボックスがある、ECM.DocumentLibraries アドイン開始ページを示すスクリーン ショット。](media/d58b9d12-808e-4f2b-9065-31e6d735dbaa.png)

ユーザーが **[作成]** を選ぶと、Default.aspx.cs の **CreateLibrary_Click** メソッドにより選択された既定テンプレートがチェックされ、ContentTypeManager.cs の **CreateITDocumentLibrary** または **CreateContosoDocumentLibrary** が呼び出されます。次のコードを参照してください。
    
**メモ**  この記事で提供されるコードは、明示または黙示のいかなる種類の保証なしに現状のまま提供されるものであり、特定目的への適合性、商品性、権利侵害の不存在についての暗黙的な保証は一切ありません。

```C#
protected void CreateLibrary_Click(object sender, EventArgs e)
        {
            try
            {
                var _spContext = SharePointContextProvider.Current.GetSharePointContext(Context);
                var _templateSelectedItem = this.DocumentTemplateType.Value;
                var _libraryToCreate = this.GetLibraryToCreate();
                using (var _ctx = _spContext.CreateUserClientContextForSPHost())
                {
                    
                    _ctx.ApplicationName = "AMS ECM.DocumentLibraries";
                    ContentTypeManager _manager = new ContentTypeManager();
                    switch(_templateSelectedItem)
                    {
                        case "IT Document":
                            _manager.CreateITDocumentLibrary(_ctx, _libraryToCreate);
                            break;
                        case "Contoso Document":
                            _manager.CreateContosoDocumentLibrary(_ctx, _libraryToCreate);
                            break;
                    }
                 }

                Response.Redirect(this.Url.Value);
            }
            catch (Exception _ex)
            {
                throw;
            }
        }

```

次に、 **CreateContosoDocumentLibrary** メソッドによって、次のコード サンプルで示されているように、以下のタスクが実行されます。

1. マネージ メタデータ サービスでカスタム フィールドを作成します。
    
2. コンテンツ タイプを作成します。 
    
3. カスタム フィールドをコンテンツ タイプに関連付けます。
    
4. このコンテンツ タイプのドキュメント ライブラリを作成します。

```C#
        public void CreateContosoDocumentLibrary(ClientContext ctx, Library library)
        {
            // Check the fields.
            if (!ctx.Web.FieldExistsById(FLD_CLASSIFICATION_ID)){
                ctx.Web.CreateTaxonomyField(FLD_CLASSIFICATION_ID, 
                                            FLD_CLASSIFICATION_INTERNAL_NAME, 
                                            FLD_CLASSIFICATION_DISPLAY_NAME, 
                                            FIELDS_GROUP_NAME, 
                                            TAXONOMY_GROUP, 
                                            TAXONOMY_TERMSET_CLASSIFICATION_NAME);
            }
            
            // Check the content type.
            if (!ctx.Web.ContentTypeExistsById(CONTOSODOCUMENT_CT_ID)){
                ctx.Web.CreateContentType(CONTOSODOCUMENT_CT_NAME, 
                                          CT_DESC, CONTOSODOCUMENT_CT_ID, 
                                          CT_GROUP);
            }

            // Associate fields to content types.
            if (!ctx.Web.FieldExistsByNameInContentType(CONTOSODOCUMENT_CT_NAME, FLD_CLASSIFICATION_INTERNAL_NAME)){
                ctx.Web.AddFieldToContentTypeById(CONTOSODOCUMENT_CT_ID, 
                                                  FLD_CLASSIFICATION_ID.ToString(), 
                                                  false);
            }
            CreateLibrary(ctx, library, CONTOSODOCUMENT_CT_ID);
          
        }

```


              **CreateContosoDocumentLibrary** は、OfficeDevPnP.Core の一部である **CreateTaxonomyField** メソッドを呼び出します。**CreateTaxonomyField** により、プロバイダー向けのホスト型アドインから マネージ メタデータ サービス の中にフィールドが作成されます。

```C#
public static Field CreateTaxonomyField(this Web web, Guid id, string internalName, string displayName, string group, TermSet termSet, bool multiValue = false)
        {
            internalName.ValidateNotNullOrEmpty("internalName");
            displayName.ValidateNotNullOrEmpty("displayName");
            termSet.ValidateNotNullOrEmpty("termSet");

            try
            {
                var _field = web.CreateField(id, internalName, multiValue ? "TaxonomyFieldTypeMulti" : "TaxonomyFieldType", true, displayName, group, "ShowField=\"Term1033\"");

                WireUpTaxonomyField(web, _field, termSet, multiValue);
                _field.Update();

                web.Context.ExecuteQuery();

                return _field;
            }
            catch (Exception)
            {
                /// If there is an exception, the hidden field might be present.
                FieldCollection _fields = web.Fields;
                web.Context.Load(_fields, fc => fc.Include(f => f.Id, f => f.InternalName));
                web.Context.ExecuteQuery();
                var _hiddenField = id.ToString().Replace("-", "");

                var _field = _fields.FirstOrDefault(f => f.InternalName == _hiddenField);
                if (_field != null)
                {
                    _field.DeleteObject();
                    web.Context.ExecuteQuery();
                }
                throw;

            }
        }
```


                **CreateContosoDocumentLibrary** は、OfficeDevPnP.Core に含まれる **CreateContentType** メソッドを呼び出します。 
                **CreateContentType** は、新しいコンテンツ タイプを作成します。

```C#
public static ContentType CreateContentType(this Web web, string name, string description, string id, string group, ContentType parentContentType = null)
        {
            LoggingUtility.Internal.TraceInformation((int)EventId.CreateContentType, CoreResources.FieldAndContentTypeExtensions_CreateContentType01, name, id);

            // Load the current collection of content types.
            ContentTypeCollection contentTypes = web.ContentTypes;
            web.Context.Load(contentTypes);
            web.Context.ExecuteQuery();
            ContentTypeCreationInformation newCt = new ContentTypeCreationInformation();

            // Set the properties for the content type.
            newCt.Name = name;
            newCt.Id = id;
            newCt.Description = description;
            newCt.Group = group;
            newCt.ParentContentType = parentContentType;
            ContentType myContentType = contentTypes.Add(newCt);
            web.Context.ExecuteQuery();

            // Return the content type object.
            return myContentType;
        }

```

**CreateContosoDocumentLibrary** は、OfficeDevPnP.Core の一部である **AddFieldToContentTypeById** メソッドを呼び出します。 **AddFieldToContentTypeById** は、フィールドをコンテンツ タイプに関連付けます。

```C#
public static void AddFieldToContentTypeById(this Web web, string contentTypeID, string fieldID, bool required = false, bool hidden = false)
        {
            // Get content type.
            ContentType ct = web.GetContentTypeById(contentTypeID);
            web.Context.Load(ct);
            web.Context.Load(ct.FieldLinks);
            web.Context.ExecuteQuery();

            // Get field.
            Field fld = web.Fields.GetById(new Guid(fieldID));

            // Add field association to content type.
            AddFieldToContentType(web, ct, fld, required, hidden);
        }
```

**CreateContosoDocumentLibrary** により、ContentTypeManager.cs の **CreateLibrary** メソッドが呼び出され、ドキュメント ライブラリが作成されます。 **CreateLibrary** メソッドにより、ドキュメント ライブラリの説明、ドキュメントのバージョン、関連するコンテンツ タイプなどのライブラリ設定値が割り当てられます。

```C#
private void CreateLibrary(ClientContext ctx, Library library, string associateContentTypeID)
        {
            if (!ctx.Web.ListExists(library.Title))
            {
                ctx.Web.AddList(ListTemplateType.DocumentLibrary, library.Title, false);
                List _list = ctx.Web.GetListByTitle(library.Title);
                if(!string.IsNullOrEmpty(library.Description)) {
                    _list.Description = library.Description;
                }

                if(library.VerisioningEnabled) {
                    _list.EnableVersioning = true;
                }

                _list.ContentTypesEnabled = true;
                _list.Update();
                ctx.Web.AddContentTypeToListById(library.Title, associateContentTypeID, true);
                // Remove the default Document Content Type.
                _list.RemoveContentTypeByName(ContentTypeManager.DEFAULT_DOCUMENT_CT_NAME);
                ctx.Web.Context.ExecuteQuery();
            }
            else
            {
                throw new Exception("A list, survey, discussion board, or document library with the specified title already exists in this Web site.  Please choose another title.");
            }
        }
```

**CreateLibrary** により、OfficeDevPnP.Core の一部である ListExtension の **RemoveContentTypeByName** が呼び出されます。 **RemoveContentTypeByName** により、ドキュメント ライブラリ上の既定のコンテンツ タイプが削除されます。

```C#
        public static void RemoveContentTypeByName(this List list, string contentTypeName)
        {
            if (string.IsNullOrEmpty(contentTypeName))
            {
                throw (contentTypeName == null)
                  ? new ArgumentNullException("contentTypeName")
                  : new ArgumentException(CoreResources.Exception_Message_EmptyString_Arg, "contentTypeName");
            }

            ContentTypeCollection _cts = list.ContentTypes;
            list.Context.Load(_cts);

            IEnumerable<ContentType> _results = list.Context.LoadQuery<ContentType>(_cts.Where(item => item.Name == contentTypeName));
            list.Context.ExecuteQuery();

            ContentType _ct = _results.FirstOrDefault();
            if (_ct != null)
            {
                _ct.DeleteObject();
                list.Update();
                list.Context.ExecuteQuery();
            }
        }
```

ドキュメント ライブラリを作成した後、ドキュメント ライブラリの  **[ライブラリの設定]** に移動して、ドキュメント ライブラリに割り当てられている名前、説明、ドキュメントのバージョン設定、コンテンツ タイプ、およびアドインのカスタム フィールドを確認します。

**図 2. アドインによって適用されるライブラリ設定値 **

![名前、Web アドレス、および説明のフィールドが強調表示されている [ドキュメント ライブラリ設定] ページのスクリーン ショット。](media/aedf5107-bacb-4872-8ad4-8e66b1afead8.png)

## その他のリソース
<a name="bk_addresources"> </a>

-  [SharePoint 2013 と SharePoint Online 用のエンタープライズ コンテンツ管理ソリューション](Enterprise-Content-Management-solutions-for-SharePoint-2013-and-SharePoint-Online.md)
    
-  [ECM.Autotagging サンプル アプリ](https://github.com/OfficeDev/PnP/tree/master/Samples/ECM.Autotagging)
