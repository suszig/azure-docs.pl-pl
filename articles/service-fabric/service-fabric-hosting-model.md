---
title: "Sieć szkieletowa usług Azure, Model hostowania | Dokumentacja firmy Microsoft"
description: "Opisuje relację między repliki (lub wystąpień) wdrożonej usługi sieci szkieletowej Servic i procesu hosta usługi."
services: service-fabric
documentationcenter: .net
author: harahma
manager: timlt
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/15/2017
ms.author: harahma
ms.openlocfilehash: ecc9038cf895ddaeb06dd0e4e9852d5ef4a4513a
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2017
---
# <a name="service-fabric-hosting-model"></a>Model obsługi sieci szkieletowej usług
Ten artykuł zawiera omówienie aplikacji modele udostępniane przez usługi sieć szkieletowa usług hostingu i opisano różnice między **udostępnionego procesu** i **wyłącznego procesu** modeli. Opisuje wygląd wdrożonej aplikacji na węzeł sieci szkieletowej usług i relacji między repliki (lub wystąpień), usługi i procesu hosta usługi.

Przed kontynuacją, upewnij się, że znasz [Model aplikacji sieci szkieletowej usług] [ a1] i zrozumienie różnych pojęcia i relacji między nimi. 

> [!NOTE]
> W tym artykule, dla uproszczenia chyba że jawnie wymienionych:
>
> - Wszystkie używa programu word *repliki* dotyczy zarówno repliki usługi stanowej lub wystąpienia usługi bezstanowej.
> - *Elementu CodePackage* jest równoważna poddanego *ServiceHost* procesu, który rejestruje *ServiceType* i replik hostów usług tego *ServiceType* .
>

Aby poznać hostingu modelu, poinformuj nas przeprowadzenie przykładem. Daj nam powiedzieć mamy *atrybutów ApplicationType* MyAppType, który ma *ServiceType* MyServiceType, który jest dostarczany przez *ServicePackage* MyServicePackage, który ma *Elementu CodePackage* MyCodePackage, która rejestruje *ServiceType* "MyServiceType" po uruchomieniu.

Załóżmy, że mamy 3 węzła klastra i utworzymy *aplikacji* **fabric: / App1** typu "MyAppType". Wewnątrz to *aplikacji* **fabric: / App1** utworzymy usługi **sieci szkieletowej: / App1/ServiceA** typu MyServiceType, która ma 2 partycjach (powiedzieć **P1**  &  **P2**) i 3 replik jednej partycji. Na poniższym diagramie przedstawiono widoku tej aplikacji, w jakiej kończy się wdrożonej w węźle.

<center>
![Widok węzła wdrożonej aplikacji][node-view-one]
</center>

Sieć szkieletowa usług aktywowany MyServicePackage, który rozpoczął MyCodePackage, który jest hostem repliki z obu partycji tj. **P1** & **P2**. Należy pamiętać, że wszystkie węzły w klastrze mają tego samego widoku ponieważ Wybraliśmy liczby replik jednej partycji równej liczby węzłów w klastrze. Utwórz nową usługę **sieci szkieletowej: / App1/ServiceB** w aplikacji **sieci szkieletowej: / App1** mającego 1 partycji (powiedzieć **P3**) i 3 replik jednej partycji. Poniższy diagram przedstawia nowy widok w węźle:

<center>
![Widok węzła wdrożonej aplikacji][node-view-two]
</center>

Jak widać, Service Fabric umieszczone nowej repliki dla partycji **P3** usługi **fabric: / App1/ServiceB** w istniejących aktywacji "MyServicePackage". Umożliwia teraz utworzyć inną *aplikacji* **sieci szkieletowej: / App2** typu "MyAppType" i wewnątrz **sieci szkieletowej: / App2** Utwórz usługę **sieci szkieletowej: / App2/ServiceA** mającego 2 partycjach (powiedzieć **P4** & **5**) i 3 replik jednej partycji. Następujące diagramy przedstawia nowy widok węzła:

<center>
![Widok węzła wdrożonej aplikacji][node-view-three]
</center>

Teraz usługi sieć szkieletowa aktywował nową kopię MyServicePackage, która uruchamia nową kopię "MyCodePackage" i repliki z obu partycji usługi **fabric: / App2/ServiceA** (tj. **P4**  &  **5**) są umieszczane w tej nowej kopii "MyCodePackage".

## <a name="shared-process-model"></a>Udostępnione modelu procesów
Widzieliśmy powyżej jest domyślny model udostępniane przez usługi sieć szkieletowa usług hostowania i jest nazywany **udostępnionego procesu** modelu. W tym modelu dla danego *aplikacji*tylko jeden kopia danego *ServicePackage* została aktywowana w *węzła* (co spowoduje włączenie wszystkich *CodePackages* zawarte w niej) i wszystkich replik wszystkich usług danego *ServiceType* są umieszczane w *elementu CodePackage* który który rejestruje *ServiceType*. Innymi słowy, wszystkich replik wszystkich usług w węźle danego *ServiceType* udostępniać ten sam proces.

## <a name="exclusive-process-model"></a>Model procesu wyłączności
Inne model hostingu udostępniane przez usługi sieć szkieletowa jest **wyłącznego procesu** modelu. W tym modelu na danym *węzła*, objęcia każdej repliki usługi Service Fabric aktywuje nową kopię *ServicePackage* (co spowoduje włączenie wszystkich *CodePackages* zawarte w niej) Replika jest umieszczona w *elementu CodePackage* zarejestrowanego *ServiceType* usługi do repliki, które należy. Innymi słowy każda replika znajduje się w dedykowanym procesie. 

Ten model jest obsługiwana wersja 5.6 sieci szkieletowej usług. **Proces wyłącznego** modelu można wybrać podczas tworzenia usługi (za pomocą [PowerShell][p1], [REST] [ r1] lub [Klienta fabricclient z rolą][c1]), określając **ServicePackageActivationMode** jako "ExclusiveProcess".

```powershell
PS C:\>New-ServiceFabricService -ApplicationName "fabric:/App1" -ServiceName "fabric:/App1/ServiceA" -ServiceTypeName "MyServiceType" -Stateless -PartitionSchemeSingleton -InstanceCount -1 -ServicePackageActivationMode "ExclusiveProcess"
```

```csharp
var serviceDescription = new StatelessServiceDescription
{
    ApplicationName = new Uri("fabric:/App1"),
    ServiceName = new Uri("fabric:/App1/ServiceA"),
    ServiceTypeName = "MyServiceType",
    PartitionSchemeDescription = new SingletonPartitionSchemeDescription(),
    InstanceCount = -1,
    ServicePackageActivationMode = ServicePackageActivationMode.ExclusiveProcess
};

var fabricClient = new FabricClient(clusterEndpoints);
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

Jeśli korzystasz z usługi domyślne w manifeście aplikacji, możesz wybrać **wyłącznego procesu** modelu, określając **ServicePackageActivationMode** atrybutu, jak pokazano poniżej:

```xml
<DefaultServices>
  <Service Name="MyService" ServicePackageActivationMode="ExclusiveProcess">
    <StatelessService ServiceTypeName="MyServiceType" InstanceCount="1">
      <SingletonPartition/>
    </StatelessService>
  </Service>
</DefaultServices>
```
Kontynuowaniem w naszym przykładzie powyżej, umożliwia tworzenie innej usługi **fabric: / App1/ServiceC** w aplikacji **sieci szkieletowej: / App1** mającego 2 partycjach (powiedzieć **P6**  &  **P7**) i 3 replik jednej partycji z **ServicePackageActivationMode** ustawioną wartość "ExclusiveProcess". Poniższy diagram przedstawia nowy widok w węźle:

<center>
![Widok węzła wdrożonej aplikacji][node-view-four]
</center>

Jak widać, Service Fabric aktywowany dwóch nowych kopii "MyServicePackage" (po jednej dla każdej repliki z partycji **P6** & **P7**) i każdej repliki jego dedykowanych kopii *Elementu CodePackage*. Innym czynnikiem, należy pamiętać, w tym miejscu, podczas **wyłącznego procesu** model używany dla danego *aplikacji*, wielu kopii danej *ServicePackage* mogą być używane w  *Węzeł*. W powyżej przykładzie przedstawiono trzy kopie "MyServicePackage" i działają dla **fabric: / App1**. Każdy z tych active kopii "MyServicePackage" ma **ServicePackageActivationId** skojarzonych z nim, który identyfikuje ją w *aplikacji* **fabric: / App1**.

Gdy tylko **udostępnionego procesu** modelu służy do *aplikacji*, takiej jak **sieci szkieletowej: / App2** w powyżej przykład istnieje tylko jedna kopia active *ServicePackage*  na *węzła* i **ServicePackageActivationId** w przypadku tego aktywacji *ServicePackage* jest "pustego ciągu".

> [!NOTE]
>- **Udostępnione procesu** model hostowania odpowiada **ServicePackageActivationMode** równy **SharedProcess**. Jest to domyślny model hostowania i **ServicePackageActivationMode** nie muszą być określane podczas tworzenia usługi.
>
>- **Proces wyłącznego** model hostowania odpowiada **ServicePackageActivationMode** równy **ExclusiveProcess** i muszą być jawnie określone w chwili tworzenia usługi. 
>
>- Hostingu modelu usługi mogą być znane, badając [opis usługi] [ p2] i spojrzenie na wartość **ServicePackageActivationMode**.
>
>

## <a name="working-with-deployed-service-package"></a>Praca z wdrożony pakiet usługi
Aktywne kopię *ServicePackage* w węźle jest określana jako [wdrożony pakiet usługi][p3]. Jak wcześniej wspomniano powyżej, gdy **wyłącznego procesu** model jest używany do tworzenia usług dla danego *aplikacji*, może to być wiele pakietów wdrożonej usługi dla tego samego  *ServicePackage*. Podczas wykonywania operacji na specyficzne dla wdrożony pakiet usługi takie jak [raportowania kondycji pakietu wdrożonej usługi] [ p4] lub [ponownego uruchamiania pakietu kodupakietuwdrożonejusługi] [ p5] itp., **ServicePackageActivationId** musi zostać zapewniony, aby zidentyfikować konkretną wdrożony pakiet usługi.

 **ServicePackageActivationId** wdrożonej usługi można uzyskać pakietu badając listę [wdrożonych pakietów usługi] [ p3] w węźle. Podczas wykonywania zapytania [wdrożone typów usług][p6], [wdrożone replik] [ p7] i [wdrożone pakiety kodu] [ p8] w węźle, zawiera także wynik zapytania **ServicePackageActivationId** pakiet z wdrożonej przez element nadrzędny.

> [!NOTE]
>- W obszarze **udostępnionego procesu** hostingu modelu, w danej *węzła*, dla danego *aplikacji*tylko jeden kopię *ServicePackage* jest aktywny . Ma ona **ServicePackageActivationId** równa *pusty ciąg* i nie musi być określony, podczas gdy operacje dotyczące wykonywania wdrożony pakiet usługi. 
>
> - W obszarze **wyłącznego procesu** hostingu modelu, w danej *węzła*, dla danego *aplikacji*, jednego lub większej liczby kopii *ServicePackage* może być aktywne. Każda aktywacja ma *niepustym* **ServicePackageActivationId** i wymaga należy określić podczas wykonywania wdrożone usługi pakietu powiązanych operacji. 
>
> - Jeśli **ServicePackageActivationId** jest pominięty, jego wartość domyślna to "pustego ciągu". Jeśli pakiet wdrożonej usługi, która została aktywowana w obszarze **udostępnionego procesu** modelu jest obecny, a następnie będzie można wykonać operacji na nim, w przeciwnym razie operacja zakończy się niepowodzeniem.
>
> - Nie zaleca się raz zapytania i pamięci podręcznej **ServicePackageActivationId** generowane dynamicznie i można zmienić z różnych przyczyn. Przed wykonaniem operacji, które należy **ServicePackageActivationId**, należy najpierw sprawdzić listę [wdrożonych pakietów usługi] [ p3] na węzeł, a następnie użyć  **ServicePackageActivationId** z wyników zapytania. Aby wykonać operację.
>
>

## <a name="guest-executable-and-container-applications"></a>Gość plik wykonywalny i kontenera aplikacji
Sieć szkieletowa usług traktuje [pliku wykonywalnego gościa] [ a2] i [kontenera] [ a3] aplikacji jako statless usług, które są niezależne, tj. Brak nie środowiska uruchomieniowego platformy Service Fabric w *ServiceHost* (proces lub kontenera). Ponieważ te usługi są niezależne, liczba replik na *ServiceHost* nie ma zastosowania do tych usług. Konfiguracja najczęściej używane z tymi usługami jest jednej partycji z [InstanceCount] [ c2] równy -1 (tj. jedna kopia kodu usługi uruchomione w każdym węźle klastra). 

Wartość domyślna **ServicePackageActivationMode** dla tych usług jest **SharedProcess** w takim przypadku platformy Service Fabric aktywuje tylko jedną kopię *ServicePackage* na  *Węzeł* dla danego *aplikacji* co oznacza, że tylko jedna kopia kodu usługi zostaną uruchomione *węzła*. Jeśli chcesz, aby wiele kopii kodu usługi do uruchamiania na *węzła* podczas tworzenia wielu usług (*Service1* do *ServiceN*) z *ServiceType* (określony w *ServiceManifest*) lub gdy usługa jest podzielona na partycje multi, należy określić **ServicePackageActivationMode** jako **ExclusiveProcess**w czasie tworzenia usługi.

## <a name="changing-hosting-model-of-an-existing-service"></a>Zmiana modelu hostingu istniejącej usługi
Zmiana modelu hostingu istniejącą usługę z **udostępnionego procesu** do **wyłącznego procesu** i na odwrót za pośrednictwem uaktualnić lub zaktualizować mechanizmu (lub w domyślnym obsługi specyfikacji w aplikacji manifest) nie jest obecnie obsługiwane. Obsługa tej funkcji wejdzie w przyszłych wersjach.

## <a name="choosing-between-shared-process-and-exclusive-process-model"></a>Wybór między procesu współużytkowanego i model procesu wyłączności
Oba te modele hostingu ma, jego zalet i wad i potrzeb użytkowników, aby ocenić, która najlepiej odpowiada wymaganiom ich. **Udostępnione procesu** model umożliwia lepsze wykorzystanie zasobów systemu operacyjnego, ponieważ procesy mniej jest zduplikowany, wiele replik w tym samym procesie można udostępniać portów itp. Jednak jeden z replik trafienia wystąpił błąd, na którym należy go zamknąć hosta usługi, będzie miało wpływ wszystkich replik w tym samym procesie.

 **Proces wyłącznego** modelu zapewnia lepszą izolację z każdej repliki w swoim własnym procesie i zachowania repliki nie ma wpływu na pozostałe repliki. Chodzi przydatną w przypadku których udostępnianie portów nie jest obsługiwana przez protokół komunikacji. Ułatwia on możliwość stosowania ładu zasobów na poziomie repliki. Z drugiej strony **wyłącznego procesu** zajmie więcej zasobów systemu operacyjnego, zgodnie z jego spowoduje utworzenie jednego procesu dla każdej repliki na węźle.

## <a name="exclusive-process-model-and-application-model-considerations"></a>Model procesu wyłączności i aplikacji zagadnienia dotyczące modelu
Zalecanym sposobem modelu aplikacji w sieci szkieletowej usług jest zachowanie jedną *ServiceType* na *ServicePackage* i ten model działa dobrze w przypadku większości aplikacji. 

Przeznaczone dla niektórych zastosowań, Service Fabric umożliwia również więcej niż jeden *ServiceType* na *ServicePackage* (jeden *elementu CodePackage* można zarejestrować więcej niż jeden  *Typ ServiceType*). Poniżej przedstawiono niektóre scenariusze, w których te konfiguracje mogą być przydatne:

- Chcesz optymalizować wykorzystanie zasobów systemu operacyjnego dochodzi do uruchamiania procesów mniej i po zwiększeniu repliki na proces.
- Repliki z różnych Serivcetype konieczne udostępnianie niektórych typowych danych ma wysoką inicjowania lub pamięci kosztów.
- Masz oferty bezpłatnej usługi i chcesz zawiesić limit wykorzystania zasobów przez umieszczenie wszystkich replik usługi, w tym samym procesie.

**Proces wyłącznego** model hostowania nie jest spójna z modelem aplikacji istnieniem wielu *Serivcetype* na *ServicePackage*. Jest to spowodowane wielu *Serivcetype* na *ServicePackage* zaprojektowano w celu osiągnięcia wyższej zasobów, udostępnianie między replikami i umożliwia zwiększeniu repliki na proces. Jest to sprzeczne z co **wyłącznego procesu** modelu zaprojektowano w celu osiągnięcia.

Należy wziąć pod uwagę w przypadku wielu *Serivcetype* na *ServicePackage* z różnymi *elementu CodePackage* zarejestrowanie każdego *ServiceType* . Załóżmy, że mamy *ServicePackage* MultiTypeServicePackge, która ma dwa *CodePackages*:

- MyCodePackageA, która rejestruje *ServiceType* "MyServiceTypeA".
- MyCodePackageB, która rejestruje *ServiceType* "MyServiceTypeB".

Teraz, umożliwia powiedzieć, utworzymy *aplikacji* **sieci szkieletowej: / SpecialApp** i wewnątrz **sieci szkieletowej: / SpecialApp** utworzymy następujące dwie usługi z **wyłączności Proces** modelu:

- Usługi **fabric: / SpecialApp/ServiceA** typu "MyServiceTypeA" z dwóch partycji (powiedzieć **P1** i **P2**) i 3 replik jednej partycji.
- Usługi **fabric: / SpecialApp/ServiceB** typu "MyServiceTypeB" z dwóch partycji (powiedzieć **P3** i **P4**) i 3 replik jednej partycji.

W danym węźle obu usług ma dwie repliki. Ponieważ użyliśmy **wyłącznego procesu** modelu do tworzenia usług sieci szkieletowej usług uaktywni nową kopię "MyServicePackge" dla każdej repliki. Każda aktywacja "MultiTypeServicePackge" spowoduje uruchomienie kopię "MyCodePackageA" i "MyCodePackageB". Jednak tylko jeden "MyCodePackageA" lub "MyCodePackageB" będzie obsługiwał replikę dla którego "MultiTypeServicePackge" został aktywowany. Poniższy diagram przedstawia widok węzła:

<center>
![Widok węzła wdrożonej aplikacji][node-view-five]
</center>

 Jak widać, podczas aktywacji "MultiTypeServicePackge" dla replik partycji **P1** usługi **fabric: / SpecialApp/ServiceA**jest "MyCodePackageB" i "MyCodePackageA" jest hostem repliki tylko do pracy. Podobnie w przypadku aktywacji "MultiTypeServicePackge" dla replik partycji **P3** usługi **fabric: / SpecialApp/ServiceB**, obsługuje replikę "MyCodePackageB" i "MyCodePackageA" jest tylko uruchomiona i tak dalej. W związku z tym większej liczby *CodePackages* (rejestrowanie różnych *Serivcetype*) na *ServicePackage*, wyższy będzie użycie zasobów nadmiarowe. 
 
 Z drugiej strony, jeśli utworzymy usługami **sieci szkieletowej: / SpecialApp/ServiceA** i **sieci szkieletowej: / SpecialApp/ServiceB** z **udostępnionego procesu** modelu uaktywni sieci szkieletowej usług tylko jedna kopia "MultiTypeServicePackge" dla *aplikacji* **fabric: / SpecialApp** (zgodnie z wcześniej widzieliśmy). "MyCodePackageA" będzie obsługiwać wszystkie repliki usługi **sieci szkieletowej: / SpecialApp/ServiceA** (lub dowolnej usługi typu MyServiceTypeA, aby dokładniej) i "MyCodePackageB" będzie obsługiwać wszystkie repliki usługi **sieci szkieletowej: / SpecialApp/ServiceB** (lub dowolnej usługi typu MyServiceTypeB, aby dokładniej). Poniższy diagram przedstawia widok węzła w tym ustawieniu: 

<center>
![Widok węzła wdrożonej aplikacji][node-view-six]
</center>

W powyższym przykładzie może uważasz, że jeśli "MyCodePackageA" rejestruje zarówno "MyServiceTypeA" i "MyServiceTypeB" i "MyCodePackageB" nie istnieje, a następnie nie będzie nie nadmiarowe *elementu CodePackage* uruchomiona. To jest prawidłowa, jednak jak już wspomniano, ten model aplikacji nie jest wyrównana z **wyłącznego procesu** model hostowania. Jeśli celem jest włączanie dla każdej repliki własne dedykowane przetworzyć następnie rejestrując zarówno *Serivcetype* z samej *elementu CodePackage* nie są potrzebne i umieszczanie każdego *ServiceType* w własne *ServicePacakge* jest bardziej naturalne wyborem.

## <a name="next-steps"></a>Następne kroki
[Pakiet aplikacji] [ a4] i przygotowania do wdrożenia.

[Wdrażanie i usunąć aplikacje] [ a5] opisano, jak zarządzać wystąpień aplikacji przy użyciu programu PowerShell.

<!--Image references-->
[node-view-one]: ./media/service-fabric-hosting-model/node-view-one.png
[node-view-two]: ./media/service-fabric-hosting-model/node-view-two.png
[node-view-three]: ./media/service-fabric-hosting-model/node-view-three.png
[node-view-four]: ./media/service-fabric-hosting-model/node-view-four.png
[node-view-five]: ./media/service-fabric-hosting-model/node-view-five.png
[node-view-six]: ./media/service-fabric-hosting-model/node-view-six.png

<!--Link references--In actual articles, you only need a single period before the slash-->
[a1]: service-fabric-application-model.md
[a2]: service-fabric-deploy-existing-app.md
[a3]: service-fabric-containers-overview.md
[a4]: service-fabric-package-apps.md
[a5]: service-fabric-deploy-remove-applications.md

[r1]: https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-createservice

[c1]: https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync
[c2]: https://docs.microsoft.com/dotnet/api/system.fabric.description.statelessservicedescription.instancecount

[p1]: https://docs.microsoft.com/powershell/servicefabric/vlatest/new-servicefabricservice
[p2]: https://docs.microsoft.com/powershell/servicefabric/vlatest/get-servicefabricservicedescription
[p3]: https://docs.microsoft.com/powershell/servicefabric/vlatest/get-servicefabricdeployedservicePackage
[p4]: https://docs.microsoft.com/powershell/servicefabric/vlatest/send-servicefabricdeployedservicepackagehealthreport
[p5]: https://docs.microsoft.com/powershell/servicefabric/vlatest/restart-servicefabricdeployedcodepackage
[p6]: https://docs.microsoft.com/powershell/servicefabric/vlatest/get-servicefabricdeployedservicetype
[p7]: https://docs.microsoft.com/powershell/servicefabric/vlatest/get-servicefabricdeployedreplica
[p8]: https://docs.microsoft.com/powershell/servicefabric/vlatest/get-servicefabricdeployedcodepackage
