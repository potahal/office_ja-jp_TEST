
# Outlook アドインのアドイン メタデータの取得と設定
ローミング設定およびカスタム プロパティを使用することにより、Outlook アドインでのみアクセス可能なカスタム データの読み込みと保存を行う方法について説明します。

 _ **適用対象:** apps for Office?| Office Add-ins?| Outlook_

Outlook アドインでカスタム データを管理する方法には、次の 2 つの方法があります。

- ユーザーのメールボックスのカスタム データを管理するローミング設定。
    
- ユーザーのメールボックス内のアイテムのカスタム データを管理するカスタム プロパティ。
    
これらの方法の両方とも Outlook アドインでのみアクセス可能なカスタム データに対するアクセスを提供しますが、各方法は他方の方法と異なる方法でデータを保存します。つまり、ローミング設定によって保存されたデータはカスタム プロパティではアクセスできず、その逆もまた同様です。データは対象のメールボックスのサーバー に保存され、アドインでサポートされるすべてのフォーム ファクターのその後の Outlook セッションでアクセスできます。 

## メールボックスごとのカスタム データ: ローミング設定


[RoamingSettings](../reference/outlook/RoamingSettings.md%28Office.15%29.md) オブジェクトを使用して、ユーザーの Exchange メールボックスに固有のデータを指定できます。このタイプのデータには、たとえばユーザーの個人データや基本設定があります。メール アドインは、その実行を許可されているデバイス (デスクトップ、タブレット、またはスマートフォン) でローミングするときにローミング設定にアクセスできます。

 このデータへの変更は、現在の Outlook セッションの設定値のメモリ内コピーに格納されます。更新後にすべてのローミング設定値を明示的に保存して、ユーザーが次にアドインを同じデバイスで開いても、サポートされている他のデバイスで開いても、その設定値を使用できるようにしてください。


### ローミング設定の形式


 **RoamingSettings** オブジェクトのデータは、シリアル化された JavaScript Object Notation (JSON) 文字列として保存されています。次に示すのはこの構造の例です。 `add-in_setting_name_0`、 `add-in_setting_name_1`、および  `add-in_setting_name_2` という 3 つのローミング設定が定義されていると想定しています。


```
{
  "add-in_setting_name_0":"add-in_setting_value_0",
  "add-in_setting_name_1":"add-in_setting_value_1",
  "add-in_setting_name_2":"add-in_setting_value_2"
}
```


### ローミング設定の読み込み


通常、メール アドインでは、 [Office.initialize](http://msdn.microsoft.com/ja-jp/library/727adf79-a0b5-48d2-99c7-6642c2c334fc%28Office.15%29.aspx) イベント ハンドラーでローミング設定を読み込みます。次の JavaScript コードは、既存のローミング設定を読み込み、2 つの設定 "customerName" と "customerBalance" の値を取得する例を示しています。


```
var _mailbox;
var _settings;
var _customerName;
var _customerBalance;

// The initialize function is required for all add-ins.
Office.initialize = function () {
  // Initialize instance variables to access API objects.
  _mailbox = Office.context.mailbox;
  _settings = Office.context.roamingSettings;
  _customerName = _settings.get("customerName");
  _customerBalance = _settings.get("customerBalance");
}

```


### ローミング設定の作成または割り当て


前の例の続きで、次の JavaScript 関数  `setAddInSetting` は、 [RoamingSettings.set](../reference/outlook/RoamingSettings.html%28Office.15%29.md) メソッドを使用して `cookie` という名前の設定に今日の日付を設定し、 [RoamingSettings.saveAsync](https://dev.outlook.com/reference/add-ins/RoamingSettings.html%28Office.15%29.md) メソッドを使用してすべてのローミング設定をサーバーに保存することによってデータを保存します。この **set** メソッドは、指定した設定が存在しない場合は新規に作成し、指定した値を割り当てます。 **saveAsync** メソッドは非同期でローミング設定を保存します。このコード サンプルでは、 **saveAsync** にコールバック メソッド `saveMyAddInSettingsCallback` を渡しています。非同期の呼び出しが完了すると、 `saveMyAddInSettingsCallback` が呼び出され、単一のパラメーター _asyncResult_ が渡されます。このパラメーターは [AsyncResult](https://dev.outlook.com/reference/add-ins/simple-types.md%28Office.15%29.md) オブジェクトであり、非同期呼び出しについての結果と詳細情報が格納されています。オプションの _userContext_ パラメーターを使用すると、非同期呼び出しからコールバック関数に任意の状態の情報を渡すことができます。


```
// Set a roaming setting.
function setAddInSetting() {
  _settings.set("cookie", Date());
  // Save roaming settings for the mailbox
  // to the server so that they will be available
  // in the next session.
  _settings.saveAsync(saveMyAddInSettingsCallback);
}

// Callback method after saving custom roaming settings.
function saveMyAddInSettingsCallback(asyncResult) {
  if (asyncResult.status == Office.AsyncResultStatus.Failed) {
    // Handle the failure.
  }
}
```


### ローミング設定の削除


さらに、前の例の続きで、次の JavaScript 関数  `removeAddInSetting` では、 [RoamingSettings.remove](../reference/outlook/RoamingSettings.md%28Office.15%29.md) メソッドを使用して `cookie` 設定を削除し、すべてのローミング設定を Exchange Server に戻して保存する方法を示します。


```
// Remove an add-in setting.
function removeAddInSetting()
{
  _settings.remove("cookie");
  // Save changes to the roaming settings for the mailbox
  // to the server so that they will be available
  // in the next session.
  _settings.saveAsync(saveMyAddInSettingsCallback);
}
```


## メールボックス内のアイテムごとのカスタム データ: カスタム プロパティ


[CustomProperties](../reference/outlook/CustomProperties.md%28Office.15%29.md) オブジェクトを使用して、ユーザーのメールボックス内のアイテムに固有のデータを指定することもできます。たとえば、メール アドインで特定のメッセージを分類し、カスタム プロパティ `messageCategory` を使用してそのカテゴリのメモを付けることができます。または、メール アドインでメッセージ内の会議の提案から予定を作成した場合に、カスタム プロパティを使用してそれぞれの予定を追跡できます。これにより、ユーザーが再度そのメッセージを開いた場合に、メール アドインによってもう一度予定を作成するように提案されることはありません。

ローミング設定と同様に、カスタム プロパティに対する変更は現在の Outlook セッションのプロパティのメモリ内コピーに格納されます。これらのカスタム プロパティが次のセッションで使用できるようにするには、すべてのカスタム プロパティをサーバーに保存します。

これらアドイン固有およびアイテム固有のカスタム プロパティは、 **CustomProperties** オブジェクトを使用することによってのみアクセスできます。これらのプロパティは、Outlook オブジェクト モデル内のカスタムの MAPI ベースの [UserProperties](http://msdn.microsoft.com/library/20b49c86-d74f-9bda-382c-559af278c148%28Office.15%29.aspx)、およびExchange Web サービス (EWS) の拡張プロパティとは異なります。Outlook オブジェクト モデルまたは EWS を使用して  **CustomProperties** にアクセスすることはできません。

ただし、メール アドインは EWS [GetItem](http://msdn.microsoft.com/en-us/library/e3590b8b-c2a7-4dad-a014-6360197b68e4%28Office.15%29.aspx) 操作を使用して、MAPI ベースの拡張プロパティを取得できます。サーバー側でコールバック トークンを使用して、またはクライアント側で [mailbox.makeEwsRequestAsync](../reference/outlook/Office.context.mailbox.md%28Office.15%29.md) メソッドを使用して **GetItem** にアクセスします。 **GetItem** 要求では、必要なカスタムの拡張プロパティをプロパティ セットに指定します。メール アドインでは、拡張プロパティを作成したり変更するのに **makeEwsRequestAsync** および EWS [CreateItem](http://msdn.microsoft.com/library/78a52120-f1d0-4ed7-8748-436e554f75b6%28Office.15%29.aspx) と [UpdateItem](http://msdn.microsoft.com/library/5d027523-e0bc-4da2-b60b-0cb9fc1fdfe4%28Office.15%29.aspx) 操作を使用することもできます。

MAPI プロパティ、Outlook の  **UserProperties**、EWS の拡張プロパティ、コールバック トークン機構および  **makeEwsRequestAsync** の使用方法については、「 [その他のリソース](30217d63-7615-4f3f-8618-c91e4e60cd43.md#MailAppCustomProperties_AdditionalResources)」セクションを参照してください。


### カスタム プロパティの使用


カスタム プロパティを使用するには、 [loadCustomPropertiesAsync](../reference/outlook/Office.context.mailbox.item.html%28Office.15%29.md) メソッドを呼び出してプロパティ バッグを読み込む必要があります。現在のアイテムに対してカスタム プロパティが既に設定されている場合は、この時点で、Exchange Server から読み込まれます。プロパティ バッグを作成した後、 [set](https://dev.outlook.com/reference/add-ins/CustomProperties.html%28Office.15%29.md) メソッドと [get](https://dev.outlook.com/reference/add-ins/CustomProperties.html%28Office.15%29.md) メソッドを使用して、カスタム プロパティを追加および取得できます。プロパティ バッグに対して行った変更を保存するには、 [saveAsync](https://dev.outlook.com/reference/add-ins/CustomProperties.md%28Office.15%29.md) メソッドを使用して、Exchange Server に変更を保存する必要があります。


 >**メモ**  Outlook for Mac はカスタム プロパティをキャッシュに入れないため、ユーザーのネットワークがダウンした場合、Outlook for Mac のメール アドインはカスタム プロパティにアクセスできなくなります。


### カスタム プロパティの例


以下の例では、カスタム プロパティを使用する単純な Outlook アドインのメソッドのセットを示しています。この例を出発点として、カスタム プロパティを使用するアドインを作成できます。 

以下の例には、次のメソッドが含まれています。


- [Office.initialize](http://msdn.microsoft.com/ja-jp/library/727adf79-a0b5-48d2-99c7-6642c2c334fc%28Office.15%29.aspx) ? アドインを初期化し、Exchange Server からカスタム プロパティ バッグを読み込みます。
    
-  **customPropsCallback** ? サーバーから返されるカスタム プロパティ バッグを取得し、後で使用できるように保存します。
    
-  **updateProperty** ? 特定のプロパティを設定または更新し、変更をサーバーに保存します。
    
-  **removeProperty** ? プロパティ バッグから特定のプロパティを削除し、その削除をサーバーに保存します。
    



```
var _mailbox;
var _customProps;

// The initialize function is required for all add-ins.
Office.initialize = function () {
  _mailbox = Office.context.mailbox;
  _mailbox.item.loadCustomPropertiesAsync(customPropsCallback);
}

// Callback function from loading custom properties.
function customPropsCallback(asyncResult) {
  if (asyncResult.status == Office.AsyncResultStatus.Failed) {
    // Handle the failure.
  }
  else {
    // Successfully loaded custom properties,
    // can get them from the asyncResult argument.
    _customProps = asyncResult.value;
  }
}

// Get individual custom property.
function getProperty() {
  var myProp = customProps.get("myProp");
}

// Set individual custom property.
function updateProperty(name, value) {
  _customProps.set(name, value);
  // Save all custom properties to server.
  _customProps.saveAsync(saveCallback);
}

// Remove a custom property.
function removeProperty(name) {
  _customProps.remove(name);
  // Save all custom properties to server.
  _customProps.saveAsync(saveCallback);
}

// Callback function from saving custom properties.
function saveCallback() {
  if (asyncResult.status == Office.AsyncResultStatus.Failed) {
    // Handle the failure.
  }
}
```


## その他のリソース



- [Outlook アドイン](../outlook/outlook-add-ins.md)
    
- [Outlook アドインのアーキテクチャと機能の概要](../outlook/overview.md)
    
- [MAPI Property Overview](http://msdn.microsoft.com/library/02e5b23f-1bdb-4fbf-a27d-e3301a359573%28Office.15%29.aspx)
    
- [Outlook のプロパティの概要](http://msdn.microsoft.com/library/242c9e89-a0c5-ff89-0d2a-410bd42a3461%28Office.15%29.aspx)
    
- [Outlook アドインから Web サービスを呼び出す](../outlook/web-services.md)
    
- [プロパティと交換に EWS での拡張プロパティ](http://msdn.microsoft.com/library/68623048-060e-4602-b3fa-62617a94cf72%28Office.15%29.aspx)
    
- [プロパティ セットと応答、図形 EWS Exchange](http://msdn.microsoft.com/library/04a29804-6067-48e7-9f5c-534e253a230e%28Office.15%29.aspx)
    


