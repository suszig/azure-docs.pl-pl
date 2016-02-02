<properties 
   pageTitle="Azure コマンド ライン インターフェイスを使用して Azure Data Lake Analytics を管理する | Azure" 
   description="Azure CLI を使用して、Azure Data Lake Analytics のアカウント、データ ソース、ジョブ、およびユーザーを管理する方法について説明します。" 
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
   ms.date="11/03/2015"
   ms.author="jgao"/>


# Azure コマンド ライン インターフェイス (CLI) を使用して Azure Data Lake Analytics を管理する

[AZURE.INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Azure を使用して、Azure Data Lake Analytics のアカウント、データ ソース、ユーザー、およびジョブを管理する方法について説明します。 他のツールを使用する管理のトピックを表示するには、上のタブ セレクターをクリックします。

**前提条件**

このチュートリアルを読み始める前に、次の項目を用意する必要があります。

- **Azure サブスクリプション**。 参照してください [取得 Azure 無料試用版](https://azure.microsoft.com/pricing/free-trial/)します。
- **Azure CLI**。 参照してください [をインストールし、Azure cli](xplat-cli.md)します。
    - ダウンロードし、インストール、 **プレリリース** [Azure CLI ツール](https://github.com/MicrosoftBigData/AzureDataLake/releases) このデモを完了するためにします。
- **認証**。次のコマンドを使用します。

        azure login

    職場または学校のアカウントを使用した認証の詳細については、次を参照してください。 [Azure CLI から Azure サブスクリプションへの接続](xplat-cli-connect.md)します。
- **Azure リソース マネージャー モードへの切り替え**。次のコマンドを使用します。

        azure config mode arm


**Data Lake Store と Data Lake Analytics のコマンド一覧を表示するには:**

    azure datalake store
    azure datalake analytics

## アカウントの管理

Data Lake Analytics ジョブを実行するには、Data Lake Analytics アカウントが必要です。 Azure HDInsight とは異なり料は発生しません Analytics アカウントされていないとき 
ジョブを実行します。 ジョブの実行時にのみ課金されます。 詳細については、次をご覧ください。 
[Azure Data Lake 分析の概要](data-lake-analytics-overview.md)します。

### アカウントの作成

    azure datalake analytics account create "<Data Lake Analytics Account Name>" "<Azure Location>" "<Resource Group Name>" "<Default Data Lake Account Name>"

### アカウントの更新

次のコマンドは、既存の Data Lake Analytics アカウントのプロパティを更新します。

    azure datalake analytics account set "<Data Lake Analytics Account Name>"

### アカウントの一覧表示

Data Lake Analytics アカウントを一覧表示します。

    azure datalake analytics account list

特定のリソース グループ内の Data Lake Analytics アカウントをリストします。

    azure datalake analytics account list -g "<Azure Resource Group Name>"

特定の Data Lake Analytics アカウントの詳細を取得します。

    azure datalake analytics account show -g "<Azure Resource Group Name>" -n "<Data Lake Analytics Account Name>"

### Data Lake Analytics アカウントの削除

    azure datalake analytics account delete "<Data Lake Analytics Account Name>"

## アカウント データ ソースの管理

Data Lake Analytics では現在、以下のデータ ソースがサポートされています。

- [Azure Data Lake ストレージ](data-lake-storage-overview.md)
- [Azure ストレージ](storage-introduction.md)

Analytics アカウントを作成する際は、既定値である Azure データ湖のストレージ アカウントを指定する必要があります。 
入力します。 既定の ADL ストレージ アカウントは、ジョブ メタデータとジョブ監査ログを保存するために使用されます。 用意した後 
Analytics アカウントを作成するには、追加のデータ湖のストレージ アカウントまたは Azure ストレージ アカウントを追加できます。

### 既定の ADL ストレージ アカウントの検索

    azure datalake analytics account show "<Data Lake Analytics Account Name>"

値は、properties:datalakeStoreAccount:name の下に表示されます。

### 他の Azure BLOB ストレージ アカウントの追加

    azure datalake analytics account datasource add -n "<Data Lake Analytics Account Name>" -b "<Azure Blob Storage Account Short Name>" -k "<Azure Storage Account Key>"

>[AZURE.NOTE] BLOB ストレージの短い名のみがサポートされます。 "myblob.blob.core.windows.net" などの FQDN 名は使用しないでください。

### 他の Data Lake Store アカウントの追加

    azure datalake analytics account datasource add -n "<Data Lake Analytics Account Name>" -l "<Data Lake Store Account Name>" [-d]

[-d] は追加される Data Lake が既定の Data Lake アカウントかどうかを示すスイッチであり、省略可能です。

### 既存のデータ ソースの更新

既存の Data Lake Store アカウントを既定として設定するには:

    azure datalake analytics account datasource set -n "<Data Lake Analytics Account Name>" -l "<Azure Data Lake Store Account Name>" -d

既存の BLOB ストレージ アカウント キーを更新するには:

    azure datalake analytics account datasource set -n "<Data Lake Analytics Account Name>" -b "<Blob Storage Account Name>" -k "<New Blob Storage Account Key>"

### データ ソースの一覧表示:

    azure datalake analytics account show "<Data Lake Analytics Account Name>"

![データ ソースを一覧表示している Data Lake Analytics](./media/data-lake-analytics-manage-use-cli/data-lake-analytics-list-data-source.png)

### データ ソースの削除:

Data Lake Store アカウントを削除するには:

    azure datalake analytics account datasource delete "<Data Lake Analytics Account Name>" "<Azure Data Lake Store Account Name>"

BLOB ストレージ アカウントを削除するには:

    azure datalake analytics account datasource delete "<Data Lake Analytics Account Name>" "<Blob Storage Account Name>"

## ジョブの管理

ジョブを作成するには、Data Lake Analytics アカウントが必要です。 詳細については、次を参照してください。 [管理 Data Lake 分析アカウント](#manage-accounts)します。

### ジョブのリスト

    azure datalake analytics job list -n "<Data Lake Analytics Account Name>"

![データ ソースを一覧表示している Data Lake Analytics](./media/data-lake-analytics-manage-use-cli/data-lake-analytics-list-jobs.png)

### ジョブの詳細の取得

    azure datalake analytics job show -n "<Data Lake Analytics Account Name>" -j "<Job ID>"

### ジョブの送信

> [AZURE.NOTE] ジョブの既定の優先度は 1000 で、ジョブの並列処理の既定の次数は 1 です。

    azure datalake analytics job create  "<Data Lake Analytics Account Name>" "<Job Name>" "<Script>"

### ジョブの取り消し

list コマンドを使用してジョブ ID を検索した後、cancel を使用してそのジョブを取り消します。

    azure datalake analytics job list -n "<Data Lake Analytics Account Name>"
    azure datalake analytics job cancel "<Data Lake Analytics Account Name>" "<Job ID>"

## カタログの管理

U-SQL カタログを使用して、U-SQL スクリプトで共有できるように、データとコードを構成します。 カタログでは、Azure Data Lake のデータを使用して可能な限り最高のパフォーマンスを実現できます。 詳細については、次を参照してください。 [U-SQL カタログ](data-lake-analytics-use-u-sql-catalog.md)します。

### カタログ項目のリスト

    #List databases
    azure datalake analytics catalog list -n "<Data Lake Analytics Account Name>" -t database
    
    #List tables
    azure datalake analytics catalog list -n "<Data Lake Analytics Account Name>" -t table

タイプには、データベース、スキーマ、アセンブリ、externaldatasource、テーブル、tablevaluedfunction、または tablestatistics が含まれます。

### カタログ シークレットの作成

    azure datalake analytics catalog secret create -n "<Data Lake Analytics Account Name>" <databaseName> <hostUri> <secretName>

### カタログ シークレットの変更

    azure datalake analytics catalog secret set -n "<Data Lake Analytics Account Name>" <databaseName> <hostUri> <secretName>

### カタログ シークレットの削除

    azure datalake analytics catalog secrete delete -n "<Data Lake Analytics Account Name>" <databaseName> <hostUri> <secretName>

## ARM グループの使用

アプリケーションが通常、web アプリ、データベース、データベース サーバー、ストレージ、たとえば、多くのコンポーネントの構成します。
およびサード パーティのサービスです。 Azure リソース マネージャー (ARM) では、アプリケーションでのリソースを操作できます。 
Azure リソース グループと呼ばれるグループです。 デプロイ、更新、監視、すべての削除、 
1 つ、組織的な操作で、アプリケーションのリソース。 展開用テンプレートを使用します。 
テンプレートは、テスト、ステージングと運用環境などのさまざまな環境を操作できます。 課金を明確にすることができます。 
グループ全体のロールアップ コストを表示することによって組織の 詳細については、[Azure を参照してください。 
リソース マネージャー Overview](resource-group-overview.md) します。

Data Lake Analtyics サービスには、次のコンポーネントを含めることができます。

- Azure Data Lake Analytics アカウント
- 必要な既定の Azure Data Lake Storage アカウント
- 追加の Azure Data Lake Storage アカウント
- 追加の Azure Storage アカウント

管理しやすくするために 1 つの ARM グループの下にこれらすべてのコンポーネントを作成することができます。

![Azure Data Lake Analytics のアカウントとストレージ](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-arm-structure.png)

Data Lake Analytics アカウントと従属するストレージ アカウントは同じ Azure データ センターに配置する必要があります。
ただし、ARM グループは別のデータ センターに配置できます。


## 関連項目

- [Microsoft Azure Data Lake 分析の概要](data-lake-analytics-overview.md)
- [Azure ポータルを使用して Data Lake 分析を使ってみる](data-lake-analytics-get-started-portal.md)
- [Azure ポータルを使用して Azure Data Lake 分析を管理します。](data-lake-analytics-use-portal.md)
- [監視し、Azure ポータルを使用して Azure Data Lake 分析ジョブのトラブルシューティング](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)






