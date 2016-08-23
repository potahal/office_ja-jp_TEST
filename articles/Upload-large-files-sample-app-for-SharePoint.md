# 大きいファイルをアップロードするサンプル アドイン (SharePoint)

SaveBinaryDirect、ContentStream、StartUpload、ContinueUpload、FinishUploadUpload を使用して、2MB を超えるファイルを SharePoint および SharePoint Online にアップロードします。 

_**適用対象:** SharePoint 用アドイン | SharePoint 2013 | SharePoint Online_

[Core.LargeFileUpload](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.LargeFileUpload) サンプルは、プロバイダー向けのホスト型アドインを使用して大きなファイルを SharePoint にアップロードする方法、および 2 MB のアップロード ファイル制限を回避する方法を示しています。このソリューションは、2 MB より大きいファイルを SharePoint にアップロードする場合に使用します。このサンプルは、以下のいずれかの方法を使用することによって、大きなファイルをドキュメント ライブラリにアップロードするコンソール アプリケーションとして実行します。

- [Microsoft.SharePoint.Client.File](https://msdn.microsoft.com/library/office/ee538285.aspx) クラスの ****SaveBinaryDirect**** メソッド。
    
- [FileCreationInformation](https://msdn.microsoft.com/library/office/microsoft.sharepoint.client.filecreationinformation.contentstream.aspx) クラスの ****ContentStream**** プロパティ。
    
- [File](https://msdn.microsoft.com/library/office/microsoft.sharepoint.client.file.startupload.aspx) クラスの [**StartUpload**](https://msdn.microsoft.com/library/office/microsoft.sharepoint.client.file.continueupload.aspx)、[**ContinueUpload**](https://msdn.microsoft.com/library/office/microsoft.sharepoint.client.file.finishupload.aspx)、および ****FinishUpload**** メソッド。
    
次の表は、ファイルのアップロード方法として使用できるもののリストを示しており、それぞれの方法をいつ使用するかも示しています。

**ファイルをアップロードする方法**

|**ファイルをアップロードする方法**|**考慮事項**|**いつ使用するか?**|**サポートされるプラットフォーム**|
|:-----|:-----|:-----|:-----|
| [FileCreationInformation](https://msdn.microsoft.com/library/office/microsoft.sharepoint.client.filecreationinformation.content.aspx) クラスの **Content** プロパティ。|アップロードできるファイルの最大サイズは 2 MB です。30 分後にタイムアウトが発生します。|2 MB 以下のファイルをアップロードする場合にのみ使用します。 |SharePoint Server 2013、SharePoint Online|
| **File** クラスの **SaveBinaryDirect** メソッド。|ファイル サイズの制限はありません。30 分後にタイムアウトが発生します。|この方法を使用するのは、ユーザー用の認証ポリシーを使用している場合に限られます。ユーザー用の認証ポリシーは SharePoint アドインでは使用できませんが、ネイティブ デバイス アプリ、Windows PowerShell、および Windows コンソール アプリケーションでは使用できます。|SharePoint Server 2013、SharePoint Online|
| **FileCreationInformation** クラスの **ContentStream** プロパティ。|ファイル サイズの制限はありません。30 分後にタイムアウトが発生します。|推奨される場合:<br/>- SharePoint Server 2013。<br/>- SharePoint Online (ファイルが 10 MB 未満の場合)。|SharePoint Server 2013、SharePoint Online|
|**File** クラスで **StartUpload**、**ContinueUpload**、**FinishUpload** メソッドを使用して、1 つのファイルを一連のチャンクとしてアップロードします。|ファイル サイズの制限はありません。30 分後にタイムアウトが発生します。タイムアウトを避けるため、ファイルの各チャンクは、その前のチャンクの完了後 30 分以内にアップロードする必要があります。 |ファイルが 10 MB より大きい場合、SharePoint Online に対して推奨。|SharePoint Online|

## はじめに
<a name="sectionSection0"> </a>

まず GitHub の [Office 365 Developer Patterns and Practices](https://github.com/OfficeDev/PnP/tree/dev) プロジェクトから、[Core.LargeFileUpload](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.LargeFileUpload) サンプル アドインをダウンロードします。

## Core.LargeFileUpload サンプル アプリの使用
<a name="sectionSection1"> </a>

このコード サンプルを開始すると、コンソール アプリケーションが表示されます。SharePoint Online サイト コレクション URL および Office 365 のログオン資格情報を入力する必要があります。認証の後、コンソール アプリケーションに例外が表示されます。この例外は、FileUploadService.cs の  **UploadDocumentContent** メソッドが **FileCreationInformation.Content** プロパティを使用することによって、2 MB より大きいファイルをアップロードしようとする際に発生します。さらに、 **Docs** と呼ばれるドキュメンテーション ライブラリがまだ存在していないなら、 **UploadDocumentContent** によってそれが作成されます。 **Docs** ドキュメント ライブラリは、後で、このコード サンプルの中で使用されます。

**メモ**  この記事で提供されるコードは、明示または黙示のいかなる種類の保証なしに現状のまま提供されるものであり、特定目的への適合性、商品性、権利侵害の不存在についての暗黙的な保証は一切ありません。

```C#
public void UploadDocumentContent(ClientContext ctx, string libraryName, string filePath)
        {
            Web web = ctx.Web;

            // Ensure that target library exists. Create if it is missing.
            if (!LibraryExists(ctx, web, libraryName))
            {
                CreateLibrary(ctx, web, libraryName);
            }

            FileCreationInformation newFile = new FileCreationInformation();

         // The next line of code causes an exception to be thrown for files larger than 2 MB.
            newFile.Content = System.IO.File.ReadAllBytes(filePath);
            newFile.Url = System.IO.Path.GetFileName(filePath);

            // Get instances to the given library.
            List docs = web.Lists.GetByTitle(libraryName);
            
 // Add file to the library.
            Microsoft.SharePoint.Client.File uploadFile = docs.RootFolder.Files.Add(newFile);
            ctx.Load(uploadFile);
            ctx.ExecuteQuery();
        } 

```

FileUploadService.cs において、ドキュメント ライブラリに大きなファイルをアップロードするために使用できる 3 つのオプションがこのコード サンプルにより提供されます。

- **File.SaveBinaryDirect** メソッド。
    
- **FileCreationInformation.ContentStream** プロパティ。
    
- **File** クラスの **StartUpload**、**ContinueUpload**、および **FinishUpload** メソッド。
    
FileUploadService.cs において  **SaveBinaryDirect** は、 **Microsoft.SharePoint.Client.File.SaveBinaryDirect** メソッドおよび **FileStream** オブジェクトを使用することによって、ファイルをドキュメント ライブラリにアップロードします。

```C#
public void SaveBinaryDirect(ClientContext ctx, string libraryName, string filePath)
        {
            Web web = ctx.Web;
            // Ensure that the target library exists. Create it if it is missing.
            if (!LibraryExists(ctx, web, libraryName))
            {
                CreateLibrary(ctx, web, libraryName);
            }

            using (FileStream fs = new FileStream(filePath, FileMode.Open))
            {
                Microsoft.SharePoint.Client.File.SaveBinaryDirect(ctx, string.Format("/{0}/{1}", libraryName, System.IO.Path.GetFileName(filePath)), fs, true);
            }

        } 

```

FileUploadService.cs では、 **UploadDocumentContentStream** により、 **FileCreationInformation.ContentStream** プロパティと **FileStream** オブジェクトを使用して、ファイルがドキュメント ライブラリにアップロードされます。

```C#
public void UploadDocumentContentStream(ClientContext ctx, string libraryName, string filePath)
        {

            Web web = ctx.Web;
            // Ensure that the target library exists. Create it if it is missing.
            if (!LibraryExists(ctx, web, libraryName))
            {
                CreateLibrary(ctx, web, libraryName);
            }

            using (FileStream fs = new FileStream(filePath, FileMode.Open))
            {
                FileCreationInformation flciNewFile = new FileCreationInformation();

                // This is the key difference for the first case - using ContentStream property
                flciNewFile.ContentStream = fs;
                flciNewFile.Url = System.IO.Path.GetFileName(filePath);
                flciNewFile.Overwrite = true;

                List docs = web.Lists.GetByTitle(libraryName);
                Microsoft.SharePoint.Client.File uploadFile = docs.RootFolder.Files.Add(flciNewFile);

                ctx.Load(uploadFile);
                ctx.ExecuteQuery();
            }
        }

```

FileUploadService.cs の中で、 **UploadFileSlicePerSlice** は、1 つの大きなファイルを、一連のチャンクまたはフラグメントの集合としてドキュメント ライブラリにアップロードします。 **UploadFileSlicePerSlice** は、以下のタスクを実行します。

1. 新しい GUID を取得します。ファイルを複数のチャンクに分割してアップロードするには、固有の GUID を使用する必要があります。 
    
2. チャンクのブロック サイズをバイト数として計算します。ブロック サイズのバイト数を計算するため、 **UploadFileSlicePerSlice** では **fileChunkSizeInMB** を使用し、個々のチャンクのサイズを MB で指定しています。 
    
3. アップロードするファイルのサイズ (**fileSize**) が、チャンクのサイズ (**blockSize**) 以下かどうかを調べます。
    
    1. **fileSize** がチャンク サイズ以下の場合でも、このサンプルでは、 **FileCreationInformation.ContentStream** プロパティを使用することによりファイルがアップロードされるようになっています。推奨されるチャンク サイズは、10 MB 以上です。
      
    2. **fileSize** がチャンク サイズより大きい場合、
    
        1. ファイルの 1 個のチャンクが  **buffer** に読み込まれます。
    
        2. チャンク サイズがファイル サイズに等しい場合は、ファイル全体が読み込まれます。チャンクが **lastBuffer** にコピーされます。次に、**lastBuffer** により、**File.FinishUpload** を使用してチャンクがアップロードされます。
    
    3. チャンク サイズがファイル サイズと等しくない場合、ファイルから読むチャンクは複数個あります。**File.StartUpload** を呼び出すことにより、最初のチャンクがアップロードされます。**fileoffset** が、最初のチャンクからアップロードされたバイト数に設定され、それが次のチャンクの開始点として使用されます。次のチャンクが読み込まれる際、最後のチャンクにまだ達していないなら、**File.ContinueUpload** が呼び出されて、ファイルの次のチャンクがアップロードされます。最後のチャンクが読み込まれるまでこのプロセスが繰り返されます。最後のチャンクが読み込まれると、**File.FinishUpload** により最後のチャンクがアップロードされ、ファイルがコミットされます。このメソッドが終了すると、ファイルの内容が変更されます。

**メモ** 以下のベスト プラクティスについて考慮してください。
- アップロードが中断した場合に備えて、再試行メカニズムを使用してください。ファイルのアップロードが中断された場合、そのファイルは未完ファイルと呼ばれます。未完ファイルのアップロードは、アップロード中断の後、すぐにでも再開できます。未完ファイルの中断後、6 時間から 24 時間経過すると、未完ファイルはサーバーから削除されます。この削除期間は予告なく変更されることがあります。
- ファイルを複数のチャンクに分割して SharePoint Online にアップロードする場合、SharePoint Online においてそのファイルにロックがかけられます。中断が発生した場合、そのファイルは、15 分間ロックされたままになっています。その 15 分以内に、ファイルの次のチャンクが SharePoint Online にアップロードされないなら、ロックは除去されます。ロックが除去された後、アップロードを再開できます。あるいは、別のユーザーがそのファイルのアップロードを開始する可能性もあります。別のユーザーがそのファイルのアップロードを開始した場合、未完ファイルは SharePoint Online から削除されます。アップロード中断後にファイルがロックされたままになる時間は、予告なしに変更されることがあります。
- チャンク サイズは変更可能ですが、推奨されるチャンク サイズは 10 MB です。
- 正常にアップロードされたチャンクを追跡して、中断されたチャンクを再開します。

チャンクは、順番にアップロードする必要があります。複数のスライスを同時に並行してアップロードすることはできません (マルチスレッドのアプローチを使用するなど)。

```
 public Microsoft.SharePoint.Client.File UploadFileSlicePerSlice(ClientContext ctx, string libraryName, string fileName, int fileChunkSizeInMB = 3)
        {
            // Each sliced upload requires a unique ID.
            Guid uploadId = Guid.NewGuid();

            // Get the name of the file.
            string uniqueFileName = Path.GetFileName(fileName);

            // Ensure that target library exists, and create it if it is missing.
            if (!LibraryExists(ctx, ctx.Web, libraryName))
            {
                CreateLibrary(ctx, ctx.Web, libraryName);
            }
            // Get the folder to upload into. 
            List docs = ctx.Web.Lists.GetByTitle(libraryName);
            ctx.Load(docs, l => l.RootFolder);
            // Get the information about the folder that will hold the file.
            ctx.Load(docs.RootFolder, f => f.ServerRelativeUrl);
            ctx.ExecuteQuery();

            // File object.
            Microsoft.SharePoint.Client.File uploadFile;

            // Calculate block size in bytes.
            int blockSize = fileChunkSizeInMB * 1024 * 1024;

            // Get the information about the folder that will hold the file.
            ctx.Load(docs.RootFolder, f => f.ServerRelativeUrl);
            ctx.ExecuteQuery();


            // Get the size of the file.
            long fileSize = new FileInfo(fileName).Length;

            if (fileSize <= blockSize)
            {
                // Use regular approach.
                using (FileStream fs = new FileStream(fileName, FileMode.Open))
                {
                    FileCreationInformation fileInfo = new FileCreationInformation();
                    fileInfo.ContentStream = fs;
                    fileInfo.Url = uniqueFileName;
                    fileInfo.Overwrite = true;
                    uploadFile = docs.RootFolder.Files.Add(fileInfo);
                    ctx.Load(uploadFile);
                    ctx.ExecuteQuery();
                    // Return the file object for the uploaded file.
                    return uploadFile;
                }
            }
            else
            {
                // Use large file upload approach.
                ClientResult<long> bytesUploaded = null;

                FileStream fs = null;
                try
                {
                    fs = System.IO.File.Open(fileName, FileMode.Open, FileAccess.Read, FileShare.ReadWrite);
                    using (BinaryReader br = new BinaryReader(fs))
                    {
                        byte[] buffer = new byte[blockSize];
                        Byte[] lastBuffer = null;
                        long fileoffset = 0;
                        long totalBytesRead = 0;
                        int bytesRead;
                        bool first = true;
                        bool last = false;

                        // Read data from file system in blocks. 
                        while ((bytesRead = br.Read(buffer, 0, buffer.Length)) > 0)
                        {
                            totalBytesRead = totalBytesRead + bytesRead;

                            // You've reached the end of the file.
                            if (totalBytesRead == fileSize)
                            {
                                last = true;
                                // Copy to a new buffer that has the correct size.
                                lastBuffer = new byte[bytesRead];
                                Array.Copy(buffer, 0, lastBuffer, 0, bytesRead);
                            }

                            if (first)
                            {
                                using (MemoryStream contentStream = new MemoryStream())
                                {
                                    // Add an empty file.
                                    FileCreationInformation fileInfo = new FileCreationInformation();
                                    fileInfo.ContentStream = contentStream;
                                    fileInfo.Url = uniqueFileName;
                                    fileInfo.Overwrite = true;
                                    uploadFile = docs.RootFolder.Files.Add(fileInfo);

                                    // Start upload by uploading the first slice. 
                                    using (MemoryStream s = new MemoryStream(buffer))
                                    {
                                        // Call the start upload method on the first slice.
                                        bytesUploaded = uploadFile.StartUpload(uploadId, s);
                                        ctx.ExecuteQuery();
                                        // fileoffset is the pointer where the next slice will be added.
                                        fileoffset = bytesUploaded.Value;
                                    }

                                    // You can only start the upload once.
                                    first = false;
                                }
                            }
                            else
                            {
                                // Get a reference to your file.
                                uploadFile = ctx.Web.GetFileByServerRelativeUrl(docs.RootFolder.ServerRelativeUrl + System.IO.Path.AltDirectorySeparatorChar + uniqueFileName);

                                if (last)
                                {
                                    // Is this the last slice of data?
                                    using (MemoryStream s = new MemoryStream(lastBuffer))
                                    {
                                        // End sliced upload by calling FinishUpload.
                                        uploadFile = uploadFile.FinishUpload(uploadId, fileoffset, s);
                                        ctx.ExecuteQuery();

                                        // Return the file object for the uploaded file.
                                        return uploadFile;
                                    }
                                }
                                else
                                {
                                    using (MemoryStream s = new MemoryStream(buffer))
                                    {
                                        // Continue sliced upload.
                                        bytesUploaded = uploadFile.ContinueUpload(uploadId, fileoffset, s);
                                        ctx.ExecuteQuery();
                                        // Update fileoffset for the next slice.
                                        fileoffset = bytesUploaded.Value;
                                    }
                                }
                            }

                        } // while ((bytesRead = br.Read(buffer, 0, buffer.Length)) > 0)
                    }
                }
                finally
                {
                    if (fs != null)
                    {
                        fs.Dispose();
                    }
                }
            }

            return null;
        }
```

コード サンプルが完了した後は、Office 365 サイトで **[最近使用したファイル]**  >  **[Docs]** を選択することによって、 **Docs** ドキュメント ライブラリに移動することができます。 **Docs** ドキュメント ライブラリの中に、3 個の大きなファイルが含まれていることを確認してください。

## その他のリソース
<a name="bk_addresources"> </a>

-  [SharePoint 2013 と SharePoint Online 用のエンタープライズ コンテンツ管理ソリューション](Enterprise-Content-Management-solutions-for-SharePoint-2013-and-SharePoint-Online.md)
    
-  [Core.BulkDocumentUploader サンプル](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.BulkDocumentUploader)
