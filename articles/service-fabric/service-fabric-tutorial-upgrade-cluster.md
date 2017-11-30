---
title: "Uaktualnij środowisko uruchomieniowe usługi sieć szkieletowa usług Azure | Dokumentacja firmy Microsoft"
description: "Informacje o sposobie uaktualniania środowiska uruchomieniowego klastra sieci szkieletowej usług hostowanymi na platformie Azure przy użyciu programu PowerShell."
services: service-fabric
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/28/2017
ms.author: adegeo
ms.openlocfilehash: 15acfbce3bde585ed2b39762b08733901133a3dd
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/29/2017
---
# <a name="upgrade-the-runtime-of-a-service-fabric-cluster"></a>Uaktualnienie środowiska uruchomieniowego klastra sieci szkieletowej usług

W tym samouczku jest częścią cztery serii i opisano sposób uaktualniania środowiska uruchomieniowego platformy Service Fabric w klastrze usługi sieć szkieletowa usług Azure. Tej części samouczek jest przeznaczony dla klastrów sieci szkieletowej usług działających na platformie Azure i nie ma zastosowania do hostowania samoobsługowego klastrów sieci szkieletowej usług.

> [!WARNING]
> Tej części samouczka wymaga środowiska PowerShell. Obsługa uaktualniania środowiska uruchomieniowego klastra nie jest jeszcze obsługiwana za pomocą narzędzi wiersza polecenia platformy Azure. Alternatywnie można uaktualnić klaster w portalu. Aby uzyskać więcej informacji, zobacz [uaktualnienia klastra usługi sieć szkieletowa usług Azure](service-fabric-cluster-upgrade.md).

Jeśli klaster jest już uruchomiona najnowsze środowisko uruchomieniowe usługi sieć szkieletowa, nie trzeba wykonać ten krok. W tym artykule można jednak zainstalować wszystkie obsługiwane środowisko uruchomieniowe w klastrze usługi sieć szkieletowa usług Azure.

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Wersja klastra do odczytu
> * Ustaw wersję klastra

## <a name="prerequisites"></a>Wymagania wstępne
Przed rozpoczęciem tego samouczka:
- Jeśli nie masz subskrypcji platformy Azure, Utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- Zainstaluj [programu Azure Powershell w wersji modułu 4.1 lub wyższej](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) lub [Azure CLI 2.0](/cli/azure/install-azure-cli).
- Tworzenie bezpiecznej [klastra systemu Windows](service-fabric-tutorial-create-vnet-and-windows-cluster.md) lub [klaster systemu Linux](service-fabric-tutorial-create-vnet-and-linux-cluster.md) na platformie Azure
- W przypadku wdrażania klastra systemu Windows, konfigurowanie środowiska projektowego systemu Windows. Zainstaluj [programu Visual Studio 2017](http://www.visualstudio.com) i **Azure programowanie**, **ASP.NET i sieć web development**, i **aplikacji dla wielu platform .NET Core**obciążeń.  Następnie skonfiguruj [środowiska programowania .NET](service-fabric-get-started.md).
- Jeśli w przypadku wdrażania klastra Linux należy skonfigurować środowisko projektowe Java na [Linux](service-fabric-get-started-linux.md) lub [MacOS](service-fabric-get-started-mac.md).  Zainstaluj [usługi sieci szkieletowej interfejsu wiersza polecenia](service-fabric-cli.md). 

### <a name="sign-in-to-azure"></a>Logowanie do platformy Azure
Zaloguj się na konto Azure Wybierz subskrypcję, przed wykonaniem polecenia platformy Azure.

```powershell
Login-AzureRmAccount
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

## <a name="get-the-runtime-version"></a>Pobierz wersję środowiska uruchomieniowego

Po podłączeniu do platformy Azure, należy wybrać subskrypcję, zawierającą klaster sieci szkieletowej usług, można uzyskać wersji środowiska uruchomieniowego klastra.

```powershell
Get-AzureRmServiceFabricCluster -ResourceGroupName SFCLUSTERTUTORIALGROUP -Name aztestcluster `
    | Select-Object ClusterCodeVersion
```

Lub tylko pobranie listy wszystkich klastrów w ramach subskrypcji z następujących czynności:

```powershell
Get-AzureRmServiceFabricCluster | Select-Object Name, ClusterCodeVersion
```

Uwaga **ClusterCodeVersion** wartości. Ta wartość będzie używana w następnej sekcji.

## <a name="upgrade-the-runtime"></a>Uaktualnienie środowiska uruchomieniowego

Należy użyć wartości **ClusterCodeVersion** z poprzedniej sekcji za `Get-ServiceFabricRuntimeUpgradeVersion` polecenia cmdlet, aby dowiedzieć się, jakie wersje są dostępne do uaktualnienia do. To polecenie cmdlet można uruchamiać tylko na komputerze, który został połączony z Internetem. Na przykład, jeśli chcesz zobaczyć wersji środowiska uruchomieniowego można uaktualnienia do wersji `5.7.198.9494`, użyj następującego polecenia:

```powershell
Get-ServiceFabricRuntimeUpgradeVersion -BaseVersion "5.7.198.9494"
```

Listę wersji można określić klastra usługi sieć szkieletowa usług Azure do uaktualnienia do nowszej środowiska wykonawczego. Na przykład jeśli wersja `6.0.219.9494` można uaktualnić do, użyj następującego polecenia w celu uaktualnienia klastra.

```powershell
Set-AzureRmServiceFabricUpgradeType -ResourceGroupName SFCLUSTERTUTORIALGROUP `
                                    -Name aztestcluster `
                                    -UpgradeMode Manual `
                                    -Version "6.0.219.9494"
```

> [!IMPORTANT]
> Uaktualnienie środowiska uruchomieniowego klastra może potrwać bardzo długo. PowerShell jest blokowane podczas uaktualniania. Aby sprawdzić stan uaktualnienia, można użyć innej sesji programu PowerShell.

Stan uaktualnienia można monitorować przy użyciu albo programu PowerShell lub `sfctl` interfejsu wiersza polecenia.

Najpierw połącz się z klastrem przy użyciu certyfikatu SSL utworzony w pierwszej części samouczka. Użyj `Connect-ServiceFabricCluster` polecenia cmdlet lub `sfctl cluster upgrade-status`.

```powershell
$endpoint = "<mycluster>.southcentralus.cloudapp.azure.com:19000"
$thumbprint = "63EB5BA4BC2A3BADC42CA6F93D6F45E5AD98A1E4"

Connect-ServiceFabricCluster -ConnectionEndpoint $endpoint `
                             -KeepAliveIntervalInSec 10 `
                             -X509Credential -ServerCertThumbprint $thumbprint `
                             -FindType FindByThumbprint -FindValue $thumbprint `
                             -StoreLocation CurrentUser -StoreName My
```

```azurecli
sfctl cluster select --endpoint https://aztestcluster.southcentralus.cloudapp.azure.com:19080 \
--pem ./aztestcluster201709151446.pem --no-verify
```

Następnie użyj `Get-ServiceFabricClusterUpgrade` lub `sfctl cluster upgrade-status` Aby wyświetlić stan. Podobny do następującego wyniku jest wyświetlana.

```powershell
Get-ServiceFabricClusterUpgrade

TargetCodeVersion                          : 6.0.219.9494
TargetConfigVersion                        : 3
StartTimestampUtc                          : 11/28/2017 3:09:48 AM
UpgradeState                               : RollingForwardPending
UpgradeDuration                            : 00:09:00
CurrentUpgradeDomainDuration               : 00:09:00
NextUpgradeDomain                          : 1
UpgradeDomainsStatus                       : { "0" = "Completed";
                                             "1" = "Pending";
                                             "2" = "Pending";
                                             "3" = "Pending";
                                             "4" = "Pending" }
UpgradeKind                                : Rolling
RollingUpgradeMode                         : Monitored
FailureAction                              : Rollback
ForceRestart                               : False
UpgradeReplicaSetCheckTimeout              : 37201.09:59:01
HealthCheckWaitDuration                    : 00:05:00
HealthCheckStableDuration                  : 00:05:00
HealthCheckRetryTimeout                    : 00:45:00
UpgradeDomainTimeout                       : 02:00:00
UpgradeTimeout                             : 12:00:00
ConsiderWarningAsError                     : False
MaxPercentUnhealthyApplications            : 0
MaxPercentUnhealthyNodes                   : 100
ApplicationTypeHealthPolicyMap             : {}
EnableDeltaHealthEvaluation                : True
MaxPercentDeltaUnhealthyNodes              : 0
MaxPercentUpgradeDomainDeltaUnhealthyNodes : 0
ApplicationHealthPolicyMap                 : {}
```

```azurecli
sfctl cluster upgrade-status

{
  "codeVersion": "6.0.219.9494",
  "configVersion": "3",

... item cut to save space ...

  },
  "upgradeDomains": [
    {
      "name": "0",
      "state": "Completed"
    },
    {
      "name": "1",
      "state": "Pending"
    },
    {
      "name": "2",
      "state": "Pending"
    },
    {
      "name": "3",
      "state": "Pending"
    },
    {
      "name": "4",
      "state": "Pending"
    }
  ],
  "upgradeDurationInMilliseconds": "PT1H2M4.63889S",
  "upgradeState": "RollingForwardPending"
}
```

## <a name="conclusion"></a>Podsumowanie
W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Pobierz wersję środowiska uruchomieniowego klastra
> * Uaktualnienie środowiska uruchomieniowego klastra
> * Monitor uaktualnienia
