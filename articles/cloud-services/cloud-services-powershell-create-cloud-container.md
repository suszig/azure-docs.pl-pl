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
1. Microsoft Azure PowerShell コマンドレットをインストール [Azure PowerShell のダウンロード](http://go.microsoft.com/?linkid=9811175&clcid=0x409)します。 手順の詳細について Azure PowerShell コマンドレットをインストールしてして、Azure サブスクリプションへの接続を参照してください [をインストールして、Azure PowerShell を構成する方法](../powershell-install-configure.md)します。

2. **New-azureservice** は空のクラウド サービス コンテナーを作成するコマンドレットです。

    ```
    New-AzureService [-ServiceName] <String> [-AffinityGroup] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
    New-AzureService [-ServiceName] <String> [-Location] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
```

   An example invoking the cmdlet is:
```
New-AzureService -ServiceName "mytestcloudservice" -Location "North Central US" -Label "mytestcloudservice"
```

3. For more information about creating Azure Cloud Service, please run:
```
Get-help New-AzureService
```

4. Next steps:

   - To manage the Cloud Service deployment, please refer to [Get-AzureService](https://msdn.microsoft.com/library/azure/dn495131.aspx), [Remove-AzureService](https://msdn.microsoft.com/library/azure/dn495120.aspx), and [Set-AzureService](https://msdn.microsoft.com/library/azure/dn495242.aspx) commands. Also please refer to [How to Configure Cloud Services](cloud-services-how-to-configure.md)

    - To publish your Cloud Service project to Azure, please refer to **PublishCloudService.ps1** code sample from [Continuous Delivery for Cloud Service in Azure](cloud-services-dotnet-continuous-delivery.md)
 

