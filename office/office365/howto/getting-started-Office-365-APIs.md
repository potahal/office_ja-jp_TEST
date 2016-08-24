ms.TocTitle:Office 365 API でアプリを作成するTitle:Office 365 API でアプリを作成するDescription:Office 365 API を使用して、お客様のドキュメント、電子メール、連絡先、または予定表データに接続する iOS、Android、JavaScript、または ASP.NET サンプル アプリケーションを作成します。ms.ContentId:907209f2-ed2a-4c81-b7d1-9b1a5ef497a5ms.topic: 記事 (方法) ms.date:2015 年 8 月 13 日

<script src="https://cdn.optimizely.com/js/3043860527.js"></script> 

[!INCLUDE [O365API リポジトリ スタイルの追加](../includes/controls/addo365apistyles.xml)]

# Office 365 API でアプリを作成する

_**適用対象:** Office 365_

[!INCLUDE [プラットフォームのフィルター バーの追加](../includes/controls/addplatformsfilter.xml)]

Office 365 API は、Exchange Online からメール、予定表、連絡先など、SharePoint Online および OneDrive for Business からファイルやフォルダーなど、Azure Active Directory からユーザーやグループなどの Office 365 データにアクセスできるようにする REST サービスです。



[!INCLUDE [iOS セクションの開始](../includes/controls/iossection.xml)]


**iOS アプリを開発するのではない場合** このページの右上隅にあるコントロールを使用して、開発するアプリの種類を選択してください。


##Office 365 API で iOS アプリを作成する

<p class="previewnote">このドキュメントで取り上げる機能は、現時点ではプレビュー段階にあります。プレビュー機能を使用する方法の詳細については、[Office 365 プラットフォームの開発者機能のプレビュー] (..\howto\platform-development-preview-features-overview.md) を参照してください。</p>


Office 365 API と統合する iOS アプリを作成できます。iOS アプリを作成する場合、 [REST API](..\API\API-Catalog.md) に対して、Office 365 とやり取りするように直接プログラミングしたり、REST API に基づいて構築されたオブジェクト モデルである [Office 365 iOS SDK](https://github.com/OfficeDev/Office-365-SDK-for-iOS) を使用して、アプリを作成したりすることができます。このトピックでは、Office 365 iOS SDK を使用して、ユーザーの電子メールを取得し、ユーザーのデバイス上にそれを表示する基本的な iOS アプリを作成する手順を説明します。このチュートリアルは、Office 365 Connect のサンプルに基づいています。

Office 365 iOS コード サンプルについては、次を参照してください。

- [iOS 版 Office 365 Connect アプリ](https://github.com/OfficeDev/O365-iOS-Connect#office-365-connect-app-for-ios)

- [iOS 用 Office 365 コード スニペット](https://github.com/OfficeDev/O365-iOS-Snippets)

- [Email Peek - Office 365 を使用して構築された iOS アプリ](https://github.com/OfficeDev/O365-iOS-EmailPeek/)

**注** これらのコード サンプルでは、 [iOS 用 Office 365 SDK](https://github.com/OfficeDev/Office-365-SDK-for-iOS) を使用して、Office 365 に接続しています。


##始める前に

Office 365 API にアクセスするアプリケーションを作成する前に、開発者環境を設定する必要があります。これは、ツールと環境が正常であることを確認する 3 つの 1 回限りのタスクから構成されます。

1. アプリの作成に使用する iOS アプリ開発環境を設定します。これには、 [CocoaPods](http://cocoapods.org) 環境のインストールとセットアップが含まれます。

2. Office 365 API にアクセスするために、ビジネス向け Office 365 サブスクリプションを取得します。

3. アプリを作成して管理できるように、Office 365 サブスクリプションを Azure Active Directory と関連付けます。

まだこれらの手順のいずれかを実行する必要がある場合、セットアップの詳細については、「 [Office 365 開発環境のセットアップ](..\howto\setup-development-environment.md)」を参照してください。


##アプリの作成と依存関係の追加
ここでは、iOS プロジェクトを作成し、CocoaPods を使用して、iOS 用 Office 365 SDK と [iOS 用 Azure Active Directory 認証ライブラリ (ADAL)](https://github.com/AzureAD/azure-activedirectory-library-for-objc) への依存関係を追加します。

###SimpleMailApp プロジェクトの作成
1.	Xcode を開きます。
2.	**[ファイル] > [新規] > [プロジェクト]** の順に選択します。
3.	**[iOS]** プロジェクト テンプレートの **[*Applications]** セクションで、**[Single View Application]** テンプレートを選択して、**[Next]** を選択します。
4.	**[Product Name]** に「**SimpleMailApp**」を指定し、**[Language]** に **[Objective-C]**、**[Devices]** に **[Universal]**、**[Organization Identifier]** に値を選択し、**[Next]** をクリックします。
5.	プロジェクトの場所を選択し、バージョン管理する必要があるかどうかを指定して、**[Create]** を選択します。
6.	プロジェクトが作成されたら、Xcode を閉じます。 
 
Cocoapods コマンドは、プロジェクト フォルダーのルートから実行する必要があるため、ターミナルから、プロジェクト ディレクトリに移動します。プロジェクトの作成時に、既定の場所を変更しなかった場合、プロジェクトは**デスクトップ**の **[SimpleMailApp]** ディレクトリに置かれます。

###SimpleMailApp プロジェクトに対し Cocoapods を有効にする
1.	次のコマンドを実行して、プロジェクトの Podfile を初期化します。

    ```objective-c
		pod init
```

2.	次のコマンドを使用して、Podfile を開きます。

    ```objective-c
		Open podfile
```

3.	開いている Podfile に次の定義を追加して、iOS 用 Office 365 SDK および iOS 用 ADAL SDK の依存関係を宣言します。

    ```objective-c
    pod 'ADALiOS', '~> 1.2.0'
    pod 'Office365', '= 0.9.0'
```
    これらの定義は、target ステートメントと end ステートメントの間に置く必要があるため、結果は次のようになります。

    ```objective-c
    target 'SimpleMailApp' do
    pod 'ADALiOS', '~> 1.2.0'
    pod 'Office365', '= 0.9.0'
    end
    ```
     
4.	Podfile を閉じます。

**注** iOS 用 Office 365 SDK は現在開発者プレビュー段階にあります。つまり、完成前に変更されることがあり、SDK を使用して作成されたコードを破棄させるような変更が発生する可能性もあります。SDK では、互換性を 3 つの部分のバージョン番号 (major.minor.patch) を使用して指定する [セマンティック バージョニング](http://semver.org/)を使用しています。バージョン 1.0 に達するまで、最新およびその他の重要な変更に対して、マイナー バージョン番号が増分されます。

###SimpleMailApp プロジェクトの依存関係の構成
これらの依存関係を構成し、それらと既存のプロジェクトを新しいワークスペースに追加するには、ターミナルから、次のコマンドを実行します。

    pod install

###Azure Active Directory にアプリケーションを登録する
1.	Office 365 開発者向けサイト資格情報を使用して、 [Azure 管理ポータル](https://manage.windowsazure.com/)にサインインします。

2.	左側のメニューで **[Active Directory]** をクリックしてから、Office 365 開発者向けサイトのディレクトリをクリックします。 
![Azure Management Portal](images/O365APIs_RegisterApp_1.png)

3.	上部のメニューで、**[アプリケーション]** をクリックします。

4.	下部のメニューから、**[追加]** をクリックします。  
![Azure Management Portal Add Application](images/O365APIs_RegisterApp_2.png)

5.	**[何を行いますか]** ページで、**[所属組織が開発しているアプリケーションの追加]** をクリックします。

6.	**[アプリケーションについてお聞かせください]** ページで、アプリケーション名に「**SimpleMailApp**」と指定し、**[種類]** に **[ネイティブ クライアント アプリケーション]** を選択します。

7.	ページの右下隅にある矢印アイコンをクリックします。

8.	**[アプリケーション情報]`http://localhost/simplemailappproject` ページで、リダイレクト URI を指定します。この例の場合は、「**」と指定してください。後で SimpleMailApp プロジェクトをコーディングするときに必要になるため、URI を書き留めておきます。ページの右下隅にあるチェック ボックスをクリックします。

9.	アプリケーションが正常に追加されたら、アプリケーションの [クイック スタート] ページに移動します。ここで、上部のメニューにある **[構成]** をクリックします。

10.	**[他のアプリケーションに対するアクセス許可]** で、**[アプリケーションの追加]** をクリックします。

11.	**[Office 365 Exchange Online]** をクリックし、チェック マーク アイコンをクリックします。
![Azure Management Portal Add Application](images/O365APIs_RegisterApp_5.png)

12.	**[他のアプリケーションに対するアクセス許可]** で、**[Office 365 Exchange Online]** の **[デリゲートされたアクセス許可]** 列をクリックし、**[ユーザーとしてメールを送信]** を選択します。  
![Azure Management Portal Add Application](images/O365APIs_RegisterApp_6.png)

    指定したアクセス許可は、アプリのユーザーに対し、Azure がアプリのアクセス許可要求に同意するように求める場合に表示されます。一般に、アプリが実際に必要とするサービスのみを要求し、アプリがその機能を実行できる各サービスの最低レベルのアクセス許可を指定します。

13.	**[クライアント ID]** に指定された値をコピーします。これは後で、SimpleMailApp プロジェクトをコーディングするときに必要になります。

14.	下部のメニューで、**[保存]** をクリックします。


## アプリのコーディング 
Xcode で **SimpleMailApp** ワークスペースを開きます。 

###Azure AD によって認証し、アクセス トークンを取得する
Office 365 API にアクセスするにはアクセス トークンが必要であるため、アクセス トークンを取得して管理するロジックをアプリケーションに実装する必要があります。[iOS および OSX 用 Azure Active Directory 認証ライブラリ (ADAL)](https://github.com/AzureAD/azure-activedirectory-library-for-objc) は、わずか数行のコードで、アプリケーションで認証を管理できるようになります。Azure Active Directory での認証の詳細については、Azure.com の「 [Azure Active Directory とは](https://azure.microsoft.com/en-us/documentation/articles/active-directory-whatis/)」を参照してください。最初に、iOS および OSX 用 ADAL を使用して、アプリの認証を管理するヘッダー ファイルとクラス AuthenticationManager を作成します。

####AuthenticationManager クラスを作成するには
1. SimpleMailApp プロジェクト フォルダーを右クリックし、**[New File]** を選択して、**[iOS]** セクションの **[Cocoa Touch Class]** をクリックし、**[Next]** をクリックします。

2. **[Class]** として「**AuthenticationManager**」を指定し、**[Subclass of:]** に **[NSObject]** を指定して、**[Next]** をクリックします。

3. **[Create]** をクリックして、クラスおよびヘッダー ファイルを作成します。

####AuthenticationManager ヘッダー ファイルをコーディングするには

1. AuthenticationManager.h に次のコード ディレクティブを追加して、必要な Office 365 iOS SDK および ADAL SDK ヘッダー ファイルをインポートします。

    ```objective-c
#import <Foundation/Foundation.h>
#import <ADALiOS/ADAuthenticationContext.h>
#import <ADALiOS/ADAuthenticationSettings.h>
#import <ADALiOS/ADLogger.h>
#import <ADALiOS/ADInstanceDiscovery.h>
#import <office365_odata_base/office365_odata_base.h>
```

6. ADAL SDK から、依存関係の挿入を使用して、認証オブジェクトにアクセスする **ADALDependencyResolver** オブジェクトのプロパティを宣言します。

    ```objective-c
@property (readonly, nonatomic) ADALDependencyResolver *dependencyResolver;
```

7. **AuthenticationManager** クラスをシングルトンとして指定します。

    ```objective-c
+(AuthenticationManager *)sharedInstance;
```

8. アクセスおよび更新トークンを取得し、クリアするためのメソッドを宣言します。

    ```objective-c
//retrieve token
-(void)acquireAuthTokenWithResourceId:(NSString *)resourceId completionHandler:(void (^)(BOOL authenticated))completionBlock;
//clear token
-(void)clearCredentials;
```

####AuthenticationManager クラスをコーディングするには

1. **@implementation** 宣言の上で、リダイレクト URI、クライアント ID、および権限の静的変数を宣言します。

    ```objective-c
static NSString * const REDIRECT_URL_STRING = @"redirectUri";
static NSString * const CLIENT_ID           = @"clientID";
static NSString * const AUTHORITY           = @"https://login.microsoftonline.com/common";
```

    *redirectUri* を、Azure AD にアプリを登録した手順 8 からコピーした値に、*clientID* を、同じ手順の手順 13 で指定した値に置き換えます。

2. **@implementation** の上の ***interface** 宣言で、次のプロパティを宣言します。

    ```objective-c
@interface AuthenticationManager ()
@property (strong,    nonatomic) ADAuthenticationContext *authContext;
@property (readwrite, nonatomic) ADALDependencyResolver  *dependencyResolver;
@property (readonly, nonatomic) NSURL    *redirectURL;
@property (readonly, nonatomic) NSString *authority;
@property (readonly, nonatomic) NSString *clientId;
@end
```
3. コンストラクターのコードを実装に追加します。 

	```objective-c
-(instancetype)init
{
    self = [super init];
    if (self) {
        // These are settings that you need to set based on your
        // client registration in Azure AD.
        _redirectURL = [NSURL URLWithString:REDIRECT_URL_STRING];
        _authority = AUTHORITY;
        _clientId = CLIENT_ID;
    }
    return self;
}
```

4. アプリケーションで単一の認証マネージャーを使用するために次のコードを追加します。

	```objective-c
+(AuthenticationManager *)sharedInstance
{
    static AuthenticationManager *sharedInstance;
    static dispatch_once_t onceToken;
 
    // Initialize the AuthenticationManager only once.
    dispatch_once(&onceToken, ^{
        sharedInstance = [[AuthenticationManager alloc] init];
    });
 
    return sharedInstance;
}
```
5. Azure AD からユーザーのアクセスおよび更新トークンを取得します。

    ```objective-c
-(void)acquireAuthTokenWithResourceId:(NSString *)resourceId completionHandler:(void (^)(BOOL authenticated))completionBlock
{
        ADAuthenticationError *error;
    self.authContext = [ADAuthenticationContext authenticationContextWithAuthority:self.authority error:&error];
[self.authContext acquireTokenWithResource:resourceId
                                      clientId:self.clientId
                                   redirectUri:self.redirectURL
                               completionBlock:^(ADAuthenticationResult *result) {
                                   if (AD_SUCCEEDED != result.status) {
                                       completionBlock(NO);
                                   }
                                   else {
                                       NSUserDefaults *userDefaults = [NSUserDefaults standardUserDefaults];
                                       [userDefaults setObject:result.tokenCacheStoreItem.userInformation.userId
                                                        forKey:@"LogInUser"];
                                       [userDefaults synchronize];
                                       self.dependencyResolver = [[ADALDependencyResolver alloc] initWithContext:self.authContext
                                                                                                      resourceId:resourceId
                                                                                                        clientId:self.clientId
                                                                                                     redirectUri:self.redirectURL];
                                       completionBlock(YES);
                                   }
                               }];
}
```

    初めてアプリケーションを実行すると、AUTHORITY const に指定された URL (手順 1 を参照) に要求が送信され、資格情報を入力できるログイン ページにリダイレクトされます。ログインが成功すると、応答にアクセスおよび更新トークンが含まれます。それ以降にアプリケーションが実行すると、認証マネージャーは、トークン キャッシュがクリアされていない限り、クライアント要求の認証に、そのアクセスまたは更新トークンを使用します。

6. 最後に、トークン キャッシュをクリアし、アプリケーションの cookie を削除して、ユーザーをログアウトさせるコードを追加します。

    ```objective-c
-(void)clearCredentials{
    id<ADTokenCacheStoring> cache = [ADAuthenticationSettings sharedInstance].defaultTokenCacheStore;
    ADAuthenticationError *error;
 
       if ([[cache allItemsWithError:&error] count] > 0)
        [cache removeAllWithError:&error];
        NSHTTPCookieStorage *cookieStore = [NSHTTPCookieStorage sharedHTTPCookieStorage];
    for (NSHTTPCookie *cookie in cookieStore.cookies) {
        [cookieStore deleteCookie:cookie];
    }
}
```

###Office 365 への接続
次に、Office 365 に接続し、探索サービスを使用して、Exchange サービス エンドポイントを取得するクラスをプロジェクトに追加する必要があります。

####Office365ClientFetcher クラスおよびヘッダー ファイルを作成するには
1. SimpleMailApp プロジェクト フォルダーを右クリックし、**[New File]** を選択して、**[iOS]** セクションの **[Cocoa Touch Class]** をクリックし、**[Next]** をクリックします。

2. **[Class]** として「**Office365ClientFetcher**」を指定し、**[Subclass of:]** に **[NSObject]** を指定して、**[Next]** をクリックします。

3. **[Create]** をクリックして、クラスおよびヘッダー ファイルを作成します。

####Office365ClientFetcher ヘッダー ファイルをコーディングするには
1. Office365ClientFetcher.h に次のコード ディレクティブを追加して、必要な Office 365 iOS SDK ヘッダー ファイルをインポートします。

    ```objective-c
#import <Foundation/Foundation.h>
#import <office365_odata_base/office365_odata_base.h>
#import <office365_exchange_sdk/office365_exchange_sdk.h>
#import "MSDiscoveryClient.h"
#import <MSOutlookServicesClient.h>
```

2. Outlook および探索サービス クライアントを取得するためのメソッドを宣言します。

    ```objective-c
-(void)fetchOutlookClient:(void (^)(MSOutlookServicesClient *outlookClient))callback;
-(void)fetchDiscoveryClient:(void (^)(MSDiscoveryClient *discoveryClient))callback;
```

####Office365ClientFetcher クラスをコーディングするには

1. Office365ClientFetcher および AuthenticationManager ヘッダー ファイルをインポートします。

	```objective-c
#import "Office365ClientFetcher.h"
#import "AuthenticationManager.h"
```

5. Office365ClientFetcher クラスに実装コードを追加します。

	```objective-c
-(void)fetchOutlookClient:(void (^)(MSOutlookServicesClient *outlookClient))callback
{
    // Get an instance of the authentication controller.
    AuthenticationManager *authenticationManager = [AuthenticationManager sharedInstance];
    [authenticationManager acquireAuthTokenWithResourceId:@"https://outlook.office365.com/"
                                           completionHandler:^(BOOL authenticated) {
        if(authenticated){
            NSUserDefaults *userDefaults = [NSUserDefaults standardUserDefaults];
            NSDictionary *serviceEndpoints = [userDefaults objectForKey:@"O365ServiceEndpoints"];
            // Gets the MSOutlookServicesClient with the URL for the Mail service.
            callback([[MSOutlookServicesClient alloc] initWithUrl:serviceEndpoints[@"Mail"]
                                       dependencyResolver:authenticationManager.dependencyResolver]);
        }
        else{
            //Display an alert in case of an error
            dispatch_async(dispatch_get_main_queue(), ^{
                NSLog(@"Error in the authentication");
                UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Error"
                                                                message:@"Authentication failed. Check the log for errors."
                                                               delegate:self
                                                      cancelButtonTitle:@"OK"
                                                      otherButtonTitles:nil];
                [alert show];
            });
        }
    }];
}
-(void)fetchDiscoveryClient:(void (^)(MSDiscoveryClient *discoveryClient))callback
{
    AuthenticationManager *authenticationManager = [AuthenticationManager sharedInstance];
    [authenticationManager acquireAuthTokenWithResourceId:@"https://api.office.com/discovery/"
                                        completionHandler:^(BOOL authenticated) {
        if (authenticated) {
            callback([[MSDiscoveryClient alloc] initWithUrl:@"https://api.office.com/discovery/v1.0/me/"
                                         dependencyResolver:authenticationManager.dependencyResolver]);
        }
        else {
            dispatch_async(dispatch_get_main_queue(), ^{
                NSLog(@"Error in the authentication");
                UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Error"
                                                                message:@"Authentication failed. This may be because the Internet connection is offline  or perhaps the credentials are incorrect. Check the log for errors and try again."
                                                               delegate:self
                                                      cancelButtonTitle:@"OK"
                                                      otherButtonTitles:nil];
                [alert show];
            });
        }
    }];
}
```

###ビュー コントローラー
次に、Office 365 サービスに接続し、認証をトリガーしてから、結果を UI に表示するメソッドを呼び出す必要があります。ここで、Office 365 iOS Connect サンプルのビュー コントローラーおよび UI コントロールと同じ名前を使用して、UI コントロールを含むビュー コントローラー クラスを作成します。これにより、Connect サンプルからストーリー ボードをダウンロードして使用できるため、ストーリー ボードをビュー コントローラーに接続するために通常必要な手順をスキップできます。

####ビュー コントローラーを追加するには
1. SimpleMailApp プロジェクトを右クリックし、**[New File]** を選択し、**[iOS]** セクションの **[Cocoa Touch Class]** をクリックして、**[Next]** をクリックします。

2. **[Subclass of:]** に **[UIViewController]** を選択し、**[Class]** として 「**SendMailViewController**」を指定して、**[Next]** をクリックします。

3. **[Create]** をクリックして、クラスおよびヘッダー ファイルを作成します。 

####ビュー コントローラーをコーディングするには
初めに、SendMailViewController.m に次のコード ディレクティブを追加して、必要なヘッダー ファイルをインポートします。

```objective-c
#import "SendMailViewController.h"
#import "Office365ClientFetcher.h"
#import "AuthenticationManager.h"
#import "MSDiscoveryServiceInfoCollectionFetcher.h"
```

次に、**SendMailViewController** インターフェイスで次のプロパティとメソッドを宣言します。

```objective-c
@interface SendMailViewController ()

@property (weak, nonatomic) IBOutlet UILabel *headerLabel;
@property (weak, nonatomic) IBOutlet UITextView *mainContentTextView;
@property (weak, nonatomic) IBOutlet UITextView *statusTextView;
@property (weak, nonatomic) IBOutlet UITextField *emailTextField;
@property (weak, nonatomic) IBOutlet UIBarButtonItem *disconnectBarButtonItem;
@property (weak, nonatomic) IBOutlet UIButton *sendMailButton;
@property (weak, nonatomic) IBOutlet UIActivityIndicatorView *activityIndicator;

@property (strong, nonatomic) Office365ClientFetcher *baseController;
@property (strong, nonatomic) NSMutableDictionary *serviceEndpointLookup;

-(IBAction)sendMailTapped:(id)sender;
-(IBAction)disconnectTapped:(id)sender;

@end
```

**SendMailViewController** 実装に次のコードを追加します。

```objective-c
@implementation SendMailViewController

#pragma mark - Properties
-(Office365ClientFetcher *)baseController
{
    if (!_baseController) {
        _baseController = [[Office365ClientFetcher alloc] init];
    }

    return _baseController;
}

#pragma mark - Lifecycle Methods
- (void)viewDidLoad
{
    [super viewDidLoad];

    self.disconnectBarButtonItem.enabled = NO;
    self.sendMailButton.hidden = YES;
    self.emailTextField.hidden = YES;
    self.mainContentTextView.hidden = YES;
    self.headerLabel.hidden = YES;

    [self connectToOffice365];
}

#pragma mark - IBActions
//Send a mail message when the Send button is clicked
-(IBAction)sendMailTapped:(id)sender
{
    [self sendMailMessage];
}

// Clear the token cache and update the UI when the Disconnect button is tapped
-(IBAction)disconnectTapped:(id)sender
{
    self.disconnectBarButtonItem.enabled = NO;
    self.sendMailButton.hidden = YES;
    self.mainContentTextView.text = @"You're no longer connected to Office 365.";
    self.headerLabel.hidden = YES;
    self.emailTextField.hidden = YES;
    self.statusTextView.hidden = YES;

    // Clear the access and refresh tokens from the credential cache. You need to clear cookies
    // since ADAL uses information stored in the cookies to get a new access token.
    AuthenticationManager *authenticationManager = [AuthenticationManager sharedInstance];
    [authenticationManager clearCredentials];
}

#pragma mark - Helper Methods
-(void)connectToOffice365
{
    [self.baseController fetchDiscoveryClient:^(MSDiscoveryClient *discoveryClient) {
        MSDiscoveryServiceInfoCollectionFetcher *servicesInfoFetcher = [discoveryClient getservices];

        // Call the Discovery Service and get back an array of service endpoint information
        NSURLSessionTask *servicesTask = [servicesInfoFetcher readWithCallback:^(NSArray *serviceEndpoints, MSODataException *error) {
            if (serviceEndpoints) {

                self.serviceEndpointLookup = [[NSMutableDictionary alloc] init];

                for(MSDiscoveryServiceInfo *serviceEndpoint in serviceEndpoints) {
                    self.serviceEndpointLookup[serviceEndpoint.capability] = serviceEndpoint.serviceEndpointUri;
                }

                // Keep track of the service endpoints in the user defaults
                NSUserDefaults *userDefaults = [NSUserDefaults standardUserDefaults];

                [userDefaults setObject:self.serviceEndpointLookup
                                 forKey:@"O365ServiceEndpoints"];

                [userDefaults synchronize];

                dispatch_async(dispatch_get_main_queue(), ^{
                   
                    NSString *userEmail = [userDefaults stringForKey:@"LogInUser"];
                    NSArray *parts = [userEmail componentsSeparatedByString: @"@"];

                    self.headerLabel.text = [NSString stringWithFormat:@"Hi %@!", parts[0]];
                    self.headerLabel.hidden = NO;
                    self.mainContentTextView.hidden = NO;
                    self.emailTextField.text = userEmail;
                    self.statusTextView.text = @"";
                    self.disconnectBarButtonItem.enabled = YES;
                    self.sendMailButton.hidden = NO;
                    self.emailTextField.hidden = NO;
                });
            }
            else {
                dispatch_async(dispatch_get_main_queue(), ^{
                    NSLog(@"Error in the authentication: %@", error);

                    UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Error"
                                                                    message:@"Authentication failed. This may be because the Internet connection is offline  or perhaps the credentials are incorrect. Check the log for errors and try again."
                                                                   delegate:nil
                                                          cancelButtonTitle:@"OK"
                                                          otherButtonTitles:nil];
                    [alert show];
                });
            }
        }];
        
        [servicesTask resume];
    }];
}

-(void)sendMailMessage
{
    MSOutlookServicesMessage *message = [self buildMessage];

    [self.baseController fetchOutlookClient:^(MSOutlookServicesClient *outlookClient) {
        dispatch_async(dispatch_get_main_queue(), ^{
            // Show the activity indicator
            [self.activityIndicator startAnimating];
        });

        MSOutlookServicesUserFetcher *userFetcher = [outlookClient getMe];
        MSOutlookServicesUserOperations *userOperations = [userFetcher operations];

        // Send the mail message. This results in a call to the service.
        NSURLSessionTask *task = [userOperations sendMailWithMessage:message
                                                     saveToSentItems:YES
                                                            callback:^(int returnValue, MSODataException *error) {
            NSString *statusText;

            if (error == nil) {
                statusText = @"Check your inbox, you have a new message. :)";
            }
            else {
                statusText = @"The email could not be sent. Check the log for errors.";
                NSLog(@"%@",[error localizedDescription]);
            }

            // Update the UI.
            dispatch_async(dispatch_get_main_queue(), ^{
                self.statusTextView.text = statusText;
                [self.activityIndicator stopAnimating];
            });
        }];

        [task resume];
    }];
}

//Compose the mail message
-(MSOutlookServicesMessage *)buildMessage
{
    // Create a new message. Set properties on the message.
    MSOutlookServicesMessage *message = [[MSOutlookServicesMessage alloc] init];
    message.Subject = @"Welcome to Office 365 development on iOS with the Office 365 Connect sample";

    // Get the recipient's email address.
    // See the helper method getRecipients to understand the usage.
    NSString *toEmail = self.emailTextField.text;

    MSOutlookServicesRecipient *recipient = [[MSOutlookServicesRecipient alloc] init];

    recipient.EmailAddress = [[MSOutlookServicesEmailAddress alloc] init];
    recipient.EmailAddress.Address = [toEmail stringByTrimmingCharactersInSet:[NSCharacterSet whitespaceCharacterSet]];

    message.ToRecipients = (NSMutableArray<MSOutlookServicesRecipient> *)[[NSMutableArray alloc] initWithObjects:recipient, nil];

    // Get the email text and put in the email body.
    NSString *filePath = [[NSBundle mainBundle] pathForResource:@"EmailBody" ofType:@"html" ];
    NSString *body = [NSString stringWithContentsOfFile:filePath encoding:NSUTF8StringEncoding error:nil];
    message.Body = [[MSOutlookServicesItemBody alloc] init];
    message.Body.ContentType = MSOutlookServices_BodyType_HTML;
    message.Body.Content = body;

    return message;
}

@end
```
####Main.storyboard
作成したビュー コントローラーの名前は、 [Office 365 iOS Connect サンプル](https://github.com/OfficeDev/O365-iOS-Connect)の名前と同じであるため、サンプルと同じ名前を持つコントロールを指定するコードによって、サンプルのストーリーボードを使用できます。ストーリーボードを取得するには、サンプルを [ダウンロード](https://github.com/OfficeDev/O365-iOS-Connect/archive/master.zip)し、ダウンロードの O365-iOS-Connect-master\Sample\O365-iOS-Connect\Base.lproj フォルダー内の **Main.storyboard** ファイルを見つけます。

このファイルをプロジェクトの SimpleMailApp\SimpleMailApp\Base.lproj フォルダーにコピーし、その場所にある既存のバージョンのファイルを上書きします。

###電子メール ソース
最後の手順は、サンプルで電子メールの本文を作成するために使用する HTML ファイルを作成することです。HTML ファイルをプロジェクトに追加し、それに EmailBody.html という名前を付けます。必要に応じてそれをカスタマイズしたり、基本テスト用に次のコードを使用したりできます。

```html
<html>
    <body>
        <p>Sent from SimpleMailApp iOS sample</p>
    </body>
</html>
```

<a name="bk_test" />
##アプリのテスト
Xcode からアプリケーションを構築し、実行します。これにより、iOS シミュレータが起動して、**[Connect to Office 365]** リンクが表示されます。それをクリックすると、資格情報の入力を求められます。正常にログインしたら、ログインしたアカウントの電子メール アドレスを含むテキスト ボックスと **[Send]** リンクが表示されます。**[Send]** をクリックすると、テキスト ボックスで指定したアドレスに電子メール メッセージが送信されます。


## 次の手順

メール API を使用するアプリケーションを構築したところで、アプリで使用できる他の Office 365 REST API を調査できます。

- [Office 365 API リファレンス](..\api\api-catalog.md)


## その他の技術情報

- [Office 365 iOS SDK](https://github.com/OfficeDev/Office-365-SDK-for-iOS)
- [Office 365 開発環境のセットアップ](..\howto\setup-development-environment.md)
- [iOS 版 Office 365 Connect アプリ](https://github.com/OfficeDev/O365-iOS-Connect)
- [iOS 用 Office 365 コード スニペット](https://github.com/OfficeDev/O365-iOS-Snippets)





[!INCLUDE [iOS セクションの終了](../includes/controls/iossection.xml)]

[!INCLUDE [Android セクションの開始](../includes/controls/androidsection.xml)]

**Android アプリを開発するのではない場合** このページの右上隅にあるコントロールを使用して、開発するアプリの種類を選択してください。

## Office 365 API で Android アプリを作成する

Office 365 REST API でできることを少し体験するため、このトピックでは、Exchange Online サーバーから情報を取得する簡単なアプリケーションを作成する方法を示します。

![エミュレーター ウィンドウで実行中のアプリケーションのスクリーン ショット。使用可能なアクションがボタンに表示されます。](images\O365APIs_RunningApp1-50Percent.png)

最初の Office 365 アプリケーションに取り組む前に、注意する必要があるいくつかの 1 回限りのタスクがあり、さらにツールを用意していることを確認する必要があります。その後、最初のアプリに取り掛かることができます。


##始める前に

Office 365 API にアクセスするアプリケーションを作成する前に、開発者環境を設定する必要があります。これは、ツールと環境が正常であることを確認する 3 つの 1 回限りのタスクから構成されます。

1. アプリの作成に使用する Android アプリ開発環境を設定します。
1. Office 365 API にアクセスするために、ビジネス向け Office 365 サブスクリプションを取得します。
1. アプリを作成して管理できるように、Office 365 サブスクリプションを Azure AD と関連付けます。

まだこれらの手順のいずれかを実行する必要がある場合、セットアップの詳細については、「<a href="setup-development-environment?android">Office 365 開発環境のセットアップ</a>」を参照してください。


##アプリの作成と依存関係の追加
この手順では、アプリを作成し、Android 用 Office 365 SDK および Android 用 Azure Active Directory 認証ライブラリをプロジェクトに追加します。これらの両方のライブラリのソースは GitHub から入手でき、希望に応じて、Office 365 SDK のバイナリを [Bintray](https://bintray.com/msopentech/Maven/Office-365-SDK-for-Android/view) からダウンロードできます。


###Android Studio の依存関係の追加

Gradle の Android Studio のビルトイン サポートを使用して、アプリケーションの依存関係を管理できます。アプリケーションに依存関係を追加するには

1. Android Studio で、新しい Android アプリケーションを作成します。
1. アプリ モジュールで `app/build.gradle` を見つけて、次の依存関係を追加します。”outlook-services” のみを使用しますが、参考のため、ここにそれらをすべて一覧表示しています。

```no-highlight
dependencies {
    // base OData library:
    compile group: 'com.microsoft.services', name: 'odata-engine-core', version: '+'
    compile group: 'com.microsoft.services', name: 'odata-engine-android-impl', version: '+', ext:'aar'

    // choose the discovery and outlook services
    compile group: 'com.microsoft.services', name: 'discovery-services', version: '+'
    compile group: 'com.microsoft.services', name: 'outlook-services', version: '+'

    // Azure Active Directory Authentication Library
    compile group: 'com.microsoft.aad', name: 'adal', version: '+'
    }
```


###Eclipse の依存関係の追加

Eclipse を使用している場合、Android 用 Azure Active Directory 認証ライブラリと Office 365 SDK ライブラリをワークスペースに追加するために、もう少し手動で作業を行う必要があります。依存関係をワークスペースに追加するには 

1. [Azure Active Directory 認証ライブラリ](https://github.com/AzureAD/azure-activedirectory-library-for-android)をダウンロードするか、クローンを作成します。

1. Eclipse を起動して、アプリ用に新しいワークスペースを作成します。

1. Active Directory Azure ライブラリから AuthenticationActivity プロジェクトを新しいワークスペースにインポートします。

1. Android サポート ライブラリを AuthenticationActivity プロジェクトに追加します。これを実行するには、プロジェクトを右クリックし、**[Android ツール]**、**[サポートのライブラリの追加]** の順に選択します。

1.  [gson ライブラリ](https://code.google.com/p/google-gson/)の最新バージョンをダウンロードします。

1. gson jar ファイルを AuthenticationActivity プロジェクトの libs フォルダーに追加します。

1. Android 用 Office 365 SDK から jar ファイルを追加します。Bintray から jar ファイルをダウンロードするか、または Android 用 Office 365 SDK のクローンを作成して構築し、jar ファイルをプロジェクトにコピーします。

	**Jar ファイルをダウンロードするには**

   [Bintray](https://bintray.com/msopentech/Maven/Office-365-SDK-for-Android/view) から [Android 用 Office 365 SDK](https://github.com/OfficeDev/Office-365-SDK-for-Android) の Jar ファイルをダウンロードします。次に示す jar ファイルを [libs] フォルダーに追加する必要があります。
    * odata-engine-android-impl-0.11.1jar
    * outlook-services-0.11.1.jar
    * file-services-0.11.1.jar
    * discovery-services-0.11.1.jar
	<br />**注意** You can use version 0.11.1 or later of the jars.
	
	**Jar ファイルを作成するには**
	
	1. [Android 用 Office 365 SDK](https://github.com/OfficeDev/Office-365-SDK-for-Android) のクローンを作成します。
	2. sdk ディレクトリに移動します。
	3. `.\gradlew clean` を実行します。
	4. `.\gradlew assemble` を実行します。

###マニフェストの更新
アプリケーションが Azure AD と Office 365 にアクセスできるように、**AndroidManifest.xml** ファイルに 2 つのアクセス許可を追加する必要があります。

    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />

###Office 365 サービスの統合
Office 365 アプリケーションのコードの作成を開始する前に、アプリケーションを Azure AD に登録して、アプリケーションが Office 365 サービスにアクセスするためのアクセス許可を設定する簡単な作業を実行する必要があります。これはアプリケーションごとに 1 回だけ実行する必要があります。


####Azure AD にアプリを登録する
1. Office 365 開発者向けサイト資格情報を使用して、 [Azure 管理ポータル](https://manage.windowsazure.com/)にサインインします。

1. 左側のメニューで **[Active Directory]** をクリックしてから、Office 365 開発者向けサイトのディレクトリをクリックします。

![A screenshot of the Azure Management Portal website. The item 'Active Directory' is selected in the left navigation pane. In the main pane, the Directory tab is selected. The name of the current directory is highlighted.](images\O365APIs_RegisterApp_1.png)

1. 上部のメニューで、**[アプリケーション]** をクリックします。

1. 下部のメニューから、**[追加]** をクリックします。

![A screenshot of the directory information page. In the menu bar at the bottom of the page, the New icon is highlighted.](images\O365APIs_RegisterApp_2.png)
1. **[何を行いますか]** ページで、**[所属組織が開発しているアプリケーションの追加]** をクリックします。

1. **[アプリケーションについてお聞かせください]** ページで、アプリケーションに名前を付け、**[種類]** に **[ネイティブ クライアント アプリケーション]** を選択します。

1. ページの右下隅にある矢印アイコンをクリックします。

1. **[アプリケーション情報]** ページで、リダイレクト URI を指定します。この例では **http://localhost/simplemailapp** を指定してから、続いて、ページの右下隅にあるチェック ボックスをクリックします。

1. アプリケーションが正常に追加されたら、アプリケーションの [クイック スタート] ページが表示されます。 

アプリが Azure AD に登録されて、クライアント ID が割り当てられます。この情報は、 [アプリケーションをセットアップする](#bk_context)ときに必要になります。ただし、構成しなければならない、アプリの重要な側面がまだ残っています。	

####Office 365 サービスの追加とアクセス許可の設定

次に、アプリに必要な Office 365 API の適切なアクセス許可を指定する必要があります。そのために、アプリに必要な API が含まれる Office 365 サービスへのアクセスを追加してから、そのサービスの API から必要なアクセス許可を指定します。このサンプル アプリケーションでは、ユーザーの受信トレイを読み取る必要があるため、次のようにアクセス許可を設定します。

1. Azure 管理ポータルで、アプリケーションの構成ページの一番下までスクロールダウンし、**[他のアプリケーションへのアクセス許可]** で **[アプリケーションの追加]** を選択します。
 
![A screenshot of the app configuration page for your app, on the Azure Management Portal website. Under the page section titled 'permission to other applications', the 'add application' button is highlighted.](images\O365APIs_RegisterApp_4.png)

1. **[Office 365 Exchange Onlin]** サービスを選択します。
	
	1. サービス名を選択し、プラス記号をクリックしてサービスを追加します。 
	1. 追加したサービスが **[選択済み]** 列にリストされます。 
	1. チェック マーク アイコンをクリックして、選択を保存してください。
![A screenshot of the 'permissions to other applications' page. The available applications are listed in a table. Next to the name of each is the plus icon. At the far right is a column that lists the applications you have added to your app. A check mark icon at the bottom of the page is highlighted.](images\O365APIs_RegisterApp_5.png)

	アプリの構成ページが再表示されます。

1. **[他のアプリケーションに対するアクセス許可]** で、**[Office 365 Exchange Online]** の **[デリゲートされたアクセス許可]** 列をクリックし、**[Read users' mail]** を指定します。

![A screenshot of the app configuration page for your app, on the Azure Management Portal website. Under the page section titled 'permission to other applications', the application you have just added are listed in a table. Next to the name of each application is a column titled 'Delegated permissions'. This column displays a drop-down menu of the permissions you can request for your app from each application you added.](images\GetStarted_DelegatedPermissions.png)

	指定したアクセス許可は、アプリのユーザーに対し、Azure がアプリのアクセス許可要求に同意するように求める場合に表示されます。一般に、アプリが実際に必要とするサービスのみを要求し、アプリがその機能を実行できる各サービスの最低レベルのアクセス許可を指定します。


## アプリのコーディング
次のサンプル コードは、Android アプリケーションを作成するために必要なすべての Azure AD および Office 365 機能を使用する簡単なアプリケーションです。 

###Office 365 への接続
Office 365 サービスを呼び出す前に、Azure AD によってアプリケーションを認証する必要があります。Azure Active Directory での認証の詳細については、Azure.com の「 [Azure Active Directory とは](https://azure.microsoft.com/en-us/documentation/articles/active-directory-whatis/)」を参照してください。

次のコードは、Azure Active Directory 認証ライブラリの 2 つのオブジェクトを使用して、アプリケーションの認証を管理する **AuthenticationController** オブジェクトを定義しています。 

最初の **ADALDependencyResolver** では、依存関係の挿入を使用して、アプリケーションの残りの部分で **AuthenticationController** オブジェクトにアクセスできるようにします。2 つ目の **AuthenticationContext** では Azure AD による認証を管理します。これは次を実行します。

1. ユーザーが初めて Office 365 にログインしたときに、そのユーザーに認証 UI を表示します。

2. サービス クライアントがトークンにアクセスできるように、アプリケーションのアクセス トークンを保存します。

3. 必要に応じて、アクセス トークンを更新します。必要な場合は、再度認証 UI を表示します。

アプリケーションを実行すると、トークンに関する情報がアプリケーション ログに送信されます。この情報は通常、表示されずに、アプリケーション内で使用されます。 

    package com.microsoft.office365.auth_discover_snack;

    import android.app.Activity;
    import android.util.Log;

    import com.google.common.util.concurrent.SettableFuture;
    import com.microsoft.aad.adal.AuthenticationCallback;
    import com.microsoft.aad.adal.AuthenticationContext;
    import com.microsoft.aad.adal.AuthenticationResult.AuthenticationStatus;
    import com.microsoft.aad.adal.PromptBehavior;
    import com.microsoft.services.odata.impl.ADALDependencyResolver;
    import com.microsoft.services.odata.interfaces.DependencyResolver;

    public class AuthenticationController {
        private AuthenticationContext authContext;
        private ADALDependencyResolver dependencyResolver;
        private Activity contextActivity;
        private String resourceId;

        public static synchronized AuthenticationController getInstance() {
            if (INSTANCE == null) {
                INSTANCE = new AuthenticationController();
            }
            return INSTANCE;
        }
        private static AuthenticationController INSTANCE;

    private AuthenticationController() {
        resourceId = Constants.DISCOVERY_RESOURCE_ID;
    }

    public void setContextActivity(final Activity contextActivity) {
        this.contextActivity = contextActivity;
    }

    public void setResourceId(final String resourceId) {
        this.resourceId = resourceId;
        this.dependencyResolver.setResourceId(resourceId);
    }

    public SettableFuture<Boolean> initialize() {

        final SettableFuture<Boolean> result = SettableFuture.create();

        if (verifyAuthenticationContext()) {
            getAuthenticationContext().acquireToken(
                    this.contextActivity,
                    this.resourceId,
                    Constants.CLIENT_ID,
                    Constants.REDIRECT_URI,
                    PromptBehavior.Auto,
                    new AuthenticationCallback<AuthenticationResult>() {

                        @Override
                        public void onSuccess(final AuthenticationResult authenticationResult) {
                            if (authenticationResult != null && authenticationResult.getStatus() == AuthenticationStatus.Succeeded) {
                                dependencyResolver = new ADALDependencyResolver(
                                        getAuthenticationContext(),
                                        resourceId,
                                        Constants.CLIENT_ID);
                                Log.i("AuthenticationController", "initialize - Token acquired\n" +
                                                "    Token info:\n" +
                                                "      TenantId - " + authenticationResult.getTenantId() + "\n" +
                                                "      AccessToken - " + authenticationResult.getAccessToken() + "\n" +
                                                "      AccessTokenType - " + authenticationResult.getAccessTokenType() + "\n" +
                                                "      RefreshToken - " + authenticationResult.getRefreshToken() + "\n" +
                                                "      ExpiresOn - " + authenticationResult.getExpiresOn() + "\n" +
                                                "      IsMultiResourceRefreshToken - " + authenticationResult.getIsMultiResourceRefreshToken() + "\n" +
                                                "      IdToken - " + authenticationResult.getIdToken() + "\n" +
                                                "    User info:\n" +
                                                "      DisplayableId - " + authenticationResult.getUserInfo().getDisplayableId() + "\n" +
                                                "      UserId - " + authenticationResult.getUserInfo().getUserId() + "\n" +
                                                "      FamilyName - " + authenticationResult.getUserInfo().getFamilyName() + "\n" +
                                                "      GivenName - " + authenticationResult.getUserInfo().getGivenName()
                                );
                                result.set(true);
                            }
                        }

                        @Override
                        public void onError(Exception t) {
                            Log.e("AuthenticationController", "initialize - " + t.getMessage());
                            result.setException(t);
                        }
                    }
            );
        } else {
            result.setException(new Throwable("Auth context verification failed. Did you set a context activity?"));
        }
        return result;
    }

    public AuthenticationContext getAuthenticationContext() {
        if (authContext == null) {
            try {
                authContext = new AuthenticationContext(this.contextActivity, Constants.AUTHORITY_URL, false);
            } catch (Throwable t) {
                Log.e("AuthenticationController", "getAuthenticationContext - " + t.toString());
            }
        }
        return authContext;
    }

    public DependencyResolver getDependencyResolver() {
        return getInstance().dependencyResolver;
    }

    private boolean verifyAuthenticationContext() {
        if (this.contextActivity == null) {
            Log.e("AuthenticationController", "verifyAuthenticationContext - " + "Must set context activity");
            return false;
        }
        return true;
    }
    }

###探索サービスの使用

探索サービスは、アプリケーションに Office 365 サービスのエンドポイントの場所を提供します。プロバイダー、API バージョン、サービス エンドポイント URI、およびその他の情報を取得できます。

**DiscoveryController** オブジェクトは、Android 用 Office 365 SDK の **DiscoveryClient** オブジェクトを使用して、Office 365からサービスの一覧を取得します。この一覧はアプリケーションのログ ファイルに送信されますが、アプリケーションではサービスの一覧を使用して、Office 365 サービスのエンドポイントを検索します。

    package com.microsoft.office365.auth_discover_snack;

    import android.util.Log;

    import com.google.common.util.concurrent.FutureCallback;
    import com.google.common.util.concurrent.Futures;
    import com.google.common.util.concurrent.ListenableFuture;
    import com.google.common.util.concurrent.SettableFuture;
    import com.microsoft.discoveryservices.ServiceInfo;
    import com.microsoft.discoveryservices.odata.DiscoveryClient;
    import com.microsoft.services.odata.impl.ADALDependencyResolver;

    import java.util.List;

    public class DiscoveryController {
    private List<ServiceInfo> mServices;

    public static synchronized DiscoveryController getInstance() {
        if (INSTANCE == null) {
            INSTANCE = new DiscoveryController();
        }
        return INSTANCE;
    }
    private static DiscoveryController INSTANCE;

    public SettableFuture<Boolean> initialize() {

        final SettableFuture<Boolean> result = SettableFuture.create();

        AuthenticationController.getInstance().setResourceId(Constants.DISCOVERY_RESOURCE_ID);
        ADALDependencyResolver dependencyResolver = (ADALDependencyResolver) AuthenticationController
                .getInstance().getDependencyResolver();

        DiscoveryClient discoveryClient = new DiscoveryClient(Constants.DISCOVERY_RESOURCE_URL, dependencyResolver);

        try {
            ListenableFuture<List<ServiceInfo>> services = discoveryClient.getservices().read();
            Futures.addCallback(services,
                    new FutureCallback<List<ServiceInfo>>() {
                        @Override
                        public void onSuccess(final List<ServiceInfo> services) {
                            getInstance().mServices = services;
                            StringBuilder servicesLogDescription = new StringBuilder();
                            servicesLogDescription.append("initialize - Services discovered\n");
                            servicesLogDescription.append("    Service info:\n");

                            String serviceProperty;
                            for (ServiceInfo service : services){
                                serviceProperty = "      ServiceName - " + service.getserviceName() + "\n";
                                servicesLogDescription.append(serviceProperty);
                                serviceProperty = "      ServiceId - " + service.getserviceId() + "\n";
                                servicesLogDescription.append(serviceProperty);
                                serviceProperty = "      Capability - " + service.getcapability() + "\n";
                                servicesLogDescription.append(serviceProperty);
                                serviceProperty = "      EntityKey - " + service.getentityKey() + "\n";
                                servicesLogDescription.append(serviceProperty);
                                serviceProperty = "      ProviderName - " + service.getproviderName() + "\n";
                                servicesLogDescription.append(serviceProperty);
                                serviceProperty = "      ProviderId - " + service.getproviderId() + "\n";
                                servicesLogDescription.append(serviceProperty);
                                serviceProperty = "      ServiceApiVersion - " + service.getserviceApiVersion() + "\n";
                                servicesLogDescription.append(serviceProperty);
                                serviceProperty = "      ServiceEndpointUri - " + service.getserviceEndpointUri() + "\n";
                                servicesLogDescription.append(serviceProperty);
                                serviceProperty = "      ServiceResourceId - " + service.getserviceResourceId() + "\n";
                                servicesLogDescription.append(serviceProperty);
                                serviceProperty = "      ServiceAccountType - " + service.getserviceAccountType() + "\n";
                                servicesLogDescription.append(serviceProperty);
                            }

                            Log.i("DiscoveryController", servicesLogDescription.toString());
                            result.set(true);
                        }

                        @Override
                        public void onFailure(final Throwable t) {
                            Log.e("DiscoveryController", "discoverServices - " + t.getMessage());
                            result.setException(t);
                        }
                    });
        } catch (Exception e) {
            Log.e("DiscoveryController", "discoverServices - " + e.getMessage());
            result.setException(e);
        }
        return result;
    }

    public ServiceInfo getService(String capability) {
        if (mServices == null)
            throw new NullPointerException("Services have not been discovered. "
                    + "Use discoverServices function first.");
        for (ServiceInfo service : mServices)
            if (service.getcapability().equals(capability))
                return service;

        return null;
    }
    }


###Office 365 API データへのアクセス

Azure AD による認証を管理する **AuthenticationController** と、Office 365 サービスのエンドポイントを提供する **DiscoveryController** によって、アプリケーションは、Office 365 から情報を取得する呼び出しを開始できます。 

**MailController** オブジェクトは両方を使用して、ユーザーの受信トレイ内の電子メールの一覧を取得します。今回も、この簡単なアプリケーションは、件名行をアプリケーション ログ ファイルに書き込むだけです。

    package com.microsoft.office365.auth_discover_snack;

    import android.util.Log;

    import com.google.common.util.concurrent.FutureCallback;
    import com.google.common.util.concurrent.Futures;
    import com.google.common.util.concurrent.ListenableFuture;
    import com.google.common.util.concurrent.SettableFuture;
    import com.microsoft.discoveryservices.ServiceInfo;
    import com.microsoft.outlookservices.Message;
    import com.microsoft.outlookservices.odata.OutlookClient;
    import com.microsoft.services.odata.impl.ADALDependencyResolver;

    import java.util.List;

    public class MailController {

    public static synchronized MailController getInstance() {
        if (INSTANCE == null) {
            INSTANCE = new MailController();
        }
        return INSTANCE;
    }
    private static MailController INSTANCE;

    public SettableFuture<Boolean> initialize() {

        final SettableFuture<Boolean> result = SettableFuture.create();

        ServiceInfo service = DiscoveryController.getInstance().getService(Constants.MAIL_CAPABILITY);

        AuthenticationController.getInstance().setResourceId(service.getserviceResourceId());
        ADALDependencyResolver dependencyResolver = (ADALDependencyResolver) AuthenticationController
                .getInstance().getDependencyResolver();

        OutlookClient mailClient = new OutlookClient(service.getserviceEndpointUri(), dependencyResolver);

        try {
            ListenableFuture<List<Message>> mailItems = mailClient
                    .getMe()
                    .getFolder("Inbox")
                    .getMessages()
                    .read();
            Futures.addCallback(mailItems,
                    new FutureCallback<List<Message>>() {
                        @Override
                        public void onSuccess(final List<Message> mailItems) {
                            StringBuilder mailItemsLogDescription = new StringBuilder();
                            mailItemsLogDescription.append("initialize - Mail retrieved\n");
                            mailItemsLogDescription.append("    Mail items::\n");

                            String mailItemSubject;
                            for (Message mailItem : mailItems){
                                mailItemSubject = "      Subject: " + mailItem.getSubject() + "\n";
                                mailItemsLogDescription.append(mailItemSubject);
                            }

                            Log.i("MailController", mailItemsLogDescription.toString());
                            result.set(true);
                        }

                        @Override
                        public void onFailure(final Throwable t) {
                            Log.e("MailController", "initialize - " + t.getMessage());
                            result.setException(t);
                        }
                    });
        } catch (Exception e) {
            Log.e("MailController", "initialize - " + e.getMessage());
            result.setException(e);
        }
        return result;
    }
    }


###コンテキストとアプリケーションのアクティビティ

この最初の Office 365 アプリケーションに必要なクラスは他に 2 つあります。1 つ目はアプリケーションを定義する情報を格納する定数クラスです。最初の 3 つの定数は、Azure AD 認証と Office 365 探索サービスのエンドポイントを提供します。次の 2 つの定数は、 [アプリを Azure AD に登録した](#bk_register)ときに作成した識別情報です。最後の定数は、アプリが Office 365 メール サービスにアクセスする必要があることを指定します。

    package com.microsoft.office365.auth_discover_snack;

    interface Constants {
    public static final String AUTHORITY_URL = "https://login.microsoftonline.com/common";
    public static final String DISCOVERY_RESOURCE_URL = "https://api.office.com/discovery/v1.0/me/";
    public static final String DISCOVERY_RESOURCE_ID = "https://api.office.com/discovery/";
    public static final String CLIENT_ID = "<Assigned by Azure AD when you registered your app.>";
    public static final String REDIRECT_URI = "<Defined when you registered your app.>";

    public static final String MAIL_CAPABILITY = "Mail";
    
    }

2 つ目のクラスは、アプリケーションの UI を提供するアクティビティ クラスです。UI は 3 つのボタンで構成されます。それぞれ、アプリを認証するためのボタン、探索サービスからエンドポイント情報を取得するためのボタン、および電子メール サービスから受信トレイ内の電子メールを取得するためのボタンです。他に必要なメソッドは、**AuthenticationActivity** が完了したときに呼び出される **onActivityResult** メソッドだけです。

    package com.microsoft.office365.auth_discover_snack;

    import android.content.Intent;
    import android.os.Bundle;
    import android.support.v7.app.ActionBarActivity;
    import android.util.Log;
    import android.view.View;

    import com.google.common.util.concurrent.FutureCallback;
    import com.google.common.util.concurrent.Futures;
    import com.google.common.util.concurrent.SettableFuture;

    public class MainActivity extends ActionBarActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }

    public void onAuthenticateButtonClick(View v){
        AuthenticationController.getInstance().setContextActivity(this);
        SettableFuture<Boolean> authenticated = AuthenticationController.getInstance().initialize();

        Futures.addCallback(authenticated, new FutureCallback<Boolean>() {
            @Override
            public void onSuccess(Boolean result) {
                Log.i("MainActivity", "onAuthenticateButtonClick - Authentication successful");
            }
            @Override
            public void onFailure(final Throwable t) {
                Log.e("MainActivity", "onAuthenticateButtonClick - " + t.getMessage());
            }
        });
    }

    public void onDiscoverButtonClick(View v){
        SettableFuture<Boolean> discovered = DiscoveryController.getInstance().initialize();

        Futures.addCallback(discovered, new FutureCallback<Boolean>() {
            @Override
            public void onSuccess(Boolean result) {
                Log.i("MainActivity", "onDiscoverButtonClick - Services discovered");
            }
            @Override
            public void onFailure(final Throwable t) {
                Log.e("MainActivity", "onDiscoverButtonClick - " + t.getMessage());
            }
        });
    }

    public void onGetMailButtonClick(View v) {
        SettableFuture<Boolean> mailRetrieved = MailController.getInstance().initialize();

        Futures.addCallback(mailRetrieved, new FutureCallback<Boolean>() {
            @Override
            public void onSuccess(Boolean result) {
                Log.i("MainActivity", "onGetMailButtonClick - Mail retrieved");
            }
            @Override
            public void onFailure(final Throwable t) {
                Log.e("MainActivity", "onGetMailButtonClick - " + t.getMessage());
            }
        });
    }

    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        Log.i("MainActivity", "onActivityResult - AuthenticationActivity has come back with results");
        super.onActivityResult(requestCode, resultCode, data);
        AuthenticationController
                .getInstance()
                .getAuthenticationContext()
                .onActivityResult(requestCode, resultCode, data);
    }
    }

###UI

必要な最後の部分は、Azure AD および Office 365 サービスを実行するユーザー インターフェイスです。このレイアウトではそれだけを実行する 3 つのボタンを提供します。

    <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:tools="http://schemas.android.com/tools" android:layout_width="match_parent"
        android:layout_height="match_parent" android:paddingLeft="@dimen/activity_horizontal_margin"
        android:paddingRight="@dimen/activity_horizontal_margin"
        android:paddingTop="@dimen/activity_vertical_margin"
        android:paddingBottom="@dimen/activity_vertical_margin" tools:context=".MainActivity">

        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="AUTHENTICATE TO OFFICE 365"
            android:id="@+id/authenticateButton"
            android:layout_alignParentLeft="true"
            android:layout_alignParentStart="true"
            android:layout_marginTop="21dp"
            android:onClick="onAuthenticateButtonClick" />

        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="DISCOVERY SERVICES"
            android:id="@+id/discoverButton"
            android:layout_below="@+id/authenticateButton"
            android:layout_alignParentLeft="true"
            android:layout_alignParentStart="true"
            android:layout_marginTop="21dp"
            android:onClick="onDiscoverButtonClick" />

        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="GET MAIL"
            android:id="@+id/getMailButton"
            android:layout_below="@+id/discoverButton"
            android:layout_alignParentLeft="true"
            android:layout_alignParentStart="true"
            android:layout_marginTop="21dp"
            android:onClick="onGetMailButtonClick" />
    </RelativeLayout>

##アプリのテスト

IDE 内からアプリケーションを実行し、コードの動作を確認します。アプリケーションを起動すると、3 つのボタンが表示されます。各ボタンをクリックすると、IDE のログ ウィンドウに情報が送信されます。

![エミュレーター ウィンドウで実行中のアプリケーションのスクリーン ショット。使用可能なアクションがボタンに表示されます。](images\O365APIs_RunningApp1-50Percent.png)

**[Authenticate to Office 365]** ボタンをクリックすると、アプリは **AuthenticationController** を呼び出し、次にこれが Android 用 Azure Active Directory 認証ライブラリによって提供される認証ワークフローを呼び出します。有効なトークンを使用できない場合、Office 365 にログインするフォームが表示されます。

![Office 365 ログイン ページのスクリーン ショット。](images\O365APIs_RunningApp2-50Percent.png)

ログインした後に、**AuthenticationController** は認証トークンに関する情報をログ ウィンドウに書き込みます。

![Office 365 から返されたトークン情報のスクリーン ショット。情報には、トークン、有効期限、および電子メール アドレス、名字、名前など、ユーザーに関する情報が含まれます。](images\O365APIs_RunningApp3.png)

**[DISCOVER SERVICES]** ボタンをクリックして、Office 365 サーバーから利用できる REST サービス エンドポイントの一覧を取得します。

![Office 365 REST サービス一覧のスクリーン ショット。一覧には、サービス名、サービス ID、機能、エンティティ キー、プロバイダー名、プロバイダー ID、サービス API バージョン、サービス エンドポイント URI、サービス リソース ID、およびサービス アカウントの種類が含まれます。](images\O365APIs_RunningApp4.png)

**[Get mail]** ボタンをクリックして、ユーザーの受信トレイ内の電子メール メッセージの一覧を取得します。この例では、各電子メール メッセージの件名のみが一覧表示されます。

![ユーザーの受信トレイからの電子メール件名行の一覧のスクリーン ショット。](images\O365APIs_RunningApp5.png)



## 次の手順

Office 365 から電子メールを取得する簡単なアプリケーションを構築したところで、REST API 全体の使用方法を示すより複雑なアプリケーションを調べてみる準備ができました。

* [Android 版 Office 365 API スタート プロジェクト](https://github.com/OfficeDev/O365-Android-Start)


## その他の技術情報


-  [Office 365 プラットフォーム上での開発の概要](..\howto\platform-development-overview.md)
-  [Android 用 Office 365 コード サンプル](http://dev.office.com/code-samples?filters=Android)
    


[!INCLUDE [Android セクションの終了](../includes/controls/androidsection.xml)]

[!INCLUDE [JavaScript セクションの開始](../includes/controls/javascriptsection.xml)]

**Angular アプリを開発するのではない場合** 右上隅にあるコントロールを使用して、開発するアプリの種類を選択してください。

## Office 365 API で Angular アプリを作成する

 
Office 365 で最初の Angular アプリケーションに取り組む前に、<a href="setup-development-environment?javascript">Office 365 開発環境のセットアップ</a>や適切なツールの取得など、完了しておく必要があるいくつかの 1 回限りのタスクがあります。

セットアップ後に、簡単な [Angular](https://angularjs.org/) アプリ **SimpleMailApp** を作成する手順を説明します。このアプリでは、[JavaScript 用 Active Directory 認証ライブラリ (ADAL)](https://github.com/AzureAD/azure-activedirectory-library-for-js) を使用してユーザーを認証し、[cross-origin resource sharing (CORS)](.\create-web-apps-using-CORS-to-access-files-in-Office-365.md) を使用した REST 呼び出しによりユーザーの電子メールを取得します。

![アプリのスクリーン ショット](images/AngularGettingStartedScreenshot.png)

ADAL JS はシングルページ アプリケーション (SPA) と Angular を考慮して設計されたため、この記事は Angular フロント エンドに重点を置いていますこのライブラリは、SPA で必要とする相互作用パターンを想定しています。Angular を使用せずに、ライブラリを使用することもできますが、はるかに多くのコードを書く必要があります。ADAL JS を使用せずに Office 365 API を使用することに興味がある場合は、「 [CORS を使用して Office 365 API にアクセスする JavaScript Web アプリケーションの作成](.\create-web-apps-using-CORS-to-access-files-in-Office-365.md)」を参照してください。

## 始める前に

Office 365 API にアクセスするアプリケーションを作成する前に、開発者環境を設定する必要があります。これは、ツールと環境が正常であることを確認する 3 つの 1 回限りのタスクから構成されます。

1. IDE、Git、Node.js など、Angular アプリケーションを作成するためのツールをダウンロードします。 
2. Office 365 API にアクセスするために、ビジネス向け Office 365 サブスクリプションを取得します。
3. アプリを作成して管理できるように、Office 365 サブスクリプションを Azure AD と関連付けます。

まだこれらの手順のいずれかを実行する必要がある場合、セットアップの詳細については、「<a href="setup-development-environment?javascript">Office 365 開発環境のセットアップ</a>」を参照してください。

**注** Node.js と具体的にはそのパッケージ マネージャー (npm) が、この資料のプロジェクトの依存関係をインストールするために必要です。それは、Office 365 API による開発には必要ありません。

## アプリの作成と依存関係の追加 

この手順では、開発済みの空のプロジェクトを利用して、Angular アプリをセットアップし、JavaScript 用 Azure Active Directory 認証ライブラリ (ADAL) をそれに追加して、Azure 管理ポータルでそれを登録することによって Azure と通信するようにアプリケーションをセットアップします。

### SimpleMailApp プロジェクトのセットアップ

できるだけ早く作業を始められるように、作成済みの空のプロジェクトを利用します。このプロジェクトは [GitHub](https://github.com/OfficeDev/O365-Angular-GettingStarted) にあります。 

1. Git を使用して、コマンド ラインから空のプロジェクトのクローンを作成します。
 
    ```git clone https://github.com/OfficeDev/O365-Angular-GettingStarted.git```

    **注意** Git に慣れていない場合は、[GitHub](https://github.com/OfficeDev/O365-Angular-GettingStarted) から直接コードをダウンロードできます。

2.次に、新しいプロジェクト フォルダーに移動し、Node.js のパッケージ マネージャー npm を使用して、プロジェクトの依存関係をインストールします。

    ```cd O365-Angular-GettingStarted/Starter```    
    ```npm install```     

**注**: リポジトリには *Completed* フォルダーもあります。そのフォルダー内のプロジェクトは、Azure テナント情報で構成し、依存関係をインストールすると、デプロイする準備ができます。

### SimpleMailApp プロジェクトへの ADAL JS の追加

ADAL JS は、このようなシングルページ アプリケーション (SPA) での Azure AD ユーザーのサインオンと Azure AD によってセキュリティ保護された JavaScript Web API からの直接の使用の両方を完全にサポートする JavaScript ライブラリです。

ADAL.js には 2 つのファイルで 2 つの階層があります。

* **adal.js** すべての JavaScript アプリケーションで使用します。これには、ドメイン固有ロジック、OAuth2 メッセージの構築と解析、トークンのキャッシュなどの低レベル関数が含まれます。詳細については、「[ADAL JS v1 の概要](http://www.cloudidentity.com/blog/2015/02/19/introducing-adal-js-v1/)」を参照してください。
* **adal-angular.js** Angular アプリケーション用の adal.js と一緒に含まれます。Angular フレームワークと互換性のある特定の関数が含まれます。

これらのファイルの両方をプロジェクトに含め、Azure AD 認証のためのプログラミング モデルにアクセスし、この資料の後で呼び出す Outlook メール REST API のように、Azure AD によってセキュリティ保護された API を呼び出します。

CDN からビットを取得できます。

* [adal.min.js](https://secure.aadcdn.microsoftonline-p.com/lib/1.0.0/js/adal.min.js) ([ミニファイされていないバージョン](https://secure.aadcdn.microsoftonline-p.com/lib/1.0.0/js/adal.js))
* [adal-angular.min.js](https://secure.aadcdn.microsoftonline-p.com/lib/1.0.0/js/adal-angular.min.js) ([non-minified version](https://secure.aadcdn.microsoftonline-p.com/lib/1.0.0/js/adal-angular.js))

ADAL 依存関係を含める最も簡単な方法は、*Starter/public* にある **index.html** ファイル内の ```<head>`` タグの末尾の ```src``` 属性として、CDN リンクを使用した ```script``` タグを追加することです。

```html
<script src="https://secure.aadcdn.microsoftonline-p.com/lib/1.0.0/js/adal.min.js"></script>
<script src="https://secure.aadcdn.microsoftonline-p.com/lib/1.0.0/js/adal-angular.min.js"></script>
```

### Azure AD にアプリを登録する

Office 365 アプリケーションのコードの作成を開始する前に、アプリケーションを Azure Active Directory に登録して、アプリケーションで Office 365 サービスを使用するためのアクセス許可を設定する簡単な作業を実行する必要があります。これはアプリケーションごとに 1 回だけ実行する必要があります。

1.Office 365 ビジネス アカウント資格情報を使用して、[Azure 管理ポータル](https://manage.windowsazure.com/) にサインインします。

2.左側の列にある **Active Directory** ノードをクリックして、Office 365 サブスクリプションにリンクされているディレクトリを選択します。
    
![Azure Management Portal](images/O365APIs_RegisterApp_1.png)

3.**[アプリケーション]** タブを選択してから、画面の下部にある **[追加]** を選択します。
    
![Azure Management Portal Add Application](images/O365APIs_RegisterApp_2.png)

4.ポップアップ ウィンドウで、**[所属組織が開発しているアプリケーションの追加]** を選択します。矢印をクリックして続行します。

5.**SimpleMailApp** など、アプリの名前を選択して、[種類] として **[Web アプリケーションまたは Web API]** を選択します。矢印をクリックして続行します。

6.**[サインオン URL]** の値は、アプリケーションをホストする場所の URL です。サンプル プロジェクトには *http://127.0.0.1:8080/* を使用します。

7.**[アプリケーション ID/URI]** の値は、アプリケーションを識別する Azure AD の一意の識別子です。http://{your_subdomain}/SimpleMailApp を使用することができます。{your_subdomain} は、Office 365 ビジネス アカウントでサインアップするときに指定した .onmicrosoft のサブドメインです。次に、チェック マークをクリックしてアプリケーションをプロビジョニングします。

8.アプリケーションが正常に追加されたら、アプリケーションの [クイック スタート] ページに移動します。ここから、**[構成]** タブを選択します。

9.**[他のアプリケーションへのアクセス許可]** セクションにスクロールダウンして、**[アプリケーションの追加]** ボタンをクリックします。

![Add application for web app](images/GettingStartedJavaScriptAddPermissionsMail.png)

10.この資料では、ユーザーの電子メールを取得する方法を示すため、**Office 365 Exchange Online** アプリケーションを追加します。アプリケーションの行にあるプラス記号をクリックし、右上にあるチェック マークをクリックしてそれを追加します。次に、右下にあるチェック マークをクリックして続行します。

![Add Exchange permissions](images/O365APIs_RegisterApp_5.png)

11.**Office 365 Exchange Online** 行で、**[デリゲートされたアクセス許可]** を選択し、選択リストの **[ユーザー メールの読み取り]** を選択します。

![Add application for web app](images/GetStarted_DelegatedPermissions.png)

    指定したアクセス許可は、アプリのユーザーに対し、Azure がアプリのアクセス許可要求に同意するように求める場合に表示されます。一般に、アプリが実際に必要とするサービスのみを要求し、アプリがその機能を実行できる各サービスの最低レベルのアクセス許可を指定します。

12.**[保存]** をクリックして、アプリケーションの構成を保存します。

### OAuth 2.0 の暗黙的な付与フローを許可するようアプリを構成する

Office 365 API 要求のアクセス トークンを取得するため、アプリケーションは OAuth の暗黙的な付与フローを使用します。既定では OAuth の暗黙的な付与フローは許可されていないため、これを許可するようにアプリケーションのマニフェストを更新する必要があります。

1.Azure の管理ポータルで、アプリケーションのエントリの **[構成]** タブを選択します。

2.ドロワーの **[マニフェストの管理]** ボタンを使用して、アプリケーションのマニフェスト ファイルをダウンロードし、コンピューターに保存します。

3.テキスト エディターでマニフェスト ファイルを開きます。**oauth2AllowImplicitFlow** プロパティを検索します。既定では *false* に設定されています。*true* に変更してファイルを保存します。

4.**[マニフェストの管理]** ボタンを使用して、更新されたマニフェスト ファイルをアップロードします。

アプリを正常に作成し、Azure AD に登録しました。最後の手順は、Outlook メール REST API に対して要求するコードを追加することです。

## アプリのコーディング

続行する前に、プロジェクトの *Starter* ディレクトリにまだいて、**index.html** に **adal.js** と **adal-angular.js** を含めていることを確認します。その後、Azure AD による認証に進みます。  

### Azure AD による認証とアクセス トークンの取得

[JS 用 ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-js) は、アプリケーションで認証を管理するための機能を提供します。アクセスおよび更新トークンのキャッシュと取得にそれを使用できます。*public/scripts* の **app.js** ファイルのみ編集すれば、認証を機能させることができます。

初めに、8 行目の *app* モジュールに必須モジュールとして *AdalAngular* を追加します。

```javascript
angular
  .module('app', [
    'ngRoute', 
    'AdalAngular'
  ])
```

次に、13 行目のモジュールの config 関数の引数として渡して、*AdalAngular* が提供するサービスを構成します。さらにこの時点で、サービスを構成するために必要な Angular の *$http* モジュールも追加します。

```javascript
function config($routeProvider, $httpProvider, adalAuthenticationServiceProvider) {
```

次の手順は、ADAL API が公開され、アプリケーション用に構成できるように *config* 関数の *adalAuthenticationServiceProvider* を初期化することです。プロバイダーの初期化に必要な引数は、*tenant*、*clientId*、および *cacheLocation* の 3 つがあります。*tenant* 値は、Office 365 ビジネス アカウントにサインアップするときに指定した **.onmicrosoft** のサブドメインです。*clientId* は Azure 管理ポータルのアプリケーションのページの **[構成]** タブの下で見つかるアプリケーションのクライアント ID です。最後に、アプリを Internet Explorer で実行する場合、*localStorage* の値で *cacheLocation* を含めます。

```javascript
// テナント名とクライアント ID (Azure の管理ポータルにあります) で ADAL プロバイダーを初期化します。
adalAuthenticationServiceProvider.init(
  {
    tenant: '{your_subdomain}.onmicrosoft.com',
    clientId: 'XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX',
    cacheLocation: 'localStorage'
  },
  $httpProvider
  );
```

この時点で、*AdalAngular* モジュールを構成したので、それを使用して、アプリケーションでユーザーを認証できます。Angular アプリケーションでこれを行う効率的な方法は、ユーザーを認証する必要があるルートを設定することです。ここでの例では、認証されたユーザーのみがアプリを表示できるようにするため、*.when* メソッドに渡すオブジェクトに *requireADLogin* メンバーを追加することによって、*$routeProvider* 構成の root ルートを保護します。

```javascript
$routeProvider
  .when('/', {
    templateUrl: 'views/home.html',
    controller: 'HomeController',
    controllerAs: 'home',
    requireADLogin: true     // Designates that the user must be authenticated to view this page.
  })
  .otherwise({
    redirectTo: '/'
  });
```

### Office 365 からユーザーの電子メールを取得する

ユーザーを認証したので、認証トークンを使用して [メール](https://msdn.microsoft.com/ja-jp/office/office365/api/mail-rest-operations) REST API に対して HTTP 要求を行うことができます。[ファイル](https://msdn.microsoft.com/office/office365/APi/files-rest-operations) REST API、[予定表](https://msdn.microsoft.com/office/office365/APi/calendar-rest-operations) REST API、および [連絡先](https://msdn.microsoft.com/office/office365/APi/contacts-rest-operations) REST API の CORS サポートも利用できます。

ユーザーの電子メールを取得するためにメール REST API に対して HTTP 要求を行うには、クロス オリジン リソース共有 (CORS) を使用して、初期化時にメール REST API の URL を宣言する必要があります。これは ADAL JS がそれを信頼することがわかるようにするためです。これを行うには、*endpoints* オブジェクトを **app.js** の *adalAuthenticationServiceProvider.init* コードに渡します。追加する必要のあるエンドポイントは、*https://outlook.office365.com* です。

```javascript 
var endpoints = {
  'https://outlook.office365.com': 'https://outlook.office365.com'
};
	
// テナント名とクライアント ID (Azure の管理ポータルにあります) で ADAL プロバイダーを初期化します。
adalAuthenticationServiceProvider.init(
  {
    tenant: '{your_subdomain}.onmicrosoft.com',
    clientId: 'XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX',
    cacheLocation: 'localStorage',
    endpoints: endpoints
  },
  $httpProvider
  );
```

CORS を使用して、メール REST API に対してHTTP 要求を行うように ADAL JS を設定しています。あとは、ユーザーの電子メールを取得する HTTP 要求を行うだけです。これを **homeController.js** で行います。これは *public/controllers* フォルダーにあります。

空のコントローラーに次のコードを追加します。これは HTTP 要求をセットアップし、実行します。応答を受け取ると、応答からのデータをビュー モデルに添付し、それをビュー (**public/views/home.html**) からアクセスできるようにします。

```javascript
// 要求しているリソースの URL を渡します。
$http.get("https://outlook.office365.com/api/v1.0/me/messages")
  .then(function(response) {
    $log.debug('HTTP request to Mail API returned successfully.');
    vm.emails = response.data.value;
  }, function(error) {
    $log.error('HTTP request to Mail API failed.');
  });
```

最後に、受け取ったデータを表示するいくつかの HTML を追加しましょう。次のコードを *public/views* フォルダーにある **home.html** に追加します。

```html
<div class="container-fluid">
  <row>
    <div class="col-md-8 col-md-offset-2" style="margin-top: 20px;">
      <table class="table table-hover">
        <thead>
          <tr>
            <th>From</th>
            <th>Subject</th>
            <th>Body Preview</th>
            <th>Web Link</th>
          </tr>
        </thead>
        <tbody>
          <tr ng-repeat="email in home.emails">
            <td>{{email.Sender.EmailAddress.Name}} ({{email.Sender.EmailAddress.Address}})</td>
            <td>{{email.Subject}}</td>
            <td>{{email.BodyPreview}}</td>
            <td><a href={{email.WebLink}}>View on OWA</a></td>
          </tr>

          <tr ng-if="home.emails.length == 0">
            <td colspan="4" align="center">Your inbox is empty.</td>
          </tr>
        </tbody>
      </table>
    </div>
  </row>
</div>
```

## アプリのテスト

アプリケーションをテストし、Azure AD によって、ユーザーが正しく認証され、ユーザーの電子メールが取得されることを確認します。*Starter* ディレクトリ内で、コマンド プロンプトからコマンド `node server.js` を入力します。これにより、ポート 8080 でリッスンする開発サーバーが起動します。Web ブラウザーで *http://127.0.0.1:8080/* に移動します。手順を正しく実行していれば、アプリケーションによって Azure Active Directory サインイン ページが表示されるはずです。Office 365 資格情報 (**user@{your_subdomain}.onmicrosoft.com**) を使用してサインインし、電子メールを検索するアプリケーションに戻ることを確認します。

おめでとうございます。 これで、Office 365 API を使用して、Angular アプリを構築できました。

![App Screenshot](images/AngularGettingStartedScreenshot.png)

## 次の手順

メール API を使用するアプリを構築したところで、アプリで使用できる他の Office 365 REST API を調査できます。

- [Office 365 API reference](.\rest-api-overview.md)

## その他の技術情報

-  [Office 365 API スタート プロジェクトおよびコード サンプル](..\howto\Starter-projects-and-code-samples.md) 
-  [Office Developer on GitHub](https://github.com/OfficeDev)


[!INCLUDE [END JavaScript section](../includes/controls/javascriptsection.xml)]


[!INCLUDE [BEGIN ASP.NET MVC section](../includes/controls/aspnetmvcsection.xml)]

**ASP.NET MVC の Web アプリを開発するのではない場合** 右上隅にあるコントロールを使用して、開発するアプリの種類を選択してください。

## Office 365 API で ASP.NET MVC アプリを作成する


アプリを作成する場合、Office 365 とやり取りするように REST API に対して直接プログラミングすることも、Office Developer Tools for Visual Studio を使用することもできます。

Office Developer Tools for Visual Studio には、Office 365 API サービスにアクセスするためのクライアント ライブラリと UI の拡張機能が含まれます。.NET Framework および JavaScript クライアント ライブラリにより、任意のデバイスやプラットフォームから Office 365 REST API と簡単に対話できるようになります。Visual Studio UI 拡張機能により、Office 365 サービスをアプリ プロジェクトに簡単に追加できます。

このトピックでは、Visual Studio で、Office 365 API を使用する簡単な MVC (モデル ビュー コントローラー) Web アプリケーションの作成手順を説明します。これには次が含まれます。

1. [アプリの作成と依存関係の追加](#bk_createApp)
2. [アプリのコーディング](#bk_codeYourAppVS)
3. [アプリのテスト](#bk_test)




<a name="bk_prerequisites" />
##始める前に

Office 365 API にアクセスするアプリケーションを作成する前に、開発者環境を設定する必要があります。これは、ツールと環境が正常であることを確認する 3 つの 1 回限りのタスクから構成されます。

3.アプリの作成に使用する Visual Studio と Office Developer Tools をダウンロードします。
1.Office 365 API にアクセスするために、ビジネス向け Office 365 サブスクリプションを取得します。
2.アプリを作成して管理できるように、Office 365 サブスクリプションを Azure AD と関連付けます。

まだこれらの手順のいずれかを完了する必要がある場合、セットアップの詳細については、「<a href="setup-development-environment?aspnet">Office 365 開発環境のセットアップ</a>」を参照してください。





## アプリの作成と依存関係の追加

開発環境をセットアップしたら、Visual Studio を使用して、Office 365 API を使用する Web アプリケーションを作成する準備ができました。

これから構築する例は、Azure AD シングル サインオンを使用して認証し、Office 365 からユーザーの連絡先情報を 読み取るシングルテナント MVC です。モデルとして、GitHub 上の [Office 365 シングル テナント MVC プロジェクト](https://github.com/OfficeDev/O365-WebApp-SingleTenant) を使用します。

使用している Visual Studio のバージョンを選択してください。

[!INCLUDE [Add the VS versions filter bar](../includes/controls/vsversionsfilter.xml)]


1.Visual Studio で、**[ファイル]** > **[新規作成]** を選択します。
2.**[Visual C#]** > **[Web]** で、**[ASP.NET Web アプリケーション]** を選択します。プロジェクトの名前を付けて、**[OK]** を選択します。
3.**[MVC]** を選択します。
4.**[認証の変更] を選択し、**[認証なし]** を選択し、**[OK]** を 2 回選択して、プロジェクトを作成します。

Office 365 API サービスは、ユーザーの Office 365 データに対し、Azure AD を使用してセキュリティ保護された認証を行います。Office 365 API にアクセスするには、Azure AD にアプリを登録する必要があります。


### Office 365 サービスをプロジェクトに追加する際に、Visual Studio を利用してアプリを登録する

Visual Studio プロジェクトを作成する場合、Office 365 サービスをプロジェクトに追加すると、自動的にアプリの登録が処理されます。Office 365 サービスをプロジェクトに追加するプロセスでは、Visual Studio を使用して次の操作を実行できます。

[!INCLUDE [BEGIN VS2013 section](../includes/controls/vs2013section.xml)]

-  アプリが Web アプリケーションまたはネイティブ アプリのどちらであるかを指定して、アプリを Azure AD に登録する
-  アプリのプロパティ (アプリの名前、リダイレクト/応答エンドポイント、テナント範囲など) を構成する

[!INCLUDE [END VS2013 section](../includes/controls/vs2013section.xml)]

[!INCLUDE [BEGIN VS2015 section](../includes/controls/vs2015section.xml)]

-  Azure AD にアプリを登録する

[!INCLUDE [END VS2015 section](../includes/controls/vs2015section.xml)]

-  Office 365 サービスに接続する
-  Office 365 サービスでアプリに必要な API に対するアクセス許可レベルを指定する
-  アプリが接続する Office 365 サービスに応じて必要になる **NuGet packages** をプロジェクトに追加する

次のプロジェクト テンプレートでは、Office 365 API を接続先サービスとしてサポートしています。

- .NET Windows ストア 8.1 アプリ
- .NET Windows ストア 8.1 ユニバーサル アプリ
- .NET Windows Phone 8.1 アプリ
- .NET Windows Phone 8.1 Silverlight アプリ
- Windows フォーム アプリケーション
- WPF アプリケーション
- ASP.NET MVC Web Applications
- ASP.NET Web Forms Applications
- Xamarin Android および iOS アプリケーション
- マルチデバイス ハイブリッド (Cordova) アプリ



### Visual Studio サービス マネージャーを使用してアプリを登録し、Office 365 API をプロジェクトに追加する


Visual Studio の**サービス マネージャー**を使用して、Office 365 API を追加して構成します。

[!INCLUDE [BEGIN VS2013 section](../includes/controls/vs2013section.xml)]

1.**ソリューション エクスプローラー**で、プロジェクト ノードを選択します。
    
2.プロジェクト ノードを右クリックするか、マウスのボタンを押したままにして、**[追加]** > **[接続済みサービス]** を選択します。
    
3.アプリを登録します。
	
	**[サービス マネージャー]** ダイアログ ボックスの上部にある **[Office 365]** リンクをクリックして、**[アプリを登録する]** を選択します。Office 365 開発者の組織のテナント管理者アカウントでサインインします。

	これにより、Azure Active Directory へのアプリの登録が開始され、OAuth 経由でアプリを認証できます。
    
    Office 365 にログオンすると、利用可能な Office 365 API サービスの一覧が表示されます。Office 365 API の一覧も表示されます。

	各サービスの右側の **[アクセス許可]** 列が空であることがわかります。
    
4.接続する Office 365 API を選択し、それぞれのアクセス許可レベルを指定します。

	1.この例では、**[ユーザーとグループ]** を選択します。**[アクセス許可]** を選択します。

![A screenshot that shows the Services Manager dialog box with the Users and Groups service Office 365 API selected and the Permissions link highlighted.](..\howto\images\O365_RegisterApp_User.png)

	2.**[サインオンを有効にして、ユーザーのプロファイルを読み取る]** を選択し、**[適用]** を選択します。

![A screenshot that shows the Users and Groups Permissions dialog box with the Enable sign-on and read users' profile permission selected.](..\howto\images\O365_RegisterApp_UserPerms.png)

	1.次に、**[連絡先]** を選択します。**[アクセス許可]** を選択します。
	2.**[ユーザーの連絡先の読み取り]** を選択し、**[適用]** を選択します。
	
		これを実行すると、Visual Studio によって、選択した API を含む Office 365 サービスが Azure AD 内のアプリに追加され、API のアクセス許可レベルが指定したレベルに設定されます。

6.アプリのプロパティを設定します。
	
	**[サービス マネージャー]** ダイアログ ボックスで **[アプリのプロパティ]** を選択します。
	
	設定できるアプリのプロパティは、アプリ プロジェクトが Web サービスまたは Web アプリケーションであるか、あるいはネイティブ アプリケーション (携帯電話プロジェクトなど) であるかによって異なります。
	
	たとえば、ここで構築しているような Web アプリケーションの場合、このサンプル アプリを、開発者組織以外の Office 365 組織が使用できるようにすることができます。

	ここでは、アプリのプロパティは、既定のままにしておきます。
	
	**[サービス マネージャー]** ダイアログ ボックスに、以下が表示されます。
	
	- プロジェクトに追加するために選択したサービス
	- The permissions for each service. 
	
![A screenshot of the Services Manager dialog box after the Users and Groups and Contact APIs have been configured, showing that both APIs have Read permissions.](..\howto\images\O365_RegisterApp_ServiceManager.png)
	
6.**[OK]** をクリックします。

この時点で、Visual Studio は必要な **NuGet パッケージ**をプロジェクトに追加します。追加される NuGet パッケージは、追加する Office 365 API によって異なります。

[!INCLUDE [END VS2013 section](../includes/controls/vs2013section.xml)]

[!INCLUDE [BEGIN VS2015 section](../includes/controls/vs2015section.xml)]

1.**ソリューション エクスプローラー**で、Office 365 サービスを追加するプロジェクト ノードを選択します。
    
2.プロジェクト ノードを右クリックするか、マウスのボタンを押したままにして、**[追加]** > **[接続済みサービス]** を選択します。

3.**[Microsoft]** タブで、**[Office 365 API]** を選択し、**[構成]** を選択します。

	**Office 365 API サービスの構成** ダイアログ ボックスが表示されます。
    
3.アプリを登録します。
	
	**ドメインの選択** ページで、アプリを登録する Office 365 ドメインを入力するか選択します。例: <i>contoso.onmicrosoft.com</i>。**[次へ]** を選択します。現在まだサインインしていない場合は、Office 365 へのサインインが必要な場合があります。
	
	新しい Azure AD アプリケーションを作成するには、**アプリケーションの構成** ページで選択します。
	
	チェック ボックスをオンにして、アプリケーションでシングル サインオンを使用します。シングル サインオンには SSL が必要なため、このオプションを選択すると、自動的にプロジェクトの SSL が有効になります。
	
	**[次へ]** を選択します。

	Visual Studio がアプリを登録します。
    
4.接続する Office 365 API を選択し、それぞれのアクセス許可レベルを指定します。この例では、次のようにします。
	
	1.**[連絡先]** を選択します。
		
	2.**[連絡先の読み取り]** をオンにします。
		
	2.**[ユーザーとグループ]** を選択します。
		
	4.**[サインインとユーザー プロファイルの読み取り]** をオンにします。
		
6.**[完了]** を選択します。
	
この時点で、Visual Studio は必要な **NuGet パッケージ**をプロジェクトに追加します。追加される NuGet パッケージは、追加した Office 365 API に応じて異なります。
    

[!INCLUDE [END VS2015 section](../includes/controls/vs2015section.xml)]



    
## アプリのコーディング

Office 365 サービスをプロジェクトに追加したら、アプリのユーザーを認証し、ユーザーの Office 365 データにアクセスするコードを書く必要があります。

認証プロセスは、MVC などの Web アプリケーションを構築する場合と、Windows ストア アプリなどのネイティブ アプリケーションを構築する場合で異なります。

この ASP.NET MVC の例では、キャッシュにデータベースを使用する、永続的な [Active Directory Authentication Library](https://msdn.microsoft.com/ja-jp/library/azure/jj573266.aspx) (ADAL) トークン キャッシュを採用します。ADAL を使用すれば、Active Directory (AD) (この場合は Azure AD) に対してユーザーを認証してから、API 呼び出しを保護するためのアクセス トークンを取得できます。

ここから、例の構築は次の 3 つの主要なステップで構成されます。

 - [Azure AD を使用した認証用にプロジェクトを構成する](#bk_AuthenticateAzure)

 - [Azure Active Directory を使用して認証する](#bk_AuthenticateAzureAD)

 - [Office 365 API にアクセスする](#bk_accessOffice365APIs)



###Azure Active Directory を使用した認証用にプロジェクトを構成する

構築している例では、Office 365 資格情報の認証のみが必要であるため、[OWIN](http://owin.org/) &nbsp; [Katana](http://katanaproject.codeplex.com/documentation) および [Active Directory Authentication Library](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) (ADAL) を使用します。

ただし最初に、Secure Sockets Layer (SSL) を使用するようにプロジェクトを構成する必要があります。

####プロジェクトの SSL を有効にする 

Visual Studio では、既定で、プロジェクトが SSL 用に構成されません。プロジェクトで SSL を有効にしていない場合は、ここで、該当するリダイレクト URL を使用して Office 365 サービスを更新します。

SSL を有効にするには、次のようにします。

1.**ソリューション エクスプローラー**で、プロジェクトを選択します。
2.**[プロパティ]** で、**[SSL 有効]** を **[True]** に設定します。
	
	Visual Studio が **[SSL URL]** の値を指定します。

次に、ホーム ページとして HTTPS エンドポイントを使用するようにアプリケーションを更新します。

1.	**ソリューション エクスプローラー**で、プロジェクトを右クリックして、**[プロパティ]** を選択します。
2.	**サーバー**で、**プロジェクトの URL** を Visual Studio が **SSL URL** 用に作成した HTTPS エンドポイントに設定します。

最後に、Office 365 サービスを更新します。

3.**ソリューション エクスプローラー**で、プロジェクトを右クリックして、**[追加]** > **[接続済みサービス]** の順に選択します。必要に応じてサインインします。

	Visual Studio から、プロジェクト内の 1 つ以上のリダイレクト URL が Azure AD 内のアプリケーション エントリに存在しないことが通知されます。

4.**[はい]** を選択して、Azure AD のアプリケーション エントリに、セキュリティ保護されたリダイレクト URL を追加します。
5.**[OK]** を選択して、**サービス マネージャー**を終了します。



####認証に必要な NuGet パッケージを管理する

次の Azure AD および OWIN Katana パッケージを認証用にインストールする必要があります。

- [Active Directory Authentication Library](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory)
- [EntityFramework](https://www.nuget.org/packages/EntityFramework)
- [Microsoft.Owin.Host.SystemWeb](https://www.nuget.org/packages/Microsoft.Owin.Host.SystemWeb)
- [Microsoft.Owin.Security.Cookies](https://www.nuget.org/packages/Microsoft.Owin.Security.Cookies)
- [Microsoft.Owin.Security.OpenIdConnect](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect)

これらのパッケージは、依存関係として選択された NuGet パッケージもインストールします。

NuGet パッケージをダウンロードしてインストールするには、次のようにします。

1.**ソリューション エクスプローラー**で、プロジェクトを右クリックします。
2.**[NuGet パッケージの管理...]** を選択します。 
3.**[NuGet パッケージの管理]** で、**[オンライン]** を選択します。
4.検索ボックスを使用して必要なパッケージを探します。
5.パッケージを選択し、**[インストール]** を選択します。
6.**[プロジェクトの選択]** で、**[OK]** を選択します。
7.画面の指示に従って、すべてのライセンスを受け入れます。

####認証を有効にするように web.config ファイルを更新します。

Office 365 サービスをプロジェクトに追加すると、Visual Studio が次のプロパティを Azure AD 内のアプリケーションのレジストリと、ソリューション内の **web.config** ファイルに追加します。


    <add key="ida:ClientId" value="app-azure-ad-client-id" />
    <add key="ida:ClientSecret" value="app-azure-ad-password" />
    <add key="ida:TenantId" value="azure-tenant-where-app-is-registered" />
    <add key="ida:AADInstance" value="https://login.microsoftonline.com/" 

<!--
In addition, to successfully authenticate against Azure AD, a single-tenant web application must be able to specify the login authority, including the appropriate tenant ID. The authority follows the pattern

	https://login.windows.net/ [tenantId]

where [tenantId] is the ID of the Azure tenant in which the application is registered.

Because of this, you'll need to add the tenant ID to the project's web.config file. You can retrieve the tenant ID for your application from the [Azure Management Portal](https://manage.windowsazure.com).
 

1. Sign into the [Azure Management Portal](https://manage.windowsazure.com/), using your Office 365 subscription credentials.

2. In the left navigation panel, select **Active Directory**. Be sure the **Directory** tab is selected, and then choose the directory name for the Office 365 tenancy in which your app is registered.

	At this point, the URL in the browser window will include a GUID that is the tenant ID of your directory.

3. Copy only the GUID from the browser URL. This is the ID for the tenancy in which your application is registered.

After you have copied the tenant ID, you can add it to the project's web.config file. 

1. In **Solution Explorer**, double-click **Web.config**.
2. In the web.config file, in the **appSettings** section, add a new key, containing the tenant ID, directly under the ida:AuthorizationUri key.

		<add key="ida:TenantId" value="your-Office-365-tenant-ID"/>

3.ファイルを保存します。

-->

####ADAL 認証トークンを管理するためのデータベースを作成します。

次に、ADAL トークン キャッシュとして機能するデータベースをプロジェクトに追加します。

ADAL トークン キャッシュの詳細については、「[ADAL v2 の新しいトークン キャッシュ](http://www.cloudidentity.com/blog/2014/07/09/the-new-token-cache-in-adal-v2/)」を参照してください。


ADAL トークン キャッシュとして機能するデータベースを追加するには、次のようにします。

1.**ソリューション エクスプローラー**で、**App_Data** フォルダーを右クリックして、**[追加]** > **[新しいアイテム...]** の順に選択します。
2.**[データ]** が選択されていることを確認してから、**[SQL Server データベース]** を選択します。データベースに「**ADALTokenCacheDb**」という名前を付けて、**[追加]** を選択します。

プロジェクトの web.config にデータベース接続が追加されていることを確認します。

1.**ソリューション エクスプローラー**で、**web.config** をダブルクリックします。
2.次のセクションが web.config ファイルの **appSettings** セクションの後ろに追加されていることを確認します。それがない場合は追加します。
	
	```XML
	<connectionStrings>
  	<add name="DefaultConnection"
    	connectionString="Data Source=(LocalDB)\v11.0;AttachDbFilename=|DataDirectory|\ADALTokenCacheDb.mdf;Integrated Security=True" providerName="System.Data.SqlClient" />
	</connectionStrings>
	```
	
3.ファイルを保存します。

####認証に必要なファイルをコピーする

次に、[Office 365 シングル テナント MVC プロジェクト](https://github.com/OfficeDev/O365-WebApp-SingleTenant) GitHub プロジェクトからいくつかのファイルをインポートします。

GitHub プロジェクトからファイルをコピーするには、次のようにします。

1.ブラウザーで、コピーするファイルに移動します (ファイルが下に一覧表示されます)。
2.**[RAW]** ボタンを右クリックして、**[対象をファイルに保存]** を選択します。
3.ファイルをコンピューターに保存します。
4.Visual Studio の**ソリューション エクスプローラー**のプロジェクト ノードで、後述するように、指定したフォルダを右クリックして、**[追加]** > **[既存の項目]** の順に選択します。
5.コンピューター上の保存場所からファイルを選択します。

次のファイルをプロジェクト内の **Models** フォルダーにコピーします。これらのクラスは、永続的な ADAL トークン キャッシュをどのように構築して、トークンを保存するために使用できるかを示しています。

- [ADALTokenCache.cs](https://github.com/OfficeDev/O365-WebApp-SingleTenant/blob/master/O365-WebApp-SingleTenant/Models/ADALTokenCache.cs)
- [ApplicationDbContext.cs](https://github.com/OfficeDev/O365-WebApp-SingleTenant/blob/master/O365-WebApp-SingleTenant/Models/ApplicationDbContext.cs)

新しいフォルダー **Utils** を作成します。

1.**ソリューション エクスプローラー**で、プロジェクトを右クリックして、**[追加]** > **[新しいフォルダー]** の順に選択します。
2.フォルダーに **Utils** という名前を付けます。

次のファイルをそのフォルダーにコピーします。このヘルパー クラスには、起動時に、プロジェクトの web.config ファイルから、ADAL 認証オブジェクトの作成に必要な値を読み取るメンバー変数が含まれています。

- [SettingsHelper.cs](https://github.com/OfficeDev/O365-WebApp-SingleTenant/blob/master/O365-WebApp-SingleTenant/Utils/SettingsHelper.cs)


最後に、[_LoginPartial.cshtml](https://github.com/OfficeDev/O365-WebApp-SingleTenant/blob/master/O365-WebApp-SingleTenant/Views/Shared/_LoginPartial.cshtml) ファイルをプロジェクトの **Views** > **Shared** フォルダーにコピーします。



###Azure Active Directory を使用して認証する

これでプロジェクトが認証用に構成されたため、Azure AD を使用して認証を処理する機能を実際に追加できます。

###Azure AD シングル サインオンを構成する

次に、実際に認証を処理するために、OWIN スタートアップ クラスをプロジェクトに追加する必要があります。OWIN スタートアップ クラスを追加するには、次のようにします。

1.ソリューション エクスプローラーで、プロジェクト名を右クリックして、**[追加]** > **[新しいアイテム...]** の順に選択します。
2.**[新しいアイテムの追加]** の **[Web]** で、**[全般]** を選択してから、**[OWIN スタートアップ クラス]** を選択します。
3.**[名前]** に、「**Startup**」と入力します。
3.**[追加]** を選択します。

新しいスタートアップ クラスがプロジェクトに追加されます。

次に、認証を処理するコードを追加します。

1.ファイルの名前空間の参照を次のように置き換えます。

	```C#
		
		using Microsoft.IdentityModel.Clients.ActiveDirectory;
		using Microsoft.Owin.Security;
		using Microsoft.Owin.Security.Cookies;
		using Microsoft.Owin.Security.OpenIdConnect;
		using O365_WebApp_SingleTenant.Models;
		using O365_WebApp_SingleTenant.Utils;
		using Owin;
		using System;
		using System.IdentityModel.Claims;
		using System.Threading.Tasks;
		using System.Web;
		using Microsoft.Owin;
	```
	
2.次のメソッド **ConfigureAuth** をスタートアップ クラスに追加します。
	
	```C#

        public void ConfigureAuth(IAppBuilder app)
        {
            app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

            app.UseCookieAuthentication(new CookieAuthenticationOptions());

            app.UseOpenIdConnectAuthentication(
                new OpenIdConnectAuthenticationOptions
                {
                    ClientId = SettingsHelper.ClientId,
                    Authority = SettingsHelper.Authority,

                    Notifications = new OpenIdConnectAuthenticationNotifications()
                    {                       
                        // If there is a code in the OpenID Connect response, redeem it for an access token and refresh token, and store those away.
                        AuthorizationCodeReceived = (context) =>
                        {
                            var code = context.Code;
                            ClientCredential credential = new ClientCredential(SettingsHelper.ClientId, SettingsHelper.AppKey);
                            String signInUserId = context.AuthenticationTicket.Identity.FindFirst(ClaimTypes.NameIdentifier).Value;

                            AuthenticationContext authContext = new AuthenticationContext(SettingsHelper.Authority, new ADALTokenCache(signInUserId));
                            AuthenticationResult result = authContext.AcquireTokenByAuthorizationCode(code, new Uri(HttpContext.Current.Request.Url.GetLeftPart(UriPartial.Path)), credential, SettingsHelper.AADGraphResourceId);

                            return Task.FromResult(0);
                        },
                        RedirectToIdentityProvider = (context) =>
                        {
                            // This ensures that the address used for sign in and sign out is picked up dynamically from the request
                            // this allows you to deploy your app (to Azure Web Sites, for example)without having to change settings
                            // Remember that the base URL of the address used here must be provisioned in Azure AD beforehand.
                            string appBaseUrl = context.Request.Scheme + "://" + context.Request.Host + context.Request.PathBase;
                            context.ProtocolMessage.RedirectUri = appBaseUrl + "/";
                            context.ProtocolMessage.PostLogoutRedirectUri = appBaseUrl;

                            return Task.FromResult(0);
                        },
                        AuthenticationFailed = (context) =>
                        {
                            // Suppress the exception if you don't want to see the error
                            context.HandleResponse();
                            return Task.FromResult(0);
                        }
                    }

                });
        }
	```
	
3.**ConfigureAuth メソッド**を呼び出すコードを **Startup.Configuration** メソッドに追加します。
	
	```C#
        public void Configuration(IAppBuilder app)
        {
            ConfigureAuth(app);
        }
	```
	
####Office 365 のサインインおよびサインアウト用のコントローラーを追加する

サインインとサインアウトを処理するためのアカウント コントローラーを追加します。

1.**ソリューション エクスプローラー**で、**Controllers** フォルダーを右クリックして、**[追加]** > **[コントローラー...]** の順に選択します。
2.**[MVC 5 コントローラー - 空]** を選択し、**[追加]** を選択します。
3.[コントローラー名] に「**AccountController**」と入力し、**[追加]** を選択します。
4.コントローラー ファイル内の名前空間参照を次のように置き換えます。
	
	```C#
		using Microsoft.IdentityModel.Clients.ActiveDirectory;
		using Microsoft.Owin.Security;
		using Microsoft.Owin.Security.Cookies;
		using Microsoft.Owin.Security.OpenIdConnect;
		using O365_WebApp_SingleTenant.Utils;
		using System.Security.Claims;
		using System.Web;
		using System.Web.Mvc;
	```
	
5.**Index() メソッド**を削除します。
6.サインインとサインアウトを処理する次のメソッドを **AccountController** クラスに追加します。
	 
	```C#
        public void SignIn()
        {
            if (!Request.IsAuthenticated)
            {
                HttpContext.GetOwinContext().Authentication.Challenge(new AuthenticationProperties { RedirectUri = "/" }, OpenIdConnectAuthenticationDefaults.AuthenticationType);
            }
        }
        public void SignOut()
        {
            string callbackUrl = Url.Action("SignOutCallback", "Account", routeValues: null, protocol: Request.Url.Scheme);

            HttpContext.GetOwinContext().Authentication.SignOut(
                new AuthenticationProperties { RedirectUri = callbackUrl },
                OpenIdConnectAuthenticationDefaults.AuthenticationType, CookieAuthenticationDefaults.AuthenticationType);
        }

        public ActionResult SignOutCallback()
        {
            if (Request.IsAuthenticated)
            {
                // Redirect to home page if the user is authenticated.
                return RedirectToAction("Index", "Home");
            }

            return View();
        }
	```


###Office 365 API にアクセスする

これで、ユーザーの Office 365 データにアクセスするコードを追加する準備ができました。

####連絡先情報ビューのモデルを作成する 

まず、連絡先ビューの基になるクラスを作成する必要があります。

1.**ソリューション エクスプローラー**で、**Models** フォルダーを右クリックして、**[追加]** > **[クラス...]** の順に選択します。
2.**[クラス]** を選択し、名前として、「**MyContact**」と入力し、**[追加]** を選択します。
3.クラスで、次のコード行を追加します。
	
	```C#

	    public class MyContact
	    {
	        public string Name { get; set; }
	    }
	```
	
4.ファイルを保存します。
	
####連絡先情報用のコントローラーを追加する

次に、ユーザーの連絡先情報用のコントローラーを追加します。

1.**ソリューション エクスプローラー**で、**Controllers** フォルダーを右クリックして、**[追加]** > **[コントローラー]** の順に選択します。
2.**[MVC 5 コントローラー - 空]** を選択し、**[追加]** を選択します。
3.[コントローラー名] に「**ContactsController**」と入力し、**[追加]** を選択します。
	
	Visual Studio が新しい空のコントローラーをプロジェクトに追加します。
	
4.コントローラー ファイル内の名前空間参照を次のように置き換えます。

	```C#
		using Microsoft.IdentityModel.Clients.ActiveDirectory;
		using Microsoft.Office365.Discovery;
		using Microsoft.Office365.OutlookServices;
		using O365_WebApp_SingleTenant.Models;
		using O365_WebApp_SingleTenant.Utils;
		using System.Collections.Generic;
		using System.Security.Claims;
		using System.Threading.Tasks;
		using System.Web.Mvc;
	```
	
	プロジェクトの Models 名前空間の名前空間参照を追加してください。
	
	```C#
			using [projectname].Models;
	```
	
5.**[Authorize]** 属性を **ContactsController** クラスに追加します。
	
	**[Authorize]** 属性は、このコントローラーにアクセスする前に、ユーザーが認証されていることを要求します。
	
	```C#
    	[Authorize]
		public class ContactsController : Controller
	```
	
6.Index() メソッドを非同期に変更します。
	 
	```C#
		public async Task<ActionResult> Index()
	```
	
7.Index() メソッドに次のコードを追加します。
	   
	このコードにより、サインインしている Office 365 ユーザー ID と Office 365 テナントの権限を使用して、認証コンテキストが作成されます。
	
	```C#
            List<MyContact> myContacts = new List<MyContact>();

            var signInUserId = ClaimsPrincipal.Current.FindFirst(ClaimTypes.NameIdentifier).Value;
            var userObjectId = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;

            AuthenticationContext authContext = new AuthenticationContext(SettingsHelper.Authority, new ADALTokenCache(signInUserId));
	```
	
8.**try-catch** ブロックを **Index() メソッド**内の上記コードのすぐ下に追加します。
	
	```C#

            try
            {
 
            }
            catch (AdalException exception)
            {
                //handle token acquisition failure
                if (exception.ErrorCode == AdalError.FailedToAcquireTokenSilently)
                {
                    authContext.TokenCache.Clear();

                    //handle token acquisition failure
                }
            }

            return View(myContacts);
	```
	
9.上のコードの **try** ブロックで、**Index()** メソッドに次のコードを追加します。
	
	このコードは、アプリケーションのクライアント ID とアプリのパスワード、およびユーザーの資格情報を渡して、探索サービスへのアクセス トークンの取得を試みます。ユーザー認証トークンがキャッシュされているため、コントローラーは、ユーザーに資格情報の入力を再度求める必要なく、自動的に必要なアクセス トークンを取得できます。
	
	アクセス トークンにより、アプリケーションは探索サービス クライアント オブジェクトを作成し、探索クライアント オブジェクトを使用して、Office 365 サービス連絡先 API のリソース エンドポイントを特定できます。
	
	```C#
                DiscoveryClient discClient = new DiscoveryClient(SettingsHelper.DiscoveryServiceEndpointUri,
                    async () =>
                    {
                        var authResult = await authContext.AcquireTokenSilentAsync(SettingsHelper.DiscoveryServiceResourceId, new ClientCredential(SettingsHelper.ClientId, SettingsHelper.AppKey), new UserIdentifier(userObjectId, UserIdentifierType.UniqueId));

                        return authResult.AccessToken;
                    });

                var dcr = await discClient.DiscoverCapabilityAsync("Contacts");
	```
	
10.最後に、<b>try</b> ブロック内の上記コードのすぐ下で、次のコードを *Index()** メソッドに追加します。
	
	このコードは、Office 365 サービス連絡先 API のリソース エンドポイントに接続し、再度自動的にアプリケーションとユーザーの資格情報を渡して、Outlook サービスへのアクセス トークンを取得します。
		
	コードは、アクセス トークンを受け取ると、Outlook クライアント オブジェクトを初期化できます。その後で、Outlook クライアント オブジェクトの **Me** プロパティを使用して、このユーザーの連絡先情報を取得します。	
	
	最後に、コントローラーがユーザーの連絡先の一覧を読み取って、表示名が一覧表示されたビューを返します。
	
	```C#

                OutlookServicesClient exClient = new OutlookServicesClient(dcr.ServiceEndpointUri,
                    async () =>
                    {
                        var authResult = await authContext.AcquireTokenSilentAsync(dcr.ServiceResourceId, new ClientCredential(SettingsHelper.ClientId, SettingsHelper.AppKey), new UserIdentifier(userObjectId, UserIdentifierType.UniqueId));

                        return authResult.AccessToken;
                    });

                var contactsResult = await exClient.Me.Contacts.ExecuteAsync();

                do
                {
                    var contacts = contactsResult.CurrentPage;
                    foreach (var contact in contacts)
                    {
                        myContacts.Add(new MyContact { Name = contact.DisplayName });
                    }

                    contactsResult = await contactsResult.GetNextPageAsync();

                } while (contactsResult != null);
	```
	
	完成した **Index()** メソッドは次のようになるはずです。
	
	```C#
        public async Task<ActionResult> Index()
        {
            List<MyContact> myContacts = new List<MyContact>();

            var signInUserId = ClaimsPrincipal.Current.FindFirst(ClaimTypes.NameIdentifier).Value;
            var userObjectId = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;

            AuthenticationContext authContext = new AuthenticationContext(SettingsHelper.Authority, new ADALTokenCache(signInUserId));
            
            try
            {
                DiscoveryClient discClient = new DiscoveryClient(SettingsHelper.DiscoveryServiceEndpointUri,
                    async () =>
                    {
                        var authResult = await authContext.AcquireTokenSilentAsync(SettingsHelper.DiscoveryServiceResourceId, new ClientCredential(SettingsHelper.ClientId, SettingsHelper.AppKey), new UserIdentifier(userObjectId, UserIdentifierType.UniqueId));

                        return authResult.AccessToken;
                    });

                var dcr = await discClient.DiscoverCapabilityAsync("Contacts");                

                OutlookServicesClient exClient = new OutlookServicesClient(dcr.ServiceEndpointUri,
                    async () =>
                    {
                        var authResult = await authContext.AcquireTokenSilentAsync(dcr.ServiceResourceId, new ClientCredential(SettingsHelper.ClientId, SettingsHelper.AppKey), new UserIdentifier(userObjectId, UserIdentifierType.UniqueId));

                        return authResult.AccessToken;
                    });

                var contactsResult = await exClient.Me.Contacts.ExecuteAsync();

                do
                {
                    var contacts = contactsResult.CurrentPage;
                    foreach (var contact in contacts)
                    {
                        myContacts.Add(new MyContact { Name = contact.DisplayName });
                    }

                    contactsResult = await contactsResult.GetNextPageAsync();

                } while (contactsResult != null);
            }
            catch (AdalException exception)
            {
                //handle token acquisition failure
                if (exception.ErrorCode == AdalError.FailedToAcquireTokenSilently)
                {
                    authContext.TokenCache.Clear();

                    //handle token acquisition failure
                }
            }

            return View(myContacts);
        }
	```
	
####連絡先データ用のビューを作成する

次に、ユーザーの連絡先情報用のビューを作成します。以前作成した *MyContact** クラスに基づいてビューを作成することによって、これを実現します。

1.**ソリューション エクスプローラー**で、**Contacts** フォルダーを右クリックして、**[追加]** > **[ビュー]** の順に選択します。
2.**[ビューの追加]** ダイアログ ボックスで、新しいビューを定義します。
	- **[ビュー名] に対して、「**Index**」と入力します。
	- **[テンプレート]** で、**[一覧]** を選択します。
	- **[モデル クラス]** で、**[MyContact]** を選択します。
	- **[追加]** を選択します。



####ナビゲーション バーに MyContacts リンクを追加する

1.**ソリューション エクスプローラー**の **Shared** フォルダーで、_Layout.cshtml ファイルをダブルクリックします 。

2.MyContacts ページのアクション リンクを追加し、ユーザー ログイン用の部分クラスを挿入します。
3.ナビゲーション バーの HTML を自動生成されたものから更新します。
	
	```ASP
            <div class="navbar-collapse collapse">
                <ul class="nav navbar-nav">
                    <li>@Html.ActionLink("Home", "Index", "Home")</li>
                    <li>@Html.ActionLink("About", "About", "Home")</li>
                    <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
                </ul>
            </div>
	```
	
	To the following, to add an action link for the MyContacts page, and inject the partial class for the user log-in.
	
	```ASP
            <div class="navbar-collapse collapse">
                <ul class="nav navbar-nav">
                    <li>@Html.ActionLink("Home", "Index", "Home")</li>
                    <li>@Html.ActionLink("About", "About", "Home")</li>
                    <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
                    <li>@Html.ActionLink("My Contacts", "Index", "Contacts")</li>
                </ul>
                @Html.Partial("_LoginPartial")
            </div>
	```
	
<a name="bk_test" />
##アプリのテスト

これで、ソリューションを実行する準備ができました。F5 キーを押して、Web アプリケーションをデバッグします。

これが新しい開発環境の場合、Visual Studio によって、IIS SSL 証明書を構成するように求められることがあります。**[はい]** を 2 回選択して、先に進みます。

1.Visual Studio により、選択したブラウザーで Web アプリケーションが起動されます。

2.ページの右上隅にある **[サインイン]** を選択し、Web アプリケーションを登録した Office 365 テナントにサインインします。
	
	組織のユーザーのみがシングルテナント アプリケーションへのサインインを許可されるため、同意する必要はありません。アプリケーションがマルチ テナントの場合は、Azure AD にアプリが要求されたアクセス許可が一覧表示される同意ページが表示されるため、アプリにそれらのアクセス許可を付与することに同意できます。
	
3.サインインすると、ホーム ページの最上部のナビゲーション バーに自分の電子メール アドレスと **[サインアウト]** が表示されるはずです。
4.**[個人用の連絡先]** を選択します。
	
	**[個人用の連絡先]** ページで、テナントから Exchange 連絡先の名前が取得され、表示されます。
	
これで、Office 365 API をカスタマイズして統合するための Web アプリケーション プロジェクトが完成しました。


## 次の手順

アプリで使用可能な Office 365 REST APIについて調べます。

- [Office 365 API リファレンス](..\api\api-catalog.md)

GitHub の [Office コード サンプル リポジトリ](https://github.com/OfficeDev) にアクセスして、さまざまなプロジェクトの種類において Office 365 API を使用するコード サンプルをダウンロードするか調べます。これには次のようなものがあります。

- [Windows ストア アプリ用 Office 365 スタート プロジェクト](https://github.com/OfficeDev/O365-Windows-Start)
- [Windows ストア、電話、およびユニバーサル アプリで Office 365 に接続する](https://github.com/OfficeDev/O365-Win-Connect)
- [ASP.NET MVC 用 Office 365 スタート プロジェクト](https://github.com/OfficeDev/O365-ASPNETMVC-Start)


## その他の技術情報

- [Office 365 シングル テナント MVC プロジェクト](https://github.com/OfficeDev/O365-WebApp-SingleTenant)
- [Office 365 ASP.NET MVC スタート プロジェクト](https://github.com/OfficeDev/O365-ASPNETMVC-Start)
-  [Office 365 API スタート プロジェクトおよびコード サンプル](..\howto\Starter-projects-and-code-samples.md) 
 




[!INCLUDE [END ASP.NET MVC section](../includes/controls/aspnetmvcsection.xml)]



[!INCLUDE [Enable filtering functionality ](../includes/controls/enablefiltering.xml)]


