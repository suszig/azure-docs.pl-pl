---
title: "Różnice w usłudze Azure Service Fabric w systemie Linux i Windows | Microsoft Docs"
description: "Różnice między usługą Azure Service Fabric w systemie Linux a usługą Azure Service Fabric w systemie Windows."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: d552c8cd-67d1-45e8-91dc-871853f44fc6
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/23/2018
ms.author: subramar
ms.openlocfilehash: 0a8dd9936268a4e4cb8b744a87111c3bbf3f062d
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/02/2018
---
# <a name="differences-between-service-fabric-on-linux-and-windows"></a>Różnice między usługą Service Fabric w systemach Linux i Windows

Pewne funkcje, które są obsługiwane w systemie Windows, nie są jeszcze obsługiwane w systemie Linux. Po pewnym czasie zestawy funkcji będą działać w obydwu systemach i w każdej nowej wersji ta różnica między funkcjami będzie się zmniejszać. Oto różnice między najnowszymi dostępnymi wersjami (wersją 6.0 w systemie Windows i wersją 6.0 w systemie Linux): 

* Wszystkie modele programowania są w wersji zapoznawczej (Java/C# — Reliable Actors, Reliable Stateless Services i Reliable Stateful Services)
* Usługa Envoy (ReverseProxy) jest dostępna w wersji zapoznawczej w systemie Linux
* Autonomiczny instalator dla systemu Linux nie jest dostępny w systemie Linux
* Przekierowywanie konsoli (nieobsługiwane w klastrach produkcyjnych w systemie Linux ani Windows)
* Usługa analizy błędów w systemie Linux
* Usługa DNS dla usług Service Fabric (usługa DNS jest obsługiwana dla kontenerów w systemie Linux)
* Odpowiedniki poleceń interfejsu wiersza polecenia dla niektórych poleceń programu PowerShell (lista znajduje się poniżej — większość z nich ma zastosowanie tylko do klastrów autonomicznych)

Narzędzia programistyczne używane w systemie Windows różnią się także od tych używanych w systemie Linux. W systemie Windows można korzystać z narzędzi Visual Studio, PowerShell, VSTS i ETW, a w systemie Linux z narzędzi Yeoman, Eclipse, Jenkins i LTTng.

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



## <a name="next-steps"></a>Kolejne kroki
* [Przygotowywanie środowiska projektowego w systemie Linux](service-fabric-get-started-linux.md)
* [Przygotowywanie środowiska projektowego w systemie OSX](service-fabric-get-started-mac.md)
* [Create and deploy your first Service Fabric Java application on Linux using Yeoman](service-fabric-create-your-first-linux-application-with-java.md) (Tworzenie i wdrażanie pierwszej aplikacji Java usługi Service Fabric w systemie Linux przy użyciu programu Yeoman)
* [Create and deploy your first Service Fabric Java application on Linux using Service Fabric Plugin for Eclipse](service-fabric-get-started-eclipse.md) (Tworzenie i wdrażanie pierwszej aplikacji Java usługi Service Fabric w systemie Linux przy użyciu wtyczki usługi Service Fabric dla środowiska Eclipse)
* [Create your first CSharp application on Linux](service-fabric-create-your-first-linux-application-with-csharp.md) (Tworzenie pierwszej aplikacji CSharp w systemie Linux)
* [Use the Service Fabric CLI to manage your applications](service-fabric-application-lifecycle-sfctl.md) (Zarządzanie aplikacjami przy użyciu interfejsu wiersza polecenia usługi Service Fabric)
