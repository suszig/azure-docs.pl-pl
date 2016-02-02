<properties
   pageTitle="Azure PowerShell コマンドを使用して空のクラウド サービス コンテナーを作成する方法"
   description="この記事では、PowerShell スクリプトを使用して、クラウド サービス コンテナーを作成し、クラウド サービスに関連する管理操作を実行する方法について説明します。"
   services="cloud-services"
   documentationCenter=".net"
   authors="cawaMS"
   manager="paulyuk" 
   editor=""/>

<tags
   ms.service="cloud-services"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="powershell"
   ms.workload="na"
   ms.date="10/14/2015"
   ms.author="cawa"/>


# Azure PowerShell コマンドを使用して空のクラウド サービス コンテナーを作成する方法

1. Microsoft Azure PowerShell コマンドレットをインストール [Azure PowerShell のダウンロード](http://go.microsoft.com/?linkid=9811175&clcid=0x409)します。手順の詳細について Azure PowerShell コマンドレットをインストールしてして、Azure サブスクリプションへの接続を参照してください [をインストールして、Azure PowerShell を構成する方法](../powershell-install-configure.md)します。

2. **New-AzureService** は、空のクラウド サービス コンテナーを作成するためのコマンドレットです。

    ```
    New-azureservice [-ServiceName] <String> [-アフィニティ グループ] <String> [-ラベル] <String>] [の説明] <String>] [-ReverseDnsFqdn] <String>] [<CommonParameters>]
    New-azureservice [-ServiceName] <String> [-場所] <String> [-ラベル] <String>] [の説明] <String>] [-ReverseDnsFqdn] <String>] [<CommonParameters>]
```

   An example invoking the cmdlet is:
```
New-AzureService -ServiceName "mytestcloudservice" -Location "North Central US" -Label "mytestcloudservice"
```

3. For more information about creating Azure Cloud Service, please run:
```
Get-help New-AzureService
```

4. 次のステップ:

   - クラウド サービスの展開を管理するを参照してください [Get-azureservice](https://msdn.microsoft.com/library/azure/dn495131.aspx), 、[Remove-azureservice](https://msdn.microsoft.com/library/azure/dn495120.aspx), 、および [Set-azureservice](https://msdn.microsoft.com/library/azure/dn495242.aspx) コマンドです。 また、を参照してください [クラウド サービスの構成方法](cloud-services-how-to-configure.md)

    - Azure にクラウド サービス プロジェクトを発行するを参照してください **PublishCloudService.ps1** コード サンプルを [Azure のクラウド サービスの継続的な配信](cloud-services-dotnet-continuous-delivery.md)






