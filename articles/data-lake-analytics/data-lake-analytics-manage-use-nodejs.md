<properties 
   pageTitle="Azure SDK for Node.js を使用して Azure Data Lake Analytics を管理する | Azure" 
   description="Azure SDK for Node.js を使用して Data Lake Analytics アカウント、データ ソース、ジョブ、およびユーザーを管理する方法について説明します" 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="mumian" 
   manager="paulettm" 
   editor="cgronlun"/>

<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="12/11/2015"
   ms.author="jgao"/>


# Azure SDK for Node.js を使用して Azure Data Lake Analytics を管理する

[AZURE.INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Azure ADK for Node.js を使用して、Azure Data Lake Analytics アカウント、ジョブ、およびカタログを管理できます。 他のツールを使用する管理のトピックを表示するには、上のタブ セレクターをクリックします。

**前提条件**

このチュートリアルを読み始める前に、次の項目を用意する必要があります。

- **Azure サブスクリプション**。 参照してください [取得 Azure 無料試用版](https://azure.microsoft.com/pricing/free-trial/)します。
- **Azure Data Lake Analytics アカウント**。 参照してください [Azure ポータルを使用して Azure Data Lake 分析を使ってみる](data-lake-analytics-get-started-portal.md) アカウントを作成します。
- **Data Lake Analytics アカウントへのアクセス許可を持つサービス プリンシパル**。 参照してください [Azure リソース マネージャーでサービス プリンシパルの認証](resource-group-authenticate-service-principal.md)します。

## SDK のインストール

次のコマンドを使用して SDK をインストールします。

1. Install [Node.js](https://nodejs.org/).
2. コマンド プロンプト、terminal または bash ウィンドウで次のコマンドを実行します。

        npm install async
        npm install adal-node
        npm install azure-common
        npm install azure-arm-datalake-analytics


## Node.js のサンプル

次の例では、指定した Azure Data Lake Analytics アカウントのジョブの一覧を取得します。

    var async = require('async');
    var adalNode = require('adal-node');
    var azureCommon = require('azure-common');
    var azureDataLakeAnalytics = require('azure-arm-datalake-analytics');
    
    var resourceUri = 'https://management.core.windows.net/';
    var loginUri = 'https://login.windows.net/'
    
    var clientId = 'application_id_(guid)';
    var clientSecret = 'application_password';
    
    var tenantId = 'aad_tenant_id';
    var subscriptionId = 'azure_subscription_id';
    var resourceGroup = 'adla_resourcegroup_name';
    
    var accountName = 'adla_account_name';
    
    var context = new adalNode.AuthenticationContext(loginUri+tenantId);
    
    var client;
    var response;
    
    async.series([
        function (next) {
            context.acquireTokenWithClientCredentials(resourceUri, clientId, clientSecret, function(err, result){
                if (err) throw err;
                response = result;
                next();
            });
        },
        function (next) {
            var credentials = new azureCommon.TokenCloudCredentials({
                subscriptionId : subscriptionId,
                authorizationScheme : response.tokenType,
                token : response.accessToken
            });
    
            client = azureDataLakeAnalytics.createDataLakeAnalyticsJobManagementClient(credentials, 'azuredatalakeanalytics.net');
    
            next();
        },
        function (next) {
            client.jobs.list(resourceGroup, accountName, function(err, result){
                if (err) throw err;
                console.log(result);
            });
        }
    ]);

## 関連項目

- [Azure SDK for Node.js](http://azure.github.io/azure-sdk-for-node/)
- [Microsoft Azure Data Lake 分析の概要](data-lake-analytics-overview.md)
- [Azure ポータルを使用して Data Lake 分析を使ってみる](data-lake-analytics-get-started-portal.md)
- [Azure ポータルを使用して Azure Data Lake 分析を管理します。](data-lake-analytics-use-portal.md)
- [監視し、Azure ポータルを使用して Azure Data Lake 分析ジョブのトラブルシューティング](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)






