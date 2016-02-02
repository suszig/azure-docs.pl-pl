<properties
    pageTitle="PowerApps Enterprise での API からの Swagger 2.0 API 定義の作成 | Microsoft Azure"
    description="既存の API から作成した Swagger 2.0 API 定義から API を登録する方法を説明します"
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


# Swagger 2.0 API 定義からの API の登録

多くの組織には、ユーザーが自分のアプリ内で使用できる API が既にあります。 これらの API をアプリで使用するには、Azure ポータルで API を「登録」する必要があります。 以下のオプションを使用できます。

- 構築済みの登録 [マネージ API を Microsoft IT のマネージ API または](powerapps-register-from-available-apis.md)します。
- Web アプリ、API アプリおよび内でホストされるモバイル アプリを登録 [App Service 環境内](powerapps-register-api-hosted-in-app-service.md)します。
- Swagger 2.0 API 定義 (このトピック内の) を使用して、独自のいずれかの Swagger API を登録します。

この記事では、既存の API から作成した **Swagger 2.0 API 定義を使用して独自のいずれかの API を登録する**方法を説明します。

#### 開始のための前提条件

- サインアップ [PowerApps エンタープライズ](powerapps-get-started-azure-portal.md)
- 作成、 [app service 環境](powerapps-get-started-azure-portal.md)

## Swagger 2.0 API 定義を使用した既存 API の登録

既存の API をとても簡単に登録できます。 手順は次のとおりです。

1. 作成、 [Swagger 2.0](http://swagger.io) の既存の API を API の定義。 PowerApps は、API 定義の形式として Swagger 2.0 を使用します。 参照されているツールを使用して、 [Swagger 2.0 の web サイト](http://swagger.io) Swagger 2.0 API 定義を簡単に作成するためです。 次の点に注意してください。

    - `ホスト` プロパティは、既存の API の実際のエンドポイントを指す必要があります。 スキームやサブ パスは使用しないでください。 たとえば、入力 `api.contoso.com`します。 <br/><br/>
    - `BasePath` が存在する場合、プロパティが、既存の API エンドポイントのサブ パスを一覧表示する必要があります。 フォワード スラッシュで始まる `/`します。 たとえば、入力 `/purchaseorderapi`します。

2. 既存の API がしっかりと app service 環境からアクセスできることを確認します。  <br/><br/>
    a) 場合は、API をインターネットを使用してアクセスできるようにするのに慣れている場合は、app service 環境と既存の API の HTTP 基本アクセス認証を設定できます。 Go [here](powerapps-configure-apis.md) to see how. <br/><br/>
    b) お客様の組織のネットワーク内で、API を保持する場合、組織のネットワークを安全にアクセスするように、app service 環境の仮想ネットワークを設定できます。 Go [here](../app-service-app-service-environment-intro.md) to see how.

3. [Azure ポータル](https://portal.azure.com/), [ **PowerApps**, 、し、[ **管理 Api**:  
![][11]
4. Api の管理で、選択 **追加**:  
![][12]
5. **[API の追加]** で、API のプロパティを入力します。

    - **[名前]** に、API の名前を入力します。 入力した名前が、API のランタイム URL に含まれていることを確認してください。 名前は意味があり、組織内で一意のものにしてください。
    - **[ソース]** で **[Swagger 2.0 からインポート]** を選択します。

6. **API の定義 (Swagger 2.0)**, 、Swagger 2.0 API 定義ファイルをアップロードします。  
 ![][13]
7. **[追加]** を選択して、手順を完了します。

> [AZURE.TIP] API を登録するときに、app service 環境内に API を登録しています。 API が App Service Environment に登録されると、同じ App Service Environment 内の他のアプリでも使用することができます。

## まとめと次のステップ

このトピックでは、Swagger 2.0 API 定義から API を登録する方法を説明しました。 PowerApps についての関連トピックとリソースがあります。

- [API を構成します](powerapps-configure-apis.md)
- [ユーザーが Api にアクセスできるように](powerapps-manage-api-connection-user-access.md)
- [PowerApps で、アプリの作成を開始します。](https://powerapps.microsoft.com/tutorials/)



[11]: ./media/powerapps-register-existing-api-from-api-definition/registered-apis-part.png 
[12]: ./media/powerapps-register-existing-api-from-api-definition/add-api-button.png 
[13]: ./media/powerapps-register-existing-api-from-api-definition/add-api-blade.png 

