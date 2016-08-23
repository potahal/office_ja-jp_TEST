# SharePoint サイトにカスタム リボンを追加する

SharePoint サイトにカスタム リボンを追加または削除します。カスタム リボンのイベントを処理するには、埋め込み JavaScript の技法を使用して JavaScript イベント ハンドラーを追加します。

_**適用対象:** SharePoint 用アドイン | SharePoint 2013 | SharePoint Online_

[Core.RibbonCommands](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.RibbonCommands) コード サンプルは、SharePoint サイトにカスタム リボンを追加する方法を示しています。 このソリューションは、以下の操作を行う場合に使用します。

- SharePoint のサイトまたはリストにカスタム リボン、グループ、またはボタンを追加する。
    
- 特定のコンテンツ タイプ、サイト、またはリストに対してカスタム リボンを表示する。


              **メモ**  このコード サンプルでは、リボンのボタンから発生するイベントを処理する JavaScript 関数を呼び出す方法を説明します。このコード サンプルでは、リボンのボタンのために JavaScript イベント ハンドラー関数を実装する方法については説明しません。JavaScript イベント ハンドラー関数を実装するには、埋め込み JavaScript の技法を使用して、カスタム リボンを表示するすべてのページに JavaScript イベント ハンドラー関数を埋め込みます。JavaScript を埋め込む方法の詳細については、「[JavaScript を使用して SharePoint サイト UI をカスタマイズする](Customize-your-SharePoint-site-UI-by-using-JavaScript.md)」を参照してください。

## はじめに
<a name="sectionSection0"> </a>

まず、[Core.RibbonCommands](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.RibbonCommands) サンプル アドインを、GitHub の [Office 365 Developer Patterns and Practices](https://github.com/OfficeDev/PnP/tree/dev) プロジェクトからダウンロードします。

## Core.RibbonCommands アプリを使用する
<a name="sectionSection1"> </a>

このコード サンプルを実行する場合は、スタート ページの **[Register the ribbon]** (リボンの登録) で **[Add Ribbon]** (リボンの追加) を選択します。 ページを更新してから、**[ドキュメント]** > **[カスタム タブ]** を選択すると、カスタム リボンが表示されます。

このコード サンプルでは、Models\RibbonCommands.xml を使用してカスタム リボンを定義します。RibbonCommands.xml は、カスタム リボン グループ、ボタン、およびリボンの UI イベント ハンドラーを定義します。詳細については、「 [SharePoint 2010 Server リボンのカスタマイズと展開](https://msdn.microsoft.com/library/office/gg552606%28v=office.14%29.aspx)」と「 [Server リボン XML](https://msdn.microsoft.com/library/office/ff407290%28v=office.14%29.aspx)」を参照してください。

カスタム リボンは、ホスト Web のすべてのサイトとリストに表示されます。これは、**RegistrationId="0x01"** であり、**RegistrationType="ContentType"** であるためです。  
                **RegistrationId="0x01"** および **RegistrationType="ContentType"** により、**"0x01"** から継承するすべてのコンテンツ タイプ (**Item** クラスから継承するコンテンツ タイプ) にリボンを表示することを指定しています。 カスタム コンテンツ タイプにリボンを適用するには、"0x01" をカスタム コンテンツ タイプの ID に置き換えます。 リストにリボンを適用するには、RegistrationType の値を **List** に変更します。 

**メモ**  この記事で提供されるコードは、明示または黙示のいかなる種類の保証なしに現状のまま提供されるものであり、特定目的への適合性、商品性、権利侵害の不存在についての暗黙的な保証は一切ありません。

```XML
<?xml version="1.0" encoding="utf-8" ?>
<Elements xmlns="http://schemas.microsoft.com/sharepoint/">
  <CustomAction
    Id="CustomCustomRibbonTab"
    Location="CommandUI.Ribbon.ListView"
    RegistrationId="0x01"
    RegistrationType="ContentType"
    Sequence="100"
    >
    <CommandUIExtension>
      <CommandUIDefinitions>
        <CommandUIDefinition
          Location="Ribbon.Tabs._children">
          <Tab
            Id="Ribbon.CustomRibbonTab"
            Title="Custom Tab"
            Description="Custom Tab Description"
            Sequence="501">
            <Scaling
              Id="Ribbon.CustomRibbonTab.Scaling">
              <MaxSize
                Id="Ribbon.CustomRibbonTab.MaxSize"
                GroupId="Ribbon.CustomRibbonTab.ManageCustomGroup"
                Size="OneLargeTwoMedium"/>
              <MaxSize
                Id="Ribbon.CustomRibbonTab.TabTwoMaxSize"
                GroupId="Ribbon.CustomRibbonTab.NewOpenCustomGroup"
                Size="TwoLarge" />
              <Scale
                Id="Ribbon.CustomRibbonTab.Scaling.CustomTabScaling"
                GroupId="Ribbon.CustomRibbonTab.ManageCustomGroup"
                Size="OneLargeTwoMedium" />
              <Scale
                Id="Ribbon.CustomRibbonTab.Scaling.CustomSecondTabScaling"
                GroupId="Ribbon.CustomRibbonTab.NewOpenCustomGroup"
                Size="TwoLarge" />
            </Scaling>
            <Groups Id="Ribbon.CustomRibbonTab.Groups">
              <Group
                Id="Ribbon.CustomRibbonTab.ManageCustomGroup"
                Description="Group to Custom Functions"
                Title="Manage Item"
                Sequence="52"
                Template="Ribbon.Templates.CustomTemplate">
                <Controls Id="Ribbon.CustomRibbonTab.ManageCustomGroup.Controls">
                  <Button
                    Id="Ribbon.CustomRibbonTab.ManageCustomGroup.Accept"
                    Command="CustomRibbonTab.AcceptCustomCommand"
                    Sequence="15"
                    Image32by32="{SiteUrl}/_layouts/15/1033/Images/formatmap32x32.png?rev=23"
                    Image32by32Top="-68"
                    Image32by32Left="-272"
                    Description="Accept Item"
                    LabelText="Accept"
                    TemplateAlias="AWR" />
                  <Button
                    Id="Ribbon.CustomRibbonTab.ManageCustomGroup.Reject"
                    Command="CustomRibbonTab.RejectCustomCommand"
                    Sequence="17"
                    Image16by16="{SiteUrl}/_layouts/15/1033/Images/formatmap16x16.png?rev=23"
                    Image16by16Top="-216"
                    Image16by16Left="-290"
                    Description="Reject Item"
                    LabelText="Reject"
                    TemplateAlias="RWR"/>
                  <Button
                    Id="Ribbon.CustomRibbonTab.ManageCustomGroup.Verify"
                    Command="CustomRibbonTab.VerifyCustomCommand"
                    Sequence="19"
                    Image16by16="{SiteUrl}/_layouts/15/1033/Images/formatmap16x16.png?rev=23"
                    Image16by16Top="-126"
                    Image16by16Left="-144"
                    Description="Verify Item"
                    LabelText="Verify"
                    TemplateAlias="ACWR"/>
                  <Button
                   Id="Ribbon.CustomRibbonTab.ManageCustomGroup.Close"
                   Command="CustomRibbonTab.CloseCustomCommand"
                   Sequence="19"
                   Image32by32="{SiteUrl}/_layouts/15/1033/Images/formatmap32x32.png?rev=23"
                   Image32by32Top="-0"
                   Image32by32Left="-34"
                   Description="Close Item"
                   LabelText="Close"
                   TemplateAlias="CWR"/>
                  <Button
                   Id="Ribbon.CustomRibbonTab.ManageCustomGroup.Copy"
                   Command="CustomRibbonTab.CopyCustomCommand"
                   Sequence="19"
                   Image32by32="{SiteUrl}/_layouts/15/1033/Images/formatmap32x32.png?rev=23"
                   Image32by32Top="-442"
                   Image32by32Left="-67"
                   Description="Copy Item"
                   LabelText="Copy"
                   TemplateAlias="CPWR"/>
                </Controls>
              </Group>
              <Group
                Id="Ribbon.CustomRibbonTab.NewOpenCustomGroup"
                Description="Group to manage item"
                Title="New &amp;amp; Open"
                Sequence="53"
                Template="Ribbon.Templates.CustomTemplate">
                <Controls Id="Ribbon.CustomRibbonTab.NewOpenCustomGroup.Controls">
                  <Button
                    Id="Ribbon.CustomRibbonTab.NewOpenCustomGroup.New"
                    Command="CustomRibbonTab.NewCustomCommand"
                    Sequence="15"
                    Image32by32="{SiteUrl}/_layouts/15/1033/Images/formatmap32x32.png?rev=23"
                    Image32by32Top="-33"
                    Image32by32Left="-66"
                    Description="New Item"
                    LabelText="New"
                    TemplateAlias="LOR"/>
                  <Button
                   Id="Ribbon.CustomRibbonTab.NewOpenCustomGroup.Open"
                   Command="CustomRibbonTab.OpenCustomCommand"
                   Sequence="15"
                   Image32by32="{SiteUrl}/_layouts/15/1033/Images/formatmap32x32.png?rev=23"
                   Image32by32Top="-170"
                   Image32by32Left="-138"
                   Description="Open Item"
                   LabelText="Open"
                   TemplateAlias="LORS"/>
                </Controls>
              </Group>
            </Groups>
          </Tab>
        </CommandUIDefinition>
        <CommandUIDefinition Location="Ribbon.Templates._children">
          <GroupTemplate Id="Ribbon.Templates.CustomTemplate">
            <Layout
              Title="OneLargeTwoMedium"
              LayoutTitle="OneLargeTwoMedium">
              <Section Alignment="Top" Type="OneRow">
                <Row>
                  <ControlRef DisplayMode="Large" TemplateAlias="AWR" />
                </Row>
              </Section>
              <Section Alignment="Top" Type="TwoRow">
                <Row>
                  <ControlRef DisplayMode="Medium" TemplateAlias="RWR" />
                </Row>
                <Row>
                  <ControlRef DisplayMode="Medium" TemplateAlias="ACWR" />
                </Row>
              </Section>
              <Section Alignment="Top" Type="OneRow">
                <Row>
                  <ControlRef DisplayMode="Large" TemplateAlias="CWR" />
                </Row>
              </Section>
              <Section Alignment="Top" Type="OneRow">
                <Row>
                  <ControlRef DisplayMode="Large" TemplateAlias="CPWR" />
                </Row>
              </Section>
            </Layout>
            <Layout
             Title="TwoLarge"
             LayoutTitle="TwoLarge">
              <Section Alignment="Top" Type="OneRow">
                <Row>
                  <ControlRef DisplayMode="Large" TemplateAlias="LOR" />
                </Row>
              </Section>
              <Section Alignment="Top" Type="OneRow">
                <Row>
                  <ControlRef DisplayMode="Large" TemplateAlias="LORS" />
                </Row>
              </Section>
            </Layout>
          </GroupTemplate>
        </CommandUIDefinition>
      </CommandUIDefinitions>
      <CommandUIHandlers>
        <CommandUIHandler
          Command="CustomRibbonTab.AcceptCustomCommand"
          CommandAction="javascript:GetCurrentItem('AP');"/>
        <CommandUIHandler
          Command="CustomRibbonTab.RejectCustomCommand"
          CommandAction="javascript:GetCurrentItem('RJ');"/>
        <CommandUIHandler
          Command="CustomRibbonTab.VerifyCustomCommand"
          CommandAction="javascript:GetCurrentItem('AK');"/>
        <CommandUIHandler
          Command="CustomRibbonTab.NewCustomCommand"
          CommandAction="javascript:AddNewCustom();"/>
        <CommandUIHandler
          Command="CustomRibbonTab.OpenCustomCommand"
          CommandAction="javascript:OpenExistingCustom();"/>
        <CommandUIHandler
          Command="CustomRibbonTab.CloseCustomCommand"
          CommandAction="javascript:CloseExistingCustom();"/>
        <CommandUIHandler
          Command ="CustomRibbonTab.CopyCustomCommand"
          CommandAction="Javascript:CopyCustom();"/>
      </CommandUIHandlers>
    </CommandUIExtension>
  </CustomAction>
</Elements>
```

**メモ**  埋め込み JavaScript の技法を使用してリボンのボタンのイベント処理を実装する場合は、 **CommandUIHandler** 要素で定義されているメソッドを JavaScript ファイルで実装する必要があります。たとえば、 **GetCurrentItem** や **AddNewCustom** などの関数を埋め込み JavaScript ファイルで実装する必要があります。

Default.aspx の **InitializeButton_Click** は、次のタスクを実行します。

1. **GetCustomActionXmlNode** を呼び出して XML ファイルを読み取り、RibbonCommands.xml で定義されている **CustomAction** オブジェクトを返します。 **CustomAction** オブジェクトには、リボンのカスタマイズ マークアップが含まれています。
    
2. **CustomAction** オブジェクトから、いくつかの要素および属性の値を読み取ります。
    
3. **CommandUIExtension** 要素 (リボンのグループ、ボタン、および UI イベント ハンドラーを定義している) を **xmlContent** という文字列に変換します。
    
4.  新しいカスタム アクションは、**clientContext.Web.UserCustomActions.Add** を使用して作成します。
    
5. リボンのカスタマイズ マークアップ (**xmlContent** に入っている) を、**CustomAction.CommandUIExtension** を使用して SharePoint サイトに追加します。
    
6. カスタム リボンを登録するために、**CustomAction.RegistrationId** と **CustomAction.RegistrationType** の属性値を手順 2 で読み取った **CustomAction** オブジェクトの値に設定します。

```C#
 protected void InitializeButton_Click(object sender, EventArgs e) {
            var spContext = SharePointContextProvider.Current.GetSharePointContext(Context);

            using (var clientContext = spContext.CreateUserClientContextForSPHost()) {
                clientContext.Load(clientContext.Web, web => web.UserCustomActions);
                clientContext.ExecuteQuery();

                // Get the XML elements file and get the CommandUIExtension node.
                var customActionNode = GetCustomActionXmlNode();
                var customActionName = customActionNode.Attribute("Id").Value;
                var commandUIExtensionNode = customActionNode.Element(ns + "CommandUIExtension");
                var xmlContent = commandUIExtensionNode.ToString();
                var location = customActionNode.Attribute("Location").Value;
                var registrationId = customActionNode.Attribute("RegistrationId").Value;
                var registrationTypeString = customActionNode.Attribute("RegistrationType").Value;
                var registrationType = (UserCustomActionRegistrationType)Enum.Parse(typeof(UserCustomActionRegistrationType), registrationTypeString);

                var sequence = 1000;
                if (customActionNode.Attribute(ns + "Sequence") != null) {
                    sequence = Convert.ToInt32(customActionNode.Attribute(ns + "Sequence").Value);
                }

                // Determine if the custom action exists already.
                var customAction = clientContext.Web.UserCustomActions.FirstOrDefault(uca => uca.Name == customActionName);

                // If the custom action does not exist, create it.
                if (customAction == null) {
                    // create the ribbon.
                    customAction = clientContext.Web.UserCustomActions.Add();
                    customAction.Name = customActionName;
                }

                // Set custom action properties.
                customAction.Location = location;
                customAction.CommandUIExtension = xmlContent; // CommandUIExtension xml
                customAction.RegistrationId = registrationId;
                customAction.RegistrationType = registrationType;
                customAction.Sequence = sequence;

                customAction.Update();
                clientContext.Load(customAction);
                clientContext.ExecuteQuery();
            }
        }
```

## その他のリソース
<a name="bk_addresources"> </a>

-  [Office 365 開発パターンとプラクティス (ソリューション ガイダンス)](Office-365-development-patterns-and-practices-solution-guidance.md)
    
-  [JavaScript を使用して SharePoint サイト UI をカスタマイズする](Customize-your-SharePoint-site-UI-by-using-JavaScript.md)
    
-  [SharePoint 2010 Server リボンのカスタマイズと展開](https://msdn.microsoft.com/library/office/gg552606%28v=office.14%29.aspx)
    
-  [Server リボン XML](https://msdn.microsoft.com/library/office/ff407290%28v=office.14%29.aspx)
    
