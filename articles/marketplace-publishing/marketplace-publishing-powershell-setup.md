<properties
   pageTitle="Marketplace 向けの VM を作成するための PowerShell のセットアップ | Microsoft Azure"
   description="Azure PowerShell をセットアップしてオプションのプロセス フローとして使用し、VM イメージを作成して Azure Marketplace にデプロイし、販売する方法を説明します"
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager=""
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/08/2015"
   ms.author="hascipio"/>

# Azure Marketplace のプランを作成するための Azure PowerShell のセットアップ
Azure PowerShell を設定する方法の詳細については、次を参照してください。 [をインストールして、Azure PowerShell を構成する方法](../powershell-install-configure.md)します。 証明書方式を使用すると、認証に必要な証明書がダウンロードおよびインポートされるため簡単に処理できます。 必要な証明書を取得するを使用して、 **Get-azurepublishsettingsfile** コマンドレットです。 求められたらファイルを保存します。 PowerShell セッションに、証明書をインポートするには、使用、 **Import-azurepublishsettingsfile** コマンドレットです。

構成し、PowerShell セッションの一般的な Microsoft Azure サブスクリプションの設定を保存を使用して、 **Set-azuresubscription** と **Select-azuresubscription** コマンドレット。

        Set-AzureSubscription -SubscriptionName “mySubName” -CurrentStorageAccountName “mystorageaccount”
        Select-AzureSubscription -SubscriptionName "mySubName" –Current

最初のコマンドは、既定のストレージ アカウントをサブスクリプションに関連付けます (一部の VM プロビジョニング操作に必要)。  2 番目のコマンドは、そのサブスクリプションを現在のサブスクリプションにします (他のコマンドレットによって認識される)。

## 関連項目
- [Microsoft Azure Marketplace へのプランの発行方法](marketplace-publishing-getting-started.md)
- [Marketplace 向けの仮想マシン イメージの作成](marketplace-publishing-vm-image-creation.md)

