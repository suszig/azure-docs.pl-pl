<properties
    pageTitle="SaaS コネクタと API Apps での OAUTH セキュリティ | Azure"
    description="Azure App Service のコネクタと API Apps における OAUTH セキュリティのほか、マイクロサービス アーキテクチャ、SaaS について説明します。"
    services="app-service\logic"
    documentationCenter=""
    authors="MandiOhlinger"
    manager="dwrede"
    editor="cgronlun"/>

<tags
    ms.service="app-service-logic"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/15/2015"
    ms.author="mandia"/> 


# SaaS コネクタでの OAUTH セキュリティについて
Facebook、Twitter、DropBox など、サービスとしてのソフトウェア (SaaS) の多くは、ユーザーが OAUTH プロトコルを使用して認証する必要があります。  Logic Apps からこれらの SaaS コネクタを使用する場合、Logic Apps デザイナーで [承認] をクリックするだけという、シンプルなユーザー エクスペリエンスが用意されています。 する **Authorize**, で (いない場合) にサインインし、同意を提供する必要がありますあなたに代わって SaaS サービスに接続します。 同意して承認すると、Logic Apps からこれらの SaaS サービスにアクセスできます。

## 独自の SaaS アプリの作成
これらの SaaS サービスで既にアプリケーションを作成して登録しているために、このようなシンプルなエクスペリエンスが可能になります。  場合によっては、独自のアプリケーションを登録し、使用することがあります。  これは、必要に応じて、たとえば、たとえば、カスタム アプリケーションでこれらの SaaS コネクタを使用する場合 [SaaS コネクタ API アプリのデプロイ](app-service-api-connnect-your-app-to-saas-connector.md)します。 この例では DropBox コネクタを使用しますが、そのプロセスは、OAUTH に依存するすべてのコネクタで同じです。

Logic Apps の場合でも、既定のアプリケーションを使用する代わりに、独自のアプリケーションを使用できます。 [承認] ボタンを使用して接続に失敗した場合、独自のアプリケーションを作成してみてください。 Twitter コネクタの場合の手順を次に示します。

1. Azure プレビュー ポータルで、Twitter コネクタを開きます。 移動して **参照** > **API Apps**します。 Twitter コネクタを選択します。  
    ![][1]

2. 選択 **設定** > **認証**:  
    ![][2]

3. コピー、 **リダイレクト URI** 値。  
    ![][3]

4. 移動して [Twitter](http://apps.twitter.com) と **新しいアプリを作成する**です。  **コールバック URL** プロパティには、貼り付け、 **リダイレクト URI** Twitter コネクタから値をコピーします。
    ![][4]  
5. 自分の Twitter アプリケーションの作成時に選択 **キー and Access Tokens**します。 これらの値をコピーします。
6. これらの値を貼り付け、Twitter コネクタの認証設定で、 **クライアント ID** と **クライアント シークレット** プロパティ。   
    ![][5]  
7. コネクタの設定を保存します。  

これで、Logic Apps からコネクタを使用できるようになります。 Logic Apps からこのコネクタを使用する場合、既定のアプリケーションではなく、独自のアプリケーションを使用します。  

> [AZURE.NOTE] アプリケーションを既に許可する場合は、アプリケーションを再度承認する必要があります。


<!--Image references-->
[1]: ./media/app-service-logic-oauth-security/TwitterConnector.png
[2]: ./media/app-service-logic-oauth-security/Authentication.png
[3]: ./media/app-service-logic-oauth-security/RedirectURI.png
[4]: ./media/app-service-logic-oauth-security/TwitterApp.png
[5]: ./media/app-service-logic-oauth-security/TwitterKeys.png

