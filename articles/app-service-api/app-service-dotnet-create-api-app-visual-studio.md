<properties 
    pageTitle="API アプリとしての Web API プロジェクトの構成" 
    description="Visual Studio 2013 を使用して、API アプリとして Web API プロジェクトを構成する方法を説明します。 " 
    services="app-service\api" 
    documentationCenter=".net" 
    authors="bradygaster" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-api" 
    ms.workload="web" 
    ms.tgt_pltfrm="dotnet" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="11/10/2015" 
    ms.author="tdykstra"/>

# API アプリとしての Web API プロジェクトの構成

[AZURE.INCLUDE [app-service-api-v2-note](../../includes/app-service-api-v2-note.md)]

## 概要

このチュートリアルでは、既存の Web API プロジェクトを利用し、として配置するように構成する方法、 [API アプリ](app-service-api-apps-why-best-platform.md) で [Azure App Service](../app-service/app-service-value-prop-what-is.md)します。 以降のチュートリアル シリーズ番組のハウツー [展開](app-service-dotnet-deploy-api-app.md) と [デバッグ](../app-service-dotnet-remotely-debug-api-app.md) このチュートリアルで作成した API アプリ プロジェクトです。

API アプリについては、次を参照してください。 [API apps とは?](app-service-api-apps-why-best-platform.md)します。

[AZURE.INCLUDE [install-sdk-2015-2013](../../includes/install-sdk-2015-2013.md)]

このチュートリアルでは、Azure SDK for .NET 2.6 以降のバージョンが必要です。

## Web API プロジェクトの構成 

このセクションでは、API アプリとして既存の Web API プロジェクトを構成する方法を示します。 まず Web API プロジェクトのテンプレートを使用して Web API プロジェクトを作成してから、このプロジェクトを API アプリとして構成します。

1. Visual Studio 2015 または Visual Studio 2013 を開きます。

2. クリックして **ファイル > [新しいプロジェクト**します。 

3. 選択、 **ASP.NET Web アプリケーション** テンプレートです。

4. 確認して、 **Application Insights をプロジェクトに追加** ] チェック ボックスがオフになっています。 

4. プロジェクトに名前を *ContactsList*

    ![](./media/app-service-dotnet-create-api-app-visual-studio/01-filenew-v3.png)

5. Click **OK**.

6.  **新しい ASP.NET プロジェクト** ダイアログ ボックスで、選択、 **空** プロジェクト テンプレートです。

7. クリックして、 **Web API** チェック ボックスをオンします。

8. クリア、 **クラウドでホスト** オプション。

    ![](./media/app-service-dotnet-create-api-app-visual-studio/webapinewproj.png)

9. クリックして **OK** プロジェクトを生成します。

    ![](./media/app-service-dotnet-create-api-app-visual-studio/sewebapi.png)

10.  **ソリューション エクスプ ローラー**, 、(ソリューションではありません)、プロジェクトを右クリックし、[ **追加 > Azure API App SDK**します。

    ![](./media/app-service-dotnet-create-api-app-visual-studio/addapiappsdk.png)

11.  **API アプリ メタデータ ソースの選択** ダイアログ ボックスで、をクリックして **メタデータの自動生成**します。 

    ![](./media/app-service-dotnet-create-api-app-visual-studio/chooseswagger.png)

    この選択により、Swagger の動的 UI が有効になります。これについてはチュートリアルの後半で説明します。 Swagger メタデータ ファイルのアップロードを選択する場合は、ファイル名と共に保存 *apiDefinition.swagger.json*, 次のセクションで説明したように、します。 

12. Click **OK**. 
 
    この時点で、Visual Studio によって API アプリの NuGet パッケージがインストールされ、API アプリのメタデータが Web API プロジェクトに追加されます。  

[AZURE.INCLUDE [app-service-api-review-metadata](../../includes/app-service-api-review-metadata.md)]

[AZURE.INCLUDE [app-service-api-define-api-app](../../includes/app-service-api-define-api-app.md)]

[AZURE.INCLUDE [app-service-api-direct-deploy-metadata](../../includes/app-service-api-direct-deploy-metadata.md)]

## 次のステップ

API アプリを展開する準備ができました、行うことができる、 [API アプリのデプロイ](app-service-dotnet-deploy-api-app.md) を行うにはチュートリアルです。
 

