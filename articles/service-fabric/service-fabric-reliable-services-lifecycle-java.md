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
ms.openlocfilehash: 80eb68346dd05c256c60725eb082aa0651fe7cbd
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/18/2017
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

## <a name="notes-on-service-lifecycle"></a>Uwagi dotyczące cyklu życia usług
* Zarówno `runAsync()` — metoda i `createServiceInstanceListeners` wywołania są opcjonalne. Usługa może mieć ich, zarówno lub nie. Na przykład, jeśli usługa jest jego pracy w odpowiedź na wywołania użytkownika, jest niepotrzebna go do zaimplementowania `runAsync()`. Wymagane są tylko odbiorników komunikacji i ich skojarzonego kodu. Podobnie tworzenie i zwracanie odbiorników komunikacji jest opcjonalne, ponieważ usługa może mieć tylko pracy tła i dlatego tylko musi implementować`runAsync()`
* Jest on prawidłowy dla usługi, aby ukończyć `runAsync()` pomyślnie i z powrotem od niego. To nie jest uznawane za stanu błędu i stanowić Praca w tle kończenia usług. Dla stanowych usług niezawodnej `runAsync()` może zostać wywołana, jeśli usługi zostały obniżenie poziomu z podstawowego i następnie podwyższony do podstawowego.
* Jeśli zamknie usługę `runAsync()` przez zgłaszanie nieoczekiwany wyjątek, to jest błąd obiekt usługi jest zamknięta i zgłoszony błąd kondycji.
* Gdy nie ma żadnego limitu czasu na zwracanie z tych metod, natychmiast spowoduje utratę możliwości zapisu i z tego powodu nie można ukończyć rzeczywistą pracę. Zaleca się, czy zwracać możliwie jak najszybciej po otrzymaniu żądania anulowania. Jeśli usługa nie odpowiada na te wywołania interfejsu API w rozsądnym czasie sieci szkieletowej usług wymuszanie może zakończyć korzystanie z usługi. Zwykle to tylko odbywa się podczas uaktualniania aplikacji lub gdy usługa jest usuwana. Tego limitu czasu wynosi 15 minut domyślnie.
* Błędy w `onCloseAsync()` doprowadzi do ścieżki `onAbort()` wywoływana, która jest możliwość optymalnych ostatniej szansy, usługi wyczyścić Konfigurowanie i wszystkie zasoby, które zostały przejęte ich wydania.

> [!NOTE]
> Niezawodne usługi stanowej nie są obsługiwane w języku java jeszcze.
>
>

## <a name="next-steps"></a>Następne kroki
* [Wprowadzenie do niezawodne usługi](service-fabric-reliable-services-introduction.md)
* [Niezawodne usługi szybki start](service-fabric-reliable-services-quick-start.md)
* [Niezawodne usługi advanced użycia](service-fabric-reliable-services-advanced-usage.md)
