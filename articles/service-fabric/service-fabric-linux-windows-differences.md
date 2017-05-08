---
title: "Różnice w usłudze Azure Service Fabric w systemie Linux i Windows | Microsoft Docs"
description: "Różnice między usługą Azure Service Fabric w wersji zapoznawczej w systemie Linux a usługą Azure Service Fabric w systemie Windows."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: d552c8cd-67d1-45e8-91dc-871853f44fc6
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/23/2017
ms.author: subramar
ms.translationtype: Human Translation
ms.sourcegitcommit: e155891ff8dc736e2f7de1b95f07ff7b2d5d4e1b
ms.openlocfilehash: 00c1f10f7df865344233143ce9cdf546e64acca0
ms.contentlocale: pl-pl
ms.lasthandoff: 05/02/2017


---
# <a name="differences-between-service-fabric-on-linux-preview-and-windows-generally-available"></a>Różnice między usługą Service Fabric w systemie Linux (wersja zapoznawcza) i w systemie Windows (wersja ogólnie dostępna)

Ponieważ usługa Service Fabric w systemie Linux jest w wersji zapoznawczej, pewne funkcje, które są obsługiwane w systemie Windows, nie są jeszcze obsługiwane w systemie Linux. Gdy usługa Service Fabric w systemie Linux stanie się ogólnie dostępna, zestawy funkcji będą takie same.

* Niezawodne kolekcje (i niezawodne usługi stanowe) nie są obsługiwane w systemie Linux.
* Zwrotny serwer proxy nie jest dostępny w systemie Linux.
* Autonomiczny instalator nie jest dostępny w systemie Linux.
* Walidacja schematu XML dla plików manifestu nie jest wykonywana w systemie Linux. 
* Przekierowywanie konsoli nie jest obsługiwane w systemie Linux. 
* Usługa analizy błędów nie jest dostępna w systemie Linux.
* Obsługa usługi Azure Active Directory nie jest dostępna w systemie Linux.
* Niektóre polecenia interfejsu wiersza polecenia będące odpowiednikami poleceń programu PowerShell nie są dostępne.
* Względem klastra z systemem Linux można wykonywać tylko podzbiór poleceń programu PowerShell (więcej informacji na ten temat znajduje się w następnej sekcji).

>[!NOTE]
>Przekierowywanie konsoli nie jest obsługiwane w klastrach produkcyjnych, nawet w systemie Windows.

Narzędzia programistyczne używane w systemie Windows różnią się także od tych używanych w systemie Linux. W systemie Windows można korzystać z narzędzi VisualStudio, Powershell, VSTS i ETW, a w systemie Linux z narzędzi Yeoman, Eclipse, Jenkins i LTTng.

## <a name="powershell-cmdlets-that-do-not-work-against-a-linux-service-fabric-cluster"></a>Polecenia cmdlet programu PowerShell, które nie działają względem klastra usługi Service Fabric z systemem Linux

* Invoke-ServiceFabricChaosTestScenario
* Invoke-ServiceFabricFailoverTestScenario
* Invoke-ServiceFabricPartitionDataLoss
* Invoke-ServiceFabricPartitionQuorumLoss
* Restart-ServiceFabricPartition
* Start-ServiceFabricNode
* Stop-ServiceFabricNode
* Get-ServiceFabricImageStoreContent
* Get-ServiceFabricChaosReport
* Get-ServiceFabricNodeTransitionProgress
* Get-ServiceFabricPartitionDataLossProgress
* Get-ServiceFabricPartitionQuorumLossProgress
* Get-ServiceFabricPartitionRestartProgress
* Get-ServiceFabricTestCommandStatusList
* Remove-ServiceFabricTestState
* Start-ServiceFabricChaos
* Start-ServiceFabricNodeTransition
* Start-ServiceFabricPartitionDataLoss
* Start-ServiceFabricPartitionQuorumLoss
* Start-ServiceFabricPartitionRestart
* Stop-ServiceFabricChaos
* Stop-ServiceFabricTestCommand
* Cmd
* Get-ServiceFabricNodeConfiguration
* Get-ServiceFabricClusterConfiguration
* Get-ServiceFabricClusterConfigurationUpgradeStatus
* Get-ServiceFabricPackageDebugParameters
* New-ServiceFabricPackageDebugParameter
* New-ServiceFabricPackageSharingPolicy
* Add-ServiceFabricNode
* Copy-ServiceFabricClusterPackage
* Get-ServiceFabricRuntimeSupportedVersion
* Get-ServiceFabricRuntimeUpgradeVersion
* New-ServiceFabricCluster
* New-ServiceFabricNodeConfiguration
* Remove-ServiceFabricCluster
* Remove-ServiceFabricClusterPackage
* Remove-ServiceFabricNodeConfiguration
* Test-ServiceFabricClusterManifest
* Test-ServiceFabricConfiguration
* Update-ServiceFabricNodeConfiguration
* Approve-ServiceFabricRepairTask
* Complete-ServiceFabricRepairTask
* Get-ServiceFabricRepairTask
* Invoke-ServiceFabricDecryptText
* Invoke-ServiceFabricEncryptSecret
* Invoke-ServiceFabricEncryptText
* Invoke-ServiceFabricInfrastructureCommand
* Invoke-ServiceFabricInfrastructureQuery
* Remove-ServiceFabricRepairTask
* Start-ServiceFabricRepairTask
* Stop-ServiceFabricRepairTask
* Update-ServiceFabricRepairTaskHealthPolicy



## <a name="next-steps"></a>Następne kroki
* [Przygotowywanie środowiska projektowego w systemie Linux](service-fabric-get-started-linux.md)
* [Przygotowywanie środowiska projektowego w systemie OSX](service-fabric-get-started-mac.md)
* [Create and deploy your first Service Fabric Java application on Linux using Yeoman](service-fabric-create-your-first-linux-application-with-java.md) (Tworzenie i wdrażanie pierwszej aplikacji Java usługi Service Fabric w systemie Linux przy użyciu programu Yeoman)
* [Create and deploy your first Service Fabric Java application on Linux using Service Fabric Plugin for Eclipse](service-fabric-get-started-eclipse.md) (Tworzenie i wdrażanie pierwszej aplikacji Java usługi Service Fabric w systemie Linux przy użyciu wtyczki usługi Service Fabric dla środowiska Eclipse)
* [Create your first CSharp application on Linux](service-fabric-create-your-first-linux-application-with-csharp.md) (Tworzenie pierwszej aplikacji CSharp w systemie Linux)
* [Use the Azure CLI to manage your Service Fabric applications](service-fabric-azure-cli.md) (Używanie interfejsu wiersza polecenia platformy Azure do zarządzania aplikacjami usługi Service Fabric)

