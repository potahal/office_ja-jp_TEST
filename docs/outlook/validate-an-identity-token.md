
# Exchange の ID トークンを検証する
Outlook アドインのユーザーのメール アカウントと Web サービスが提供する情報とを結び付ける、Exchange 2013 ID トークンを検証する方法について説明します。

 _ **適用対象:** apps for Office?| Office Add-ins?| Outlook_


![関連するコード スニペットおよびサンプル アプリ](../../images/mod_icon_links_samples.png)[Outlook-Add-in-JavaScript-ValidateIdentityToken](https://github.com/OfficeDev/Outlook-Add-in-JavaScript-ValidateIdentityToken)

Outlook アドインから ID トークンが送信されることがありますが、要求を信頼する前に、適切な Exchange サーバーから受け取ったトークンであることを検証する必要があります。この記事では、C# で作成した検証オブジェクトで Exchange の ID トークンを検証する方法の例について説明します。なお、検証には他のプログラミング言語も使用できます。トークンの検証に必要な手順については、「[JSON Web Token (JWT) Internet Draft](http://self-issued.info/docs/draft-goland-json-web-token-00.mdl)」に記載されています。

ID トークンの検証およびユーザーの一意識別子の取得は 4 つのステップで進めることをお勧めします。まず、Base64 で URL エンコードされた文字列から JSON Web トークン (JWT) を抽出します。次に、トークンが整形式であること、自分の Outlook アドイン向けのトークンであること、有効期限が切れていないこと、認証メタデータ ドキュメントの有効な URL を抽出できることを確認します。次に、Exchange サーバーから認証メタデータ ドキュメントを取得し、ID トークンに添付されている署名を検証します。最後に、ユーザーの Exchange ID と認証メタデータ ドキュメントの URL のハッシュ値を求めることによって、ユーザーの一意識別子を計算します。全体で見ると一見複雑なプロセスですが、細分化したそれぞれの作業は非常にシンプルです。
これらの例を含むソリューションは、Web ([Outlook-Add-in-JavaScript-ValidateIdentityToken](https://github.com/OfficeDev/Outlook-Add-in-JavaScript-ValidateIdentityToken)) からダウンロードできます。
 **この記事の内容**

![1 つ](../../images/mod_icon_one.png)[ID トークンを検証するためのセットアップ](#id-トークンを検証するためのセットアップ)

![2 つ](../../images/mod_icon_two.png)[JSON Web トークンの抽出](#json-web-トークンの抽出)

![3 つ](../../images/mod_icon_three.png)[JWT の解析](#jwt-の解析)

![4 つ](../../images/mod_icon_four.png)[ID トークンの署名の検証](#id-トークンの署名の検証)

![5 つ](../../images/mod_icon_five.png)[Exchange アカウントの一意 ID の計算](#exchange-アカウントの一意-id-の計算)

![関連するコード スニペットおよびサンプル アプリ](../../images/mod_icon_links_samples.png)[ユーティリティ オブジェクト](#ユーティリティ-オブジェクト)


## ID トークンを検証するためのセットアップ


この記事のコード例では、Windows Identity Foundation (WIF) と、WIF を JSON トークン用のハンドラーで拡張する DLL を使用します。必要なアセンブリは次の場所からダウンロードできます。


- [Windows Identity Foundation](http://msdn.microsoft.com/ja-jp/security/aa570351)
    
- [Windows.IdentityModel.Extensions.dll (32 ビット アプリケーション用)](http://download.microsoft.com/download/0/1/D/01D06854-CA0C-46F1-ADBA-EBF86010DCC6/MicrosoftIdentityExtensions-32.msi)
    
- [Windows.IdentityModel.Extensions.dll (64 ビット アプリケーション用)](http://download.microsoft.com/download/0/1/D/01D06854-CA0C-46F1-ADBA-EBF86010DCC6/MicrosoftIdentityExtensions-64.msi)
    

## JSON Web トークンの抽出


 **Decode** ファクトリ メソッドでは、Exchange サーバーからの JWT を、トークンを構成する 3 つの文字列に分割してから、2 番目の例で示す **Base64Decode** メソッドを使用して、JWT のヘッダーとペイロードを JSON 文字列にデコードします。その文字列を **JsonToken** のコンストラクターに渡し、この中で JWT の内容を検証して、 **JsonToken** オブジェクトの新しいインスタンスを返します。


```C#
    public static JsonToken Decode(string rawToken)
    {
      string[] tokenParts = rawToken.Split('.');

      if (tokenParts.Length != 3)
      {
        throw new ApplicationException("Token must have three parts separated by '.' characters.");
      }

      string encodedHeader = tokenParts[0];
      string encodedPayload = tokenParts[1];
      string signature = tokenParts[2];

      string decodedHeader = Base64Decode(encodedHeader);
      string decodedPayload = Base64Decode(encodedPayload);

      JavaScriptSerializer serializer = new JavaScriptSerializer();

      Dictionary<string, string> header = serializer.Deserialize<Dictionary<string, string>>(decodedHeader);
      Dictionary<string, string> payload = serializer.Deserialize<Dictionary<string, string>>(decodedPayload);

      return new JsonToken(header, payload, signature);
    }
```

 **Base64Decode** メソッドは、「[JSON Web Token (JWT) Internet Draft](http://self-issued.info/docs/draft-goland-json-web-token-00.mdl)」の付録「Notes on implementing base64url encoding without padding」に記載されているデコードのロジックを実装しています。




```C#
    public static Encoding TextEncoding = Encoding.UTF8;

    private static char Base64PadCharacter = '=';
    private static char Base64Character62 = '+';
    private static char Base64Character63 = '/';
    private static char Base64UrlCharacter62 = '-';
    private static char Base64UrlCharacter63 = '_';

    private static byte[] DecodeBytes(string arg)
    {
      if (String.IsNullOrEmpty(arg))
      {
        throw new ApplicationException("String to decode cannot be null or empty.");
      }

      StringBuilder s = new StringBuilder(arg);
      s.Replace(Base64UrlCharacter62, Base64Character62);
      s.Replace(Base64UrlCharacter63, Base64Character63);

      int pad = s.Length % 4;
      s.Append(Base64PadCharacter, (pad == 0) ? 0 : 4 - pad);

      return Convert.FromBase64String(s.ToString());
    }

    private static string Base64Decode(string arg)
    {
      return TextEncoding.GetString(DecodeBytes(arg));
    }
```


## JWT の解析


 **JsonToken** オブジェクトのコンストラクターでは、JWT の構造と内容をチェックして、JWT が有効かどうかを判断します。これは認証メタデータ ドキュメントを要求する前に行うのが適切です。JWT に適切なクレームが含まれていない場合や、JWT が有効期間内でない場合に、Exchange サーバーへの呼び出しやそれに伴う遅延を回避できます。

コンストラクターでは、複数のユーティリティ メソッドを呼び出して、別のクレームが存在するかどうかや範囲内かどうかを判断します。問題がある場合は、ユーティリティ メソッドによってアプリケーション例外がスローされます。例外がスローされなかった場合は、 **IsValid** プロパティを **true** に設定し、トークンの署名の検証に入ることができます。

それぞれのユーティリティ メソッドについてはこの後説明します。




```C#
    public JsonToken(Dictionary<string, string> header, Dictionary<string, string> payload, string signature)
    {

      // Assume that the token is invalid to start out.
      this.IsValid = false;

      // Set the private dictionaries that contain the claims.
      this.headerClaims = header;
      this.payloadClaims = payload;
      this.signature = signature;

      // If there is no "appctx" claim in the token, throw an ApplicationException.
      if (!this.payloadClaims.ContainsKey(AuthClaimTypes.AppContext))
      {
        throw new ApplicationException(String.Format("The {0} claim is not present.", AuthClaimTypes.AppContext));
      }

      appContext = new JavaScriptSerializer().Deserialize<Dictionary<string, string>>(payload[AuthClaimTypes.AppContext]);


      // Validate the header fields.
      this.ValidateHeader();

      // Determine whether the token is within its valid time.
      this.ValidateLifetime();

      // Validate that the token was sent to the correct URL.
      this.ValidateAudience();

      // Validate the token version.
      this.ValidateVersion();

      // Make sure that the appctx contains an authentication
      // metadata location.
      this.ValidateMetadataLocation();

      // If the token passes all the validation checks, we
      // can assume that it is valid.
      this.IsValid = true;
    }
```


### ValidateHeader メソッド

 **ValidateHeader** メソッドでは、必要なクレームがトークン ヘッダーに含まれているかどうかと、クレームが正しい値かどうかをチェックします。ヘッダーは次のように設定されている必要があります。それ以外の場合は、メソッドはアプリケーション例外をスローして終了します。

{ "typ" : "JWT", "alg" : "RS256", "x5t" : "<thumbprint>" }
```C#
    private void ValidateHeaderClaim(string key, string value)
    {
      if (!this.headerClaims.ContainsKey(key))
      {
        throw new ApplicationException(String.Format("Header does not contain \"{0}\" claim.", key));
      }

      if (!value.Equals(this.headerClaims[key]))
      {
        throw new ApplicationException(String.Format("\"{0}\" claim must be \"{0}\".", key, value));
      }
    }

    private void ValidateHeader()
    {
      ValidateHeaderClaim(AuthClaimTypes.TokenType, Config.TokenType);
      ValidateHeaderClaim(AuthClaimTypes.Algorithm, Config.Algorithm);
    
      if (!this.headerClaims.ContainsKey(AuthClaimTypes.x509Thumprint))
      {
        throw new ApplicationException(String.Format("Header does not contain \"{0}\" claim.", AuthClaimTypes.x509Thumprint));
      }
    }


```


### ValidateLifetime メソッド

JWT には 2 つの日時が指定されています。1 つは "nbf" ("not before" の略) で、トークンが有効になる日時を表します。もう 1 つは "exp" で、トークンの有効期限が切れる日時を表します。これら 2 つの日時の間に該当するトークンのみを有効とみなします。このメソッドでは、トークンに指定された時刻の前後に 5 分間の余裕を持たせて確認を行うことによって、サーバーとクライアントの間の小さな時刻設定の違いが許容されます。


```C#
    private void ValidateLifetime()
    {
      if (!this.payloadClaims.ContainsKey(AuthClaimTypes.ValidFrom))
      {
        throw new ApplicationException(
          String.Format("The \"{0}\" claim is missing from the token.", AuthClaimTypes.ValidFrom));
      }

      if (!this.payloadClaims.ContainsKey(AuthClaimTypes.ValidTo))
      {
        throw new ApplicationException(
          String.Format("The \"{0}\" claim is missing from the token.", AuthClaimTypes.ValidTo));
      }

      DateTime unixEpoch = new DateTime(1970, 1, 1, 0, 0, 0,DateTimeKind.Utc);

      TimeSpan padding = new TimeSpan(0, 5, 0);

      DateTime validFrom = unixEpoch.AddSeconds(int.Parse(this.payloadClaims[AuthClaimTypes.ValidFrom]));
      DateTime validTo = unixEpoch.AddSeconds(int.Parse(this.payloadClaims[AuthClaimTypes.ValidTo]));

      DateTime now = DateTime.UtcNow;

      if (now < (validFrom - padding))
      {
        throw new ApplicationException(String.Format("The token is not valid until {0}.", validFrom));
      }

      if (now > (validTo + padding))
      {
        throw new ApplicationException(String.Format("The token is not valid after {0}.", validFrom));
      }
    }
```

 **validFrom** ("nbf") と **validTo** ("exp") の日時は、1970 年 1 月 1 日からの経過秒数を表す UNIX 時間で送られます。Exchange サーバーと検証コードを実行するサーバーとの時差に伴う問題を回避するために、日付と時刻の計算には UTC を使用します。


### ValidateAudience メソッド

ID トークンは、そのトークンを要求したアドインに対してのみ有効です。 **ValidateAudience** メソッドでは、トークンの対象を表すクレームをチェックして、Outlook アドインが予期する URL と一致することを確認します。


```C#
    private void ValidateAudience()
    {
      if (!this.payloadClaims.ContainsKey(AuthClaimTypes.Audience))
      {
        throw new ApplicationException(String.Format("The \"{0}\" claim is missing from the application context.", AuthClaimTypes.Audience));
      }

      string location = Config.Audience.Replace("/", "-").Replace("\\", "-");
      string audience = this.payloadClaims[AuthClaimTypes.Audience].Replace("/", "-").Replace("\\", "-");

      if (!location.Equals(audience))
      {
        throw new ApplicationException(String.Format(
          "The audience URL does not match. Expected {0}; got {1}.",
          Config.Audience, this.payloadClaims[AuthClaimTypes.Audience]));
      }
    }

```


### ValidateVersion メソッド

 **ValidateVersion** メソッドは、ID トークンのバージョンをチェックし、予期するバージョンに一致することを確認します。トークンのバージョンが異なると、中のクレームも異なります。バージョンをチェックすることによって、予期するクレームが ID トークンに含まれていることを確認できます。


```
    private void ValidateVersion()
    {
      if (!this.appContext.ContainsKey(AuthClaimTypes.MsExchExtensionVersion))
      {
        throw new ApplicationException(String.Format("The \"{0}\" claim is missing from the token.", AuthClaimTypes.MsExchExtensionVersion));
      }

      if (!Config.Version.Equals(this.appContext[AuthClaimTypes.MsExchExtensionVersion]))
      {
        throw new ApplicationException(String.Format(
          "The version does not match. Expected {0}; got {1}.",
          Config.Version, this.appContext[AuthClaimTypes.MsExchExtensionVersion]));
      }
    }

```


### ValidateMetadataLocation メソッド

Exchange サーバー上に格納されている認証メタデータ オブジェクトには、ID トークン内の署名を検証するために必要な情報が含まれています。 **ValidateMetadataLocation** メソッドは、ID トークンに認証メタデータの URL のクレームが含まれていることを確認します。署名を実際に検証する処理は次の手順で行います。


```C#
    private void ValidateMetadataLocation()
    {
      if (!this.appContext.ContainsKey(AuthClaimTypes.MsExchAuthMetadataUrl))
      {
        throw new ApplicationException(String.Format("The \"{0}\" claim is missing from the token.", AuthClaimTypes.MsExchAuthMetadataUrl));
      }
    }

```


## ID トークンの署名の検証


署名を検証するために必要なクレームが JWT に含まれていることを確認できたら、Windows Identity Foundation (WIF) と WIF 拡張を使用してトークンの署名を検証できます。署名の検証に必要な情報は次のとおりです。


- Exchange サーバーから送られた、Base64 で URL エンコードされた元の ID トークン文字列
    
- JWT からの認証メタデータ ドキュメントの場所
    
- JWT からの対象の URL
    
この例では、 **IdentityToken** オブジェクトのコンストラクターで Exchange サーバーから認証メタデータ ドキュメントを取得し、ID トークンの署名を検証します。ID トークンが有効な場合は、ID トークンに含まれている一意のユーザー ID を、 **IdentityToken** オブジェクトのインスタンスを使用して取得できます。




```C#
    public IdentityToken(string rawToken, string audience, string authMetadataEndpoint)
    {
      X509Certificate2 currentCertificate = null;

      currentCertificate = AuthMetadata.GetSigningCertificate(new Uri(authMetadataEndpoint));

      JsonWebSecurityTokenHandler jsonTokenHandler =
          GetSecurityTokenHandler(audience, authMetadataEndpoint, currentCertificate);

      SecurityToken jsonToken = jsonTokenHandler.ReadToken(rawToken);
      JsonWebSecurityToken webToken = (JsonWebSecurityToken)jsonToken;

      SigningCertificateThumbprint = currentCertificate.Thumbprint;
      Issuer = webToken.Issuer;
      Audience = webToken.Audience;
      ValidTo = webToken.ValidTo;
      ValidFrom = webToken.ValidFrom;
      foreach (JsonWebTokenClaim claim in webToken.Claims)
      {
        if (claim.ClaimType.Equals(AuthClaimTypes.AppContextSender))
        {
          ApplicationContextSender = claim.Value;
        }

        if (claim.ClaimType.Equals(AuthClaimTypes.IsBrowserHostedApp))
        {
          IsBrowserHostedApp = claim.Value == "true";
        }

        if (claim.ClaimType.Equals(AuthClaimTypes.AppContext))
        {
          string[] appContextClaims = claim.Value.Split(',');
          Dictionary<string, string> appContext =
              new JavaScriptSerializer().Deserialize<Dictionary<string, string>>(claim.Value);
          AuthenticationMetaDataUrl = appContext[AuthClaimTypes.MsExchAuthMetadataUrl];
          ExchangeID = appContext[AuthClaimTypes.MsExchImmutableId];
          TokenVersion = appContext[AuthClaimTypes.MsExchTokenVersion];
        }
      }
    }


```

 **IdentityToken** オブジェクトのコンストラクターのコードでは、Exchange サーバーからのクレームでインスタンスのプロパティを設定する処理が大きな部分を占めています。このコンストラクターでは、 **GetSecurityTokenHandler** メソッドを呼び出して、Exchange の ID トークンを検証するトークン ハンドラーを取得しています。 **GetSecurityTokenHandler** メソッドは、 **GetMetadataDocument** と **GetSigningCertificate** という 2 つのユーティリティ メソッドを呼び出して、Exchange サーバーから署名証明書を取得する処理を行います。以下のセクションではこれらのメソッドについて説明します。


### GetSecurityTokenHandler メソッド

 **GetSecurityTokenHandler** メソッドでは、ID トークンを検証する WIF トークン ハンドラーを返します。メソッドのコードの大部分は、検証を実行するためのトークン ハンドラーの初期化ですが、 **GetSigningCertificate** メソッドを呼び出して、トークンの署名に使用された X.509 証明書を Exchange サーバーから取得しています。


```
    private JsonWebSecurityTokenHandler GetSecurityTokenHandler(string audience,
        string authMetadataEndpoint,
        X509Certificate2 currentCertificate)
    {
      JsonWebSecurityTokenHandler jsonTokenHandler = new JsonWebSecurityTokenHandler();
      jsonTokenHandler.Configuration = new SecurityTokenHandlerConfiguration();

      jsonTokenHandler.Configuration.AudienceRestriction = new AudienceRestriction(AudienceUriMode.Always);
      jsonTokenHandler.Configuration.AudienceRestriction.AllowedAudienceUris.Add(
        new Uri(audience, UriKind.RelativeOrAbsolute));

      jsonTokenHandler.Configuration.CertificateValidator = X509CertificateValidator.None;

      jsonTokenHandler.Configuration.IssuerTokenResolver =
        SecurityTokenResolver.CreateDefaultSecurityTokenResolver(
          new ReadOnlyCollection<SecurityToken>(new List<SecurityToken>(
            new SecurityToken[]
            {
              new X509SecurityToken(currentCertificate)
            })), false);

      ConfigurationBasedIssuerNameRegistry issuerNameRegistry = new ConfigurationBasedIssuerNameRegistry();
      issuerNameRegistry.AddTrustedIssuer(currentCertificate.Thumbprint, Config.ExchangeApplicationIdentifier);
      jsonTokenHandler.Configuration.IssuerNameRegistry = issuerNameRegistry;

      return jsonTokenHandler;
    }
```


### GetSigningCertificate メソッド

 **GetSigningCertificate** メソッドでは、 **GetMetadataDocument** メソッドを呼び出して Exchange サーバーから認証メタデータを取得し、認証メタデータ ドキュメントの最初の X.509 証明書を返します。ドキュメントが存在しない場合はアプリケーション例外をスローします。


```
    private X509Certificate2 GetSigningCertificate(Uri authMetadataEndpoint)
    {
      JsonAuthMetadataDocument document = GetMetadataDocument(authMetadataEndpoint);

      if (null != document.keys &amp;&amp; document.keys.Length > 0)
      {
        JsonKey signingKey = document.keys[0];

        if (null != signingKey &amp;&amp; null != signingKey.keyValue)
        {
          return new X509Certificate2(Encoding.UTF8.GetBytes(signingKey.keyValue.value));
        }
      }

      throw new ApplicationException("The metadata document does not contain a signing certificate.");
    }

```


### GetMetadataDocument メソッド

認証メタデータ ドキュメントには、Exchange の ID トークンの署名を検証するために必要な情報が含まれています。ドキュメントは JSON 文字列として送られます。 **GetMetatDataDocument** メソッドは、Exchange の ID トークンで指定された場所にドキュメントを要求し、JSON 文字列をカプセル化したオブジェクトを返します。URL に認証メタデータ ドキュメントが含まれていない場合はアプリケーション例外をスローします。


```
    private JsonAuthMetadataDocument GetMetadataDocument(Uri authMetadataEndpoint)
    {
      // Uncomment the next line if your Exchange server uses the default
      // self-signed certificate.
      // ServicePointManager.ServerCertificateValidationCallback = Config.CertificateValidationCallback;

      byte[] acsMetadata;
      using (WebClient webClient = new WebClient())
      {
        acsMetadata = webClient.DownloadData(authMetadataEndpoint);
      }
      string jsonResponseString = Encoding.UTF8.GetString(acsMetadata);

      JsonAuthMetadataDocument document = new JavaScriptSerializer().Deserialize<JsonAuthMetadataDocument>(jsonResponseString);

      if (null == document)
      {
        throw new ApplicationException(String.Format("No authentication metadata document found at {0}.", authMetadataEndpoint));
      }

      return document;
    }
```

既定では、Exchange サーバーは自己署名 X.509 証明書を使用して認証メタデータ ドキュメントの要求を認証します。ルート サーバーまでつながる証明書をインストールしない場合は、証明書検証のコールバック メソッドを作成する必要があります。メソッドを作成しない場合、認証メタデータ ドキュメントの要求は失敗となります。

.NET Framework の System.Net 名前空間の  **ServicePointManager** クラスを使用すると、 **ServerCertificateValidationCallback** プロパティを設定することによって、検証のコールバック メソッドをフックできます。開発とテストに適した証明書検証のコールバック メソッドの例については、「 [Validating X509 certificates](http://msdn.microsoft.com/ja-jp/library/dd633677%28EXCHG.80%29.aspx)」を参照してください。


 **セキュリティ メモ**  証明書検証のコールバック メソッドを使用する場合は、組織のセキュリティ要件を満たしていることを確認してください。


## Exchange アカウントの一意 ID の計算


Exchange アカウントの一意識別子は、認証メタデータ ドキュメントの URL とアカウントの Exchange ID を使用してハッシュを求めることによって作成できます。この一意識別子は、Outlook アドイン Web サービスのシングル サインオン (SSO) システムの作成に使用できます。SSO での一意識別子の使用の詳細については、「 [Exchange の ID トークンを使用してユーザーを認証する](../outlook/authenticate-a-user-with-an-identity-token.md)」をご覧ください。

 **UniqueUserIdentification** プロパティは、 **System.Security.Cryptography** 名前空間の標準の SHA256 プロバイダーを使用して、Exchange ID と認証メタデータ URL のソルトを使用した SHA 256 ハッシュを作成します。


 **セキュリティ メモ**  認証メタデータ ドキュメントと Exchange ID からハッシュを求めてアカウントの一意識別子を作成する必要があります。Exchange ID のみを使用すると、許可されていないユーザーがサービスを利用できる場合があります。また、認証およびセキュリティを扱う場合の常として、この方法で作成された一意識別子の使用が、アプリケーションのセキュリティ要件を満たしている必要があります。




```C#
    // Salt to apply when creating unique ID.
    private byte[] Salt = new byte[] {<Provide random salt bytes here };

    private string ComputeUniqueIdentification()
    {
      byte[] inputBytes = Encoding.ASCII.GetBytes(string.Concat(ExchangeID, AuthenticationMetaDataUrl));

      // Combine input bytes and salt.
      byte[] saltedInput = new byte[Salt.Length + inputBytes.Length];
      Salt.CopyTo(saltedInput, 0);
      inputBytes.CopyTo(saltedInput, Salt.Length);

      // Compute the unique key.
      byte[] hashedBytes = SHA256CryptoServiceProvider.Create().ComputeHash(saltedInput);

      // Convert the hashed value to a string and return.
      return BitConverter.ToString(hashedBytes);
    }

    public string UniqueUserIdentification
    {
      get { return ComputeUniqueIdentification(); }
    }


```


## ユーティリティ オブジェクト


この記事のコード例では、使用する定数のフレンドリ名を定めるユーティリティ オブジェクトをいくつか使用しています。これらのユーティリティ オブジェクトを次の表に示します。


**表 1: ユーティリティ オブジェクト**


|**オブジェクト**|**説明**|
|:-----|:-----|
|**AuthClaimsType**|トークンの検証コードが使用するクレームの識別子を 1 か所にまとめます。|
|**Config**|ID トークンを検証する定数を提供します。|
|**JsonAuthMetadataDocument**|Exchange サーバーから送られた JSON 認証メタデータ ドキュメントをカプセル化します。|

### AuthClaimTypes オブジェクト

 **AuthClaimTypes** オブジェクトは、トークンの検証コードが使用するクレームの識別子を 1 か所にまとめます。JWT の標準のクレームと、Exchange の ID トークン用のクレームの両方を含みます。


```
  public class AuthClaimTypes
  {
    public const string NameIdentifier =
        JsonWebTokenConstants.ReservedClaims.NameIdentifier;
    public const string MsExchImmutableId = "msexchuid";
    public const string MsExchTokenVersion = "version";
    public const string MsExchAuthMetadataUrl = "amurl";

    public const string AppContext =
        JsonWebTokenConstants.ReservedClaims.AppContext;
    public const string Audience =
        JsonWebTokenConstants.ReservedClaims.Audience;
    public const string Issuer =
        JsonWebTokenConstants.ReservedClaims.Issuer;
    public const string ValidFrom =
        JsonWebTokenConstants.ReservedClaims.NotBefore;
    public const string ValidTo =
        JsonWebTokenConstants.ReservedClaims.ExpiresOn;

    public const string AppContextSender = "appctxsender";
    public const string IsBrowserHostedApp = "isbrowserhostedapp";

    public const string TokenType = "typ";
    public const string Algorithm = "alg";
    public const string x509Thumbprint = "x5t";      
  }
```


### Config オブジェクト

 **Config** オブジェクトは、ID トークンの検証に使用する定数と、ルート証明書までつながる X.509 証明書がサーバーにない場合に使用できる証明書検証のコールバック メソッドを含みます。


 **セキュリティ メモ**  セキュリティ証明書のコールバック メソッドが必要なのは、サーバーが既定の自己署名証明書を使用している場合のみです。この例のコールバック メソッドは、証明書が自己署名証明書の場合には  **false** を返すので、組織のセキュリティ要件を満たすコールバック メソッドに置き換える必要があります。開発とテストに適した証明書検証のコールバック メソッドの例については、「 [Validating X509 certificates](http://msdn.microsoft.com/ja-jp/library/dd633677%28EXCHG.80%29.aspx)」を参照してください。


```
  public static class Config
  {
    public static string Algorithm = "RS256";
    public static string Audience = @"https:\\localhost:44300\Pages\IdentityTest.html";
    public static string TokenType = "JWT";
    public static string Version = "ExIdTok.V1";

    public static string ExchangeApplicationIdentifier = "Exchange";

    internal static bool CertificateValidationCallback(
    object sender,
    System.Security.Cryptography.X509Certificates.X509Certificate certificate,
    System.Security.Cryptography.X509Certificates.X509Chain chain,
    System.Net.Security.SslPolicyErrors sslPolicyErrors)
    {
      // If the certificate is a valid, signed certificate, return true.
      if (sslPolicyErrors == System.Net.Security.SslPolicyErrors.None)
      {
        return true;
      }

      // If there are errors in the certificate chain, look at each error to determine the cause.
      else
      {
        return false;
      }
    }
  }
```


### JsonAuthMetadataDocument オブジェクト

 **JsonAuthMetadataDocument** オブジェクトは、プロパティを通じて認証メタデータ ドキュメントの内容を公開します。


```
using System;

namespace IdentityTest
{
  public class JsonAuthMetadataDocument
  {
    public string id { get; set; }
    public string version { get; set; }
    public string name { get; set; }
    public string realm { get; set; }
    public string serviceName { get; set; }
    public string issuer { get; set; }
    public string [] allowedAudiences { get; set; }
    public JsonKey[] keys;
    public JsonEndpoint[] endpoints;
  }

  public class JsonEndpoint
  {
    public string location { get; set; }
    public string protocol { get; set; }
    public string usage { get; set; }
  }

  public class JsonKey
  {
    public string usage { get; set; }
    public JsonKeyValue keyValue { get; set; }
  }

  public class JsonKeyValue
  {
    public string type { get; set; }
    public string value { get; set; }
  }
}

```


## その他のリソース



- [Exchange の ID トークンを使用して Outlook アドインを認証する](../outlook/authentication.md)
    
- [Exchange の ID トークンの内部](../outlook/inside-the-identity-token.md)
    
