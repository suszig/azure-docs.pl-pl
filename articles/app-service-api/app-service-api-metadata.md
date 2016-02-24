<properties
    pageTitle="API の検出とコードの生成用の App Service API Apps メタデータ | Microsoft Azure"
    description="Azure App Service の API Apps で Swagger メタデータを使用して、API の検出とコードの生成を簡単にする方法について説明します。"
    services="app-service\api"
    documentationCenter=".net"
    authors="tdykstra"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-api"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="11/25/2015"
    ms.author="tdykstra"/>

# API の検出とコードの生成用の App Service API Apps メタデータ 

サポート [Swagger 2.0](http://swagger.io/) API のメタデータが App Service API Apps に組み込まれています。 Swagger を使用する必要はありませんが、使用すると、検出と使用をより簡単にする API Apps の機能を活用できます。   

## Swagger のエンドポイント

API アプリのプロパティでは、その API アプリの Swagger 2.0 JSON メタデータを提供するエンドポイントを指定できます。 このエンドポイントは、API アプリのベース URL、または絶対 URL を基準に指定できます。 絶対 URL は、API アプリの外部を示します。 

多くのダウンストリーム クライアント (Visual Studio のコード生成、PowerApps の "API の追加" フローなど) では、URL にパブリックにアクセスできる (ユーザーやサービスの認証で保護されていない) ことが必要です。 つまり、App Service 認証を使用していて、なおかつ API 定義をアプリ内から公開する場合、API に匿名でアクセスするトラフィックを許可する認証オプションを使用する必要があります。 詳細については、次を参照してください。 [App Service API Apps の認証と承認](app-service-api-authentication.md)します。

### ポータルのブレード

 [Azure ポータル](https://portal.azure.com/) URL を表示し、変更のエンドポイント、 **API の定義** ブレードです。

![](./media/app-service-api-metadata/apidefblade.png)

### Azure リソース マネージャーのプロパティ

Azure リソース マネージャーの Azure PowerShell、CLI などのツールを使用して、API アプリの API 定義の URL を構成することも、または [リソース エクスプ ローラー](https://resources.azure.com/)します。 

`apiDefinition` プロパティは、<site name>/web リソースの Microsoft.Web/sites/config リソース タイプで設定します。 たとえば、 **リソース エクスプ ローラー**, には、 **サブスクリプション > {サブスクリプション} > resourceGroups > {リソース グループ} > プロバイダー > Microsoft.Web > サイト > {ƒtƒcƒg} > 構成 > web**, と cors のプロパティが表示されます。

        "apiDefinition": {
          "url": "https://contactslistapi.azurewebsites.net/swagger/docs/v1"
        }

### 既定値

Visual Studio を使用して API アプリを作成すると、API 定義のエンドポイントは、自動的に、その API アプリのベース URL に `/swagger/docs/v1` が追加されたものに設定されます。 これは、既定の URL を [Swashbuckle](https://www.nuget.org/packages/Swashbuckle) NuGet パッケージでは、動的に生成された機能を ASP.NET Web API プロジェクトのメタデータの Swagger です。 

## コードの生成

Azure API アプリに Swagger を統合する利点の 1 つは、自動コード生成です。 生成されたクライアント クラスにより、API アプリを呼び出すコードの記述が容易になります。

API アプリのクライアント コードは、Visual Studio を使用して生成するか、コマンド ラインから生成することができます。 ASP.NET Web API プロジェクトを Visual Studio でのクライアント クラスを生成する方法については、次を参照してください。 [API Apps と ASP.NET を使ってみる](app-service-api-dotnet-get-started.md#codegen)します。 サポートされているすべての言語のコマンドラインから実行する方法については、の readme ファイルを参照してください、 [Azure/autorest](https://github.com/azure/autorest) GitHub.com のリポジトリにします。
 
## 次のステップ

手順に従って作成するステップ バイ ステップ チュートリアル、配置、および API アプリの使用を参照してください [Azure App Service で API アプリを使ってみる](app-service-api-dotnet-get-started.md)します。

