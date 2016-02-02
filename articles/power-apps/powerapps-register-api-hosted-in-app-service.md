<properties
    pageTitle="App Service 環境でホストされる API を PowerApps Enterprise で開発または作成する | Microsoft Azure"
    description="Azure ポータルを使用して App Service 環境でホストされるカスタム API を登録する方法をについて説明します。"
    services=""
    suite="powerapps"
    documentationCenter="" 
    authors="MandiOhlinger"
    manager="dwrede"
    editor=""/>

<tags
   ms.service="powerapps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="12/09/2015"
   ms.author="guayan"/>


# App Service 環境内でホストされている API の登録

PowerApps は、クラウド内またはオンプレミス内でホストされている既存の API の登録をサポートしています。非常に強力です。 シナリオによっては、新しい API を開発または作成することがあります。 次のような場合です。

- 組織で使用する新しい機能を実装する。
- アプリを作成するユーザーのために、既存の機能やデータを基にして、より優れたエクスペリエンスを提供する。

すべての既存機能を活用する app service 環境内で独自の Api をホストしている場合、 [app service 環境](../app-service-app-service-environment-intro.md), を行い、統合エクスペリエンスを向上します。

これらの API をアプリで使用するには、Azure ポータルで API を「登録」する必要があります。 以下のオプションを使用できます。

- 構築済みの登録 [マネージ API を Microsoft IT のマネージ API または](powerapps-register-from-available-apis.md)します。
- App Service Environment (このトピックの) 内にホストされた Web アプリ、API アプリ、およびモバイル アプリを登録します。
- 使用する独自 Api の Swagger のいずれかの登録、 [Swagger 2.0 API 定義](powerapps-register-existing-api-from-api-definition.md)します。

この記事では、**アプリ サービス環境内でホストされている Web アプリ、API アプリ、モバイル アプリを登録する**方法について説明します。

#### 開始のための前提条件

- サインアップ [PowerApps エンタープライズ](powerapps-get-started-azure-portal.md)します。
- 作成、 [app service 環境](powerapps-get-started-azure-portal.md)します。


## App Service 環境での API の開発とデプロイ

App service 環境では API を簡単に開発できます。 Web API を作成して使用し、好みのプログラミング言語を選択する [Swagger 2.0](http://swagger.io) に API の定義について説明します。 次に例をいくつか示します。

- [構築し、Azure App Service で .NET のデプロイ](../app-service-api-dotnet-get-started.md)
- [構築し、Azure App Service での Java API アプリのデプロイ](../app-service-api-java-api-app.md)
- [構築し、Azure App Service での Node.js API アプリのデプロイ](../app-service-api-nodejs-api-app.md)

App Service 環境への Web API のデプロイでは、Visual Studio からのデプロイや、ソース管理システムを使用した継続的なデプロイなどのオプションもあります。 [Azure App Service で web アプリのデプロイ](../web-sites-deploy.md) は適切なリソースです。

## App Service 環境でのカスタム API の登録

App Service 環境に API をデプロイした後は、以下の手順を使用して登録します。

1. [Azure ポータル](https://portal.azure.com/), [ **PowerApps**, 、し、[ **管理 Api**:  
![][11]
2. Api の管理で、選択 **追加**:  
![][12]
3. **[API の追加]** で、API のプロパティを入力します。

    - **[名前]** に、API の名前を入力します。 入力した名前が、API のランタイム URL に含まれていることを確認してください。 名前は意味があり、組織内で一意のものにしてください。
    - **ソース**, [ **App Service 環境でホストされている Api からのインポート**:  
    ![][13]
4. **[App Service 環境でホストされている API]** で、インポートするAPI を選択します。 この一覧が app service 環境内のすべての web アプリ、API アプリとモバイル アプリを示しています。 その **apiDefinition.url** プロパティが構成されます。 API をインポートするには、このプロパティを使用して公開される Swagger 2.0 API 定義を使用します。 この URL は、API を登録するときに、パブリックにアクセスできることを確認します。  
![][14]
5. **[追加]** を選択して、手順を完了します。

> [AZURE.TIP] API を登録すると、その API が App Service Environment に登録されます。 API が App Service Environment に登録されると、同じ App Service Environment 内の他のアプリでも使用することができます。

## まとめと次のステップ

このトピックでは、App Service 環境内でホストされている API を登録する方法について説明しました。 PowerApps についての関連トピックとリソースがあります。

- [API を構成します](powerapps-configure-apis.md)
- [ユーザーが Api にアクセスできるように](powerapps-manage-api-connection-user-access.md)
- [PowerApps で、アプリの作成を開始します。](https://powerapps.microsoft.com/tutorials/)



[11]: ./media/powerapps-register-api-hosted-in-app-service/registered-apis-part.png 
[12]: ./media/powerapps-register-api-hosted-in-app-service/add-api-button.png 
[13]: ./media/powerapps-register-api-hosted-in-app-service/add-api-blade.png 
[14]: ./media/powerapps-register-api-hosted-in-app-service/add-api-select-from-ase.png 

