## API アプリのメタデータの確認

メタデータに API アプリが含まれているとして展開される Web API プロジェクトが、 *apiapp.json* ファイルと *メタデータ* フォルダーです。

![](./media/app-service-api-review-metadata/metadatainse.png)

既定の内容、 *apiapp.json* ファイルには、次の例がようになります。

        {
            "$schema": "http://json-schema.org/schemas/2014-11-01/apiapp.json#",
            "id": "ContactsList",
            "namespace": "microsoft.com",
            "gateway": "2015-01-14",
            "version": "1.0.0",
            "title": "ContactsList",
            "summary": "",
            "author": "",
            "endpoints": {
                "apiDefinition": "/swagger/docs/v1",
                "status": null
            }
        }

通知、 `apiDefinition` エンドポイント `/swagger/docs/v1`: 既定では、API アプリ プロジェクトで使用する、 [Swashbuckle](https://www.nuget.org/packages/Swashbuckle) 自動を提供する NuGet パッケージ [Swagger](http://swagger.io/) メタデータの生成。 

このチュートリアルでは、既定値をそのまま使用できます。  [API アプリのメタデータ](#api-app-metadata) チュートリアルの後でこのメタデータをカスタマイズする方法について説明します。

