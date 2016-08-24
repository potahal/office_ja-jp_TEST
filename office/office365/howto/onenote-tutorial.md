---
ms.Toctitle: Quick Start tutorial
title: "チュートリアル:OneNote アプリを作成する" 
description: "最初の OneNote アプリを作成する"
ms.ContentId: 513111e6-35e9-4d7d-b986-4ca2fa33b6cb
ms.date: January 12, 2016
---

[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]
[!INCLUDE [Add the ONAPI repo styles](../includes/controls/addonapistyles.xml)]

# チュートリアル:OneNote アプリを作成する

*__適用対象:__OneDrive のコンシューマー ノートブック | Office 365 のエンタープライズ ノートブック*

このチュートリアルでは、OneNote のコンテンツを取得して作成するためにOneNote API を使用する、シンプルなアプリを作成する方法を示します。ここで作成するアプリは、OneNote REST API を使用し、次の 2 つの呼び出しを行います。

<p id="outdent">**直近に変更された 10 のセクションの*名前*と *ID* を取得する**</p>
<p id="indent">`GET ../notes/sections?select=name,id&top=10&orderby=lastModifiedTime%20desc`</p>
<p id="outdent">**特定のセクションにページを作成する**</p>
<p id="indent">`POST ../notes/sections/{id}/pages`</p>

プラットフォームを選択する: 

- [iOS 向け OneNote アプリを作成する](#ios)

- [ASP.NET 向けの OneNote アプリを作成する](#aspnet)

>このチュートリアルの目的は、OneNote API にアクセスする方法を示すことですが、運用環境向けのコードは含まれていません。 アプリを作成するときには、潜在的なセキュリティ、検証、その他コードの品質に関係する問題が生じないかどうか、コードを慎重に確認してください。

<a name="ios"></a>
## iOS 向け OneNote アプリを作成する

アプリは認証とネットワークの呼び出しを処理するために、[OneDrive SDK for iOS](https://github.com/OneDrive/onedrive-sdk-ios) を使用します。<!--This tutorial provides examples for both the [Swift](https://developer.apple.com/swift/) and Objective C programming languages. -->

<a name="prereqs-ios"></a>
### 前提条件
以下に、このチュートリアルを使用するために必要なものを示します。

- Apple から入手した [Xcode 7](https://developer.apple.com/xcode/)。
- [CocoaPods](https://cocoapods.org/) 依存管理マネージャー。 CocoaPods を使い慣れていない場合は、[CocoaPods ガイド](https://guides.cocoapods.org/)をご覧ください。
- [Azure の管理ポータル](http://manage.windowsazure.com/)で登録したネイティブ クライアント アプリ、または [Microsoft アカウント デベロッパー センター](http://go.microsoft.com/fwlink/p/?LinkId=193157)で登録したモバイル クライアント アプリ。(「[アプリを登録する](../howto/onenote-auth.md)」方法をご覧ください。)

<br />
<p id="top-padding">**iOS 向け OneNote アプリを作成するには、以下を行います。**</p>
<p id="indent">1. [プロジェクトを作成する](#create-project-ios)</p>
<p id="indent">2. [OneDrive SDK 依存関係を追加する](#add-dependencies-ios)</p>
<p id="indent">3. [UI を構築する](#build-ui-ios)</p>
<p id="indent">4. [認証サポートを追加する](#add-auth-ios)</p>
<p id="indent">5. [OneNote API を呼び出す](#call-api-ios)</p>

キー サンプル ファイルの「[完全なコード例](#complete-example-ios)」は、このチュートリアルの末尾にあります。 


<a name="create-project-ios"></a>
### Xcode でプロジェクトを作成する

1. Xcode で、iOS 向けの *OneNote-iOS-App* という名前の **[Single View Application]** (シングル ビュー アプリケーション) プロジェクトを作成します。 <!--Swift or -->[Objective C] を選択し、[Devices] で [iPhone] を選択します。 
2. プロジェクトを作成後、Xcode を閉じます。 Podfile を作成した後、ワークスペースを開きます。
 

<a name="add-dependencies-ios"></a>
### OneDrive SDK 依存関係を追加する

このチュートリアルのアプリでは、Microsoft アカウント (以前の *Live Connect*) と Azure Active Directory 認証の両方に OneDrive SDK を使用します。 Microsoft アカウント認証は、OneDrive のコンシューマー ノートブックにアクセスするために使用します。 Azure AD 認証は、Office 365 のエンタープライズ ノートブックにアクセスするために使用します。

1.  Podfile を作成し、Xcode でファイルを開くために、ターミナルで以下のコマンドを実行します。

   ```
   cd {path to the OneNote-iOS-App project directory} 
   touch podfile 
   open -a xcode podfile 
   ```

2. Podfile に OneDrive SDK 依存関係を追加し、ファイルを保存します。

   ```
   pod 'OneDriveSDK'
   ```

3. 依存関係をインストールし、Xcode でプロジェクト ワークスペースを開くために、ターミナルで以下のコマンドを実行します。インストールが完了すると、確認メッセージが表示されます。

   ```
   pod install
   open onenote-ios-app.xcworkspace/
   ```

#### iOS 9.0 を対象とする Xcode 7 アプリ

Xcode 7 で iOS 9 アプリを対象とする場合、PFS 例外を有効にする必要があります。手順については、iOS 向け OneDrive SDK の [readme](https://github.com/OneDrive/onedrive-sdk-ios) ファイルの**iOS 9 アプリのトランスポート セキュリティ** セクションをご覧ください。


<a name="build-ui-ios"></a>
### UI を構築する

直近にユーザーが変更した 10 のセクションを表示するピッカーと、選択したセクションに OneNote のページを作成するボタンを追加します。

1. Xcode で、[Main.storyboard] を開き、[size class control] (キャンバスの下にあります) を *wCompact/hAny* に変更します。 

2. [Object Library] からキャンバスに [Picker View] と[Button] をドラッグします。 ボタンのテキストは、*Create page* にします。 

2. 次の手順で、ピッカー ビューの接続を作成します。  
   a. Control キーを押しながら、[Picker View] をキャンバスの上にある [View Controller] アイコンにドラッグします。 **[dataSource]** outlet を選択します。  
   b. **[delegate]** outlet について同様の操作を繰り返します。  
   c. **[View]、[Assistant Editor]、[Show Assistant Editor]** の順に選択し、2 番目のウィンドウで ViewController.h を開きます。  
   d. Control キーを押しながら、キャンバス内のピッカー ビューを **[@interface]** コード ブロックにドラッグします。 *sectionPicker* という名前の **Outlet** 接続を挿入します。  

2. 次の手順で、ボタンの接続を作成します。  
   a. Control キーを押しながら、キャンバス内のボタンを **[@interface]** コード ブロックにドラッグします。 *createPageButton* という名前の **Outlet** 接続を挿入します。  
   b. アシスタント エディターで ViewController.m を開きます。  
   c. Control キーを押しながら、キャンバス内のボタンを **[@implementation]** コード ブロックにドラッグします。 **Touch Up Inside** イベントにのための、*createPage* という名前の **Action** 接続を挿入します。 

2. **UIPickerViewDelegate** プロトコルと **UIPickerViewDataSource** プロトコルを宣言します。

   ViewController.h は、以下のようになっています。

   ```objective-c
    import <UIKit/UIKit.h>

    @interface ViewController : UIViewController<UIPickerViewDelegate, UIPickerViewDataSource>
 
    @property (weak, nonatomic) IBOutlet UIPickerView *sectionPicker;
    @property (weak, nonatomic) IBOutlet UIButton *createPageButton;
 
    @end
   ```

2. ViewController.m で、次のデリゲート メソッドをピッカー ビューに追加します。

   ```objective-c
    #pragma mark - Delegate Methods
   -(NSInteger)numberOfComponentsInPickerView:(UIPickerView *)pickerView {
       return 1;
   }

   -(NSInteger)pickerView:(UIPickerView *)pickerView numberOfRowsInComponent:(NSInteger)component {
       return sectionNamesForPicker.count;
   }

   -(NSString *)pickerView:(UIPickerView*)pickerView titleForRow:(NSInteger)row forComponent:(NSInteger)component {
       return [sectionNamesForPicker objectAtIndex:row];
   }
   ```

   **sectionNamesForPicker** というエラーが表示されますが、気にしないでください。 あとで変数を追加します。

2. **viewDidLoad** メソッドで、ピッカーに接続するために、`[super viewDidLoad]` という行のあとに以下のコードを追加します。

   ```objective-c
    self.sectionPicker.delegate = self;
    self.sectionPicker.dataSource = self;
   ```

<a name="add-auth-ios"></a>
### 認証サポートを追加する

OneDrive SDK は、認証と承認を処理します。必要なのは、アプリケーションに ID を提供し、ODClient を使用することだけです。SDK は、ユーザーがアプリケーションを初回に実行するときに、サインイン UI を呼び出します。その後、アカウント情報は保存されます。(詳しくは、「[SDK での認証](https://github.com/OneDrive/onedrive-sdk-ios/blob/master/docs/auth.md)」をご覧ください。) 

1. AppDelegate.m で、OneDrive SDK をインポートします。

   ```objective-c
    #import <OneDriveSDK/OneDriveSDK.h>
   ```

2. **didFinishLaunchingWithOptions** メソッドを、次のコードに置き換えます。 

   次に、プレース ホルダーのプロパティ値を、登録したアプリの情報に置き換えます。単独のアプリでテストするだけの場合、使用しないプロパティはコメント アウトしても構いません。   

   ```objective-c
    - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    
        // Set the client ID and permission scopes of your app registered on the Microsoft account Developer Center.
        static NSString *const msaClientId = @"000000001A123456";
        static NSString *const msaScopesString = @"wl.signin,wl.offline_access,office.onenote_update";
    
        // Set the client ID and redirect URI of your app registered on the Azure Management Portal.
        static NSString *const aadClientId = @"0b18d05c-386d-4133-b481-az1234567890";
        static NSString *const aadRedirectUri = @"http://localhost/";
    
        // Set properties on the ODClient.
        NSArray *const msaScopes = [msaScopesString componentsSeparatedByString:@","];
        [ODClient setMicrosoftAccountAppId:msaClientId
                                    scopes:msaScopes];
    
        [ODClient setActiveDirectoryAppId:aadClientId
                               capability:@"Notes"
                              redirectURL:aadRedirectUri];
        return YES;
    }
   ```

   >このアプリでは、一度に 1 アカウント (Microsoft アカウント、職場または学校のアカウント) でサインインします。 アカウントの種類、および複数アカウントの保存の両方をサポートする方法については、[CloudRoll](https://github.com/OfficeDev/O365-iOS-CloudRoll) のサンプルをご覧ください。
 
2. ViewController.h で、OneDrive SDK をインポートし、ODClient オブジェクトのプロパティを宣言します。SDK に対するすべての呼び出しは、ODClient オブジェクトを介して行われます。

   a. 次のように import ステートメントを追加します。

   ```objective-c
   #import <OneDriveSDK/OneDriveSDK.h>
   ```

   b. 次のように、**[@interface]** コード ブロックに **client** プロパティを追加します。

   ```objective-c
   @property (strong, nonatomic) ODClient *client;
   ```

2. ViewController.m で、認証済みの ODClient を取得するため、**viewDidLoad** メソッドの末尾に、次のコードを追加します。 

  SDK は、ユーザーがアプリケーションを初回に実行するときに、サインイン UI を呼び出します。その後、アカウント情報は保存されます。 

   ```objective-c
    [ODClient clientWithCompletion:^(ODClient *odClient, NSError *error) {
        if (!error){
            self.client = odClient;
            [self getSections];
        }
        else {
            NSLog(@"Error with auth: %@", [error localizedDescription]);
        }
    }];
   ```

   次のセクションでは、**getSections** メソッドを追加します。

2. ViewController.m で、**sendRequest** メソッドを **[@implementation]** コード ブロックに追加します。 

   このメソッドは、必要な**承認**ヘッダーを *GET sections* と *POST pages* 要求に追加し、データ転送タスクを作成します。 

   ```objective-c
    // Send the request.
    - (void)sendRequest:(NSMutableURLRequest *)request {

        // Add the required Authorization header with access token.
        [self.client.authProvider appendAuthHeaders:request completion:^(NSMutableURLRequest *requests, NSError *error) {

            // This app also uses the OneDrive SDK to send HTTP requests.
            [[self.client.httpProvider dataTaskWithRequest:(request)
                    completionHandler:^(NSData *data,
                    NSURLResponse *response,
                    NSError *error) {
                        [self handleResponse:data response:response error:error];
            }] resume];
        }];
    }
   ```

これで、Onenote サービスに呼び出しを行う準備が整いました。
 

<a name="call-api-ios"></a>
### OneNote API を呼び出す

アプリが読み込まれると、直近に変更された 10 のセクションの名前と ID を取得され、セクション名がピッカーに表示されます。選択したセクションで、新しいページが作成されます。

1. ViewController.h で、次のように応答を保存するプロパティを追加します。

   ```objective-c
   // Variables to store the response data.
   @property (strong, nonatomic) NSHTTPURLResponse *returnResponse;
   @property (strong, nonatomic) NSMutableData *returnData;
   ```

   次の手順のすべてのコードを ViewController.m の **[@implementation]** コード ブロックに追加します。アプリの作成中に表示されるエラーは気にしないでください。コードが完成すれば、それらのエラーはなくなります。

2. ViewController.m で、OneNote サービス ルート URL、セクション名と ID の辞書、ピッカーに表示されるセクション名の配列に対する変数を追加します。

   ```objective-c
   static NSString const *serviceRootUrl = @"https://www.onenote.com/api/v1.0/me/notes/";
   NSMutableDictionary *sectionNamesAndIds;
   NSArray *sectionNamesForPicker;
   ```

2. **getSections** メソッドを追加して、*GET sections* 要求を構築します。

   ```objective-c
    // Build the "GET sections" request.
    - (void)getSections {

        // Construct the request URI and the request.
        NSString *sectionsEndpoint =
                [serviceRootUrl stringByAppendingString:@"sections?select=name,id&top=10&orderby=lastModifiedTime%20desc"];
        NSMutableURLRequest *request =
                [[NSMutableURLRequest alloc] initWithURL:[[NSURL alloc] initWithString:sectionsEndpoint]];
        request.HTTPMethod = @"GET";
        if (self.client)
        {
        
            // Send the HTTP request.
            [self sendRequest:request];
        }
        _createPageButton.enabled = false;
    }
   ```

2. *handleResponse* メソッドを追加して、*GET sections* と *POST pages* 要求からの応答を処理します。

   ```objective-c                    
   // Handle the response.
   - (void)handleResponse:(NSData *)data response:(NSURLResponse *)response error:(NSError *) error {
       
       // Log the response.
       NSLog(@"Response %@ with error %@.\n", response, error);
       NSString *stringData = [[NSString alloc] initWithData:data encoding:NSUTF8StringEncoding];
       NSLog(@"Body: %@.\n", stringData);
       
       // Store the response.
       self.returnData = [[NSMutableData alloc] init];
       NSMutableData *convertedData = [data mutableCopy];
       [self.returnData appendData:convertedData];
       self.returnResponse = (NSHTTPURLResponse *)response;
    
       NSInteger status = [self.returnResponse statusCode];
    
       // Check for "GET sections" success.
       if (status == 200) {
           NSLog(@"Sections retrieved!\n");
        
           // Get the section data and populate the picker.
           [self getSectionNamesAndIds];
       }
    
       // Check for "POST pages" success.
       else if (status == 201) {
           NSLog(@"Page created!\n");
        
           // Get the page object and parse out some properties.
           NSDictionary *pageProperties = [self convertData];
           NSString *selfLink = [pageProperties objectForKey:@"self"];
           NSDictionary *links = [pageProperties objectForKey:@"links"];
           NSString *clientUrl = [[links objectForKey:@"oneNoteClientUrl"] objectForKey:@"href"];
           NSString *webUrl = [[links objectForKey:@"oneNoteWebUrl"] objectForKey:@"href"];
           NSLog(@"Link to new page endpoint: %@\n", selfLink);
           NSLog(@"Link open page in the installed client: %@\n", clientUrl);
           NSLog(@"Link to open page in OneNote Online: %@\n", webUrl);
       }
       else {
           NSLog(@"Status code: %ld. Check the logged response for more information.", (long)status);
       }
   }
   ```

2. **convertData** メソッドを追加して、応答データを JSON に変換します。 

   ```objective-c
   // Get the OneNote entity data from the response.
   - (NSDictionary *)convertData {
    
       // Convert the message body to JSON.
       NSError *parseError;
       NSDictionary *data = [NSJSONSerialization JSONObjectWithData:self.returnData options:kNilOptions error:&parseError];
    
       if (!parseError) {
           return data;
       }
       else {
           NSLog(@"Error parsing response: %@", [parseError localizedDescription]);
           return nil;
       }
   }
   ```

2. **getSectionNamesAndIds** メソッドを追加して、セクション名と ID を保存し、ピッカーに表示させます。 

   ```objective-c
   // Store the section names and IDs, and populate the section picker.
   - (void)getSectionNamesAndIds {
    
       // Get the "value" array that contains the returned sections.
       NSDictionary *results = [self convertData];
    
       // Add the name-id pairs to sectionNamesAndIds, which is used to map section names to IDs.
       if ([results objectForKey:@"value"] != nil) {
           NSDictionary *sections =[results objectForKey:@"value"];
           sectionNamesAndIds = [[NSMutableDictionary alloc] init];
           for (NSMutableDictionary *dict in sections) {
               NSString *sectionName = [dict objectForKey:@"name"];
               NSString *sectionId = [dict objectForKey:@"id"];
               sectionNamesAndIds[sectionName] = sectionId;
           }
       }
    
       // Populate the picker with the section names.
       sectionNamesForPicker = [sectionNamesAndIds allKeys];
       dispatch_async(dispatch_get_main_queue(), ^{[_sectionPicker reloadComponent:0];});
    
       _createPageButton.enabled = true;
   }
   ```

2. ボタン アクションを追加したときに作成した **createPage** メソッドを編集します。 このコードは、シンプルな HTML ページを作成します。  

   ```objective-c
   // Create a simple page.
   - (IBAction)createPage:(id)sender {
    
       // Get the ID of the section that's selected in the picker.
       NSInteger row = [self.sectionPicker selectedRowInComponent:0];
       NSString *selectedSectionName = sectionNamesForPicker[row];
       NSString *selectedSectionId = sectionNamesAndIds[selectedSectionName];
    
       // Construct the request URI and the request.
       NSString *pagesEndpoint = [NSString stringWithFormat:@"sections/%@/pages", selectedSectionId];
       NSString *fullEndpoint = [serviceRootUrl stringByAppendingString:pagesEndpoint];
       NSString *date = [self formatDate];
       NSString *simpleHtml = [NSString stringWithFormat:@"<html>"
                               "<head>"
                               "<title>A page created from simple HTML from iOS</title>"
                               "<meta name=\"created\" content=\"%@\" />"
                               "</head>"
                               "<body>"
                               "<p>This is some <b>simple</b> <i>formatted</i> text.</p>"
                               "</body>"
                               "</html>", date];
    
       NSData *presentation = [simpleHtml dataUsingEncoding:NSUTF8StringEncoding];
       NSMutableURLRequest * request = [[NSMutableURLRequest alloc] initWithURL:[[NSURL alloc] initWithString:fullEndpoint]];
       request.HTTPMethod = @"POST";
       request.HTTPBody = presentation;
       [request addValue:@"text/html" forHTTPHeaderField:@"Content-Type"];
       if (self.client)
       {
        
           // Send the HTTP request.
           [self sendRequest:request];
       }
   }
   ```

2. **formatDate** メソッドを追加して、**メタ** タグのタイムスタンプ用に ISO 8601 形式の日付を取得します。

   ```objective-c
   // Format the "created" date. OneNote requires the ISO 8601 format.
   - (NSString *)formatDate {
       NSDate *now = [NSDate date];
       NSDateFormatter *dateFormatter = [[NSDateFormatter alloc] init];
       NSLocale *enUSPOSIXLocale = [NSLocale localeWithLocaleIdentifier:@"en_US_POSIX"];
       [dateFormatter setLocale:enUSPOSIXLocale];
       [dateFormatter setDateFormat:@"yyyy-MM-dd'T'HH:mm:ssZZZZZ"];
       return [dateFormatter stringFromDate:now];
   }
   ```

アプリを作成後、iPhone または iPhone エミュレーターでそのアプリを実行してテストします。Microsoft アカウント、職場または学校アカウントを使用してサインインします。

アプリが開いているときに、ページを作成したいセクションを選択し、**Create page** を選択します。Xcode の出力ウィンドウでログ メッセージを確認します。呼び出しが動作すると、出力ウィンドウには、新しいページのリソースの URI と OneNote でページを開くためのリンクが表示されます。


<a name="next-steps"></a>
**次の手順:**

機能、入力の検証、エラー処理を追加します。

たとえば、このメソッドを呼び出すサインアウト ボタンを追加します。

```objective-c
- (IBAction)signOut:(UIButton *)sender  {
    [self.client signOutWithCompletion:^(NSError *signOutError) {
        self.client = nil;
        NSLog(@"Logged out.");
    }];
}
```

OneNote API で実現可能な事柄については、「[OneNote API で開発する](../howto/onenote-create-page.md)」という記事をご覧ください。

<a name="complete-example-ios"></a>
### 完全なコード例

**AppDelegate.m**

[!INCLUDE [ios complete example](../includes/onenote/ios-tutorial-appdelegate-m.txt)]

<br />
**ViewController.h**

[!INCLUDE [ios complete example](../includes/onenote/ios-tutorial-viewcontroller-h.txt)]

<br />
**ViewController.m**

[!INCLUDE [ios complete example](../includes/onenote/ios-tutorial-viewcontroller-m.txt)]

<a name="aspnet"></a>
## ASP.NET MVC 向けの OneNote アプリを作成する

この Web アプリケーションは、[Azure Active Directory Authentication Library (ADAL) for .NET](http://go.microsoft.com/fwlink/?LinkId=258232) を使用して、複数のテナントから職場や学校のアカウントを認証します。

<a name="prereqs-aspnet"></a>
### 前提条件
以下に、このチュートリアルを使用するために必要なものを示します。

- [Visual Studio 2015](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx)。 無償の Visual Studio Community エディションを使用できます。
- [Azure の管理ポータル](http://manage.windowsazure.com/)に登録されている Web アプリケーションで、デリゲートされた次のアクセス許可があるもの。 
    - Windows Azure Active Directory の**サインインとユーザー プロファイルの読み込み**
    - OneNote の **OneNote ノートブックの表示と変更**  

  <br />
            アプリ作成時に、Visual Studio はWeb アプリを登録しますが、再度 OneNote のアクセス許可を追加し、アプリ キーを生成する必要があります。(詳細については、「[アプリの登録](../howto/onenote-auth.md#register-aad)」をご覧ください。)

<br />
<p id="top-padding">**ASP.NET MVC を使用して OneNote アプリを作成するには、以下を行います。**</p>
<p id="indent">1. [プロジェクトを作成する](#create-project-aspnet)</p>
<p id="indent">2. [.NET ライブラリ用の ADAL を追加する](#add-dependencies-aspnet)</p>
<p id="indent">3. [UI を構築する](#build-ui-aspnet)</p>
<p id="indent">4. [認証サポートを追加する](#add-auth-aspnet)</p>
<p id="indent">5. [OneNote API を呼び出す](#call-api-aspnet)</p>

キー サンプル ファイルの「[完全なコード例](#complete-example-aspnet)」は、このチュートリアルの末尾にあります。 


<a name="create-project-aspnet"></a>
### Visual Studio でプロジェクトを作成する

1. Visual Studio で、*OneNote-WebApp* という名前の **ASP.NET Web アプリケーション** プロジェクトを作成します。  

2. **[MVC]** テンプレートを選択し、**[以下にフォルダーおよびコア参照を追加]** オプションに、[MVC] が選択されていることを確認します。   

2. **[認証の変更]** を選択し、**[職場および学校のアカウント]** を選択します。  

2. **[クラウド - 複数の組織]** を選択し、開発者のテナントのドメイン名 (たとえば、*contoso.onmicrosoft.com*) を入力します。 

必要に応じて、Microsoft Azure の **[クラウドにホストする]** の設定を残しても消去しても構いません。この設定は、このチュートリアルでは必須ではありません。それ以外はすべて既定の設定にしてください。

<br />
Visual Studio は Web アプリを Azure に登録しますが、[Azure の管理ポータル](http://manage.windowsazure.com/)で、そのアプリの構成を完了する必要があります。

1. ポータルのテナント ディレクトリで、**[アプリケーション]** を選択し、*[OneNote-Web]* アプリをクリックして、構成ページを開きます。
 
2. **[キー]** セクションで、新しいキーの継続期間を選択します。

2. **[他のアプリケーションに対するアクセス許可]** セクションで、OneNote アプリケーションを追加し、**[OneNote ノートブックの表示と変更]** 代理アクセス許可を追加します  (*[詳細を見る](../howto/onenote-auth.md#onenote-perms-aad)*)。

2. アプリに対する変更を保存し、ポータルを閉じる前に新しいキーのコピーを作成します。 あとでそれを使用します。


<a name="add-dependencies-aspnet"></a>
### .NET に ADAL を追加する

このアプリは、Azure AD への認証および承認に [Active Directory Authentication Library](http://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) (ADAL) を使用します。 このアプリは、バージョン 2.19.208020213 を使用して作成されました。

1.  Visual Studio で、**[ツール]、[NuGet パッケージ マネージャー]、[パッケージ マネージャー コンソール]** の順に選択し、コンソールで次のコマンドを実行します。 

   ```powershell
   Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
   ```


<a name="build-ui-aspnet"></a>
### UI を構築する

このアプリは、HomeController のために、次の 2 つのビューを使用します。Index.cshtml と Page.cshtml です。

1. Views/Home/Index.cshtml の内容を以下のコードに置き換えます。これにより、親セクションを選択するためのドロップダウン リスト、ページ名を入力するためのテキスト ボックス、ボタンを追加します。

   ```html
   @model OneNote_WebApp.Models.SectionsViewModel

   @{
       ViewBag.Title = "Index";
   }

   <h2>OneNote ASP.NET MVC web application</h2>

   @Html.Label("Choose a section to create the page in.")

   @using (Html.BeginForm("CreatePageAsync", "Home", new AjaxOptions { UpdateTargetId = "create-page" }))
   {
       <div id="create-page">
           @Html.DropDownListFor(
               m => m.SectionId,
               new SelectList(Model.Sections, "Id", "Name", Model.SectionId))
           @Html.ValidationMessageFor(m => m.SectionId)
           <br />
           <br />
           <table>
               <tr>
                   <td>
                       @Html.Label("Enter a name for the new page.")
                       <br />
                       @Html.TextBox("page-name", null, new { @style = "width=80" })
                   </td>
               </tr>
           </table>
           <button>Create page</button>
       </div>
   }
   ```

2. Views/Home フォルダーで、*Page* という名前のビューを作成し、次のコードを追加します。このビューでは、新しく作成したページのプロパティが表示されます。

   ```html
   @model OneNote_WebApp.Models.PageViewModel

   @{
        ViewBag.Title = "Page";
   }

   <h2>Page: @Model.Title</h2>

   <table>
       <tr>
           <td>@Html.Label("Self link: ")</td>
           <td>@Model.Self</td>
       </tr>
       <tr>
           <td>@Html.Label("Native client link: ")</td>
           <td><a href="@Model.PageLinks.ClientUrl">@Model.PageLinks.ClientUrl</a></td>
       </tr>
       <tr>
           <td>@Html.Label("Web client link: ")</td>
           <td><a href="@Model.PageLinks.WebUrl">@Model.PageLinks.WebUrl</a></td>
       </tr>
   </table>
   ```

<a name="add-auth-aspnet"></a>
### 認証サポートを追加する

.NET クライアント ライブラリの ADAL は、認証と承認のプロセスを処理します。必要なのは、アプリケーションの ID を提供し、ペアになっている呼び出しを追加することだけです。 

1. ルートの Web.config ファイルで、**appSettings** ノードに以下のキーと値のペアを追加します。ClientId と AADInstance は、Visual Studio によってすでに追加済みであることにご注意ください。

   ```html
   <add key="ida:AppKey" value="ENTER-your-app-key-here" />
   <add key="ida:OneNoteResourceId" value="https://onenote.com/" />
   ```
   
2. アプリ キー用のプレースホルダー値を以前に作成したキーに置き換えます。

2. App_Start/Start.Auth.cs で、次の **using** ステートメントを追加します。

   ```csharp
   using Microsoft.IdentityModel.Clients.ActiveDirectory;
   ```

2. **Startup** クラスのグローバル変数を次のコードに置き換えます。 HomeController の **GetAuthorizedClient** メソッドは、4 つのパブリック変数も使用します。

   ```csharp
   public static string ClientId = ConfigurationManager.AppSettings["ida:ClientId"];
   public static string AppKey = ConfigurationManager.AppSettings["ida:AppKey"];
   public static string AADInstance = ConfigurationManager.AppSettings["ida:AADInstance"];
   public static string OneNoteResourceId = ConfigurationManager.AppSettings["ida:OneNoteResourceId"];
   private string Authority = AADInstance + "common"; 
   ```

2. **ConfigureAuth** メソッドで、**app.UseOpenIdConnectAuthentication** メソッドを次のコードに置き換えます。 ADAL は、トークンと他の情報をトークン キャッシュに保存します  (キャッシュされる内容を表示するには、タスクを戻す前にこの行を追加します: `var cache = context.TokenCache.ReadItems();`)。

   ```csharp
   app.UseOpenIdConnectAuthentication(
      new OpenIdConnectAuthenticationOptions
      {
          ClientId = ClientId,
          Authority = Authority,
          TokenValidationParameters = new System.IdentityModel.Tokens.TokenValidationParameters
          {
              ValidateIssuer = false
          },
          Notifications = new OpenIdConnectAuthenticationNotifications()
          {
               AuthorizationCodeReceived = (context) =>
               {
                   var code = context.Code;
                   ClientCredential credential = new ClientCredential(ClientId, AppKey);
                   Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext authContext =
                      new Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext(Authority);
                   AuthenticationResult result = authContext.AcquireTokenByAuthorizationCode(
                       code,
                       new Uri(HttpContext.Current.Request.Url.GetLeftPart(UriPartial.Path)),
                       credential,
                       OneNoteResourceId
                   );
                   return Task.FromResult(0);
               },
               AuthenticationFailed = (context) =>
               {
                   context.HandleResponse();
                   if (context.Exception.HResult == -2146233088) //IDX10311: Nonce is null
                   {
                       context.OwinContext.Response.Redirect("Home/Index");
                   }
                   return Task.FromResult(0);
               }
           }
       });
   ```

2. Controllers/HomeController.cs で、次の **using** ステートメントを追加します。

   ```csharp
   using Microsoft.IdentityModel.Clients.ActiveDirectory;
   using Microsoft.Owin.Security;
   using Microsoft.Owin.Security.OpenIdConnect;
   using Newtonsoft.Json;
   using System.IO;
   using System.Net.Http;
   using System.Net.Http.Headers;
   using System.Security.Claims;
   using System.Text;
   using System.Threading.Tasks;
   using OneNote_WebApp.Models;
   ```

2. **HomeController** クラスで、**GetAuthorizedClient** メソッドを追加します。このメソッドは、OneNote サービスに REST 要求を行うのに使用される HttpClient を作成し、構成します。このメソッドは、アクセス トークンも取得し、それをクライアントに追加します。 

   ```csharp
   private HttpClient GetAuthorizedClient()
   {
       HttpClient client = new HttpClient();

       string userObjectId = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
       string tenantId = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid").Value;
       ClientCredential credential = new ClientCredential(Startup.ClientId, Startup.AppKey);
       AuthenticationContext authContext = new AuthenticationContext(Startup.AADInstance + tenantId);

       try
       {
           // Call AcquireTokenSilent to get the access token. This first tries to get the token from cache.
           AuthenticationResult authResult = authContext.AcquireTokenSilent(
               Startup.OneNoteResourceId,
               credential,
               new UserIdentifier(userObjectId, UserIdentifierType.UniqueId));
           client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", authResult.AccessToken);
           client.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
       }
       catch (AdalSilentTokenAcquisitionException)
       {
           HttpContext.GetOwinContext().Authentication.Challenge(
               new AuthenticationProperties() { RedirectUri = "/" },
               OpenIdConnectAuthenticationDefaults.AuthenticationType);
           return null;
       }

       return client;
   }
   ```

これで、OneNote サービスに呼び出しを行い、応答を解析する準備が整いました。
 

<a name="call-api-aspnet"></a>
### OneNote API を呼び出す

アプリが読み込まれると、直近に変更された 10 のセクションの名前と ID が取得され、セクション名をドロップダウン リストに表示させます。選択したセクションで、新しい OneNote ページが作成されます。

1. **HomeController** クラスで、OneNote のエンドポイントのグローバル変数、新しいページに追加するイメージ ファイルのパスを追加します。

   ```csharp
   public static string OneNoteRoot = "https://www.onenote.com/api/v1.0/me/notes/";
   public static string SectionsEndpoint = "sections?select=name,id&top=10&orderby=lastModifiedTime%20desc";
   public static string PagesEndpoint = "sections/{0}/pages";
   public static string PathToImageFile = @"C:\<local-path>\logo.png";
   ```

2. **PathToImageFile** 変数のプレースホルダーのパスとファイル名を、ローカルの PNG イメージを指すように変更します。

2. **Index** メソッドを次のコードに置き換えます。 これで、セクションが取得され、Index ビューのために **SectionsViewModel** が準備され、ビューが読み込まれます。

   ```csharp
   public async Task<ActionResult> Index()
   {
       SectionsViewModel viewModel = new SectionsViewModel();
       try
       {
           viewModel.Sections = await GetSectionsAsync();
       }
       catch (Exception ex)
       {
           return View("Error", new HandleErrorInfo(new Exception(ex.Message), "Home", "GetSectionsAsync"));
       }
       return View(viewModel);
   }
   ```

2. *GET sections* 要求を構築して送信し、応答を解析するために、**GetSectionsAsync** メソッドを追加します。

   ```csharp    
   [Authorize]
   [HttpGet]   
   public async Task<IEnumerable<Section>> GetSectionsAsync()
   {
       List<Section> sections = new List<Section>();
         
       HttpClient client = GetAuthorizedClient();
       HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Get, OneNoteRoot + SectionsEndpoint);
       HttpResponseMessage response = await client.SendAsync(request);
       if (response.IsSuccessStatusCode)
       {

           // Parse the JSON response.
           string stringResult = await response.Content.ReadAsStringAsync();
           Dictionary<string, dynamic> result = JsonConvert.DeserializeObject<Dictionary<string, dynamic>>(stringResult);
           foreach (var item in result["value"])
           {
               var current = item.ToObject<Dictionary<string, string>>();
               Section section = new Section
               {
                   Name = current["name"],
                   Id = current["id"]
               };
               sections.Add(section);
           }
       }
       else
       {
           throw new Exception("Error getting sections: " + response.StatusCode.ToString());
       }
       return sections;
   }
   ```

2. *POST pages* マルチパート要求を構築し送信し、応答を解析するために、**CreatePageAsync** メソッドを追加します。この要求は、シンプルな HTML ページを作成します。 

   ```csharp       
   [Authorize]
   [HttpPost]
   public async Task<ActionResult> CreatePageAsync()
   {
       HttpClient client = GetAuthorizedClient();

       // Get user input.
       string selectedSectionId = Request.Form["SectionId"];
       string pageName = Request.Form["page-name"];
       string pagesEndpoint = string.Format("sections/{0}/pages", selectedSectionId);

       // Define the page content, which includes an uploaded image.
       const string imagePartName = "imageBlock1";
       string iso8601Date = DateTime.Now.ToString("o");
       string pageHtml = "<html>" +
                         "<head>" +
                         "<title>" + pageName + "</title>" +
                         "<meta name=\"created\" content=\"" + iso8601Date + "\" />" +
                         "</head>" +
                         "<body>" +
                         "<h1>This is a page with an image</h1>" +
                         "<img src=\"name:" + imagePartName +
                         "\" alt=\"No mis monos\" width=\"250\" height=\"200\" />" +
                         "</body>" +
                         "</html>";

       HttpResponseMessage response;

       // Build the 'POST pages' request.
       var stream = new FileStream(PathToImageFile, FileMode.Open);
       using (var imageContent = new StreamContent(stream))
       {
           try
           {
               imageContent.Headers.ContentType = new MediaTypeHeaderValue("image/png");
               MultipartFormDataContent pageContent = new MultipartFormDataContent
               {
                   {new StringContent(pageHtml, Encoding.UTF8, "text/html"), "Presentation"},
                   {imageContent, imagePartName}
               };

               response = await client.PostAsync(OneNoteRoot + pagesEndpoint, pageContent);
               if (!response.IsSuccessStatusCode)
               {
                   throw new Exception(response.StatusCode + ": " + response.ReasonPhrase);
               }
               else
               {

                   // Parse the JSON response.
                   string stringResult = await response.Content.ReadAsStringAsync();
                   Dictionary<string, dynamic> pageData = JsonConvert.DeserializeObject<Dictionary<string, dynamic>>(stringResult);
                   Dictionary<string, dynamic> linksData = JsonConvert.DeserializeObject<Dictionary<string, dynamic>>(pageData["links"].ToString());
                   Links pageLinks = new Links
                   {
                       ClientUrl = new Uri(linksData["oneNoteClientUrl"]["href"].ToString()),
                       WebUrl = new Uri(linksData["oneNoteWebUrl"]["href"].ToString())
                   };
                   PageViewModel pageViewModel = new PageViewModel
                   {
                       Title = pageData["title"],
                       Self = new Uri(pageData["self"]),
                       PageLinks = pageLinks
                   };
                   return View("../home/page", pageViewModel);
               }
           }
           catch (Exception ex)
           {
               return View("Error", new HandleErrorInfo(new Exception(ex.Message), "Home", "CreatePageAsync"));
           }
       }
   }
   ```

2. Models フォルダーで、*Resource.cs* という名前の新しいクラスを追加し、次のコードを使用します。このコードでは、ドメイン モデルを定義します。このドメイン モデルは、OneNote のセクション、ページ、および、**Index** ビューまた **Page** ビューで OneNote データを表すビュー モデルを表します。

   ```csharp
   using System;
   using System.Collections.Generic;
   using System.IO;

   namespace OneNote_WebApp.Models
   {

       // Common properties of OneNote entities.
       public class Resource
       {
           public string Id { get; set; }
           public string CreatedBy { get; set; }
           public DateTimeOffset CreatedTime { get; set; }
           public string LastModifiedBy { get; set; }
           public DateTimeOffset LastModifiedTime { get; set; }
           public Uri Self { get; set; }
       }
      
       // A OneNote section with some key properties. 
       public class Section : Resource
       {
           public bool IsDefault { get; set; }
           public string Name { get; set; }
           public ICollection<Page> Pages { get; set; }
           public Uri PagesUrl { get; set; }
       }
   
       // A OneNote page with some key properties.
       // This app doesn't use the Page model.
       public class Page : Resource
       {
           public Stream Content { get; set; }
           public Uri ContentUrl { get; set; }
           public Links PageLinks { get; set; }
           public string Title { get; set; }
       }
            
       // The links that open a OneNote page in the installed client or in OneNote Online.
       public class Links
       {
           public Uri ClientUrl { get; set; }
           public Uri WebUrl { get; set; }
       }
      
       // The view model used to populate the section drop-down list.
       public class SectionsViewModel
       {
           public string SectionId { get; set; }
           public IEnumerable<Section> Sections { get; set; }
       }
      
       // The view model used to display properties of the new page.
       public class PageViewModel
       {
           public string Title { get; set; }
           public Uri Self { get; set; }
           public Links PageLinks { get; set; }
       }
   }
   ```


アプリを作成した後、F5 キーを押してデバッグ実行します。

*OwinStartupAttribute を含むアセンブリが見つかりませんでした...* エラーを受け取った場合、ルート ディレクトリの Startup.cs class の **using** ステートメントの後に、次の属性を追加します  (*このエラーについての[詳細](http://www.asp.net/aspnet/overview/owin-and-katana/owin-startup-class-detection)*)。

```
[assembly: OwinStartup(typeof(OneNote_WebApp.Startup))]
```

職場または学校のアカウントを使用してアプリにサインインします。アカウントには、少なくとも 1 つのセクションを含む、少なくとも 1 つのノートブックが必要です。アプリで、ページを作成したいセクションを選択し、**[Create page]** を選択します。正常に実行できた場合、アプリには、タイトルとそれ自体、および新しいページのリンクが表示されます。

<a name="complete-example-aspnet"></a>
### 完全なコード例


**Startup.Auth.cs**

[!INCLUDE [aspnet complete example](../includes/onenote/aspnet-tutorial-startup-auth.txt)]

<br />
**HomeController**

[!INCLUDE [aspnet complete example](../includes/onenote/aspnet-tutorial-homecontroller.txt)]

<br />
手順には、**Index.cshtml**、**Page.cshtml**、**Resource.cs** 全体が示されています。


**次の手順:**OneNote API で実現可能な事柄については、「[OneNote API で開発する](../howto/onenote-create-page.md)」という記事をご覧ください。


<!--

## Create a OneNote app for Android

This tutorial creates an app that uses the Office 365 SDK for Android to authenticate and to access the OneNote APIs. If you prefer to use Live Connect (consumer auth), Azure AD (enterprise auth), or the OneNote API (REST) directly 
 check out the [Android REST API Explorer for OneNote](https://github.com/OneNoteDev/Android-REST-API-Explorer).

This example accesses Office 365 notebooks, though the SDK also supports SharePoint-hosted notebooks and Live Connect auth for consumer notebooks. (The SDK includes a sample that uses Live Connect(xx).)

After you've set up your development environment() and registered your app(), we'll follow these steps in Android Studio:

1. Create a project and configure dependencies
2. Set up authentication 
3. Construct the API client
4. Use client methods to call the OneNote API 

**Note:** If you're new to Android development, visit the [Get Started with Android Studio](http://developer.android.com/develop/index.html) site for help setting up your development environment and building Android apps.

Before you begin, you'll need:
- The client ID and redirect URI of a native client application that's registered in Azure Active Directory. (See instructions for [setting up accounts](http://blogs.msdn.com/b/onenotedev/archive/2015/04/30/set-up-your-azure-office-365-tenant.aspx) and [registering the app](http://blogs.msdn.com/b/onenotedev/archive/2015/04/30/register-your-application-in-azure-ad.aspx).)
- The [View and modify OneNote notebooks permission scope for OneNote](http://blogs.msdn.com/b/onenotedev/archive/2015/04/30/register-your-application-in-azure-ad.aspx#SpecifyOneNotePerms). We'll use the highest level of OneNote permissions to simplify testing, but you should request the lowest level required by your app as a best practice.
- OneNote [provisioned for your work or school account](http://blogs.msdn.com/b/onenotedev/archive/2015/04/30/set-up-your-azure-office-365-tenant.aspx#ProvisionOneDriveForBusiness).

### Create a project and configure dependencies in build.gradle

1. From the Android Studio splash screen, choose **Start a new Android Studio project**. Enter a name and choose **Next**.
2. Choose **Phone and Tablet**, set **Minimum SDK** to API 18, and then choose **Next**. 
3. Choose **Blank Activity**, then choose **Next**. Keep the default activity settings, and choose **Finish**.
4. In Project view, expand **Gradle Scripts**, and double-click **build.gradle (Module: app)** to open it.
5. In the dependencies closure, add the following dependencies:

    compile 'com.microsoft.services:onenote-services:0.14.2'
    compile 'com.microsoft.orc:orc-engine-android:0.14.2@aar'
    compile 'com.microsoft.aad:adal:1.1.3@aar'

6. Choose the **Sync Project with Gradle Files** button in the toolbar. This downloads the dependencies so Android Studio can assist in coding with them.
7. Open AndroidManifest.xml (app/manifests), and add the following line to the manifest section:

```
     <uses-permission android:name="android.permission.INTERNET" />
```

8. Open activity_main.xml (app/res/layout) and add the following `id` tag to the "Hello World" text view control.

```
    android:id="@+id/messages"
```
  
### Set up authentication and initialize the dependency manager 

1. Right-click the **values** folder (app/res/values) and choose **New > Values resource file**. Name the file *adal_settings*. 
2. In the `resources` section, add the following code. Then, replace the placeholder values for the client ID and redirect URI with your actual values. (You can get them on the application's Configure tab in the [Azure Management Portal](https://manage.windowsazure.com).)

```
    <string name="AADAuthority">https://login.microsoftonline.com/common</string>
    <string name="AADResourceId">https://onenote.com</string>
    <string name="AADClientId">Your Client ID</string>
    <string name="AADRedirectUrl">Your Redirect URI</string>
```

3. Open the `MainActivity` class (app/java/{package-name}), and add the following imports.

```java
    import android.content.Intent;
    import android.util.Log;
    import android.widget.TextView;
    import com.google.common.util.concurrent.FutureCallback;
    import com.google.common.util.concurrent.Futures;
    import com.google.common.util.concurrent.SettableFuture;
    import com.microsoft.aad.adal.AuthenticationCallback;
    import com.microsoft.aad.adal.AuthenticationContext;
    import com.microsoft.aad.adal.AuthenticationResult;
    import com.microsoft.aad.adal.PromptBehavior;
    import com.microsoft.onenote.api.Page;
    import com.microsoft.onenote.api.orc.OneNoteApiClient;
    import com.microsoft.services.android.impl.ADALDependencyResolver;
    import java.security.NoSuchAlgorithmException;
    import java.util.List;
    import javax.crypto.NoSuchPaddingException;
    import static com.microsoft.aad.adal.AuthenticationResult.AuthenticationStatus;
```

4. Add the following instance fields to the `MainActivity` class:

```java
    private AuthenticationContext mAuthContext;
    private ADALDependencyResolver mResolver;
    private TextView messagesTextView;
```

5. Add the following method to the `MainActivity` class. This constructs and initializes ADAL's `AuthenticationContext`, carries out interactive logon, and constructs the ADALDependencyResolver using `AuthenticationContext`.

```java
    protected SettableFuture<Boolean> logon() {
        final SettableFuture<Boolean> result = SettableFuture.create();

        try {
            mAuthContext = new AuthenticationContext(this, getString(R.string.AADAuthority), true);
            mAuthContext.acquireToken(
                this,
                getString(R.string.AADResourceId),
                getString(R.string.AADClientId),
                getString(R.string.AADRedirectUrl),
                PromptBehavior.Auto,
                new AuthenticationCallback<AuthenticationResult>() {
                    @Override
                    public void onSuccess(final AuthenticationResult authenticationResult) {
                        if (authenticationResult != null
                                && authenticationResult.getStatus() == AuthenticationStatus.Succeeded) {
                            mResolver = new ADALDependencyResolver(
                            mAuthContext,
                            getString(R.string.AADResourceId),
                            getString(R.string.AADClientId));
                            result.set(true);
                        }
                    }
                    @Override
                    public void onError(Exception e) {
                    result.setException(e);
                   }
                });
        } catch (NoSuchAlgorithmException | NoSuchPaddingException e) {
            e.printStackTrace();
            result.setException(e);
        }
        return result;
    }
```

6. Add the following method to the `MainActivity` class. This passes the authentication result back to `AuthenticationContext`.

```java
    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        mAuthContext.onActivityResult(requestCode, resultCode, data);
    }
```

7. Add the following code to the MainActivity.onCreate method. This caches the `messages` TextView, and then calls `logon()` and hooks up to its completion. 

```java
    messagesTextView = (TextView) findViewById(R.id.messages);
    Futures.addCallback(logon(), new FutureCallback<Boolean>() {
        @Override
        public void onSuccess(Boolean result) {
            // construct the API client
        }

        @Override
        public void onFailure(Throwable t) {
            Log.e("logon", t.getMessage());
        }
    });
```

### Construct the API client

1. Add the following code to the `onSuccess` method of the FutureCallback that you just added in the previous step. This creates the API client and calls a method that uses the API client.

```java
    mClient = new OneNoteApiClient(oneNoteBaseUrl, mResolver);
    getPages();
    //createNotebook();
```

2. Add the following instance fields to the `MainActivity` class. OneNote support for Office 365 notebooks is in Preview, so we'll use the `/beta` path for the OneNote base URL.

```java
    private static final String oneNoteBaseUrl = "https://www.onenote.com/api/beta/";
    private OneNoteApiClient mClient;
```

### Use client methods to call the OneNote API

1. Add the following method to the `MainActivity` class. This uses the API client to get the name and ID of your three newest OneNote pages.

```java
    protected void getPages() {
        Futures.addCallback(
            mClient.getme()
                    .getNote()
                    .getPages()
                    .top(3)
                    .select("title,self")
                    .orderBy("createdTime desc")
                    .read(),
            new FutureCallback<List<Page>>() {
                @Override
                public void onSuccess(final List<Page> result) {
                    runOnUiThread(new Runnable() {
                        @Override
                        public void run() {
                            for (Page page : result) {
                                String data = String.format(
                                        "Title: %1$s, Endpoint: %2$s\n",
                                        page.getTitle(), page.getSelf());
                                messagesTextView.append(data);}
                        }
                    });
                }
                @Override
                public void onFailure(final Throwable t) {
                    Log.e("getPages", t.getMessage());
                }
            });
    }       
```

2. Run the app. If successful, the app displays the names and endpoint URIs of the last three pages you created in your Office 365 notebooks.

You can use the SDK to do the same operations that are [supported in the REST API](http://dev.onenote.com/docs#/reference). For example, use the following method to create a notebook and then show its `id` and `oneNoteWebUrl` properties. 

1. In the `MainActivity` class, add these `import` statements.  

```java
    import com.microsoft.onenote.api.Notebook;
    import java.util.Random;
```

2. And then add the following method. 

```java
    protected void createNotebook() {
        Random random =new Random();
        Notebook notebook = new Notebook();
        notebook.setName("Notebook" + random.nextInt(1000));
        Futures.addCallback(
            mClient.getme()
                    .getNote()
                    .getNotebooks()
                    .add(notebook),
            new FutureCallback<Notebook>() {
                @Override
                public void onSuccess(final Notebook result) {
                    runOnUiThread(new Runnable() {
                        @Override
                        public void run() {
                            String data = String.format(
                                    "ID: %1$s\nWeb link: %2$s",
                                    result.getId(),
                                    result.getLinks().getOneNoteWebUrl().getHref());
                            messagesTextView.setText(data);
                        }
                    });
                }
                @Override
                public void onFailure(final Throwable t) {
                    Log.e("createNotebook", t.getMessage());
                }
            });
    }
```

3.  In the `onSuccess` method, comment out the call to `getPages()` and uncomment `createNotebook()`, as shown below:

```java
    //getPages();
    createNotebook();
```

For examples of other OneNote operations, take a look at the [OneNoteTest](https://github.com/OfficeDev/Office-365-SDK-for-Android/blob/master/tests/e2e-test-app/app/src/main/java/com/microsoft/office365/test/integration/tests/OneNoteTests.java) file. 
  **Note!** Unlike the asynchronous calls used in the previous examples, these calls are sent synchronously by using the `get` method. For example: 

```java
    List<Notebook> notebooks = client.getme().getNote().getNotebooks().read().get();
```

### OneNoteSample project uses Live Connect

There's a OneNoteSample project in the SDK that uses Live Connect authentication to get your notebooks that are stored on OneDrive. To run it, just import the project into Android Studio and set your client ID in the `Constants` class.

-->

<a name="see-also"></a>
## その他の技術情報

- [OneNote ページの作成](../howto/onenote-create-page.md)
- [画像とファイルを追加する](../howto/onenote-images-files.md)
- [OneNote の開発](../howto/onenote-landing.md)
- [OneNote デベロッパー センター](http://dev.onenote.com/)
- [OneNote の開発者ブログ](http://go.microsoft.com/fwlink/?LinkID=390183)
- [スタック オーバーフローに関する OneNote の開発の質問](http://go.microsoft.com/fwlink/?LinkID=390182) 
- [OneNote GitHub のリポジトリ](http://go.microsoft.com/fwlink/?LinkID=390178)  



