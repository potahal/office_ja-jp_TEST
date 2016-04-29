
# Exchange の ID トークンを使用してユーザーを認証する
認証オブジェクトを、ID トークンで表される一意の ID および一連のサービスの資格情報と照合する方法を示すコード例を示します。

 _ **適用対象:** apps for Office?| Office Add-ins?| Outlook_

情報サービスにシングル サインオン (SSO) 認証スキームを実装できます。SSO 認証スキームを実装すれば、Outlook アドインを使用するユーザーが、Exchange サーバー資格情報を使用して情報サービスに接続できます。この記事では、シンプルな  **Dictionary** オブジェクト ベースのユーザー データ ストアを使用して資格情報を照合する方法について説明します。

 >**メモ**  これは SSO に関するごく簡単な例で、運用コードで使用することはできません。ID と認証を処理するときは、コードが組織のセキュリティ要件を満たしていることを確認する必要があります。


## SSO 認証を使用するための前提条件


SSO に ID トークンを使用するには、サービス アプリケーションが有効な ID トークンを持つ必要があります。次の記事では、ID トークンについて、および ID トークンの要求および検証方法について説明します。


- [Exchange の ID トークンの内部](../outlook/inside-the-identity-token.md)
    
- [Exchange で ID トークンを使用して Outlook アドインからサービスを呼び出す](../outlook/call-a-service-by-using-an-identity-token.md)
    
- [Exchange のトークン検証ライブラリを使用する](../outlook/use-the-token-validation-library.md) (マネージ コードを使用する場合)、または [Exchange の ID トークンを検証する](../outlook/validate-an-identity-token.md) (トークン検証用の独自のコードを記述する場合)
    

## ユーザーの認証


以下のコード例は、ID トークンによって表される一意の ID とサービスを使用するための一連の資格情報を照合する簡易的な認証オブジェクトを示します。 **TokenAuthentication** クラスは **GetResponseFromService** メソッドを提供します。このメソッドは、既に認証済みのトークンに対する応答を返します。また、このメソッドは、資格情報を指定するようにユーザーに指示し、指定された資格情報を認証して ID トークンに関連付けます。ここに示すコードは完全なものではなく、次のオブジェクトとメソッドが提供されることを前提としています。



|**オブジェクト/メソッド**|**説明**|
|:-----|:-----|
|**LocalCredentials** オブジェクト|サービスを使用するためのユーザーの資格情報を表します。オブジェクトの構造は、サービスの要件によって異なります。|
|**IdentityToken** オブジェクト|Outlook アドインからサービスに送信されるユーザー ID トークンが含まれます。このオブジェクトには、少なくともユーザーの一意の Exchange ID と、トークンを発行したサーバーの認証メタデータ URL が含まれている必要があります。ここの例では、「 [Exchange の ID トークンを検証する](../outlook/validate-an-identity-token.md)」で定義されている ID トークン オブジェクトを使用しています。|
|**JsonResponse** オブジェクト|サービスからの応答を表します。このオブジェクトは JSON オブジェクトにシリアル化できます。|
|**CallService** メソッド|サービスを使用するためのユーザーの資格情報を含む  **LocalCredentials** オブジェクトと、サービス要求のためのデータを含むオブジェクトを使用して、サービスを呼び出します。このメソッドは、資格情報が有効であれば、要求の結果を含む **JsonReponse** オブジェクトを返します。資格情報が有効でなければ、 **null** を返します。|
|**GetCredentialsResponse** メソッド|**JsonReponse** オブジェクトを返します。メール Office アドインは、このオブジェクトを、サービスを使用するための資格情報の要求として認識します。|
|**LocalCredentialsAreValid** メソッド|サービスに対して有効な資格情報が指定されている場合は、 **true** を返します。有効な資格情報が指定されていない場合は、 **false** を返します。|

 >**メモ**  これは ID トークンを使用する方法に関する 1 つの提案に過ぎません。ID と認証を処理するときは、コードが組織のセキュリティ要件を満たしていることを確認する必要があります。


```
    public class TokenAuthentication
    {
        // This example uses a Dictionary object to store local credentials. Your application should use
        // a data store that is appropriate to the security requirements of your organization.
        private Dictionary<string, LocalCredentials> AuthenticationCache = new Dictionary<string, LocalCredentials>();

        // Salt to apply when creating unique ID.
        private byte[] Salt = new byte[] {25, 139, 201, 13};

        private JsonResponse CallService(LocalCredentials credentials, object data)
        {
            // Calls the local service to get the response for the user.
            return null;
        }

        private JsonResponse GetCredentialsResponse()
        {
            // Creates a response that tells the Outlook add-in to
            // request the user's credentials for the service.
            return null;
        }

        private bool LocalCredentialsAreValid(LocalCredentials credentials)
        {
            // Returns true if the service recognizes the credentials provided.
            return false;
        }

        private string ComputeSHA256Hash(string uniqueId, string authenticationMetadataUrl, byte[] salt)
        {
            byte[] inputBytes = Encoding.ASCII.GetBytes(string.Concat(uniqueId, authenticationMetadataUrl));

            // Combine input bytes and salt.
            byte[] saltedInput = new byte[salt.Length + inputBytes.Length];
            salt.CopyTo(saltedInput, 0);
            inputBytes.CopyTo(saltedInput, salt.Length);

            // Compute the unique key.
            byte[] hashedBytes = SHA256CryptoServiceProvider.Create().ComputeHash(saltedInput);

            // Convert the hashed value to a string and return.
            return BitConverter.ToString(hashedBytes);
        }

        public JsonResponse GetResponseFromService(IdentityToken token, LocalCredentials credentials, object data)
        {
            JsonResponse response = null;
            // This method should never be called with a null token.
            if (null == token)
            {
                throw new ArgumentNullException("token");
            }

            if (null == credentials)
            {
                string uniqueKey = ComputeSHA256Hash(token.ExchangeID, token.AuthenticationMetadataUrl, Salt);
                if (!AuthenticationCache.ContainsKey(uniqueKey))
                {
                    // The user's credentials are not in the authentication cache. Ask
                    // for the credentials.
                    response = GetCredentialsResponse();
                }
                else
                {
                    // The user's credentials are in the cache; make a request.
                    var serviceResponse = CallService(AuthenticationCache[uniqueKey], data);

                    if (null == serviceResponse)
                    {
                        // There was a problem with the stored credentials. For example,
                        // the user has ended their subscription to the service, or the
                        // credentials have expired. Get new credentials.
                        response = GetCredentialsResponse();
                    }
                    else
                    {
                        // The service returned a response to the user. Return the
                        // service response.
                        response = serviceResponse;
                    }
                }
            }
            else
            {
                // If the credentials are not null, it's a request to add an identity
                // to the authentication cache. Check to determine whether the local credentials
                // sent to the service are known.
                if (LocalCredentialsAreValid(credentials))
                {
                    // The local credentials are known. Add them to the 
                    // cached credentials.
                    string uniqueKey = ComputeSHA256Hash(token.ExchangeID, token.AuthenticationMetadataUrl, Salt);
                    AuthenticationCache.Add(uniqueKey, credentials);

                    // Get a response from the service.
                    var serviceResponse = CallService(AuthenticationCache[uniqueKey], data);

                    if (null == serviceResponse)
                    {
                        // There was a problem with the stored credentials.
                        response = GetCredentialsResponse();
                    }
                    else
                    {
                        // Return the service response to the user.
                        response = serviceResponse;
                    }
                }
            }

            return response;
        }
    }}
```


## 管理検証ライブラリによるユーザーの認証


管理ライブラリを使用して ID トークンを検証する場合は、一意のキーを計算する必要はありません。 **AppIdentityToken** クラスの **UniqueUserIdentification** プロパティは、ユーザーの一意のキーとして直接使用できます。次のコード例では、前のコード例の **GetResponseFromService** メソッドを変更して **AppIdentityToken** クラスを使用できるようにしています。


```
        public JsonResponse GetResponseFromService(AppIdentityToken token, LocalCredentials credentials, object data)
        {
            JsonResponse response = null;
            // This method should never be called with a null token.
            if (null == token)
            {
                throw new ArgumentNullException("token");
            }

            if (null == credentials)
            {
                string uniqueKey = token.UniqueUserIdentitification;
                if (!AuthenticationCache.ContainsKey(uniqueKey))
                {
                    // The user's credentials are not in the authentication cache. Ask
                    // for the credentials.
                    response = GetCredentialsResponse();
                }
                else
                {
                    // User's credentials are in the cache. Make a request.
                    var serviceResponse = CallService(AuthenticationCache[uniqueKey], data);

                    if (null == serviceResponse)
                    {
                        // There was a problem with the stored credentials. For example,
                        // the user has ended their subscription to the service, or the
                        // credentials have expired. Get new credentials.
                        response = GetCredentialsResponse();
                    }
                    else
                    {
                        // The service returned a response to the user. Return the
                        // service response.
                        response = serviceResponse;
                    }
                }
            }
            else
            {
                // If the credentials are not null, it's a request to add an identity
                // to the authentication cache. Check to determine whether the local credentials
                // sent to the service are known.
                if (LocalCredentialsAreValid(credentials))
                {
                    // The local credentials are known. Add them to the 
                    // cached credentials. 
                    string uniqueKey = token.UniqueUserIdentitification;
                    AuthenticationCache.Add(uniqueKey, credentials);

                    // Get a response from the service.
                    var serviceResponse = CallService(AuthenticationCache[uniqueKey], data);

                    if (null == serviceResponse)
                    {
                        // There was a problem with the stored credentials.
                        response = GetCredentialsResponse();
                    }
                    else
                    {
                        // Return the service response to the user.
                        response = serviceResponse;
                    }
                }
            }

            return response;
        }
```


## その他のリソース



- [Exchange の ID トークンを使用して Outlook アドインを認証する](../outlook/authentication.md)
    
- [Exchange で ID トークンを使用して Outlook アドインからサービスを呼び出す](../outlook/call-a-service-by-using-an-identity-token.md)
    
- [Exchange のトークン検証ライブラリを使用する](../outlook/use-the-token-validation-library.md)
    
- [Exchange の ID トークンを検証する](../outlook/validate-an-identity-token.md)
    
