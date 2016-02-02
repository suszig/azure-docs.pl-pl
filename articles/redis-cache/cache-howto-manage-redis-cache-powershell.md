<properties
    pageTitle="Azure PowerShell を使用した Azure Redis Cache の管理 | Microsoft Azure"
    description="Azure PowerShell を使用して Azure Redis Cache の管理タスクを実行する方法について説明します。"
    services="redis-cache"
    documentationCenter="" 
    authors="steved0x" 
    manager="dwrede" 
    editor=""/>

<tags
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="12/16/2015" 
    ms.author="sdanie"/>


# Azure PowerShell を使用した Azure Redis Cache の管理

> [AZURE.SELECTOR]
- [PowerShell](cache-howto-manage-redis-cache-powershell.md)
- [Azure CLI](cache-manage-cli.md)


このトピックでは、Azure Redis Cache インスタンスの作成、更新、スケールなどの一般的なタスクを実行する方法、アクセス キーを再生成する方法、キャッシュに関する情報を表示する方法について説明します。 Azure Redis キャッシュの PowerShell コマンドレットの一覧については、次を参照してください。 [Azure Redis Cache コマンドレット](https://msdn.microsoft.com/library/azure/mt634513.aspx)します。

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)] [従来のデプロイ モデル](#classic) この記事の後半で説明します。

## 前提条件

>[AZURE.IMPORTANT] 初めてサブスクリプションの Azure ポータルを使用して、Redis cache を作成する、ポータルの登録、 `Microsoft.Cache` そのサブスクリプションの名前空間。 PowerShell を使用して、サブスクリプションで最初の Redis cache を作成しようとする場合は、次のコマンドを使用してその名前空間をまず登録する必要があります。それ以外の場合コマンドレット `新規 AzureRmRedisCache` と `Get AzureRmRedisCache` は失敗します。
>
>`レジスタ AzureRmResourceProvider ProviderNamespace"Microsoft.Cache"`

Azure PowerShell をインストール済みである場合、Azure PowerShell Version 1.0.0 以降であることが必要です。 インストールした Azure PowerShell のバージョンは、Azure PowerShell コマンド プロンプトで次のコマンドを使用して確認できます。

    Get-Module azure | format-table version

[AZURE.INCLUDE [powershell-preview](../../includes/powershell-preview-inline-include.md)]

まず、次のコマンドで Azure にログオンする必要があります。

    Login-AzureRmAccount

Microsoft Azure のサインイン ダイアログで、Azure アカウントの電子メール アドレスとそのパスワードを指定します。

次に、Azure サブスクリプションが複数ある場合は、使用する Azure サブスクリプションを設定する必要があります。 現在のサブスクリプションを一覧表示するには、次のコマンドを実行します。

    Get-AzureRmSubscription | sort SubscriptionName | Select SubscriptionName

サブスクリプションを指定するには、次のコマンドを実行します。 次の例では、サブスクリプション名は `ContosoSubscription`します。

    Select-AzureRmSubscription -SubscriptionName ContosoSubscription

Azure リソース マネージャーで Windows PowerShell を使用するには、以下が必要です。

- Windows PowerShell バージョン 3.0 または 4.0。 Windows PowerShell のバージョンを調べるには入力:`$PSVersionTable` の値を確認 `PSVersion` が 3.0 または 4.0。 互換性のあるバージョンをインストールするを参照してください。 [Windows Management Framework 3.0 ](http://www.microsoft.com/download/details.aspx?id=34595) または [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855)します。

このチュートリアルに表示されているコマンドレットの詳しいヘルプを確認には、Get-Help コマンドレットを使用します。

    Get-Help <cmdlet-name> -Detailed

たとえば、ヘルプを参照するため、 `新規 AzureRmRedisCache` コマンドレットの種類。

    Get-Help New-AzureRmRedisCache -Detailed

## Azure Redis Cache 用の PowerShell で使用されるプロパティ

次の表は、Azure PowerShell を使用して Azure Redis Cache インスタンスを作成し、管理する際に一般的に使用されるパラメーターのプロパティと説明を示しています。

| パラメーター| 説明| 既定値|
|--------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| 名前| キャッシュの名前| |
| Location (場所)| キャッシュの場所| |
| ResourceGroupName| キャッシュを作成するリソース グループの名前| |
| サイズ| キャッシュのサイズ。有効な値: P1、P2、P3、P4、C0、C1、C2、C3、C4、C5、C6、250 MB、1 GB、2.5 GB、6 GB、13 GB、26 GB、53 GB| 1GB|
| ShardCount| クラスタリングが有効になっている Premium キャッシュを作成する際に作成するシャードの数。有効な値: 1、2、3、4、5、6、7、8、9、10| |
| SKU| キャッシュの SKU を指定します。有効な値: Basic、Standard、Premium| Standard|
| RedisConfiguration| maxmemory-delta、maxmemory-policy、notify-keyspace-events の Redis 構成設定を指定します。maxmemory-delta と notify-keyspace-events は Standard および Premium のキャッシュでのみ使用できます。| |
| EnableNonSslPort| 非 SSL ポートが有効になっているかどうかを示します。| False|
| MaxMemoryPolicy| このパラメーターは廃止されました。代わりに、RedisConfiguration を使用します。| |
| StaticIP| VNET でキャッシュをホストする場合に、キャッシュのサブネットで一意の IP アドレスを指定します。| |
| サブネット| VNET でキャッシュをホストする場合に、キャッシュをデプロイするサブネットの名前を指定します。| |
| VirtualNetwork| VNET でキャッシュをホストする場合に、キャッシュをデプロイする VNET のリソース ID を指定します。| |
| KeyType| アクセス キーを更新するときに再生成するアクセス キーを指定します。有効な値: Primary、Secondary| | | |


## Redis Cache の作成方法

使用して新しい Azure Redis Cache インスタンスが作成された、 [新規 AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) コマンドレットです。

使用可能なパラメーターとその説明の一覧を表示する `新規 AzureRmRedisCache`, 、次のコマンドを実行します。

    PS C:\> Get-Help New-AzureRmRedisCache -detailed
    
    NAME
        New-AzureRmRedisCache
    
    SYNOPSIS
        Creates a new redis cache.
    
    SYNTAX
        New-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Location <String> [-RedisVersion <String>]
        [-Size <String>] [-Sku <String>] [-MaxMemoryPolicy <String>] [-RedisConfiguration <Hashtable>] [-EnableNonSslPort
        <Boolean>] [-ShardCount <Integer>] [-VirtualNetwork <String>] [-Subnet <String>] [-StaticIP <String>]
        [<CommonParameters>]
    
    DESCRIPTION
        The New-AzureRmRedisCache cmdlet creates a new redis cache.
    
    PARAMETERS
        -Name <String>
            Name of the redis cache to create.
    
        -ResourceGroupName <String>
            Name of resource group in which to create the redis cache.
    
        -Location <String>
            Location in which to create the redis cache.
    
        -RedisVersion <String>
            RedisVersion is deprecated and will be removed in future release.
    
        -Size <String>
            Size of the redis cache. The default value is 1GB or C1. Possible values are P1, P2, P3, P4, C0, C1, C2, C3,
            C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB.
    
        -Sku <String>
            Sku of redis cache. The default value is Standard. Possible values are Basic, Standard and Premium.
    
        -MaxMemoryPolicy <String>
            The 'MaxMemoryPolicy' setting has been deprecated. Please use 'RedisConfiguration' setting to set
            MaxMemoryPolicy. e.g. -RedisConfiguration @{"maxmemory-policy" = "allkeys-lru"}
    
        -RedisConfiguration <Hashtable>
            All Redis Configuration Settings. Few possible keys: rdb-backup-enabled, rdb-storage-connection-string,
            rdb-backup-frequency, maxmemory-delta, maxmemory-policy, notify-keyspace-events, maxmemory-samples,
            slowlog-log-slower-than, slowlog-max-len, list-max-ziplist-entries, list-max-ziplist-value,
            hash-max-ziplist-entries, hash-max-ziplist-value, set-max-intset-entries, zset-max-ziplist-entries,
            zset-max-ziplist-value etc.
    
        -EnableNonSslPort <Boolean>
            EnableNonSslPort is used by Azure Redis Cache. If no value is provided, the default value is false and the
            non-SSL port will be disabled. Possible values are true and false.
    
        -ShardCount <Integer>
            The number of shards to create on a Premium Cluster Cache.
    
        -VirtualNetwork <String>
            The exact ARM resource ID of the virtual network to deploy the redis cache in. Example format:
            /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1
    
        -Subnet <String>
            Required when deploying a redis cache inside an existing Azure Virtual Network.
    
        -StaticIP <String>
            Required when deploying a redis cache inside an existing Azure Virtual Network.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

既定のパラメーターを使用してキャッシュを作成するには、次のコマンドを実行します。

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US"

`ResourceGroupName`, 、`名`, 、および `場所` 、必要なパラメーターが、残りの部分は省略可能で、既定値を設定します。 前述のコマンドを実行すると、Standard SKU の Azure Redis Cache インスタンスが作成され、指定した名前、場所、リソース グループが設定されます。サイズは 1 GB に設定され、非 SSL ポートは無効になっています。

Premium キャッシュを作成する場合は、P1 (6 GB ～ 60 GB)、P2 (13 GB ～ 130 GB)、P3 (26 GB ～ 260 GB)、P4 (53 GB ～ 530 GB) のいずれかのサイズを指定します。 クラスタ リングを有効にするを使用してシャード数を指定、 `ShardCount` パラメーター。 次の例では、3 つのシャードを指定して P1 の Premium キャッシュを作成しています。 P1 Premium キャッシュはサイズが 6 GB です。3 つのシャードを指定したため、合計サイズは 18 GB (3 x 6 GB) です。

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -Sku Premium -Size P1 -ShardCount 3

値を指定する、 `RedisConfiuration` パラメーター内の値を囲む `{}` のようなキー/値のペアとして `@{[maxmemory ポリシー] =「ペアを-ランダムな」、「通知-keyspace-イベント」=「山は水深」}`します。 次の例では、標準的な 1 GB のキャッシュと `ペアをランダム` maxmemory ポリシーとキースペース通知を使用して構成 `山は水深`します。 詳細については、次を参照してください。 [キースペース通知 (詳細設定)](cache-configure.md#keyspace-notifications-advanced-settings) と [maxmemory-policy と maxmemory-reserved](cache-configure.md#maxmemory-policy-and-maxmemory-reserved)します。

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -RedisConfiguration @{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}

## Redis Cache の更新方法

使用して azure Redis Cache インスタンスが更新され、 [セット AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634518.aspx) コマンドレットです。

使用可能なパラメーターとその説明の一覧を表示する `セット AzureRmRedisCache`, 、次のコマンドを実行します。

    PS C:\> Get-Help Set-AzureRmRedisCache -detailed
    
    NAME
        Set-AzureRmRedisCache
    
    SYNOPSIS
        Set redis cache updatable parameters.
    
    SYNTAX
        Set-AzureRmRedisCache -Name <String> -ResourceGroupName <String> [-Size <String>] [-Sku <String>]
        [-MaxMemoryPolicy <String>] [-RedisConfiguration <Hashtable>] [-EnableNonSslPort <Boolean>] [-ShardCount
        <Integer>] [<CommonParameters>]
    
    DESCRIPTION
        The Set-AzureRmRedisCache cmdlet sets redis cache parameters.
    
    PARAMETERS
        -Name <String>
            Name of the redis cache to update.
    
        -ResourceGroupName <String>
            Name of the resource group for the cache.
    
        -Size <String>
            Size of the redis cache. The default value is 1GB or C1. Possible values are P1, P2, P3, P4, C0, C1, C2, C3,
            C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB.
    
        -Sku <String>
            Sku of redis cache. The default value is Standard. Possible values are Basic, Standard and Premium.
    
        -MaxMemoryPolicy <String>
            The 'MaxMemoryPolicy' setting has been deprecated. Please use 'RedisConfiguration' setting to set
            MaxMemoryPolicy. e.g. -RedisConfiguration @{"maxmemory-policy" = "allkeys-lru"}
    
        -RedisConfiguration <Hashtable>
            All Redis Configuration Settings. Few possible keys: rdb-backup-enabled, rdb-storage-connection-string,
            rdb-backup-frequency, maxmemory-delta, maxmemory-policy, notify-keyspace-events, maxmemory-samples,
            slowlog-log-slower-than, slowlog-max-len, list-max-ziplist-entries, list-max-ziplist-value,
            hash-max-ziplist-entries, hash-max-ziplist-value, set-max-intset-entries, zset-max-ziplist-entries,
            zset-max-ziplist-value etc.
    
        -EnableNonSslPort <Boolean>
            EnableNonSslPort is used by Azure Redis Cache. The default value is null and no change will be made to the
            currently configured value. Possible values are true and false.
    
        -ShardCount <Integer>
            The number of shards to create on a Premium Cluster Cache.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

`セット AzureRmRedisCache` などのプロパティを更新するために使用できる `サイズ`, 、`Sku`, 、`EnableNonSslPort` と `RedisConfiguration` 値。

次のコマンドを実行すると、myCache という名前の Redis Cache の maxmemory-policy が更新されます。

    Set-AzureRmRedisCache -ResourceGroupName "myGroup" -Name "myCache" -RedisConfiguration @{"maxmemory-policy" = "allkeys-random"}

## PowerShell を使用した Redis Cache のスケール方法

`セット AzureRmRedisCache` 、Azure Redis cache の規模の設定に使用できるインスタンスの場合、 `サイズ`, 、`Sku`, 、または `ShardCount` プロパティを変更します。
>[AZURE.NOTE]PowerShell を使用してキャッシュをスケールする場合、Azure ポータルからキャッシュをスケールする場合と同じ制限とガイドラインが適用されます。 別の価格レベルにスケーリングできますが、次のような制約があります。
>
>-スケールできない間、 **Premium** キャッシュします。
>に規模の設定することはできません、 **標準** するためのキャッシュ、 **基本的な** キャッシュします。
>-からを拡張、 **基本的な** するためのキャッシュ、 **標準** キャッシュですが、同時に、サイズは変更できません。 サイズを変更する必要がある場合、後続のスケーリング操作でサイズを変更できます。
>-下に大きなサイズの拡大/縮小することはできません、 **C0 (250 MB)** サイズ。
>
>詳細については、次を参照してください。 [スケール Azure Redis Cache 方法](cache-how-to-scale.md)します。

次の例は、名前付きキャッシュをスケーリングする方法を示しています。 `myCache` 2.5 GB のキャッシュします。 このコマンドは、Basic と Standard の両方のキャッシュで使用できます。

    Set-AzureRmRedisCache -ResourceGroupName myGroup -Name myCache -Size 2.5GB

このコマンドを発行した後、キャッシュの状態が返されます (を呼び出すことのような `Get AzureRmRedisCache`)。 注意してください、 `ProvisioningState` は `スケーリング`します。

    PS C:\> Set-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup -Size 2.5GB
    
    
    Name               : mycache
    Id                 : /subscriptions/12ad12bd-abdc-2231-a2ed-a2b8b246bbad4/resourceGroups/mygroup/providers/Mi
                         crosoft.Cache/Redis/mycache
    Location           : South Central US
    Type               : Microsoft.Cache/Redis
    HostName           : mycache.redis.cache.windows.net
    Port               : 6379
    ProvisioningState  : Scaling
    SslPort            : 6380
    RedisConfiguration : {[maxmemory-policy, volatile-lru], [maxmemory-reserved, 150], [notify-keyspace-events, KEA],
                         [maxmemory-delta, 150]...}
    EnableNonSslPort   : False
    RedisVersion       : 3.0
    Size               : 1GB
    Sku                : Standard
    ResourceGroupName  : mygroup
    PrimaryKey         : ....
    SecondaryKey       : ....
    VirtualNetwork     :
    Subnet             :
    StaticIP           :
    TenantSettings     : {}
    ShardCount         :

スケーリング処理の完了時に、 `ProvisioningState` に変更されます `Succeeded`します。 Basic から Standard に変更した後にサイズを変更するなど、前の操作に続けてスケーリング操作の実行が必要になる場合は、前の操作が完了するまで待つ必要があります。そうでないと、次のようなエラーが表示されます。

    Set-AzureRmRedisCache : Conflict: The resource '...' is not in a stable state, and is currently unable to accept the update request.

## Redis Cache に関する情報を取得する方法

使用してキャッシュに関する情報を取得できます、 [Get AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634514.aspx) コマンドレットです。

使用可能なパラメーターとその説明の一覧を表示する `Get AzureRmRedisCache`, 、次のコマンドを実行します。

    PS C:\> Get-Help Get-AzureRmRedisCache -detailed
    
    NAME
        Get-AzureRmRedisCache
    
    SYNOPSIS
        Gets details about a single cache or all caches in the specified resource group or all caches in the current
        subscription.
    
    SYNTAX
        Get-AzureRmRedisCache [-Name <String>] [-ResourceGroupName <String>] [<CommonParameters>]
    
    DESCRIPTION
        The Get-AzureRmRedisCache cmdlet gets the details about a cache or caches depending on input parameters. If both
        ResourceGroupName and Name parameters are provided then Get-AzureRmRedisCache will return details about the
        specific cache name provided.
    
        If only ResourceGroupName is provided than it will return details about all caches in the specified resource group.
    
        If no parameters are given than it will return details about all caches the current subscription.
    
    PARAMETERS
        -Name <String>
            The name of the cache. When this parameter is provided along with ResourceGroupName, Get-AzureRmRedisCache
            returns the details for the cache.
    
        -ResourceGroupName <String>
            The name of the resource group that contains the cache or caches. If ResourceGroupName is provided with Name
            then Get-AzureRmRedisCache returns the details of the cache specified by Name. If only the ResourceGroup
            parameter is provided, then details for all caches in the resource group are returned.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

現在のサブスクリプションのすべてのキャッシュに関する情報を返す、次のように実行します。 `Get AzureRmRedisCache` パラメーターなし。

    Get-AzureRmRedisCache

特定のリソース グループ内のすべてのキャッシュに関する情報を返す、次のように実行します。 `Get AzureRmRedisCache` で、 `ResourceGroupName` パラメーター。

    Get-AzureRmRedisCache -ResourceGroupName myGroup

特定のキャッシュに関する情報を返すには、次のように実行します。 `Get AzureRmRedisCache` で、 `名` 、キャッシュの名前を含むパラメーターおよび `ResourceGroupName` そのキャッシュを含むリソース グループのパラメーターです。

    PS C:\> Get-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup
    
    Name               : mycache
    Id                 : /subscriptions/12ad12bd-abdc-2231-a2ed-a2b8b246bbad4/resourceGroups/myGroup/providers/Mi
                         crosoft.Cache/Redis/mycache
    Location           : South Central US
    Type               : Microsoft.Cache/Redis
    HostName           : mycache.redis.cache.windows.net
    Port               : 6379
    ProvisioningState  : Succeeded
    SslPort            : 6380
    RedisConfiguration : {[maxmemory-policy, volatile-lru], [maxmemory-reserved, 62], [notify-keyspace-events, KEA],
                         [maxclients, 1000]...}
    EnableNonSslPort   : False
    RedisVersion       : 3.0
    Size               : 1GB
    Sku                : Standard
    ResourceGroupName  : myGroup
    VirtualNetwork     :
    Subnet             :
    StaticIP           :
    TenantSettings     : {}
    ShardCount         :

## Redis Cache のアクセス キーを取得する方法

使用することができます、キャッシュのアクセス キーを取得する、 [Get AzureRmRedisCacheKey](https://msdn.microsoft.com/library/azure/mt634516.aspx) コマンドレットです。

使用可能なパラメーターとその説明の一覧を表示する `Get AzureRmRedisCacheKey`, 、次のコマンドを実行します。

    PS C:\> Get-Help Get-AzureRmRedisCacheKey -detailed
    
    NAME
        Get-AzureRmRedisCacheKey
    
    SYNOPSIS
        Gets the accesskeys for the specified redis cache.
    
    
    SYNTAX
        Get-AzureRmRedisCacheKey -Name <String> -ResourceGroupName <String> [<CommonParameters>]
    
    DESCRIPTION
        The Get-AzureRmRedisCacheKey cmdlet gets the access keys for the specified cache.
    
    PARAMETERS
        -Name <String>
            Name of the redis cache.
    
        -ResourceGroupName <String>
            Name of the resource group for the cache.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

キャッシュのキーを取得する、 `Get AzureRmRedisCacheKey` コマンドレットの名前を渡します、キャッシュ キャッシュを格納するリソース グループの名前。

    PS C:\> Get-AzureRmRedisCacheKey -Name myCache -ResourceGroupName myGroup
    
    PrimaryKey   : b2wdt43sfetlju4hfbryfnregrd9wgIcc6IA3zAO1lY=
    SecondaryKey : ABhfB757JgjIgt785JgKH9865eifmekfnn649303JKL=

## Redis Cache のアクセス キーを再生成する方法

使用することができます、キャッシュのアクセス キーを再生成する、 [新規 AzureRmRedisCacheKey](https://msdn.microsoft.com/library/azure/mt634512.aspx) コマンドレットです。

使用可能なパラメーターとその説明の一覧を表示する `新規 AzureRmRedisCacheKey`, 、次のコマンドを実行します。

    PS C:\> Get-Help New-AzureRmRedisCacheKey -detailed
    
    NAME
        New-AzureRmRedisCacheKey
    
    SYNOPSIS
        Regenerates the access key of a redis cache.
    
    SYNTAX
        New-AzureRmRedisCacheKey -Name <String> -ResourceGroupName <String> -KeyType <String> [-Force] [<CommonParameters>]
    
    DESCRIPTION
        The New-AzureRmRedisCacheKey cmdlet regenerate the access key of a redis cache.
    
    PARAMETERS
        -Name <String>
            Name of the redis cache.
    
        -ResourceGroupName <String>
            Name of the resource group for the cache.
    
        -KeyType <String>
            Specifies whether to regenerate the primary or secondary access key. Possible values are Primary or Secondary.
    
        -Force
            When the Force parameter is provided, the specified access key is regenerated without any confirmation prompts.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

キャッシュのプライマリまたはセカンダリ キーを再生成する、 `新規 AzureRmRedisCacheKey` コマンドレットどちらかを指定し、リソース グループ名に渡し、 `プライマリ` または `セカンダリ` の `KeyType` パラメーター。 次の例では、キャッシュのセカンダリ アクセス キーが再生成されます。

    PS C:\> New-AzureRmRedisCacheKey -Name myCache -ResourceGroupName myGroup -KeyType Secondary
    
    Confirm
    Are you sure you want to regenerate Secondary key for redis cache 'myCache'?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y
    
    
    PrimaryKey   : b2wdt43sfetlju4hfbryfnregrd9wgIcc6IA3zAO1lY=
    SecondaryKey : c53hj3kh4jhHjPJk8l0jji785JgKH9865eifmekfnn6=

## Redis Cache を削除する方法

Redis キャッシュを削除するを使用して、 [削除 AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634515.aspx) コマンドレットです。

使用可能なパラメーターとその説明の一覧を表示する `削除 AzureRmRedisCache`, 、次のコマンドを実行します。

    PS C:\> Get-Help Remove-AzureRmRedisCache -detailed
    
    NAME
        Remove-AzureRmRedisCache
    
    SYNOPSIS
        Remove redis cache if exists.
    
    SYNTAX
        Remove-AzureRmRedisCache -Name <String> -ResourceGroupName <String> [-Force] [-PassThru] [<CommonParameters>
    
    DESCRIPTION
        The Remove-AzureRmRedisCache cmdlet removes a redis cache if it exists.
    
    PARAMETERS
        -Name <String>
            Name of the redis cache to remove.
    
        -ResourceGroupName <String>
            Name of the resource group of the cache to remove.
    
        -Force
            When the Force parameter is provided, the cache is removed without any confirmation prompts.
    
        -PassThru
            By default Remove-AzureRmRedisCache removes the cache and does not return any value. If the PassThru par
            is provided then Remove-AzureRmRedisCache returns a boolean value indicating the success of the operatio
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

次の例で、キャッシュの名前 `myCache` を削除します。

    PS C:\> Remove-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup
    
    Confirm
    Are you sure you want to remove redis cache 'myCache'?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y

<a name="classic"></a>
## PowerShell クラシック デプロイ モデルを使用した Azure Redis Cache インスタンスの管理

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] [リソース マネージャー モデル](cache-howto-manage-redis-cache-powershell.md) この記事の冒頭で説明します。

次のスクリプトは、クラシック デプロイ モデルを使用して Azure Redis Cache を作成、更新、および削除する方法を示しています。

        $VerbosePreference = "Continue"
    
        # Create a new cache with date string to make name unique.
        $cacheName = "MovieCache" + $(Get-Date -Format ('ddhhmm'))
        $location = "West US"
        $resourceGroupName = "Default-Web-WestUS"
    
        $movieCache = New-AzureRedisCache -Location $location -Name $cacheName  -ResourceGroupName $resourceGroupName -Size 250MB -Sku Basic
    
        # Wait until the Cache service is provisioned.
    
        for ($i = 0; $i -le 60; $i++)
        {
            Start-Sleep -s 30
            $cacheGet = Get-AzureRedisCache -ResourceGroupName $resourceGroupName -Name $cacheName
            if ([string]::Compare("succeeded", $cacheGet[0].ProvisioningState, $True) -eq 0)
            {
                break
            }
            If($i -eq 60)
            {
                exit
            }
        }
    
        # Update the access keys.
    
        Write-Verbose "PrimaryKey: $($movieCache.PrimaryKey)"
        New-AzureRedisCacheKey -KeyType "Primary" -Name $cacheName  -ResourceGroupName $resourceGroupName -Force
        $cacheKeys = Get-AzureRedisCacheKey -ResourceGroupName $resourceGroupName  -Name $cacheName
        Write-Verbose "PrimaryKey: $($cacheKeys.PrimaryKey)"
    
        # Use Set-AzureRedisCache to set Redis cache updatable parameters.
        # Set the memory policy to Least Recently Used.
    
        Set-AzureRedisCache -Name $cacheName -ResourceGroupName $resourceGroupName -RedisConfiguration @{"maxmemory-policy" = "AllKeys-LRU"}
    
        # Delete the cache.
    
        Remove-AzureRedisCache -Name $movieCache.Name -ResourceGroupName $movieCache.ResourceGroupName  -Force

## 次のステップ

Azure での Windows PowerShell の使用の詳細については、次のリソースを参照してください。

- [MSDN の azure Redis Cache コマンドレットのドキュメント](https://msdn.microsoft.com/library/azure/mt634513.aspx)
- [Azure リソース マネージャー コマンドレット](http://go.microsoft.com/fwlink/?LinkID=394765): AzureResourceManager モジュールのコマンドレットを使用する方法について説明します。
- [を使用してリソース グループを Azure のリソースを管理](../azure-portal/resource-group-portal.md): 作成し、Azure ポータルでリソース グループを管理する方法について説明します。
- [Azure ブログ](http://blogs.msdn.com/windowsazure): Azure での新機能について説明します。
- [Windows PowerShell のブログ](http://blogs.msdn.com/powershell): Windows PowerShell の新機能について説明します。
- ["Hey, Scripting Guy!" ブログ] (http://blogs.technet.com/b/heyscriptingguy/): 実際のヒントとテクニックを Windows PowerShell コミュニティから取得します。





