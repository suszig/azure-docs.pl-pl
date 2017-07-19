---
title: "Wprowadzenie do usługi Stream Analytics | Microsoft Docs"
description: "Poznaj usługę Stream Analytics — zarządzaną usługę, która pomaga analizować dane przesyłane strumieniowo z Internetu rzeczy (IoT) w czasie rzeczywistym."
keywords: "analiza jako usługa, usługi zarządzane, przetwarzanie strumienia, stream analytics, co to jest stream analytics"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 613c9b01-d103-46e0-b0ca-0839fee94ca8
ms.service: stream-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 06/16/2017
ms.author: jeffstok
ms.translationtype: Human Translation
ms.sourcegitcommit: 6dbb88577733d5ec0dc17acf7243b2ba7b829b38
ms.openlocfilehash: 421bdfb3132bc8c9f193bcca8d55c9cf9eba1c3b
ms.contentlocale: pl-pl
ms.lasthandoff: 07/04/2017


---

# <a name="what-is-stream-analytics"></a>Co to jest usługa Stream Analytics?

Usługa Azure Stream Analytics jest w pełni zarządzanym aparatem przetwarzania zdarzeń, który umożliwia ustawienie w czasie rzeczywistym obliczeń analitycznych na danych przesyłanych strumieniowo. Dane mogą pochodzić między innymi z urządzeń, czujników, witryn internetowych, kanałów informacyjnych mediów społecznościowych, aplikacji, systemów infrastruktury itp. 

## <a name="what-can-i-use-stream-analytics-for"></a>Do czego można używać usługi Stream Analytics?

Za pomocą usługi Stream Analytics można badać duże ilości danych napływających z urządzeń lub procesów, wyodrębniać informacje ze strumienia danych oraz wyszukiwać wzorce, trendy i relacje. W zależności od zawartości danych można wykonywać zadania aplikacji. Na przykład można zgłaszać alerty, rozpoczynać przepływy pracy automatyzacji, przekazywać informacje do narzędzia do raportowania, takiego jak usługa Power BI, lub przechowywać dane na potrzeby późniejszej analizy. 

Przykłady scenariuszy zastosowania analizy przesyłania strumieniowego:

* Spersonalizowana analiza giełdowych transakcji handlowych w czasie rzeczywistym i alerty oferowane przez firmy z branży usług finansowych.
* Wykrywanie oszustw w czasie rzeczywistym w oparciu o badanie danych transakcji. 
* Usługi ochrony danych i tożsamości.
* Analiza danych wygenerowanych przez czujniki i siłowniki osadzone w obiektach fizycznych (Internet rzeczy — IoT).
* Analiza strumienia kliknięć w sieci Web.
* Zastosowania związane z zarządzaniem relacjami z klientami (CRM, Customer Relationship Management), takie jak generowanie alertów w sytuacji spadku poziomu obsługi klienta w danym przedziale czasu.

## <a name="how-does-stream-analytics-work"></a>Jak działa usługa Stream Analytics?

Na poniższym diagramie przedstawiono potok analizy przesyłania strumieniowego, przedstawiający sposób, w jaki dane są pozyskiwane, analizowane, a następnie wysyłane w celu prezentacji lub wykonania akcji. 

![Potok usługi Stream Analytics](./media/stream-analytics-introduction/stream_analytics_intro_pipeline.png)

Analiza przesyłania strumieniowego jest uruchamiana w powiązaniu ze źródłem danych przesyłanych strumieniowo. Dane można pozyskać z urządzenia do platformy Azure przy użyciu Centrum zdarzeń platformy Azure lub usługi IoT Hub. Można również pobrać dane z magazynu danych, na przykład z usługi Azure Blob Storage. 

Aby zbadać strumień, należy utworzyć *zadanie* analizy przesyłania strumieniowego, które określa, skąd pochodzą dane. Zadanie określa również *przekształcenie* &mdash; jak wyszukiwać dane, wzorce lub relacje. Dla tego zadania analiza przesyłania strumieniowego obsługuje język zapytań przypominający SQL, który umożliwia filtrowanie, sortowanie, agregowanie i łączenie danych przesyłanych strumieniowo w określonym czasie.

Na koniec zadanie określa miejsce wysłania przekształconych danych. Dzięki temu można kontrolować, co należy zrobić w odpowiedzi na informacje, które zostały przeanalizowane. Na przykład w odpowiedzi na wyniki analizy można:

* Wysłać polecenie zmiany ustawień urządzenia. 
* Wysłać dane do kolejki monitorowanej przez proces, który podejmuje działania zależne od tego, co znajduje w danych. 
* Wysłać dane do pulpitu nawigacyjnego usługi Power BI w celu raportowania.
* Wysłać dane do magazynu takiego jak Data Lake Store, bazy danych programu SQL Server albo magazynu obiektów blob lub tabel platformy Azure.

W trakcie trwania zadania można je monitorować i dostosować liczbę zdarzeń przetwarzanych w ciągu sekundy. Można także ustawić zadania tak, aby tworzyły dzienniki diagnostyczne na potrzeby rozwiązywania problemów.

## <a name="key-capabilities-and-benefits"></a>Najważniejsze funkcje i korzyści

Usługa Stream Analytics została zaprojektowana tak, aby była łatwa w użyciu, elastyczna, skalowalna do dowolnej wielkości zadania i ekonomiczna.

### <a name="connectivity-to-many-inputs-and-outputs"></a>Łączność z wieloma wejściami i wyjściami

Usługa Stream Analytics łączy się bezpośrednio z usługami [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) i [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/) w celu pozyskiwania danych ze strumieni oraz z [usługą Azure Blob Storage](https://docs.microsoft.com/azure/storage/storage-introduction#blob-storage-accounts) w celu pozyskiwania danych historycznych. W przypadku pobierania danych z centrów zdarzeń usługę Stream Analytics można łączyć z innymi źródłami danych i aparatami przetwarzania.

Dane wejściowe zadania mogą także zawierać dane referencyjne (dane statyczne lub zmieniające się powoli). Dane przesyłane strumieniowo można połączyć z tymi danymi referencyjnymi w celu wykonania operacji wyszukiwania w taki sam sposób, jak w przypadku zapytań bazy danych.

Dane wyjściowe zadania usługi Stream Analytics można przekierować w wiele miejsc. Mogą być one zapisywane do magazynu, takich jak magazyn obiektów blob lub tabel usługi Azure Storage, bazy danych SQL platformy Azure, magazynu usługi Azure Data Lake Store lub bazy danych Azure Cosmos DB. Stamtąd dane mogą zostać przekazane do analizy wsadowej za pomocą usługi Azure HDInsight. Dane wyjściowe można wysłać do innej usługi, aby zostały wykorzystane w innym procesie, takim jak centra zdarzeń, tematy usługi Azure Service Bus lub kolejki. W celu wizualizacji dane wyjściowe można wysłać do usługi Power BI.

### <a name="ease-of-use"></a>Łatwość obsługi

Do definiowania przekształceń można użyć prostego, deklaratywnego [języka zapytań usługi Stream Analytics](https://msdn.microsoft.com/library/azure/dn834998.aspx), który umożliwia tworzenie zaawansowanych analiz bez konieczności programowania. Język zapytań przyjmuje dane przesyłane strumieniowo jako dane wejściowe. Następnie można filtrować i sortować dane, agregować wartości, wykonywać obliczenia, łączyć dane (w ramach strumienia lub z danymi referencyjnymi) oraz używać funkcji geoprzestrzennych. Zapytania można edytować w portalu, korzystając z funkcji IntelliSense i funkcji sprawdzania składni. Można je także testować przy użyciu przykładowych danych wyodrębnianych na żywo ze strumienia.

### <a name="extensible-query-language"></a>Rozszerzalny język zapytań

Możliwości języka zapytań można rozszerzyć, definiując i wywołując dodatkowe funkcje. Można zdefiniować wywołania funkcji w usłudze Azure Machine Learning, aby skorzystać z rozwiązań usługi Azure Machine Learning. Można również zintegrować funkcje JavaScript zdefiniowane przez użytkownika (UDF, user-defined functions), aby wykonywać złożone obliczenia w ramach zapytań usługi Stream Analytics.

### <a name="scalability"></a>Skalowalność

Usługa Stream Analytics może obsługiwać maksymalnie 1 GB danych przychodzących w ciągu sekundy. Integracja z usługami [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) i [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/) pozwala zadaniom pozyskiwać w ciągu sekundy miliony zdarzeń przychodzących z połączonych urządzeń, strumieni kliknięć, plików dziennika i innych źródeł. Za pomocą funkcji partycji centrów zdarzeń obliczenia można podzielić na etapy logiczne, z których każdy może zostać dalej podzielony w celu zwiększenia skalowalności.

### <a name="low-cost"></a>Niski koszt

Jako usługa w chmurze usługa Stream Analytics jest zoptymalizowana pod kątem niskiego kosztu. Usługę zaprojektowano pod kątem płatności zgodnie z rzeczywistym użyciem jednostek przesyłania strumieniowego i ilości danych przetwarzanych przez system. Użycie jest określane na podstawie ilości przetworzonych zdarzeń i ilości mocy obliczeniowej udostępnionej w klastrze do obsługi zadań usługi Stream Analytics.

### <a name="reliability-quick-recovery-and-repeatability"></a>Niezawodność, szybkie odzyskiwanie i powtarzalność

Jako usługa zarządzana w chmurze usługa Stream Analytics pomaga uniknąć utraty danych i zapewnia ciągłość działalności biznesowej. Jeśli wystąpią błędy, usługa zapewnia wbudowane funkcje odzyskiwania. Dzięki możliwości wewnętrznego przechowywania stanu usługa zapewnia powtarzalne wyniki, co gwarantuje możliwość archiwizacji zdarzeń i ponownego stosowania przetwarzania w przyszłości, zawsze uzyskując takie same wyniki. Umożliwia to klientom przechodzenie wstecz w czasie i badanie obliczeń podczas ustalania głównej przyczyny problemu, analizy warunkowej itp.

## <a name="next-steps"></a>Następne kroki

* Rozpoczynanie pracy przez [eksperymentowanie z danymi wejściowymi i zapytaniami z urządzeń IoT](stream-analytics-get-started-with-azure-stream-analytics-to-process-data-from-iot-devices.md).
* Tworzenie [rozwiązania typu end-to-end usługi Stream Analytics](stream-analytics-real-time-fraud-detection.md), które analizuje metadane telefonu, wyszukując fałszywe wywołania.
* Poznaj przeznaczony dla usługi Stream Analytics język zapytań przypominający SQL oraz unikatowe koncepcje, takie jak [funkcje okna](stream-analytics-window-functions.md).
* Dowiedz się, jak [skalować zadania usługi Stream Analytics](stream-analytics-scale-jobs.md). 
* Dowiedz się, jak [integrować usługi Stream Analytics i Azure Machine Learning](stream-analytics-machine-learning-integration-tutorial.md).
* Znajdź odpowiedzi na swoje pytania dotyczące usługi Stream Analytics na [forum usługi Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/home?forum=AzureStreamAnalytics).


