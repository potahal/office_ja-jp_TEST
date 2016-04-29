
# Outlook アドインのマニフェスト
閲覧フォームまたは新規作成フォーム用の Outlook アドインのマニフェストを作成します。

 _ **適用対象:** apps for Office?| Office Add-ins?| Outlook_

Outlook アドインは、XML のアドイン マニフェストと Web ページの 2 つの構成要素から成り、それを Office アドイン用 JavaScript ライブラリ (office.js) が支えます。マニフェストには、アドインが Outlook クライアントと統合する方法が記述されます。現在、マニフェスト スキーマには、 **VersionOverrides** を含めた 3 つのバージョンがあります。現時点では、開発者はマニフェスト スキーマ バージョン 1.1 と **VersionOverrides** 1.0 を使用するコマンドで主にアドインを作成する必要があります。 以下に例を示します。

 >**メモ**  次のサンプルの URL 値はすべて「YOUR_WEB_SERVER」で始まります。この値はプレースホルダーであり、有効な実際のマニフェストでは、この部分には有効な HTTPS Web URL が入ります。




```XML
<?xml version="1.0" encoding="UTF-8"?>
<!--Created:cb85b80c-f585-40ff-8bfc-12ff4d0e34a9-->
<OfficeApp
  xmlns="http://schemas.microsoft.com/office/appforoffice/1.1"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:bt="http://schemas.microsoft.com/office/officeappbasictypes/1.0"
  xmlns:mailappor="http://schemas.microsoft.com/office/mailappversionoverrides/1.0"
  xsi:type="MailApp">
  <Id>7164e750-dc86-49c0-b548-1bac57abdc7c</Id>
  <Version>1.0.0.0</Version>
  <ProviderName>Microsoft Outlook Dev Center</ProviderName>
  <DefaultLocale>en-US</DefaultLocale>
  <DisplayName DefaultValue="Add-in Command Demo" />
  <Description DefaultValue="Adds command buttons to the ribbon in Outlook"/>
  <IconUrl DefaultValue="YOUR_WEB_SERVER/images/blue-64.png" />
  <HighResolutionIconUrl DefaultValue="YOUR_WEB_SERVER/images/blue-80.png" />
  <Hosts>
    <Host Name="Mailbox" />
  </Hosts>
  <Requirements>
    <Sets>
      <Set Name="MailBox" MinVersion="1.1" />
    </Sets>
  </Requirements>
  <!-- These elements support older clients that don't support add-in commands -->
  <FormSettings>
    <Form xsi:type="ItemRead">
      <DesktopSettings>
        <!-- NOTE: Just reusing the read taskpane page that is invoked by the button
             on the ribbon in clients that support add-in commands. You can 
             use a completely different page if desired -->
        <SourceLocation DefaultValue="YOUR_WEB_SERVER/AppRead/TaskPane/TaskPane.html"/>
        <RequestedHeight>450</RequestedHeight>
      </DesktopSettings>
    </Form>
    <Form xsi:type="ItemEdit">
      <DesktopSettings>
        <SourceLocation DefaultValue="YOUR_WEB_SERVER/AppCompose/Home/Home.html"/>
      </DesktopSettings>
    </Form>
  </FormSettings>
  <Permissions>ReadWriteItem</Permissions>
  <Rule xsi:type="RuleCollection" Mode="Or">
    <Rule xsi:type="ItemIs" ItemType="Message" FormType="Edit" />
    <Rule xsi:type="ItemIs" ItemType="Appointment" FormType="Edit" />
    <Rule xsi:type="ItemIs" ItemType="Message" FormType="Read" />
    <Rule xsi:type="ItemIs" ItemType="Appointment" FormType="Read" />
  </Rule>
  <DisableEntityHighlighting>false</DisableEntityHighlighting>

  <VersionOverrides xmlns="http://schemas.microsoft.com/office/mailappversionoverrides" xsi:type="VersionOverridesV1_0">

    <Requirements>
      <bt:Sets DefaultMinVersion="1.3">
        <bt:Set Name="Mailbox" />
      </bt:Sets>
    </Requirements>
    <Hosts>
      <Host xsi:type="MailHost">

        <DesktopFormFactor>
          <FunctionFile resid="functionFile" />
          
          <!-- Custom pane, only applies to read form -->
          <ExtensionPoint xsi:type="CustomPane">
            <RequestedHeight>100</RequestedHeight> 
            <SourceLocation resid="customPaneUrl"/>
            <Rule xsi:type="RuleCollection" Mode="Or">
              <Rule xsi:type="ItemIs" ItemType="Message"/>
              <Rule xsi:type="ItemIs" ItemType="AppointmentAttendee"/>
            </Rule>
          </ExtensionPoint>
          
          <!-- Message compose form -->
          <ExtensionPoint xsi:type="MessageComposeCommandSurface">
            <OfficeTab id="TabDefault">
              <Group id="msgComposeDemoGroup">
                <Label resid="groupLabel" />
                <Tooltip resid="groupTooltip" />
                <!-- Function (UI-less) button -->
                <Control xsi:type="Button" id="msgComposeFunctionButton">
                  <Label resid="funcComposeButtonLabel" />
                  <Tooltip resid="funcComposeButtonTooltip" />
                  <Supertip>
                    <Title resid="funcComposeSuperTipTitle" />
                    <Description resid="funcComposeSuperTipDescription" />
                  </Supertip>
                  <Icon>
                    <bt:Image size="16" resid="blue-icon-16" />
                    <bt:Image size="32" resid="blue-icon-32" />
                    <bt:Image size="80" resid="blue-icon-80" />
                  </Icon>
                  <Action xsi:type="ExecuteFunction">
                    <FunctionName>addDefaultMsgToBody</FunctionName>
                  </Action>
                </Control>
                <!-- Menu (dropdown) button -->
                <Control xsi:type="Menu" id="msgComposeMenuButton">
                  <Label resid="menuComposeButtonLabel" />
                  <Tooltip resid="menuComposeButtonTooltip" />
                  <Supertip>
                    <Title resid="menuComposeSuperTipTitle" />
                    <Description resid="menuComposeSuperTipDescription" />
                  </Supertip>
                  <Icon>
                    <bt:Image size="16" resid="red-icon-16" />
                    <bt:Image size="32" resid="red-icon-32" />
                    <bt:Image size="80" resid="red-icon-80" />
                  </Icon>
                  <Items>
                    <Item id="msgComposeMenuItem1">
                      <Label resid="menuItem1ComposeLabel" />
                      <Tooltip resid="menuItem1ComposeTip" />
                      <Supertip>
                        <Title resid="menuItem1ComposeLabel" />
                        <Description resid="menuItem1ComposeTip" />
                      </Supertip>
                      <Icon>
                        <bt:Image size="16" resid="red-icon-16" />
                        <bt:Image size="32" resid="red-icon-32" />
                        <bt:Image size="80" resid="red-icon-80" />
                      </Icon>
                      <Action xsi:type="ExecuteFunction">
                        <FunctionName>addMsg1ToBody</FunctionName>
                      </Action>
                    </Item>
                    <Item id="msgComposeMenuItem2">
                      <Label resid="menuItem2ComposeLabel" />
                      <Tooltip resid="menuItem2ComposeTip" />
                      <Supertip>
                        <Title resid="menuItem2ComposeLabel" />
                        <Description resid="menuItem2ComposeTip" />
                      </Supertip>
                      <Icon>
                        <bt:Image size="16" resid="red-icon-16" />
                        <bt:Image size="32" resid="red-icon-32" />
                        <bt:Image size="80" resid="red-icon-80" />
                      </Icon>
                      <Action xsi:type="ExecuteFunction">
                        <FunctionName>addMsg2ToBody</FunctionName>
                      </Action>
                    </Item>
                    <Item id="msgComposeMenuItem3">
                      <Label resid="menuItem3ComposeLabel" />
                      <Tooltip resid="menuItem3ComposeTip" />
                      <Supertip>
                        <Title resid="menuItem3ComposeLabel" />
                        <Description resid="menuItem3ComposeTip" />
                      </Supertip>
                      <Icon>
                        <bt:Image size="16" resid="red-icon-16" />
                        <bt:Image size="32" resid="red-icon-32" />
                        <bt:Image size="80" resid="red-icon-80" />
                      </Icon>
                      <Action xsi:type="ExecuteFunction">
                        <FunctionName>addMsg3ToBody</FunctionName>
                      </Action>
                    </Item>
                  </Items>
                </Control>
                <!-- Task pane button -->
                <Control xsi:type="Button" id="msgComposeOpenPaneButton">
                  <Label resid="paneComposeButtonLabel" />
                  <Tooltip resid="paneComposeButtonTooltip" />
                  <Supertip>
                    <Title resid="paneComposeSuperTipTitle" />
                    <Description resid="paneComposeSuperTipDescription" />
                  </Supertip>
                  <Icon>
                    <bt:Image size="16" resid="green-icon-16" />
                    <bt:Image size="32" resid="green-icon-32" />
                    <bt:Image size="80" resid="green-icon-80" />
                  </Icon>
                  <Action xsi:type="ShowTaskpane">
                    <SourceLocation resid="composeTaskPaneUrl" />
                  </Action>
                </Control>
              </Group>
            </OfficeTab>
          </ExtensionPoint>
          
          <!-- Appointment compose form -->
          <ExtensionPoint xsi:type="AppointmentOrganizerCommandSurface">
            <OfficeTab id="TabDefault">
              <Group id="apptComposeDemoGroup">
                <Label resid="groupLabel" />
                <Tooltip resid="groupTooltip" />
                <!-- Function (UI-less) button -->
                <Control xsi:type="Button" id="apptComposeFunctionButton">
                  <Label resid="funcComposeButtonLabel" />
                  <Tooltip resid="funcComposeButtonTooltip" />
                  <Supertip>
                    <Title resid="funcComposeSuperTipTitle" />
                    <Description resid="funcComposeSuperTipDescription" />
                  </Supertip>
                  <Icon>
                    <bt:Image size="16" resid="blue-icon-16" />
                    <bt:Image size="32" resid="blue-icon-32" />
                    <bt:Image size="80" resid="blue-icon-80" />
                  </Icon>
                  <Action xsi:type="ExecuteFunction">
                    <FunctionName>addDefaultMsgToBody</FunctionName>
                  </Action>
                </Control>
                <!-- Menu (dropdown) button -->
                <Control xsi:type="Menu" id="apptComposeMenuButton">
                  <Label resid="menuComposeButtonLabel" />
                  <Tooltip resid="menuComposeButtonTooltip" />
                  <Supertip>
                    <Title resid="menuComposeSuperTipTitle" />
                    <Description resid="menuComposeSuperTipDescription" />
                  </Supertip>
                  <Icon>
                    <bt:Image size="16" resid="red-icon-16" />
                    <bt:Image size="32" resid="red-icon-32" />
                    <bt:Image size="80" resid="red-icon-80" />
                  </Icon>
                  <Items>
                    <Item id="apptComposeMenuItem1">
                      <Label resid="menuItem1ComposeLabel" />
                      <Tooltip resid="menuItem1ComposeTip" />
                      <Supertip>
                        <Title resid="menuItem1ComposeLabel" />
                        <Description resid="menuItem1ComposeTip" />
                      </Supertip>
                      <Icon>
                        <bt:Image size="16" resid="red-icon-16" />
                        <bt:Image size="32" resid="red-icon-32" />
                        <bt:Image size="80" resid="red-icon-80" />
                      </Icon>
                      <Action xsi:type="ExecuteFunction">
                        <FunctionName>addMsg1ToBody</FunctionName>
                      </Action>
                    </Item>
                    <Item id="apptComposeMenuItem2">
                      <Label resid="menuItem2ComposeLabel" />
                      <Tooltip resid="menuItem2ComposeTip" />
                      <Supertip>
                        <Title resid="menuItem2ComposeLabel" />
                        <Description resid="menuItem2ComposeTip" />
                      </Supertip>
                      <Icon>
                        <bt:Image size="16" resid="red-icon-16" />
                        <bt:Image size="32" resid="red-icon-32" />
                        <bt:Image size="80" resid="red-icon-80" />
                      </Icon>
                      <Action xsi:type="ExecuteFunction">
                        <FunctionName>addMsg2ToBody</FunctionName>
                      </Action>
                    </Item>
                    <Item id="apptComposeMenuItem3">
                      <Label resid="menuItem3ComposeLabel" />
                      <Tooltip resid="menuItem3ComposeTip" />
                      <Supertip>
                        <Title resid="menuItem3ComposeLabel" />
                        <Description resid="menuItem3ComposeTip" />
                      </Supertip>
                      <Icon>
                        <bt:Image size="16" resid="red-icon-16" />
                        <bt:Image size="32" resid="red-icon-32" />
                        <bt:Image size="80" resid="red-icon-80" />
                      </Icon>
                      <Action xsi:type="ExecuteFunction">
                        <FunctionName>addMsg3ToBody</FunctionName>
                      </Action>
                    </Item>
                  </Items>
                </Control>
                <!-- Task pane button -->
                <Control xsi:type="Button" id="apptComposeOpenPaneButton">
                  <Label resid="paneComposeButtonLabel" />
                  <Tooltip resid="paneComposeButtonTooltip" />
                  <Supertip>
                    <Title resid="paneComposeSuperTipTitle" />
                    <Description resid="paneComposeSuperTipDescription" />
                  </Supertip>
                  <Icon>
                    <bt:Image size="16" resid="green-icon-16" />
                    <bt:Image size="32" resid="green-icon-32" />
                    <bt:Image size="80" resid="green-icon-80" />
                  </Icon>
                  <Action xsi:type="ShowTaskpane">
                    <SourceLocation resid="composeTaskPaneUrl" />
                  </Action>
                </Control>
              </Group>
            </OfficeTab>
          </ExtensionPoint>
          
          <!-- Message read form -->
          <ExtensionPoint xsi:type="MessageReadCommandSurface">
            <OfficeTab id="TabDefault">
              <Group id="msgReadDemoGroup">
                <Label resid="groupLabel" />
                <Tooltip resid="groupTooltip" />
                <!-- Function (UI-less) button -->
                <Control xsi:type="Button" id="msgReadFunctionButton">
                  <Label resid="funcReadButtonLabel" />
                  <Tooltip resid="funcReadButtonTooltip" />
                  <Supertip>
                    <Title resid="funcReadSuperTipTitle" />
                    <Description resid="funcReadSuperTipDescription" />
                  </Supertip>
                  <Icon>
                    <bt:Image size="16" resid="blue-icon-16" />
                    <bt:Image size="32" resid="blue-icon-32" />
                    <bt:Image size="80" resid="blue-icon-80" />
                  </Icon>
                  <Action xsi:type="ExecuteFunction">
                    <FunctionName>getSubject</FunctionName>
                  </Action>
                </Control>
                <!-- Menu (dropdown) button -->
                <Control xsi:type="Menu" id="msgReadMenuButton">
                  <Label resid="menuReadButtonLabel" />
                  <Tooltip resid="menuReadButtonTooltip" />
                  <Supertip>
                    <Title resid="menuReadSuperTipTitle" />
                    <Description resid="menuReadSuperTipDescription" />
                  </Supertip>
                  <Icon>
                    <bt:Image size="16" resid="red-icon-16" />
                    <bt:Image size="32" resid="red-icon-32" />
                    <bt:Image size="80" resid="red-icon-80" />
                  </Icon>
                  <Items>
                    <Item id="msgReadMenuItem1">
                      <Label resid="menuItem1ReadLabel" />
                      <Tooltip resid="menuItem1ReadTip" />
                      <Supertip>
                        <Title resid="menuItem1ReadLabel" />
                        <Description resid="menuItem1ReadTip" />
                      </Supertip>
                      <Icon>
                        <bt:Image size="16" resid="red-icon-16" />
                        <bt:Image size="32" resid="red-icon-32" />
                        <bt:Image size="80" resid="red-icon-80" />
                      </Icon>
                      <Action xsi:type="ExecuteFunction">
                        <FunctionName>getItemClass</FunctionName>
                      </Action>
                    </Item>
                    <Item id="msgReadMenuItem2">
                      <Label resid="menuItem2ReadLabel" />
                      <Tooltip resid="menuItem2ReadTip" />
                      <Supertip>
                        <Title resid="menuItem2ReadLabel" />
                        <Description resid="menuItem2ReadTip" />
                      </Supertip>
                      <Icon>
                        <bt:Image size="16" resid="red-icon-16" />
                        <bt:Image size="32" resid="red-icon-32" />
                        <bt:Image size="80" resid="red-icon-80" />
                      </Icon>
                      <Action xsi:type="ExecuteFunction">
                        <FunctionName>getDateTimeCreated</FunctionName>
                      </Action>
                    </Item>
                    <Item id="msgReadMenuItem3">
                      <Label resid="menuItem3ReadLabel" />
                      <Tooltip resid="menuItem3ReadTip" />
                      <Supertip>
                        <Title resid="menuItem3ReadLabel" />
                        <Description resid="menuItem3ReadTip" />
                      </Supertip>
                      <Icon>
                        <bt:Image size="16" resid="red-icon-16" />
                        <bt:Image size="32" resid="red-icon-32" />
                        <bt:Image size="80" resid="red-icon-80" />
                      </Icon>
                      <Action xsi:type="ExecuteFunction">
                        <FunctionName>getItemID</FunctionName>
                      </Action>
                    </Item>
                  </Items>
                </Control>
                <!-- Task pane button -->
                <Control xsi:type="Button" id="msgReadOpenPaneButton">
                  <Label resid="paneReadButtonLabel" />
                  <Tooltip resid="paneReadButtonTooltip" />
                  <Supertip>
                    <Title resid="paneReadSuperTipTitle" />
                    <Description resid="paneReadSuperTipDescription" />
                  </Supertip>
                  <Icon>
                    <bt:Image size="16" resid="green-icon-16" />
                    <bt:Image size="32" resid="green-icon-32" />
                    <bt:Image size="80" resid="green-icon-80" />
                  </Icon>
                  <Action xsi:type="ShowTaskpane">
                    <SourceLocation resid="readTaskPaneUrl" />
                  </Action>
                </Control>
              </Group>
            </OfficeTab>
          </ExtensionPoint>
          
          <!-- Appointment read form -->
          <ExtensionPoint xsi:type="AppointmentAttendeeCommandSurface">
            <OfficeTab id="TabDefault">
              <Group id="apptReadDemoGroup">
                <Label resid="groupLabel" />
                <Tooltip resid="groupTooltip" />
                <!-- Function (UI-less) button -->
                <Control xsi:type="Button" id="apptReadFunctionButton">
                  <Label resid="funcReadButtonLabel" />
                  <Tooltip resid="funcReadButtonTooltip" />
                  <Supertip>
                    <Title resid="funcReadSuperTipTitle" />
                    <Description resid="funcReadSuperTipDescription" />
                  </Supertip>
                  <Icon>
                    <bt:Image size="16" resid="blue-icon-16" />
                    <bt:Image size="32" resid="blue-icon-32" />
                    <bt:Image size="80" resid="blue-icon-80" />
                  </Icon>
                  <Action xsi:type="ExecuteFunction">
                    <FunctionName>getSubject</FunctionName>
                  </Action>
                </Control>
                <!-- Menu (dropdown) button -->
                <Control xsi:type="Menu" id="apptReadMenuButton">
                  <Label resid="menuReadButtonLabel" />
                  <Tooltip resid="menuReadButtonTooltip" />
                  <Supertip>
                    <Title resid="menuReadSuperTipTitle" />
                    <Description resid="menuReadSuperTipDescription" />
                  </Supertip>
                  <Icon>
                    <bt:Image size="16" resid="red-icon-16" />
                    <bt:Image size="32" resid="red-icon-32" />
                    <bt:Image size="80" resid="red-icon-80" />
                  </Icon>
                  <Items>
                    <Item id="apptReadMenuItem1">
                      <Label resid="menuItem1ReadLabel" />
                      <Tooltip resid="menuItem1ReadTip" />
                      <Supertip>
                        <Title resid="menuItem1ReadLabel" />
                        <Description resid="menuItem1ReadTip" />
                      </Supertip>
                      <Icon>
                        <bt:Image size="16" resid="red-icon-16" />
                        <bt:Image size="32" resid="red-icon-32" />
                        <bt:Image size="80" resid="red-icon-80" />
                      </Icon>
                      <Action xsi:type="ExecuteFunction">
                        <FunctionName>getItemClass</FunctionName>
                      </Action>
                    </Item>
                    <Item id="apptReadMenuItem2">
                      <Label resid="menuItem2ReadLabel" />
                      <Tooltip resid="menuItem2ReadTip" />
                      <Supertip>
                        <Title resid="menuItem2ReadLabel" />
                        <Description resid="menuItem2ReadTip" />
                      </Supertip>
                      <Icon>
                        <bt:Image size="16" resid="red-icon-16" />
                        <bt:Image size="32" resid="red-icon-32" />
                        <bt:Image size="80" resid="red-icon-80" />
                      </Icon>
                      <Action xsi:type="ExecuteFunction">
                        <FunctionName>getDateTimeCreated</FunctionName>
                      </Action>
                    </Item>
                    <Item id="apptReadMenuItem3">
                      <Label resid="menuItem3ReadLabel" />
                      <Tooltip resid="menuItem3ReadTip" />
                      <Supertip>
                        <Title resid="menuItem3ReadLabel" />
                        <Description resid="menuItem3ReadTip" />
                      </Supertip>
                      <Icon>
                        <bt:Image size="16" resid="red-icon-16" />
                        <bt:Image size="32" resid="red-icon-32" />
                        <bt:Image size="80" resid="red-icon-80" />
                      </Icon>
                      <Action xsi:type="ExecuteFunction">
                        <FunctionName>getItemID</FunctionName>
                      </Action>
                    </Item>
                  </Items>
                </Control>
                <!-- Task pane button -->
                <Control xsi:type="Button" id="apptReadOpenPaneButton">
                  <Label resid="paneReadButtonLabel" />
                  <Tooltip resid="paneReadButtonTooltip" />
                  <Supertip>
                    <Title resid="paneReadSuperTipTitle" />
                    <Description resid="paneReadSuperTipDescription" />
                  </Supertip>
                  <Icon>
                    <bt:Image size="16" resid="green-icon-16" />
                    <bt:Image size="32" resid="green-icon-32" />
                    <bt:Image size="80" resid="green-icon-80" />
                  </Icon>
                  <Action xsi:type="ShowTaskpane">
                    <SourceLocation resid="readTaskPaneUrl" />
                  </Action>
                </Control>
              </Group>
            </OfficeTab>
          </ExtensionPoint>

        </DesktopFormFactor>
      </Host>
    </Hosts>

    <Resources>
      <bt:Images>
        <!-- Blue icon -->
        <bt:Image id="blue-icon-16" DefaultValue="YOUR_WEB_SERVER/images/blue-16.png"/>
        <bt:Image id="blue-icon-32" DefaultValue="YOUR_WEB_SERVER/images/blue-32.png"/>
        <bt:Image id="blue-icon-80" DefaultValue="YOUR_WEB_SERVER/images/blue-80.png"/>
        <!-- Red icon -->
        <bt:Image id="red-icon-16" DefaultValue="YOUR_WEB_SERVER/images/red-16.png"/>
        <bt:Image id="red-icon-32" DefaultValue="YOUR_WEB_SERVER/images/red-32.png"/>
        <bt:Image id="red-icon-80" DefaultValue="YOUR_WEB_SERVER/images/red-80.png"/>
        <!-- Green icon -->
        <bt:Image id="green-icon-16" DefaultValue="YOUR_WEB_SERVER/images/green-16.png"/>
        <bt:Image id="green-icon-32" DefaultValue="YOUR_WEB_SERVER/images/green-32.png"/>
        <bt:Image id="green-icon-80" DefaultValue="YOUR_WEB_SERVER/images/green-80.png"/>
      </bt:Images>
      <bt:Urls>
        <bt:Url id="functionFile" DefaultValue="YOUR_WEB_SERVER/FunctionFile/Functions.html"/>
        <bt:Url id="readTaskPaneUrl" DefaultValue="YOUR_WEB_SERVER/AppRead/TaskPane/TaskPane.html"/>
        <bt:Url id="composeTaskPaneUrl" DefaultValue="YOUR_WEB_SERVER/AppCompose/TaskPane/TaskPane.html"/>
        <bt:Url id="customPaneUrl" DefaultValue="YOUR_WEB_SERVER/AppRead/CustomPane/CustomPane.html"/>"
      </bt:Urls>
      <bt:ShortStrings>
        <bt:String id="groupLabel" DefaultValue="Add-in Demo"/>
        <!-- Compose mode -->
        <bt:String id="funcComposeButtonLabel" DefaultValue="Insert default message"/>
        <bt:String id="menuComposeButtonLabel" DefaultValue="Insert message"/>
        <bt:String id="paneComposeButtonLabel" DefaultValue="Insert custom message"/>

        <bt:String id="funcComposeSuperTipTitle" DefaultValue="Inserts the default message"/>
        <bt:String id="menuComposeSuperTipTitle" DefaultValue="Choose a message to insert"/>
        <bt:String id="paneComposeSuperTipTitle" DefaultValue="Enter your own text to insert"/>
        
        <bt:String id="menuItem1ComposeLabel" DefaultValue="Hello World!"/>
        <bt:String id="menuItem2ComposeLabel" DefaultValue="Add-in commands are cool!"/>
        <bt:String id="menuItem3ComposeLabel" DefaultValue="Visit dev.outlook.com"/>

        <!-- Read mode -->
        <bt:String id="funcReadButtonLabel" DefaultValue="Get subject"/>
        <bt:String id="menuReadButtonLabel" DefaultValue="Get property"/>
        <bt:String id="paneReadButtonLabel" DefaultValue="Display all properties"/>

        <bt:String id="funcReadSuperTipTitle" DefaultValue="Gets the subject of the message or appointment"/>
        <bt:String id="menuReadSuperTipTitle" DefaultValue="Choose a property to get"/>
        <bt:String id="paneReadSuperTipTitle" DefaultValue="Get all properties"/>
        
        <bt:String id="menuItem1ReadLabel" DefaultValue="Get item class"/>
        <bt:String id="menuItem2ReadLabel" DefaultValue="Get date time created"/>
        <bt:String id="menuItem3ReadLabel" DefaultValue="Get item ID"/>
      </bt:ShortStrings>
      <bt:LongStrings>
        <bt:String id="groupTooltip" DefaultValue="Add-in Demo of the different command types"/>
        <!-- Compose mode -->
        <bt:String id="funcComposeButtonTooltip" DefaultValue="Inserts text into body of the message or appointment"/>
        <bt:String id="menuComposeButtonTooltip" DefaultValue="Inserts your choice of text into body of the message or appointment"/>
        <bt:String id="paneComposeButtonTooltip" DefaultValue="Opens a pane where you can enter text to insert in the body of the message or appointment"/>
        
        <bt:String id="funcComposeSuperTipDescription" DefaultValue="Inserts text into body of the message or appointment. This is an example of a function button."/>
        <bt:String id="menuComposeSuperTipDescription" DefaultValue="Inserts your choice of text into body of the message or appointment. This is an example of a drop-down menu button."/>
        <bt:String id="paneComposeSuperTipDescription" DefaultValue="Opens a pane where you can enter text to insert in the body of the message or appointment. This is an example of a button that opens a task pane."/>
        
        <bt:String id="menuItem1ComposeTip" DefaultValue="Inserts Hello World! into the body of the message or appointment." />
        <bt:String id="menuItem2ComposeTip" DefaultValue="Inserts Add-in commands are cool! into the body of the message or appointment." />
        <bt:String id="menuItem3ComposeTip" DefaultValue="Inserts Visit dev.outlook.com into the body of the message or appointment." />

        <!-- Read mode -->
        <bt:String id="funcReadButtonTooltip" DefaultValue="Gets the subject of the message or appointment and displays it in the info bar"/>
        <bt:String id="menuReadButtonTooltip" DefaultValue="Gets the selected property of the message or appointment and displays it in the info bar"/>
        <bt:String id="paneReadButtonTooltip" DefaultValue="Opens a pane displaying all available properties of the message or appointment"/>
        
        <bt:String id="funcReadSuperTipDescription" DefaultValue="Gets the subject of the message or appointment and displays it in the info bar. This is an example of a function button."/>
        <bt:String id="menuReadSuperTipDescription" DefaultValue="Gets the selected property of the message or appointment and displays it in the info bar. This is an example of a drop-down menu button."/>
        <bt:String id="paneReadSuperTipDescription" DefaultValue="Opens a pane displaying all available properties of the message or appointment. This is an example of a button that opens a task pane."/>
        
        <bt:String id="menuItem1ReadTip" DefaultValue="Gets the item class of the message or appointment and displays it in the info bar." />
        <bt:String id="menuItem2ReadTip" DefaultValue="Gets the date and time the message or appointment was created and displays it in the info bar." />
        <bt:String id="menuItem3ReadTip" DefaultValue="Gets the item ID of the message or appointment and displays it in the info bar." />
      </bt:LongStrings>
    </Resources>
  </VersionOverrides>
</OfficeApp>

```


## スキーマのバージョン

すべての Outlook クライアントが同時に最新機能をサポートするようになるとは限りません。Outlook ユーザーが古いバージョンの Outlook を保有している場合もあるためです。スキーマの複数のバージョンがあることで、開発者は、最新機能が使用可能な場合は最新機能を使用し、なおかつ古いバージョンでも機能する、下位互換性のあるアドインを作成できます。

マニフェストの  **VersionOverrides** 要素が、この一例です。 **VersionOverrides** 内で定義されたすべての要素は、マニフェストの他の部分にある同じ要素をオーバーライドします。つまり、Outlook は、可能な場合は常に、 **VersionOverrides** セクションにあるものを使用してアドインをセットアップします。ただし、Outlook のバージョンが特定のバージョンの **VersionOverrides** をサポートしていない場合、Outlook はこれを無視して、マニフェストの残りの部分の情報のみを使用します。

このアプローチでは、開発者は個別のマニフェストを複数作成する必要がなく、すべてを 1 つのファイルで定義することになります。

現在のスキーマのバージョンは次のとおりです。


|||
|:-----|:-----|
|バージョン|説明|
|v1.0|JavaScript API for Office バージョン 1.0 をサポートします。Outlook アドインであれば、閲覧フォームがサポートされることになります。|
|v1.1|JavaScript API for Office バージョン 1.1 と  **VersionOverrides** をサポートします。Outlook アドインで、新規作成フォームもサポートされることになります。|
|**VersionOverrides** 1.0|JavaScript API for Office の最新バージョンをサポートします。アドイン コマンドをサポートします。|
この記事では、1.1 マニフェストの要件を取り上げます。アドイン マニフェストで  **VersionOverrides** 要素を使用するとしても、 **VersionOverrides** をサポートしていない古いクライアントでもアドインが機能できるように 1.1 マニフェスト要素を組み込むことは重要です。


## ルート要素

Outlook アドイン マニフェストのルート要素は  **OfficeApp** です。この要素はまた、既定の名前空間、スキーマのバージョン、およびアドインの種類を宣言します。開始タグと終了タグの間にマニフェストのその他すべての要素を配置します。ルート要素の例を以下に示します。


```XML
<OfficeApp
  xmlns="http://schemas.microsoft.com/office/appforoffice/1.1"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:bt="http://schemas.microsoft.com/office/officeappbasictypes/1.0"
  xmlns:mailappor="http://schemas.microsoft.com/office/mailappversionoverrides/1.0"
  xsi:type="MailApp">

  <!-- the rest of the manifest>

</OfficeApp>
```


## バージョン

特定のアドインのバージョンです。開発者がマニフェスト内で何かを更新する場合、バージョンも同様に上げる必要があります。このように、新しいマニフェストをインストールすると、既存のマニフェストが上書きされ、ユーザーは新機能を取得します。このアドインがストアに送信済みのものであれば、新しいマニフェストが再度送信され検証される必要があります。その後、このアドインのユーザーは、新しく更新されたマニフェストが承認されてから数時間後に、このマニフェストを自動的に取得します。

アドインに必要なアクセス許可が変更された場合、アドインをアップグレードしてこれに再同意することを求めるメッセージがユーザーに表示されます。管理者が組織全体に対してこのアドインをインストールしていた場合は、管理者が最初に再同意をする必要があります。それまでの間、ユーザーには引き続き古い機能が表示されます。


## VersionOverrides

 **VersionOverrides** 要素は、アドイン コマンドの情報の場所です。詳細については、「 [Outlook アドイン マニフェストでアドイン コマンドを定義する](../outlook/manifests/define-add-in-commands.md)」をご覧ください。


## ローカライズ

名前、説明、および読み込む URL など、アドインのいくつかの側面は、各種のロケール用にローカライズする必要があります。これらの要素は、既定値を指定してから、 **VersionOverrides** 要素内の **Resources** 要素でロケールのオーバーライドを指定することによって簡単にローカライズできます。イメージ、URL、および文字列のオーバーライド方法を以下に示します。


```XML
<Resources>
    <bt:Images>
      <bt:Image id="icon1_16x16" DefaultValue="https://contoso.com/images/app_icon_small.png" >
        <bt:Override Locale="ar-sa" Value="https://contoso.com/images/app_icon_small_arsa.png" />
        <!-- add information for other locales -->

    <bt:Urls>
      <bt:Url id="residDesktopFuncUrl" DefaultValue="https://contoso.com/urls/page_appcmdcode.html" >
        <bt:Override Locale="ar-sa" Value="https://contoso.com/urls/page_appcmdcode.html?lcid=ar-sa" />
        <!-- add information for other locales -->

    <bt:ShortStrings> 
      </bt:String>
      <bt:String id="residViewTemplates" DefaultValue="Launch My Add-in">
        <bt:Override Locale="ar-sa" Value="????? ???? ??????? ????????" />
        <!-- add information for other locales -->
    </bt:ShortStrings>

  </Resources>
```

スキーマ リファレンスには、ローカライズできる要素に関する詳しい情報が含まれています。


## Hosts

Outlook アドインでは、次のように  **Hosts** 要素を指定します。


```XML
<OfficeApp>
...
  <Hosts>
    <Host Name="Mailbox" />
  </Hosts>
...
</OfficeApp>
```

これは、 **VersionOverrides** 要素内の **Hosts** 要素とは別個のものになります。この点については、「 [Outlook アドイン マニフェストでアドイン コマンドを定義する](../outlook/manifests/define-add-in-commands.md)」で説明されています。


## Requirements

 **Requirements** 要素は、アドインで使用できる API のセットを指定します。Outlook アドインでは、要件セットが Mailbox であり、値が 1.1 以上である必要があります。最新の要件セットのバージョンについては、API リファレンスをご覧ください。要件セットについて詳しくは、「 [Outlook アドインの API](../outlook/apis.md)」をご覧ください。

 **Requirements** 要素を **VersionOverrides** 要素に含めることも可能です。そうすることにより、 **VersionOverrides** をサポートするクライアントでアドインが読み込まれた場合用の、アドインの別の要件を指定できます。

次の例では、 **Sets** 要素の **DefaultMinVersion** 属性を使用して office.js バージョン 1.1 以降を要求し、 **Set** 要素の **MinVersion** 属性を使用してMailbox 要件セットのバージョン 1.1 を要求しています。




```XML
<OfficeApp>
...
  <Requirements>
    <Sets DefaultMinVersion="1.1">
      <Set Name="MailBox" MinVersion="1.1" />
    </Sets>
  </Requirements>
...
</OfficeApp>
```


## Form settings

 **FormSettings** 要素は、スキーマ 1.1 のみをサポートしていて **VersionOverrides** はサポートしていない、古い Outlook クライアントによって使用されます。開発者はこの要素を使用して、このようなクライアントでアドインがどのように表示されるかを定義します。 **ItemRead** と **ItemEdit** の 2 つの部分があります。 **ItemRead** は、ユーザーがメッセージと予定を読み込むときに、アドインがどのように表示されるかを指定するために使用します。 **ItemEdit** では、ユーザーが返信や新しいメッセージまたは予定を作成したり、(ユーザーが開催者の場合に) 予定を編集したりするときにアドインがどのように表示されるかについて記述します。

これらの設定は、 **Rule** 要素のアクティブ化ルールと直接関連します。アドインにおいてそのアドインが作成モードのメッセージ上に表示されるように指定する場合は、 **ItemEdit** フォームを指定する必要があります。

詳細は、「[Schema reference for Office Add-ins manifests (v1.1)](http://msdn.microsoft.com/library/7e0cadc3-f613-8eb9-7ef-9032cbb97f92.aspx)」をご覧ください。


## アプリ ドメイン

 **SourceLocation** 要素に指定するアドインの開始ページのドメインは、そのアドインの既定のドメインです。 **AppDomains** 要素と **AppDomain** 要素を使用しない場合、アドインが別のドメインに移動しようとすると、ブラウザーがそのアドイン ウィンドウの外に新しいウィンドウを開きます。アドインが、アドイン ウィンドウ内の別のドメインに移動できるようにするには、アドインのマニフェストに **AppDomains** 要素を追加し、その **AppDomain** サブ要素に各追加ドメインを含めます。

次のサンプルでは、アドインがアドイン ウィンドウ内で移動できる 2 番目のドメインとして  `https://www.contoso2.com` を指定しています。




```XML
<OfficeApp>
...
  <AppDomains>
    <AppDomain>https://www.contoso2.com</AppDomain>
  </AppDomains>
...
</OfficeApp>
```

アプリ ドメインは、ポップアップ ウィンドウと、リッチ クライアントで実行するアドインとの間での Cookie の共有を有効にするためにも必要です。


## アクセス許可

 **Permissions** 要素には、アドインに必要なアクセス許可を含めます。通常は、使用する予定の実際のメソッドに応じて、そのアドインに必要な必要最低限のアクセス許可を指定してください。たとえば、新規作成フォームでアクティブ化され、 [item.requiredAttendees](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.item.html%28Office.15%29.md) などのアイテム プロパティを読み取るだけで書き込みはせず、かつ [mailbox.makeEwsRequestAsync](http://dev.outlook.com/reference/add-ins/Office.context.mailbox.html%28Office.15%29.md) を呼び出して Exchange Web Services の操作にアクセスすることのないメール アドインでは、 **ReadItem** アクセス許可を指定する必要があります。利用できるアクセス許可について詳しくは、「 [ユーザーのメールボックスにアクセスする Outlook アドインのためのアクセス許可を指定する](../../docs/outlook/privacy/understanding-outlook-add-in-permissions.md)」をご覧ください。


**メール アドインの 4 層アクセス許可モデル**

![メール アプリ スキーマ v1.1 の 4 階層アクセス許可モデル](../../images/olowa15wecon_Permissions_4Tier.png)
```XML
<OfficeApp>
...
  <Permissions>ReadWriteItem</Permissions>
...
</OfficeApp>

```


## アクティブ化ルール

アクティブ化ルールは、 **Rule** 要素で指定します。 **Rule** 要素は、1.1 マニフェストの **OfficeApp** 要素の子として、さらには **VersionOverrides** の **ExtensionPoint** 要素の子として含めることができます。 **VersionOverrides** におけるこの要素の使用法について詳しくは、「 [Outlook アドイン マニフェストでアドイン コマンドを定義する](../outlook/manifests/define-add-in-commands.md)」をご覧ください。

アクティブ化ルールを使用すると、現在選択されているアイテムについての以下の 1 つ以上の条件に基づいてアドインをアクティブ化できます。


- アイテムの種類またはメッセージ クラス (あるいはその両方)
    
- 特定の種類の既知のリソース (住所または電話番号など) が存在すること
    
- 本文、件名、送信者の電子メール アドレスにおける正規表現の一致
    
- 添付ファイルが存在すること
    
アクティブ化ルールの詳細とサンプルについて詳しくは、「 [Outlook アドインのアクティブ化ルール](../outlook/manifests/activation-rules.md)」をご覧ください。


## 次の手順: アドイン コマンド


基本のマニフェストを定義したら、 [アドインのアドイン コマンドを定義](../outlook/manifests/define-add-in-commands.md)します。アドイン コマンドは、リボン内にボタンを表示して、ユーザーがアドインを簡単かつ直感的な方法でアクティブ化できるようにします。詳細は、「 [Outlook のアドイン コマンド](../outlook/add-in-commands-for-outlook.md)」をご覧ください。


## その他の技術情報



- [Outlook アドイン](../outlook/outlook-add-ins.md)
    
- [Outlook アドインのアクティブ化ルール](../outlook/manifests/activation-rules.md)
    
- [Office アドインのローカライズ](../../develop/localization.md)
    
- [デスクトップ、タブレット、モバイル デバイスで実行する Outlook 用メール アドインを作成する (スキーマ v1.1)](8d425fb3-8a7c-429d-87b3-8046e964b153.md)
    
- [Outlook アドインに関するプライバシー、アクセス許可、セキュリティ](../../docs/develop/privacy-and-security.md)
    
- [Outlook アドインの API](../outlook/apis.md)
    
- [Office アドインの XML マニフェスト](../../docs/overview/add-in-manifests.md)
    
- [Office アドインのマニフェスト向けのスキーマ リファレンス (v1.1)](http://msdn.microsoft.com/library/7e0cadc3-f613-8eb9-57ef-9032cbb97f92%28Office.15%29.aspx)
    
- [アイテムの種類とメッセージ クラス](http://msdn.microsoft.com/library/15b709cc-7486-b6c7-88a3-4a4d8e0ab292%28Office.15%29.aspx)
    
- [Office アドインの設計ガイドライン](../add-in-design.md)
    
- [ユーザーのメールボックスにアクセスする Outlook アドインのためのアクセス許可を指定する](../../docs/outlook/privacy/understanding-outlook-add-in-permissions.md)
    
- [正規表現アクティブ化ルールを使用して Outlook アドインを表示する](../outlook/use-regular-expressions-to-show-an-outlook-add-in.md)
    
- [Outlook アイテム内の文字列を既知のエンティティとして照合する](../outlook/match-strings-in-an-item-as-well-known-entities.md)
    
