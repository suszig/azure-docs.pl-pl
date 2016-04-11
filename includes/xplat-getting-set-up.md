<properties services="virtual-machines" title="Setting up Azure CLI for service management" authors="squillace" solutions="" manager="timlt" editor="tysonn" />

<tags
   ms.service="virtual-machine"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="linux"
   ms.workload="infrastructure"
   ms.date="04/13/2015"
   ms.author="rasquill" />

## Azure CLI の使用

次の手順を実行すると、適切なサブスクリプションで Azure CLI の最新バージョンを簡単に使用できます。 Azure CLI のインストールで最初に、アカウントに接続して表示する必要がある場合、 [Azure コマンド ライン インターフェイス (Azure CLI)](xplat-cli-install.md)します。

### 手順 1. Azure CLI のバージョンを更新する

サービス管理モードで命令型コマンドに Azure CLI を使用するときは、できれば最新バージョンを使用します。 バージョンを確認するには、「`azure --version`」と入力します。 次のような結果が表示されます。

    $ azure --version
    0.8.17 (node: 0.10.25)

Azure CLI のバージョンを更新する場合は、「 [Azure CLI](https://github.com/Azure/azure-xplat-cli)します。

### 手順 2. Azure アカウントとサブスクリプションを設定する

使用するアカウントに Azure CLI を接続すると、複数のサブスクリプションが存在する場合があります。 場合は、する必要が使用可能なサブスクリプション、アカウント確認」と入力して `azure account list`, 、」と入力して使用するサブスクリプションを選択し、 `azure account set <subscription id or name> true` 場所 _サブスクリプション id または名前_ はサブスクリプション id またはサブスクリプション名の現在のセッションで使用したいです。 次のように表示されます。

    $ azure account set "Visual Studio Ultimate with MSDN" true
    info:    Executing command account set
    info:    Setting subscription to "Visual Studio Ultimate with MSDN" with id "0e220bf6-5caa-4e9f-8383-51f16b6c109f".
    info:    Changes saved
    info:    account set command OK

> [AZURE.NOTE] Azure アカウントがない場合、MSDN サブスクリプションのサブスクリプションがアクティブ化して無料の Azure クレジットを取得できます、 [MSDN サブスクリプション会員の特典は、ここ](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) -無料のアカウントを使用することもできます。 どちらを使用しても Azure にアクセスできます。

