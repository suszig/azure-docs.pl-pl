---
title: "Konfigurowanie autonomicznego klastra usługi Azure Service Fabric | Microsoft Docs"
description: "Utwórz autonomiczny klaster programowania z trzema węzłami działającymi na tym samym komputerze. Po ukończeniu tej konfiguracji wszystko będzie gotowe do tworzenia klastra dla wielu maszyn."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/11/2017
ms.author: ryanwi
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: 6da8b21014966edd9f4cea0fd27f6973b2b820f0
ms.lasthandoff: 04/12/2017


---

# <a name="create-your-first-service-fabric-standalone-cluster"></a>Tworzenie pierwszego autonomicznego klastra usługi Service Fabric
Autonomiczny klaster usługi Service Fabric można utworzyć na wszystkich maszynach wirtualnych lub komputerach z systemem Windows Server 2012 R2 lub Windows Server 2016 lokalnie lub w chmurze. Ten przewodnik szybkiego startu pomaga utworzyć autonomiczny klaster programowania w ciągu zaledwie kilku minut.  Po zakończeniu na pojedynczym komputerze będzie dostępny klaster z trzema węzłami, do którego można wdrażać aplikacje.

## <a name="before-you-begin"></a>Przed rozpoczęciem
Usługa Service Fabric udostępnia pakiet instalacyjny służący do tworzenia autonomicznych klastrów usługi Service Fabric.  [Pobierz pakiet instalacyjny](http://go.microsoft.com/fwlink/?LinkId=730690).  Rozpakuj go do folderu, na przykład *C:\temp\Microsoft.Azure.ServiceFabric.WindowsServer*, na komputerze lub maszynie wirtualnej, na której zostanie skonfigurowany klaster programowania.  Zawartość pakietu instalacyjnego została szczegółowo opisana [tutaj](service-fabric-cluster-standalone-package-contents.md).

Administrator klastra wdrażający i konfigurujący klaster musi mieć uprawnienia administratora na komputerze. Usługi Service Fabric nie można zainstalować na kontrolerze domeny.

## <a name="validate-the-environment"></a>Weryfikowanie środowiska
Skrypt *TestConfiguration.ps1* w pakiecie autonomicznym jest używany jako analizator najlepszych rozwiązań, który weryfikuje, czy można wdrożyć klaster w danym środowisku. Listę wymagań wstępnych i wymagań dotyczących środowiska można znaleźć w temacie [Deployment preparation](service-fabric-cluster-standalone-deployment-preparation.md) (Przygotowywanie środowiska). Uruchom skrypt, aby zweryfikować, czy można utworzyć klaster programowania:

```powershell
.\TestConfiguration.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json
```
## <a name="create-the-cluster"></a>Tworzenie klastra
W ramach pakietu instalacyjnego jest instalowanych kilka przykładowych plików konfiguracji klastra. *ClusterConfig.Unsecure.DevCluster.json* to najprostsza konfiguracja klastra: niezabezpieczony klaster z trzema węzłami działającymi na pojedynczym komputerze. Na potrzeby tego samouczka nie trzeba modyfikować żadnego z domyślnych ustawień konfiguracji.  Inne pliki konfiguracji zawierają opis klastrów dla jednej lub wielu maszyn, zabezpieczonych za pomocą certyfikatów X.509 lub zabezpieczeń systemu Windows.  Dalsze informacje na temat zabezpieczeń klastra usługi Service Fabric można znaleźć w temacie [Secure a cluster](service-fabric-cluster-security.md) (Zabezpieczanie klastra). 

W celu utworzenia klastra programowania z trzema węzłami uruchom skrypt *CreateServiceFabricCluster.ps1* w sesji programu PowerShell administratora:

```powershell
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json -AcceptEULA
```

Pakiet środowiska uruchomieniowego usługi Service Fabric jest automatyczne pobierany i instalowany podczas tworzenia klastra.

## <a name="connect-to-the-cluster"></a>Łączenie z klastrem
Klaster programowania z trzema węzłami został uruchomiony. Moduł ServiceFabric programu PowerShell został zainstalowany w środowisku uruchomieniowym.  Można sprawdzić, czy w środowisku uruchomieniowym usługi Service Fabric klaster działa na tym samym komputerze, czy też na komputerze zdalnym.  Polecenie cmdlet [Connect-ServiceFabricCluster](/powershell/module/ServiceFabric/Connect-ServiceFabricCluster) umożliwia ustanowienie połączenia z klastrem.   

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint localhost:19000
```
Inne przykłady łączenia z klastrem można znaleźć w temacie [Nawiązywanie połączenia z zabezpieczonym klastrem](service-fabric-connect-to-secure-cluster.md). Po nawiązaniu połączenia z klastrem użyj polecenia cmdlet [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode), aby wyświetlić listę węzłów w klastrze oraz informacje o stanie każdego węzła. Element **HealthState** powinien mieć wartość *OK* dla każdego węzła.

```powershell
PS C:\temp\Microsoft.Azure.ServiceFabric.WindowsServer> Get-ServiceFabricNode |Format-Table

NodeDeactivationInfo NodeName IpAddressOrFQDN NodeType  CodeVersion ConfigVersion NodeStatus NodeUpTime NodeDownTime HealthState
-------------------- -------- --------------- --------  ----------- ------------- ---------- ---------- ------------ -----------
                     vm2      localhost       NodeType2 5.5.216.0   0                     Up 03:00:07   00:00:00              Ok
                     vm1      localhost       NodeType1 5.5.216.0   0                     Up 03:00:02   00:00:00              Ok
                     vm0      localhost       NodeType0 5.5.216.0   0                     Up 03:00:01   00:00:00              Ok
```

## <a name="visualize-the-cluster-using-service-fabric-explorer"></a>Wizualizowanie klastra za pomocą narzędzia Service Fabric Explorer
[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) to odpowiednie narzędzie do wizualizowania klastra i zarządzania aplikacjami.  Service Fabric Explorer to usługa, która działa w klastrze otwieranym w przeglądarce po przejściu do adresu [http://localhost:19080/Explorer](http://localhost:19080/Explorer). 

Pulpit nawigacyjny klastra zawiera omówienie klastra, w tym podsumowanie kondycji węzła i aplikacji. Widok węzła przedstawia fizyczny układ klastra. Dla danego węzła można sprawdzić, które aplikacje mają kod wdrożony w tym węźle.

![Service Fabric Explorer][service-fabric-explorer]

## <a name="remove-the-cluster"></a>Usuwanie klastra
Aby usunąć klaster, uruchom skrypt *RemoveServiceFabricCluster.ps1* programu PowerShell z folderu pakietu i przekaż ścieżkę do pliku konfiguracji JSON. Możesz opcjonalne określić lokalizację dziennika usuwania.

```powershell
# Removes Service Fabric cluster nodes from each computer in the configuration file.
.\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json -Force
```

Aby usunąć środowisko uruchomieniowe usługi Service Fabric z komputera, uruchom poniższy skrypt programu PowerShell z folderu pakietu.

```powershell
# Removes Service Fabric from the current computer.
.\CleanFabric.ps1
```

## <a name="next-steps"></a>Następne kroki
Teraz po skonfigurowaniu autonomicznego klastra programowania możesz spróbować wykonać następujące czynności:
* [Konfigurowanie autonomicznego klastra dla wielu maszyn](service-fabric-cluster-creation-for-windows-server.md) i włączanie zabezpieczeń
* [Wdrażanie aplikacji przy użyciu programu PowerShell](service-fabric-deploy-remove-applications.md)

[service-fabric-explorer]: ./media/service-fabric-get-started-standalone-cluster/sfx.png
