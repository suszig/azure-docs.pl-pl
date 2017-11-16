---
title: "Następne kroki tworzenia projektu sieci szkieletowej usług | Dokumentacja firmy Microsoft"
description: "Ten artykuł zawiera łącza do zestawu zadań związanych z projektowaniem core dla sieci szkieletowej usług"
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 299d1f97-1ca9-440d-9f81-d1d0dd2bf4df
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/28/2017
ms.author: rwike77
ms.openlocfilehash: e04f9e57c65da42da73a5ee6a0b601dcbb318aaa
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/14/2017
---
# <a name="your-service-fabric-application-and-next-steps"></a>Sieć szkieletowa usług aplikacji i następne kroki
Utworzono aplikację sieci szkieletowej usług Azure. W tym artykule opisano w skład projektu i potencjalne poniższe kroki.

## <a name="your-application"></a>Aplikacja
Każdej nowej aplikacji zawiera projekt aplikacji. Może to być jeden lub dwa dodatkowe projekty, w zależności od typu wybranych usług.

### <a name="the-application-project"></a>Projekt aplikacji
Projekt aplikacji obejmuje:

* Zestaw odwołania do usług, które składają się na aplikację.
* Profilów (węzła 1 lokalnego, 5 węzła lokalnego i chmura) używanych do obsługi preferencje dotyczące pracy z różnych środowiskach — takie jak preferencje dotyczące punktu końcowego klastra oraz czy można wykonywać wdrożenia uaktualnienia domyślnie trzy publikowania.
* Parametr aplikacji trzy pliki (taki sam jak powyżej) można do obsługi konfiguracje specyficzne dla środowiska aplikacji, takie jak liczba partycji można utworzyć dla usługi.
* Skrypt wdrożenia, który służy do wdrażania aplikacji z poziomu wiersza polecenia lub w ramach automatycznych ciągłego potoku integracji i wdrażania.
* Manifest aplikacji zawiera opis aplikacji. Manifest można znaleźć w folderze ApplicationPackageRoot.

### <a name="stateless-service"></a>Usługi bezstanowej
Po dodaniu nowej usługi bezstanowej Visual Studio dodaje projekt do rozwiązania, który zawiera typ podrzędne w stosunku `StatelessService`. Usługa zwiększa zmienną lokalną w licznika.

### <a name="stateful-service"></a>Usługi stanowej
Po dodaniu nowej usługi stanowej Visual Studio dodaje projekt do rozwiązania, który zawiera typ podrzędne w stosunku `StatefulService`. Usługa zwiększa licznik w jego `RunAsync` — metoda i przechowuje wyniki w `ReliableDictionary`.

### <a name="actor-service"></a>Usługa aktora
Po dodaniu nowego niezawodnego aktora Visual Studio dodaje dwa projekty do rozwiązania: aktora projektu i projektu interfejsu.

Projektu aktora zapewnia metody ustawiania i pobierania wartości licznika, który jest niezawodnie utrwalone w ramach stanu aktora. Projekt interfejsu udostępnia interfejs, który inne usługi można użyć do wywołania aktora.

### <a name="stateless-web-api"></a>Bezstanowe interfejsu API sieci Web
Bezstanowych projekt interfejsu API sieci Web udostępnia usługi sieci web podstawowe, który można otworzyć aplikację dla klientów zewnętrznych. Aby uzyskać więcej informacji na temat projektu strukturę, zobacz [usług interfejsu API sieci Web sieci szkieletowej usług za pomocą hostingu samodzielnego OWIN](service-fabric-reliable-services-communication-webapi.md).


### <a name="aspnet-core"></a>Platformy ASP.NET core
Zestaw SDK sieci szkieletowej usług zawiera ten sam zestaw platformy ASP.NET Core szablonów, które są dostępne dla projektów platformy ASP.NET Core autonomicznej: pusta, [interfejsu API sieci Web][aspnet-webapi], i [aplikacji sieci Web][aspnet-webapp].

### <a name="guest-executables-and-guest-containers"></a>Gość kontenerów plików wykonywalnych i gościa

Usługa sieci szkieletowej "Gość" to usługa, która jest kompilowany bez modele programowania dla platformy. Pliki binarne gościa można spakować albo [bezpośrednio w pakiecie aplikacji](service-fabric-deploy-existing-app.md) lub [za pośrednictwem obrazu kontenera](service-fabric-deploy-container.md). W obu przypadkach program Visual Studio tworzy artefakty niezbędne w **ApplicationPackageRoot** folderu projektu aplikacji. Visual Studio nie utworzy nowy projekt usługi, ponieważ kod już istnieje w innym miejscu. Jeśli chcesz zarządzać projektów gościa równolegle z projektu aplikacji sieci szkieletowej usług, możesz dodać je do tego samego rozwiązania Visual Studio.

## <a name="next-steps"></a>Następne kroki
### <a name="create-an-azure-cluster"></a>Tworzenie klastra platformy Azure
Zestaw SDK sieci szkieletowej usług zapewnia klaster lokalny projektowania i testowania. Aby utworzyć klaster na platformie Azure, zobacz [konfigurowania klastra sieci szkieletowej usług w portalu Azure][create-cluster-in-portal].

### <a name="publish-your-application-to-azure"></a>Publikowanie aplikacji Azure
Można opublikować aplikacji bezpośrednio z programu Visual Studio z klastrem platformy Azure. Aby dowiedzieć się więcej, zobacz temat [publikowania aplikacji na platformie Azure][publish-app-to-azure].

### <a name="use-service-fabric-explorer-to-visualize-your-cluster"></a>Wizualizowanie klastra za pomocą Eksploratora sieci szkieletowej usług
Service Fabric Explorer oferuje prosty sposób na wizualizowanie klastra, w tym wdrożone aplikacje i fizycznego układu. Aby dowiedzieć się więcej, zobacz [wizualizacja klastra przy użyciu Eksploratora usługi sieć szkieletowa][visualize-with-sfx].

### <a name="version-and-upgrade-your-services"></a>Wersja i uaktualniania usług
Sieć szkieletowa usług umożliwia niezależnie od wersji i uaktualniania usług niezależne w aplikacji. Aby dowiedzieć się więcej, zobacz [wersji i uaktualniania usług][app-upgrade-tutorial].

### <a name="configure-continuous-integration-with-visual-studio-team-services"></a>Konfigurowanie ciągłej integracji z Visual Studio Team Services
Aby dowiedzieć się, jak można skonfigurować procesu ciągłej integracji aplikacji usługi Service Fabric, zobacz [skonfigurować ciągłe integrację z programem Visual Studio Team Services][ci-with-vso].

<!-- Links -->
[add-web-frontend]: service-fabric-add-a-web-frontend.md
[create-cluster-in-portal]: service-fabric-cluster-creation-via-portal.md
[publish-app-to-azure]: service-fabric-manage-application-in-visual-studio.md
[visualize-with-sfx]: service-fabric-visualizing-your-cluster.md
[ci-with-vso]: service-fabric-set-up-continuous-integration.md
[reliable-services-webapi]: service-fabric-reliable-services-communication-webapi.md
[app-upgrade-tutorial]: service-fabric-application-upgrade-tutorial.md
[aspnet-webapi]: https://docs.asp.net/en/latest/tutorials/first-web-api.html
[aspnet-webapp]: https://docs.asp.net/en/latest/tutorials/first-mvc-app/index.html
