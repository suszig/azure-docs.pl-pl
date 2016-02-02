<properties 
    pageTitle="Azure コマンド ライン インターフェイス (Azure CLI) を使用して Azure Redis Cache を作成および管理する方法" 
    description="任意のプラットフォームに Azure CLI をインストールする方法、Azure CLI を使って Azure アカウントに接続する方法、Azure CLI で Redis Cache を作成および管理する方法。" 
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
    ms.date="12/02/2015" 
    ms.author="sdanie"/>


# Azure コマンド ライン インターフェイス (Azure CLI) を使用して Azure Redis Cache を作成および管理する方法

> [AZURE.SELECTOR]
- [PowerShell](cache-howto-manage-redis-cache-powershell.md)
- [Azure CLI](cache-manage-cli.md)


Azure CLI は、任意のプラットフォームから Azure インフラストラクチャを管理する優れた方法です。 この記事では、Azure CLI を使用して Azure Redis Cache インスタンスを作成および管理する方法について説明します。

## 前提条件

Azure CLI を使用して Azure Redis Cache インスタンスを作成および管理するには、次の手順を実行する必要があります。

-   Azure アカウントが必要です。 持っていない場合は、作成、 [無料試用版アカウント](http://azure.microsoft.com/pricing/free-trial/) 時間はわずかにします。
-   [Azure CLI のインストール](../xplat-cli-install.md)します。
-   Azure CLI のインストールを個人の Azure アカウント、または、職場の接続または学校の Azure アカウントと Azure CLI を使用して、ログイン、 `azure ログイン` コマンドです。 相違点を理解して、選択、次を参照してください。 [Azure サブスクリプションへの接続から、Azure コマンド ライン インターフェイス (Azure CLI)](../xplat-cli-connect.md)します。
-   次のコマンドを実行する前に Azure CLI リソース マネージャー モードに切り替えて実行して、 `azure config モード arm` コマンドです。 詳細については、次を参照してください。 [Azure リソース マネージャー モードの設定](../virtual-machines/xplat-cli-azure-resource-manager.md#setting-the-azure-resource-manager-mode)します。

## Redis Cache のプロパティ

Redis Cache インスタンスを作成および更新する場合には、次のプロパティを使用します。

| プロパティ| Switch| 説明|
|------------------|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| name| -n, --name| Redis Cache の名前です。|
| resource group| -g, --resource-group| リソース グループの名前です。|
| location| -l, --location| キャッシュを作成する場所です。|
| size| -z, --size| Redis Cache のサイズです。有効な値: [C0、C1、C2、C3、C4、C5、C6]|
| sku| -x, --sku| Redis SKU です。値は次のいずれかです: [Basic、Standard]|
| MaxMemoryPolicy| -m, --max-memory-policy| Redis Cache の MaxMemoryPolicy プロパティです。有効な値: [AllKeysLRU、AllKeysRandom、NoEviction、VolatileLRU、VolatileRandom、VolatileTTL]|
| EnableNonSslPort| -e、--enable-non-ssl-port| Redis Cache の EnableNonSslPort プロパティです。キャッシュの非 SSL ポートを有効にする場合は、このフラグを追加します。|
| subscription| -s、--subscription| サブスクリプションの識別子です。|
| key type| -t、--key-type| 更新するキーの種類です。有効な値: [Primary、Secondary]|

## すべての Redis Cache コマンドを参照してください。

すべての Redis Cache コマンドとそのパラメーターを表示する、 `azure redis キャッシュ用-h` コマンドです。

    C:\>azure rediscache -h
    help:    Commands to manage your Azure Redis Cache(s)
    help:
    help:    Create a Redis Cache
    help:      rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]
    help:
    help:    Delete an existing Redis Cache
    help:      rediscache delete [--name <name> --resource-group <resource-group> ]
    help:
    help:    List all Redis Caches within your Subscription or Resource Group
    help:      rediscache list [options]
    help:
    help:    Show properties of an existing Redis Cache
    help:      rediscache show [--name <name> --resource-group <resource-group>]
    help:
    help:    Change settings of an existing Redis Cache
    help:      rediscache set [--name <name> --resource-group <resource-group> --max-memory-policy <max-memory-policy>]
    help:
    help:    Renew the authentication key for an existing Redis Cache
    help:      rediscache renew-key [--name <name> --resource-group <resource-group> ]
    help:
    help:    Lists Primary and Secondary key of an existing Redis Cache
    help:      rediscache list-keys [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help  output usage information
    help:
    help:    Current Mode: arm (Azure Resource Management)

## Redis Cache の作成

Redis Cache を作成するには、次のコマンドを使用します。

    azure rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]

このコマンドの詳細については、実行、 `azure redis キャッシュ用に作成する-h` コマンドです。

    C:\>azure rediscache create -h
    help:    Create a Redis Cache
    help:
    help:    Usage: rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]
    help:
    help:    Options:
    help:      -h, --help                                   output usage information
    help:      -v, --verbose                                use verbose output
    help:      -vv                                          more verbose with debug output
    help:      --json                                       use json output
    help:      -n, --name <name>                            Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>        Name of the Resource Group
    help:      -l, --location <location>                    Location to create cache.
    help:      -z, --size <size>                            Size of the Redis Cache. Valid values: [C0, C1, C2, C3, C4, C5, C6]
    help:      -x, --sku <sku>                              Redis SKU. Should be one of : [Basic, Standard]
    help:      -m, --max-memory-policy <max-memory-policy>  MaxMemoryPolicy property of the Redis Cache. Valid values: [AllKeysLRU, AllKeysRandom, NoEviction, VolatileLRU, VolatileRandom, VolatileTTL]
    help:      -e, --enable-non-ssl-port                    EnableNonSslPort property of the Redis Cache. Add this flag if you want to enable the Non SSL Port for your cache
    help:      -s, --subscription <id>                      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## 既存の Redis Cache を削除する

Redis Cache を削除するには、次のコマンドを使用します。

    azure rediscache delete [--name <name> --resource-group <resource-group> ]

このコマンドの詳細については、実行、 `azure redis キャッシュ用削除-h` コマンドです。

    C:\>azure rediscache delete -h
    help:    Delete an existing Redis Cache
    help:
    help:    Usage: rediscache delete [--name <name> --resource-group <resource-group> ]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group under which the cache exists
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## サブスクリプションまたはリソース グループ内のすべての Redis Cache を一覧表示する

サブスクリプションまたはリソース グループ内のすべての Redis Cache を一覧表示するには、次のコマンドを使用します。

    azure rediscache list [options]

このコマンドの詳細については、実行、 `azure redis キャッシュ用の一覧には、-h` コマンドです。

    C:\>azure rediscache list -h
    help:    List all Redis Caches within your Subscription or Resource Group
    help:
    help:    Usage: rediscache list [options]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -g, --resource-group <resource-group>  Name of the Resource Group
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## 既存の Redis Cache のプロパティを表示する

既存の Redis Cache のプロパティを表示するには、次のコマンドを使用します。

    azure rediscache show [--name <name> --resource-group <resource-group>]

このコマンドの詳細については、実行、 `azure redis キャッシュ用に表示する-h` コマンドです。

    C:\>azure rediscache show -h
    help:    Show properties of an existing Redis Cache
    help:
    help:    Usage: rediscache show [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## 既存の Redis Cache の設定を変更する

既存の Redis Cache の設定を変更するには、次のコマンドを使用します。

    azure rediscache set [--name <name> --resource-group <resource-group> --max-memory-policy <max-memory-policy>]

このコマンドの詳細については、実行、 `azure redis キャッシュ用の設定 -h` コマンドです。

    C:\>azure rediscache set -h
    help:    Change settings of an existing Redis Cache
    help:
    help:    Usage: rediscache set [--name <name> --resource-group <resource-group> --max-memory-policy <max-memory-policy>]
    help:
    help:    Options:
    help:      -h, --help                                   output usage information
    help:      -v, --verbose                                use verbose output
    help:      -vv                                          more verbose with debug output
    help:      --json                                       use json output
    help:      -n, --name <name>                            Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>        Name of the Resource Group
    help:      -m, --max-memory-policy <max-memory-policy>  Max Memory Policy of the Redis Cache. Valid values: [AllKeysLRU, AllKeysRandom, NoEviction, VolatileLRU, VolatileRandom, VolatileTTL]
    help:      -s, --subscription <subscription>            the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## 既存の Redis Cache の認証キーを更新する

既存の Redis Cache の認証キーを更新するには、次のコマンドを使用します。

    azure rediscache renew-key [--name <name> --resource-group <resource-group> --key-type <key-type>]

指定 `プライマリ` または `セカンダリ` の `キー型`します。

このコマンドの詳細については、実行、 `azure redis キャッシュ用の更新キー h` コマンドです。

    C:\>azure rediscache renew-key -h
    help:    Renew the authentication key for an existing Redis Cache
    help:
    help:    Usage: rediscache renew-key [--name <name> --resource-group <resource-group> ]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group under which cache exists
    help:      -t, --key-type <key-type>              type of key to renew
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## 既存の Redis Cache のプライマリ キーとセカンダリ キーを一覧表示する

既存の Redis Cache のプライマリ キーとセカンダリ キーを一覧表示するには、次のコマンドを使用します。

    azure rediscache list-keys [--name <name> --resource-group <resource-group>]

このコマンドの詳細については、実行、 `azure redis キャッシュ用のキーの一覧表示 h` コマンドです。

    C:\>azure rediscache list-keys -h
    help:    Lists Primary and Secondary key of an existing Redis Cache
    help:
    help:    Usage: rediscache list-keys [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group under which Cache exists
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)



