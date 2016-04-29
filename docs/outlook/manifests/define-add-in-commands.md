
# Outlook アドイン マニフェストでアドイン コマンドを定義する
Outlook アドインのマニフェストの  **VersionOverrides** 要素を使用して、アドイン コマンドを定義します。

 _ **適用対象:** apps for Office?| Office Add-ins?| Outlook_

アドイン コマンドをサポートするため、 **VersionOverrides** 要素内のアドイン マニフェスト v1.1 にいくつかの要素が追加されました。マニフェストに **VersionOverrides** 要素が含まれている場合、アドイン コマンドをサポートする Outlook のバージョンでは、その要素内の情報を使用してアドインが読み込まれます。アドイン コマンドをサポートしない以前のバージョンの Outlook では、「 [Outlook アドインのマニフェスト](../outlook/manifests/manifests.md)」で説明するように、その要素は無視されて引き続き古い要素が使用されます。

クライアント アプリケーションが  **VersionOverrides** ノードを認識する場合、アドインの名前はリボンに表示され、読み込み/作成ウィンドウには表示されません。これらの場所に、アドインは表示されません。


## VersionOverrides 要素

 **VersionOverrides** 要素は、アドインが実装するアドイン コマンドの情報を含むルート要素です。これはマニフェスト スキーマ v1.1 以上でサポートされますが、定義は VersionOverrides v1.0 スキーマでされています。 **VersionOverrides** の属性は次のとおりです。


|
|
|**属性**|**説明**|
|:-----|:-----|
|**xmlns**| 必ず指定します。スキーマの場所です。"http://schemas.microsoft.com/office/mailappversionoverrides" と指定する必要があります。|
|**xsi:type**|必ず指定します。スキーマのバージョンです。このトピックで説明するバージョンは "VersionOverridesV1_0" です。|
次の表は、 **VersionOverrides** の子要素を示しています。


|
|
|**要素**|**説明**|
|:-----|:-----|
|**Description**|アドインの説明です。マニフェストのすべての親部分の  **Description** 要素を上書きします。説明のテキストは、 **Resources** 要素の中の **LongString** 要素の子要素に含まれています。 **Description** 要素の **resid** 属性は、テキストが含まれる **String** 要素の **id** 属性の値に設定されます。|
|**Requirements**|Office アドインがアクティブ化する必要がある Office.js の最小の要件セットとバージョンを指定します。 [Outlook アドインのマニフェスト](../outlook/manifests/manifests.md) と同じように定義します。マニフェストの親部分にある **Requirements** 要素を上書きします。|
|**Hosts**|必ず指定します。ホストの種類とその設定のコレクションを指定します。マニフェストの親部分の  **Hosts** 要素を上書きします。 **xsi:type** 属性を "MailHost" に設定する必要があり、 **FormFactor** 子要素を含む必要があります。|
|**Resources**|マニフェストの他の要素によって参照されるリソースのコレクション (文字列、URL、画像) を定義します。これについては、このトピックの後半にある「 [リソース要素](#リソース要素)」セクションで説明します。|
 **VersionOverrides** とその子要素の例を次に示します。




```XML
<OfficeApp>
...
  <VersionOverrides xmlns="http://schemas.microsoft.com/office/mailappversionoverrides" xsi:type="VersionOverridesV1_0">
    <Description resid="residDescription" />
    <Requirements>
      <!-- add information on requirements -->
    </Requirements>
    <Hosts>
      <Host xsi:type="MailHost">
        <!-- add information on form factors -->
      </Host>
    </Hosts>
    <Resources> 
      <!-- add information on resources -->
   </Resources>
</VersionOverrides>
...
</OfficeApp>
```


## FormFactor 要素

 **FormFactor** 要素では、特定のフォーム ファクターのアドインの設定を指定します。 **Hosts** / **Host** 要素の子ノードです。現在はデスクトップ ( **DesktopFormFactor**) のみを指定できます。 **Resources** ノードを除くそのフォーム ファクターのアドイン情報をすべて含みます。

フォーム ファクターには、 **FunctionFile** 要素と 1 つ以上の **ExtensionPoint** 要素が含まれています。詳細については、この後の「 [FunctionFile 要素](#functionfile-要素)」セクションと「 [ExtensionPoint 要素](#extensionpoint-要素)」セクションをご覧ください。 **FormFactor** とその子ノードの例を次に示します。




```XML
<OfficeApp>
...
  <VersionOverrides xmlns="http://schemas.microsoft.com/office/mailappversionoverrides" xsi:type="VersionOverridesV1_0">
  ...
    <Hosts>
      <Host xsi:type="MailHost">
        <DesktopFormFactor>
          <FunctionFile resid="residDesktopFuncUrl" />
          <ExtensionPoint xsi:type="CustomPane">
            <!-- information on this extension point -->
          </ExtensionPoint> 

          <!-- possibly more ExtensionPoint elements -->

        </DesktopFormFactor>
      </Host>
    </Hosts>
  ...
  </VersionOverrides>
...
</OfficeApp>
```


## FunctionFile 要素


 **FunctionFile** 要素は、 **FormFactor** の子要素です。UI を表示するのではなく JavaScript 関数を実行するアドイン コマンドで公開する操作のソース コード ファイルを指定します。 **FunctionFile** 要素の **resid** 属性は、UI なしアドイン コマンド ボタンが使用するすべての JavaScript 関数が含まれるまたは読み込まれる HTML ファイルの URL を含む **Resources** 要素にある **Url** 要素の **id** 属性に設定されます。詳細については、この記事の「 [ボタン コントロール](#ボタン-コントロール)」セクションをご覧ください。

 **FunctionFile** 要素で示される HTML ファイルの JavaScript は、 `Office.initialize` を呼び出して、1 つのパラメーター ( `event`) を取る名前付きの関数を定義する必要があります。この関数は、 [item.notificationMessages](../reference/outlook/Office.context.mailbox.item.md%28Office.15%29.md) API を使用して、ユーザーに進行状況、成功、失敗を示す必要があります。また、実行が終了したときに[event.completed (JavaScript API for Office)](http://msdn.microsoft.com/library/89db707a-a522-4a71-a830-801d0106527b%28Office.15%29.aspx) を呼び出す必要もあります。関数の名前は、UI なしボタンの **FunctionName** 要素で使用されます。

trackMessage 関数を定義する HTML ファイルの例を次に示します。




```
Office.intialize = function () {
    doAuth();
}
function trackMessage (event) {
    var buttonId = event.source.id;    
    var itemId = Office.context.mailbox.item.id;
    // save this message
    event.completed();
}

```


## ExtensionPoint 要素


 **ExtensionPoint** 要素では、アドインが機能を公開する場所を定義します。この要素は **FormFactor** の子要素です。各フォーム ファクターで、開発者は次の **xsi:type** の値がある **ExtensionPoint** 要素を定義できます。


- CustomPane 
    
- MessageReadCommandSurface 
    
- MessageComposeCommandSurface 
    
- AppointmentOrganizerCommandSurface 
    
- AppointmentAttendeeCommandSurface
    

### CustomPane

 **CustomPane** 拡張点では、指定したルールを満たすときにアクティブ化されるアドインを定義します。これは閲覧フォーム専用で、水平方向のウィンドウに表示されます。 **CustomPane** の要素は次のとおりです。


|
|
|**要素**|**説明**|
|:-----|:-----|
|**RequestedHeight**| 省略可能。デスクトップ コンピューターで実行する場合に、表示ウィンドウに必要な高さをピクセル単位で指定します。32 ～ 450 ピクセルを指定できます。これは閲覧アドインと同じです (「[RequestedHeight 要素 (ItemReadTabletMailAppSettings complexType) (アプリ マニフェスト スキーマ v1.1)](http://msdn.microsoft.com/library/6296f5b0-3d5b-5ab9-eee9-55a7eb90f92c%28Office.15%29.aspx)」をご覧ください)。|
|**SourceLocation**|必ず指定します。アドインのソース コード ファイルの URL です。これは  **Resources** 要素の **Url** 要素を指します。|
|**Rule**|必ず指定します。アドインをアクティブ化するタイミングを指定する規則または規則のコレクションです。 [ItemIs](http://msdn.microsoft.com/ja-jp/library/f7dac4a3-1574-9671-1eda-47f092390669%28Office.15%29.aspx) 規則に、 **ItemType** は "Message" または "AppointmentAttendee" のいずれかである、および **FormType** 属性がないという変更点があることを除いて、 [Outlook アドインのマニフェスト](../outlook/manifests/manifests.md) での定義と同じです。詳細については、「 [カスタム ウィンドウの Outlook アドイン](../outlook/custom-pane-outlook-add-ins.md)」および「 [Outlook アドインのアクティブ化ルール](../outlook/manifests/activation-rules.md)」をご覧ください。|
|**DisableEntityHighlighting**|省略可能。このメール アドインでエンティティの強調表示をオフにするかどうかを指定します。 |
 次の例では、メッセージであるアイテム、添付ファイルがあるアイテム、またはアドレスが含まれたアイテム用のカスタム ウィンドウを定義しています。




```
<ExtensionPoint xsi:type="CustomPane">
   <RequestedHeight>100< /RequestedHeight> 
   <SourceLocation resid="residReadTaskpaneUrl"/>
   <Rule xsi:type="RuleCollection" Mode="Or">
     <Rule xsi:type="ItemIs" ItemType="Message"/>
     <Rule xsi:type="ItemHasAttachment"/>
     <Rule xsi:type="ItemHasKnownEntity" EntityType="Address"/>
   </Rule>
</ExtensionPoint>
```


### MessageReadCommandSurface

この拡張点により、メールの閲覧ビューのコマンド サーフェスにボタンが配置されます。Outlook デスクトップでは、これはリボンに表示されます。

開発者は、アドイン コマンドを使用するリボンのタブとグループを指定します。これは既定のタブ ([ **ホーム**]、[ **メッセージ**]、[ **会議**] のいずれか)、またはアドインで定義されたカスタム タブになります。既定のタブに追加する場合、追加できるのは、アドインごとに 1 つのグループのみです。カスタム タブでは、アドインは最大 10 個グループまで作成できます。各グループは、どのタブに表示されるかに関係なくコントロールの数は最大 6 個です。アドインは 1 つのカスタム タブに制限されています。

既定のリボンのタブにあるグループの例は、次のとおりです。




```XML
<ExtensionPoint xsi:type="MessageReadCommandSurface">
  <OfficeTab id="TabDefault">
    <Group id="msgreadTabMessage.grp1">
      <Label resid="residTemplateManagement" />
      <Control xsi:type="Button" id="Button1" >
       <!-- information on the control -->
      </Control>
       <!-- other controls, as needed -->
    </Group>
  </OfficeTab>
</ExtensionPoint>

```

ここで、


|
|
|**要素**|**説明**|
|:-----|:-----|
|**OfficeTab**|必ず指定します。使用する既存のタブです。現在、 **id** 属性には "TabDefault" のみを指定できます。|
|**Group**|タブ内のユーザー インターフェイスの拡張点のグループ。グループには最大 6 個のコントロールを指定できます。 **id** 属性は必ず指定します。最大 125 文字の文字列です。|
|**Label**|必ず指定します。グループのラベルです。 **resid** 属性には、 **Resources** 要素の **ShortStrings** 要素にある **String** 要素の **id** 属性の値を設定する必要があります。|
|**Control**|グループには、1 つ以上のコントロールが必要です。現在は、ボタンとメニューのみがサポートされています。詳細は、次の「 [ボタン コントロール](#ボタン-コントロール)」セクションと「[メニュー (ドロップダウン ボタン) コントロール](#メニュー-ドロップダウン-ボタン-コントロール)」セクションをご覧ください。|
次の例に示すように、開発者は  **CustomTab** 要素を使用して、リボンにカスタム タブを作成することもできます。




```XML
<ExtensionPoint xsi:type="MessageReadCommandSurface">
  <CustomTab id="TabCustom1">
    <Group id="msgreadCustomTab.grp1">
      <Label resid="residCustomTabGroupLabel"/>
      <Control xsi:type="Button" id="Button2">
        <!-- information on the control -->
      </Control>
      <!-- other controls, as needed -->
    </Group>
    <Label resid="customTabLabel1"/>
  </CustomTab>
</ExtensionPoint>
```

詳細は次のとおりです。


|
|
|**要素**|**説明**|
|:-----|:-----|
|**CustomTab**|必ず指定します。 **id** 属性はマニフェスト内で一意でなければなりません。|
|**Group**|タブ内のユーザー インターフェイスの拡張点のグループ。グループには最大 6 個のコントロールを指定できます。 **id** 属性は必ず指定します。最大 125 文字の文字列です。|
|**Label**|必ず指定します。グループのラベルです。 **resid** 属性には、 **Resources** 要素の **ShortStrings** 要素にある **String** 要素の **id** 属性の値を設定する必要があります。|
|**Control**|グループには、1 つ以上のコントロールが必要です。現在は、ボタンとメニューのみがサポートされています。詳細は、次の「 [ボタン コントロール](#ボタン-コントロール)」セクションと「[メニュー (ドロップダウン ボタン) コントロール](#メニュー-ドロップダウン-ボタン-コントロール)」セクションをご覧ください。|
|**Label**|必須。カスタム タブのラベルです。 **resid** 属性には、 **Resources** 要素の **ShortStrings** 要素にある **String** 要素の **id** 属性の値を設定する必要があります。|

#### ボタン コントロール


ボタンは、ユーザーによって選択された場合に 1 つの操作を実行します。関数を実行するか、作業ウィンドウを表示します。

ボタン コントロールは、次のようになります。




```
<Control xsi:type="Button" id="<choose a descriptive name>" >
  <!-- include button elements, as described in the following table -->
</Control>
```

ここで、 **id** 属性は、最大 125 文字の文字列です。ボタン要素は次の表で説明するとおりです。


|
|
|**ボタン要素**|**Description**|
|:-----|:-----|
|**Label**|必ず指定します。ボタンのテキストです。 **resid** 属性には、 **Resources** 要素の **ShortStrings** 要素にある **String** 要素の **id** 属性の値を設定する必要があります。|
|**Supertip**|必ず指定します。このボタンのヒントであり、次のものによって定義されます。
|||
|:-----|:-----|
|**Title**|必ず指定します。ヒントのテキストです。 **resid** 属性には、 **Resources** 要素の **ShortStrings** 要素にある **String** 要素の **id** の値を設定する必要があります。|
|**Description**|必ず指定します。ヒントの記述です。 **resid** 属性には、 **Resources** 要素の **LongStrings** 要素にある **String** 要素の **id** 属性の値を設定する必要があります。|
|
|**Icon**|必ず指定します。ボタンの  **Image** 要素が含まれます。
|||
|:-----|:-----|
|**Image**|ボタンの画像。 **resid** 属性には、 **Resources** 要素の **Images** 要素にある **Image** 要素の **id** 属性の値を設定する必要があります。 **size** 属性は、画像のサイズをピクセル単位で示します。他に 5 つのサイズ (20、24、40、48、64 ピクセル) がサポートされていますが、3 つの画像のサイズ (16、32、80 ピクセル) を必ず指定します。|
|
|**Action**|必ず指定します。ユーザーがボタンを選択したときに実行する操作を指定します。次のものによって定義されます。
|||
|:-----|:-----|
|**xsi:type**|この属性は、ユーザーがボタンをクリックしたときに実行される操作の種類を指定します。次のいずれかを指定できます。
<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>"ExecuteFunction"</p></li><li><p>"ShowTaskpane"</p></li></ul>|
|**FunctionName**|**xsi:type** が "ExecuteFunction" のときに必ず指定する要素です。実行する関数の名前を指定します。関数は、 **FunctionFile** 要素に指定されたファイルに含まれています。|
|**SourceLocation**|**xsi:type** が "ShowTaskpane" のときに必ず指定する要素です。このアクションのソース ファイルの場所を指定します。 **resid** 属性は、 **Resources** 要素の **Urls** 要素にある **Url** 要素の **id** 属性の値を指定します。|
|
 _UI なしボタン_ の例を次に示します。UI なしボタンは、 `getSubject` という名前の関数を実行します。




```
<Control xsi:type="Button" id="msgReadFunctionButton">
  <Label resid="funcReadButtonLabel" />
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

```

 _[作業ウィンドウ] ボタン_ コントロールは、作業ウィンドウを起動するボタンです。[作業ウィンドウ] ボタンでは、切り替えをサポートしていません。[作業ウィンドウ] ボタンの例を次に示します。




```
<Control xsi:type="Button" id="msgReadOpenPaneButton">
  <Label resid="paneReadButtonLabel" />
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

```


#### メニュー (ドロップダウン ボタン) コントロール


メニューは、静的なオプションの一覧を定義します。各メニュー項目は、関数を実行したり、作業ウィンドウを表示したりします。サブメニューはサポートされません。 

メニュー コントロールの構文は次のとおりです。




```
<Control xsi:type="Menu" id="<choose a descriptive name>" >
  <!-- include menu elements, as described in the following table -->
</Control>
```

ここで、 **id** 属性は、最大 125 文字の文字列です。メニュー要素は次の表に記載するとおりです。


|
|
|**メニュー要素**|**説明**|
|:-----|:-----|
|**Label**|必ず指定します。メニューのテキストです。 **resid** 属性には、 **Resources** 属性の **ShortStrings** 要素にある **String** 要素の **id** 属性の値を設定する必要があります。|
|**SuperTip**|必ず指定します。メニューのヒントであり、次のものによって定義されます。
|||
|:-----|:-----|
|**Title**|必ず指定します。ヒントのテキストです。 **resid** 属性には、 **Resources** 要素の **ShortStrings** 要素にある **String** 要素の **id** の値を設定する必要があります。|
|**Description**|必ず指定します。ヒントの記述です。 **resid** 属性には、 **Resources** 要素の **LongStrings** 要素にある **String** 要素の **id** 属性の値を設定する必要があります。|
|
|**Icon**|必ず指定します。メニューの  **Image** 要素が含まれます。
|||
|:-----|:-----|
|**Image**|メニューのの画像。 **resid** 属性には、 **Resources** 要素の **Images** 要素にある **Image** 要素の **id** 属性の値を設定する必要があります。 **size** 属性は、画像のサイズをピクセル単位で示します。他に 5 つのサイズ (20、24、40、48、64 ピクセル) がサポートされていますが、3 つの画像のサイズ (16、32、80 ピクセル) を必ず指定します。|
|
|**Items**|必ず指定します。メニューの  **Item** 要素が含まれます。各 **Item** 要素には [ボタン コントロール](#ボタン-コントロール)として同じ子要素が含まれています。|


メニューの例を次に示します。




```
<Control xsi:type="Menu" id="msgReadMenuButton">
  <Label resid="menuReadButtonLabel" />
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
  </Items>
</Control>

```


### MessageComposeCommandSurface

メールの新規作成フォームを使用してアドイン用のリボンにボタンを配置します。これは MessageReadCommandSurface と同じように定義されます。


### AppointmentOrganizerCommandSurface

会議の開催者に表示されるフォームのリボンにボタンを配置します。これは MessageReadCommandSurface と同じように定義されます。


### AppointmentAttendeeCommandSurface

会議の出席者に表示されるフォームのリボンにボタンを配置します。これは MessageReadCommandSurface と同じように定義されます。


## リソース要素


 **Resources** 要素には、 **VersionOverrides** ノードのアイコン、文字列、URL が含まれます。manifest 要素は、リソースの **Id** を使用してリソースを指定します。このことは、特にリソースに異なるロケールのバージョンがある場合に、マニフェストのサイズを管理できるようにしておくのに役立ちます。 **Id** には最大 32 文字を指定できます。

 **Resources** ノードでは、次のリソースを定義します。各リソースには 1 つ以上の **Override** 子要素を含めることができ、それによって特定のロケール用のリソースを定義することができます。


|
|
|**リソース**|**説明**|
|:-----|:-----|
|**Images**/ **Image**|アイコンの画像に HTTPS URL を指定します。各アイコンには 3 つの  **Image** 要素を指定する必要があり、それぞれに次の 3 つの必須サイズのいずれかを指定します。
<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>16x16</p></li><li><p>32x32</p></li><li><p>80x80</p></li></ul>上記の他に次のサイズもサポートされていますが、指定する必要はありません。
<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>20x20</p></li><li><p>24x24</p></li><li><p>40x40</p></li><li><p>48x48</p></li><li><p>64x64</p></li></ul>|
|**Urls**/ **Url**|HTTPS URL の場所を指定します。URL には最大 2048 文字まで指定できます。 |
|**ShortStrings**/ **String**|**Label** 要素と **Title** 要素のテキスト。各 **String** に含まれる文字数は最大 125 文字です。|
|**LongStrings**/ **String**|**Description** 属性のテキスト。各 **String** に含まれる文字数は最大 250 文字です。|

 >**メモ**  リソースを定義するときは、次の要件に留意してください。

 **Resources** 要素の例を次に示します。




```
<Resources>
  <bt:Images>
    <!-- Blue icon -->
    <bt:Image id="blue-icon-16" DefaultValue="YOUR_WEB_SERVER/images/blue-16.png"/>
    <bt:Image id="blue-icon-32" DefaultValue="YOUR_WEB_SERVER/images/blue-32.png"/>
    <bt:Image id="blue-icon-80" DefaultValue="YOUR_WEB_SERVER/images/blue-80.png"/>
  </bt:Images>
  <bt:Urls>
    <bt:Url id="functionFile" DefaultValue="YOUR_WEB_SERVER/FunctionFile/Functions.html"/>
    <!-- other URLs -->
  </bt:Urls>
  <bt:ShortStrings>
    <bt:String id="groupLabel" DefaultValue="Add-in Demo">
      <bt:Override Locale="ar-sa" Value="??? ??????? ????????" />
    </bt:String>
    <!-- Other short strings -->
  </bt:ShortStrings>
  <bt:LongStrings>
    <bt:String id="funcReadSuperTipDescription" DefaultValue="Gets the subject of the message or appointment.">
      <bt:Override Locale="ar-sa" Value="???? ??? ????? ??????? ?? ???????." />
    </bt:String>
    <!-- Other long strings -->
  </bt:LongStrings>
</Resources>

```


## ルールの変更点


次の変更は、マニフェストのルールに影響します。


- アクティブ化ルールは、各エントリ ポイント内に指定されるようになりました。
    
- [ItemIs](http://msdn.microsoft.com/ja-jp/library/f7dac4a3-1574-9671-1eda-47f092390669%28Office.15%29.aspx) は変更され、 **ItemType** が Message または AppointmentAttendee のいずれかとなり、 **FormType** 属性はなくなりました。
    
- [ItemHasKnownEntity](http://msdn.microsoft.com/library/87e10fd2-eab4-c8aa-bec3-dff92d004d39%28Office.15%29.aspx) は、列挙型ではなくエンティティの種類の文字列を受け付けるように変更されています。
    

## サンプル マニフェスト


完全なサンプル マニフェストについては、GitHub にある [コマンドデモ](https://github.com/jasonjoh/command-demo/blob/master/command-demo-manifest.xml%28Office.15%29.aspx)のサンプルをご覧ください。


## その他の技術情報



- [Outlook のアドイン コマンド](../outlook/add-in-commands-for-outlook.md)
    
- [Outlook アドインのマニフェスト](../outlook/manifests/manifests.md)
    
- [command-demo sample](https://github.com/jasonjoh/command-demo.aspx)
    
