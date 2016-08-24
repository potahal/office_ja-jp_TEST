---
ms.Toctitle: Outlook People REST API reference (Preview)
title: "Outlook People REST API リファレンス (プレビュー)"
description: "ms.TocTitle:Outlook People REST API リファレンス (プレビュー)Title:Outlook People REST API リファレンスDescription:サービス全体の人物についての情報へのアクセスを提供する People REST API と対話する方法に関するリファレンスです。ms.ContentId:99aec234-31a1-46bd-8a01-3f0d2fea22f8ms.topic: リファレンス (API) ms.date:2016 年 5 月 16 日"
ms.ContentId: 99aec234-31a1-46bd-8a01-3f0d2fea22f8
ms.date: May 16, 2016

---
[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]

# Outlook People REST API リファレンス (プレビュー)

[!INCLUDE [Add the Outlook REST API filters--beta default v1 v2 disabled](../includes/controls/addOutlookversion_betadefault_v1v2disabled.xml)] 

<p class="previewnote"><p class="previewnote">このドキュメントで取り上げる People API は、現時点ではプレビュー段階にあります。 Preview features are subject to change prior to finalization, and may break code that uses them. プレビュー機能は、最終処理までに変更される場合があります。それらを使用するコードを破棄する場合もあります。このため、一般に、実稼働コードでは実稼働バージョンの API のみを使用してください。可能な場合には、現在バージョン 2.0 が優先バージョンです。</p> If available, v2.0 is currently the preferred version..</p>


The Outlook People API helps you get information about the people that are important to you from across mail, contacts, and social networks. You can combine information about a person from many sources in a single request to a single endpoint. You can use the API to access information secured by Azure Active Directory in Office 365. It also provides acces to Microsoft accounts in these domains: office.com, hotmail.com, live.com and office365.com.

**注** この記事の内容は、同じ方法ですべてのサポート対象ドメインに適用されます。わかりやすくするために、これ以降の記述では For simplicity, the rest of this article uses **"outlook.office.com" to refer to accounts in all domains.**

## すべての People API の操作

The People API returns relevent _person_ entities with each request. A _person_ aggregates information from across mail, contacts and social networks. People API は、要求ごとに関連する person エンティティを返します。person は、メール、連絡先、およびソーシャル ネットワークのすべてからの情報を集約します。結果は、関連性の順序に並べ替えられます。この関連性は要求で指定した条件によって判断され、複数のコミュニケーション、コラボレーションおよびビジネスのリレーションシップに基づいてランク付けされます。

人物の一覧を参照する  検索を使用して人物の一覧を取得する

##People REST API の使用

###認証

Like other [Outlook REST API](..\api\use-outlook-rest-api.md#DefineOutlookRESTAPI), for every request to the People API, you include a valid access token. You must register and identify your app and obtain the appropriate authorization to get an access token. You can [find out more](..\api\use-outlook-rest-api.md#ShortRegAuthWorkflow) about some streamlined registration and authorization options for you.

###サポートされている REST 処理およびエンドポイント

People REST API を操作する場合は、サポートされているエンドポイントに HTTP GET 要求を送信します。 

パス URL リソース名とクエリ パラメーターは、大文字と小文字が区別されません。ただし、割り当てた値、エンティティ ID、および base64 でエンコードされた値では大文字と小文字が区別されます。

すべての People REST API 要求で次のルート URL 形式を使用します。

`https://outlook.office.com/api/{version}/{user context}`

###API のバージョン

{version}`{version}` は、指定したルート URL の REST API のバージョンを表します。指定可能なバージョンは The only version that you can specify is `beta`.

* beta`beta`:このバージョンはプレビューであるため、運用コードでは使用しないでください。URL の例は An example URL is `https://outlook.office.com/api/beta/me/people`. betaこのバージョンはプレビュー状態であるため、運用コードでは使用しないでください。exampleURL は http://outlook.office.com/api/beta/me/events です。このバージョンには、GA 段階の最新の API と、プレビュー段階の追加 API セットが含まれており、最終処理までに変更される場合があります。
  
###ターゲット ユーザー

{user_context}`{user_context}` は、現在サインインしているユーザーです。People API は、すべての要求を現在のユーザーの代理として実行します。 REST _要求_でユーザー コンテキストを指定するには、次の方法を使用します。

- With the `me` shortcut: `/api/{version}/me`. The root URL becomes `https://outlook.office.com/api/{version}/me`.

サーバー _responses_ では、ユーザー コンテキストは users/{AAD_userId@AAD_tenandId} 形式で識別されます。

<a name="BrowsePeople"> </a>
###人物の参照

[Paging a response](#BrowsePaging) | 
[Sorting a response](#BrowseSort) | 
[Setting the number of people in a response](#BrowsePageSize) | 
[Selecting the fields returned](#BrowseSelecting) |
[Filtering the response](#BrowseFiltering) |
[Filtering and selecting the fields returned](#BrowseSelectingAndFiltering)

__**必要なスコープ**: https://outlook.office.com/people.read__

<a name="DefaultBrowse"> </a>次に示す要求では、コミュニケーション、コラボレーション、およびビジネスのリレーションシップに基づいて、ユーザーに最も関連のある人物を取得します。既定では、応答ごとに 10 件のレコードが返されます。ただし、$top パラメーターを使用することで、これを変更できます。 次に示す要求では、コミュニケーション、コラボレーション、およびビジネスのリレーションシップに基づいて、ユーザーに最も関連のある人物を取得します。既定では、応答ごとに 10 件のレコードが返されます。ただし、_$top_ パラメーターを使用することで、[これを変更](#BrowsePageSize)できます。

```no-highlight
https://outlook.office.com/api/beta/me/people/
```
要求に対する応答は次のようになります。

```no-highlight
    {
      "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/People",
      "value": [
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQAMPSuxK9KrREk1tt36xsca8=')",
          "Id": "AAUQAMPSuxK9KrREk1tt36xsca8=",
          "Sources": [
            {
              "Type": "Directory"
            }
          ],
          "DisplayName": "Stuart Bui",
          "GivenName": "Stuart",
          "Surname": "Bui",
          "Title": "LEAD",
          "EmailAddresses": [
            {
              "Address": "sbui@contoso.com"
            }
          ],
          "CompanyName": "CONTOSO",
          "OfficeLocation": "Building 4"
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQAMLasTGasi1NmGqss0adMLI=')",
          "Id": "AAUQAMLasTGasi1NmGqss0adMLI=",
          "Sources": [
            {
              "Type": "Directory"
            }
          ],
          "DisplayName": "Phillip Wooley",
          "GivenName": "Phillip",
          "Surname": "Wooley",
          "Title": "MANAGER",
          "EmailAddresses": [
            {
              "Address": "phillipw@contoso.com"
            }
          ],
          "CompanyName": "CONTOSO",
          "OfficeLocation": "Building 4"
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQACfUjORcHM1Juw-lAPzHFIk=')",
          "Id": "AAUQACfUjORcHM1Juw-lAPzHFIk=",
          "Sources": [
            {
              "Type": "Directory"
            }
          ],
          "DisplayName": "Nestor Kellum",
          "GivenName": "Nestor",
          "Surname": "Kellum",
          "Title": "ENGINEER",
          "EmailAddresses": [
            {
              "Address": "nestork@contoso.com"
            },
             {
              "Address": "nestork@hotmail.com"
            }
          ],
          "CompanyName": "CONTOSO",
          "OfficeLocation": "Building 4"
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQADSJ952xczZGkD0tsvxPomg=')",
          "Id": "AAUQADSJ952xczZGkD0tsvxPomg=",
          "Sources": [
            {
              "Type": "Directory"
            }
          ],
          "DisplayName": "Lacey Burns",
          "GivenName": "Lacey",
          "Surname": "Burns",
          "Title": "MANAGER",
          "EmailAddresses": [
            {
              "Address": "laceyb@contoso.com"
            }
          ],
          "CompanyName": "CONTOSO",
          "OfficeLocation": "Building 4"
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQAMJuQ5gE3IdFl1lpkykoJeY=')",
          "Id": "AAUQAMJuQ5gE3IdFl1lpkykoJeY=",
          "Sources": [
            {
              "Type": "Directory"
            }
          ],
          "DisplayName": "Ralph Foret",
          "GivenName": "Ralph",
          "Surname": "Foret",
          "Title": "LEAD",
          "EmailAddresses": [
            {
              "Address": "ralphf@contoso.com"
            }
          ],
          "CompanyName": "CONTOSO",
          "OfficeLocation": "Building 4"
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQAMrt5YwDXhJIlWoRZX9ebnw=')",
          "Id": "AAUQAMrt5YwDXhJIlWoRZX9ebnw=",
          "Sources": [
            {
              "Type": "Directory"
            }
          ],
          "DisplayName": "Emery Hixson",
          "GivenName": "Emery",
          "Surname": "Hixson",
          "Title": "MANAGER",
          "EmailAddresses": [
            {
              "Address": "emeryh@contoso.com"
            }
          ],
          "CompanyName": "CONTOSO",
          "OfficeLocation": "Building 4"
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQAOFizzhIt3ZCh4pW33s_iOQ=')",
          "Id": "AAUQAOFizzhIt3ZCh4pW33s_iOQ=",
          "Sources": [
            {
              "Type": "Directory"
            }
          ],
          "DisplayName": "Stephanie Ray",
          "GivenName": "Stephanie",
          "Surname": "Ray",
          "Title": "MANAGER",
          "EmailAddresses": [
            {
              "Address": "stephanier@contoso.com"
            }
          ],
          "CompanyName": "CONTOSO",
          "OfficeLocation": "Building 12"
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQAKirlq5lgdFHuhZ8O9FI8XA=')",
          "Id": "AAUQAKirlq5lgdFHuhZ8O9FI8XA=",
          "Sources": [
            {
              "Type": "Directory"
            }
          ],
          "DisplayName": "Natalie Roach",
          "GivenName": "Natalie",
          "Surname": "Roach",
          "Title": "LEAD",
          "EmailAddresses": [
            {
              "Address": "natalier@contoso.com"
            }
          ],
          "CompanyName": "CONTOSO",
          "OfficeLocation": "Building 4"
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQAKAXFd--O4lGr6RNJzbBXwE=')",
          "Id": "AAUQAKAXFd--O4lGr6RNJzbBXwE=",
          "Sources": [
            {
              "Type": "Directory"
            }
          ],
          "DisplayName": "Ignacio Slayton",
          "GivenName": "Ignacio",
          "Surname": "Slayton",
          "Title": "MANAGER",
          "EmailAddresses": [
            {
              "Address": "ignacios@contoso.com"
            }
          ],
          "CompanyName": "CONTOSO",
          "OfficeLocation": "Building 3"
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQAAxHEg0Rus9CmXr0FE4muco=')",
          "Id": "AAUQAAxHEg0Rus9CmXr0FE4muco=",
          "Sources": [
            {
              "Type": "Directory"
            }
          ],
          "DisplayName": "Clayton Flanigan",
          "GivenName": "Clayton",
          "Surname": "Flanigan",
          "Title": "ENGINEER",
          "EmailAddresses": [
            {
              "Address": "claytonf@contoso.com"
            }
          ],
          "CompanyName": "CONTOSO",
          "OfficeLocation": "Building 4"
        }
      ],
      "@odata.nextLink": "https://outlook.office.com/api/beta/me/people/?%24top=10&%24skip=10"
    }

```

****

人物の続きのページの要求

最初の応答では関連のある人物の完全なリストが含めきれない場合は、追加の情報ページを要求するために、_$top_ と _$skip_ を使用して 2 番目の要求を行うことができます。前の要求に追加情報が含まれている場合は、次の要求でサーバーから人物についての後続ページを取得します。 If the [previous request](#DefaultBrowse) has additional information, the following request gets the next page of people from the server.

```no-highlight
https://outlook.office.com/api/beta/me/people/?$top=10&$skip=10
```

サーバーからの応答を次に示します。

```no-highlight
    {
      "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/People",
      "value": [
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQADrqbKGdhxdNkjXAVQHEvMQ=')",
          "Id": "AAUQADrqbKGdhxdNkjXAVQHEvMQ=",
          "Sources": [
            {
              "Type": "Directory"
            }
          ],
          "DisplayName": "Monte Lafferty",
          "GivenName": "Monte",
          "Surname": "Lafferty",
          "Title": "ENGINEER",
          "EmailAddresses": [
            {
              "Address": "montel@contoso.com"
            }
          ],
          "CompanyName": "CONTOSO",
          "OfficeLocation": "Building 4"
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQAHiSySeJLAtAo6t0FuPmns0=')",
          "Id": "AAUQAHiSySeJLAtAo6t0FuPmns0=",
          "Sources": [
            {
              "Type": "Directory"
            }
          ],
          "DisplayName": "Madelyn Cooley",
          "GivenName": "Madelyn",
          "Surname": "Cooley",
          "Title": "MANAGER",
          "EmailAddresses": [
            {
              "Address": "madelync@contoso.com"
            }
          ],
          "CompanyName": "CONTOSO",
          "OfficeLocation": "Building 11"
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQACQ-gdlH_4hPvR5o7cUkm7A=')",
          "Id": "AAUQACQ-gdlH_4hPvR5o7cUkm7A=",
          "Sources": [
            {
              "Type": "Directory"
            }
          ],
          "DisplayName": "Leo Scholl",
          "GivenName": "Leo",
          "Surname": "Scholl",
          "Title": "ENGINEER",
          "EmailAddresses": [
            {
              "Address": "leos@contoso.com"
            }
          ],
          "CompanyName": "CONTOSO",
          "OfficeLocation": "Building 4"
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQAHc2322QSPBJtxkeVUcXPGs=')",
          "Id": "AAUQAHc2322QSPBJtxkeVUcXPGs=",
          "Sources": [
            {
              "Type": "Directory"
            }
          ],
          "DisplayName": "Lucinda Burke",
          "GivenName": "Lucinda",
          "Surname": "Burke",
          "Title": "ENGINEER",
          "EmailAddresses": [
            {
              "Address": "shoebm@exchange.contoso.com"
            }
          ],
          "CompanyName": "CONTOSO",
          "OfficeLocation": "Building 4"
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQAEld1zm_6W9Covp5NAGvszw=')",
          "Id": "AAUQAEld1zm_6W9Covp5NAGvszw=",
          "Sources": [
            {
              "Type": "Directory"
            }
          ],
          "DisplayName": "Ernesto Fellows",
          "GivenName": "Ernesto",
          "Surname": "Fellows",
          "Title": "LEAD",
          "EmailAddresses": [
            {
              "Address": "ernestof@contoso.com"
            }
          ],
          "CompanyName": "CONTOSO",
          "OfficeLocation": "Building 4"
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQACgDLi02r8RDmtK-40rUPXE=')",
          "Id": "AAUQACgDLi02r8RDmtK-40rUPXE=",
          "Sources": [
            {
              "Type": "Directory"
            }
          ],
          "DisplayName": "Margret Nash",
          "GivenName": "Margret",
          "Surname": "Nash",
          "Title": "ENGINEER",
          "EmailAddresses": [
            {
              "Address": "margretn@contoso.com"
            }
          ],
          "CompanyName": "CONTOSO",
          "OfficeLocation": "Building 4"
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQAKXZsceAVjRDiUASCswoQxY=')",
          "Id": "AAUQAKXZsceAVjRDiUASCswoQxY=",
          "Sources": [
            {
              "Type": "Directory"
            }
          ],
          "DisplayName": "Wendy Monroe",
          "GivenName": "Wendy",
          "Surname": "Monroe",
          "Title": "ARCHITECT",
          "EmailAddresses": [
            {
              "Address": "wendym@contoso.com"
            }
          ],
          "CompanyName": "CONTOSO",
          "OfficeLocation": "Building 4"
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQAO-ckLra2wFBjbgc1-F5yyQ=')",
          "Id": "AAUQAO-ckLra2wFBjbgc1-F5yyQ=",
          "Sources": [
            {
              "Type": "Directory"
            }
          ],
          "DisplayName": "Alberta Johns",
          "GivenName": "Alberta",
          "Surname": "Johns",
          "Title": "ENGINEER",
          "EmailAddresses": [
            {
              "Address": "albertaj@contoso.com"
            }
          ],
          "CompanyName": "CONTOSO",
          "OfficeLocation": "Building 4"
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQAO-Giffc5HxMibij3YNPuvU=')",
          "Id": "AAUQAO-Giffc5HxMibij3YNPuvU=",
          "Sources": [
            {
              "Type": "Directory"
            }
          ],
          "DisplayName": "Leticia Bullock",
          "GivenName": "Leticia",
          "Surname": "Bullock",
          "Title": "MANAGER",
          "EmailAddresses": [
            {
              "Address": "leticiab@contoso.com"
            }
          ],
          "CompanyName": "CONTOSO",
          "OfficeLocation": "Building 4"
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQAE6BF792ae9KoMQeDZbxDK0=')",
          "Id": "AAUQAE6BF792ae9KoMQeDZbxDK0=",
          "Sources": [
            {
              "Type": "Directory"
            }
          ],
          "DisplayName": "Angelita Dillard",
          "GivenName": "Angelita",
          "Surname": "Dillard",
          "Title": "LEAD",
          "EmailAddresses": [
            {
              "Address": "angelitad@contoso.com"
            }
          ],
          "CompanyName": "CONTOSO",
          "OfficeLocation": "Building 3"
        }
      ],
      "@odata.nextLink": "https://outlook.office.com/api/beta/me/people/?%24skip=20&%24top=10"
    }
```

****

応答の並べ替え

By default the people in the response are sorted by their relevance to your query. You can change the order of the people in the response using the _$orderby_ parameter. This query selects the people most relevant to you, sorts them by their display name, and then returns the first 10 people on the sorted list.

```no-highlight
https://outlook.office.com/api/beta/me/people/?$orderby=DisplayName 
```
サーバーからの応答を次に示します。

```no-highlight
      "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/People",
      "value": [
        {
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQAO46ZoZRt65Ajfvu2d8atiw=')",
          "Id": "AAUQAO46ZoZRt65Ajfvu2d8atiw=",
          "Sources": [
            {
              "Type": "Directory"
            }
          ],
          "DisplayName": "Angelina Blackwell",
          "GivenName": "Angelina",
          "Surname": "Blackwell",
          "Title": "MANAGER",
          "EmailAddresses": [
            {
              "Address": "angelinab@exchange.contoso.com"
            }
          ],
          "CompanyName": "CONTOSO",
          "OfficeLocation": "Building 5"
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQAGaI4BZyQURDtuk0nNvBYGk=')",
          "Id": "AAUQAGaI4BZyQURDtuk0nNvBYGk=",
          "Sources": [
            {
              "Type": "Directory"
            }
          ],
          "DisplayName": "Benita Wilkerson",
          "GivenName": "Benita",
          "Surname": "Wilkerson",
          "Title": "RESEARCHER",
          "EmailAddresses": [
            {
              "Address": "benitaw@contoso.com"
            }
          ],
          "CompanyName": "CONTOSO",
          "OfficeLocation": "Building C"
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQAKOPqJuLWk9NtmwOx29rbew=')",
          "Id": "AAUQAKOPqJuLWk9NtmwOx29rbew=",
          "Sources": [
            {
              "Type": "Directory"
            }
          ],
          "DisplayName": "Callie Sweet",
          "GivenName": "Callie",
          "Surname": "Sweet",
          "Title": "ENGINEER",
          "EmailAddresses": [
            {
              "Address": "callies@contoso.com"
            }
          ],
          "CompanyName": "CONTOSO",
          "OfficeLocation": "Building 2"
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('MkU4MDBGNDUtMEY3OS00MzM4LTg3RUUtNENFOTFBRURCODcxMjF2cmVxdWVzdC5zdXBwb3J0QG9lLjIxdmlhbmV0LmNvbQ==')",
          "Id": "MkU4MDBGNDUtMEY3OS00MzM4LTg3RUUtNENFOTFBRURCODcxMjF2cmVxdWVzdC5zdXBwb3J0QG9lLjIxdmlhbmV0LmNvbQ==",
          "Sources": [
            {
              "Type": "Communication History"
            }
          ],
          "DisplayName": "contoso.support",
          "GivenName": null,
          "Surname": null,
          "Title": null,
          "EmailAddresses": [
            {
              "Address": "contoso.support@outlook.office.com"
            }
          ],
          "CompanyName": null,
          "OfficeLocation": null
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQACtCaoUN6WdNgpyCSeKSP6E=')",
          "Id": "AAUQACtCaoUN6WdNgpyCSeKSP6E=",
          "Sources": [
            {
              "Type": "Directory"
            }
          ],
          "DisplayName": "Dianne Frederick",
          "GivenName": "Dianne",
          "Surname": "Frederick",
          "Title": "ENGINEER",
          "EmailAddresses": [
            {
              "Address": "diannef@contoso.com"
            }
          ],
          "CompanyName": "CONTOSO",
          "OfficeLocation": "Building 3"
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQAKNvearlKU1PsHmDb-xmDrQ=')",
          "Id": "AAUQAKNvearlKU1PsHmDb-xmDrQ=",
          "Sources": [
            {
              "Type": "Directory"
            }
          ],
          "DisplayName": "Jenifer Dotson",
          "GivenName": "Jenifer",
          "Surname": "Dotson",
          "Title": "SENENGINEER",
          "EmailAddresses": [
            {
              "Address": "jeniferd@contoso.com"
            }
          ],
          "CompanyName": "CONTOSO",
          "OfficeLocation": "Building 4"
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQAAVBb0xn3ctFiZreqwDwZf0=')",
          "Id": "AAUQAAVBb0xn3ctFiZreqwDwZf0=",
          "Sources": [
            {
              "Type": "Directory"
            }
          ],
          "DisplayName": "Nancy Washington",
          "GivenName": "Nancy",
          "Surname": "Washington",
          "Title": "MANAGER",
          "EmailAddresses": [
            {
              "Address": "nancyw@exchange.contoso.com"
            }
          ],
          "CompanyName": "CONTOSO",
          "OfficeLocation": "Building 3"
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQAKirlq5lgdFHuhZ8O9FI8XA=')",
          "Id": "AAUQAKirlq5lgdFHuhZ8O9FI8XA=",
          "Sources": [
            {
              "Type": "Directory"
            }
          ],
          "DisplayName": "Natalie Roach",
          "GivenName": "Natalie",
          "Surname": "Roach",
          "Title": "LEAD",
          "EmailAddresses": [
            {
              "Address": "natalier@contoso.com"
            }
          ],
          "CompanyName": "CONTOSO",
          "OfficeLocation": "Building 4"
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQAKVYmyKcnQ9Njw5bvInrWYI=')",
          "Id": "AAUQAKVYmyKcnQ9Njw5bvInrWYI=",
          "Sources": [
            {
              "Type": "Directory"
            }
          ],
          "DisplayName": "Natalie Roach's Team",
          "GivenName": null,
          "Surname": null,
          "Title": null,
          "EmailAddresses": [
            {
              "Address": "ntteam@contoso.com"
            }
          ],
          "CompanyName": null,
          "OfficeLocation": null
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQAFcMhzA6m4pBlHmc3Qa_PpA=')",
          "Id": "AAUQAFcMhzA6m4pBlHmc3Qa_PpA=",
          "Sources": [
            {
              "Type": "Directory"
            }
          ],
          "DisplayName": "Queen Duncan",
          "GivenName": "Queen",
          "Surname": "Duncan",
          "Title": "MANAGER",
          "EmailAddresses": [
            {
              "Address": "queend@contoso.com"
            }
          ],
          "CompanyName": "CONTOSO",
          "OfficeLocation": "Building 3"
        }
      ],
      "@odata.nextLink": "https://outlook.office.com/api/beta/me/people/?%24orderby=DisplayName&%24top=10&%24skip=10"
    }
```

****

返される人物の数と返されるフィールドの変更

応答で返される人物の数は、_$top_ パラメーターを設定することで変更できます。 

The following example requests the 1,000 most relevant people. 次に示す例では、最も関連のある 1,000 人の人物を要求します。また、この要求では、人物の表示名のみを要求することで、サーバーから返されるデータの量も制限しています。

```no-highlight
https://outlook.office.com/api/beta/me/people/?$top=1000&$Select=DisplayName
```

サーバーからの応答を次に示します。

```no-highlight
    {
      "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/People(DisplayName)",
      "value": [
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQAMPSuxK9KrREk1tt36xsca8=')",
          "Id": "AAUQAMPSuxK9KrREk1tt36xsca8=",
          "DisplayName": "Brian Remick"
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQAMLasTGasi1NmGqss0adMLI=')",
          "Id": "AAUQAMLasTGasi1NmGqss0adMLI=",
          "DisplayName": "Phillip Wooley"
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQACfUjORcHM1Juw-lAPzHFIk=')",
          "Id": "AAUQACfUjORcHM1Juw-lAPzHFIk=",
          "DisplayName": "Nestor Kellum"
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQADSJ952xczZGkD0tsvxPomg=')",
          "Id": "AAUQADSJ952xczZGkD0tsvxPomg=",
          "DisplayName": "Lacey Burns"
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQAMJuQ5gE3IdFl1lpkykoJeY=')",
          "Id": "AAUQAMJuQ5gE3IdFl1lpkykoJeY=",
          "DisplayName": "Ralph Foret"
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQAMrt5YwDXhJIlWoRZX9ebnw=')",
          "Id": "AAUQAMrt5YwDXhJIlWoRZX9ebnw=",
          "DisplayName": "Emery Hixson"
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQAOFizzhIt3ZCh4pW33s_iOQ=')",
          "Id": "AAUQAOFizzhIt3ZCh4pW33s_iOQ=",
          "DisplayName": "Stephanie Ray"
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQAKirlq5lgdFHuhZ8O9FI8XA=')",
          "Id": "AAUQAKirlq5lgdFHuhZ8O9FI8XA=",
          "DisplayName": "Natalie Roach"
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQAKAXFd--O4lGr6RNJzbBXwE=')",
          "Id": "AAUQAKAXFd--O4lGr6RNJzbBXwE=",
          "DisplayName": "Ignacio Slayton"
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQAAxHEg0Rus9CmXr0FE4muco=')",
          "Id": "AAUQAAxHEg0Rus9CmXr0FE4muco=",
          "DisplayName": "Clayton Flanigan"
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQADrqbKGdhxdNkjXAVQHEvMQ=')",
          "Id": "AAUQADrqbKGdhxdNkjXAVQHEvMQ=",
          "DisplayName": "Monte Lafferty"
        },
        ... etc
```

****

返されるフィールドの選択

サーバーから返されるデータの量は、1 つ以上のフィールドを選択する _$select_ パラメーターを使用することで制限できます。_@odata.id_ フィールドは常に返されます。

次に示す例では、最も関連のある 10 人の人物の _DisplayName_ と _EmailAddress_ に応答を制限します。

```no-highlight
https://outlook.office.com/api/beta/me/people/?$select=DisplayName,EmailAddresses
```
サーバーからの応答を次に示します。

```no-highlight
    {
      "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/People(DisplayName,EmailAddresses)",
      "value": [
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQAMPSuxK9KrREk1tt36xsca8=')",
          "Id": "AAUQAMPSuxK9KrREk1tt36xsca8=",
          "DisplayName": "Brian Remick",
          "EmailAddresses": [
            {
              "Address": "bremick@contoso.com"
            }
          ]
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQAMLasTGasi1NmGqss0adMLI=')",
          "Id": "AAUQAMLasTGasi1NmGqss0adMLI=",
          "DisplayName": "Phillip Wooley",
          "EmailAddresses": [
            {
              "Address": "phillipw@contoso.com"
            }
          ]
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQACfUjORcHM1Juw-lAPzHFIk=')",
          "Id": "AAUQACfUjORcHM1Juw-lAPzHFIk=",
          "DisplayName": "Nestor Kellum",
          "EmailAddresses": [
            {
              "Address": "nestork@contoso.com"
            },
             {
              "Address": "nestork@outlook.office.com"
            }
          ]
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQADSJ952xczZGkD0tsvxPomg=')",
          "Id": "AAUQADSJ952xczZGkD0tsvxPomg=",
          "DisplayName": "Lacey Burns",
          "EmailAddresses": [
            {
              "Address": "laceyb@contoso.com"
            }
          ]
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQAMJuQ5gE3IdFl1lpkykoJeY=')",
          "Id": "AAUQAMJuQ5gE3IdFl1lpkykoJeY=",
          "DisplayName": "Ralph Foret",
          "EmailAddresses": [
            {
              "Address": "ralphf@contoso.com"
            }
          ]
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQAMrt5YwDXhJIlWoRZX9ebnw=')",
          "Id": "AAUQAMrt5YwDXhJIlWoRZX9ebnw=",
          "DisplayName": "Emery Hixson",
          "EmailAddresses": [
            {
              "Address": "emeryh@contoso.com"
            }
          ]
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQAOFizzhIt3ZCh4pW33s_iOQ=')",
          "Id": "AAUQAOFizzhIt3ZCh4pW33s_iOQ=",
          "DisplayName": "Stephanie Ray",
          "EmailAddresses": [
            {
              "Address": "stephanier@contoso.com"
            }
          ]
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQAKirlq5lgdFHuhZ8O9FI8XA=')",
          "Id": "AAUQAKirlq5lgdFHuhZ8O9FI8XA=",
          "DisplayName": "Natalie Roach",
          "EmailAddresses": [
            {
              "Address": "natalier@contoso.com"
            }
          ]
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQAKAXFd--O4lGr6RNJzbBXwE=')",
          "Id": "AAUQAKAXFd--O4lGr6RNJzbBXwE=",
          "DisplayName": "Ignacio Slayton",
          "EmailAddresses": [
            {
              "Address": "ignacios@contoso.com"
            }
          ]
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQAAxHEg0Rus9CmXr0FE4muco=')",
          "Id": "AAUQAAxHEg0Rus9CmXr0FE4muco=",
          "DisplayName": "Clayton Flanigan",
          "EmailAddresses": [
            {
              "Address": "claytonf@contoso.com"
            }
          ]
        }
      ],
      "@odata.nextLink": "https://outlook.office.com/api/beta/me/people/?%24select=DisplayName%2cEmailAddresses&%24top=10&%24skip=10"
    }
```

****

フィルターを使用した応答の制限

_$filter_ パラメーターを使用すると、指定した条件に等しいレコードを持つ人物のみに応答を制限できます。 

次に示すクエリでは、ソース "Communications History" によって人物を制限します。

```no-highlight
https://outlook.office.com/api/beta/me/people/?$Filter=Sources/Any (source: source/Type  eq 'Communications History') 
```

```no-highlight
    {
      "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/People",
      "value": [
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('MkU4MDBGNDUtMEY3OS00MzM4LTg3RUUtNENFOTFBRURCODcxcGVvcGxlcmVsZXZhbmNlQHNlcnZpY2UuZXhjaGFuZ2UubWljcm9zb2Z0LmNvbQ==')",
          "Id": "MkU4MDBGNDUtMEY3OS00MzM4LTg3RUUtNENFOTFBRURCODcxcGVvcGxlcmVsZXZhbmNlQHNlcnZpY2UuZXhjaGFuZ2UubWljcm9zb2Z0LmNvbQ==",
          "Sources": [
            {
              "Type": "Communication History"
            }
          ],
          "DisplayName": "People Relevance",
          "GivenName": null,
          "Surname": null,
          "Title": null,
          "EmailAddresses": [
            {
              "Address": "peoplerelevance@service.exchange.contoso.com"
            }
          ],
          "CompanyName": null,
          "OfficeLocation": null
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('MkU4MDBGNDUtMEY3OS00MzM4LTg3RUUtNENFOTFBRURCODcxcmNoaUByY2hpbGF3LmNvbQ==')",
          "Id": "MkU4MDBGNDUtMEY3OS00MzM4LTg3RUUtNENFOTFBRURCODcxcmNoaUByY2hpbGF3LmNvbQ==",
          "Sources": [
            {
              "Type": "Communication History"
            }
          ],
          "DisplayName": "Edna Dickson",
          "GivenName": null,
          "Surname": null,
          "Title": null,
          "EmailAddresses": [
            {
              "Address": "ednad@outlook.office.com"
            }
          ],
          "CompanyName": null,
          "OfficeLocation": null
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('MkU4MDBGNDUtMEY3OS00MzM4LTg3RUUtNENFOTFBRURCODcxMjF2cmVxdWVzdC5zdXBwb3J0QG9lLjIxdmlhbmV0LmNvbQ==')",
          "Id": "MkU4MDBGNDUtMEY3OS00MzM4LTg3RUUtNENFOTFBRURCODcxMjF2cmVxdWVzdC5zdXBwb3J0QG9lLjIxdmlhbmV0LmNvbQ==",
          "Sources": [
            {
              "Type": "Communication History"
            }
          ],
          "DisplayName": "contoso.support",
          "GivenName": null,
          "Surname": null,
          "Title": null,
          "EmailAddresses": [
            {
              "Address": "contoso.support@contoso.com"
            }
          ],
          "CompanyName": null,
          "OfficeLocation": null
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('MkU4MDBGNDUtMEY3OS00MzM4LTg3RUUtNENFOTFBRURCODcxUGVvcGxlOTExMUBzZXJ2aWNlLmV4Y2hhbmdlLm1pY3Jvc29mdC5jb20=')",
          "Id": "MkU4MDBGNDUtMEY3OS00MzM4LTg3RUUtNENFOTFBRURCODcxUGVvcGxlOTExMUBzZXJ2aWNlLmV4Y2hhbmdlLm1pY3Jvc29mdC5jb20=",
          "Sources": [
            {
              "Type": "Communication History"
            }
          ],
          "DisplayName": "gethelp@contoso.com",
          "GivenName": null,
          "Surname": null,
          "Title": null,
          "EmailAddresses": [
            {
              "Address": "gethelp@contoso.com"
            }
          ],
          "CompanyName": null,
          "OfficeLocation": null
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('MkU4MDBGNDUtMEY3OS00MzM4LTg3RUUtNENFOTFBRURCODcxc21hcnRhbGVydHNAbWljcm9zb2Z0LmNvbQ==')",
          "Id": "MkU4MDBGNDUtMEY3OS00MzM4LTg3RUUtNENFOTFBRURCODcxc21hcnRhbGVydHNAbWljcm9zb2Z0LmNvbQ==",
          "Sources": [
            {
              "Type": "Communication History"
            }
          ],
          "DisplayName": "smartalerts@contoso.com",
          "GivenName": null,
          "Surname": null,
          "Title": null,
          "EmailAddresses": [
            {
              "Address": "smartalerts@contoso.com"
            }
          ],
          "CompanyName": null,
          "OfficeLocation": null
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('MkU4MDBGNDUtMEY3OS00MzM4LTg3RUUtNENFOTFBRURCODcxam9lQG9saXZlYW5kZ29vc2UuY29t')",
          "Id": "MkU4MDBGNDUtMEY3OS00MzM4LTg3RUUtNENFOTFBRURCODcxam9lQG9saXZlYW5kZ29vc2UuY29t",
          "Sources": [
            {
              "Type": "Communication History"
            }
          ],
          "DisplayName": "Emily Burnett",
          "GivenName": null,
          "Surname": null,
          "Title": null,
          "EmailAddresses": [
            {
              "Address": "emilyb@contoso.com"
            }
          ],
          "CompanyName": null,
          "OfficeLocation": null
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('MkU4MDBGNDUtMEY3OS00MzM4LTg3RUUtNENFOTFBRURCODcxYS1jYW1hbm5AbWljcm9zb2Z0LmNvbQ==')",
          "Id": "MkU4MDBGNDUtMEY3OS00MzM4LTg3RUUtNENFOTFBRURCODcxYS1jYW1hbm5AbWljcm9zb2Z0LmNvbQ==",
          "Sources": [
            {
              "Type": "Communication History"
            }
          ],
          "DisplayName": "Bridgett Baxter",
          "GivenName": null,
          "Surname": null,
          "Title": null,
          "EmailAddresses": [
            {
              "Address": "bridgettb@contoso.com"
            }
          ],
          "CompanyName": null,
          "OfficeLocation": null
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('MkU4MDBGNDUtMEY3OS00MzM4LTg3RUUtNENFOTFBRURCODcxc3RlZmFuLmJ1cmFrQGluc2lkZXZpZXcuY29t')",
          "Id": "MkU4MDBGNDUtMEY3OS00MzM4LTg3RUUtNENFOTFBRURCODcxc3RlZmFuLmJ1cmFrQGluc2lkZXZpZXcuY29t",
          "Sources": [
            {
              "Type": "Communication History"
            }
          ],
          "DisplayName": "Charlotte Stark",
          "GivenName": null,
          "Surname": null,
          "Title": null,
          "EmailAddresses": [
            {
              "Address": "charlottes@insideview.com"
            }
          ],
          "CompanyName": null,
          "OfficeLocation": null
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('MkU4MDBGNDUtMEY3OS00MzM4LTg3RUUtNENFOTFBRURCODcxbWFyaWFuYXN0ZXBwQG91dGxvb2suY29t')",
          "Id": "MkU4MDBGNDUtMEY3OS00MzM4LTg3RUUtNENFOTFBRURCODcxbWFyaWFuYXN0ZXBwQG91dGxvb2suY29t",
          "Sources": [
            {
              "Type": "Communication History"
            }
          ],
          "DisplayName": "herminiah@outlook.office.com",
          "GivenName": null,
          "Surname": null,
          "Title": null,
          "EmailAddresses": [
            {
              "Address": "herminiah@outlook.office.com"
            }
          ],
          "CompanyName": null,
          "OfficeLocation": null
        }
      ],
      "@odata.nextLink": "https://outlook.office.com/api/beta/me/people/?%24Filter=Sources%2fAny+(source%3a+source%2fType++eq+%27Communication+History%27)&%24top=10&%24skip=10"
    }
```

****

フィルター処理された応答で返されるフィールドを選択する

_$select_ パラメーターと _$filter_ パラメーターを組み合わせることで、ユーザーに関連のある人物のカスタム リストを作成し、アプリケーションで必要になるフィールドのみを取得できます。 

次に示す例では、指定した名前と等しい表示名を持つ人物の _DisplayName_ と _EmailAddress_ を取得します。この例では、表示名が "Nestor Kellum" と等しい人物のみが返されます。 次に示す例では、指定した名前と等しい表示名を持つ人物の DisplayName と EmailAddress を取得します。この例では、表示名が "Nestor Kellum" と等しい人物のみが返されます。 

```no-highlight
https://outlook.office.com/api/beta/me/people/?$select=DisplayName,EmailAddresses&$Filter=DisplayName eq 'Nestor Kellum' 
```

サーバーからの応答を次に示します。

```no-highlight
    {
      "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/People(DisplayName,EmailAddresses)",
      "value": [
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQACfUjORcHM1Juw-lAPzHFIk=')",
          "Id": "AAUQACfUjORcHM1Juw-lAPzHFIk=",
          "DisplayName": "Nestor Kellum",
          "EmailAddresses": [
            {
              "Address": "nestork@contoso.com"
            },
             {
              "Address": "nestork@outlook.office.com"
            }
          ]
        }
      ]
    }
```

****

<a name="SearchPeople"> </a>
###人物の検索

トピックの検索 
<!-- [Limiting a search response by topic](#SearchFilter) | -->
[あいまい検索の実行](#FuzzySearch)

__**必要なスコープ**: https://outlook.office.com/people.read__

**検索による人物の選択**

_$search_ パラメーターを使用して、特定の条件セットを満たす人物を選択します。 

The following search query returns relevant people whose _GivenName_ or _Surname_ begins with the letter "j".

```no-highlight
https://outlook.office.com/api/beta/me/people/?$search=j
```
サーバーからの応答を次に示します。

```no-highlight
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQAC4PuXFfR6pAin18WlyOqxI=')",
          "Id": "AAUQAC4PuXFfR6pAin18WlyOqxI=",
          "Sources": [
            {
              "Type": "Directory"
            }
          ],
          "DisplayName": "Jacquelyn Cox",
          "GivenName": "Jacquelyn",
          "Surname": "Cox",
          "Title": "ENGINEER",
          "EmailAddresses": [
            {
              "Address": "jacquelync@contoso.com"
            }
          ],
          "CompanyName": "CONTOSO",
          "OfficeLocation": "Building S"
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQAFE5-QvXOi1KhbNrQQaV-dk=')",
          "Id": "AAUQAFE5-QvXOi1KhbNrQQaV-dk=",
          "Sources": [
            {
              "Type": "Directory"
            }
          ],
          "DisplayName": "Julie Yates",
          "GivenName": "Julie",
          "Surname": "Yates",
          "Title": "MANAGER",
          "EmailAddresses": [
            {
              "Address": "juliey@contoso.com"
            }
          ],
          "CompanyName": "CONTOSO",
          "OfficeLocation": "Building 2"
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQAChuH75CMB5NsGP0QBDcX2g=')",
          "Id": "AAUQAChuH75CMB5NsGP0QBDcX2g=",
          "Sources": [
            {
              "Type": "Directory"
            }
          ],
          "DisplayName": "Juliette Mitchell",
          "GivenName": "Juliette",
          "Surname": "Mitchell",
          "Title": "MANAGER",
          "EmailAddresses": [
            {
              "Address": "juliettem@contoso.com"
            }
          ],
          "CompanyName": "CONTOSO",
          "OfficeLocation": "Building 4"
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQAMsDacKpHNpFj7ViBXFiRZI=')",
          "Id": "AAUQAMsDacKpHNpFj7ViBXFiRZI=",
          "Sources": [
            {
              "Type": "Directory"
            }
          ],
          "DisplayName": "Katheryn Johns",
          "GivenName": "Katheryn",
          "Surname": "Johns",
          "Title": "ENGINEER",
          "EmailAddresses": [
            {
              "Address": "katherynj@contoso.com"
            }
          ],
          "CompanyName": "CONTOSO",
          "OfficeLocation": "Building T"
        }
      ],
      "@odata.nextLink": "https://outlook.office.com/api/beta/me/people/?%24search=d&%24top=10&%24skip=10"
    }
```

****

検索による関連するトピックの指定

次に示す要求は、名前に "ma" が含まれていて、"Aziz Ansari" というコメディアンに興味を示している関連のある人物を返します。

```no-highlight
 https://outlook.office.com/api/beta/me/people/?$search="ma topic: Aziz Ansari" 
 ```
 サーバーからの応答を次に示します。
 
 ```no-highlight
    {
      "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/People",
      "value": [
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQACgDLi02r8RDmtK-40rUPXE=')",
          "Id": "AAUQACgDLi02r8RDmtK-40rUPXE=",
          "Sources": [
            {
              "Type": "Directory"
            }
          ],
          "DisplayName": "Margret Nash",
          "GivenName": "Margret",
          "Surname": "Nash",
          "Title": "ENGINEER",
          "EmailAddresses": [
            {
              "Address": "margretn@contoso.com"
            }
          ],
          "CompanyName": "CONTOSO",
          "OfficeLocation": "Building 4"
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQAHiSySeJLAtAo6t0FuPmns0=')",
          "Id": "AAUQAHiSySeJLAtAo6t0FuPmns0=",
          "Sources": [
            {
              "Type": "Directory"
            }
          ],
          "DisplayName": "Madelyn Cooley",
          "GivenName": "Madelyn",
          "Surname": "Cooley",
          "Title": "MANAGER",
          "EmailAddresses": [
            {
              "Address": "madelync@contoso.com"
            }
          ],
          "CompanyName": "CONTOSO",
          "OfficeLocation": "Building 11"
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQAAebvnx4LnNKsFWppdcopJw=')",
          "Id": "AAUQAAebvnx4LnNKsFWppdcopJw=",
          "Sources": [
            {
              "Type": "Directory"
            }
          ],
          "DisplayName": "Albert Raley",
          "GivenName": "Albert",
          "Surname": "Raley",
          "Title": "MANAGER",
          "EmailAddresses": [
            {
              "Address": "albertr@contoso.com"
            }
          ],
          "CompanyName": "CONTOSO",
          "OfficeLocation": "Bulding T"
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQAPqWcitEhEFNuvaSLteeN9M=')",
          "Id": "AAUQAPqWcitEhEFNuvaSLteeN9M=",
          "Sources": [
            {
              "Type": "Directory"
            }
          ],
          "DisplayName": "Emanuel Grider",
          "GivenName": "Emanuel",
          "Surname": "Grider",
          "Title": "MANAGER",
          "EmailAddresses": [
            {
              "Address": "edh@contoso.com"
            }
          ],
          "CompanyName": "CONTOSO",
          "OfficeLocation": "Building 4"
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQAIqrTSzA3N1BglLTWFzgql4=')",
          "Id": "AAUQAIqrTSzA3N1BglLTWFzgql4=",
          "Sources": [
            {
              "Type": "Directory"
            }
          ],
          "DisplayName": "Frederick Magnuson",
          "GivenName": "Frederick",
          "Surname": "Magnuson",
          "Title": "LEAD",
          "EmailAddresses": [
            {
              "Address": "fredrickm@service.exchange.contoso.com"
            }
          ],
          "CompanyName": "CONTOSO",
          "OfficeLocation": "Building 3"
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQAId9cw-r_cZDtd5mU6QVvR4=')",
          "Id": "AAUQAId9cw-r_cZDtd5mU6QVvR4=",
          "Sources": [
            {
              "Type": "Directory"
            }
          ],
          "DisplayName": "Beryl Maddox",
          "GivenName": "Beryl",
          "Surname": "Maddox",
          "Title": "MANAGER",
          "EmailAddresses": [
            {
              "Address": "berylm@contoso.com"
            }
          ],
          "CompanyName": "CONTOSO",
          "OfficeLocation": "Building 3"
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQAPbEMn4Y_EhPoPJIAfe9-v4=')",
          "Id": "AAUQAPbEMn4Y_EhPoPJIAfe9-v4=",
          "Sources": [
            {
              "Type": "Directory"
            }
          ],
          "DisplayName": "Vilma Young",
          "GivenName": "Vilma",
          "Surname": "Young",
          "Title": "LEAD",
          "EmailAddresses": [
            {
              "Address": "vilmay@contoso.com"
            }
          ],
          "CompanyName": "CONTOSO",
          "OfficeLocation": "Building T"
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQANxLXO4NLchEiyteaLb_HNM=')",
          "Id": "AAUQANxLXO4NLchEiyteaLb_HNM=",
          "Sources": [
            {
              "Type": "Directory"
            }
          ],
          "DisplayName": "Edna Foreman",
          "GivenName": "Edna",
          "Surname": "Foreman",
          "Title": null,
          "EmailAddresses": [
            {
              "Address": "ednaf@contoso.com"
            }
          ],
          "CompanyName": "CONTOSO",
          "OfficeLocation": "Building L"
        },
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAQkADA5OWM3MmE5LTNmYmItNDk1NS1hZDY0LTAwYjBhZjc4MTg3YwAQABMxxzYk20pLinJq19EtuKc=')",
          "Id": "AAQkADA5OWM3MmE5LTNmYmItNDk1NS1hZDY0LTAwYjBhZjc4MTg3YwAQABMxxzYk20pLinJq19EtuKc=",
          "Sources": [
            {
              "Type": "Outlook Contacts"
            }
          ],
          "DisplayName": "Tammi Pickett",
          "GivenName": "Tammi",
          "Surname": "Pickett",
          "Title": "Engineer",
          "EmailAddresses": [
            {
              "Address": "tammip@contoso.com"
            }
          ],
          "CompanyName": "Contoso",
          "OfficeLocation": null
        },
      "@odata.nextLink": "https://outlook.office.com/api/beta/me/people/?%24search=%22ed+topic%3a+Aziz+ansari%22&
%24top=10&%24skip=10"
    }
```
 
 ****
 
 あいまい検索の実行
 
 次に示す要求では、"Hermaini Hall" という名前の人物について検索を実行します。"Herminia Hull" という名前の関連のある人物が存在するため Because there is a relevant person named "Herminia Hull," the information for "Herminia Hull" is returned.
 
 ```no-highlight
 https://outlook.office.com/api/beta/me/people/?$search="hermaini hall"
 ```
 
 サーバーからの応答を次に示します。
 
 ```no-highlight
    {
      "@odata.context": "https://outlook.office.com/api/beta/$metadata#Me/People",
      "value": [
        {
          "@odata.id": "https://outlook.office.com/api/beta/users/4579DA25-3624-44CD-AE44-9777EEB9D292@F56380F1-06A5-4239-B31E-C8B44DF1F05A/People('AAUQAL5wQK1L1MNNsL4qzoFWZiQ=')",
          "Id": "AAUQAL5wQK1L1MNNsL4qzoFWZiQ=",
          "Sources": [
            {
              "Type": "Directory"
            }
          ],
          "DisplayName": "Herminia Hull",
          "GivenName": "Herminia",
          "Surname": "Hull",
          "Title": "MANAGER",
          "EmailAddresses": [
            {
              "Address": "herminiah@contoso.com"
            }
          ],
          "CompanyName": "CONTOSO",
          "OfficeLocation": "Building 4"
        }
      ]
    }
```

****

[!INCLUDE [Enable filtering functionality ](../includes/controls/enablefiltering.xml)]

