<properties 
    pageTitle="PowerShell を使用した Azure Cloud Services での診断の有効化 | Microsoft Azure" 
    description="PowerShell を使用して Cloud Services の診断を有効にする方法について説明します。" 
    services="cloud-services" 
    documentationCenter=".net" 
    authors="sbtron" 
    manager="" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="11/16/2015" 
    ms.author="saurabh"/>



# PowerShell を使用した Azure Cloud Services での診断の有効化

Azure 診断拡張機能を使用して、Cloud Service からアプリケーション ログやパフォーマンス カウンターなどの診断データを収集できます。 この記事では、PowerShell を使用して Cloud Service の Azure 診断拡張機能を有効にする方法について説明します。 参照してください [をインストールして、Azure PowerShell を構成する方法](powershell-install-configure.md) のこの記事に必要な前提条件です。

## Cloud Service のデプロイの一環としての診断拡張機能の有効化

こう診断拡張機能を有効にするシナリオの継続的な統合型の適切なのです。渡すことによって、クラウド サービスの展開の一部として、診断拡張機能を有効にすることができます、 *ExtensionConfiguration* パラメーターを [New-azuredeployment](https://msdn.microsoft.com/library/azure/mt589089.aspx) コマンドレットです。  *ExtensionConfiguration* パラメーターを使用して作成できる診断構成の配列を受け取る、 [新規 AzureServiceDiagnosticsExtensionConfig](https://msdn.microsoft.com/library/azure/mt589168.aspx) コマンドレットです。

次の例は、診断構成がそれぞれ異なる WebRole と WorkerRole を含む Cloud Service の診断を有効にする方法を示しています。

    $service_name = "MyService"
    $service_package = "CloudService.cspkg"
    $service_config = "ServiceConfiguration.Cloud.cscfg"
    $diagnostics_storagename = "myservicediagnostics"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml" 
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"
    
    $primary_storagekey = (Get-AzureStorageKey -StorageAccountName "$diagnostics_storagename").Primary
    $storage_context = New-AzureStorageContext -StorageAccountName $diagnostics_storagename -StorageAccountKey $primary_storagekey
    
    $webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -Storage_context $storageContext -DiagnosticsConfigurationPath $webrole_diagconfigpath
    $workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -StorageContext $storage_context -DiagnosticsConfigurationPath $workerrole_diagconfigpath
    
    
    New-AzureDeployment -ServiceName $service_name -Slot Production -Package $service_package -Configuration $service_config -ExtensionConfiguration @($webrole_diagconfig,$workerrole_diagconfig) 

## 既存の Cloud Service での診断拡張機能の有効化

使用することができます、 [Set-azureservicediagnosticsextension](https://msdn.microsoft.com/library/azure/mt589140.aspx) コマンドレットが既に実行されているクラウド サービスで診断を有効にします。


    $service_name = "MyService"
    $diagnostics_storagename = "myservicediagnostics"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml" 
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"
    $primary_storagekey = (Get-AzureStorageKey -StorageAccountName "$diagnostics_storagename").Primary
    $storage_context = New-AzureStorageContext -StorageAccountName $diagnostics_storagename -StorageAccountKey $primary_storagekey
    
    Set-AzureServiceDiagnosticsExtension -StorageContext $storage_context -DiagnosticsConfigurationPath $webrole_diagconfigpath -ServiceName $service_name -Slot Production -Role "WebRole" 
    Set-AzureServiceDiagnosticsExtension -StorageContext $storage_context -DiagnosticsConfigurationPath $workerrole_diagconfigpath -ServiceName $service_name -Slot Production -Role "WorkerRole"

## 診断拡張機能の現在の構成の取得

使用して、 [Get AzureServiceDiagnosticsExtension](https://msdn.microsoft.com/library/azure/mt589204.aspx) コマンドレットをクラウド サービスの現在の診断構成を取得します。

    Get-AzureServiceDiagnosticsExtension -ServiceName "MyService"

## 診断拡張機能の削除

診断を使用するクラウド サービスをオフにする、 [Remove-azureservicediagnosticsextension](https://msdn.microsoft.com/library/azure/mt589183.aspx) コマンドレットです。

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService"

*Role* パラメーターを指定せずに *Set-AzureServiceDiagnosticsExtension* または *New-AzureServiceDiagnosticsExtensionConfig* を使用して診断拡張機能を有効にした場合は、*Role* パラメーターを指定せずに *Remove-AzureServiceDiagnosticsExtension* を使用して拡張機能を削除できます。 拡張機能を有効にするときに *Role* パラメーターを使用した場合は、拡張機能を削除するときにもこのパラメーターを使用する必要があります。

個々のロールから診断拡張機能を削除するには、次のコマンドを実行します。

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService" -Role "WebRole"

## 次のステップ

- Azure 診断やその他の手法に関する問題のトラブルシューティングを使用した追加のガイダンスについては、次を参照してください。 [Azure クラウド サービスおよび仮想マシンで診断を有効にすると](cloud-services-dotnet-diagnostics.md)します。
- [診断構成スキーマ](https://msdn.microsoft.com/library/azure/dn782207.aspx) 診断拡張機能のさまざまな xml 構成オプションについて説明します。
- 仮想マシンの診断拡張機能を有効にする方法については、を参照してください [を監視し、Azure リソース マネージャーのテンプレートを使用して診断を Windows 仮想マシンを作成](virtual-machines-extensions-diagnostics-windows-template.md)。







