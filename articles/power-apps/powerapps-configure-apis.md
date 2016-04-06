<properties
    pageTitle="Azure ポータルでの PowerApps API プロパティの変更または更新 | Microsoft Azure"
    description="カスタム アイコンの追加、XML ポリシーの更新、または PowerApps API の Swagger 定義の更新"
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
   ms.date="11/25/2015"
   ms.author="guayan"/>

# 既存 API とプロパティの更新

App Service 環境で登録した API とは、本質的にはバックエンド サービスのプロキシです。 API を作成したあとで、そのプロパティを変更する場合があります。 たとえば、次のような変更があります。 

- API にカスタム アイコンを追加する。
- API によって使用されるバックエンドを保護する方法を変更する。 
- API の表示名をよりユーザー フレンドリーな名前に更新する。


#### 開始のための前提条件

- サインアップ [PowerApps エンタープライズ](powerapps-get-started-azure-portal.md)します。
- 作成、 [app service 環境](powerapps-get-started-azure-portal.md)します。
- 登録、 [API](powerapps-register-from-available-apis) 環境内でします。

## カスタム アイコンの追加またはユーザー フレンドリー表示名の追加

1.  [Azure ポータル](https://portal.azure.com), 、API のブレードを開きます。
2. 選択 **設定をすべて**です。
3.  **設定**, [ **全般**:  
![][11]

[全般] では次の設定を変更できます。

設定 | 説明
--- | ---
表示名 | 一覧表示するときにこの名前は使用、 *利用可能な接続* PowerApps にします。 既定では、API のリソース名を使用するので、PowerApps ユーザーにはわかりやすい名前になっていない可能性があります。 ユーザー フレンドリーな表示名を入力できます。 たとえば、名前を付けます。 **新しい顧客の注文** または **営業履歴を表示します**します。  
アイコン URL | 自身の API にカスタム アイコンを追加できます。 一覧表示するときに、アイコンが使用されます、 *利用可能な接続* と *接続* PowerApps にします。 既定では、次のアイコンが使用されます。 <br/><br/>![][12] <br/><br/>カスタム アイコンを使用する場合<br/><ul><li>アイコンの URL をパブリックにアクセス可能にする必要があります。</li><li>.Png または .svg ファイルを指定できます。 Png ファイルを使用する 40 x 40 ピクセルをお勧めします。</li></ul>
URL スキーム | API がサポートする 1 つまたは複数のスキームを選択します。 選択できる **HTTP**, 、**HTTPS**, 、または **HTTP および HTTPS**します。 既定では、HTTP および HTTPS が可能です。 <br/><br/>App service 環境では、バックエンド構成に基づく方式が自動的に構成されます。 そのため、追加で何らかの構成を行う必要があれば、バックエンド サービスを開発または変更できます。 
バックエンド サービスによる認証 | App Service 環境でバックエンド サービスを登録後、クライアントが API を使用してのみバックエンド サービスを呼び出すよう、バックエンドをセキュリティ保護することをお勧めします。 次のオプションは、バックエンドが配置に基づきがあります。<br/><br/><ul><li><strong>この API を使用してのみアクセスできるように</strong>: このオプションは、バックエンドは、app service 環境に配置した場合にのみ使用できます。 これを選択すると、バックエンド上のホスト名はいずれも無効になります。 API のプロキシがまた同じ app service 環境で実行されているために、バックエンドにアクセスできます。</li><li><strong>HTTP 基本認証</strong>: このオプションは、バックエンドを展開する場所に関係なく使用できます。 これが選択されている場合は、ユーザー名とパスワードを入力します。 プロキシがバックエンドを呼び出すと、プロキシは HTTP 基本認証を使用して、HTTP 承認ヘッダーのユーザー名とパスワードを渡します。 バックエンド サービスが最後に、確認する必要があります (認証)、ユーザー名とパスワードを入力します。<br/><br/>ASP.NET Web API 2 で HTTP 基本認証を実装する方法の詳細については、次を参照してください。 [ASP.NET Web API 2 での認証フィルター](http://www.asp.net/web-api/overview/security/authentication-filters)します。</li></ul>


## API の Swagger の更新

1.  [Azure ポータル](https://portal.azure.com), 、API のブレードを開きます。
2. 選択 **設定をすべて**です。
3.  **設定**, [ **API の定義**:  
![][13]

**Swagger 2.0** はサポートされている API 定義の形式です。 現在の API の定義は、埋め込み JSON エディターで書かれています。 インラインで編集したり、新しい JSON ファイルをアップロードしたりできます。 したら **保存** 変更内容は、このブレードで、API の定義に発生したエラーを含むすべてのエラーは表示されます。

- Swagger 2.0 の詳細については、次を参照してください。、 [公式の Swagger web サイト](http://swagger.io)します。
- API を開発する際に Swagger 2.0 を取得する方法の詳細については、次のページを参照してください。  
    - [Azure App Service での ASP.NET API アプリの作成](../app-service-dotnet-create-api-app.md)
    - [Azure App Service での Java API アプリの構築とデプロイ](../app-service-api-java-api-app.md)
    - [Azure App Service での Node.js API アプリの構築とデプロイ](../app-service-api-nodejs-api-app.md)
    - [Swashbuckle が生成する API 定義をカスタマイズする](../app-service-api-dotnet-swashbuckle-customize.md)
- PowerApps の Swagger 2.0 を使用するベスト プラクティスの詳細については、次を参照してください。 [PowerApps 用 API の開発](powerapps-develop-api.md)します。

## API の XML ポリシーの更新

1.  [Azure ポータル](https://portal.azure.com), 、API のブレードを開きます。
2. 選択 **設定をすべて**です。
3.  **設定**, [ **ポリシー**:  
![][14]

このポリシーでサポートされている同じポリシー [Azure API Management](https://azure.microsoft.com/services/api-management)します。 現在のポリシーは、組み込みの XML エディターで書かれています。 インラインでの編集、または新しい XML ファイルのアップロードが可能です。 したら **保存** 変更内容のエラーは API のポリシーに関する問題のトラブルを含め、このブレードで表示します。

[Azure API Management のポリシー](../api-management-howto-policies.md) を構成して、ポリシーについての詳細について適切なリソースであります。


## まとめと次のステップ
API を作成後、このトピックの手順を使用して設定変更をしたり、設定の一部をカスタマイズしたりすることも可能です。 

PowerApps の詳細については、次の関連するトピックやリソースも参照してください。

- [API を構成して AAD 保護バックエンドに接続する](powerapps-configure-apis-aad.md)
- [PowerApps 用 API の開発](powerapps-develop-api.md)

[11]: ./media/powerapps-configure-apis/api-settings-general.png
[12]: ./media/powerapps-configure-apis/api-default-icon.png
[13]: ./media/powerapps-configure-apis/api-settings-api-definition.png
[14]: ./media/powerapps-configure-apis/api-settings-policy.png


