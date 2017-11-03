---
title: "Omówienie sieci szkieletowej usług na platformie Azure | Dokumentacja firmy Microsoft"
description: "Przegląd sieci szkieletowej usług, w którym aplikacje składają się z wielu mikrousług w celu zapewnienia skalowalności i odporności. Sieć szkieletowa usług to platforma systemów rozproszonych używany do tworzenia skalowalnych, niezawodne i łatwe zarządzane aplikacje dla chmury."
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: masnider
ms.assetid: bbcc652a-a790-4bc4-926b-e8cd966587c0
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: overview
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/20/2017
ms.author: msfussell
ms.custom: mvc
ms.openlocfilehash: 8ff0d38a679b673b148dd808050eda82060cfe80
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="overview-of-azure-service-fabric"></a>Omówienie usługi Azure Service Fabric
Usługa Azure Service Fabric to platforma systemów rozproszonych ułatwiająca pakowanie i wdrażanie skalowalnych i niezawodnych mikrousług i kontenerów oraz zarządzanie nimi. Sieć szkieletowa usług dotyczy również istotne trudności w tworzenie i zarządzanie natywnych aplikacji w chmurze. Deweloperzy i administratorzy mogą uniknąć złożonych problemów związanych z infrastrukturą i skoncentrować się na implementowaniu wymagających obciążeń o znaczeniu strategicznym, które są skalowalne, niezawodne i łatwe w zarządzaniu. Sieć szkieletowa usług reprezentuje Platforma następnej generacji do tworzenia i zarządzania te klasy korporacyjnej, warstwy 1, skali chmury aplikacji uruchomionych w kontenerach.

Ten krótki film pokazuje sieci szkieletowej usług i mikrousług:<center><a target="_blank" href="https://aka.ms/servicefabricvideo">  
<img src="./media/service-fabric-overview/OverviewVid.png" WIDTH="360" HEIGHT="244">  
</a></center>

## <a name="applications-composed-of-microservices"></a>Aplikacje składa się z mikrousług 
Sieć szkieletowa usług umożliwia tworzenie i zarządzanie nimi skalowalne i niezawodne aplikacje składa się z mikrousług, które są uruchamiane na o wysokiej gęstości współużytkowanej puli maszyn, które są określone jako klaster. Zapewnia zaawansowany, lekkie środowisko uruchomieniowe do tworzenia rozproszonych, skalowalnych i bezstanowe i stanowe mikrousług uruchomione w kontenerach. Udostępnia również możliwości zarządzania kompleksowej aplikacji do udostępniania, wdrażania, monitorowania, uaktualnienie/poprawki i usunąć wdrożonych aplikacji, takich jak konteneryzowanych usługi.

Sieć szkieletowa usług obsługuje wiele usług firmy Microsoft już dziś, łącznie z bazy danych SQL Azure, Azure DB rozwiązania Cosmos, Cortana, Microsoft Power BI, Microsoft Intune, Azure Event Hubs, Centrum IoT Azure, Dynamics 365, Skype dla firm i wiele podstawowych usług platformy Azure.

Sieć szkieletowa usług jest dostosowane do utworzenia chmury macierzysty usług, które można włączyć mały, zgodnie z potrzebami i powiększania na ogromną skalę z setkami lub tysiącami maszyn.

Współczesne Internet skali usługi są wbudowane z mikrousług. Mikrousług przykłady bramy protokołu, profile użytkowników, zakupy wózki spisu przetwarzania, kolejek i umieszcza w pamięci podręcznej. Sieć szkieletowa usług to platforma mikrousług, która zapewnia unikatową nazwę, która może być bezstanowe i stanowe co mikrousługi (lub kontenera).

Sieć szkieletowa usług zapewnia kompleksowe środowisko wykonawcze i cyklem życia możliwości zarządzania dla aplikacji, które składają się z tymi mikrousług. Obsługuje mikrousług wewnątrz kontenerów, które są wdrożone i aktywowana w ramach klastra sieci szkieletowej usług. Przenoszenie z maszyn wirtualnych do kontenerów umożliwia zwiększenie kolejności o wielkości w gęstości. Podobnie innego rzędu w gęstość staje się możliwe, po przeniesieniu z kontenerów do mikrousług w tych kontenerach. Na przykład jeden klaster bazy danych SQL Azure obejmuje setki komputerów z systemem dziesiątki tysięcy kontenery hostujących łącznie setki tysięcy baz danych. Każda baza danych jest mikrousługi stanowe sieci szkieletowej usług. 

Aby uzyskać więcej informacji na temat podejścia mikrousług przeczytaj [Dlaczego podejście mikrousług do tworzenia aplikacji?](service-fabric-overview-microservices.md)

## <a name="container-deployment-and-orchestration"></a>Wdrażanie kontenera i aranżacji
Sieć szkieletowa usług to firmy Microsoft [orchestrator kontenera](service-fabric-cluster-resource-manager-introduction.md) wdrażanie mikrousług w klastrze maszyn. Mikrousług mogą być opracowane na wiele sposobów korzystania z [usługi sieć szkieletowa modele programowania](service-fabric-choose-framework.md), [platformy ASP.NET Core](service-fabric-reliable-services-communication-aspnetcore.md), do wdrażania [żadnego kodu wybór](service-fabric-deploy-existing-app.md). Ważne można mieszać zarówno usług w procesach i usług w kontenerach w tej samej aplikacji. Jeśli chcesz, aby [wdrażania i zarządzania nimi kontenery](service-fabric-containers-overview.md), sieć szkieletowa usług to doskonałe jako orchestrator kontenera.

## <a name="any-os-any-cloud"></a>Dowolnego systemu operacyjnego, wszelkie chmury
Sieć szkieletowa usług działa wszędzie. Klastrów można utworzyć dla sieci szkieletowej usług w wielu środowiskach, w tym na platformie Azure lub lokalnie, w systemie Windows Server lub w systemie Linux. Można nawet utworzyć klastry na innych chmur publicznych. Ponadto środowisko projektowe w zestawie SDK to **identyczne** do środowiska produkcyjnego, z żadnych emulatorów związane. Innymi słowy co działa w klastrze lokalnym programowanie wdraża klastry w innych środowiskach.

![Platforma sieci szkieletowej usług][Image1]

Dla systemu Windows dla deweloperów zestawu .NET SDK usługi sieci szkieletowej jest zintegrowany z programu Visual Studio i środowiska Powershell. Zobacz [przygotowywanie środowiska projektowego w systemie Windows](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-get-started.md). Rozwoju systemu Linux zestawu Java SDK usługi sieci szkieletowej jest zintegrowany z Eclipse, a narzędzia Yeoman służy do generowania szablonów dla języka Java, .NET Core i kontenera aplikacji. Zobacz [przygotowywanie środowiska projektowego w systemie Linux](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-get-started.md)

Aby uzyskać więcej informacji na temat tworzenia klastrów, przeczytaj [tworzenia klastra w systemie Windows Server lub Linux](service-fabric-deploy-anywhere.md) lub na platformie Azure, tworzenia klastra [za pośrednictwem portalu Azure](service-fabric-cluster-creation-via-portal.md).

## <a name="stateless-and-stateful-microservices-for-service-fabric"></a>Bezstanowe i stanowe mikrousług dla sieci szkieletowej usług
Sieć szkieletowa usług umożliwia tworzenie aplikacji, które składają się z mikrousług lub kontenerów. Bezstanowe mikrousług (takie jak protokół bramy i serwerów proxy sieci web) nie obsługują modyfikowalną stanu poza żądania i odpowiedzi z usługi. Role procesów roboczych usługi w chmurze Azure są przykładem bezstanowego usługi. Stanowe mikrousług (na przykład konta użytkowników, baz danych, urządzeń, wózkach sklepowych zakupów i kolejek) Obsługa modyfikowalna, autorytatywnych stan żądania i odpowiedzi. Współczesnych aplikacji internetowych skali składają się z kombinacji usług bezstanowych i stanowych mikrousług. 

Klucza zróżnicowanie z sieci szkieletowej usług jest jego skoncentrowanie się na tworzeniu usługi stanowej, za pomocą [wbudowanych modele programowania ](service-fabric-choose-framework.md) lub konteneryzowanych usług stanowych. [Scenariuszy aplikacji](service-fabric-application-scenarios.md) opisano scenariusze, w których są używane usługi stanowej.


## <a name="application-lifecycle-management"></a>Zarządzanie cyklem życia aplikacji
Sieć szkieletowa usług zapewnia obsługę aplikacji pełny cykl życia i CI/CD, łącznie z kontenerów aplikacji w chmurze. Tego cyklu obejmuje programowanie za pomocą wdrażania, zarządzania infrastrukturą i konserwacji do likwidacji ostatecznego.

Możliwości zarządzania cyklem życia aplikacji usługi sieć szkieletowa umożliwić administratorów aplikacji i operatory IT za pomocą prostego, niski touch przepływów pracy zainicjować obsługę administracyjną, wdrażanie, zastosować poprawki i monitorowania aplikacji. Te wbudowane przepływy pracy znacznie zmniejszyć obciążenie na operatory IT zapewnienie ciągłej dostępności aplikacji.

Większość aplikacji składają się z kombinacji mikrousług bezstanowe i stanowe, kontenery i inne pliki wykonywalne, które są wdrożone razem. Dzięki użyciu silne typów w aplikacjach, usługi Service Fabric umożliwia wdrożenie wielu wystąpień aplikacji. Każde wystąpienie jest zarządzane i zaktualizować niezależnie. Ważne sieci szkieletowej usług można wdrożyć kontenerów lub wszystkie pliki wykonywalne i ich niezawodne. Na przykład sieci szkieletowej usług można wdrażać .NET, platformy ASP.NET Core, node.js, Windows kontenerów, Linux kontenerów, Java maszyn wirtualnych, skrypty, kątową lub dosłownie wszystko, co stanowi aplikacji.

Sieć szkieletowa usług jest zintegrowany z elementu konfiguracji/CD narzędzi takich jak [Visual Studio Team Services](https://www.visualstudio.com/team-services/), [Wpięć](https://jenkins.io/index.html), i [wdrażanie Ośmiornica](https://octopus.com/) i mogą być używane z innych popularnych narzędzi CI/CD.

Aby uzyskać więcej informacji dotyczących zarządzania cyklem życia aplikacji, przeczytaj [cyklem życia aplikacji](service-fabric-application-lifecycle.md). Aby uzyskać więcej informacji o sposobie wdrażania dowolny kod, zobacz [wdrażanie pliku wykonywalnego gościa](service-fabric-deploy-existing-app.md).

## <a name="key-capabilities"></a>Najważniejsze możliwości
Przy użyciu usługi Service Fabric, można:

* Wdróż do platformy Azure lub lokalnymi centrami danych, uruchomienia systemu Windows lub Linux z zero zmian w kodzie. Jednokrotnego zapisu, a następnie wdrożyć dowolnym do żadnego klastra sieci szkieletowej usług.
* Tworzenie skalowalnych aplikacji, które składają się z mikrousług przy użyciu usługi Service Fabric programowania modele, kontenery lub dowolny kod.
* Opracowywania wysoce niezawodne bezstanowe i stanowe mikrousług. Uproszczenie projektowania aplikacji przy użyciu mikrousług stanowych. 
* Nowe modelu programowania Reliable Actors umożliwia utworzenie obiektów chmury z kodem samodzielną i stanu.
* Wdrażanie i organizowania kontenerów, które obejmują kontenery systemu Windows i Linux kontenerów. Sieć szkieletowa usług to orchestrator wiedzieć, stanowe, kontenera danych.
* Wdrażanie aplikacji w sekundach o wysokiej gęstości z setkami lub tysiącami aplikacji lub kontenery dla poszczególnych komputerów.
* Wdrażanie różnych wersji aplikacji tego samego siebie i zaktualizuj niezależnie każdej aplikacji.
* Zarządzanie cyklem życia aplikacji bez żadnych przestojów, w tym aktualizacje krytyczne i nierozdzielających.
* Skalowanie w poziomie oraz skalowanie liczby węzłów w klastrze. Podczas skalowania węzłów, aplikacje są skalowane automatycznie.
* Monitorowanie i diagnozowanie kondycji aplikacji oraz ustawić zasady wykonywania automatycznej naprawy.
* Obejrzyj równoważenia zasobów organizowania rozpowszechnianie aplikacji w ramach klastra. Sieć szkieletowa usług odzyskiwania z błędami i optymalizuje rozkład obciążenia na podstawie dostępnych zasobów.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Następne kroki
* Więcej informacji:
  * [Dlaczego mikrousług podejścia do tworzenia aplikacji?](service-fabric-overview-microservices.md)
  * [Terminologia — omówienie](service-fabric-technical-overview.md)
* Trwa konfigurowanie Twojego [środowiska projektowego systemu Windows](service-fabric-get-started.md)  
* Trwa konfigurowanie Twojego [środowisko programistyczne systemu Linux](service-fabric-get-started-linux.md)
* Uzyskaj informacje o [opcjach pomocy technicznej usługi Service Fabric](service-fabric-support.md)

[Image1]: media/service-fabric-overview/Service-Fabric-Overview.png
