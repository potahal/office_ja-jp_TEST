
# Excel、Word、および PowerPoint のマニフェストでアドイン コマンドを作成する (プレビュー)
マニフェストで  **VersionOverrides** を使用して、Excel、Word、および PowerPoint の アドイン コマンドを定義します。

 _ **適用対象:** apps for Office?| Excel 2016?| Office Add-ins?| PowerPoint 2016?| Word 2016_

アドイン コマンドは、操作を実行する指定された UI 要素を使用して、既定の Office UI をカスタマイズする簡単な方法を提供します。 アドイン コマンドを使用して、以下を実行できます。

- アドインの機能を簡単に使用できる UI 要素またはエントリ ポイントを作成します。
    
- ボタン、またはボタンのドロップダウンリストをリボンに追加します。
    
- それぞれがオプションのサブメニューを含む個々のメニュー項目を、特定のコンテキスト (ショートカット) メニューに追加します。
    
- アドイン コマンドが選択されると、操作を実行します。以下の操作を実行できます。
    
      - ユーザーが操作する 1 つ以上の作業ウィンドウ アドインを表示します。作業ウィンドウ アドイン内部で、Office の UI ファブリック を使用してカスタム UI を作成する HTML を表示できます。
    
     _または_
    
  - 通常はいずれの UI も表示しないで実行する JavaScript コードを実行します。
    
この記事では、アドイン コマンドを定義するマニフェストの編集方法について説明します。次の図に、アドイン コマンドを定義するのに使用される要素の階層を示します。これらの要素は、この記事で詳細に説明します。

![マニフェスト内のアドイン コマンド要素の概要](../../images/080da303-51c4-4882-b74a-7ba11517c0ad.png)


## 手順 1: サンプルから始める

「 [Office-Add-in-Commands-Samples](https://github.com/OfficeDev/Office-Add-in-Command-Sample)」にあるサンプルのいずれかから始めることを強くお勧め します。必要に応じて、このガイドの手順に従って独自のマニフェストを作成できます。「Office-Add-in-Commands-Samples」サイト内で XSD ファイルを使用してご使用のマニフェストを検証できます。アドイン コマンドを使用する前に、「[Excel、Word および PowerPoint のアドイン コマンド (プレビュー)](../../docs/design/add-in-commands-for-excel-and-word-preview.md)」をお読みください。


## 手順 2: 作業ウィンドウ アドインを作成する

アドイン コマンドを使用するには、最初に作業ウィンドウ アドインを作成してから、アドインのマニフェストをこの記事で説明するように変更する必要があります。コンテンツ アドインではアドイン コマンドを使用することはできません。既存のマニフェストを更新している場合、「 [手順 3: VersionOverrides 要素を追加する](#手順-3-versionoverrides-要素を追加する)」で説明するように、 **VersionOverrides** 要素をマニフェストに追加できます。

次の例は、Office 2013アドインのマニフェストを示します。 **VersionOverrides** 要素がないため、このマニフェストにはアドイン コマンドがありません。Office 2013 は、アドイン コマンドをサポートしませんが、 **VersionOverrides** をこのマニフェストに追加することで、アドインは Office 2013 と Office 2016 の両方で動作します。Office 2013 では、アドインはアドインコマンドを表示せず、 **SourceLocation** の値を使用して、アドインを単一の作業ウィンドウ アドインとして実行します。Office 2016 では、 **VersionOverrides** 要素が含まれない場合、アドインを実行するために **SourceLocation** が使用されます。しかし、 **VersionOverrides** を含める場合、アドインにアドイン コマンドのみが表示され、アドインが単一の作業ウィンドウ アドインとして表示されません。




```XML
<OfficeApp xmlns="http://schemas.microsoft.com/office/appforoffice/1.1" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:bt="http://schemas.microsoft.com/office/officeappbasictypes/1.0" xmlns:ov="http://schemas.microsoft.com/office/taskpaneappversionoverrides" xsi:type="TaskPaneApp">
  <Id>657a32a9-ab8a-4579-ac9f-df1a11a64e52</Id>
  <Version>1.0.0.0</Version>
  <ProviderName>Contoso</ProviderName>
  <DefaultLocale>en-US</DefaultLocale>
  <DisplayName DefaultValue="Contoso Add-in Commands" />
  <Description DefaultValue="Contoso Add-in Commands"/>
  <IconUrl DefaultValue="~remoteAppUrl/Images/Icon_32.png" />
 
  <AppDomains>
    <AppDomain>AppDomain1</AppDomain>
    <AppDomain>AppDomain2</AppDomain>
    <AppDomain>AppDomain3</AppDomain>
  </AppDomains>
  <Hosts>
    <Host Name="Workbook" />
  </Hosts>
  <DefaultSettings>
    <SourceLocation DefaultValue="https://www.contoso.com/Pages/Home.aspx" />
  </DefaultSettings>
  <Permissions>ReadWriteDocument</Permissions>

 <!-- The VersionOverrides element is inserted at this location in the manifest. -->

</OfficeApp>

```


## 手順 3: VersionOverrides 要素を追加する


 **VersionOverrides** 要素は、アドイン コマンドの定義を含むルート要素です。 **VersionOverrides** は、マニフェストの **OfficeApp** 要素の子要素です。次の表に、 **VersionOverrides** 要素の属性の一覧を示します。


|
|
|**属性**|**説明**|
|:-----|:-----|
|**xmlns**|必須。スキーマの場所。"http://schemas.microsoft.com/office/taskpaneappversionoverrides" でなければなりません。|
|**xsi:type**|必須。スキーマのバージョン。この記事で説明されているスキーマのバージョンは "VersionOverridesV1_0" です。|
次の表は、 **VersionOverrides** の子要素です。


|
|
|**要素**|**説明**|
|:-----|:-----|
|**Description**|省略可能。アドインの説明です。この子の  **Description** 要素は、マニフェストの親部分の **Description** 要素を上書きします。この **Description** 要素の **resid** 属性は、 **String** 要素の **id** に設定されます。 **String** 要素には、 **Description** のテキストが含まれます 。|
|**Requirements**|省略可能。アドインが必要とする Office.js の最小要件セットとバージョンを指定します。この子の  **Requirements** 要素は、マニフェストの親部分の **Requirements** 要素を上書きします。詳しくは、「 [Office のホストと API の要件を指定する](../../docs/overview/specify-office-hosts-and-api-requirements.md)」をご覧ください。|
|**Hosts**|必須。Office ホストのコレクションを指定します。子の  **Hosts** 要素は、マニフェストの親部分の **Hosts** 要素を上書きします。"Workbook" または "Document" に設定された **xsi:type** 属性を含める必要があります。|
|**Resources**|他のマニフェスト要素が参照するリソースのコレクション (文字列、URL、イメージ) を定義します。たとえば、 **Description** 要素の値は、 **Resources** の子要素を参照します。 **Resources** 要素については、この記事の「 [手順 7: Resources 要素を追加する](#手順-7-resources-要素を追加する)」で説明します。|
次の例で、 **VersionOverrides** 要素と子要素を使用する方法を示します。




```XML
<OfficeApp>
...
  <VersionOverrides xmlns="http://schemas.microsoft.com/office/taskpaneappversionoverrides" xsi:type="VersionOverridesV1_0">
    <Description resid="residDescription" />
    <Requirements>
      <!-- add information about requirement sets -->
    </Requirements>
    <Hosts>
      <Host xsi:type="Workbook">
        <!-- add information about form factors -->
      </Host>
      <Host xsi:type="Document">
        <!-- add information about form factors -->
      </Host>
    </Hosts>
    <Resources> 
      <!-- add information about resources -->
   </Resources>
</VersionOverrides>
...
</OfficeApp>
```


## 手順 4: Hosts、Host、DesktopFormFactor 要素を追加する


 **Hosts** 要素には、1 つ以上の **Host** 要素が含まれます。 **Host** 要素は、特定の Office ホストを指定します。 **Host** 要素には、アドイン が Office のホストにインストールされた後で表示するアドイン コマンドを指定する子要素が含まれます。同じアドイン コマンドを複数の異なる Office ホストで使用する場合、各 **Host** で子要素を重複ささせる必要があります。

 **DesktopFormFactor** 要素は、Windows デスクトップ上の Office で動作するアドインの設定と、Office Online (ブラウザー内) の設定を指定します。

以下は、 **Hosts** 、 **Host** 、 **DesktopFormFactor** 要素の例です。




```XML
<OfficeApp>
...
  <VersionOverrides xmlns="http://schemas.microsoft.com/office/taskpaneappversionoverrides" xsi:type="VersionOverridesV1_0">
  ...
    <Hosts>
      <Host xsi:type="Workbook">
        <DesktopFormFactor>

              <!-- information about FunctionFile and ExtensionPoint -->

        </DesktopFormFactor>
      </Host>
    </Hosts>
  ...
  </VersionOverrides>
...
</OfficeApp>
```


## 手順 5: FunctionFile 要素を追加する


 **FunctionFile** 要素は、アドイン コマンドが **ExecuteFunction** を使用するときに実行される JavaScript コードを含むファイルを指定します (詳しくは、「 [ボタン コントロール](#ボタン-コントロール)」をご覧ください)。 **FunctionFile** 要素の **resid** 属性は、アドイン コマンドが必要とするすべての JavaScript ファイルを含む HTML ファイルに設定されます。JavaScript ファイルに直接リンクすることはできません。 HTML ファイルのみにリンクできます。ファイル名は、 **Resources** 要素の **Url** 要素として指定されます。

 **FunctionFile** 要素の例を以下に示します。




```XML
<DesktopFormFactor>
          <FunctionFile resid="residDesktopFuncUrl" />
          <ExtensionPoint xsi:type="PrimaryCommandSurface">
            <!-- information about this extension point -->
          </ExtensionPoint> 

          <!-- You can define more than one ExtensionPoint element as needed -->

        </DesktopFormFactor>
```


 >**重要**  JavaScript コード が  `Office.initialize` を呼び出していることを確認します。

 **FunctionFile** 要素によって参照される HTML ファイルの JavaScript は、 `Office.initialize` を呼び出す必要があります。 **FunctionName** 要素 (詳しくは、「 [ボタン コントロール](#ボタン-コントロール)」をご覧ください) は、 **FunctionFile** の関数を使用します。

次のコードは、 **FunctionName** で使用される関数の実装方法を示しています。




```
<script>
        // The initialize function must be run each time a new page is loaded.
        (function () {
            Office.initialize = function (reason) {
               // If you need to initialize something you can do so here. 
            };
        })();

            // Your function must be in the global namespace.
        function writeText(event) {

            // Implement your custom code here. The following code is a simple example.  

            Office.context.document.setSelectedDataAsync("ExecuteFunction works. Button ID=" + event.source.id,
                function (asyncResult) {
                    var error = asyncResult.error;
                    if (asyncResult.status === "failed") {
                        // Show error message. 
                    }
                    else {
                        // Show success message.
                    }
                });
           // Calling event.completed is required. event.completed lets the platform know that processing has completed. 
	   event.completed();
        }

    </script>

```


 >**重要**   **event.completed** に対する呼び出しにより、イベントが正常に処理されたことが通知されます。同一のアドイン コマンドを複数回クリックするなど、関数を複数回呼び出すと、すべてのイベントが自動的にキューに入れられます。最初のイベントが自動的に実行され、その他のイベントはキューに残ります。関数により **event.completed** が呼び出されると、キューに入れられている、その関数に対する次の呼び出しが実行されます。 **event.completed** を実装する必要があります。実装しない場合、関数は実行されません。


## 手順 6: ExtensionPoint 要素を追加する


 **ExtensionPoint** 要素は、Office UI のどこにアドイン コマンドを表示するか定義します。次の **xsi:type** 値を使用して、 **ExtensionPoint** 要素を定義できます。


-  **PrimaryCommandSurface**。Office のリボンを参照します。
    
-  **ContextMenu**。Office UI で右クリックしたときに表示されるショートカット メニューです。
    
次の例は、 **PrimaryCommandSurface** と **ContextMenu** の属性値を持つ **ExtensionPoint** 要素を使用する方法と、各要素と併用する必要がある子要素を示しています。


 >**重要**  ID 属性を含む要素では、一意の ID を指定してください。会社の名前と ID を使用することをお勧めします。たとえば、次の形式にします。<CustomTab id=”mycompanyname.mygroupname”>




```XML
 <ExtensionPoint xsi:type="PrimaryCommandSurface">
            <CustomTab id="Contoso Tab">
            <!-- If you want to use a default tab that comes with Office, remove the above CustomTab element, and then uncomment the following OfficeTab element -->
             <!-- <OfficeTab id="TabData"> -->
              <Label resid="residLabel4" />
              <Group id="Group1Id12">
                <Label resid="residLabel4" />
                <Icon>
                  <bt:Image size="16" resid="icon1_32x32" />
                  <bt:Image size="32" resid="icon1_32x32" />
                  <bt:Image size="80" resid="icon1_32x32" />
                </Icon>
                <Tooltip resid="residToolTip" />
                <Control xsi:type="Button" id="Button1Id1">
                  
                   <!-- information about the control -->
                </Control>   
                <!-- other controls, as needed -->                                    
              </Group>
            </CustomTab>
          </ExtensionPoint>

        <ExtensionPoint xsi:type="ContextMenu">
          <OfficeMenu id="ContextMenuCell">
            <Control xsi:type="Menu" id="ContextMenu2">
                   <!-- information about the control -->
            </Control>   
           <!-- other controls, as needed -->         
          </OfficeMenu>
         </ExtensionPoint>
```


|
|
|**要素**|**説明**|
|:-----|:-----|
|**CustomTab**|カスタム タブをリボンに追加する場合は必須です (  **PrimaryCommandSurface** を使用)。 **CustomTab** 要素を使用する場合、 **OfficeTab** 要素は使用できません。 **id** 属性が必要です。|
|**OfficeTab**|既定の Office リボン タブを拡張する場合は必須です ( **PrimaryCommandSurface** を使用)。 **OfficeTab** 要素を使用する場合、 **CustomTab** 要素は使用できません。 **id** 属性と共に使用するその他のタブの値については、「 [既定の Office リボン タブの値](#既定の-office-リボン-タブの値)」をご覧ください。|
|**OfficeMenu**|アドイン コマンドを既定のコンテキスト メニューに追加する場合は必須です ( **ContextMenu** を使用)。 **id** 属性は、以下のように設定する必要があります。
<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>Excel や Word の場合は <span class="keyword">ContextMenuText</span>。テキストが選択され、ユーザーが選択されたテキストを右クリックしたときに、コンテキスト メニューに項目が表示されます。</p></li><li><p>Excel の <span class="keyword">ContextMenuCell</span>。ユーザーがスプレッドシートのセルを右クリックしたときに、コンテキスト メニューに項目が表示されます。</p></li></ul>|
|**Group**|タブのユーザー インターフェイスの拡張点のグループ。グループには、最大 6 個のコントロールを指定できます。 **id** 属性が必要です。id は最大 125 文字の文字列です。|
|**Label**|必須。グループのラベル。 **resid** 属性は、 **String** 要素の **id** 属性の値に設定する必要があります。 **String** 要素は、 **Resources** 要素の子要素である **ShortStrings** 要素の子要素です。|
|**Icon**|必須。小さいフォーム ファクターのデバイス、または表示されるボタンが多すぎるときに使用されるグループのアイコンを指定します。 **resid** 属性は、 **Image** 要素の **id** 属性の値に設定する必要があります。 **Image** 要素は、 **Resources** 要素の子要素である **Images** 要素の子要素です。 **size** 属性は、イメージのサイズをピクセル単位で指定します。3 つのイメージのサイズ (16、32、80) が必要です。5 つのオプションのサイズ (20、24、40、48、64) もサポートされています。|
|**Tooltip**|省略可能。グループのツールヒント。 **resid** 属性は、 **String** 要素の **id** 属性の値に設定する必要があります。 **String** 要素は、 **Resources** 要素の子要素である **LongStrings** 要素の子要素です。|
|**Control**|各グループには、1 つ以上のコントロールが必要です。 **Control** 要素は、 **Button** または **Menu** のいずれかにすることができます。ボタンのコントロールのドロップダウンリストを指定するには、 **Menu** を使用します。現在、ボタンとメニューのみがサポートされています。詳しくは、「 [ボタン コントロール](#ボタン-コントロール)」および「 [メニュー コントロール](#メニュー-コントロール)」のセクションをご覧ください。
 >**メモ**  トラブルシューティングを簡単にするために、 **Control** 要素と関連する **Resources** 子要素を一度に 1 つずつ追加することをお勧めします。

|

### ボタン コントロール


ボタンは、ユーザーが選択したときに 1 つの操作を実行します。JavaScript 関数を実行するか、作業ウィンドウを表示することができます。次の例は、2 つのボタンを定義する方法を示しています。最初のボタンは UI を表示しないで JavaScript 関数を実行し、2 つ目のボタンは作業ウィンドウを表示します。 **Control** 要素では、次のようになります。


-  **type** 属性は必須で、 **Button** に設定する必要があります。
    
-  **Control** 要素の **id** 属性は、最大 125 文字の文字列です。
    

```XML
        <!-- Define a control that calls a JavaScript function. -->

                 <Control xsi:type="Button" id="Button1Id1">
                  <Label resid="residLabel" />
                  <Tooltip resid="residToolTip" />
                  <Supertip>
                    <Title resid="residLabel" />
                    <Description resid="residToolTip" />
                  </Supertip>
                  <Icon>
                    <bt:Image size="16" resid="icon1_32x32" />
                    <bt:Image size="32" resid="icon1_32x32" />
                    <bt:Image size="80" resid="icon1_32x32" />
                  </Icon>
                  <Action xsi:type="ExecuteFunction">
                    <FunctionName>getData</FunctionName>
                  </Action>
                </Control>


                <!-- Define a control that shows a task pane. -->

                <Control xsi:type="Button" id="Button2Id1">
                  <Label resid="residLabel2" />
                  <Tooltip resid="residToolTip" />
                  <Supertip>
                    <Title resid="residLabel" />
                    <Description resid="residToolTip" />
                  </Supertip>
                  <Icon>
                    <bt:Image size="16" resid="icon2_32x32" />
                    <bt:Image size="32" resid="icon2_32x32" />
                    <bt:Image size="80" resid="icon2_32x32" />
                  </Icon>
                  <Action xsi:type="ShowTaskpane">
                    <SourceLocation resid="residUnitConverterUrl" />
                  </Action>
                </Control>
```


|
|
|**要素**|**説明**|
|:-----|:-----|
|**Label**|必須。ボタンのテキスト。 **resid** 属性は、 **String** 要素の **id** 属性の値に設定する必要があります。 **String** 要素は、 **Resources** 要素の子要素である **ShortStrings** 要素の子要素です。|
|**Tooltip**|省略可能。ボタンのツールヒント。 **resid** 属性は、 **String** 要素の **id** 属性の値に設定する必要があります。 **String** 要素は、 **Resources** 要素の子要素である **LongStrings** 要素の子要素です。|
|**Supertip**|必須。このボタンのヒントであり、次のものによって定義されます。
<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><span class="keyword">Title</span></p><p>必須。ヒントのテキスト。<b>resid</b>  属性は、<span class="keyword">String</span> 要素の <span class="keyword">id</span> 属性の値に設定する必要があります。<span class="keyword">String</span> 要素は、<b>Resources</b>  要素の子要素である <span class="keyword">ShortStrings</span> 要素の子要素です。</p></li><li><p><span class="keyword">Description</span></p><p>必須。ヒントの説明。<b>resid</b>  属性は、<span class="keyword">String</span> 要素の <span class="keyword">id</span> 属性の値に設定する必要があります。<span class="keyword">String</span> 要素は、<b>Resources</b>  要素の子要素である <span class="keyword">LongStrings</span> 要素の子要素です。</p></li></ul>|
|**Icon**|必須。ボタンの  **Image** 要素を含みます。画像ファイルは, .png 形式である必要があります。
<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><span class="keyword">Image</span></p><p>ボタンに表示する画像を定義します。<b>resid</b>  属性は、<span class="keyword">Image</span> 要素の <span class="keyword">id</span> 属性の値に設定する必要があります。<span class="keyword">Image</span> 要素は、<b>Resources</b>  要素の子要素である <span class="keyword">Images</span> 要素の子要素です。<span class="keyword">size</span> 属性は、イメージのサイズをピクセル単位で示します。3 つのイメージのサイズ (16、32、80) が必要です。5 つのオプションのサイズ (20、24、40、48、64) もサポートされています。</p></li></ul>|
|**Action**|必須。ユーザーがボタンを選択したときに実行するアクションを指定します。 **xsi:type** 属性の値は次のいずれかを指定することができます。
<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><span class="keyword">ExecuteFunction</span>。<span class="keyword">FunctionFile</span> によって参照されるファイルにある JavaScript 関数を実行します。<span class="keyword">ExecuteFunction</span> は、UI を表示しません。<span class="keyword">FunctionName</span> 子要素は、実行する関数の名前を指定します。</p></li><li><p><span class="keyword">ShowTaskPane</span>。作業ウィンドウ アドインを表示します。<span class="keyword">SourceLocation</span> 子要素は、表示する作業ウィンドウ アドイン ソース ファイルの位置を指定します。<b>resid</b>  属性は、<b>Resources</b>  要素の <span class="keyword">Urls</span> 要素の <span class="keyword">Url</span> 要素にある <span class="keyword">id</span> 属性の値に設定する必要があります。</p></li></ul>|

### メニュー コントロール


 **Menu** コントロールは、 **PrimaryCommandSurface** または **ContextMenu** のいずれかで使用でき、以下を定義します。


- ルート レベルのメニュー 項目。
    
- サブメニュー項目のリスト。
    
 **PrimaryCommandSurface** と共に使用すると、ルートのメニュー項目がリボンのボタンとして表示されます。 ボタンを選択すると、ドロップダウン リストとしてサブメニューが表示されます。 **ContextMenu** と共に使用すると、サブメニューを持つメニュー項目が、コンテキスト メニューに挿入されます。両方とも、各サブメニュー項目は、JavaScript 関数を実行するか、作業ウィンドウを表示することができます。現時点では、サブメニューの 1 つのレベルのみがサポートされます。

次の例では、2 つのサブメニュー項目を持つメニュー項目を定義する方法を示します。最初のサブメニュー項目は作業ウィンドウを表示し、2 つ目のサブメニュー項目は、JavaScript 関数を実行します。 **Control** 要素では、次のようになります。


-  **xsi:type** 属性は必須で、 **Menu** に設定する必要があります。
    
-  **id** 属性は、最大 125 文字の文字列です。
    



```
<Control xsi:type="Menu" id="TestMenu2">
              <Label resid="residLabel3" />
              <Tooltip resid="residToolTip" />
              <Supertip>
                <Title resid="residLabel" />
                <Description resid="residToolTip" />
              </Supertip>
              <Icon>
                <bt:Image size="16" resid="icon1_32x32" />
                <bt:Image size="32" resid="icon1_32x32" />
                <bt:Image size="80" resid="icon1_32x32" />
              </Icon>
              <Items>
                <Item id="showGallery2">
                  <Label resid="residLabel3"/>
                  <Supertip>
                    <Title resid="residLabel" />
                    <Description resid="residToolTip" />
                  </Supertip>
                  <Icon>
                    <bt:Image size="16" resid="icon1_32x32" />
                    <bt:Image size="32" resid="icon1_32x32" />
                    <bt:Image size="80" resid="icon1_32x32" />
                  </Icon>
                  <Action xsi:type="ShowTaskpane">
                    <TaskpaneId>MyTaskPaneID1</TaskpaneId>
                    <SourceLocation resid="residUnitConverterUrl" />
                  </Action>
                </Item>
              <Item id="showGallery3">
                  <Label resid="residLabel5"/>
                  <Supertip>
                    <Title resid="residLabel" />
                    <Description resid="residToolTip" />
                  </Supertip>
                  <Icon>
                    <bt:Image size="16" resid="icon4_32x32" />
                    <bt:Image size="32" resid="icon4_32x32" />
                    <bt:Image size="80" resid="icon4_32x32" />
                  </Icon>
                  <Action xsi:type="ExecuteFunction">
                    <FunctionName>getButton</FunctionName>
                  </Action>
                </Item>
              </Items>
            </Control>

```


|
|
|**要素**|**説明**|
|:-----|:-----|
|**Label**|必須。 ルートのメニュー項目のテキスト。 **resid** 属性は、 **String** 要素の **id** 属性の値に設定する必要があります。 **String** 要素は、 **Resources** 要素の子要素である **ShortStrings** 要素の子要素です。|
|**Tooltip**|省略可能。メニューのツールヒント。 **resid** 属性は、 **String** 要素の **id** 属性の値に設定する必要があります。 **String** 要素は、 **Resources** 要素の子要素である **LongStrings** 要素の子要素です。|
|**SuperTip**|必須。メニューのヒントであり、次のものによって定義されます。
<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><span class="keyword">Title</span></p><p>必須。ヒントのテキスト。<b>resid</b>  属性は、<span class="keyword">String</span> 要素の <span class="keyword">id</span> 属性の値に設定する必要があります。<span class="keyword">String</span> 要素は、<b>Resources</b>  要素の子要素である <span class="keyword">ShortStrings</span> 要素の子要素です。</p></li><li><p><span class="keyword">Description</span></p><p>必須。ヒントの説明。<b>resid</b>  属性は、<span class="keyword">String</span> 要素の <span class="keyword">id</span> 属性の値に設定する必要があります。<span class="keyword">String</span> 要素は、<b>Resources</b>  要素の子要素である <span class="keyword">LongStrings</span> 要素の子要素です。</p></li></ul>|
|**Icon**|必須。メニューの  **Image** 要素を含みます。画像ファイルは, .png 形式である必要があります。
<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><span class="keyword">Image</span></p><p>メニューの画像。<b>resid</b>  属性は、<span class="keyword">Image</span> 要素の <span class="keyword">id</span> 属性の値に設定する必要があります。<span class="keyword">Image</span> 要素は、<b>Resources</b>  要素の子要素である <span class="keyword">Images</span> 要素の子要素です。<span class="keyword">size</span> 属性は、イメージのサイズをピクセル単位で示します。3 つのイメージのサイズ (16、32、80) が必要です。5 つのオプションのサイズ (20、24、40、48、64) もサポートされています。</p></li></ul>|
|**Items**|必須。 各サブメニュー項目の **Item** 要素を含みます。各 **Item** 要素は、 [ボタン コントロール](#ボタン-コントロール)と同じ子要素を含みます。|

## 手順 7: Resources 要素を追加する


 **Resources** 要素は、 **VersionOverrides** 要素の異なる子要素で使用されるリソースを含みます。リソースには、アイコン、文字列、URL が含まれます。マニフェスト内の要素は、リソースの **id** を参照してリソースを使用できます。 **id** を使用すると、特に、異なるロケールのリソースの異なるバージョンがある場合、マニフェストを編成するのに役立ちます。 **id** は、最大 32 文字です。

次の表に、 **Resources** 要素を使用する方法の例を示します。各リソースは、特定のロケールに異なるリソースを定義する 1 つ以上の **Override** 子要素を持つことができます。




```XML
<Resources>
      <bt:Images>
        <bt:Image id="icon1_16x16" DefaultValue="https://www.contoso.com/Images/icon_default.png">
          <bt:Override Locale="ja-jp" Value="https://www.contoso.com/Images/ja-jp16-icon_default.png" />
        </bt:Image>
        <bt:Image id="icon1_32x32" DefaultValue="https://www.contoso.com/Images/icon_default.png">
          <bt:Override Locale="ja-jp" Value="https://www.contoso.com/Images/ja-jp32-icon_default.png" />
        </bt:Image>
        <bt:Image id="icon1_80x80" DefaultValue="https://www.contoso.com/Images/icon_default.png">
          <bt:Override Locale="ja-jp" Value="https://www.contoso.com/Images/ja-jp80-icon_default.png" />
        </bt:Image>        
      </bt:Images>
      <bt:Urls>
        <bt:Url id="residDesktopFuncUrl" DefaultValue="https://www.contoso.com/Pages/Home.aspx">
          <bt:Override Locale="ja-jp" Value="https://www.contoso.com/Pages/Home.aspx" />
        </bt:Url>        
      </bt:Urls>
      <bt:ShortStrings>
        <bt:String id="residLabel" DefaultValue="GetData">
          <bt:Override Locale="ja-jp" Value="JA-JP-GetData" />
        </bt:String>      
      </bt:ShortStrings>
      <bt:LongStrings>
        <bt:String id="residToolTip" DefaultValue="Get data for your document.">
          <bt:Override Locale="ja-jp" Value="JA-JP - Get data for your document." />
        </bt:String>
      </bt:LongStrings>
    </Resources>
```


|
|
|**リソース**|**説明**|
|:-----|:-----|
|**Images**/ **Image**|イメージ ファイルへの HTTPS URL を指定します。各イメージは、次の 3 つの必須のイメージ サイズを定義する必要があります。
<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>16×16</p></li><li><p>32×32</p></li><li><p>80×80</p></li></ul>次のイメージ サイズもサポートされますが、必須ではありません。
<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>20×20</p></li><li><p>24×24</p></li><li><p>40×40</p></li><li><p>48×48</p></li><li><p>64×64</p></li></ul>|
|**Urls**/ **Url**|HTTPS URL の場所を指定します。URL には最大 2048 文字まで指定できます。|
|**ShortStrings**/ **String**|**Label** 要素と **Title** 要素のテキスト。各 **String** は最大 125 文字です。|
|**LongStrings**/ **String**|**Tooltip** と **Description** 要素のテキスト。各 **String** は最大 250 文字です。|



 >**メモ**   **Image** 要素と **Url** 要素のすべての URL で Secure Sockets Layer (SSL) を使用する必要があります。


## 既定の Office リボン タブの値


Excel および Word で、既定の Office UI タブを使用することで、リボンにアドイン コマンドを追加できます。次の表に、 **OfficeTab** 要素の **id** 属性で使用できる値を示します。タブの値は大文字と小文字を区別します。



|**Office ホスト アプリケーション**|**タブの値**|
|:-----|:-----|
|Excel|**TabHome** **TabInsert** **TabPageLayoutExcel** **TabFormulas** **TabData** **TabReview** **TabView** **TabDeveloper** **TabAddIns** **TabPrintPreview** **TabBackgroundRemoval**|
|Word|**TabHome** **TabInsert** **TabWordDesign** **TabPageLayoutWord** **TabReferences** **TabMailings** **TabReviewWord** **TabView** **TabDeveloper** **TabAddIns** **TabBlogPost** **TabBlogInsert** **TabPrintPreview** **TabOutlining** **TabConflicts** **TabBackgroundRemoval** **TabBroadcastPresentation**|
|PowerPoint|**TabHome** **TabInsert** **TabDesign** **TabTransitions** **TabAnimations** **TabSlideShow** **TabReview** **TabView** **TabDeveloper** **TabAddIns** **TabPrintPreview** **TabMerge** **TabGrayscale** **TabBlackAndWhite** **TabBroadcastPresentation** **TabSlideMaster** **TabHandoutMaster** **TabNotesMaster** **TabBackgroundRemoval** **TabSlideMasterHome**|

## その他の技術情報



- [Excel、Word および PowerPoint のアドイン コマンド (プレビュー)](../../docs/design/add-in-commands-for-excel-and-word-preview.md)
    
- [Outlook アドイン マニフェストでアドイン コマンドを定義する](../outlook/manifests/define-add-in-commands.md)
    
