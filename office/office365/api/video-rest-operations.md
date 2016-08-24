---
ms.Toctitle: Video REST API reference 
title: "ビデオ REST API リファレンス" 
description: "SharePoint Online の Office 365 ビデオ サービスでのビデオとチャネルに関する探索、操作、情報取得のためのビデオ REST API の使用方法のリファレンス。"
ms.ContentId: 4202e210-44be-4289-808d-3b7358181cc0
ms.date: November 17, 2015
---

# ビデオ REST API リファレンス


 _**適用対象:** SharePoint Online | Office 365_

REST ビデオ API を使用すると、SharePoint Online で Office 365 ビデオ サービスのビデオを検出して対話できます。ビデオおよびチャネルに関する情報を取得したり、新しいビデオをアップロードしたり、ビデオをストリーミングするための情報を取得できます。

<a name="Overview"> </a>
## ビデオ REST API の使用

ビデオ REST API で使用されるオブジェクトには、ビデオとチャネルの 2 種類があります。

ビデオ REST API を操作するには、サポートされているメソッド ( GET、POST、MERGE または DELETE) を使用する HTTP 要求を送信します。

「ビデオ ポータルの操作」セクションで説明されているように、すべてのビデオ API 要求は、探索サービスから取得したルート URL を使用します。

パス URL リソース名とクエリ パラメーターは、大文字と小文字が区別されません。ただし、割り当てた値、エンティティ ID、その他の base64 でエンコードされた値では大文字と小文字を区別します。

Office 365 API は Microsoft Azure Active Directory (Azure AD) と OAuth を使用して、[アプリケーション要求を認証します](..\howto\common-app-authentication-tasks.md)。
アプリケーションからビデオ API にアクセスする場合は、[適切な範囲のアクセス許可](..\howto\common-app-authentication-tasks.md#sectionSection0)で Azure AD に登録する必要があります。
Office 365 ビデオ REST API は、OData 4.0 標準をサポートしており、アプリは RESTful インターフェイスを使用して Office 365 のビデオ データと対話することができます。

アクセス許可の対象は、3 つのカスタム グループに分かれます。

 - 管理者は、チャネルの設定を変更し、ビデオを編集できます。
 - コントリビューターは、ビデオの作成、読み取り、更新、削除 (CRUD) を行うことができます。
 - 閲覧者は、ビデオを表示することだけができます。

各チャネルのチャネル所有者は、組織内のユーザーがどのグループに属するかを決定します。さらに、SharePoint テナント管理者も同じことを決定できます。


**注** 詳しくは、「[Office 365 プラットフォームでの開発](..\howto\platform-development-overview.md)」を参照してください。

<a name="VideoPortalOperations"> </a>
## ビデオ ポータルの操作

ビデオ ポータルのルート URL を取得して、他のすべてのビデオ REST API 操作で使用できます。ビデオ ポータルがセットアップされていて有効かどうかを判別できます。

****

<a name="GetPortalInformation"> </a>
**ビデオ ポータルに関する情報の取得**

O365 [探索サービス](..\api\discovery-service-rest-operations.md)を使用してルート SharePoint サイト コレクション URL (RootSite) を取得した後、その URL から VideoService.Discover を呼び出して、SharePoint Online のビデオ ポータルの URL を取得します。それを以降のすべての呼び出しで使用します。ビデオ ポータルがセットアップされていて有効かどうかを判別します。

**注** 探索サービスにアクセスするには、会社の SharePoint ポータルにログインする必要があります。

通常、SharePoint のルート サイト コレクションのエンドポイント URL は次のようになります (架空の企業 Contoso の場合)。

https://contoso.sharepoint.com/

探索サービスによって返される同じ会社のビデオ ポータルのエンドポイント URL は、次のようになります。

https://contoso.sharepoint.com/portals/hub

```no-highlight
GET {RootSite}/_api/VideoService.Discover
```

**注***RootSite* パラメーターは、SharePoint のルート サイト コレクションのエンドポイント URL を表す文字列であり、探索サービスから取得します。|

 **応答の種類**

- **IsVideoPortalEnabled** - ポータルが有効でセットアップされている場合は True を返し、ポータルが有効ではないか、またはセットアップされていない場合は False を返します。
- **VideoPortalURL** - 以降のすべての呼び出しで使用されるビデオ ポータルのエンドポイント URL です。

**注** バージョン管理情報が使用できるようになったときに簡単に追加できるよう、この URL に対して定数を定義して使用することをお勧めします。

[!code-REST-i[Get_Portal_Info](./_data/video_api_get_portal_info.json)]


<a name="ChannelOperations"> </a>
## チャネルの操作

ビデオはチャネルに保存されます。ユーザーがビデオをアップロードできるをすべてのチャネルのリストと、ユーザーが表示できるすべてのチャネルのリストを取得できます。チャネルの ID、チャネルの色、チャネルのタイトル、チャネルに含まれるすべてのビデオのリストなど、特定のチャネルについての情報を取得することもできます。

****

<a name="GetChannelsInfo"> </a>
### ユーザーが表示またはアップロードできるチャネルに関する情報の取得

<a name="GetUploadChannels"> </a>
**ユーザーがビデオをアップロードできるチャネルのリストの取得**

ユーザーがビデオをアップロードできるチャネルのリストを取得します。これらは、ユーザーが所有者またはエディターのアクセス許可を持っているチャネルです。


```no-highlight
GET {VideoPortalURL}/_api/VideoService/CanEditChannels
```

**注** この呼び出しおよび以降のすべての呼び出しでは、*VideoPortalURL* は、VideoService.Discover の呼び出しで取得した、ビデオ ポータルのエンドポイント URL を表す文字列です。


 **応答の種類**

Channel オブジェクトのリストを返します。

**注** ビデオ ポータルに多くのチャネルがある場合、この API が戻るまでに長い時間がかかる可能性があります。


****

<a name="GetViewChannels"> </a>
**ユーザーが表示できるチャネルのリストの取得**


ユーザーが表示できる全チャネルのリストを取得します。これらは、ユーザーが所有者、エディター、またはビューアーのアクセス許可を持っているチャネルです。


```no-highlight
GET {VideoPortalURL}/_api/VideoService/Channels
```

 **応答の種類**

Channel オブジェクトのリストを返します。

[!code-REST-i[Get_ChannelView_Info](./_data/video_api_get_channels_view.json)]


****

<a name="GetChannelInfo"> </a>
### 特定のチャネルに関する情報の取得

<a name="GetInfo"> </a>
**チャネル ID、チャネルの色、チャネルのタイトルの取得**

```no-highlight
GET {VideoPortalURL}/_api/VideoService/Channels('{channelId}')
```
**要求 URL パラメーター**

|**必須のパラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|channelId|string|チャネルの ID。|


 **応答の種類**

チャネルに関する次の情報を返します。
- ID - チャネルの ID。
- TileHtmlColor - チャネルの色。
- Title - チャネルのタイトル。

[!code-REST-i[Get_Channel_Info](./_data/video_api_get_channel_info.json)]



<a name="GetVideoList"> </a>
**チャネルの全ビデオのリストの取得**

指定されたチャネルのすべてのビデオのリストを取得します。

```no-highlight
GET {VideoPortalURL}/_api/VideoService/Channels('{channelId}')/Videos
```

**要求 URL パラメーター**

|**必須のパラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|channelId|string|チャネルの ID。|


 **応答の種類**

Video オブジェクトのリストを返します。

[!code-REST-i[Get_Video_List](./_data/video_api_get_video_list.json)]


****

<a name="GetPlayableVideos"> </a>
**チャネルで再生可能な最新のビデオのリストの取得**

チャネルにアップロードされた最新のビデオのソートされたリストを取得し、再生する準備がまだできていないビデオをすべて除外します。ただし、自分でアップロードしたものは除外されません。

チャネルでコード変換が完了して生成できる状態 (VideoProcessingStatus = 2) になっているすべてのビデオ、および再生する準備ができていなくても現在ログインしているユーザーによってアップロードされたビデオの、ソートされたリストが返されます。  

```no-highlight
GET {VideoPortalURL}/_api/VideoService/Channels('{channelId}')/GetAllVideos
```

**要求 URL パラメーター**

|**必須のパラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|channelId|string|チャネルの ID。|


 **応答の種類**

Video オブジェクトのリストを返します

****

<a name="VideoOperations"> </a>
## ビデオの操作

チャネルから特定のビデオの情報を取得し、ビデオが表示された回数および再生の情報を取得します。また、チャネルにビデオをアップロードし、チャネルのビデオを更新し、チャネルからビデオを削除します。
****


<a name="GetVideoInfo"> </a>
### ビデオに関する情報の取得

作成日、タイトル、再生時間、ビデオのサムネイルの URL、処理状態など、特定のビデオに関する情報を取得します。

```no-highlight
GET {VideoPortalURL}/_api/VideoService/Channels('{channelId}')/Videos('{videoId}')
```

**要求 URL パラメーター**

|**必須のパラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|channelId|string|チャネルの ID。|
|videoId|string|ビデオの ID。|

 **応答の種類**

次の情報を返します。
- CreatedDate -- ビデオが最初にアップロードされた日付。
- Title -- ビデオのタイトル。
- VideoDurationInSeconds -- ビデオの再生時間 (秒単位)。
- ThumbnailURL - ビデオのサムネイル イメージの URL。
- VideoProcessingStatus - ビデオの処理の状態。以下の値が設定されます。
    - 0 -- (既定値) -- ビデオはまだ再生用に処理されていません。
    - 1 -- ビデオは取得されて処理中です。
    - 2 -- ビデオは再生できる状態です。
    - 3 -- ビデオは、処理のために Azure Media Services へのアップロード中にエラーが発生しました。
    - 4 -- エラー -- 一般エラー -- ストリーミング用にビデオを処理できません。
    - 5 -- エラー -- タイムアウト エラー -- ストリーミング用にビデオを処理できません。
    - 6 -- エラー -- サポートされない形式 -- ビデオ ファイルの種類は、Azure Media Services によるストリーミング再生にサポートされていません。

[!code-REST-i[Get_Video_Info](./_data/video_api_get_video_info.json)]

****

<a name="GetViewCountInfo"> </a>
**ビデオの表示回数の取得**

表示回数は検索分析によって集計されるので、表示回数は検索エンドポイントから Video オブジェクトを取得するときにのみ返されます。したがって、ハブまたはチャネルの /Search エンドポイントから取得されたとき以外は、ViewCount プロパティの値は正しくありません。1 つのビデオの表示数を取得するには、ビデオの ID を使用して検索クエリを実行します。


```no-highlight
GET {VideoPortalURL}/_api/videoservice/Channels('{channelId}')/search/query('{videoId}')?$Select=ViewCount
```

**要求 URL パラメーター**

|**必須のパラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|channelId|string|チャネルの ID。|
|videoId|string|ビデオの ID。|

**クエリ パラメーター**

|名前|型|説明|
|:-----|:-----|:-----|
|_$Select=ViewCount|string|応答に含める表示回数。|

 **応答の種類**

ビデオが表示された回数を返します。

[!code-REST-i[Get_View_Count](./_data/video_api_get_view_count.json)]


****

<a name="GetPlaybackInfo"> </a>
### ビデオの再生に関する情報の取得

<a name="GetVideoManifest"> </a>
**ビデオの Azure Media Services マニフェストの URL の取得**

ビデオの Azure Media Services マニフェストの URL を取得します。Azure Media Services アセットの再生をサポートするプレーヤーに、このマニフェストをフィードできます。

```no-highlight
GET {VideoPortalURL}/_api/VideoService/Channels('{channelId}')/Videos('{videoId}')/GetPlaybackUrl('{streamingFormatType}')
```

**要求 URL パラメーター**

|**必須のパラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|channelId|string|チャネルの ID。|
|videoId|string|ビデオの ID。|
|streamingFormatType|数値|ビデオのストリーミング形式の種類。|

*streamingFormatType* パラメーターは次のいずれかの値です。
- 1 -- Smooth Streaming または MPEG-DASH
- 0 -- HLS ストリーミング

**応答の種類**

ビデオのマニフェストの URL を返します。

[!code-REST-i[Get_Format_Info](./_data/video_api_get_streaming_format.json)]


****

<a name="GetDecryptionToken"> </a>
**ビデオの暗号化の解除にアクセスするためのベアラー トークンの取得**

O365 のビデオはすべて AES で暗号化されます。Smooth Streaming または MPEG-DASH 形式でビデオを再生するには、最初に、コンテンツの復号化にアクセスするためのベアラー トークンを取得する必要があります。この API は、プレーヤーがコンテンツを復号化できる認証トークンを返します。

ただし、HLS ストリーミングの場合は、HLS 形式のマニフェストを取得するときに使用するマニフェスト URL の一部になっているため、キー アクセス トークンを別途取得する必要はありません。


```no-highlight
GET {VideoPortalURL}/_api/VideoService/Channels('{channelId}')/Videos('{videoId}')/GetStreamingKeyAccessToken
```

**要求 URL パラメーター**

|**必須のパラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|channelId|string|チャネルの ID。|
|videoId|string|ビデオの ID。|


 **応答の種類**

プレーヤーがコンテンツの暗号化の解除ができる認証トークンを返します。

[!code-REST-i[Get_Bearer_Token](./_data/video_api_get_bearer_token.json)]

****

<a name="UploadVideos"> </a>
### チャネルへのビデオのアップロード

ビデオをアップロードするプレースホルダーとして機能する空のビデオ オブジェクトを作成します。

それが済むと、小さい 1 つのビデオを 1 つの POST 呼び出しで、またはチャンクになった大きい1 つのビデオを複数の POST 呼び出しで、アップロードできます。

<a name="CreateEmptyVideoObject"> </a>
**ビデオをアップロードするためのプレースホルダーの作成**


ビデオをアップロードする場所のプレースホルダーとして機能する空のビデオ オブジェクトを作成します。

```no-highlight
POST {VideoPortalURL}/_api/VideoService/Channels('{channelId}')/Videos
```

**要求 URL パラメーター**

|**必須のパラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|channelId|string|チャネルの ID。|


**要求本文**

{ '__metadata': { 'type':'SP.Publishing.VideoItem' }, 'Description': ' {         *説明のテキスト*
    } ', 'Title': ' {         *ビデオのタイトル*
    } ' }


|名前|型|説明|
|:-----|:-----|:-----|
|metadata|SP.Publishing.VideoItem|更新するオブジェクトの種類|
|Description|string|ビデオの説明。|
|Title|string|ビデオのタイトル。|
|FileName|string|ビデオのファイル名。|


**応答の種類**

VideoObject -- ビデオのアップロード先のオブジェクト。 返される ID を、アップロードを開始するビデオ ID として使用します。

****

<a name="UploadSmallVideo"> </a>
**単一の POST での小さいビデオのアップロード**

1 回の POST 送信に収まる小さな単一のビデオをアップロードします。


```no-highlight
POST {VideoPortalURL}/_api/VideoService/Channels('{channelId}')/Videos('{videoId}')/GetFile()/SaveBinaryStream
```
[//]: # (just checking that the above empty parens for GetFile are intentional)

**要求 URL パラメーター**

|**必須のパラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|channelId|string|チャネルの ID。|
|videoId|string|ビデオの ID。|

**要求本文**

ファイルのバイナリ ストリーム。 

**応答の種類**

200 を返します。 応答本文はありません。

****

<a name="UploadLargerVideoInChunks"> </a>
###チャンクでの大きいビデオのアップロード

1 回の POST 呼び出しには収まらない大きさのビデオをアップロードする場合、またはチャンクでのアップロードをキャンセルするには、次の呼び出しを使用します。


<a name="StartUploading"> </a>
**前に作成したビデオ オブジェクトへのアップロードの開始**

チャンクになったビデオのアップロードを開始します。

```no-highlight
POST {VideoPortalURL}/_api/VideoService/Channels('{channelId}')/Videos('{videoId}')/GetFile()/StartUpload(uploadId=guid'{yourGeneratedGuid}')
```

**要求 URL パラメーター**

|**必須のパラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|channelId|string|チャネルの ID。|
|videoId|string|ビデオの ID。|
|yourGeneratedGuid|GUID|アップロード セッション用に生成する GUID。|


**応答の種類**

200 を返します。 応答本文はありません。

****

<a name="UploadEachChunk"> </a>
**前に作成したビデオ オブジェクトへのファイルの各チャンクのアップロード**

ファイルの次のチャンクのアップロードを続けます。 必要な回数だけ繰り返します。

```no-highlight
POST {VideoPortalURL}/_api/VideoService/Channels('{channelId}')/Videos('{videoId}')/GetFile()/ContinueUpload(uploadId=guid'{yourGeneratedGuid}',fileOffset='{offsetSize}')
```

**要求 URL パラメーター**

|**必須のパラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|channelId|string|チャネルの ID。|
|videoId|string|ビデオの ID。|
|yourGeneratedGuid|GUID|アップロード セッション用に生成する GUID。|
|offsetSize|整数|既にアップロードされたバイト数。|

**注:**8 MB のチャンクをアップロードした場合、最初のチャンクのオフセットは 0 で、2 番目のチャンクのオフセットは 8 * 1024 = 8192 です。


**要求本文**

このチャンクのファイルのバイナリ ストリーム。

**応答の種類**

200 を返します。 応答本文はありません。

****


<a name="FinishUploading"> </a>
**前に作成したビデオ オブジェクトへのファイルの最終チャンクのアップロード終了**

チャンクになったビデオのアップロードを終了します。

```no-highlight
POST {VideoPortalURL}/_api/VideoService/Channels('{channelId}')/Videos('{videoId}')/GetFile()/FinishUpload(uploadId=guid'{yourGeneratedGuid}',fileOffset='{offsetSize}')
```

**要求 URL パラメーター**

|**必須のパラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|channelId|string|チャネルの ID。|
|videoId|string|ビデオの ID。|
|yourGeneratedGuid|GUID|アップロード セッション用に生成する GUID。|
|offsetSize|整数|既にアップロードされたバイト数。|

**要求本文**

最後のチャンクのファイルのバイナリ ストリーム。

**応答の種類**

200 を返します。 応答本文はありません。

****

<a name="CancelChunkedUploading"> </a>
**アップロードのキャンセル**

チャンクになったビデオのアップロードをキャンセルします。

```no-highlight
POST {VideoPortalURL}/_api/VideoService/Channels('{channelId}')/Videos('{videoId}')/GetFile()/CancelUpload(uploadId=guid'{yourGeneratedGuid}')
```

**要求 URL パラメーター**

|**必須のパラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|channelId|string|チャネルの ID。|
|videoId|string|ビデオの ID。|
|yourGeneratedGuid|GUID|アップロード セッション用に生成する GUID。|

**応答の種類**

200 を返します。 応答本文はありません。

****


<a name="UpdateVideo"> </a>
###ビデオのメタデータの更新
**チャネルの既存ビデオのメタデータの更新**


ビデオのタイトルと説明を変更します。

```no-highlight
POST {VideoPortalURL}/_api/VideoService/Channels('{channelId}')/Videos('{videoId}')
```

**要求 URL パラメーター**

|**必須のパラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|channelId|string|チャネルの ID。|
|videoId|string|ビデオの ID。|


**要求ヘッダー**

|名前|型|説明|
|:-----|:-----|:-----|
|X-HTTP-Method|string|X-HTTP-Method プロパティの値は MERGE です。|


**要求本文**

{'__metadata':{'type':'SP.Publishing.VideoItem'},'Description':'{*説明のテキスト*}', 'Title':'{*ビデオのタイトル*}'}

|名前|型|説明|
|:-----|:-----|:-----|
|metadata|SP.Publishing.VideoItem|ビデオ オブジェクトの型。|
|説明|string|ビデオの説明。|
|Title|string|ビデオのタイトル。|

 **応答の種類**

200 を返します。 応答本文はありません。

****

<a name="DeleteVideos"> </a>
## チャネルからのビデオの削除



<a name="DeleteVideo"> </a>
**チャネルからの既存ビデオの削除**


```no-highlight
 POST {VideoPortalURL}/_api/VideoService/Channels('{channelId}')/Videos('{videoId}')
```

**要求 URL パラメーター**

|**必須のパラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|channelId|string|チャネルの ID。|
|videoId|string|ビデオの ID。|


**要求ヘッダー**

|名前|型|説明|
|:-----|:-----|:-----|
|X-HTTP-Method|string|X-HTTP-Method プロパティの値は DELETE です。|

 **応答の種類**

200 を返します。 応答本文はありません。

****

<a name="EmbedVideo"> </a>
## 別のページにビデオを埋め込む

****

<a name="GetEmbedCodeConfigureParameters"> </a>
**別の Web ページにビデオ アイテムを埋め込むためのコードを取得し、パラメーターの値を指定する**

この呼び出しを使用して、埋め込まれたウィンドウの幅と高さや、ページを開いたときにビデオを自動的に再生させるかどうか、ビデオを一時停止したときに特定の情報をビデオ プレーヤーに表示するかどうかなど、渡すパラメーターの値を指定できます。この情報には、ビデオのタイトル、再生時間、再生回数のカウント、およびチャネルの名前が含まれています。 

```no-highlight
GET {VideoPortalURL}/_api/VideoService/Channels('{channelId}')/Videos('{videoId}')/GetVideoEmbedCode?width={width}&height={height}&autoplay={true/false}&showinfo={true/false}
```

パラメーターの値を渡さなかった場合、Office 365 はいくつかの実用的な既定設定 (幅 120、高さ 230 など) を選択します。

**要求 URL パラメーター**

|**必須のパラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|channelId|string|チャネルの ID。|
|videoId|string|ビデオの ID。|
|width|数値|埋め込みビデオ ウィンドウの幅。|
|height|数値|埋め込みビデオ ウィンドウの高さ。|
|自動再生|true/false|埋め込まれたビデオを自動的に再生するかどうか。|
|ShowInfo|true/false|ビデオを一時停止したときにビデオのタイトル、再生時間、表示回数、およびチャネル名をプレーヤーに表示するかどうか。|


**応答の種類**

埋め込みコードを返します。

****

<a name="GetEmbedCodeDefaultValues"> </a>
**既定値を使用して、別の Web ページにビデオ アイテムを埋め込むためのコードを取得する**

この呼び出しを使用して、埋め込みビデオ プレーヤーの既定値を指定する埋め込みコードを取得できます。

```no-highlight
GET {VideoPortalURL}/_api/VideoService/Channels('{channelId}')/Videos('{videoId}')/?$Select=Title,DefaultEmbedCode
```

ビデオ オブジェクトでは、"DefaultEmbedCode" プロパティは自動的に返されません。"DefaultEmbedCode" を取得するには、$select オプションを使用する必要があります。

$select オプションを使用して、タイトル、既定値の埋め込みコードなど、いずれかのビデオ プロパティを返すビデオ アイテムを要求することができます。

**要求 URL パラメーター**

|**必須のパラメーター**|**種類**|**説明**|
|:-----|:-----|:-----|
|channelId|string|チャネルの ID。|
|videoId|string|ビデオの ID。|


**応答の種類**

$select を使用して、要求する既定のプロパティ (既定の埋め込みコード値など) を返します。

****

<a name="GetVideoItemFromPlayerPageURL"> </a>
**プレーヤー ページ URL からビデオ アイテムの情報および既定の埋め込みコードを取得する**

O365 ビデオ ポータル プレーヤー ページの URL がわかっている場合は、この呼び出しを使用して、ビデオ ID、チャンネル ID、およびビデオに関する他の情報を取得できます。

```no-highlight
POST {VideoPortalURL}/_api/VideoService/GetVideoByURL?$Select=Title,Description,CreatedDate,DefaultEmbedCode,VideoDurationInSeconds,ID,VideoProcessingStatus
```

$select オプションを使用して、以下に示すように、タイトル、既定値の埋め込みコードなど、いずれかのビデオ プロパティを返すビデオ アイテムを要求することができます。


**要求ヘッダー**

Accept=application/json;odata=verbose

Content-Type=application/json;odata=verbose


**要求本文**

{'videoFileRelativeUrl':'https://*root_SharePoint_site*/portals/hub/_layouts/15/PointPublishing.aspx?app=video&p=p&chid=b74774cb-faad-43a0-8de9-cb263e38d75d&vid=e5b66725-9f87-4813-9b50-b24fe80c9c20'}

**VideoFileRelativeURL** は、O365 ビデオ ポータル プレーヤー ページのビデオの相対パスまたは絶対 URL です。

****

<a name="NextSteps"> </a>
## 次の手順

アプリケーション開発を開始する準備ができている方にも、単に詳しい情報を必要としている方にも、最適なコンテンツをご用意しています。


- 始める準備ができていますか。まず、[環境を設定](..\howto\setup-development-environment.md)してみましょう。

- これらの API を使用してさらに実習してみますか。この[ビデオ API 向けの実習](http://dev.office.com/hands-on-labs/4589)をご覧ください。

Office 365 プラットフォームの使い方の詳細については、次のリンク先をご覧ください。

- [Office 365 プラットフォーム上での開発](..\howto\platform-development-overview.md)

- [Office 365 アプリの認証](..\howto\common-app-authentication-tasks.md)

- [Office 365 アプリへの共通の同意の手動による追加](..\howto\add-common-consent-manually.md)

- [Office 365 ビデオの無効化または再有効化](https://support.office.com/en-us/article/Manage-your-Office-365-Video-portal-c059465b-eba9-44e1-b8c7-8ff7793ff5da#BKMK_Disabling)

- [予定表 REST 操作](..\api\calendar-rest-operations.md)

- [連絡先 REST 操作](..\api\contacts-rest-operations.md)

- [OneDrive API](https://dev.onedrive.com/)

- [探索サービス REST 操作](..\api\discovery-service-rest-operations.md)

- [メール REST 操作](..\api\mail-rest-operations.md)

- [OneDrive API](https://dev.onedrive.com/)
