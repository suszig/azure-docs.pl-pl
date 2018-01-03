---
title: "Przegląd cyklu życia Azure Usługa sieci szkieletowej niezawodnej usługi | Dokumentacja firmy Microsoft"
description: "Więcej informacji na temat zdarzeń różnych cyklu życia w sieci szkieletowej usług niezawodne usługi"
services: Service-Fabric
documentationcenter: java
author: PavanKunapareddyMSFT
manager: timlt
ms.assetid: 
ms.service: Service-Fabric
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/30/2017
ms.author: pakunapa;
ms.openlocfilehash: 93fd003ff5ba7673e5a719fb1ced0cbb97034610
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/21/2017
---
# <a name="reliable-services-lifecycle-overview"></a>Przegląd cyklu życia niezawodne usługi
> [!div class="op_single_selector"]
> * [C# w systemie Windows](service-fabric-reliable-services-lifecycle.md)
> * [Java w systemie Linux](service-fabric-reliable-services-lifecycle-java.md)
>
>

Rozważania na temat cykli niezawodnych usług, podstawowe informacje o cyklu życia są najważniejsze. Ogólnie rzecz biorąc:

* Podczas uruchamiania
  * Usługi są wykonane
  * Mają możliwość utworzenia i zwraca zero lub więcej obiektów nasłuchujących
  * Wszystkie zwracane obiekty nasłuchujące są otwarte, zezwolenie na komunikację z usługą
  * Wywoływana jest metoda runAsync usługi, dzięki czemu usługa długo działa lub pracy w tle
* Podczas zamykania
  * Token anulowania przekazany do runAsync została anulowana, a odbiorniki są zamknięte
  * To znaczy po zakończeniu sam obiekt usługi jest niszczone

Brak szczegółów wokół dokładne kolejność tych zdarzeń. W szczególności kolejność zdarzeń mogą się nieznacznie zmieniać w zależności od tego, czy usługa niezawodnej jest Stateless lub Stateful. Ponadto usługi stanowej, zostały radzenia sobie z scenariusz podstawowy wymiany. Podczas tej sekwencji rolę podstawową jest przenoszona do innej repliki (lub wróci) bez zamknięcie usługi. Na koniec mamy Pomyśl o warunkach błędu lub niepowodzenie.

## <a name="stateless-service-startup"></a>Uruchamianie usługi bezstanowej
Cykl życia usługi bezstanowej jest bardzo prosta. Poniżej przedstawiono kolejność zdarzeń:

1. Usługa jest tworzony
2. Następnie w dwie czynności równoległe stanie:
    - `StatelessService.createServiceInstanceListeners()`jest wywoływany i wszystkie zwracane obiekty nasłuchujące (`CommunicationListener.openAsync()` jest wywoływane dla każdego odbiornika)
    - Metodzie runAsync usługi (`StatelessService.runAsync()`) jest nazywany
3. Jeśli jest obecny, wywoływana jest metoda onOpenAsync tej usługi (w szczególności `StatelessService.onOpenAsync()` jest wywoływana. To jest rzadko zastąpienia, ale jest ona dostępna).

Należy koniecznie należy pamiętać, że nie kolejność nie między wywołaniami do tworzenia i otwierania odbiorników i runAsync. Przed rozpoczęciem runAsync odbiorniki może zostać otwarty. Podobnie runAsync może się okazać wywoływane przed odbiorników komunikacji są otwarte lub nawet zostały wykonane. Jeśli wymagana jest każdej synchronizacji, jest pozostawiany bez jako wykonywania do implementujący. Typowe rozwiązania:

* Czasami odbiorników nie może działać, dopóki niektóre informacje o utworzeniu lub pracy wykonanej. Dla usług bezstanowych, które zwykle można zrobić pracy w Konstruktorze tej usługi podczas `createServiceInstanceListeners()` wywołać, lub jako część konstrukcji samego odbiornika.
* Czasami kod w runAsync nie ma się rozpocząć, dopóki odbiorniki są otwarte. W takim przypadku niezbędne jest dodatkowe koordynacji. Jednym z typowych rozwiązań jest niektórych flagi w odbiorników wskazujący, po zakończeniu, który zaznaczono w runAsync przed kontynuowaniem pracy rzeczywistej.

## <a name="stateless-service-shutdown"></a>Zamykanie usługi bezstanowej
Podczas zamykania usługi bezstanowej, tego samego wzorca jest wykonać tylko w odwrotnej:

1. Równoległe
    - Są zamykane wszystkie otwarte odbiorników (`CommunicationListener.closeAsync()` jest wywoływane dla każdego odbiornika)
    - Token anulowania przekazany do `runAsync()` zostało anulowane (sprawdzanie token anulowania `isCancelled` właściwość zwraca wartość true i wywołuje tokenu `throwIfCancellationRequested` metoda zgłasza `CancellationException`)
2. Raz `closeAsync()` uzupełnia w każdym odbiornika i `runAsync()` również zakończeniu usługi `StatelessService.onCloseAsync()` metoda jest wywoływana, jeśli jest obecny (ponownie jest rzadko zastąpienie).
3. Po `StatelessService.onCloseAsync()` zakończeniu obiekt usługi jest niszczone

## <a name="stateful-service-startup"></a>Uruchamianie usługi stanowej
Usługi stanowej mają podobnego wzorca do usług bezstanowych, kilka zmian. Uruchamianie usługi stanowej, kolejność zdarzeń jest następująca:

1. Usługa jest tworzony.
2. `StatefulServiceBase.onOpenAsync()`jest wywoływana. To wywołanie nie jest zazwyczaj zastępowany przez usługę.
3. Równolegle się zdarzyć następujących czynności:
    - `StatefulServiceBase.createServiceReplicaListeners()`jest wywoływane. 
      - Jeśli usługa jest usługą podstawowy, wszystkie zwracane obiekty nasłuchujące są otwarte. `CommunicationListener.openAsync()`jest wywoływana na każdym odbiornika.
      - Jeśli usługa jest usługą dodatkowej, tylko te odbiorniki oznaczona jako `listenOnSecondary = true` są otwarte. Odbiorniki otwarte na pomocnicze bazy danych jest mniej typowych.
    - Jeśli usługa jest obecnie podstawowym, usługa firmy `StatefulServiceBase.runAsync()` metoda jest wywoływana.
4. Po wszystkich replik odbiornika dla `openAsync()` wywołuje Zakończ i `runAsync()` jest nazywany `StatefulServiceBase.onChangeRoleAsync()` jest wywoływana. To wywołanie nie jest zazwyczaj zastępowany przez usługę.

Podobnie jak usług bezstanowych, nie ma żadnych koordynację między tymi kolejność, w którym są tworzone i otworzyć odbiorniki i kiedy **runAsync** jest wywoływana. Jeśli potrzebujesz koordynacji rozwiązania są znacznie takie same. Istnieje jeden przypadek dodatkowe usługi stanowej. Powiedzieć, że wywołania, które przyjeździe odbiorników komunikacji wymagają informacje przechowywane w niektórych [niezawodnej kolekcje](service-fabric-reliable-services-reliable-collections.md). Ponieważ odbiorników komunikacji może otworzyć przed niezawodnej kolekcje są do odczytu lub zapisu, a przed **runAsync** można uruchomić niektórych dodatkowych koordynacji jest konieczne. Rozwiązanie najprostszym i najczęściej jest w przypadku odbiorników komunikacji zwrócić kod błędu, który klient używa do ponowić żądanie.

## <a name="stateful-service-shutdown"></a>Zamknięcie usługi stanowej
Podobnie jak usługi bezstanowej zdarzenia cyklu życia podczas zamykania są takie same jak podczas uruchamiania, ale wycofane. Gdy trwa zamykanie usługi stanowej, wykonywane są następujące zdarzenia:

1. Równolegle:
    - Są zamykane wszystkie otwarte odbiorników. `CommunicationListener.closeAsync()`jest wywoływana na każdym odbiornika.
    - Token anulowania przekazany do `runAsync()` zostało anulowane. Wywołania z tokenem anulowania `isCancelled()` metoda zwraca wartość true i wywołuje tokenu `throwIfCancellationRequested()` metoda zgłasza `OperationCanceledException`.
2. Po `closeAsync()` zakończeniu na każdym odbiornika i `runAsync()` również zakończeniu usługi `StatefulServiceBase.onChangeRoleAsync()` jest wywoływana. To wywołanie nie jest zazwyczaj zastępowany przez usługę.

   > [!NOTE]  
   > Poczekaj na potrzeby **runAsync** do zakończenia jest konieczny tylko wtedy, jeśli ta replika jest repliki podstawowej.

3. Po `StatefulServiceBase.onChangeRoleAsync()` zakończeniu metody `StatefulServiceBase.onCloseAsync()` metoda jest wywoływana. To wywołanie jest rzadko zastąpienia, ale jest ona dostępna.
3. Po `StatefulServiceBase.onCloseAsync()` zakończeniu obiekt usługi jest niszczone.

## <a name="stateful-service-primary-swaps"></a>Zamiany głównej usługi stanowej
Po uruchomieniu usługi stanowej replik podstawowych tej usługi stanowej ma ich odbiorników komunikacji otwarty i ich **runAsync** wywołana metoda. Replik pomocniczych są wykonane, ale nie kolejne wywołania w temacie. Po uruchomieniu usługi stanowej repliki, który jest obecnie podstawowej, można zmienić. Co to znaczy w kategoriach zdarzenia cyklu życia, które można wyświetlić repliki Zachowanie, które widzi stanowe repliki zależy od tego, czy jest repliki trwa obniżyć poziom jest podwyższany podczas wymiany.

### <a name="for-the-primary-thats-demoted"></a>Dla serwera podstawowego, który jest obniżenie poziomu
Dla repliki podstawowej jest obniżenie poziomu sieć szkieletowa usług musi tę replikę, aby zatrzymać przetwarzanie komunikatów i zakończyć pracę tła, których wykonywanie operacji. W związku z tym w tym kroku wygląda tak, jak podczas zamykania usługi. Jeden różnica polega na czy usługi nie jest niszczone lub zamknięta, ponieważ pozostaje pomocniczego. Następujące interfejsy API są nazywane:

1. Równolegle:
    - Są zamykane wszystkie otwarte odbiorników. `CommunicationListener.closeAsync()`jest wywoływana na każdym odbiornika.
    - Token anulowania przekazany do `runAsync()` zostało anulowane. Sprawdź token anulowania `isCancelled()` metoda zwraca wartość true i wywołuje tokenu `throwIfCancellationRequested()` metoda zgłasza `OperationCanceledException`.
2. Po `closeAsync()` zakończeniu na każdym odbiornika i `runAsync()` również zakończeniu usługi `StatefulServiceBase.onChangeRoleAsync()` jest wywoływana. To wywołanie nie jest zazwyczaj zastępowany przez usługę.

### <a name="for-the-secondary-thats-promoted"></a>Dla lokacji dodatkowej, która zostanie podniesiona
Podobnie sieci szkieletowej usług wymaga repliki pomocniczej, która zostanie podniesiona do nasłuchiwania komunikatów przesyłania rozpoczęcie i wszelkie niezbędne do ukończenia zadania w tle. W związku z tym proces ten wygląda tak, jak podczas tworzenia usługi z tą różnicą, że sama repliki już istnieje. Następujące interfejsy API są nazywane:

1. Równolegle:
    - `StatefulServiceBase.createServiceReplicaListeners()`jest wywoływany i wszelkie zwrócił odbiorników są otwarte. `CommunicationListener.openAsync()`jest wywoływana na każdym odbiornika.
    - Usługi `StatefulServiceBase.runAsync()` metoda jest wywoływana.
2. Po wszystkich replik odbiornika dla `openAsync()` wywołuje Zakończ i `runAsync()` jest nazywany `StatefulServiceBase.onChangeRoleAsync()` jest wywoływana. To wywołanie nie jest zazwyczaj zastępowany przez usługę.


### <a name="common-issues-during-stateful-service-shutdown-and-primary-demotion"></a>Typowe problemy podczas zamykania usługi stanowej i obniżania poziomu podstawowego
Sieć szkieletowa usług zmienia podstawowej usługi stanowej z różnych przyczyn. Najbardziej typowe są [klastra, ponowne równoważenie](service-fabric-cluster-resource-manager-balancing.md) i [uaktualniania aplikacji](service-fabric-application-upgrade.md). Podczas tych czynności (a także podczas zamykania normalnego użytkowania, np. zostanie wyświetlony, jeśli usługa została usunięta), należy pamiętać, że usługa przestrzegać `cancellationToken`. 

Usług, które nie obsługują anulowania prawidłowo może wystąpić pewne problemy. Te operacje są wolne, ponieważ usługi sieć szkieletowa czeka bezpiecznie zatrzymania usługi. Ostatecznie to prowadzić do uaktualnienia nie powiodło się tego limitu czasu i wycofać. Nie można uwzględnić token anulowania powodują imbalanced klastrów. Klastry stać się równowagi, ponieważ węzły uzyskać gorących, ale nie można rebalanced usługi, ponieważ trwa zbyt długo, aby przenieść je w innym miejscu. 

Ponieważ usługi są stanowe, również jest prawdopodobne, że używają one [niezawodnej kolekcje](service-fabric-reliable-services-reliable-collections.md). W sieci szkieletowej usług podczas obniżania poziomu podstawowego jedną pierwszy co się stanie, jest odebraniu podstawowy stan do zapisu. Prowadzi to do drugiego zestawu problemy, które mogą wpłynąć na cyklem życia usługi. Kolekcje zwracany wyjątki oparte na czas i czy jest przenoszony repliki lub zamykania. Te wyjątki powinny być traktowane jako poprawnie. Wyjątki generowane przez sieć szkieletowa usług można podzielić na stałe [(`FabricException`)](https://docs.microsoft.com/en-us/java/api/system.fabric.exception) i przejściowych [(`FabricTransientException`)](https://docs.microsoft.com/en-us/java/api/system.fabric.exception._fabric_transient_exception) kategorii. Stałe wyjątki powinny być rejestrowane i zgłoszony podczas przejściowej wyjątków można ponowić opartych na logice niektórych ponów próbę.

Obsługa wyjątków, które pochodzą od stosowania `ReliableCollections` w połączeniu z zdarzenia cyklu życia usługi jest ważnym elementem przetestowaniu i zweryfikowaniu niezawodnej usługi. Zaleca się zawsze uruchamiaj usługi Obciążenie podczas wykonywania uaktualnienia i [testowania chaos](service-fabric-controlled-chaos.md) przed wdrożeniem w środowisku produkcyjnym. Te podstawowe kroki pomoc, sprawdź, czy usługa jest poprawnie zaimplementowana i poprawnie obsługuje zdarzenia cyklu życia.

## <a name="notes-on-service-lifecycle"></a>Uwagi dotyczące cyklu życia usług
* Zarówno `runAsync()` — metoda i `createServiceInstanceListeners/createServiceReplicaListeners` wywołania są opcjonalne. Usługa może mieć ich, zarówno lub nie. Na przykład, jeśli usługa jest jego pracy w odpowiedź na wywołania użytkownika, jest niepotrzebna go do zaimplementowania `runAsync()`. Wymagane są tylko odbiorników komunikacji i ich skojarzonego kodu. Podobnie tworzenie i zwracanie odbiorników komunikacji jest opcjonalne, ponieważ usługa może mieć tylko pracy tła i dlatego tylko musi implementować`runAsync()`
* Jest on prawidłowy dla usługi, aby ukończyć `runAsync()` pomyślnie i z powrotem od niego. To nie jest uznawane za stanu błędu i stanowić Praca w tle kończenia usług. Dla stanowych usług niezawodnej `runAsync()` może zostać wywołana, jeśli usługi zostały obniżenie poziomu z podstawowego i następnie podwyższony do podstawowego.
* Jeśli zamknie usługę `runAsync()` przez zgłaszanie nieoczekiwany wyjątek, to jest błąd obiekt usługi jest zamknięta i zgłoszony błąd kondycji.
* Gdy nie ma żadnego limitu czasu na zwracanie z tych metod, natychmiast spowoduje utratę możliwości zapisu i z tego powodu nie można ukończyć rzeczywistą pracę. Zaleca się, czy zwracać możliwie jak najszybciej po otrzymaniu żądania anulowania. Jeśli usługa nie odpowiada na te wywołania interfejsu API w rozsądnym czasie sieci szkieletowej usług wymuszanie może zakończyć korzystanie z usługi. Zwykle to tylko odbywa się podczas uaktualniania aplikacji lub gdy usługa jest usuwana. Tego limitu czasu wynosi 15 minut domyślnie.
* Błędy w `onCloseAsync()` doprowadzi do ścieżki `onAbort()` wywoływana, która jest możliwość optymalnych ostatniej szansy, usługi wyczyścić Konfigurowanie i wszystkie zasoby, które zostały przejęte ich wydania.

## <a name="next-steps"></a>Kolejne kroki
* [Wprowadzenie do niezawodne usługi](service-fabric-reliable-services-introduction.md)
* [Niezawodne usługi szybki start](service-fabric-reliable-services-quick-start-java.md)
* [Niezawodne usługi advanced użycia](service-fabric-reliable-services-advanced-usage.md)
