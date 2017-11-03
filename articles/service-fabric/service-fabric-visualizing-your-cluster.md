---
title: "Wizualizacja klastra przy użyciu Eksploratora usługi sieć szkieletowa | Dokumentacja firmy Microsoft"
description: "Eksploratora usługi sieć szkieletowa jest oparte na sieci web narzędzie do sprawdzania i zarządzanie aplikacjami w chmurze i węzłów w klastrze usługi sieć szkieletowa usług Microsoft Azure."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: c875b993-b4eb-494b-94b5-e02f5eddbd6a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/28/2017
ms.author: ryanwi
ms.openlocfilehash: 965ffc0f8cec26cccbe6e6459731afc234111f4d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="visualize-your-cluster-with-service-fabric-explorer"></a>Wizualizowanie klastra przy użyciu narzędzia Service Fabric Explorer
Eksploratora usługi sieć szkieletowa jest oparte na sieci web narzędzie do sprawdzania i zarządzanie aplikacjami i węzły w klastrze usługi sieć szkieletowa usług Azure. Service Fabric Explorer znajduje się bezpośrednio w klastrze, dzięki czemu jest zawsze dostępne, niezależnie od tego, w którym jest uruchomiona klastra.

## <a name="video-tutorial"></a>Samouczek wideo

Aby dowiedzieć się, jak używać Eksploratora usługi sieć szkieletowa, obejrzyj następujące wideo Microsoft Virtual Academy:

[<center><img src="./media/service-fabric-visualizing-your-cluster/SfxVideo.png" WIDTH="360" HEIGHT="244"></center>](https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=bBTFg46yC_9806218965)

## <a name="connect-to-service-fabric-explorer"></a>Połącz do Eksploratora usługi sieć szkieletowa
Po wykonaniu instrukcji do [przygotowywanie środowiska projektowego](service-fabric-get-started.md), Service Fabric Explorer można uruchomić w klastrze lokalnym, przechodząc do http://localhost: 19080/Explorer.

## <a name="understand-the-service-fabric-explorer-layout"></a>Zrozumienie układu Service Fabric Explorer
Za pomocą Eksploratora usługi sieć szkieletowa można nawigować przy użyciu drzewa po lewej stronie. W katalogu głównym drzewa pulpit nawigacyjny klastra zawiera omówienie klastra, w tym podsumowanie aplikacji i kondycji węzła.

![Pulpit nawigacyjny klastra Service Fabric Explorer][sfx-cluster-dashboard]

### <a name="view-the-clusters-layout"></a>Wyświetl układ klastra
Węzły w klastrze usługi sieć szkieletowa usług są umieszczane między dwuwymiarowa siatki domen błędów i uaktualnienia domen. To umieszczania gwarantuje, że aplikacje pozostają dostępne obecności awarie sprzętu i aktualizacje aplikacji. Możesz wyświetlić bieżący klaster układ przy użyciu mapy klastra.

![Mapa klastra Service Fabric Explorer][sfx-cluster-map]

### <a name="view-applications-and-services"></a>Wyświetl aplikacje i usługi
Klaster zawiera dwa poddrzewa: jeden dla aplikacji i drugi dla węzłów.

Można skorzystać z widoku aplikacji poruszać się po hierarchii logicznej usługi Service Fabric: aplikacji, usług, partycji i replik.

W przykładzie poniżej aplikacji **moja_aplikacja** składa się z dwóch usług **MyStatefulService** i **WebService**. Ponieważ **MyStatefulService** jest obiektem stanowym, zawiera partycji z jednego podstawowego i dwóch replik pomocniczych. Z kolei WebSvcService jest bezstanowych i zawiera pojedyncze wystąpienie.

![Widok aplikacji w narzędziu Service Fabric Explorer][sfx-application-tree]

Na każdym poziomie drzewa w okienku głównym zostaną wyświetlone odpowiednie informacje o elemencie. Można na przykład Zobacz stan kondycji i wersji określonej usługi.

![Okienko essentials Service Fabric Explorer][sfx-service-essentials]

### <a name="view-the-clusters-nodes"></a>Wyświetlanie węzłów klastra
Widok węzła przedstawia fizyczny układ klastra. Dla danego węzła można sprawdzić, które aplikacje mają kod wdrożony w tym węźle. W szczególności widać replik, które są aktualnie uruchomione istnieje.

## <a name="actions"></a>Akcje
Service Fabric Explorer oferuje możliwość szybkiego wywołanie akcji w węzłach, aplikacji i usług w ramach klastra.

Na przykład, aby usunąć aplikację, wybierz aplikację z drzewa po lewej stronie, a następnie wybierz **akcje** > **Usuń aplikację**.

![Usunięcie aplikacji w narzędziu Service Fabric Explorer][sfx-delete-application]

> [!TIP]
> Te same akcje można wykonać przez kliknięcie przycisku wielokropka obok każdego elementu.
>
>

W poniższej tabeli wymieniono akcje dostępne dla każdego obiektu:

| **Jednostki** | **Akcja** | **Opis** |
| --- | --- | --- |
| Typ aplikacji |Cofnij aprowizację typu |Usuwa pakiet aplikacji z magazynu obrazu klastra. Wymaga wszystkie aplikacje tego typu, aby najpierw należy usunąć. |
| Aplikacja |Usuń aplikację |Usuwanie aplikacji, w tym jej usług i ich stan (jeśli istnieje). |
| Usługa |Usuwanie usługi |(Jeśli istnieje), należy usunąć usługę i jego stan. |
| Węzeł |Activate |Aktywuj węzła. |
| Węzeł | Dezaktywuj (Wstrzymaj) | Wstrzymanie węzła w jego bieżącym stanie. Usługi nadal działać, ale usługi sieć szkieletowa nie aktywnego przenosi niczego na lub wyłącz go, chyba że jest wymagany w celu zapobiegania niespójność danych lub awarii. Ta akcja zwykle jest używana do włączenia usług debugowania w określonym węźle, aby upewnić się, że nie są przenoszone podczas kontroli. | |
| Węzeł | Dezaktywuj (ponowne uruchomienie) | Bezpieczne przenoszenie wszystkich usług w pamięci wyłącz węzeł i zamknij usług trwałych. Zazwyczaj używany, gdy procesy hosta lub komputera, konieczne jest ponowne uruchomienie. | |
| Węzeł | Dezaktywuj (Usuń dane) | Bezpiecznie zamknąć wszystkie usługi uruchomione w węźle po utworzeniu wystarczające zapasowe replik. Zazwyczaj używane do węzła (lub co najmniej jej magazynem) jest przełączana trwale poza Komisji. | |
| Węzeł | Usuń stan węzła | Usuń wiedzy replik węzła z klastra. Zazwyczaj używany podczas uznaje nieodwracalny węzeł już nie powiodło się. | |
| Węzeł | Ponowne uruchamianie | Symulację awarii węzła poprzez ponowne uruchomienie węzła. Więcej informacji [tutaj](/powershell/module/servicefabric/restart-servicefabricnode?view=azureservicefabricps) | |

Od momentu rzeczywistości działają destrukcyjnie wiele działań, użytkownik może się monit o potwierdzenie Twoich zamiarów, przed zakończeniem działania.

> [!TIP]
> Wszystkie akcje, które mogą być wykonywane za pomocą Eksploratora usługi sieć szkieletowa można również przeprowadzić za pomocą programu PowerShell lub interfejsu API REST, aby włączyć automatyzacji.
>
>

Umożliwia także Eksploratora usługi sieć szkieletowa można utworzyć wystąpienia aplikacji dla danej aplikacji typu i wersji. Wybierz typ aplikacji, w widoku drzewa, a następnie kliknij przycisk **Utwórz wystąpienie aplikacji** link obok wersji chcesz w okienku po prawej stronie.

![Tworzenie wystąpienia aplikacji w narzędziu Service Fabric Explorer][sfx-create-app-instance]

> [!NOTE]
> Obecnie nie mogą być parametryczne wystąpień aplikacji utworzonych za pomocą Eksploratora usługi sieć szkieletowa. Są one tworzone przy użyciu wartości parametrów domyślnych.
>
>

## <a name="connect-to-a-remote-service-fabric-cluster"></a>Połącz do zdalnego klastra usługi sieć szkieletowa
Jeśli znasz punktu końcowego klastra i masz wystarczające uprawnienia do Eksploratora usługi sieć szkieletowa dostęp z dowolnej przeglądarki. Jest to spowodowane Eksploratora usługi sieć szkieletowa jest po prostu innej usługi, która działa w klastrze.

### <a name="discover-the-service-fabric-explorer-endpoint-for-a-remote-cluster"></a>Odnajdywanie punktu końcowego Service Fabric Explorer zdalnego klastra
Aby uzyskać dostęp do Eksploratora usługi sieć szkieletowa dla danego klastra, punkt przeglądarkę, aby:

http://&lt;your klastra endpoint&gt;: 19080/Explorer

W przypadku klastrów platformy Azure pełny adres URL jest również dostępna w okienku essentials klastra w portalu Azure.

### <a name="connect-to-a-secure-cluster"></a>Nawiązywanie połączenia z zabezpieczonym klastrem
Można kontrolować dostęp klienta do klastra usługi sieć szkieletowa z certyfikatów lub przy użyciu usługi Azure Active Directory (AAD).

Jeśli próby połączenia do Eksploratora usługi sieć szkieletowa w klastrze bezpieczny, następnie w zależności od konfiguracji klastra będzie trzeba przedstawić certyfikat, klienta lub zaloguj się za pomocą usługi AAD.

## <a name="next-steps"></a>Następne kroki
* [Omówienie testowania](service-fabric-testability-overview.md)
* [Zarządzanie aplikacji usługi Service Fabric w programie Visual Studio](service-fabric-manage-application-in-visual-studio.md)
* [Wdrażanie aplikacji sieci szkieletowej usług za pomocą programu PowerShell](service-fabric-deploy-remove-applications.md)

<!--Image references-->
[sfx-cluster-dashboard]: ./media/service-fabric-visualizing-your-cluster/SfxClusterDashboard.png
[sfx-cluster-map]: ./media/service-fabric-visualizing-your-cluster/SfxClusterMap.png
[sfx-application-tree]: ./media/service-fabric-visualizing-your-cluster/SfxApplicationTree.png
[sfx-service-essentials]: ./media/service-fabric-visualizing-your-cluster/SfxServiceEssentials.png
[sfx-delete-application]: ./media/service-fabric-visualizing-your-cluster/SfxDeleteApplication.png
[sfx-create-app-instance]: ./media/service-fabric-visualizing-your-cluster/SfxCreateAppInstance.png
