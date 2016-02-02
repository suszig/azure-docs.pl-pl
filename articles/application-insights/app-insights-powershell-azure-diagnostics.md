<properties
    pageTitle="PowerShell を使用した Application Insights への Azure 診断の送信 | Microsoft Azure"
    description="Application Insights にパイプするための Azure 診断の構成を自動化します。"
    services="application-insights"
    documentationCenter=".net"
    authors="sbtron"
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="11/17/2015"
    ms.author="awills"/>


# PowerShell を使用した Application Insights への Azure 診断の送信

[Microsoft Azure](https://azure.com) できる [Azure 診断を送信するよう構成](app-insights-azure-diagnostics.md) に [Visual Studio Application Insights](app-insights-overview.md)します。 診断は、Azure Cloud Services および Azure VM に関するものです。 このデータは、Application Insights SDK を使用するアプリケーション内から送信されるテレメトリを補完します。 Azure での新規リソース作成プロセスを自動化する一部として、PowerShell を使用して診断を構成できます。

## Cloud Service のデプロイの一環としての診断拡張機能の有効化

`New-azuredeployment` コマンドレットは、パラメーターを持つ `ExtensionConfiguration`, 、診断構成の配列を受け取る。 これらを使用して作成することができます、 `新規 AzureServiceDiagnosticsExtensionConfig` コマンドレットです。 次に例を示します。

```ps

    $service_package = "CloudService.cspkg"
    $service_config = "ServiceConfiguration.Cloud.cscfg"
    $diagnostics_storagename = "myservicediagnostics"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml" 
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

    $primary_storagekey = (Get-AzureStorageKey `
     -StorageAccountName "$diagnostics_storagename").Primary
    $storage_context = New-AzureStorageContext `
       -StorageAccountName $diagnostics_storagename `
       -StorageAccountKey $primary_storagekey

    $webrole_diagconfig = `
     New-AzureServiceDiagnosticsExtensionConfig `
      -Role "WebRole" -Storage_context $storageContext `
      -DiagnosticsConfigurationPath $webrole_diagconfigpath
    $workerrole_diagconfig = `
     New-AzureServiceDiagnosticsExtensionConfig `
      -Role "WorkerRole" `
      -StorageContext $storage_context `
      -DiagnosticsConfigurationPath $workerrole_diagconfigpath

    New-AzureDeployment `
      -ServiceName $service_name `
      -Slot Production `
      -Package $service_package `
      -Configuration $service_config `
      -ExtensionConfiguration @($webrole_diagconfig,$workerrole_diagconfig)
```

## 既存の Cloud Service での診断拡張機能の有効化

既存のサービスで使用して `Set-azureservicediagnosticsextension`します。

```ps

    $service_name = "MyService"
    $diagnostics_storagename = "myservicediagnostics"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml" 
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"
    $primary_storagekey = (Get-AzureStorageKey `
         -StorageAccountName "$diagnostics_storagename").Primary
    $storage_context = New-AzureStorageContext `
        -StorageAccountName $diagnostics_storagename `
        -StorageAccountKey $primary_storagekey

    Set-AzureServiceDiagnosticsExtension `
        -StorageContext $storage_context `
        -DiagnosticsConfigurationPath $webrole_diagconfigpath `
        -ServiceName $service_name `
        -Slot Production `
        -Role "WebRole" 
    Set-AzureServiceDiagnosticsExtension `
        -StorageContext $storage_context `
        -DiagnosticsConfigurationPath $workerrole_diagconfigpath `
        -ServiceName $service_name `
        -Slot Production `
        -Role "WorkerRole"
```

## 診断拡張機能の現在の構成の取得

```ps

    Get-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```


## 診断拡張機能の削除

```ps

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```

いずれかを使用して診断拡張機能を有効にした場合 `Set-azureservicediagnosticsextension` または `新規 AzureServiceDiagnosticsExtensionConfig` ロール パラメーターを指定しないことを削除する拡張機能を使用して、 `Remove-azureservicediagnosticsextension` ロール パラメーターがない場合。 拡張機能を有効にするときに Role パラメーターを使用した場合は、拡張機能を削除するときにもこのパラメーターを使用する必要があります。

個々のロールから診断拡張機能を削除するには、次のコマンドを実行します。

```ps

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService" -Role "WebRole"
```


## 関連トピック

* [Azure 監視クラウド サービスの Application Insights でのアプリ](app-insights-cloudservices.md)
* [Azure 診断を Application Insights に送信します。](app-insights-azure-diagnostics.md)






