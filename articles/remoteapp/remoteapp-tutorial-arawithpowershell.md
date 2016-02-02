<properties
   pageTitle="Powershell とともに Azure RemoteApp を使用開始する | Microsoft Azure"
   description="Powershell とともに Azure RemoteApp を使用開始する方法の詳細"
   services="remoteapp"
   documentationCenter=""
   authors="guscatalano"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="compute"
   ms.date="12/05/2015"
   ms.author="guscatal;spatnaik;elizapo"/>




# Powershell とともに Azure RemoteApp を使用開始する

=====================================


## コマンドレットの取得

-------------

Azure Powershell コマンドレットをダウンロードする必要があります [ここ](http://go.microsoft.com/?linkid=9811175), 、RemoteApp がそれに含まれるものです。

Azure RemoteApp のコマンドレットのヘルプをチェック_アウト [ここ](https://msdn.microsoft.com/library/mt428031.aspx)します。

## サブスクリプションを使用するために Azure コマンドレットを構成します。

------------------

次の [このガイド](../powershell-install-configure.md) 、Azure サブスクリプションに対してコマンドレットを使用できるようにします。

## Create a cloud collection (クラウド コレクションを作成する)

--------------------

次のコマンドを実行するだけです。

    New-AzureRemoteAppCollection -Collectionname RAppO365Col1 -ImageName "Office 365 ProPlus (Subscription required)" -Plan Basic -Location "West US" - Description "Office 365 Collection."

上記のコマンドを実行すると、Microsoft Office 365 アプリケーション  (Excel、OneNote、Outlook、PowerPoint、Visio および Word) が自動的に発行されます。

コレクションの作成には、完了まで 30 分以上かかる場合があります。 そのため、このコマンドは追跡に使用できる次のような ID を返します。


    Get-AzureRemoteAppOperationResult -TrackingId xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

コレクションが完成したら、次のコマンドを使用して、コレクションにユーザーを追加できます。

    Add-AzureRemoteAppUser -CollectionName RAppO365Col1 -Type microsoftAccount -UserUpn someone@domain.com

これで完了です。 ユーザーは、Azure RemoteApp クライアントを使用してアプリケーションに接続することである必要があります [ここ](https://www.remoteapp.windowsazure.com/)します。

## 利用可能なコマンドレット

他にも多くのコマンドがあり、それらのコマンドに関するドキュメントも間もなく公開される予定です。

基本的な RemoteApp コレクションのコマンドレット:

- New-AzureRemoteAppCollection
- Get-AzureRemoteAppCollection
- Set-AzureRemoteAppCollection
- Update-AzureRemoteAppCollection
- Remove-AzureRemoteAppCollection
- Add-AzureRemoteAppUser
- Get-AzureRemoteAppUser
- Remove-AzureRemoteAppUser
- Get-AzureRemoteAppSession
- Disconnect-AzureRemoteAppSession
- Invoke-AzureRemoteAppSessionLogoff
- Send-AzureRemoteAppSessionMessage
- Get-AzureRemoteAppProgram
- Get-AzureRemoteAppStartMenuProgram
- Publish-AzureRemoteAppProgram
- Unpublish-AzureRemoteAppProgram
- Get-AzureRemoteAppCollectionUsageDetails
- Get-AzureRemoteAppCollectionUsageSummary
- Get-AzureRemoteAppPlan

RemoteApp 仮想ネットワークのコマンドレット:

- New-AzureRemoteAppVNet
- Get-AzureRemoteAppVNet
- Set-AzureRemoteAppVNet
- Remove-AzureRemoteAppVNet
- Get-AzureRemoteAppVpnDevice
- Get-- AzureRemoteAppVpnDeviceConfigScript
- Reset-AzureRemoteAppVpnSharedKey

RemoteApp テンプレート イメージのコマンドレット:

- New-AzureRemoteAppTemplateImage
- Get-AzureRemoteAppTemplateImage
- Rename-AzureRemoteAppTemplateImage
- Remove-AzureRemoteAppTemplateImage

その他の RemoteApp のコマンドレット:

- Get-AzureRemoteAppLocation
- Get-AzureRemoteAppWorkspace
- Set-AzureRemoteAppWorkspace
- Get-AzureRemoteAppOperationResult





