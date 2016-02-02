## API アプリのメタデータ

このセクションでは、カスタマイズ可能な API アプリのメタデータに関する追加情報を提供します。

*apiapp.json* ファイル内のほとんどのプロパティと、*Metadata* フォルダー内のファイルは、Azure Marketplace で API アプリ パッケージがどのように表示されるかに影響します。 以降のセクションでは、Azure サブスクリプションの API アプリにコードをデプロイしたときに、どのプロパティとファイルが API アプリに影響するかについて説明します。

### API アプリ ID

`Id` プロパティ API アプリの名前を指定します。 次に例を示します。

        "id": "ContactsList",

![](./media/app-service-api-direct-deploy-metadata/apiappname.png)

### 名前空間

設定、 `名前空間` プロパティには、Azure Active Directory テナントのドメイン。 ドメインを確認するには、ブラウザーを開きます、 [Azure クラシック ポータル](https://manage.windowsazure.com/), 、参照 **Active Directory**, を選択し、 **ドメイン** ] タブをクリックします。 次に例を示します。

        "namespace": "contoso.onmicrosoft.com",

### 動的 Swagger API の定義

API アプリが動的なを返せる場合 [Swagger](http://swagger.io/) API の定義を API 定義 JSON を返す GET 要求の相対 URL を格納で、 `endpoints.apiDefinition` プロパティです。 次に例を示します。

        "endpoints": {
            "apiDefinition": "/swagger/docs/v1"
        }

> **注:** Swashbuckle を使用して Swagger API の定義を生成している場合は、Web API コントローラーでの HTTP メソッドのオーバーロードによって、操作 ID の重複が発生します。 詳細については、次を参照してください。 [をカスタマイズする Swashbuckle で生成された操作 id](../article/app-service-api/app-service-api-dotnet-swashbuckle-customize.md)します。

### 静的 Swagger API の定義

静的なを提供する [Swagger](http://swagger.io/) 2.0 API の定義ファイルで、ファイルを保存、 *メタデータ* フォルダーとファイル名 *apiDefinition.swagger.json*

![](./media/app-service-api-direct-deploy-metadata/apidefinmetadata.png)

ままにして `endpoints.apiDefinition` のうち、 *apiapp.json* ファイルまたはその値を null に設定します。 両方を含めた場合、 `endpoints.apiDefinition` URL と *apiDefinition.swagger.json* ファイル、URL が優先され、ファイルは無視されます。





