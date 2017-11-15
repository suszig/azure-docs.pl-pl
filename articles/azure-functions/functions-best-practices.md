---
title: "Najlepsze rozwiązania dotyczące funkcji platformy Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się najlepsze rozwiązania i wzorce dla usługi Azure Functions."
services: functions
documentationcenter: na
author: wesmc7777
manager: cfowler
editor: 
tags: 
keywords: "środowisko Azure functions, wzorce, najlepszym rozwiązaniem, funkcji, przetwarzania elementów webhook, dynamiczne obliczeń, architektura niekorzystającą zdarzeń"
ms.assetid: 9058fb2f-8a93-4036-a921-97a0772f503c
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/16/2017
ms.author: glenga
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d59ef16de433ac9691f6996eab2bf56f056feb88
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="optimize-the-performance-and-reliability-of-azure-functions"></a>Optymalizuj wydajność i niezawodność usługi Azure Functions

Ten artykuł zawiera wskazówki dotyczące poprawy wydajności i niezawodności sieci [niekorzystającą](https://azure.microsoft.com/overview/serverless-computing/) funkcji aplikacji. 


## <a name="avoid-long-running-functions"></a>Unikaj długotrwałe funkcji

Funkcje dużych, długotrwałą mogą powodować problemy nieoczekiwany limitu czasu. Funkcja może stać się duża ze względu na wiele zależności Node.js. Importowanie zależności mogą powodować zwiększone obciążenie uruchomień spowodować nieoczekiwane przekroczeń limitu czasu. Zależności są ładowane zarówno jawnie i niejawnie. Moduł pojedynczego załadowane przez kod może załadować własną dodatkowych modułów.  

Jeśli to możliwe, zrefaktoryzuj długie funkcje na mniejsze funkcja ustawia które współpracują ze sobą i szybko powrócić odpowiedzi. Na przykład elementu webhook lub funkcja wyzwalacza HTTP może wymagać potwierdzenia odpowiedzi w określonym terminie. jest typowe dla elementów webhook wymagają natychmiastowej reakcji. Ładunek wyzwalacza HTTP można przekazać w kolejce do przetworzenia przez funkcję wyzwalacza kolejki. Takie podejście umożliwia odroczenie rzeczywistą pracę i zwracać natychmiastowej reakcji.


## <a name="cross-function-communication"></a>Funkcja komunikacji między

Podczas integracji wiele funkcji, zazwyczaj jest najlepszym rozwiązaniem jest użycie kolejek magazynu dla wielu funkcji komunikacji.  Głównym celem jest kolejki magazynu to tańsze i znacznie łatwiejsze do udostępniania. 

Poszczególne wiadomości w kolejce magazynu są ograniczone w rozmiarze 64 KB. Jeśli chcesz przekazać większych wiadomości pomiędzy funkcjami usługi Azure Service Bus kolejka może służyć do obsługi wiadomości rozmiarów do 256 KB.

Tematy usługi Service Bus są przydatne, jeśli potrzebujesz filtrowania przed rozpoczęciem przetwarzania komunikatu.

Centra zdarzeń są przydatne do obsługi dużych obciążeń komunikacji.


## <a name="write-functions-to-be-stateless"></a>Pisanie funkcji jako bezstanowy 

Funkcje powinny być bezstanowych i idempotentności, jeśli to możliwe. Kojarzenie żadnych informacji o stanie wymagane z danymi. Na przykład kolejność przetwarzania prawdopodobnie byłyby skojarzony `state` elementu członkowskiego. Funkcja może przetworzyć zamówienia na podstawie w tym stanie, gdy ta funkcja jest bezstanowych. 

Funkcje idempotentności są szczególnie zalecane z wyzwalaczy czasomierza. Na przykład jeśli coś, co należy bezwzględnie uruchom raz dziennie, Zapisz, można je uruchomić czasie dnia z takie same wyniki. Funkcja można zamknąć po żadnej pracy dla danego dnia. Także jeśli poprzedniej uruchamianie nie powiodło się, aby zakończyć, przy następnym uruchomieniu należy wybierz miejsca, w którym.


## <a name="write-defensive-functions"></a>Pisanie funkcji obrony

Załóżmy, że funkcja może wystąpić wyjątek w dowolnym momencie. Projektowanie funkcji z możliwością kontynuować od poprzedniego punktu kończyć się niepowodzeniem podczas wykonywania dalej. Rozważmy scenariusz, który wymaga następujących czynności:

1. Zapytanie o 10 000 wierszy w bazie danych.
2. Tworzenie komunikatu w kolejce dla każdego z tych wierszy do dalszego przetwarzania wiersz w dół.
 
W zależności od tego, jak złożoność system jest, może być: zachowuje nieprawidłowo zaangażowany usługami podrzędnymi, awarii sieci lub limit przydziału ogranicza osiągnęło itp. Wszystkie te mogą wpływać na funkcji w dowolnym momencie. Należy projektować funkcji, aby móc przywrócić go.

Jak kodu reagować, jeśli wystąpi błąd po wstawieniu 5000 tych elementów w kolejce do przetworzenia? Śledzenie elementów w zestawie, które zostały zakończone. W przeciwnym razie możesz wstawić je ponownie po następnym. Może to mieć poważny wpływ na Twoje przepływu pracy. 

Jeśli element kolejki został już przetworzony, Zezwalaj funkcja ma być pusta.

Skorzystaj z środków obrony już dostarczona dla składników używanych przez platformę Azure funkcji. Na przykład, zobacz **obsługi wiadomości w kolejce skażone** w dokumentacji dotyczącej [kolejki magazynu Azure wyzwalaczy i powiązań](functions-bindings-storage-queue.md#trigger---poison-messages). 

## <a name="dont-mix-test-and-production-code-in-the-same-function-app"></a>Nie można mieszać testowych i produkcyjnych kodu w tej samej aplikacji — funkcja

Funkcje w obrębie aplikacji funkcji udostępniania zasobów. Na przykład pamięci jest udostępniony. Jeśli używasz aplikacji funkcji w środowisku produkcyjnym, nie dodawaj funkcji związanych z testu i zasoby do niego. Może spowodować nieoczekiwane obciążenie podczas wykonywania kodu produkcyjnego.

Należy zachować ostrożność ładowanie w aplikacjach funkcji produkcji. Pamięć jest uśredniana dla poszczególnych funkcji w aplikacji.

Jeśli masz współużytkowanego zespołu, do którego odwołuje się wiele funkcji .net, umieść ją w typowych folderu udostępnionego. Odwołanie zestawu z instrukcją podobny do poniższego przykładu: 

    #r "..\Shared\MyAssembly.dll". 

W przeciwnym razie jest łatwe przypadkowo wdrożono wiele wersji testu z tych samych danych binarnych, które zachowują się inaczej pomiędzy funkcjami.

Nie używaj pełnego rejestrowania w kodzie produkcyjnym. Ma ona negatywny wpływ na wydajność.



## <a name="use-async-code-but-avoid-blocking-calls"></a>Użyj kodu asynchroniczne, ale unikania blokowania połączeń

Programowanie asynchroniczne jest zalecanym najlepszym rozwiązaniem. Jednak zawsze uniknąć odwołujące się do `Result` właściwość lub wywołanie `Wait` metoda `Task` wystąpienia. Takie podejście może doprowadzić do wyczerpania wątku.


[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

## <a name="next-steps"></a>Następne kroki
Więcej informacji zawierają następujące zasoby:

Ponieważ usługi Azure Functions używa usługi Azure App Service, należy wziąć pod uwagę wskazówki dotyczące usługi aplikacji.
* [Wzorce i praktyki optymalizacji wydajności HTTP](https://docs.microsoft.com/azure/architecture/antipatterns/improper-instantiation/)
