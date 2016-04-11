<properties
   pageTitle="Azure Batch PowerShell の使用 | Microsoft Azure"
   description="Azure Batch サービスの管理に使用できる Azure PowerShell のコマンドレットについて簡単に説明します。"
   services="batch"
   documentationCenter=""
   authors="dlepow"
   manager="timlt"
   editor=""/>

<tags
   ms.service="batch"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="powershell"
   ms.workload="big-compute"
   ms.date="10/13/2015"
   ms.author="danlep"/>

# Azure Batch PowerShell コマンドレットの使用
この記事では、Batch アカウントの管理と Batch ジョブ、タスク、他の詳細に関する情報の取得に使用できる Azure PowerShell コマンドレットについて簡単に紹介します。

詳細なコマンドレットの構文については次のように入力します。 `get-help <Cmdlet_name>` が見たり、 [Azure Batch コマンドレット リファレンス](https://msdn.microsoft.com/library/azure/mt125957.aspx)します。

[AZURE.INCLUDE [powershell-preview-include](../../includes/powershell-preview-include.md)]

## 前提条件

* **Azure PowerShell** -Batch コマンドレットは、Azure リソース マネージャー モジュールに出荷します。 参照してください [Azure リソース マネージャー コマンドレット](https://msdn.microsoft.com/library/azure/mt125356.aspx) 前提条件、インストール手順、および基本的な使用法をします。



* **バッチ プロバイダーの名前空間 (1 回限りの操作) に登録** -、Batch アカウントを使用する前に、バッチ プロバイダーの名前空間を登録する必要があります。 この操作は、サブスクリプションごとに 1 回のみ実行する必要があります。

    ```
    Register-AzureRMResourceProvider -ProviderNamespace Microsoft.Batch
    ```

## Batch アカウントとキーを管理する


### Batch アカウントを作成する

**新しい AzureRmBatchAccount** 指定されたリソース グループに新しい Batch アカウントを作成します。 リソース グループがない場合は作成を実行して、1 つ、 [新規 AzureRmResourceGroup](https://msdn.microsoft.com/library/azure/mt603739.aspx) コマンドレット、Azure のリージョンのいずれかを示す、 **場所** "Central US"などのパラメーターです。 次に例を示します。

```
New-AzureRmResourceGroup –Name MyBatchResourceGroup –location "Central US"
```

次に、また、アカウント名を指定する、リソース グループに新しい Batch アカウントのアカウントを作成 <*account_name*> と Batch サービスが利用可能な場所です。 アカウントの作成は完了までに数分かかる場合があります。 次に例を示します。

```
New-AzureRmBatchAccount –AccountName <account_name> –Location "Central US" –ResourceGroupName MyBatchResourceGroup
```

> [AZURE.NOTE] Batch アカウント名では、Azure に一意で、3 ~ 24 文字で、および小文字と数字のみを使用する必要があります。

### アカウントのアクセス キーを取得する
**Get AzureRmBatchAccountKeys** は、Azure Batch アカウントに関連付けられているアクセス キーを示します。 たとえば、作成したアカウントのプライマリ キーとセカンダリ キーを取得するには、次のように実行します。

```
$Account = Get-AzureBatchAccountKeys –AccountName <accountname>

$Account.PrimaryAccountKey

$Account.SecondaryAccountKey
```

### 新しいアクセス キーを生成する
**新しい AzureRmBatchAccountKey** Azure Batch アカウントの新しいプライマリまたはセカンダリ アカウント キーを生成します。 たとえば、Batch アカウントの新しいプライマリ キーを生成するには、次のように入力します。

```
New-AzureRmBatchAccountKey -AccountName <account_name> -KeyType Primary
```

> [AZURE.NOTE] 新しいセカンダリ キーを生成するには、"Secondary"を指定、 **KeyType** パラメーター。 プライマリ キーとセカンダリ キーは個別に再生成する必要があります。

### Batch アカウントを削除する
**削除 AzureRmBatchAccount** Batch アカウントを削除します。 次に例を示します。

```
Remove-AzureRmBatchAccount -AccountName <account_name>
```

メッセージが表示されたら、削除するアカウントを確認します。 アカウントの削除は完了するまでに時間がかかる場合があります。

## ジョブ、タスク、およびその他の詳細のクエリ

コマンドレットを使用して **Get-azurebatchjob**, 、**Get-azurebatchtask**, 、および **Get-azurebatchpool** 、Batch アカウントで作成したエンティティを照会します。

これらのコマンドレットを使用するには、まず AzureBatchContext オブジェクトを作成してアカウント名とキーを格納する必要があります。

```
$context = Get-AzureRmBatchAccountKeys -AccountName <account_name>
```

このコンテキストを使用して、Batch サービスと対話するコマンドレットに渡す、 **BatchContext** パラメーター。

> [AZURE.NOTE] 既定では、アカウントのプライマリ キーは、認証のために使用されるが、BatchAccountContext オブジェクトの変更することで使用するキーを明示的に選択できる **KeyInUse** プロパティ: `$context.KeyInUse = "Secondary"`です。


### データのクエリ

たとえば、次のように使用します。 **Get-azurebatchpools** プールを検索します。 デフォルトの自分のアカウントですべてのプールを照会この既にある場合、BatchAccountContext オブジェクト *$context*:

```
Get-AzureBatchPool -BatchContext $context
```
### OData フィルターを使用する

使用して OData フィルターを指定することができます、 **フィルター** に関心があるオブジェクトだけを検索するパラメーターです。 たとえば、ID が "myPool" で始まるすべてのプールを検索できます。

```
$filter = "startswith(id,'myPool')"

Get-AzureBatchPool -Filter $filter -BatchContext $context
```

このメソッドは、ローカルのパイプラインで “Where-Object” を使用するほど柔軟ではありません。 ただし、クエリは Batch サービスに直接送信されるため、すべてのフィルター処理がサーバー側で行われ、インターネットの帯域幅を節約できます。

### ID パラメーターの使用

代わりに OData フィルターには、使用する、 **Id** パラメーター。 "myPool" という ID の特定のプールを照会するには:

```
Get-AzureBatchPool -Id "myPool" -BatchContext $context

```
 **Id** 全 id の検索しないワイルドカードや OData 形式のフィルター パラメーターをサポートしています。

### パイプラインを作成する

Batch コマンドレットは、コマンドレット間でデータを送信するために PowerShell パイプラインを活用できます。 これは、パラメーターを指定するのと同じ効果がありますが、複数のエンティティを簡単に一覧表示できます。 たとえば、次のコマンドでは、自分のアカウントのすべてのタスクを検索しています。

```
Get-AzureBatchJob -BatchContext $context | Get-AzureBatchTask -BatchContext $context
```

### MaxCount パラメーターを使用する

既定では、各コマンドレットは最大で 1000 のオブジェクトを返します。 この制限に達した場合戻るより少ないオブジェクトをフィルターで絞りまたはのいずれか、最大を使用して明示的に設定、 **MaxCount** パラメーター。 次に例を示します。

```
Get-AzureBatchTask -MaxCount 2500 -BatchContext $context

```

上限の境界を削除するには、次のように設定します。 **MaxCount** を 0 以下です。

## 関連トピック
* [Azure PowerShell のダウンロード](http://go.microsoft.com/?linkid=9811175)
* [Azure PowerShell のインストールおよび構成方法](../powershell-install-configure.md)
* [Azure Batch コマンドレット リファレンス](https://msdn.microsoft.com/library/azure/mt125957.aspx)
* [効率的な Batch サービスのクエリ](batch-efficient-list-queries.md)

