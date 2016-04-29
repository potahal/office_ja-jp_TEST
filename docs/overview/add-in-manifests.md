
# Office アドインの XML マニフェスト
Office アドインのマニフェスト XML ファイルを作成または編集します。

 _ **適用対象:** Access apps for SharePoint?| apps for Office?| Excel?| Office Add-ins?| Outlook?| PowerPoint?| Project?| Word_

Office アドイン の XML マニフェスト ファイルを使用すると、エンド ユーザーが Office ドキュメントや Office アプリケーションと共にアドインをインストールし、使用する場合に、アドインがどのようにアクティブ化されるかを宣言で説明できます。 [offappmanifest.xsd](http://msdn.microsoft.com/ja-jp/library/d5f72bff-3446-c64f-02ca-ab10b5648789%28Office.15%29.aspx) では、サポートされているすべての Office アプリケーション (リッチ クライアント アプリケーションと対応する Web アプリの Web クライアントの両方) に共通する XML スキーマを定義します。

 >**メモ**  Office アドイン では、バージョン 1.1 のマニフェスト スキーマを使用する必要があります。マニフェスト 1.1 を使用するように アドイン を更新する方法については、「 [JavaScript API for Office およびマニフェスト スキーマ ファイルのバージョンを更新する](../docs/develop/update-your-javascript-api-for-office-and-manifest-schema-version.md)」をご覧ください。

このスキーマに基づいた XML マニフェスト ファイルを使用すると、Office アドインで次のことができます。

- ID、バージョン、説明、表示名、および既定のロケールを指定することで、アプリ自体について説明する。
    
- Office アドインの UI を提供する HTML ファイルの場所を指定する。
    
- コンテンツ Office アドインに必要な既定のサイズ、および Outlook Office アドインに必要な高さを指定する。
    
- ドキュメントの読み取り、書き込みなど、Office アドインに必要なアクセス許可を宣言する。
    
- コンテンツ (ドキュメント内)、作業ウィンドウ内、または、メッセージ、予定、会議出席依頼のアイテムでコンテキストに応じてなど、アプリがどのように使用され、表示されるかを指定する。
    
- Outlook Office アドインでは、アプリがアクティブ化されてメッセージ、予定、または会議出席依頼アイテムとやり取りを行うコンテキストを指定するルールを定義する。
    
マニフェスト v1.1 XML ファイルの例については、「 [マニフェスト v1.1 の XML ファイルの例](#マニフェスト-v1.1-の-xml-ファイルの例)」を参照してください。

## 必須要素


次の表では、3 種類の Office アドイン および辞書の作業ウィンドウ アドインに必要な要素を示しています。


 >**重要**  Office ストア に送信されるアドインでは、 **SourceLocation** 要素に指定されるソース ファイルの場所などのすべてのアドインの場所は、SSL (HTTPS) でセキュリティ保護される必要があります。詳細については、「 [提出するときに避ける必要のあるいくつかの一般的なエラーはどれか](http://msdn.microsoft.com/library/0ceb385c-a608-40cc-8314-78e39d6c75d0%28Office.15%29.aspx#bk_q2)」を参照してください。


**Office アドインの種類ごとの必要な要素**


|**要素**|**コンテンツ**|**作業ウィンドウ**|**辞書作業ウィンドウ**|**Outlook**|
|:-----|:-----|:-----|:-----|:-----|
|[OfficeApp](http://msdn.microsoft.com/ja-jp/library/68f1cada-66f8-4341-45f5-14e0634c24fb%28Office.15%29.aspx)|X|X|X|X|
|[Id](http://msdn.microsoft.com/ja-jp/library/67c4344a-935c-09d6-1282-55ee61a2838b%28Office.15%29.aspx)|X|X|X|X|
|[Version](http://msdn.microsoft.com/ja-jp/library/6a8bbaa5-ee8c-6824-4aba-cb1a804269f6%28Office.15%29.aspx)|X|X|X|X|
|[ProviderName](http://msdn.microsoft.com/ja-jp/library/0062693a-fafa-ea2d-051a-75dac0f6c323%28Office.15%29.aspx)|X|X|X|X|
|[DefaultLocale](http://msdn.microsoft.com/ja-jp/library/04796a3a-3afa-dc85-db66-4677560c185c%28Office.15%29.aspx)|X|X|X|X|
|[DisplayName](http://msdn.microsoft.com/ja-jp/library/529159ca-53bf-efcf-c245-e572dab0ef57%28Office.15%29.aspx)|X|X|X|X|
|[Description](http://msdn.microsoft.com/ja-jp/library/bcce6bad-23d0-7631-7d8c-1064b8453b5a%28Office.15%29.aspx)|X|X|X|X|
|[IconUrl](http://msdn.microsoft.com/library/c7dac2d4-4fda-6fc7-3774-49f02b2d3e1e%28Office.15%29.aspx)|X|X|X|X|
|[HighResolutionIconUrl](http://msdn.microsoft.com/library/ff7b2647-ec8e-70dc-4e4a-e1a1377ff3f2%28Office.15%29.aspx)||||X|
|[DefaultSettings (ContentApp)](http://msdn.microsoft.com/ja-jp/library/f7edc689-551f-1a17-ea81-ffd58f534557%28Office.15%29.aspx)[DefaultSettings (TaskPaneApp)](http://msdn.microsoft.com/ja-jp/library/36e3d139-56a4-fb3d-0a21-cbd14e606765%28Office.15%29.aspx)|X|X|X||
|[SourceLocation (ContentApp)](http://msdn.microsoft.com/ja-jp/library/00d95bb0-e8f5-647f-790a-0aa3aabc8141%28Office.15%29.aspx)[SourceLocation (TaskPaneApp)](http://msdn.microsoft.com/ja-jp/library/e6ea8cd4-7c8b-1da7-d8f8-8d3c80a088bc%28Office.15%29.aspx)|X|X|X||
|[DesktopSettings](http://msdn.microsoft.com/ja-jp/library/da9fd085-b8cc-2be0-d329-2aa1ef5d3f1c%28Office.15%29.aspx)||||X|
|[SourceLocation (MailApp)](http://msdn.microsoft.com/ja-jp/library/3792d389-bebd-d19a-9d90-35b7a0bfc623%28Office.15%29.aspx)||||X|
|[Permissions (ContentApp)](http://msdn.microsoft.com/ja-jp/library/9f3dcf9c-fced-c115-4f0d-38d60fb7c583%28Office.15%29.aspx)[Permissions (TaskPaneApp)](http://msdn.microsoft.com/ja-jp/library/d4cfe645-353d-8240-8495-f76fb36602fe%28Office.15%29.aspx)[Permissions (MailApp)](http://msdn.microsoft.com/ja-jp/library/c20cdf29-74b0-564c-e178-b75d148b36d1%28Office.15%29.aspx)|X|X|X|X|
|[Rule (RuleCollection)](http://msdn.microsoft.com/ja-jp/library/c6ce9d52-4b53-c6a6-de7e-c64106135c81%28Office.15%29.aspx)[Rule (MailApp)](http://msdn.microsoft.com/ja-jp/library/56dfc32e-2b8c-1724-05be-5595baf38aa3%28Office.15%29.aspx)||||X|
|[Dictionary](http://msdn.microsoft.com/ja-jp/library/f78898f4-059e-d5dc-5eab-1f6b92214068%28Office.15%29.aspx)|||X||
|[*Requirements (MailApp)](http://msdn.microsoft.com/ja-jp/library/9536ea30-34f7-76b5-7f30-1508626840e4%28Office.15%29.aspx)||||X|
|[*Set](http://msdn.microsoft.com/ja-jp/library/1506daa1-332c-30e1-6402-3371bcd0b895%28Office.15%29.aspx)[**Sets (MailAppRequirements)](http://msdn.microsoft.com/ja-jp/library/2a6a2484-eeee-37e4-43bc-c185e8ae0d1d%28Office.15%29.aspx)||||X|
|[*Form](http://msdn.microsoft.com/ja-jp/library/77a8ac83-c22b-1225-4fc4-ba4038b68648%28Office.15%29.aspx)[**FormSettings](http://msdn.microsoft.com/ja-jp/library/0d1a311d-939d-78c1-e968-89ddf7ebc4b4%28Office.15%29.aspx)||||X|
|[*Sets (Requirements)](http://msdn.microsoft.com/ja-jp/library/509be287-b532-87c6-71ac-64f3a4bbd3af%28Office.15%29.aspx)||||X|
|[*Hosts](http://msdn.microsoft.com/library/f9a739c1-3daf-c03a-2bd9-4a2a6b870101%28Office.15%29.aspx)||||X|
* Office アドイン マニフェスト スキーマ バージョン 1.1 で追加されました。


## マニフェスト v1.1 の XML ファイルの例


以下のセクションに、コンテンツ、作業ウィンドウ、Outlook、辞書 の Office アドインにおけるマニフェスト v1.1 XML ファイルの例を示します。

Visual Studio を使用して Office アドインを開発する場合、基になる XML マークアップを手動で変更せずに、Visual Studio のマニフェスト デザイナーを使用して Office アドインのマニフェスト設定を変更できます。既定では、Visual Studio で Office アドイン マニフェスト ファイルを開くと、マニフェスト デザイナーで開きます。マニフェスト デザイナーにより、マニフェストのフィールドが体系化され、探しやすくなります。フィールドによっては、フィールドの有効値を含むドロップダウン リストがあるので、データ入力エラーが少なくなります。


### コンテンツ Office アドイン マニフェスト v1.1 の例



```XML
<?xml version="1.0" encoding="utf-8"?>
<OfficeApp 
  xmlns="http://schemas.microsoft.com/office/appforoffice/1.1" 
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
  xsi:type="ContentApp">
  <Id>01eac144-e55a-45a7-b6e3-f1cc60ab0126</Id>
  <AlternateId>en-US\WA123456789</AlternateId>
  <Version>1.0.0.0</Version>
  <ProviderName>Microsoft</ProviderName>
  <DefaultLocale>en-US</DefaultLocale>
  <DisplayName DefaultValue="Sample content add-in" />
  <Description DefaultValue="Describe the features of this app." />
  <IconUrl DefaultValue="https://contoso.com/ENUSIcon.png" />
  <Hosts>
    <Host Name="Workbook" />
    <Host Name="Database" />
  </Hosts>
  <Requirements>
    <Sets DefaultMinVersion="1.1">
      <Set Name="TableBindings" />
    </Sets>
  </Requirements>  
  <DefaultSettings>
    <SourceLocation DefaultValue="https://contoso.com/apps/content.html" />
    <RequestedWidth>400</RequestedWidth> 
    <RequestedHeight>400</RequestedHeight>
  </DefaultSettings>
  <Permissions>Restricted</Permissions>
  <AllowSnapshot>true</AllowSnapshot>
</OfficeApp>
```


### 作業ウィンドウ Office アドイン マニフェスト v1.1 の例



```XML
<?xml version="1.0" encoding="UTF-8"?>
<OfficeApp xmlns=
  "http://schemas.microsoft.com/office/appforoffice/1.1"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:type="TaskPaneApp">
  <Id>412ce350-4161-4ad0-a5f5-0ec9d2cd3570</Id>
  <Version>1.0.0.0</Version>
  <ProviderName>Microsoft</ProviderName>
  <DefaultLocale>en-US</DefaultLocale>
  <DisplayName DefaultValue="Sample task pane add-in">
    <Override Locale="en-US" Value="Project add-in"/> 
  </DisplayName>
  <Description DefaultValue="Describe the features of this app.">
    <Override Locale="en-US" Value="Adds project management information to documents"/>
  </Description>
  <IconUrl DefaultValue="https://contoso.com.sa/ProjectApp/Topgunas-SA.png">
    <Override Locale="en-US" Value="https://contoso.com/ProjectApp/Topgunen-US.png"/>
  </IconUrl>
  <AppDomains>
    <AppDomain>http://www.projectlogin.com</AppDomain>
    <AppDomain>http://m.projectlogin.com</AppDomain>
    <AppDomain>http://www.projectlogin.com.sa</AppDomain>
    <AppDomain>http://m.projectlogin.com.sa</AppDomain>
  </AppDomains>
  <Hosts>
    <Host Name = "Document"/>
    <Host Name = "Workbook"/>
    <Host Name = "Presentation"/>
    <Host Name = "Project"/>
  </Hosts>
  <DefaultSettings>
    <SourceLocation DefaultValue="https://contoso.com.sa/ProjectApp/ProjectAppar_SA.html">
      <Override Locale="en-US" Value="https://contoso.com/ProjectApp/ProjectAppen-US.html"/>
    </SourceLocation>
  </DefaultSettings>
  <Permissions>ReadWriteDocument</Permissions>
</OfficeApp>
```


### Outlook Office アドインマニフェスト v1.1 の例



```XML
<?xml version="1.0" encoding="utf-8"?>
<OfficeApp xmlns=
  "http://schemas.microsoft.com/office/appforoffice/1.1"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
  xsi:type="MailApp">

  <Id>971E76EF-D73E-567F-ADAE-5A76B39052CF</Id>
  <Version>1.0</Version>
  <ProviderName>Microsoft</ProviderName>
  <DefaultLocale>en-us</DefaultLocale>
  <DisplayName DefaultValue="YouTube"/>
  <Description DefaultValue=
    "Watch YouTube videos referenced in the e-mails you  
    receive without leaving your email client.">
    <Override Locale="fr-fr" Value="Visualisez les videos
      YouTube referencees dans vos courriers electronique
      directement depuis Outlook et Outlook Web App."/>
  </Description>
  <!-- Change the following line to specify    -->
  <!-- the web serverthat hosts the icon file. -->
  <IconUrl DefaultValue=
    "https://webserver/YouTube/YouTubeLogo.png"/>

  <Hosts>
    <Host Name="Mailbox" />
  </Hosts>
  <Requirements>
    <Sets DefaultMinVersion="1.1">
      <Set Name="Mailbox" />
    </Sets>
  </Requirements>

  <FormSettings>
    <Form xsi:type="ItemRead">
      <DesktopSettings>
        <!-- Change the following line to specify     -->
        <!-- the web server that hosts the HTML file. -->
        <SourceLocation DefaultValue=
          "https://webserver/YouTube/YouTube_read_desktop.htm" />
        <RequestedHeight>216</RequestedHeight>
      </DesktopSettings>
      <TabletSettings>
        <!-- Change the following line to specify     -->
        <!-- the web server that hosts the HTML file. -->
        <SourceLocation DefaultValue=
          "https://webserver/YouTube/YouTube_read_tablet.htm" />
        <RequestedHeight>216</RequestedHeight>
      </TabletSettings>
    </Form>
    <Form xsi:type="ItemEdit">
      <DesktopSettings>
        <!-- Change the following line to specify     -->
        <!-- the web server that hosts the HTML file. -->
        <SourceLocation DefaultValue=
          "https://webserver/YouTube/YouTube_compose_desktop.htm" />
      </DesktopSettings>
      <TabletSettings>
        <!-- Change the following line to specify     -->
        <!-- the web server that hosts the HTML file. -->
        <SourceLocation DefaultValue=
          "https://webserver/YouTube/YouTube_compose_tablet.htm" />
      </TabletSettings>
    </Form>
  </FormSettings>

  <Permissions>ReadWriteItem</Permissions>
  <Rule xsi:type="RuleCollection" Mode="Or"> 
    <Rule xsi:type="RuleCollection" Mode="And">
      <Rule xsi:type="RuleCollection" Mode="Or">
        <Rule xsi:type="ItemIs" ItemType="Appointment" FormType="Read" />
        <Rule xsi:type="ItemIs" ItemType="Message" FormType="Read" />
      </Rule>
      <Rule xsi:type="ItemHasRegularExpressionMatch" 
        PropertyName="BodyAsPlaintext" RegExName="VideoURL" 
        RegExValue=
        "http://(((www\.)?youtube\.com/watch\?v=)|
        (youtu\.be/))[a-zA-Z0-9_-]{11}" />
    </Rule>
    <Rule xsi:type="RuleCollection" Mode="Or">
      <Rule xsi:type="ItemIs" ItemType="Appointment" FormType="Edit" />
      <Rule xsi:type="ItemIs" ItemType="Message" FormType="Edit" /> 
    </Rule> 
  </Rule>
</OfficeApp>

```


### 辞書 Office アドイン マニフェスト v1.1 の例



```XML
<?xml version="1.0" encoding="UTF-8"?>
<OfficeApp 
  xmlns="http://schemas.microsoft.com/office/appforoffice/1.1" 
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
  xsi:type="TaskPaneApp">
  <Id>6f28f837-8326-4231-a033-ea1d80ff9fb3</Id>
  <Version>15.0</Version>
  <ProviderName>Microsoft Office Demo Dictionary</ProviderName>
  <DefaultLocale>en-us</DefaultLocale>
  <!--DisplayName is the name that will appear in the user's list of applications.-->
  <DisplayName DefaultValue="Microsoft Office Demo Dictionary" />
  <!--Description is a 2-3 sentence description of this dictionary. -->
  <Description DefaultValue="The Microsoft Office Demo Dictionary is an example built to demonstrate how a publisher could create a dictionary that integrates with Office. It does not return real definitions." />
  <!--IconUrl is the URI for the icon that will appear in the user's list of applications.-->
  <IconUrl DefaultValue="https://officeimg.vo.msecnd.net/_layouts/images/general/office_logo.jpg" />
  <Hosts>
    <Host Name = "Document"/>
    <Host Name = "Workbook"/>
    <Host Name = "Presentation"/>
    <Host Name = "Project"/>
  </Hosts>
  <DefaultSettings>
    <!--Replace the value specified for DefaultValue below with the URL for your dictionary-->
    <SourceLocation DefaultValue="https://contoso.com/ExampleDictionary/DictionaryHome.html" />
  </DefaultSettings>
  <!--Permissions is the set of permissions a user will have to give your dictionary. If you need write access, such as to allow a user to replace the highlighted word with a synonym, use ReadWriteDocument. -->
  <Permissions>ReadDocument</Permissions>
  <Dictionary>
    <!--TargetDialects is the set of dialects your dictionary contains. For example, if your dictionary applies to Spanish (Mexico) and Spanish (Peru), but not Spanish (Spain), you can specify that here. This is for different dialects of the same language. Please do NOT put more than one language (e.g. Spanish and English) here. Publish separate languages as separate dictionaries. -->
    <TargetDialects>
      <TargetDialect>EN-AU</TargetDialect>
      <TargetDialect>EN-BZ</TargetDialect>
      <TargetDialect>EN-CA</TargetDialect>
      <TargetDialect>EN-029</TargetDialect>
      <TargetDialect>EN-HK</TargetDialect>
      <TargetDialect>EN-IN</TargetDialect>
      <TargetDialect>EN-ID</TargetDialect>
      <TargetDialect>EN-IE</TargetDialect>
      <TargetDialect>EN-JM</TargetDialect>
      <TargetDialect>EN-MY</TargetDialect>
      <TargetDialect>EN-NZ</TargetDialect>
      <TargetDialect>EN-PH</TargetDialect>
      <TargetDialect>EN-SG</TargetDialect>
      <TargetDialect>EN-ZA</TargetDialect>
      <TargetDialect>EN-TT</TargetDialect>
      <TargetDialect>EN-GB</TargetDialect>
      <TargetDialect>EN-US</TargetDialect>
      <TargetDialect>EN-ZW</TargetDialect>
    </TargetDialects>
    <!--QueryUri is the address of this dictionary's XML webservice (which we use to put definitions in additional contexts, such as the spelling checker.)-->
    <QueryUri DefaultValue="https://contoso.com/ExampleDictionary/WebService.asmx/Define?word="/>
    <!--Citation Text, Dictionary Name, and Dictionary Home Page will be combined to form the citation line - e.g. this would produce "Examples by: Microsoft", where "Microsoft" is a hyperlink to http://www.microsoft.com-->
    <CitationText DefaultValue="Examples by: " />
    <DictionaryName DefaultValue="Microsoft" />
    <DictionaryHomePage DefaultValue="https://www.microsoft.com" />
  </Dictionary>
</OfficeApp>
```


## Office アドインのマニフェストを検証する


Office アドインを説明するマニフェスト ファイルが正しく、完全であることを確認するため、XML スキーマ定義 (XSD) ファイルに対してマニフェストを検証します。マニフェストの検証には、XML スキーマ検証ツールか Visual Studio を使用できます。 [Office アプリ互換性キット](https://www.microsoft.com/en-us/download/details.aspx?id=46831)をダウンロードし、アドインに対して実行することもできます。

スキーマに対するマニフェストの検証の詳細については、「[XML Schema (XSD) 検証ツール](http://stackoverflow.com/questions/124865/xml-schema-xsd-validation-tool)」を参照してください。


 >**メモ**  offappmanifest.xsd ファイルに対するマニフェストの検証は使用されなくなりました。マニフェストのスキーマをバージョン 1.1 に更新することに関しては、「 [JavaScript API for Office およびマニフェスト スキーマ ファイルのバージョンを更新する](../docs/develop/update-your-javascript-api-for-office-and-manifest-schema-version.md)」をご覧ください。


## アドイン ウィンドウで開くドメインの指定


既定では、アドインがスタート ページ (アドインのマニフェスト ファイルの [SourceLocation](http://msdn.microsoft.com/ja-jp/library/00d95bb0-e8f5-647f-790a-0aa3aabc8141%28Office.15%29.aspx) 要素に指定される) をホストするドメインとは異なるドメインの URL に移動しようとすると、移動先の URL は Office ホスト アプリケーションのアドイン ウィンドウとは別の新しいブラウザー ウィンドウに開きます。この既定の動作は、埋め込まれている **iframe** 要素によるアドイン ウィンドウ内での予期しないページ ナビゲーションからユーザーを守るためのものです。

この動作を変更するには、マニフェスト ファイルの [AppDomains](http://msdn.microsoft.com/ja-jp/library/13cf867d-9b24-786f-0687-6bcdc954628e%28Office.15%29.aspx) 要素で指定するドメインの一覧で、アドイン ウィンドウで開く各ドメインを指定します。この一覧にないドメインの URL にアドインが移動しようとすると、その URL は (アドイン ウィンドウとは別の) 新しいブラウザー ウィンドウで開きます。

次の XML マニフェストの例では、 **SourceLocation** 要素に指定された `https://www.contoso.com` ドメインのメイン アドイン ページをホストします。また、この例では、 **AppDomains** 要素リストの [AppDomain](http://msdn.microsoft.com/ja-jp/library/2a0353ec-5e09-6fbf-1636-4bb5dcebb9bf%28Office.15%29.aspx) 要素に `https://www.northwindtraders.com` ドメインも指定しています。アドインが www.northwindtraders.com ドメイン内のページに移動すると、そのページはアドイン ウィンドウで開きます。




```XML
<?xml version="1.0" encoding="UTF-8"?>
<OfficeApp xmlns="http://schemas.microsoft.com/office/appforoffice/1.1" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="TaskPaneApp">
  <Id>c6890c26-5bbb-40ed-a321-37f07909a2f0</Id>
  <Version>1.0</Version>
  <ProviderName>Contoso, Ltd</ProviderName>
  <DefaultLocale>en-US</DefaultLocale>
  <DisplayName DefaultValue="Northwind Traders Excel" />
  <Description DefaultValue="Search Northwind Traders data from Excel"/>
  <AppDomains>
    <AppDomain>https://www.northwindtraders.com</AppDomain>
  </AppDomains>
  <DefaultSettings>
    <SourceLocation DefaultValue="https://www.contoso.com/search_app/Default.aspx" />
  </DefaultSettings>
  <Permissions>ReadWriteDocument</Permissions>
</OfficeApp>
```


## その他のリソース



- [Office のホストと API の要件を指定する](../../docs/overview/specify-office-hosts-and-api-requirements.md)
    
- [Excel、Word、および PowerPoint のマニフェストでアドイン コマンドを作成する (プレビュー)](../../docs/design/create-add-in-commands-in-your-manifest-preview.md)
    
- [Office アドインのローカライズ](../../develop/localization.md)
    
- [Office アドインのマニフェスト向けのスキーマ リファレンス (v1.1)](http://msdn.microsoft.com/ja-jp/library/7e0cadc3-f613-8eb9-57ef-9032cbb97f92%28Office.15%29.aspx)
    
