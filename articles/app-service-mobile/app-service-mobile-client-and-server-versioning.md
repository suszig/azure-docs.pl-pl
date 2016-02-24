<properties
  pageTitle="Mobile Apps と Mobile Services のクライアントとサーバーの SDK バージョン管理 | Azure App Service"
  description="Mobile Services と Azure Mobile Apps のクライアント SDK およびサーバー SDK バージョンとの互換性の一覧"
  services="app-service\mobile"
  documentationCenter=""
  authors="lindydonna" 
  manager="dwrede"
  editor=""/>

<tags
  ms.service="app-service-mobile"
  ms.workload="mobile"
  ms.tgt_pltfrm="mobile-multiple"
  ms.devlang="dotnet"
  ms.topic="article"
  ms.date="12/15/2015"
  ms.author="donnam"/>

# Mobile Apps と Mobile Services のクライアントとサーバーのバージョン管理

Azure Mobile Services の最新バージョンは、 **Mobile Apps** Azure App Service の機能です。 

<!-- Azure App Service offers a number of platform benefits over Mobile Services, including continuous integration and deployment, staging lots, and VNET support.
 -->

Mobile Apps のクライアントとサーバーの Sdk はモバイル サービスの基となったが、 *いない* 互いに矛盾します。 つまり、使用する必要があります、 *Mobile Apps* クライアント SDK で、 *Mobile Apps* サーバー SDK と同様に、 *Mobile Services*します。 この契約は、クライアント/サーバー SDK により使用される特殊なヘッダー値 `ZUMO-API-VERSION` により適用されます。

注: するたびにこのドキュメントを表す、 *Mobile Services* バックエンドでは、必ずしも必要はありません Mobile Services でホストされるようにします。 これはコード変更を加えずに App Service で実行するモバイル サービスを移行することですが、サービスを使用する場合 *Mobile Services*  SDK のバージョン。

コード変更を加えず App Service への移行に関する詳細については、記事を参照してください。 [Migrate a Mobile Service to Azure App Service]します。

## ヘッダーの仕様

キー `ZUMO-API-VERSION` を HTTP ヘッダーかクエリ文字列に指定できます。 値は、フォームのバージョン文字列 **x.y.z**します。

次に例を示します。

Https://service.azurewebsites.net/tables/TodoItem を取得します。

HEADERS: ZUMO-API-VERSION: 2.0.0

POST https://service.azurewebsites.net/tables/TodoItem?ZUMO-API-VERSION=2.0.0

## バージョン チェックの除外

値を設定してバージョンの確認を避けることができます **true** アプリ設定に **MS_SkipVersionCheck**します。 これは web.config か Azure ポータルの [アプリケーション設定] セクションで指定します。

> [AZURE.NOTE] モバイル サービスとモバイル アプリ、オフラインの同期、認証、およびプッシュ通知の面で特に間での動作の変更の数があります。 テストを完了し、動作変更によりアプリの機能停止がないことを確認してからバージョン チェックを除外してください。

## すべてのバージョンの互換性のまとめ

以下の表は、あらゆる種類のクライアント/サーバー間の互換性をまとめたものです。 バックエンドがいずれか mobile classfied **サービス** または Mobile **アプリ** 、サーバーを使用して SDK に基づきます。

|                           | **モバイル サービス** Node.js または .NET | **モバイル アプリ** Node.js または .NET |
| ----------                | -----------------------             |   ----------------              |
| [Mobile Services クライアント] | [OK]                                  | エラー *                         |
| [Mobile Apps クライアント]     | エラー *                             | [OK]                              |

指定することによって制御できます \*This **MS_SkipVersionCheck**します。


<!-- IMPORTANT!  The anchors for Mobile Services and Mobile Apps MUST be 1.0.0 and 2.0.0 respectively, since there is an exception error message that uses those anchors. --> 

<!-- NOTE: the fwlink to this document is http://go.microsoft.com/fwlink/?LinkID=690568 -->

## <a name="1.0.0"></a>モバイル サービス クライアントとサーバー

次の表に、クライアント Sdk と互換性のある **Mobile Services**します。

注: Mobile Services クライアント Sdk *しない* ヘッダー値を送る `ZUMO-API-VERSION`します。 サービスがこのヘッダーやクエリ文字列値を受け取った場合、前述のように明示的に除外しない限り、エラーが返されます。

### <a name="MobileServicesClients"></a> モバイル *サービス* クライアント Sdk

| クライアント プラットフォーム                   | バージョン                                                                   | バージョンのヘッダー値 |
| -------------------               | ------------------------                                                  | -------------------  |
| 管理されたクライアント (Windows、Xamarin) | [1.3.2](https://www.nuget.org/packages/WindowsAzure.MobileServices/1.3.2) | 該当なし                  |
| iOS                               | [2.2.2](http://aka.ms/gc6fex)                                             | 該当なし                  |
| Android                           | [2.0.3](https://go.microsoft.com/fwLink/?LinkID=280126)                   | 該当なし                  |
| HTML                              | [1.2.7](http://ajax.aspnetcdn.com/ajax/mobileservices/MobileServices.Web-1.2.7.min.js) | 該当なし     |

### モバイル *サービス* サーバー Sdk

| サーバー プラットフォーム  | バージョン                                                                                                        | 同意済みのバージョン ヘッダー |
| ---------------- | ------------------------------------------------------------                                                   | ----------------------- |
| .NET             | [WindowsAzure.MobileServices.Backend.* バージョン 1.0.x](https://www.nuget.org/packages/WindowsAzure.MobileServices.Backend/) | * * バージョン ヘッダーなし * * |
| Node.js          | (近日対応予定)                        | **バージョン ヘッダーなし** |

<!-- TODO: add Node npm version -->

### Mobile Services バックエンドの動作

| ZUMO-API-VERSION | MS_SkipVersionCheck の値 | 応答 |
| ---------------- | ---------------------------- | -------- |
| 指定なし    | 任意                          | 200 - OK |
| 任意の値        | True                         | 200 - OK |
| 任意の値        | 偽/指定なし          | 400 - 正しくない要求 | 

## <a name="2.0.0"></a>Azure のモバイル アプリのクライアントとサーバー

### <a name="MobileAppsClients"></a> モバイル *アプリ* クライアント Sdk

クライアント SDK の次のバージョン以降で導入されたバージョンのチェックの **Azure Mobile Apps**:

| クライアント プラットフォーム                   | バージョン                   | バージョンのヘッダー値 |
| -------------------               | ------------------------  | -----------------    |
| 管理されたクライアント (Windows、Xamarin) | [2.0.0](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/2.0.0) | 2.0.0 |
| iOS                               | [3.0.0](http://go.microsoft.com/fwlink/?LinkID=529823) | 2.0.0  |
| Android                           | [3.0.0](http://go.microsoft.com/fwlink/?LinkID=717033&clcid=0x409) | 3.0.0 |

<!-- TODO: add HTML version when released -->

### モバイル *アプリ* サーバー Sdk

バージョン チェックは次のサーバー SDK バージョンに含まれています。

| サーバー プラットフォーム  | SDK                                                                                                        | 同意済みのバージョン ヘッダー |
| ---------------- | ------------------------------------------------------------                                                   | ----------------------- |
| .NET             | [Microsoft.Azure.Mobile.Server.](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) | 2.0.0 |
| Node.js          | [azure-mobile-apps バージョン 1.0-beta1 (以降)](https://www.npmjs.com/package/azure-mobile-apps)                         | 2.0.0 |

### Mobile Apps バックエンドの動作

| ZUMO-API-VERSION | MS_SkipVersionCheck の値 | 応答 |
| ---------------- | ---------------------------- | -------- |
| x.y.z または Null    | True                         | 200 - OK |
| Null             | 偽/指定なし          | 400 - 正しくない要求 |
| 1.x.y            | 偽/指定なし          | 400 - 正しくない要求 |
| 2.0.0-2.x.y      | 偽/指定なし          | 200 - OK |
| 3.0.0-3.x.y      | 偽/指定なし          | 400 - 正しくない要求 |


## 次のステップ

- [Mobile Service を Azure App Service に移行する]


[Mobile Services clients]: #MobileServicesClients
[Mobile Apps clients]: #MobileAppsClients


[Mobile App Server SDK]: http://www.nuget.org/packages/microsoft.azure.mobile.server
[Migrate a Mobile Service to Azure App Service]: app-service-mobile-migrating-from-mobile-services.md


