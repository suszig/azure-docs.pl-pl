---
title: "Wiadomości Porównanie usług Azure"
description: "Porównanie usługi Azure Event siatki, centra zdarzeń i usługi Service Bus. Zaleca się usługi, która do użycia na potrzeby różnych scenariuszy."
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 03/16/2018
ms.author: tomfitz
ms.openlocfilehash: 30bbe7442cac96a1dcf6959cac2abedd61454a29
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/17/2018
---
# <a name="choose-between-azure-services-that-deliver-messages"></a>Wybór między usługami Azure, których dostarczania komunikatów

System Azure oferuje trzy usługi wspomagające z dostarczania komunikaty o zdarzeniach w całym rozwiązaniu. Te usługi są:

* [Zdarzenie siatki](/azure/event-grid/)
* [Event Hubs](/azure/event-hubs/)
* [Service Bus](/azure/service-bus-messaging/)

Mimo że mają pewnych podobieństw każdej usługi jest przeznaczony dla konkretnego scenariuszy. W tym artykule opisano różnice między tymi usługami i pomaga zrozumieć co do wyboru dla aplikacji. W wielu przypadkach obsługi uzupełniają i mogą być używane razem.

## <a name="event-vs-message-services"></a>Zdarzenia, a usługi wiadomości

Jest to ważna różnica zauważyć między usług dostarczających zdarzenia i usług, które dostarczenia komunikatu.

### <a name="event"></a>Wydarzenie

Zdarzenie jest lekki powiadomienie z informacją o warunku lub zmiany stanu. Wydawca zdarzeń ma nie oczekiwania dotyczące sposobu obsługi zdarzenia. Konsument zdarzenia decyduje o tym, co należy zrobić powiadomienia. Zdarzenia mogą być odrębne jednostki lub częścią serii.

Zdarzenia odrębny raport zmiana stanu i można wykonać akcję. Do wykonania następnego kroku, użytkownik musi znać tylko że coś się stało. Dane zdarzenia zawiera informacje o co się stało, ale nie ma danych, który wywołał zdarzenie. Na przykład zdarzenia powiadamia użytkowników czy plik został utworzony. Może on zawierać ogólne informacje o pliku, ale nie zawiera w samym pliku. Zdarzenia odrębny idealnie nadają się do niekorzystającą rozwiązania, które musi przebiegać proces skalowania.

Zdarzenia serii raport warunek i są załadowanych. Zdarzenia są uporządkowane czasu i powiązanych. Konsument musi Sekwencyjna serie zdarzeń do analizowania, co się stało.

### <a name="message"></a>Komunikat

Komunikat jest nieprzetworzone dane utworzone przez usługę mają być używane lub przechowywane w innym miejscu. Komunikat zawiera dane, która wyzwoliła potok wiadomości. Wydawca wiadomości ma oczekiwanie temat klienta obsługi wiadomości. Kontrakt istnieje między partnerami. Na przykład wydawcy wysyła wiadomość z danych pierwotnych i oczekuje konsumenta, aby utworzyć plik z tych danych i wysyłać odpowiedzi, gdy praca jest wykonywana.

## <a name="comparison-of-services"></a>Porównanie usług

| Usługa | Przeznaczenie | Typ | Kiedy stosować |
| ------- | ------- | ---- | ----------- |
| Event Grid | Reaktywne programowania | Rozkład zdarzeń (odrębny) | Reagowanie na zmiany stanu |
| Event Hubs | Dane big data potoku | Zdarzenie przesyłania strumieniowego (seria) | Dane telemetryczne i przesyłanie strumieniowe danych rozproszonych |
| Service Bus | Enterprise wysokiej wartości do obsługi komunikatów | Komunikat | Kolejność przetwarzania i transakcji finansowych |

### <a name="event-grid"></a>Event Grid

Siatka zdarzeń jest płyty montażowej obsługi zdarzeń, która umożliwia programowanie sterowane zdarzeniami, reaktywne. Używa publikowanie-subskrypcji modelu. Wydawcy zdarzeń emisji, ale ma nie oczekuje o tym, które zdarzenia są obsługiwane. Subskrybenci zdecyduj, zdarzenia, które chcą, aby obsłużyć.

Siatka zdarzeń jest ściśle zintegrowana z usługami Azure i może być zintegrowana z usługami innych firm. Go upraszcza użycia zdarzeń i obniża koszty, eliminując konieczność stałej sondowania. Zdarzenie siatki wydajne i niezawodne trasy zdarzenia z platformy Azure i zasobów innych niż Azure. Przekazuje zdarzenia do punktów końcowych w zarejestrowany subskrybenta. Komunikat zdarzenia zawiera informacje potrzebne do reagowania na zmiany w usługach i aplikacjach. Siatka zdarzeń nie jest potoku danych i nie dostarcza rzeczywistego obiektu, który został zaktualizowany.

Ma następującą charakterystykę:

* dynamicznie skalowalności
* Niski koszt
* Niekorzystającą

### <a name="event-hubs"></a>Event Hubs

Usługa Azure Event Hubs jest potoku danych big data. Ułatwia on przechwytywania, przechowywania i powtarzania danych strumienia danych telemetrycznych i zdarzeń. Dane mogą pochodzić z wielu źródeł współbieżnych. Centra zdarzeń umożliwia telemetrii i zdarzeń danych będą dostępne w różnych usługach infrastruktury i analiza strumienia przetwarzania. Jest ona dostępna jako strumienie danych lub partie powiązane zdarzenia. Ta usługa udostępnia jedno rozwiązanie, która umożliwia pobieranie danych szybkiego przetwarzania w czasie rzeczywistym, a także powtarzane powtarzania przechowywanych danych pierwotnych. Go przechwycić dane przesyłane strumieniowo do pliku do przetwarzania i analizy.

Ma następującą charakterystykę:

* małe opóźnienia
* Umożliwia odbieranie i przetwarzania miliony zdarzeń na sekundę

### <a name="service-bus"></a>Service Bus

Usługa Service Bus jest przeznaczony dla tradycyjnych przedsiębiorstw. Te aplikacje przedsiębiorstwa wymagają transakcji, porządkowanie, wykrywania duplikatów i natychmiastowe spójności. Usługi Service Bus umożliwia aplikacjom native chmurze zapewnia niezawodne stanu zarządzania przejścia dla procesów biznesowych. Podczas obsługi komunikatów wysokiej wartości, które nie mogą być utracone ani zduplikowany, należy użyć usługi Azure Service Bus. Usługi Service Bus również ułatwia wysokiej bezpiecznej komunikacji między hybrydowych rozwiązań w chmurze i mogą łączyć z istniejącymi systemami lokalnymi do rozwiązań w chmurze.

Usługa Service Bus jest obsługiwane przez brokera obsługi komunikatów systemu. Przechowuje komunikaty w "brokerze" (na przykład kolejki), aż strona odbierająca jest gotowa do odbierania wiadomości.

Ma następującą charakterystykę:

* dostawy niezawodnej komunikatów asynchronicznych (enterprise wiadomości jako usługa), który wymaga sondowania
* Zaawansowane funkcje obsługi wiadomości, takie jak FIFO, przetwarzania wsadowego sesjach transakcji, obsługa utraconych komunikatów, danych czasowych formantu, routing i filtrowanie i wykrywania duplikatów

## <a name="use-the-services-together"></a>Jednocześnie korzystać z usług

W niektórych przypadkach należy korzystać z usług obok siebie do pełnienia ról distinct. Na przykład witryn handlu elektronicznego, można użyć usługi Service Bus przetworzyć zamówienie, centra zdarzeń do przechwytywania danych telemetrycznych lokacji i siatki zdarzeń w celu reagowania na zdarzenia, takie jak element został wysłany.

W innych przypadkach to je połączyć ze sobą w celu utworzenia potoku zdarzeń i danych. Siatka zdarzeń służy do reagowania na zdarzenia w innych usługach. Na przykład do migracji danych do magazynu danych za pomocą siatki zdarzeń z usługą Event Hubs, zobacz [strumienia danych big data do magazynu danych](event-grid-event-hubs-integration.md). Na poniższej ilustracji przedstawiono przepływ pracy do strumieniowego przesyłania danych.

![Strumień danych — omówienie](./media/compare-messaging-services/overview.png)

## <a name="next-steps"></a>Kolejne kroki

* Aby uzyskać więcej informacji na temat wiadomości usług Azure, zobacz we wpisie blogu [zdarzeń, punktów danych i wiadomości — Wybieranie prawo Azure usługa obsługi komunikatów dla danych](https://azure.microsoft.com/blog/events-data-points-and-messages-choosing-the-right-azure-messaging-service-for-your-data/).
* Aby obejrzeć wprowadzenie do siatki zdarzeń, zobacz [o siatki zdarzeń](overview.md).
* Aby rozpocząć pracę z siatki zdarzeń, zobacz [tworzenie i tras niestandardowych zdarzeń siatki zdarzeń Azure](custom-event-quickstart.md).
* Aby rozpocząć pracę z usługą Event Hubs, zobacz [tworzenie przestrzeni nazw usługi Event Hubs i Centrum zdarzeń za pomocą portalu Azure](../event-hubs/event-hubs-create.md).
* Aby rozpocząć pracę z usługą Service Bus, zobacz [tworzenie przestrzeni nazw usługi Service Bus przy użyciu portalu Azure](../service-bus-messaging/service-bus-create-namespace-portal.md).